Customer
========

A Customer of the business. Customer accounts store contact information for the customer, saving logged-in customers the
trouble of having to provide it at every checkout.

Properties
----------

| Property  | Explanation                                                                                         |
|-----------|-----------------------------------------------------------------------------------------------------|
| id        | The UUID identifier for the Customer                                                                |
| firstName | First name of the Customer                                                                          |
| lastName  | Last name of the Customer                                                                           |
| email     | E-mail of the Customer                                                                              |
| phone     | Phone number of the customer (not necessarily the same as the address's phone)                      |
| pets      | List of pets of the Customer has registered in this business                                        |
| createdAt | Date when the Customer was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)      | 
| updatedAt | Date when the Customer was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) |

Endpoints
---------

### GET /customers

Receive a list of all Customers.

| Parameter        | Explanation                                                              |
|------------------|--------------------------------------------------------------------------|
| page             | Page to show                                                             |
| size             | Amount of results per page                                               |
| q                | Search Customers containing the given text in their name, email or phone |

#### GET /customers

`HTTP/1.1 200 OK`

```json
[
  {
    "createdAt": "2013-01-03T09:11:51-03:00",
    "updatedAt": "2013-03-11T09:14:11-03:00",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john.doe@example.com",
    "id": "fd20c781-9fe9-44f3-990c-d3f0c900cc34",
    "phone": null,
    "pets": [
      {
        "id": "88213a23-c05c-4c28-a418-9f5f7e507e28",
        "familyType": "ANIMAL",
        "type": "DOG",
        "name": "Rex",
        "createdAt": "2013-01-03T09:11:51-03:00",
        "updatedAt": "2013-03-10T11:13:01-03:00"
      }
    ]
  }
]
```

### GET /customers/{id}

Receive a single Customer

#### GET /customers/fd20c781-9fe9-44f3-990c-d3f0c900cc34

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-01-03T09:11:51-03:00",
  "updated_at": "2013-03-11T09:14:11-03:00",
  "email": "john.doe@example.com",
  "id": "fd20c781-9fe9-44f3-990c-d3f0c900cc34",
  "phone": null,
  "pets": [
    {
      "id": "88213a23-c05c-4c28-a418-9f5f7e507e28",
      "familyType": "ANIMAL",
      "type": "DOG",
      "name": "Rex",
      "created_at": "2013-01-03T09:11:51-03:00",
      "updated_at": "2013-03-10T11:13:01-03:00"
    }
  ]
}
```
