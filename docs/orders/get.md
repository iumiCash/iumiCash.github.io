# Get order

## Get order API

Orders are retrieved using the following HTTP method and end-point combination:

`POST /api/v1/orders/<order_id>/`


### Path parameters

???+ info "Path parameters"

    `order_id` *[ID][identifier]*
    :    Unique identifier of the order as per iumiCash system.


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    The identifier (such as a bearer token) is required to validate the third-party system to access the resource. 
         To make REST API calls, include the bearer token in this header with the `Bearer` authentication scheme. 
         The value is `Bearer <Access-Token>`. To get access token see access token.

    `Content-Type` *string* **required**
    :    The media type is required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Response

???+ success "Response"

    `id` *[ID][identifier]* **unique**
    
    :    Unique identifier of the order as per iumiCash system.

    `external_id` *string* **unique**

    :    Unique identifier of the order as per the vendor’s system. 

    `created_at` *datetime*
    
    :    Date and time of the order initial creation in ISO format.

    `updated_at` *datetime*
    
    :    Date and time of the order last update in ISO format.

    `status` *enum* 
    
    :    The order status can be one of the following:
    
         * `created`
         * `approved`
         * `cancelled`
         * `completed`

    `items` *list of [*item object*](#item)*

    :    Order items in the order.

    `links` *list of [*HATEOAS links*](#hateoas)*
    
    :    An array of HATEOAS links related to this order processing (such as to get the latest status of the order, etc.).



### Examples

???+ example "Examples"

    === "Request"

        An example request using cURL.

        ```bash
        curl -v -X POST https://api.iumi.cash/api/v1/orders/542c2b97bac0595474108b48/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer <Access-Token>"'
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        ```bash
        {
          "id": "542c2b97bac0595474108b48",
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
          "links": [
            {
              "link": "https://api.iumi.cash/api/v1/orders/542c2b97bac0595474108b48/",
              "rel": "self",
              "method": "get"
            }
          ]
        }
        ```


[idempotency]: ../idempotency.md
[client secret]: ../vendors/vendor_registration.md
[identifier]: ../types.md#iumicash-identifier
