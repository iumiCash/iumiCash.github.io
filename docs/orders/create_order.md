# Create order

## Create order API

Creates an order

`POST /api/v1/orders/`


### Headers

???+ info "Header parameters"

    `RequestId` *string*
    :    Idempotency key for request. See [idempotency] for more information.

    `Authorization` *string* **required**
    :    To make REST API calls, include the bearer token in this header with the `Bearer` authentication scheme. The value is `Bearer <Access-Token>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Request

???+ info "Request body"

    `external_id` *string* **required**
    :    Unique identifier of your system. iumiCash will save this to this order, 
         so you can further identify iumiCash order with your order instance.

    `items` *list of [*item object*](#item)* **required**
    :    Order items in the order.

    `application_context` [*object*](#application_context) **required**
    :    Callback/redirect URIs that used during payment process.


### Response

???+ success "Response"

    `id` *UUID* **unique**
    
    :    iumiCash identifier. Using to unique identify resourses (in this example, order etc.)

    `external_id` *string* **unique**

    :    Unique identifier of your system. 

    `created_at` *datetime*
    
    :    Created time of the order in ISO format.

    `updated_at` *datetime*
    
    :    Updated time of the order in ISO format.

    `status` *enum* 
    
    :    Order status. Can be one of follows:
    
         * `created`
         * `approved`
         * `captured`
         * `cancelled`
         * `completed`

    `items` *list of [*item object*](#item)*

    :    Order items in the order.

    `application_context` [*object*](#application_context)

    :    Callback/redirect URIs that used during payment process.

    `links` *list of [*HATEOAS links*](#hateoas)*
    
    :    An array of request-related HATEOAS links. For example, 
         to complete payer approval, use `approve` link to redirect the payer.


### Examples

???+ example "Examples"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://api.iumi.cash/api/v1/orders/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer <Access-Token>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "external_id": "123456",
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
            "callback_url": "https://starlink.com/api/v1/iumiCash/callback/",
            "success_url": "https://starlink.com/success.html",
            "error_url": "https://starlink.com/error.html"
          }
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        ```bash
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
        ```

## Objects

Request and response objects


### item

???+ info "Description"

    `name` *string*
    :   Name of the order.
    
    `description` *string*
    :   Description of the order.
    
    `amount` [*object*](#amount)
    :   Amount object.
    
    `count` *integer*
    :   Count of this item.


### amount

???+ info "Description"

    `value` *Decimal*
    :   amount
    
    `currency_code` *enum*. See [available currencies](#currencies).
    :   Currency of item


### application_context

???+ info "Description"

    `callback_url` *URL string*
    :   Later, the iumiCash will send [callback data] to this endpoint. 
        Callback data will contain order object, so you can check order status in a iumiCash.
    
        This endpoint should return `200 OK` with content `OK`. Otherwise, the iumiCash will make retry
        requests (once a hour etc.) until the vendor returns successfull response. 
        It required to make sure that the vendor processed order data.
    :   !!! tip
            For more information, see [callback data].
    
    `success_url` *URL string*
    :   When order succeeds the iumiCash will redirect user to this url.
    
    `error_url` *URL string*
    :   When order failed somehow (user cancels order etc.) the iumiCash will redirect user to this url.


### HATEOAS

???+ info "Description"

    `link` *URL string*
    :   The complete target URL. To make the related call, combine the method with this `link`.
    
    `rel` *enum*
    :   The link relation type.
        
        Possible values:
       
        * `self` - do `method` with current resource.
        * `approve`
        * `capture`
        * `cancel`

    `method` *enum*
    :   The HTTP method required to make the related call. 
        
        Possible values:
        
        * `GET`
        * `POST`


### Currencies

???+ info "Description"

    Possible values:

    * `SBD`
    * `NZD`


[idempotency]: ../idempotency.md
[client secret]: ../authentication/vendor_registration.md
[callback data]: ../orders/callback.md
