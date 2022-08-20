JWT Notes

Standards

JWT JSON Web Token RFC 7519
JWS JSON Web Signature RFC 7515
JWE JSON Web Encryption RFC 7516
JWK JSON Web Key RFC 7517 (Discovery endpoint)

# id_token
- iss
- aud (client_id)
- sub (user)
- exp
- iat

- amr, acr, auth_time

# access_token

stateful, stateless

RFC 9068 JWT Profile for OAuth 2.0 Access tokens

"typ": "at+jwt"

iss, aud, sub, exp, iat, jti, client_id

amr, acr, auth_time