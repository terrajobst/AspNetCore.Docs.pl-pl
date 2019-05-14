---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: 9c7bc25d813ad17825ef03f5176882993cc2dd63
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610331"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="ce297-103">Podstawy platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce297-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="ce297-104">W tym artykule przedstawiono kluczowe tematy zrozumieć, jak opracowywać aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce297-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="ce297-105">Klasa Startup</span><span class="sxs-lookup"><span data-stu-id="ce297-105">The Startup class</span></span>

<span data-ttu-id="ce297-106">`Startup` Klasy jest, gdy:</span><span class="sxs-lookup"><span data-stu-id="ce297-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="ce297-107">Wszystkie wymagane przez aplikację usługi są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="ce297-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="ce297-108">Żądanie obsługi potoku jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="ce297-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="ce297-109">Kod, aby skonfigurować (lub *zarejestrować*) usługi jest dodawany do `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="ce297-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ce297-110">*Usługi* przedstawiono składniki, które są używane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="ce297-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="ce297-111">Na przykład obiekt kontekstu platformy Entity Framework Core to usługa.</span><span class="sxs-lookup"><span data-stu-id="ce297-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="ce297-112">Kod, aby skonfigurować żądanie obsługi potoku jest dodawany do `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="ce297-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="ce297-113">Potok składa się jako serię *oprogramowania pośredniczącego* składników.</span><span class="sxs-lookup"><span data-stu-id="ce297-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="ce297-114">Na przykład oprogramowanie pośredniczące może obsługiwać żądań dotyczących plików statycznych lub przekierowywanie żądań HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ce297-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="ce297-115">Każdy oprogramowania pośredniczącego wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="ce297-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="ce297-116">Poniżej znajduje się przykładowy `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="ce297-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="ce297-117">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ce297-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="ce297-118">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="ce297-118">Dependency injection (services)</span></span>

<span data-ttu-id="ce297-119">ASP.NET Core ma framework iniekcji (DI) wbudowane zależności czy services sprawia, że skonfigurowano dostępne dla klasy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce297-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="ce297-120">Jednym ze sposobów, aby pobrać wystąpienie usługi w klasie jest tworzenie konstruktora przy użyciu parametru wymaganego typu.</span><span class="sxs-lookup"><span data-stu-id="ce297-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="ce297-121">Parametr może być typ usługi lub interfejs.</span><span class="sxs-lookup"><span data-stu-id="ce297-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="ce297-122">DI system oferuje usługi w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ce297-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="ce297-123">W tym miejscu to klasa, która używa DI w celu uzyskania obiektu kontekstu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ce297-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="ce297-124">Przykładem iniekcji konstruktora jest wyróżniony wiersz:</span><span class="sxs-lookup"><span data-stu-id="ce297-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="ce297-125">Gdy DI jest wbudowany, służy ona do umożliwiają podłączenie w kontenerze Inwersja kontroli (IoC) innych firm, jeśli użytkownik sobie tego życzy.</span><span class="sxs-lookup"><span data-stu-id="ce297-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="ce297-126">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ce297-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="ce297-127">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="ce297-127">Middleware</span></span>

<span data-ttu-id="ce297-128">Żądanie obsługi Potok składa się jako serię składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="ce297-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="ce297-129">Każdy składnik wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="ce297-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="ce297-130">Zgodnie z Konwencją składnik oprogramowania pośredniczącego jest dodawane do potoku za pomocą jego `Use...` metody rozszerzenia w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="ce297-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="ce297-131">Na przykład, aby umożliwić renderowanie pliki statyczne, należy wywołać `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="ce297-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="ce297-132">Wyróżniony kod w poniższym przykładzie konfiguruje żądania obsługi potoku:</span><span class="sxs-lookup"><span data-stu-id="ce297-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="ce297-133">Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego i możesz napisać niestandardowego oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="ce297-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="ce297-134">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="ce297-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="ce297-135">Host</span><span class="sxs-lookup"><span data-stu-id="ce297-135">The host</span></span>

