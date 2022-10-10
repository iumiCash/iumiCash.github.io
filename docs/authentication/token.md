# Generate access token

## Generate access token API


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

    `redirect_uri` *URL string*
    :   If the redirect URI was included in the initial authorization request, 
        the service must require it in the token request as well. 
        The redirect URI in the token request must be an exact match of the redirect URI 
        that was used when generating the authorization code. 

        The service must reject the request otherwise.

    `code` *string*
    :    Specification by OAuth 2.0 protocol. 

    :   !!! info
             Required when `grant_type` is `authorization_code`. See [code] flow for details.

    `refresh_token` *string*
    :    Specification by OAuth 2.0 protocol. 

    :   !!! info
            Required when `grant_type` is `refresh_token`. See [refresh] flow for details.

    `state` *string*
    :    Specification by OAuth 2.0 protocol. 

    :   ??? info
            An opaque value used by the client to maintain state between the request and callback. 
            The authorization server includes this value when redirecting the user-agent back to the client. 
            The parameter SHOULD be used for preventing cross-site request forgery
    


    
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

In this section shown different `authorization`/`token obtaining` flows


#### Client credentials

You need this flow when you make calls as `vendor`. For example, when working with [order].

!!! Tip 
    
    If you want to make API calls as `end-user`, see [code] article.


???+ example "Examples"

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
            curl -v -X POST "https://api.iumi.cash/api/v1/oauth/token" \
            -u "<CLIENT_ID>:<CLIENT_SECRET>" \
            -H "Content-Type: application/json" \
            -d ' \
            {
              "grant_type": "client_credentials"
            }
            '
        ```
     
    === "Response"
    
        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        ```bash
        {
           "scope": "profile email https://api.iumi.cash/api/v1/recurrent",
           "access_token": "A21AAInqMcMvhHyuYQ0xIMSNbl4BIEwkvJ93HukaWZ7o3akLHD-5U9bFC2enYoVKzClY8kGl13TCyKAO25DKYEB1bPR3bRrXg",
           "token_type": "Bearer",
           "expires_in": 28800,
           "refresh_token": "eRtAQTVyGJedojtUbkMoHWWOdMMl5ujCDk_94gAA21AAInqMcMvhHyuYQ0-5U9bFenYoVKzClY8kGl13WWOdMMl5ujCDk_94gA",
           "refresh_token_expires_in": 86400,
           "nonce": "2022-04-06T20:10:53ZtyLRwCnLB3eGapVvhCVLScK8IUTqwqz2yUmMEOQbTEg"
        }
        ```

#### Refresh token

Use this flow when your `access_token` has expired.

!!! note "Refresh token"
    Refresh tokens has infinite lifetime duration.

???+ example "Examples"

    === "Request"
        This example shows request to refresh your access token
        
        !!! note
            
            This examples shows `refresh_token` **grant_type** flow. See [client credentials] or [code] for other flows.

        ??? Tip
            
            If you are using `curl` for making requests, you can pass your client_id and client_secret by format
            
             `-u "<CLIENT_ID>:<CLIENT_SECRET>"`
             
             It is the same for passing `Authorization` header explicitly:
             
             `-H "Authorization: Basic e3tjbGllbnRfaWR9fTp7e2NsaWVudF9zZWNyZXR9fQ==`
    
        ```bash
            curl -v -X POST "https://api.iumi.cash/api/v1/oauth/token" \
            -u "<CLIENT_ID>:<CLIENT_SECRET>" \
            -H "Content-Type: application/json" \
            -d ' \
            {
              "grant_type": "refresh_token",
              "refresh_token": "eRtAQTVyGJedojtUbkMoHWWOdMMl5ujCDk_94gAA21AAInqMcMvhHyuYQ0-5U9bFenYoVKzClY8kGl13WWOdMMl5ujCDk_94gA"
            }
            '
        ```
     
    === "Response"
    
        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        ```bash
        {
           "scope": "profile email https://api.iumi.cash/api/v1/recurrent",
           "access_token": "A21AAInqMcMvhHyuYQ0xIMSNbl4BIEwkvJ93HukaWZ7o3akLHD-5U9bFC2enYoVKzClY8kGl13TCyKAO25DKYEB1bPR3bRrXg",
           "token_type": "Bearer",
           "expires_in": 28800,
           "refresh_token": "eRtAQTVyGJedojtUbkMoHWWOdMMl5ujCDk_94gAA21AAInqMcMvhHyuYQ0-5U9bFenYoVKzClY8kGl13WWOdMMl5ujCDk_94gA",
           "refresh_token_expires_in": 86400,
           "nonce": "2022-04-06T20:10:53ZtyLRwCnLB3eGapVvhCVLScK8IUTqwqz2yUmMEOQbTEg"
        }
        ```


#### Authorization code

Use this flow when you need access API calls as `end-user`.

!!! Tip

    To obtain `authorization_code`, see [authorization code].

???+ example "Examples"

    === "Request"
        
        !!! note
            
            This examples shows `authorization_code` **grant_type** flow. See [client credentials] or [refresh] for other flows.

        ??? Tip
            
            If you are using `curl` for making requests, you can pass your client_id and client_secret by format
            
             `-u "<CLIENT_ID>:<CLIENT_SECRET>"`
             
             It is the same for passing `Authorization` header explicitly:
             
             `-H "Authorization: Basic e3tjbGllbnRfaWR9fTp7e2NsaWVudF9zZWNyZXR9fQ==`
    
        ```bash
            curl -v -X POST "https://api.iumi.cash/api/v1/oauth/token" \
            -u "<CLIENT_ID>:<CLIENT_SECRET>" \
            -H "Content-Type: application/json" \
            -d ' \
            {
              "grant_type": "authorization_code",
              "redirect_uri": "https://starlink.com/callback",
              "code": "dsfhkj2389HJK93hjkf349hjsdf34rkjhd"
            }
            '
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        ```bash
        {
           "scope": "profile email https://api.iumi.cash/api/v1/recurrent",
           "access_token": "A21AAInqMcMvhHyuYQ0xIMSNbl4BIEwkvJ93HukaWZ7o3akLHD-5U9bFC2enYoVKzClY8kGl13TCyKAO25DKYEB1bPR3bRrXg",
           "token_type": "Bearer",
           "expires_in": 28800,
           "refresh_token": "eRtAQTVyGJedojtUbkMoHWWOdMMl5ujCDk_94gAA21AAInqMcMvhHyuYQ0-5U9bFenYoVKzClY8kGl13WWOdMMl5ujCDk_94gA",
           "refresh_token_expires_in": 86400,
           "nonce": "2022-04-06T20:10:53ZtyLRwCnLB3eGapVvhCVLScK8IUTqwqz2yUmMEOQbTEg"
        }
        ```


[vendor registration]: ../vendors/vendor_registration.md
[code]: #authorization-code
[refresh]: #refresh-token
[client credentials]: #client-credentials
[order]: ../orders/index.md
[authorization code]: ../authentication/authorize.md
