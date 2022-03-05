# OpenAPI and Swagger

The OpenAPI Specification (OAS) defines a standard, programming language-agnostic interface description for HTTP APIs, which allows both humans and computers to discover and understand the capabilities of a service without requiring access to source code, additional documentation, or inspection of network traffic. When properly defined via OpenAPI, a consumer can understand and interact with the remote service with a minimal amount of implementation logic. Similar to what interface descriptions have done for lower-level programming, the OpenAPI Specification removes guesswork in calling a service. [^1]

Swagger is the name associated with some of the most well-known, and widely used tools for implementing the OpenAPI specification. [^2]

OpenAPI is a standard to describe REST APIs and it allows you to declare your API security method, endpoints, request/response data, and HTTP status messages. Together, these define your API in a single document. These are useful during the design phase, but can also be useful throughout the API lifecycle. [^3]

Each API microservice should expose an OpenAPI document and UI for being able to understand the operations that a service supports.  The UI ideally allows for a user to authenticate and execute operations.  The UI should be found at a common location, i.e. `/swagger`, for easy discovery.

[^1]: https://github.com/OAI/OpenAPI-Specification/
[^2]: https://swagger.io/blog/api-strategy/difference-between-swagger-and-openapi/
[^3]: https://blog.stoplight.io/rest-api-standards-do-they-even-exist