# Authentication

## Authentication

iumiCash REST APIs use [OAuth 2.0](https://oauth.net/2/) access tokens to authenticate requests. 
Your access token authorizes you to use the iumiCash REST API servers.

To call a REST API in your integration, you must exchange your **client ID** and **client secret** for an access token.  

You can find your **client ID** and **client secret** by logging in to iumiCash Developer Dashboard 
or somehow (we can send them to your email etc.).


!!! Tip

    For more information see [vendor registration].


## Generate access token


`POST /api/v1/oauth/token/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    Separate your Base64-encoded **client ID** and **client secret** by a colon (:).
    
         Format:  `Basic {Your Base64-encoded client_id:client_secret}` without curly braces.
         
         Example: `Basic e3tjbGllbnRfaWR9fTp7e2NsaWVudF9zZWNyZXR9fQ==`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Request

???+ info "Request body"

    `grant_type` *enum* **required**
    :    Specification by OAuth 2.0 protocol. 
    
        Our backend implements follows:
        
        * `client_credentials`
        * `authorization_code`
        * `refresh_token`
        
    `code` *string*
    :    Specification by OAuth 2.0 protocol. 
    
         NOTE: Required when `grant_type` is `authorization_code`. See [code]() flow for details.
        
    `refresh_token` *string*
    :    Specification by OAuth 2.0 protocol. 
    
         NOTE: Required when `grant_type` is `refresh_token`. See [refresh token]() flow for details.

    
### Response

???+ success "Response"

    `scope` *string*
    
    :    A list of space-separated permissions associated with the access token.

    `access_token` *string*
    
    :    Identifies the actual token used to call resourses.

    `token_type` *string*
    
    :    Defines the type of token. It defaults to `Bearer`.

    `expires_in` *integer*
    
    :    Identifies the number of seconds until the access token expires.

    `refresh_token` *string* 
    
    :    Identifies the actual token used to refresh the access token.

    `refresh_token_expires_in` *integer* 
    
    :    Identifies the number of seconds until the refresh token expires.

    `nonce` *string* 
    
    :    A one-time use random string generated from server-specific data, used to prevent replay attacks.


### Examples

???+ "Examples"

    === "Request"
        This example shows request to obtain **access token**
        
        !!! note
            
            This examples shows `client_credentials` **grant_type** flow. See [code] or [refresh] for other flows.
        ??? Tip
            
            If you are using `curl` for making requests, you can pass your client_id and client_secret by format
            
             `-u "<CLIENT_ID>:<CLIENT_SECRET>"`
             
             It is the same for passing `Authorization` header explicitly:
             
             `-H "Authorization: Basic e3tjbGllbnRfaWR9fTp7e2NsaWVudF9zZWNyZXR9fQ==`
    
        ```bash
            curl -v -X POST "https://iumi.cash/api/v1/oauth/token" \
            -u "<CLIENT_ID>:<CLIENT_SECRET>" \
            -H "Content-Type: application/json" \
            -d ' \
            {
              "grant_type": "client_credentials",
            }
            '
        ```
     
    === "Response"
    
        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        ```bash
        {
           "scope": "profile email https://iumi.cash/api/v1/recurrent",
           "access_token": "A21AAInqMcMvhHyuYQ0xIMSNbl4BIEwkvJ93HukaWZ7o3akLHD-5U9bFC2enYoVKzClY8kGl13TCyKAO25DKYEB1bPR3bRrXg",
           "token_type": "Bearer",
           "expires_in": 28800,
           "refresh_token": "eRtAQTVyGJedojtUbkMoHWWOdMMl5ujCDk_94gAA21AAInqMcMvhHyuYQ0-5U9bFenYoVKzClY8kGl13WWOdMMl5ujCDk_94gA",
           "refresh_token_expires_in": 86400,
           "nonce": "2022-04-06T20:10:53ZtyLRwCnLB3eGapVvhCVLScK8IUTqwqz2yUmMEOQbTEg"
        }
        ```

## Refresh token

WIP


## Authorization code

WIP


[vendor registration]: /authentication/vendor_registration/
[code]: #authorization-code
[refresh]: #refresh-token
