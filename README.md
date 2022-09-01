Reference
https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-bicep?tabs=CLI
https://dev.to/kenakamu/debug-net-5-function-with-visual-studio-visual-studio-code-5235
https://github.com/Azure/azure-functions-dotnet-worker
https://github.com/Azure/open-service-broker-azure/issues/540
https://docs.microsoft.com/en-us/learn/modules/build-first-bicep-deployment-pipeline-using-github-actions/

```
az group create --name exampleRG --location eastus
az deployment group create --resource-group exampleRG --template-file main.bicep --parameters appInsightsLocation=<app-location>
```

Install Azure Functions Core Tools using https://github.com/Azure/azure-functions-core-tools

brew tap azure/functions
brew install azure-functions-core-tools@4

Then create new Functions App project

### Create a .NET Isolated project
In an empty directory, run `func init` and select `dotnet (Isolated Process)`.

### Add a function
Run `func new` and select any trigger (`HttpTrigger` is a good one to start). Fill in the function name.

### Run functions locally
Run `func host start` in the sample app directory.


### Debug
Run `func start --dotnet-isolated-debug` and attach.


To push to Azure using GitHub actions and Bicep scripts

create workflow.yml

Before we can run Bicep scripts via GitHub actions, we must authorize GitHub. Create Azure AD identity and grant permissions to GitHub

$githubOrganizationName = ‘streeram’
$githubRepositoryName = ‘Sriram’FunctionApps’

$applicationRegistration = New-AzADApplication -DisplayName ‘SriramFunc'

New-AzADAppFederatedCredential `
   -Name 'SriramFunc-workflow' `
   -ApplicationObjectId $applicationRegistration.Id `
   -Issuer 'https://token.actions.githubusercontent.com' `
   -Audience 'api://AzureADTokenExchange' `
   -Subject "repo:$($githubOrganizationName)/$($githubRepositoryName):ref:refs/heads/main"




$resourceGroup = New-AzResourceGroup -Name SriramFunc -Location australiasoutheast

New-AzADServicePrincipal -AppId $applicationRegistration.AppId
New-AzRoleAssignment `
   -ApplicationId $($applicationRegistration.AppId) `
   -RoleDefinitionName Contributor `
   -Scope $resourceGroup.ResourceId
