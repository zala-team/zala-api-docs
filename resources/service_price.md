# Service Price

A Price can be the current or future value of a service when a service is booked in Zala's business.

## Properties

| Property              | Explanation                                                                                                                                                                                                 |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                    | The unique UUID identifier for the Service's Price. Will be `current` if is the current start price or a UUID identifier if is a future price.                                                              |
| serviceId             | [Service](https://github.com/zala-team/zala-api-docs/blob/master/resources/service.md) owner of this Price                                                                                                  |
| price                 | Base price of the service in Money format. `null` if the service use variants                                                                                                                               |
| firstBookingEnabled   | If the service has an additional fee for the first booking of the [Customer](https://github.com/zala-team/zala-api-docs/blob/master/resources/customer.md). `true` or `false`                               |
| firstBookingFeeAmount | Price of the additional fee of the service in Money format.                                                                                                                                                 |
| downPaymentEnabled    | If the service has an upfront fee to be paid at the checkout.                                                                                                                                               |
| downPaymentPercentage | Percentage of the checkout to be required to be paid upfront at the checkout.                                                                                                                               |
| downPaymentAmount     | Amount required to be paid upfront to the booking date in Money format. `null` if there is no up-front fee                                                                                                  |
| variantPrices         | [Variant Price](#variant-price) array of prices of all the [Service Variants](https://github.com/zala-team/zala-api-docs/blob/master/resources/service_variant.md). `Null` if service doesn't use variants. |
| createdAt             | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                                                 |
| updatedAt             | Date when the Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                                            |

#### Variant Price

| Property   | Explanation                                                                                                                                |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| id         | The unique UUID identifier for the [Service Variants](https://github.com/zala-team/zala-api-docs/blob/master/resources/service_variant.md) |
| price      | Base price of the service's variant in Money format. `null` if the service use variants                                                    |
| parameters | If the service has parameters will contain an array with the price of each parameter. [Variant Parameter Price](#variant-parameter-price)  |

#### Variant Parameter Price

| Property | Explanation                                                   |
|----------|---------------------------------------------------------------|
| key      | Parameter unique identifier for the variant. Ex: `size`       |
| value    | A possible value for the key parameter. Ex `small` or `large` |
| price    | Price of the additional fee of the service in Money format.   |

## Endpoints

### GET /services/{service-id}/prices

Receive a list of all (current or future) Services prices.

| Parameter | Explanation                                                                                                      |
|-----------|------------------------------------------------------------------------------------------------------------------|
| current   | If `true` will return just the current price. By default is `false` and all future prices are returned.          |
| startsAt  | Show Prices that start after the given date (inclusive) [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601) |
| page      | Page to show                                                                                                     |
| size      | Amount of results per page                                                                                       |

#### GET /services/23987439-2a3d-462c-810b-f6b8e57d4964/prices

`HTTP/1.1 200 OK`

```json
[
  {
    "id": "fa04d601-661c-4f4f-aec3-c181adda8652",
    "venueId": "52e15a31-639a-41fe-ac7e-d41841934aaf",
    "serviceId": "23987439-2a3d-462c-810b-f6b8e57d4964",
    "price": null,
    "firstBookingEnabled": true,
    "firstBookingFeeAmount": {
      "value": 1000.00,
      "currency": "ARS"
    },
    "downPaymentEnabled": true,
    "downPaymentPercentage": null,
    "downPaymentAmount": {
      "value": 2000.00,
      "currency": "ARS"
    },
    "variantPrices": [
      {
        "id": "fa04d601-661c-4f4f-aec3-c181adda8652",
        "price": {
          "value": 20000.00,
          "currency": "ARS"
        },
        "parameters": null
      }
    ],
    "createdAt": "2013-04-07T09:11:51-00:00",
    "updatedAt": "2013-04-08T11:11:51-00:00"
  }
]
```
