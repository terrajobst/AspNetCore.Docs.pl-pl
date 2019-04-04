---
title: Rejestrowanie w programie ASP.NET Core
author: tdykstra
description: Więcej informacji na temat struktury rejestrowania w programie ASP.NET Core. Odnajdywanie dostawcy wbudowane funkcje rejestrowania i Dowiedz się więcej na temat popularnych dostawców innych firm.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: c6543ec1f2295c21c6a693ac8bd16ee07ec11381
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327410"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="35ead-104">Rejestrowanie w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35ead-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="35ead-105">Przez [Steve Smith](https://ardalis.com/) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="35ead-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="35ead-106">Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców rejestrowania wbudowanych oraz innych firm.</span><span class="sxs-lookup"><span data-stu-id="35ead-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="35ead-107">W tym artykule przedstawiono sposób korzystania z interfejsu API rejestrowania za pomocą wbudowanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="35ead-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="35ead-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="35ead-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="35ead-109">Dodawanie dostawcy</span><span class="sxs-lookup"><span data-stu-id="35ead-109">Add providers</span></span>

<span data-ttu-id="35ead-110">Dostawcy logowania Wyświetla lub są przechowywane dzienniki.</span><span class="sxs-lookup"><span data-stu-id="35ead-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="35ead-111">Na przykład konsola dostawca Wyświetla dzienniki konsoli i dostawcy usługi Azure Application Insights przechowuje je w usłudze Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="35ead-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="35ead-112">Dzienniki mogą być wysyłane do wielu miejsc docelowych, dodając wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="35ead-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="35ead-113">Aby dodać dostawcę, należy wywołać dostawcy `Add{provider name}` metody rozszerzenia w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="35ead-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="35ead-114">Wywołania szablonu projektu domyślnego <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, która dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="35ead-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="35ead-115">Konsola</span><span class="sxs-lookup"><span data-stu-id="35ead-115">Console</span></span>
* <span data-ttu-id="35ead-116">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35ead-116">Debug</span></span>
* <span data-ttu-id="35ead-117">EventSource (począwszy od programu ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="35ead-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="35ead-118">Jeśli używasz `CreateDefaultBuilder`, domyślnych dostawców można zastąpić własnymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="35ead-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="35ead-119">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, i dostawców, o których chcesz dodać.</span><span class="sxs-lookup"><span data-stu-id="35ead-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="35ead-120">Można użyć dostawcy, instalowanie pakietu NuGet i wywołanie metody rozszerzenia dostawcy w wystąpieniu <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span><span class="sxs-lookup"><span data-stu-id="35ead-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="35ead-121">Platforma ASP.NET Core [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) zapewnia `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="35ead-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="35ead-122">`AddConsole` i `AddDebug` metody rozszerzenia są zdefiniowane w [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pakietów.</span><span class="sxs-lookup"><span data-stu-id="35ead-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="35ead-123">Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` jest metoda w wystąpieniu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="35ead-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="35ead-124">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) dodaje rejestrowania dostawców w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="35ead-124">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="35ead-125">Aby uzyskać dane wyjściowe dziennika z kodu, który jest wykonywany wcześniej, należy dodać rejestrowania dostawców w `Startup` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="35ead-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="35ead-126">Dowiedz się więcej o [wbudowane funkcje rejestrowania dostawców](#built-in-logging-providers) i [rejestrowania innych dostawców](#third-party-logging-providers) w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="35ead-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="35ead-127">Twórz dzienniki</span><span class="sxs-lookup"><span data-stu-id="35ead-127">Create logs</span></span>

<span data-ttu-id="35ead-128">Pobierz <xref:Microsoft.Extensions.Logging.ILogger%601> obiekt z DI.</span><span class="sxs-lookup"><span data-stu-id="35ead-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="35ead-129">Poniższy przykład kontrolera tworzy `Information` i `Warning` dzienniki.</span><span class="sxs-lookup"><span data-stu-id="35ead-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="35ead-130">*Kategorii* jest `TodoApiSample.Controllers.TodoController` (w pełni kwalifikowaną nazwę klasy z `TodoController` w przykładowej aplikacji):</span><span class="sxs-lookup"><span data-stu-id="35ead-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="35ead-131">W poniższym przykładzie stron Razor utworzy dzienników za pomocą `Information` jako *poziom* i `TodoApiSample.Pages.AboutModel` jako *kategorii*:</span><span class="sxs-lookup"><span data-stu-id="35ead-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="35ead-132">Poprzedni przykład tworzy dzienniki za pomocą `Information` i `Warning` jako *poziom* i `TodoController` klasy *kategorii*.</span><span class="sxs-lookup"><span data-stu-id="35ead-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="35ead-133">Dziennik *poziom* wskazuje ważność rejestrowane zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="35ead-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="35ead-134">Dziennik *kategorii* jest ciągiem, który jest skojarzony z każdym dzienniku.</span><span class="sxs-lookup"><span data-stu-id="35ead-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="35ead-135">`ILogger<T>` Wystąpienie tworzy dzienniki, które mają w pełni kwalifikowaną nazwę typu `T` kategorię.</span><span class="sxs-lookup"><span data-stu-id="35ead-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="35ead-136">[Poziomy](#log-level) i [kategorie](#log-category) omówiona bardziej szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="35ead-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="35ead-137">Twórz dzienniki przy uruchamianiu</span><span class="sxs-lookup"><span data-stu-id="35ead-137">Create logs in Startup</span></span>

<span data-ttu-id="35ead-138">Zapisywanie dzienników `Startup` klasy, należy umieścić `ILogger` parametru w sygnatury konstruktora:</span><span class="sxs-lookup"><span data-stu-id="35ead-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="35ead-139">Twórz dzienniki w programie</span><span class="sxs-lookup"><span data-stu-id="35ead-139">Create logs in Program</span></span>

<span data-ttu-id="35ead-140">Zapisywanie dzienników `Program` klasy, Uzyskaj `ILogger` wystąpienia DI:</span><span class="sxs-lookup"><span data-stu-id="35ead-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="35ead-141">Nie metody asynchronicznej rejestratora</span><span class="sxs-lookup"><span data-stu-id="35ead-141">No asynchronous logger methods</span></span>

<span data-ttu-id="35ead-142">Rejestrowanie powinno być tak szybko, że nie jest wart spadek wydajności kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="35ead-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="35ead-143">Jeśli magazyn danych rejestrowania jest powolne, nie zapisanie w nim bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="35ead-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="35ead-144">Należy wziąć pod uwagę początkowo zapisywanie komunikatów dziennika do szybkiego magazynu, a następnie przenieść je do magazynu powolne później.</span><span class="sxs-lookup"><span data-stu-id="35ead-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="35ead-145">Na przykład Zaloguj się do kolejki komunikatów, która ma odczytu utrwalana w magazynie powolne przez inny proces.</span><span class="sxs-lookup"><span data-stu-id="35ead-145">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="configuration"></a><span data-ttu-id="35ead-146">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="35ead-146">Configuration</span></span>

<span data-ttu-id="35ead-147">Rejestrowanie dostawcy konfiguracji jest dostarczane przez przynajmniej jednego dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="35ead-147">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="35ead-148">Formaty plików (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="35ead-148">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="35ead-149">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="35ead-149">Command-line arguments.</span></span>
* <span data-ttu-id="35ead-150">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="35ead-150">Environment variables.</span></span>
* <span data-ttu-id="35ead-151">Obiekty .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="35ead-151">In-memory .NET objects.</span></span>
* <span data-ttu-id="35ead-152">Niezaszyfrowane [Menedżera klucz tajny](xref:security/app-secrets) magazynu.</span><span class="sxs-lookup"><span data-stu-id="35ead-152">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="35ead-153">Użytkownik zaszyfrowanych przechowywanych informacji, takich jak [usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="35ead-153">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="35ead-154">Dostawcy niestandardowi (zainstalowane lub utworzone).</span><span class="sxs-lookup"><span data-stu-id="35ead-154">Custom providers (installed or created).</span></span>

<span data-ttu-id="35ead-155">Na przykład konfiguracja rejestrowania są często dostarczane przez `Logging` części plików ustawień aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-155">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="35ead-156">W poniższym przykładzie pokazano zawartość typowej *appsettings. Development.JSON* pliku:</span><span class="sxs-lookup"><span data-stu-id="35ead-156">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="35ead-157">`Logging` Właściwość może mieć `LogLevel` dziennika właściwości dostawcy (konsoli znajduje się) i innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="35ead-157">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="35ead-158">`LogLevel` Właściwości `Logging` Określa minimalny [poziom](#log-level) logowania dla wybranych kategorii.</span><span class="sxs-lookup"><span data-stu-id="35ead-158">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="35ead-159">W tym przykładzie `System` i `Microsoft` kategorie dziennika na `Information` poziom, a wszystkie pozostałe rejestrowania `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="35ead-159">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="35ead-160">Inne właściwości w obszarze `Logging` Określ rejestrowania dostawców.</span><span class="sxs-lookup"><span data-stu-id="35ead-160">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="35ead-161">W przykładzie występuje dla dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="35ead-161">The example is for the Console provider.</span></span> <span data-ttu-id="35ead-162">Jeśli dostawca obsługuje [dziennika zakresy](#log-scopes), `IncludeScopes` wskazuje, czy są włączone.</span><span class="sxs-lookup"><span data-stu-id="35ead-162">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="35ead-163">Właściwość dostawcy (takich jak `Console` w przykładzie) mogą także określić `LogLevel` właściwości.</span><span class="sxs-lookup"><span data-stu-id="35ead-163">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="35ead-164">`LogLevel` w obszarze dostawcy określa poziomy dziennika dla tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="35ead-164">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="35ead-165">Jeśli nie określono poziomy w `Logging.{providername}.LogLevel`, że zastępują one nic w `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="35ead-165">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

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

<span data-ttu-id="35ead-166">`LogLevel` klucze reprezentują nazwy dziennika.</span><span class="sxs-lookup"><span data-stu-id="35ead-166">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="35ead-167">`Default` Klucz ma zastosowanie do dzienników, które nie zostały jawnie wymienione.</span><span class="sxs-lookup"><span data-stu-id="35ead-167">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="35ead-168">Reprezentuje wartość [poziom dziennika](#log-level) stosowane do danego dziennika.</span><span class="sxs-lookup"><span data-stu-id="35ead-168">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="35ead-169">Aby uzyskać informacje dotyczące implementowania dostawcy konfiguracji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="35ead-169">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="35ead-170">Przykładowe dane wyjściowe z rejestrowania</span><span class="sxs-lookup"><span data-stu-id="35ead-170">Sample logging output</span></span>

<span data-ttu-id="35ead-171">Za pomocą przykładowego kodu pokazano w poprzedniej sekcji dzienniki są wyświetlane w konsoli, gdy aplikacja jest uruchamiana z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="35ead-171">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="35ead-172">Oto przykład danych wyjściowych konsoli:</span><span class="sxs-lookup"><span data-stu-id="35ead-172">Here's an example of console output:</span></span>

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

<span data-ttu-id="35ead-173">Poprzedni dzienniki wygenerowane przez wysłał żądanie HTTP Get do przykładowej aplikacji pod `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="35ead-173">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="35ead-174">Oto przykład tego samego dzienników, w jakiej występują w oknie Debugowanie Uruchom przykładową aplikację w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="35ead-174">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="35ead-175">Dzienniki, które są tworzone przez `ILogger` wywołania pokazano w poprzedniej sekcji zaczyna się od "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="35ead-175">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="35ead-176">Dzienniki, które zaczynają się od "Microsoft" kategorie pochodzą z kodu struktury programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="35ead-176">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="35ead-177">Platforma ASP.NET Core i kodu aplikacji korzystają z tego samego interfejsu API rejestrowania i dostawców.</span><span class="sxs-lookup"><span data-stu-id="35ead-177">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="35ead-178">W dalszej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-178">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="35ead-179">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="35ead-179">NuGet packages</span></span>

<span data-ttu-id="35ead-180">`ILogger` i `ILoggerFactory` interfejsy są w [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), i znajdują się w domyślnej implementacji dla nich [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="35ead-180">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="35ead-181">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="35ead-181">Log category</span></span>

<span data-ttu-id="35ead-182">Gdy `ILogger` obiekt zostanie utworzony, *kategorii* określono dla niej.</span><span class="sxs-lookup"><span data-stu-id="35ead-182">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="35ead-183">Tej kategorii jest uwzględnione w każdej wiadomości dziennika utworzone przez to wystąpienie `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="35ead-183">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="35ead-184">Kategoria może być dowolnym ciągiem, ale Konwencji jest użycie nazwy klasy, takie jak "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="35ead-184">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="35ead-185">Użyj `ILogger<T>` można pobrać `ILogger` wystąpienie, które korzysta z w pełni kwalifikowana nazwa typu z `T` kategorię:</span><span class="sxs-lookup"><span data-stu-id="35ead-185">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="35ead-186">Aby jawnie określić kategorię, należy wywołać `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="35ead-186">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="35ead-187">`ILogger<T>` jest równoważne z wywoływaniem `CreateLogger` z w pełni kwalifikowana nazwa typu z `T`.</span><span class="sxs-lookup"><span data-stu-id="35ead-187">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="35ead-188">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="35ead-188">Log level</span></span>

<span data-ttu-id="35ead-189">Każdy dziennik Określa <xref:Microsoft.Extensions.Logging.LogLevel> wartość.</span><span class="sxs-lookup"><span data-stu-id="35ead-189">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="35ead-190">Poziom dziennika wskazuje ważność lub ważność.</span><span class="sxs-lookup"><span data-stu-id="35ead-190">The log level indicates the severity or importance.</span></span> <span data-ttu-id="35ead-191">Na przykład można napisać `Information` logowania, jeśli metoda zakończy się normalnie, a jeśli tak, to a `Warning` Zaloguj się, gdy metoda zwróci wartość *404 Nie znaleziono* kod stanu.</span><span class="sxs-lookup"><span data-stu-id="35ead-191">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="35ead-192">Poniższy kod tworzy `Information` i `Warning` dzienników:</span><span class="sxs-lookup"><span data-stu-id="35ead-192">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="35ead-193">W poprzednim kodzie pierwszy parametr jest [identyfikator zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="35ead-193">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="35ead-194">Drugi parametr jest szablon wiadomości z symboli zastępczych dla wartości argumentów dostarczone przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="35ead-194">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="35ead-195">Parametry metody są wyjaśnione w [komunikatu sekcji szablonu](#log-message-template) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="35ead-195">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="35ead-196">Metody, które obejmują poziom w nazwie metody logowania (na przykład `LogInformation` i `LogWarning`) są [metody rozszerzenia dla ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="35ead-196">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="35ead-197">Wywoływanie tych metod `Log` metody, która przyjmuje `LogLevel` parametru.</span><span class="sxs-lookup"><span data-stu-id="35ead-197">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="35ead-198">Możesz wywołać `Log` bezpośrednio zamiast jednej z tych metod rozszerzenia, ale składnia jest stosunkowo skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="35ead-198">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="35ead-199">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.Extensions.Logging.ILogger> i [kod źródłowy rozszerzenia rejestratora](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="35ead-199">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="35ead-200">Platforma ASP.NET Core definiuje następujące poziomy dziennika, w tym miejscu uporządkowane od najniższej do najwyższej ważności.</span><span class="sxs-lookup"><span data-stu-id="35ead-200">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="35ead-201">Śledzenie = 0</span><span class="sxs-lookup"><span data-stu-id="35ead-201">Trace = 0</span></span>

  <span data-ttu-id="35ead-202">Aby uzyskać informacje, które są zazwyczaj przydatne tylko w przypadku debugowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-202">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="35ead-203">Te komunikaty mogą zawierać dane poufne aplikacji i dlatego nie można włączyć w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="35ead-203">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="35ead-204">*Domyślnie wyłączone.*</span><span class="sxs-lookup"><span data-stu-id="35ead-204">*Disabled by default.*</span></span>

* <span data-ttu-id="35ead-205">Debugowanie = 1</span><span class="sxs-lookup"><span data-stu-id="35ead-205">Debug = 1</span></span>

  <span data-ttu-id="35ead-206">Aby uzyskać informacje, które mogą być użyteczne podczas programowania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-206">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="35ead-207">Przykład: `Entering method Configure with flag set to true.` Włącz `Debug` poziom dzienniki w środowisku produkcyjnym, tylko wtedy, gdy rozwiązywania problemów, z powodu dużej liczby dzienników.</span><span class="sxs-lookup"><span data-stu-id="35ead-207">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="35ead-208">Informacje o = 2</span><span class="sxs-lookup"><span data-stu-id="35ead-208">Information = 2</span></span>

  <span data-ttu-id="35ead-209">Do śledzenia ogólny przebieg aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-209">For tracking the general flow of the app.</span></span> <span data-ttu-id="35ead-210">Te dzienniki są zazwyczaj mają niektóre wartości długoterminowe.</span><span class="sxs-lookup"><span data-stu-id="35ead-210">These logs typically have some long-term value.</span></span> <span data-ttu-id="35ead-211">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="35ead-211">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="35ead-212">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="35ead-212">Warning = 3</span></span>

  <span data-ttu-id="35ead-213">Nietypowe lub nieoczekiwanych zdarzeń w usłudze flow aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-213">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="35ead-214">Mogą one zawierać błędy lub inne warunki, które nie powodują aplikacji zatrzymać, ale może być konieczne należy zbadać.</span><span class="sxs-lookup"><span data-stu-id="35ead-214">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="35ead-215">Obsługiwane wyjątki są spójne użyj `Warning` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="35ead-215">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="35ead-216">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="35ead-216">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="35ead-217">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="35ead-217">Error = 4</span></span>

  <span data-ttu-id="35ead-218">Błędy i wyjątki, które nie mogą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="35ead-218">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="35ead-219">Te komunikaty wskazują wystąpił błąd podczas bieżącego działania lub operacji (takie jak bieżące żądanie HTTP), nie wystąpił błąd całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-219">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="35ead-220">Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="35ead-220">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="35ead-221">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="35ead-221">Critical = 5</span></span>

  <span data-ttu-id="35ead-222">Dla błędów, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="35ead-222">For failures that require immediate attention.</span></span> <span data-ttu-id="35ead-223">Przykłady: utratą danych, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="35ead-223">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="35ead-224">Umożliwia kontrolowanie, ile dane wyjściowe dziennika są zapisywane na nośniku określonego poziomu dziennika lub wyświetlić okno.</span><span class="sxs-lookup"><span data-stu-id="35ead-224">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="35ead-225">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35ead-225">For example:</span></span>

* <span data-ttu-id="35ead-226">W środowisku produkcyjnym, należy wysłać `Trace` za pośrednictwem `Information` poziomu do woluminu magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="35ead-226">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="35ead-227">Wyślij `Warning` za pośrednictwem `Critical` danych wartość przechowywania.</span><span class="sxs-lookup"><span data-stu-id="35ead-227">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="35ead-228">Podczas tworzenia aplikacji, Wyślij `Warning` za pośrednictwem `Critical` do konsoli i Dodaj `Trace` za pośrednictwem `Information` podczas rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="35ead-228">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="35ead-229">[Filtrowanie dziennika](#log-filtering) sekcję w dalszej części tego artykułu opisano sposób kontrolowania poziomy dziennika, który dostawca obsługuje.</span><span class="sxs-lookup"><span data-stu-id="35ead-229">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="35ead-230">Platforma ASP.NET Core zapisuje dzienniki zdarzeń framework.</span><span class="sxs-lookup"><span data-stu-id="35ead-230">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="35ead-231">Przykłady dzienników wcześniej w tym artykule wykluczone dzienniki poniżej `Information` poziomie, więc nie `Debug` lub `Trace` poziom Dzienniki zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="35ead-231">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="35ead-232">Oto przykład dzienniki konsoli produkowanych przez przykładową aplikację skonfigurowana do wyświetlania `Debug` dzienników:</span><span class="sxs-lookup"><span data-stu-id="35ead-232">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="35ead-233">Identyfikator zdarzenia logowania</span><span class="sxs-lookup"><span data-stu-id="35ead-233">Log event ID</span></span>

<span data-ttu-id="35ead-234">Można określić w każdym dzienniku *identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="35ead-234">Each log can specify an *event ID*.</span></span> <span data-ttu-id="35ead-235">Przykładowa aplikacja robi to przy użyciu zdefiniowanej lokalnie `LoggingEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="35ead-235">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="35ead-236">Identyfikator zdarzenia kojarzy zestaw zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="35ead-236">An event ID associates a set of events.</span></span> <span data-ttu-id="35ead-237">Na przykład wszystkie dzienniki związane z wyświetlanie listy elementów na stronie może być 1001.</span><span class="sxs-lookup"><span data-stu-id="35ead-237">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="35ead-238">Dostawcy logowania mogą być przechowywane identyfikator zdarzenia w polu Identyfikator w komunikacie rejestrowania lub wcale.</span><span class="sxs-lookup"><span data-stu-id="35ead-238">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="35ead-239">Dostawca debugowania nie Wyświetla identyfikatory zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="35ead-239">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="35ead-240">Konsola dostawca przedstawia identyfikatory zdarzeń w nawiasach kwadratowych po kategorii:</span><span class="sxs-lookup"><span data-stu-id="35ead-240">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="35ead-241">Szablon wiadomości dziennika</span><span class="sxs-lookup"><span data-stu-id="35ead-241">Log message template</span></span>

<span data-ttu-id="35ead-242">Każdy dziennik Określa szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="35ead-242">Each log specifies a message template.</span></span> <span data-ttu-id="35ead-243">Szablon wiadomości mogą zawierać symbole zastępcze, dla których są podane argumenty.</span><span class="sxs-lookup"><span data-stu-id="35ead-243">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="35ead-244">Użyj nazw symboli zastępczych, nie liczby.</span><span class="sxs-lookup"><span data-stu-id="35ead-244">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="35ead-245">Kolejność symboli zastępczych, nie ich nazwy, określa parametry, które służą do zapewniania ich wartości.</span><span class="sxs-lookup"><span data-stu-id="35ead-245">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="35ead-246">W poniższym kodzie Zauważ, że nazwy parametrów są poza kolejnością w szablonie wiadomości:</span><span class="sxs-lookup"><span data-stu-id="35ead-246">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="35ead-247">Ten kod tworzy komunikat dziennika przy użyciu wartości parametrów w sekwencji:</span><span class="sxs-lookup"><span data-stu-id="35ead-247">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="35ead-248">Struktury rejestrowania działa w ten sposób, aby zaimplementować rejestrowania dostawców [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="35ead-248">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="35ead-249">Same argumenty są przekazywane do systemu rejestrowania, a nie tylko szablon sformatowany komunikat.</span><span class="sxs-lookup"><span data-stu-id="35ead-249">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="35ead-250">Te informacje umożliwiają dostawcom rejestrowania do przechowywania wartości parametrów jako pola.</span><span class="sxs-lookup"><span data-stu-id="35ead-250">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="35ead-251">Załóżmy na przykład, rejestratora metody wywołania się następująco:</span><span class="sxs-lookup"><span data-stu-id="35ead-251">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="35ead-252">Jeśli dzienniki jest wysyłana do usługi Azure Table Storage, każda jednostka usługi Azure Table może mieć `ID` i `RequestTime` właściwości, które upraszcza zapytań dotyczących danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="35ead-252">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="35ead-253">Zapytania można znaleźć wszystkie dzienniki w ramach określonego `RequestTime` zakresu bez analizowania limit czasu wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="35ead-253">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="35ead-254">Rejestrowania wyjątków</span><span class="sxs-lookup"><span data-stu-id="35ead-254">Logging exceptions</span></span>

<span data-ttu-id="35ead-255">Metody rejestratora mają przeciążenia, które umożliwiają przekazywanie wyjątek, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="35ead-255">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="35ead-256">Różnych dostawców obsługiwać informacje o wyjątku na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="35ead-256">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="35ead-257">Oto przykład danych wyjściowych debugowania dostawcy w kodzie pokazanym powyżej.</span><span class="sxs-lookup"><span data-stu-id="35ead-257">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="35ead-258">Filtrowanie dziennika</span><span class="sxs-lookup"><span data-stu-id="35ead-258">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="35ead-259">Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="35ead-259">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="35ead-260">Żadnych dzienników poniżej minimalnego poziomu nie są przekazywane do tego dostawcy, dzięki czemu nie uzyskać wyświetlane lub przechowywane.</span><span class="sxs-lookup"><span data-stu-id="35ead-260">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="35ead-261">Aby pominąć wszystkie dzienniki, określ `LogLevel.None` jako minimalny poziom rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-261">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="35ead-262">Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="35ead-262">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="35ead-263">Tworzenie reguły filtrów w konfiguracji</span><span class="sxs-lookup"><span data-stu-id="35ead-263">Create filter rules in configuration</span></span>

<span data-ttu-id="35ead-264">Kod wywołuje szablon projektu `CreateDefaultBuilder` skonfigurować rejestrowanie dla dostawców konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-264">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="35ead-265">`CreateDefaultBuilder` Metoda konfiguruje również rejestrowania do wyszukania konfiguracji w `Logging` sekcji przy użyciu kodu, podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="35ead-265">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="35ead-266">Dane konfiguracji Określa poziomy minimalna dziennika przez dostawcę i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="35ead-266">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="35ead-267">Ten kod JSON tworzy sześć reguły filtru: jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jeden dla wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="35ead-267">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="35ead-268">Wybrano jedną regułę dla każdego dostawcy podczas `ILogger` obiekt zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="35ead-268">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="35ead-269">Reguły filtrowania w kodzie</span><span class="sxs-lookup"><span data-stu-id="35ead-269">Filter rules in code</span></span>

<span data-ttu-id="35ead-270">Poniższy przykład pokazuje, jak zarejestrować reguły filtrów w kodzie:</span><span class="sxs-lookup"><span data-stu-id="35ead-270">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="35ead-271">Drugi `AddFilter` określa dostawcę debugowania przy użyciu jego nazwy.</span><span class="sxs-lookup"><span data-stu-id="35ead-271">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="35ead-272">Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określa typ dostawcy.</span><span class="sxs-lookup"><span data-stu-id="35ead-272">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="35ead-273">Jak reguły filtrowania zostaną zastosowane.</span><span class="sxs-lookup"><span data-stu-id="35ead-273">How filtering rules are applied</span></span>

<span data-ttu-id="35ead-274">Dane konfiguracji i `AddFilter` kod przedstawiony w powyższych przykładach Tworzenie reguły pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="35ead-274">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="35ead-275">Pierwsze sześć pochodzą przykład konfiguracji i ostatnie dwa pochodzą w przykładzie kodu.</span><span class="sxs-lookup"><span data-stu-id="35ead-275">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="35ead-276">Wartość liczbowa</span><span class="sxs-lookup"><span data-stu-id="35ead-276">Number</span></span> | <span data-ttu-id="35ead-277">Dostawca</span><span class="sxs-lookup"><span data-stu-id="35ead-277">Provider</span></span>      | <span data-ttu-id="35ead-278">Kategorie, które zaczynają się od...</span><span class="sxs-lookup"><span data-stu-id="35ead-278">Categories that begin with ...</span></span>          | <span data-ttu-id="35ead-279">Minimalny poziom rejestrowania</span><span class="sxs-lookup"><span data-stu-id="35ead-279">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="35ead-280">1</span><span class="sxs-lookup"><span data-stu-id="35ead-280">1</span></span>      | <span data-ttu-id="35ead-281">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35ead-281">Debug</span></span>         | <span data-ttu-id="35ead-282">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="35ead-282">All categories</span></span>                          | <span data-ttu-id="35ead-283">Informacje</span><span class="sxs-lookup"><span data-stu-id="35ead-283">Information</span></span>       |
| <span data-ttu-id="35ead-284">2</span><span class="sxs-lookup"><span data-stu-id="35ead-284">2</span></span>      | <span data-ttu-id="35ead-285">Konsola</span><span class="sxs-lookup"><span data-stu-id="35ead-285">Console</span></span>       | <span data-ttu-id="35ead-286">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="35ead-286">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="35ead-287">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="35ead-287">Warning</span></span>           |
| <span data-ttu-id="35ead-288">3</span><span class="sxs-lookup"><span data-stu-id="35ead-288">3</span></span>      | <span data-ttu-id="35ead-289">Konsola</span><span class="sxs-lookup"><span data-stu-id="35ead-289">Console</span></span>       | <span data-ttu-id="35ead-290">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="35ead-290">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="35ead-291">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35ead-291">Debug</span></span>             |
| <span data-ttu-id="35ead-292">4</span><span class="sxs-lookup"><span data-stu-id="35ead-292">4</span></span>      | <span data-ttu-id="35ead-293">Konsola</span><span class="sxs-lookup"><span data-stu-id="35ead-293">Console</span></span>       | <span data-ttu-id="35ead-294">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="35ead-294">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="35ead-295">Błąd</span><span class="sxs-lookup"><span data-stu-id="35ead-295">Error</span></span>             |
| <span data-ttu-id="35ead-296">5</span><span class="sxs-lookup"><span data-stu-id="35ead-296">5</span></span>      | <span data-ttu-id="35ead-297">Konsola</span><span class="sxs-lookup"><span data-stu-id="35ead-297">Console</span></span>       | <span data-ttu-id="35ead-298">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="35ead-298">All categories</span></span>                          | <span data-ttu-id="35ead-299">Informacje</span><span class="sxs-lookup"><span data-stu-id="35ead-299">Information</span></span>       |
| <span data-ttu-id="35ead-300">6</span><span class="sxs-lookup"><span data-stu-id="35ead-300">6</span></span>      | <span data-ttu-id="35ead-301">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="35ead-301">All providers</span></span> | <span data-ttu-id="35ead-302">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="35ead-302">All categories</span></span>                          | <span data-ttu-id="35ead-303">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35ead-303">Debug</span></span>             |
| <span data-ttu-id="35ead-304">7</span><span class="sxs-lookup"><span data-stu-id="35ead-304">7</span></span>      | <span data-ttu-id="35ead-305">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="35ead-305">All providers</span></span> | <span data-ttu-id="35ead-306">System</span><span class="sxs-lookup"><span data-stu-id="35ead-306">System</span></span>                                  | <span data-ttu-id="35ead-307">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35ead-307">Debug</span></span>             |
| <span data-ttu-id="35ead-308">8</span><span class="sxs-lookup"><span data-stu-id="35ead-308">8</span></span>      | <span data-ttu-id="35ead-309">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35ead-309">Debug</span></span>         | <span data-ttu-id="35ead-310">Microsoft</span><span class="sxs-lookup"><span data-stu-id="35ead-310">Microsoft</span></span>                               | <span data-ttu-id="35ead-311">Śledzenia</span><span class="sxs-lookup"><span data-stu-id="35ead-311">Trace</span></span>             |

<span data-ttu-id="35ead-312">Gdy `ILogger` obiekt zostanie utworzony, `ILoggerFactory` obiektu wybiera jedną regułę na dostawcy, aby zastosować do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="35ead-312">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="35ead-313">Wszystkie komunikaty napisane przez `ILogger` wystąpienia są filtrowane w zależności od wybranej reguły.</span><span class="sxs-lookup"><span data-stu-id="35ead-313">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="35ead-314">Najbardziej określonej reguły dla każdego dostawcy i pary kategoria możliwe jest wybrana w zaufanym dostępne reguły.</span><span class="sxs-lookup"><span data-stu-id="35ead-314">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="35ead-315">Następujące algorytm jest używany dla każdego dostawcy podczas `ILogger` jest tworzona dla danej kategorii:</span><span class="sxs-lookup"><span data-stu-id="35ead-315">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="35ead-316">Wybierz wszystkie reguły, które odpowiadają przez dostawcę lub jego alias.</span><span class="sxs-lookup"><span data-stu-id="35ead-316">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="35ead-317">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły przy użyciu dostawcy puste.</span><span class="sxs-lookup"><span data-stu-id="35ead-317">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="35ead-318">W wyniku poprzedniego kroku wybierz reguły z najdłużej dopasowania prefiksu kategorii.</span><span class="sxs-lookup"><span data-stu-id="35ead-318">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="35ead-319">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="35ead-319">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="35ead-320">Jeśli wybrano wiele reguł, **ostatniego** jeden.</span><span class="sxs-lookup"><span data-stu-id="35ead-320">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="35ead-321">Jeśli nie zaznaczono żadnych reguł, użyj `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="35ead-321">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="35ead-322">Przy użyciu poprzednich listę reguł, załóżmy, że możesz utworzyć `ILogger` obiektu dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="35ead-322">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="35ead-323">W przypadku dostawcy debugowania mają zastosowanie reguły 1, 6 i 8.</span><span class="sxs-lookup"><span data-stu-id="35ead-323">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="35ead-324">Reguła 8 jest najbardziej specyficzną, więc to jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="35ead-324">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="35ead-325">W przypadku dostawcy konsoli mają zastosowanie reguły, 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="35ead-325">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="35ead-326">Reguła 3 jest bardziej konkretny od pozostałych.</span><span class="sxs-lookup"><span data-stu-id="35ead-326">Rule 3 is most specific.</span></span>

<span data-ttu-id="35ead-327">Wynikowy `ILogger` wystąpienia wysyła dzienniki `Trace` poziom i nowsze wersje do debugowania dostawcy.</span><span class="sxs-lookup"><span data-stu-id="35ead-327">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="35ead-328">Dzienniki programu `Debug` poziomu i nowszych są wysyłane do dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="35ead-328">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="35ead-329">Aliasy dostawcy</span><span class="sxs-lookup"><span data-stu-id="35ead-329">Provider aliases</span></span>

<span data-ttu-id="35ead-330">Każdy dostawca definiuje *alias* które mogą być używane w konfiguracji zamiast w pełni kwalifikowana nazwa typu.</span><span class="sxs-lookup"><span data-stu-id="35ead-330">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="35ead-331">W przypadku dostawców wbudowanych należy stosować następujące aliasy:</span><span class="sxs-lookup"><span data-stu-id="35ead-331">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="35ead-332">Konsola</span><span class="sxs-lookup"><span data-stu-id="35ead-332">Console</span></span>
* <span data-ttu-id="35ead-333">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35ead-333">Debug</span></span>
* <span data-ttu-id="35ead-334">Dziennik zdarzeń</span><span class="sxs-lookup"><span data-stu-id="35ead-334">EventLog</span></span>
* <span data-ttu-id="35ead-335">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="35ead-335">AzureAppServices</span></span>
* <span data-ttu-id="35ead-336">TraceSource</span><span class="sxs-lookup"><span data-stu-id="35ead-336">TraceSource</span></span>
* <span data-ttu-id="35ead-337">EventSource</span><span class="sxs-lookup"><span data-stu-id="35ead-337">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="35ead-338">Minimalny poziom domyślny</span><span class="sxs-lookup"><span data-stu-id="35ead-338">Default minimum level</span></span>

<span data-ttu-id="35ead-339">Istnieje minimalne ustawienie poziomie staje się skuteczny tylko wtedy, gdy nie z konfiguracji czy kodu reguły dla danego dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="35ead-339">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="35ead-340">Poniższy przykład pokazuje, jak ustawić minimalny poziom:</span><span class="sxs-lookup"><span data-stu-id="35ead-340">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="35ead-341">Jeśli nie zostanie jawnie ustawiona minimalny poziom, wartością domyślną jest `Information`, co oznacza, że `Trace` i `Debug` dzienniki są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="35ead-341">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="35ead-342">Funkcje filtrowania</span><span class="sxs-lookup"><span data-stu-id="35ead-342">Filter functions</span></span>

<span data-ttu-id="35ead-343">Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorie, które nie mają zasady przypisane do nich przez konfiguracji czy kodu.</span><span class="sxs-lookup"><span data-stu-id="35ead-343">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="35ead-344">Kod w funkcji ma dostęp do typu Dostawca, kategoria i poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="35ead-344">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="35ead-345">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35ead-345">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="35ead-346">Niektórzy dostawcy rejestrowania umożliwiają określenie, kiedy zapisany na nośniku lub ignorowane dzienniki na podstawie poziomu dziennika i kategorii.</span><span class="sxs-lookup"><span data-stu-id="35ead-346">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="35ead-347">`AddConsole` i `AddDebug` metody rozszerzenia, zapewnienia przeciążenia, które akceptują kryteria filtrowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-347">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="35ead-348">Następujący przykładowy kod powoduje, że Konsola dostawca ignorowanie dzienniki poniżej `Warning` poziomu, podczas gdy dostawca debugowania ignoruje dzienniki tworzonych w ramach.</span><span class="sxs-lookup"><span data-stu-id="35ead-348">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="35ead-349">`AddEventLog` Metoda ma przeciążenia, które przyjmuje `EventLogSettings` wystąpienia, co może zawierać funkcji filtrowania w jego `Filter` właściwości.</span><span class="sxs-lookup"><span data-stu-id="35ead-349">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="35ead-350">Dostawca TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jej poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` używa.</span><span class="sxs-lookup"><span data-stu-id="35ead-350">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="35ead-351">Aby ustawić reguły filtrowania dla wszystkich dostawców, które są zarejestrowane w usłudze `ILoggerFactory` wystąpienia, należy użyć `WithFilter` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="35ead-351">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="35ead-352">Poniższy przykład ogranicza framework dzienniki (kategoria zaczyna się od "Microsoft" lub "System") ostrzeżeń podczas logowania się na poziomie debugowania dla dzienników tworzona przez kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-352">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="35ead-353">Aby zapobiec sytuacji, w której wszelkie dzienniki zapisywana, podaj `LogLevel.None` jako minimalny poziom rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-353">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="35ead-354">Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="35ead-354">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="35ead-355">`WithFilter` Metody rozszerzenia są dostarczane przez [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="35ead-355">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="35ead-356">Metoda ta zwraca nowy `ILoggerFactory` zarejestrowano wystąpienia, która będzie filtrować komunikaty dziennika, przekazywane do wszystkich dostawców rejestratora.</span><span class="sxs-lookup"><span data-stu-id="35ead-356">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="35ead-357">Nie wpływa na inne `ILoggerFactory` wystąpienia, w tym oryginalny `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="35ead-357">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="35ead-358">Kategorie systemu i poziomy</span><span class="sxs-lookup"><span data-stu-id="35ead-358">System categories and levels</span></span>

<span data-ttu-id="35ead-359">Poniżej przedstawiono niektóre kategorie używane przez platformy ASP.NET Core i Entity Framework Core, z uwagi na temat co dzienniki można oczekiwać od nich:</span><span class="sxs-lookup"><span data-stu-id="35ead-359">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="35ead-360">Kategoria</span><span class="sxs-lookup"><span data-stu-id="35ead-360">Category</span></span>                            | <span data-ttu-id="35ead-361">Uwagi</span><span class="sxs-lookup"><span data-stu-id="35ead-361">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="35ead-362">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="35ead-362">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="35ead-363">Diagnostyka ogólnego ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="35ead-363">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="35ead-364">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="35ead-364">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="35ead-365">Klucze, które zostały uznane za znaleziono i używane.</span><span class="sxs-lookup"><span data-stu-id="35ead-365">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="35ead-366">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="35ead-366">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="35ead-367">Hosty dozwolone.</span><span class="sxs-lookup"><span data-stu-id="35ead-367">Hosts allowed.</span></span> |
| <span data-ttu-id="35ead-368">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="35ead-368">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="35ead-369">Ile żądań HTTP potrzebny do ukończenia, a także czas rozpoczęcia pracy.</span><span class="sxs-lookup"><span data-stu-id="35ead-369">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="35ead-370">Zestawy startowe, które hostingu zostały załadowane.</span><span class="sxs-lookup"><span data-stu-id="35ead-370">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="35ead-371">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="35ead-371">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="35ead-372">Diagnostyka MVC i Razor.</span><span class="sxs-lookup"><span data-stu-id="35ead-372">MVC and Razor diagnostics.</span></span> <span data-ttu-id="35ead-373">Powiązanie modelu, wykonywanie filtru, kompilacja widoku wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="35ead-373">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="35ead-374">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="35ead-374">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="35ead-375">Trasę odpowiednią dla informacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-375">Route matching information.</span></span> |
| <span data-ttu-id="35ead-376">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="35ead-376">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="35ead-377">Rozpoczęcie połączenia zatrzymać i zachować aktywności odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="35ead-377">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="35ead-378">Informacje o certyfikacie protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="35ead-378">HTTPS certificate information.</span></span> |
| <span data-ttu-id="35ead-379">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="35ead-379">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="35ead-380">Obsługiwane pliki.</span><span class="sxs-lookup"><span data-stu-id="35ead-380">Files served.</span></span> |
| <span data-ttu-id="35ead-381">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="35ead-381">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="35ead-382">Ogólne Diagnostyka platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="35ead-382">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="35ead-383">Działanie i konfiguracji wykrywania zmian migracje baz danych.</span><span class="sxs-lookup"><span data-stu-id="35ead-383">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="35ead-384">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="35ead-384">Log scopes</span></span>

 <span data-ttu-id="35ead-385">A *zakres* można grupować zestaw operacji logicznej.</span><span class="sxs-lookup"><span data-stu-id="35ead-385">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="35ead-386">Ta metoda grupowania może służyć do dołączenia do tych samych danych do każdego dziennika, który jest tworzony jako część zestawu.</span><span class="sxs-lookup"><span data-stu-id="35ead-386">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="35ead-387">Na przykład każdy dziennik utworzony jako część przetwarzania transakcji może zawierać identyfikator transakcji.</span><span class="sxs-lookup"><span data-stu-id="35ead-387">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="35ead-388">Zakres jest `IDisposable` typu, który jest zwracany przez <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> metody i trwa do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="35ead-388">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="35ead-389">Użyj zakresu opakowując wywołania rejestratora w `using` bloku:</span><span class="sxs-lookup"><span data-stu-id="35ead-389">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="35ead-390">Poniższy kod umożliwia zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="35ead-390">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="35ead-391">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="35ead-391">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="35ead-392">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="35ead-392">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="35ead-393">Aby uzyskać informacji na temat konfigurowania, zobacz [konfiguracji](#configuration) sekcji.</span><span class="sxs-lookup"><span data-stu-id="35ead-393">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="35ead-394">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="35ead-394">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="35ead-395">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="35ead-395">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="35ead-396">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="35ead-396">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="35ead-397">Każdy komunikat dziennika zawiera informacje o określonym zakresie:</span><span class="sxs-lookup"><span data-stu-id="35ead-397">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="35ead-398">Wbudowane funkcje rejestrowania dostawców</span><span class="sxs-lookup"><span data-stu-id="35ead-398">Built-in logging providers</span></span>

<span data-ttu-id="35ead-399">ASP.NET Core jest dostarczana w następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="35ead-399">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="35ead-400">Console</span><span class="sxs-lookup"><span data-stu-id="35ead-400">Console</span></span>](#console-provider)
* [<span data-ttu-id="35ead-401">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="35ead-401">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="35ead-402">EventSource</span><span class="sxs-lookup"><span data-stu-id="35ead-402">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="35ead-403">EventLog</span><span class="sxs-lookup"><span data-stu-id="35ead-403">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="35ead-404">TraceSource</span><span class="sxs-lookup"><span data-stu-id="35ead-404">TraceSource</span></span>](#tracesource-provider)

<span data-ttu-id="35ead-405">Opcje dla [rejestrowania na platformie Azure](#logging-in-azure) zostały omówione w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="35ead-405">Options for [Logging in Azure](#logging-in-azure) are covered later in this article.</span></span>

<span data-ttu-id="35ead-406">Informacje dotyczące rejestrowania strumienia wyjściowego stdout, zobacz <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> i <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="35ead-406">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="35ead-407">Konsola dostawcy</span><span class="sxs-lookup"><span data-stu-id="35ead-407">Console provider</span></span>

<span data-ttu-id="35ead-408">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakiet dostawcy wysyła dane wyjściowe dziennika do konsoli.</span><span class="sxs-lookup"><span data-stu-id="35ead-408">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="35ead-409">[Przeciążenia AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) pozwalają przekazać minimalny poziom rejestrowania, funkcję filtru i atrybut typu wartość logiczna, która wskazuje, czy zakresy są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="35ead-409">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="35ead-410">Innym rozwiązaniem jest przekazywanie `IConfiguration` obiektu, który można określić zakresy pomocy technicznej i poziomów rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-410">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="35ead-411">Dostawca konsola ma znaczący wpływ na wydajność i zwykle nie jest przeznaczone do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="35ead-411">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="35ead-412">Podczas tworzenia nowego projektu w programie Visual Studio, `AddConsole` metoda wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="35ead-412">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="35ead-413">Ten kod, który odwołuje się do `Logging` części *appSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="35ead-413">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="35ead-414">Ustawienia pokazano limit framework dzienniki, aby ostrzeżenia zezwalając aplikacji do logowania na poziomie debugowania, jak wyjaśniono w [filtrowanie dziennika](#log-filtering) sekcji.</span><span class="sxs-lookup"><span data-stu-id="35ead-414">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="35ead-415">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="35ead-415">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="35ead-416">Aby wyświetlić rejestrowania danych wyjściowych konsoli, otwórz wiersz polecenia w folderze projektu, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="35ead-416">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="35ead-417">Debugowanie dostawcy</span><span class="sxs-lookup"><span data-stu-id="35ead-417">Debug provider</span></span>

<span data-ttu-id="35ead-418">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakiet dostawcy zapisuje dane wyjściowe dziennika za pomocą [system.Diagnostics.Debug —](/dotnet/api/system.diagnostics.debug) klasy (`Debug.WriteLine` wywołania metody).</span><span class="sxs-lookup"><span data-stu-id="35ead-418">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="35ead-419">W systemie Linux, tego dostawcy zapisuje dzienniki na */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="35ead-419">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="35ead-420">[Przeciążenia AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) pozwalają przekazać minimalny poziom rejestrowania lub funkcję filtru.</span><span class="sxs-lookup"><span data-stu-id="35ead-420">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="35ead-421">Dostawca źródła zdarzeń</span><span class="sxs-lookup"><span data-stu-id="35ead-421">EventSource provider</span></span>

<span data-ttu-id="35ead-422">W przypadku aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszym, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zaimplementować pakiet dostawcy śledzenia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="35ead-422">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="35ead-423">W Windows, używa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="35ead-423">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="35ead-424">Dostawca jest dla wielu platform, ale nie są dostępne żadne zdarzenie gromadzenia i wyświetlania narzędzia jeszcze dla systemu Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="35ead-424">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="35ead-425">Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [narzędzia PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="35ead-425">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="35ead-426">Istnieją inne narzędzia umożliwiające wyświetlanie dzienników zdarzeń systemu Windows, ale narzędzia PerfView zapewnia najlepsze środowisko do pracy z zdarzenia ETW emitowane przez platformę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="35ead-426">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="35ead-427">Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, Dodaj parametry `*Microsoft-Extensions-Logging` do **dodatkowych dostawców** listy.</span><span class="sxs-lookup"><span data-stu-id="35ead-427">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="35ead-428">(Nie przegap gwiazdki na początku ciągu).</span><span class="sxs-lookup"><span data-stu-id="35ead-428">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia Perfview dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="35ead-430">Dostawca dziennika zdarzeń Windows</span><span class="sxs-lookup"><span data-stu-id="35ead-430">Windows EventLog provider</span></span>

<span data-ttu-id="35ead-431">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pakiet dostawcy wysyła dane wyjściowe dziennika w dzienniku zdarzeń Windows.</span><span class="sxs-lookup"><span data-stu-id="35ead-431">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="35ead-432">[Przeciążenia AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) umożliwiają przekazanej `EventLogSettings` lub minimalny poziom rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="35ead-432">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="35ead-433">TraceSource dostawcy</span><span class="sxs-lookup"><span data-stu-id="35ead-433">TraceSource provider</span></span>

<span data-ttu-id="35ead-434">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa dostawcy pakietu <xref:System.Diagnostics.TraceSource> bibliotek i dostawców.</span><span class="sxs-lookup"><span data-stu-id="35ead-434">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

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

<span data-ttu-id="35ead-435">[Przeciążenia AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) pozwalają przekazać przełącznik źródła i odbiornika śledzenia.</span><span class="sxs-lookup"><span data-stu-id="35ead-435">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="35ead-436">Aby użyć tego dostawcy, aplikacja ma do uruchamiania na .NET Framework (a nie .NET Core).</span><span class="sxs-lookup"><span data-stu-id="35ead-436">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="35ead-437">Dostawca komunikaty można kierować do różnych [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takie jak <xref:System.Diagnostics.TextWriterTraceListener> używane w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-437">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="35ead-438">Poniższy przykład umożliwia skonfigurowanie `TraceSource` dostawcy, która rejestruje `Warning` i wyższych komunikaty w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="35ead-438">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a><span data-ttu-id="35ead-439">Rejestrowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="35ead-439">Logging in Azure</span></span>

<span data-ttu-id="35ead-440">Aby uzyskać informacji na temat rejestrowania na platformie Azure zobacz następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="35ead-440">For information about logging in Azure, see the following sections:</span></span>

* [<span data-ttu-id="35ead-441">Dostawca usługi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="35ead-441">Azure App Service provider</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="35ead-442">Przesyłanie strumieniowe dzienników platformy Azure</span><span class="sxs-lookup"><span data-stu-id="35ead-442">Azure log streaming</span></span>](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [<span data-ttu-id="35ead-443">Rejestrowanie śledzenia w usłudze Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="35ead-443">Azure Application Insights trace logging</span></span>](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="35ead-444">Dostawca usługi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="35ead-444">Azure App Service provider</span></span>

<span data-ttu-id="35ead-445">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pakiet dostawcy zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji w usłudze Azure App Service i do [magazynu obiektów blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="35ead-445">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="35ead-446">Pakiet dostawcy jest dostępna dla aplikacji przeznaczonych dla platformy .NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="35ead-446">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="35ead-447">Jeśli przeznaczony dla platformy .NET Core, pamiętaj o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="35ead-447">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="35ead-448">Pakiet dostawcy znajduje się w programie ASP.NET Core [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="35ead-448">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="35ead-449">Pakiet dostawcy nie jest zawarty w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="35ead-449">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="35ead-450">Aby użyć dostawcy, zainstaluj pakiet.</span><span class="sxs-lookup"><span data-stu-id="35ead-450">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="35ead-451">Nie jawnie wywołać <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span><span class="sxs-lookup"><span data-stu-id="35ead-451">Don't explicitly call <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span></span> <span data-ttu-id="35ead-452">Dostawca automatycznie staje się dostępny do aplikacji, gdy aplikacja jest wdrożona w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="35ead-452">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="35ead-453">Jeśli przeznaczonych dla platformy .NET Framework lub odwołuje się do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, Dodaj pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="35ead-453">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="35ead-454">Wywoływanie `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="35ead-454">Invoke `AddAzureWebAppDiagnostics`:</span></span>

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

<span data-ttu-id="35ead-455"><xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> Przeciążenia umożliwia przekazywanie w <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="35ead-455">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="35ead-456">Obiekt ustawień można zastąpić domyślne ustawienia, takie jak rejestrowanie danych wyjściowych szablonu, nazwa obiektu blob i limit rozmiaru pliku.</span><span class="sxs-lookup"><span data-stu-id="35ead-456">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="35ead-457">(*Danych wyjściowych szablonu* jest szablon wiadomości, która jest stosowana do wszystkich dzienników oprócz co to jest dostarczana z `ILogger` wywołania metody.)</span><span class="sxs-lookup"><span data-stu-id="35ead-457">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="35ead-458">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="35ead-458">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="35ead-459">Podczas wdrażania aplikacji usługi App Service aplikacja honoruje ustawienia w [dzienniki diagnostyczne](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) części **usługi App Service** strony w witrynie Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="35ead-459">When you deploy to an App Service app, the application honors the settings in the [Diagnostic Logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="35ead-460">Jeśli te ustawienia zostaną zaktualizowane, zmiany zaczynają obowiązywać natychmiast bez konieczności ponownego uruchomienia lub ponownego wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-460">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Ustawienia rejestrowania platformy Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="35ead-462">Domyślną lokalizacją dla plików dziennika jest *D:\\macierzystego\\LogFiles\\aplikacji* folder i nazwę pliku domyślny jest *yyyymmdd.txt diagnostyki*.</span><span class="sxs-lookup"><span data-stu-id="35ead-462">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="35ead-463">Domyślnego limitu rozmiaru pliku to 10 MB, a maksymalną domyślną liczbę plików, które przechowywane jest 2.</span><span class="sxs-lookup"><span data-stu-id="35ead-463">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="35ead-464">Domyślna nazwa obiektu blob to *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="35ead-464">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="35ead-465">Dostawca działa tylko w przypadku, gdy projekt jest uruchamiany w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="35ead-465">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="35ead-466">Nie ma wpływu, gdy projekt jest uruchamiany lokalnie&mdash;nie zapisywać pliki lokalne lub lokalnym magazynem projektowym dla obiektów blob.</span><span class="sxs-lookup"><span data-stu-id="35ead-466">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

### <a name="azure-log-streaming"></a><span data-ttu-id="35ead-467">Przesyłanie strumieniowe dzienników platformy Azure</span><span class="sxs-lookup"><span data-stu-id="35ead-467">Azure log streaming</span></span>

<span data-ttu-id="35ead-468">Przesyłanie strumieniowe dzienników platformy Azure umożliwia wyświetlanie dzienników aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="35ead-468">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="35ead-469">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="35ead-469">The app server</span></span>
* <span data-ttu-id="35ead-470">Serwer sieci web</span><span class="sxs-lookup"><span data-stu-id="35ead-470">The web server</span></span>
* <span data-ttu-id="35ead-471">Śledzenie żądań zakończonych niepowodzeniem</span><span class="sxs-lookup"><span data-stu-id="35ead-471">Failed request tracing</span></span>

<span data-ttu-id="35ead-472">Aby skonfigurować, przesyłanie strumieniowe dzienników platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="35ead-472">To configure Azure log streaming:</span></span>

* <span data-ttu-id="35ead-473">Przejdź do **dzienniki diagnostyczne** strony na stronie portalu swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-473">Navigate to the **Diagnostics Logs** page from your app's portal page.</span></span>
* <span data-ttu-id="35ead-474">Ustaw **rejestrowanie aplikacji (system plików)** do **na**.</span><span class="sxs-lookup"><span data-stu-id="35ead-474">Set **Application Logging (Filesystem)** to **On**.</span></span>

![Strona portalu dzienniki diagnostyczne platformy Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="35ead-476">Przejdź do **przesyłanie strumieniowe dzienników** strony, aby wyświetlić komunikaty aplikacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-476">Navigate to the **Log Streaming** page to view app messages.</span></span> <span data-ttu-id="35ead-477">Logowanie przeprowadzono za pomocą aplikacji za pośrednictwem `ILogger` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="35ead-477">They're logged by the app through the `ILogger` interface.</span></span>

![Przesyłanie strumieniowe dzienników aplikacji z portalu Azure](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="35ead-479">Rejestrowanie śledzenia w usłudze Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="35ead-479">Azure Application Insights trace logging</span></span>

<span data-ttu-id="35ead-480">Zestaw SDK usługi Application Insights można zbierać i zgłaszać dzienniki generowane przez infrastrukturę rejestrowania platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="35ead-480">The Application Insights SDK can collect and report logs generated by the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="35ead-481">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="35ead-481">For more information, see the following resources:</span></span>

* [<span data-ttu-id="35ead-482">Omówienie usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="35ead-482">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* [<span data-ttu-id="35ead-483">Usługa Application Insights dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35ead-483">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* <span data-ttu-id="35ead-484">[Rejestrowanie kart w usłudze Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span><span class="sxs-lookup"><span data-stu-id="35ead-484">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* [<span data-ttu-id="35ead-485">Przykłady implementacji ILogger szczegółowych informacji w aplikacji</span><span class="sxs-lookup"><span data-stu-id="35ead-485">Application Insights ILogger implementation samples</span></span>](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="35ead-486">Rejestrowanie innych dostawców</span><span class="sxs-lookup"><span data-stu-id="35ead-486">Third-party logging providers</span></span>

<span data-ttu-id="35ead-487">Struktury rejestrowania innych firm, które działają z platformą ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="35ead-487">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="35ead-488">[elmah.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="35ead-488">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="35ead-489">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="35ead-489">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="35ead-490">[JSNLog](http://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="35ead-490">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="35ead-491">[KissLog.net](https://kisslog.net/) ([repozytorium GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="35ead-491">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="35ead-492">[Loggr](http://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="35ead-492">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="35ead-493">[NLog](http://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="35ead-493">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="35ead-494">[Sentry](https://sentry.io/welcome/) ([repozytorium GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="35ead-494">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="35ead-495">[Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="35ead-495">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>
* <span data-ttu-id="35ead-496">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repozytorium Github](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="35ead-496">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="35ead-497">Niektóre środowiska innych producentów mogą wykonywać [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="35ead-497">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="35ead-498">Za pomocą środowiska innych producentów są podobne do przy użyciu jednej z wbudowanych dostawców:</span><span class="sxs-lookup"><span data-stu-id="35ead-498">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="35ead-499">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="35ead-499">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="35ead-500">Wywołaj `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="35ead-500">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="35ead-501">Aby uzyskać więcej informacji można znaleźć w temacie każdy dostawca dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="35ead-501">For more information, see each provider's documentation.</span></span> <span data-ttu-id="35ead-502">Rejestrowanie innych firm nie są obsługiwani przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="35ead-502">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35ead-503">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="35ead-503">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
