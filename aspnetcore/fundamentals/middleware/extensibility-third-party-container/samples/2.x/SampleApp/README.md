# <a name="aspnet-core-middleware-extensibility-sample"></a>Przykład rozszerzeń oprogramowania pośredniczącego platformy ASP.NET Core

Ten przykład ilustruje użycie [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) i [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) przy użyciu 3rd kontenera iniekcja zależności innych firm, [proste iniektor](https://simpleinjector.org). W tym przykładzie przedstawiono funkcje opisane w [aktywacji oprogramowania pośredniczącego z kontenerem innych firm w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/extensibility-third-party-container).