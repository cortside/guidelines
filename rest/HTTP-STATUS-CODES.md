# Http Status Codes

[W3C HTTP Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

[IANA HTTP Status Code Registry](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)

[HTTP Status Codes](http://www.restapitutorial.com/httpstatuscodes.html)

[Wikipedia HTTP Status Codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

## 2xx Success

This class of status code indicates that the client's request was successfully received, understood, and accepted.

| Code | Name | Description | Use Notes |
|---|---|---|---|
| 200 | OK | The request has succeeded. | Response to a successful `GET`, `PUT`, `PATCH` request. An existing resource is updated synchronously.|
| 201 | Created | The request has been fulfilled and resulted in a new resource being created. | Response to a `POST` request that results in entity creation.  It should be combined with a `Location` header pointing to the location of the new resource. |
| 202 | Accepted | The request has been accepted for processing, but the processing has not been completed. The request might or might not eventually be acted upon, as it might be disallowed when processing actually takes place. | Accepted request concerning a `POST`, `PUT`, `DELETE`, or `PATCH` request that will be processed asynchronously. |
| 204 | No Content | The server has fulfilled the request but does not need to return an entity-body, and might want to return updated metainformation. The response MAY include new or updated metainformation in the form of entity-headers, which if present SHOULD be associated with the requested variant. | Response to a successful request that will return empty body (as a `DELETE` request) |

## 3xx Redirection

This class of status code indicates that further action needs to be taken by the user agent in order to fulfill the request. The action required MAY be carried out by the user agent without interaction with the user if and only if the method used in the second request is GET or HEAD. A client SHOULD detect infinite redirection loops, since such loops generate network traffic for each redirection.

| Code | Name | Description | Use Note |
|---|---|---|---|
| 303 | SeeOther | The response to the request can be found under a different URI and SHOULD be retrieved using a GET method on that resource. This method exists primarily to allow the output of a POST-activated script to redirect the user agent to a selected resource. The new URI is not a substitute reference for the originally requested resource. The 303 response MUST NOT be cached, but the response to the second (redirected) request might be cacheable. |
| 304 | NotModified | If the client has performed a conditional GET request and access is allowed, but the document has not been modified, the server SHOULD respond with this status code. The 304 response MUST NOT contain a message-body, and thus is always terminated by the first empty line after the header fields. | Used when HTTP caching headers are in use (e.g. etags) |

## 4xx Client Error

The 4xx class of status code is intended for cases in which the client seems to have erred. Except when responding to a HEAD request, the server SHOULD include an entity containing an explanation of the error situation, and whether it is a temporary or permanent condition. These status codes are applicable to any request method. User agents SHOULD display any included entity to the user.

| Code | Name | Description | Use Notes |
|---|---|---|---|
| 400 | Bad Request | The request could not be understood by the server due to malformed syntax. The client SHOULD NOT repeat the request without modifications. | Generic error response to `POST`, `PUT`, `PATCH`, `DELETE` requests. 
| 401 | Unauthorized | The request requires user authentication. The response MUST include a WWW-Authenticate header field (section 14.47) containing a challenge applicable to the requested resource. The client MAY repeat the request with a suitable Authorization header field (section 14.8). If the request already included Authorization credentials, then the 401 response indicates that authorization has been refused for those credentials. If the 401 response contains the same challenge as the prior response, and the user agent has already attempted authentication at least once, then the user SHOULD be presented the entity that was given in the response, since that entity might include relevant diagnostic information. HTTP access authentication is explained in "HTTP Authentication: Basic and Digest Access Authentication" [43]. | Request failed because user is not authenticated |
| 403 | Forbidden | The server understood the request, but is refusing to fulfill it. Authorization will not help and the request SHOULD NOT be repeated. If the request method was not HEAD and the server wishes to make public why the request has not been fulfilled, it SHOULD describe the reason for the refusal in the entity. If the server does not wish to make this information available to the client, the status code 404 (Not Found) can be used instead. | Request failed because user does not have authorization to access a specific resource |
| 404 | Not Found | The server has not found anything matching the Request-URI. No indication is given of whether the condition is temporary or permanent. The 410 (Gone) status code SHOULD be used if the server knows, through some internally configurable mechanism, that an old resource is permanently unavailable and has no forwarding address. This status code is commonly used when the server does not wish to reveal exactly why the request has been refused, or when no other response is applicable. | Requesting for a resource that does not exist.  A resource does not exist if any part of it does not exist.  For example, if order 1 does not exist, this resource must return 404: /orders/1/items |
| 412 | Precondition Failed | The precondition given in one or more of the request-header fields evaluated to false when it was tested on the server. This response code allows the client to place preconditions on the current resource metainformation (header field data) and thus prevent the requested method from being applied to a resource other than the one intended. | This response happens when condition defined by headers such as `If-Matched` is not fulfilled.
| 415 | Unsupported Media Type | The origin server is refusing to service the request because the payload is in a format not supported by this method on the target resource. | The format problem might be due to the request's indicated `Content-Type` or `Content-Encoding`, or as a result of inspecting the data directly.
| 422 | Unprocessable Entity | The server understands the content type of the request entity (hence a 415 Unsupported Media Type status code is inappropriate), and the syntax of the request entity is correct (thus a 400 Bad Request status code is inappropriate) but was unable to process the contained instructions. | Request failed because server is unable to process the request. One possible reason of this is that the request entity is in an invalid state. 
| 423 | Locked | The source or destination resource of a method is locked. | This response SHOULD contain an appropriate precondition or postcondition code, such as 'lock-token-submitted' or 'no-conflicting-lock'.  The ability to keep more than one person from working on a   document at the same time.  This prevents the "lost update problem", in which modifications are lost as first one author, then another, writes changes without merging the other author's changes.|
| 428 | Precondition Required | The origin server requires the request to be conditional. | Its typical use is to avoid the "lost update" problem, where a client GETs a resource's state, modifies it, and PUTs it back to the server, when meanwhile a third party has modified the state on the server, leading to a conflict. By requiring requests to be conditional, the server can assure that clients are working with the correct copies. |

Client errors may contain more details in the body. Below is an example:
```
{
    "errors": [
        {
            "type": "InvalidOrderStateMessage",
            "message": "Order is not in a valid state."
        }
    ]
}
```
Each error has a `type` and a `message`, where `type` is the error type, and `message` is the error description.


## 5xx Server Error

Response status codes beginning with the digit "5" indicate cases in which the server is aware that it has erred or is incapable of performing the request. Except when responding to a HEAD request, the server SHOULD include an entity containing an explanation of the error situation, and whether it is a temporary or permanent condition. User agents SHOULD display any included entity to the user. These response codes are applicable to any request method.

| Code | Name | Description |
|---|---|---|---|
| 500 | InternalServerError | The server encountered an unexpected condition which prevented it from fulfilling the request. |
| 503 | ServiceUnavailable | The server is currently unable to handle the request due to a temporary overloading or maintenance of the server. The implication is that this is a temporary condition which will be alleviated after some delay. If known, the length of the delay MAY be indicated in a Retry-After header. If no Retry-After is given, the client SHOULD handle the response as it would for a 500 response. |





## Notes to be incorporated:
```
Cort [3:54 PM]
imagine a scenario where you have to cancel an order where when you cancel you have to supply a reason.  The reasons are associated with "permissions" and a user must have that permission in order to be able to use that reason when cancelling.  If a user were to choose a reason they have permission for the operation would succeed.  If they do not, the operation would return an http 4.....what?  403?

Steve [3:55 PM]
400 bad request
```
