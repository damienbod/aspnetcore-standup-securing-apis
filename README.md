
# ASP.NET Core Securing APIs

Part 1 see: https://github.com/damienbod/aspnetcore-standup-authn-authz

- Delegated APIs (UI authentication)
- Application APIs (no user)
- On behalf of flow
- Microsoft Identity Platform CAE APIs
- Mixing auth, no auth APIs in an application
- Swagger auth
- Using multiple IDPs in an API service
- Mixing UIs and APIs in an application
- Using certificates to protect APIs
- Legacy, unsecure flows

## Delegated APIs (UI authentication)

- API security using JWT access tokens
  - No logout possible
- [API security using introspection](https://github.com/damienbod/spnetcore-standup-securing-apis/blob/main/images/api_introspection.md)
  - Logout with the revocation endpoint
- API security using cookies
  - BFF Backend for frontend
  - Reverse proxy

  
## Application APIs (no user)

- [OAuth2 Resource Owner Credentials Flow](https://github.com/damienbod/spnetcore-standup-securing-apis/blob/main/images/Resource%20Owner%20Credentials%20Flow.md)
- Microsoft Identity using application tokens
  - [Azure Managed Identities](https://github.com/damienbod/spnetcore-standup-securing-apis/blob/main/images/managed-identities.md)
  - secrets
  - certificates with client assertions
- Microsoft Identity Platform using Managed Identities

## On behalf of flow

- Microsoft Identity Platform

## Microsoft Identity Platform CAE APIs

- Step up using CAE
- Graph / Office CAE

## Mixing auth, no auth APIs in an application

## Swagger auth

## Using multiple IDPs in an API service

- Schemes
- Policies

## Mixing UIs and APIs in an application

- Schemes

## Using certificates to protect APIs

- using a client/server certificates
- mtls and the access token

## Legacy, unsecure flows

- What about SAS?
- What about username & password to get an access token?


# Protecting APIs

- JWT Bearer Tokens
- [Introspection](https://github.com/damienbod/aspnetcore-standup-authn-authz/blob/main/images/api_introspection.md)
- Cookies
- User access tokens versus application access tokens 




# Examples identity provider clients:

## OIDC clients

OpenIddict with Razor Pages, Blazor WASM BFF and Angular OpenID Connect Code Flow with PKCE clients and ASP.NET Core APIs

https://github.com/damienbod/AspNetCoreOpeniddict

Differrent ASP.NET Core applications using OpenID Connect Hybrid flow Code Flow, Code Flow with PKCE, JWT APIs, Device Code flow. Force ASP.NET Core OpenID Connect client to require MFA. Send MFA signin requirement to OpenID Connect server using ASP.NET Core Identity and IdentityServer4. Requiring MFA for Admin Pages in an ASP.NET Core Identity application. Require user password verification with ASP.NET Core Identity to access Razor Page

https://github.com/damienbod/AspNetCoreHybridFlowWithApi

Auth0 with Angular and an ASP.NET Core API

https://github.com/damienbod/Auth0AngularAspNetCoreApi

Securing Blazor Web assembly using Cookies and Auth0, securing multiple Auth0 APIs in ASP.NET Core using OAuth Bearer tokens, securing OAuth Bearer tokens from multiple Identity Providers in an ASP.NET Core API

https://github.com/damienbod/SeparatingApisPerSecurityLevel

Securing an ASP.NET Core API which uses multiple access tokens, securing a Web API using multiple token servers

https://github.com/damienbod/ApiJwtWithTwoSts

## Azure AD, Azure AD B2C clients, Continuous Access

Azure samples: ASP.NET Core Web App which lets sign-in users (including in your org, many orgs, orgs + personal accounts, sovereign clouds) and call Web APIs (including Microsoft Graph)

https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/

Azure AD flows using ASP.NET Core and Microsoft.Identity for user, application aunthentication, authorization.

https://github.com/damienbod/AzureADAuthRazorUiServiceApiCertificate

Examples of implementing UIs, APIs using Azure AD as the token server with Angular, ASP.NET Core clients

https://github.com/damienbod/AzureAD-Auth-MyUI-with-MyAPI

Azure AD Continuous Access in an ASP.NET Core Razor Page app using a Web API, Azure AD Continuous Access (CA) step up with ASP.NET Core Blazor using a Web API, Azure AD Continuous Access (CA) standalone with Blazor ASP.NET Core, Force MFA in Blazor using Azure AD and Continuous Access

https://github.com/damienbod/AspNetCoreAzureADCAE

Securing ASP.NET Core Razor, Web APIs with Azure B2C external and Azure AD internal identities. Using Azure security groups in ASP.NET Core with an Azure B2C Identity Provider. Create Azure B2C users with Microsoft Graph and ASP.NET Core. Transforming identity claims in ASP.NET Core and Cache. Onboarding new users in an ASP.NET Core application using Azure B2C. Using multiple Azure B2C user flows from ASP.NET Core

https://github.com/damienbod/azureb2c-fed-azuread

PWA with Blazor Backend for frontend (BFF) and Azure B2C

https://github.com/damienbod/PwaBlazorBffAzureB2C

Add extra claims to an Azure B2C user flow using API connectors and ASP.NET Core. Implement certificate authentication in ASP.NET Core for an Azure B2C API connector

https://github.com/damienbod/AspNetCoreB2cExtraClaims

# Links:

Different links for security which can be used with ASP.NET Core.

## Docs

https://docs.microsoft.com/en-us/aspnet/core/security/

https://openid.net/connect/

https://oauth.net/2/

## GRPC

https://docs.microsoft.com/en-us/aspnet/core/grpc/authn-and-authz

## Microsoft Graph

https://docs.microsoft.com/en-us/graph/overview?view=graph-rest-1.0

https://github.com/damienbod/AspNetCoreBlazorMicrosoftGraph

https://github.com/damienbod/TeamsAdminUI

## YARP reverse proxy

https://github.com/microsoft/reverse-proxy

## Data Encryption, Certificates, Identity

https://github.com/damienbod/SendingEncryptedData

https://github.com/damienbod/AspNetCoreCertificates

## FIDO2

https://github.com/passwordless-lib/fido2-net-lib

https://github.com/damienbod/AspNetCoreIdentityFido2Mfa

## Microsoft.Identity.Web

https://github.com/AzureAD/microsoft-identity-web

## OpenIddict

https://github.com/openiddict/openiddict-core

## Keycloak

https://www.keycloak.org/

https://github.com/tuxiem/AspNetCore-keycloak

## DuendeSoftware

https://github.com/DuendeSoftware

## Auth0

https://auth0.com

## Blazor BFF

https://github.com/damienbod/Blazor.BFF.OpenIDConnect.Template

https://github.com/damienbod/Blazor.BFF.AzureAD.Template

https://github.com/damienbod/Blazor.BFF.AzureB2C.Template

https://github.com/DuendeSoftware/BFF

## Session protection

https://github.com/andrewlock/NetEscapades.AspNetCore.SecurityHeaders

## Self soverign identity (SSI), Azure VC

https://docs.microsoft.com/en-us/azure/active-directory/verifiable-credentials/verifiable-credentials-configure-tenant

https://github.com/Azure-Samples/active-directory-verifiable-credentials-dotnet

https://github.com/swiss-ssi-group

## Azure Functions security

https://github.com/damienbod/AzureFunctionsSecurity
