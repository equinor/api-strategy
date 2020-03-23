# Equinor REST API Guidelines

## Introduction

This document contains the internal REST API design guidelines at Equinor. The guidelines have been created to nurture quality and consistency in Equinor APIs, which are core attributes of the [Equinor API Strategy](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md). Teams at Equinor should use this document as a baseline when defining their API standards.

The guidelines are focused on a set of core topics, and are not intended to cover all aspects of REST API design. Links to other, more extensive API guidelines are included in the [References](#references) section. References and supplementing sources of information are linked throughout the document. 


## Table Of Contents

* [RESTful](#rest)
* [OpenAPI Specification](#open-api)
* [Resources](#resources)
* [HTTP Methods](#http-method)
* [HTTP Status Codes](#http-status-codes)
* [Query parameters](#query-parameters)
* [Data Formats](#data-formats)
* [Naming](#naming)
* [Versioning](#versioning)
* [References](#references)
* [Appendix](#appendix)


## <a id="rest"></a>REST

REST APIs should be created with a minimum of [REST maturity level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2). In essence, maturity level 2 means:

- HTTP(S) as transport protocol
- APIs are modeled around [resources](#resources), with URIs that uniquely identifies the resources
- Resources are accessed through the standard [HTTP methods](#http-method)
- APIs respond with standard [HTTP status codes](#http-status-codes)

See the [What is REST?](#what-is-rest) section in the Appendix for an explanation of the concept of REST.

## <a id="open-api"></a>OpenAPI Specification
REST APIs should be developed [API first](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md#api-first), defined using the [OpenAPI Specification](https://swagger.io/specification/).

Use the newest version of OpenAPI that is supported by the [API Gateway](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md#api-gateway). What is currently supported can be found in [Azure API Management - API import restrictions](https://docs.microsoft.com/en-us/azure/api-management/api-management-api-import-restrictions).

The OpenAPI [`info`](https://swagger.io/specification/#infoObject) object should be extensively populated to provide clarity for API client developers. Always include `description` and [`contact`](https://swagger.io/specification/#contactObject) information, and include `termsOfService` and [`license`](https://swagger.io/specification/#licenseObject) information when possible. 

### OpenAPI tools
There are a number of tools available for working with OpenAPI specifications. The list below contains a few examples. For an extensive list of available tooling, see [OpenAPI.Tools](https://openapi.tools/).

  * [Swagger Editor online](https://swagger.io/tools/swagger-editor/)
  * [VS Code OpenAPI extension](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)


## <a id="resources"></a>Resources

A _resource_ is the fundamental concept of a REST API. A resource is an object with data, types, relations to other objects, etc. Resources can be grouped into collections or exist on their own. 

Resources are accessed by a combination of a [HTTP method](#http-method) and a resource identifier ([URI](https://en.wikipedia.org/wiki/URI)).

* HTTP `GET` of `https://api.equinor.com/well-api/wells` should return a collection of wells
* HTTP `GET` of `https://api.equinor.com/well-api/wells/{well-id}` should return a single well, with the given well id

A resource is similar to an object instance in an object-oriented programming language, with the main difference being that in REST the operations are limited to the standard [HTTP methods](#http-method).

The resource model of a REST API does not have to, and often should not, mirror the internal domain model of the application. It should be developed with a focus on the API client developers, as expressed in the [design principles of the API Strategy](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md#design-principles).

### Sub-resources
Sub-resources (i.e. nested resources) are used to express hierarchical relations in the resource model. 

* HTTP `GET` of `https://api.equinor.com/well-api/wells/{well-id}/wellbores` should return a collection of wellbores belonging to the specific well

Nested resources provides _readability_. In the example above, it is evident that the requested wellbores belongs to a specific well. Domain model relations like _aggregation_ ("belongs to") and composition ("part of") are often well suited for being expressed as hierarchical relations in the resource model. When child objects have _relative IDs_, modeling them as sub-resources is often the obvious (or only viable) solution. 

Avoid many levels of nesting, as this will complicate the API and reduce the readability of the URIs. Two levels are often enough, three levels should be maximum.

Further reading: [Blog post: REST API Design Best Practices for Sub and Nested Resources](https://www.moesif.com/blog/technical/api-design/REST-API-Design-Best-Practices-for-Sub-and-Nested-Resources/)

### Avoid verbs in URIs
The URI should only be used to address the resource model, and _not contain any operations / verbs_. The only operations available in REST APIs are the [HTTP methods](#http-method).
Procedures, calculations, functions, etc. should be modeled as _resources_.

| Avoid this | Instead, do something like this |
| ---- | ---- |
| `GET /createWell` | `POST /wells` |
| `GET /calculatePerfectWell` | `POST /well/{well-id}/perfect-well-calculation`<br/>`PUT /well/{well-id}/perfect-well-calculation/start-calc`<br/>`GET /well/{well-id}/perfect-well-calculation/result`|


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

HTTP Status codes are returned from the server in response to an HTTP request, to indicate whether the request has completed successfully. HTTP status codes should be used accurately and consistently with the semantics of the HTTP standard. 

The status codes are grouped in five classes:

* 1xx informational response – the request was received, continuing process
* 2xx successful – the request was successfully received, understood, and accepted
* 3xx redirection – further action needs to be taken in order to complete the request
* 4xx client error – the request contains bad syntax or cannot be fulfilled
* 5xx server error – the server failed to fulfill an apparently valid request

Links to status code lists:

* [MDN web docs - HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
* [REST API Tutorial - HTTP Status Codes](https://www.restapitutorial.com/httpstatuscodes.html)


## <a id="query-parameters"></a>Query parameters

Query parameters are suitable for non-hierarchical search, filter and pagination parameters of `GET` requests. Sensitive data like session tokens shall not be places in query parameters, as request URIs are often stored in server logs and browser caches and are vulnerable to [HTTP referrer leakage](https://portswigger.net/kb/issues/00500400_cross-domain-referer-leakage). 


## <a id="data-formats"></a>Data formats

[UTF-8](https://en.wikipedia.org/wiki/UTF-8) should be used for encoding text and textual representation of data.

JSON is the preferred data format for REST APIs. Other formats like XML, WITSML, etc can be used if this give a clear benefit.


## <a id="naming"></a>Naming

See [Naming conventions and guidelines](https://github.com/equinor/api-strategy/blob/master/docs/naming.md).


## <a id="versioning"></a>Versioning

Handling changes to APIs without breaking client integration can be a challenge. There is no industry standard approach to versioning for REST APIs. A few different approaches are common. 

API versioning can come with a significant cost. Supporting multiple versions of the API can make development, testing, operation and evolving the API much more complex. Therefore, versioning should be avoided whenever possible, and the _API Evolution_ (no versioning) approach should be used.

If a versioning scheme is used, the principles of [semantic versioning](https://semver.org/) should be applied.

Further reading:

- [API Versioning Has No "Right Way](https://apisyouwonthate.com/blog/api-versioning-has-no-right-way)
- [Troy Hunt - Your API versioning is wrong...](https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/)


### API Evolution (no versioning)

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


## <a id="references"></a>References

### API Guidelines

* [Microsoft REST API Guidelines](https://github.com/microsoft/api-guidelines)
* [Microsoft Azure Web API design guidelines](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)
* [Zalando Restful API Guidelines](https://opensource.zalando.com/restful-api-guidelines/)
* [GOV.UK API technical and data standards](https://www.gov.uk/guidance/gds-api-technical-and-data-standards)

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

Although Roy Fielding emphasizes that APIs need to adhere to _all_ of the REST constraints to be called RESTful, few actually do so. Particularly the concept of "Hypermedia as the engine of application state" (HATEOAS), which is part of the "Uniform interface" constraint, is rarely implemented. This principle is somewhat controversial, and seen by many as adding complexity without giving real value (e.g. [Why I Hate HATEOAS](https://jeffknupp.com/blog/2014/06/03/why-i-hate-hateoas/)). 

The lack of a precise definition and the lack of consensus around the HATEOAS principle can be a source of confusion. Fortunately, the [Richardson Maturity Model](http://martinfowler.com/articles/richardsonMaturityModel.html) brings clarity by breaking down the principles of REST and taking the industry conventions into account. [REST maturity level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2) has become the de facto standard for RESTful APIs. 

Note that maturity level 2 is not strictly "RESTful" according to Fielding's definition, but we use this term for level 2 APIs as this is common practice in the industry.



# TODO


## <a id="security"></a>Security

* OAuth2.0 & OIDC
* Ref to API strategy security section

## <a id="deprecation"></a>Deprecation

* Client & partner consent to deprecation
* Reflection in API spec
* Monitor usage

