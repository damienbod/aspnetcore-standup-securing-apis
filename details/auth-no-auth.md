

# Mixing auth and no auth APIs in a project

```csharp
services.AddControllers(options =>
{
	var policy = new AuthorizationPolicyBuilder()
		.RequireAuthenticatedUser()
		 // if using multiple schemes, you need to define them.
		 //    .AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme, "MyJwtApiScheme", "OpenIdConnect")  
		.Build();
	options.Filters.Add(new AuthorizeFilter(policy));
});

services.AddAuthorization(options =>
{
	options.AddPolicy("ValidateAccessTokenPolicy", validateAccessTokenPolicy =>
	{
		// Validate ClientId from token
		// only accept tokens issued ....
		validateAccessTokenPolicy.RequireClaim("azp", "ad6b0351-92b4-4ee9-ac8d-3e76e5fd1c67");
	});
});
```

```csharp
[HttpGet]
[AllowAnonymous]
public async Task<IActionResult> GetUnsecureData()
{
```
