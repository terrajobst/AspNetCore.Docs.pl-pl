---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: a6c848987c97103864fd5410922346e85a68c353
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856233"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="978b8-103">Podstawy platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="978b8-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="978b8-104">W tym artykule przedstawiono kluczowe tematy zrozumieć, jak opracowywać aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="978b8-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="978b8-105">Klasa Startup</span><span class="sxs-lookup"><span data-stu-id="978b8-105">The Startup class</span></span>

<span data-ttu-id="978b8-106">`Startup` Klasy jest, gdy:</span><span class="sxs-lookup"><span data-stu-id="978b8-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="978b8-107">Skonfigurowano usługi wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="978b8-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="978b8-108">Żądanie obsługi potoku jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="978b8-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="978b8-109">*Usługi* przedstawiono składniki, które są używane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="978b8-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="978b8-110">Na przykład rejestrowanie składnik to usługa.</span><span class="sxs-lookup"><span data-stu-id="978b8-110">For example, a logging component is a service.</span></span> <span data-ttu-id="978b8-111">Kod, aby skonfigurować (lub *zarejestrować*) usługi jest dodawany do `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="978b8-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="978b8-112">Żądanie obsługi Potok składa się jako serię *oprogramowania pośredniczącego* składników.</span><span class="sxs-lookup"><span data-stu-id="978b8-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="978b8-113">Na przykład oprogramowanie pośredniczące może obsługiwać żądań dotyczących plików statycznych lub przekierowywanie żądań HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="978b8-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="978b8-114">Każdy oprogramowania pośredniczącego wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="978b8-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="978b8-115">Kod, aby skonfigurować żądanie obsługi potoku jest dodawany do `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="978b8-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="978b8-116">Poniżej znajduje się przykładowy `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="978b8-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="978b8-117">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="978b8-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="978b8-118">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="978b8-118">Dependency injection (services)</span></span>

<span data-ttu-id="978b8-119">ASP.NET Core ma framework iniekcji (DI) wbudowane zależności czy services sprawia, że skonfigurowano dostępne dla klasy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="978b8-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="978b8-120">Jednym ze sposobów, aby pobrać wystąpienie usługi w klasie jest tworzenie konstruktora przy użyciu parametru wymaganego typu.</span><span class="sxs-lookup"><span data-stu-id="978b8-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="978b8-121">Parametr może być typ usługi lub interfejs.</span><span class="sxs-lookup"><span data-stu-id="978b8-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="978b8-122">DI system oferuje usługi w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="978b8-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="978b8-123">W tym miejscu to klasa, która używa DI w celu uzyskania obiektu kontekstu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="978b8-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="978b8-124">Przykładem iniekcji konstruktora jest wyróżniony wiersz:</span><span class="sxs-lookup"><span data-stu-id="978b8-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="978b8-125">Gdy DI jest wbudowany, służy ona do umożliwiają podłączenie w kontenerze Inwersja kontroli (IoC) innych firm, jeśli użytkownik sobie tego życzy.</span><span class="sxs-lookup"><span data-stu-id="978b8-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="978b8-126">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="978b8-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="978b8-127">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="978b8-127">Middleware</span></span>

<span data-ttu-id="978b8-128">Żądanie obsługi Potok składa się jako serię składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="978b8-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="978b8-129">Każdy składnik wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="978b8-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="978b8-130">Zgodnie z Konwencją składnik oprogramowania pośredniczącego jest dodawane do potoku za pomocą jego `Use...` metody rozszerzenia w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="978b8-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="978b8-131">Na przykład, aby umożliwić renderowanie pliki statyczne, należy wywołać `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="978b8-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="978b8-132">Wyróżniony kod w poniższym przykładzie konfiguruje żądania obsługi potoku:</span><span class="sxs-lookup"><span data-stu-id="978b8-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="978b8-133">Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego i możesz napisać niestandardowego oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="978b8-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="978b8-134">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="978b8-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="978b8-135">Host</span><span class="sxs-lookup"><span data-stu-id="978b8-135">Host</span></span>

