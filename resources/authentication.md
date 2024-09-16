# Authentication

We provide authorization and user authentication by a restricted form of OAuth 2. In a glance:

- The only grant type we support is the ["authorization code"](https://oauth.net/2/grant-types/authorization-code/) one.
- Our [access tokens](https://oauth.net/2/access-tokens/) don't expire. They become invalid only after you get a new one, or if the user uninstalls your app.
- Along with the access token we provide an `business_id`, which is the ID of the business. This key is mandatory for making requests to our API. It is also useful for authenticating app users on your website (see User authentication).

## Introduction

When creating an app, you'll be prompted to complete some information regarding your application URLs ([see URLs](https://github.com/zala-team/zala-api-docs/blob/master/resources/authentication.md#urls)). 
You also have to select the scopes your App needs and set a redirection URL, which is the URL where we will redirect the client once the application is installed. The scopes specify the resources for which the app is asking authorization of its users ([see Scopes](https://github.com/zala-team/zala-api-docs/blob/master/resources/authentication.md#scopes)). The redirection URL is used as part of the authorization flow (see below).

## Authorization flow

The authorization flow is pretty standard:

1. The user, from his Zala admin, clicks on a button to install your app. Or, alternatively, he goes to https://zala.app/apps/(app_id)/authorize (if he is not logged in, he is prompted to do so).
2. He is redirected to a page where he has to authorize the scopes your app needs (if he has already done it, this step is skipped).
3. He is redirected to your app's redirection URL with an authorization code, which expires in 30 seconds.
4. Using your app's credentials and the authorization code, you can obtain an access token by making a POST request to https://zala.app/apps/authorize/token. (Don't forget to also send `grant_type=authorization_code`, see the example below).

#### Example

Assume that your app has:

- id = 123
- redirection URL = https://www.example.com/
- scopes = `read_orders`, `read_services`, `read_transactions`
- client_secret = <your-client-secret>

1. Business with ID acme_business goes to https://zala.app/apps/123/authorize?state=csrf-code
2. User accepts.
3. He gets redirected to https://zala.app/?code=xyz&state=csrf-code.
4. Then you do:

```sh
curl -d '{"clientId": "0000", "clientSecret": "xxxxxxxx", "grantType": "authorization_code", "code": "xxxxxxxx" }' \
-X POST "https://zala.app/apps/authorize/token"
```

and receive:

```json
{
  "accessToken": "61181d08b7e328d256736hdcb671c3ce50b8af5",
  "tokenType": "bearer",
  "scope": "read_orders,read_services,read_transactions",
  "userId": "acme_business"
}
```

> At some requests, you will get the response with a `business_id` field containing the data of `user_id`

## URLs

Your application URLs are important to provide the best Merchant/Consumer experience while using our platform. All of them are required.

- **URL where we will redirect to the Merchant after installing the application:** URL to which the user will be redirected after installing the App. This is your callback URL. You must use it to get the authorization code and generate the access token as explained above (see Authorization Flow).

- **URL:** URL of your admin panel that the user will access to use the application. For example, the first page the user sees after logging in your application.

- **Privacy Policy URL:** this URL is displayed in the App Details Page in the App Store. Works as a transparency tool between company and user.

- **Support URL:** URL where the user will be redirected when they need help. This could be a self-service support page or contact form. If the Support URL is empty, then the Merchant will be redirected to the support email.

- **Support Email:** contact email for support where App users can contact.

## Scopes

Apps should only ask for the access scopes they need. If an app only needs to read services, it should only ask for the `read_services` scope.

**If you ask for any write scope, the read scope is implied.**

As [Webhooks](https://github.com/zala-team/zala-api-docs/blob/master/resources/webhook.md) rely on other resources, you will only be able to register webhooks for the resources you were granted permission to use.

The available scopes for the API are:

- read_business
  - Business
- read_services
  - Service
  - Service Variant
- read_customers
  - Customer
  - Customer Pets
- read_orders
  - Order
- read_transactions
  - Transaction
- read_venues
  - Venue
