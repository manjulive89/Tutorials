# MicroService Security

* [Starting Point](https://www.youtube.com/watch?v=wpA0N7kHaDo)
* [Spring Security Deep Dive](https://www.youtube.com/watch?v=AdsnM6OTepc)


# Microservice Characteristics

* Services in a microservice architecture are independently deployable.
* Services are organized around business capabilities.
* Services can be implemented using different
  - programming languages,
  - databases,
  - hardware and software environment,
  - depending on what fits best.
* Services are small in size,
  - messaging-enabled,
  - bounded by contexts,
  - autonomously developed,
  - decentralized and built and released with automated processes.
* independent deployable units.
* a variant of the service-oriented architecture (SOA) structural style— that arranges an application as a collection of loosely coupled services.
*  Services are fine-grained and the protocols are lightweight.

# Benefits of Microservices

* `Modularity`: This makes the application easier to understand, develop, test, and become more resilient to architecture erosion. This benefit is often argued in comparison to the complexity of monolithic architectures.

* `Scalability`: Since microservices are implemented and deployed independently of each other, i.e. they run within independent processes, they can be monitored and scaled independently. Integration of heterogeneous and legacy systems: microservices is considered as a viable mean for modernizing existing monolithic software application. There are experience reports of several companies who have successfully replaced (parts of) their existing software by microservices, or are in the process of doing so. The process for Software modernization of legacy applications is done using an incremental approach.

* `Distributed development`: it parallelizes development by enabling small autonomous teams to develop, deploy and scale their respective services independently. It also allows the architecture of an individual service to emerge through continuous refactoring. Microservice-based architectures facilitate continuous delivery and deployment.

# Issues with Microservices
Criticism

* Services form information Silos.

* Inter-service calls over a network have a higher cost in terms of network latency and message processing time than in-process calls within a monolithic service process.

* Testing and deployment are more complicated.

* Moving responsibilities between services is more difficult. It may involve communication between different teams, rewriting the functionality in another language or fitting it into a different infrastructure. However, Microservices can be deployed independently from the rest of the application while teams working on monoliths need to synchronize to deploy together.
Viewing the size of services as the primary structuring mechanism can lead to too many services when the alternative of internal modularization may lead to a simpler design. This requires the use of applications helping understanding the overall architecture of the applications and interdependencies between components.

* Two-phased commits are regarded as an anti-pattern in microservices-based architectures as this results in a tighter coupling of all the participants within the transaction. However, lack of this technology causes awkward dances which have to be implemented by all the transaction participants in order to maintain data consistency.

* Development and support of many services is more challenging if they are built with different tools and technologies - this is especially a problem if engineers move between projects frequently.

# Security
* You need to authorize and authanticate for each service call (Session independent).
  - Authorization
  - Authantication

## Best Practicies

### Defence in Dept

* Provide a layers of security.
* Variation of security,

## In trasit security
* Mutual / two way SSL
  - Issue with key management
* One way SSL
  - Client uses the Server public key to encrypt and decrypt the messages. All services share same key pair.
    - Key may be comparimised

### Session Afinity and sesssion management (User base interactions)
* Session infomration is saved on Server Side
  - Single Server save all the infomration.
    - Not scalebale, and single point of failure.
  - Session infomration is shared across servers
    - Cookies, store the Session tokens.
    - Seission (HTTP Post) ID/Token
      - Sticky Session session is stored on a single server
        - No useful when there are autoscaling and complex routiings
      - Storing session information in Memory and session information is accessed the in-memory cluster.
        - Complex in setting up
      - Storing Session informaton in database and session informaiton is extraaced from database each time
        - Performance impacts.

* Session information is saved on client side
* Mainaining user session information including authorization and authantication

  - Java Web Token (JWT) : Store session information in an envlop add header and footer, encrypt the envlop and pass that to user to maintain this informaition.

### API Gateway

* Is a layer that directly deals with client application to protect the Service layer. This layer also perform authorization and authantication.

### Distributed Tracing, monitoring, and logging

* Continious loging and monitoring.
* Logging proceess to identify the failure and its cause, and location.

# Various ways of Securing MicroServices

* Kerberos Tickets (Active directory), reqquires a Kerberos Server
* SAML Token (XML/SOAP), requires a SAML Server
* JWT Token (JSON/HTTP and MicrServices) prefered
  - It is standardized

# Security Stack Standards

* JSON Web Algorithm(JWA) : mainly deals with defining the Security Metadata (what encryption algorithm is used. etc.)
* JSON Web Key (JWK) : mainly deals with defining the Security Metadata (what encryption algorithm is used. etc.)
* JSON Web signature (JWS) : to protect the message from being tempared with, like hashing.
* JSON Web encryption (JwE) : encrypt the paylload
* JSON Web Token (JWT) : it is a based 64 of each of the following parts seperared by periods. The end result is a string that could be used anywhere.
  - Header : Algorithm used
  - Body: Arbitraty (programmer defined) infoormation
  - Signature:
* OAut2:
* OpenID Connect:

The JWT is either encrypted or signed.

Anyone can see the content of JWT but they can not change it.
It is placed in HTTP header as `Authorization:`
Similarly for RabitMQ header

You can use OAuth2 / OpennID and get and validate JWT Token.

#### More information abooout Json Web Token
* [Wiki](https://en.wikipedia.org/wiki/JSON_Web_Token)
* [Introduction](https://jwt.io/introduction/)
* [jwt.io](https://jwt.io/) you can see the JWT and what algorithm is used for specific JWT. Also provides library to read JWT with any programming language.
* [Youtube Intrduction](https://www.youtube.com/watch?v=cKjgkNt-tFg)
* [Great Resource for OAuth2](https://auth0.com/docs/getting-started)


### OAuth2
* It is generally a method to ask for user permission or using a resource or data.

* Application use the third party applicatiion to generate token
  - works for both authorization and authantiction
  - If API Token is missing the server routes the calls to the third party for authantication.
    - my not be the best way to secure services.

* MicroService uses Client ccredential part of OAuth2
* OAuth2 can be used for both User authantcation and server authantication.
  - There are 4 flows thet OAuth2 uses. each is for a specific use. include web user authanication and application/service authantication

##### OAuth2 commponenets and terminology

* Resource Owner
* CLients
* OAuth2 Authorization Server
* Resource Server

### OpenID
* is build on top of OAuth2, JWT, and TLS. it is most opinionated.
  - Specific fields are required in JWT.
* Defines a stanrd way of tokens

### Miroservice Security Patterns

* Use API Gateway A.K.A. Edge MicroServices
  - Internal MicroServices
  - Edge MicroService
  - Client

* [Backend For Frontend (BFF)](https://samnewman.io/patterns/architectural/bff/)

* Always use the JWT with MicroServices.

* In Spring security you can use `@PreAuthorized("#oauth2.hasScope('app1')")` for authorizing the service.
  - This means that the `app1` is authorized by OAuth2 server as indicated bby user, as a resource to share with this application. or Simply app1 is in the  in Scope. Scop is define in JWT as standard that OpenID connect and OAuth2 is required.
  - This is a java  annotation as part of Spring security.

*In Spring Security also need to use `@EnableResourceServer` this token indicated that
  - This application will not interact with any application that does not have JWT token. Part of Spring Security.
    - There may be some configuration in application.yaml in resources(maven java applicatiion structure) of Java Application.
