Zala API
====================

This is a REST-style API that uses JSON for serialization and OAuth 2 for authentication.

Where do I start?
----------------

Want to get started with API integration? Here's a quick check list:

1. Register as a partner in [Zala](https://zala.app)
2. Once inside your partner's admin panel, go to the "Apps" section and create your app.
3. Read up on [how to authenticate](#authentication) your app with us.
4. Read the API docs to understand what you can do with your app.

Making a request
----------------

All URLs start with `https://api.zala.app/v1/{business_id}` or with `https://sandbox.zala.app/v1/{business_id}`. **SSL only**. The path is prefixed with the business id and
the API version.
If we change the API in backward-incompatible ways, we'll bump the version marker and maintain stable support for the
old URLs.

So if you want to access the business with id acme_business via the API the url will
be `https://api.zala.app/v1/acme_business`.

To make a request for all the business's products you would do the following in curl:

```shell
curl -H 'Authentication: bearer ACCESS_TOKEN ' \
  -H 'User-Agent: MyApp (name@email.com)' \
  https://api.zala.app/v1/acme_business/orders
```

where `ACCESS_TOKEN` is the business's access token for your app (see Authentication).

Authentication
--------------

We follow the OAuth 2 framework for letting users authorize your application to use Zala on their behalf. Quite briefly,
when a user installs it you can obtain an access token (a secret string denoting your rights over his business), which
you have to include in the header of every request (as shown in the above example).

Read the [authentication guide](https://github.com/zala-team/zala-api-docs/blob/master/resources/authentication.md) to get
started.


Identify your app
-----------------

In every API request, you must include a `User-Agent` header with the name of your application and a link to it or your
email address so we can get in touch with you. Here's a couple of examples:

    User-Agent: Super app (http://superapp.com/contact)
    or
    User-Agent: Awesome app (awesome@app.com)

If you don't supply this header, you will get a `400 Bad Request` response.

Just JSON
-----------------

All data is sent and received as JSON. Our format is to have no root element and we use camelCase to describe
attribute keys. This means that you have to send `Content-Type: application/json; charset=utf-8` when POSTing or PUTing
data into Zala.

You'll receive a `415 Unsupported Media Type` response code if you leave out the `Content-Type` header.


Client errors
-------------

These are the possible types of client errors on API calls that receive request bodies:

* Sending invalid JSON will result in a `400 Bad Request` response.

```
HTTP/1.1 400 Bad Request
Content-Length: 34

{"error": "Problems parsing JSON"}
```

Server errors
-------------

If Zala is having trouble, you might see a 5xx error. `500` means that the app is entirely down, but you might also
see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`. It's your responsibility in all of these
cases to retry your request later.


Rate limiting
-------------

In order to control the incoming traffic from the API, you can perform a limited number of requests in a given period of
time.
Currently, the approach used to limit the API usage is based on
a [Leaky Bucket algorithm](https://en.wikipedia.org/wiki/Leaky_bucket)
implementation, where the default bucket size is of 40 requests with a leaky rate of 2 requests per second,
which means that you can perform up to 2 requests per second, with bursts of 40 requests,
without getting a [429 Too Many Requests](http://tools.ietf.org/html/draft-nottingham-http-new-status-02#section-4)
error.
We use the following headers to provide information about the limit usage:

* `X-Rate-Limit-Limit` : Total requests that can be done in a given period, in the case, the bucket size.
* `X-Rate-Limit-Remaining` : Remaining requests to completely fill the bucket.
* `X-Rate-Limit-Reset` : Remaining milliseconds to completely empty the bucket.

Pagination
----------

Requests that return multiple items will be paginated to 30 items by default. You can specify further pages with
the `page` parameter. You can also set a custom page size up to 200 with the `size` parameter.

```shell
curl -H 'Authentication: bearer ACCESS_TOKEN ' \
  -H 'User-Agent: MyApp (name@email.com)' \
  https://api.zala.app/v1/acme_business/orders?page=2&per_page=100
```

Note that page numbering is 1-based and that omitting the `page` parameter will return the first page.

To check the total count of the results you can use the `X-Total-Count` header:

```
X-Total-Count: 156
```

To check the next and previous links for pagination you can use the `Link` header:

```
Link: <https://api.zala.app/v1/acme_business/orders?page=3&size=100>; rel="next",
  <https://api.zala.app/v1/acme_business/orders?page=50&size=100>; rel="last"
```

The possible `rel` values are:

* `next` : Shows the URL of the immediate next page of results.
* `last` : Shows the URL of the last page of results.
* `first` : Shows the URL of the first page of results.
* `prev` : Shows the URL of the immediate previous page of results.

You **should** use the `Link` URLs instead of building your own.

HTTP Verbs
----------

Where possible, API v1 strives to use appropriate HTTP verbs for each action.

* `GET` : Used for retrieving resources.
* `POST` : Used for creating resources.
* `PUT` :  Used for replacing resources or collections. For PUT requests with no body attribute, be sure to set the
  Content-Length header to zero.
* `DELETE` : Used for deleting resources.

Suspension of API access due to lack of payment
-----------------------------------------------

Being a monthly subscription service, it's possible that a business will not renew its service.
In this case, the business will go offline and the API will be inaccessible.

In either case, all API calls will return a `402 Payment Required`
response, [Webhooks](https://github.com/zala-team/zala-api-docs/blob/master/resources/webhook.md) will not be called. Please
make sure you handle this error code to notify the user that he needs to resume his payment instead of displaying a
generic server error.

Once the required payment is made, the API becomes accessible again.

API resources

* [Business](https://github.com/zala-team/zala-api-docs/blob/master/resources/business.md)
* [Customer](https://github.com/zala-team/zala-api-docs/blob/master/resources/customer.md)
* [Order](https://github.com/zala-team/zala-api-docs/blob/master/resources/order.md)
* [Service](https://github.com/zala-team/zala-api-docs/blob/master/resources/service.md)
* [Service Prices](https://github.com/zala-team/zala-api-docs/blob/master/resources/service_price.md)
* [Service Variant](https://github.com/zala-team/zala-api-docs/blob/master/resources/service_variant.md)
* [Venue](https://github.com/zala-team/zala-api-docs/blob/master/resources/venue.md)
* [Webhook (In development)](https://github.com/zala-team/zala-api-docs/blob/master/resources/webhook.md)

Help us make it better
----------------------

Please tell us how we can make the API better. If you have a specific feature request or if you found a bug, please use
GitHub issues. Feel free to fork these docs and send a pull request with improvements.

To talk with us about the API, feel free to write to <mailto:api@zala.app>.