<span data-ttu-id="978b8-136">Tworzy aplikację ASP.NET Core *hosta* przy uruchamianiu.</span><span class="sxs-lookup"><span data-stu-id="978b8-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="978b8-137">Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="978b8-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="978b8-138">Implementację serwera HTTP</span><span class="sxs-lookup"><span data-stu-id="978b8-138">An HTTP server implementation</span></span>
* <span data-ttu-id="978b8-139">Składniki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="978b8-139">Middleware components</span></span>
* <span data-ttu-id="978b8-140">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="978b8-140">Logging</span></span>
* <span data-ttu-id="978b8-141">DI</span><span class="sxs-lookup"><span data-stu-id="978b8-141">DI</span></span>
* <span data-ttu-id="978b8-142">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="978b8-142">Configuration</span></span>

<span data-ttu-id="978b8-143">Głównym powodem wraz ze wszystkimi zasobami współzależne aplikacji w jeden obiekt jest zarządzanie okresem istnienia: kontrolę nad uruchamianiem aplikacji i łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="978b8-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="978b8-144">Dostępne są dwa hosty: ogólne hosta i hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="978b8-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="978b8-145">Ogólny hosta jest zalecana i hosta sieci Web jest dostępna tylko dla zapewnienia zgodności.</span><span class="sxs-lookup"><span data-stu-id="978b8-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="978b8-146">Kod w celu utworzenia hosta jest `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="978b8-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="978b8-147">`CreateDefaultBuilder` i `ConfigureWebHostDefaults` metody Konfigurowanie hosta z powszechnie używane opcje, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="978b8-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="978b8-148">Użyj [Kestrel](#servers) jako integracja sieci web serwera i Włącz usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="978b8-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="978b8-149">Konfiguracja obciążenia z *appsettings.json*, *appsettings. { Nazwa środowiska} .json*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="978b8-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="978b8-150">Wyślij rejestrowania danych wyjściowych do konsoli i debugowanie dostawców.</span><span class="sxs-lookup"><span data-stu-id="978b8-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="978b8-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="978b8-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="978b8-152">Dostępne są dwa hosty: Host sieci Web i ogólny hosta.</span><span class="sxs-lookup"><span data-stu-id="978b8-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="978b8-153">W programie ASP.NET Core 2.x, ogólny Host znajduje się tylko w scenariuszach bez sieci web.</span><span class="sxs-lookup"><span data-stu-id="978b8-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="978b8-154">Kod w celu utworzenia hosta jest `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="978b8-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="978b8-155">`CreateDefaultBuilder` Metoda konfiguruje hosta przy użyciu powszechnie używane opcje, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="978b8-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="978b8-156">Użyj [Kestrel](#servers) jako integracja sieci web serwera i Włącz usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="978b8-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="978b8-157">Konfiguracja obciążenia z *appsettings.json*, *appsettings. { Nazwa środowiska} .json*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="978b8-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="978b8-158">Wyślij rejestrowania danych wyjściowych do konsoli i debugowanie dostawców.</span><span class="sxs-lookup"><span data-stu-id="978b8-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="978b8-159">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="978b8-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="978b8-160">Scenariusze non-sieci web</span><span class="sxs-lookup"><span data-stu-id="978b8-160">Non-web scenarios</span></span>

<span data-ttu-id="978b8-161">Ogólny hosta umożliwia innych typów aplikacji, aby korzystać z kompleksowych framework rozszerzeń, takich jak rejestrowanie, wstrzykiwanie zależności (DI), Konfiguracja i zarządzanie okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="978b8-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="978b8-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="978b8-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="978b8-163">Serwery</span><span class="sxs-lookup"><span data-stu-id="978b8-163">Servers</span></span>

<span data-ttu-id="978b8-164">Aplikacji ASP.NET Core używa implementację serwera HTTP, aby nasłuchiwała żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="978b8-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="978b8-165">Żądania powierzchnie serwerów aplikacji jako zbiór [funkcje na żądanie](xref:fundamentals/request-features) składające się na `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="978b8-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="978b8-166">Windows</span><span class="sxs-lookup"><span data-stu-id="978b8-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="978b8-167">Platforma ASP.NET Core oferuje następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="978b8-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="978b8-168">*Kestrel* jest serwerem sieci web dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="978b8-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="978b8-169">Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="978b8-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="978b8-170">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="978b8-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="978b8-171">*Serwer HTTP IIS* jest serwerem dla systemu windows, która korzysta z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="978b8-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="978b8-172">Z tym serwerem aplikacji ASP.NET Core i usługi IIS uruchamiane w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="978b8-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="978b8-173">*Sterownik HTTP.sys* dotyczy serwera Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="978b8-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="978b8-174">macOS</span><span class="sxs-lookup"><span data-stu-id="978b8-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="978b8-175">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="978b8-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="978b8-176">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="978b8-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="978b8-177">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="978b8-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="978b8-178">Linux</span><span class="sxs-lookup"><span data-stu-id="978b8-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="978b8-179">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="978b8-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="978b8-180">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="978b8-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="978b8-181">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="978b8-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="978b8-182">Windows</span><span class="sxs-lookup"><span data-stu-id="978b8-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="978b8-183">Platforma ASP.NET Core oferuje następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="978b8-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="978b8-184">*Kestrel* jest serwerem sieci web dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="978b8-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="978b8-185">Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="978b8-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="978b8-186">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="978b8-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="978b8-187">*Sterownik HTTP.sys* dotyczy serwera Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="978b8-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="978b8-188">macOS</span><span class="sxs-lookup"><span data-stu-id="978b8-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="978b8-189">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="978b8-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="978b8-190">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="978b8-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="978b8-191">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="978b8-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="978b8-192">Linux</span><span class="sxs-lookup"><span data-stu-id="978b8-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="978b8-193">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="978b8-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="978b8-194">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="978b8-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="978b8-195">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="978b8-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="978b8-196">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="978b8-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="978b8-197">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="978b8-197">Configuration</span></span>

