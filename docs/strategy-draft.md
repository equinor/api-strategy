# Equinor API Strategy - DRAFT

## Introduction
Application programming interfaces (APIs) are a core element of any digital business platform. APIs provide the interfaces between apps, data and services. These interfaces enables the connection between people, businesses and things. With proper management, APIs can be an enabler for innovation, faster development of digital products and new business models. This can positively affect the profitability of the company, a concept referred to in the industry as _the API Economy_. 

With the latest version of TR1621 (v7), Equinor is heading in an "API first" direction; software components should offer APIs to communicate with other components, share data and functionality. To further capitalize on the potentials of the API Economy, Equinor is establishing an API Strategy. The API strategy outlines the direction for management, design and development of APIs in Equinor.

This document distinguish between 4 categories of APIs, based on target audience; _app-private_, _private_, _partner_ and _public_. See the [Appendix](#api-categories) for a description of each category.


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


### <a name="api-portal"></a>API Portal
The API Portal is the central API registry, and the main tool to achieve _API discoverability_. The Equinor API portal is available at <https://api.equinor.com> and is implemented on top of [Microsoft Azure API Management](https://azure.microsoft.com/en-us/services/api-management). 

Key characteristics:

- Provides self service mechanisms for API client developers to gain access to APIs
- _All_ private and public APIs shall be published in the API Portal. App-private and partner APIs may be published if needed.
- Provides configurable _visibility_ of APIs
  - App-private APIs - if listed in the API portal - should only be visible to relevant client developers
  - Private APIs should be visible to everyone in Equinor (i.e. to all users in Equinor AD)
  - Partner APIs may have public or limited visibility, depending on the need for the particular API 
  - Public APIs should be visible to everyone internally and externally

The API portal should provide the following information about each API:
  
- Interface description: operations, input/output data, error codes
- Downloadable [OpenAPI specification](https://swagger.io/specification/)
- Access requirements, including information about how to apply for access
- Links to Terms of Service and SLA for the API, if such documents exists


### <a name="api-gateway"></a>API Gateway
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

With the rate of change in our industry, assumptions about scope and visibility of the API might change quickly. An important principle for our API development is to _develop the APIs in such a way that it is ready to be made available outside its current scope_. This could mean opening an app-private API to everyone in Equinor (private), opening it to external business partners or as a public API on the Internet. 

APIs should only be _app-private_ if it contains necessary optimizations that makes in unsuitable for other clients, or the API metadata have a security classification that prevents the API from being private (and visible to everyone in the company). 


### Consistency
To facilitate API adoption and make it easier to do reviews between teams following the API First principle, we should strive for _consistency_ in our APIs. [REST](#rest) is the preferred API mechanism in Equinor, except for industrial automation, where [OPC UA](#opc-ua) is the preference.

Other protocols like AMQP, MQTT and GraphQL may be used when they bring significant benefits. But keep in mind that these protocols are not supported by the OpenAPI specification nor by our [API portal](#api-portal) & [API gateway](#api-gateway), and are best suited for app-private APIs.


### <a name="rest"></a>REST

REST in the preferred API style in Equinor. See the [What is REST?](#what-is-rest) section in the Appendix for a brief explanation of the concept of REST.

APIs should be created with a minimum of [REST maturity level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2). In essence, maturity level 2 means:

- HTTP(S) as transport protocol
- APIs are modeled around resources (not actions). A resource is a part of the application's domain model.
- Use HTTP actions (GET, PUT, POST, DELETE, etc). Verbs in URLs are not allowed.
- Use standard HTTP status codes

All RESTful APIs should be specified with [OpenAPI specification](https://swagger.io/specification/).


### <a name="opc-ua"></a>OPC UA
[OPC](https://opcfoundation.org/about/what-is-opc/) Unified Architecture ([OPC UA](https://opcfoundation.org/about/opc-technologies/opc-ua/) / IEC 62541) is an open interoperability standard for secure and reliable exchange of data in industrial automation / OT.  The standard defines interface that includes access to real-time data, monitoring of alarms and events, access to historical data and other applications.

OPC UA is the preferred connectivity framework within industrial automation in Equinor, and will also be used to connect and integrate industrial automation and control systems (OT) with enterprise systems, business processes and analytics (IT).

Further readings:

- [OPA UA explained in 1 minute (movie)](https://www.youtube.com/watch?v=-tDGzwsBokY)
- [Industrial Internet Vocabulary](https://www.iiconsortium.org/vocab/index.htm)

## Appendix
### <a name="api-categories"></a>API Categories
In this document we distinguish between 4 API categories


| Term | Description |
| :--- | :--- |
| App-private | app-private APIs are available to a limited set of clients, within the same functional scope. These APIs are typically optimized for a particular consumer, like a web or mobile client. app-private APIs are sometimes referred to as _application-private_ or _app-internal_ APIs.|
| Private | Private APIs are available to clients within the company. Private APIs are sometimes referred to as _internal_ APIs. |
| Partner | Partner APIs are available to selected business partners |
| Public | Public APIs are publicly available on the Internet |

### <a name="what-is-rest"></a>What is REST?
Representational state transfer (REST) is an architecture style for web APIs, defined by Roy Fielding in year 2000 in his [doctoral dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm). REST defines a set of architectural constraints, as listed below. APIs operating within these constraints are called RESTful APIs.

- [Client-server architecture](https://en.wikipedia.org/wiki/Representational_state_transfer#Client-server_architecture)
- [Statelessness](https://en.wikipedia.org/wiki/Representational_state_transfer#Statelessness)
- [Cacheability](https://en.wikipedia.org/wiki/Representational_state_transfer#Cacheability)
- [Uniform interface](https://en.wikipedia.org/wiki/Representational_state_transfer#Uniform_interface)
- [Layered system](https://en.wikipedia.org/wiki/Representational_state_transfer#Layered_system)
- [Code on demand (optional)](https://en.wikipedia.org/wiki/Representational_state_transfer#Code_on_demand_(optional))

The REST constraints does not contain technical specifications, like transport protocol, data formats, etc. Consequently, there is no official technical standard for RESTful APIs. However, since its creation in 2000, industry standard conventions have emerged, particularly regarding the usage of HTTP.

Although Roy Fielding emphasizes that APIs need to adhere to _all_ of the REST constraints to be called RESTful, few actually do so. Particularly the concept of "Hypermedia as the engine of application state" (HATEOAS), which is part of the "Uniform interface" constraint, is rarely implemented. This principle is somewhat controversial, and seen by many as adding complexity without giving real value (e.g. [Why I Hate HATEOAS](https://jeffknupp.com/blog/2014/06/03/why-i-hate-hateoas/)). 

The lack of a precise definition and the lack of consensus around the HATEOAS principle can be a source of confusion. Fortunately, the [Richardson Maturity Model](http://martinfowler.com/articles/richardsonMaturityModel.html) brings clarity by breaking down the principles of REST and taking the industry conventions into account. [REST maturity level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2) has become the de facto standard for RESTful APIs. 

Note that maturity level 2 is not strictly "RESTful" according to Fielding's definition, but we use this term for level 2 APIs as this is common practice in the industry.

Further reading on REST:

- [Microsoft Azure REST design guidelines](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [Blog post: 8 Tips For Designing Quality REST APIs](https://nordicapis.com/8-tips-for-designing-quality-rest-apis/)


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
- [API Design - The guidelines (Hacker Noon)](https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9)

