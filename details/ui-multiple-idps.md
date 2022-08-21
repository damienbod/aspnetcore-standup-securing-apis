
# Using mutliple identity providers in an ASP.NET Core UI

```csharp
var services = builder.Services;
var configuration = builder.Configuration;
var env = builder.Environment;

// store the aad login
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie();

services.AddAuthentication()
    .AddMicrosoftIdentityWebApp(configuration.GetSection("AzureAdT1"), "t1", "cookiet1");

services.Configure<OpenIdConnectOptions>("t1", options =>
{
    var existingOnTokenValidatedHandler = options.Events.OnTokenValidated;
    options.Events.OnTokenValidated = async context =>
    {
        await existingOnTokenValidatedHandler(context);

        if (context.Principal != null)
        {
            await context.HttpContext.SignInAsync(
                CookieAuthenticationDefaults.AuthenticationScheme, context.Principal);
        }
    };
});

services.AddAuthentication()
    .AddMicrosoftIdentityWebApp(configuration.GetSection("AzureAdT2"), "t2", "cookiet2");

services.Configure<OpenIdConnectOptions>("t2", options =>
{
    var existingOnTokenValidatedHandler = options.Events.OnTokenValidated;
    options.Events.OnTokenValidated = async context =>
    {
        await existingOnTokenValidatedHandler(context);

        if(context.Principal != null)
        {
            await context.HttpContext.SignInAsync(
                CookieAuthenticationDefaults.AuthenticationScheme, context.Principal);
        }
    };
});
```

```json
  "AzureAdT1": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "damienbodhotmail.onmicrosoft.com",
    "TenantId": "7ff95b15-dc21-4ba6-bc92-824856578fc1",
    "ClientId": "46d2f651-813a-4b5c-8a43-63abcb4f692c",
    "CallbackPath": "/signin-oidc/t1",
    "SignedOutCallbackPath ": "/SignoutCallbackOidc"
    // "ClientSecret": "add secret to the user secrets"
  },
  "AzureAdT2": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "damienbodhotmail.onmicrosoft.com",
    "TenantId": "7ff95b15-dc21-4ba6-bc92-824856578fc1",
    "ClientId": "8e2b45c2-cad0-43c3-8af2-b32b73de30e4",
    "CallbackPath": "/signin-oidc/t2",
    "SignedOutCallbackPath ": "/SignoutCallbackOidc"
    // "ClientSecret": "add secret to the user secrets"
  },
```

# Links

https://github.com/damienbod/AspNetCore6Experiments/blob/main/AspNetCoreRazorMultiClients/Program.cs
