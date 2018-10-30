# <a name="aspnet-core-web-api-controller-sample"></a>Przykładowe Kontroler interfejsu API sieci Web platformy ASP.NET Core

Ta przykładowa aplikacja składa się z następujących projektów:

- **WebApiSample.Api.22*: projekt platformy ASP.NET Core 2.2 przeznaczony dla platformy .NET Core 2.2.
- **WebApiSample.Api.21**: projekt platformy ASP.NET Core 2.1 przeznaczony dla platformy .NET Core 2.1.
- **WebApiSample.Api.Pre21**: projekt platformy ASP.NET Core 2.0, przeznaczony dla platformy .NET Core 2.0.
- **WebApiSample.DataAccess**: Biblioteka klas .NET Standard 2.0 służy jako warstwa dostępu do danych 2 projektów interfejsu API sieci Web.

Ten przykład ilustruje różnice między tworzenia kontrolera interfejsu API sieci Web:

- [Dziedziczyć klasy ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
