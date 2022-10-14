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
$cosmosDbName="jeevplayeconomy"
az cosmosdb create --name $cosmosDbName --resource-group $appname --kind MongoDB --enable-free-tier
```