## Authenticate User

This message is a request to the plugin to authenticate and authorize a user. Along with the user credentials GoCD server sends the `<authConfig/>`
and its corresponding `<roleConfig/>`.

In cases where multiple `<authConfig/>` are configured for a plugin, GoCD sends this message for each `<authConfig/>` along
with it's corresponding `<roleConfig/>` until a successful authentication.

GoCD forces a perodic re-authentication of users, this is to ensure any changes like removing of users or roles in the external authorization server are reflected in GoCD.

<p class='request-name-heading'>Request name</p>

`go.cd.authorization.authenticate-user`

<p class='request-body-heading'>Request body</p>

> The plugin will receive the following JSON body for password based authorization —

```json
{
  "credentials" : {
    "username": "jdoe",
    "password": "secret"
  },
  "auth_configs": [{
    "id": "internal_ldap",
    "configuration": {
      "url": "ldap://ldap1.example.com"
    }
  }],
  "role_configs": [{
    "name": "admin",
    "auth_config_id": "internal_ldap",
    "configuration": {
      "memberOf": "ou=some-value"
    }
  },
  {
    "name": "view",
    "auth_config_id": "internal_ldap",
    "configuration": {
      "memberOf": "ou=some-value"
    }
  }]
}
```

<p class='attributes-table-follows'></p>

| Key            | Type     | Description                                                                   |
|----------------|----------|-------------------------------------------------------------------------------|
| `credentials`  | `Object` | For a password based plugins, the server sends the 'username' and 'password' provided by the user at the time of login. For web based plugins, the server sends the data received from [Fetch Access Token](#fetch-access-token) call made to the plugin.|
| `auth_configs` | `Object` | This key contains a `<authconfig>` passed as a list.  |
| `role_configs` | `Object` | This key contains list of `<roleconfig>` configured corresponding to the given `<authConfig\>`|

<p class='response-code-heading'>Response Body</p>

> An example response body —

```json
{
  "user": {
    "username": "jdoe",
    "display_name": "John Doe",
    "email_id": "jdoe@example.com"
  },
  "roles": ["blackbird", "spacetiger"]
}
```

The response body will contain the following JSON elements:

<p class='attributes-table-follows'></p>

| Key                   | Type      | Description |
| --------------------- | --------- | ----------- |
| `user`                | `Object`  | This json object contains user details like `username`, `display_name` and `email_id`. |
| `roles`               | `Array`   | Array of roles associated with authenticated user. |

The plugin is expected to return status `200` if it can understand the request.
