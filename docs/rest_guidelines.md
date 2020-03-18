# Equinor REST API Guidelines

## Introduction

The Equinor REST API Guidelines contains the internal REST API design guidelines at Equinor. The guidelines focuses on a set of core topics, and are not intended to cover all aspects of API design. Links to other, more extensive gudelines are included in the references section.

The Equinor REST API guidelines have been created with the goal of contributing to quality and consistency in Equinor APIs, which are core attributes of the [Equinor API Strategy](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md). Teams at Equinor should use this document as a baseline when defining their API standards.


# Table Of Contents

* [OpenAPI Specification](#open-api)
* [Security](#security)
* [Resources](#resources)
* [Naming](#naming)
* [HTTP Methods](#http-method)
* [HTTP Status Codes](#http-status-codes)
* [Input parameters](#input-parameters)
* [Versioning](#versioning)
* [Data Formats](#data-formats)
* [Data Formats](#data-formats)
* [References](#references)

## <a id="open-api"></a>OpenAPI Specification
REST APIs should be developed [API first](https://github.com/equinor/api-strategy/blob/master/docs/strategy.md#api-first), defined using the [OpenAPI Specification](https://swagger.io/specification/).

Use the newest version of OpenAPI that is supported by the API Gateway. This can be verified [here](https://docs.microsoft.com/en-us/azure/api-management/api-management-api-import-restrictions).

The [`info`](https://swagger.io/specification/#infoObject) object should be extensively populated to provide clarity for API client developers. Always include `description` and `contact` information, and include `termsOfService` and `license` information when this exists. 

### OpenAPI tools
There are a number of tools available for working with OpenAPI specifications. The list below contains a few examples. For an extensive list of available toolins, see [OpenAPI.Tools](https://openapi.tools/).

  * [Swagger Editor online](https://swagger.io/tools/swagger-editor/)
  * [VS Code OpenAPI extension](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)



## <a id="resources"></a>Resources

* Focus on resources. No verbs in URLs
* Examples
* Sub resources

## <a id="naming"></a>Naming

* Reference to naming conventions
* Reference to OMNIA guidelines

## <a id="http-method"></a>HTTP Methods
Briefly explained

* GET
* POST
* DELETE
* PUT
* Include more?

## <a id="http-status-codes"></a>HTTP Status codes

* Emphasize importance of using appropriate status codes.
* Either compile a list, or point to a good reference

## <a id="input-parameters"></a>Input parameters

* When to use path parameters
* When to use query params

## <a id="versioning"></a>Versioning

* Reflect on the need for versioning, and alternatives
* Reccomendations
* Compatibility

## <a id="data-formats"></a>Data formats

* Unicode encoding
* JSON for most cases
* Other formats acceptable

## <a id="security"></a>Security

* OAuth2.0 & OIDC
* Ref to API strategy security section

## <a id="deprecation"></a>Deprecation

* Client & partner consent to deprection
* Reflection in API spec
* Monitor usage

## <a id="references"></a>References

* [Microsoft REST API Guidelines](https://github.com/microsoft/api-guidelines)
* [Zalando Restful API Guidelines](https://opensource.zalando.com/restful-api-guidelines/)
* [GOV.UK API technical and data standards](https://www.gov.uk/guidance/gds-api-technical-and-data-standards)
