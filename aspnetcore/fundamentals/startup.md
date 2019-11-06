---
title: Uruchamianie aplikacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak Klasa startowa w ASP.NET Core konfiguruje usługi i potok żądań aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/02/2019
uid: fundamentals/startup
ms.openlocfilehash: 081eaa772d136477a37a3392877886327e0cda7c
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634034"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="0628f-103">Uruchamianie aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0628f-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="0628f-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex)i [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="0628f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="0628f-105">Klasa `Startup` konfiguruje usługi i potok żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="0628f-106">Klasa Startup</span><span class="sxs-lookup"><span data-stu-id="0628f-106">The Startup class</span></span>

<span data-ttu-id="0628f-107">Aplikacje ASP.NET Core używają klasy `Startup`, która nosi nazwę `Startup` według Konwencji.</span><span class="sxs-lookup"><span data-stu-id="0628f-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="0628f-108">Klasa `Startup`:</span><span class="sxs-lookup"><span data-stu-id="0628f-108">The `Startup` class:</span></span>

* <span data-ttu-id="0628f-109">Opcjonalnie zawiera <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> metodę konfigurowania *usług*aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="0628f-110">Usługa to składnik wielokrotnego użytku, który zapewnia funkcjonalność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="0628f-111">Usługi są *rejestrowane* w `ConfigureServices` i używane w aplikacji przez [iniekcję zależności (DI)](xref:fundamentals/dependency-injection) lub <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span><span class="sxs-lookup"><span data-stu-id="0628f-111">Services are *registered* in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="0628f-112">Zawiera <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> metodę tworzenia potoku przetwarzania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="0628f-113">`ConfigureServices` i `Configure` są wywoływane przez środowisko uruchomieniowe środowiska ASP.NET Core podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0628f-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="0628f-114">Powyższy przykład jest przeznaczony dla [Razor Pages](xref:razor-pages/index); wersja MVC jest podobna.</span><span class="sxs-lookup"><span data-stu-id="0628f-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

<span data-ttu-id="0628f-115">Klasa `Startup` jest określana podczas kompilowania [hosta](xref:fundamentals/index#host) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="0628f-116">Klasa `Startup` jest zazwyczaj określana przez wywołanie metody [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) na konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="0628f-116">The `Startup` class is typically specified by calling the [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="0628f-117">Host udostępnia usługi, które są dostępne dla konstruktora klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0628f-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="0628f-118">Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0628f-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="0628f-119">Usługa Host i aplikacje są dostępne w `Configure` i w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="0628f-120">Tylko następujące typy usług można wstrzyknąć do konstruktora `Startup`, gdy jest używany [host generyczny](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span><span class="sxs-lookup"><span data-stu-id="0628f-120">Only the following service types can be injected into the `Startup` constructor when using the [Generic Host](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="0628f-121">Większość usług nie jest dostępna do momentu wywołania metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0628f-121">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0628f-122">Host udostępnia usługi, które są dostępne dla konstruktora klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0628f-122">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="0628f-123">Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0628f-123">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="0628f-124">Usługa Host i aplikacje są następnie dostępne w `Configure` i w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-124">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="0628f-125">Typowym zastosowaniem [iniekcji zależności](xref:fundamentals/dependency-injection) do klasy `Startup` jest wstrzyknięcie:</span><span class="sxs-lookup"><span data-stu-id="0628f-125">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="0628f-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> skonfigurować usługi według środowiska.</span><span class="sxs-lookup"><span data-stu-id="0628f-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="0628f-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> odczytać konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0628f-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="0628f-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> utworzyć rejestrator w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0628f-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="0628f-129">Większość usług nie jest dostępna do momentu wywołania metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0628f-129">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

### <a name="multiple-startup"></a><span data-ttu-id="0628f-130">Wielokrotne uruchomienie</span><span class="sxs-lookup"><span data-stu-id="0628f-130">Multiple Startup</span></span>

<span data-ttu-id="0628f-131">Gdy aplikacja definiuje oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`), odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0628f-131">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="0628f-132">Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet.</span><span class="sxs-lookup"><span data-stu-id="0628f-132">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="0628f-133">Jeśli aplikacja jest uruchamiana w środowisku deweloperskim i zawiera zarówno klasę `Startup`, jak i klasę `StartupDevelopment`, używana jest Klasa `StartupDevelopment`.</span><span class="sxs-lookup"><span data-stu-id="0628f-133">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="0628f-134">Aby uzyskać więcej informacji, zobacz [Korzystanie z wielu środowisk](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="0628f-134">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="0628f-135">Aby uzyskać więcej informacji na temat hosta, zobacz [hosta](xref:fundamentals/index#host) .</span><span class="sxs-lookup"><span data-stu-id="0628f-135">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="0628f-136">Informacje dotyczące obsługi błędów podczas uruchamiania można znaleźć w temacie [Obsługa wyjątków uruchamiania](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="0628f-136">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="0628f-137">Metoda ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="0628f-137">The ConfigureServices method</span></span>

<span data-ttu-id="0628f-138">Metoda <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> jest:</span><span class="sxs-lookup"><span data-stu-id="0628f-138">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="0628f-139">Opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="0628f-139">Optional.</span></span>
* <span data-ttu-id="0628f-140">Wywoływane przez hosta przed metodą `Configure`, aby skonfigurować usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-140">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="0628f-141">Gdzie są ustawiane [Opcje konfiguracji](xref:fundamentals/configuration/index) zgodnie z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="0628f-141">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="0628f-142">Host może skonfigurować niektóre usługi przed wywołaniem metod `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0628f-142">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="0628f-143">Aby uzyskać więcej informacji, zobacz [hosta](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="0628f-143">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="0628f-144">W przypadku funkcji wymagających istotnej konfiguracji w <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>istnieją `Add{Service}` metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="0628f-144">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="0628f-145">Na przykład **Dodaj**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores i **Add**RazorPages:</span><span class="sxs-lookup"><span data-stu-id="0628f-145">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

<span data-ttu-id="0628f-146">Dodanie usług do kontenera usług sprawia, że są one dostępne w aplikacji i w metodzie `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0628f-146">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="0628f-147">Usługi są rozwiązywane za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection) lub z <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span><span class="sxs-lookup"><span data-stu-id="0628f-147">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0628f-148">Zobacz [SetCompatibilityVersion](xref:mvc/compatibility-version) , aby uzyskać więcej informacji na temat `SetCompatibilityVersion`.</span><span class="sxs-lookup"><span data-stu-id="0628f-148">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

::: moniker-end

## <a name="the-configure-method"></a><span data-ttu-id="0628f-149">Metoda Configure</span><span class="sxs-lookup"><span data-stu-id="0628f-149">The Configure method</span></span>

<span data-ttu-id="0628f-150">Metoda <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> służy do określania, w jaki sposób aplikacja reaguje na żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="0628f-150">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="0628f-151">Potok żądań jest konfigurowany przez dodanie składników [pośredniczących](xref:fundamentals/middleware/index) do wystąpienia <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0628f-151">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="0628f-152">`IApplicationBuilder` jest dostępna dla metody `Configure`, ale nie jest ona zarejestrowana w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="0628f-152">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="0628f-153">Hosting tworzy `IApplicationBuilder` i przekazuje go bezpośrednio do `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0628f-153">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="0628f-154">[Szablony ASP.NET Core](/dotnet/core/tools/dotnet-new) konfigurują potok z obsługą:</span><span class="sxs-lookup"><span data-stu-id="0628f-154">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="0628f-155">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="0628f-155">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="0628f-156">Procedura obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="0628f-156">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="0628f-157">Zabezpieczenia protokołu HTTP Strict Transport (HSTS)</span><span class="sxs-lookup"><span data-stu-id="0628f-157">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="0628f-158">Przekierowanie HTTPS</span><span class="sxs-lookup"><span data-stu-id="0628f-158">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="0628f-159">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="0628f-159">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="0628f-160">ASP.NET Core [MVC](xref:mvc/overview) i [Razor Pages](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="0628f-160">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

::: moniker range="< aspnetcore-3.0"

* [<span data-ttu-id="0628f-161">Ogólne rozporządzenie o ochronie danych (Rodo)</span><span class="sxs-lookup"><span data-stu-id="0628f-161">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="0628f-162">Powyższy przykład jest przeznaczony dla [Razor Pages](xref:razor-pages/index); wersja MVC jest podobna.</span><span class="sxs-lookup"><span data-stu-id="0628f-162">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

<span data-ttu-id="0628f-163">Każda metoda rozszerzenia `Use` dodaje jeden lub więcej składników pośredniczących do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="0628f-163">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="0628f-164">Na przykład <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> konfiguruje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) do obsłużynia [plików statycznych](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="0628f-164">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="0628f-165">Każdy składnik pośredniczący w potoku żądania jest odpowiedzialny za wywoływanie następnego składnika w potoku lub krótkiego obwodu łańcucha, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="0628f-165">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0628f-166">Dodatkowe usługi, takie jak `IWebHostEnvironment`, `ILoggerFactory`lub dowolne elementy zdefiniowane w `ConfigureServices`, można określić w podpisie metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0628f-166">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="0628f-167">Te usługi są wstrzykiwane, jeśli są dostępne.</span><span class="sxs-lookup"><span data-stu-id="0628f-167">These services are injected if they're available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0628f-168">Dodatkowe usługi, takie jak `IHostingEnvironment` i `ILoggerFactory`lub dowolne elementy zdefiniowane w `ConfigureServices`, można określić w podpisie metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0628f-168">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="0628f-169">Te usługi są wstrzykiwane, jeśli są dostępne.</span><span class="sxs-lookup"><span data-stu-id="0628f-169">These services are injected if they're available.</span></span>

::: moniker-end

<span data-ttu-id="0628f-170">Aby uzyskać więcej informacji na temat używania `IApplicationBuilder` i kolejności przetwarzania oprogramowania pośredniczącego, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="0628f-170">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="0628f-171">Konfigurowanie usług bez uruchamiania</span><span class="sxs-lookup"><span data-stu-id="0628f-171">Configure services without Startup</span></span>

<span data-ttu-id="0628f-172">Aby skonfigurować usługi i potok przetwarzania żądań bez użycia klasy `Startup`, wywołaj `ConfigureServices` i `Configure` wygodne metody w konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="0628f-172">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="0628f-173">Wiele wywołań `ConfigureServices` dołączyć do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="0628f-173">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="0628f-174">Jeśli istnieje wiele wywołań metod `Configure`, używane jest ostatnie wywołanie `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0628f-174">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="0628f-175">Zwiększanie uruchamiania przy użyciu filtrów uruchamiania</span><span class="sxs-lookup"><span data-stu-id="0628f-175">Extend Startup with startup filters</span></span>

<span data-ttu-id="0628f-176">Użyj <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span><span class="sxs-lookup"><span data-stu-id="0628f-176">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span></span>

* <span data-ttu-id="0628f-177">Aby skonfigurować oprogramowanie pośredniczące na początku lub na końcu aplikacji [Skonfiguruj](#the-configure-method) potok oprogramowania pośredniczącego bez jawnego wywołania do `Use{Middleware}`.</span><span class="sxs-lookup"><span data-stu-id="0628f-177">To configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline without an explicit call to `Use{Middleware}`.</span></span> <span data-ttu-id="0628f-178">`IStartupFilter` jest używany przez ASP.NET Core do dodawania wartości domyślnych na początku potoku bez konieczności jawnego rejestrowania przez autora aplikacji domyślnego oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="0628f-178">`IStartupFilter` is used by ASP.NET Core to add defaults to the beginning of the pipeline without having to make the app author explicitly register the default middleware.</span></span> <span data-ttu-id="0628f-179">`IStartupFilter` umożliwia innym wywołaniem składnika `Use{Middleware}` w imieniu autora aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-179">`IStartupFilter` allows a different component call `Use{Middleware}` on behalf of the app author.</span></span>
* <span data-ttu-id="0628f-180">Tworzenie potoku metod `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0628f-180">To create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="0628f-181">[IStartupFilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) może ustawić oprogramowanie pośredniczące do uruchomienia przed lub po oprogramowaniu pośredniczącym dodanym przez biblioteki.</span><span class="sxs-lookup"><span data-stu-id="0628f-181">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="0628f-182">`IStartupFilter` implementuje <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, który odbiera i zwraca `Action<IApplicationBuilder>`.</span><span class="sxs-lookup"><span data-stu-id="0628f-182">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="0628f-183"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> definiuje klasę, aby skonfigurować potok żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-183">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="0628f-184">Aby uzyskać więcej informacji, zobacz [Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="0628f-184">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="0628f-185">Każdy `IStartupFilter` może dodać jeden lub więcej middlewares w potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="0628f-185">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="0628f-186">Filtry są wywoływane w kolejności, w jakiej zostały dodane do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="0628f-186">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="0628f-187">Filtry mogą dodać oprogramowanie pośredniczące przed lub po przekazaniu kontroli do następnego filtru, w związku z czym dołączają do początku lub na końcu potoku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-187">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="0628f-188">Poniższy przykład pokazuje, jak zarejestrować oprogramowanie pośredniczące w `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="0628f-188">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="0628f-189">Oprogramowanie pośredniczące `RequestSetOptionsMiddleware` ustawia wartość opcji z parametru ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="0628f-189">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="0628f-190">`RequestSetOptionsMiddleware` jest konfigurowana w klasie `RequestSetOptionsStartupFilter`:</span><span class="sxs-lookup"><span data-stu-id="0628f-190">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="0628f-191">`RequestSetOptionsMiddleware` jest konfigurowana w klasie `RequestSetOptionsStartupFilter`:</span><span class="sxs-lookup"><span data-stu-id="0628f-191">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

<span data-ttu-id="0628f-192">`IStartupFilter` jest zarejestrowany w kontenerze usługi w programie <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span><span class="sxs-lookup"><span data-stu-id="0628f-192">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

<span data-ttu-id="0628f-193">Gdy podano parametr ciągu zapytania dla `option`, oprogramowanie pośredniczące przetwarza przypisanie wartości, zanim oprogramowanie pośredniczące ASP.NET Core renderuje odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="0628f-193">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="0628f-194">Kolejność wykonywania oprogramowania pośredniczącego jest ustawiana w kolejności `IStartupFilter` rejestracji:</span><span class="sxs-lookup"><span data-stu-id="0628f-194">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="0628f-195">Wiele implementacji `IStartupFilter` może współistnieć z tymi samymi obiektami.</span><span class="sxs-lookup"><span data-stu-id="0628f-195">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="0628f-196">Jeśli kolejność jest ważna, określ kolejność `IStartupFilter` rejestracji usługi, aby odpowiadały kolejności, w jakiej middlewares.</span><span class="sxs-lookup"><span data-stu-id="0628f-196">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="0628f-197">Biblioteki mogą dodawać oprogramowanie pośredniczące z co najmniej jedną `IStartupFilter` implementacjami, które są uruchamiane przed lub po innym oprogramowaniu pośredniczącym aplikacji zarejestrowanym w usłudze `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="0628f-197">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="0628f-198">Aby wywoływać `IStartupFilter` oprogramowanie pośredniczące przed dodaniem oprogramowania pośredniczącego przez `IStartupFilter`biblioteki:</span><span class="sxs-lookup"><span data-stu-id="0628f-198">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="0628f-199">Umieść rejestrację usługi przed dodaniem biblioteki do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="0628f-199">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="0628f-200">Aby później wywołać, należy ustawić rejestrację usługi po dodaniu biblioteki.</span><span class="sxs-lookup"><span data-stu-id="0628f-200">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="0628f-201">Dodaj konfigurację podczas uruchamiania z zestawu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="0628f-201">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="0628f-202">Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0628f-202">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="0628f-203">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0628f-203">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0628f-204">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0628f-204">Additional resources</span></span>

* [<span data-ttu-id="0628f-205">Host</span><span class="sxs-lookup"><span data-stu-id="0628f-205">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
