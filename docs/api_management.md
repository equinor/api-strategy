# API management

API management is the process of creating and publishing web APIs, enforcing their usage policies, controlling access, nurturing the subscriber community, collecting and analyzing usage statistics, and reporting on performance.

Functions typically found in API management products include:
 * **Gateway:** a server that acts as an API front-end, receives API requests, enforces throttling and security policies, passes requests to the back-end service and then passes the response back to the requester. A gateway often includes a transformation engine to orchestrate and modify the requests and responses on the fly. A gateway can also provide functionality such as collecting analytics data and providing caching. The gateway can provide functionality to support authentication, authorization, security, audit and regulatory compliance.
 * **Publishing tools:** a collection of tools that API providers use to define APIs, for instance using the OpenAPI or RAML specifications, generate API documentation, manage access and usage policies for APIs, test and debug the execution of API, including security testing and automated generation of tests and test suites, deploy APIs into production, staging, and quality assurance environments, and coordinate the overall API lifecycle.
 * **Developer portal/API store:** community site, typically branded by an API provider, that can encapsulate for API users in a single convenient source information and functionality including documentation, tutorials, sample code, software development kits, an interactive API console and sandbox to trial APIs, the ability to subscribe to the APIs and manage subscription keys such as OAuth2 Client ID and Client Secret, and obtain support from the API provider and user and community.
 * **Reporting and analytics:** functionality to monitor API usage and load (overall hits, completed transactions, number of data objects returned, amount of compute time and other internal resources consumed, volume of data transferred). This can include real-time monitoring of the API with alerts being raised directly or via a higher-level network management system, for instance, if the load on an API has become too great, as well as functionality to analyse historical data, such as transaction logs, to detect usage trends. Functionality can also be provided to create synthetic transactions that can be used to test the performance and behavior of API endpoints. The information gathered by the reporting and analytics functionality can be used by the API provider to optimize the API offering within an organization's overall continuous improvement process and for defining software Service-Level Agreements for APIs.
 * **Monetization:** functionality to support charging for access to commercial APIs. This functionality can include support for setting up pricing rules, based on usage, load and functionality, issuing invoices and collecting payments including multiple types of credit card payments.

## What does Equinor want to achieve with an API management tool

* **Security** Provide an additional level of security for blocking attacks and prevent misuse, by enforcing security policies and throttling, and providing logging and monitoring functionality.
* **Developer experience to ensure adoption** Central API repository where business partners (external) and internal API consumers can easily discover, test and learn how to use the APIs, using self-service mechanisms. 
* **Decoupling** Decouple data and functionality made available via APIs from the back-end systems. Back-end locations could be on-prem, in the cloud or both, in principle without affecting the API consumers. 
* **Governance** Shared functionality for consumer authentication, API subscription handling and possibility to write or declare custom rules.

## List of popular tools
### Open source
* [APIAxle](http://apiaxle.com/)
* [APIman](http://www.apiman.io/latest/)
* [APIUmbrella](https://apiumbrella.io/)
* [Gravitee](https://gravitee.io/)
* [Kong Inc.](https://getkong.org/)
* [KrakenD](https://www.krakend.io/)
* [Tyk](https://tyk.io/)
* [WSO2](https://wso2.com/api-management/)

### Proprietary
* [3scale](http://www.3scale.net/api-management/) (now owned by Red Hat[6])
* [Apigee](http://apigee.com/) (now owned by Google)
* [App42](http://api.shephertz.com/) API Gateway
* [Asseco]() (Ceptor API Management)
* [Axway](http://www.axway.com/en/enterprise-solutions/api-management) (acquired Vordel)
* [CA API Management](http://www.ca.com/) (formerly Layer 7, acquired by CA Technologies)
* [DreamFactory](https://www.dreamfactory.com/)
* [IBM API Connect](https://apim.ibmcloud.com/)
* [Microsoft (Azure API Management)](https://azure.microsoft.com/en-us/services/api-management/)
* [MuleSoft](http://www.mulesoft.com/)
* [Oracle API Platform Cloud Service](http://www.oracle.com/us/products/middleware/soa/api-management/overview/index.html)
* [Restlet](https://restlet.com/)
* [Rogue Wave Software](https://www.roguewave.com/products/akana/solutions/api-management) (acquired Akana)
* [Runscope](https://www.runscope.com/api-monitoring)
* [Sensedia](https://sensedia.com/en/) (part of CI&T[7])
* [SmartBear](https://smartbear.com/)
* [TIBCO Mashery](https://www.tibco.com/products/api-management)
