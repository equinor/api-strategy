# Equinor API Strategy - DRAFT

## Introduction
Application programming interfaces (APIs) are a core element of any digital business platform. APIs provide the interfaces between apps, data and services and enables the connection between people, businesses and things. With proper management, APIs can be an enabler for innovation and faster development of digital products and new business models. With the recently released TR1621 v7, Equinor is heading towards an "API first strategy". Meaning software components should offer APIs to communicate with other components, share data , functions etc.

The Equinor API Strategy outlines the direction for management, design and development of APIs in Equinor.

This document distinguish between 4 categories of APIs, based on target audience; _limited_, _private_, _partner_ and _public_. See the [Definitions](#api-categories) section for a description of each category.


## Goals

The goal of the API Strategy is to deliver a number of operational and strategic benefits to Equinor:

- **Increased efficiency in software development.** Getting access to data, often from another team or part of the organization, can be a time consuming process for development teams. In many cases, the consumers of these data need to do similar processing. By being able to reuse existing APIs providing data and processing capabilities - rather than building from scratch - development teams can develop new applications faster.
- **Increased agility in software architecture**. By building our applications on top of APIs and applying [microservice architecture principles](https://martinfowler.com/articles/microservices.html) we can create a more agile software architecture, making our software systems more adaptable to change. An agile architecture will in turn support the business becoming more agile.
- **Revitalize legacy applications.** Integrating with legacy applications is often difficult due to technical barriers and knowledge gaps. Consequently, these monolith's data and functionalities are often only accessible within the systems themselves. By building modern APIs on top of legacy systems, we can extract more value by making their abilities broadly and easily available. The APIs can also serve as an abstraction layer that can facilitate modularization and modernization. 
- **Enabler for Innovation** The availability of and easy access to a broad range of data and functionality can facilitate innovation. By combining data in new ways we can potentially gain new insights and build new services that brings added value to the company. The API Platform can enable innovation internally on our private APIs, but possibly also on public APIs by external parties (without direct investment by Equinor).
- **New business opportunities.** A number of businesses are getting significant income from the "API Economy". A common model of operation is to offer public APIs where the clients pay for consumption (as subscription or pr request). Another approach is to offer free public APIs where clients indirectly contributes to revenue by growing an existing business platform or ecosystem. Providing APIs to selected business is a third approach where new (or existing) business relationships is strengthened by API integration.


## API Platform

To reach the goals of the API Strategy, Equinor will establish an **API platform** exposing a significant part of Equinor data and functionality through APIs. Key characteristics of the API platform:

- **Well crafted, secure APIs.** The APIs in the platform should be crafted according to industry standards with high emphasis on security.
- **API discoverability.** Which APIs exists in the platform and what capabilities they have should be easy to discover for API consumers.
- **Developer experience (DX).** The user experience for developers (a.k.a. developer experience) that develops API clients is key for adoption of the APIs. A good DX requires a well designed APIs, API documentation, self-service based access, useful error messages, predictability in operations, etc.


## API Management Tools and roles

API Management tools are key to supporting a secure and effective API development process and reaching the goals of the API Strategy. The figure below describes how these tools fits into the system context of a typical private or public API, including interacting roles.

![alt text](images/API-C4.png "API Landscape")


### API Portal
The API Portal is the central API registry, and the main tool to achieve _API discoverability_. The Equinor API portal is available at <https://api.equinor.com> and is implemented on top of [Microsoft Azure API Management](https://azure.microsoft.com/en-us/services/api-management). 

Key characteristics:

- Provides self service mechanisms for API client developers to gain access to APIs
- _All_ private and public APIs shall be listed in the API Portal. Limited and partner APIs may be listed if needed.
- Provides configurable visibility of APIs
  - Limited APIs - if listed in the API portal - should only be visible to relevant client developers
  - Private APIs should be visible to everyone in Equinor (i.e. to all users in Equinor AD)
  - Partner APIs may have public or limited visibility, depending on the need for the particular API 
  - Public APIs should be visible to everyone internal and externally
  

The API portal should provide the following information about each API:
  
- Interface description: operations, input/output data, error codes
- Downloadable [OpenAPI specification](https://swagger.io/specification/)
- Access requirements, including information about how to apply for access
- Links to Terms of Service and SLA for the API, if such documents exists
 

### API Gateway
The API Gateway provides key security, bridging and monitoring features. Equinor uses [Microsoft Azure API Management](https://azure.microsoft.com/en-us/services/api-management) as API Gateway. APIs exposed through the API Gateway will be available at a subURL of `api.equinor.com`. Azure API Management provides the following key features:

- Security features like throttling and IP restrictions, JWT token validation, HTTP header validation
- Authentication policies
- Response caching
- Monitoring
- Access to APIs on the on-prem network through Azure ExpressRoute
  

The figure above also describes the interaction between API management tools and key roles. Please note that the developer roles represent personas/stereotypes, and will generally be the responsibility of a software DevOps team.


### API developer
The API developer creates the API on top of a backend system, and mange the API through its lifecycle. The API developer deploys the API to the API Gateway and API Portal. The API developer is responsible for ensuring that the API is well crafted, secure and properly documented.  Other things the API developer needs to consider; versioning strategy, should an SLA and/or Terms of Service be created, how to receive feedback from API client developers, etc.

### API client developer
This is the developer of an App that consumes the API. The API client developer will use the API portal to discover the existence of APIs and find relevant information about them. A key factor deciding whether an API client developer will use a particular API is the developer experience (DX) of that API. She is much more likely to start and keep using an API if the DX is good, and also recommend the API to others. Without a proper DX he/she will probably look for alternatives.

### End user
This is the user of the client app, and consumer of the data and functionality exposed by the API. The end user's primary concern is the stability and availability of the app, often (happily) unaware of the chain of dependencies behind.



## API Design

### Design Principles

#### API First
API First is one of our core API design principles. API First has two key elements:

- Define the API using a standard specification language before any line of code is written
- Get feedback on the API definition from team members and client developers

With the API first approach, we can achieve

- Evolving the API and learn about it’s usage in an efficient matter, without having to write any code
- Decoupling of API design and development. The API definition becomes a contract that teams can work on without having to wait for the implementation to be completed. And the implementation can be changed / replaced without impacting the clients.
- Specifying APIs with a standard specification language facilitates usage of tools for generating documentation, mock code, automatic quality checks, API Management tools, etc.

#### API As a Product
A key factor to enable the evolution of an API platform is treating our APIs as products.

Key elements in the API as a product principle:

- Focus on the users - your customers. Put yourself in their place, understand their needs
- Put emphasize in the user experience (developer experience) - usability, simplicity, etc. Take care not exposing any inner workings, implementation details, or internal naming schemes in the API
- Improve the product/API over time
- Make use of customer feedback

#### Be prepared to externalize
Assuming your API will forever remain within the current scope can limit the potential of the API. In many cases APIs are initially developed for a particular client. Because there is (currently) only one client, API developers might be tempted to lower the standards for the API, and go easy on design, specification, documentation, etc. Then, when other parties are interested, there might be a backlog of things to improve before new clients can start consuming. Similarly, with a particular client in mind, the API could easily be designed with unnecessary optimizations making it less suited for other clients.

With the rate of change in our industry, assumptions about scope and visibility of the API might change quickly. An important principle for our API development is to _develop the APIs in such a way that it is ready to be made available outside its current scope_. This could mean opening a limited API to everyone in Equinor (private), opening it to external business partners or as a public API on the Internet. 

APIs should only be _limited_ if it contains necessary optimizations that makes in unsuitable for other clients, or the API metadata have a security classification that prevents the API from being private (and visible to everyone in the company). 


### Consistency
To facilitate API adoption and make it easier to do reviews between teams following the API First principle, we should strive for _consistency_ in our APIs. _RESTful APIs_ is the preferred API mechanism in Equinor. In some cases, where a different approach has significant benefits, protocols like AMQP/MQTT, WebSockets and OPC UA may be used. These protocols are neither supported by the OpenAPI specification, nor by our API portal & gateway tools, and should only be used for limited APIs.


### REST
REST in the preferred API style in the API platform. RESTful APIs should be created with a minimum of [REST Maturity Level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2). In essence, maturity level 2 means:

- HTTP as transport protocol
- APIs are modeled around resources (not actions). A resource is a part of the application's domain model.
- Use HTTP actions. Verbs in URLs are not allowed.

All REST APIs should be specified with [OpenAPI specification](https://swagger.io/specification/).

The API strategy does not contain detailed guidelines for how to create REST APIs. Recommended links to REST guidelines:

- [Microsoft Azure REST design guidelines](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [8 Tips For Designing Quality REST APIs](https://nordicapis.com/8-tips-for-designing-quality-rest-apis/)
- [Zalando API Guidelines](https://opensource.zalando.com/restful-api-guidelines)
- [Roy Fielding Dissertation - Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

## Definitions
### <a name="api-categories"></a>API Categories
In this document we distinguish between 4 API categories


| Term | Description |
| :--- | :--- |
| Limited | Limited APIs are available to a limited set of clients within the company. Typically optimized for a particular consumer, like a web or mobile client. |
| Private | Private APIs are available to clients within the company |
| Partner | Partner APIs are available to selected business partners |
| Public | Public APIs are publicly available on the Internet |

## References & useful links

### OpenAPI Specification
- [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification/)
- [OpenAPI Specification - Web Page](https://swagger.io/specification/)
- [OpenAPI Specification Mind Map](https://openapi-map.apihandyman.io/)

### Publications & guidelines
- [Roy Fielding Dissertation - Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
- [Microsoft Azure REST design guidelines](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [Zalando API Guidelines](https://opensource.zalando.com/restful-api-guidelines)
- [OMNIA API](https://docs.omnia.equinor.com/ocd/apim/)

### Blogs & blog posts
- [Nordic APIs](https://nordicapis.com/)
- [API Evangelist](http://apievangelist.com/)
- [8 Tips For Designing Quality REST APIs (Nordic APIs)](https://nordicapis.com/8-tips-for-designing-quality-rest-apis/)
- [API Design - The guidelines (Hacker Noon)](https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9)

