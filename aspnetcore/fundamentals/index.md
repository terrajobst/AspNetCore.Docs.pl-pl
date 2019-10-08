---
title: Podstawy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe koncepcje tworzenia aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/index
ms.openlocfilehash: a70d6aa05a2c92d19076b8d6e4ea24d7554368b6
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007120"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="951b3-103">Podstawy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="951b3-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="951b3-104">Ten artykuł zawiera omówienie najważniejszych tematów dotyczących sposobu tworzenia aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="951b3-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="951b3-105">Klasa Startup</span><span class="sxs-lookup"><span data-stu-id="951b3-105">The Startup class</span></span>

<span data-ttu-id="951b3-106">Klasa `Startup` to:</span><span class="sxs-lookup"><span data-stu-id="951b3-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="951b3-107">Usługi wymagane przez aplikację są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="951b3-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="951b3-108">Zdefiniowano potok obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="951b3-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="951b3-109">*Usługi* są składnikami, które są używane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="951b3-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="951b3-110">Na przykład składnik rejestrowania to usługa.</span><span class="sxs-lookup"><span data-stu-id="951b3-110">For example, a logging component is a service.</span></span> <span data-ttu-id="951b3-111">Kod do konfigurowania (lub *rejestrowania*) usług jest dodawany do metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="951b3-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="951b3-112">Potok obsługi żądań składa się z serii komponentów *oprogramowania pośredniczącego* .</span><span class="sxs-lookup"><span data-stu-id="951b3-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="951b3-113">Na przykład oprogramowanie pośredniczące może obsługiwać żądania dotyczące plików statycznych lub przekierowywać żądania HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="951b3-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="951b3-114">Każde oprogramowanie pośredniczące wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="951b3-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="951b3-115">Kod do konfigurowania potoku obsługi żądań jest dodawany do metody `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="951b3-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="951b3-116">Oto przykład `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="951b3-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="951b3-117">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="951b3-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="951b3-118">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="951b3-118">Dependency injection (services)</span></span>

<span data-ttu-id="951b3-119">ASP.NET Core ma wbudowaną platformę wstrzykiwania zależności (DI), która udostępnia skonfigurowane usługi dla klas aplikacji.</span><span class="sxs-lookup"><span data-stu-id="951b3-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="951b3-120">Jednym ze sposobów uzyskania wystąpienia usługi w klasie jest utworzenie konstruktora z parametrem wymaganego typu.</span><span class="sxs-lookup"><span data-stu-id="951b3-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="951b3-121">Parametr może być typem usługi lub interfejsem.</span><span class="sxs-lookup"><span data-stu-id="951b3-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="951b3-122">System DI zapewnia usługę w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="951b3-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="951b3-123">Oto Klasa, która używa funkcji DI do pobrania obiektu kontekstu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="951b3-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="951b3-124">Wyróżniony wiersz jest przykładem iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="951b3-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="951b3-125">Podczas gdy program jest wbudowany, został zaprojektowany z myślą o umożliwieniu podłączenia kontenera kontroli (IoC) innej firmy.</span><span class="sxs-lookup"><span data-stu-id="951b3-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="951b3-126">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="951b3-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="951b3-127">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="951b3-127">Middleware</span></span>

<span data-ttu-id="951b3-128">Potok obsługi żądań składa się z serii komponentów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="951b3-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="951b3-129">Każdy składnik wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="951b3-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="951b3-130">Zgodnie z Konwencją składnik pośredniczący jest dodawany do potoku przez wywoływanie metody rozszerzenia `Use...` w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="951b3-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="951b3-131">Na przykład, aby włączyć renderowanie plików statycznych, wywołaj `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="951b3-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="951b3-132">Wyróżniony kod w poniższym przykładzie konfiguruje potok obsługi żądań:</span><span class="sxs-lookup"><span data-stu-id="951b3-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="951b3-133">ASP.NET Core zawiera rozbudowany zestaw wbudowanych programów pośredniczących i można napisać niestandardowe oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="951b3-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="951b3-134">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="951b3-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="951b3-135">Host</span><span class="sxs-lookup"><span data-stu-id="951b3-135">Host</span></span>

