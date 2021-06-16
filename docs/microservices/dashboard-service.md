<h1 align="center">
<img src="https://www.proximitycr.com/wp-content/uploads/2019/04/Java-EE.png" alt="MkDocs icon" width="170">
<br>Logiweb Dashboard
</h1>

## Description

<p>
This is the 2nd part application for T-Systems Java School and 
the third microservice within Logiweb Microservices Application.
Dashboard application isn't alike any other microservices 
since it's written with Java EE technology stack
</p>

<!-- https://shields.io/ -->

## Technical requirements
<ol>
<li>Implement a separate client application - an electronic scoreboard that will show full information about the last orders (at least 10), their number will depend on your UI.</li>
<li>On the same screen, the summary information on drivers and trucks for the current month should be displayed. How many drivers are there, how many available / unavailable. How many trucks are there, how many are available / used on the order / out of order.</li>
<li>The data must be loaded at startup and stored on the client side. Data reloading is carried out in case of receiving a notification from the server.</li>
</ol>

## Project structure

Here we also have MVC structure, although the technology stack is different. 
So, JSF is used on frontend side, whereas, EJB are used to implement Model * Controller tiers.

#### Messaging
Dashboard depends on Logiweb Service. Together, they use messaging technology provided by Google (Pub/Sub service).

This is how it works:
<img src="https://miro.medium.com/max/5652/1*RKAP8xY3Btg2WQnq5kuk3A.png"/>

Logiweb sends an arbitrary message (E.g. "trigger") and Dashboard catches it:
```java
...
// Instantiate an asynchronous message receiver.
MessageReceiver receiver =
        (PubsubMessage message, AckReplyConsumer consumer) -> {
        // Handle incoming message, then ack the received message.
            logger.info("Id: " + message.getMessageId());
            logger.info("Data: " + message.getData().toStringUtf8());

            consumer.ack();

            pushBean.sendMessage("update");
        };

Subscriber subscriber = null;
try {
    subscriber = Subscriber.newBuilder(subscriptionName, receiver).build();
    // Start the subscriber.
    subscriber.startAsync().awaitRunning();
    System.out.printf("Listening for messages on %s:\n", subscriptionName);
    // Allow the subscriber to run for 30s unless an unrecoverable error occurs.
    subscriber.awaitTerminated(30, TimeUnit.SECONDS);
} catch (TimeoutException timeoutException) {
   // Shut down the subscriber after 30s. Stop receiving messages.
   subscriber.stopAsync();
}     
...
```

#### Web Socket
Once the message is received, I need to trigger my JSF file. This is what WebSocket is used for. 
It allows us to interact with browser, so there's no need to update a page manually.
Now I just need to push some message to FST and catch it on the other side:

It's worth noticing that channel name must be the same:
```java
@Singleton
public class PushBean {

    @Inject
    @Push(channel = "websocket")
    private PushContext pushContext;

    public void sendMessage(String message) {
        pushContext.send(message);
    }

}
```

JFS side:
```html
<h:head>
...
    <h:outputScript>
        function onMessage(message) {
<!--        Do whatever we want here -->
        }
    </h:outputScript>
</h:head>

<h:body>
...
    <f:websocket channel="websocket" onmessage="onMessage"/>
</h:body>
```

#### Web Services
Then, I just need to receive new data from main Logiweb application using Web Services:
```java
public List<OrderStatsDTO> getOrders() throws JsonProcessingException {
    return getObjectMapper().readValue(
        getOrderTarget().request(MediaType.APPLICATION_JSON).get(String.class),
        new TypeReference<List<OrderStatsDTO>>(){}
    );     
}
```

...and just use it in JSF:

```html
 <ui:repeat value="#{restReceiver.orders}" var="order" varStatus="outer_loop">
    <tr>
        <td>#{order.id}</td>
        <td>#{order.finished ? "Finished" : "In process"}</td>
        <td>#{order.regNum}</td>
        <td>#{order.driver1Name}</td>
        <td>#{order.driver2Name}</td>
    </tr>
</ui:repeat>
```

Now it's done, the data will be updating automatically once it's changed by main application!
## Technology stack
<dl>
<li>Enterprise Java Beans</li>
<li>MQ</li>
<li>Web Services</li>
<li>Wildfly AS</li>
<li>JSF</li>
</dl>

