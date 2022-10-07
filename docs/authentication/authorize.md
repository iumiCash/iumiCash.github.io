# Authorization code

## Authorization code flow

The first step of OAuth 2 is to get authorization from the user. 
The iumiCash accomplished this by displaying an authorization page to the user.


1. First, create link to redirect the user to:

    ```bash
    https://api.iumi.cash/api/v1/oauth/authorize/?response_type=code
      &client_id=CLIENT_ID
      &redirect_uri=REDIRECT_URI
      &scope=email
      &state=1234zxc
    ```
   
2. The user sees the authorization prompt, requested scopes (permissions), application, that requested access.
   
3. After user `allows`, the iumiCash will redirect the user back to your `redirect_uri` with an authorization code:

    ```bash
    https://example.com/callback?code=AUTH_CODE&state=1234zxc 
    ```

4. With this `code` you can obtain `access_token` via [token request][token].


`GET /api/v1/oauth/authorize/`


### Query parameters

???+ info "Query parameters"

    `response_type` *enum*
    :   Indicates that your server expects to receive an authorization code. Possible values:
        
        * `code` 

    `client_id` *string*
    :    Public identifier of your app. You can obtain if from [vendor registration].

    `redirect_uri` *URL string*
    :    Tells the authorization server where to redirect user after approving the request.

    `scope` *string*
    :    One or more space-separated strings indicating which permissions the application in requesting.

    `state` *string*
    :    The application generates a random string and includes it in the request. 
         It should then check that the same value is returned after the user authorizes the app. 
         This is used to prevent CSRF attacks.

    
### Response

???+ success "Response"

    `code` *string*
    
    :    The server returns the authorization code in the query string

    `state` *string*
    
    :    The application generates a random string and includes it in the request. 
         It should then check that the same value is returned after the user authorizes the app. 
         This is used to prevent CSRF attacks.


[vendor registration]: ../vendors/vendor_registration.md
[code]: #authorization-code
[order]: ../orders/index.md
[recurrent payments]: ../orders/recurrent_payment.md
[token]: ../authentication/token.md#authorization-code

