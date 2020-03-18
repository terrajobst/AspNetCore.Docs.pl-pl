---
title: Podstawy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe koncepcje tworzenia aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: a16a2fbb4ad2a79f96b6646ffdc359619d361a25
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434320"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="77fe2-103">Podstawy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77fe2-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="77fe2-104">Ten artykuł zawiera omówienie najważniejszych tematów dotyczących sposobu tworzenia aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77fe2-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="77fe2-105">Klasa Startup</span><span class="sxs-lookup"><span data-stu-id="77fe2-105">The Startup class</span></span>

<span data-ttu-id="77fe2-106">Klasa `Startup` to:</span><span class="sxs-lookup"><span data-stu-id="77fe2-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="77fe2-107">Usługi wymagane przez aplikację są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="77fe2-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="77fe2-108">Zdefiniowano potok obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="77fe2-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="77fe2-109">*Usługi* są składnikami, które są używane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="77fe2-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="77fe2-110">Na przykład składnik rejestrowania to usługa.</span><span class="sxs-lookup"><span data-stu-id="77fe2-110">For example, a logging component is a service.</span></span> <span data-ttu-id="77fe2-111">Kod do konfigurowania (lub *rejestrowania*) usług jest dodawany do metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="77fe2-112">Potok obsługi żądań składa się z serii komponentów *oprogramowania pośredniczącego* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="77fe2-113">Na przykład oprogramowanie pośredniczące może obsługiwać żądania dotyczące plików statycznych lub przekierowywać żądania HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="77fe2-114">Każde oprogramowanie pośredniczące wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="77fe2-115">Kod do konfigurowania potoku obsługi żądań został dodany do metody `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="77fe2-116">Oto przykładowa Klasa `Startup`:</span><span class="sxs-lookup"><span data-stu-id="77fe2-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="77fe2-117">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="77fe2-118">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="77fe2-118">Dependency injection (services)</span></span>

<span data-ttu-id="77fe2-119">ASP.NET Core ma wbudowaną platformę wstrzykiwania zależności (DI), która udostępnia skonfigurowane usługi dla klas aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="77fe2-120">Jednym ze sposobów uzyskania wystąpienia usługi w klasie jest utworzenie konstruktora z parametrem wymaganego typu.</span><span class="sxs-lookup"><span data-stu-id="77fe2-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="77fe2-121">Parametr może być typem usługi lub interfejsem.</span><span class="sxs-lookup"><span data-stu-id="77fe2-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="77fe2-122">System DI zapewnia usługę w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="77fe2-123">Oto Klasa, która używa funkcji DI do pobrania obiektu kontekstu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="77fe2-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="77fe2-124">Wyróżniony wiersz jest przykładem iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="77fe2-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="77fe2-125">Podczas gdy program jest wbudowany, został zaprojektowany z myślą o umożliwieniu podłączenia kontenera kontroli (IoC) innej firmy.</span><span class="sxs-lookup"><span data-stu-id="77fe2-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="77fe2-126">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="77fe2-127">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="77fe2-127">Middleware</span></span>

<span data-ttu-id="77fe2-128">Potok obsługi żądań składa się z serii komponentów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="77fe2-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="77fe2-129">Każdy składnik wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="77fe2-130">Zgodnie z Konwencją składnik pośredniczący jest dodawany do potoku przez wywołanie jego metody rozszerzenia `Use...` w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="77fe2-131">Na przykład, aby włączyć renderowanie plików statycznych, wywołaj `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="77fe2-132">Wyróżniony kod w poniższym przykładzie konfiguruje potok obsługi żądań:</span><span class="sxs-lookup"><span data-stu-id="77fe2-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="77fe2-133">ASP.NET Core zawiera rozbudowany zestaw wbudowanych programów pośredniczących i można napisać niestandardowe oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="77fe2-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="77fe2-134">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="77fe2-135">Host</span><span class="sxs-lookup"><span data-stu-id="77fe2-135">Host</span></span>

<span data-ttu-id="77fe2-136">Aplikacja ASP.NET Core kompiluje *hosta* podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="77fe2-137">Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="77fe2-138">Implementacja serwera HTTP</span><span class="sxs-lookup"><span data-stu-id="77fe2-138">An HTTP server implementation</span></span>
* <span data-ttu-id="77fe2-139">Składniki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="77fe2-139">Middleware components</span></span>
* <span data-ttu-id="77fe2-140">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="77fe2-140">Logging</span></span>
* <span data-ttu-id="77fe2-141">FOSFORAN</span><span class="sxs-lookup"><span data-stu-id="77fe2-141">DI</span></span>
* <span data-ttu-id="77fe2-142">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="77fe2-142">Configuration</span></span>

