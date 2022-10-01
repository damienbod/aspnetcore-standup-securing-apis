

# Certificates with client assertions

```csharp
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder
	.Create(_configuration["CallApi:ClientId"])
		.WithAuthority(new Uri(authority))
		.WithCertificate(cert)
		.Build();
```

Check for azpacr == 2

```csharp
public void ConfigureServices(IServiceCollection services)
{
	JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

	services.AddSingleton<IAuthorizationHandler, HasServiceApiRoleHandler>();

	services.AddMicrosoftIdentityWebApiAuthentication(Configuration);

	services.AddAuthorization(options =>
	{
		options.AddPolicy("ValidateAccessTokenPolicy", validateAccessTokenPolicy =>
		{
			validateAccessTokenPolicy.Requirements.Add(new HasServiceApiRoleRequirement());

			// Validate ClientId from token
			validateAccessTokenPolicy.RequireClaim("azp", Configuration["AzureAd:ClientId"]);

			// only allow tokens which used "Private key JWT Client authentication"
			// // https://docs.microsoft.com/en-us/azure/active-directory/develop/access-tokens
			// Indicates how the client was authenticated. For a public client, the value is "0". 
			// If client ID and client secret are used, the value is "1". 
			// If a client certificate was used for authentication, the value is "2".
			validateAccessTokenPolicy.RequireClaim("azpacr", "2");
		});
	});

	services.AddControllers();
}
```

```
2022-08-03 20:09:40.364 +02:00 [DBG] MSAL Request: {request}
Method: POST, RequestUri: 'https://login.microsoftonline.com/7ff95b15-dc21-4ba6-bc92-824856578fc1/oauth2/v2.0/token', Version: 1.1, Content: System.Net.Http.StreamContent, Headers:
{
  Content-Type: application/x-www-form-urlencoded
}
 
client_id=b178f3a5-7588-492a-924f-72d7887b7e48
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1...
&scope=api%3A%2F%2Fb178f3a5-7588-492a-924f-72d7887b7e48%2F.default
&grant_type=client_credentials
 
MSAL Response: {response}
StatusCode: 200, ReasonPhrase: 'OK', Version: 1.1, Content: System.Net.Http.HttpConnectionResponseContent, Headers:
{
  Content-Type: application/json; charset=utf-8
}
 
{
 "token_type":"Bearer",
 "expires_in":3599,"ext_expires_in":3599,
 "acce
```

![clientassertion](https://damienbod.files.wordpress.com/2022/08/clientassertion_token_01.png)



## Links

https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication

https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-net-client-assertions

https://docs.microsoft.com/en-us/azure/architecture/multitenant-identity/client-certificate

https://damienbod.com/2022/08/08/debug-logging-microsoft-identity-client-and-the-msal-oauth-client-credentials-flow/

