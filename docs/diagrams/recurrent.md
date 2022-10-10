# Recurrent payment flow


This sequence diagram shows how One-Time payment works.

???+ "Diagram"

    ```mermaid
    sequenceDiagram
        autonumber

        participant Vendor
        participant Auth
        participant iumiCash
        participant iumiFront

        Vendor->Auth: Get access_token
        Auth-->Vendor: Response

        Vendor->iumiCash:Create *order*
        iumiCash-->Vendor:Return created order

        Vendor->iumiFront: Redirects to *capture_url*
        alt Client not authorized
        iumiFront->iumiCash: Redirects user to login
        iumiCash-->iumiFront: Returns client_access_token
        end

        iumiFront->iumiCash: Send approve request
        iumiCash-->iumiFront: Returning order status
        alt success
        iumiFront-->Vendor: If status *success*, redirects the user to *success_url*
        else fail
        iumiFront-->Vendor: If another status: redirects to *error_url* 
        end

        iumiCash->Vendor: Callback to *callback_url*
        Vendor-->iumiCash: The vendor responses with 200

    ```

??? info "Step descriptions"

    1. Click **Pay via iumiCash**
    :   The end user clicks button `Pay via iumiCash` on the vendor's site.
    
    1. Get access_token
    :   On the same time, the vendor have to obtain `access_token` from `api/v1/oauth/token/` endpoint
        with `grant_type=client_credentials`. See [access token] for more details.
    
    1. Response
    :   The iumiCash returns `access_token` and other useful fields. See [access token]'s `Response` tab for details.
    
    1. Create *order*
    :   The vendor constructs [order data] and makes [create order] request.
    
    1. Return created order
    :   The iumiCash returns [order]. In `links` section returned `approve` [hateoas] link that the vendor
        will use in the next step.
    
    1. Redirects to *capture_url*
    :   The vendor redirects the user to `approve` [hateoas] `link` obtained in the previous step.
    
    1. Redirects user to login
    :   If the user is not authorized the iumiCash redirects a user to default `login` form

        !!! Note
            This step only executed only if the user is not authorized

        !!! Info
            This step executed on the iumiCash side.
    
    1. Returns client_access_token
    :   The iumiCash returns user's `client_access_token` and stores it in cookies.

        !!! Note
            This step only executed only if the user is not authorized
        
        !!! Info
            This step executed on the iumiCash side.
     
    1. Client confirmation form
    :   The iumiCash shows to the user confirmation form. It renders `money amount`, `description` etc.
    
    1. Client approves
    :   The user approves [order] via clicking `Approve` button.
    
    1. Send approve request
    :   The iumiCash front makes actual `approve` request to iumiCash.
    
    1. Returning order status
    :   The iumiCash returns [order] object with changed status.
    
    1. If status *success*, redirects user to *success_url*
    :   If status *success*, redirects user to *success_url*
    
    1. If another status: redirects to *error_url* 
    :   If another status: redirects to *error_url* 
    
    1. Callback to *callback_url*
    :   On the same time with `returning order status` on previous steps, the iumiCash will send
        `POST` request to `callback_url` with [callback data].
    
    1. The vendor responses with 200
    :   The vendor should response with `HTTP 200 OK` status and `OK` content as described in [callback data].


[access token]: ../authentication/token.md#client-credentials
[order data]: ../orders/create_order.md#request
[create order]: ../orders/create_order.md#create-order-api
[order]: ../orders/create_order.md#response
[hateoas]: ../orders/create_order.md#hateoas
[callback data]: ../orders/callback.md
