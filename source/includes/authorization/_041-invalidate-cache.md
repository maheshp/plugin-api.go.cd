## Invalidate Users Cache

This message allows a plugin invalidate logged in users session in GoCD.

This message can be used by plugins which listen to changes in the authorization servers to trigger a re-authentication of users in GoCD.
  
<p class='request-name-heading'>Request name</p>

`go.processor.authorization.invalidate-cache`

<p class='request-body-heading'>Request body</p>

The server will not parse a request body.

<p class='response-code-heading'>Response code</p>

The plugin is expected to return status `200` if it can understand the request.

The server will not provide any response body.
