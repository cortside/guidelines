# Microservices

Microservices is an acrchitecutre that allows for an enterprise application or system to be broken up in to smaller logical or contextual components.  These smaller components are called services.

![Microservices Architecture](images/Microservice_Architecture.png)

(<https://microservices.io/patterns/microservices.html>)

TODO: add event/message relationships

> ## Advantages
> 
> * Allows larger number of software engineers or teams to participate in developing the overall enterprise system.
> * Breaks parts system components into smaller parts that can be more easily understood on their own.
> * Supports ability for continuous integration and continuous deployment.
> * Can allow for varying parts of the systems to scale at different rates that others.
> * Can allow for components to vary by technology as needed or makes most sense for a given context.
>   * This also means that services can be based on varying versions of an implementation framework without having to incur a synchronous cost to updating all components at the same time.
> 
> ## Disadvantages
> 
> * Addition complexity of creating a distributed system.
> * Distributed transactions.
> * Must implement the inter-service communication mechanism and deal with service availability and partial failures.
> * Additional effort in handling service requests that may span multiple services.
> * Testing of the interactions between services across the system.
> * Required coordination between multiple teams for operations that will span multiple services.
> * Complexity of deployment, management and monitoring of system.

## System decomposition into services

Decomposition of the The enterprise system into services should have an objective of smaller set of responsibilities with a mindset of a single responsiblity (the Single Responsibility Pattern for classes aptly apply to services as well).  The enterprise system can be decomposed into service by means of several methods:

* Business capability
* Domain-driven design subdomain
* Verb or use case based on a particular action (i.e. communication service that is responsible for sending and forwarding communications via supported methods (sms, email, slack, mail print))
* Noun or specific resource, where service is responsible for all actions on given set of entities or resources (i.e. user service that is responsible for user management)

## Data consistency and transactionality

Services should be loosely coupled and each service should have it's own database (Database per Service Pattern).  Given that any operation might require calls to multiple services, a transaction (2 phased-commit or distributed) is not generally an option or desirable.  To handle ensuring data consistency, services can publish events (Saga Pattern) when data state needs to change or to complete an operation.  Other services can consume these events, bringing all data into an eventual consistent state.  Using this kind of pattern also allows for a high level of resilience as unavaialble services can pick up events (messages) to being data into consistency when service is again available.

## Inter-Service Communication

There are two basic messaging patterns that services can use to communicate with other services within the enterprise system:

### Synchronous communication

This is where a service can call another service via HTTP requests/respsonses.  This option is a synchronous messaging pattern because the caller waits for a response from the receiver.  While other synchrounous communication methods are available, for my purposes this is always HTTP.

> #### Advantages
> 
> * Simpler method and well understood
> 
> #### Disadvantages
> 
> * Adds additional latency
> * Additional code for handling retries or circuit breaking
> * Can create unnecessary dependencies on transactional completeness
> * Tighter service coupling
> * Possible multiple requests for multiple interested services

Syncronous communication should generally be avoided in handling of incoming HTTP requests unless caller needs to know the disposition of the operation or needs information as a result of the operation completing.

### Asynchronous messaging

This is where a service will publish an AMQP message without waiting for a response, and one or more services process the message asynchronously.  While there are many choices in message brokers and messaging protocols, for my purposes AMPQ is the protocol of choice.  AMQP (Advanced Message Queuing Protocol) is a standard that allows for message broker selection without vendor lock in.

> #### Advantages
> 
> * Expected eventual consistency and handling.
> * Simplier scenario for handling retries and service outages.
> * Looser service coupling, publisher does not know if or how many consumers there are, nor knows about their intent of consuming the message.
> * Resilience of handling, consumer can consume when service is available and service can be scaled to expedite consumption backlog
> * Initial request responsiveness, by means of not having the caller wait for all parts of the operation to complete.
> 
> #### Disadvantages
> 
> * More complex, less understood, additional dependency on message broker.
> * Throughput can be the cost of resilience.  
> * Queued workload may cause delays to current workloads.

Asynchronous messaging should be used for as many service to service communications as possible, especially to 3rd party services beyond the control of the enterprise system.
