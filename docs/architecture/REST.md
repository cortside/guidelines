# REST (REpresentational State Transfer)

REST is an architectural style, first proposed by Roy Fielding, based on hypermedia that guides API design.  While it's not specific to HTTP, it is most commonly used over HTTP.  An advantage of REST over HTTP is that it uses open standards, and does not bind the implementation of the API or the client applications to any specific implementation. For example, a REST web service and client applications could be written in any language or toolset that can generate HTTP requests and parse HTTP responses.

Here are some of the main design principles of RESTful APIs using HTTP:

* Resource
    * any object, data or service that can be accessed by the client
    * the nouns that the HTTP verbs act on
* Resource identifier
    * the URI that uniquely identifies a given resource
      * i.e. https://api.domain.com/api/v1/users/61d78b92-3e20-4c7c-bdb8-8748b20c4cb7
* Representation
    * the data format for a given resource through which resources are manipulated
        * example of a GET request for the the user resource noted above

            ```json
            {
                "userId": "61d78b92-3e20-4c7c-bdb8-8748b20c4cb7",
                "firstName": "Elmer",
                "lastName": "Fudd",
                "mailingAddress": {
                    "street": "610 E. 4th St.",
                    "city": "Santa Ana",
                    "state": "CA",
                    "postalCode": "92701",
                },
                "emailAddresses": [
                    "elmer@fudd.com",
                    "chuckjones@gmail.com"    
                ]
            }
            ```
  
  * both the response and the request, when not part of the resource identifier, will have a representation
  * JSON is the most common data format for data representation
* Uniform interface via use of HTTP verbs and status
  * use of standard HTTP verbs to perform operations on resources, most common operations are:
    * GET: retrieve data
    * POST: add new data
    * PUT: update existing data
    * PATCH: update a subset of existing data
    * DELETE: remove data
  * use of standard HTTP status codes to denote success and failure disposition
    * 200: Success
    * 201: Created
    * 401: Unauthorized
    * 403: Forbidden
    * 404: Not found
* Stateless request model
  * HTTP requests should be independent and may occur in any order
  * each request should be an atomic operation
  * no server affinity
* Use of hypermedia links
  * driven by hypermedia links that are contained in the representation, i.e. links to get or update the customer associated with the order.

    ```json
    {
        "orderId": "c7fbc3b2-96f8-4917-9a9f-044d2a204078",
        "productId":2,
        "quantity":4,
        "orderValue":16.60,
        "links": [
            {"rel":"product","href":"https://api.domain.com/api/v1/customers/ac180807-3bb8-4039-a13f-57e856177c3e", "action":"GET" },
            {"rel":"product","href":"https://api.domain.com/api/v1/customers/ac180807-3bb8-4039-a13f-57e856177c3e", "action":"PUT" }
        ]
    }
    ```

## Richardson Maturity Model

Leonard Richardson proposed the following levels of REST as a means to describe it's elements.  Roy Fielding, who first proposed REST, stated that level 3 was a pre-condition of REST.

- Level 0 - Remote Procedure Invocation
  - Makes use of HTTP as a transport
  - Similar/same mechanism used for SOAP or XML-RPC
- Level 1 - Resources
  - Breaks larger, singular service endpoint into multiple resources
- Level 2 - HTTP Verbs
  - Uses HTTP verbs to standardize similar situations in the same way
- Level 3 - Hypermedia Controls (HATEOAS)
  - Introduces discoverability, providing a way of making a protocol more self-documenting

Many well used APIs today do not fully comply with the definition of REST that Roy Fielding proposed, but implement somewhere through level 2.  When an API does not comply with level 3 it is often called RESTful.

For my purposes, level 2 provides most of my expected benefits of REST without the additional complexities of the metadata required for level 3.  This does mean that resource identifiers cannot change without publication and instructing callers of any changes to resource structure.
