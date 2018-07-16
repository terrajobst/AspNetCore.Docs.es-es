<span data-ttu-id="275e7-101">Cuando la aplicación requiera varios proveedores, encadene los métodos de extensión del proveedor detrás de [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span><span class="sxs-lookup"><span data-stu-id="275e7-101">When the app requires multiple providers, chain the provider extension methods behind [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span></span>

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
