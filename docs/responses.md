# Responses

## API responses

iumiCash API returns HTTP status codes in response to calls. Some API calls also return JSON response bodies 
that include information about the resource including one or more contextual [HATEOAS] links.
HATEOAS links can be used to request further information or submit calls to operate with data based on prebuilt links returned by API. 

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

Failed requests return either `HTTP 4xx` status codes or `HTTP 5xx` status codes depending on the cause of the failure.

???+ info "Response"

    `error` *string*
    
    :    Error code.

    `description` *string*
    
    :    Human-readable error description.

    `field_errors` *list of [*object*](#field-error)*
    
    :    List of errors. 


For failed requests that are returned by iumiCash API the returned HTTP status codes are in the range of 4xx. 
For authentication specific `HTTP 4xx` status codes, see [Authorization errors].
    
???+ info "HTTP 4xx"

    `HTTP 4xx` status code description
    
    | Status code | Error code | Description | 
    | ----------- | ----------- | ----------------------------- |
    | `HTTP 400 Bad Request` | `USER_PAYMENT_LIMIT` | The user applied amount limits that do not allow to process this request.  |
    | `HTTP 400 Bad Request` | `USER_NOT_ACTIVE` | The user is no longer active in iumiCash. |
    | `HTTP 400 Bad Request` | `ACCOUNT_BALANCE_IS_LOW` | The user does not have sufficient iumiCash balance to fulfill this transaction. |
    | `HTTP 400 Bad Request` | `DISABLED_BY_USER` | The user restricted the vendor to process this request. |
    | `HTTP 400 Bad Request`  | `INVALID_REQUEST` | The server could not understand the request. Can be one of this: <ul><li>The API cannot convert the payload data.</li><li>The data is not in the expected data format.</li><li>A required field is not available.</li><ul> | 
    | `HTTP 404 Not Found` | `RESOURCE_NOT_FOUND` | The server did not find anything that matches the requested URI. Either the URI is incorrect, or the resource is not available. For example, no data exists in the database with that key. |
    | `HTTP 405 Method Not Allowed` | `METHOD_NOT_SUPPORTED` | The service or the resource does not support the requested HTTP method. |
    | `HTTP 422 Unprocessable Entity` | `UNPROCESSIBLE_ENTITY` | The API cannot complete the requested action due to business validation or other issues. |

For failed requests that are returned by the API server the returned HTTP status codes are in the range of 5xx. For example, such errors may occur when iumiCash API service is unreachable.
    
???+ info "HTTP 5xx"

    `HTTP 5xx` status codes description
    
    | Status code | Error code | Description | 
    | ----------- | ----------- | ----------------------------- |
    | `HTTP 500 Internal Server Error`  | Not applicable | A system or an application error occurred. Although the client appears to have provided a correct request, something unexpected occurred on the server. | 
    | `HTTP 503 Service Unavailable` | Not applicable | The server cannot handle the request due to temporary maintenance or outage. |
    


### Authorization errors

iumiCash API follows industry standard OAuth 2.0 authorization protocol and returns the 
`HTTP 400`, `HTTP 401`, `HTTP 403` status codes for authorization errors. 

Most of them usually occur due to token-related issues and can be cleared by making sure 
that the token is present and hasn't expired.

???+ info "HTTP 4xx"

    `HTTP 4xx` status code description
    
    | Status code | Error code | Description | 
    | ----------- | ----------- | ----------------------------- |
    | `HTTP 400 Bad Request`  | INVALID_REQUEST | Check request body and possible data in the specified resourse. For example: <ul><li>Incorrect response_type.</li><li>Unsupported grant_type.</li><li>Incorrect redirect_uris.</li></ul> | 
    | `HTTP 401 Unauthorized` | INVALID_AUTHZ | Check authorization parameters for any errors. For example: <ul><li>Authorization header is not present.</li><li>Invalid client credentials.</li><li>Invalid refresh_token.</li><li>Invalid authorization code.</li></ul> | 
    | `HTTP 403 Forbidden` | NOT_AUTHORIZED | Authorization failed due to insufficient permissions. Make sure that you have access to the related resource. |


### Validation errors

For validation errors, iumiCash returns the `HTTP 400 Bad Request` status code.

To prevent validation errors, ensure that parameters are the right type and conform to constraints.

### Examples

???+ example "Response"
    
    Example error response.

    !!! danger
        
        Be careful! In the event of `HTTP 5xx` error status codes, iumiCash API will **NOT** return `application/json` content type header in the response. 
        This may happen while iumiCash servers are unreachable or unresponsive. 
        `HTTP 5xx` status codes are returned in `text/plain` response type by the server itself, e.g. [apache](https://httpd.apache.org/),
        [nginx](https://nginx.org/), [traefik](https://traefik.io/).

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
    
    :    Field containing the error.

    `message` *string*
    
    :    Validation message for this field.

    
[HATEOAS]: orders/create_order.md#hateoas
[Authorization errors]: #authorization-errors
[Validation errors]: #validation-errors
