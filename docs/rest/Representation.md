# Representation

## JSON as request and response bodies

All data sent to and received from the api should be as JSON.  Using the same format between request and response will keep symmetry between them.

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

## Property name format

Use camel case in parameters and properties (e.g. `firstName`) instead of snake case (e.g. `first_name`).  Array types should have plural property names.

```json
{ 
  "firstName": "John",
  "lastName": "Doe",
  "emailAddresses": [
    "john.doe@gmail.com",
    "johnd@acme.com"
  ]
}
```

## Resource Identifiers

Each resource should have a unique identifier. Use UUIDs (aka GUIDs) unless you
have a very good reason not to. Do not use Ids that will not be globally unique across instances of the service or other resources in the service, especially auto-incrementing Ids.

Render UUIDs as a lowercase string in `8-4-4-4-12` format, e.g.:

```json
{
  "userId": "01234567-89ab-cdef-0123-456789abcdef"
}
```

## Null values

Absence of or unknown value should be exposed as `null` instead of an empty string or omitting the property.

Example of an unknown value or state:

```json
{ 
  "firstName": "John",
  "lastName": "Doe",
  "company": null,
  "emailAddresses": [
    "john.doe@gmail.com",
    "johnd@acme.com"
  ]
}
```

## Empty collections

Collections should be returned as an empty collection instead of `null`. Some client frameworks cannot iterate over `null` and need extra null checking.  Iteration over `[]` is always possible.

```json
{ 
  "firstName": "John",
  "lastName": "Doe",
  "company": null,
  "emailAddresses": []
}
```

## Use UTC times formatted in ISO8601

Accept and return times in UTC only. Render times as a string [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) format (`yyyy-MM-ddTHH:mm:ss.SSSZ`)

Example:

```json
{
  // ...
  "createdDate": "2022-01-16T17:52:52.848Z",
  "lastModifiedDate": "2022-01-16T17:52:52.848Z",
  // ...
}
```

The consumer is responsible for doing any time zone conversion that may be needed.

## Enum values

Enum values are represented as camel cased strings.

C# code:

```csharp
public enum Color {
  Red,
  Yellow,
  Blue,
  NavyBlue
}
```

Json:

```json
{
  "color": "navyBlue"
}
```

## Nesting foreign resources relations

Nest foreign resources references, even if the only information is the resource identifier of the object referred to, with a nested object, e.g.:

```json
{
  "orderId": "01234567-89ab-cdef-0123-456789abcdef",
  "status": "shipped",
  "merchant": {
    "merchantId": "132ce4ff-97d9-320a-9138-548f1ccb5378",
    "name": "ACME Corporation"
  },
  "customer": {
    "customerId": "b622e839-8674-4832-a358-577476b15953"
  }
  // ...
}
```

Instead of a flat structure e.g.:

```json
{
  "orderId": "01234567-89ab-cdef-0123-456789abcdef",
  "status": "shipped",
  "merchantId": "132ce4ff-97d9-320a-9138-548f1ccb5378",
  "merchantName": "ACME Corporation",
  "customerId": "b622e839-8674-4832-a358-577476b15953"
  // ...
}
```

This will make it possible to add additional properties from the foreign resource without having to change any existing properties or duplicating properties.

```json
{
  "orderId": "01234567-89ab-cdef-0123-456789abcdef",
  "status": "shipped",
  "merchant": {
    "merchantId": "132ce4ff-97d9-320a-9138-548f1ccb5378",
    "name": "ACME Corporation"
  },
  "customer": {
    "customerId": "b622e839-8674-4832-a358-577476b15953",
    "email": "wile.e.coyote@gmail.com"
  }
  // ...
}
```

## Pretty print by default & ensure gzip is supported

An API that pretty prints by default is much more approachable. The cost of the extra data transfer is negligible, especially when you compare to the cost of not implementing gzip.

```bash
curl https://www.acme.com/api/v1/users/01234567-89ab-cdef-0123-456789abcdef \
    -H "Accept-Encoding: gzip"
```

```json
{ 
  "firstName": "John",
  "lastName": "Doe",
  "company": null,
  "emailAddresses": [
    "john.doe@gmail.com",
    "johnd@acme.com"
  ]
}
```

Instead of e.g.:

```json
{"firstName":"John","lastName":"Doe","company":null,"emailAddresses":["john.doe@gmail.com","johnd@acme.com"]}
```

## Filtering

### Simple filters in query string

Use a unique query string parameter for each property that implements filtering.  For example, when requesting a list of orders from the /orders endpoint, you may want to limit these to only those by status and delivery address state.

This could be accomplished with a request like:

```bash
curl -X GET https://www.acme.com/api/v1/orders?status=shipped&deliveryAddress.state=UT
```

Simple rules for query string filters:

* refer directly to collection entities (`status`, not `orders.status`)
* use "." (dot) to indicate a (unique) field name in nested structure (`deliveryAddress.state`, not just `state`)

Sample response:

```json
{
  "results":[
    {
      "orderId":"01234567-89ab-cdef-0123-456789abcdef",
      "status":"shipped",
      "merchant":{
        "merchantId":"132ce4ff-97d9-320a-9138-548f1ccb5378",
        "name":"ACME Corporation"
      },
      "customer":{
        "customerId":"b622e839-8674-4832-a358-577476b15953"
      }
    }
  ]
}
```

## Sorting

Similar to filtering, a sort query parameter can be used to denote how to sort the results. Multiple sorting arguments can be handled by letting the sort parameter take in list of comma separated fields.  Prefix each sorting argument with a negative to denote descending sort order.

For example:

* GET /tickets?sort=-priority - Retrieves a list of tickets in descending order of priority
* GET /tickets?sort=-priority,createdDate - Retrieves a list of tickets in descending order of priority. Within a specific priority, older tickets are ordered first

### Aliases for common queries

To simplify common searches, an alias endpoint can be created.  For example, the recently closed tickets query above could be packaged up as:

* GET /tickets/recentlyclosed - Retrieves the list of tickets that have recently closed within the last 24 hours

## Wrap collection in object

Always return root element as an object. This way you can add extra `metadata` fields to the response without breaking compatibility.

For example, in the case of the `offers` collection, you can add a metadata field such as `count` containing a number of matching offers:

```json
{
  "itemCount":1,
  "pageNumber":1,
  "pageSize":10,
  "pageCount":1,
  "results":[
    {
      "orderId":"01234567-89ab-cdef-0123-456789abcdef",
      "status":"shipped",
      "merchant":{
        "merchantId":"132ce4ff-97d9-320a-9138-548f1ccb5378",
        "name":"ACME Corporation"
      },
      "customer":{
        "customerId":"b622e839-8674-4832-a358-577476b15953"
      }
    }
  ]
}
```

When a collection is in the root, you cannot add extra field there.

```json
[
  {
    "orderId":"01234567-89ab-cdef-0123-456789abcdef",
    "status":"shipped",
    "merchant":{
      "merchantId":"132ce4ff-97d9-320a-9138-548f1ccb5378",
      "name":"ACME Corporation"
    },
    "customer":{
      "customerId":"b622e839-8674-4832-a358-577476b15953"
    }
  }
]
```

Metadata should only contain direct properties of the response set, not properties of the members of the response set.

## Paging

Similar to filtering and sorting, query parameters for `pageSize` and `pageNumber` can be used to limit the number of results and where in the results to start from.

For example:

* GET /tickets?pageNumber=5&pageSize=100
