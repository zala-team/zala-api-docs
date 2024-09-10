Business
======

The Business resource contains general settings and information about a Zala's business.

Properties
----------

| Property     | Explanation                                                                                 |
|--------------|---------------------------------------------------------------------------------------------|
| id           | The unique string identifier for the Business like `acme-business`                          |
| name         | Displayable name of the business                                                            |
| description  | A brief description of the business                                                         |
| email        | Business owner's e-mail                                                                     |
| logo         | Business logo URL. (always https)                                                           |
| timezone     | Timezone in [IANA format](https://en.wikipedia.org/wiki/Tz_database)                        |
| phone        | Store's phone                                                                               |
| mainCurrency | Store's main currency in [ISO 4217 format](https://en.wikipedia.org/wiki/Tz_database)       |
| createdAt    | Date when the Store was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) |
| updatedAt    | Date when the Store was updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) |

Endpoints
---------

### GET /business

Receive a single Business.

#### GET /business

`HTTP/1.1 200 OK`

```json
{
  "createdAt": "2013-01-01T05:12:51-03:00",
  "updatedAt": "2013-01-01T05:12:52-03:00",
  "id": "acme_business",
  "logo": "//d26lpennugtm8s.cloudfront.net/stores/046/themes/common/logo-ff622335866ee56df3bceed2e9d41469.png",
  "mainCurrency": "ARS",
  "name": "Acme Business",
  "description": "A business that books everything",
  "email": "acme@business.com.ar",
  "phone": "+54 9 11 1234-5678",
  "timezone": "America/Argentina/Buenos_Aires"
}
```
