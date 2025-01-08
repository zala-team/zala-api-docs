Pet
========

A Pet of a Customer registered in the business. Pets are entities that can receive the service by the business when they
work with animals.

Properties
----------

| Property   | Explanation                                                                                             |
|------------|---------------------------------------------------------------------------------------------------------|
| id         | The UUID identifier for the Customer                                                                    |
| familyType | Always will be `ANIMAL`.                                                                                |
| type       | `DOG` or `CAT`                                                                                          |
| name       | Name provided by the Customer                                                                           |
| sex        | Sex will be provided by the business. Can take the values of `MALE`, `FEMALE` or `null`                 |
| breed      | The breed of the animal provided by the business. String.                                               |
| weight     | Weight measured in grams. 15000.0g = 15kg                                                               |
| color      | A string representation of the animal color                                                             |
| birthDate  | Date when the Customer Pet was born in `yyyy-mm-dd` format                                              |
| createdAt  | Date when the Customer Pet was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)      | 
| updatedAt  | Date when the Customer Pet was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) |

Endpoints
---------

### GET /customers/{id}/pets/{petId}

Receive a single Customer Pet

#### GET /customers/fd20c781-9fe9-44f3-990c-d3f0c900cc34/pets/88a21581-fd67-4ddf-bd70-784699e6fa10

`HTTP/1.1 200 OK`

```json
{
  "id": "88a21581-fd67-4ddf-bd70-784699e6fa10",
  "familyType": "ANIMAL",
  "type": "DOG",
  "name": "xxx",
  "sex": "FEMALE",
  "breed": "Caniche",
  "weight": 15000.0,
  "color": "Blanco",
  "birthDate": "2020-02-04",
  "createdAt": "2024-10-02T11:04:37.668715Z",
  "updatedAt": "2024-10-02T11:04:37.668715Z"
}
```

### PUT /customers/{id}/pets/{petId}

Update a single Customer Pet. Properties that are not sent will be set to null.

| Parameter | Explanation                                                                             |
|-----------|-----------------------------------------------------------------------------------------|
| name      | Name for the animal. Required field.                                                    |
| sex       | Sex will be provided by the business. Can take the values of `MALE`, `FEMALE` or `null` |
| breed     | The breed of the animal provided by the business. String.                               |
| weight    | Weight measured in grams. 15000.0g = 15kg                                               |
| color     | A string representation of the animal color                                             |
| birthDate | Date when the Customer Pet was born in `yyyy-mm-dd` format                              |

#### PUT /customers/fd20c781-9fe9-44f3-990c-d3f0c900cc34/pets/88a21581-fd67-4ddf-bd70-784699e6fa10

##### Request

```json
{
  "name": "Scooby",
  "sex": "MALE",
  "breed": "Labrador",
  "weight": 35000.0,
  "color": "Blanco",
  "birthDate": "2020-02-04"
}
```

`HTTP/1.1 200 OK`

##### Response

```json
{
  "id": "88a21581-fd67-4ddf-bd70-784699e6fa10",
  "familyType": "ANIMAL",
  "type": "DOG",
  "name": "Scooby",
  "sex": "MALE",
  "breed": "Labrador",
  "weight": 35000.0,
  "color": "Blanco",
  "birthDate": "2020-02-04",
  "createdAt": "2024-10-02T11:04:37.668715Z",
  "updatedAt": "2024-10-02T11:04:37.668715Z"
}
```
