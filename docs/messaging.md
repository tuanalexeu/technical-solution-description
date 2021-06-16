<h1 align="center">
<img src="https://assets.zabbix.com/img/brands/rabbitmq.svg" alt="MkDocs icon" width="170">
<br>MQ Broker
</h1>


## Description

<p>
I use different MQ Broker throughout development. Those are ActiveMQ, RabbitMQ and 
Google Pub/Sub service which is being used now.
</p>

<img src="https://miro.medium.com/max/5652/1*RKAP8xY3Btg2WQnq5kuk3A.png"/>

<!-- https://shields.io/ -->

## ActiveMQ

Apache ActiveMQ is an open source message broker written in Java together with a full Java Message Service client. 
It provides "Enterprise Features" which in this case means fostering the communication from more than one client or server.

This is how it worked:

Spring side:

```java
@Configuration
@EnableJms
public class JmsConfig {
    @Bean
    public Context context() throws NamingException {
       
        // Some properties to be used by Connection factory
        
        return new InitialContext(props);
    }

    @Bean
    public JMSProducer jmsProducer(@Qualifier("jmsContext") JMSContext context) {
        return context.createProducer();
    }

    @Bean
    public JMSContext jmsContext(TopicConnectionFactory connectionFactory) {
        return connectionFactory.createContext(SEC_CRED_LOGIN, SEC_CRED_PASS);
    }

    @Bean
    public TopicConnectionFactory connectionFactory() throws NamingException {
        TopicConnectionFactory connectionFactory = (TopicConnectionFactory) context().lookup("java:/RemoteJmsXA");

        // Some security configuration

        return userCredentialsConnectionFactoryAdapter;
    }

    @Bean
    public JmsTemplate jmsTemplate() throws NamingException {
        JmsTemplate jmsTemplate = new JmsTemplate(connectionFactory());
        jmsTemplate.setDefaultDestinationName("Logiweb");
        return jmsTemplate;
    }

    @Bean
    public Topic topic() throws Exception {
        TopicConnectionFactory factory = connectionFactory();

        Topic topic;

        try (TopicConnection topicConnection = factory.createTopicConnection(SEC_CRED_LOGIN, SEC_CRED_PASS);
             TopicSession session = topicConnection.createTopicSession(false, Session.AUTO_ACKNOWLEDGE)) {
            topicConnection.start();
            topic = session.createTopic("Logiweb");
        }
        return topic;
    }

}
```

JavaEE side:

```java
@MessageDriven(name = "DemoMDB", activationConfig = {
        @ActivationConfigProperty(
                propertyName = "destinationLookup",
                propertyValue = "java:global/remoteContext/Logiweb"),
        @ActivationConfigProperty(
                propertyName = "destinationType",
                propertyValue = "javax.jms.Topic"),
        @ActivationConfigProperty(
                propertyName = "acknowledgeMode",
                propertyValue = "Auto-acknowledge")
})
public class MessageDrivenBean implements MessageListener {
    @SneakyThrows
    @Override
    public void onMessage(Message message) {
        TextMessage textMessage = (TextMessage) message;
        // Some business logic
    }
}
```

## RabbitMQ
RabbitMQ is an open-source message-broker software that originally implemented the Advanced Message Queuing Protocol 
and has since been extended with a plug-in architecture to support Streaming Text Oriented Messaging Protocol, MQ Telemetry 
Transport, and other protocols.

The code:

```java
@Service
@PropertySource("classpath:messaging.properties")
public class MessageServiceImpl implements MessageService {
    
    @Autowired
    private Environment environment;
    Connection connection;
    Channel channel;
    
    @SneakyThrows
    @PostConstruct
    public void init() {
        // Establish connection with host localhost
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost(environment.getProperty("rabbitmq.host"));
        connectionFactory.setUsername(environment.getProperty("rabbitmq.user"));
        connectionFactory.setPassword(environment.getProperty("rabbitmq.password"));

        connection = connectionFactory.newConnection();
        channel = connection.createChannel();

        // Declare new queue named Logiweb
        channel.queueDeclare("Logiweb", false, false, false, null);
    }


    @Override
    public void send(String message) {
        try {
            // Publish basic text message
            channel.basicPublish("", "Logiweb", null, message.getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Here we catch the message:
```java
@WebListener
public class Listener implements ServletContextListener {
   @PostConstruct
   private void init() {

      ExecutorService service = Executors.newCachedThreadPool();
      
      ConnectionFactory factory = new ConnectionFactory();
      factory.setHost("localhost");
      Connection connection = factory.newConnection();
      Channel channel = connection.createChannel();

      channel.queueDeclare("Logiweb", false, false, false, null);
      
      service.submit(() -> {

         while (true) {
            DeliverCallback deliverCallback = (consumerTag, delivery) -> {
               String message = new String(delivery.getBody(), StandardCharsets.UTF_8);
               // some business logic
            };

            try {
               channel.basicConsume("Logiweb", true, deliverCallback, consumerTag -> { });
            } catch (IOException e) {
               e.printStackTrace();
            }
         }
      });
   }
}
```

## Google Pub/Sub or how it works now
Pub/Sub is an asynchronous messaging service that decouples services that produce events from services that process events.

You can use Pub/Sub as messaging-oriented middleware or event ingestion and delivery for streaming analytics pipelines.

#### Preparing
In order to be able to use Pub/Sub, we must first enable Google SDK api (And, of course, we need a running project we will register the service at)

#### Code

```java
@Service
public class MessageServiceImpl implements MessageService {

    @Value("${PUBSUB_TOPIC}")
    private String topicId;

    @Value("${PROJECT_ID_ENV}")
    private String projectId;
    
    public static void publishMessage(String projectId, String topicId, String message)
            throws IOException, ExecutionException, InterruptedException {
        TopicName topicName = TopicName.of(projectId, topicId);

        Publisher publisher = null;
        try {
            // Create a publisher instance with default settings bound to the topic
            publisher = Publisher.newBuilder(topicName).build();

            ByteString data = ByteString.copyFromUtf8(message);
            PubsubMessage pubsubMessage = PubsubMessage.newBuilder().setData(data).build();

            // Once published, returns a server-assigned message id (unique within the topic)
            ApiFuture<String> messageIdFuture = publisher.publish(pubsubMessage);
            String messageId = messageIdFuture.get();
        } finally {
            if (publisher != null) {
                // When finished with the publisher, shutdown to free up resources.
                publisher.shutdown();
                publisher.awaitTermination(1, TimeUnit.MINUTES);
            }
        }
    }
}
```

We need to know project id and topic name to send a message, 
then if we want to receive the message, we will also need subscription name