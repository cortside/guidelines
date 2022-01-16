# Resource

The key abstraction of information in REST is a resource. Any information that can be named can be a resource: a user, an order, a shipment, a payment, a collection of other resources, etc. Usually a resource is something that can be persisted and retrieved or is the result of a calculation or algorithm.

## Name

Use Nouns when naming resources instead of verbs or adjectives. HTTP methods are the verbs (i.e. GET, POST, PUT, DELETE) that will act on the nouns.  The names should represent the contents of a resource or the resources themselves. Hence, it makes more sense to actually use nouns instead.

Use the plural version of a resource name to be consistent when referring to particular resources, e.g.:
```
/users
/orders
/shipments
/transactions
/payments
```

## Resource Identifiers

Use UUIDs (aka GUIDs) unless you have a very good reason not to. Do not use Ids that will not be globally unique across instances of the service or other resources in the service, especially auto-incrementing Ids.

Provide UUIDs as a lowercase string in `8-4-4-4-12` format, e.g.:

```
01234567-89ab-cdef-0123-456789abcdef
```

## Lowercase paths

Use lowercase resource names, e.g.:

```
/shippingfees
/communicationmethods
```

## Resource child data

Use the resource identifier as part of the path for entity owned children.

i.e.:
```
/users/{userId}/repositories
/orders/{orderId}/shipments
```

Because orders for users A and B are different, these collections should have different URLs.

## Minimize resources nesting

In data models with nested parent/child resource relationships, paths
may become deeply nested, e.g.:

```javascript
/users/{userId}/orders/{orderId}/shipments/{shipmentId}
```

Limit nesting depth by preferring to locate resources at the root
path. Use nesting to indicate scoped collections. For example, for the
case above where shipping belongs to an offer:

```javascript
/users/{userId}
/users/{userId}/orders
/orders/{orderId}
/orders/{orderId}/shipments
/shipments/{shipmentId}
```

## Versioning

To prevent unexpected breaking changes to users, all resources should be versioned.  A version should always required and no default version should be assumed.

Versioning should be handled via the resource name (URI).  This supports easier discovery via browser and simpler support for caching.

```
https://www.acme.com/api/v1/products
```

## Varied data formats (representations)

If you need different representations of the same object, construct different resource names to distinguish difference.

To get only public data use:
```
GET /orders
```
To get order information that is specific to merchants:
```
GET /merchantorders
GET /merchant/orders
```
To get orders that are in the process of being fulfilled:
```
GET /openorders
```

## Use HTTP methods to operate on collections and entities

The HTTP methods are the verbs that act on the noun-based resources.  The most commonly used HTTP methods are POST, GET, PUT, and DELETE. These correspond to create, read, update, and delete (or CRUD) operations, respectively.

| Resource / HTTP method | POST (create)    | GET (read)  | PUT (update)           | DELETE (delete)    |
| ---------------------- | ---------------- | ----------- | ---------------------- | ------------------ |
| /users                 | Create new user  | List users  | Error                  | Error              |
| /users/{userId}          | Error            | Get user    | Update user if exists  | Delete user        |

### Update and create must return a resource representation

`PUT` and `POST` methods may modify fields of the underlying resource that were not part of the provided request, including system generated values such as resourceId and audit stamp properties. To prevent an API consumer from having to call the API again for an updated representation, have the API return the updated (or created) representation as part of the response.

#### Create entity

* server requires all entity properties to be located in a request body

Sample request:

```bash
curl -X POST https://www.acme.com/api/v1/users \
    -d
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@gmail.com"
}
```

Sample response:

