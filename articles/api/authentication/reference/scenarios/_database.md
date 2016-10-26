# Username & Password

These connections support active (API-based) authentication and passive (browser based) authentication. Active authentication can be invoked from anywhere (a script, server to server, etc.) while passive authentication occurs in the browser through the login page.

<aside class="warning">
These endpoints only work for database connections.
</aside>


## Signup

Given a user's credentials, and a `connection`, this endpoint will create a new user using active authentication.


```http
POST https://${account.namespace}/dbconnections/signup
Content-Type: 'application/json'
{
  "client_id":   "{client_id}",
  "email":       "",
  "password":    "",
  "connection":  "",
}
```

```shell
shell
```

```javascript
javascript
```

```csharp
csharp
```

> This command returns a JSON object in this format:

```json
{
  "_id": "",
  "email_verified": false,
  "email": ""
}
```

### Query Parameters

| Parameter        | Type       | Description |
|:-----------------|:-----------|:------------|
| `client_id`      | string     | the `client_id` of your app |
| `email`          | string     | the user's email address |
| `password `      | string     | the user's desired password |
| `connection`     | string     | the name of an identity provider configured to your app |


## Login

You authenticate a database user with the [Resource Owner](#resource-owner) endpoint.


## Change Password

Given a user's `email` address and a `connection`, Auth0 will send a change password email.

```http
POST https://${account.namespace}/dbconnections/change_password
Content-Type: 'application/json'
{
  "client_id":   "{client_id}",
  "email":       "",
  "password":    "",
  "connection":  "",
}
```

```shell
shell
```

```javascript
javascript
```

```csharp
csharp
```




<aside class="notice">
For more information, see: <a href='/connections/database/password-change'>Changing a User's Password</a>.
</aside>

### HTTP Request

`POST https://${account.namespace}/dbconnections/change_password`

### Query Parameters

| Parameter        | Type       | Description |
|:-----------------|:-----------|:------------|
| `client_id`      | string     | the `client_id` of your app |
| `email`          | string     | the user's email address |
| `password `      | string     | the new password (optional, see remarks) |
| `connection`     | string     | the name of an identity provider configured to your app |

<aside class="warning">
If you are using Lock version 9 and above, do not set the password field, or you will receive a *password is not allowed* error. You can only set the password if you are using Lock version 8.
</aside>

### Remarks

* If a password is provided, when the user clicks on the confirm password change link, the new password specified in this POST will be set for this user.
* If a password is NOT provided, when the user clicks on the password change link they will be redirected to a page asking them for a new password.