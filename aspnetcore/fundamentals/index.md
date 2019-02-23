---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: 'Dowiedz się, podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.'
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/index
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="d6ee8-103">Podstawy platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6ee8-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="d6ee8-104">W tym artykule przedstawiono kluczowe tematy zrozumieć, jak opracowywać aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="d6ee8-105">Klasa początkowa</span><span class="sxs-lookup"><span data-stu-id="d6ee8-105">The Startup class</span></span>

<span data-ttu-id="d6ee8-106">`Startup` Klasy jest, gdy:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="d6ee8-107">Wszystkie wymagane przez aplikację usługi są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="d6ee8-108">Żądanie obsługi potoku jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="d6ee8-109">Kod, aby skonfigurować (lub *zarejestrować*) usługi jest dodawany do `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d6ee8-110">*Usługi* przedstawiono składniki, które są używane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="d6ee8-111">Na przykład obiekt kontekstu platformy Entity Framework Core to usługa.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="d6ee8-112">Kod, aby skonfigurować żądanie obsługi potoku jest dodawany do `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="d6ee8-113">Potok składa się jako serię *oprogramowania pośredniczącego* składników.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="d6ee8-114">Na przykład oprogramowanie pośredniczące może obsługiwać żądań dotyczących plików statycznych lub przekierowywanie żądań HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="d6ee8-115">Każdy oprogramowania pośredniczącego wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d6ee8-116">Poniżej znajduje się przykładowy `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

::: moniker-end

<span data-ttu-id="d6ee8-117">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-117">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="d6ee8-118">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="d6ee8-118">Dependency injection (services)</span></span>

<span data-ttu-id="d6ee8-119">ASP.NET Core ma framework iniekcji (DI) wbudowane zależności czy services sprawia, że skonfigurowano dostępne dla klasy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="d6ee8-120">Jednym ze sposobów, aby pobrać wystąpienie usługi w klasie jest tworzenie konstruktora przy użyciu parametru wymaganego typu.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="d6ee8-121">Parametr może być typ usługi lub interfejs.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="d6ee8-122">DI system oferuje usługi w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-122">The DI system provides the service at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d6ee8-123">W tym miejscu to klasa, która używa DI w celu uzyskania obiektu kontekstu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="d6ee8-124">Przykładem iniekcji konstruktora jest wyróżniony wiersz:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

::: moniker-end

<span data-ttu-id="d6ee8-125">Gdy DI jest wbudowany, służy ona do umożliwiają podłączenie w kontenerze Inwersja kontroli (IoC) innych firm, jeśli użytkownik sobie tego życzy.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="d6ee8-126">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-126">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="d6ee8-127">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="d6ee8-127">Middleware</span></span>

