# Play.Infra
Play Economy Play.Infra microservice.

## Add the GitHub package source
```powershell
$owner="jeevdotnetmicroservice"
$gh_pat="[PAT HERE]"

dotnet nuget add source --username jeev --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```

## Creating the Azure resource group
```powershell
$appname="playeconomy"
az group create --name $appname --location eastus
```

## Creating the Cosmos DB account
```powershell
$resourceName="jeevplayeconomy"
az cosmosdb create --name $resourceName --resource-group $appname --kind MongoDB --enable-free-tier
```

## Creating the Service Bus namespace
```powershell
az servicebus namespace create --name $resourceName --resource-group $appname --sku Standard
```