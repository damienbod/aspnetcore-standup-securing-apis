
![multiple APIs same IDP](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/multiple-apis.png)

# Using mutliple identity providers in APIs

```csharp
public void ConfigureServices(IServiceCollection services)
{
	JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();
	// IdentityModelEventSource.ShowPII = true;


	// Adds Microsoft Identity platform (AAD v2.0) support to protect this Api
	services.AddMicrosoftIdentityWebApiAuthentication(Configuration, "AzureAd", "myADscheme");

	services.AddAuthentication(options =>
	{
		options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
		options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
	}).AddJwtBearer(options =>
	{
		options.Authority = "https://dev-damienbod.eu.auth0.com/";
		options.Audience = "https://auth0-api1";
	});

	services.AddSingleton<IAuthorizationHandler, UserApiScopeHandler>();

	services.AddAuthorization(policies =>
	{
		policies.AddPolicy("p-user-api-auth0", p =>
		{
			p.Requirements.Add(new UserApiScopeHandlerRequirement());
			// Validate id of application for which the token was created
			p.RequireClaim("azp", "AScjLo16UadTQRIt2Zm1xLHVaEaE1feA");
		});

		policies.AddPolicy("p-service-api-auth0", p =>
		{
			// Validate id of application for which the token was created
			p.RequireClaim("azp", "naWWz6gdxtbQ68Hd2oAehABmmGM9m1zJ");
			p.RequireClaim("gty", "client-credentials");
		});
	});

	services.AddControllers(options =>
	{
		var policy = new AuthorizationPolicyBuilder()
			.RequireAuthenticatedUser()
			.Build();
		options.Filters.Add(new AuthorizeFilter(policy));
	});
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
	JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

	// ...

	app.UseRouting();

	app.UseAuthentication();
	app.UseAuthorization();

	app.UseEndpoints(endpoints =>
	{
		endpoints.MapControllers();
	});
}
```

# Links

https://github.com/damienbod/SeparatingApisPerSecurityLevel

https://github.com/damienbod/SeparatingApisPerSecurityLevel/tree/main/MyApi/Controllers

https://github.com/damienbod/AzureADAuthRazorUiServiceApiCertificate/tree/main/BlazorWithApis/BlazorAzureADWithApis/Server/Services
