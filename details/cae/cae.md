
# Azure Continuous access & continuous access evaluation protected APIs

- Protecting the API using Azure Auth contexts and policies
- Step up using Azure continuous access
- Continuous access evaluation (CAE) with Graph / Office APIs

## CA with Auth Context and id token

![CA with Auth Context and id token](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/cae/setup_cae_id_token.png)

## CA with Auth Context and access token

![CA with Auth Context and access token](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/cae/setup_cae_access_token.png)

## Setup Continuous access auth contexts

https://github.com/damienbod/AspNetCoreAzureADCAE/blob/main/CaeAdministrationTool/CAE/CAEAdminServices.cs

https://github.com/damienbod/AspNetCoreAzureADCAE/blob/main/CaeAdministrationTool/CAE/GraphAuthContextAdmin.cs

![CA with Auth Context and access token](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/cae/azuread_cae_01.png)

## Setup Continuous access policy

![CA with Auth Context and access token](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/cae/azuread_cae_03.png)

## Setup Continuous Azure App registrations

### id token xms_cc

![CA with Auth Context and xms_cc](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/cae/id_token_xms_cc.png)

### access token xms_cc

![CA with Auth Context and xms_cc](https://github.com/damienbod/aspnetcore-standup-securing-apis/blob/main/details/cae/access_token_xms_cc.png)

## Use in applications

access token:

https://github.com/damienbod/AspNetCoreAzureADCAE/blob/main/CaeApi/CaeApi/CAE/CaeClaimsChallengeService.cs

id token:

https://github.com/damienbod/AspNetCoreAzureADCAE/blob/main/RazorCaeStandalone/Cae/CaeClaimsChallengeService.cs

# Links

https://github.com/damienbod/AspNetCoreAzureADCAE

https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/concept-continuous-access-evaluation

https://cloudbrothers.info/continuous-access-evaluation/