<span data-ttu-id="77fe2-143">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="77fe2-144">Dostępne są dwa hosty: Host ogólny i host sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="77fe2-145">Zalecany jest host ogólny, a host sieci Web jest dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="77fe2-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="77fe2-146">Kod służący do tworzenia hosta znajduje się w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="77fe2-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="77fe2-147">Metody `CreateDefaultBuilder` i `ConfigureWebHostDefaults` umożliwiają skonfigurowanie hosta z najczęściej używanymi opcjami, takimi jak następujące:</span><span class="sxs-lookup"><span data-stu-id="77fe2-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="77fe2-148">Użyj [Kestrel](#servers) jako serwera sieci Web i Włącz INTEGRACJĘ usług IIS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="77fe2-149">Załaduj konfigurację z pliku *appSettings. JSON*, *appSettings. { Nazwa środowiska}. JSON*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="77fe2-150">Wyślij dane wyjściowe rejestrowania do konsoli programu i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="77fe2-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="77fe2-152">Scenariusze inne niż internetowe</span><span class="sxs-lookup"><span data-stu-id="77fe2-152">Non-web scenarios</span></span>

<span data-ttu-id="77fe2-153">Host ogólny umożliwia innym typom aplikacji korzystanie z rozszerzeń struktury wycinania, takich jak rejestrowanie, iniekcja zależności (DI), konfiguracja i zarządzanie okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-153">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="77fe2-154">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-154">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="77fe2-155">Serwery</span><span class="sxs-lookup"><span data-stu-id="77fe2-155">Servers</span></span>

<span data-ttu-id="77fe2-156">Aplikacja ASP.NET Core używa implementacji serwera HTTP do nasłuchiwania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="77fe2-156">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="77fe2-157">Serwer wyświetla żądania do aplikacji jako zestaw [funkcji żądania](xref:fundamentals/request-features) złożonych do `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-157">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="77fe2-158">Windows</span><span class="sxs-lookup"><span data-stu-id="77fe2-158">Windows</span></span>](#tab/windows)

<span data-ttu-id="77fe2-159">ASP.NET Core udostępnia następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="77fe2-159">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="77fe2-160">*Kestrel* to Międzyplatformowy serwer sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-160">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="77fe2-161">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-161">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="77fe2-162">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-162">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="77fe2-163">*Serwer http IIS* jest serwerem dla systemu Windows, który korzysta z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-163">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="77fe2-164">Na tym serwerze aplikacja ASP.NET Core i usługi IIS działają w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-164">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="77fe2-165">*Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-165">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="77fe2-166">macOS</span><span class="sxs-lookup"><span data-stu-id="77fe2-166">macOS</span></span>](#tab/macos)

<span data-ttu-id="77fe2-167">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-167">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="77fe2-168">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-168">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="77fe2-169">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-169">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="77fe2-170">Linux</span><span class="sxs-lookup"><span data-stu-id="77fe2-170">Linux</span></span>](#tab/linux)

<span data-ttu-id="77fe2-171">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-171">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="77fe2-172">W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-172">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="77fe2-173">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-173">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="77fe2-174">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-174">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="77fe2-175">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="77fe2-175">Configuration</span></span>

<span data-ttu-id="77fe2-176">ASP.NET Core udostępnia platformę konfiguracji, która pobiera ustawienia jako pary nazwa-wartość z uporządkowanego zestawu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-176">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="77fe2-177">Istnieją Wbudowani dostawcy konfiguracji dla różnych źródeł, takich jak pliki *. JSON* , pliki *. XML* , zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="77fe2-177">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="77fe2-178">Możesz również napisać niestandardowych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-178">You can also write custom configuration providers.</span></span>

<span data-ttu-id="77fe2-179">Można na przykład określić, że konfiguracja pochodzi z pliku *appSettings. JSON* i zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-179">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="77fe2-180">Następnie po zażądaniu wartości parametru *ConnectionString* struktura najpierw sprawdza plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-180">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="77fe2-181">Jeśli wartość jest tam znaleziona, ale również w zmiennej środowiskowej, pierwszeństwo ma wartość ze zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="77fe2-181">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="77fe2-182">Aby zarządzać poufnymi danymi konfiguracyjnymi, takimi jak hasła, ASP.NET Core zapewnia [Narzędzie tajnego Menedżera](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="77fe2-182">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="77fe2-183">W przypadku wpisów tajnych produkcji zalecamy [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="77fe2-183">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="77fe2-184">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="77fe2-185">Opcje</span><span class="sxs-lookup"><span data-stu-id="77fe2-185">Options</span></span>

<span data-ttu-id="77fe2-186">Jeśli to możliwe, ASP.NET Core są zgodne ze *wzorcem opcji* przechowywania i pobierania wartości konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-186">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="77fe2-187">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="77fe2-187">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="77fe2-188">Na przykład poniższy kod ustawia opcje obiektów WebSockets:</span><span class="sxs-lookup"><span data-stu-id="77fe2-188">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="77fe2-189">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-189">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="77fe2-190">Środowiska</span><span class="sxs-lookup"><span data-stu-id="77fe2-190">Environments</span></span>

<span data-ttu-id="77fe2-191">Środowiska wykonawcze, takie jak *programowanie*, *przemieszczanie*i *produkcja*, są pierwszą klasą koncepcji w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77fe2-191">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="77fe2-192">Aby określić środowisko, w którym działa aplikacja, należy ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-192">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="77fe2-193">ASP.NET Core odczytuje tę zmienną środowiskową przy uruchamianiu aplikacji i zapisuje wartość w implementacji `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-193">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="77fe2-194">Obiekt środowiska jest dostępny w dowolnym miejscu w aplikacji za pomocą funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="77fe2-194">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="77fe2-195">Następujący przykładowy kod z klasy `Startup` konfiguruje aplikację w celu dostarczania szczegółowych informacji o błędzie tylko wtedy, gdy są uruchamiane w programie Development:</span><span class="sxs-lookup"><span data-stu-id="77fe2-195">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="77fe2-196">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-196">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="77fe2-197">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="77fe2-197">Logging</span></span>

