# Errors

## Generate structured errors

In case of validation errors, the response body would be:

```text
HTTP/1.1 400 Bad Request
```

```json
{
    "errors": [
        {
            "type": "InvalidOrderStateMessage",
            "message": "Order is not in a valid state"
        }
    ]
}
```

Generate consistent, structured response bodies concerning errors.  Errors collection may contain one or more errors.

* type - error code in a string representation; it can be an exception name i.e. `InvalidOrderStateMessage`,
* message - internal error message for a developer

Messages should not be ended with a dot or comma. Some clients might want to concatenate all user messages in to one big message and they should be able to decide
if comas, dots or HTML list tags are the appropriate separators for their UI.

A developer can use `Trace-Id` header from a response to determine the exact flow for a given error in micro-services.