<span data-ttu-id="ce297-136">Tworzy aplikację ASP.NET Core *hosta* przy uruchamianiu.</span><span class="sxs-lookup"><span data-stu-id="ce297-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="ce297-137">Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="ce297-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="ce297-138">Implementację serwera HTTP</span><span class="sxs-lookup"><span data-stu-id="ce297-138">An HTTP server implementation</span></span>
* <span data-ttu-id="ce297-139">Składniki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="ce297-139">Middleware components</span></span>
* <span data-ttu-id="ce297-140">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="ce297-140">Logging</span></span>
* <span data-ttu-id="ce297-141">DI</span><span class="sxs-lookup"><span data-stu-id="ce297-141">DI</span></span>
* <span data-ttu-id="ce297-142">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="ce297-142">Configuration</span></span>

<span data-ttu-id="ce297-143">Głównym powodem wraz ze wszystkimi zasobami współzależne aplikacji w jeden obiekt jest zarządzanie okresem istnienia: kontrolę nad uruchamianiem aplikacji i łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="ce297-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="ce297-144">Kod w celu utworzenia hosta jest `Program.Main` i następuje [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="ce297-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="ce297-145">Metody są wywoływane w celu skonfigurowania każdego zasobu należącego do hosta.</span><span class="sxs-lookup"><span data-stu-id="ce297-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="ce297-146">Metoda Konstruktor jest wywoływana w celu wyciągniesz go razem i Utwórz wystąpienie obiektu hosta.</span><span class="sxs-lookup"><span data-stu-id="ce297-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce297-147">`CreateHostBuilder` jest nazwą specjalną, który identyfikuje metodę konstruktora do składników zewnętrznych, takich jak [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="ce297-147">`CreateHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="ce297-148">W ASP.NET Core 3.0 lub nowszej, ogólne hosta (`Host` klasy) lub hosta sieci Web (`WebHost` klasy) może być używana w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ce297-148">In ASP.NET Core 3.0 or later, Generic Host (`Host` class) or Web Host (`WebHost` class) can be used in a web app.</span></span> <span data-ttu-id="ce297-149">Ogólny hosta jest zalecana i hosta sieci Web jest dostępna dla zapewnienia zgodności.</span><span class="sxs-lookup"><span data-stu-id="ce297-149">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="ce297-150">Udostępnia platformę `CreateDefaultBuilder` i `ConfigureWebHostDefaults` metody, aby skonfigurować hosta z powszechnie używane opcje, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="ce297-150">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="ce297-151">Użyj [Kestrel](#servers) jako integracja sieci web serwera i Włącz usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="ce297-151">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="ce297-152">Konfiguracja obciążenia z *appsettings.json*, *appsettings. { Nazwa środowiska} .json*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ce297-152">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="ce297-153">Wyślij rejestrowania danych wyjściowych do konsoli i debugowanie dostawców.</span><span class="sxs-lookup"><span data-stu-id="ce297-153">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="ce297-154">Oto przykładowy kod, który tworzy hosta.</span><span class="sxs-lookup"><span data-stu-id="ce297-154">Here's sample code that builds a host.</span></span> <span data-ttu-id="ce297-155">Metody, które skonfigurować hosta z powszechnie używane opcje są wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="ce297-155">The methods that set up the host with commonly used options are highlighted:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="ce297-156">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="ce297-156">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce297-157">`CreateWebHostBuilder` jest nazwą specjalną, który identyfikuje metodę konstruktora do składników zewnętrznych, takich jak [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="ce297-157">`CreateWebHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="ce297-158">Platforma ASP.NET Core 2.x używa hosta sieci Web (`WebHost` klasy) dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ce297-158">ASP.NET Core 2.x uses Web Host (`WebHost` class) for web apps.</span></span> <span data-ttu-id="ce297-159">Udostępnia platformę `CreateDefaultBuilder` skonfigurować hosta z powszechnie używane opcje, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="ce297-159">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="ce297-160">Użyj [Kestrel](#servers) jako integracja sieci web serwera i Włącz usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="ce297-160">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="ce297-161">Konfiguracja obciążenia z *appsettings.json*, *appsettings. { Nazwa środowiska} .json*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ce297-161">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="ce297-162">Wyślij rejestrowania danych wyjściowych do konsoli i debugowanie dostawców.</span><span class="sxs-lookup"><span data-stu-id="ce297-162">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="ce297-163">Oto przykładowy kod, który tworzy hosta:</span><span class="sxs-lookup"><span data-stu-id="ce297-163">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="ce297-164">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="ce297-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="ce297-165">Scenariusze zaawansowane hosta</span><span class="sxs-lookup"><span data-stu-id="ce297-165">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce297-166">Ogólny Host jest dostępny dla dowolnej aplikacji platformy .NET Core użyć&mdash;nie tylko aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce297-166">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="ce297-167">Ogólny hosta (`Host` klasy) umożliwia innych typów aplikacji, aby korzystać z kompleksowych framework rozszerzeń, takich jak zarządzanie okresem istnienia rejestrowania, DI, konfiguracji i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce297-167">Generic Host (`Host` class) allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="ce297-168">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ce297-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce297-169">Host sieci Web zaprojektowano do uwzględnienia implementację serwera HTTP, który nie jest wymagane dla innych typów aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="ce297-169">Web Host is designed to include an HTTP server implementation, which isn't required for other kinds of .NET apps.</span></span> <span data-ttu-id="ce297-170">Począwszy od platformy ASP.NET Core 2.1, ogólny hosta (`Host` klasy) jest dostępna dla dowolnej aplikacji platformy .NET Core użyć&mdash;nie tylko aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce297-170">Starting in ASP.NET Core 2.1, the Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="ce297-171">Ogólny hosta umożliwia innych typów aplikacji, aby korzystać z kompleksowych framework rozszerzeń, takich jak zarządzanie okresem istnienia rejestrowania, DI, konfiguracji i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce297-171">Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="ce297-172">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ce297-172">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

<span data-ttu-id="ce297-173">Host umożliwia również uruchamianie zadań w tle.</span><span class="sxs-lookup"><span data-stu-id="ce297-173">You can also use the host to run background tasks.</span></span> <span data-ttu-id="ce297-174">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="ce297-174">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="ce297-175">Serwery</span><span class="sxs-lookup"><span data-stu-id="ce297-175">Servers</span></span>

<span data-ttu-id="ce297-176">Aplikacji ASP.NET Core używa implementację serwera HTTP, aby nasłuchiwała żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce297-176">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="ce297-177">Żądania powierzchnie serwerów aplikacji jako zbiór [funkcje na żądanie](xref:fundamentals/request-features) składające się na `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="ce297-177">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="ce297-178">Windows</span><span class="sxs-lookup"><span data-stu-id="ce297-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="ce297-179">Platforma ASP.NET Core oferuje następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="ce297-179">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="ce297-180">*Kestrel* jest serwerem sieci web dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="ce297-180">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="ce297-181">Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ce297-181">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="ce297-182">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="ce297-182">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="ce297-183">*Serwer HTTP IIS* jest serwerem dla systemu windows, która korzysta z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ce297-183">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="ce297-184">Z tym serwerem aplikacji ASP.NET Core i usługi IIS uruchamiane w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="ce297-184">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="ce297-185">*Sterownik HTTP.sys* dotyczy serwera Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="ce297-185">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ce297-186">macOS</span><span class="sxs-lookup"><span data-stu-id="ce297-186">macOS</span></span>](#tab/macos)

<span data-ttu-id="ce297-187">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="ce297-187">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="ce297-188">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="ce297-188">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="ce297-189">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ce297-189">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ce297-190">Linux</span><span class="sxs-lookup"><span data-stu-id="ce297-190">Linux</span></span>](#tab/linux)

<span data-ttu-id="ce297-191">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="ce297-191">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="ce297-192">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="ce297-192">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="ce297-193">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ce297-193">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="ce297-194">Windows</span><span class="sxs-lookup"><span data-stu-id="ce297-194">Windows</span></span>](#tab/windows)

<span data-ttu-id="ce297-195">Platforma ASP.NET Core oferuje następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="ce297-195">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="ce297-196">*Kestrel* jest serwerem sieci web dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="ce297-196">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="ce297-197">Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ce297-197">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="ce297-198">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="ce297-198">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="ce297-199">*Sterownik HTTP.sys* dotyczy serwera Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="ce297-199">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ce297-200">macOS</span><span class="sxs-lookup"><span data-stu-id="ce297-200">macOS</span></span>](#tab/macos)

<span data-ttu-id="ce297-201">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="ce297-201">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="ce297-202">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="ce297-202">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="ce297-203">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ce297-203">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ce297-204">Linux</span><span class="sxs-lookup"><span data-stu-id="ce297-204">Linux</span></span>](#tab/linux)

<span data-ttu-id="ce297-205">ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform.</span><span class="sxs-lookup"><span data-stu-id="ce297-205">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="ce297-206">ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.</span><span class="sxs-lookup"><span data-stu-id="ce297-206">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="ce297-207">Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](http://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ce297-207">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="ce297-208">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="ce297-208">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="ce297-209">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="ce297-209">Configuration</span></span>

<span data-ttu-id="ce297-210">Platforma ASP.NET Core zapewnia środowisko konfiguracji, które pobiera ustawienia jako pary nazwa wartość z uporządkowany zestaw dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ce297-210">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="ce297-211">Brak dostawców wbudowanych konfiguracji dla różnych źródeł, takich jak *.json* pliki, *.xml* plików, zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="ce297-211">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="ce297-212">Można także napisać dostawców konfiguracji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="ce297-212">You can also write custom configuration providers.</span></span>

<span data-ttu-id="ce297-213">Na przykład, można określić tę konfigurację pochodzi z *appsettings.json* i zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="ce297-213">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="ce297-214">Wówczas, gdy wartość *ConnectionString* jest wymagana, struktura wygląda pierwszy w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="ce297-214">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="ce297-215">Jeśli wartość znajduje się tam, ale także w zmiennej środowiskowej, wartość zmiennej środowiskowej wyższy priorytet.</span><span class="sxs-lookup"><span data-stu-id="ce297-215">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="ce297-216">Zarządzanie konfiguracji poufne dane, takie jak hasła, ASP.NET Core zapewnia [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="ce297-216">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="ce297-217">Wpisy tajne w środowisku produkcyjnym, zaleca się [usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="ce297-217">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="ce297-218">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ce297-218">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="ce297-219">Opcje</span><span class="sxs-lookup"><span data-stu-id="ce297-219">Options</span></span>

<span data-ttu-id="ce297-220">Jeśli to możliwe, następujące platformy ASP.NET Core *wzorzec opcje* do przechowywania i pobierania wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ce297-220">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="ce297-221">Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="ce297-221">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="ce297-222">Na przykład poniższy kod ustawia opcje WebSockets:</span><span class="sxs-lookup"><span data-stu-id="ce297-222">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="ce297-223">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="ce297-223">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="ce297-224">Środowiska</span><span class="sxs-lookup"><span data-stu-id="ce297-224">Environments</span></span>

<span data-ttu-id="ce297-225">Środowiska wykonawcze, takich jak *rozwoju*, *przemieszczania*, i *produkcji*, to najwyższej jakości pojęcie w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce297-225">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="ce297-226">Można określić, ustawiając działa w środowisku aplikacji `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="ce297-226">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="ce297-227">Platforma ASP.NET Core odczytuje tę zmienną przy uruchamianiu aplikacji i przechowuje wartość w `IHostingEnvironment` implementacji.</span><span class="sxs-lookup"><span data-stu-id="ce297-227">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="ce297-228">Obiekt środowiska jest dostępny w aplikacji za pośrednictwem DI w dowolnym miejscu.</span><span class="sxs-lookup"><span data-stu-id="ce297-228">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="ce297-229">Poniższy przykładowy kod z `Startup` klasy konfiguruje aplikację, aby zapewnić szczegółowe informacje o błędzie, tylko wtedy, gdy działa w trakcie opracowywania:</span><span class="sxs-lookup"><span data-stu-id="ce297-229">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="ce297-230">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ce297-230">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="ce297-231">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="ce297-231">Logging</span></span>

<span data-ttu-id="ce297-232">Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców rejestrowania wbudowanych oraz innych firm.</span><span class="sxs-lookup"><span data-stu-id="ce297-232">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="ce297-233">Dostępni dostawcy są następujące:</span><span class="sxs-lookup"><span data-stu-id="ce297-233">Available providers include the following:</span></span>

* <span data-ttu-id="ce297-234">Konsola</span><span class="sxs-lookup"><span data-stu-id="ce297-234">Console</span></span>
* <span data-ttu-id="ce297-235">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="ce297-235">Debug</span></span>
* <span data-ttu-id="ce297-236">Śledzenie zdarzeń na Windows</span><span class="sxs-lookup"><span data-stu-id="ce297-236">Event Tracing on Windows</span></span>
* <span data-ttu-id="ce297-237">Dziennik zdarzeń Windows</span><span class="sxs-lookup"><span data-stu-id="ce297-237">Windows Event Log</span></span>
* <span data-ttu-id="ce297-238">TraceSource</span><span class="sxs-lookup"><span data-stu-id="ce297-238">TraceSource</span></span>
* <span data-ttu-id="ce297-239">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ce297-239">Azure App Service</span></span>
* <span data-ttu-id="ce297-240">Usługi Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="ce297-240">Azure Application Insights</span></span>

<span data-ttu-id="ce297-241">Zapis loguje się z dowolnego miejsca w kodzie aplikacji uzyskując `ILogger` obiekt z DI i wywoływania metod dziennika.</span><span class="sxs-lookup"><span data-stu-id="ce297-241">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="ce297-242">Poniżej przedstawiono przykładowy kod, który używa `ILogger` obiektu przy użyciu iniekcji konstruktora i wywołań metod rejestrowania, które są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="ce297-242">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="ce297-243">`ILogger` Interfejs pozwala przekazać dowolną liczbę pól do dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="ce297-243">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="ce297-244">Pola są często używane do konstruowania ciąg komunikatu, ale dostawca może także wysłać im jako oddzielne pola w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="ce297-244">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="ce297-245">Ta funkcja umożliwia rejestrowanie dostawców, aby zaimplementować [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="ce297-245">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="ce297-246">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="ce297-246">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="ce297-247">Routing</span><span class="sxs-lookup"><span data-stu-id="ce297-247">Routing</span></span>

<span data-ttu-id="ce297-248">A *trasy* jest wzorzec adresu URL, który jest mapowany do obsługi.</span><span class="sxs-lookup"><span data-stu-id="ce297-248">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="ce297-249">Program obsługi jest zwykle strony Razor, metody akcji kontrolera MVC lub oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="ce297-249">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="ce297-250">ASP.NET Core routingu zapewnia kontrolę nad adresów URL używanych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="ce297-250">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="ce297-251">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="ce297-251">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="ce297-252">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="ce297-252">Error handling</span></span>

<span data-ttu-id="ce297-253">Platforma ASP.NET Core ma wbudowane funkcje obsługi błędów, takich jak:</span><span class="sxs-lookup"><span data-stu-id="ce297-253">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="ce297-254">Stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="ce297-254">A developer exception page</span></span>
* <span data-ttu-id="ce297-255">Strony błędów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="ce297-255">Custom error pages</span></span>
* <span data-ttu-id="ce297-256">Stan statycznej strony kodowe</span><span class="sxs-lookup"><span data-stu-id="ce297-256">Static status code pages</span></span>
* <span data-ttu-id="ce297-257">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="ce297-257">Startup exception handling</span></span>

<span data-ttu-id="ce297-258">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="ce297-258">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="ce297-259">Zgłaszanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="ce297-259">Make HTTP requests</span></span>

<span data-ttu-id="ce297-260">Implementacja `IHttpClientFactory` jest dostępna dla tworzenia `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ce297-260">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="ce297-261">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="ce297-261">The factory:</span></span>

* <span data-ttu-id="ce297-262">Stanowi centralną lokalizację do nazywania i konfigurowanie logiczne `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ce297-262">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="ce297-263">Na przykład *github* klienta można zarejestrować i skonfigurowane do korzystania z usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="ce297-263">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="ce297-264">Domyślne klienta można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="ce297-264">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="ce297-265">Obsługuje rejestrację i tworzenie łańcucha wielu obsługi delegowania do tworzenia potoku oprogramowania pośredniczącego usługi wychodzące żądanie.</span><span class="sxs-lookup"><span data-stu-id="ce297-265">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="ce297-266">Wzorzec ten jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce297-266">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="ce297-267">Wzorzec zapewnia mechanizm zarządzania odciąż przekrojowe zagadnienia dotyczące żądań HTTP, w tym usługi pamięć podręczna obsługi serializacji i rejestrowania błędów.</span><span class="sxs-lookup"><span data-stu-id="ce297-267">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="ce297-268">Integruje się z *Polly*, popularne biblioteki innych firm dotyczące obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="ce297-268">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="ce297-269">Zarządza buforowanie i okresem istnienia bazowego `HttpClientMessageHandler` wystąpienia, aby uniknąć problemów DNS, które występują, gdy ręcznego zarządzania `HttpClient` okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="ce297-269">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="ce297-270">Dodanie obsługi można skonfigurować rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="ce297-270">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="ce297-271">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="ce297-271">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="ce297-272">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="ce297-272">Content root</span></span>

<span data-ttu-id="ce297-273">Główny zawartości jest ścieżka podstawowa do prywatnej zawartości używany przez aplikację, takie jak jego pliki Razor.</span><span class="sxs-lookup"><span data-stu-id="ce297-273">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="ce297-274">Domyślnie zawartość katalogu głównego jest podstawową ścieżkę dla pliku wykonywalnego hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce297-274">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="ce297-275">Być może alternatywną lokalizację określony podczas [tworzenia hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="ce297-275">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce297-276">Aby uzyskać więcej informacji, zobacz [zawartości głównego](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="ce297-276">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ce297-277">Aby uzyskać więcej informacji, zobacz [zawartości głównego](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="ce297-277">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="ce297-278">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="ce297-278">Web root</span></span>

<span data-ttu-id="ce297-279">Katalog główny sieci web (znany także jako *webroot*) to ścieżka podstawowa do publicznej, statycznej zasobów, takich jak CSS, JavaScript i plików obrazów.</span><span class="sxs-lookup"><span data-stu-id="ce297-279">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="ce297-280">Oprogramowanie pośredniczące plików statycznych posłużą tylko pliki z katalogu głównego sieci web (i jego podkatalogi) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ce297-280">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="ce297-281">Ścieżka katalogu głównego sieci web, wartość domyślna to *{głównego zawartości} / wwwroot*, ale może innej lokalizacji, należy określić podczas [tworzenia hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="ce297-281">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="ce297-282">W aparacie Razor (*.cshtml*) plików ukośnika tylda `~/` wskazuje katalog główny sieci web.</span><span class="sxs-lookup"><span data-stu-id="ce297-282">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="ce297-283">Począwszy od ścieżki `~/` są określane jako ścieżek wirtualnych.</span><span class="sxs-lookup"><span data-stu-id="ce297-283">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="ce297-284">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="ce297-284">For more information, see <xref:fundamentals/static-files>.</span></span>
