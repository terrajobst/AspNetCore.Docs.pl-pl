---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/index
ms.openlocfilehash: 11dc6336ae7667038983c967f28232bef325f5bb
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637773"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="41bfb-103">Podstawy platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41bfb-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="41bfb-104">Aplikacji ASP.NET Core jest aplikacją konsoli, która tworzy serwer sieci web w jego `Program.Main` metody.</span><span class="sxs-lookup"><span data-stu-id="41bfb-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="41bfb-105">`Main` Metody jest to aplikacja *zarządzany punkt wejścia*:</span><span class="sxs-lookup"><span data-stu-id="41bfb-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="41bfb-106">.NET Core hosta:</span><span class="sxs-lookup"><span data-stu-id="41bfb-106">The .NET Core Host:</span></span>

* <span data-ttu-id="41bfb-107">Ładunki [środowisko uruchomieniowe programu .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="41bfb-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="41bfb-108">Korzysta z pierwszym argumentem wiersza polecenia jako ścieżka do zarządzanej plik binarny, który zawiera punkt wejścia (`Main`) i rozpoczyna wykonywanie kodu.</span><span class="sxs-lookup"><span data-stu-id="41bfb-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="41bfb-109">`Main` Wywołuje metodę [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), zgodny [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern) w celu utworzenia hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="41bfb-110">Konstruktor ma metody definiujące serwera sieci web (na przykład <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) i Klasa początkowa (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="41bfb-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="41bfb-111">W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web jest przydzielany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="41bfb-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="41bfb-112">Host sieci web platformy ASP.NET Core podejmie próbę uruchomienia [Internet Information Services (IIS)](https://www.iis.net/), jeśli jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="41bfb-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="41bfb-113">Inne serwery sieci web, takich jak [HTTP.sys](xref:fundamentals/servers/httpsys), mogą być używane przez wywołanie metody rozszerzenia odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="41bfb-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="41bfb-114">`UseStartup` zostało wyjaśnione w dalszej [uruchamiania](#startup) sekcji.</span><span class="sxs-lookup"><span data-stu-id="41bfb-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="41bfb-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, zwracany typ `WebHost.CreateDefaultBuilder` wywołania, oferuje wiele metod opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="41bfb-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="41bfb-116">Oto niektóre z tych metod `UseHttpSys` do hostowania aplikacji w pliku HTTP.sys i <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> do określania zawartości katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="41bfb-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="41bfb-117"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> i <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> metod tworzenia <xref:Microsoft.AspNetCore.Hosting.IWebHost> obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="41bfb-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="41bfb-118">.NET Core hosta:</span><span class="sxs-lookup"><span data-stu-id="41bfb-118">The .NET Core Host:</span></span>

* <span data-ttu-id="41bfb-119">Ładunki [środowisko uruchomieniowe programu .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="41bfb-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="41bfb-120">Korzysta z pierwszym argumentem wiersza polecenia jako ścieżka do zarządzanej plik binarny, który zawiera punkt wejścia (`Main`) i rozpoczyna wykonywanie kodu.</span><span class="sxs-lookup"><span data-stu-id="41bfb-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="41bfb-121">`Main` Metoda używa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, zgodny [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern) w celu utworzenia hosta aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="41bfb-122">Konstruktor ma metody definiujące serwer sieci web (na przykład <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) i Klasa początkowa (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="41bfb-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="41bfb-123">W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) używany jest serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="41bfb-124">Inne serwery sieci web, takich jak [HTTP.sys](xref:fundamentals/servers/httpsys), mogą być używane przez wywołanie metody rozszerzenia odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="41bfb-124">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="41bfb-125">`UseStartup` zostało wyjaśnione w dalszej [uruchamiania](#startup) sekcji.</span><span class="sxs-lookup"><span data-stu-id="41bfb-125">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="41bfb-126">`WebHostBuilder` udostępnia wiele metod opcjonalne, w tym <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do hostowania w usługach IIS i usług IIS Express i <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> do określania zawartości katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="41bfb-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="41bfb-127"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> i <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> metod tworzenia <xref:Microsoft.AspNetCore.Hosting.IWebHost> obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="41bfb-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="41bfb-128">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="41bfb-128">Startup</span></span>

<span data-ttu-id="41bfb-129">`UseStartup` Metody `WebHostBuilder` Określa `Startup` klasy dla twojej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="41bfb-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="41bfb-130">`Startup` Klasa jest gdzie definiuje się Potok żądań obsługi i którym skonfigurowano wszystkich usług wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="41bfb-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="41bfb-131">`Startup` Klasy musi być publiczna i może zawierać następujących metod:</span><span class="sxs-lookup"><span data-stu-id="41bfb-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="41bfb-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definiuje [usług](#dependency-injection-services) używanych przez aplikację (na przykład, ASP.NET Core MVC, platformy Entity Framework Core, tożsamość).</span><span class="sxs-lookup"><span data-stu-id="41bfb-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="41bfb-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definiuje [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) o nazwie w Potok żądań.</span><span class="sxs-lookup"><span data-stu-id="41bfb-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="41bfb-134">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="41bfb-135">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="41bfb-135">Content root</span></span>

<span data-ttu-id="41bfb-136">Główny zawartości jest ścieżka podstawowa do żadnej zawartości, używanych przez aplikację, takie jak [stron Razor](xref:razor-pages/index), MVC widoków i statyczne elementy zawartości.</span><span class="sxs-lookup"><span data-stu-id="41bfb-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="41bfb-137">Domyślnie zawartość katalogu głównego jest tej samej lokalizacji co aplikację podstawową ścieżkę dla pliku wykonywalnego, hostowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41bfb-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="41bfb-138">Katalog główny sieci Web (webroot)</span><span class="sxs-lookup"><span data-stu-id="41bfb-138">Web root (webroot)</span></span>

<span data-ttu-id="41bfb-139">Webroot aplikacji jest to katalog, w do projektu zawierającego zasoby publicznej, statycznej, takie jak CSS, JavaScript i plików obrazów.</span><span class="sxs-lookup"><span data-stu-id="41bfb-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="41bfb-140">Domyślnie *wwwroot* jest webroot.</span><span class="sxs-lookup"><span data-stu-id="41bfb-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="41bfb-141">Dla elementu Razor (*.cshtml*) plików ukośnika tylda `~/` wskazuje webroot.</span><span class="sxs-lookup"><span data-stu-id="41bfb-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="41bfb-142">Począwszy od ścieżki `~/` są określane jako ścieżek wirtualnych.</span><span class="sxs-lookup"><span data-stu-id="41bfb-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="41bfb-143">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="41bfb-143">Dependency injection (services)</span></span>

<span data-ttu-id="41bfb-144">A *usługi* to składnik, który jest przeznaczony do wspólnej wykorzystania w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41bfb-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="41bfb-145">Usługi są udostępniane za pośrednictwem [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="41bfb-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="41bfb-146">Platforma ASP.NET Core obejmuje natywne kontener Inwersja kontroli (IoC), który obsługuje [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="41bfb-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="41bfb-147">Jeśli chcesz, możesz zastąpić domyślny kontener.</span><span class="sxs-lookup"><span data-stu-id="41bfb-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="41bfb-148">W uzupełnieniu do jego [luźne sprzężenia korzyści](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI sprawia, że usługi, takie jak [rejestrowania](xref:fundamentals/logging/index), która jest dostępna w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41bfb-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="41bfb-149">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="41bfb-150">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="41bfb-150">Middleware</span></span>

<span data-ttu-id="41bfb-151">W programie ASP.NET Core redagowania Twojego żądania potoku przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="41bfb-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="41bfb-152">Oprogramowanie pośredniczące platformy ASP.NET Core wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="41bfb-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="41bfb-153">Zgodnie z Konwencją składnik oprogramowania pośredniczącego o nazwie "XYZ" zostanie dodany do potoku za pomocą wywołania `UseXYZ` metody rozszerzenia w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="41bfb-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="41bfb-154">Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego i można napisać własne niestandardowe oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="41bfb-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="41bfb-155">[Otwórz interfejs sieci Web platformy .NET (OWIN)](xref:fundamentals/owin), który umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web jest obsługiwana w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41bfb-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="41bfb-156">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index> i <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="41bfb-157">Inicjowanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="41bfb-157">Initiate HTTP requests</span></span>

<span data-ttu-id="41bfb-158"><xref:System.Net.Http.IHttpClientFactory> jest dostępna w celu uzyskania dostępu do <xref:System.Net.Http.HttpClient> wystąpień do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="41bfb-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="41bfb-159">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="41bfb-160">Środowiska</span><span class="sxs-lookup"><span data-stu-id="41bfb-160">Environments</span></span>

<span data-ttu-id="41bfb-161">Środowisk, takich jak *rozwoju* i *produkcji*, są najwyższej jakości pojęcie w programie ASP.NET Core i można ustawić przy użyciu zmiennej środowiskowej, plik ustawień i argument wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="41bfb-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="41bfb-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="41bfb-163">Hosting</span><span class="sxs-lookup"><span data-stu-id="41bfb-163">Hosting</span></span>

<span data-ttu-id="41bfb-164">Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*, który jest odpowiedzialny za zarządzanie uruchamiania i okres istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41bfb-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="41bfb-165">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="41bfb-166">Serwery</span><span class="sxs-lookup"><span data-stu-id="41bfb-166">Servers</span></span>

<span data-ttu-id="41bfb-167">Model hostingu w programie ASP.NET Core bezpośrednio nie nasłuchuje żądań.</span><span class="sxs-lookup"><span data-stu-id="41bfb-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="41bfb-168">Model hostingu opiera się na implementację serwera HTTP do przekazywania żądań do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41bfb-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="41bfb-169">Windows</span><span class="sxs-lookup"><span data-stu-id="41bfb-169">Windows</span></span>](#tab/windows)

<span data-ttu-id="41bfb-170">Platforma ASP.NET Core oferuje następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="41bfb-170">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="41bfb-171">[Kestrel](xref:fundamentals/servers/kestrel) serwera zarządzanego dla wielu platform sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-171">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="41bfb-172">Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="41bfb-172">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="41bfb-173">Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="41bfb-173">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="41bfb-174">Serwer HTTP usług IIS (`IISHttpServer`) jest [wewnątrz procesowego](xref:fundamentals/servers/index#in-process-hosting-model) dla usług IIS.</span><span class="sxs-lookup"><span data-stu-id="41bfb-174">IIS HTTP Server (`IISHttpServer`) is an [in-process server](xref:fundamentals/servers/index#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="41bfb-175">[Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) serwer jest serwerem sieci web dla platformy ASP.NET Core na Windows.</span><span class="sxs-lookup"><span data-stu-id="41bfb-175">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="41bfb-176">macOS</span><span class="sxs-lookup"><span data-stu-id="41bfb-176">macOS</span></span>](#tab/macos)

<span data-ttu-id="41bfb-177">Używa platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel) implementacji serwera.</span><span class="sxs-lookup"><span data-stu-id="41bfb-177">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="41bfb-178">Kestrel jest serwerem zarządzanego dla wielu platform sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-178">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="41bfb-179">Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="41bfb-179">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="41bfb-180">Linux</span><span class="sxs-lookup"><span data-stu-id="41bfb-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="41bfb-181">Używa platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel) implementacji serwera.</span><span class="sxs-lookup"><span data-stu-id="41bfb-181">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="41bfb-182">Kestrel jest serwerem zarządzanego dla wielu platform sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-182">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="41bfb-183">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](http://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="41bfb-183">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="41bfb-184">Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="41bfb-184">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="41bfb-185">Windows</span><span class="sxs-lookup"><span data-stu-id="41bfb-185">Windows</span></span>](#tab/windows)

<span data-ttu-id="41bfb-186">Platforma ASP.NET Core oferuje następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="41bfb-186">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="41bfb-187">[Kestrel](xref:fundamentals/servers/kestrel) serwera zarządzanego dla wielu platform sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-187">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="41bfb-188">Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="41bfb-188">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="41bfb-189">Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="41bfb-189">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="41bfb-190">[Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) serwer jest serwerem sieci web dla platformy ASP.NET Core na Windows.</span><span class="sxs-lookup"><span data-stu-id="41bfb-190">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="41bfb-191">macOS</span><span class="sxs-lookup"><span data-stu-id="41bfb-191">macOS</span></span>](#tab/macos)

<span data-ttu-id="41bfb-192">Używa platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel) implementacji serwera.</span><span class="sxs-lookup"><span data-stu-id="41bfb-192">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="41bfb-193">Kestrel jest serwerem zarządzanego dla wielu platform sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-193">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="41bfb-194">Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="41bfb-194">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="41bfb-195">Linux</span><span class="sxs-lookup"><span data-stu-id="41bfb-195">Linux</span></span>](#tab/linux)

<span data-ttu-id="41bfb-196">Używa platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel) implementacji serwera.</span><span class="sxs-lookup"><span data-stu-id="41bfb-196">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="41bfb-197">Kestrel jest serwerem zarządzanego dla wielu platform sieci web.</span><span class="sxs-lookup"><span data-stu-id="41bfb-197">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="41bfb-198">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](http://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="41bfb-198">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="41bfb-199">Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="41bfb-199">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="41bfb-200">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-200">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="41bfb-201">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="41bfb-201">Configuration</span></span>

<span data-ttu-id="41bfb-202">Platforma ASP.NET Core używa modelu konfiguracji na podstawie par nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="41bfb-202">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="41bfb-203">Model konfiguracji nie jest oparty na <xref:System.Configuration> lub *web.config*. Konfiguracja pobiera ustawienia z uporządkowany zestaw dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="41bfb-203">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="41bfb-204">Dostawcy wbudowana Konfiguracja obsługują różne formaty plików (XML, JSON, INI), zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="41bfb-204">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="41bfb-205">Można również napisać własne dostawców konfiguracji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="41bfb-205">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="41bfb-206">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="41bfb-207">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="41bfb-207">Logging</span></span>

<span data-ttu-id="41bfb-208">Platforma ASP.NET Core obsługuje rejestrowanie interfejsu API, który współdziała z różnych dostawców logowania.</span><span class="sxs-lookup"><span data-stu-id="41bfb-208">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="41bfb-209">Wbudowani dostawcy obsługuje wysyłanie dzienników do jednego lub więcej miejsc docelowych.</span><span class="sxs-lookup"><span data-stu-id="41bfb-209">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="41bfb-210">Może służyć struktur rejestrowania innych firm.</span><span class="sxs-lookup"><span data-stu-id="41bfb-210">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="41bfb-211">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-211">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="41bfb-212">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="41bfb-212">Error handling</span></span>

<span data-ttu-id="41bfb-213">Platforma ASP.NET Core ma wbudowane scenariusze obsługi błędów w aplikacji, w tym stroną wyjątków dla deweloperów, strony błędów niestandardowych, stron kodowych stan statycznych i uruchamiania obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="41bfb-213">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="41bfb-214">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-214">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="41bfb-215">Routing</span><span class="sxs-lookup"><span data-stu-id="41bfb-215">Routing</span></span>

<span data-ttu-id="41bfb-216">Platforma ASP.NET Core oferuje scenariusze routing żądań aplikacji do obsługi trasy.</span><span class="sxs-lookup"><span data-stu-id="41bfb-216">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="41bfb-217">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="41bfb-218">Zadania w tle</span><span class="sxs-lookup"><span data-stu-id="41bfb-218">Background tasks</span></span>

<span data-ttu-id="41bfb-219">Zadania w tle są implementowane jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="41bfb-219">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="41bfb-220">Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="41bfb-220">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="41bfb-221">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-221">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="41bfb-222">Dostęp do obiektu HttpContext</span><span class="sxs-lookup"><span data-stu-id="41bfb-222">Access HttpContext</span></span>

<span data-ttu-id="41bfb-223">`HttpContext` podczas przetwarzania żądań ze stronami Razor i MVC jest automatycznie dostępny.</span><span class="sxs-lookup"><span data-stu-id="41bfb-223">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="41bfb-224">W sytuacjach gdzie `HttpContext` nie jest łatwo dostępne, możesz uzyskać dostęp `HttpContext` za pośrednictwem <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interfejsu i jego domyślna implementacja <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-224">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="41bfb-225">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="41bfb-225">For more information, see <xref:fundamentals/httpcontext>.</span></span>