<span data-ttu-id="978b8-198">Platforma ASP.NET Core zapewnia środowisko konfiguracji, które pobiera ustawienia jako pary nazwa wartość z uporządkowany zestaw dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="978b8-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="978b8-199">Brak dostawców wbudowanych konfiguracji dla różnych źródeł, takich jak *.json* pliki, *.xml* plików, zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="978b8-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="978b8-200">Można także napisać dostawców konfiguracji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="978b8-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="978b8-201">Na przykład, można określić tę konfigurację pochodzi z *appsettings.json* i zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="978b8-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="978b8-202">Wówczas, gdy wartość *ConnectionString* jest wymagana, struktura wygląda pierwszy w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="978b8-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="978b8-203">Jeśli wartość znajduje się tam, ale także w zmiennej środowiskowej, wartość zmiennej środowiskowej wyższy priorytet.</span><span class="sxs-lookup"><span data-stu-id="978b8-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="978b8-204">Zarządzanie konfiguracji poufne dane, takie jak hasła, ASP.NET Core zapewnia [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="978b8-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="978b8-205">Wpisy tajne w środowisku produkcyjnym, zaleca się [usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="978b8-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="978b8-206">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="978b8-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="978b8-207">Opcje</span><span class="sxs-lookup"><span data-stu-id="978b8-207">Options</span></span>

<span data-ttu-id="978b8-208">Jeśli to możliwe, następujące platformy ASP.NET Core *wzorzec opcje* do przechowywania i pobierania wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="978b8-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="978b8-209">Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="978b8-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="978b8-210">Na przykład poniższy kod ustawia opcje WebSockets:</span><span class="sxs-lookup"><span data-stu-id="978b8-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="978b8-211">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="978b8-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="978b8-212">Środowiska</span><span class="sxs-lookup"><span data-stu-id="978b8-212">Environments</span></span>

<span data-ttu-id="978b8-213">Środowiska wykonawcze, takich jak *rozwoju*, *przemieszczania*, i *produkcji*, to najwyższej jakości pojęcie w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="978b8-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="978b8-214">Można określić, ustawiając działa w środowisku aplikacji `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="978b8-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="978b8-215">Platforma ASP.NET Core odczytuje tę zmienną przy uruchamianiu aplikacji i przechowuje wartość w `IHostingEnvironment` implementacji.</span><span class="sxs-lookup"><span data-stu-id="978b8-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="978b8-216">Obiekt środowiska jest dostępny w aplikacji za pośrednictwem DI w dowolnym miejscu.</span><span class="sxs-lookup"><span data-stu-id="978b8-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="978b8-217">Poniższy przykładowy kod z `Startup` klasy konfiguruje aplikację, aby zapewnić szczegółowe informacje o błędzie, tylko wtedy, gdy działa w trakcie opracowywania:</span><span class="sxs-lookup"><span data-stu-id="978b8-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="978b8-218">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="978b8-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="978b8-219">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="978b8-219">Logging</span></span>

<span data-ttu-id="978b8-220">Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców rejestrowania wbudowanych oraz innych firm.</span><span class="sxs-lookup"><span data-stu-id="978b8-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="978b8-221">Dostępni dostawcy są następujące:</span><span class="sxs-lookup"><span data-stu-id="978b8-221">Available providers include the following:</span></span>

* <span data-ttu-id="978b8-222">Konsola</span><span class="sxs-lookup"><span data-stu-id="978b8-222">Console</span></span>
* <span data-ttu-id="978b8-223">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="978b8-223">Debug</span></span>
* <span data-ttu-id="978b8-224">Śledzenie zdarzeń na Windows</span><span class="sxs-lookup"><span data-stu-id="978b8-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="978b8-225">Dziennik zdarzeń Windows</span><span class="sxs-lookup"><span data-stu-id="978b8-225">Windows Event Log</span></span>
* <span data-ttu-id="978b8-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="978b8-226">TraceSource</span></span>
* <span data-ttu-id="978b8-227">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="978b8-227">Azure App Service</span></span>
* <span data-ttu-id="978b8-228">Usługi Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="978b8-228">Azure Application Insights</span></span>

<span data-ttu-id="978b8-229">Zapis loguje się z dowolnego miejsca w kodzie aplikacji uzyskując `ILogger` obiekt z DI i wywoływania metod dziennika.</span><span class="sxs-lookup"><span data-stu-id="978b8-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="978b8-230">Poniżej przedstawiono przykładowy kod, który używa `ILogger` obiektu przy użyciu iniekcji konstruktora i wywołań metod rejestrowania, które są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="978b8-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="978b8-231">`ILogger` Interfejs pozwala przekazać dowolną liczbę pól do dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="978b8-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="978b8-232">Pola są często używane do konstruowania ciąg komunikatu, ale dostawca może także wysłać im jako oddzielne pola w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="978b8-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="978b8-233">Ta funkcja umożliwia rejestrowanie dostawców, aby zaimplementować [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="978b8-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="978b8-234">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="978b8-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="978b8-235">Routing</span><span class="sxs-lookup"><span data-stu-id="978b8-235">Routing</span></span>

<span data-ttu-id="978b8-236">A *trasy* jest wzorzec adresu URL, który jest mapowany do obsługi.</span><span class="sxs-lookup"><span data-stu-id="978b8-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="978b8-237">Program obsługi jest zwykle strony Razor, metody akcji kontrolera MVC lub oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="978b8-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="978b8-238">ASP.NET Core routingu zapewnia kontrolę nad adresów URL używanych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="978b8-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="978b8-239">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="978b8-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="978b8-240">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="978b8-240">Error handling</span></span>

<span data-ttu-id="978b8-241">Platforma ASP.NET Core ma wbudowane funkcje obsługi błędów, takich jak:</span><span class="sxs-lookup"><span data-stu-id="978b8-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="978b8-242">Stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="978b8-242">A developer exception page</span></span>
* <span data-ttu-id="978b8-243">Strony błędów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="978b8-243">Custom error pages</span></span>
* <span data-ttu-id="978b8-244">Stan statycznej strony kodowe</span><span class="sxs-lookup"><span data-stu-id="978b8-244">Static status code pages</span></span>
* <span data-ttu-id="978b8-245">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="978b8-245">Startup exception handling</span></span>

<span data-ttu-id="978b8-246">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="978b8-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="978b8-247">Zgłaszanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="978b8-247">Make HTTP requests</span></span>

<span data-ttu-id="978b8-248">Implementacja `IHttpClientFactory` jest dostępna dla tworzenia `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="978b8-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="978b8-249">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="978b8-249">The factory:</span></span>

* <span data-ttu-id="978b8-250">Stanowi centralną lokalizację do nazywania i konfigurowanie logiczne `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="978b8-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="978b8-251">Na przykład *github* klienta można zarejestrować i skonfigurowane do korzystania z usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="978b8-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="978b8-252">Domyślne klienta można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="978b8-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="978b8-253">Obsługuje rejestrację i tworzenie łańcucha wielu obsługi delegowania do tworzenia potoku oprogramowania pośredniczącego usługi wychodzące żądanie.</span><span class="sxs-lookup"><span data-stu-id="978b8-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="978b8-254">Wzorzec ten jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="978b8-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="978b8-255">Wzorzec zapewnia mechanizm zarządzania odciąż przekrojowe zagadnienia dotyczące żądań HTTP, w tym usługi pamięć podręczna obsługi serializacji i rejestrowania błędów.</span><span class="sxs-lookup"><span data-stu-id="978b8-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="978b8-256">Integruje się z *Polly*, popularne biblioteki innych firm dotyczące obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="978b8-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="978b8-257">Zarządza buforowanie i okresem istnienia bazowego `HttpClientMessageHandler` wystąpienia, aby uniknąć problemów DNS, które występują, gdy ręcznego zarządzania `HttpClient` okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="978b8-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="978b8-258">Dodanie obsługi można skonfigurować rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="978b8-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="978b8-259">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="978b8-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="978b8-260">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="978b8-260">Content root</span></span>

<span data-ttu-id="978b8-261">Główny zawartości jest ścieżka podstawowa do prywatnej zawartości używany przez aplikację, takie jak jego pliki Razor.</span><span class="sxs-lookup"><span data-stu-id="978b8-261">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="978b8-262">Domyślnie zawartość katalogu głównego jest podstawową ścieżkę dla pliku wykonywalnego hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="978b8-262">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="978b8-263">Być może alternatywną lokalizację określony podczas [tworzenia hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="978b8-263">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="978b8-264">Aby uzyskać więcej informacji, zobacz [zawartości głównego](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="978b8-264">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="978b8-265">Aby uzyskać więcej informacji, zobacz [zawartości głównego](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="978b8-265">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="978b8-266">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="978b8-266">Web root</span></span>

<span data-ttu-id="978b8-267">Katalog główny sieci web (znany także jako *webroot*) to ścieżka podstawowa do publicznej, statycznej zasobów, takich jak CSS, JavaScript i plików obrazów.</span><span class="sxs-lookup"><span data-stu-id="978b8-267">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="978b8-268">Oprogramowanie pośredniczące plików statycznych posłużą tylko pliki z katalogu głównego sieci web (i jego podkatalogi) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="978b8-268">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="978b8-269">Ścieżka katalogu głównego sieci web, wartość domyślna to *{głównego zawartości} / wwwroot*, ale może innej lokalizacji, należy określić podczas [tworzenia hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="978b8-269">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="978b8-270">W aparacie Razor ( *.cshtml*) plików ukośnika tylda `~/` wskazuje katalog główny sieci web.</span><span class="sxs-lookup"><span data-stu-id="978b8-270">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="978b8-271">Począwszy od ścieżki `~/` są określane jako ścieżek wirtualnych.</span><span class="sxs-lookup"><span data-stu-id="978b8-271">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="978b8-272">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="978b8-272">For more information, see <xref:fundamentals/static-files>.</span></span>
