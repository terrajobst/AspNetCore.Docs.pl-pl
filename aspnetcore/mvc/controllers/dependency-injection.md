---
title: Wstrzykiwanie zależności do kontrolerów w ASP.NET Core
author: ardalis
description: Odkryj, jak ASP.NET Core kontrolery MVC żądają swoich zależności jawnie za pośrednictwem konstruktorów z iniekcją zależności w ASP.NET Core.
ms.author: riande
ms.date: 02/24/2019
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 202b62d4b30c5c61c407abdc8509a2a75e181cb6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660084"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="6950c-103">Wstrzykiwanie zależności do kontrolerów w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6950c-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="6950c-104">Autorzy [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT)i [Steve Smith](https://github.com/ardalis)</span><span class="sxs-lookup"><span data-stu-id="6950c-104">By [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://github.com/ardalis)</span></span>

<span data-ttu-id="6950c-105">ASP.NET Core kontrolery MVC żądają zależności jawnie za pośrednictwem konstruktorów.</span><span class="sxs-lookup"><span data-stu-id="6950c-105">ASP.NET Core MVC controllers request dependencies explicitly via constructors.</span></span> <span data-ttu-id="6950c-106">ASP.NET Core ma wbudowaną obsługę [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6950c-106">ASP.NET Core has built-in support for [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6950c-107">Dzięki temu aplikacje są łatwiejsze do testowania i konserwowania.</span><span class="sxs-lookup"><span data-stu-id="6950c-107">DI makes apps easier to test and maintain.</span></span>

<span data-ttu-id="6950c-108">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6950c-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="6950c-109">Iniekcja konstruktora</span><span class="sxs-lookup"><span data-stu-id="6950c-109">Constructor Injection</span></span>

<span data-ttu-id="6950c-110">Usługi są dodawane jako parametr konstruktora, a środowisko uruchomieniowe rozwiązuje usługę z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="6950c-110">Services are added as a constructor parameter, and the runtime resolves the service from the service container.</span></span> <span data-ttu-id="6950c-111">Usługi są zwykle definiowane przy użyciu interfejsów.</span><span class="sxs-lookup"><span data-stu-id="6950c-111">Services are typically defined using interfaces.</span></span> <span data-ttu-id="6950c-112">Rozważmy na przykład aplikację, która wymaga bieżącego czasu.</span><span class="sxs-lookup"><span data-stu-id="6950c-112">For example, consider an app that requires the current time.</span></span> <span data-ttu-id="6950c-113">Poniższy interfejs uwidacznia usługę `IDateTime`:</span><span class="sxs-lookup"><span data-stu-id="6950c-113">The following interface exposes the `IDateTime` service:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Interfaces/IDateTime.cs?name=snippet)]

<span data-ttu-id="6950c-114">Poniższy kod implementuje interfejs `IDateTime`:</span><span class="sxs-lookup"><span data-stu-id="6950c-114">The following code implements the `IDateTime` interface:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Services/SystemDateTime.cs?name=snippet)]

<span data-ttu-id="6950c-115">Dodaj usługę do kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="6950c-115">Add the service to the service container:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup1.cs?name=snippet&highlight=3)]

<span data-ttu-id="6950c-116">Aby uzyskać więcej informacji na temat <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, zobacz [di okresy istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="6950c-116">For more information on <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, see [DI service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="6950c-117">Poniższy kod wyświetla powitanie użytkownika w oparciu o godzinę:</span><span class="sxs-lookup"><span data-stu-id="6950c-117">The following code displays a greeting to the user based on the time of day:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="6950c-118">Uruchom aplikację i zostanie wyświetlony komunikat z uwzględnieniem czasu.</span><span class="sxs-lookup"><span data-stu-id="6950c-118">Run the app and a message is displayed based on the time.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="6950c-119">Iniekcja akcji przy użyciu FromServices</span><span class="sxs-lookup"><span data-stu-id="6950c-119">Action injection with FromServices</span></span>

<span data-ttu-id="6950c-120"><xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> umożliwia wstrzykiwanie usługi bezpośrednio do metody akcji bez użycia iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="6950c-120">The <xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> enables injecting a service directly into an action method without using constructor injection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet2)]

## <a name="access-settings-from-a-controller"></a><span data-ttu-id="6950c-121">Ustawienia dostępu z kontrolera</span><span class="sxs-lookup"><span data-stu-id="6950c-121">Access settings from a controller</span></span>

<span data-ttu-id="6950c-122">Dostęp do ustawień aplikacji lub konfiguracji z poziomu kontrolera jest typowym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="6950c-122">Accessing app or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="6950c-123">*Wzorzec opcji* opisany w <xref:fundamentals/configuration/options> jest preferowanym podejściem do zarządzania ustawieniami.</span><span class="sxs-lookup"><span data-stu-id="6950c-123">The *options pattern* described in <xref:fundamentals/configuration/options> is the preferred approach to manage settings.</span></span> <span data-ttu-id="6950c-124">Ogólnie rzecz biorąc nie należy bezpośrednio wstrzyknąć <xref:Microsoft.Extensions.Configuration.IConfiguration> do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6950c-124">Generally, don't directly inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into a controller.</span></span>

<span data-ttu-id="6950c-125">Utwórz klasę, która reprezentuje opcje.</span><span class="sxs-lookup"><span data-stu-id="6950c-125">Create a class that represents the options.</span></span> <span data-ttu-id="6950c-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6950c-126">For example:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Models/SampleWebSettings.cs?name=snippet)]

<span data-ttu-id="6950c-127">Dodaj klasę konfiguracji do kolekcji usług:</span><span class="sxs-lookup"><span data-stu-id="6950c-127">Add the configuration class to the services collection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup.cs?highlight=4&name=snippet1)]

<span data-ttu-id="6950c-128">Skonfiguruj aplikację do odczytywania ustawień z pliku w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="6950c-128">Configure the app to read the settings from a JSON-formatted file:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Program.cs?name=snippet&range=10-15)]

<span data-ttu-id="6950c-129">Poniższy kod żąda ustawień `IOptions<SampleWebSettings>` z kontenera usługi i używa ich w metodzie `Index`:</span><span class="sxs-lookup"><span data-stu-id="6950c-129">The following code requests the `IOptions<SampleWebSettings>` settings from the service container and uses them in the `Index` method:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/SettingsController.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="6950c-130">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6950c-130">Additional resources</span></span>

* <span data-ttu-id="6950c-131">Zobacz <xref:mvc/controllers/testing>, aby dowiedzieć się, jak ułatwić testowanie kodu przez jawne żądanie zależności w kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="6950c-131">See <xref:mvc/controllers/testing> to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

* <span data-ttu-id="6950c-132">[Zastąp domyślny kontener iniekcji zależności z implementacją innej firmy](xref:fundamentals/dependency-injection#default-service-container-replacement).</span><span class="sxs-lookup"><span data-stu-id="6950c-132">[Replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement).</span></span>
