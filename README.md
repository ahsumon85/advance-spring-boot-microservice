
# Overview
The architecture is composed by five services:

   * [`micro-eureka-server`](https://github.com/habibsumoncse/secure-spring-boot-microservice#eureka-service): Service **Discovery Server** created with Eureka
   * [`micro-api-getway`](https://github.com/habibsumoncse/secure-spring-boot-microservice#api-gateway-service): API Gateway created with Zuul that uses the discovery-service to send the requests to the services. It uses Ribbon as a Load Balancer
   * [`micro-auth-service`](https://github.com/habibsumoncse/secure-spring-boot-microservice#authorization-service): Simple REST service created with `Spring Boot, Spring Cloud Oauth2, Spring Data JPA, MySQL` to use as an **authorization service**
   * [`micro-item-service`](https://github.com/habibsumoncse/secure-spring-boot-microservice#item-service): Simple REST service created with `Spring Boot, Spring Data JPA, MySQL` to use as a **resource service**
   * [`micro-sales-service`](https://github.com/habibsumoncse/secure-spring-boot-microservice#sales-service): Simple REST service created with `Spring Boot, Spring Data JPA, MySQL` to use as a **resource service**

### tools you will need
* Maven 3.0+ is your build tool
* Your favorite IDE but we will recommend `STS-4-4.4.1 version`. We use STS.
* MySQL server
* JDK 1.8+

##
# Eureka Service

Eureka Server is an application that holds the information about all client-service applications. Every Micro service will register into the Eureka server and Eureka server knows all the client applications running on each port and IP address. Eureka Server is also known as Discovery Server.

## How to run eureka service?

### Build Project
Now, you can create an executable JAR file, and run the Spring Boot application by using the Maven or Gradle commands shown below −
For Maven, use the command as shown below −

`mvn clean install`

or

**Project import in sts4 IDE** 
```File > import > maven > Existing maven project > Root Directory-Browse > Select project form root folder > Finish```

### Run project 

After “BUILD SUCCESSFUL”, you can find the JAR file under the build/libs directory.
Now, run the JAR file by using the following command −

Run on terminal `java –jar <JARFILE> `

 Run on sts IDE
 
 `click right button on the project >Run As >Spring Boot App`
 
Eureka Discovery-Service URL: `http://localhost:8761`

##
# Authorization Service

![1](https://user-images.githubusercontent.com/31319842/99893591-9d003b80-2cab-11eb-9f06-1d5a785c775b.png)

Whenever we think of microservices and distributed applications, the first point that comes to mind is security. Obviously, in distributed architectures, it is really difficult to manage security as we do not have much control over the application. So in this situation, we always need to have a central entry point to this distributed architecture. This is the reason why, in microservices, we have a separate and dedicated layer for all these purposes. This layer is known as the API Gateway. It is an entry point for a microservice's architecture.

To maintain security, the first necessary condition is to restrict direct microservice calls for outside callers. All calls should only go through the API Gateway. The API Gateway is mainly responsible for authentication and authorization of the API requests made by external callers. Also, this layer performs the routing of API requests that come from external clients to respective microservices. This allows the API Gateway to act as an entry point for all its respective microservices. So, we can say the API Gateway is mainly responsible for the security of microservices.

## How to run auth service?

### Build Project
Now, you can create an executable JAR file, and run the Spring Boot application by using the Maven or Gradle commands shown below −
For Maven, use the command as shown below −

`mvn clean install`

or

**Project import in sts4 IDE** 
```File > import > maven > Existing maven project > Root Directory-Browse > Select project form root folder > Finish```

### Run project 

After “BUILD SUCCESSFUL”, you can find the JAR file under the build/libs directory.
Now, run the JAR file by using the following command −

Run on terminal `java –jar <JARFILE> `

 Run on sts IDE
 
 `click right button on the project >Run As >Spring Boot App`
 
Then will refresh Eureka Discovery-Service URL: `http://localhost:8761` and we will see auth instance running on `9191` port.

### Test Authorization Service
***Get Access Token***

Let’s get the access token for `admin` by passing his credentials as part of header along with authorization details of appclient by sending `client_id` `client_pass` `username` `userpsssword`

Now hit the POST method URL via POSTMAN to get the OAUTH2 token.

***http://localhost:8080/oauth/token***

Now, add the Request Headers as follows −

*`Authorization` − Basic Auth with your Client Id and Client secret.

*`Content Type` − application/x-www-form-urlencoded
![1](https://user-images.githubusercontent.com/31319842/95816138-2e40d180-0d40-11eb-99c7-403cdf7ef070.png)

Now, add the Request Parameters as follows −

* `grant_type` = password
* `username` = your username
* ` password` = your password
![2](https://user-images.githubusercontent.com/31319842/95816163-3bf65700-0d40-11eb-9c87-7b721e0a268f.png)

**HTTP POST Response**
```
{ 
  "access_token":"000ff762-414c-4605-858a-0ed7bee6f68e",
  "token_type":"bearer",
  "refresh_token":"79aabc70-f310-4c49-bf7e-516208b3bef4",
  "expires_in":999999,
  "scope":"read write"
}
```

##
# Item Service -resource service

Now we will see `micro-item-service` as a resource service. The `micro-item-service` a REST API that lets you CRUD (Create, Read, Update, and Delete) products. It creates a default set of items when the application loads using an `ItemApplicationRunner` bean.

## How to run item service?

### Build Project
Now, you can create an executable JAR file, and run the Spring Boot application by using the Maven shown below −
For Maven, use the command as shown below −

**Project import in sts4 IDE** 
```File > import > maven > Existing maven project > Root Directory-Browse > Select project form root folder > Finish```

### Run project 

After “BUILD SUCCESSFUL”, you can find the JAR file under the build/libs directory.
Now, run the JAR file by using the following command −

 `java –jar <JARFILE> `
 Run on sts IDE
 `click right button on the project >Run As >Spring Boot App`
 
Eureka Discovery-Service URL: `http://localhost:8761`

After sucessfully run we can refresh Eureka Discovery-Service URL: `http://localhost:8761` will see `item-server` instance gate will be running on `8280` port

***Test HTTP GET Request on item-service -resource service***
```
curl --request GET 'localhost:8180/item-api/item/find' --header 'Authorization: Bearer 62e2545c-d865-4206-9e23-f64a34309787'
```
* Here `[http://localhost:8180/item-api/item/find]` on the `http` means protocol, `localhost` for hostaddress `8180` are gateway service port because every api will be transmit by the   
  gateway service, `item-api` are application context path of item service and `/item/find` is method URL.

* Here `[Authorization: Bearer 62e2545c-d865-4206-9e23-f64a34309787']` `Bearer` is toiken type and `62e2545c-d865-4206-9e23-f64a34309787` is auth service provided token


### For getting All API Information
On this repository we will see `secure-microservice-architecture.postman_collection.json` file, this file have to `import` on postman then we will ses all API information for testing api.


##
# Sales Service -resource service
Now we will see `micro-sales-service` as a resource service. The `micro-sales-service` a REST API that lets you CRUD (Create, Read, Update, and Delete) products. It creates a default set of items when the application loads using an `SalesApplicationRunner` bean.

Add the following dependencies:

* **Web:** Spring MVC and embedded Tomcat
* **Actuator:** features to help you monitor and manage your application
* **EurekaClient:** for service registration
* **JPA:** to save/retrieve data
* **MySQL:** to use store data on database
* **RestRepositories:** to expose JPA repositories as REST endpoints
* **Hibernate validator:** to use runtime exception handling and return error messages
* **oauth2:** to use api endpoint security and user access auth permission

#### Configure Application info and Oauth2 Configuration to check token validaty from auth service

* `security.oauth2.resource.token-info-uri=http://localhost:9191/auth-api/oauth/check_token` That is used to check user given token validaty from authorization service.
* `security.oauth2.client.client-id=mobile` Here `moblie` client-id that was we are already input in auth database of `micro-auth-service`
* `security.oauth2.client.client-secret=pin` Here `pin` client-password that was we are already input in auth database of `micro-auth-service`

Below we was used for checking user given token the following link `[http://localhost:9191/auth-api/oauth/check_token]` on the `http` means protocol, `localhost` for hostaddress, `9191` are port of `micro-auth-service`, we know auth service up on `9191` port `auth-api` are application context path of 'micro-auth-service' and `/oauth/check_token` is used to check token from auth service by spring security oauth2.
```
#Application Configuration
server.port=8380
spring.application.name=sales-server
server.servlet.context-path=/sales-api

#oauth2 configuration
security.oauth2.resource.token-info-uri=http://localhost:9191/auth-api/oauth/check_token
security.oauth2.client.client-id=mobile
security.oauth2.client.client-secret=pin
```

#### Enable oauth2 on sales service as a resource service
Now add the `@EnableResourceServer` and `@Configuration` annotation on Spring boot application class present in src folder. With this annotation, this artifact will act like a resource service. With this `@EnableResourceServer` annotation, this artifact will act like a resource service.


```
@Configuration
@EnableResourceServer
public class ResourceServerConfiguration extends ResourceServerConfigurerAdapter {

    private static final String RESOURCE_ID = "microservice";
    private static final String SECURED_READ_SCOPE = "#oauth2.hasScope('READ')";
    private static final String SECURED_WRITE_SCOPE = "#oauth2.hasScope('WRITE')";
    private static final String SECURED_PATTERN = "/**";

    @Override
    public void configure(ResourceServerSecurityConfigurer resources) {
        resources.resourceId(RESOURCE_ID);
    }

    @Override
    public void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .sessionManagement().disable()
                .authorizeRequests()
//                 .antMatchers("/sales/list").permitAll()
                .and()
                .requestMatchers()                
                .antMatchers(SECURED_PATTERN).and().authorizeRequests()
                .antMatchers(HttpMethod.POST, SECURED_PATTERN)
                .access(SECURED_WRITE_SCOPE)
                .anyRequest().access(SECURED_READ_SCOPE);
    }
}
```
## How to run sales service?

### Build Project
Now, you can create an executable JAR file, and run the Spring Boot application by using the Maven shown below −
For Maven, use the command as shown below −

**Project import in sts4 IDE** 
```File > import > maven > Existing maven project > Root Directory-Browse > Select project form root folder > Finish```

### Run project 

After “BUILD SUCCESSFUL”, you can find the JAR file under the build/libs directory.
Now, run the JAR file by using the following command −

 `java –jar <JARFILE> `
 Run on sts IDE
 `click right button on the project >Run As >Spring Boot App`
 
Eureka Discovery-Service URL: `http://localhost:8761`

After sucessfully run we can refresh Eureka Discovery-Service URL: `http://localhost:8761` will see `sales-server` instance gate will be run on `http://localhost:8280` port

#### Test HTTP GET Request on resource service -resource service
```
curl --request GET 'localhost:8180/sales-api/sales/find' --header 'Authorization: Bearer 62e2545c-d865-4206-9e23-f64a34309787'
```
* Here `[http://localhost:8180/sales-api/sales/find]` on the `http` means protocol, `localhost` for hostaddress `8180` are gateway service port because every api will be transmit by the   
  gateway service, `sales-api` are application context path of item service and `/sales/find` is method URL.

* Here `[Authorization: Bearer 62e2545c-d865-4206-9e23-f64a34309787']` `Bearer` is toiken type and `62e2545c-d865-4206-9e23-f64a34309787` is auth service provided token


### For getting All API Information
On this repository we will see `secure-microservice-architecture.postman_collection.json` file, this file have to `import` on postman then we will ses all API information for testing api.


##
# API Gateway Service

Gateway Server is an application that transmit all API to desire services. every resource services information such us: `service-name, context-path` will beconfigured into the gateway service and every request will transmit configured services by gateway


## How to run API Gateway Service?

### Build Project
Now, you can create an executable JAR file, and run the Spring Boot application by using the Maven or Gradle commands shown below −
For Maven, use the command as shown below −

`mvn clean install`
or

**Project import in sts4 IDE** 
```File > import > maven > Existing maven project > Root Directory-Browse > Select project form root folder > Finish```

### Run project 

After “BUILD SUCCESSFUL”, you can find the JAR file under the build/libs directory.
Now, run the JAR file by using the following command −

 `java –jar <JARFILE> `

 Run on sts IDE
 
 `click right button on the project >Run As >Spring Boot App`
 
After sucessfully run we can refresh Eureka Discovery-Service URL: `http://localhost:8761` will see `zuul-server` on eureka dashboard. the gateway instance will be run on `http://localhost:8180` port

![Screenshot from 2020-11-15 11-21-33](https://user-images.githubusercontent.com/31319842/99894579-6af0d880-2caf-11eb-84aa-d41b16cfbd12.png)

After we seen start auth, sales, item, zuul instance then we can try `secure-microservice-architecture.postman_collection.json` imported API from postman with token


