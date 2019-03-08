---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665512"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="e4dd3-101">Przykładowe Kontroler interfejsu API sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4dd3-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="e4dd3-102">Ta przykładowa aplikacja składa się z następujących projektów:</span><span class="sxs-lookup"><span data-stu-id="e4dd3-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="e4dd3-103">**WebApiSample.Api.22**: Projekt platformy ASP.NET Core 2.2, przeznaczonych dla platformy .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="e4dd3-103">**WebApiSample.Api.22**: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="e4dd3-104">**WebApiSample.Api.21**: Projekt platformy ASP.NET Core 2.1, przeznaczonych dla platformy .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e4dd3-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="e4dd3-105">**WebApiSample.Api.Pre21**: Projekt platformy ASP.NET Core 2.0, przeznaczonych dla platformy .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e4dd3-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="e4dd3-106">**WebApiSample.DataAccess**: Biblioteka klas .NET Standard 2.0 służy jako warstwa dostępu do danych 2 projektów interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e4dd3-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="e4dd3-107">Ten przykład ilustruje różnice między tworzenia kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="e4dd3-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="e4dd3-108">Dziedziczyć klasy ControllerBase</span><span class="sxs-lookup"><span data-stu-id="e4dd3-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="e4dd3-109">Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="e4dd3-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
