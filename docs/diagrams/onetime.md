# One-Time payment flow


This sequence diagram shows how One-Time payment works.

???+ "Diagram"

    ```mermaid
    sequenceDiagram
        autonumber

        actor Client

        participant Vendor
        participant Auth
        participant iumiCash

        Client->Vendor: Click *Pay via iumiCash*
        alt Vendor has no `access_token` and `refresh_token`
        Vendor->Auth: Authorization Code request to /oauth/authorize/
        Auth->Client: Redirect to login/approve
        Client->Auth: Authenticate and approve
        Auth->Vendor: Redirect to `redirect_uri`
        Vendor->Auth: Obtain `access_token` from /oauth/token/
        Auth->Auth: Validate
        Auth-->Vendor: Return client's `access_token` and `refresh_token`
        end

        Vendor->iumiCash: Create `order`
        iumiCash-->Vendor: Return created order

    ```

??? info "Step descriptions"

    1. Click **Pay via iumiCash**
    :   The end user clicks button `Pay via iumiCash` on the vendor's site.
    
    2. Authorization Code request to /oauth/authorize/
    :   The vendor constructs URL to obtain user's credentials.

        See [authorize request][authorize] for detailed request.
    
    3. Redirect to login/approve
    :   The iumiCash redirects user to login/approve page. On this page shown
        which vendor wants to obtain user's credentials and which credentials vendor wants.

        !!! tip
            Also, on this page shown [vendor's verification status][vendor verification].
    
    4. Authenticate and approve
    :   User logins and approves vendor request.

    5. Redirect to `redirect_uri`
    :   The iumiCash redirects user to `redirect_uri`, which specified by vendor
        at step 2. In redirected URL the iumiCash will add `AUTH_CODE` in query parameters 
        (See [authorization code])

    6. Obtain `access_token` from /oauth/token/
    :   The vendor makes request to /oauth/token/ (see [token request][token]) 
        with obtained `AUTH_CODE` from previous step

    7. Validate
    :   The iumiCash validate request.

    8. Return client's `access_token` and `refresh_token`
    :   The iumiCash returns client's `access_token` and `refresh_token`. 
        See [access token] or [refresh token] for more information

    9. Create *order*
    :   The vendor constructs [order data] and makes [create order] request.

    10. Return created order
    :   The iumiCash returns [order].


[access token]: ../authentication/token.md#authorization-code
[refresh token]: ../authentication/token.md#refresh-token
[order data]: ../orders/create_order.md#request
[create order]: ../orders/create_order.md#create-order-api
[order]: ../orders/create_order.md#response
[hateoas]: ../orders/create_order.md#hateoas
[authorize]: ../authentication/authorize.md
[vendor verification]: ../vendors/verification.md
[authorization code]: ../authentication/authorize.md#response
[token]: ../authentication/token.md
