# API Strategy - Team mandate

## Background and purpose

APIs is key to achieving business agility, unlocking new value in existing assets, and accelerating the process of delivering new ideas to the market.

APIs can deliver a variety of operational and strategic benefits. For example, revitalizing a legacy system with modern APIs encapsulates intellectual property and data contained within that system, making this information reusable by developers who might not know how to use it directly (and probably would not want to). Likewise, building APIs onto monument systems makes it possible to extract more value from IT assets, while at the same time using valuable existing data to drive new innovations.

Incorporating APIs into new applications could also allow for easier consumption and reuse across new web, mobile, and IoT experiences, not to mention the option for exposing those APIs externally to enable new business models and partner ecosystems.

With the resently released [TR1621 v7](), we are heading towards an "API first strategy". Meaning software components should offer APIs to communicate with other components, share data , functions etc.

To accelrate this process CIT via Chief Engineer IT, needs to establish an API strategy, policies and guidelines for development of APIs within the company.

## Team

Chief Engineer IT has appointed Leading advisor Øyvind Rønne to lead in this initative, responsible for establishing the "API strategy" team. The team is responsible for delivering requested items from the backlog, through iterations and continous learning.

## Deliverables

High level topics which needs to be explored and concluded :

- **API portal:** a means for developers to discover, collaborate, consume, and publish APIs. To support the overall goal of self-service, the portal needs to describe APIs in a way that represents their functionality, context (the business semantics of what they do, and how they do it), nonfunctional requirements (scalability, security, response times, volume limits, and resiliency dimensions of the service), versioning, and metrics tracking usage, feedback, and performance. For units without mature master data or architectural standards, the API portal can still offer visibility into existing APIs and provide contact information for individuals who can describe features, functions, and technical details of services.
- **API gateway:** a mechanism that allows consumers to become authenticated and to “contract” with API specifications and policies that are built into the API itself. Gateways make it possible to decouple the “API proxy”—the node by which consumers logically interact with the service—from the underlying application for which the actual service is being implemented. The gateway layer may offer the means to load balance and throttle API usage.
- **API management and monitoring:** a centralized and managed control level that provides monitoring, service level management, SDLC process integration, and role-based access management. It includes the ability to instrument and measure API usage, and even capabilities to price and bill charge-back based on API consumption—to internal, or potentially external, parties.
- **API Examples:** document examples of topics to cover when developing sound APIs, such as code, documetation of API, amount of processing within the API etc. (Explored through iterations).
- **Key performance indicators (KPIs)** - should be defined for all exposed services. Deploying an API makes a service reusable. But is that service being reused enough to justify the maintenance required to continue exposing
it? By developing KPIs for each service, we could determine how effectively APIs are supporting the goals.


## How we work

- We shold embrace the [open API model](https://www.openapis.org/) and not waste our time (and everyone else’s) trying to plot every aspect of our API strategy journey.
- We should encourage to let demand drive scope, and let teams and developers determine the value of APIs being created based on what they are actively consuming. Teams should have to justify decisions not to reuse.
- We should encourage to keep the spirit of autonomy alive within teams, and let the best APIs win.
