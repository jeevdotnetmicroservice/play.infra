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

## Creating the AKS Registry
```powershell
az feature register --name EnablePodIdentityPreview --namespace Microsoft.ContainerService
az extension add --name aks-preview

az aks create -n $rpname -g $rgname --node-vm-size Standard_B2s --node-count 2 --attach-acr $rpname --enable-pod-identity --network-plugin azure

az aks get-credentials --name $rpname --resource-group $rgname
```

## Creating the Azure Key Vault
```powershell
az keyvault create -n $rpname -g $rgname
```

## Installing Emissary-ingress
```powershell
helm repo add datawire https://app.getambassador.io
helm repo update

kubectl apply -f https://app.getambassador.io/yaml/emissary/3.2.0/emissary-crds.yaml
kubectl wait --timeout=90s --for=condition=available deployment emissary-apiext -n emissary-system

$namespace="emissary"
helm install emissary-ingress datawire/emissary-ingress --set service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"=$rpname -n $namespace --create-namespace

kubectl rollout status deployment/emissary-ingress -n $namespace  -w
```

## Configuring Emissary-ingress routing
```powershell
kubectl apply -f .\emissary-ingress\listener.yaml -n $namespace
kubectl apply -f .\emissary-ingress\mappings.yaml -n $namespace
```

## Installing cert-manager
```powershell
helm repo add jetstack https://charts.jetstack.io
helm repo update

helm install cert-manager jetstack/cert-manager --version v1.10.0 --set installCRDs=true --namespace $namespace

```