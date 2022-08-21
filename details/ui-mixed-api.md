
# Mixing a UI and an API client

```csharp
string[]? initialScopes = configuration.GetValue<string>("UserApiOne:ScopeForAccessToken")?.Split(' ');

services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    options.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultAuthenticateScheme = CookieAuthenticationDefaults.AuthenticationScheme;
}).AddMicrosoftIdentityWebApp(configuration, "AzureAd", OpenIdConnectDefaults.AuthenticationScheme)
    .EnableTokenAcquisitionToCallDownstreamApi(initialScopes)
    .AddMicrosoftGraph("https://graph.microsoft.com/beta",
        "User.ReadBasic.All user.read")
    .AddInMemoryTokenCaches();

services.AddAuthentication("MyJwtApiScheme")
    .AddMicrosoftIdentityWebApi(configuration, "AzureAdMyApi", "MyJwtApiScheme");

services.AddControllersWithViews(options =>
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));

services.AddRazorPages().AddMvcOptions(options =>
{
    //var policy = new AuthorizationPolicyBuilder()
    //    .RequireAuthenticatedUser()
    //    .Build();
    //options.Filters.Add(new AuthorizeFilter(policy));
}).AddMicrosoftIdentityUI();
```

JWT API

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Swashbuckle.AspNetCore.Annotations;

namespace BlazorBffAzureADWithApi.Server.Controllers;

[Route("api/[controller]")]
[ApiController]
[Authorize(AuthenticationSchemes = "MyJwtApiScheme")]
[Produces("application/json")]
[SwaggerTag("Using to provide a public api for different clients")]
public class MyApiJwtProtectedController : ControllerBase
{
    [HttpGet]
    [ProducesResponseType(StatusCodes.Status200OK, Type = typeof(string))]
    [SwaggerOperation(OperationId = "MyApiJwtProtected-Get", Summary = "Returns string with details")]
    public IActionResult Get()
    {
        return Ok("yes my public api protected with Azure AD and JWT works");
    }
}
```

Cookies

```csharp
using Microsoft.AspNetCore.Authentication.Cookies;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;

namespace BlazorBffAzureADWithApi.Server.Controllers;

[ValidateAntiForgeryToken]
[Authorize(AuthenticationSchemes = CookieAuthenticationDefaults.AuthenticationScheme)]
[ApiController]
[Route("api/[controller]")]
public class DirectApiController : ControllerBase
{
    [HttpGet]
    public IEnumerable<string> Get()
    {
        return new List<string> { "some data", "more data", "loads of data" };
    }
}
```


# Links

https://github.com/damienbod/AspNetCore6Experiments/blob/main/BlazorBffAzureADWithApi/Server/Program.cs

https://damienbod.com/2021/10/04/implement-a-secure-api-and-a-blazor-app-in-the-same-asp-net-core-project-with-azure-ad-authentication/