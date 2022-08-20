# id tokens, access tokens, refresh tokens

# Standards

- RFC 7519 JWT JSON Web Token 
- RFC 7515 JWS JSON Web Signature 
- RFC 7516 JWE JSON Web Encryption 
- RFC 7517 JWK JSON Web Key (Discovery endpoint)
- RFC 9068 JWT Profile for OAuth 2.0 Access tokens
- RFC 7009 [OAuth 2.0 Token Revocation](https://www.rfc-editor.org/rfc/rfc7009)

# id tokens

- only for the client, never intended or used by the API client.
- disable alg=none in IDP, solution
- don't use symmetric keys

## claims

	iss, aud (client_id), sub (user), exp, iat

	amr, acr, auth_time

	Azure: xms_cc claim for CAE, CA

# access tokens

- 2 types: stateful and stateless
- only for the API, never opened or used by the app client.
- use introspection if possible
- validate standard required claims
- validate signature
- disable alg=none in IDP, solution
- don't use symmetric keys
- use client assertion if possible
- use revocation if possible on logout
- On AAD, use CAE for azure services if possible
- avoid SPAs on Azure due to full logout is required

## claims

	"typ": "at+jwt" // RFC 9068 JWT Profile for OAuth 2.0 Access tokens

	iss, aud, sub, exp, iat, jti, client_id

	amr, acr, auth_time

	Azure: xms_cc claim for CAE, CA

# refresh tokens

- long lived
- used to refresh the session
- dangerous to lose...
