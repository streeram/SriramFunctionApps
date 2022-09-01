Reference
https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-bicep?tabs=CLI
https://dev.to/kenakamu/debug-net-5-function-with-visual-studio-visual-studio-code-5235
https://github.com/Azure/azure-functions-dotnet-worker

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
