# Best Practices

Quick list of best practices

## RESTful urls and actions

The key principles of REST involve separating your API into logical resources. These resources are manipulated using HTTP requests where the method has specific meaning.

### HTTP Methods

Commonly used HTTP methods include:

* GET: retrieve data
* POST: add new data
* PUT: update existing data
* PATCH: update a subset of existing data
* DELETE: remove data

### Resource Names

Resources are sometimes referred to as the nouns that the HTTP verbs act upon.  Resource names should be pluralized.

For example, if your API stored tickets, a resource name might be `tickets`. The path to access that resource might be /api/v1/tickets. The resource would then be combined with HTTP methods like so:

* GET /tickets - Retrieves a list of tickets
* GET /tickets/9ba46eeb-5a72-46d7-84dd-f4177dcdf23c - Retrieves a specific ticket
* POST /tickets - Creates a new ticket
* PUT /tickets/12 - Updates ticket #12
* PATCH /tickets/12 - Partially updates ticket #12
* DELETE /tickets/12 - Deletes ticket #12

Identifiers might be UUIDs, integers, hashes, or other values that are auto-generated.

## JSON responses

RESTful APIs can consume and deliver data in multiple possible formats such as plain text, HTML, XML, YAML, and JSON.  While it's possible for multiple formats, it's generally accepted that JSON (JavaScript Object Notation) is the commonly chosen data format.  JSON should be use for both response represenations as well as request body input.

### Property naming conventions

When using JSON, follow JavaScript naming convention of camelCase for property names.

### Pretty print by default & ensure gzip is supported

An API that pretty prints by default is much more approachable. The cost of the extra data transfer is negligible, especially when you compare to the cost of not implementing gzip.

## HTTP Statuses

Since REST APIs depend upon HTTP standards, each request’s status is used to communicate the result of the request, such as success or failure. Each status code provides a machine-readable response, plus a human-readable message.

* 200: Success
* 201: Created
* 401: Unauthorized
* 403: Forbidden
* 404: Not found

A larger applicable list of statuses commonly used HTTP statuses can be found [here](HTTPStatusCodes.md).

## Updates and creation should return a resource representation

PUT, POST or PATCH requests may make modifications to properties of the underlying resource that weren't part of the provided parameters (for example: createdDate or lastModifiedDate timestamps). To prevent an API consumer from having to hit the API again for an updated representation, have the API return the updated (or created) representation as part of the response.

In case of a POST that resulted in a creation, use a HTTP 201 status code and include a Location header that points to the URL of the new resource. Both of those in should be in addition to including the newly created resource representation as the body of the response.

## Versioning

APIs should always be versioned.  This is true even when it's not expected to change often or at all as change is inevitable.  An API version is a specifc contract with it's consumers, when a change is needed in the request or response shape or with it's action a new version should be created.  An API might support multiple versions a time to allow consumers time to use the new version.

There are multiple ways to version an API, including by URL as part of the path or query string as well as by header.  To support browser explorability, versioning by path in URL is the preferred method.

## Result filtering, sorting & searching

It's best to keep the base resource URLs as lean as possible. Complex result filters, sorting requirements and advanced searching (when restricted to a single type of resource) can all be easily implemented as query parameters on top of the base URL. Let's look at these in more detail:

### Filtering

Use a unique query parameter for each property that implements filtering. For example, when requesting a list of tickets from the /tickets endpoint, you may want to limit these to only those in the open state. This could be accomplished with a request like GET /tickets?state=open. Here, state is a query parameter that implements a filter.

### Sorting

Similar to filtering, a sort query parameter can be used to denote how to sort the results. Multiple sorting arguments can be handled by letting the sort parameter take in list of comma separated fields.  Prefix each sorting argument with a negative to denote descending sort order.

For example:

* GET /tickets?sort=-priority - Retrieves a list of tickets in descending order of priority
* GET /tickets?sort=-priority,createdDate - Retrieves a list of tickets in descending order of priority. Within a specific priority, older tickets are ordered first

### Aliases for common queries

To simplify common searches, an alias endpoint can be created.  For example, the recently closed tickets query above could be packaged up as GET /tickets/recentlyclosed

## Documentation

A good API will provide documentation that is easy to locate and be as assessible as the API itself.  The documentation should show examples of complete request/response cycles. Preferably, the requests should be pastable examples - either links that can be pasted into a browser or curl examples that can be pasted into a terminal.

## Caching

HTTP provides a built-in caching framework that can be taken advantage of by including some additional outbound response headers and do a little validation when you receive some inbound request headers.

