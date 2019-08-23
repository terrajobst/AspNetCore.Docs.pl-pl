---
title: Uruchamianie aplikacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak Klasa startowa w ASP.NET Core konfiguruje usługi i potok żądań aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 8/7/2019
uid: fundamentals/startup
ms.openlocfilehash: 8866ee9210a91754d8050d0b91ff52c3d3fe0836
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975437"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="9d50d-103">Uruchamianie aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d50d-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="9d50d-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex)i [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="9d50d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="9d50d-105">`Startup` Klasa konfiguruje usługi i potok żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="9d50d-106">Klasa Startup</span><span class="sxs-lookup"><span data-stu-id="9d50d-106">The Startup class</span></span>

<span data-ttu-id="9d50d-107">Aplikacje ASP.NET Core używają `Startup` klasy, która jest nazywana `Startup` Konwencją.</span><span class="sxs-lookup"><span data-stu-id="9d50d-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="9d50d-108">`Startup` Klasa:</span><span class="sxs-lookup"><span data-stu-id="9d50d-108">The `Startup` class:</span></span>

* <span data-ttu-id="9d50d-109">Opcjonalnie zawiera <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> metodę konfigurowania *usług*aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="9d50d-110">Usługa to składnik wielokrotnego użytku, który zapewnia funkcjonalność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="9d50d-111">Usługi są konfigurowane&mdash;również jako *zarejestrowane*&mdash; `ConfigureServices` w aplikacji i używane przez [iniekcję zależności (di)](xref:fundamentals/dependency-injection) lub. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*></span><span class="sxs-lookup"><span data-stu-id="9d50d-111">Services are configured&mdash;also described as *registered*&mdash;in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="9d50d-112"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Zawiera metodę tworzenia potoku przetwarzania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="9d50d-113">`ConfigureServices`i `Configure` są wywoływane przez środowisko uruchomieniowe ASP.NET Core podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="9d50d-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="9d50d-114">Powyższy przykład jest przeznaczony dla [Razor Pages](xref:razor-pages/index); wersja MVC jest podobna.</span><span class="sxs-lookup"><span data-stu-id="9d50d-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

<span data-ttu-id="9d50d-115">Klasa jest określana podczas kompilowania hosta aplikacji. [](xref:fundamentals/index#host) `Startup`</span><span class="sxs-lookup"><span data-stu-id="9d50d-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="9d50d-116">Klasa jest zwykle określona przez [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) wywołanie metody w konstruktorze hosta: `Startup`</span><span class="sxs-lookup"><span data-stu-id="9d50d-116">The `Startup` class is typically specified by calling the [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="9d50d-117">Host udostępnia usługi, które są dostępne dla `Startup` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="9d50d-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="9d50d-118">Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`programu.</span><span class="sxs-lookup"><span data-stu-id="9d50d-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="9d50d-119">Usługi hosta i aplikacji są dostępne w `Configure` systemach i w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="9d50d-120">Tylko następujące typy usług można wstrzyknąć do `Startup` konstruktora w przypadku użycia: <xref:Microsoft.Extensions.Hosting.IHostBuilder></span><span class="sxs-lookup"><span data-stu-id="9d50d-120">Only the following service types can be injected into the `Startup` constructor when using <xref:Microsoft.Extensions.Hosting.IHostBuilder>:</span></span>

* `IWebHostEnvironment`
* `IHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="9d50d-121">Większość usług nie jest dostępna do momentu `Configure` wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="9d50d-121">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d50d-122">Host udostępnia usługi, które są dostępne dla `Startup` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="9d50d-122">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="9d50d-123">Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`programu.</span><span class="sxs-lookup"><span data-stu-id="9d50d-123">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="9d50d-124">Zarówno usługi hosta, jak i aplikacje są dostępne w `Configure` systemie i w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-124">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="9d50d-125">Typowym zastosowaniem [iniekcji zależności](xref:fundamentals/dependency-injection) do `Startup` klasy jest wstrzyknięcie:</span><span class="sxs-lookup"><span data-stu-id="9d50d-125">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="9d50d-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>Aby skonfigurować usługi według środowiska.</span><span class="sxs-lookup"><span data-stu-id="9d50d-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="9d50d-127"><xref:Microsoft.Extensions.Configuration.IConfiguration>Aby odczytać konfigurację.</span><span class="sxs-lookup"><span data-stu-id="9d50d-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="9d50d-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory>do utworzenia rejestratora w `Startup.ConfigureServices`programie.</span><span class="sxs-lookup"><span data-stu-id="9d50d-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

::: moniker-end
<span data-ttu-id="9d50d-129">Alternatywą dla iniekcji `IWebHostEnvironment` jest użycie podejścia opartego na konwencjach.</span><span class="sxs-lookup"><span data-stu-id="9d50d-129">An alternative to injecting `IWebHostEnvironment` is to use a conventions-based approach.</span></span>
::: moniker range=">= aspnetcore-3.0"

::: moniker-end

::: moniker range="< aspnetcore-3.0"
<span data-ttu-id="9d50d-130">Alternatywą dla iniekcji `IHostingEnvironment` jest użycie podejścia opartego na konwencjach.</span><span class="sxs-lookup"><span data-stu-id="9d50d-130">An alternative to injecting `IHostingEnvironment` is to use a conventions-based approach.</span></span>
::: moniker-end

<span data-ttu-id="9d50d-131">Gdy aplikacja definiuje oddzielne `Startup` klasy dla różnych środowisk (na `StartupDevelopment`przykład), odpowiednia `Startup` Klasa jest wybierana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="9d50d-131">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="9d50d-132">Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet.</span><span class="sxs-lookup"><span data-stu-id="9d50d-132">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="9d50d-133">Jeśli aplikacja jest uruchamiana w środowisku deweloperskim i zawiera `Startup` klasę `StartupDevelopment` i klasę, `StartupDevelopment` używana jest Klasa.</span><span class="sxs-lookup"><span data-stu-id="9d50d-133">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="9d50d-134">Aby uzyskać więcej informacji, zobacz [Korzystanie z wielu środowisk](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="9d50d-134">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="9d50d-135">Aby uzyskać więcej informacji na temat hosta, zobacz [hosta](xref:fundamentals/index#host) .</span><span class="sxs-lookup"><span data-stu-id="9d50d-135">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="9d50d-136">Informacje dotyczące obsługi błędów podczas uruchamiania można znaleźć w temacie [Obsługa wyjątków uruchamiania](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="9d50d-136">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="9d50d-137">Metoda ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="9d50d-137">The ConfigureServices method</span></span>

<span data-ttu-id="9d50d-138"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> Metoda jest:</span><span class="sxs-lookup"><span data-stu-id="9d50d-138">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="9d50d-139">Opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="9d50d-139">Optional.</span></span>
* <span data-ttu-id="9d50d-140">Wywoływane przez hosta przed `Configure` metodą konfigurowania usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-140">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="9d50d-141">Gdzie są ustawiane [Opcje konfiguracji](xref:fundamentals/configuration/index) zgodnie z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="9d50d-141">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="9d50d-142">Przed `Startup` wywołaniem metod host może skonfigurować niektóre usługi.</span><span class="sxs-lookup"><span data-stu-id="9d50d-142">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="9d50d-143">Aby uzyskać więcej informacji, zobacz [hosta](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="9d50d-143">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="9d50d-144">W przypadku funkcji wymagających znaczącej konfiguracji `Add{Service}` istnieją metody rozszerzające w systemie. <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection></span><span class="sxs-lookup"><span data-stu-id="9d50d-144">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="9d50d-145">Na przykład **Dodaj**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores i **Add**RazorPages:</span><span class="sxs-lookup"><span data-stu-id="9d50d-145">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

<span data-ttu-id="9d50d-146">Dodanie usług do kontenera usług sprawia, że są one dostępne w aplikacji i w `Configure` metodzie.</span><span class="sxs-lookup"><span data-stu-id="9d50d-146">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="9d50d-147">Usługi są rozwiązywane za pośrednictwem iniekcji <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> [zależności](xref:fundamentals/dependency-injection) lub z.</span><span class="sxs-lookup"><span data-stu-id="9d50d-147">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d50d-148">Aby [](xref:mvc/compatibility-version) uzyskać więcej informacji `SetCompatibilityVersion`, zobacz SetCompatibilityVersion.</span><span class="sxs-lookup"><span data-stu-id="9d50d-148">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

::: moniker-end

## <a name="the-configure-method"></a><span data-ttu-id="9d50d-149">Metoda Configure</span><span class="sxs-lookup"><span data-stu-id="9d50d-149">The Configure method</span></span>

<span data-ttu-id="9d50d-150"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Metoda służy do określania, w jaki sposób aplikacja reaguje na żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d50d-150">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="9d50d-151">Potok żądań jest konfigurowany przez dodanie do [](xref:fundamentals/middleware/index) <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> wystąpienia składników pośredniczących.</span><span class="sxs-lookup"><span data-stu-id="9d50d-151">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="9d50d-152">`IApplicationBuilder`jest dostępny dla `Configure` metody, ale nie jest ona zarejestrowana w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="9d50d-152">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="9d50d-153">Hosting tworzy `IApplicationBuilder` i przekazuje go bezpośrednio do programu `Configure`.</span><span class="sxs-lookup"><span data-stu-id="9d50d-153">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="9d50d-154">[Szablony ASP.NET Core](/dotnet/core/tools/dotnet-new) konfigurują potok z obsługą:</span><span class="sxs-lookup"><span data-stu-id="9d50d-154">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="9d50d-155">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="9d50d-155">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="9d50d-156">Procedura obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="9d50d-156">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="9d50d-157">Zabezpieczenia protokołu HTTP Strict Transport (HSTS)</span><span class="sxs-lookup"><span data-stu-id="9d50d-157">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="9d50d-158">Przekierowanie HTTPS</span><span class="sxs-lookup"><span data-stu-id="9d50d-158">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="9d50d-159">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="9d50d-159">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="9d50d-160">ASP.NET Core [MVC](xref:mvc/overview) i [Razor Pages](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="9d50d-160">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

::: moniker range="< aspnetcore-3.0"

* [<span data-ttu-id="9d50d-161">Ogólne rozporządzenie o ochronie danych (Rodo)</span><span class="sxs-lookup"><span data-stu-id="9d50d-161">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="9d50d-162">Powyższy przykład jest przeznaczony dla [Razor Pages](xref:razor-pages/index); wersja MVC jest podobna.</span><span class="sxs-lookup"><span data-stu-id="9d50d-162">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

<span data-ttu-id="9d50d-163">Każda `Use` Metoda rozszerzenia dodaje jeden lub więcej składników pośredniczących do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="9d50d-163">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="9d50d-164">Na przykład program <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> skonfiguruje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) , aby obsługiwało [pliki statyczne](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="9d50d-164">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="9d50d-165">Każdy składnik pośredniczący w potoku żądania jest odpowiedzialny za wywoływanie następnego składnika w potoku lub krótkiego obwodu łańcucha, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="9d50d-165">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d50d-166">Dodatkowe usługi, takie jak `IWebHostEnvironment`, `ILoggerFactory`lub dowolne elementy zdefiniowane w `ConfigureServices`programie, można określić w `Configure` podpisie metody.</span><span class="sxs-lookup"><span data-stu-id="9d50d-166">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="9d50d-167">Te usługi są wstrzykiwane, jeśli są dostępne.</span><span class="sxs-lookup"><span data-stu-id="9d50d-167">These services are injected if they're available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d50d-168">Dodatkowe usługi, takie jak `IHostingEnvironment` i `ILoggerFactory`lub dowolne elementy zdefiniowane w `ConfigureServices`programie, można określić w `Configure` podpisie metody.</span><span class="sxs-lookup"><span data-stu-id="9d50d-168">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="9d50d-169">Te usługi są wstrzykiwane, jeśli są dostępne.</span><span class="sxs-lookup"><span data-stu-id="9d50d-169">These services are injected if they're available.</span></span>

::: moniker-end

<span data-ttu-id="9d50d-170">Aby uzyskać więcej informacji na temat sposobu `IApplicationBuilder` użycia i kolejności przetwarzania oprogramowania pośredniczącego, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="9d50d-170">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="9d50d-171">Konfigurowanie usług bez uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9d50d-171">Configure services without Startup</span></span>

<span data-ttu-id="9d50d-172">Aby skonfigurować usługi i potok przetwarzania żądań bez użycia `Startup` klas, wywołań `ConfigureServices` i `Configure` wygodnych metod w konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="9d50d-172">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="9d50d-173">Wiele wywołań do `ConfigureServices` dołączenia do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="9d50d-173">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="9d50d-174">Jeśli istnieje `Configure` wiele wywołań metod, używane jest `Configure` ostatnie wywołanie.</span><span class="sxs-lookup"><span data-stu-id="9d50d-174">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="9d50d-175">Zwiększanie uruchamiania przy użyciu filtrów uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9d50d-175">Extend Startup with startup filters</span></span>

<span data-ttu-id="9d50d-176">Użyj <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> , aby skonfigurować oprogramowanie pośredniczące na początku lub na końcu aplikacji [Skonfiguruj](#the-configure-method) potok oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9d50d-176">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="9d50d-177">`IStartupFilter`służy do tworzenia potoku `Configure` metod.</span><span class="sxs-lookup"><span data-stu-id="9d50d-177">`IStartupFilter` is used to create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="9d50d-178">[IStartupFilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) może ustawić oprogramowanie pośredniczące do uruchomienia przed lub po oprogramowaniu pośredniczącym dodanym przez biblioteki.</span><span class="sxs-lookup"><span data-stu-id="9d50d-178">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="9d50d-179">`IStartupFilter`implementuje <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, który odbiera i `Action<IApplicationBuilder>`zwraca.</span><span class="sxs-lookup"><span data-stu-id="9d50d-179">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="9d50d-180"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> Definiuje klasę, aby skonfigurować potok żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-180">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="9d50d-181">Aby uzyskać więcej informacji, zobacz [Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="9d50d-181">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="9d50d-182">Każdy `IStartupFilter` z nich może dodać jeden lub więcej middlewares w potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="9d50d-182">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="9d50d-183">Filtry są wywoływane w kolejności, w jakiej zostały dodane do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9d50d-183">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="9d50d-184">Filtry mogą dodać oprogramowanie pośredniczące przed lub po przekazaniu kontroli do następnego filtru, w związku z czym dołączają do początku lub na końcu potoku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d50d-184">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="9d50d-185">Poniższy przykład pokazuje, jak zarejestrować oprogramowanie pośredniczące w programie `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="9d50d-185">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="9d50d-186">`RequestSetOptionsMiddleware` Oprogramowanie pośredniczące ustawia wartość opcji z parametru ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="9d50d-186">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="9d50d-187">`RequestSetOptionsMiddleware` Jest skonfigurowany`RequestSetOptionsStartupFilter` w klasie:</span><span class="sxs-lookup"><span data-stu-id="9d50d-187">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="9d50d-188">`RequestSetOptionsMiddleware` Jest skonfigurowany`RequestSetOptionsStartupFilter` w klasie:</span><span class="sxs-lookup"><span data-stu-id="9d50d-188">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

<span data-ttu-id="9d50d-189">Jest zarejestrowany w kontenerze usługi w <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>. `IStartupFilter`</span><span class="sxs-lookup"><span data-stu-id="9d50d-189">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

<span data-ttu-id="9d50d-190">Po podaniu parametru `option` ciągu zapytania, oprogramowanie pośredniczące przetwarza przypisanie wartości przed wyrenderowaniem przez ASP.NET Core oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="9d50d-190">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="9d50d-191">Kolejność wykonywania oprogramowania pośredniczącego jest określana na podstawie `IStartupFilter` kolejności rejestracji:</span><span class="sxs-lookup"><span data-stu-id="9d50d-191">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="9d50d-192">Wiele `IStartupFilter` implementacji może współistnieć z tymi samymi obiektami.</span><span class="sxs-lookup"><span data-stu-id="9d50d-192">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="9d50d-193">Jeśli kolejność jest ważna, określ kolejność `IStartupFilter` rejestracji usług w celu dopasowania ich do kolejności, w jakiej middlewares powinny działać.</span><span class="sxs-lookup"><span data-stu-id="9d50d-193">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="9d50d-194">Biblioteki mogą dodawać oprogramowanie pośredniczące z co najmniej `IStartupFilter` jedną implementacją, która jest uruchamiana przed lub po innym `IStartupFilter`oprogramowaniu pośredniczącym aplikacji zarejestrowanym w usłudze.</span><span class="sxs-lookup"><span data-stu-id="9d50d-194">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="9d50d-195">Aby wywołać `IStartupFilter` oprogramowanie pośredniczące przed dodaniem oprogramowania pośredniczącego przez `IStartupFilter`bibliotekę:</span><span class="sxs-lookup"><span data-stu-id="9d50d-195">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="9d50d-196">Umieść rejestrację usługi przed dodaniem biblioteki do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9d50d-196">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="9d50d-197">Aby później wywołać, należy ustawić rejestrację usługi po dodaniu biblioteki.</span><span class="sxs-lookup"><span data-stu-id="9d50d-197">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="9d50d-198">Dodaj konfigurację podczas uruchamiania z zestawu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="9d50d-198">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="9d50d-199">Implementacja umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zewnętrznego zestawu poza `Startup` klasą aplikacji. <xref:Microsoft.AspNetCore.Hosting.IHostingStartup></span><span class="sxs-lookup"><span data-stu-id="9d50d-199">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="9d50d-200">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="9d50d-200">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d50d-201">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9d50d-201">Additional resources</span></span>

* [<span data-ttu-id="9d50d-202">Host</span><span class="sxs-lookup"><span data-stu-id="9d50d-202">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
