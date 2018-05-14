# <a name="aspnet-core-web-api-controller-sample"></a>Przykładowe Kontroler interfejsu API sieci Web platformy ASP.NET Core

Ta przykładowa aplikacja składa się z następujących projektów:

- **WebApiSample.Api**: projekt platformy ASP.NET Core 2.1 przeznaczony dla platformy .NET Core 2.1.
- **WebApiSample.Api.Pre21**: projekt programu ASP.NET 2.0 Core przeznaczonych dla platformy .NET Core 2.0.
- **WebApiSample.DataAccess**: A .NET 2.0 standardowej biblioteki klas służy jako warstwy dostępu do danych dla projektów sieci Web API 2.

W tym przykładzie przedstawiono różnice tworzenia kontrolera interfejsu API sieci Web:

- [Klasa wyprowadzona z ControllerBase](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#derive-class-from-controllerbase)
- [Dodawanie adnotacji z ApiControllerAttribute — klasa](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#annotate-class-with-apicontrollerattribute)