

# Certificate authentication

- using a client/server certificates
- mtls and the access token

![Reverse proxy](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/Reverse-proxy.md)

## API service setup for Azure App service
```csharp
services.AddSingleton<MyCertificateValidationService>();

// Not needed for Azure App service
services.AddCertificateForwarding(options =>
{
	options.CertificateHeader = "X-ARR-ClientCert";
	options.HeaderConverter = (headerValue) =>
	{

		X509Certificate2? clientCertificate = null;
		if (!string.IsNullOrWhiteSpace(headerValue))
		{
			byte[] bytes = Convert.FromBase64String(headerValue);
			clientCertificate = new X509Certificate2(bytes);
		}

		return clientCertificate;
	};
});

services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme)
	.AddCertificate(options => // code from ASP.NET Core sample
	{
		// https://docs.microsoft.com/en-us/aspnet/core/security/authentication/certauth
		options.AllowedCertificateTypes = CertificateTypes.SelfSigned;
		
		// Default values
		//options.AllowedCertificateTypes = CertificateTypes.Chained;
		//options.RevocationFlag = X509RevocationFlag.ExcludeRoot;
		//options.RevocationMode = X509RevocationMode.Online;
		//options.ValidateCertificateUse = true;
		//options.ValidateValidityPeriod = true;

		options.Events = new CertificateAuthenticationEvents
		{
			OnCertificateValidated = context =>
			{
				var validationService =
					context.HttpContext.RequestServices.GetService<MyCertificateValidationService>();

				if (validationService!.ValidateCertificate(context.ClientCertificate))
				{
					var claims = new[]
					{
						new Claim(ClaimTypes.NameIdentifier, context.ClientCertificate.Subject, ClaimValueTypes.String, context.Options.ClaimsIssuer),
						new Claim(ClaimTypes.Name, context.ClientCertificate.Subject, ClaimValueTypes.String, context.Options.ClaimsIssuer)
					};

					context.Principal = new ClaimsPrincipal(new ClaimsIdentity(claims, context.Scheme.Name));
					context.Success();
				}
				else
				{
					context.Fail("invalid cert");
				}

				return Task.CompletedTask;
			}
		};
	});
```

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
	if (env.IsDevelopment())
	{
		app.UseDeveloperExceptionPage();
	}

	app.UseHttpsRedirection();

	app.UseRouting();

    // Can be added for local dev, or other host providers
	//app.UseCertificateForwarding();
	app.UseAuthentication();
	app.UseAuthorization();

	app.UseEndpoints(endpoints =>
	{
		endpoints.MapControllers();
	});
}
```

```csharp
public class MyCertificateValidationService 
{
    private readonly ILogger<MyCertificateValidationService> _logger;

    public MyCertificateValidationService(ILogger<MyCertificateValidationService> logger)
    {
        _logger = logger;
    }

    public bool ValidateCertificate(X509Certificate2 clientCertificate)
    {
        return CheckIfThumbprintIsValid(clientCertificate);
    }

    private bool CheckIfThumbprintIsValid(X509Certificate2 clientCertificate)
    {
        var listOfValidThumbprints = new List<string>
        {
            "723A4D916F008B8464E1D314C6FABC1CB1E926BD"
        };

        if (listOfValidThumbprints.Contains(clientCertificate.Thumbprint))
        {
            _logger.LogInformation($"Custom auth-success for certificate  {clientCertificate.FriendlyName} {clientCertificate.Thumbprint}");

            return true;
        }

        _logger.LogWarning($"auth failed for certificate  {clientCertificate.FriendlyName} {clientCertificate.Thumbprint}");

        return false;
    }
}
```

## Implement a client 

```csharp
private static async Task<JsonDocument> GetApiDataUsingHttpClientHandler()
{
    // a prod env would need to read the certificate directly from the os, or a key vault
	var cert = new X509Certificate2("client.pfx", "1234"); 
	var handler = new HttpClientHandler();
	handler.ClientCertificates.Add(cert);
	var client = new HttpClient(handler);

	var request = new HttpRequestMessage()
	{
		RequestUri = new Uri("https://azurecertauth20201108214641.azurewebsites.net/WeatherForecast"),
		Method = HttpMethod.Get,
	};
	var response = await client.SendAsync(request);
	if (response.IsSuccessStatusCode)
	{
		var responseContent = await response.Content.ReadAsStringAsync();
		Console.WriteLine(responseContent);
		var data = JsonDocument.Parse(responseContent);
		return data;
	}

	throw new ApplicationException($"Status code: {response.StatusCode}, Error: {response.ReasonPhrase}");
}
```

# Links

[GRPC example](https://github.com/damienbod/AspNetCoreCertificates/tree/main/examplesUsingCertificateAuthentication/GrpcCertAuthChainedCertificate)

[ASP.NET Core with Azure App service deployment](https://github.com/damienbod/AspNetCoreCertificates/tree/main/examplesUsingCertificateAuthentication/AzureCertAuth)

https://damienbod.com/2021/11/22/implement-certificate-authentication-in-asp-net-core-for-an-azure-b2c-api-connector/

https://github.com/damienbod/AspNetCoreB2cExtraClaims

https://docs.microsoft.com/en-us/aspnet/core/security/authentication/certauth

https://docs.microsoft.com/en-us/azure/app-service/configure-ssl-certificate-in-code

https://docs.microsoft.com/en-us/aspnet/core/grpc/authn-and-authz#client-certificate-authentication

https://github.com/damienbod/AspNetCoreCertificates/tree/main/examplesUsingCertificateAuthentication/AzureCertAuth

https://damienbod.com/2022/02/07/using-blazor-with-a-yarp-downstream-api-protected-using-certificate-authentication/

https://www.rfc-editor.org/rfc/rfc5246

https://github.com/damienbod/IdentityServer4AspNetCoreIdentityTemplate/tree/main/content/StsServerIdentity/Services/Certificate