<span data-ttu-id="951b3-136">Aplikacja ASP.NET Core kompiluje *hosta* podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="951b3-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="951b3-137">Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="951b3-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="951b3-138">Implementacja serwera HTTP</span><span class="sxs-lookup"><span data-stu-id="951b3-138">An HTTP server implementation</span></span>
* <span data-ttu-id="951b3-139">Składniki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="951b3-139">Middleware components</span></span>
* <span data-ttu-id="951b3-140">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="951b3-140">Logging</span></span>
* <span data-ttu-id="951b3-141">FOSFORAN</span><span class="sxs-lookup"><span data-stu-id="951b3-141">DI</span></span>
* <span data-ttu-id="951b3-142">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="951b3-142">Configuration</span></span>

<span data-ttu-id="951b3-143">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="951b3-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="951b3-144">Dostępne są dwa hosty: Host ogólny i host sieci Web.</span><span class="sxs-lookup"><span data-stu-id="951b3-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="951b3-145">Zalecany jest host ogólny, a host sieci Web jest dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="951b3-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="951b3-146">Kod służący do tworzenia hosta jest w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="951b3-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="951b3-147">Metody `CreateDefaultBuilder` i `ConfigureWebHostDefaults` służą do konfigurowania hosta z najczęściej używanymi opcjami, takimi jak następujące:</span><span class="sxs-lookup"><span data-stu-id="951b3-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="951b3-148">Użyj [Kestrel](#servers) jako serwera sieci Web i Włącz INTEGRACJĘ usług IIS.</span><span class="sxs-lookup"><span data-stu-id="951b3-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="951b3-149">Załaduj konfigurację z pliku *appSettings. JSON*, *appSettings. { Nazwa środowiska}. JSON*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="951b3-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="951b3-150">Wyślij dane wyjściowe rejestrowania do konsoli programu i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="951b3-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="951b3-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="951b3-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="951b3-152">Dostępne są dwa hosty: host sieci Web i Host ogólny.</span><span class="sxs-lookup"><span data-stu-id="951b3-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="951b3-153">W ASP.NET Core 2. x Host generyczny jest przeznaczony tylko dla scenariuszy innych niż sieci Web.</span><span class="sxs-lookup"><span data-stu-id="951b3-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="951b3-154">Kod służący do tworzenia hosta jest w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="951b3-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="951b3-155">Metoda `CreateDefaultBuilder` konfiguruje hosta za pomocą często używanych opcji, takich jak następujące:</span><span class="sxs-lookup"><span data-stu-id="951b3-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="951b3-156">Użyj [Kestrel](#servers) jako serwera sieci Web i Włącz INTEGRACJĘ usług IIS.</span><span class="sxs-lookup"><span data-stu-id="951b3-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="951b3-157">Załaduj konfigurację z pliku *appSettings. JSON*, *appSettings. { Nazwa środowiska}. JSON*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="951b3-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="951b3-158">Wyślij dane wyjściowe rejestrowania do konsoli programu i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="951b3-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="951b3-159">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="951b3-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="951b3-160">Scenariusze inne niż internetowe</span><span class="sxs-lookup"><span data-stu-id="951b3-160">Non-web scenarios</span></span>

<span data-ttu-id="951b3-161">Host ogólny umożliwia innym typom aplikacji korzystanie z rozszerzeń struktury wycinania, takich jak rejestrowanie, iniekcja zależności (DI), konfiguracja i zarządzanie okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="951b3-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="951b3-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="951b3-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="951b3-163">Serwery</span><span class="sxs-lookup"><span data-stu-id="951b3-163">Servers</span></span>

<span data-ttu-id="951b3-164">Aplikacja ASP.NET Core używa implementacji serwera HTTP do nasłuchiwania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="951b3-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="951b3-165">Serwer wyświetla żądania do aplikacji jako zestaw [funkcji żądania](xref:fundamentals/request-features) składających się na `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="951b3-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="951b3-166">Windows</span><span class="sxs-lookup"><span data-stu-id="951b3-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="951b3-167">ASP.NET Core udostępnia następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="951b3-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="951b3-168">*Kestrel* to Międzyplatformowy serwer sieci Web.</span><span class="sxs-lookup"><span data-stu-id="951b3-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="951b3-169">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="951b3-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="951b3-170">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="951b3-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="951b3-171">*Serwer http IIS* jest serwerem dla systemu Windows, który korzysta z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="951b3-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="951b3-172">Na tym serwerze aplikacja ASP.NET Core i usługi IIS działają w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="951b3-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="951b3-173">*Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="951b3-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="951b3-174">macOS</span><span class="sxs-lookup"><span data-stu-id="951b3-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="951b3-175">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="951b3-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="951b3-176">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="951b3-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="951b3-177">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="951b3-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="951b3-178">Linux</span><span class="sxs-lookup"><span data-stu-id="951b3-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="951b3-179">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="951b3-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="951b3-180">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="951b3-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="951b3-181">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="951b3-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="951b3-182">Windows</span><span class="sxs-lookup"><span data-stu-id="951b3-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="951b3-183">ASP.NET Core udostępnia następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="951b3-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="951b3-184">*Kestrel* to Międzyplatformowy serwer sieci Web.</span><span class="sxs-lookup"><span data-stu-id="951b3-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="951b3-185">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="951b3-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="951b3-186">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="951b3-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="951b3-187">*Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="951b3-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="951b3-188">macOS</span><span class="sxs-lookup"><span data-stu-id="951b3-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="951b3-189">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="951b3-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="951b3-190">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="951b3-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="951b3-191">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="951b3-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="951b3-192">Linux</span><span class="sxs-lookup"><span data-stu-id="951b3-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="951b3-193">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="951b3-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="951b3-194">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="951b3-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="951b3-195">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="951b3-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="951b3-196">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="951b3-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="951b3-197">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="951b3-197">Configuration</span></span>

<span data-ttu-id="951b3-198">ASP.NET Core udostępnia platformę konfiguracji, która pobiera ustawienia jako pary nazwa-wartość z uporządkowanego zestawu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="951b3-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="951b3-199">Istnieją Wbudowani dostawcy konfiguracji dla różnych źródeł, takich jak pliki *. JSON* , pliki *. XML* , zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="951b3-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="951b3-200">Możesz również napisać niestandardowych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="951b3-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="951b3-201">Można na przykład określić, że konfiguracja pochodzi z pliku *appSettings. JSON* i zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="951b3-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="951b3-202">Następnie po zażądaniu wartości parametru *ConnectionString* struktura najpierw sprawdza plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="951b3-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="951b3-203">Jeśli wartość jest tam znaleziona, ale również w zmiennej środowiskowej, pierwszeństwo ma wartość ze zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="951b3-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="951b3-204">Aby zarządzać poufnymi danymi konfiguracyjnymi, takimi jak hasła, ASP.NET Core zapewnia [Narzędzie tajnego Menedżera](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="951b3-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="951b3-205">W przypadku wpisów tajnych produkcji zalecamy [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="951b3-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="951b3-206">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="951b3-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="951b3-207">Opcje</span><span class="sxs-lookup"><span data-stu-id="951b3-207">Options</span></span>

<span data-ttu-id="951b3-208">Jeśli to możliwe, ASP.NET Core są zgodne ze *wzorcem opcji* przechowywania i pobierania wartości konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="951b3-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="951b3-209">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="951b3-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="951b3-210">Na przykład poniższy kod ustawia opcje obiektów WebSockets:</span><span class="sxs-lookup"><span data-stu-id="951b3-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="951b3-211">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="951b3-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="951b3-212">Wiejski</span><span class="sxs-lookup"><span data-stu-id="951b3-212">Environments</span></span>

<span data-ttu-id="951b3-213">Środowiska wykonawcze, takie jak *programowanie*, *przemieszczanie*i *produkcja*, są pierwszą klasą koncepcji w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="951b3-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="951b3-214">Aby określić środowisko, w którym działa aplikacja, należy ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="951b3-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="951b3-215">ASP.NET Core odczytuje tę zmienną środowiskową przy uruchamianiu aplikacji i zapisuje wartość w implementacji `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="951b3-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="951b3-216">Obiekt środowiska jest dostępny w dowolnym miejscu w aplikacji za pomocą funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="951b3-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="951b3-217">Następujący przykładowy kod z klasy `Startup` konfiguruje aplikację w celu dostarczania szczegółowych informacji o błędzie tylko wtedy, gdy działa w trakcie tworzenia:</span><span class="sxs-lookup"><span data-stu-id="951b3-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="951b3-218">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="951b3-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="951b3-219">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="951b3-219">Logging</span></span>

<span data-ttu-id="951b3-220">ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm.</span><span class="sxs-lookup"><span data-stu-id="951b3-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="951b3-221">Dostępne są następujące dostawcy:</span><span class="sxs-lookup"><span data-stu-id="951b3-221">Available providers include the following:</span></span>

* <span data-ttu-id="951b3-222">Konsola</span><span class="sxs-lookup"><span data-stu-id="951b3-222">Console</span></span>
* <span data-ttu-id="951b3-223">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="951b3-223">Debug</span></span>
* <span data-ttu-id="951b3-224">Śledzenie zdarzeń w systemie Windows</span><span class="sxs-lookup"><span data-stu-id="951b3-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="951b3-225">Dziennik zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="951b3-225">Windows Event Log</span></span>
* <span data-ttu-id="951b3-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="951b3-226">TraceSource</span></span>
* <span data-ttu-id="951b3-227">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="951b3-227">Azure App Service</span></span>
* <span data-ttu-id="951b3-228">Application Insights platformy Azure</span><span class="sxs-lookup"><span data-stu-id="951b3-228">Azure Application Insights</span></span>

<span data-ttu-id="951b3-229">Zapisuj dzienniki z dowolnego miejsca w kodzie aplikacji, pobierając obiekt `ILogger` z metod rejestrowania i wywoływania.</span><span class="sxs-lookup"><span data-stu-id="951b3-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="951b3-230">Oto przykładowy kod, który używa obiektu `ILogger`, z iniekcją konstruktora i wyskakującymi wywołaniami metody rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="951b3-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="951b3-231">Interfejs `ILogger` umożliwia przekazanie dowolnej liczby pól dostawcy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="951b3-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="951b3-232">Pola są często używane do konstruowania ciągu komunikatu, ale dostawcy mogą również wysyłać je jako oddzielne pola do magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="951b3-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="951b3-233">Ta funkcja umożliwia dostawcom rejestrowania implementowanie [rejestrowania semantycznego, znanego również jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="951b3-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="951b3-234">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="951b3-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="951b3-235">Routing</span><span class="sxs-lookup"><span data-stu-id="951b3-235">Routing</span></span>

<span data-ttu-id="951b3-236">*Trasa* jest WZORCEM adresu URL, który jest mapowany do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="951b3-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="951b3-237">Procedura obsługi jest zazwyczaj stroną Razor, metodą akcji w kontrolerze MVC lub w oprogramowaniu pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="951b3-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="951b3-238">Routing ASP.NET Core zapewnia kontrolę nad adresami URL używanymi przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="951b3-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="951b3-239">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="951b3-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="951b3-240">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="951b3-240">Error handling</span></span>

<span data-ttu-id="951b3-241">ASP.NET Core ma wbudowane funkcje do obsługi błędów, takie jak:</span><span class="sxs-lookup"><span data-stu-id="951b3-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="951b3-242">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="951b3-242">A developer exception page</span></span>
* <span data-ttu-id="951b3-243">Niestandardowe strony błędów</span><span class="sxs-lookup"><span data-stu-id="951b3-243">Custom error pages</span></span>
* <span data-ttu-id="951b3-244">Statyczne strony kodów stanu</span><span class="sxs-lookup"><span data-stu-id="951b3-244">Static status code pages</span></span>
* <span data-ttu-id="951b3-245">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="951b3-245">Startup exception handling</span></span>

<span data-ttu-id="951b3-246">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="951b3-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="951b3-247">Zgłaszanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="951b3-247">Make HTTP requests</span></span>

<span data-ttu-id="951b3-248">Implementacja `IHttpClientFactory` jest dostępna do tworzenia wystąpień `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="951b3-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="951b3-249">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="951b3-249">The factory:</span></span>

* <span data-ttu-id="951b3-250">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień logicznej `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="951b3-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="951b3-251">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="951b3-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="951b3-252">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="951b3-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="951b3-253">Obsługuje rejestrację i łańcuch wielu procedur delegowania, aby utworzyć potok pośredniczący żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="951b3-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="951b3-254">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="951b3-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="951b3-255">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="951b3-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="951b3-256">Integruje się z usługą *Polly*, popularną biblioteką innej firmy na potrzeby obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="951b3-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="951b3-257">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS, które występują podczas ręcznego zarządzania okresami istnienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="951b3-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="951b3-258">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="951b3-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="951b3-259">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="951b3-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="951b3-260">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="951b3-260">Content root</span></span>

<span data-ttu-id="951b3-261">Katalog główny zawartości jest ścieżką podstawową do:</span><span class="sxs-lookup"><span data-stu-id="951b3-261">The content root is the base path to the:</span></span>

* <span data-ttu-id="951b3-262">Plik wykonywalny obsługujący aplikację ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="951b3-262">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="951b3-263">Skompilowane zestawy wchodzące w skład aplikacji ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="951b3-263">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="951b3-264">Pliki zawartości inne niż kod używane przez aplikację, takie jak:</span><span class="sxs-lookup"><span data-stu-id="951b3-264">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="951b3-265">Pliki Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="951b3-265">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="951b3-266">Pliki konfiguracji ( *. JSON*, *. XML*)</span><span class="sxs-lookup"><span data-stu-id="951b3-266">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="951b3-267">Pliki danych ( *. DB*)</span><span class="sxs-lookup"><span data-stu-id="951b3-267">Data files (*.db*)</span></span>
* <span data-ttu-id="951b3-268">[Katalog główny sieci Web](#web-root), zazwyczaj opublikowany folder *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="951b3-268">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="951b3-269">Podczas tworzenia:</span><span class="sxs-lookup"><span data-stu-id="951b3-269">During development:</span></span>

* <span data-ttu-id="951b3-270">Katalog główny zawartości domyślnie jest katalogiem głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="951b3-270">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="951b3-271">Katalog główny projektu służy do tworzenia:</span><span class="sxs-lookup"><span data-stu-id="951b3-271">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="951b3-272">Ścieżka do plików zawartości nienależących do kodu aplikacji w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="951b3-272">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="951b3-273">[Katalog główny sieci Web](#web-root), zazwyczaj folder *wwwroot* w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="951b3-273">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="951b3-274">Alternatywna ścieżka katalogu głównego zawartości może być określona podczas [kompilowania hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="951b3-274">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="951b3-275">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="951b3-275">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="951b3-276">Alternatywna ścieżka katalogu głównego zawartości może być określona podczas [kompilowania hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="951b3-276">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="951b3-277">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="951b3-277">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="951b3-278">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="951b3-278">Web root</span></span>

<span data-ttu-id="951b3-279">Katalog główny sieci Web jest ścieżką podstawową do publicznych, niekodowych, statycznych plików zasobów, takich jak:</span><span class="sxs-lookup"><span data-stu-id="951b3-279">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="951b3-280">Arkusze stylów ( *. css*)</span><span class="sxs-lookup"><span data-stu-id="951b3-280">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="951b3-281">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="951b3-281">JavaScript (*.js*)</span></span>
* <span data-ttu-id="951b3-282">Obrazy ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="951b3-282">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="951b3-283">Pliki statyczne są obsługiwane domyślnie tylko z katalogu głównego (i katalogów podrzędnych) sieci Web.</span><span class="sxs-lookup"><span data-stu-id="951b3-283">Static files are only served by default from the web root directory (and sub-directories).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="951b3-284">Ścieżka katalogu głównego sieci Web jest domyślnie ustawiona na *{Content root}/wwwroot*, ale podczas [kompilowania hosta](#host)można określić inny katalog internetowy w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="951b3-284">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="951b3-285">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="951b3-285">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="951b3-286">Ścieżka katalogu głównego sieci Web jest domyślnie ustawiona na *{Content root}/wwwroot*, ale podczas [kompilowania hosta](#host)można określić inny katalog internetowy w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="951b3-286">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="951b3-287">Aby uzyskać więcej informacji, zobacz [katalog główny sieci Web](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="951b3-287">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

::: moniker-end

<span data-ttu-id="951b3-288">W plikach Razor ( *. cshtml*), ukośnik (`~/`) wskazuje na katalog główny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="951b3-288">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="951b3-289">Ścieżka rozpoczynająca się od `~/` jest nazywana *ścieżką wirtualną*.</span><span class="sxs-lookup"><span data-stu-id="951b3-289">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="951b3-290">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="951b3-290">For more information, see <xref:fundamentals/static-files>.</span></span>
