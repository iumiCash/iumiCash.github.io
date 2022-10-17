# Requests

## API requests

In order to retrieve, submit, update or delete data, a REST API request is created. 
A typical REST API request includes HTTP methods (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`), 
the URL to the API service and the URI with query parameters. 
A REST API request may include one or more HTTP request headers.

Optionally, `GET` calls may include query parameters to filter, limit the size of, and sort the data in the responses. 
`GET` calls do not include JSON request body.

Most `POST`, `PUT`, and `PATCH` calls require a JSON request body.

## URL

iumiCash offers multiple URL's to its API services, which are provided by iumiCash together with credentials:

* Sandbox URL: used for testing purposes only and does not affect production data.
* Production URL: used for real data operations.

## HTTP request headers

`Authorization` *string*
:    The identifier (such as a bearer token) is required to validate a third-party system to access a resource.
     To make REST API calls, include the bearer token in this header with the `Bearer` authentication scheme. 
     The value is `Bearer <Access-Token>`. To get `access token` see [access token].

`Content-Type` *string*
:    The media type is required for operations with a request body. The value is `application/<format>`, where format is `json`.

`RequestId` *string*
:    The idempotency key is used to correlate request payloads with response payloads. See [idempotency] for more information.



[idempotency]: idempotency.md
[access token]: authentication/token.md#generate-access-token-api
