# Requests

## API requests

To make a REST API request, you combine the HTTP `GET`, `POST`, `PUT`, `PATCH`, or `DELETE` method, 
the URL to the API service, the URI to a resource to query, submit data to, update, or delete,
and one or more HTTP request headers.

!!! Tip
    The URL to the API service is either:

    * Sandbox: `https://sandbox-api.iumi.cash`
    * Production: `https://api.iumi.cash`

Optionally, you can include `query parameters` on `GET` calls to filter,
limit the size of, and sort the data in the responses.

!!! Tip

    Most `POST`, `PUT`, and `PATCH` calls require a JSON request body.


## HTTP request headers

`Authorization` *string*
:    To make REST API calls, include the bearer token in this header with the `Bearer` authentication scheme. 
     The value is `Bearer <Access-Token>`. To get `access token` see [access token].

`Content-Type` *string*
:    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.

`RequestId` *string*
:    Idempotency key for request. See [idempotency] for more information.


[idempotency]: idempotency.md
[access token]: authentication/token.md#generate-access-token-api
