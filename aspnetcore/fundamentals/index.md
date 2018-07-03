---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 33786bf78567a1aa12a1ac97d44d1a596ec4c3be
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144979"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="401d9-103">Podstawy platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="401d9-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="401d9-104">Aplikacji ASP.NET Core jest aplikacją konsoli, która tworzy serwer sieci web w jego `Main` metody:</span><span class="sxs-lookup"><span data-stu-id="401d9-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="401d9-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="401d9-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="401d9-106">`Main` Wywołuje metodę `WebHost.CreateDefaultBuilder`, która jest zgodna z wzorcem konstruktora w celu utworzenia hosta aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="401d9-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="401d9-107">Konstruktor ma metody definiujące serwer sieci web (na przykład `UseKestrel`) i Klasa początkowa (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="401d9-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="401d9-108">W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web jest przydzielany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="401d9-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="401d9-109">Host sieci web platformy ASP.NET Core próbuje uruchomić na serwerze IIS, jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="401d9-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="401d9-110">Inne serwery sieci web, takich jak [HTTP.sys](xref:fundamentals/servers/httpsys), mogą być używane przez wywołanie metody rozszerzenia odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="401d9-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="401d9-111">`UseStartup` opisano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="401d9-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="401d9-112">`IWebHostBuilder`, zwracany typ `WebHost.CreateDefaultBuilder` wywołania, oferuje wiele metod opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="401d9-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="401d9-113">Oto niektóre z tych metod `UseHttpSys` do hostowania aplikacji w pliku HTTP.sys i `UseContentRoot` do określania zawartości katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="401d9-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="401d9-114">`Build` i `Run` metod tworzenia `IWebHost` obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="401d9-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="401d9-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="401d9-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="401d9-116">`Main` Metoda używa `WebHostBuilder`, który jest zgodny ze wzorcem konstruktora w celu utworzenia hosta aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="401d9-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="401d9-117">Konstruktor ma metody definiujące serwer sieci web (na przykład `UseKestrel`) i Klasa początkowa (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="401d9-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="401d9-118">W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) używany jest serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="401d9-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="401d9-119">Inne serwery sieci web, takich jak [WebListener](xref:fundamentals/servers/weblistener), mogą być używane przez wywołanie metody rozszerzenia odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="401d9-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="401d9-120">`UseStartup` opisano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="401d9-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="401d9-121">`WebHostBuilder` udostępnia wiele metod opcjonalne, w tym `UseIISIntegration` do hostowania w usługach IIS i usług IIS Express i `UseContentRoot` do określania zawartości katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="401d9-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="401d9-122">`Build` i `Run` metod tworzenia `IWebHost` obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="401d9-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="401d9-123">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="401d9-123">Startup</span></span>

