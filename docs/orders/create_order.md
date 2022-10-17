# Create order

## Create order API

Orders are created using the following HTTP method and end-point combination:

`POST /api/v1/orders/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    The identifier (such as a bearer token) is required to validate the third-party system to access the resource. 
         To make REST API calls, include the bearer token in this header with the `Bearer` authentication scheme. 
         The value is `Bearer <Access-Token>`. To get access token see access token.

    `Content-Type` *string* **required**
    :    The media type is required for operations with a request body. 
         The value is `application/<format>`, where format is `json`.
    
    `RequestId` *string*
    :    The idempotency key is used to correlate request payloads with response payloads. See [idempotency] for more information.



### Request

???+ info "Request body"

    `external_id` *string* **required**
    :    This is a unique identifier for the vendor’s system. iumiCash saves this identifier as part of the order record. 
         Orders can be cross-checked between iumiCash and the vendors system using this identifier.

    `items` *list of [*item object*](#item)* **required**
    :    Items included in the order.


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
         * `captured`
         * `cancelled`
         * `completed`

    `items` *list of [*item object*](#item)*

    :    Items included in the order.

    `links` *list of [*HATEOAS links*](#hateoas)*
    
    :    An array of HATEOAS links related to this order processing (such as to get the latest status of the order, etc.).

!!! tip
    If the vendor failed to get the order details upon creation, then the vendor can submit a request via iumiCash API separately to get the details of the order.

### Examples

???+ example "Examples"

    === "Request"

        An example request using cURL.

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
          ]
        }
        '
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
    :   Number of the specific item in the order.


### amount

???+ info "Description"

    `value` *Decimal*
    :   Price of one item in the order.
    
    `currency_code` *enum*. See [available currencies](#currencies).
    :   Currency of the item price.


### HATEOAS

???+ info "Description"

    `link` *URL string*
    :   The complete target URL. To make the related call, combine the method with this `link`.
    
    `rel` *enum*
    :   The link relation type.
        
        Possible values:
       
        * `self`
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

    Possible values may be restricted to a particular vendor and include:

    * `NZD`
    * `SBD`
    * `USD`


[idempotency]: ../idempotency.md
[client secret]: ../vendors/vendor_registration.md
