# Stackoverflow

> https://stackoverflow.com/questions/47709451/pass-jwt-refresh-token-on-header-or-body

The jwt specification recommends (but does not require) sending the access tokens in an authorization header of type Bearer. But there is no mention of the refresh tokens.

Refresh tokens are an Oauth2 concept. If you read the Rfc6749 specification, to refresh an access token, the refresh token is sent using a form parameter in a POST request

> ## 6. Refreshing an Access Token ...

```
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA
```
 
You can use the example of oauth2 as reference (pass it in the body), although if you do not use oauth2, you have no obligation, so use the method to send that best suits your project.
