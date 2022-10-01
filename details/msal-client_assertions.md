

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


## Links

https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication

https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-net-client-assertions

https://docs.microsoft.com/en-us/azure/architecture/multitenant-identity/client-certificate

https://damienbod.com/2022/08/08/debug-logging-microsoft-identity-client-and-the-msal-oauth-client-credentials-flow/