<span data-ttu-id="401d9-124">`UseStartup` Metody `WebHostBuilder` Określa `Startup` klasy dla twojej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="401d9-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="401d9-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="401d9-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="401d9-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="401d9-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="401d9-127">`Startup` Klasa jest gdzie definiuje się Potok żądań obsługi i którym skonfigurowano wszystkich usług wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="401d9-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="401d9-128">`Startup` Klasy musi być publiczna i może zawierać następujących metod:</span><span class="sxs-lookup"><span data-stu-id="401d9-128">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="401d9-129">`ConfigureServices` definiuje [usług](#dependency-injection-services) używanych przez aplikację (na przykład, ASP.NET Core MVC, platformy Entity Framework Core, tożsamość).</span><span class="sxs-lookup"><span data-stu-id="401d9-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="401d9-130">`Configure` definiuje [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) dla żądania potoku.</span><span class="sxs-lookup"><span data-stu-id="401d9-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="401d9-131">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="401d9-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="401d9-132">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="401d9-132">Content root</span></span>

<span data-ttu-id="401d9-133">Główny zawartości jest ścieżka podstawowa do żadnej zawartości, używanych przez aplikację, takie jak widoki, [stron Razor](xref:razor-pages/index), a zasoby statyczne.</span><span class="sxs-lookup"><span data-stu-id="401d9-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:razor-pages/index), and static assets.</span></span> <span data-ttu-id="401d9-134">Domyślnie zawartość katalogu głównego jest taka sama jak podstawowa ścieżka aplikacji dla pliku wykonywalnego hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="401d9-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="401d9-135">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="401d9-135">Web root</span></span>

<span data-ttu-id="401d9-136">Katalog główny aplikacji sieci web jest to katalog, w do projektu zawierającego zasoby publicznej, statycznej, takie jak CSS, JavaScript i plików obrazów.</span><span class="sxs-lookup"><span data-stu-id="401d9-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="401d9-137">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="401d9-137">Dependency injection (services)</span></span>

<span data-ttu-id="401d9-138">Usługa jest składnikiem, który jest przeznaczony do wspólnej wykorzystania w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="401d9-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="401d9-139">Usługi są udostępniane za pośrednictwem [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="401d9-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="401d9-140">Platforma ASP.NET Core zawiera natywny **I**nWymagana **o**f **C**kontener ontrola (IoC), który obsługuje [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="401d9-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="401d9-141">W razie potrzeby można zastąpić domyślny kontener natywnych.</span><span class="sxs-lookup"><span data-stu-id="401d9-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="401d9-142">Oprócz korzyści z jej Poluzuj powiązania DI sprawia, że usługi dostępne w całej aplikacji (na przykład [rejestrowania](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="401d9-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="401d9-143">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="401d9-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="401d9-144">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="401d9-144">Middleware</span></span>

<span data-ttu-id="401d9-145">W programie ASP.NET Core redagowania Twojego żądania potoku przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="401d9-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="401d9-146">Oprogramowanie pośredniczące platformy ASP.NET Core wykonuje logiki przetwarzania asynchronicznego `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w sekwencji lub kończy żądanie bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="401d9-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="401d9-147">Składnik oprogramowania pośredniczącego o nazwie "XYZ", jest dodawany przez wywołanie `UseXYZ` metody rozszerzenia w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="401d9-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="401d9-148">Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="401d9-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="401d9-149">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="401d9-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="401d9-150">Routing</span><span class="sxs-lookup"><span data-stu-id="401d9-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="401d9-151">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="401d9-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="401d9-152">Oprogramowanie pośredniczące kompresji odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="401d9-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="401d9-153">Oprogramowanie pośredniczące ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="401d9-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="401d9-154">[OWIN](http://owin.org)— oparte na oprogramowaniu pośredniczącym jest dostępna dla aplikacji platformy ASP.NET Core i można napisać własne niestandardowe oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="401d9-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="401d9-155">Aby uzyskać więcej informacji, zobacz [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) i [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="401d9-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="401d9-156">Inicjowanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="401d9-156">Initiate HTTP requests</span></span>

<span data-ttu-id="401d9-157">Informacji o używaniu `IHttpClientFactory` dostęp do `HttpClient` wystąpień do wysyłania żądań HTTP, zobacz [żądań HTTP zainicjować](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="401d9-157">For information about using `IHttpClientFactory` to access `HttpClient` instances to make HTTP requests, see [Initiate HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="401d9-158">Środowiska</span><span class="sxs-lookup"><span data-stu-id="401d9-158">Environments</span></span>

<span data-ttu-id="401d9-159">Środowiskach, takich jak "Programowanie" i "Produkcyjne" to najwyższej jakości pojęcie w programie ASP.NET Core i można ustawić za pomocą zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="401d9-159">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="401d9-160">Aby uzyskać więcej informacji, zobacz [używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="401d9-160">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="401d9-161">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="401d9-161">Configuration</span></span>

<span data-ttu-id="401d9-162">Platforma ASP.NET Core używa modelu konfiguracji na podstawie par nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="401d9-162">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="401d9-163">Model konfiguracji nie jest oparty na `System.Configuration` lub *web.config*. Konfiguracja pobiera ustawienia z uporządkowany zestaw dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="401d9-163">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="401d9-164">Dostawcy wbudowana Konfiguracja obsługują różne formaty plików (XML, JSON, INI) i zmiennych środowiskowych w celu włączenia konfiguracji opartej na środowisku.</span><span class="sxs-lookup"><span data-stu-id="401d9-164">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="401d9-165">Można również napisać własne dostawców konfiguracji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="401d9-165">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="401d9-166">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="401d9-166">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="401d9-167">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="401d9-167">Logging</span></span>

<span data-ttu-id="401d9-168">Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców logowania.</span><span class="sxs-lookup"><span data-stu-id="401d9-168">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="401d9-169">Wbudowani dostawcy obsługuje wysyłanie dzienników do jednego lub więcej miejsc docelowych.</span><span class="sxs-lookup"><span data-stu-id="401d9-169">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="401d9-170">Może służyć struktur rejestrowania innych firm.</span><span class="sxs-lookup"><span data-stu-id="401d9-170">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="401d9-171">Aby uzyskać więcej informacji, zobacz [rejestrowania](xref:fundamentals/logging/index)</span><span class="sxs-lookup"><span data-stu-id="401d9-171">For more information, see [Logging](xref:fundamentals/logging/index)</span></span>

## <a name="error-handling"></a><span data-ttu-id="401d9-172">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="401d9-172">Error handling</span></span>

<span data-ttu-id="401d9-173">Platforma ASP.NET Core ma wbudowane funkcje obsługi błędów w aplikacji, w tym stroną wyjątków dla deweloperów, strony błędów niestandardowych, stron kodowych stan statycznych i uruchamiania obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="401d9-173">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="401d9-174">Aby uzyskać więcej informacji, zobacz [sposób obsługi błędów](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="401d9-174">For more information, see [how to handle errors](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="401d9-175">Routing</span><span class="sxs-lookup"><span data-stu-id="401d9-175">Routing</span></span>

<span data-ttu-id="401d9-176">Platforma ASP.NET Core oferuje funkcje routingu żądań aplikacji do obsługi trasy.</span><span class="sxs-lookup"><span data-stu-id="401d9-176">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="401d9-177">Aby uzyskać więcej informacji, zobacz [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="401d9-177">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="401d9-178">Dostawcy plików</span><span class="sxs-lookup"><span data-stu-id="401d9-178">File providers</span></span>

<span data-ttu-id="401d9-179">Platforma ASP.NET Core przenosi dostępu do systemu plików przy użyciu dostawcy plików, która zapewnia wspólny interfejs do pracy z plikami na wielu platformach.</span><span class="sxs-lookup"><span data-stu-id="401d9-179">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="401d9-180">Aby uzyskać więcej informacji, zobacz [dostawcy plików](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="401d9-180">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="401d9-181">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="401d9-181">Static files</span></span>

<span data-ttu-id="401d9-182">Oprogramowanie pośredniczące plików statycznych służy plików statycznych, takich jak HTML, CSS, obrazów i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="401d9-182">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="401d9-183">Aby uzyskać więcej informacji, zobacz [pliki statyczne](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="401d9-183">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="401d9-184">Hosting</span><span class="sxs-lookup"><span data-stu-id="401d9-184">Hosting</span></span>

<span data-ttu-id="401d9-185">Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*, który jest odpowiedzialny za zarządzanie uruchamiania i okres istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="401d9-185">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="401d9-186">Aby uzyskać więcej informacji, zobacz [hostów w programie ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="401d9-186">For more information, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="401d9-187">Stan sesji i aplikacji</span><span class="sxs-lookup"><span data-stu-id="401d9-187">Session and app state</span></span>

<span data-ttu-id="401d9-188">Platforma ASP.NET Core oferuje kilka metod, aby zachować stan sesji i aplikacji, gdy użytkownik przegląda aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="401d9-188">ASP.NET Core offers several approaches to preserve session and app state while the user browses a web app.</span></span>

<span data-ttu-id="401d9-189">Aby uzyskać więcej informacji, zobacz [stan sesji i aplikacji](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="401d9-189">For more information, see [Session and app state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="401d9-190">Serwery</span><span class="sxs-lookup"><span data-stu-id="401d9-190">Servers</span></span>

<span data-ttu-id="401d9-191">Model hostingu w programie ASP.NET Core bezpośrednio nie nasłuchuje żądań.</span><span class="sxs-lookup"><span data-stu-id="401d9-191">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="401d9-192">Model hostingu opiera się na implementację serwera HTTP do przekazywania żądań do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="401d9-192">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="401d9-193">Przekazane żądanie opakowaniu jako zbiór obiektów funkcji, które mogą być udostępniane za pośrednictwem interfejsów.</span><span class="sxs-lookup"><span data-stu-id="401d9-193">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="401d9-194">Platforma ASP.NET Core obejmuje serwera zarządzanego dla wielu platform sieci web, nazywanego [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="401d9-194">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="401d9-195">Kestrel jest często uruchamiać za produkcyjnym serwerze sieci web, takich jak [IIS](https://www.iis.net/) lub [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="401d9-195">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="401d9-196">Kestrel może działać jako serwer graniczny.</span><span class="sxs-lookup"><span data-stu-id="401d9-196">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="401d9-197">Aby uzyskać więcej informacji, zobacz [serwerów](xref:fundamentals/servers/index) i następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="401d9-197">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="401d9-198">Kestrel</span><span class="sxs-lookup"><span data-stu-id="401d9-198">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="401d9-199">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="401d9-199">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="401d9-200">[Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="401d9-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="401d9-201">Globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="401d9-201">Globalization and localization</span></span>

<span data-ttu-id="401d9-202">Tworzenie wielojęzycznych witryny sieci Web za pomocą programu ASP.NET Core umożliwia witryny dotrzeć do większej liczby osób.</span><span class="sxs-lookup"><span data-stu-id="401d9-202">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="401d9-203">ASP.NET Core oferuje usługi oraz oprogramowanie pośredniczące lokalizowanie w różnych językach i kultur.</span><span class="sxs-lookup"><span data-stu-id="401d9-203">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="401d9-204">Aby uzyskać więcej informacji, zobacz [lokalizacja i globalizacja](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="401d9-204">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="401d9-205">Funkcje na żądanie</span><span class="sxs-lookup"><span data-stu-id="401d9-205">Request features</span></span>

<span data-ttu-id="401d9-206">Szczegóły implementacji serwera sieci Web związanych z żądań HTTP i odpowiedzi są zdefiniowane w interfejsach.</span><span class="sxs-lookup"><span data-stu-id="401d9-206">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="401d9-207">Te interfejsy są używane przez implementacje serwera i oprogramowania pośredniczącego do tworzenia i modyfikowania potoku hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="401d9-207">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="401d9-208">Aby uzyskać więcej informacji, zobacz [żądania funkcji](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="401d9-208">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="401d9-209">Zadania w tle</span><span class="sxs-lookup"><span data-stu-id="401d9-209">Background tasks</span></span>

<span data-ttu-id="401d9-210">Zadania w tle są implementowane jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="401d9-210">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="401d9-211">Usługa hostowana jest klasą z logiką zadań tła, który implementuje [pomocą interfejsu IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="401d9-211">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="401d9-212">Aby uzyskać więcej informacji, zobacz [zadania z usługami hostowanymi w tle](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="401d9-212">For more information, see [Background tasks with hosted services](xref:fundamentals/host/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="401d9-213">Otwarty interfejs internetowy dla platformy .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="401d9-213">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="401d9-214">Platforma ASP.NET Core obsługuje Open Web Interface for .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="401d9-214">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="401d9-215">OWIN umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.</span><span class="sxs-lookup"><span data-stu-id="401d9-215">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="401d9-216">Aby uzyskać więcej informacji, zobacz [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="401d9-216">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="401d9-217">Funkcja WebSockets</span><span class="sxs-lookup"><span data-stu-id="401d9-217">WebSockets</span></span>

<span data-ttu-id="401d9-218">[WebSocket](https://wikipedia.org/wiki/WebSocket) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP.</span><span class="sxs-lookup"><span data-stu-id="401d9-218">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="401d9-219">Jest używana w przypadku aplikacji, takie jak rozmowy, giełdowych, gry i dowolnym miejscu działanie funkcji w czasie rzeczywistym w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="401d9-219">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="401d9-220">Platforma ASP.NET Core obsługuje funkcje gniazda sieci web.</span><span class="sxs-lookup"><span data-stu-id="401d9-220">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="401d9-221">Aby uzyskać więcej informacji, zobacz [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="401d9-221">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="401d9-222">Microsoft.AspNetCore.App meta Microsoft.aspnetcore.all</span><span class="sxs-lookup"><span data-stu-id="401d9-222">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="401d9-223">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) meta Microsoft.aspnetcore.all upraszcza zarządzanie pakietami.</span><span class="sxs-lookup"><span data-stu-id="401d9-223">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span> <span data-ttu-id="401d9-224">Aby uzyskać więcej informacji, zobacz [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="401d9-224">For more information, see [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="401d9-225">Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="401d9-225">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="401d9-226">[Pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) meta Microsoft.aspnetcore.all dla platformy ASP.NET Core obejmuje:</span><span class="sxs-lookup"><span data-stu-id="401d9-226">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="401d9-227">Wszystkie obsługiwane pakiety przez zespół programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="401d9-227">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="401d9-228">We wszystkich obsługiwanych pakietów w kolejności od platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="401d9-228">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="401d9-229">Zależności wewnętrzne i firm 3 używane przez program ASP.NET Core i Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="401d9-229">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="401d9-230">Aby uzyskać więcej informacji, zobacz [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="401d9-230">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="401d9-231">Środowisko uruchomieniowe programu .NET core i .NET Framework</span><span class="sxs-lookup"><span data-stu-id="401d9-231">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="401d9-232">Aplikacji ASP.NET Core mogą określać docelową środowisko uruchomieniowe platformy .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="401d9-232">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="401d9-233">Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="401d9-233">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="401d9-234">Wybieranie między platformą ASP.NET Core i ASP.NET</span><span class="sxs-lookup"><span data-stu-id="401d9-234">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="401d9-235">Aby uzyskać więcej informacji na temat wybierania między platformą ASP.NET Core i ASP.NET, zobacz [wybór między platformą ASP.NET Core i ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="401d9-235">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
