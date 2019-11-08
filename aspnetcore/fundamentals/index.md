---
title: Podstawy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe koncepcje tworzenia aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/index
ms.openlocfilehash: 7173a732a04bf3e598adef298fa9120c15dd52fb
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799368"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="45c02-103">Podstawy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45c02-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="45c02-104">Ten artykuł zawiera omówienie najważniejszych tematów dotyczących sposobu tworzenia aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="45c02-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="45c02-105">Klasa Startup</span><span class="sxs-lookup"><span data-stu-id="45c02-105">The Startup class</span></span>

<span data-ttu-id="45c02-106">Klasa `Startup` to:</span><span class="sxs-lookup"><span data-stu-id="45c02-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="45c02-107">Usługi wymagane przez aplikację są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="45c02-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="45c02-108">Zdefiniowano potok obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="45c02-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="45c02-109">*Usługi* są składnikami, które są używane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="45c02-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="45c02-110">Na przykład składnik rejestrowania to usługa.</span><span class="sxs-lookup"><span data-stu-id="45c02-110">For example, a logging component is a service.</span></span> <span data-ttu-id="45c02-111">Kod do konfigurowania (lub *rejestrowania*) usług jest dodawany do metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="45c02-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="45c02-112">Potok obsługi żądań składa się z serii komponentów *oprogramowania pośredniczącego* .</span><span class="sxs-lookup"><span data-stu-id="45c02-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="45c02-113">Na przykład oprogramowanie pośredniczące może obsługiwać żądania dotyczące plików statycznych lub przekierowywać żądania HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="45c02-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="45c02-114">Każde oprogramowanie pośredniczące wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="45c02-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="45c02-115">Kod do konfigurowania potoku obsługi żądań został dodany do metody `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="45c02-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="45c02-116">Oto przykładowa Klasa `Startup`:</span><span class="sxs-lookup"><span data-stu-id="45c02-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="45c02-117">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="45c02-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="45c02-118">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="45c02-118">Dependency injection (services)</span></span>

<span data-ttu-id="45c02-119">ASP.NET Core ma wbudowaną platformę wstrzykiwania zależności (DI), która udostępnia skonfigurowane usługi dla klas aplikacji.</span><span class="sxs-lookup"><span data-stu-id="45c02-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="45c02-120">Jednym ze sposobów uzyskania wystąpienia usługi w klasie jest utworzenie konstruktora z parametrem wymaganego typu.</span><span class="sxs-lookup"><span data-stu-id="45c02-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="45c02-121">Parametr może być typem usługi lub interfejsem.</span><span class="sxs-lookup"><span data-stu-id="45c02-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="45c02-122">System DI zapewnia usługę w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="45c02-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="45c02-123">Oto Klasa, która używa funkcji DI do pobrania obiektu kontekstu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="45c02-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="45c02-124">Wyróżniony wiersz jest przykładem iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="45c02-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="45c02-125">Podczas gdy program jest wbudowany, został zaprojektowany z myślą o umożliwieniu podłączenia kontenera kontroli (IoC) innej firmy.</span><span class="sxs-lookup"><span data-stu-id="45c02-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="45c02-126">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="45c02-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="45c02-127">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="45c02-127">Middleware</span></span>

<span data-ttu-id="45c02-128">Potok obsługi żądań składa się z serii komponentów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="45c02-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="45c02-129">Każdy składnik wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="45c02-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="45c02-130">Zgodnie z Konwencją składnik pośredniczący jest dodawany do potoku przez wywołanie jego metody rozszerzenia `Use...` w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="45c02-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="45c02-131">Na przykład, aby włączyć renderowanie plików statycznych, wywołaj `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="45c02-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="45c02-132">Wyróżniony kod w poniższym przykładzie konfiguruje potok obsługi żądań:</span><span class="sxs-lookup"><span data-stu-id="45c02-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="45c02-133">ASP.NET Core zawiera rozbudowany zestaw wbudowanych programów pośredniczących i można napisać niestandardowe oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="45c02-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="45c02-134">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="45c02-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="45c02-135">Host</span><span class="sxs-lookup"><span data-stu-id="45c02-135">Host</span></span>

