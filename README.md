# asp-net-with-docker-example
ASP.NET Core 6.0 WEB API in Docker with HTTPS

## Create certificate
Open PowerShell and run:
```bash
dotnet dev-certs https -ep $env:USERPROFILE\.aspnet\https\WeatherAPI.pfx -p pa55w0rd!
```
A cert is going to be generated at:
`C:\Users\%USER%\.aspnet\https`

Run command:
```
dotnet dev-certs https --trust
```

## Configure your .csproj web API 
Insert tag at node `<PropertyGroup>` -> `<UserSecretsId>{Unique-Id}</UserSecretsId>`

Save .csproj file and run below commands:
```
cd <project_root_path>
dotnet user-secrets set "Kestrel:Certificates:Development:Password" C:\Users\%USER%\AppData\Roaming\Microsoft\UserSecrets"pa55w0rd!"
```

## Creating container ON HAND
`docker container run -p 8080:80 -p 8081:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8081 -e ASPNETCORE_ENVIRONMENT=Development -v $env:APPDATA\microsoft\UserSecrets\:/root/.microsoft/usersecrets -v $env:USERPROFILE\.aspnet\https:/root/.aspnet/https/ --name api-dev weatherapi`