## Errors

An API should provide a useful error message in a known consumable format. The representation of an error should be no different than the representation of any resource, just with its own set of fields. 

The API should always return sensible HTTP status codes. API errors typically break down into 2 types: 400 series status codes for client issues & 500 series status codes for server issues. At a minimum, the API should standardize that all 400 series errors come with consumable JSON error representation.

JSON output representation for something like this would look like:

```json
{
  "errors": [
    {
      "type": "BadError",
      "message" : "Something bad happened"
    }
  ]
}
```

Validation errors for PUT, PATCH and POST requests will need a field/property breakdown. This is best modeled by populating a value for the offending property, like so:

```json
{
  "errors": [
    {
      "type": "ValidationError",
      "property" : "firstName",
      "message" : "First name cannot have fancy characters"
    },
    {
       "type": "PasswordValidationError", 
       "property" : "password",
       "message" : "Password cannot be blank"
    }
  ]
}
```

In the case of unhandled exceptions, it's possible to expose the exception details for non-production environments, like this:

```js
{
    "errors": [
        {
            "type": "InternalServerErrorResponseException",
            "message": "Unhandled exception",
            "exception": {
                "ClassName": "Cortside.Common.Messages.MessageExceptions.InternalServerErrorResponseException",
                "Message": "Unhandled exception",
                "Data": null,
                "InnerException": {
                    "ClassName": "Cortside.Common.Messages.MessageListException",
                    "Message": "One or more error occurred.",
                    "Data": null,
                    "InnerException": null,
                    "HelpURL": null,
                    "StackTraceString": "   at Cortside.Common.Messages.MessageList.ThrowIfAny[T]()\r\n   at Acme.ShoppingCart.Domain.Entities.Customer.Update(String firstName, String lastName, String email) in C:\\Work\\cortside\\coeus\\shoppingcart-api\\src\\Acme.ShoppingCart.Domain\\Entities\\Customer.cs:line 36\r\n   at Acme.ShoppingCart.Domain.Entities.Customer..ctor(String firstName, String lastName, String email) in C:\\Work\\cortside\\coeus\\shoppingcart-api\\src\\Acme.ShoppingCart.Domain\\Entities\\Customer.cs:line 13\r\n   at Acme.ShoppingCart.DomainService.CustomerService.CreateCustomerAsync(CustomerDto dto) in C:\\Work\\cortside\\coeus\\shoppingcart-api\\src\\Acme.ShoppingCart.DomainService\\CustomerService.cs:line 26\r\n   at Acme.ShoppingCart.Facade.CustomerFacade.CreateCustomerAsync(CustomerDto dto) in C:\\Work\\cortside\\coeus\\shoppingcart-api\\src\\Acme.ShoppingCart.Facade\\CustomerFacade.cs:line 23\r\n   at Acme.ShoppingCart.WebApi.Controllers.CustomerController.CreateCustomerAsync(CreateCustomerModel input) in C:\\Work\\cortside\\coeus\\shoppingcart-api\\src\\Acme.ShoppingCart.WebApi\\Controllers\\CustomerController.cs:line 125\r\n   at Microsoft.AspNetCore.Mvc.Infrastructure.ActionMethodExecutor.TaskOfIActionResultExecutor.Execute(IActionResultTypeMapper mapper, ObjectMethodExecutor executor, Object controller, Object[] arguments)\r\n   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeActionMethodAsync>g__Logged|12_1(ControllerActionInvoker invoker)\r\n   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeNextActionFilterAsync>g__Awaited|10_0(ControllerActionInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)\r\n   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Rethrow(ActionExecutedContextSealed context)\r\n   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Next(State& next, Scope& scope, Object& state, Boolean& isCompleted)\r\n   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.InvokeInnerFilterAsync()\r\n--- End of stack trace from previous location ---\r\n   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeNextExceptionFilterAsync>g__Awaited|26_0(ResourceInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)",
                    "RemoteStackTraceString": null,
                    "RemoteStackIndex": 0,
                    "ExceptionMethod": null,
                    "HResult": -2146233088,
                    "Source": "Cortside.Common.Messages",
                    "WatsonBuckets": null
                },
                "HelpURL": null,
                "StackTraceString": null,
                "RemoteStackTraceString": null,
                "RemoteStackIndex": 0,
                "ExceptionMethod": null,
                "HResult": -2146233088,
                "Source": null,
                "WatsonBuckets": null
            }
        }
    ]
}
```
