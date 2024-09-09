# Order

An order is created when a customer completes the checkout process.

#### Table of Contents

> [Get all orders](#GET-orders)
>
> [Get an order](#GET-ordersid)

## Properties

| Property                   | Explanation                                                                                                                                                                                                                              |
| -------------------------- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                         | The unique numeric identifier for the Order. It's different from `number`                                                                                                                                                                |
| token                      | Specifies the location of the Order                                                                                                                                                                                                      |
| number                     | Unique numberc identifier for an Order used by the shop owner and customers. It's sequential and starts at 100                                                                                                                           |
| customer                   | [Customer](https://github.com/zala/zala-api-docs/blob/master/resources/customer.md) that purchased this Order. Only given if the 'read_customers' scope is set for the app                                                               |
| products                   | List of the products purchased by the `customer`. Contents are explained below and values hold are the ones corresponding to the time the products were purchased                                                                        |
| note                       | Customer's note about the order                                                                                                                                                                                                          |
| owner_note                 | Store owner's note about the order                                                                                                                                                                                                       |
| coupon                     | List of coupons applied to the order                                                                                                                                                                                                     |
| discount                   | Total value of the discount applied to the price of the order                                                                                                                                                                            |
| subtotal                   | Price of the order before shipping                                                                                                                                                                                                       |
| total                      | Total price of the order including shipping and discounts                                                                                                                                                                                |
| total_usd                  | Total price of the order in US dollars                                                                                                                                                                                                   |
| currency                   | The total spent's currency in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217)                                                                                                                                                   |
| language                   | Order's language used by the customer during the checkout process                                                                                                                                                                        |
| gateway                    | ID of the payment provider that processed the order payment transaction.                                                                                                                                                                 |
| gateway_id                 | [Read-only] External transaction ID used by the payment provider.                                                                                                                                                                        |
| gateway_name               | [Read-only] Name of the payment provider of the order.                                                                                                                                                                                   |
| shipping                   | The shipping method used                                                                                                                                                                                                                 |
| shipping_pickup_type       | "ship" if the order is going to be shipped; "pickup" if it's going to be picked up from a store branch                                                                                                                                   |
| shipping_store_branch_name | If order is going to be picked up, shows the store branch name                                                                                                                                                                           |
| shipping_address           | The customer's shipping address where the order will be shipped                                                                                                                                                                          |
| shipping_tracking_number   | The shipping tracking number for the order. This may be null if not available                                                                                                                                                            |
| shipping_min_days          | The minimum number of weekdays needed for the order to be delivered                                                                                                                                                                      |
| shipping_max_days          | The maximum number of weekdays needed for the order to be delivered                                                                                                                                                                      |
| shipping_cost_owner        | The shipping cost the store owner has to pay to the shipping company.                                                                                                                                                                    |
| shipping_cost_customer     | The shipping cost the customer has to pay to the store owner.                                                                                                                                                                            |
| shipping_option            | The shipping option chosen by the customer during the checkout process.                                                                                                                                                                  |
| shipping_option_code       | The shipping option code selected by the consumers.                                                                                                                                                                                      |
| shipping_option_reference  | The shipping option reference provided by a custom shipping carrier during the checkout process.                                                                                                                                         |
| shipping_pickup_details    | The shipping pickup details (address and/or business hours) of the selected pickup point.                                                                                                                                                |
| shipping_tracking_url      | The shipping tracking URL where the customer can check for the shipment status.                                                                                                                                                          |
| billing_address            | Billing address for the order                                                                                                                                                                                                            |
| billing_number             | Billing number for the order                                                                                                                                                                                                             |
| billing_floor              | Billing floor for the order                                                                                                                                                                                                              |
| billing_locality           | Billing locality for the order                                                                                                                                                                                                           |
| billing_zipcode            | Billing zipcode for the order                                                                                                                                                                                                            |
| billing_city               | Billing city for the order                                                                                                                                                                                                               |
| billing_province           | Billing province for the order                                                                                                                                                                                                           |
| billing_country            | Billing country code for the order                                                                                                                                                                                                       |
| extra                      | A JSON object containing custom information. Can be set via the API or through custom form fields of name "extra[key]" on the cart's checkout form in the storefront                                                                     |
| storefront                 | Origin of the order. Possible values are "store" (order created in the storefront), "meli" (order imported from Mercado Libre), "api" (order created via API) or "form" (order created in the admin panel with the draft orders feature) |
| weight                     | Order's total weight, in kilograms                                                                                                                                                                                                       |
| status                     | Order's status. Possible values are "open", "closed" or "cancelled"                                                                                                                                                                      |
| payment_status             | Order's payment status. Possible values are "authorized", "pending", "paid", "abandoned", "refunded" or "voided"                                                                                                                         |
| shipping_status            | Order's shipping status. Possible values are "unpacked", "fulfilled" (means "shipped") or "unfulfilled" (means "unshipped")                                                                                                              |
| next_action                | Next available operation in the orders flow                                                                                                                                                                                              |
| payment_details            | A JSON object containing payment details.                                                                                                                                                                                                |
| shipped_at                 | Date when the Order was shipped in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                                                                              |
| paid_at                    | Date when the order was paid in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).                                                                                                                                                |
| cancel_reason              | Reason why the store owner cancelled an Order. Possible values are "customer", "fraud", "inventory" or "other"                                                                                                                           |
| created_at                 | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                                                                              |
| updated_at                 | Date when the Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                                                                         |
| client_details             | Customer details for analytics.                                                                                                                                                                                                          |

The `products` field has the following contents:

| Property      | Explanation                                                                                                 |
| ------------- |-------------------------------------------------------------------------------------------------------------|
| product_id    | [Product](https://github.com/zala/zala-api-docs/blob/master/resources/product.md) purchased                 |
| variant_id    | [Product Variant](https://github.com/zala/zala-api-docs/blob/master/resources/product_variant.md) purchased |
| name          | Product's name at the time of purchase                                                                      |
| price         | Product's price at the time of purchase                                                                     |
| quantity      | Quantity purchased                                                                                          |
| weight        | Product's weight at the time of purchase                                                                    |
| width         | Product's width at the time of purchase                                                                     |
| height        | Product's height at the time of purchase                                                                    |
| depth         | Product's depth at the time of purchase                                                                     |
| free_shipping | Indicates if the product has free shipping or not.                                                          |
| issues        | Possibles issues that can happen to an order. Contents are explained below.                                 |

#### Payment Details

The `payment_details` field has the following contents:
| Property| Type | Explanation |
|--|--|--|
|method| String |Payment method selected.|
|credit_card_company| String |Credit card company.|
|installments| Integer |The unique numeric identifier for the promotional discount.|

## Endpoints

### GET /orders

Receive a list of all Orders.

| Parameter      | Explanation                                                                                                                                       |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| status         | Show Orders with a given state. Possible values are "any" (default), "open", "closed" or "cancelled"                                              |
| payment_status | Show Orders with a given payment state. Possible values are "any" (default), "pending", "authorized", "paid", "abandoned", "refunded" or "voided" |
| customer_ids   | Restrict results to the specified customer IDs (comma-separated)                                                                                  |
| page           | Page to show                                                                                                                                      |
| size           | Amount of results per page                                                                                                                        |
| q              | Search Orders by the given number; or containing the given text in the customer name or email                                                     |

#### GET /orders

`HTTP/1.1 200 OK`

```json
[
  {
    "id": 450789469,
    "token": "0fe3e5421c2328227c47433c6c4f8ece2791f822",
    "store_id": "271146",
    "shipping_min_days": 1,
    "shipping_max_days": 2,
    "billing_name": "John Doe",
    "billing_phone": "555-123-0413",
    "billing_address": "Evergreen Terrace",
    "billing_number": "742",
    "billing_floor": null,
    "billing_locality": null,
    "billing_zipcode": "97475",
    "billing_city": "Springfield",
    "billing_province": "Oregon",
    "billing_country": "US",
    "shipping_cost_owner": "470.00",
    "shipping_cost_customer": "470.00",
    "coupon": [
      {
        "code": "SUPERDISCOUNT"
      }
    ],
    "promotional_discount": {
      "id": null,
      "store_id": 729001,
      "order_id": "972900182",
      "created_at": "2022-02-25T15:18:17+0000",
      "total_discount_amount": "0.00",
      "contents": [],
      "promotions_applied": []
    },
    "subtotal": "5190.00",
    "discount": "0.00",
    "discount_coupon": "0.00",
    "discount_gateway": "0.00",
    "total": "5660.00",
    "total_usd": "52.86",
    "checkout_enabled": true,
    "weight": "0.500",
    "currency": "USD",
    "language": "en",
    "gateway": "paypal",
    "gateway_id": "20396106644",
    "gateway_name": "PayPal",
    "shipping": "ups",
    "shipping_option": "Synthetics",
    "shipping_option_code": "table_131313",
    "shipping_option_reference": null,
    "shipping_pickup_details": null,
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_store_branch_name": null,
    "shipping_pickup_type": "ship",
    "shipping_suboption": [],
    "extra": {
      "gift-wrap": "deluxe"
    },
    "storefront": "mobile",
    "note": null,
    "created_at": "2022-02-23T12:25:18+0000",
    "updated_at": "2022-02-23T19:58:44+0000",
    "completed_at": {
      "date": "2022-02-23 12:25:18.000000",
      "timezone_type": 3,
      "timezone": "UTC"
    },
    "next_action": "noop",
    "payment_details": {
      "method": "credit_card",
      "credit_card_company": "amex",
      "installments": "6"
    },
    "attributes": [],
    "customer": {
      "id": 101,
      "name": "John Doe",
      "email": "john.doe@example.com",
      "identification": "41913703",
      "phone": "+5491126653230",
      "note": null,
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2022-02-23T12:25:18+0000",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "name": "John Doe",
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2022-02-23T12:25:18+0000",
        "zipcode": "97475"
      },
      "addresses": [
        {
          "address": "Evergreen Terrace",
          "city": "Springfield",
          "country": "US",
          "created_at": "2022-02-23T12:25:18+0000",
          "default": true,
          "floor": null,
          "id": 1234,
          "locality": null,
          "name": "John Doe",
          "number": "742",
          "phone": "555-123-0413",
          "province": "Oregon",
          "updated_at": "2022-02-23T12:25:18+0000",
          "zipcode": "97475"
        }
      ],
      "billing_name": "John Doe",
      "billing_phone": "555-123-0413",
      "billing_address": "Evergreen Terrace",
      "billing_number": "742",
      "billing_floor": null,
      "billing_locality": null,
      "billing_zipcode": "97475",
      "billing_city": "Springfield",
      "billing_province": "Oregon",
      "billing_country": "US",
      "extra": {},
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "last_order_id": 9001,
      "active": false,
      "created_at": "2022-02-23T12:25:18+0000",
      "updated_at": "2022-02-23T12:25:18+0000",
      "accepts_marketing": false,
      "accepts_marketing_updated_at": "2022-02-23T12:20:49+0000"
    },
    "products": [
      {
        "id": 972900182,
        "depth": "45.00",
        "height": "3.00",
        "name": "Pokeball",
        "price": "5190.00",
        "product_id": 58208779,
        "image": {
          "id": 109832308,
          "product_id": 58208779,
          "src": "https://www.popsockets.com/dw/image/v2/BFSM_PRD/on/demandware.static/-/Sites-popsockets-master-catalog/default/dw9eb9511a/images/hi-res/Poke-Ball-Gloss_01_Top-View.png?sw=800&sh=800",
          "position": 1,
          "alt": [],
          "created_at": "2020-07-31T16:43:58+0000",
          "updated_at": "2021-08-26T18:26:46+0000"
        },
        "quantity": "1",
        "free_shipping": false,
        "weight": "0.50",
        "width": "34.00",
        "variant_id": "169164456",
        "variant_values": ["Red", "S"],
        "properties": [],
        "sku": "12331471",
        "barcode": null
      }
    ],
    "number": 10872,
    "cancel_reason": null,
    "owner_note": null,
    "cancelled_at": null,
    "closed_at": "2022-02-23T19:58:44+0000",
    "read_at": null,
    "status": "open",
    "payment_status": "paid",
    "shipping_address": {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2022-02-23T12:19:44+0000",
      "default": false,
      "floor": null,
      "id": 1234,
      "locality": null,
      "name": "John Doe",
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2022-02-23T19:58:44+0000",
      "zipcode": "97475",
      "customs": {
        "reference": "580A",
        "between_streets": "Entre Calles"
      }
    },
    "shipping_status": "unshipped",
    "shipped_at": "2022-02-23T19:58:30+0000",
    "paid_at": "2022-02-23T12:25:18+0000",
    "landing_url": "http://www.example.com?source=abc",
    "client_details": {
      "browser_ip": "192.0.0.1",
      "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 15_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/98.0.4758.97 Mobile/15E148 Safari/604.1"
    },
    "app_id": null
  }
]
```
### GET /orders/{id}

Receive a single Order

| Parameter | Explanation                                               |
| --------- | --------------------------------------------------------- |
| fields    | Comma-separated list of fields to include in the response |

#### GET /orders/450789469

`HTTP/1.1 200 OK`

```json
{
  "cancel_reason": null,
  "created_at": "2008-01-10T11:00:00-05:00",
  "currency": "USD",
  "gateway": "paypal",
  "gateway_id": "20396106644",
  "gateway_name": "PayPal",
  "id": 450789469,
  "landing_site": "http://www.example.com?source=abc",
  "language": "en",
  "location_id": null,
  "name": "#1001",
  "note": null,
  "number": 1,
  "owner_note": null,
  "payment_status": "pending",
  "shipping": "ups",
  "shipping_status": "unshipped",
  "shipping_tracking_number": null,
  "shipping_tracking_url": null,
  "shipping_min_days": 2,
  "shipping_max_days": 4,
  "shipping_cost_owner": "20.00",
  "shipping_cost_customer": "20.00",
  "shipping_option": "Synthetics",
  "shipping_option_code": "table_131313",
  "shipping_option_reference": null,
  "status": "open",
  "subtotal": "38.00",
  "token": "898544a54283414238f74cd08f0efd3916f74b75",
  "discount": "0.00",
  "price": "58.00",
  "price_usd": "58.00",
  "weight": "2.00",
  "updated_at": "2008-01-10T11:00:00-05:00",
  "shipped_at": "2008-01-10T11:00:00-05:00",
  "products": [
    {
      "depth": null,
      "height": null,
      "price": "19.00",
      "product_id": 1234,
      "quantity": 2,
      "free_shipping": false,
      "variant_id": 101,
      "weight": "2.00",
      "width": null
    }
  ],
  "customer": {
    "created_at": "2013-01-03T09:11:51-03:00",
    "updated_at": "2013-03-11T09:14:11-03:00",
    "email": "john.doe@example.com",
    "id": "fd20c781-9fe9-44f3-990c-d3f0c900cc34",
    "phone": null,
    "active": true,
    "pets": [
      {
        "id": "88213a23-c05c-4c28-a418-9f5f7e507e28",
        "familyType": "ANIMAL",
        "type": "DOG",
        "name": "Rex",
        "created_at": "2013-01-03T09:11:51-03:00",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      }
    ]
  }
}
```