<span data-ttu-id="77fe2-198">ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm.</span><span class="sxs-lookup"><span data-stu-id="77fe2-198">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="77fe2-199">Dostępne są następujące dostawcy:</span><span class="sxs-lookup"><span data-stu-id="77fe2-199">Available providers include the following:</span></span>

* <span data-ttu-id="77fe2-200">Konsola</span><span class="sxs-lookup"><span data-stu-id="77fe2-200">Console</span></span>
* <span data-ttu-id="77fe2-201">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="77fe2-201">Debug</span></span>
* <span data-ttu-id="77fe2-202">Śledzenie zdarzeń w systemie Windows</span><span class="sxs-lookup"><span data-stu-id="77fe2-202">Event Tracing on Windows</span></span>
* <span data-ttu-id="77fe2-203">Dziennik zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="77fe2-203">Windows Event Log</span></span>
* <span data-ttu-id="77fe2-204">TraceSource</span><span class="sxs-lookup"><span data-stu-id="77fe2-204">TraceSource</span></span>
* <span data-ttu-id="77fe2-205">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="77fe2-205">Azure App Service</span></span>
* <span data-ttu-id="77fe2-206">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="77fe2-206">Azure Application Insights</span></span>

<span data-ttu-id="77fe2-207">Zapisuj dzienniki z dowolnego miejsca w kodzie aplikacji, pobierając `ILogger` obiekt z metod rejestrowania i wywoływania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-207">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="77fe2-208">Poniżej przedstawiono przykładowy kod, który używa obiektu `ILogger`, z iniekcją konstruktora i wyróżniania wywołań metody rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-208">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="77fe2-209">Interfejs `ILogger` umożliwia przekazanie dowolnej liczby pól dostawcy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-209">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="77fe2-210">Pola są często używane do konstruowania ciągu komunikatu, ale dostawcy mogą również wysyłać je jako oddzielne pola do magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-210">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="77fe2-211">Ta funkcja umożliwia dostawcom rejestrowania implementowanie [rejestrowania semantycznego, znanego również jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="77fe2-211">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="77fe2-212">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="77fe2-213">Routing</span><span class="sxs-lookup"><span data-stu-id="77fe2-213">Routing</span></span>

<span data-ttu-id="77fe2-214">*Trasa* jest WZORCEM adresu URL, który jest mapowany do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="77fe2-214">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="77fe2-215">Procedura obsługi jest zazwyczaj stroną Razor, metodą akcji w kontrolerze MVC lub w oprogramowaniu pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="77fe2-215">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="77fe2-216">Routing ASP.NET Core zapewnia kontrolę nad adresami URL używanymi przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="77fe2-216">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="77fe2-217">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="77fe2-218">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="77fe2-218">Error handling</span></span>

<span data-ttu-id="77fe2-219">ASP.NET Core ma wbudowane funkcje do obsługi błędów, takie jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-219">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="77fe2-220">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="77fe2-220">A developer exception page</span></span>
* <span data-ttu-id="77fe2-221">Niestandardowe strony błędów</span><span class="sxs-lookup"><span data-stu-id="77fe2-221">Custom error pages</span></span>
* <span data-ttu-id="77fe2-222">Statyczne strony kodów stanu</span><span class="sxs-lookup"><span data-stu-id="77fe2-222">Static status code pages</span></span>
* <span data-ttu-id="77fe2-223">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="77fe2-223">Startup exception handling</span></span>

<span data-ttu-id="77fe2-224">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-224">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="77fe2-225">Zgłaszanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="77fe2-225">Make HTTP requests</span></span>

<span data-ttu-id="77fe2-226">Implementacja `IHttpClientFactory` jest dostępna do tworzenia wystąpień `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-226">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="77fe2-227">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="77fe2-227">The factory:</span></span>

* <span data-ttu-id="77fe2-228">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-228">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="77fe2-229">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="77fe2-229">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="77fe2-230">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="77fe2-230">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="77fe2-231">Obsługuje rejestrację i łańcuch wielu procedur delegowania, aby utworzyć potok pośredniczący żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="77fe2-231">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="77fe2-232">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77fe2-232">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="77fe2-233">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-233">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="77fe2-234">Integruje się z usługą *Polly*, popularną biblioteką innej firmy na potrzeby obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-234">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="77fe2-235">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="77fe2-235">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="77fe2-236">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="77fe2-236">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="77fe2-237">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-237">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="77fe2-238">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="77fe2-238">Content root</span></span>

<span data-ttu-id="77fe2-239">Katalog główny zawartości jest ścieżką podstawową do:</span><span class="sxs-lookup"><span data-stu-id="77fe2-239">The content root is the base path to the:</span></span>

* <span data-ttu-id="77fe2-240">Plik wykonywalny obsługujący aplikację ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="77fe2-240">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="77fe2-241">Skompilowane zestawy wchodzące w skład aplikacji ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="77fe2-241">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="77fe2-242">Pliki zawartości inne niż kod używane przez aplikację, takie jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-242">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="77fe2-243">Pliki Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-243">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="77fe2-244">Pliki konfiguracji ( *. JSON*, *. XML*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-244">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="77fe2-245">Pliki danych ( *. DB*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-245">Data files (*.db*)</span></span>
* <span data-ttu-id="77fe2-246">[Katalog główny sieci Web](#web-root), zazwyczaj opublikowany folder *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-246">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="77fe2-247">Podczas tworzenia:</span><span class="sxs-lookup"><span data-stu-id="77fe2-247">During development:</span></span>

* <span data-ttu-id="77fe2-248">Katalog główny zawartości domyślnie jest katalogiem głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="77fe2-248">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="77fe2-249">Katalog główny projektu służy do tworzenia:</span><span class="sxs-lookup"><span data-stu-id="77fe2-249">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="77fe2-250">Ścieżka do plików zawartości nienależących do kodu aplikacji w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="77fe2-250">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="77fe2-251">[Katalog główny sieci Web](#web-root), zazwyczaj folder *wwwroot* w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="77fe2-251">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="77fe2-252">Alternatywna ścieżka katalogu głównego zawartości może być określona podczas [kompilowania hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="77fe2-252">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="77fe2-253">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-253">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

## <a name="web-root"></a><span data-ttu-id="77fe2-254">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="77fe2-254">Web root</span></span>

<span data-ttu-id="77fe2-255">Katalog główny sieci Web jest ścieżką podstawową do publicznych, niekodowych, statycznych plików zasobów, takich jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-255">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="77fe2-256">Arkusze stylów ( *. css*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-256">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="77fe2-257">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-257">JavaScript (*.js*)</span></span>
* <span data-ttu-id="77fe2-258">Obrazy ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-258">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="77fe2-259">Pliki statyczne są obsługiwane domyślnie tylko z katalogu głównego (i katalogów podrzędnych) sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-259">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="77fe2-260">Ścieżka katalogu głównego sieci Web jest domyślnie ustawiona na *{Content root}/wwwroot*, ale podczas [kompilowania hosta](#host)można określić inny katalog internetowy w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-260">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="77fe2-261">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-261">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

<span data-ttu-id="77fe2-262">Zapobiegaj publikowaniu plików w pliku *wwwroot* przy użyciu [elementu projektu\<Content >](/visualstudio/msbuild/common-msbuild-project-items#content) .</span><span class="sxs-lookup"><span data-stu-id="77fe2-262">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="77fe2-263">Poniższy przykład uniemożliwia opublikowanie zawartości w katalogu *wwwroot/lokalnym* i podkatalogach:</span><span class="sxs-lookup"><span data-stu-id="77fe2-263">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="77fe2-264">Aby zapobiec publikowaniu zasobów tożsamości statycznej do katalogu głównego sieci Web, zobacz <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-264">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

<span data-ttu-id="77fe2-265">W plikach Razor ( *. cshtml*), ukośnik (`~/`) wskazuje na katalog główny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-265">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="77fe2-266">Ścieżka rozpoczynająca się od `~/` jest nazywana *ścieżką wirtualną*.</span><span class="sxs-lookup"><span data-stu-id="77fe2-266">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="77fe2-267">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-267">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="77fe2-268">Ten artykuł zawiera omówienie najważniejszych tematów dotyczących sposobu tworzenia aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77fe2-268">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="77fe2-269">Klasa Startup</span><span class="sxs-lookup"><span data-stu-id="77fe2-269">The Startup class</span></span>

<span data-ttu-id="77fe2-270">Klasa `Startup` to:</span><span class="sxs-lookup"><span data-stu-id="77fe2-270">The `Startup` class is where:</span></span>

* <span data-ttu-id="77fe2-271">Usługi wymagane przez aplikację są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="77fe2-271">Services required by the app are configured.</span></span>
* <span data-ttu-id="77fe2-272">Zdefiniowano potok obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="77fe2-272">The request handling pipeline is defined.</span></span>

<span data-ttu-id="77fe2-273">*Usługi* są składnikami, które są używane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="77fe2-273">*Services* are components that are used by the app.</span></span> <span data-ttu-id="77fe2-274">Na przykład składnik rejestrowania to usługa.</span><span class="sxs-lookup"><span data-stu-id="77fe2-274">For example, a logging component is a service.</span></span> <span data-ttu-id="77fe2-275">Kod do konfigurowania (lub *rejestrowania*) usług jest dodawany do metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-275">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="77fe2-276">Potok obsługi żądań składa się z serii komponentów *oprogramowania pośredniczącego* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-276">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="77fe2-277">Na przykład oprogramowanie pośredniczące może obsługiwać żądania dotyczące plików statycznych lub przekierowywać żądania HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-277">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="77fe2-278">Każde oprogramowanie pośredniczące wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-278">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="77fe2-279">Kod do konfigurowania potoku obsługi żądań został dodany do metody `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-279">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="77fe2-280">Oto przykładowa Klasa `Startup`:</span><span class="sxs-lookup"><span data-stu-id="77fe2-280">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="77fe2-281">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="77fe2-282">Wstrzykiwanie zależności (usługi)</span><span class="sxs-lookup"><span data-stu-id="77fe2-282">Dependency injection (services)</span></span>

<span data-ttu-id="77fe2-283">ASP.NET Core ma wbudowaną platformę wstrzykiwania zależności (DI), która udostępnia skonfigurowane usługi dla klas aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-283">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="77fe2-284">Jednym ze sposobów uzyskania wystąpienia usługi w klasie jest utworzenie konstruktora z parametrem wymaganego typu.</span><span class="sxs-lookup"><span data-stu-id="77fe2-284">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="77fe2-285">Parametr może być typem usługi lub interfejsem.</span><span class="sxs-lookup"><span data-stu-id="77fe2-285">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="77fe2-286">System DI zapewnia usługę w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-286">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="77fe2-287">Oto Klasa, która używa funkcji DI do pobrania obiektu kontekstu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="77fe2-287">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="77fe2-288">Wyróżniony wiersz jest przykładem iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="77fe2-288">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="77fe2-289">Podczas gdy program jest wbudowany, został zaprojektowany z myślą o umożliwieniu podłączenia kontenera kontroli (IoC) innej firmy.</span><span class="sxs-lookup"><span data-stu-id="77fe2-289">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="77fe2-290">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-290">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="77fe2-291">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="77fe2-291">Middleware</span></span>

<span data-ttu-id="77fe2-292">Potok obsługi żądań składa się z serii komponentów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="77fe2-292">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="77fe2-293">Każdy składnik wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-293">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="77fe2-294">Zgodnie z Konwencją składnik pośredniczący jest dodawany do potoku przez wywołanie jego metody rozszerzenia `Use...` w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-294">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="77fe2-295">Na przykład, aby włączyć renderowanie plików statycznych, wywołaj `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-295">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="77fe2-296">Wyróżniony kod w poniższym przykładzie konfiguruje potok obsługi żądań:</span><span class="sxs-lookup"><span data-stu-id="77fe2-296">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="77fe2-297">ASP.NET Core zawiera rozbudowany zestaw wbudowanych programów pośredniczących i można napisać niestandardowe oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="77fe2-297">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="77fe2-298">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-298">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="77fe2-299">Host</span><span class="sxs-lookup"><span data-stu-id="77fe2-299">Host</span></span>

<span data-ttu-id="77fe2-300">Aplikacja ASP.NET Core kompiluje *hosta* podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-300">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="77fe2-301">Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-301">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="77fe2-302">Implementacja serwera HTTP</span><span class="sxs-lookup"><span data-stu-id="77fe2-302">An HTTP server implementation</span></span>
* <span data-ttu-id="77fe2-303">Składniki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="77fe2-303">Middleware components</span></span>
* <span data-ttu-id="77fe2-304">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="77fe2-304">Logging</span></span>
* <span data-ttu-id="77fe2-305">FOSFORAN</span><span class="sxs-lookup"><span data-stu-id="77fe2-305">DI</span></span>
* <span data-ttu-id="77fe2-306">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="77fe2-306">Configuration</span></span>

<span data-ttu-id="77fe2-307">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-307">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="77fe2-308">Dostępne są dwa hosty: host sieci Web i Host ogólny.</span><span class="sxs-lookup"><span data-stu-id="77fe2-308">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="77fe2-309">W ASP.NET Core 2. x Host generyczny jest przeznaczony tylko dla scenariuszy innych niż sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-309">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="77fe2-310">Kod służący do tworzenia hosta znajduje się w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="77fe2-310">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="77fe2-311">Metoda `CreateDefaultBuilder` konfiguruje hosta z najczęściej używanymi opcjami, takimi jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-311">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="77fe2-312">Użyj [Kestrel](#servers) jako serwera sieci Web i Włącz INTEGRACJĘ usług IIS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-312">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="77fe2-313">Załaduj konfigurację z pliku *appSettings. JSON*, *appSettings. { Nazwa środowiska}. JSON*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-313">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="77fe2-314">Wyślij dane wyjściowe rejestrowania do konsoli programu i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-314">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="77fe2-315">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-315">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="77fe2-316">Scenariusze inne niż internetowe</span><span class="sxs-lookup"><span data-stu-id="77fe2-316">Non-web scenarios</span></span>

<span data-ttu-id="77fe2-317">Host ogólny umożliwia innym typom aplikacji korzystanie z rozszerzeń struktury wycinania, takich jak rejestrowanie, iniekcja zależności (DI), konfiguracja i zarządzanie okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-317">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="77fe2-318">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-318">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="77fe2-319">Serwery</span><span class="sxs-lookup"><span data-stu-id="77fe2-319">Servers</span></span>

<span data-ttu-id="77fe2-320">Aplikacja ASP.NET Core używa implementacji serwera HTTP do nasłuchiwania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="77fe2-320">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="77fe2-321">Serwer wyświetla żądania do aplikacji jako zestaw [funkcji żądania](xref:fundamentals/request-features) złożonych do `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-321">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="77fe2-322">Windows</span><span class="sxs-lookup"><span data-stu-id="77fe2-322">Windows</span></span>](#tab/windows)

<span data-ttu-id="77fe2-323">ASP.NET Core udostępnia następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="77fe2-323">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="77fe2-324">*Kestrel* to Międzyplatformowy serwer sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-324">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="77fe2-325">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-325">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="77fe2-326">Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-326">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="77fe2-327">*Serwer http IIS* jest serwerem dla systemu Windows, który korzysta z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-327">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="77fe2-328">Na tym serwerze aplikacja ASP.NET Core i usługi IIS działają w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-328">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="77fe2-329">*Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-329">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="77fe2-330">macOS</span><span class="sxs-lookup"><span data-stu-id="77fe2-330">macOS</span></span>](#tab/macos)

<span data-ttu-id="77fe2-331">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-331">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="77fe2-332">Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-332">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="77fe2-333">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-333">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="77fe2-334">Linux</span><span class="sxs-lookup"><span data-stu-id="77fe2-334">Linux</span></span>](#tab/linux)

<span data-ttu-id="77fe2-335">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-335">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="77fe2-336">Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-336">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="77fe2-337">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-337">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="77fe2-338">Windows</span><span class="sxs-lookup"><span data-stu-id="77fe2-338">Windows</span></span>](#tab/windows)

<span data-ttu-id="77fe2-339">ASP.NET Core udostępnia następujące implementacje serwera:</span><span class="sxs-lookup"><span data-stu-id="77fe2-339">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="77fe2-340">*Kestrel* to Międzyplatformowy serwer sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-340">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="77fe2-341">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-341">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="77fe2-342">Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-342">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="77fe2-343">*Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="77fe2-343">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="77fe2-344">macOS</span><span class="sxs-lookup"><span data-stu-id="77fe2-344">macOS</span></span>](#tab/macos)

<span data-ttu-id="77fe2-345">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-345">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="77fe2-346">Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-346">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="77fe2-347">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-347">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="77fe2-348">Linux</span><span class="sxs-lookup"><span data-stu-id="77fe2-348">Linux</span></span>](#tab/linux)

<span data-ttu-id="77fe2-349">ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-349">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="77fe2-350">Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-350">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="77fe2-351">Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="77fe2-351">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="77fe2-352">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-352">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="77fe2-353">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="77fe2-353">Configuration</span></span>

<span data-ttu-id="77fe2-354">ASP.NET Core udostępnia platformę konfiguracji, która pobiera ustawienia jako pary nazwa-wartość z uporządkowanego zestawu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-354">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="77fe2-355">Istnieją Wbudowani dostawcy konfiguracji dla różnych źródeł, takich jak pliki *. JSON* , pliki *. XML* , zmienne środowiskowe i argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="77fe2-355">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="77fe2-356">Możesz również napisać niestandardowych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="77fe2-356">You can also write custom configuration providers.</span></span>

<span data-ttu-id="77fe2-357">Można na przykład określić, że konfiguracja pochodzi z pliku *appSettings. JSON* i zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-357">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="77fe2-358">Następnie po zażądaniu wartości parametru *ConnectionString* struktura najpierw sprawdza plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-358">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="77fe2-359">Jeśli wartość jest tam znaleziona, ale również w zmiennej środowiskowej, pierwszeństwo ma wartość ze zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="77fe2-359">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="77fe2-360">Aby zarządzać poufnymi danymi konfiguracyjnymi, takimi jak hasła, ASP.NET Core zapewnia [Narzędzie tajnego Menedżera](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="77fe2-360">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="77fe2-361">W przypadku wpisów tajnych produkcji zalecamy [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="77fe2-361">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="77fe2-362">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-362">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="77fe2-363">Opcje</span><span class="sxs-lookup"><span data-stu-id="77fe2-363">Options</span></span>

<span data-ttu-id="77fe2-364">Jeśli to możliwe, ASP.NET Core są zgodne ze *wzorcem opcji* przechowywania i pobierania wartości konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-364">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="77fe2-365">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="77fe2-365">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="77fe2-366">Na przykład poniższy kod ustawia opcje obiektów WebSockets:</span><span class="sxs-lookup"><span data-stu-id="77fe2-366">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="77fe2-367">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-367">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="77fe2-368">Środowiska</span><span class="sxs-lookup"><span data-stu-id="77fe2-368">Environments</span></span>

<span data-ttu-id="77fe2-369">Środowiska wykonawcze, takie jak *programowanie*, *przemieszczanie*i *produkcja*, są pierwszą klasą koncepcji w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77fe2-369">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="77fe2-370">Aby określić środowisko, w którym działa aplikacja, należy ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-370">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="77fe2-371">ASP.NET Core odczytuje tę zmienną środowiskową przy uruchamianiu aplikacji i zapisuje wartość w implementacji `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-371">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="77fe2-372">Obiekt środowiska jest dostępny w dowolnym miejscu w aplikacji za pomocą funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="77fe2-372">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="77fe2-373">Następujący przykładowy kod z klasy `Startup` konfiguruje aplikację w celu dostarczania szczegółowych informacji o błędzie tylko wtedy, gdy są uruchamiane w programie Development:</span><span class="sxs-lookup"><span data-stu-id="77fe2-373">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="77fe2-374">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-374">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="77fe2-375">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="77fe2-375">Logging</span></span>

<span data-ttu-id="77fe2-376">ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm.</span><span class="sxs-lookup"><span data-stu-id="77fe2-376">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="77fe2-377">Dostępne są następujące dostawcy:</span><span class="sxs-lookup"><span data-stu-id="77fe2-377">Available providers include the following:</span></span>

* <span data-ttu-id="77fe2-378">Konsola</span><span class="sxs-lookup"><span data-stu-id="77fe2-378">Console</span></span>
* <span data-ttu-id="77fe2-379">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="77fe2-379">Debug</span></span>
* <span data-ttu-id="77fe2-380">Śledzenie zdarzeń w systemie Windows</span><span class="sxs-lookup"><span data-stu-id="77fe2-380">Event Tracing on Windows</span></span>
* <span data-ttu-id="77fe2-381">Dziennik zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="77fe2-381">Windows Event Log</span></span>
* <span data-ttu-id="77fe2-382">TraceSource</span><span class="sxs-lookup"><span data-stu-id="77fe2-382">TraceSource</span></span>
* <span data-ttu-id="77fe2-383">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="77fe2-383">Azure App Service</span></span>
* <span data-ttu-id="77fe2-384">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="77fe2-384">Azure Application Insights</span></span>

<span data-ttu-id="77fe2-385">Zapisuj dzienniki z dowolnego miejsca w kodzie aplikacji, pobierając `ILogger` obiekt z metod rejestrowania i wywoływania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-385">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="77fe2-386">Poniżej przedstawiono przykładowy kod, który używa obiektu `ILogger`, z iniekcją konstruktora i wyróżniania wywołań metody rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-386">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="77fe2-387">Interfejs `ILogger` umożliwia przekazanie dowolnej liczby pól dostawcy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="77fe2-387">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="77fe2-388">Pola są często używane do konstruowania ciągu komunikatu, ale dostawcy mogą również wysyłać je jako oddzielne pola do magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-388">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="77fe2-389">Ta funkcja umożliwia dostawcom rejestrowania implementowanie [rejestrowania semantycznego, znanego również jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="77fe2-389">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="77fe2-390">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-390">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="77fe2-391">Routing</span><span class="sxs-lookup"><span data-stu-id="77fe2-391">Routing</span></span>

<span data-ttu-id="77fe2-392">*Trasa* jest WZORCEM adresu URL, który jest mapowany do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="77fe2-392">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="77fe2-393">Procedura obsługi jest zazwyczaj stroną Razor, metodą akcji w kontrolerze MVC lub w oprogramowaniu pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="77fe2-393">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="77fe2-394">Routing ASP.NET Core zapewnia kontrolę nad adresami URL używanymi przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="77fe2-394">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="77fe2-395">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-395">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="77fe2-396">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="77fe2-396">Error handling</span></span>

<span data-ttu-id="77fe2-397">ASP.NET Core ma wbudowane funkcje do obsługi błędów, takie jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-397">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="77fe2-398">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="77fe2-398">A developer exception page</span></span>
* <span data-ttu-id="77fe2-399">Niestandardowe strony błędów</span><span class="sxs-lookup"><span data-stu-id="77fe2-399">Custom error pages</span></span>
* <span data-ttu-id="77fe2-400">Statyczne strony kodów stanu</span><span class="sxs-lookup"><span data-stu-id="77fe2-400">Static status code pages</span></span>
* <span data-ttu-id="77fe2-401">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="77fe2-401">Startup exception handling</span></span>

<span data-ttu-id="77fe2-402">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-402">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="77fe2-403">Zgłaszanie żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="77fe2-403">Make HTTP requests</span></span>

<span data-ttu-id="77fe2-404">Implementacja `IHttpClientFactory` jest dostępna do tworzenia wystąpień `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="77fe2-404">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="77fe2-405">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="77fe2-405">The factory:</span></span>

* <span data-ttu-id="77fe2-406">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-406">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="77fe2-407">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="77fe2-407">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="77fe2-408">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="77fe2-408">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="77fe2-409">Obsługuje rejestrację i łańcuch wielu procedur delegowania, aby utworzyć potok pośredniczący żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="77fe2-409">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="77fe2-410">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77fe2-410">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="77fe2-411">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="77fe2-411">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="77fe2-412">Integruje się z usługą *Polly*, popularną biblioteką innej firmy na potrzeby obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="77fe2-412">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="77fe2-413">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="77fe2-413">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="77fe2-414">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="77fe2-414">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="77fe2-415">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-415">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="77fe2-416">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="77fe2-416">Content root</span></span>

<span data-ttu-id="77fe2-417">Katalog główny zawartości jest ścieżką podstawową do:</span><span class="sxs-lookup"><span data-stu-id="77fe2-417">The content root is the base path to the:</span></span>

* <span data-ttu-id="77fe2-418">Plik wykonywalny obsługujący aplikację ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="77fe2-418">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="77fe2-419">Skompilowane zestawy wchodzące w skład aplikacji ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="77fe2-419">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="77fe2-420">Pliki zawartości inne niż kod używane przez aplikację, takie jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-420">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="77fe2-421">Pliki Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-421">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="77fe2-422">Pliki konfiguracji ( *. JSON*, *. XML*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-422">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="77fe2-423">Pliki danych ( *. DB*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-423">Data files (*.db*)</span></span>
* <span data-ttu-id="77fe2-424">[Katalog główny sieci Web](#web-root), zazwyczaj opublikowany folder *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="77fe2-424">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="77fe2-425">Podczas tworzenia:</span><span class="sxs-lookup"><span data-stu-id="77fe2-425">During development:</span></span>

* <span data-ttu-id="77fe2-426">Katalog główny zawartości domyślnie jest katalogiem głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="77fe2-426">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="77fe2-427">Katalog główny projektu służy do tworzenia:</span><span class="sxs-lookup"><span data-stu-id="77fe2-427">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="77fe2-428">Ścieżka do plików zawartości nienależących do kodu aplikacji w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="77fe2-428">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="77fe2-429">[Katalog główny sieci Web](#web-root), zazwyczaj folder *wwwroot* w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="77fe2-429">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="77fe2-430">Alternatywna ścieżka katalogu głównego zawartości może być określona podczas [kompilowania hosta](#host).</span><span class="sxs-lookup"><span data-stu-id="77fe2-430">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="77fe2-431">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-431">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="77fe2-432">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="77fe2-432">Web root</span></span>

<span data-ttu-id="77fe2-433">Katalog główny sieci Web jest ścieżką podstawową do publicznych, niekodowych, statycznych plików zasobów, takich jak:</span><span class="sxs-lookup"><span data-stu-id="77fe2-433">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="77fe2-434">Arkusze stylów ( *. css*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-434">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="77fe2-435">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-435">JavaScript (*.js*)</span></span>
* <span data-ttu-id="77fe2-436">Obrazy ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="77fe2-436">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="77fe2-437">Pliki statyczne są obsługiwane domyślnie tylko z katalogu głównego (i katalogów podrzędnych) sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-437">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="77fe2-438">Ścieżka katalogu głównego sieci Web jest domyślnie ustawiona na *{Content root}/wwwroot*, ale podczas [kompilowania hosta](#host)można określić inny katalog internetowy w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-438">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="77fe2-439">Aby uzyskać więcej informacji, zobacz [katalog główny sieci Web](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="77fe2-439">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="77fe2-440">Zapobiegaj publikowaniu plików w pliku *wwwroot* przy użyciu [elementu projektu\<Content >](/visualstudio/msbuild/common-msbuild-project-items#content) .</span><span class="sxs-lookup"><span data-stu-id="77fe2-440">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="77fe2-441">Poniższy przykład uniemożliwia opublikowanie zawartości w katalogu *wwwroot/lokalnym* i podkatalogach:</span><span class="sxs-lookup"><span data-stu-id="77fe2-441">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="77fe2-442">W plikach Razor ( *. cshtml*), ukośnik (`~/`) wskazuje na katalog główny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="77fe2-442">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="77fe2-443">Ścieżka rozpoczynająca się od `~/` jest nazywana *ścieżką wirtualną*.</span><span class="sxs-lookup"><span data-stu-id="77fe2-443">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="77fe2-444">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="77fe2-444">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end