* server returns the `HTTP 201 Created` status code
* whole entity is included in a body, including system generated values such as resourceId and audit stamp properties
* response will also include the `Location` header that points to the URL of the new resource ([RFC 6570](http://tools.ietf.org/html/rfc6570))
  * header `Location: https://www.acme.com/api/v1/users/01234567-89ab-cdef-0123-456789abcdef`

```json
{
  "userId": "01234567-89ab-cdef-0123-456789abcdef",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@gmail.com",
  "createdDate": "2022-01-16T17:52:52.848Z",
  "createdSubject": {
    "subjectId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "userPrincipalName": "efudd"
  },
  "lastModifiedDate": "2022-01-16T17:52:52.848Z",
  "lastModifiedSubject": {
    "subjectId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "userPrincipalName": "efudd"
  },
}
```

#### Get entities (collection)

Sample request:

```bash
curl https://www.acme.com/api/v1/users
```

Sample response:

* server returns the `HTTP 200 Ok` status code
* response should contained entities as an array
  * results should be in a wrapped object, to ease consumption of responses

```json
{
  "results": [
    {
      "userId":"01234567-89ab-cdef-0123-456789abcdef",
      "firstName":"John",
      "lastName":"Doe",
      "email":"john.doe@gmail.com",
      "createdDate":"2022-01-16T17:52:52.848Z",
      "createdSubject":{
        "subjectId":"3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "userPrincipalName":"efudd"
      },
      "lastModifiedDate":"2022-01-16T17:52:52.848Z",
      "lastModifiedSubject":{
        "subjectId":"3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "userPrincipalName":"efudd"
      }
    }
  ]
}
```

### Get selected entity

Sample request:

```bash
curl https://www.acme.com/api/v1/users/{id}
```

Sample response:

* server returns the `HTTP 200 Ok` status code
* server returns an entity in the established version

```json
{
  "userId": "01234567-89ab-cdef-0123-456789abcdef",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@gmail.com",
  "createdDate": "2022-01-16T17:52:52.848Z",
  "createdSubject": {
    "subjectId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "userPrincipalName": "efudd"
  },
  "lastModifiedDate": "2022-01-16T17:52:52.848Z",
  "lastModifiedSubject": {
    "subjectId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "userPrincipalName": "efudd"
  },
}
```

### Update entity

`PUT` and `POST` methods may modify fields of the underlying resource that were not part of the provided request (for example: `createdDate` or `lastModifiedDate` timestamps). To prevent an API consumer from having to call the API again for an updated representation, have the API return the updated (or created) representation as part of the response.

* to update an entity, the server requires being provided with all the properties that can be subject to update

Sample request:

```bash
curl -X PUT https://www.acme.com/api/v1/users/01234567-89ab-cdef-0123-456789abcdef \
    -d
{
  "firstName": "John",
  "lastName": null,
  "email": "john@gmail.com"
}
```

Sample response:

* server returns the `HTTP 200 Ok` status code
* server returns an updated entity in the established version

```json
{
  "userId": "01234567-89ab-cdef-0123-456789abcdef",
  "firstName": "John",
  "lastName": null,
  "email": "john@gmail.com",
  "createdDate": "2022-01-16T17:52:52.848Z",
  "createdSubject": {
    "subjectId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "userPrincipalName": "efudd"
  },
  "lastModifiedDate": "2022-01-16T17:52:52.848Z",
  "lastModifiedSubject": {
    "subjectId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "userPrincipalName": "efudd"
  },
}
```

### Delete entity

Sample request:

```bash
curl -X DELETE https://www.acme.com/api/v1/users/{userId}
```

Sample response:

* server returns the `HTTP 204 No Content` status code
* response body content is empty

## Return appropriate status codes

Return appropriate HTTP status codes with each response.  HTTP Status Codes are grouped into the following groupings:

* 2xx Success
* 3xx Redirection
* 4xx Client Error
* 5xx Server Error

See the follow table for specifics of which HTTP status code for the appropriate situations, [HTTP Status Codes](HTTP-STATUS-CODES.md).

### Validate errors

For error validation, return `HTTP 400 Bad Request` response body with a list of errors.  Sample error validation response:

```json
{
    "errors": [
        {
            "type": "InvalidOrderStateMessage",
            "message": "Order is not in a valid state."
        }
    ]
}
```
