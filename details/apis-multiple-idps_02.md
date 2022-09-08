
![multiple APIs same IDP](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/api_multi_idps_02.png)

# Using mutliple identity providers in APIs using ForwardDefaultSelector

```csharp
public void ConfigureServices(IServiceCollection services)
{
	services.AddAuthentication(options =>
	{
		options.DefaultScheme = "UNKNOWN";
		options.DefaultChallengeScheme = "UNKNOWN";

	})
	.AddJwtBearer(Consts.MY_AUTH0_SCHEME, options =>
	{
		options.Authority = "https://dev-damienbod.eu.auth0.com/";
		options.Audience = "https://auth0-api1";
		options.TokenValidationParameters = new TokenValidationParameters
		{
			ValidateIssuer = true,
			ValidateAudience = true,
			ValidateIssuerSigningKey = true,
			ValidAudiences = Configuration.GetSection("ValidAudiences").Get<string[]>(),
			ValidIssuers = Configuration.GetSection("ValidIssuers").Get<string[]>()
		};
	})
	.AddJwtBearer(Consts.MY_AAD_SCHEME, jwtOptions =>
	{
		jwtOptions.MetadataAddress = Configuration["AzureAd:MetadataAddress"]; 
		jwtOptions.Authority = Configuration["AzureAd:Authority"];
		jwtOptions.Audience = Configuration["AzureAd:Audience"]; 
		jwtOptions.TokenValidationParameters = new TokenValidationParameters
		{
			ValidateIssuer = true,
			ValidateAudience = true,
			ValidateIssuerSigningKey = true,
			ValidAudiences = Configuration.GetSection("ValidAudiences").Get<string[]>(),
			ValidIssuers = Configuration.GetSection("ValidIssuers").Get<string[]>()
		};
	})
	.AddPolicyScheme("UNKNOWN", "UNKNOWN", options =>
	{
		options.ForwardDefaultSelector = context =>
		{
			string authorization = context.Request.Headers[HeaderNames.Authorization];
			if (!string.IsNullOrEmpty(authorization) && authorization.StartsWith("Bearer "))
			{
				var token = authorization.Substring("Bearer ".Length).Trim();
				var jwtHandler = new JwtSecurityTokenHandler();

				if(jwtHandler.CanReadToken(token)) // it's a self contained access token and not encrypted
				{
					var issuer = jwtHandler.ReadJwtToken(token).Issuer; //.Equals("B2C-Authority"))
					if(issuer == Consts.MY_OPENIDDICT_ISS) // OpenIddict
					{
						return OpenIddictValidationAspNetCoreDefaults.AuthenticationScheme;
					}

					if (issuer == Consts.MY_AUTH0_ISS) // Auth0
					{
						return Consts.MY_AUTH0_SCHEME;
					}

					if (issuer == Consts.MY_AAD_ISS) // AAD
					{
						return Consts.MY_AAD_SCHEME;
					}
				}
			}

			// We don't know with it is
			return Consts.MY_AAD_SCHEME;
		};
	});

	// Register the OpenIddict validation components.
	services.AddOpenIddict() // Scheme = OpenIddictValidationAspNetCoreDefaults.AuthenticationScheme
		.AddValidation(options =>
		{
			// Note: the validation handler uses OpenID Connect discovery
			// to retrieve the address of the introspection endpoint.
			options.SetIssuer("https://localhost:44318/");
			options.AddAudiences("rs_dataEventRecordsApi");

			// Configure the validation handler to use introspection and register the client
			// credentials used when communicating with the remote introspection endpoint.
			//options.UseIntrospection()
			//        .SetClientId("rs_dataEventRecordsApi")
			//        .SetClientSecret("dataEventRecordsSecret");

			// disable access token encyption for this
			options.UseAspNetCore();

			// Register the System.Net.Http integration.
			options.UseSystemNetHttp();

			// Register the ASP.NET Core host.
			options.UseAspNetCore();
		});

	services.AddSingleton<IAuthorizationHandler, AllSchemesHandler>();

	services.AddAuthorization(options =>
	{
		options.AddPolicy(Consts.MY_POLICY_ALL_IDP, policyAllRequirement =>
		{
			policyAllRequirement.Requirements.Add(new AllSchemesRequirement());
		});
	});

	services.AddControllers();
}
```

## AllSchemesHandler class

```
using Microsoft.AspNetCore.Authorization;

namespace WebApi;

public class AllSchemesHandler : AuthorizationHandler<AllSchemesRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        AllSchemesRequirement requirement)
    {
        var issuer = string.Empty;
  
        var issClaim = context.User.Claims.FirstOrDefault(c => c.Type == "iss");
        if (issClaim != null)
            issuer = issClaim.Value;

        if (issuer == Consts.MY_OPENIDDICT_ISS) // OpenIddict
        {
            var scopeClaim = context.User.Claims.FirstOrDefault(c => c.Type == "scope" 
                && c.Value == "dataEventRecords");
            if (scopeClaim != null)
            {
                // scope": "dataEventRecords",
                context.Succeed(requirement);
            }
        }

        if (issuer == Consts.MY_AUTH0_ISS) // Auth0
        {
            // add require claim "gty", "client-credentials"
            var azpClaim = context.User.Claims.FirstOrDefault(c => c.Type == "azp"
                && c.Value == "naWWz6gdxtbQ68Hd2oAehABmmGM9m1zJ");
            if (azpClaim != null)
            {
                // "azp": "naWWz6gdxtbQ68Hd2oAehABmmGM9m1zJ",
                context.Succeed(requirement);
            }
        }

        if (issuer == Consts.MY_AAD_ISS) // AAD
        {
            // "azp": "46d2f651-813a-4b5c-8a43-63abcb4f692c",
            var azpClaim = context.User.Claims.FirstOrDefault(c => c.Type == "azp"
                && c.Value == "46d2f651-813a-4b5c-8a43-63abcb4f692c");
            if (azpClaim != null)
            {
                // "azp": "naWWz6gdxtbQ68Hd2oAehABmmGM9m1zJ",
                context.Succeed(requirement);
            }
        }

        return Task.CompletedTask;
    }
}

```

## Use the scheme and the handler

```csharp
[Authorize(AuthenticationSchemes = Consts.ALL_MY_SCHEMES, Policy = Consts.MY_POLICY_ALL_IDP)]
[Route("api/[controller]")]
public class ValuesController : Controller
{
    [HttpGet]
    public IEnumerable<string> Get()
    {
        return new string[] { "data 1 from the api", "data 2 from the api" };
    }
}

```

# Links

https://github.com/damienbod/SeparatingApisPerSecurityLevel

https://github.com/damienbod/SeparatingApisPerSecurityLevel/tree/main/MyApi/Controllers

https://github.com/damienbod/AzureADAuthRazorUiServiceApiCertificate/tree/main/BlazorWithApis/BlazorAzureADWithApis/Server/Services
