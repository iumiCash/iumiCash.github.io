# Responses

## API responses

iumiCash API calls return HTTP status codes. Some API calls also return JSON response bodies 
that include information about the resource including one or more contextual [HATEOAS] links. 
You can use these links to request more information about or some other prebuilt links to work with resources. 

## HTTP status codes

### Successful requests

For successful requests, iumiCash returns `HTTP 2xx` status codes.

???+ info "HTTP 2xx"
    
    `HTTP 2xx` status code description

    | Status code | Description |
    | ----------- | ----------- |
    | `HTTP 200 OK`  | The request succeeded. |
    | `HTTP 201 Created`  | A POST method successfully created a resource. |
    | `HTTP 204 No Content`  | The server successfully executed the method but returned no response body. |


### Failed requests

For failed requests, iumiCash returns `HTTP 4xx` status codes if something passed 
in the request has an error or `HTTP 5xx` status codes when something is wrong on our end 
with a server or service.

For authentication specific `HTTP 4xx` status codes, see [Authorization errors].

???+ info "Response"

    `error` *string*
    
    :    Error code.

    `description` *string*
    
    :    Human-readable error description.

    `field_errors` *list of [*object*](#field-error)*
    
    :    List of errors. 
    
???+ info "HTTP 4xx"

    `HTTP 4xx` status code description
    
    | Status code | Description | Possible causes and solutions | 
    | ----------- | ----------- | ----------------------------- |
    | `HTTP 400 Bad Request`  | The request is not well-formed, syntactically incorrect, or violates schema. | See [Validation errors]. The server could not understand the request. Can be one of this: <ul><li>The API cannot convert the payload data</li><li>The data is not in the expected data format</li><li>A required field is not available</li><ul> | 
    | `HTTP 404 Not Found` | A specified resource does not exist | The server did not find anything that matches the requested URI. Either the URI is incorrect or the resource is not available. For example, no data exists in the database at that key. |
    | `HTTP 405 Method Not Allowed` | The server does not implement the requested HTTP method. | The service or resource does not support the requested HTTP method. |
    | `HTTP 422 Unprocessable Entity` | The API cannot complete the requested action, or the request action is semantically incorrect or fails business validation. | The server could not understand the request schema. |
    
???+ info "HTTP 5xx"

    `HTTP 5xx` status code description
    
    | Status code | Description | Possible causes and solutions | 
    | ----------- | ----------- | ----------------------------- |
    | `HTTP 500 Internal Server Error`  | An internal server error has occurred. | A system or application error occurred. Although the client appears to provide a correct request, something unexpected occurred on the server. | 
    | `HTTP 503 Service Unavailable` | Service Unavailable. | The server cannot handle the request for a service due to temporary maintenance. |
    


### Authorization errors

iumiCash follows industry standard OAuth 2.0 authorization protocol and returns the 
`HTTP 400`, `HTTP 401`, `HTTP 403` status codes for authorization errors. 

!!! Tip

    Most of them usually access token-related issues and can be cleared by making sure 
    that the token is present and hasn't expired.

???+ info "HTTP 4xx"

    `HTTP 4xx` status code description
    
    | Status code | Description | Possible causes and solutions | 
    | ----------- | ----------- | ----------------------------- |
    | `HTTP 400 Bad Request`  | For example: <ul><li>Incorrect response_type</li><li>Unsupported grant_type</li><li>Incorrect redirect_uris etc.</li></ul> | Check your request body or see possible data in specified resourse. | 
    | `HTTP 401 Unauthorized` | For example: <ul><li>Authorization header is not present</li><li>Invalid client credentials</li><li>Invalid refresh_token</li><li>Invalid authorization code etc</li></ul> |
    | `HTTP 403 Forbidden` | Authorization failed due to insufficient permissions | Make sure that you have access to this resource. |

### Validation errors

For validation errors, iumiCash returns the `HTTP 400 Bad Request` status code.

!!! Tip

    To prevent validation errors ensure that parameters are the right type and conform to constraints.

### Examples

???+ example "Response"
    
    Example error response.

    !!! danger
        
        Be carefull! For `HTTP 5xx` status codes, iumiCash can send **NOT** `application/json` response type. It may happen when iumiCash servers somehow is down. `HTTP 5xx` status codes in most cases sent by `reverse proxies` e.g. [nginx](https://nginx.org/), [traefik](https://traefik.io/) in `text/plain` response type.

    ```
    {
        "error": "bad_request",
        "description": "Some reasonable error message",
        "field_errors": [
            {
                "field": "field_name",
                "message": "field error message"
            }
        ]
    }
    ```

## Objects

### field error

???+ info

    `field` *string*
    
    :    Field where error happens.

    `message` *string*
    
    :    Validation message for this message.

    
[HATEOAS]: orders/create_order.md#hateoas
[Authorization errors]: #authorization-errors
[Validation errors]: #validation-errors
