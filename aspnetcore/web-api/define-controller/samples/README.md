# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="657f9-101">Przykładowe Kontroler interfejsu API sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="657f9-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="657f9-102">Ta przykładowa aplikacja składa się z następujących projektów:</span><span class="sxs-lookup"><span data-stu-id="657f9-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="657f9-103">**WebApiSample.Api**: projekt platformy ASP.NET Core 2.1 przeznaczony dla platformy .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="657f9-103">**WebApiSample.Api**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="657f9-104">**WebApiSample.Api.Pre21**: projekt platformy ASP.NET Core 2.0, przeznaczony dla platformy .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="657f9-104">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="657f9-105">**WebApiSample.DataAccess**: Biblioteka klas .NET Standard 2.0 służy jako warstwa dostępu do danych 2 projektów interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="657f9-105">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="657f9-106">Ten przykład ilustruje różnice między tworzenia kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="657f9-106">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="657f9-107">Dziedziczyć klasy ControllerBase</span><span class="sxs-lookup"><span data-stu-id="657f9-107">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="657f9-108">Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="657f9-108">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
