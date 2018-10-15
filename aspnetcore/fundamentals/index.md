---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 83dfb5707700da01c45bae3c0c00e67ca397d402
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325474"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="3a1ad-103">Podstawy platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a1ad-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="3a1ad-104">Aplikacji ASP.NET Core jest aplikacją konsoli, która tworzy serwer sieci web w jego `Main` metody:</span><span class="sxs-lookup"><span data-stu-id="3a1ad-104">An ASP.NET Core app is a console app that creates a web server in its `Main` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="3a1ad-105">`Main` Wywołuje metodę [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), zgodny [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern) w celu utworzenia hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-105">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="3a1ad-106">Konstruktor ma metody definiujące serwer sieci web (na przykład <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) i Klasa początkowa (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="3a1ad-106">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="3a1ad-107">W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web jest przydzielany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-107">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="3a1ad-108">Host sieci web platformy ASP.NET Core próbuje uruchomić na serwerze IIS, jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-108">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="3a1ad-109">Inne serwery sieci web, takich jak [HTTP.sys](xref:fundamentals/servers/httpsys), mogą być używane przez wywołanie metody rozszerzenia odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-109">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="3a1ad-110">`UseStartup` opisano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-110">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="3a1ad-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, zwracany typ `WebHost.CreateDefaultBuilder` wywołania, oferuje wiele metod opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="3a1ad-112">Oto niektóre z tych metod `UseHttpSys` do hostowania aplikacji w pliku HTTP.sys i <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> do określania zawartości katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-112">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="3a1ad-113"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> i <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> metod tworzenia <xref:Microsoft.AspNetCore.Hosting.IWebHost> obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-113">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="3a1ad-114">`Main` Metoda używa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, zgodny [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern) w celu utworzenia hosta aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-114">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="3a1ad-115">Konstruktor ma metody definiujące serwer sieci web (na przykład <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) i Klasa początkowa (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="3a1ad-115">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="3a1ad-116">W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) używany jest serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-116">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="3a1ad-117">Inne serwery sieci web, takich jak [WebListener](xref:fundamentals/servers/weblistener), mogą być używane przez wywołanie metody rozszerzenia odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-117">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="3a1ad-118">`UseStartup` opisano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-118">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="3a1ad-119">`WebHostBuilder` udostępnia wiele metod opcjonalne, w tym <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do hostowania w usługach IIS i usług IIS Express i <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> do określania zawartości katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-119">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="3a1ad-120"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> i <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> metod tworzenia <xref:Microsoft.AspNetCore.Hosting.IWebHost> obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-120">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="3a1ad-121">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="3a1ad-121">Startup</span></span>

<span data-ttu-id="3a1ad-122">`UseStartup` Metody `WebHostBuilder` Określa `Startup` klasy dla twojej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3a1ad-122">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="3a1ad-123">`Startup` Klasa jest gdzie definiuje się Potok żądań obsługi i którym skonfigurowano wszystkich usług wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-123">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="3a1ad-124">`Startup` Klasy musi być publiczna i może zawierać następujących metod:</span><span class="sxs-lookup"><span data-stu-id="3a1ad-124">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="3a1ad-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definiuje [usług](#dependency-injection-services) używanych przez aplikację (na przykład, ASP.NET Core MVC, platformy Entity Framework Core, tożsamość).</span><span class="sxs-lookup"><span data-stu-id="3a1ad-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="3a1ad-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definiuje [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) o nazwie w Potok żądań.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="3a1ad-127">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-127">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="3a1ad-128">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="3a1ad-128">Content root</span></span>

<span data-ttu-id="3a1ad-129">Główny zawartości jest ścieżka podstawowa do żadnej zawartości, używanych przez aplikację, takie jak [stron Razor](xref:razor-pages/index), MVC widoków i statyczne elementy zawartości.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-129">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="3a1ad-130">Domyślnie zawartość katalogu głównego jest tej samej lokalizacji co aplikację podstawową ścieżkę dla pliku wykonywalnego, hostowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-130">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="3a1ad-131">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="3a1ad-131">Web root</span></span>

<span data-ttu-id="3a1ad-132">Katalog główny aplikacji sieci web jest to katalog, w do projektu zawierającego zasoby publicznej, statycznej, takie jak CSS, JavaScript i plików obrazów.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-132">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="3a1ad-133">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="3a1ad-133">Dependency injection (services)</span></span>

<span data-ttu-id="3a1ad-134">A *usługi* to składnik, który jest przeznaczony do wspólnej wykorzystania w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-134">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="3a1ad-135">Usługi są udostępniane za pośrednictwem [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3a1ad-135">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3a1ad-136">Platforma ASP.NET Core obejmuje natywne kontener Inwersja kontroli (IoC), który obsługuje [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-136">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="3a1ad-137">Jeśli chcesz, możesz zastąpić domyślny kontener.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-137">You can replace the default container if you wish.</span></span> <span data-ttu-id="3a1ad-138">W uzupełnieniu do jego [luźne sprzężenia korzyści](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI sprawia, że usługi, takie jak [rejestrowania](xref:fundamentals/logging/index), która jest dostępna w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-138">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="3a1ad-139">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-139">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="3a1ad-140">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="3a1ad-140">Middleware</span></span>

<span data-ttu-id="3a1ad-141">W programie ASP.NET Core redagowania Twojego żądania potoku przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="3a1ad-141">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="3a1ad-142">Oprogramowanie pośredniczące platformy ASP.NET Core wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-142">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="3a1ad-143">Zgodnie z Konwencją składnik oprogramowania pośredniczącego o nazwie "XYZ" zostanie dodany do potoku za pomocą wywołania `UseXYZ` metody rozszerzenia w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-143">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="3a1ad-144">Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego i można napisać własne niestandardowe oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-144">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="3a1ad-145">[Otwórz interfejs sieci Web platformy .NET (OWIN)](xref:fundamentals/owin), który umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web jest obsługiwana w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-145">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="3a1ad-146">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index> i <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-146">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="3a1ad-147">Inicjowanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="3a1ad-147">Initiate HTTP requests</span></span>

<span data-ttu-id="3a1ad-148"><xref:System.Net.Http.IHttpClientFactory> jest dostępna w celu uzyskania dostępu do <xref:System.Net.Http.HttpClient> wystąpień do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-148"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="3a1ad-149">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-149">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="3a1ad-150">Środowiska</span><span class="sxs-lookup"><span data-stu-id="3a1ad-150">Environments</span></span>

<span data-ttu-id="3a1ad-151">Środowisk, takich jak *rozwoju* i *produkcji*, są najwyższej jakości pojęcie w programie ASP.NET Core i można ustawić przy użyciu zmiennej środowiskowej, plik ustawień i argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-151">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="3a1ad-152">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-152">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="3a1ad-153">Hosting</span><span class="sxs-lookup"><span data-stu-id="3a1ad-153">Hosting</span></span>

<span data-ttu-id="3a1ad-154">Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*, który jest odpowiedzialny za zarządzanie uruchamiania i okres istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-154">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="3a1ad-155">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-155">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="3a1ad-156">Serwery</span><span class="sxs-lookup"><span data-stu-id="3a1ad-156">Servers</span></span>

<span data-ttu-id="3a1ad-157">Model hostingu w programie ASP.NET Core bezpośrednio nie nasłuchuje żądań.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-157">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="3a1ad-158">Model hostingu opiera się na implementację serwera HTTP do przekazywania żądań do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-158">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="3a1ad-159">Przekazane żądanie opakowaniu jako zbiór obiektów funkcji, które mogą być udostępniane za pośrednictwem interfejsów.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-159">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="3a1ad-160">Platforma ASP.NET Core obejmuje serwera zarządzanego dla wielu platform sieci web, nazywanego [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="3a1ad-160">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="3a1ad-161">Kestrel jest często uruchamiane za produkcyjnym serwerze sieci web, takich jak [IIS](https://www.iis.net/) lub [Nginx](http://nginx.org) w konfiguracji zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-161">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="3a1ad-162">Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-162">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="3a1ad-163">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-163">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="3a1ad-164">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="3a1ad-164">Configuration</span></span>

<span data-ttu-id="3a1ad-165">Platforma ASP.NET Core używa modelu konfiguracji na podstawie par nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-165">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="3a1ad-166">Model konfiguracji nie jest oparty na <xref:System.Configuration> lub *web.config*. Konfiguracja pobiera ustawienia z uporządkowany zestaw dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-166">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="3a1ad-167">Dostawcy wbudowana Konfiguracja obsługują różne formaty plików (XML, JSON, INI), zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-167">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="3a1ad-168">Można również napisać własne dostawców konfiguracji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-168">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="3a1ad-169">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="3a1ad-170">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="3a1ad-170">Logging</span></span>

<span data-ttu-id="3a1ad-171">Platforma ASP.NET Core obsługuje rejestrowanie interfejsu API, który współdziała z różnych dostawców logowania.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-171">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="3a1ad-172">Wbudowani dostawcy obsługuje wysyłanie dzienników do jednego lub więcej miejsc docelowych.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-172">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="3a1ad-173">Może służyć struktur rejestrowania innych firm.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-173">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="3a1ad-174">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-174">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="3a1ad-175">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="3a1ad-175">Error handling</span></span>

<span data-ttu-id="3a1ad-176">Platforma ASP.NET Core ma wbudowane scenariusze obsługi błędów w aplikacji, w tym stroną wyjątków dla deweloperów, strony błędów niestandardowych, stron kodowych stan statycznych i uruchamiania obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-176">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="3a1ad-177">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-177">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="3a1ad-178">Routing</span><span class="sxs-lookup"><span data-stu-id="3a1ad-178">Routing</span></span>

<span data-ttu-id="3a1ad-179">Platforma ASP.NET Core oferuje scenariusze routing żądań aplikacji do obsługi trasy.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-179">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="3a1ad-180">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="3a1ad-181">Dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="3a1ad-181">File Providers</span></span>

<span data-ttu-id="3a1ad-182">Platforma ASP.NET Core przenosi dostępu do systemu plików przy użyciu dostawcy plików, która zapewnia wspólny interfejs do pracy z plikami na wielu platformach.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-182">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="3a1ad-183">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-183">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="3a1ad-184">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="3a1ad-184">Static files</span></span>

<span data-ttu-id="3a1ad-185">Statyczne pliki oprogramowanie pośredniczące pełni plików statycznych, takich jak pliki HTML, CSS, obrazów i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-185">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="3a1ad-186">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-186">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="3a1ad-187">Stan sesji i aplikacji</span><span class="sxs-lookup"><span data-stu-id="3a1ad-187">Session and app state</span></span>

<span data-ttu-id="3a1ad-188">Platforma ASP.NET Core oferuje kilka metod, aby zachować stan sesji i aplikacji, gdy użytkownik przegląda aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-188">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="3a1ad-189">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-189">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="3a1ad-190">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="3a1ad-190">Globalization and localization</span></span>

<span data-ttu-id="3a1ad-191">Tworzenie wielojęzycznych witryny sieci Web za pomocą programu ASP.NET Core umożliwia witryny dotrzeć do większej liczby osób.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-191">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="3a1ad-192">Platforma ASP.NET Core oferuje usługi oraz oprogramowanie pośredniczące lokalizacja zawartości do różnych języków i kultur.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-192">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="3a1ad-193">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-193">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="3a1ad-194">Funkcje na żądanie</span><span class="sxs-lookup"><span data-stu-id="3a1ad-194">Request features</span></span>

<span data-ttu-id="3a1ad-195">Szczegóły implementacji serwera sieci Web związanych z żądań HTTP i odpowiedzi są zdefiniowane w interfejsach.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-195">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="3a1ad-196">Te interfejsy są używane przez implementacje serwera i oprogramowania pośredniczącego do tworzenia i modyfikowania potoku hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-196">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="3a1ad-197">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-197">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="3a1ad-198">Zadania w tle</span><span class="sxs-lookup"><span data-stu-id="3a1ad-198">Background tasks</span></span>

<span data-ttu-id="3a1ad-199">Zadania w tle są implementowane jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-199">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="3a1ad-200">Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-200">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="3a1ad-201">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-201">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="3a1ad-202">Dostęp do obiektu HttpContext</span><span class="sxs-lookup"><span data-stu-id="3a1ad-202">Access HttpContext</span></span>

<span data-ttu-id="3a1ad-203">`HttpContext` podczas przetwarzania żądań ze stronami Razor i MVC jest automatycznie dostępny.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-203">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="3a1ad-204">W sytuacjach gdzie `HttpContext` nie jest łatwo dostępne, możesz uzyskać dostęp `HttpContext` za pośrednictwem <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interfejsu i jego domyślna implementacja <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-204">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="3a1ad-205">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-205">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="3a1ad-206">Funkcja WebSockets</span><span class="sxs-lookup"><span data-stu-id="3a1ad-206">WebSockets</span></span>

<span data-ttu-id="3a1ad-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="3a1ad-208">Jest używana w przypadku aplikacji, takie jak rozmowy, giełdowych, gry i dowolnym miejscu działanie funkcji w czasie rzeczywistym w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-208">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="3a1ad-209">Platforma ASP.NET Core obsługuje scenariusze gniazda sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-209">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="3a1ad-210">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-210">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="3a1ad-211">Microsoft.AspNetCore.App meta Microsoft.aspnetcore.all</span><span class="sxs-lookup"><span data-stu-id="3a1ad-211">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="3a1ad-212">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) meta Microsoft.aspnetcore.all upraszcza zarządzanie pakietami.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-212">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="3a1ad-213">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-213">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="3a1ad-214">Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="3a1ad-214">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="3a1ad-215">[Pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) meta Microsoft.aspnetcore.all dla platformy ASP.NET Core obejmuje:</span><span class="sxs-lookup"><span data-stu-id="3a1ad-215">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="3a1ad-216">Wszystkie obsługiwane pakiety przez zespół programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-216">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="3a1ad-217">We wszystkich obsługiwanych pakietów w kolejności od platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-217">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="3a1ad-218">Zależności wewnętrzne i firm 3 używane przez program ASP.NET Core i Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-218">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="3a1ad-219">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-219">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="3a1ad-220">Środowisko uruchomieniowe programu .NET core i .NET Framework</span><span class="sxs-lookup"><span data-stu-id="3a1ad-220">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="3a1ad-221">Aplikacji ASP.NET Core mogą określać docelową środowisko uruchomieniowe platformy .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-221">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="3a1ad-222">Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="3a1ad-222">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="3a1ad-223">Wybieranie między platformą ASP.NET Core i ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3a1ad-223">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="3a1ad-224">Aby uzyskać więcej informacji na temat wybierania między platformą ASP.NET Core i ASP.NET, zobacz <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span><span class="sxs-lookup"><span data-stu-id="3a1ad-224">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
