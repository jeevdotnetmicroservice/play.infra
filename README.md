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
$rgname="playeconomy"
az group create --name $rgname --location eastus
```

## Creating the Cosmos DB account
```powershell
$rpname="jeevplayeconomy"
az cosmosdb create --name $rpname --resource-group $rgname --kind MongoDB --enable-free-tier
```

## Creating the Service Bus namespace
```powershell
az servicebus namespace create --name $rpname --resource-group $rgname --sku Standard
```

## Creating the Container Registry
```powershell
az acr create --name $rpname --resource-group $rgname --sku Basic
```