---
title: Wstrzykiwanie zależności do kontrolerów w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak kontrolerów platformy ASP.NET Core MVC zażądać ich zależności, które jawnie za pomocą ich konstruktory mogę przy użyciu iniekcji zależności w programie ASP.NET Core.
ms.author: riande
ms.date: 02/24/2019
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 6b08c321f4cae1f4efd8ea40300eaf4dfc2f63a1
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903070"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="98b53-103">Wstrzykiwanie zależności do kontrolerów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98b53-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="98b53-104">Przez [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Steve Smith](https://github.com/ardalis)</span><span class="sxs-lookup"><span data-stu-id="98b53-104">By [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://github.com/ardalis)</span></span>

<span data-ttu-id="98b53-105">Kontrolerów MVC platformy ASP.NET Core żądania zależności jawnie za pomocą konstruktorów.</span><span class="sxs-lookup"><span data-stu-id="98b53-105">ASP.NET Core MVC controllers request dependencies explicitly via constructors.</span></span> <span data-ttu-id="98b53-106">Platforma ASP.NET Core ma wbudowaną obsługę [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="98b53-106">ASP.NET Core has built-in support for [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="98b53-107">DI ułatwia aplikacji do testowania i obsługi.</span><span class="sxs-lookup"><span data-stu-id="98b53-107">DI makes apps easier to test and maintain.</span></span>

<span data-ttu-id="98b53-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98b53-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="98b53-109">Iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="98b53-109">Constructor Injection</span></span>

<span data-ttu-id="98b53-110">Usługi są dodawane jako parametr konstruktora i środowiska uruchomieniowego jest rozpoznawana jako usługa z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="98b53-110">Services are added as a constructor parameter, and the runtime resolves the service from the service container.</span></span> <span data-ttu-id="98b53-111">Usługi są zazwyczaj zdefiniowane przy użyciu interfejsów.</span><span class="sxs-lookup"><span data-stu-id="98b53-111">Services are typically defined using interfaces.</span></span> <span data-ttu-id="98b53-112">Rozważmy na przykład aplikacji, która wymaga bieżący czas.</span><span class="sxs-lookup"><span data-stu-id="98b53-112">For example, consider an app that requires the current time.</span></span> <span data-ttu-id="98b53-113">Następujące interfejsu ujawnia `IDateTime` usługi:</span><span class="sxs-lookup"><span data-stu-id="98b53-113">The following interface exposes the `IDateTime` service:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Interfaces/IDateTime.cs?name=snippet)]

<span data-ttu-id="98b53-114">Poniższy kod implementuje `IDateTime` interfejsu:</span><span class="sxs-lookup"><span data-stu-id="98b53-114">The following code implements the `IDateTime` interface:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Services/SystemDateTime.cs?name=snippet)]

<span data-ttu-id="98b53-115">Dodaj usługę do kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="98b53-115">Add the service to the service container:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup1.cs?name=snippet&highlight=3)]

<span data-ttu-id="98b53-116">Aby uzyskać więcej informacji na temat <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, zobacz [okresy istnienia usługi DI](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="98b53-116">For more information on <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, see [DI service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="98b53-117">Poniższy kod wyświetla pozdrowienia dla użytkownika na podstawie czasu dnia:</span><span class="sxs-lookup"><span data-stu-id="98b53-117">The following code displays a greeting to the user based on the time of day:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="98b53-118">Uruchom aplikację i zostanie wyświetlony komunikat na podstawie czasu.</span><span class="sxs-lookup"><span data-stu-id="98b53-118">Run the app and a message is displayed based on the time.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="98b53-119">Akcja iniekcja FromServices</span><span class="sxs-lookup"><span data-stu-id="98b53-119">Action injection with FromServices</span></span>

<span data-ttu-id="98b53-120"><xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> Umożliwia wstawianie usługi bezpośrednio do metody akcji bez przy użyciu iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="98b53-120">The <xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> enables injecting a service directly into an action method without using constructor injection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet2)]

## <a name="access-settings-from-a-controller"></a><span data-ttu-id="98b53-121">Ustawienia dostępu za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="98b53-121">Access settings from a controller</span></span>

<span data-ttu-id="98b53-122">Uzyskiwanie dostępu do aplikacji lub konfiguracji ustawień z poziomu kontrolera jest typowy wzorzec.</span><span class="sxs-lookup"><span data-stu-id="98b53-122">Accessing app or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="98b53-123">*Wzorzec opcje* opisanego w <xref:fundamentals/configuration/options> jest preferowanym podejściem do zarządzania ustawieniami.</span><span class="sxs-lookup"><span data-stu-id="98b53-123">The *options pattern* described in <xref:fundamentals/configuration/options> is the preferred approach to manage settings.</span></span> <span data-ttu-id="98b53-124">Ogólnie rzecz biorąc, bezpośrednio nie wstrzyknąć <xref:Microsoft.Extensions.Configuration.IConfiguration> w kontroler.</span><span class="sxs-lookup"><span data-stu-id="98b53-124">Generally, don't directly inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into a controller.</span></span>

<span data-ttu-id="98b53-125">Utwórz klasę, która reprezentuje opcje.</span><span class="sxs-lookup"><span data-stu-id="98b53-125">Create a class that represents the options.</span></span> <span data-ttu-id="98b53-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="98b53-126">For example:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Models/SampleWebSettings.cs?name=snippet)]

<span data-ttu-id="98b53-127">Dodaj klasę konfiguracji do kolekcji usługi:</span><span class="sxs-lookup"><span data-stu-id="98b53-127">Add the configuration class to the services collection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup.cs?highlight=4&name=snippet1)]

<span data-ttu-id="98b53-128">Konfigurowanie aplikacji do odczytywania ustawień z pliku w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="98b53-128">Configure the app to read the settings from a JSON-formatted file:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Program.cs?name=snippet&range=10-15)]

<span data-ttu-id="98b53-129">Poniższy kod żądania `IOptions<SampleWebSettings>` ustawienia z kontenera usługi i używa ich w `Index` metody:</span><span class="sxs-lookup"><span data-stu-id="98b53-129">The following code requests the `IOptions<SampleWebSettings>` settings from the service container and uses them in the `Index` method:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/SettingsController.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="98b53-130">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="98b53-130">Additional resources</span></span>

* <span data-ttu-id="98b53-131">Zobacz <xref:mvc/controllers/testing> dowiesz się, jak ułatwiają kodu do przetestowania przez jawne żądanie zależności w kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="98b53-131">See <xref:mvc/controllers/testing> to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

* <span data-ttu-id="98b53-132">[Zamień domyślny kontener iniekcji zależności implementacji firm](xref:fundamentals/dependency-injection#default-service-container-replacement).</span><span class="sxs-lookup"><span data-stu-id="98b53-132">[Replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement).</span></span>
