
# ASP.NET Core Securing APIs

Part 1 see: https://github.com/damienbod/aspnetcore-standup-authn-authz

- Protecting APIs Authorization Overview
  - An overview
  - Delegated APIs (UI authentication)
  - Application APIs (no user)
  - On behalf of flow
  - OAuth 2.0 Token Exchange OIDC Downstream API
  - APIs protected with Client/server certificate authentication
  - Client binding, MTLS, DPOP, FAPI
- Azure Continuous access & continuous access evaluation protected APIs
- General API security topics
  - Mixing auth, no auth APIs in an application
  - Using Swagger with protected APIs
  - Using multiple IDPs in an API service
  - Mixing UIs and APIs in an application
  - SignalR, web sockets
  - Testing secure APIs
  - Securing Web hooks
- Legacy auth, unsecure flows, using network security
- Proxies

# Protecting APIs Authorization Overview

[About tokens](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/tokens.md)

- [JWT access tokens](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/self-contained-tokens-cc.md)
- [Introspection (Stateful)](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/api_introspection.md)
- [Cookies](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/api_cookies.md)
- Client/server certificate authentication
- User access tokens versus application access tokens 

## Delegated APIs (UI authentication)

- [API security using JWT access tokens](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/delegated-tokens.md)
  - No logout possible
- [API security using introspection](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/api_introspection.md)
  - Logout with the revocation endpoint
- API security using cookies
  - [BFF Backend for frontend](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/api_cookies.md)
  - [Reverse proxy](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/Reverse-proxy.md)

## Application APIs (no user)

- [OAuth2 Resource Owner Credentials Flow](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/Resource%20Owner%20Credentials%20Flow.md)
- Microsoft Identity Platform
  - [Azure Managed Identities](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/managed-identities.md)
  - [secrets GRPC application client](https://github.com/damienbod/GrpcAzureAppServiceAppAuth)
  - [certificates with client assertions](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/msal-client_assertions.md)

## On behalf of flow

- [Microsoft Identity Platform](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow#protocol-diagram)
- [AAD with OpenIddict downstream API](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/obo_aad_openiddict.md)

## OAuth 2.0 Token Exchange

- [OIDC with Downstream API](https://github.com/damienbod/OAuthGrantExchangeOidcDownstreamApi)

## APIs protected with Client/server certificate authentication

- [Certificate authentication](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/cert-auth/cert-auth.md)

## Client binding, MTLS, DPOP, FAPI

- MTLS
- [Securing APIs using ASP.NET Core and OAuth 2.0 DPoP](https://damienbod.com/2023/08/14/securing-apis-using-asp-net-core-and-oauth-2-0-dpop/)
- FAPI

# Azure Continuous access & continuous access evaluation protected APIs

- [CA & CAE](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/cae/cae.md)

# General API security topics
  
## Mixing auth, no auth APIs in an application

- [Mixing auth, no auth APIs](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/auth-no-auth.md)

## Using Swagger with protected APIs

- [Swagger with tokens](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/swagger.md)

## Using multiple IDPs in an API service

- [Using multiple IDPs in APIs 01](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/apis-multiple-idps.md)
- [Using multiple IDPs in APIs 02 ForwardDefaultSelector](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/apis-multiple-idps_02.md)
- [Using multiple IDPs in APIs 03 AADs](https://github.com/damienbod/AspNetCoreApiAuthMultiIdentityProvider/blob/main/AadMultiApis/MultiAADWebApi/HostingExtensions.cs)
- [Using multiple IDPs in APIs 04 AAD, Auth0, OIDC](https://github.com/damienbod/AspNetCoreApiAuthMultiIdentityProvider/blob/main/MultiIdentityProvider/WebApi/HostingExtensions.cs)

## Mixing UIs and APIs in an application

- [Mixing UI with API](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/ui-mixed-api.md)

## SignalR, web sockets

- Web sockets protected with cookies
- Web sockets protected with tokens

## Testing secure APIs

- Testing application APIs
- Testing delegated APIs
  - UI test tools
  
## Securing Web hooks

# Legacy, unsecure flows, using network security

- What about shared access signature (SAS) tokens?
- What about username & password to get an access token?
- Firewalls, HTTPS termination?

# Proxies

- Yarp Reverse Proxy
- dev Proxies
- Gateways

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

## BFF (Backend for Frontend security architecture)

https://github.com/damienbod/Blazor.BFF.OpenIDConnect.Template

https://github.com/damienbod/Blazor.BFF.AzureAD.Template

https://github.com/damienbod/Blazor.BFF.AzureB2C.Template

https://github.com/DuendeSoftware/BFF

https://github.com/damienbod/bff-aspnetcore-angular

https://github.com/damienbod/bff-auth0-aspnetcore-angular

https://github.com/damienbod/bff-openiddict-aspnetcore-angular

https://github.com/damienbod/bff-azureadb2c-aspnetcore-angular

https://github.com/isolutionsag/aspnet-react-bff-proxy-example

https://github.com/damienbod/bff-aspnetcore-vuejs

## Session protection

https://github.com/andrewlock/NetEscapades.AspNetCore.SecurityHeaders

## Self soverign identity (SSI), Azure VC

https://docs.microsoft.com/en-us/azure/active-directory/verifiable-credentials/verifiable-credentials-configure-tenant

https://github.com/Azure-Samples/active-directory-verifiable-credentials-dotnet

https://github.com/swiss-ssi-group

## Azure Functions security

https://github.com/damienbod/AzureFunctionsSecurity

## Certification authentication

https://docs.microsoft.com/en-us/aspnet/core/security/authentication/certauth

https://docs.microsoft.com/en-us/aspnet/core/grpc/authn-and-authz#client-certificate-authentication

https://github.com/damienbod/AspNetCoreCertificates/tree/main/examplesUsingCertificateAuthentication/AzureCertAuth

https://damienbod.com/2022/02/07/using-blazor-with-a-yarp-downstream-api-protected-using-certificate-authentication/

https://www.rfc-editor.org/rfc/rfc5246

## OAUTH RFCs and draft standards

[OAuth 2.0 for Browser-Based Apps draft](https://datatracker.ietf.org/doc/draft-ietf-oauth-browser-based-apps/)
  
[OAuth 2.0 Security Best Current Practice draft](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics)

[Cross-Device Flows: Security Best Current Practice draft](https://datatracker.ietf.org/doc/draft-ietf-oauth-cross-device-security/)

[OAuth 2.0 Demonstrating Proof of Possession (DPoP) RFC 9449](https://datatracker.ietf.org/doc/html/rfc9449)

[OAuth 2.0 Step Up Authentication Challenge Protocol RFC 9470](https://datatracker.ietf.org/doc/rfc9470/)

[OAuth 2.0 Token Exchange RFC 8693](https://datatracker.ietf.org/doc/html/rfc8693)

[OAuth 2.0 Pushed Authorization Requests RFC 9126](https://datatracker.ietf.org/doc/html/rfc9126)

[The OAuth 2.0 Authorization Framework: JWT-Secured Authorization Request (JAR) RFC 9101](https://datatracker.ietf.org/doc/rfc9101/)

[OAuth 2.0 Rich Authorization Requests RFC 9396](https://datatracker.ietf.org/doc/html/rfc9396)