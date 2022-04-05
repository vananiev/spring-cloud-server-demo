Demo project for [spring-cloud/spring-cloud-config#2009](https://github.com/spring-cloud/spring-cloud-config/issues/2009)

Client side test
[DiscoveryClientConfigDataConfigurationTests.java#L148](https://github.com/spring-cloud/spring-cloud-config/blob/1cd03a05fbfeeaa33dc5b2a611ae89e5e34cf8f8/spring-cloud-config-client/src/test/java/org/springframework/cloud/config/client/DiscoveryClientConfigDataConfigurationTests.java#L148)
shows client expects `configPath` metadata from the cloud server.

To run application with Spring Cloud 2020.0.5 use
```shell
./mvnw spring-boot:run -Dspring-boot.run.jvmArguments="-Dspring.cloud.bootstrap.enabled=true"
```
or use Idea configuration 'SpringCloudServerDemoApplication'.

Check
```
GET http://localhost:8080/eureka/apps
```
Response contains
```xml

<metadata>
    <foo>bar</foo>
    <management.port>8080</management.port>
    <configPath>cloud-config</configPath>
</metadata>
```
This is expected behavior.

For reproduce problem change `spring-boot-starter-parent` version to `2.6.6`, and `spring-cloud.version` to `2021.0.1`.

Run app again and check same url, response is (can't reproduce)
```xml

<metadata>
    <foo>bar</foo>
    <management.port>8080</management.port>
    <configPath>cloud-config</configPath>
</metadata>
```