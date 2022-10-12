# Play.Infra
Play Economy Play.Infra microservice.

## Add the GitHub package source
```powershell
$owner="jeevdotnetmicroservice"
$gh_pat="[PAT HERE]"

dotnet nuget add source --username jeev --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```