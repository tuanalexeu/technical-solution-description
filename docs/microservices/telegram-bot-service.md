<h1 align="center">
<img src="https://www.logo.wine/a/logo/Telegram_(software)/Telegram_(software)-Logo.wine.svg" style="margin-left: 25px" alt="MkDocs icon" width="170">
<br>Telegram bot
</h1>

## Description
Finally, our clients would probably want to keep track of their orders, 
but no one wants to always visit website. For that, I registered a telegram bot called "Logiweb Bot"
which allows users to check their order status.
<!-- https://shields.io/ -->

## Project structure
The project structure is pretty much like in Chat Application. 
Bot also written on kotlin and the same maven configuration is used.

#### Bot API
In order to use bot and synchronize it with telegram server, I should first include the following dependency:
```xml
<dependency>
    <groupId>org.telegram</groupId>
    <artifactId>telegrambots-spring-boot-starter</artifactId>
    <version>4.1</version>
</dependency>
```

And then activate the context by calling `ApiContextInitializer.init()`:
```kotlin
@SpringBootApplication
class TelegramBotApplication {
    companion object {
        @JvmStatic
        fun main(args: Array<String>) {
            ApiContextInitializer.init()
            runApplication<TelegramBotApplication>(*args)
        }
    }
}
```

The only thing left to do is to extend **TelegramLongPollingBot** class 
and pass *name* and *token* of our bot:
```kotlin
@Service
class LogiwebBotService : TelegramLongPollingBot() {

    @Value("\${telegram.botName}")
    private val botName: String = ""

    @Value("\${telegram.token}")
    private val token: String = ""
    
    override fun getBotUsername(): String = botName

    override fun getBotToken(): String = token

    override fun onUpdateReceived(update: Update) {
        // Some code when user update is received
    }
}
```

It's done now, our bot is working and can be accessed from telegram:


## Technology stack
<dl>
<li>Spring Boot</li>
<li>Kotlin</li>
<li>Telegram Bot API</li>
<li>Maven</li>
</dl>