---
sidebar_position: 2
---

# Quickstart

** You can see and run a full working example for [kafka][kafka] and [rabbitmq][amqp]. **

Assuming you have a `@KafkaListener` like the following:

```java
@Service
public class ExampleConsumer {

    @KafkaListener(topics = "example-topic", containerFactory = "exampleKafkaListenerContainerFactory")
    public void receiveExamplePayload(ExamplePayloadDto payload) {
        // Do something with payload
    }

}
```

Follow the instructions below, and Springwolf will automatically pick up its metadata and generate documentation for it.

## Dependencies

Add the following dependencies:
```groovy
dependencies {
    // Provides the documentation API    
    implementation 'io.github.springwolf:springwolf-core:0.4.0'
    
    // Provides the UI - optional (recommended)
    runtimeOnly 'io.github.springwolf:springwolf-ui:0.3.0'
}
```

## Configuration Class

Add the following configuration class.

*Make sure to change the value of `CONSUMERS_BASE_PACKAGE` to the package containing your Kafka listeners.*  

```java
@Configuration
@EnableAsyncApi
public class AsyncApiConfiguration {

    private final String BOOTSTRAP_SERVERS = "kafka:29092";
    private final String CONSUMERS_BASE_PACKAGE = "io.github.stavshamir.springwolf.example.consumers";

    @Bean
    public AsyncApiDocket asyncApiDocket() {
        Info info = Info.builder()
                .version("1.0.0")
                .title("Springwolf example project")
                .build();

        Server kafkaServer = Server.builder()
            .protocol("kafka")
            .url(BOOTSTRAP_SERVERS)
            .build();

        return AsyncApiDocket.builder()
                .basePackage(CONSUMERS_BASE_PACKAGE)
                .info(info)
                .server("kafka", kafkaServer)
                .build();
    }

}
```

## View the docs
After starting the application, visit `<host>:<port>/springwolf/asyncapi-ui.html` to view the UI or `<host>:<port>/springwolf/docs` to view the raw AsyncAPI document.

If you configured a different context path in your application, make sure to prepend it to springwolf urls, i.e. `<host>:<port>/<context-path>/springwolf/asyncapi-ui.html`


[kafka]: https://github.com/springwolf/springwolf-core/tree/master/springwolf-examples/springwolf-kafka-example
[amqp]:https://github.com/springwolf/springwolf-core/tree/master/springwolf-examples/springwolf-amqp-example