<span data-ttu-id="45c02-136">Aplikacja ASP.NET Core kompiluje *hosta* podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="45c02-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="45c02-137">Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="45c02-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="45c02-138">Implementacja serwera HTTP</span><span class="sxs-lookup"><span data-stu-id="45c02-138">An HTTP server implementation</span></span>
* <span data-ttu-id="45c02-139">Składniki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="45c02-139">Middleware components</span></span>
* <span data-ttu-id="45c02-140">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="45c02-140">Logging</span></span>
* <span data-ttu-id="45c02-141">FOSFORAN</span><span class="sxs-lookup"><span data-stu-id="45c02-141">DI</span></span>
* <span data-ttu-id="45c02-142">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="45c02-142">Configuration</span></span>

<span data-ttu-id="45c02-143">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="45c02-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="45c02-144">Dostępne są dwa hosty: Host ogólny i host sieci Web.</span><span class="sxs-lookup"><span data-stu-id="45c02-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="45c02-145">Zalecany jest host ogólny, a host sieci Web jest dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="45c02-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="45c02-146">Kod służący do tworzenia hosta znajduje się w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="45c02-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="45c02-147">Metody `CreateDefaultBuilder` i `ConfigureWebHostDefaults` umożliwiają skonfigurowanie hosta z najczęściej używanymi opcjami, takimi jak następujące:</span><span class="sxs-lookup"><span data-stu-id="45c02-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="45c02-148">Użyj [Kestrel](#servers) jako serwera sieci Web i Włącz INTEGRACJĘ usług IIS.</span><span class="sxs-lookup"><span data-stu-id="45c02-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="45c02-149">Załaduj konfigurację z pliku *appSettings. JSON*, *appSettings. { Nazwa środowiska}. JSON*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="45c02-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="45c02-150">Wyślij dane wyjściowe rejestrowania do konsoli programu i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="45c02-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="45c02-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="45c02-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="45c02-152">Dostępne są dwa hosty: host sieci Web i Host ogólny.</span><span class="sxs-lookup"><span data-stu-id="45c02-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="45c02-153">W ASP.NET Core 2. x Host generyczny jest przeznaczony tylko dla scenariuszy innych niż sieci Web.</span><span class="sxs-lookup"><span data-stu-id="45c02-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="45c02-154">Kod służący do tworzenia hosta znajduje się w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="45c02-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="45c02-155">Metoda `CreateDefaultBuilder` konfiguruje hosta z najczęściej używanymi opcjami, takimi jak:</span><span class="sxs-lookup"><span data-stu-id="45c02-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="45c02-156">Użyj [Kestrel](#servers) jako serwera sieci Web i Włącz INTEGRACJĘ usług IIS.</span><span class="sxs-lookup"><span data-stu-id="45c02-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="45c02-157">Załaduj konfigurację z pliku *appSettings. JSON*, *appSettings. { Nazwa środowiska}. JSON*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="45c02-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="45c02-158">Wyślij dane wyjściowe rejestrowania do konsoli programu i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="45c02-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="45c02-159">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="45c02-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="45c02-160">Scenariusze inne niż internetowe</span><span class="sxs-lookup"><span data-stu-id="45c02-160">Non-web scenarios</span></span>

<span data-ttu-id="45c02-161">Host ogólny umożliwia innym typom aplikacji korzystanie z rozszerzeń struktury wycinania, takich jak rejestrowanie, iniekcja zależności (DI), konfiguracja i zarządzanie okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="45c02-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="45c02-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="45c02-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="45c02-163">Serwery</span><span class="sxs-lookup"><span data-stu-id="45c02-163">Servers</span></span>

<span data-ttu-id="45c02-164">Aplikacja ASP.NET Core używa implementacji serwera HTTP do nasłuchiwania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="45c02-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="45c02-165">Serwer wyświetla żądania do aplikacji jako zestaw [funkcji żądania](xref:fundamentals/request-features) złożonych do `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="45c02-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="45c02-166">Windows</span><span class="sxs-lookup"><span data-stu-id="45c02-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="45c02-167">ASP.NET Core udostępnia następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="45c02-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="45c02-168">*Kestrel* to Międzyplatformowy serwer sieci Web.</span><span class="sxs-lookup"><span data-stu-id="45c02-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="45c02-169">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="45c02-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="45c02-170">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="45c02-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="45c02-171">*Serwer http IIS* jest serwerem dla systemu Windows, który korzysta z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="45c02-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="45c02-172">Na tym serwerze aplikacja ASP.NET Core i usługi IIS działają w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="45c02-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="45c02-173">*Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="45c02-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="45c02-174">macOS</span><span class="sxs-lookup"><span data-stu-id="45c02-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="45c02-175">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="45c02-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="45c02-176">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="45c02-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="45c02-177">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="45c02-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="45c02-178">System</span><span class="sxs-lookup"><span data-stu-id="45c02-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="45c02-179">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="45c02-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="45c02-180">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="45c02-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="45c02-181">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="45c02-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="45c02-182">Windows</span><span class="sxs-lookup"><span data-stu-id="45c02-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="45c02-183">ASP.NET Core udostępnia następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="45c02-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="45c02-184">*Kestrel* to Międzyplatformowy serwer sieci Web.</span><span class="sxs-lookup"><span data-stu-id="45c02-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="45c02-185">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="45c02-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="45c02-186">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="45c02-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="45c02-187">*Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="45c02-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="45c02-188">macOS</span><span class="sxs-lookup"><span data-stu-id="45c02-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="45c02-189">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="45c02-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="45c02-190">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="45c02-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="45c02-191">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="45c02-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="45c02-192">System</span><span class="sxs-lookup"><span data-stu-id="45c02-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="45c02-193">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="45c02-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="45c02-194">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="45c02-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="45c02-195">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="45c02-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="45c02-196">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="45c02-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="45c02-197">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="45c02-197">Configuration</span></span>

<span data-ttu-id="45c02-198">ASP.NET Core udostępnia platformę konfiguracji, która pobiera ustawienia jako pary nazwa-wartość z uporządkowanego zestawu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="45c02-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="45c02-199">Istnieją Wbudowani dostawcy konfiguracji dla różnych źródeł, takich jak pliki *. JSON* , pliki *. XML* , zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="45c02-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="45c02-200">Możesz również napisać niestandardowych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="45c02-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="45c02-201">Można na przykład określić, że konfiguracja pochodzi z pliku *appSettings. JSON* i zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="45c02-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="45c02-202">Następnie po zażądaniu wartości parametru *ConnectionString* struktura najpierw sprawdza plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="45c02-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="45c02-203">Jeśli wartość jest tam znaleziona, ale również w zmiennej środowiskowej, pierwszeństwo ma wartość ze zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="45c02-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="45c02-204">Aby zarządzać poufnymi danymi konfiguracyjnymi, takimi jak hasła, ASP.NET Core zapewnia [Narzędzie tajnego Menedżera](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="45c02-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="45c02-205">W przypadku wpisów tajnych produkcji zalecamy [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="45c02-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="45c02-206">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="45c02-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="45c02-207">Opcje</span><span class="sxs-lookup"><span data-stu-id="45c02-207">Options</span></span>

<span data-ttu-id="45c02-208">Jeśli to możliwe, ASP.NET Core są zgodne ze *wzorcem opcji* przechowywania i pobierania wartości konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="45c02-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="45c02-209">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="45c02-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="45c02-210">Na przykład poniższy kod ustawia opcje obiektów WebSockets:</span><span class="sxs-lookup"><span data-stu-id="45c02-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="45c02-211">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="45c02-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="45c02-212">Wiejski</span><span class="sxs-lookup"><span data-stu-id="45c02-212">Environments</span></span>

<span data-ttu-id="45c02-213">Środowiska wykonawcze, takie jak *programowanie*, *przemieszczanie*i *produkcja*, są pierwszą klasą koncepcji w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="45c02-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="45c02-214">Aby określić środowisko, w którym działa aplikacja, należy ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="45c02-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="45c02-215">ASP.NET Core odczytuje tę zmienną środowiskową przy uruchamianiu aplikacji i zapisuje wartość w implementacji `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="45c02-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="45c02-216">Obiekt środowiska jest dostępny w dowolnym miejscu w aplikacji za pomocą funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="45c02-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="45c02-217">Następujący przykładowy kod z klasy `Startup` konfiguruje aplikację w celu dostarczania szczegółowych informacji o błędzie tylko wtedy, gdy są uruchamiane w programie Development:</span><span class="sxs-lookup"><span data-stu-id="45c02-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="45c02-218">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="45c02-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="45c02-219">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="45c02-219">Logging</span></span>

<span data-ttu-id="45c02-220">ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm.</span><span class="sxs-lookup"><span data-stu-id="45c02-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="45c02-221">Dostępne są następujące dostawcy:</span><span class="sxs-lookup"><span data-stu-id="45c02-221">Available providers include the following:</span></span>

* <span data-ttu-id="45c02-222">Konsola</span><span class="sxs-lookup"><span data-stu-id="45c02-222">Console</span></span>
* <span data-ttu-id="45c02-223">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="45c02-223">Debug</span></span>
* <span data-ttu-id="45c02-224">Śledzenie zdarzeń w systemie Windows</span><span class="sxs-lookup"><span data-stu-id="45c02-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="45c02-225">Dziennik zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="45c02-225">Windows Event Log</span></span>
* <span data-ttu-id="45c02-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="45c02-226">TraceSource</span></span>
* <span data-ttu-id="45c02-227">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="45c02-227">Azure App Service</span></span>
* <span data-ttu-id="45c02-228">Application Insights platformy Azure</span><span class="sxs-lookup"><span data-stu-id="45c02-228">Azure Application Insights</span></span>

<span data-ttu-id="45c02-229">Zapisuj dzienniki z dowolnego miejsca w kodzie aplikacji, pobierając `ILogger` obiekt z metod rejestrowania i wywoływania.</span><span class="sxs-lookup"><span data-stu-id="45c02-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="45c02-230">Poniżej przedstawiono przykładowy kod, który używa obiektu `ILogger`, z iniekcją konstruktora i wyróżniania wywołań metody rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="45c02-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="45c02-231">Interfejs `ILogger` umożliwia przekazanie dowolnej liczby pól dostawcy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="45c02-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="45c02-232">Pola są często używane do konstruowania ciągu komunikatu, ale dostawcy mogą również wysyłać je jako oddzielne pola do magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="45c02-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="45c02-233">Ta funkcja umożliwia dostawcom rejestrowania implementowanie [rejestrowania semantycznego, znanego również jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="45c02-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="45c02-234">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="45c02-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="45c02-235">Routing</span><span class="sxs-lookup"><span data-stu-id="45c02-235">Routing</span></span>

<span data-ttu-id="45c02-236">*Trasa* jest WZORCEM adresu URL, który jest mapowany do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="45c02-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="45c02-237">Procedura obsługi jest zazwyczaj stroną Razor, metodą akcji w kontrolerze MVC lub w oprogramowaniu pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="45c02-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="45c02-238">Routing ASP.NET Core zapewnia kontrolę nad adresami URL używanymi przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="45c02-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="45c02-239">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="45c02-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="45c02-240">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="45c02-240">Error handling</span></span>

<span data-ttu-id="45c02-241">ASP.NET Core ma wbudowane funkcje do obsługi błędów, takie jak:</span><span class="sxs-lookup"><span data-stu-id="45c02-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="45c02-242">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="45c02-242">A developer exception page</span></span>
* <span data-ttu-id="45c02-243">Niestandardowe strony błędów</span><span class="sxs-lookup"><span data-stu-id="45c02-243">Custom error pages</span></span>
* <span data-ttu-id="45c02-244">Statyczne strony kodów stanu</span><span class="sxs-lookup"><span data-stu-id="45c02-244">Static status code pages</span></span>
* <span data-ttu-id="45c02-245">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="45c02-245">Startup exception handling</span></span>

<span data-ttu-id="45c02-246">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="45c02-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="45c02-247">Zgłaszanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="45c02-247">Make HTTP requests</span></span>

<span data-ttu-id="45c02-248">Implementacja `IHttpClientFactory` jest dostępna do tworzenia wystąpień `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="45c02-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="45c02-249">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="45c02-249">The factory:</span></span>

* <span data-ttu-id="45c02-250">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="45c02-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="45c02-251">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="45c02-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="45c02-252">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="45c02-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="45c02-253">Obsługuje rejestrację i łańcuch wielu procedur delegowania, aby utworzyć potok pośredniczący żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="45c02-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="45c02-254">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="45c02-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="45c02-255">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="45c02-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="45c02-256">Integruje się z usługą *Polly*, popularną biblioteką innej firmy na potrzeby obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="45c02-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="45c02-257">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="45c02-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="45c02-258">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="45c02-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="45c02-259">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="45c02-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="45c02-260">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="45c02-260">Content root</span></span>

<span data-ttu-id="45c02-261">Katalog główny zawartości jest ścieżką podstawową do:</span><span class="sxs-lookup"><span data-stu-id="45c02-261">The content root is the base path to the:</span></span>

* <span data-ttu-id="45c02-262">Plik wykonywalny obsługujący aplikację ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="45c02-262">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="45c02-263">Skompilowane zestawy wchodzące w skład aplikacji ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="45c02-263">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="45c02-264">Pliki zawartości inne niż kod używane przez aplikację, takie jak:</span><span class="sxs-lookup"><span data-stu-id="45c02-264">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="45c02-265">Pliki Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="45c02-265">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="45c02-266">Pliki konfiguracji ( *. JSON*, *. XML*)</span><span class="sxs-lookup"><span data-stu-id="45c02-266">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="45c02-267">Pliki danych ( *. DB*)</span><span class="sxs-lookup"><span data-stu-id="45c02-267">Data files (*.db*)</span></span>
* <span data-ttu-id="45c02-268">[Katalog główny sieci Web](#web-root), zazwyczaj opublikowany folder *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="45c02-268">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="45c02-269">Podczas tworzenia:</span><span class="sxs-lookup"><span data-stu-id="45c02-269">During development:</span></span>

* <span data-ttu-id="45c02-270">Katalog główny zawartości domyślnie jest katalogiem głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="45c02-270">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="45c02-271">Katalog główny projektu służy do tworzenia:</span><span class="sxs-lookup"><span data-stu-id="45c02-271">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="45c02-272">Ścieżka do plików zawartości nienależących do kodu aplikacji w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="45c02-272">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="45c02-273">[Katalog główny sieci Web](#web-root), zazwyczaj folder *wwwroot* w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="45c02-273">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="45c02-274">Alternatywna ścieżka katalogu głównego zawartości może być określona podczas [kompilowania hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="45c02-274">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="45c02-275">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="45c02-275">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="45c02-276">Alternatywna ścieżka katalogu głównego zawartości może być określona podczas [kompilowania hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="45c02-276">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="45c02-277">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="45c02-277">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="45c02-278">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="45c02-278">Web root</span></span>

<span data-ttu-id="45c02-279">Katalog główny sieci Web jest ścieżką podstawową do publicznych, niekodowych, statycznych plików zasobów, takich jak:</span><span class="sxs-lookup"><span data-stu-id="45c02-279">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="45c02-280">Arkusze stylów ( *. css*)</span><span class="sxs-lookup"><span data-stu-id="45c02-280">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="45c02-281">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="45c02-281">JavaScript (*.js*)</span></span>
* <span data-ttu-id="45c02-282">Obrazy ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="45c02-282">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="45c02-283">Pliki statyczne są obsługiwane domyślnie tylko z katalogu głównego (i katalogów podrzędnych) sieci Web.</span><span class="sxs-lookup"><span data-stu-id="45c02-283">Static files are only served by default from the web root directory (and sub-directories).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="45c02-284">Ścieżka katalogu głównego sieci Web jest domyślnie ustawiona na *{Content root}/wwwroot*, ale podczas [kompilowania hosta](#host)można określić inny katalog internetowy w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="45c02-284">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="45c02-285">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="45c02-285">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="45c02-286">Ścieżka katalogu głównego sieci Web jest domyślnie ustawiona na *{Content root}/wwwroot*, ale podczas [kompilowania hosta](#host)można określić inny katalog internetowy w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="45c02-286">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="45c02-287">Aby uzyskać więcej informacji, zobacz [katalog główny sieci Web](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="45c02-287">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

::: moniker-end

<span data-ttu-id="45c02-288">Zapobiegaj publikowaniu plików w pliku *wwwroot* przy użyciu [elementu projektu\<Content >](/visualstudio/msbuild/common-msbuild-project-items#content) .</span><span class="sxs-lookup"><span data-stu-id="45c02-288">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="45c02-289">Poniższy przykład uniemożliwia opublikowanie zawartości w katalogu *wwwroot/lokalnym* i podkatalogach:</span><span class="sxs-lookup"><span data-stu-id="45c02-289">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="45c02-290">W plikach Razor ( *. cshtml*), ukośnik (`~/`) wskazuje na katalog główny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="45c02-290">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="45c02-291">Ścieżka rozpoczynająca się od `~/` jest nazywana *ścieżką wirtualną*.</span><span class="sxs-lookup"><span data-stu-id="45c02-291">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="45c02-292">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="45c02-292">For more information, see <xref:fundamentals/static-files>.</span></span>