<span data-ttu-id="d6ee8-128">Żądanie obsługi Potok składa się jako serię składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="d6ee8-129">Każdy składnik wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="d6ee8-130">Zgodnie z Konwencją składnik oprogramowania pośredniczącego jest dodawane do potoku za pomocą jego `Use...` metody rozszerzenia w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="d6ee8-131">Na przykład, aby umożliwić renderowanie pliki statyczne, należy wywołać `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d6ee8-132">Wyróżniony kod w poniższym przykładzie konfiguruje żądania obsługi potoku:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

::: moniker-end

<span data-ttu-id="d6ee8-133">Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego i możesz napisać niestandardowego oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="d6ee8-134">Aby uzyskać więcej informacji, zobacz [oprogramowania pośredniczącego](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-134">For more information, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="d6ee8-135">Host</span><span class="sxs-lookup"><span data-stu-id="d6ee8-135">The host</span></span>

<span data-ttu-id="d6ee8-136">Tworzy aplikację ASP.NET Core *hosta* przy uruchamianiu.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="d6ee8-137">Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-137">The host is an object that encapsulates all of the the app's resources, such as:</span></span>

* <span data-ttu-id="d6ee8-138">Implementację serwera HTTP</span><span class="sxs-lookup"><span data-stu-id="d6ee8-138">An HTTP server implementation</span></span>
* <span data-ttu-id="d6ee8-139">Składniki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="d6ee8-139">Middleware components</span></span>
* <span data-ttu-id="d6ee8-140">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="d6ee8-140">Logging</span></span>
* <span data-ttu-id="d6ee8-141">DI</span><span class="sxs-lookup"><span data-stu-id="d6ee8-141">DI</span></span>
* <span data-ttu-id="d6ee8-142">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="d6ee8-142">Configuration</span></span>

<span data-ttu-id="d6ee8-143">Głównym powodem wraz ze wszystkimi zasobami współzależne aplikacji w jeden obiekt jest zarządzanie okresem istnienia: kontrolę nad uruchamianiem aplikacji i łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="d6ee8-144">Kod w celu utworzenia hosta jest `Program.Main` i następuje [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="d6ee8-145">Metody są wywoływane w celu skonfigurowania każdego zasobu należącego do hosta.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="d6ee8-146">Metoda Konstruktor jest wywoływana w celu wyciągniesz go razem i Utwórz wystąpienie obiektu hosta.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d6ee8-147">Platforma ASP.NET Core 2.x używa hosta sieci Web ( `WebHost` klasy) dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-147">ASP.NET Core 2.x uses Web Host (the `WebHost` class) for web apps.</span></span> <span data-ttu-id="d6ee8-148">Udostępnia platformę `CreateDefaultBuilder` metody rozszerzenia, które Konfigurowanie hosta z powszechnie używane opcje, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-148">The framework provides `CreateDefaultBuilder` extension methods that set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="d6ee8-149">Użyj [Kestrel](#servers) jako integracja sieci web serwera i Włącz usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-149">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="d6ee8-150">Konfiguracja obciążenia z *appsettings.json*, zmienne środowiskowe, argumenty wiersza polecenia i innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-150">Load configuration from *appsettings.json*, environment variables, command line arguments, and other sources.</span></span>
* <span data-ttu-id="d6ee8-151">Wyślij rejestrowania danych wyjściowych do konsoli i debugowanie dostawców.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-151">Send logging output to the console and debug providers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="d6ee8-152">Oto przykładowy kod, który tworzy hosta:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-152">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="d6ee8-153">Aby uzyskać więcej informacji, zobacz [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-153">For more information, see [Web Host](xref:fundamentals/host/web-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="d6ee8-154">W programie ASP.NET Core 3.0 to hosta sieci Web (`WebHost` klasy) lub ogólny hosta (`Host` klasy) może być używana w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-154">In ASP.NET Core 3.0, Web Host (`WebHost` class) or Generic Host (`Host` class) can be used in a web app.</span></span> <span data-ttu-id="d6ee8-155">Ogólny hosta jest zalecana i hosta sieci Web jest dostępna dla zapewnienia zgodności.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-155">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="d6ee8-156">Udostępnia platformę `CreateDefaultBuilder` i `ConfigureWebHostDefaults` metody rozszerzenia, które Konfigurowanie hosta z powszechnie używane opcje, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-156">The framework provides `CreateDefaultBuilder` and `ConfigureWebHostDefaults` extension methods that set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="d6ee8-157">Użyj [Kestrel](#servers) jako integracja sieci web serwera i Włącz usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-157">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="d6ee8-158">Konfiguracja obciążenia z *appsettings.json*, *appsettings. [ .Json EnvironmentName]*, zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-158">Load configuration from *appsettings.json*, *appsettings.[EnvironmentName].json*, environment variables, and command line arguments.</span></span>
* <span data-ttu-id="d6ee8-159">Wyślij rejestrowania danych wyjściowych do konsoli i debugowanie dostawców.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-159">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="d6ee8-160">Oto przykładowy kod, który tworzy hosta.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-160">Here's sample code that builds a host.</span></span> <span data-ttu-id="d6ee8-161">Metody rozszerzenia, które skonfigurować hosta z powszechnie używane opcje są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-161">The extension methods that set up the host with commonly used options are highlighted.</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="d6ee8-162">Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host) i [hosta sieci Web](xref:fundamentals/host/web-host)</span><span class="sxs-lookup"><span data-stu-id="d6ee8-162">For more information, see [Generic Host](xref:fundamentals/host/generic-host) and [Web Host](xref:fundamentals/host/web-host)</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="d6ee8-163">Scenariusze zaawansowane hosta</span><span class="sxs-lookup"><span data-stu-id="d6ee8-163">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="d6ee8-164">Host sieci Web jest przeznaczony do zawierać implementację serwera HTTP, który nie jest wymagany dla innych rodzajów aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-164">Web Host is designed to include an HTTP server implementation, which isn't needed for other kinds of .NET apps.</span></span> <span data-ttu-id="d6ee8-165">Począwszy od hosta ogólny 2.1 (`Host` klasy) jest dostępna dla dowolnej aplikacji platformy .NET Core użyć&mdash;nie tylko aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-165">Starting in 2.1, Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="d6ee8-166">Ogólny hosta pozwala korzystać z kompleksowych funkcji, takich jak rejestrowanie, zarządzanie okresem istnienia DI, konfiguracji i aplikacji, w przypadku innych typów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-166">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="d6ee8-167">Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-167">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="d6ee8-168">Ogólny Host jest dostępny dla dowolnej aplikacji platformy .NET Core użyć&mdash;nie tylko aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-168">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="d6ee8-169">Ogólny hosta pozwala korzystać z kompleksowych funkcji, takich jak rejestrowanie, zarządzanie okresem istnienia DI, konfiguracji i aplikacji, w przypadku innych typów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-169">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="d6ee8-170">Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-170">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

<span data-ttu-id="d6ee8-171">Host umożliwia również uruchamianie zadań w tle.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-171">You can also use the host to run background tasks.</span></span> <span data-ttu-id="d6ee8-172">Aby uzyskać więcej informacji, zobacz [zadania w tle](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-172">For more information, see [Background tasks](xref:fundamentals/host/hosted-services).</span></span>

## <a name="servers"></a><span data-ttu-id="d6ee8-173">Serwery</span><span class="sxs-lookup"><span data-stu-id="d6ee8-173">Servers</span></span>

<span data-ttu-id="d6ee8-174">Aplikacji ASP.NET Core używa implementację serwera HTTP, aby nasłuchiwała żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-174">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="d6ee8-175">Żądania powierzchnie serwerów aplikacji jako zbiór [funkcje na żądanie](xref:fundamentals/request-features) składające się na `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-175">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="d6ee8-176">Windows</span><span class="sxs-lookup"><span data-stu-id="d6ee8-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="d6ee8-177">Platforma ASP.NET Core oferuje następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-177">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="d6ee8-178">*Kestrel* jest serwerem sieci web dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-178">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="d6ee8-179">Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-179">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="d6ee8-180">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="d6ee8-181">*Serwer HTTP IIS* jest serwerem dla systemu windows, która korzysta z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-181">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="d6ee8-182">Z tym serwerem aplikacji ASP.NET Core i usługi IIS uruchamiane w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-182">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="d6ee8-183">*Sterownik HTTP.sys* dotyczy serwera Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-183">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d6ee8-184">macOS</span><span class="sxs-lookup"><span data-stu-id="d6ee8-184">macOS</span></span>](#tab/macos)

