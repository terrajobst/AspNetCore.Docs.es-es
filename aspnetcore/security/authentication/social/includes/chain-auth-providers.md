## <a name="multiple-authentication-providers"></a><span data-ttu-id="ca4ae-101">Varios proveedores de autenticación</span><span class="sxs-lookup"><span data-stu-id="ca4ae-101">Multiple authentication providers</span></span>

<span data-ttu-id="ca4ae-102">Cuando la aplicación requiera varios proveedores, encadene los métodos de extensión del proveedor detrás de [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span><span class="sxs-lookup"><span data-stu-id="ca4ae-102">When the app requires multiple providers, chain the provider extension methods behind [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span></span>

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
