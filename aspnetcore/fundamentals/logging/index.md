---
title: Rejestrowanie w programie ASP.NET Core
author: tdykstra
description: Więcej informacji na temat struktury rejestrowania w programie ASP.NET Core. Odnajdywanie dostawcy wbudowane funkcje rejestrowania i Dowiedz się więcej na temat popularnych dostawców innych firm.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 435f06b85af4a1a5a78a870c2add3e15ff1ffe89
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837274"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="13bb5-104">Rejestrowanie w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13bb5-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="13bb5-105">Przez [Steve Smith](https://ardalis.com/) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="13bb5-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="13bb5-106">Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców rejestrowania wbudowanych oraz innych firm.</span><span class="sxs-lookup"><span data-stu-id="13bb5-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="13bb5-107">W tym artykule przedstawiono sposób korzystania z interfejsu API rejestrowania za pomocą wbudowanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="13bb5-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="13bb5-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="13bb5-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="13bb5-109">Dodawanie dostawcy</span><span class="sxs-lookup"><span data-stu-id="13bb5-109">Add providers</span></span>

<span data-ttu-id="13bb5-110">Dostawcy logowania Wyświetla lub są przechowywane dzienniki.</span><span class="sxs-lookup"><span data-stu-id="13bb5-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="13bb5-111">Na przykład konsola dostawca Wyświetla dzienniki konsoli i dostawcy usługi Azure Application Insights przechowuje je w usłudze Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="13bb5-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="13bb5-112">Dzienniki mogą być wysyłane do wielu miejsc docelowych, dodając wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="13bb5-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="13bb5-113">Aby dodać dostawcę, należy wywołać dostawcy `Add{provider name}` metody rozszerzenia w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="13bb5-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="13bb5-114">Wywołania szablonu projektu domyślnego <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, która dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="13bb5-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="13bb5-115">Konsola</span><span class="sxs-lookup"><span data-stu-id="13bb5-115">Console</span></span>
* <span data-ttu-id="13bb5-116">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="13bb5-116">Debug</span></span>
* <span data-ttu-id="13bb5-117">EventSource (począwszy od programu ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="13bb5-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="13bb5-118">Jeśli używasz `CreateDefaultBuilder`, domyślnych dostawców można zastąpić własnymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="13bb5-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="13bb5-119">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, i dostawców, o których chcesz dodać.</span><span class="sxs-lookup"><span data-stu-id="13bb5-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="13bb5-120">Można użyć dostawcy, instalowanie pakietu NuGet i wywołanie metody rozszerzenia dostawcy w wystąpieniu <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span><span class="sxs-lookup"><span data-stu-id="13bb5-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="13bb5-121">Platforma ASP.NET Core [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) zapewnia `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="13bb5-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="13bb5-122">`AddConsole` i `AddDebug` metody rozszerzenia są zdefiniowane w [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pakietów.</span><span class="sxs-lookup"><span data-stu-id="13bb5-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="13bb5-123">Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` jest metoda w wystąpieniu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="13bb5-124">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) dodaje rejestrowania dostawców w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="13bb5-124">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="13bb5-125">Aby uzyskać dane wyjściowe dziennika z kodu, który jest wykonywany wcześniej, należy dodać rejestrowania dostawców w `Startup` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="13bb5-126">Dowiedz się więcej o [wbudowane funkcje rejestrowania dostawców](#built-in-logging-providers) i [rejestrowania innych dostawców](#third-party-logging-providers) w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="13bb5-127">Twórz dzienniki</span><span class="sxs-lookup"><span data-stu-id="13bb5-127">Create logs</span></span>

<span data-ttu-id="13bb5-128">Pobierz <xref:Microsoft.Extensions.Logging.ILogger%601> obiekt z DI.</span><span class="sxs-lookup"><span data-stu-id="13bb5-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="13bb5-129">Poniższy przykład kontrolera tworzy `Information` i `Warning` dzienniki.</span><span class="sxs-lookup"><span data-stu-id="13bb5-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="13bb5-130">*Kategorii* jest `TodoApiSample.Controllers.TodoController` (w pełni kwalifikowaną nazwę klasy z `TodoController` w przykładowej aplikacji):</span><span class="sxs-lookup"><span data-stu-id="13bb5-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="13bb5-131">W poniższym przykładzie stron Razor utworzy dzienników za pomocą `Information` jako *poziom* i `TodoApiSample.Pages.AboutModel` jako *kategorii*:</span><span class="sxs-lookup"><span data-stu-id="13bb5-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="13bb5-132">Poprzedni przykład tworzy dzienniki za pomocą `Information` i `Warning` jako *poziom* i `TodoController` klasy *kategorii*.</span><span class="sxs-lookup"><span data-stu-id="13bb5-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="13bb5-133">Dziennik *poziom* wskazuje ważność rejestrowane zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="13bb5-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="13bb5-134">Dziennik *kategorii* jest ciągiem, który jest skojarzony z każdym dzienniku.</span><span class="sxs-lookup"><span data-stu-id="13bb5-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="13bb5-135">`ILogger<T>` Wystąpienie tworzy dzienniki, które mają w pełni kwalifikowaną nazwę typu `T` kategorię.</span><span class="sxs-lookup"><span data-stu-id="13bb5-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="13bb5-136">[Poziomy](#log-level) i [kategorie](#log-category) omówiona bardziej szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="13bb5-137">Twórz dzienniki przy uruchamianiu</span><span class="sxs-lookup"><span data-stu-id="13bb5-137">Create logs in Startup</span></span>

<span data-ttu-id="13bb5-138">Zapisywanie dzienników `Startup` klasy, należy umieścić `ILogger` parametru w sygnatury konstruktora:</span><span class="sxs-lookup"><span data-stu-id="13bb5-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="13bb5-139">Twórz dzienniki w programie</span><span class="sxs-lookup"><span data-stu-id="13bb5-139">Create logs in Program</span></span>

<span data-ttu-id="13bb5-140">Zapisywanie dzienników `Program` klasy, Uzyskaj `ILogger` wystąpienia DI:</span><span class="sxs-lookup"><span data-stu-id="13bb5-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="13bb5-141">Nie metody asynchronicznej rejestratora</span><span class="sxs-lookup"><span data-stu-id="13bb5-141">No asynchronous logger methods</span></span>

<span data-ttu-id="13bb5-142">Rejestrowanie powinno być tak szybko, że nie jest wart spadek wydajności kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="13bb5-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="13bb5-143">Jeśli magazyn danych rejestrowania jest powolne, nie zapisanie w nim bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="13bb5-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="13bb5-144">Należy wziąć pod uwagę początkowo zapisywanie komunikatów dziennika do szybkiego magazynu, a następnie przenieść je do magazynu powolne później.</span><span class="sxs-lookup"><span data-stu-id="13bb5-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="13bb5-145">Na przykład, jeśli masz rejestrowania programu SQL Server nie chcesz to zrobić bezpośrednio w `Log` metody, ponieważ `Log` metody są synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="13bb5-145">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="13bb5-146">Zamiast tego należy synchronicznie Dodawanie dziennika komunikatów do kolejki w pamięci i mieć procesu roboczego tła ściągają komunikaty z kolejki, aby wykonywać pracę asynchroniczną wypychanie danych do programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="13bb5-146">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="13bb5-147">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="13bb5-147">Configuration</span></span>

<span data-ttu-id="13bb5-148">Rejestrowanie dostawcy konfiguracji jest dostarczane przez przynajmniej jednego dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="13bb5-148">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="13bb5-149">Formaty plików (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="13bb5-149">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="13bb5-150">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="13bb5-150">Command-line arguments.</span></span>
* <span data-ttu-id="13bb5-151">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="13bb5-151">Environment variables.</span></span>
* <span data-ttu-id="13bb5-152">Obiekty .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="13bb5-152">In-memory .NET objects.</span></span>
* <span data-ttu-id="13bb5-153">Niezaszyfrowane [Menedżera klucz tajny](xref:security/app-secrets) magazynu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-153">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="13bb5-154">Użytkownik zaszyfrowanych przechowywanych informacji, takich jak [usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="13bb5-154">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="13bb5-155">Dostawcy niestandardowi (zainstalowane lub utworzone).</span><span class="sxs-lookup"><span data-stu-id="13bb5-155">Custom providers (installed or created).</span></span>

<span data-ttu-id="13bb5-156">Na przykład konfiguracja rejestrowania są często dostarczane przez `Logging` części plików ustawień aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-156">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="13bb5-157">W poniższym przykładzie pokazano zawartość typowej *appsettings. Development.JSON* pliku:</span><span class="sxs-lookup"><span data-stu-id="13bb5-157">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

<span data-ttu-id="13bb5-158">`Logging` Właściwość może mieć `LogLevel` dziennika właściwości dostawcy (konsoli znajduje się) i innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="13bb5-158">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="13bb5-159">`LogLevel` Właściwości `Logging` Określa minimalny [poziom](#log-level) logowania dla wybranych kategorii.</span><span class="sxs-lookup"><span data-stu-id="13bb5-159">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="13bb5-160">W tym przykładzie `System` i `Microsoft` kategorie dziennika na `Information` poziom, a wszystkie pozostałe rejestrowania `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="13bb5-160">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="13bb5-161">Inne właściwości w obszarze `Logging` Określ rejestrowania dostawców.</span><span class="sxs-lookup"><span data-stu-id="13bb5-161">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="13bb5-162">W przykładzie występuje dla dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="13bb5-162">The example is for the Console provider.</span></span> <span data-ttu-id="13bb5-163">Jeśli dostawca obsługuje [dziennika zakresy](#log-scopes), `IncludeScopes` wskazuje, czy są włączone.</span><span class="sxs-lookup"><span data-stu-id="13bb5-163">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="13bb5-164">Właściwość dostawcy (takich jak `Console` w przykładzie) mogą także określić `LogLevel` właściwości.</span><span class="sxs-lookup"><span data-stu-id="13bb5-164">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="13bb5-165">`LogLevel` w obszarze dostawcy określa poziomy dziennika dla tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-165">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="13bb5-166">Jeśli nie określono poziomy w `Logging.{providername}.LogLevel`, że zastępują one nic w `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="13bb5-166">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

<span data-ttu-id="13bb5-167">`LogLevel` klucze reprezentują nazwy dziennika.</span><span class="sxs-lookup"><span data-stu-id="13bb5-167">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="13bb5-168">`Default` Klucz ma zastosowanie do dzienników, które nie zostały jawnie wymienione.</span><span class="sxs-lookup"><span data-stu-id="13bb5-168">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="13bb5-169">Reprezentuje wartość [poziom dziennika](#log-level) stosowane do danego dziennika.</span><span class="sxs-lookup"><span data-stu-id="13bb5-169">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="13bb5-170">Aby uzyskać informacje dotyczące implementowania dostawcy konfiguracji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="13bb5-170">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="13bb5-171">Przykładowe dane wyjściowe z rejestrowania</span><span class="sxs-lookup"><span data-stu-id="13bb5-171">Sample logging output</span></span>

<span data-ttu-id="13bb5-172">Za pomocą przykładowego kodu pokazano w poprzedniej sekcji dzienniki są wyświetlane w konsoli, gdy aplikacja jest uruchamiana z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="13bb5-172">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="13bb5-173">Oto przykład danych wyjściowych konsoli:</span><span class="sxs-lookup"><span data-stu-id="13bb5-173">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

<span data-ttu-id="13bb5-174">Poprzedni dzienniki wygenerowane przez wysłał żądanie HTTP Get do przykładowej aplikacji pod `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="13bb5-174">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="13bb5-175">Oto przykład tego samego dzienników, w jakiej występują w oknie Debugowanie Uruchom przykładową aplikację w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="13bb5-175">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="13bb5-176">Dzienniki, które są tworzone przez `ILogger` wywołania pokazano w poprzedniej sekcji zaczyna się od "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="13bb5-176">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="13bb5-177">Dzienniki, które zaczynają się od "Microsoft" kategorie pochodzą z kodu struktury programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13bb5-177">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="13bb5-178">Platforma ASP.NET Core i kodu aplikacji korzystają z tego samego interfejsu API rejestrowania i dostawców.</span><span class="sxs-lookup"><span data-stu-id="13bb5-178">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="13bb5-179">W dalszej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-179">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="13bb5-180">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="13bb5-180">NuGet packages</span></span>

<span data-ttu-id="13bb5-181">`ILogger` i `ILoggerFactory` interfejsy są w [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), i znajdują się w domyślnej implementacji dla nich [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="13bb5-181">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="13bb5-182">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="13bb5-182">Log category</span></span>

<span data-ttu-id="13bb5-183">Gdy `ILogger` obiekt zostanie utworzony, *kategorii* określono dla niej.</span><span class="sxs-lookup"><span data-stu-id="13bb5-183">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="13bb5-184">Tej kategorii jest uwzględnione w każdej wiadomości dziennika utworzone przez to wystąpienie `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="13bb5-184">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="13bb5-185">Kategoria może być dowolnym ciągiem, ale Konwencji jest użycie nazwy klasy, takie jak "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="13bb5-185">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="13bb5-186">Użyj `ILogger<T>` można pobrać `ILogger` wystąpienie, które korzysta z w pełni kwalifikowana nazwa typu z `T` kategorię:</span><span class="sxs-lookup"><span data-stu-id="13bb5-186">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="13bb5-187">Aby jawnie określić kategorię, należy wywołać `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="13bb5-187">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="13bb5-188">`ILogger<T>` jest równoważne z wywoływaniem `CreateLogger` z w pełni kwalifikowana nazwa typu z `T`.</span><span class="sxs-lookup"><span data-stu-id="13bb5-188">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="13bb5-189">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="13bb5-189">Log level</span></span>

<span data-ttu-id="13bb5-190">Każdy dziennik Określa <xref:Microsoft.Extensions.Logging.LogLevel> wartość.</span><span class="sxs-lookup"><span data-stu-id="13bb5-190">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="13bb5-191">Poziom dziennika wskazuje ważność lub ważność.</span><span class="sxs-lookup"><span data-stu-id="13bb5-191">The log level indicates the severity or importance.</span></span> <span data-ttu-id="13bb5-192">Na przykład można napisać `Information` logowania, jeśli metoda zakończy się normalnie, a jeśli tak, to a `Warning` Zaloguj się, gdy metoda zwróci wartość *404 Nie znaleziono* kod stanu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-192">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="13bb5-193">Poniższy kod tworzy `Information` i `Warning` dzienników:</span><span class="sxs-lookup"><span data-stu-id="13bb5-193">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="13bb5-194">W poprzednim kodzie pierwszy parametr jest [identyfikator zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="13bb5-194">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="13bb5-195">Drugi parametr jest szablon wiadomości z symboli zastępczych dla wartości argumentów dostarczone przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="13bb5-195">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="13bb5-196">Parametry metody są wyjaśnione w [komunikatu sekcji szablonu](#log-message-template) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-196">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="13bb5-197">Metody, które obejmują poziom w nazwie metody logowania (na przykład `LogInformation` i `LogWarning`) są [metody rozszerzenia dla ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="13bb5-197">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="13bb5-198">Wywoływanie tych metod `Log` metody, która przyjmuje `LogLevel` parametru.</span><span class="sxs-lookup"><span data-stu-id="13bb5-198">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="13bb5-199">Możesz wywołać `Log` bezpośrednio zamiast jednej z tych metod rozszerzenia, ale składnia jest stosunkowo skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="13bb5-199">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="13bb5-200">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.Extensions.Logging.ILogger> i [kod źródłowy rozszerzenia rejestratora](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="13bb5-200">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="13bb5-201">Platforma ASP.NET Core definiuje następujące poziomy dziennika, w tym miejscu uporządkowane od najniższej do najwyższej ważności.</span><span class="sxs-lookup"><span data-stu-id="13bb5-201">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="13bb5-202">Śledzenie = 0</span><span class="sxs-lookup"><span data-stu-id="13bb5-202">Trace = 0</span></span>

  <span data-ttu-id="13bb5-203">Aby uzyskać informacje, które są zazwyczaj przydatne tylko w przypadku debugowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-203">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="13bb5-204">Te komunikaty mogą zawierać dane poufne aplikacji i dlatego nie można włączyć w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="13bb5-204">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="13bb5-205">*Domyślnie wyłączone.*</span><span class="sxs-lookup"><span data-stu-id="13bb5-205">*Disabled by default.*</span></span>

* <span data-ttu-id="13bb5-206">Debugowanie = 1</span><span class="sxs-lookup"><span data-stu-id="13bb5-206">Debug = 1</span></span>

  <span data-ttu-id="13bb5-207">Aby uzyskać informacje, które mogą być użyteczne podczas programowania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-207">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="13bb5-208">Przykład: `Entering method Configure with flag set to true.` Włącz `Debug` poziom dzienniki w środowisku produkcyjnym, tylko wtedy, gdy rozwiązywania problemów, z powodu dużej liczby dzienników.</span><span class="sxs-lookup"><span data-stu-id="13bb5-208">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="13bb5-209">Informacje o = 2</span><span class="sxs-lookup"><span data-stu-id="13bb5-209">Information = 2</span></span>

  <span data-ttu-id="13bb5-210">Do śledzenia ogólny przebieg aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-210">For tracking the general flow of the app.</span></span> <span data-ttu-id="13bb5-211">Te dzienniki są zazwyczaj mają niektóre wartości długoterminowe.</span><span class="sxs-lookup"><span data-stu-id="13bb5-211">These logs typically have some long-term value.</span></span> <span data-ttu-id="13bb5-212">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="13bb5-212">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="13bb5-213">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="13bb5-213">Warning = 3</span></span>

  <span data-ttu-id="13bb5-214">Nietypowe lub nieoczekiwanych zdarzeń w usłudze flow aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-214">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="13bb5-215">Mogą one zawierać błędy lub inne warunki, które nie powodują aplikacji zatrzymać, ale może być konieczne należy zbadać.</span><span class="sxs-lookup"><span data-stu-id="13bb5-215">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="13bb5-216">Obsługiwane wyjątki są spójne użyj `Warning` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="13bb5-216">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="13bb5-217">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="13bb5-217">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="13bb5-218">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="13bb5-218">Error = 4</span></span>

  <span data-ttu-id="13bb5-219">Błędy i wyjątki, które nie mogą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="13bb5-219">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="13bb5-220">Te komunikaty wskazują wystąpił błąd podczas bieżącego działania lub operacji (takie jak bieżące żądanie HTTP), nie wystąpił błąd całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-220">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="13bb5-221">Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="13bb5-221">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="13bb5-222">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="13bb5-222">Critical = 5</span></span>

  <span data-ttu-id="13bb5-223">Dla błędów, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="13bb5-223">For failures that require immediate attention.</span></span> <span data-ttu-id="13bb5-224">Przykłady: utratą danych, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="13bb5-224">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="13bb5-225">Umożliwia kontrolowanie, ile dane wyjściowe dziennika są zapisywane na nośniku określonego poziomu dziennika lub wyświetlić okno.</span><span class="sxs-lookup"><span data-stu-id="13bb5-225">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="13bb5-226">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="13bb5-226">For example:</span></span>

* <span data-ttu-id="13bb5-227">W środowisku produkcyjnym, należy wysłać `Trace` za pośrednictwem `Information` poziomu do woluminu magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="13bb5-227">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="13bb5-228">Wyślij `Warning` za pośrednictwem `Critical` danych wartość przechowywania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-228">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="13bb5-229">Podczas tworzenia aplikacji, Wyślij `Warning` za pośrednictwem `Critical` do konsoli i Dodaj `Trace` za pośrednictwem `Information` podczas rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="13bb5-229">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="13bb5-230">[Filtrowanie dziennika](#log-filtering) sekcję w dalszej części tego artykułu opisano sposób kontrolowania poziomy dziennika, który dostawca obsługuje.</span><span class="sxs-lookup"><span data-stu-id="13bb5-230">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="13bb5-231">Platforma ASP.NET Core zapisuje dzienniki zdarzeń framework.</span><span class="sxs-lookup"><span data-stu-id="13bb5-231">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="13bb5-232">Przykłady dzienników wcześniej w tym artykule wykluczone dzienniki poniżej `Information` poziomie, więc nie `Debug` lub `Trace` poziom Dzienniki zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="13bb5-232">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="13bb5-233">Oto przykład dzienniki konsoli produkowanych przez przykładową aplikację skonfigurowana do wyświetlania `Debug` dzienników:</span><span class="sxs-lookup"><span data-stu-id="13bb5-233">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="13bb5-234">Identyfikator zdarzenia logowania</span><span class="sxs-lookup"><span data-stu-id="13bb5-234">Log event ID</span></span>

<span data-ttu-id="13bb5-235">Można określić w każdym dzienniku *identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="13bb5-235">Each log can specify an *event ID*.</span></span> <span data-ttu-id="13bb5-236">Przykładowa aplikacja robi to przy użyciu zdefiniowanej lokalnie `LoggingEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="13bb5-236">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="13bb5-237">Identyfikator zdarzenia kojarzy zestaw zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="13bb5-237">An event ID associates a set of events.</span></span> <span data-ttu-id="13bb5-238">Na przykład wszystkie dzienniki związane z wyświetlanie listy elementów na stronie może być 1001.</span><span class="sxs-lookup"><span data-stu-id="13bb5-238">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="13bb5-239">Dostawcy logowania mogą być przechowywane identyfikator zdarzenia w polu Identyfikator w komunikacie rejestrowania lub wcale.</span><span class="sxs-lookup"><span data-stu-id="13bb5-239">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="13bb5-240">Dostawca debugowania nie Wyświetla identyfikatory zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="13bb5-240">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="13bb5-241">Konsola dostawca przedstawia identyfikatory zdarzeń w nawiasach kwadratowych po kategorii:</span><span class="sxs-lookup"><span data-stu-id="13bb5-241">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="13bb5-242">Szablon wiadomości dziennika</span><span class="sxs-lookup"><span data-stu-id="13bb5-242">Log message template</span></span>

<span data-ttu-id="13bb5-243">Każdy dziennik Określa szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="13bb5-243">Each log specifies a message template.</span></span> <span data-ttu-id="13bb5-244">Szablon wiadomości mogą zawierać symbole zastępcze, dla których są podane argumenty.</span><span class="sxs-lookup"><span data-stu-id="13bb5-244">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="13bb5-245">Użyj nazw symboli zastępczych, nie liczby.</span><span class="sxs-lookup"><span data-stu-id="13bb5-245">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="13bb5-246">Kolejność symboli zastępczych, nie ich nazwy, określa parametry, które służą do zapewniania ich wartości.</span><span class="sxs-lookup"><span data-stu-id="13bb5-246">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="13bb5-247">W poniższym kodzie Zauważ, że nazwy parametrów są poza kolejnością w szablonie wiadomości:</span><span class="sxs-lookup"><span data-stu-id="13bb5-247">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="13bb5-248">Ten kod tworzy komunikat dziennika przy użyciu wartości parametrów w sekwencji:</span><span class="sxs-lookup"><span data-stu-id="13bb5-248">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="13bb5-249">Struktury rejestrowania działa w ten sposób, aby zaimplementować rejestrowania dostawców [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="13bb5-249">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="13bb5-250">Same argumenty są przekazywane do systemu rejestrowania, a nie tylko szablon sformatowany komunikat.</span><span class="sxs-lookup"><span data-stu-id="13bb5-250">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="13bb5-251">Te informacje umożliwiają dostawcom rejestrowania do przechowywania wartości parametrów jako pola.</span><span class="sxs-lookup"><span data-stu-id="13bb5-251">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="13bb5-252">Załóżmy na przykład, rejestratora metody wywołania się następująco:</span><span class="sxs-lookup"><span data-stu-id="13bb5-252">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="13bb5-253">Jeśli dzienniki jest wysyłana do usługi Azure Table Storage, każda jednostka usługi Azure Table może mieć `ID` i `RequestTime` właściwości, które upraszcza zapytań dotyczących danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="13bb5-253">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="13bb5-254">Zapytania można znaleźć wszystkie dzienniki w ramach określonego `RequestTime` zakresu bez analizowania limit czasu wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="13bb5-254">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="13bb5-255">Rejestrowania wyjątków</span><span class="sxs-lookup"><span data-stu-id="13bb5-255">Logging exceptions</span></span>

<span data-ttu-id="13bb5-256">Metody rejestratora mają przeciążenia, które umożliwiają przekazywanie wyjątek, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="13bb5-256">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="13bb5-257">Różnych dostawców obsługiwać informacje o wyjątku na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="13bb5-257">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="13bb5-258">Oto przykład danych wyjściowych debugowania dostawcy w kodzie pokazanym powyżej.</span><span class="sxs-lookup"><span data-stu-id="13bb5-258">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="13bb5-259">Filtrowanie dziennika</span><span class="sxs-lookup"><span data-stu-id="13bb5-259">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="13bb5-260">Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="13bb5-260">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="13bb5-261">Żadnych dzienników poniżej minimalnego poziomu nie są przekazywane do tego dostawcy, dzięki czemu nie uzyskać wyświetlane lub przechowywane.</span><span class="sxs-lookup"><span data-stu-id="13bb5-261">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="13bb5-262">Aby pominąć wszystkie dzienniki, określ `LogLevel.None` jako minimalny poziom rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-262">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="13bb5-263">Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="13bb5-263">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="13bb5-264">Tworzenie reguły filtrów w konfiguracji</span><span class="sxs-lookup"><span data-stu-id="13bb5-264">Create filter rules in configuration</span></span>

<span data-ttu-id="13bb5-265">Kod wywołuje szablon projektu `CreateDefaultBuilder` skonfigurować rejestrowanie dla dostawców konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-265">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="13bb5-266">`CreateDefaultBuilder` Metoda konfiguruje również rejestrowania do wyszukania konfiguracji w `Logging` sekcji przy użyciu kodu, podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="13bb5-266">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="13bb5-267">Dane konfiguracji Określa poziomy minimalna dziennika przez dostawcę i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="13bb5-267">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="13bb5-268">Ten kod JSON tworzy sześć reguły filtru: jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jeden dla wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="13bb5-268">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="13bb5-269">Wybrano jedną regułę dla każdego dostawcy podczas `ILogger` obiekt zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="13bb5-269">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="13bb5-270">Reguły filtrowania w kodzie</span><span class="sxs-lookup"><span data-stu-id="13bb5-270">Filter rules in code</span></span>

<span data-ttu-id="13bb5-271">Poniższy przykład pokazuje, jak zarejestrować reguły filtrów w kodzie:</span><span class="sxs-lookup"><span data-stu-id="13bb5-271">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="13bb5-272">Drugi `AddFilter` określa dostawcę debugowania przy użyciu jego nazwy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-272">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="13bb5-273">Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określa typ dostawcy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-273">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="13bb5-274">Jak reguły filtrowania zostaną zastosowane.</span><span class="sxs-lookup"><span data-stu-id="13bb5-274">How filtering rules are applied</span></span>

<span data-ttu-id="13bb5-275">Dane konfiguracji i `AddFilter` kod przedstawiony w powyższych przykładach Tworzenie reguły pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="13bb5-275">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="13bb5-276">Pierwsze sześć pochodzą przykład konfiguracji i ostatnie dwa pochodzą w przykładzie kodu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-276">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="13bb5-277">Wartość liczbowa</span><span class="sxs-lookup"><span data-stu-id="13bb5-277">Number</span></span> | <span data-ttu-id="13bb5-278">Dostawca</span><span class="sxs-lookup"><span data-stu-id="13bb5-278">Provider</span></span>      | <span data-ttu-id="13bb5-279">Kategorie, które zaczynają się od...</span><span class="sxs-lookup"><span data-stu-id="13bb5-279">Categories that begin with ...</span></span>          | <span data-ttu-id="13bb5-280">Minimalny poziom rejestrowania</span><span class="sxs-lookup"><span data-stu-id="13bb5-280">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="13bb5-281">1</span><span class="sxs-lookup"><span data-stu-id="13bb5-281">1</span></span>      | <span data-ttu-id="13bb5-282">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="13bb5-282">Debug</span></span>         | <span data-ttu-id="13bb5-283">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="13bb5-283">All categories</span></span>                          | <span data-ttu-id="13bb5-284">Informacje</span><span class="sxs-lookup"><span data-stu-id="13bb5-284">Information</span></span>       |
| <span data-ttu-id="13bb5-285">2</span><span class="sxs-lookup"><span data-stu-id="13bb5-285">2</span></span>      | <span data-ttu-id="13bb5-286">Konsola</span><span class="sxs-lookup"><span data-stu-id="13bb5-286">Console</span></span>       | <span data-ttu-id="13bb5-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="13bb5-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="13bb5-288">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="13bb5-288">Warning</span></span>           |
| <span data-ttu-id="13bb5-289">3</span><span class="sxs-lookup"><span data-stu-id="13bb5-289">3</span></span>      | <span data-ttu-id="13bb5-290">Konsola</span><span class="sxs-lookup"><span data-stu-id="13bb5-290">Console</span></span>       | <span data-ttu-id="13bb5-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="13bb5-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="13bb5-292">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="13bb5-292">Debug</span></span>             |
| <span data-ttu-id="13bb5-293">4</span><span class="sxs-lookup"><span data-stu-id="13bb5-293">4</span></span>      | <span data-ttu-id="13bb5-294">Konsola</span><span class="sxs-lookup"><span data-stu-id="13bb5-294">Console</span></span>       | <span data-ttu-id="13bb5-295">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="13bb5-295">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="13bb5-296">Błąd</span><span class="sxs-lookup"><span data-stu-id="13bb5-296">Error</span></span>             |
| <span data-ttu-id="13bb5-297">5</span><span class="sxs-lookup"><span data-stu-id="13bb5-297">5</span></span>      | <span data-ttu-id="13bb5-298">Konsola</span><span class="sxs-lookup"><span data-stu-id="13bb5-298">Console</span></span>       | <span data-ttu-id="13bb5-299">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="13bb5-299">All categories</span></span>                          | <span data-ttu-id="13bb5-300">Informacje</span><span class="sxs-lookup"><span data-stu-id="13bb5-300">Information</span></span>       |
| <span data-ttu-id="13bb5-301">6</span><span class="sxs-lookup"><span data-stu-id="13bb5-301">6</span></span>      | <span data-ttu-id="13bb5-302">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="13bb5-302">All providers</span></span> | <span data-ttu-id="13bb5-303">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="13bb5-303">All categories</span></span>                          | <span data-ttu-id="13bb5-304">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="13bb5-304">Debug</span></span>             |
| <span data-ttu-id="13bb5-305">7</span><span class="sxs-lookup"><span data-stu-id="13bb5-305">7</span></span>      | <span data-ttu-id="13bb5-306">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="13bb5-306">All providers</span></span> | <span data-ttu-id="13bb5-307">System</span><span class="sxs-lookup"><span data-stu-id="13bb5-307">System</span></span>                                  | <span data-ttu-id="13bb5-308">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="13bb5-308">Debug</span></span>             |
| <span data-ttu-id="13bb5-309">8</span><span class="sxs-lookup"><span data-stu-id="13bb5-309">8</span></span>      | <span data-ttu-id="13bb5-310">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="13bb5-310">Debug</span></span>         | <span data-ttu-id="13bb5-311">Microsoft</span><span class="sxs-lookup"><span data-stu-id="13bb5-311">Microsoft</span></span>                               | <span data-ttu-id="13bb5-312">Śledzenia</span><span class="sxs-lookup"><span data-stu-id="13bb5-312">Trace</span></span>             |

<span data-ttu-id="13bb5-313">Gdy `ILogger` obiekt zostanie utworzony, `ILoggerFactory` obiektu wybiera jedną regułę na dostawcy, aby zastosować do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="13bb5-313">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="13bb5-314">Wszystkie komunikaty napisane przez `ILogger` wystąpienia są filtrowane w zależności od wybranej reguły.</span><span class="sxs-lookup"><span data-stu-id="13bb5-314">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="13bb5-315">Najbardziej określonej reguły dla każdego dostawcy i pary kategoria możliwe jest wybrana w zaufanym dostępne reguły.</span><span class="sxs-lookup"><span data-stu-id="13bb5-315">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="13bb5-316">Następujące algorytm jest używany dla każdego dostawcy podczas `ILogger` jest tworzona dla danej kategorii:</span><span class="sxs-lookup"><span data-stu-id="13bb5-316">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="13bb5-317">Wybierz wszystkie reguły, które odpowiadają przez dostawcę lub jego alias.</span><span class="sxs-lookup"><span data-stu-id="13bb5-317">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="13bb5-318">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły przy użyciu dostawcy puste.</span><span class="sxs-lookup"><span data-stu-id="13bb5-318">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="13bb5-319">W wyniku poprzedniego kroku wybierz reguły z najdłużej dopasowania prefiksu kategorii.</span><span class="sxs-lookup"><span data-stu-id="13bb5-319">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="13bb5-320">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="13bb5-320">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="13bb5-321">Jeśli wybrano wiele reguł, **ostatniego** jeden.</span><span class="sxs-lookup"><span data-stu-id="13bb5-321">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="13bb5-322">Jeśli nie zaznaczono żadnych reguł, użyj `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="13bb5-322">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="13bb5-323">Przy użyciu poprzednich listę reguł, załóżmy, że możesz utworzyć `ILogger` obiektu dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="13bb5-323">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="13bb5-324">W przypadku dostawcy debugowania mają zastosowanie reguły 1, 6 i 8.</span><span class="sxs-lookup"><span data-stu-id="13bb5-324">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="13bb5-325">Reguła 8 jest najbardziej specyficzną, więc to jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="13bb5-325">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="13bb5-326">W przypadku dostawcy konsoli mają zastosowanie reguły, 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="13bb5-326">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="13bb5-327">Reguła 3 jest bardziej konkretny od pozostałych.</span><span class="sxs-lookup"><span data-stu-id="13bb5-327">Rule 3 is most specific.</span></span>

<span data-ttu-id="13bb5-328">Wynikowy `ILogger` wystąpienia wysyła dzienniki `Trace` poziom i nowsze wersje do debugowania dostawcy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-328">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="13bb5-329">Dzienniki programu `Debug` poziomu i nowszych są wysyłane do dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="13bb5-329">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="13bb5-330">Aliasy dostawcy</span><span class="sxs-lookup"><span data-stu-id="13bb5-330">Provider aliases</span></span>

<span data-ttu-id="13bb5-331">Każdy dostawca definiuje *alias* które mogą być używane w konfiguracji zamiast w pełni kwalifikowana nazwa typu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-331">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="13bb5-332">W przypadku dostawców wbudowanych należy stosować następujące aliasy:</span><span class="sxs-lookup"><span data-stu-id="13bb5-332">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="13bb5-333">Konsola</span><span class="sxs-lookup"><span data-stu-id="13bb5-333">Console</span></span>
* <span data-ttu-id="13bb5-334">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="13bb5-334">Debug</span></span>
* <span data-ttu-id="13bb5-335">EventSource</span><span class="sxs-lookup"><span data-stu-id="13bb5-335">EventSource</span></span>
* <span data-ttu-id="13bb5-336">Dziennik zdarzeń</span><span class="sxs-lookup"><span data-stu-id="13bb5-336">EventLog</span></span>
* <span data-ttu-id="13bb5-337">TraceSource</span><span class="sxs-lookup"><span data-stu-id="13bb5-337">TraceSource</span></span>
* <span data-ttu-id="13bb5-338">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="13bb5-338">AzureAppServicesFile</span></span>
* <span data-ttu-id="13bb5-339">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="13bb5-339">AzureAppServicesBlob</span></span>
* <span data-ttu-id="13bb5-340">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="13bb5-340">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="13bb5-341">Minimalny poziom domyślny</span><span class="sxs-lookup"><span data-stu-id="13bb5-341">Default minimum level</span></span>

<span data-ttu-id="13bb5-342">Istnieje minimalne ustawienie poziomie staje się skuteczny tylko wtedy, gdy nie z konfiguracji czy kodu reguły dla danego dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="13bb5-342">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="13bb5-343">Poniższy przykład pokazuje, jak ustawić minimalny poziom:</span><span class="sxs-lookup"><span data-stu-id="13bb5-343">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="13bb5-344">Jeśli nie zostanie jawnie ustawiona minimalny poziom, wartością domyślną jest `Information`, co oznacza, że `Trace` i `Debug` dzienniki są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="13bb5-344">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="13bb5-345">Funkcje filtrowania</span><span class="sxs-lookup"><span data-stu-id="13bb5-345">Filter functions</span></span>

<span data-ttu-id="13bb5-346">Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorie, które nie mają zasady przypisane do nich przez konfiguracji czy kodu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-346">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="13bb5-347">Kod w funkcji ma dostęp do typu Dostawca, kategoria i poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="13bb5-347">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="13bb5-348">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="13bb5-348">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="13bb5-349">Niektórzy dostawcy rejestrowania umożliwiają określenie, kiedy zapisany na nośniku lub ignorowane dzienniki na podstawie poziomu dziennika i kategorii.</span><span class="sxs-lookup"><span data-stu-id="13bb5-349">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="13bb5-350">`AddConsole` i `AddDebug` metody rozszerzenia, zapewnienia przeciążenia, które akceptują kryteria filtrowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-350">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="13bb5-351">Następujący przykładowy kod powoduje, że Konsola dostawca ignorowanie dzienniki poniżej `Warning` poziomu, podczas gdy dostawca debugowania ignoruje dzienniki tworzonych w ramach.</span><span class="sxs-lookup"><span data-stu-id="13bb5-351">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="13bb5-352">`AddEventLog` Metoda ma przeciążenia, które przyjmuje `EventLogSettings` wystąpienia, co może zawierać funkcji filtrowania w jego `Filter` właściwości.</span><span class="sxs-lookup"><span data-stu-id="13bb5-352">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="13bb5-353">Dostawca TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jej poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` używa.</span><span class="sxs-lookup"><span data-stu-id="13bb5-353">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="13bb5-354">Aby ustawić reguły filtrowania dla wszystkich dostawców, które są zarejestrowane w usłudze `ILoggerFactory` wystąpienia, należy użyć `WithFilter` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="13bb5-354">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="13bb5-355">Poniższy przykład ogranicza framework dzienniki (kategoria zaczyna się od "Microsoft" lub "System") ostrzeżeń podczas logowania się na poziomie debugowania dla dzienników tworzona przez kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-355">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="13bb5-356">Aby zapobiec sytuacji, w której wszelkie dzienniki zapisywana, podaj `LogLevel.None` jako minimalny poziom rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-356">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="13bb5-357">Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="13bb5-357">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="13bb5-358">`WithFilter` Metody rozszerzenia są dostarczane przez [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="13bb5-358">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="13bb5-359">Metoda ta zwraca nowy `ILoggerFactory` zarejestrowano wystąpienia, która będzie filtrować komunikaty dziennika, przekazywane do wszystkich dostawców rejestratora.</span><span class="sxs-lookup"><span data-stu-id="13bb5-359">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="13bb5-360">Nie wpływa na inne `ILoggerFactory` wystąpienia, w tym oryginalny `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="13bb5-360">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="13bb5-361">Kategorie systemu i poziomy</span><span class="sxs-lookup"><span data-stu-id="13bb5-361">System categories and levels</span></span>

<span data-ttu-id="13bb5-362">Poniżej przedstawiono niektóre kategorie używane przez platformy ASP.NET Core i Entity Framework Core, z uwagi na temat co dzienniki można oczekiwać od nich:</span><span class="sxs-lookup"><span data-stu-id="13bb5-362">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="13bb5-363">Kategoria</span><span class="sxs-lookup"><span data-stu-id="13bb5-363">Category</span></span>                            | <span data-ttu-id="13bb5-364">Uwagi</span><span class="sxs-lookup"><span data-stu-id="13bb5-364">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="13bb5-365">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="13bb5-365">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="13bb5-366">Diagnostyka ogólnego ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13bb5-366">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="13bb5-367">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="13bb5-367">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="13bb5-368">Klucze, które zostały uznane za znaleziono i używane.</span><span class="sxs-lookup"><span data-stu-id="13bb5-368">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="13bb5-369">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="13bb5-369">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="13bb5-370">Hosty dozwolone.</span><span class="sxs-lookup"><span data-stu-id="13bb5-370">Hosts allowed.</span></span> |
| <span data-ttu-id="13bb5-371">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="13bb5-371">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="13bb5-372">Ile żądań HTTP potrzebny do ukończenia, a także czas rozpoczęcia pracy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-372">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="13bb5-373">Zestawy startowe, które hostingu zostały załadowane.</span><span class="sxs-lookup"><span data-stu-id="13bb5-373">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="13bb5-374">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="13bb5-374">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="13bb5-375">Diagnostyka MVC i Razor.</span><span class="sxs-lookup"><span data-stu-id="13bb5-375">MVC and Razor diagnostics.</span></span> <span data-ttu-id="13bb5-376">Powiązanie modelu, wykonywanie filtru, kompilacja widoku wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-376">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="13bb5-377">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="13bb5-377">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="13bb5-378">Trasę odpowiednią dla informacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-378">Route matching information.</span></span> |
| <span data-ttu-id="13bb5-379">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="13bb5-379">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="13bb5-380">Rozpoczęcie połączenia zatrzymać i zachować aktywności odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="13bb5-380">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="13bb5-381">Informacje o certyfikacie protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="13bb5-381">HTTPS certificate information.</span></span> |
| <span data-ttu-id="13bb5-382">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="13bb5-382">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="13bb5-383">Obsługiwane pliki.</span><span class="sxs-lookup"><span data-stu-id="13bb5-383">Files served.</span></span> |
| <span data-ttu-id="13bb5-384">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="13bb5-384">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="13bb5-385">Ogólne Diagnostyka platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="13bb5-385">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="13bb5-386">Działanie i konfiguracji wykrywania zmian migracje baz danych.</span><span class="sxs-lookup"><span data-stu-id="13bb5-386">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="13bb5-387">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="13bb5-387">Log scopes</span></span>

 <span data-ttu-id="13bb5-388">A *zakres* można grupować zestaw operacji logicznej.</span><span class="sxs-lookup"><span data-stu-id="13bb5-388">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="13bb5-389">Ta metoda grupowania może służyć do dołączenia do tych samych danych do każdego dziennika, który jest tworzony jako część zestawu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-389">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="13bb5-390">Na przykład każdy dziennik utworzony jako część przetwarzania transakcji może zawierać identyfikator transakcji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-390">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="13bb5-391">Zakres jest `IDisposable` typu, który jest zwracany przez <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> metody i trwa do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="13bb5-391">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="13bb5-392">Użyj zakresu opakowując wywołania rejestratora w `using` bloku:</span><span class="sxs-lookup"><span data-stu-id="13bb5-392">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="13bb5-393">Poniższy kod umożliwia zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="13bb5-393">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="13bb5-394">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="13bb5-394">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="13bb5-395">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-395">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="13bb5-396">Aby uzyskać informacji na temat konfigurowania, zobacz [konfiguracji](#configuration) sekcji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-396">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="13bb5-397">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="13bb5-397">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="13bb5-398">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-398">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="13bb5-399">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="13bb5-399">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="13bb5-400">Każdy komunikat dziennika zawiera informacje o określonym zakresie:</span><span class="sxs-lookup"><span data-stu-id="13bb5-400">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="13bb5-401">Wbudowane funkcje rejestrowania dostawców</span><span class="sxs-lookup"><span data-stu-id="13bb5-401">Built-in logging providers</span></span>

<span data-ttu-id="13bb5-402">ASP.NET Core jest dostarczana w następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="13bb5-402">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="13bb5-403">Console</span><span class="sxs-lookup"><span data-stu-id="13bb5-403">Console</span></span>](#console-provider)
* [<span data-ttu-id="13bb5-404">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="13bb5-404">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="13bb5-405">EventSource</span><span class="sxs-lookup"><span data-stu-id="13bb5-405">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="13bb5-406">EventLog</span><span class="sxs-lookup"><span data-stu-id="13bb5-406">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="13bb5-407">TraceSource</span><span class="sxs-lookup"><span data-stu-id="13bb5-407">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="13bb5-408">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="13bb5-408">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="13bb5-409">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="13bb5-409">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="13bb5-410">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="13bb5-410">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="13bb5-411">Informacje dotyczące rejestrowania strumienia wyjściowego stdout, zobacz <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> i <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="13bb5-411">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="13bb5-412">Konsola dostawcy</span><span class="sxs-lookup"><span data-stu-id="13bb5-412">Console provider</span></span>

<span data-ttu-id="13bb5-413">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakiet dostawcy wysyła dane wyjściowe dziennika do konsoli.</span><span class="sxs-lookup"><span data-stu-id="13bb5-413">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="13bb5-414">[Przeciążenia AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) pozwalają przekazać minimalny poziom rejestrowania, funkcję filtru i atrybut typu wartość logiczna, która wskazuje, czy zakresy są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="13bb5-414">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="13bb5-415">Innym rozwiązaniem jest przekazywanie `IConfiguration` obiektu, który można określić zakresy pomocy technicznej i poziomów rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-415">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="13bb5-416">Dostawca konsola ma znaczący wpływ na wydajność i zwykle nie jest przeznaczone do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="13bb5-416">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="13bb5-417">Podczas tworzenia nowego projektu w programie Visual Studio, `AddConsole` metoda wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="13bb5-417">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="13bb5-418">Ten kod, który odwołuje się do `Logging` części *appSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="13bb5-418">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="13bb5-419">Ustawienia pokazano limit framework dzienniki, aby ostrzeżenia zezwalając aplikacji do logowania na poziomie debugowania, jak wyjaśniono w [filtrowanie dziennika](#log-filtering) sekcji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-419">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="13bb5-420">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="13bb5-420">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="13bb5-421">Aby wyświetlić rejestrowania danych wyjściowych konsoli, otwórz wiersz polecenia w folderze projektu, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="13bb5-421">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="13bb5-422">Debugowanie dostawcy</span><span class="sxs-lookup"><span data-stu-id="13bb5-422">Debug provider</span></span>

<span data-ttu-id="13bb5-423">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakiet dostawcy zapisuje dane wyjściowe dziennika za pomocą [system.Diagnostics.Debug —](/dotnet/api/system.diagnostics.debug) klasy (`Debug.WriteLine` wywołania metody).</span><span class="sxs-lookup"><span data-stu-id="13bb5-423">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="13bb5-424">W systemie Linux, tego dostawcy zapisuje dzienniki na */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="13bb5-424">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="13bb5-425">[Przeciążenia AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) pozwalają przekazać minimalny poziom rejestrowania lub funkcję filtru.</span><span class="sxs-lookup"><span data-stu-id="13bb5-425">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="13bb5-426">Dostawca źródła zdarzeń</span><span class="sxs-lookup"><span data-stu-id="13bb5-426">EventSource provider</span></span>

<span data-ttu-id="13bb5-427">W przypadku aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszym, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zaimplementować pakiet dostawcy śledzenia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="13bb5-427">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="13bb5-428">W Windows, używa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="13bb5-428">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="13bb5-429">Dostawca jest dla wielu platform, ale nie są dostępne żadne zdarzenie gromadzenia i wyświetlania narzędzia jeszcze dla systemu Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="13bb5-429">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

<span data-ttu-id="13bb5-430">Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [narzędzia PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="13bb5-430">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="13bb5-431">Istnieją inne narzędzia umożliwiające wyświetlanie dzienników zdarzeń systemu Windows, ale narzędzia PerfView zapewnia najlepsze środowisko do pracy z zdarzenia ETW emitowane przez platformę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13bb5-431">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="13bb5-432">Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, Dodaj parametry `*Microsoft-Extensions-Logging` do **dodatkowych dostawców** listy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-432">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="13bb5-433">(Nie przegap gwiazdki na początku ciągu).</span><span class="sxs-lookup"><span data-stu-id="13bb5-433">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia Perfview dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="13bb5-435">Dostawca dziennika zdarzeń Windows</span><span class="sxs-lookup"><span data-stu-id="13bb5-435">Windows EventLog provider</span></span>

<span data-ttu-id="13bb5-436">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pakiet dostawcy wysyła dane wyjściowe dziennika w dzienniku zdarzeń Windows.</span><span class="sxs-lookup"><span data-stu-id="13bb5-436">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="13bb5-437">[Przeciążenia AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) umożliwiają przekazanej `EventLogSettings` lub minimalny poziom rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-437">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="13bb5-438">TraceSource dostawcy</span><span class="sxs-lookup"><span data-stu-id="13bb5-438">TraceSource provider</span></span>

<span data-ttu-id="13bb5-439">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa dostawcy pakietu <xref:System.Diagnostics.TraceSource> bibliotek i dostawców.</span><span class="sxs-lookup"><span data-stu-id="13bb5-439">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="13bb5-440">[Przeciążenia AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) pozwalają przekazać przełącznik źródła i odbiornika śledzenia.</span><span class="sxs-lookup"><span data-stu-id="13bb5-440">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="13bb5-441">Aby użyć tego dostawcy, aplikacja ma do uruchamiania na .NET Framework (a nie .NET Core).</span><span class="sxs-lookup"><span data-stu-id="13bb5-441">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="13bb5-442">Dostawca komunikaty można kierować do różnych [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takie jak <xref:System.Diagnostics.TextWriterTraceListener> używane w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-442">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="13bb5-443">Poniższy przykład umożliwia skonfigurowanie `TraceSource` dostawcy, która rejestruje `Warning` i wyższych komunikaty w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="13bb5-443">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="13bb5-444">Dostawca usługi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="13bb5-444">Azure App Service provider</span></span>

<span data-ttu-id="13bb5-445">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pakiet dostawcy zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji w usłudze Azure App Service i do [magazynu obiektów blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="13bb5-445">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="13bb5-446">Pakiet dostawcy jest dostępna dla aplikacji przeznaczonych dla platformy .NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="13bb5-446">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="13bb5-447">Jeśli przeznaczony dla platformy .NET Core, pamiętaj o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="13bb5-447">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="13bb5-448">Pakiet dostawcy znajduje się w programie ASP.NET Core [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="13bb5-448">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="13bb5-449">Pakiet dostawcy nie jest zawarty w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="13bb5-449">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="13bb5-450">Aby użyć dostawcy, zainstaluj pakiet.</span><span class="sxs-lookup"><span data-stu-id="13bb5-450">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="13bb5-451">Jeśli przeznaczonych dla platformy .NET Framework lub odwołuje się do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, Dodaj pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-451">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="13bb5-452">Wywoływanie `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="13bb5-452">Invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="13bb5-453"><xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> Przeciążenia umożliwia przekazywanie w <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="13bb5-453">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="13bb5-454">Obiekt ustawień można zastąpić domyślne ustawienia, takie jak rejestrowanie danych wyjściowych szablonu, nazwa obiektu blob i limit rozmiaru pliku.</span><span class="sxs-lookup"><span data-stu-id="13bb5-454">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="13bb5-455">(*Danych wyjściowych szablonu* jest szablon wiadomości, która jest stosowana do wszystkich dzienników oprócz co to jest dostarczana z `ILogger` wywołania metody.)</span><span class="sxs-lookup"><span data-stu-id="13bb5-455">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="13bb5-456">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="13bb5-456">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="13bb5-457">Podczas wdrażania aplikacji usługi App Service aplikacja honoruje ustawienia w [usługi App Service, dzienniki](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) części **usługi App Service** strony w witrynie Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="13bb5-457">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="13bb5-458">Po zaktualizowaniu następujące ustawienia zmiany zaczynają obowiązywać natychmiast bez konieczności ponownego uruchomienia lub ponownego wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-458">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="13bb5-459">**Rejestrowanie aplikacji (system plików)**</span><span class="sxs-lookup"><span data-stu-id="13bb5-459">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="13bb5-460">**Rejestrowanie aplikacji (Blob)**</span><span class="sxs-lookup"><span data-stu-id="13bb5-460">**Application Logging (Blob)**</span></span>

<span data-ttu-id="13bb5-461">Domyślną lokalizacją dla plików dziennika jest *D:\\macierzystego\\LogFiles\\aplikacji* folder i nazwę pliku domyślny jest *yyyymmdd.txt diagnostyki*.</span><span class="sxs-lookup"><span data-stu-id="13bb5-461">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="13bb5-462">Domyślnego limitu rozmiaru pliku to 10 MB, a maksymalną domyślną liczbę plików, które przechowywane jest 2.</span><span class="sxs-lookup"><span data-stu-id="13bb5-462">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="13bb5-463">Domyślna nazwa obiektu blob to *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="13bb5-463">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="13bb5-464">Dostawca działa tylko w przypadku, gdy projekt jest uruchamiany w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="13bb5-464">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="13bb5-465">Nie ma wpływu, gdy projekt jest uruchamiany lokalnie&mdash;nie zapisywać pliki lokalne lub lokalnym magazynem projektowym dla obiektów blob.</span><span class="sxs-lookup"><span data-stu-id="13bb5-465">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="13bb5-466">Przesyłanie strumieniowe dzienników platformy Azure</span><span class="sxs-lookup"><span data-stu-id="13bb5-466">Azure log streaming</span></span>

<span data-ttu-id="13bb5-467">Przesyłanie strumieniowe dzienników platformy Azure umożliwia wyświetlanie dzienników aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="13bb5-467">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="13bb5-468">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="13bb5-468">The app server</span></span>
* <span data-ttu-id="13bb5-469">Serwer sieci web</span><span class="sxs-lookup"><span data-stu-id="13bb5-469">The web server</span></span>
* <span data-ttu-id="13bb5-470">Śledzenie żądań zakończonych niepowodzeniem</span><span class="sxs-lookup"><span data-stu-id="13bb5-470">Failed request tracing</span></span>

<span data-ttu-id="13bb5-471">Aby skonfigurować, przesyłanie strumieniowe dzienników platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="13bb5-471">To configure Azure log streaming:</span></span>

* <span data-ttu-id="13bb5-472">Przejdź do **usługi App Service, dzienniki** strony na stronie portalu swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-472">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="13bb5-473">Ustaw **rejestrowanie aplikacji (system plików)** do **na**.</span><span class="sxs-lookup"><span data-stu-id="13bb5-473">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="13bb5-474">Wybierz dziennik **poziom**.</span><span class="sxs-lookup"><span data-stu-id="13bb5-474">Choose the log **Level**.</span></span>

<span data-ttu-id="13bb5-475">Przejdź do **Stream dziennika** strony, aby wyświetlić komunikaty aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-475">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="13bb5-476">Logowanie przeprowadzono za pomocą aplikacji za pośrednictwem `ILogger` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-476">They're logged by the app through the `ILogger` interface.</span></span>

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="13bb5-477">Rejestrowanie śledzenia w usłudze Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="13bb5-477">Azure Application Insights trace logging</span></span>

<span data-ttu-id="13bb5-478">[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) pakiet dostawcy zapisuje dzienniki do usługi Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="13bb5-478">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="13bb5-479">Application Insights to usługa, która monitoruje aplikację internetową i udostępnia narzędzia do tworzenia zapytań i analizowania danych telemetrycznych.</span><span class="sxs-lookup"><span data-stu-id="13bb5-479">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="13bb5-480">Jeśli używasz tego dostawcy, można zbadać i analizowanie dzienników za pomocą narzędzi usługi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="13bb5-480">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="13bb5-481">Dostawcy logowania jest uwzględniona jako zależą od elementu [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), czyli pakiet, który zawiera wszystkie dostępne dane telemetryczne dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13bb5-481">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="13bb5-482">Jeśli używasz tego pakietu, nie trzeba zainstalować pakiet dostawcy.</span><span class="sxs-lookup"><span data-stu-id="13bb5-482">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="13bb5-483">Nie używaj [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) pakietu&mdash;to ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="13bb5-483">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="13bb5-484">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="13bb5-484">For more information, see the following resources:</span></span>

* [<span data-ttu-id="13bb5-485">Omówienie usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="13bb5-485">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="13bb5-486">[Usługa Application Insights dla aplikacji platformy ASP.NET Core](/azure/azure-monitor/app/asp-net-core-no-visualstudio) — zacznij tutaj, aby zaimplementować pełnego zakresu z telemetria usługi Application Insights wraz z rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="13bb5-486">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core-no-visualstudio) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="13bb5-487">[Rejestruje ApplicationInsightsLoggerProvider dla platformy .NET Core ILogger](/azure/azure-monitor/app/ilogger) — zacznij tutaj, aby zaimplementować dostawcę rejestrowania bez reszty telemetria usługi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="13bb5-487">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="13bb5-488">[Rejestrowanie kart w usłudze Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span><span class="sxs-lookup"><span data-stu-id="13bb5-488">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* <span data-ttu-id="13bb5-489">[Instalowanie, konfigurowanie i zainicjuj zestaw SDK usługi Application Insights](/learn/modules/instrument-web-app-code-with-application-insights) — interaktywnych samouczków witrynie Learn firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="13bb5-489">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>
::: moniker-end

> [!NOTE]
> <span data-ttu-id="13bb5-490">Począwszy od 5/1/2019 artykuł zatytułowany [usługi Application Insights dla platformy ASP.NET Core](/azure/azure-monitor/app/asp-net-core) jest nieaktualne i samouczek kroki nie zadziałają.</span><span class="sxs-lookup"><span data-stu-id="13bb5-490">As of 5/1/2019, the article titled [Application Insights for ASP.NET Core](/azure/azure-monitor/app/asp-net-core) is out of date, and the tutorial steps don't work.</span></span> <span data-ttu-id="13bb5-491">Zapoznaj się [usługi Application Insights dla aplikacji platformy ASP.NET Core](/azure/azure-monitor/app/asp-net-core-no-visualstudio) zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="13bb5-491">Refer to [Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core-no-visualstudio) instead.</span></span> <span data-ttu-id="13bb5-492">Firma Microsoft zdawali sobie sprawę z problemu i jego tego problemu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-492">We are aware of the issue and are working to correct it.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="13bb5-493">Rejestrowanie innych dostawców</span><span class="sxs-lookup"><span data-stu-id="13bb5-493">Third-party logging providers</span></span>

<span data-ttu-id="13bb5-494">Struktury rejestrowania innych firm, które działają z platformą ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="13bb5-494">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="13bb5-495">[elmah.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="13bb5-495">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="13bb5-496">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="13bb5-496">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="13bb5-497">[JSNLog](http://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="13bb5-497">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="13bb5-498">[KissLog.net](https://kisslog.net/) ([repozytorium GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="13bb5-498">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="13bb5-499">[Loggr](http://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="13bb5-499">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="13bb5-500">[NLog](http://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="13bb5-500">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="13bb5-501">[Sentry](https://sentry.io/welcome/) ([repozytorium GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="13bb5-501">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="13bb5-502">[Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="13bb5-502">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="13bb5-503">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repozytorium Github](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="13bb5-503">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="13bb5-504">Niektóre środowiska innych producentów mogą wykonywać [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="13bb5-504">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="13bb5-505">Za pomocą środowiska innych producentów są podobne do przy użyciu jednej z wbudowanych dostawców:</span><span class="sxs-lookup"><span data-stu-id="13bb5-505">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="13bb5-506">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="13bb5-506">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="13bb5-507">Wywołaj `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="13bb5-507">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="13bb5-508">Aby uzyskać więcej informacji można znaleźć w temacie każdy dostawca dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="13bb5-508">For more information, see each provider's documentation.</span></span> <span data-ttu-id="13bb5-509">Rejestrowanie innych firm nie są obsługiwani przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="13bb5-509">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13bb5-510">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="13bb5-510">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
