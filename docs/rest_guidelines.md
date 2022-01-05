# Equinor REST API Guidelines [![Attribution-ShareAlike 4.0 International](https://licensebuttons.net/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/)

## Introduction

This document contains guidelines and recommendations for REST API design at Equinor. The guidelines have been created to nurture quality and consistency in Equinor APIs, which are core attributes of the [Equinor API Strategy](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md). 

The Equinor REST API Guidelines are focused on a set of core topics, and are not intended to cover all aspects of REST API design. Links to other, more extensive API guidelines are included in the [References](#references) section. References and supplementing sources of information are linked throughout the document. 


## Table Of Contents


* [REST](#rest)
* [API Security](#security)
* [OpenAPI Specification](#open-api)
* [Resources](#resources)
* [HTTP Methods](#http-method)
* [HTTP Status Codes](#http-status-codes)
* [Query parameters](#query-parameters)
* [Data Formats](#data-formats)
* [Naming](#naming)
* [Versioning](#versioning)
* [Deprecation & Sunset](#deprecation)
* [References](#references)
* [Appendix](#appendix)


## <a id="rest"></a>REST

REST APIs should be created with a minimum of [REST maturity level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2). In essence, maturity level 2 means:

- HTTP(S) as transport protocol
- APIs are modeled around [resources](#resources), with URIs that uniquely identifies the resources
- Resources are accessed and manipulated through the standard [HTTP methods](#http-method)
- APIs respond with standard [HTTP status codes](#http-status-codes)

See the [What is REST?](#what-is-rest) section in the Appendix for an explanation of the concept of REST.



## <a id="security"></a> API Security
API security guidelines are described in [API Security - Equinor API Strategy](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md#api-security).


## <a id="open-api"></a>OpenAPI Specification
REST APIs should be developed [API first](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md#api-first), and be defined using the [OpenAPI Specification](https://swagger.io/specification/).

Use the newest version of OpenAPI that is supported by the [API Gateway](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md#api-gateway). What is currently supported can be found in [Azure API Management - API import restrictions](https://docs.microsoft.com/en-us/azure/api-management/api-management-api-import-restrictions).

The OpenAPI [`info`](https://swagger.io/specification/#infoObject) object should be extensively populated to provide clarity for API client developers. Always include `description` and [`contact`](https://swagger.io/specification/#contactObject) information, and include `termsOfService` and [`license`](https://swagger.io/specification/#licenseObject) information when possible. 

There are a number of tools available for working with OpenAPI specifications, including the [Swagger Editor online](https://swagger.io/tools/swagger-editor/) and the [VS Code OpenAPI extension](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi). For an extensive list of available tooling, see [OpenAPI.Tools](https://openapi.tools/).


## <a id="resources"></a>Resources

A _resource_ is the fundamental concept of a REST API. A resource is an object with data, types, relations to other objects, etc. Resources can be grouped into collections or exist on their own. 

Resources can be accessed and manipulated by a combination of [HTTP methods](#http-method) and resource identifiers ([URI](https://en.wikipedia.org/wiki/URI)).

* `GET https://api.equinor.com/wells` should return a collection of wells
* `GET https://api.equinor.com/wells/{well-id}` should return a single well, with the given well id
*  `POST https://api.equinor.com/wells` should submit a well entity to the wells resource 

A resource is similar to an object instance in an object-oriented programming language, with the main difference being that in REST the operations are limited to the standard [HTTP methods](#http-method).

Resources should be named in _plural_ form (i.e. `wells` rather than `well`). Plural form makes it easier to achieve consistent naming across endpoints operating on different cardinalities. Plurality indicates that resources are regarded as collections, which conceptually makes sense (singleton resources can be seen as a collection with cardinality 1). E.g. in the examples above; the first retrieves all items in the collection of wells, the second retrieves a specific well from the collection, and the third adds an object to the collection of wells.

The resource model of a REST API does not have to, and often should not, mirror the internal domain model of the application. The resource model should be developed with a focus on the API client developers, as expressed in the [design principles of the API Strategy](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md#design-principles).

### Sub-resources
Sub-resources (i.e. nested resources) are used to express hierarchical relations in the resource model. 

Example: `GET https://api.equinor.com/wells/{well-id}/wellbores` should return a collection of wellbores belonging to the specific well

Nested resources provides _readability_. In the example above, it is evident that the requested wellbores belongs to a specific well. Domain model relations like _aggregation_ ("belongs to") and composition ("part of") are often well suited for being expressed as hierarchical relations in the resource model. When child objects have _relative IDs_, modeling them as sub-resources is often the obvious (or only viable) solution. 

Avoid many levels of nesting, as this will complicate the API and reduce the readability of the URIs. Two levels of nesting are often enough, three levels should be maximum.

Further reading: [Blog post: REST API Design Best Practices for Sub and Nested Resources](https://www.moesif.com/blog/technical/api-design/REST-API-Design-Best-Practices-for-Sub-and-Nested-Resources/)

### Avoid verbs in URIs
The URI should only be used to address the resource model, and _not contain any operations / verbs_. The only operations available in REST APIs are the [HTTP methods](#http-method).
Procedures, calculations, functions, etc. should be modeled as _resources_.

| Avoid this | Instead, do something like this |
| ---- | ---- |
| `GET /createWell` | `POST /wells` |
| `GET /calculatePerfectWell` | `POST /wells/{well-id}/perfect-well-calculation`<br/>`PUT /wells/{well-id}/perfect-well-calculation/start-calc`<br/>`GET /wells/{well-id}/perfect-well-calculation/result`|


## <a id="http-method"></a>HTTP Methods
The list below shows the most common HTTP methods used in REST APIs. For an extensive list, see: [MDN web docs - HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

| HTTP Method | CRUD | Description |
| --- | --- | --- |
| GET | Read | Request a representation of the specified resource or resource collection. GET requests should only retrieve data. `GET` is safe, idempotent & cacheable. |
| POST | Create | Submit an entity to the specified resource. `POST` is not safe, idempotent or cacheable. |
| PUT | Update (replace) | Replace all representations of the target resource with the request payload. `PUT` is idempotent, but not safe or cacheable. |
| PATCH | Update (modify) | Apply partial modifications to a resource. `PATCH` is not safe, idempotent or cacheable. | 
| DELETE | Delete | Deletes the specified resource. `DELETE` is idempotent, but not safe or cacheable.|

Further reading:

* [MDN - Safe](https://developer.mozilla.org/en-US/docs/Glossary/Safe)
* [MDN - Idempotent](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent)
* [MDN - Cacheable](https://developer.mozilla.org/en-US/docs/Glossary/Cacheable)
* [REST API Tutorial - HTTP methods](https://www.restapitutorial.com/lessons/httpmethods.html)
* [MDN web docs - HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)


## <a id="http-status-codes"></a>HTTP Status codes

HTTP Status codes are returned from the server in response to an HTTP request, to indicate whether the request has completed successfully. HTTP status codes should be used accurately and consistently with the semantics of the HTTP standard. The OpenAPI specification should document all possible HTTP Status codes for each endpoint so that consumers can understand which responses they can expect.

The status codes are grouped in five classes:

* 1xx informational response – the request was received, continuing process
* 2xx successful – the request was successfully received, understood, and accepted
* 3xx redirection – further action needs to be taken in order to complete the request
* 4xx client error – the request contains bad syntax or cannot be fulfilled
* 5xx server error – the server failed to fulfill an apparently valid request

In total there are over 60 possible HTTP Status codes, but APIs should focus on using a limited set in a consistent manner.

The most important HTTP Status codes are: 
* 200 OK - The request completed successfully
* 201 Created - The request completed successfully and one or more new resources were created
* 204 No Content - The request completed successfully and there is no additional content in the response (commonly used for HTTP PATCH)
* 400 Bad Request - The request is not compliant with the OpenAPI specification (for example missing parameters in the query string or request body invalid)
* 403 Forbidden - User does not have sufficient authorizations
* 404 Not Found - The resource does not exist or an non-existing endpoint has been called
* 409 Conflict - The request cannot be completed due to conflict with resource current state
* 500 Internal Server Error - The server failed to fulfill an apparently valid request
* 503 Service unavailable - API or a service it depends on is temporarily unavailable


Links to status code lists:
* [Hypertext Transfer Protocol (HTTP) Status Code Registry](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)
* [MDN web docs - HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
* [REST API Tutorial - HTTP Status Codes](https://www.restapitutorial.com/httpstatuscodes.html)


## <a id="query-parameters"></a>Query parameters

Query parameters are suitable for non-hierarchical search, filter and pagination parameters of `GET` requests. Sensitive data like session tokens shall not be places in query parameters, as request URIs are often stored in server logs and browser caches and are vulnerable to [HTTP referrer leakage](https://portswigger.net/kb/issues/00500400_cross-domain-referer-leakage). 


## <a id="data-formats"></a>Data formats

[UTF-8](https://en.wikipedia.org/wiki/UTF-8) should be used for encoding text and textual representation of data.

JSON is the preferred data format for REST APIs. Other formats like XML, WITSML, etc. can be used in cases where it gives a clear benefit. Offering multiple formats should then be considered, by supporting [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation).

### Dates

[RFC3339](https://tools.ietf.org/html/rfc3339) with UTC is the recommended date format.<br/> Example: `1956-02-08T12:34:56Z` = February 8th, 1956. 12:34:56 UTC

## <a id="naming"></a>Naming

General naming conventions and guidelines are described in [API Strategy - Naming conventions and guidelines](https://github.com/equinor/api-strategy/blob/master/docs/naming.md).


## <a id="versioning"></a>Versioning

Handling changes to APIs without breaking client integration can be a challenge. There is no industry standard approach to versioning of REST APIs. The most common alternatives are included in this section. 

API versioning can come with a significant cost. Supporting multiple versions of the API can make development, testing, operation and evolving the API much more complex. Therefore, versioning should be avoided whenever possible, and the _API Evolution_ (no versioning) approach should be used.

If a versioning scheme is used, the principles of [semantic versioning](https://semver.org/) should be applied.

Further reading:

- [API Versioning Has No "Right Way](https://apisyouwonthate.com/blog/api-versioning-has-no-right-way)
- [Troy Hunt - Your API versioning is wrong...](https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/)


### <a id="api-evolution"></a>API Evolution (no versioning)

API Evolution is the concept of striving to not make breaking changes to the API until there are absolutely no other options. This could imply building converter and adapter logic that will require a considerable effort from the development team (but still easier to handle than the complexities of versioning). 

Eventually, when breaking changes _must_ happen, the API developers should communicate the changes clearly  before implementing them in a predictable manner. Client developers should be contacted directly and mechanisms like the `deprecated` field of the OpenAPI specification should be applied.

If the changes are of major scale, creating a new resource or a new API could also be considered.

Further reading: [APIs you won't hate - API Evolution for REST/HTTP APIs](https://apisyouwonthate.com/blog/api-evolution-for-rest-http-apis)


### URI Versioning

In URI versioning, the API version is stated as part of the URI. E.g: `https://api.equinor.com/v1/wells/wellbores/`. This is an easy to understand and commonly used approach. The version number is explicit stated in URI, which makes it clear and concise to the user. 

Removing versions will break integration for client that have not yet migrated, thus resulting in an explicit (rather than silent) failure.

With URI versioning, the URI is no longer a unique address to the resource, but to a _version_ of the resource. This could be seen as a violation of the [Uniform interface](https://en.wikipedia.org/wiki/Representational_state_transfer#Uniform_interface) constraint of REST.


### Header versioning

Header versioning is implemented either by use of the standard `Accept` HTTP header, or by use of a custom header, e.g. `api-version`. The client specifies which version it supports using the header field, and the server responds accordingly.

With header versioning, the URI serves as a unique address to the resource and it is stable over time. But header versioning comes with added complexity for clients developers, who have to figure out which versioning header to provide and which version to request. Lack of standardized way of handling missing version/accept header (return the latest or earliest supported version?) could also be a source of confusion.


## <a id="deprecation"></a>Deprecation & Sunset

At some point, API endpoints, API versions, or features of an [unversioned API](#api-evolution) will need to be phased out.

When planning a phase out, API developers should set a _sunset date_, which is the planned date when the endpoint/version/feature becomes unavailable. The endpoint/version/feature should be marked as _deprecated_ at the same time as the sunset date is published. A sunset date should be published at least _6 months_ in advance.

All endpoints containing deprecated elements should return the [`Deprecation: <deprecation date>|"true"`](https://tools.ietf.org/html/draft-dalal-deprecation-header-02) and [`Sunset: <sunset date>`](https://tools.ietf.org/html/rfc8594) HTTP headers. 

All deprecated items should be marked in the OpenAPI specification, by setting the `deprecated` field and adding a comment in the corresponding `description` field.

Before the eventual shut down, API developers must monitor the API and make sure that clients have migrated away from the deprecated functionality.

When an endpoint or version is no longer available, the server should return `HTTP 403: Forbidden`, with a message explaining that the endpoint/version is no longer supported. After some time, the endpoint/version should become unreachable and the server can return a standard `HTTP 404: Not Found` like for other unrecognized requests.

Further reading: [Zalando REST API Guidelines](https://opensource.zalando.com/restful-api-guidelines/#deprecation)

## <a id="references"></a>References

### API Guidelines

| Document | Description |
| -------- | ----------- |
| [Microsoft REST API Guidelines](https://github.com/microsoft/api-guidelines) | Microsoft's internal, company-wide REST API design guidelines. A long list of guidelines covering most aspects of REST API design in detail. |
| [Microsoft Azure RESTful web API design](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design) | A readable introduction to the core concepts of REST API design. | 
| [Zalando RESTful API Guidelines](https://opensource.zalando.com/restful-api-guidelines/) | An excellent and comprehensive collection of REST API guidelines. Each guideline is categorized as either `MUST`, `SHOULD` or `MAY`. 
| [GOV.UK API technical and data standards](https://www.gov.uk/guidance/gds-api-technical-and-data-standards) | A set of recommendations and guidelines covering API design, development and operations. | 

### Publications & articles
- [Roy Fielding Dissertation - Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

## Appendix

### <a id="what-is-rest"></a>What is REST?
Representational state transfer (REST) is an architecture style for web APIs, defined by Roy Fielding in year 2000 in his [doctoral dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm). REST defines a set of architectural constraints, as listed below. APIs operating within these constraints are called RESTful APIs.

- [Client-server architecture](https://en.wikipedia.org/wiki/Representational_state_transfer#Client-server_architecture)
- [Statelessness](https://en.wikipedia.org/wiki/Representational_state_transfer#Statelessness)
- [Cacheability](https://en.wikipedia.org/wiki/Representational_state_transfer#Cacheability)
- [Uniform interface](https://en.wikipedia.org/wiki/Representational_state_transfer#Uniform_interface)
- [Layered system](https://en.wikipedia.org/wiki/Representational_state_transfer#Layered_system)
- [Code on demand (optional)](https://en.wikipedia.org/wiki/Representational_state_transfer#Code_on_demand_(optional))

The REST constraints does not contain technical specifications, like transport protocol, data formats, etc. Consequently, there is no official technical standard for RESTful APIs. However, since its creation in 2000, industry standard conventions have emerged, particularly regarding the usage of HTTP.

Although Roy Fielding emphasizes that APIs need to adhere to _all_ REST constraints to be called RESTful, few actually do so. Particularly the concept of "Hypermedia as the engine of application state" (HATEOAS), which is part of the "Uniform interface" constraint, is rarely implemented. This principle is somewhat controversial, and seen by many as adding complexity without giving real value (e.g. [Why I Hate HATEOAS](https://jeffknupp.com/blog/2014/06/03/why-i-hate-hateoas/)). 

The lack of a precise definition and the lack of consensus around the HATEOAS principle can be a source of confusion. Fortunately, the [Richardson Maturity Model](http://martinfowler.com/articles/richardsonMaturityModel.html) brings clarity by breaking down the principles of REST and taking the industry conventions into account. [REST maturity level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2) has become the de facto standard for RESTful APIs. 

Note that maturity level 2 is not strictly "RESTful" according to Fielding's definition, but we use this term for level 2 APIs as this is common practice in the industry.
