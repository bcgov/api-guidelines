---

title: "Government of British Columbia API Guidelines DRAFT"
description: "The purpose of these guidelines is to promote consistency and provide guidance around the use of Application Programming Interfaces (APIs) across the BC government, and to enable exchange and integration of data between systems, agencies, businesses and citizens."

---
_NOTE: This is a working draft, [posted here for your feedback](https://github.com/bcgov/api-guidelines/issues/new). We gratefully acknowledge the assistance of [the Government of Canada in sharing their API standards](https://www.canada.ca/en/government/system/digital-government/modern-emerging-technologies/government-canada-standards-apis.html), providing a baseline for this document._

# Government of British Columbia API Guidelines DRAFT

**Purpose** : The purpose of these guidelines is to promote consistency and provide guidance around the use of Application Programming Interfaces (APIs) across the BC government, and to enable exchange and integration of data between systems, agencies, businesses and citizens.

- **Why API First?** – APIs are an efficient and secure way to share data and expose functionality between government&#39;s digital services.  &quot;API First&quot; is a design principle which stipulates that an API is the first interface for a given application; it is the first artefact to be developed over the data layer, and it is self described.

- **What&#39;s in the API Guidelines?** – the B.C. Government API Guidelines provide design and best practices for building interfaces between systems or exposing data for secondary use.  Other topics include security, metadata, versioning and management.  The guidelines have been developed by BC Government IT professionals using internal references and a thorough review of several public and private sector leaders.

- **When to use the API Guidelines?** – most real time data integration requirements are best satisfied through APIs.   In general, data sharing and integration also encourage the use of APIs to promote platform independence and loosely coupled service design.

# API Design

## Design Principles:

- **_API as a Product_** : apply the product owner role to API's
- **_Simplicity and Reusability_** : strive to make the API the best way for clients to consume your data
- **_Consistency_** : design API&#39;s with a common look and feel using a consistent style and syntax
- **_Security by Design_** : adopt a philosophy where security is inherent in API development
- **_Continuous Improvement_** : actively improve and maintain API&#39;s over time by incorporating consumer feedback
- **_Sustainability_** : avoid short-term optimizations at the expense of unnecessary client-side obligations
- **_Quality_** : ensure flexibility, scalability and that your API presents actionable data to the consumer
- **_Well Described_** : adopt a simple, consistent and durable API specification and endpoint naming standard that includes API meta information
- **_Open Standards Based_** : stay compliant with the standard HTTP methods including status and error codes

## Design Patterns:

- **_Use a RESTful Approach_** – use HTTPS request/response format for data access and manipulation and provide proper error responses to the client
- **_Use JSON_** – use JavaScript Object Notation (JSON) as the message structure for all web service APIs
  - Form responses as a JSON object and not an array
  - Use consistent grammar case by using underscore or CamelCase
  - APIs into legacy systems may be required to use a different representation such as XML
- **_Use URIs to represent resources_** – URIs (Uniform Resource Identifiers) represent business entities, not the operations on those entities.  If data is returned as a part of a response, use URIs to uniquely identify the data as a resource
- **_Always Use HTTPS_** –
  - ACCEPT and CONTENT-TYPE request headers are mandatory
  - AUTHORIZATION header is mandatory for secured APIs
  - Pass the API key in the header rather than the URI
  - API keys/tokens must be securely setup and used appropriately
  - The response must contain the CONTENT-TYPE header
  - If caching is desirable, response headers must also include CACHE-CONTROL
- **_Don&#39;t overload verbs_** – Each verb should represent a single operation on a given resource. Avoid using request parameters to pass additional operations. The following are appropriate uses of HTTP verbs in the context of a RESTful API:
  - GET – read either a single or a collection resource
  - POST – create a new resource or initiate an action
  - PUT – update or replace an existing resource
  - DELETE – remove a resource
- **Follow properties according to** [**RFC 7231**](https://tools.ietf.org/html/rfc7231) and **[RFC 5789](https://tools.ietf.org/html/rfc5789)**:

| **Method** | **Safe** | **Idempotent** | **Cacheable** |
| --- | --- | --- | --- |
| [GET](https://tools.ietf.org/html/rfc7231#section-4.3.1) | :heavy_check_mark: Yes | :heavy_check_mark: Yes | :heavy_check_mark: Yes |
| [HEAD](https://tools.ietf.org/html/rfc7231#section-4.3.2) | :heavy_check_mark: Yes | :heavy_check_mark: Yes | :heavy_check_mark: Yes |
| [POST](https://tools.ietf.org/html/rfc7231#section-4.3.3) | :x: No | :heavy_exclamation_mark: No, but [**Should**](https://opensource.zalando.com/restful-api-guidelines/#229)[: Consider To Design POST and PATCH Idempotent](https://opensource.zalando.com/restful-api-guidelines/#229) | :small_orange_diamond: May, but only if specific [POST](https://opensource.zalando.com/restful-api-guidelines/#post) endpoint is [safe](https://opensource.zalando.com/restful-api-guidelines/#safe). **Hint:** not supported by most caches. |
| [PUT](https://tools.ietf.org/html/rfc7231#section-4.3.4) | :x: No | :heavy_check_mark: Yes | :x: No |
| [PATCH](https://tools.ietf.org/html/rfc5789) | :x: No | :heavy_exclamation_mark: No, but [**Should**](https://opensource.zalando.com/restful-api-guidelines/#229)[: Consider To Design POST and PATCH Idempotent](https://opensource.zalando.com/restful-api-guidelines/#229) | :x: No |
| [DELETE](https://tools.ietf.org/html/rfc7231#section-4.3.5) | :x: No | :heavy_check_mark: Yes | :x: No |
| [OPTIONS](https://tools.ietf.org/html/rfc7231#section-4.3.7) | :heavy_check_mark: Yes | :heavy_check_mark: Yes | :x: No |
| [TRACE](https://tools.ietf.org/html/rfc7231#section-4.3.8) | :heavy_check_mark: Yes | :heavy_check_mark: Yes | :x: No |

**_Source:_** [_https://opensource.zalando.com/restful-api-guidelines/#http-requests_](https://opensource.zalando.com/restful-api-guidelines/#http-requests)

- **_USE W3C HTTP Methods_** – follow the standard methods as described by the W3 Consortium - [https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)
- **_Segment response data for large queries_** – APIs exposing large datasets must support some form of data segmentation. The following are some common patterns for pagination along with appropriate use cases:

-
  - _page_ and _per\_page_ – best used to navigate large static datasets (e.g., reference data) where the same set of data is likely to be returned given the same page reference over time
  - _offset_ and _limit_ – best used for APIs fronting Structured Query Language (SQL) based backends where the offset represents the data cursor on a given indexed column
  - _since_ and _limit_ – Best used for queries where the consumer is interested in the delta since the last query and the backend data structure is indexed based on time

- **_Respond with message schemas that are easy to understand and consume -_**  **the following practices should be applied:**

  - **_Use common information models_** – Leverage industry recognized common information models where possible. If you must define your own information model, create a model which is technology and platform agnostic and avoid using proprietary schemas
  - **_Standardized error codes_** – Avoid building custom error codes and schemes which require heavy parsing by the consuming system. Conform to common HTTP status codes when building RESTful API&#39;s - [https://www.w3.org/Protocols/HTTP/HTRESP.html](https://www.w3.org/Protocols/HTTP/HTRESP.html)
  - **_Abstract internal technical details_** – Responses, including error messages, should abstract technical details which the API consumer has no visibility into. Internal technical errors, thread dumps, and process identifiers should all be removed from response data
  - **_Implement stateless interactions_** – The interaction between API consumer and provider must be stateless. APIs must not expect any concept of session or management of state on the part of the consumer.  Any transaction where multiple APIs are called in a repeatable sequence to create a singular business interaction should be implemented as a composite API
  - **_Prefer &#39;Pull&#39; over &#39;Push&#39;_** – use APIs to pull data rather than push data into databases. Having consuming systems query APIs based on specific parameters ensures that only data that&#39;s required in the context of a business process or transaction is being passed, and that the transmission of data is granular and secure.  Data sink APIs and Event Driven Architectures (EDA) often require additional complexity in queuing considerations; however, this approach to data streams can provide lower latency and intermittent fault tolerance than batch or bulk data integration techniques.  Bulk or batch data integration techniques and tools should be reserved for data synchronization patterns when network limitations are prevalent, or another limitation dictates its necessity
  - **_Bulk Dataset&#39;s via API&#39;s_** – in some cases APIs will be used to transfer bulk datasets between systems or external to BC Government. In those scenarios, apply the following considerations:
    - _small datasets_ – Smaller datasets should be returned in low overhead formats (e.g., Comma-separated values (CSV) or JSON) rather than XML. Do not use compressed file attachments as this is a possible method of bypassing content scanning mechanisms
    - _trigger API_ – An API may be implemented as a trigger to initiate an out-of-band interface such as a managed file transfer
    - _search and link API_ – If the dataset is published on file servers already available to the consumer, an API could be implemented to return a link to a specific file based on specific request parameters

# API Security

The following practices must be followed for any API which provides access to protected or privileged data and are strongly recommended as part of a &quot;security by design&quot; philosophy for all API development.  Outside of this baseline set of security controls, additional controls (e.g., message-level encryption, mutual authentication, and digital signatures) may be required based on the sensitivity of the data:

- **_Secure Data in Transit_** – always send data over a secure and encrypted network connection, regardless of data classification; enable TLS 1.2 or higher
- **_Security by Design_** – durable API design will include protection against common API attacks such as buffer overflows, SQL injection and cross site scripting. Treat all submitted data as untrusted and validate before processing.  Data validation (for both input parameters and inbound data) should be considered in the service tier but should also extend into the data model itself, with such considerations as data staging, mandatory values and referential integrity constraints as appropriate
- **_Do not put Sensitive Data in URIs_** – use the JSON payload to submit queries for sensitive data rather than putting it in the URI string
- **_Authenticate and Authorize_**  **–** ensure only privileged or authorized users can invoke your API once they are properly authenticated using either an API key or OAUTH token
  - Ensure that the API key/secret is adequately secured
  - Use API keys with all data API&#39;s to track and meter usage
  - For each API key, rate limits are applied across all API requests
  - For system-to-system integrations consider key/secret revocation and reissue capabilities
  - See here for BC Government standards on identity management: [https://www2.gov.bc.ca/gov/content/governments/services-for-government/policies-procedures/im-it-standards/find-a-standard#id\_mgt](https://www2.gov.bc.ca/gov/content/governments/services-for-government/policies-procedures/im-it-standards/find-a-standard#id_mgt)
  - See here for BC Government standards on general IT security: [https://www2.gov.bc.ca/gov/content/governments/services-for-government/policies-procedures/im-it-standards/find-a-standard#it\_sec](https://www2.gov.bc.ca/gov/content/governments/services-for-government/policies-procedures/im-it-standards/find-a-standard#it_sec)
- **_Token management_** – avoid using custom or proprietary tokens in favor of open industry standards such as JSON Web Token (JWT).  All access tokens must expire within a reasonable amount of time (less than 24 hours) and refresh intervals should reflect the security characteristics of the data being accessed.  Use fine grained access and the principle of least permission when defining tokens
- **_Restrict dynamic or open queries_** – the ability to inject consumer defined query strings or objects into an API must be limited to open data, reporting, and statistical APIs only, and strictly prohibited on master data, transactional or business APIs. Dynamic and open queries create dangerous attack surfaces for APIs. It&#39;s better to invest more effort in identifying all the valid query use cases and design the API to specifically meet them
- **_Restrict wildcard queries_** – wildcard queries in APIs can be dangerous from a data performance perspective. If wildcard characters are allowed, ensure there are restrictions on which and how many parameters can have wildcard input to prevent large data query sizes
- **_Integrate Security Testing_**  **-** automate security testing to validate any new changes to API source code and to ensure robustness of requested changes. Assess the change impact and conduct testing accordingly
- **_Audit Access to Sensitive Data_** – all API based access to non-public data must be logged and retained for audit purposes.  Logging attributes must include the source system, client identifier and associated timestamp from the target system
- **_Monitor and Log API Activity_** – track API usage and activity to identify performance bottlenecks, peak usage periods and abnormal access patterns. Use open standards-based logging frameworks such as CEF (Common Event Format) and collect logs in a central repository
  - When usage limits are exceeded, by default, a 1-hour timeout (block) will be used
  - X-RATELIMIT-LIMIT and X-RATELIMIT-REMAINING can be inspected in the HTTP response to view current usage

# API Encoding and Metadata

Consistent metadata and encoding ensures that APIs are interoperable across organizations and helps to maintain data consistency. The following practices should be followed when defining your API:

- **_Use Unicode for Encoding_** – Unicode Transformation Format-8 ([UTF-8](http://unicode.org/faq/utf_bom.html#utf8-1)) is the standard encoding type for all text and textual representations of data through APIs. It must be used for all APIs published across the BC Government for both internal and external consumption
- **_Standardize Datetime Format_** – use the ISO 8601 Standard for datetime representation in all BC Government APIs.  The standard date format is YYYY-MM-DD while timestamp format YYYY—MM-DD HH24:MI:SS.  If other formats are required due to source system limitations, convert it to the standard format within the API
- **_Support Official Languages_** – ensure the API can return responses in both English and French.   External facing APIs must reply with content in the requested language if the backend data support it. APIs must interpret the ACCEPT-LANGUAGE HTTP header and return the appropriate content. If the header is not set, then content in both languages should be returned
- **_Publish to the BC Data Catalogue_** – publishing a metadata record to the BC Data Catalogue helps people discover your API and promote its use
  - The BC Government API Registry can be found here: [https://catalogue.data.gov.bc.ca/he/group/bc-government-api-registry](https://catalogue.data.gov.bc.ca/he/group/bc-government-api-registry)

# API Documentation

BC Government APIs must be published to the API store to be discoverable.  The documentation MVP for BC Government APIs includes everything the API does, including resources, endpoints and methods, parameters, error codes, and example requests and responses:

- **_Use the OpenAPI Specification_** – OpenAPI is a machine-readable interface specification for RESTful APIs. There are open source tools (e.g., [Swagger](https://swagger.io/solutions/api-documentation/)) which can then generate human-readable documentation from this specification which avoids the need to create and maintain separate documentation
- **_Publish code samples and test data_** – the most effective way to document the scope and functionality of an API is to publish the code and data examples used to validate it alongside the API contract
  - Be sure to include standard and custom error response codes and interpret their specific meaning
  - Tools such as Postman and curl can be used to test request/response for sample API calls
- **_Maintain concise documentation_** – if there is a need to extend the API documentation beyond the OpenAPI specification, this is an indication that the API is too large and/or too complex
  - Be sure to document differences between versions of the API
  - Include documentation on any consumption constraints such as availability, authorization and rate limiting
- **_Gather Feedback_** – provide a clear mechanism to allow consumers to provide feedback, issue identification and enhancement requests

  - The BC Government API Registry can be found here:
[https://catalogue.data.gov.bc.ca/group/bc-government-api-registry](https://catalogue.data.gov.bc.ca/group/bc-government-api-registry)
  - The BC Government API Github Repo can be found here:
[https://github.com/bcgov/api-specs](https://github.com/bcgov/api-specs)

# API Consumption

The best way to validate your API design is to consume it with a production application within your organization.  Ideally, once the data layer is built, the next step is to build the application on top of an API:

- **_Build once for Multiple Channels_** – APIs should be designed in such a way that they can be consumed by systems internal to BC Government, external agencies and the broader public. Design should accommodate for all levels of access to encourage reuse
- **_Consume what you Build_** – build your application on top of an API layer that connects to the data layer rather than creating hard dependencies.  This ensures the API is production ready for consumers outside of the line of business

# API Lifecycle Management

APIs will change over time as corresponding source systems evolve.  To provide a robust and durable interface to applications, API lifecycle management must include a standard versioning scheme such that any changes to an API do not break the contract with existing consumers:

- **_Use Standard Endpoints_**  **–** each endpoint is a combination of the URI path (e.g. www.gov.bc.ca/finances/accounts/) and the verb (e.g. GET, PUT, POST, DELETE)

  _`https://<base domain>/<business function>/<application name>/<plural noun>`_

- **_API Versioning_** – each iteration of an API must be versioned. Every change to an API, no matter how small, should be indicated by a new version. Follow the `v<Major>.<Minor>.<Patch>` versioning structure whereby:
  - _Major_ = Significant release which is likely to break backwards compatibility
  - _Minor_ = Addition of optional attributes or new functionality that is backwards compatible, but should be tested
  - _Patch_ = Internal fix which should not impact the schema and/or contract of the API

  _For example:_

  - moving from v1.1.0 to v1.1.1 would allow a simple deploy-in-place upgrade
  - moving from v1.1.0 to v2.0.0 would be a major release and would require the legacy version to be kept while consumers test and migrate to the new version

- **_Use the Accept Header to Version_** – using a resource specific header approach can be used to maintain a single and consistent URI for an API.  This method also allows for other parameters such as caching, compression and content negotiation.  IETF legitimized this approach in [RFC4627](http://www.ietf.org/rfc/rfc4627.txt).

  _For example:_
  `Accept: application/vnd.example+json;version=1.0`

- **_Avoid URI Versioning_** – you are guaranteed to break client integration when using URI versioning and is therefore recommended to version within the Accept header

- **_Respect Existing Consumer Dependencies_** – support at least one previous major version (N-1) to ensure consuming systems have time to migrate to the latest version of the API
  - Set and publish a version deprecation policy and timeline so consumers can plan their dependencies accordingly
  - Ensure adequate testing on all minor and major releases
  - Backport high value changes to the previous (N-1) version where version integrity can be maintained
  - Provide a way to gather feedback from consumers to inform future development
- **_API Ownership_** – publish a designated point of contact to the API Store as part of the metadata record; including name, organization, email and phone (for high critically API&#39;s)
- **_Define an SLO up Front_** – each API should have a clearly defined Service Level Objective (SLO), which should include:
  - Support contact and availability
  - Service uptime objective (e.g., 99%)
  - Support response time (e.g., within the hour, 24 hours, best effort)
  - Scheduled outages (e.g., nightly, weekly, every 2nd Sunday evening)
  - Throughput limit (e.g., 100 requests per second per consumer)
  - Message size limit (e.g., <1Mb per request)
- **_API Publishing_** – all APIs should be published to the API Store for the purposes of discovery and lifecycle management.  APIs must be tagged with the appropriate metadata to indicate their desired audience (security classification) and appropriate usage patterns

# API Performance Management

API performance should be benchmarked periodically to ensure the performance and capacity meets the expectations of the SLO. This may include:

- **_API Load Testing_** – run performance tests against the API using a simulated workload to determine the response time and throughput.  Performance tests should be integrated into the development lifecycle, preferably through an automated CI/CD pipeline
- **_Publish Performance Data_** – performance summaries (e.g., average response time) should be included in the metadata record for the API as well as the SLO
- **_Performance Monitoring_** – performance should be monitored and reported on routinely, particularly as part of major releases
- **_API Throttling_** – throttling mechanisms should be implemented to control throughput against the stated SLO (e.g. number of requests per second).  This is typically handled by the API Gateway:
  - Information on the primary BC Government API Gateway can be found [here](https://developer.gov.bc.ca/Developer-Tools/API-Gateway-(powered-by-Kong-CE))
