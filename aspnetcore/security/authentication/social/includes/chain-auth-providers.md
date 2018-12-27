## <a name="multiple-authentication-providers"></a><span data-ttu-id="838b9-101">Wielu dostawców uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="838b9-101">Multiple authentication providers</span></span>

<span data-ttu-id="838b9-102">Jeśli aplikacja wymaga wielu dostawców, łańcucha metody rozszerzenia dostawcy za [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span><span class="sxs-lookup"><span data-stu-id="838b9-102">When the app requires multiple providers, chain the provider extension methods behind [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span></span>

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
