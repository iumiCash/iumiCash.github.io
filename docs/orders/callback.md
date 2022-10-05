# Order callback mechanism

!!! danger "WIP"

## Why you need to configure callback?

When user redirected to iumiCash.

## Callback data

Order information that the iumiCash sends to the vendor's `callback_url`.

Callback data will contain order object, so you can check order status in a iumiCash.

The iumiCash backend will send `POST` request to `callback_url` with `iumicash-signature` header.

!!! Info

    This endpoint should return `200 OK` with content `OK`. Otherwise, the iumiCash will make retry
    requests (once a hour etc.) while the vendor returns successfull response. 
    It required to make sure that the vendor processes order data.

!!! danger "Security tip"

    The iumiCash will send additional `iumicash-signature` header parameter that contains data signature
    signed with `client_secret` (see [client secret] for more information).
    
    So your backend can verify that request was made by the iumiCash.


### Example

???+ example "Examples"

    === "Request"

        Example request with cURL that the iumiCash backend will send to your [`callback_url`](#callback-data).

        ```bash
        curl -v -X POST https://starlink.com/api/v1/iumiCash/callback/ \
        -H "Content-Type: application/json" \
        -H "iumicash-signature: <some-calculated-order-specific-signature>" \
        -d ' \
        {
          "id": "42481508-af81-43b9-82dd-d47d9e040ece",
          "external_id": "123456",
          "created_at": "2022-09-12T13:27:09.860466",
          "updated_at": "2022-09-12T13:27:09.860466",
          "status": "created",
          "items": [
            {
              "name": "T-shirt",
              "description": "Brand new T-shirt",
              "amount": {
                "value": 100,
                "currency_code": "USD"
              },
              "count": 1
            }
          ],
          "application_context": {
            "callback_url": "https://starlink.com/api/v1/iumiCash/callback/"
            "success_url": "https://starlink.com/success.html",
            "error_url": "https://starlink.com/error.html"
          },
          "links": [
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "self",
              "method": "get"
            },
            {
              "link": "https://api.iumi.cash/checkout/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "approve",
              "method": "get"
            },
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/capture/",
              "rel": "capture",
              "method": "post"
            },
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/cancel/",
              "rel": "cancel",
              "method": "post"
            }
          ]
        }
        '
        ```

    === "Response"

        A successful request have to return the `HTTP 200 OK` status code and `OK` content.

        ```python
        def post(request: Request) -> Response:
            iumicash_signature = request.headers.get("iumicash-signature")
            order_data = request.data
            ...
            some business logic and validation
            ...
            return Response(data="OK", status_code=status.HTTP_200_OK)
        ```


[idempotency]: ../idempotency.md
[client secret]: ../authentication/vendor_registration.md