<span data-ttu-id="d6ee8-185">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-185">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d6ee8-186">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d6ee8-187">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-187">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d6ee8-188">Linux</span><span class="sxs-lookup"><span data-stu-id="d6ee8-188">Linux</span></span>](#tab/linux)

<span data-ttu-id="d6ee8-189">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d6ee8-190">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d6ee8-191">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="d6ee8-192">Windows</span><span class="sxs-lookup"><span data-stu-id="d6ee8-192">Windows</span></span>](#tab/windows)

<span data-ttu-id="d6ee8-193">Platforma ASP.NET Core oferuje następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-193">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="d6ee8-194">*Kestrel* jest serwerem sieci web dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-194">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="d6ee8-195">Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-195">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="d6ee8-196">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-196">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="d6ee8-197">*Sterownik HTTP.sys* dotyczy serwera Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-197">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d6ee8-198">macOS</span><span class="sxs-lookup"><span data-stu-id="d6ee8-198">macOS</span></span>](#tab/macos)

<span data-ttu-id="d6ee8-199">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-199">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d6ee8-200">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-200">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d6ee8-201">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-201">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d6ee8-202">Linux</span><span class="sxs-lookup"><span data-stu-id="d6ee8-202">Linux</span></span>](#tab/linux)

<span data-ttu-id="d6ee8-203">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-203">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d6ee8-204">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-204">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d6ee8-205">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](http://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-205">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="d6ee8-206">Aby uzyskać więcej informacji, zobacz [serwerów](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-206">For more information, see [Servers](xref:fundamentals/servers/index).</span></span>

## <a name="configuration"></a><span data-ttu-id="d6ee8-207">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="d6ee8-207">Configuration</span></span>

<span data-ttu-id="d6ee8-208">Platforma ASP.NET Core zapewnia środowisko konfiguracji, które pobiera ustawienia jako pary nazwa wartość z uporządkowany zestaw dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-208">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="d6ee8-209">Brak dostawców wbudowanych konfiguracji dla różnych źródeł, takich jak *.json* pliki, *.xml* plików, zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-209">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="d6ee8-210">Można także napisać dostawców konfiguracji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-210">You can also write custom configuration providers.</span></span>

<span data-ttu-id="d6ee8-211">Na przykład, można określić tę konfigurację pochodzi z *appsettings.json* i zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-211">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="d6ee8-212">Wówczas, gdy wartość *ConnectionString* jest wymagana, struktura wygląda pierwszy w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-212">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="d6ee8-213">Jeśli wartość znajduje się tam, ale także w zmiennej środowiskowej, wartość zmiennej środowiskowej wyższy priorytet.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-213">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="d6ee8-214">Zarządzanie konfiguracji poufne dane, takie jak hasła, ASP.NET Core zapewnia [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-214">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d6ee8-215">Wpisy tajne w środowisku produkcyjnym, zaleca się [usługi Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-215">For production secrets, we recommend [Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span></span>

<span data-ttu-id="d6ee8-216">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-216">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="options"></a><span data-ttu-id="d6ee8-217">Opcje</span><span class="sxs-lookup"><span data-stu-id="d6ee8-217">Options</span></span>

<span data-ttu-id="d6ee8-218">Jeśli to możliwe, następujące platformy ASP.NET Core *wzorzec opcje* do przechowywania i pobierania wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-218">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="d6ee8-219">Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-219">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="d6ee8-220">Na przykład poniższy kod ustawia opcje WebSockets:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-220">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="d6ee8-221">Aby uzyskać więcej informacji, zobacz [opcje](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-221">For more information, see [Options](xref:fundamentals/configuration/options).</span></span>

## <a name="environments"></a><span data-ttu-id="d6ee8-222">Środowiska</span><span class="sxs-lookup"><span data-stu-id="d6ee8-222">Environments</span></span>

<span data-ttu-id="d6ee8-223">Środowiska wykonawcze, takich jak *rozwoju*, *przemieszczania*, i *produkcji*, to najwyższej jakości pojęcie w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-223">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="d6ee8-224">Można określić, ustawiając działa w środowisku aplikacji `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-224">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d6ee8-225">Platforma ASP.NET Core odczytuje tę zmienną przy uruchamianiu aplikacji i przechowuje wartość w `IHostingEnvironment` implementacji.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-225">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="d6ee8-226">Obiekt środowiska jest dostępny w aplikacji za pośrednictwem DI w dowolnym miejscu.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-226">The environment object is available anywhere in the app via DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d6ee8-227">Poniższy przykładowy kod z `Startup` klasy konfiguruje aplikację, aby zapewnić szczegółowe informacje o błędzie, tylko wtedy, gdy działa w trakcie opracowywania:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-227">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

::: moniker-end

<span data-ttu-id="d6ee8-228">Aby uzyskać więcej informacji, zobacz [środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-228">For more information, see [Environments](xref:fundamentals/environments).</span></span>

## <a name="logging"></a><span data-ttu-id="d6ee8-229">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="d6ee8-229">Logging</span></span>

<span data-ttu-id="d6ee8-230">Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców rejestrowania wbudowanych oraz innych firm.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-230">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="d6ee8-231">Dostępni dostawcy są następujące:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-231">Available providers include the following:</span></span>

* <span data-ttu-id="d6ee8-232">Konsola</span><span class="sxs-lookup"><span data-stu-id="d6ee8-232">Console</span></span>
* <span data-ttu-id="d6ee8-233">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="d6ee8-233">Debug</span></span>
* <span data-ttu-id="d6ee8-234">Śledzenie zdarzeń na Windows</span><span class="sxs-lookup"><span data-stu-id="d6ee8-234">Event Tracing on Windows</span></span>
* <span data-ttu-id="d6ee8-235">Dziennik zdarzeń Windows</span><span class="sxs-lookup"><span data-stu-id="d6ee8-235">Windows Event Log</span></span>
* <span data-ttu-id="d6ee8-236">TraceSource</span><span class="sxs-lookup"><span data-stu-id="d6ee8-236">TraceSource</span></span>
* <span data-ttu-id="d6ee8-237">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d6ee8-237">Azure App Service</span></span>
* <span data-ttu-id="d6ee8-238">Usługi Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="d6ee8-238">Azure Application Insights</span></span>

<span data-ttu-id="d6ee8-239">Zapis loguje się z dowolnego miejsca w kodzie aplikacji uzyskując `ILogger` obiekt z DI i wywoływania metod dziennika.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-239">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d6ee8-240">Poniżej przedstawiono przykładowy kod, który używa `ILogger` obiektu przy użyciu iniekcji konstruktora i wywołań metod rejestrowania, które są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-240">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

::: moniker-end

<span data-ttu-id="d6ee8-241">`ILogger` Interfejs pozwala przekazać dowolną liczbę pól do dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-241">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="d6ee8-242">Pola są często używane do konstruowania ciąg komunikatu, ale dostawca może także wysłać im jako oddzielne pola w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-242">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="d6ee8-243">Ta funkcja umożliwia rejestrowanie dostawców, aby zaimplementować [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-243">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="d6ee8-244">Aby uzyskać więcej informacji, zobacz [rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-244">For more information, see [Logging](xref:fundamentals/logging/index).</span></span>

## <a name="routing"></a><span data-ttu-id="d6ee8-245">Routing</span><span class="sxs-lookup"><span data-stu-id="d6ee8-245">Routing</span></span>

<span data-ttu-id="d6ee8-246">A *trasy* jest wzorzec adresu URL, który jest mapowany do obsługi.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-246">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="d6ee8-247">Program obsługi jest zwykle strony Razor, metody akcji kontrolera MVC lub oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-247">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="d6ee8-248">ASP.NET Core routingu zapewnia kontrolę nad adresów URL używanych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-248">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="d6ee8-249">Aby uzyskać więcej informacji, zobacz [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-249">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="error-handling"></a><span data-ttu-id="d6ee8-250">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="d6ee8-250">Error handling</span></span>

<span data-ttu-id="d6ee8-251">Platforma ASP.NET Core ma wbudowane funkcje obsługi błędów, takich jak:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-251">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="d6ee8-252">Stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="d6ee8-252">A developer exception page</span></span>
* <span data-ttu-id="d6ee8-253">Strony błędów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="d6ee8-253">Custom error pages</span></span>
* <span data-ttu-id="d6ee8-254">Stan statycznej strony kodowe</span><span class="sxs-lookup"><span data-stu-id="d6ee8-254">Static status code pages</span></span>
* <span data-ttu-id="d6ee8-255">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="d6ee8-255">Startup exception handling</span></span>

<span data-ttu-id="d6ee8-256">Aby uzyskać więcej informacji, zobacz [obsługi błędów](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-256">For more information, see [Error handling](xref:fundamentals/error-handling).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="make-http-requests"></a><span data-ttu-id="d6ee8-257">Zgłaszanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="d6ee8-257">Make HTTP requests</span></span>

<span data-ttu-id="d6ee8-258">Implementacja `IHttpClientFactory` jest dostępna dla tworzenia `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-258">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="d6ee8-259">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="d6ee8-259">The factory:</span></span>

* <span data-ttu-id="d6ee8-260">Stanowi centralną lokalizację do nazywania i konfigurowanie logiczne `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-260">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d6ee8-261">Na przykład *github* klienta można zarejestrować i skonfigurowane do korzystania z usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-261">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="d6ee8-262">Domyślne klienta można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-262">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="d6ee8-263">Obsługuje rejestrację i tworzenie łańcucha wielu obsługi delegowania do tworzenia potoku oprogramowania pośredniczącego usługi wychodzące żądanie.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-263">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d6ee8-264">Wzorzec ten jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-264">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="d6ee8-265">Wzorzec zapewnia mechanizm zarządzania odciąż przekrojowe zagadnienia dotyczące żądań HTTP, w tym usługi pamięć podręczna obsługi serializacji i rejestrowania błędów.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-265">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="d6ee8-266">Integruje się z *Polly*, popularne biblioteki innych firm dotyczące obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-266">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="d6ee8-267">Zarządza buforowanie i okresem istnienia bazowego `HttpClientMessageHandler` wystąpienia, aby uniknąć problemów DNS, które występują, gdy ręcznego zarządzania `HttpClient` okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-267">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d6ee8-268">Dodanie obsługi można skonfigurować rejestrowania (za pośrednictwem *ILogger*) dla wszystkich żądań wysłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-268">Adds a configurable logging experience (via *ILogger*) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d6ee8-269">Aby uzyskać więcej informacji, zobacz [żądań HTTP należy](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-269">For more information, see [Make HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="content-root"></a><span data-ttu-id="d6ee8-270">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="d6ee8-270">Content root</span></span>

<span data-ttu-id="d6ee8-271">Główny zawartości jest ścieżka podstawowa do prywatnej zawartości używany przez aplikację, takie jak jego pliki Razor.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-271">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="d6ee8-272">Domyślnie zawartość katalogu głównego jest podstawową ścieżkę dla pliku wykonywalnego hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-272">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="d6ee8-273">Być może alternatywną lokalizację określony podczas [tworzenia hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-273">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d6ee8-274">Aby uzyskać więcej informacji, zobacz [zawartości głównego](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-274">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="d6ee8-275">Aby uzyskać więcej informacji, zobacz [zawartości głównego](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-275">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="d6ee8-276">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="d6ee8-276">Web root</span></span>

<span data-ttu-id="d6ee8-277">Katalog główny sieci web (znany także jako *webroot*) to ścieżka podstawowa do publicznej, statycznej zasobów, takich jak CSS, JavaScript i plików obrazów.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-277">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="d6ee8-278">Oprogramowanie pośredniczące plików statycznych posłużą tylko pliki z katalogu głównego sieci web (i jego podkatalogi) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-278">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="d6ee8-279">Wartością domyślną jest ścieżka do katalogu głównego sieci web  *\<zawartości główny > / wwwroot*, ale innej lokalizacji może zostać określone, jeśli [tworzenia hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-279">The web root path defaults to *\<content root>/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="d6ee8-280">W aparacie Razor (*.cshtml*) plików ukośnika tylda `~/` wskazuje katalog główny sieci web.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-280">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="d6ee8-281">Począwszy od ścieżki `~/` są określane jako ścieżek wirtualnych.</span><span class="sxs-lookup"><span data-stu-id="d6ee8-281">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="d6ee8-282">Aby uzyskać więcej informacji, zobacz [pliki statyczne](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="d6ee8-282">For more information, see [Static files](xref:fundamentals/static-files).</span></span>
