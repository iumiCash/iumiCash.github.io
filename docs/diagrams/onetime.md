# One-time payment flow


The following sequence diagram shows how one-time payment works.

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

The following sequence diagram shows how one-time payment works.

??? info "Step descriptions"

    1. Click **Pay via iumiCash**
    :   The end-user clicks button `Pay via iumiCash` (or similar) in the vendor's user interface.
    
    2. Authorization Code request is sent to /oauth/authorize/
    :   The vendor constructs URL to obtain user's credentials. 

        See [authorize request][authorize] for detailed request.
    
    3. Redirect user to authorization page
    :   The iumiCash redirects user to an authorization page where the list of permissions the user grants the vendor 
        and the application details are displayed.

        !!! tip
            This page also shows the [vendor's verification status][vendor verification].
    
    4. Authenticate
    :   By logging in the user approves the vendor to use his iumiCash account for payments.

    5. Redirect to `redirect_uri`
    :   iumiCash API redirects the user to `redirect_uri` specified by the vendor at step 2. 
        The redirect URL will include `AUTH_CODE` in query parameters (see authorization code).

    6. Obtain `access_token` from /oauth/token/
    :   The vendor makes a request to /oauth/token/ (see [token request][token]) with the `AUTH_CODE` obtained in the previous step

    7. Validate
    :   iumiCash API validates the request.

    8. Return the API client `access_token` and `refresh_token`
    :   iumiCash API server returns a pair of `access_token` and `refresh_token` to the API client. 
        See [access token] or [refresh token] for more information

    9. Create *order*
    :   The vendor constructs [order data] and submits [create order] request.

    10. Return created order
    :   The iumiCash API returns the details of the created [order].


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
