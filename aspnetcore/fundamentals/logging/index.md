---
title: Logowanie ASP.NET Core
author: tdykstra
description: Dowiedz się więcej na temat struktury rejestrowania w ASP.NET Core. Odkryj wbudowanych dostawców rejestrowania i Dowiedz się więcej o popularnych dostawcach innych firm.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/11/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 4fe677e69478284db2ccab655c35b5744b6f63f9
ms.sourcegitcommit: 059ab380744fa3be3b69aa90d431b563c57092cf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68410910"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="9f454-104">Logowanie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f454-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="9f454-105">[Steve Kowalski](https://ardalis.com/) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9f454-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="9f454-106">ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm.</span><span class="sxs-lookup"><span data-stu-id="9f454-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="9f454-107">W tym artykule pokazano, jak używać interfejsu API rejestrowania z wbudowanymi dostawcami.</span><span class="sxs-lookup"><span data-stu-id="9f454-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="9f454-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f454-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="9f454-109">Dodaj dostawców</span><span class="sxs-lookup"><span data-stu-id="9f454-109">Add providers</span></span>

<span data-ttu-id="9f454-110">Dostawca rejestrowania wyświetla lub przechowuje dzienniki.</span><span class="sxs-lookup"><span data-stu-id="9f454-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="9f454-111">Na przykład dostawca konsoli wyświetla dzienniki w konsoli programu, a Dostawca usługi Azure Application Insights przechowuje je na platformie Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9f454-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="9f454-112">Dzienniki mogą być wysyłane do wielu miejsc docelowych przez dodanie wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="9f454-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9f454-113">Aby dodać dostawcę, wywołaj metodę `Add{provider name}` rozszerzenia dostawcy w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f454-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="9f454-114">Poprzedzający kod wymaga odwołania do `Microsoft.Extensions.Logging` i `Microsoft.Extensions.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="9f454-114">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="9f454-115">Domyślne wywołania <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>szablonu projektu, które dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="9f454-115">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="9f454-116">Konsola</span><span class="sxs-lookup"><span data-stu-id="9f454-116">Console</span></span>
* <span data-ttu-id="9f454-117">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="9f454-117">Debug</span></span>
* <span data-ttu-id="9f454-118">EventSource (rozpoczęcie w ASP.NET Core 2,2)</span><span class="sxs-lookup"><span data-stu-id="9f454-118">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="9f454-119">Jeśli używasz `CreateDefaultBuilder`programu, możesz zastąpić domyślnych dostawców własnymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="9f454-119">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="9f454-120">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>i Dodaj żądanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="9f454-120">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9f454-121">Aby użyć dostawcy, zainstaluj jego pakiet NuGet i Wywołaj metodę rozszerzenia dostawcy w wystąpieniu <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span><span class="sxs-lookup"><span data-stu-id="9f454-121">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="9f454-122">ASP.NET Core [iniekcja zależności (di)](xref:fundamentals/dependency-injection) udostępnia `ILoggerFactory` wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="9f454-122">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="9f454-123">Metody rozszerzenia `AddDebug`isą zdefiniowane w pakietach [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft. Extensions. Logging. Debug.](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) `AddConsole`</span><span class="sxs-lookup"><span data-stu-id="9f454-123">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="9f454-124">Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` metodę, przekazując do wystąpienia dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9f454-124">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="9f454-125">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) dodaje dostawców rejestrowania w `Startup.Configure` metodzie.</span><span class="sxs-lookup"><span data-stu-id="9f454-125">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="9f454-126">Aby uzyskać dane wyjściowe dziennika z kodu, który jest wykonywany wcześniej, Dodaj dostawców `Startup` rejestrowania w konstruktorze klas.</span><span class="sxs-lookup"><span data-stu-id="9f454-126">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="9f454-127">Więcej informacji na temat [wbudowanych dostawców rejestrowania](#built-in-logging-providers) i [dostawców rejestrowania innych](#third-party-logging-providers) firm znajduje się w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="9f454-127">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="9f454-128">Tworzenie dzienników</span><span class="sxs-lookup"><span data-stu-id="9f454-128">Create logs</span></span>

<span data-ttu-id="9f454-129">Pobierz obiekt <xref:Microsoft.Extensions.Logging.ILogger%601> z elementu di.</span><span class="sxs-lookup"><span data-stu-id="9f454-129">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9f454-130">Poniższy przykład kontrolera tworzy `Information` i `Warning` rejestruje.</span><span class="sxs-lookup"><span data-stu-id="9f454-130">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="9f454-131">*Kategoria* to `TodoApiSample.Controllers.TodoController` (w pełni `TodoController` kwalifikowana nazwa klasy w aplikacji przykładowej):</span><span class="sxs-lookup"><span data-stu-id="9f454-131">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9f454-132">Poniższy Razor Pages przykład tworzy dzienniki z użyciem `Information` jako *poziomu* i `TodoApiSample.Pages.AboutModel` jako *kategorii*:</span><span class="sxs-lookup"><span data-stu-id="9f454-132">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9f454-133">Poprzedni przykład tworzy dzienniki z `Information` i `Warning` jako  `TodoController` *Kategoria*.</span><span class="sxs-lookup"><span data-stu-id="9f454-133">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="9f454-134">*Poziom* dziennika wskazuje ważność rejestrowanego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="9f454-134">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="9f454-135">*Kategoria* dziennika jest ciągiem, który jest skojarzony z każdym dziennikiem.</span><span class="sxs-lookup"><span data-stu-id="9f454-135">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="9f454-136">Wystąpienie tworzy dzienniki, które mają w pełni kwalifikowaną nazwę typu `T` jako kategorię. `ILogger<T>`</span><span class="sxs-lookup"><span data-stu-id="9f454-136">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="9f454-137">[Poziomy](#log-level) i [Kategorie](#log-category) zostały wyjaśnione bardziej szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="9f454-137">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="9f454-138">Tworzenie dzienników w programie startowym</span><span class="sxs-lookup"><span data-stu-id="9f454-138">Create logs in Startup</span></span>

<span data-ttu-id="9f454-139">Aby napisać dzienniki w `Startup` klasie, `ILogger` Dołącz parametr do sygnatury konstruktora:</span><span class="sxs-lookup"><span data-stu-id="9f454-139">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="9f454-140">Tworzenie dzienników w programie</span><span class="sxs-lookup"><span data-stu-id="9f454-140">Create logs in Program</span></span>

<span data-ttu-id="9f454-141">Aby napisać dzienniki w `Program` klasie, Pobierz wystąpienie z elementu `ILogger` di:</span><span class="sxs-lookup"><span data-stu-id="9f454-141">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="9f454-142">Brak metod rejestratora asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="9f454-142">No asynchronous logger methods</span></span>

<span data-ttu-id="9f454-143">Rejestracja powinna być tak szybka, że nie jest to koszt wydajności kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="9f454-143">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="9f454-144">Jeśli magazyn danych rejestrowania jest wolny, nie zapisuj go bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="9f454-144">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="9f454-145">Najpierw Rozważ zapisanie komunikatów dziennika do szybkiego sklepu, a następnie przeniesienie ich do wolnego magazynu później.</span><span class="sxs-lookup"><span data-stu-id="9f454-145">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="9f454-146">Na przykład jeśli rejestrujesz się do SQL Server, nie chcesz tego robić bezpośrednio w `Log` metodzie, `Log` ponieważ metody są synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="9f454-146">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="9f454-147">Zamiast tego można synchronicznie dodawać komunikaty dziennika do kolejki w pamięci, a proces roboczy w tle ściągał komunikaty z kolejki, aby wykonać asynchroniczne działanie wypychania danych do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9f454-147">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="9f454-148">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="9f454-148">Configuration</span></span>

<span data-ttu-id="9f454-149">Konfiguracja dostawcy rejestrowania jest świadczona przez co najmniej jednego dostawcę konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="9f454-149">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="9f454-150">Formaty plików (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="9f454-150">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="9f454-151">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="9f454-151">Command-line arguments.</span></span>
* <span data-ttu-id="9f454-152">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="9f454-152">Environment variables.</span></span>
* <span data-ttu-id="9f454-153">Obiekty platformy .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="9f454-153">In-memory .NET objects.</span></span>
* <span data-ttu-id="9f454-154">Magazyn niezaszyfrowanego [klucza tajnego](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="9f454-154">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="9f454-155">Zaszyfrowany magazyn użytkowników, taki jak [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="9f454-155">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="9f454-156">Dostawcy niestandardowi (instalowani lub utworzony).</span><span class="sxs-lookup"><span data-stu-id="9f454-156">Custom providers (installed or created).</span></span>

<span data-ttu-id="9f454-157">Na przykład konfiguracja rejestrowania jest zwykle dostarczana przez `Logging` sekcję plików ustawień aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-157">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="9f454-158">Poniższy przykład pokazuje zawartość typowej wartości *appSettings. Plik Development. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9f454-158">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="9f454-159">Właściwość może mieć `LogLevel` właściwości dostawcy dzienników i. `Logging`</span><span class="sxs-lookup"><span data-stu-id="9f454-159">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="9f454-160">Właściwość w obszarze `Logging` określa minimalny poziom rejestrowania wybranych kategorii. [](#log-level) `LogLevel`</span><span class="sxs-lookup"><span data-stu-id="9f454-160">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="9f454-161">W przykładzie, `System` i `Microsoft` kategorie są rejestrowane na `Information` poziomie, a wszystkie inne logowania na `Debug` poziomie.</span><span class="sxs-lookup"><span data-stu-id="9f454-161">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="9f454-162">Inne właściwości w `Logging` obszarze Określ dostawców rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="9f454-162">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="9f454-163">Przykład dotyczy dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="9f454-163">The example is for the Console provider.</span></span> <span data-ttu-id="9f454-164">Jeśli dostawca obsługuje [zakresy rejestrowania](#log-scopes), wskazuje `IncludeScopes` , czy są one włączone.</span><span class="sxs-lookup"><span data-stu-id="9f454-164">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="9f454-165">Właściwość dostawcy (taka jak `Console` w przykładzie) może także `LogLevel` określić właściwość.</span><span class="sxs-lookup"><span data-stu-id="9f454-165">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="9f454-166">`LogLevel`w obszarze dostawca Określa poziomy do rejestrowania dla tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9f454-166">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="9f454-167">Jeśli w programie `Logging.{providername}.LogLevel`są określone poziomy, zastępują one wszystko `Logging.LogLevel`ustawione w.</span><span class="sxs-lookup"><span data-stu-id="9f454-167">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

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

<span data-ttu-id="9f454-168">`LogLevel`klucze reprezentują nazwy dzienników.</span><span class="sxs-lookup"><span data-stu-id="9f454-168">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="9f454-169">`Default` Klucz ma zastosowanie do dzienników, które nie są jawnie wymienione.</span><span class="sxs-lookup"><span data-stu-id="9f454-169">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="9f454-170">Wartość reprezentuje [poziom dziennika](#log-level) zastosowany do danego dziennika.</span><span class="sxs-lookup"><span data-stu-id="9f454-170">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="9f454-171">Informacje o implementowaniu dostawców konfiguracji znajdują <xref:fundamentals/configuration/index>się w temacie.</span><span class="sxs-lookup"><span data-stu-id="9f454-171">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="9f454-172">Przykładowe dane wyjściowe rejestrowania</span><span class="sxs-lookup"><span data-stu-id="9f454-172">Sample logging output</span></span>

<span data-ttu-id="9f454-173">W przypadku przykładowego kodu podanego w poprzedniej sekcji Dzienniki są wyświetlane w konsoli programu, gdy aplikacja jest uruchamiana z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="9f454-173">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="9f454-174">Oto przykład danych wyjściowych konsoli:</span><span class="sxs-lookup"><span data-stu-id="9f454-174">Here's an example of console output:</span></span>

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

<span data-ttu-id="9f454-175">Poprzednie dzienniki zostały wygenerowane przez utworzenie żądania HTTP GET do przykładowej aplikacji w lokalizacji `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="9f454-175">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="9f454-176">Oto przykład tych samych dzienników, które są wyświetlane w oknie debugowania podczas uruchamiania przykładowej aplikacji w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9f454-176">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="9f454-177">Dzienniki utworzone przez `ILogger` wywołania pokazane w poprzedniej sekcji zaczynają się od "TodoApi. controllers. TodoController".</span><span class="sxs-lookup"><span data-stu-id="9f454-177">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="9f454-178">Dzienniki zaczynające się od kategorii "Microsoft" pochodzą z kodu ASP.NET Core Framework.</span><span class="sxs-lookup"><span data-stu-id="9f454-178">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="9f454-179">ASP.NET Core i kod aplikacji używają tego samego rejestrowania interfejsu API i dostawców.</span><span class="sxs-lookup"><span data-stu-id="9f454-179">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="9f454-180">W pozostałej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="9f454-180">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="9f454-181">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="9f454-181">NuGet packages</span></span>

<span data-ttu-id="9f454-182">Interfejsy `ILogger` i`ILoggerFactory` są w [Microsoft. Extensions. Logging. Abstracts](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)i domyślne implementacje dla nich są w [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="9f454-182">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="9f454-183">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="9f454-183">Log category</span></span>

<span data-ttu-id="9f454-184">Po utworzeniu obiektu jest dla niego określona *Kategoria.* `ILogger`</span><span class="sxs-lookup"><span data-stu-id="9f454-184">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="9f454-185">Ta kategoria jest dołączona do każdego komunikatu dziennika utworzonego przez to wystąpienie `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="9f454-185">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="9f454-186">Kategoria może być dowolnym ciągiem, ale Konwencja ma używać nazwy klasy, takiej jak "TodoApi. controllers. TodoController".</span><span class="sxs-lookup"><span data-stu-id="9f454-186">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="9f454-187">Użyj `ILogger<T>` , aby `ILogger` uzyskać wystąpienie używające w `T` pełni kwalifikowanej nazwy typu jako kategorii:</span><span class="sxs-lookup"><span data-stu-id="9f454-187">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="9f454-188">Aby jawnie określić kategorię, wywołaj `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="9f454-188">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="9f454-189">`ILogger<T>`jest odpowiednikiem wywołania `CreateLogger` z w pełni kwalifikowaną `T`nazwą typu.</span><span class="sxs-lookup"><span data-stu-id="9f454-189">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="9f454-190">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="9f454-190">Log level</span></span>

<span data-ttu-id="9f454-191">Każdy dziennik Określa <xref:Microsoft.Extensions.Logging.LogLevel> wartość.</span><span class="sxs-lookup"><span data-stu-id="9f454-191">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="9f454-192">Poziom dziennika wskazuje ważność lub ważność.</span><span class="sxs-lookup"><span data-stu-id="9f454-192">The log level indicates the severity or importance.</span></span> <span data-ttu-id="9f454-193">Przykładowo można napisać `Information` dziennik, gdy metoda jest zwykle zakończona `Warning` , a dziennik, gdy metoda zwraca 404, *nie znaleziono* kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="9f454-193">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="9f454-194">Poniższy kod tworzy `Information` i `Warning` rejestruje:</span><span class="sxs-lookup"><span data-stu-id="9f454-194">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="9f454-195">W powyższym kodzie pierwszy parametr jest IDENTYFIKATORem [zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="9f454-195">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="9f454-196">Drugi parametr jest szablonem wiadomości z symbolami zastępczymi dla wartości argumentów dostarczonych przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="9f454-196">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="9f454-197">Parametry metody zostały wyjaśnione w [sekcji szablon komunikatu](#log-message-template) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="9f454-197">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="9f454-198">Metody rejestrowania, które obejmują poziom w nazwie metody (na przykład `LogInformation` i `LogWarning`) są metodami [rozszerzającymi dla ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="9f454-198">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="9f454-199">Te metody wywołują `Log` metodę, która `LogLevel` pobiera parametr.</span><span class="sxs-lookup"><span data-stu-id="9f454-199">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="9f454-200">`Log` Metodę można wywołać bezpośrednio zamiast jednej z tych metod rozszerzających, ale składnia jest stosunkowo skomplikowana.</span><span class="sxs-lookup"><span data-stu-id="9f454-200">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="9f454-201">Aby uzyskać więcej informacji, <xref:Microsoft.Extensions.Logging.ILogger> Zobacz i [kod źródłowy rozszerzeń rejestratora](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="9f454-201">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="9f454-202">ASP.NET Core definiuje następujące poziomy dziennika uporządkowane w tym miejscu od najniższej do najwyższej wagi.</span><span class="sxs-lookup"><span data-stu-id="9f454-202">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="9f454-203">Ślad = 0</span><span class="sxs-lookup"><span data-stu-id="9f454-203">Trace = 0</span></span>

  <span data-ttu-id="9f454-204">Aby uzyskać informacje, które są zazwyczaj cenne tylko dla debugowania.</span><span class="sxs-lookup"><span data-stu-id="9f454-204">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="9f454-205">Komunikaty te mogą zawierać poufne dane aplikacji, dlatego nie powinny być włączone w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="9f454-205">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="9f454-206">*Domyślnie wyłączona.*</span><span class="sxs-lookup"><span data-stu-id="9f454-206">*Disabled by default.*</span></span>

* <span data-ttu-id="9f454-207">Debuguj = 1</span><span class="sxs-lookup"><span data-stu-id="9f454-207">Debug = 1</span></span>

  <span data-ttu-id="9f454-208">Informacje, które mogą być przydatne podczas tworzenia i debugowania.</span><span class="sxs-lookup"><span data-stu-id="9f454-208">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="9f454-209">Przykład: `Entering method Configure with flag set to true.`Włącz `Debug` dzienniki poziomów w środowisku produkcyjnym tylko w przypadku rozwiązywania problemów, ze względu na dużą ilość dzienników.</span><span class="sxs-lookup"><span data-stu-id="9f454-209">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="9f454-210">Informacje = 2</span><span class="sxs-lookup"><span data-stu-id="9f454-210">Information = 2</span></span>

  <span data-ttu-id="9f454-211">Do śledzenia ogólnego przepływu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-211">For tracking the general flow of the app.</span></span> <span data-ttu-id="9f454-212">Te dzienniki zwykle mają pewną wartość długoterminową.</span><span class="sxs-lookup"><span data-stu-id="9f454-212">These logs typically have some long-term value.</span></span> <span data-ttu-id="9f454-213">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="9f454-213">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="9f454-214">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="9f454-214">Warning = 3</span></span>

  <span data-ttu-id="9f454-215">Dla nietypowych lub nieoczekiwanych zdarzeń w przepływie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-215">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="9f454-216">Mogą to być błędy lub inne warunki, które nie powodują zatrzymania aplikacji, ale konieczne może być zbadanie.</span><span class="sxs-lookup"><span data-stu-id="9f454-216">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="9f454-217">Obsłużone wyjątki są typowym miejscem do korzystania `Warning` z poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="9f454-217">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="9f454-218">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="9f454-218">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="9f454-219">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="9f454-219">Error = 4</span></span>

  <span data-ttu-id="9f454-220">W przypadku błędów i wyjątków, których nie można obsłużyć.</span><span class="sxs-lookup"><span data-stu-id="9f454-220">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="9f454-221">Te komunikaty wskazują niepowodzenie w bieżącym działaniu lub operacji (np. bieżące żądanie HTTP), a nie awaria całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-221">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="9f454-222">Przykładowy komunikat dziennika:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="9f454-222">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="9f454-223">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="9f454-223">Critical = 5</span></span>

  <span data-ttu-id="9f454-224">Dla niepowodzeń, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="9f454-224">For failures that require immediate attention.</span></span> <span data-ttu-id="9f454-225">Przykłady: scenariusze utraty danych, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="9f454-225">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="9f454-226">Poziom dziennika służy do kontrolowania, ile danych wyjściowych dziennika jest zapisywana w określonym nośniku lub oknie wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="9f454-226">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="9f454-227">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9f454-227">For example:</span></span>

* <span data-ttu-id="9f454-228">W obszarze produkcja, `Trace` Wyślij `Information` przez poziom do magazynu danych woluminu.</span><span class="sxs-lookup"><span data-stu-id="9f454-228">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="9f454-229">Wyślij `Warning`domagazynudanych wartości.`Critical`</span><span class="sxs-lookup"><span data-stu-id="9f454-229">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="9f454-230">`Warning` Podczas tworzenia, wysyłaj `Critical` do `Trace` konsoli i dodawaj podczas rozwiązywania problemów. `Information`</span><span class="sxs-lookup"><span data-stu-id="9f454-230">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="9f454-231">W sekcji [filtrowanie dzienników](#log-filtering) w dalszej części tego artykułu wyjaśniono, jak kontrolować poziomy dzienników obsługiwane przez dostawcę.</span><span class="sxs-lookup"><span data-stu-id="9f454-231">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="9f454-232">ASP.NET Core zapisuje dzienniki dla zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="9f454-232">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="9f454-233">Przykłady dzienników znajdujące się wcześniej w tym artykule nie `Information` wykluczają dzienników `Debug` poniżej `Trace` , dlatego nie zostały utworzone dzienniki.</span><span class="sxs-lookup"><span data-stu-id="9f454-233">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="9f454-234">Oto przykład dzienników konsoli utworzonych przez uruchomienie przykładowej aplikacji skonfigurowanej do wyświetlania `Debug` dzienników:</span><span class="sxs-lookup"><span data-stu-id="9f454-234">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="9f454-235">Identyfikator zdarzenia dziennika</span><span class="sxs-lookup"><span data-stu-id="9f454-235">Log event ID</span></span>

<span data-ttu-id="9f454-236">Każdy dziennik może określać *Identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="9f454-236">Each log can specify an *event ID*.</span></span> <span data-ttu-id="9f454-237">Aplikacja Przykładowa wykonuje to przy użyciu lokalnie zdefiniowanej `LoggingEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="9f454-237">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="9f454-238">Identyfikator zdarzenia kojarzy zestaw zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="9f454-238">An event ID associates a set of events.</span></span> <span data-ttu-id="9f454-239">Na przykład wszystkie dzienniki związane z wyświetlaniem listy elementów na stronie mogą być 1001.</span><span class="sxs-lookup"><span data-stu-id="9f454-239">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="9f454-240">Dostawca rejestrowania może przechowywać identyfikator zdarzenia w polu identyfikatora, w komunikacie rejestrowania lub wcale.</span><span class="sxs-lookup"><span data-stu-id="9f454-240">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="9f454-241">Dostawca debugowania nie pokazuje identyfikatorów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="9f454-241">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="9f454-242">Dostawca konsoli pokazuje identyfikatory zdarzeń w nawiasach po kategorii:</span><span class="sxs-lookup"><span data-stu-id="9f454-242">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="9f454-243">Szablon komunikatu dziennika</span><span class="sxs-lookup"><span data-stu-id="9f454-243">Log message template</span></span>

<span data-ttu-id="9f454-244">Każdy dziennik Określa szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9f454-244">Each log specifies a message template.</span></span> <span data-ttu-id="9f454-245">Szablon wiadomości może zawierać symbole zastępcze, dla których podano argumenty.</span><span class="sxs-lookup"><span data-stu-id="9f454-245">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="9f454-246">Użyj nazw dla symboli zastępczych, a nie liczby.</span><span class="sxs-lookup"><span data-stu-id="9f454-246">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="9f454-247">Kolejność symboli zastępczych, nie ich nazw, określa, które parametry są używane do dostarczania ich wartości.</span><span class="sxs-lookup"><span data-stu-id="9f454-247">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="9f454-248">W poniższym kodzie Zwróć uwagę, że nazwy parametrów są poza kolejnością w szablonie wiadomości:</span><span class="sxs-lookup"><span data-stu-id="9f454-248">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="9f454-249">Ten kod tworzy komunikat dziennika z wartościami parametrów w kolejności:</span><span class="sxs-lookup"><span data-stu-id="9f454-249">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="9f454-250">Struktura rejestrowania działa w ten sposób, aby dostawcy rejestrowania mogli zaimplementować [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="9f454-250">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="9f454-251">Same argumenty są przesyłane do systemu rejestrowania, a nie tylko dla sformatowanego szablonu wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9f454-251">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="9f454-252">Te informacje umożliwiają dostawcom rejestrowania przechowywanie wartości parametrów jako pól.</span><span class="sxs-lookup"><span data-stu-id="9f454-252">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="9f454-253">Na przykład załóżmy, że wywołania metod rejestratora wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="9f454-253">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="9f454-254">W przypadku wysyłania dzienników do usługi Azure Table Storage każda jednostka tabeli platformy Azure może mieć `ID` właściwości i `RequestTime` , które upraszczają zapytania dotyczące danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="9f454-254">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="9f454-255">Zapytanie może znaleźć wszystkie dzienniki w określonym `RequestTime` zakresie bez analizowania limitu czasu wiadomości tekstowej.</span><span class="sxs-lookup"><span data-stu-id="9f454-255">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="9f454-256">Wyjątki rejestrowania</span><span class="sxs-lookup"><span data-stu-id="9f454-256">Logging exceptions</span></span>

<span data-ttu-id="9f454-257">Metody rejestratora mają przeciążenia umożliwiające przekazanie wyjątku, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="9f454-257">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="9f454-258">Różni dostawcy obsługują informacje o wyjątkach na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="9f454-258">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="9f454-259">Oto przykład danych wyjściowych dostawcy debugowania z kodu pokazanego powyżej.</span><span class="sxs-lookup"><span data-stu-id="9f454-259">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="9f454-260">Filtrowanie dzienników</span><span class="sxs-lookup"><span data-stu-id="9f454-260">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9f454-261">Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="9f454-261">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="9f454-262">Wszystkie dzienniki poniżej minimalnego poziomu nie są przesyłane do tego dostawcy, więc nie są wyświetlane ani przechowywane.</span><span class="sxs-lookup"><span data-stu-id="9f454-262">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="9f454-263">Aby pominąć wszystkie dzienniki, określ `LogLevel.None` jako minimalny poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="9f454-263">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="9f454-264">Wartość `LogLevel.None` całkowita wynosi 6, która jest większa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="9f454-264">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="9f454-265">Utwórz reguły filtru w konfiguracji</span><span class="sxs-lookup"><span data-stu-id="9f454-265">Create filter rules in configuration</span></span>

<span data-ttu-id="9f454-266">Kod szablonu projektu wywołuje `CreateDefaultBuilder` w celu skonfigurowania rejestrowania dla dostawców konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="9f454-266">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="9f454-267">Metoda konfiguruje również rejestrowanie, aby wyszukać konfigurację `Logging` w sekcji przy użyciu kodu jak poniżej: `CreateDefaultBuilder`</span><span class="sxs-lookup"><span data-stu-id="9f454-267">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17)]

<span data-ttu-id="9f454-268">Dane konfiguracyjne określają minimalne poziomy dziennika według dostawcy i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="9f454-268">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="9f454-269">Ten kod JSON tworzy sześć reguł filtrowania: jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jeden dla wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="9f454-269">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="9f454-270">Dla każdego dostawcy wybierana jest pojedyncza reguła, `ILogger` gdy tworzony jest obiekt.</span><span class="sxs-lookup"><span data-stu-id="9f454-270">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="9f454-271">Filtrowanie reguł w kodzie</span><span class="sxs-lookup"><span data-stu-id="9f454-271">Filter rules in code</span></span>

<span data-ttu-id="9f454-272">Poniższy przykład pokazuje, jak zarejestrować reguły filtru w kodzie:</span><span class="sxs-lookup"><span data-stu-id="9f454-272">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="9f454-273">Drugi `AddFilter` określa dostawcę debugowania za pomocą nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="9f454-273">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="9f454-274">Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określa typu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9f454-274">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="9f454-275">Jak są stosowane reguły filtrowania</span><span class="sxs-lookup"><span data-stu-id="9f454-275">How filtering rules are applied</span></span>

<span data-ttu-id="9f454-276">Dane konfiguracji i `AddFilter` kod przedstawiony w powyższych przykładach tworzą reguły pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="9f454-276">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="9f454-277">Pierwsze sześć pochodzi z przykładu konfiguracji, a ostatnie dwa pochodzą z przykładu kodu.</span><span class="sxs-lookup"><span data-stu-id="9f454-277">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="9f454-278">Wartość liczbowa</span><span class="sxs-lookup"><span data-stu-id="9f454-278">Number</span></span> | <span data-ttu-id="9f454-279">Dostawca</span><span class="sxs-lookup"><span data-stu-id="9f454-279">Provider</span></span>      | <span data-ttu-id="9f454-280">Kategorie zaczynające się od...</span><span class="sxs-lookup"><span data-stu-id="9f454-280">Categories that begin with ...</span></span>          | <span data-ttu-id="9f454-281">Minimalny poziom rejestrowania</span><span class="sxs-lookup"><span data-stu-id="9f454-281">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="9f454-282">1</span><span class="sxs-lookup"><span data-stu-id="9f454-282">1</span></span>      | <span data-ttu-id="9f454-283">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="9f454-283">Debug</span></span>         | <span data-ttu-id="9f454-284">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="9f454-284">All categories</span></span>                          | <span data-ttu-id="9f454-285">Informacje</span><span class="sxs-lookup"><span data-stu-id="9f454-285">Information</span></span>       |
| <span data-ttu-id="9f454-286">2</span><span class="sxs-lookup"><span data-stu-id="9f454-286">2</span></span>      | <span data-ttu-id="9f454-287">Konsola</span><span class="sxs-lookup"><span data-stu-id="9f454-287">Console</span></span>       | <span data-ttu-id="9f454-288">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="9f454-288">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="9f454-289">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="9f454-289">Warning</span></span>           |
| <span data-ttu-id="9f454-290">3</span><span class="sxs-lookup"><span data-stu-id="9f454-290">3</span></span>      | <span data-ttu-id="9f454-291">Konsola</span><span class="sxs-lookup"><span data-stu-id="9f454-291">Console</span></span>       | <span data-ttu-id="9f454-292">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="9f454-292">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="9f454-293">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="9f454-293">Debug</span></span>             |
| <span data-ttu-id="9f454-294">4</span><span class="sxs-lookup"><span data-stu-id="9f454-294">4</span></span>      | <span data-ttu-id="9f454-295">Konsola</span><span class="sxs-lookup"><span data-stu-id="9f454-295">Console</span></span>       | <span data-ttu-id="9f454-296">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="9f454-296">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="9f454-297">Błąd</span><span class="sxs-lookup"><span data-stu-id="9f454-297">Error</span></span>             |
| <span data-ttu-id="9f454-298">5</span><span class="sxs-lookup"><span data-stu-id="9f454-298">5</span></span>      | <span data-ttu-id="9f454-299">Konsola</span><span class="sxs-lookup"><span data-stu-id="9f454-299">Console</span></span>       | <span data-ttu-id="9f454-300">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="9f454-300">All categories</span></span>                          | <span data-ttu-id="9f454-301">Informacje</span><span class="sxs-lookup"><span data-stu-id="9f454-301">Information</span></span>       |
| <span data-ttu-id="9f454-302">6</span><span class="sxs-lookup"><span data-stu-id="9f454-302">6</span></span>      | <span data-ttu-id="9f454-303">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="9f454-303">All providers</span></span> | <span data-ttu-id="9f454-304">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="9f454-304">All categories</span></span>                          | <span data-ttu-id="9f454-305">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="9f454-305">Debug</span></span>             |
| <span data-ttu-id="9f454-306">7</span><span class="sxs-lookup"><span data-stu-id="9f454-306">7</span></span>      | <span data-ttu-id="9f454-307">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="9f454-307">All providers</span></span> | <span data-ttu-id="9f454-308">System</span><span class="sxs-lookup"><span data-stu-id="9f454-308">System</span></span>                                  | <span data-ttu-id="9f454-309">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="9f454-309">Debug</span></span>             |
| <span data-ttu-id="9f454-310">8</span><span class="sxs-lookup"><span data-stu-id="9f454-310">8</span></span>      | <span data-ttu-id="9f454-311">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="9f454-311">Debug</span></span>         | <span data-ttu-id="9f454-312">Microsoft</span><span class="sxs-lookup"><span data-stu-id="9f454-312">Microsoft</span></span>                               | <span data-ttu-id="9f454-313">Szuka</span><span class="sxs-lookup"><span data-stu-id="9f454-313">Trace</span></span>             |

<span data-ttu-id="9f454-314">Po utworzeniu `ILogger` `ILoggerFactory` obiektu obiekt wybiera jedną regułę dla każdego dostawcy, która ma zostać zastosowana do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="9f454-314">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="9f454-315">Wszystkie komunikaty zapisywane przez `ILogger` wystąpienie są filtrowane na podstawie wybranych reguł.</span><span class="sxs-lookup"><span data-stu-id="9f454-315">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="9f454-316">Najbardziej konkretną regułą można wybrać dla każdego dostawcy i pary kategorii z dostępnych reguł.</span><span class="sxs-lookup"><span data-stu-id="9f454-316">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="9f454-317">Następujący algorytm jest używany dla każdego dostawcy, `ILogger` gdy jest tworzony dla danej kategorii:</span><span class="sxs-lookup"><span data-stu-id="9f454-317">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="9f454-318">Wybierz wszystkie reguły, które pasują do dostawcy lub jego aliasu.</span><span class="sxs-lookup"><span data-stu-id="9f454-318">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="9f454-319">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły z pustym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="9f454-319">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="9f454-320">W wyniku poprzedniego kroku wybierz pozycję reguły z najdłuższym prefiksem kategorii.</span><span class="sxs-lookup"><span data-stu-id="9f454-320">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="9f454-321">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="9f454-321">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="9f454-322">Jeśli wybrano wiele reguł, zrób to **ostatnie** .</span><span class="sxs-lookup"><span data-stu-id="9f454-322">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="9f454-323">Jeśli nie wybrano żadnych reguł, `MinimumLevel`Użyj.</span><span class="sxs-lookup"><span data-stu-id="9f454-323">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="9f454-324">Na powyższej liście reguł Załóżmy, że `ILogger` tworzysz obiekt dla kategorii "Microsoft. AspNetCore. MVC. Razor. RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="9f454-324">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="9f454-325">Dla dostawcy debugowania obowiązują reguły 1, 6 i 8.</span><span class="sxs-lookup"><span data-stu-id="9f454-325">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="9f454-326">Reguła 8 jest najbardziej specyficzna, więc jest to jedna wybrana.</span><span class="sxs-lookup"><span data-stu-id="9f454-326">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="9f454-327">W przypadku dostawcy konsoli obowiązują reguły 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="9f454-327">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="9f454-328">Reguła 3 jest najbardziej specyficzna.</span><span class="sxs-lookup"><span data-stu-id="9f454-328">Rule 3 is most specific.</span></span>

<span data-ttu-id="9f454-329">Wystąpienie wyników `ILogger` wysyła `Trace` dzienniki poziomu i powyżej do dostawcy debugowania.</span><span class="sxs-lookup"><span data-stu-id="9f454-329">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="9f454-330">`Debug` Dzienniki poziomów i powyżej są wysyłane do dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="9f454-330">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="9f454-331">Aliasy dostawców</span><span class="sxs-lookup"><span data-stu-id="9f454-331">Provider aliases</span></span>

<span data-ttu-id="9f454-332">Każdy dostawca definiuje *alias* , który może być używany w konfiguracji zamiast w pełni kwalifikowanej nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="9f454-332">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="9f454-333">W przypadku dostawców wbudowanych Użyj następujących aliasów:</span><span class="sxs-lookup"><span data-stu-id="9f454-333">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="9f454-334">Konsola</span><span class="sxs-lookup"><span data-stu-id="9f454-334">Console</span></span>
* <span data-ttu-id="9f454-335">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="9f454-335">Debug</span></span>
* <span data-ttu-id="9f454-336">EventSource</span><span class="sxs-lookup"><span data-stu-id="9f454-336">EventSource</span></span>
* <span data-ttu-id="9f454-337">Elemencie</span><span class="sxs-lookup"><span data-stu-id="9f454-337">EventLog</span></span>
* <span data-ttu-id="9f454-338">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9f454-338">TraceSource</span></span>
* <span data-ttu-id="9f454-339">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="9f454-339">AzureAppServicesFile</span></span>
* <span data-ttu-id="9f454-340">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="9f454-340">AzureAppServicesBlob</span></span>
* <span data-ttu-id="9f454-341">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="9f454-341">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="9f454-342">Domyślny poziom minimalny</span><span class="sxs-lookup"><span data-stu-id="9f454-342">Default minimum level</span></span>

<span data-ttu-id="9f454-343">Istnieje ustawienie minimalnego poziomu, które działa tylko wtedy, gdy nie mają zastosowania żadne reguły z konfiguracji lub kodu dla danego dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="9f454-343">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="9f454-344">Poniższy przykład pokazuje, jak ustawić poziom minimalny:</span><span class="sxs-lookup"><span data-stu-id="9f454-344">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="9f454-345">Jeśli poziom minimalny nie został jawnie ustawiony, wartość domyślna to `Information`, co oznacza, że `Trace` dzienniki i `Debug` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="9f454-345">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="9f454-346">Funkcje filtrowania</span><span class="sxs-lookup"><span data-stu-id="9f454-346">Filter functions</span></span>

<span data-ttu-id="9f454-347">Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorii, które nie mają przypisanych do nich reguł przez konfigurację lub kod.</span><span class="sxs-lookup"><span data-stu-id="9f454-347">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="9f454-348">Kod w funkcji ma dostęp do typu dostawcy, kategorii i poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="9f454-348">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="9f454-349">Przykład:</span><span class="sxs-lookup"><span data-stu-id="9f454-349">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9f454-350">Niektórzy dostawcy rejestrowania umożliwiają określenie, kiedy dzienniki mają być zapisywane na nośniku magazynu, lub ignorowane na podstawie poziomu dziennika i kategorii.</span><span class="sxs-lookup"><span data-stu-id="9f454-350">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="9f454-351">Metody rozszerzające `AddDebug`izapewniają przeciążenia, które akceptują kryteria filtrowania. `AddConsole`</span><span class="sxs-lookup"><span data-stu-id="9f454-351">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="9f454-352">Następujący przykładowy kod powoduje, że dostawca konsoli ignoruje dzienniki poniżej `Warning` poziomu, podczas gdy dostawca debugowania ignoruje dzienniki tworzone przez strukturę.</span><span class="sxs-lookup"><span data-stu-id="9f454-352">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="9f454-353">Metoda ma Przeciążenie, które `EventLogSettings` przyjmuje wystąpienie, które może zawierać funkcję filtrowania w swojej `Filter` właściwości. `AddEventLog`</span><span class="sxs-lookup"><span data-stu-id="9f454-353">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="9f454-354">Dostawca TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jego poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` używa.</span><span class="sxs-lookup"><span data-stu-id="9f454-354">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="9f454-355">Aby ustawić reguły filtrowania dla wszystkich dostawców zarejestrowanych w `ILoggerFactory` wystąpieniu, `WithFilter` Użyj metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="9f454-355">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="9f454-356">Poniższy przykład ogranicza dzienniki struktury (kategoria zaczyna się od "Microsoft" lub "system") do ostrzeżeń podczas rejestrowania na poziomie debugowania dla dzienników utworzonych przez kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-356">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="9f454-357">Aby uniemożliwić zapisywanie jakichkolwiek dzienników, określ `LogLevel.None` jako minimalny poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="9f454-357">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="9f454-358">Wartość `LogLevel.None` całkowita wynosi 6, która jest większa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="9f454-358">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9f454-359">Metoda rozszerzenia jest dostarczana przez pakiet NuGet [Microsoft. Extensions. Logging. Filter.](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) `WithFilter`</span><span class="sxs-lookup"><span data-stu-id="9f454-359">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="9f454-360">Metoda zwraca nowe `ILoggerFactory` wystąpienie, które będzie odfiltrować komunikaty dziennika przesłane do wszystkich dostawców rejestratorów zarejestrowanych z nim.</span><span class="sxs-lookup"><span data-stu-id="9f454-360">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="9f454-361">Nie ma to wpływu na `ILoggerFactory` żadne inne wystąpienia, łącznie `ILoggerFactory` z oryginalnym wystąpieniem.</span><span class="sxs-lookup"><span data-stu-id="9f454-361">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="9f454-362">Kategorie i poziomy systemu</span><span class="sxs-lookup"><span data-stu-id="9f454-362">System categories and levels</span></span>

<span data-ttu-id="9f454-363">Poniżej przedstawiono niektóre kategorie używane przez ASP.NET Core i Entity Framework Core, z informacjami o dziennikach, od których należy się spodziewać:</span><span class="sxs-lookup"><span data-stu-id="9f454-363">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="9f454-364">Kategoria</span><span class="sxs-lookup"><span data-stu-id="9f454-364">Category</span></span>                            | <span data-ttu-id="9f454-365">Uwagi</span><span class="sxs-lookup"><span data-stu-id="9f454-365">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="9f454-366">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="9f454-366">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="9f454-367">Ogólna Diagnostyka ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f454-367">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="9f454-368">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="9f454-368">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="9f454-369">Które klucze zostały wzięte pod uwagę, znaleziono i użyte.</span><span class="sxs-lookup"><span data-stu-id="9f454-369">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="9f454-370">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="9f454-370">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="9f454-371">Dozwolone hosty.</span><span class="sxs-lookup"><span data-stu-id="9f454-371">Hosts allowed.</span></span> |
| <span data-ttu-id="9f454-372">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="9f454-372">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="9f454-373">Jak długo trwa wykonywanie żądań HTTP i czas ich uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="9f454-373">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="9f454-374">Które hostowanie zestawów uruchamiania zostało załadowane.</span><span class="sxs-lookup"><span data-stu-id="9f454-374">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="9f454-375">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="9f454-375">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="9f454-376">Diagnostyka MVC i Razor.</span><span class="sxs-lookup"><span data-stu-id="9f454-376">MVC and Razor diagnostics.</span></span> <span data-ttu-id="9f454-377">Powiązanie modelu, wykonywanie filtru, kompilacja widoku, wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="9f454-377">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="9f454-378">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="9f454-378">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="9f454-379">Informacje o trasie.</span><span class="sxs-lookup"><span data-stu-id="9f454-379">Route matching information.</span></span> |
| <span data-ttu-id="9f454-380">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="9f454-380">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="9f454-381">Reagowanie na uruchamianie, zatrzymywanie i utrzymywanie aktywności.</span><span class="sxs-lookup"><span data-stu-id="9f454-381">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="9f454-382">Informacje o certyfikacie HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9f454-382">HTTPS certificate information.</span></span> |
| <span data-ttu-id="9f454-383">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="9f454-383">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="9f454-384">Obsługiwane pliki.</span><span class="sxs-lookup"><span data-stu-id="9f454-384">Files served.</span></span> |
| <span data-ttu-id="9f454-385">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="9f454-385">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="9f454-386">Ogólna Diagnostyka Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9f454-386">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="9f454-387">Aktywność i Konfiguracja bazy danych, wykrywanie zmian, migracje.</span><span class="sxs-lookup"><span data-stu-id="9f454-387">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="9f454-388">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="9f454-388">Log scopes</span></span>

 <span data-ttu-id="9f454-389">*Zakres* może grupować zestaw operacji logicznych.</span><span class="sxs-lookup"><span data-stu-id="9f454-389">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="9f454-390">Takie grupowanie może służyć do dołączania tych samych danych do każdego dziennika, który został utworzony jako część zestawu.</span><span class="sxs-lookup"><span data-stu-id="9f454-390">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="9f454-391">Na przykład każdy dziennik utworzony w ramach przetwarzania transakcji może zawierać identyfikator transakcji.</span><span class="sxs-lookup"><span data-stu-id="9f454-391">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="9f454-392">Zakres jest `IDisposable` typem zwracanym <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> przez metodę i obowiązuje do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="9f454-392">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="9f454-393">Użyj zakresu przez Zawijanie wywołań rejestratora w `using` bloku:</span><span class="sxs-lookup"><span data-stu-id="9f454-393">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="9f454-394">Poniższy kod włącza zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="9f454-394">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="9f454-395">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f454-395">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="9f454-396">Konfigurowanie opcji rejestratora konsoli jest wymagane do włączenia rejestrowania na podstawie zakresu. `IncludeScopes`</span><span class="sxs-lookup"><span data-stu-id="9f454-396">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="9f454-397">Informacje o konfiguracji znajdują się w sekcji [Konfiguracja](#configuration) .</span><span class="sxs-lookup"><span data-stu-id="9f454-397">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9f454-398">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f454-398">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="9f454-399">Konfigurowanie opcji rejestratora konsoli jest wymagane do włączenia rejestrowania na podstawie zakresu. `IncludeScopes`</span><span class="sxs-lookup"><span data-stu-id="9f454-399">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9f454-400">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f454-400">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="9f454-401">Każdy komunikat dziennika zawiera informacje o zakresie:</span><span class="sxs-lookup"><span data-stu-id="9f454-401">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="9f454-402">Wbudowani dostawcy rejestrowania</span><span class="sxs-lookup"><span data-stu-id="9f454-402">Built-in logging providers</span></span>

<span data-ttu-id="9f454-403">ASP.NET Core dostarcza następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="9f454-403">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="9f454-404">Console</span><span class="sxs-lookup"><span data-stu-id="9f454-404">Console</span></span>](#console-provider)
* [<span data-ttu-id="9f454-405">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="9f454-405">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="9f454-406">EventSource</span><span class="sxs-lookup"><span data-stu-id="9f454-406">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="9f454-407">EventLog</span><span class="sxs-lookup"><span data-stu-id="9f454-407">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="9f454-408">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9f454-408">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="9f454-409">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="9f454-409">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="9f454-410">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="9f454-410">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="9f454-411">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="9f454-411">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="9f454-412">Aby uzyskać informacje na temat strumienia stdout i rejestrowania debugowania za pomocą modułu <xref:test/troubleshoot-azure-iis> ASP.NET Core <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>, zobacz i.</span><span class="sxs-lookup"><span data-stu-id="9f454-412">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="9f454-413">Dostawca konsoli</span><span class="sxs-lookup"><span data-stu-id="9f454-413">Console provider</span></span>

<span data-ttu-id="9f454-414">Pakiet [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) Provider wysyła dane wyjściowe dziennika do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="9f454-414">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="9f454-415">[](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) Funkcja addoverloads umożliwia przekazywanie na minimalnym poziomie dziennika, funkcji filtru i wartości logicznej, która wskazuje, czy zakresy są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f454-415">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="9f454-416">Kolejną opcją jest przekazanie `IConfiguration` obiektu, który może określać obsługę zakresów i poziomy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="9f454-416">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="9f454-417">Aby zapoznać się z opcjami <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions>dostawcy konsoli, zobacz.</span><span class="sxs-lookup"><span data-stu-id="9f454-417">For Console provider options, see <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions>.</span></span>

<span data-ttu-id="9f454-418">Dostawca konsoli ma znaczny wpływ na wydajność i zwykle nie jest odpowiedni do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="9f454-418">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="9f454-419">Podczas tworzenia nowego projektu w programie Visual Studio `AddConsole` Metoda wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="9f454-419">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="9f454-420">Ten kod odnosi się `Logging` do sekcji pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9f454-420">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="9f454-421">Ustawienia pokazują, że można ograniczyć dzienniki struktury do ostrzeżeń, jednocześnie umożliwiając aplikacji rejestrowanie na poziomie debugowania, zgodnie z opisem w sekcji [filtrowanie dzienników](#log-filtering) .</span><span class="sxs-lookup"><span data-stu-id="9f454-421">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="9f454-422">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9f454-422">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="9f454-423">Aby wyświetlić dane wyjściowe rejestrowania konsoli, Otwórz wiersz polecenia w folderze projektu i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="9f454-423">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="9f454-424">Dostawca debugowania</span><span class="sxs-lookup"><span data-stu-id="9f454-424">Debug provider</span></span>

<span data-ttu-id="9f454-425">Pakiet [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) Provider zapisuje dane wyjściowe dziennika przy użyciu klasy [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug) (`Debug.WriteLine` wywołania metod).</span><span class="sxs-lookup"><span data-stu-id="9f454-425">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="9f454-426">W systemie Linux ten dostawca zapisuje dzienniki do */var/log/Message*.</span><span class="sxs-lookup"><span data-stu-id="9f454-426">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="9f454-427">[](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) Funkcja adddebug overloads pozwala przekazać minimalny poziom rejestrowania lub funkcję filtru.</span><span class="sxs-lookup"><span data-stu-id="9f454-427">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="9f454-428">Dostawca EventSource</span><span class="sxs-lookup"><span data-stu-id="9f454-428">EventSource provider</span></span>

<span data-ttu-id="9f454-429">W przypadku aplikacji przeznaczonych do ASP.NET Core 1.1.0 lub nowszych pakiet dostawcy [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) może zaimplementować śledzenie zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="9f454-429">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="9f454-430">W systemie Windows używa [funkcji ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="9f454-430">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="9f454-431">Dostawca jest międzyplatformowy, ale nie ma jeszcze kolekcji zdarzeń i narzędzi do wyświetlania dla systemu Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="9f454-431">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="9f454-432">Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [Narzędzia Narzędzia PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="9f454-432">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="9f454-433">Istnieją inne narzędzia do wyświetlania dzienników ETW, ale narzędzia PerfView zapewnia najlepsze środowisko pracy z zdarzeniami ETW emitowanymi przez ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9f454-433">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="9f454-434">Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, Dodaj ciąg `*Microsoft-Extensions-Logging` do listy **dodatkowych dostawców** .</span><span class="sxs-lookup"><span data-stu-id="9f454-434">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="9f454-435">(Nie przegap gwiazdki na początku ciągu znaków).</span><span class="sxs-lookup"><span data-stu-id="9f454-435">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia PerfView dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="9f454-437">Dostawca dziennika zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="9f454-437">Windows EventLog provider</span></span>

<span data-ttu-id="9f454-438">Pakiet dostawcy [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) wysyła dane wyjściowe dziennika do dziennika zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="9f454-438">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="9f454-439">[Przeciążenia addeventlog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) umożliwiają przekazywanie `EventLogSettings` lub minimalny poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="9f454-439">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="9f454-440">Dostawca TraceSource</span><span class="sxs-lookup"><span data-stu-id="9f454-440">TraceSource provider</span></span>

<span data-ttu-id="9f454-441">Pakiet dostawcy [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa <xref:System.Diagnostics.TraceSource> bibliotek i dostawców.</span><span class="sxs-lookup"><span data-stu-id="9f454-441">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

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

<span data-ttu-id="9f454-442">[Przeciążenia AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) umożliwiają przekazywanie danych w przełączniku źródłowym i odbiorniku śledzenia.</span><span class="sxs-lookup"><span data-stu-id="9f454-442">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="9f454-443">Aby można było korzystać z tego dostawcy, aplikacja musi działać na .NET Framework (a nie na platformie .NET Core).</span><span class="sxs-lookup"><span data-stu-id="9f454-443">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="9f454-444">Dostawca może kierować komunikaty do różnych odbiorników, [](/dotnet/framework/debug-trace-profile/trace-listeners)takich jak <xref:System.Diagnostics.TextWriterTraceListener> używane w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-444">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9f454-445">Poniższy przykład konfiguruje `TraceSource` dostawcę, który rejestruje `Warning` i wyższych komunikatów do okna konsoli.</span><span class="sxs-lookup"><span data-stu-id="9f454-445">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="9f454-446">Dostawca Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9f454-446">Azure App Service provider</span></span>

<span data-ttu-id="9f454-447">Pakiet [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) Provider zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji Azure App Service i w usłudze [BLOB Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9f454-447">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="9f454-448">Pakiet dostawcy jest dostępny dla aplikacji przeznaczonych dla platformy .NET Core 1,1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="9f454-448">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9f454-449">W przypadku określania wartości .NET Core należy zwrócić uwagę na następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="9f454-449">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="9f454-450">Pakiet dostawcy jest zawarty w ASP.NET Core [Microsoft. AspNetCore. All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="9f454-450">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="9f454-451">Pakiet dostawcy nie jest uwzględniony w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9f454-451">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="9f454-452">Aby użyć dostawcy, zainstaluj pakiet.</span><span class="sxs-lookup"><span data-stu-id="9f454-452">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9f454-453">Jeśli element docelowy .NET Framework lub odwołuje się `Microsoft.AspNetCore.App` do pakietu, Dodaj pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="9f454-453">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="9f454-454">Wywołaj `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="9f454-454">Invoke `AddAzureWebAppDiagnostics`:</span></span>

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

<span data-ttu-id="9f454-455"><xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> Przeciążenie umożliwia <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>przekazywanie.</span><span class="sxs-lookup"><span data-stu-id="9f454-455">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="9f454-456">Obiekt Settings może przesłonić ustawienia domyślne, takie jak szablon danych wyjściowych rejestrowania, nazwa obiektu BLOB i limit rozmiaru pliku.</span><span class="sxs-lookup"><span data-stu-id="9f454-456">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="9f454-457">(*Szablon danych wyjściowych* to szablon wiadomości, który jest stosowany do wszystkich dzienników oprócz tego, co jest dostępne `ILogger` w wywołaniu metody).</span><span class="sxs-lookup"><span data-stu-id="9f454-457">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9f454-458">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="9f454-458">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="9f454-459">Po wdrożeniu w aplikacji App Service, aplikacja będzie przestrzegać ustawień w sekcji [dzienniki App Service](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) na stronie **App Service** Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9f454-459">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="9f454-460">Po zaktualizowaniu następujących ustawień zmiany zaczynają obowiązywać natychmiast, bez konieczności ponownego uruchomienia lub ponownej wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-460">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="9f454-461">**Rejestrowanie aplikacji (system plików)**</span><span class="sxs-lookup"><span data-stu-id="9f454-461">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="9f454-462">**Rejestrowanie aplikacji (BLOB)**</span><span class="sxs-lookup"><span data-stu-id="9f454-462">**Application Logging (Blob)**</span></span>

<span data-ttu-id="9f454-463">Domyślną lokalizacją plików dziennika jest folder *D\\:\\Home\\LogFiles* , a domyślna nazwa pliku to *Diagnostics-YYYYMMDD. txt*.</span><span class="sxs-lookup"><span data-stu-id="9f454-463">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="9f454-464">Domyślny limit rozmiaru pliku wynosi 10 MB, a domyślna maksymalna liczba zachowywanych plików to 2.</span><span class="sxs-lookup"><span data-stu-id="9f454-464">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="9f454-465">Domyślną nazwą obiektu BLOB jest *{App-Name} {timestamp}/yyyy/mm/dd/hh/{GUID}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="9f454-465">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="9f454-466">Dostawca działa tylko wtedy, gdy projekt jest uruchomiony w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="9f454-466">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="9f454-467">Nie ma ono wpływu, gdy projekt jest uruchamiany lokalnie&mdash;, nie zapisuje w plikach lokalnych ani w lokalnym magazynie programistycznym dla obiektów BLOB.</span><span class="sxs-lookup"><span data-stu-id="9f454-467">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="9f454-468">Przesyłanie strumieniowe dzienników Azure</span><span class="sxs-lookup"><span data-stu-id="9f454-468">Azure log streaming</span></span>

<span data-ttu-id="9f454-469">Usługa przesyłania strumieniowego w usłudze Azure log umożliwia wyświetlanie dziennika aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="9f454-469">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="9f454-470">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="9f454-470">The app server</span></span>
* <span data-ttu-id="9f454-471">Serwer sieci Web</span><span class="sxs-lookup"><span data-stu-id="9f454-471">The web server</span></span>
* <span data-ttu-id="9f454-472">Śledzenie nieudanych żądań</span><span class="sxs-lookup"><span data-stu-id="9f454-472">Failed request tracing</span></span>

<span data-ttu-id="9f454-473">Aby skonfigurować przesyłanie strumieniowe dzienników Azure:</span><span class="sxs-lookup"><span data-stu-id="9f454-473">To configure Azure log streaming:</span></span>

* <span data-ttu-id="9f454-474">Przejdź do strony **dzienników App Service** ze strony portalu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-474">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="9f454-475">Ustaw **Rejestrowanie aplikacji (system plików)** na **włączone**.</span><span class="sxs-lookup"><span data-stu-id="9f454-475">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="9f454-476">Wybierz **poziom**dziennika.</span><span class="sxs-lookup"><span data-stu-id="9f454-476">Choose the log **Level**.</span></span>

<span data-ttu-id="9f454-477">Przejdź do strony **strumień dziennika** , aby wyświetlić komunikaty aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f454-477">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="9f454-478">Są one rejestrowane przez aplikację za pomocą `ILogger` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="9f454-478">They're logged by the app through the `ILogger` interface.</span></span>

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="9f454-479">Rejestrowanie śledzenia Application Insights platformy Azure</span><span class="sxs-lookup"><span data-stu-id="9f454-479">Azure Application Insights trace logging</span></span>

<span data-ttu-id="9f454-480">Pakiet [Microsoft. Extensions. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) Provider zapisuje dzienniki na platformie Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9f454-480">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="9f454-481">Application Insights to usługa, która monitoruje aplikację internetową i udostępnia narzędzia do wykonywania zapytań i analizowania danych telemetrycznych.</span><span class="sxs-lookup"><span data-stu-id="9f454-481">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="9f454-482">Jeśli używasz tego dostawcy, możesz wysyłać zapytania i analizować dzienniki przy użyciu narzędzi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9f454-482">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="9f454-483">Dostawca rejestrowania jest dołączony jako zależność [Microsoft. ApplicationInsights. AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), który jest pakietem, który zapewnia wszystkie dostępne dane telemetryczne dla ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f454-483">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="9f454-484">Jeśli używasz tego pakietu, nie musisz instalować pakietu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9f454-484">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="9f454-485">Nie używaj&mdash;pakietu [Microsoft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) , który jest przeznaczony dla ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="9f454-485">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="9f454-486">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="9f454-486">For more information, see the following resources:</span></span>

* [<span data-ttu-id="9f454-487">Przegląd Application Insights</span><span class="sxs-lookup"><span data-stu-id="9f454-487">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="9f454-488">[Application Insights dla ASP.NET Core aplikacji](/azure/azure-monitor/app/asp-net-core) — Zacznij tutaj, jeśli chcesz zaimplementować cały zakres Application Insights telemetrii wraz z rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="9f454-488">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="9f454-489">[ApplicationInsightsLoggerProvider for .NET Core ILogger](/azure/azure-monitor/app/ilogger) — Rozpocznij tutaj, jeśli chcesz zaimplementować dostawcę rejestrowania bez pozostałej części telemetrii Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9f454-489">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="9f454-490">[Karty rejestrowania Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="9f454-490">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="9f454-491">[Zainstaluj, skonfiguruj i zainicjuj samouczek Application INSIGHTS SDK](/learn/modules/instrument-web-app-code-with-application-insights) — interaktywny w witrynie Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="9f454-491">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>
::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="9f454-492">Dostawcy rejestrowania innych firm</span><span class="sxs-lookup"><span data-stu-id="9f454-492">Third-party logging providers</span></span>

<span data-ttu-id="9f454-493">Platformy rejestrowania innych firm, które współpracują z ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="9f454-493">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="9f454-494">[ELMAH.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="9f454-494">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="9f454-495">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([Repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="9f454-495">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="9f454-496">[JSNLog](https://jsnlog.com/) ([Repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="9f454-496">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="9f454-497">[KissLog.NET](https://kisslog.net/) ([repozytorium GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="9f454-497">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="9f454-498">[Loggr](https://loggr.net/) ([Repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="9f454-498">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="9f454-499">[NLOG](https://nlog-project.org/) ([Repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="9f454-499">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="9f454-500">[Sentry](https://sentry.io/welcome/) ([Repozytorium GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="9f454-500">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="9f454-501">[Serilog](https://serilog.net/) ([Repozytorium GitHub](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="9f454-501">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="9f454-502">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Repozytorium GitHub](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="9f454-502">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="9f454-503">Niektóre struktury innych firm mogą wykonywać [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="9f454-503">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="9f454-504">Korzystanie z struktury innej firmy jest podobne do korzystania z jednego z wbudowanych dostawców:</span><span class="sxs-lookup"><span data-stu-id="9f454-504">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="9f454-505">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="9f454-505">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="9f454-506">Wywołaj `ILoggerFactory`metodę.</span><span class="sxs-lookup"><span data-stu-id="9f454-506">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="9f454-507">Aby uzyskać więcej informacji, zobacz dokumentację każdego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9f454-507">For more information, see each provider's documentation.</span></span> <span data-ttu-id="9f454-508">Dostawcy rejestrowania innych firm nie są obsługiwani przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9f454-508">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f454-509">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9f454-509">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
