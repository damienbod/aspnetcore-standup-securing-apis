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
- avoid SPAs on Azure due to full logout trusted backend required for this

## claims

	"typ": "at+jwt" // RFC 9068 JWT Profile for OAuth 2.0 Access tokens

	iss, aud, sub, exp, iat, jti, client_id

	amr, acr, auth_time

	Azure: xms_cc claim for CAE, CA

# refresh tokens

- long lived
- used to refresh the session
- dangerous to lose...


# self contained access token

```
eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ4RWM3cktSdUN1U2tXR0thSGVMd3EwSGwtMDNmVjlVc1YzcU54YWtVUW1BIn0.eyJleHAiOjE2NTE1MTE3NDMsImlhdCI6MTY1MTUxMTQ0MywiYXV0aF90aW1lIjoxNjUxNTExMDk3LCJqdGkiOiI4ZDY5NDJkMi01YzExLTQ4YzQtYWEwMi04M2U3ZGEwMTBjZDIiLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvcmVhbG1zL215cmVhbG0iLCJhdWQiOiJvaWRjLWNvZGUtcGtjZS1hbmd1bGFyIiwic3ViIjoiMjM2OGYzZTQtMzJmYi00ZjFhLWI4NTQtZTVlY2VlZjY3Zjg4IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoib2lkYy1jb2RlLXBrY2UtYW5ndWxhciIsIm5vbmNlIjoiOTA4YTNiZDk2ZmQ2MDZmNWMwZjY4MDdhYmY5MDk5OTQ3NGg5T3pvaDMiLCJzZXNzaW9uX3N0YXRlIjoiNzM0ZjRjZDctNWFiZi00OTE3LTg2YzYtMWRkMWJjYWFhZWFjIiwiYWNyIjoiMSIsInJlc291cmNlX2FjY2VzcyI6eyJvaWRjLWNvZGUtcGtjZS1hbmd1bGFyIjp7InJvbGVzIjpbImFkbWluIl19LCJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBhY3IgcHJvZmlsZSBhcGktdXNlci1hY2Nlc3MgZW1haWwiLCJzaWQiOiI3MzRmNGNkNy01YWJmLTQ5MTctODZjNi0xZGQxYmNhYWFlYWMiLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwibmFtZSI6IkRhbWllbiBCb2QiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJkYW1pZW4iLCJnaXZlbl9uYW1lIjoiRGFtaWVuIiwiZmFtaWx5X25hbWUiOiJCb2QiLCJlbWFpbCI6ImRhbWllbl9ib2RAaG90bWFpbC5jb20ifQ.KPGdKnXUIbJVd6YAmwVk8QZZ72Tm7YM1VrWC4mHsl7Nx1JMzSW_VLKwF1rpOEX5DO-HyHZeT90js2aGnmfjmDTY7BqUeavU6seXfLZT59UmhoJN_IJTdYxYrdsNJirW77FDZ-7p0GbBmWKg1iP0cDRt4_OxqqHH9a6_dSdQJmu9t7iY7suGIO6mHNGaead6Yjdnz7LeQr5wn5LCyzBCwsrKsBGe4XWMOZP9_hEGRjI433SimpmNiaarcarCYXbmia2eDTuHIxni4TWhHggjg7mmLaXXlaTo0TsTLq8D1B60KOPDL9By2u1z4Q9ZGsj6T59Z0_jV-mZyv-ZLfmr3VIQ

```

Links

https://jwt.io

https://jwt.ms
