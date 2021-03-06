---
url: /apis
title: APIs Overview
description: Learn the basics of APIs, their role in OAuth and how to configure an API in Auth0 Dashboard.
---

# APIs

## Overview

An API is an entity that represents an external resource, capable of accepting and responding to protected resource requests made by clients. At the [OAuth2 spec](https://tools.ietf.org/html/rfc6749) an API maps to the **Resource Server**.

When a client wants to access an API's protected resources it must provide an access token. The same access token can be used to access the API's resources without having to authenticate again, until it expires.

Each API has a set of defined permissions. Clients can request a subset of those defined permissions when they execute the authorization flow, and include them in the access token as part of the **scope** request parameter.

For example, an API that holds a user's appointments, may accept two different levels of authorization: read only (scope `read:appointments`) or write (scope `write:appointments`). When a client asks the API to list a user's appointments, then the access token should contain the `read:appointments` scope. In order to edit an existing appointment or create a new one, the access token should contain the `write:appointments` scope.

> For more information on tokens please refer to: [Tokens used by Auth0](/tokens).

## How to configure an API in Auth0

If this is your first API, you will have to enable the API functionality first.

Navigate to your [Account Advanced Settings](${manage_url}/#/account/advanced), scroll down to the *Settings* section and toggle the **Enable APIs Section** switch.

![Enable APIs Section](/media/articles/api/overview/enable-api-section.png)

Click on the [APIs menu option](${manage_url}/#/apis) on the left.

> The API tab will already have one API created automatically, the **Auth0 Management API**. For more details on the features of the Management API and its available endpoints, refer to: [Management API](/api/management/v2).

Click the **+ Create API** button.

![Create a new API](/media/articles/api/overview/create-api.png)

You need to provide the following information for your API:

- **Name**: a friendly name for the API. Does not affect any functionality.

- **Identifier**: a unique identifier for the API. We recommend using a URL but note that this doesn't have to be a publicly available URL, Auth0 will not call your API at all. This value **cannot be modified** afterwards.

- **Signing Algorithm**: the algorithm to sign the tokens with. The available values are `HS256` and `RS256`. When selecting `RS256` the token will be signed with the tenant's private key. For more details on the signing algorithms go to the [Signing Algorithms paragraph](#signing-algorithms).

Fill in the required information and click the **Create** button.

Once you do so you will be navigated to the *Quick Start* of your API. Here you can find details on the implementation changes you have to do to your API, which basically consists of choosing a JWT library from a predefined list and configuring this library to validate the access tokens in your API.

![API Quick Starts](/media/articles/api/overview/quickstarts-view.png)

**NOTE**: Keep in mind that we are working on building quickstarts for more stacks, apart from those currently available.

The other available views for your API are:

- **Settings**: lists the settings for your API. Some are editable. Here you can change the token expiration time and enable offline access (this way Auth0 will allow clients to ask for Refresh Tokens for this API).

- **Scopes**: here you can define the scopes for this API, by setting a name and a description.

- **Non Interactive Clients**: lists your Non Interactive Clients. You can authorize which Non Interactive Clients can request access tokens for your API. You can optionally select a subset of the defined scopes to further limit the access that an authorized client has. Only Non Interactive Clients require explicit permission. That is because, when you authorize a non-interactive Client to access an API, Auth0 is creating a Client Grant for that Client. For more details on this case refer to: [Setting up a Client Credentials Grant using the Management Dashboard](/api-auth/config/using-the-auth0-dashboard).

- **Test**: from this view you can execute a sample Client Credentials flow with any of your Authorized Non-Interactive Clients to check that everything is working as expected.

### Signing Algorithms

When you create an API you have to select the algorithm your tokens will be signed with. The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.

> The signature is part of a JWT. If you are not familiar with the JWT structure please refer to: [JSON Web Tokens (JWTs) in Auth0](/jwt#what-is-the-json-web-token-structure-).

To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that. That algorithm, which is part of the JWT header, is the one you select for your API: `HS256` or `RS256`.

- **RS256** is an [asymmetric algorithm](https://en.wikipedia.org/wiki/Public-key_cryptography) which means that there are two keys: one public and one private (secret). Auth0 has the secret key, which is used to generate the signature, and the consumer of the JWT has the public key, which is used to validate the signature.

- **HS256** is a [symmetric algorithm](https://en.wikipedia.org/wiki/Symmetric-key_algorithm) which means that there is only one secret key, shared between the two parties. The same key is used both to generate the signature and to validate it. Special care should be taken in order for the key to remain confidential.

The most secure practice, and our recommendation, is to use **RS256**. Some of the reasons are:

- With RS256 you are sure that only the holder of the private key (Auth0) can sign tokens, while anyone can check if the token is valid using the public key.

- Under HS256, If the private key is compromised you would have to re-deploy the API with the new secret. With RS256 you can request a token that is valid for multiple audiences.

- With RS256 you can implement key rotation without having to re-deploy the API with the new secret.

For a more detailed overview of the JWT signing algorithms refer to: [JSON Web Token (JWT) Signing Algorithms Overview](https://auth0.com/blog/json-web-token-signing-algorithms-overview/).
