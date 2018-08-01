---
title: Rejestrowanie w programie ASP.NET Core
author: ardalis
description: Więcej informacji na temat struktury rejestrowania w programie ASP.NET Core. Odnajdywanie dostawcy wbudowane funkcje rejestrowania i Dowiedz się więcej na temat popularnych dostawców innych firm.
ms.author: tdykstra
ms.date: 07/24/2018
uid: fundamentals/logging/index
ms.openlocfilehash: 60777d4f8664b7f02c806abb6ca40a29602d207f
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39378654"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="c2209-104">Rejestrowanie w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2209-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="c2209-105">Przez [Steve Smith](https://ardalis.com/) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c2209-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c2209-106">Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców logowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="c2209-107">Wbudowani dostawcy umożliwiają wysyłanie dzienników do jednego lub więcej miejsc docelowych, a w ramach rejestrowania innych firm, możesz podłączyć.</span><span class="sxs-lookup"><span data-stu-id="c2209-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="c2209-108">W tym artykule pokazano, jak korzystać z interfejsu API wbudowane funkcje rejestrowania i dostawców w kodzie.</span><span class="sxs-lookup"><span data-stu-id="c2209-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2209-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c2209-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2209-110">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c2209-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="c2209-111">Instrukcje dotyczące rejestrowania strumienia wyjściowego stdout w przypadku hostowania za pomocą programu IIS, zobacz <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="c2209-111">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="c2209-112">Aby uzyskać informacji na temat rejestrowania strumienia wyjściowego stdout w usłudze Azure App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="c2209-112">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="c2209-113">Jak utworzyć dzienników</span><span class="sxs-lookup"><span data-stu-id="c2209-113">How to create logs</span></span>

<span data-ttu-id="c2209-114">Aby utworzyć dzienników, należy zaimplementować [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) obiektu z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera:</span><span class="sxs-lookup"><span data-stu-id="c2209-114">To create logs, implement an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="c2209-115">Następnie wywołań metod rejestrowania dla tego obiektu rejestratora:</span><span class="sxs-lookup"><span data-stu-id="c2209-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="c2209-116">W tym przykładzie tworzy dzienniki za pomocą `TodoController` klasy *kategorii*.</span><span class="sxs-lookup"><span data-stu-id="c2209-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="c2209-117">Kategorie są wyjaśnione [w dalszej części tego artykułu](#log-category).</span><span class="sxs-lookup"><span data-stu-id="c2209-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="c2209-118">Platformy ASP.NET Core nie zapewnia z asynchronicznej metody rejestratora ponieważ rejestrowanie powinny być rozważnie, czy nie jest warte użycia async.</span><span class="sxs-lookup"><span data-stu-id="c2209-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="c2209-119">Jeśli jesteś w sytuacji, w przypadku, gdy nie jest spełniony, należy rozważyć zmianę sposobu logowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="c2209-120">Jeśli magazyn danych jest powolne, najpierw zapisać komunikaty dziennika do szybkiego magazynu, a następnie później przenieść je do magazynu powolne.</span><span class="sxs-lookup"><span data-stu-id="c2209-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="c2209-121">Na przykład Zaloguj się do kolejki komunikatów, która ma odczytu utrwalana w magazynie powolne przez inny proces.</span><span class="sxs-lookup"><span data-stu-id="c2209-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="c2209-122">Jak dodać dostawców</span><span class="sxs-lookup"><span data-stu-id="c2209-122">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2209-123">Dostawcy logowania przyjmuje komunikaty, utworzonych za pomocą `ILogger` obiektów, wyświetla i przechowuje je.</span><span class="sxs-lookup"><span data-stu-id="c2209-123">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="c2209-124">Na przykład konsola dostawca wyświetla komunikaty na konsoli i dostawcy usługi Azure App Service można przechowywać w usłudze Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c2209-124">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="c2209-125">Aby korzystać z dostawcy, należy wywołać dostawcy `Add<ProviderName>` metody rozszerzenia w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2209-125">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="c2209-126">Domyślny szablon projektu umożliwia dostawcom rejestrowania w konsoli i debugowania, z wywołaniem [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) metody rozszerzenia w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2209-126">The default project template enables the Console and Debug logging providers with a call to the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2209-127">Dostawcy logowania przyjmuje komunikaty, utworzonych za pomocą `ILogger` obiektów, wyświetla i przechowuje je.</span><span class="sxs-lookup"><span data-stu-id="c2209-127">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="c2209-128">Na przykład konsola dostawca wyświetla komunikaty na konsoli i dostawcy usługi Azure App Service można przechowywać w usłudze Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c2209-128">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="c2209-129">Można użyć dostawcy, instalowanie pakietu NuGet i wywołanie metody rozszerzenia dostawcy w wystąpieniu [element ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c2209-129">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="c2209-130">Platforma ASP.NET Core [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI) zapewnia `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c2209-130">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="c2209-131">`AddConsole` i `AddDebug` metody rozszerzenia są zdefiniowane w [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pakietów.</span><span class="sxs-lookup"><span data-stu-id="c2209-131">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="c2209-132">Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` jest metoda w wystąpieniu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="c2209-132">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="c2209-133">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) dodaje rejestrowania dostawców w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="c2209-133">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="c2209-134">Jeśli chcesz uzyskać dane wyjściowe dziennika z kodu, który jest wykonywany wcześniej, Dodaj rejestrowania dostawców w `Startup` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="c2209-134">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="c2209-135">Dowiedz się więcej o [wbudowane funkcje rejestrowania dostawców](#built-in-logging-providers) i łącza do [rejestrowania innych dostawców](#third-party-logging-providers) w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="c2209-135">Learn more about the [built-in logging providers](#built-in-logging-providers) and find links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="configuration"></a><span data-ttu-id="c2209-136">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="c2209-136">Configuration</span></span>

<span data-ttu-id="c2209-137">Rejestrowanie dostawcy konfiguracji jest dostarczane przez przynajmniej jednego dostawcy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c2209-137">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="c2209-138">Formaty plików (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="c2209-138">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="c2209-139">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2209-139">Command-line arguments.</span></span>
* <span data-ttu-id="c2209-140">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="c2209-140">Environment variables.</span></span>
* <span data-ttu-id="c2209-141">Obiekty .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="c2209-141">In-memory .NET objects.</span></span>
* <span data-ttu-id="c2209-142">Niezaszyfrowane [Menedżera klucz tajny](xref:security/app-secrets) magazynu.</span><span class="sxs-lookup"><span data-stu-id="c2209-142">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="c2209-143">Użytkownik zaszyfrowanych przechowywanych informacji, takich jak [usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="c2209-143">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="c2209-144">Dostawcy niestandardowi (zainstalowane lub utworzone).</span><span class="sxs-lookup"><span data-stu-id="c2209-144">Custom providers (installed or created).</span></span>

<span data-ttu-id="c2209-145">Na przykład konfiguracja rejestrowania są często dostarczane przez `Logging` części plików ustawień aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2209-145">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="c2209-146">W poniższym przykładzie pokazano zawartość typowej *appsettings. Development.JSON* pliku:</span><span class="sxs-lookup"><span data-stu-id="c2209-146">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="c2209-147">`LogLevel` klucze reprezentują nazwy dziennika.</span><span class="sxs-lookup"><span data-stu-id="c2209-147">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="c2209-148">`Default` Klucz ma zastosowanie do dzienników, które nie zostały jawnie wymienione.</span><span class="sxs-lookup"><span data-stu-id="c2209-148">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="c2209-149">Reprezentuje wartość [poziom dziennika](#log-level) stosowane do danego dziennika.</span><span class="sxs-lookup"><span data-stu-id="c2209-149">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="c2209-150">Dziennik klucze tego zestawu `IncludeScopes` (`Console` w przykładzie), określ, jeśli [dziennika zakresy](#log-scopes) są włączone dla wskazanych dziennika.</span><span class="sxs-lookup"><span data-stu-id="c2209-150">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="c2209-151">`LogLevel` klucze reprezentują nazwy dziennika.</span><span class="sxs-lookup"><span data-stu-id="c2209-151">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="c2209-152">`Default` Klucz ma zastosowanie do dzienników, które nie zostały jawnie wymienione.</span><span class="sxs-lookup"><span data-stu-id="c2209-152">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="c2209-153">Reprezentuje wartość [poziom dziennika](#log-level) stosowane do danego dziennika.</span><span class="sxs-lookup"><span data-stu-id="c2209-153">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="c2209-154">Aby uzyskać informacje dotyczące implementowania dostawcy konfiguracji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c2209-154">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="c2209-155">Przykładowe dane wyjściowe z rejestrowania</span><span class="sxs-lookup"><span data-stu-id="c2209-155">Sample logging output</span></span>

<span data-ttu-id="c2209-156">Za pomocą przykładowego kodu pokazano w poprzedniej sekcji zobaczysz dzienniki w konsoli po uruchomieniu z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2209-156">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="c2209-157">Oto przykład danych wyjściowych konsoli:</span><span class="sxs-lookup"><span data-stu-id="c2209-157">Here's an example of console output:</span></span>

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

<span data-ttu-id="c2209-158">Te dzienniki zostały utworzone, przechodząc do `http://localhost:5000/api/todo/0`, która wyzwala wykonanie obu `ILogger` wywołania pokazano w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2209-158">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="c2209-159">Oto przykład tego samego dzienników, w jakiej występują w oknie Debugowanie Uruchom przykładową aplikację w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c2209-159">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="c2209-160">Dzienniki, które zostały utworzone przez `ILogger` wywołania pokazano w poprzedniej sekcji zaczyna się od "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="c2209-160">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="c2209-161">Dzienniki, które zaczynają się od "Microsoft" kategorie pochodzą z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2209-161">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="c2209-162">ASP.NET Core, sama i kodu aplikacji korzystają z tego samego interfejsu API rejestrowania i tego samego dostawcy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-162">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="c2209-163">W dalszej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-163">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="c2209-164">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="c2209-164">NuGet packages</span></span>

<span data-ttu-id="c2209-165">`ILogger` i `ILoggerFactory` interfejsy są w [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), i znajdują się w domyślnej implementacji dla nich [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="c2209-165">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="c2209-166">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="c2209-166">Log category</span></span>

<span data-ttu-id="c2209-167">A *kategorii* jest dołączany do każdego dziennika, które tworzysz.</span><span class="sxs-lookup"><span data-stu-id="c2209-167">A *category* is included with each log that you create.</span></span> <span data-ttu-id="c2209-168">Określ kategorię, po utworzeniu `ILogger` obiektu.</span><span class="sxs-lookup"><span data-stu-id="c2209-168">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="c2209-169">Kategoria może być dowolnym ciągiem, ale Konwencję polega na użyciu w pełni kwalifikowana nazwa klasy, w którym są zapisywane dzienniki.</span><span class="sxs-lookup"><span data-stu-id="c2209-169">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="c2209-170">Na przykład: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="c2209-170">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="c2209-171">Można określić kategorię, która jako ciąg lub użyć metody rozszerzenia pochodzący z tej kategorii z typu.</span><span class="sxs-lookup"><span data-stu-id="c2209-171">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="c2209-172">Aby określić kategorię, która jako ciąg znaków, należy wywołać `CreateLogger` na `ILoggerFactory` wystąpienia, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="c2209-172">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="c2209-173">W większości przypadków, będzie łatwiejszy w obsłudze `ILogger<T>`, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c2209-173">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="c2209-174">Jest to równoważne z wywoływaniem `CreateLogger` z w pełni kwalifikowana nazwa typu z `T`.</span><span class="sxs-lookup"><span data-stu-id="c2209-174">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="c2209-175">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="c2209-175">Log level</span></span>

<span data-ttu-id="c2209-176">Zawsze możesz pisać dziennika, należy określić jego [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="c2209-176">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="c2209-177">Poziom dziennika wskazuje stopień ważności lub ważności.</span><span class="sxs-lookup"><span data-stu-id="c2209-177">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="c2209-178">Na przykład można napisać `Information` logowania, jeśli metoda zakończy się normalnie, `Warning` Zaloguj się, gdy metoda zwróci kod zwrotny 404, a moduł `Error` rejestrowania podczas catch nieoczekiwany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="c2209-178">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="c2209-179">W poniższym przykładzie kodu nazwy metod (na przykład `LogWarning`) określ poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="c2209-179">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="c2209-180">Pierwszy parametr jest [identyfikator zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="c2209-180">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="c2209-181">Drugi parametr jest [szablon wiadomości z](#log-message-template) przy użyciu symboli zastępczych dla wartości argumentów dostarczone przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="c2209-181">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="c2209-182">Parametry metody zostały omówione bardziej szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="c2209-182">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="c2209-183">Metody dziennika, które obejmują poziom w nazwie metody są [metody rozszerzenia dla ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="c2209-183">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="c2209-184">W tle wywołania tych metod `Log` metody, która przyjmuje `LogLevel` parametru.</span><span class="sxs-lookup"><span data-stu-id="c2209-184">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="c2209-185">Możesz wywołać `Log` bezpośrednio zamiast jednej z tych metod rozszerzenia, ale składnia jest stosunkowo skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="c2209-185">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="c2209-186">Aby uzyskać więcej informacji, zobacz [interfejsu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) i [kod źródłowy rozszerzenia rejestratora](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="c2209-186">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="c2209-187">ASP.NET Core definiuje następujące [poziomy dziennika](/dotnet/api/microsoft.extensions.logging.loglevel), uporządkowanych w tym miejscu, z najmniejszą ważność do najwyższego.</span><span class="sxs-lookup"><span data-stu-id="c2209-187">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="c2209-188">Śledzenie = 0</span><span class="sxs-lookup"><span data-stu-id="c2209-188">Trace = 0</span></span>

  <span data-ttu-id="c2209-189">Aby uzyskać informacje, które jest przydatna tylko dla deweloperów, debugowanie problemu.</span><span class="sxs-lookup"><span data-stu-id="c2209-189">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="c2209-190">Te komunikaty mogą zawierać dane poufne aplikacji i dlatego nie można włączyć w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="c2209-190">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="c2209-191">*Domyślnie wyłączone.*</span><span class="sxs-lookup"><span data-stu-id="c2209-191">*Disabled by default.*</span></span> <span data-ttu-id="c2209-192">Przykład: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="c2209-192">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="c2209-193">Debugowanie = 1</span><span class="sxs-lookup"><span data-stu-id="c2209-193">Debug = 1</span></span>

  <span data-ttu-id="c2209-194">Aby uzyskać informacje, który ma krótkoterminowej przydatność podczas opracowywania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-194">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="c2209-195">Przykład: `Entering method Configure with flag set to true.` zwykle nie włączysz `Debug` poziom dzienniki w środowisku produkcyjnym, chyba że występuje problem z powodu dużej liczby dzienników.</span><span class="sxs-lookup"><span data-stu-id="c2209-195">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="c2209-196">Informacje o = 2</span><span class="sxs-lookup"><span data-stu-id="c2209-196">Information = 2</span></span>

  <span data-ttu-id="c2209-197">Do śledzenia ogólnego przepływu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2209-197">For tracking the general flow of the application.</span></span> <span data-ttu-id="c2209-198">Te dzienniki są zazwyczaj mają niektóre wartości długoterminowe.</span><span class="sxs-lookup"><span data-stu-id="c2209-198">These logs typically have some long-term value.</span></span> <span data-ttu-id="c2209-199">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="c2209-199">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="c2209-200">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="c2209-200">Warning = 3</span></span>

  <span data-ttu-id="c2209-201">Nietypowe lub nieoczekiwanych zdarzeń w usłudze flow aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2209-201">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="c2209-202">Mogą one zawierać błędy lub inne warunki, które nie powodują przerwania aplikacji, ale która może być konieczne należy zbadać.</span><span class="sxs-lookup"><span data-stu-id="c2209-202">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="c2209-203">Obsługiwane wyjątki są spójne użyj `Warning` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="c2209-203">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="c2209-204">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="c2209-204">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="c2209-205">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="c2209-205">Error = 4</span></span>

  <span data-ttu-id="c2209-206">Błędy i wyjątki, które nie mogą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="c2209-206">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="c2209-207">Te komunikaty wskazują wystąpił błąd podczas bieżącego działania lub operacji (takie jak bieżące żądanie HTTP), nie wystąpił błąd całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2209-207">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="c2209-208">Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="c2209-208">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="c2209-209">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="c2209-209">Critical = 5</span></span>

  <span data-ttu-id="c2209-210">Dla błędów, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="c2209-210">For failures that require immediate attention.</span></span> <span data-ttu-id="c2209-211">Przykłady: utratą danych, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="c2209-211">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="c2209-212">Poziom dziennika można użyć do kontrolowania, jak dużo danych wyjściowych dziennika są zapisywane na nośniku określonego lub wyświetlić okno.</span><span class="sxs-lookup"><span data-stu-id="c2209-212">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="c2209-213">Na przykład w środowisku produkcyjnym możesz zechcieć wszystkie dzienniki z `Information` poziomu i niższe, aby przejść do magazynu danych woluminu oraz wszystkie dzienniki z `Warning` poziomu lub nowszej, aby przejść do magazynu danych wartości.</span><span class="sxs-lookup"><span data-stu-id="c2209-213">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="c2209-214">Podczas tworzenia aplikacji, zwykle może wysyłać dzienniki `Warning` lub wyższej ważności do konsoli.</span><span class="sxs-lookup"><span data-stu-id="c2209-214">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="c2209-215">Jeśli potrzebujesz rozwiązać problem, można dodać, a następnie `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="c2209-215">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="c2209-216">[Filtrowanie dziennika](#log-filtering) sekcję w dalszej części tego artykułu opisano sposób kontrolowania poziomy dziennika, który dostawca obsługuje.</span><span class="sxs-lookup"><span data-stu-id="c2209-216">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="c2209-217">Zapisuje w ramach platformy ASP.NET Core `Debug` poziom dzienniki zdarzeń framework.</span><span class="sxs-lookup"><span data-stu-id="c2209-217">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="c2209-218">Przykłady dzienników wcześniej w tym artykule wykluczone dzienniki poniżej `Information` poziomie, więc nie `Debug` poziom dzienniki były wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="c2209-218">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="c2209-219">Oto przykład dzienniki konsoli po uruchomieniu aplikacji przykładowej, skonfigurowana do wyświetlania `Debug` i wyższych dzienniki dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="c2209-219">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="c2209-220">Identyfikator zdarzenia logowania</span><span class="sxs-lookup"><span data-stu-id="c2209-220">Log event ID</span></span>

<span data-ttu-id="c2209-221">Zawsze możesz pisać dziennika, możesz określić *identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="c2209-221">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="c2209-222">Przykładowa aplikacja robi to przy użyciu zdefiniowanej lokalnie `LoggingEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="c2209-222">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="c2209-223">Identyfikator zdarzenia jest wartość całkowitą, która umożliwia kojarzenie zestaw zarejestrowanych zdarzeń ze sobą.</span><span class="sxs-lookup"><span data-stu-id="c2209-223">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="c2209-224">Na przykład dziennika, aby dodać element do koszyka, może być identyfikator 1000 zdarzeń i dziennik ukończenia zakup może być identyfikator zdarzenia 1001.</span><span class="sxs-lookup"><span data-stu-id="c2209-224">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="c2209-225">W danych wyjściowych rejestrowania identyfikator zdarzenia może być przechowywane w polu lub zawarte w wiadomości SMS, w zależności od dostawcy.</span><span class="sxs-lookup"><span data-stu-id="c2209-225">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="c2209-226">Dostawca debugowania nie Wyświetla identyfikatory zdarzeń, ale dostawca konsoli Pokazuje je w nawiasach kwadratowych po kategorii:</span><span class="sxs-lookup"><span data-stu-id="c2209-226">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="c2209-227">Szablon wiadomości dziennika</span><span class="sxs-lookup"><span data-stu-id="c2209-227">Log message template</span></span>

<span data-ttu-id="c2209-228">Każdorazowo, gdy zapisu komunikatu dziennika, należy podać szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="c2209-228">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="c2209-229">Szablon wiadomości może być ciągiem lub może zawierać nazwanego symboli zastępczych, w której argument wartości są umieszczone.</span><span class="sxs-lookup"><span data-stu-id="c2209-229">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="c2209-230">Szablon nie jest ciągiem formatu, a symbole zastępcze powinno się nazywać, nie są numerowane.</span><span class="sxs-lookup"><span data-stu-id="c2209-230">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="c2209-231">Kolejność symboli zastępczych, nie ich nazwy, określa parametry, które służą do zapewniania ich wartości.</span><span class="sxs-lookup"><span data-stu-id="c2209-231">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="c2209-232">Jeśli masz następujący kod:</span><span class="sxs-lookup"><span data-stu-id="c2209-232">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="c2209-233">Wynikowy komunikatu dziennika wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="c2209-233">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="c2209-234">Struktury rejestrowania komunikatów formatowania w ten sposób, aby umożliwić rejestrowanie dostawców, aby zaimplementować [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="c2209-234">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="c2209-235">Ponieważ same argumenty są przekazywane do systemu rejestrowania, a nie tylko szablon sformatowany komunikat rejestrowania dostawców można przechowywać wartości parametrów jako pola oprócz szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="c2209-235">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="c2209-236">Jeśli masz kierowanie dziennika danych wyjściowych do usługi Azure Table Storage i rejestratora metody wywołania wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="c2209-236">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="c2209-237">Każda jednostka usługi Azure Table może mieć `ID` i `RequestTime` właściwości, które upraszcza zapytań dotyczących danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="c2209-237">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="c2209-238">Możesz znaleźć wszystkie dzienniki w ramach określonego `RequestTime` zakresu bez konieczności przeanalizować limit czasu wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="c2209-238">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="c2209-239">Rejestrowania wyjątków</span><span class="sxs-lookup"><span data-stu-id="c2209-239">Logging exceptions</span></span>

<span data-ttu-id="c2209-240">Metody rejestratora mają przeciążenia, które umożliwiają przekazywanie wyjątek, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c2209-240">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="c2209-241">Różnych dostawców obsługiwać informacje o wyjątku na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="c2209-241">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="c2209-242">Oto przykład danych wyjściowych debugowania dostawcy w kodzie pokazanym powyżej.</span><span class="sxs-lookup"><span data-stu-id="c2209-242">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="c2209-243">Filtrowanie dziennika</span><span class="sxs-lookup"><span data-stu-id="c2209-243">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2209-244">Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="c2209-244">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="c2209-245">Żadnych dzienników poniżej minimalnego poziomu nie są przekazywane do tego dostawcy, dzięki czemu nie uzyskać wyświetlane lub przechowywane.</span><span class="sxs-lookup"><span data-stu-id="c2209-245">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="c2209-246">Jeśli chcesz pominąć wszystkie dzienniki, można określić `LogLevel.None` jako minimalny poziom rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-246">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="c2209-247">Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="c2209-247">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="c2209-248">Tworzenie reguły filtrów w konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c2209-248">Create filter rules in configuration</span></span>

<span data-ttu-id="c2209-249">Szablony projektów tworzenie kodu, który wywołuje `CreateDefaultBuilder` skonfigurować rejestrowanie dla dostawców konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-249">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="c2209-250">`CreateDefaultBuilder` Metoda konfiguruje również rejestrowania do wyszukania konfiguracji w `Logging` sekcji przy użyciu kodu, podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="c2209-250">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="c2209-251">Dane konfiguracji Określa poziomy minimalna dziennika przez dostawcę i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c2209-251">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="c2209-252">Ten kod JSON tworzy sześć reguły filtrowania, jeden dla dostawcy debugowania, cztery dla dostawcy konsoli oraz jedną, która ma zastosowanie do wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="c2209-252">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="c2209-253">Zostanie wyświetlone, pokażemy, jak tylko jeden z tych reguł wybrano dla każdego dostawcy podczas `ILogger` obiekt zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="c2209-253">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="c2209-254">Reguły filtrowania w kodzie</span><span class="sxs-lookup"><span data-stu-id="c2209-254">Filter rules in code</span></span>

<span data-ttu-id="c2209-255">Można zarejestrować reguły filtrów w kodzie, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c2209-255">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="c2209-256">Drugi `AddFilter` określa dostawcę debugowania przy użyciu jego nazwy.</span><span class="sxs-lookup"><span data-stu-id="c2209-256">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="c2209-257">Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określa typ dostawcy.</span><span class="sxs-lookup"><span data-stu-id="c2209-257">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="c2209-258">Jak reguły filtrowania zostaną zastosowane.</span><span class="sxs-lookup"><span data-stu-id="c2209-258">How filtering rules are applied</span></span>

<span data-ttu-id="c2209-259">Dane konfiguracji i `AddFilter` kod przedstawiony w powyższych przykładach Tworzenie reguły pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c2209-259">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="c2209-260">Pierwsze sześć pochodzą przykład konfiguracji i ostatnie dwa pochodzą w przykładzie kodu.</span><span class="sxs-lookup"><span data-stu-id="c2209-260">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="c2209-261">Wartość liczbowa</span><span class="sxs-lookup"><span data-stu-id="c2209-261">Number</span></span> | <span data-ttu-id="c2209-262">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2209-262">Provider</span></span>      | <span data-ttu-id="c2209-263">Kategorie, które zaczynają się od...</span><span class="sxs-lookup"><span data-stu-id="c2209-263">Categories that begin with ...</span></span>          | <span data-ttu-id="c2209-264">Minimalny poziom rejestrowania</span><span class="sxs-lookup"><span data-stu-id="c2209-264">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="c2209-265">1</span><span class="sxs-lookup"><span data-stu-id="c2209-265">1</span></span>      | <span data-ttu-id="c2209-266">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c2209-266">Debug</span></span>         | <span data-ttu-id="c2209-267">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="c2209-267">All categories</span></span>                          | <span data-ttu-id="c2209-268">Informacje</span><span class="sxs-lookup"><span data-stu-id="c2209-268">Information</span></span>       |
| <span data-ttu-id="c2209-269">2</span><span class="sxs-lookup"><span data-stu-id="c2209-269">2</span></span>      | <span data-ttu-id="c2209-270">Konsola</span><span class="sxs-lookup"><span data-stu-id="c2209-270">Console</span></span>       | <span data-ttu-id="c2209-271">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="c2209-271">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="c2209-272">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="c2209-272">Warning</span></span>           |
| <span data-ttu-id="c2209-273">3</span><span class="sxs-lookup"><span data-stu-id="c2209-273">3</span></span>      | <span data-ttu-id="c2209-274">Konsola</span><span class="sxs-lookup"><span data-stu-id="c2209-274">Console</span></span>       | <span data-ttu-id="c2209-275">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="c2209-275">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="c2209-276">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c2209-276">Debug</span></span>             |
| <span data-ttu-id="c2209-277">4</span><span class="sxs-lookup"><span data-stu-id="c2209-277">4</span></span>      | <span data-ttu-id="c2209-278">Konsola</span><span class="sxs-lookup"><span data-stu-id="c2209-278">Console</span></span>       | <span data-ttu-id="c2209-279">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="c2209-279">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="c2209-280">Błąd</span><span class="sxs-lookup"><span data-stu-id="c2209-280">Error</span></span>             |
| <span data-ttu-id="c2209-281">5</span><span class="sxs-lookup"><span data-stu-id="c2209-281">5</span></span>      | <span data-ttu-id="c2209-282">Konsola</span><span class="sxs-lookup"><span data-stu-id="c2209-282">Console</span></span>       | <span data-ttu-id="c2209-283">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="c2209-283">All categories</span></span>                          | <span data-ttu-id="c2209-284">Informacje</span><span class="sxs-lookup"><span data-stu-id="c2209-284">Information</span></span>       |
| <span data-ttu-id="c2209-285">6</span><span class="sxs-lookup"><span data-stu-id="c2209-285">6</span></span>      | <span data-ttu-id="c2209-286">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2209-286">All providers</span></span> | <span data-ttu-id="c2209-287">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="c2209-287">All categories</span></span>                          | <span data-ttu-id="c2209-288">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c2209-288">Debug</span></span>             |
| <span data-ttu-id="c2209-289">7</span><span class="sxs-lookup"><span data-stu-id="c2209-289">7</span></span>      | <span data-ttu-id="c2209-290">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2209-290">All providers</span></span> | <span data-ttu-id="c2209-291">System</span><span class="sxs-lookup"><span data-stu-id="c2209-291">System</span></span>                                  | <span data-ttu-id="c2209-292">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c2209-292">Debug</span></span>             |
| <span data-ttu-id="c2209-293">8</span><span class="sxs-lookup"><span data-stu-id="c2209-293">8</span></span>      | <span data-ttu-id="c2209-294">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c2209-294">Debug</span></span>         | <span data-ttu-id="c2209-295">Microsoft</span><span class="sxs-lookup"><span data-stu-id="c2209-295">Microsoft</span></span>                               | <span data-ttu-id="c2209-296">Śledzenia</span><span class="sxs-lookup"><span data-stu-id="c2209-296">Trace</span></span>             |

<span data-ttu-id="c2209-297">Po utworzeniu `ILogger` obiekt do zapisywania dzienników, `ILoggerFactory` obiektu wybiera jedną regułę na dostawcy, aby zastosować do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="c2209-297">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="c2209-298">Wszystkie komunikaty napisane przez to `ILogger` obiektu są filtrowane w zależności od wybranej reguły.</span><span class="sxs-lookup"><span data-stu-id="c2209-298">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="c2209-299">Najbardziej określonej reguły dla każdego dostawcy i pary kategoria możliwe jest wybrana w zaufanym dostępne reguły.</span><span class="sxs-lookup"><span data-stu-id="c2209-299">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="c2209-300">Następujące algorytm jest używany dla każdego dostawcy podczas `ILogger` jest tworzona dla danej kategorii:</span><span class="sxs-lookup"><span data-stu-id="c2209-300">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="c2209-301">Wybierz wszystkie reguły, które odpowiadają przez dostawcę lub jego alias.</span><span class="sxs-lookup"><span data-stu-id="c2209-301">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="c2209-302">Jeśli nie zostaną znalezione, zaznacz wszystkie reguły przy użyciu dostawcy puste.</span><span class="sxs-lookup"><span data-stu-id="c2209-302">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="c2209-303">W wyniku poprzedniego kroku wybierz reguły z najdłużej dopasowania prefiksu kategorii.</span><span class="sxs-lookup"><span data-stu-id="c2209-303">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="c2209-304">Jeśli nie zostaną znalezione, zaznacz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="c2209-304">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="c2209-305">Jeśli wybrano wiele reguł **ostatniego** jeden.</span><span class="sxs-lookup"><span data-stu-id="c2209-305">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="c2209-306">Jeśli nie zaznaczono żadnych reguł, użyj `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="c2209-306">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="c2209-307">Na przykład, załóżmy, że masz powyższej listy reguł i tworzenia `ILogger` obiektu dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="c2209-307">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="c2209-308">W przypadku dostawcy debugowania mają zastosowanie reguły 1, 6 i 8.</span><span class="sxs-lookup"><span data-stu-id="c2209-308">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="c2209-309">Reguła 8 jest najbardziej specyficzną, więc to jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="c2209-309">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="c2209-310">W przypadku dostawcy konsoli mają zastosowanie reguły, 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="c2209-310">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="c2209-311">Reguła 3 jest bardziej konkretny od pozostałych.</span><span class="sxs-lookup"><span data-stu-id="c2209-311">Rule 3 is most specific.</span></span>

<span data-ttu-id="c2209-312">Po utworzeniu dzienników za pomocą `ILogger` dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" dzienniki z `Trace` poziomu i powyżej zostanie umieszczona dostawcy debugowania i dzienniki `Debug` poziomu i powyżej zostanie umieszczona na dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="c2209-312">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="c2209-313">Aliasy dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2209-313">Provider aliases</span></span>

<span data-ttu-id="c2209-314">Nazwa typu można użyć, aby określić dostawcę konfiguracji, ale każdy dostawca definiuje krótszy *alias* jest łatwiejszy w użyciu.</span><span class="sxs-lookup"><span data-stu-id="c2209-314">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="c2209-315">W przypadku dostawców wbudowanych należy stosować następujące aliasy:</span><span class="sxs-lookup"><span data-stu-id="c2209-315">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="c2209-316">Konsola</span><span class="sxs-lookup"><span data-stu-id="c2209-316">Console</span></span>
* <span data-ttu-id="c2209-317">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c2209-317">Debug</span></span>
* <span data-ttu-id="c2209-318">Dziennik zdarzeń</span><span class="sxs-lookup"><span data-stu-id="c2209-318">EventLog</span></span>
* <span data-ttu-id="c2209-319">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="c2209-319">AzureAppServices</span></span>
* <span data-ttu-id="c2209-320">TraceSource</span><span class="sxs-lookup"><span data-stu-id="c2209-320">TraceSource</span></span>
* <span data-ttu-id="c2209-321">EventSource</span><span class="sxs-lookup"><span data-stu-id="c2209-321">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="c2209-322">Minimalny poziom domyślny</span><span class="sxs-lookup"><span data-stu-id="c2209-322">Default minimum level</span></span>

<span data-ttu-id="c2209-323">Istnieje minimalne ustawienie poziomie staje się skuteczny tylko wtedy, gdy nie z konfiguracji czy kodu reguły dla danego dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="c2209-323">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="c2209-324">Poniższy przykład pokazuje, jak ustawić minimalny poziom:</span><span class="sxs-lookup"><span data-stu-id="c2209-324">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="c2209-325">Jeśli nie zostanie jawnie ustawiona minimalny poziom, wartością domyślną jest `Information`, co oznacza, że `Trace` i `Debug` dzienniki są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c2209-325">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="c2209-326">Funkcje filtrowania</span><span class="sxs-lookup"><span data-stu-id="c2209-326">Filter functions</span></span>

<span data-ttu-id="c2209-327">Można napisać kod w funkcji filtru, aby zastosować reguły filtrowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-327">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="c2209-328">Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorie, które nie mają zasady przypisane do nich przez konfiguracji czy kodu.</span><span class="sxs-lookup"><span data-stu-id="c2209-328">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="c2209-329">Kod w funkcji ma dostęp do typ dostawcy, kategorii i poziom dziennika, aby zdecydować, czy komunikat powinny być rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="c2209-329">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="c2209-330">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c2209-330">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2209-331">Niektórzy dostawcy rejestrowania umożliwiają określenie, kiedy zapisany na nośniku lub ignorowane dzienniki na podstawie poziomu dziennika i kategorii.</span><span class="sxs-lookup"><span data-stu-id="c2209-331">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="c2209-332">`AddConsole` i `AddDebug` metody rozszerzenia, zapewnienia przeciążenia, które umożliwiają przekazywanie w kryteriach filtrowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-332">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="c2209-333">Następujący przykładowy kod powoduje, że Konsola dostawca ignorowanie dzienniki poniżej `Warning` poziomu, podczas gdy dostawca debugowania ignoruje dzienniki tworzonych w ramach.</span><span class="sxs-lookup"><span data-stu-id="c2209-333">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="c2209-334">`AddEventLog` Metoda ma przeciążenia, które przyjmuje `EventLogSettings` wystąpienia, co może zawierać funkcji filtrowania w jego `Filter` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c2209-334">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="c2209-335">Dostawca TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jej poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` używa.</span><span class="sxs-lookup"><span data-stu-id="c2209-335">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="c2209-336">Możesz ustawić reguły filtrowania dla wszystkich dostawców, które są zarejestrowane w usłudze `ILoggerFactory` wystąpienia przy użyciu `WithFilter` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="c2209-336">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="c2209-337">W poniższym przykładzie ogranicza dzienniki framework (kategoria zaczyna się od "Microsoft" lub "System") z ostrzeżeniami, a w dzienniku aplikacji na poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-337">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="c2209-338">Jeśli chcesz użyć filtrowania, aby uniemożliwić wszystkie dzienniki są zapisywane dla danej kategorii, można określić `LogLevel.None` jako minimalny poziom rejestrowania dla tej kategorii.</span><span class="sxs-lookup"><span data-stu-id="c2209-338">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="c2209-339">Wartość całkowitą `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="c2209-339">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="c2209-340">`WithFilter` Metody rozszerzenia są dostarczane przez [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="c2209-340">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="c2209-341">Metoda ta zwraca nowy `ILoggerFactory` zarejestrowano wystąpienia, która będzie filtrować komunikaty dziennika, przekazywane do wszystkich dostawców rejestratora.</span><span class="sxs-lookup"><span data-stu-id="c2209-341">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="c2209-342">Nie wpływa na inne `ILoggerFactory` wystąpienia, w tym oryginalny `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c2209-342">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="c2209-343">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="c2209-343">Log scopes</span></span>

<span data-ttu-id="c2209-344">Można grupować zestaw operacji logicznej w ramach *zakres* Aby dołączyć te same dane do każdego dziennika, który jest tworzony jako część tego zbioru.</span><span class="sxs-lookup"><span data-stu-id="c2209-344">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="c2209-345">Na przykład możesz zechcieć każdy dziennik utworzony jako część przetwarzania transakcji, aby uwzględnić ten identyfikator.</span><span class="sxs-lookup"><span data-stu-id="c2209-345">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="c2209-346">Zakres jest `IDisposable` typu, który jest zwracany przez [ILogger.BeginScope&lt;stanu dławienia&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) metody i trwa do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="c2209-346">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="c2209-347">Możesz użyć zakresu, zawijania z Rejestratora wywołania w `using` zablokować, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="c2209-347">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="c2209-348">Poniższy kod umożliwia zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="c2209-348">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="c2209-349">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2209-349">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="c2209-350">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="c2209-350">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="c2209-351">Aby uzyskać informacji na temat konfigurowania, zobacz [konfiguracji](#configuration) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2209-351">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2209-352">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2209-352">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="c2209-353">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="c2209-353">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2209-354">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2209-354">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="c2209-355">Każdy komunikat dziennika zawiera informacje o określonym zakresie:</span><span class="sxs-lookup"><span data-stu-id="c2209-355">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="c2209-356">Wbudowane funkcje rejestrowania dostawców</span><span class="sxs-lookup"><span data-stu-id="c2209-356">Built-in logging providers</span></span>

<span data-ttu-id="c2209-357">ASP.NET Core jest dostarczana w następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="c2209-357">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="c2209-358">Console</span><span class="sxs-lookup"><span data-stu-id="c2209-358">Console</span></span>](#console-provider)
* [<span data-ttu-id="c2209-359">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="c2209-359">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="c2209-360">EventSource</span><span class="sxs-lookup"><span data-stu-id="c2209-360">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="c2209-361">EventLog</span><span class="sxs-lookup"><span data-stu-id="c2209-361">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="c2209-362">TraceSource</span><span class="sxs-lookup"><span data-stu-id="c2209-362">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="c2209-363">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c2209-363">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="c2209-364">Konsola dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2209-364">Console provider</span></span>

<span data-ttu-id="c2209-365">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakiet dostawcy wysyła dane wyjściowe dziennika do konsoli.</span><span class="sxs-lookup"><span data-stu-id="c2209-365">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="c2209-366">[Przeciążenia AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) umożliwiają przekazanej minimalny poziom rejestrowania, funkcję filtru i atrybut typu wartość logiczna, która wskazuje, czy zakresy są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="c2209-366">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="c2209-367">Innym rozwiązaniem jest przekazywanie `IConfiguration` obiektu, który można określić zakresy pomocy technicznej i poziomów rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-367">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="c2209-368">Jeśli rozważasz Konsola dostawca do użycia w środowisku produkcyjnym, należy pamiętać, ma znaczący wpływ na wydajność.</span><span class="sxs-lookup"><span data-stu-id="c2209-368">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="c2209-369">Podczas tworzenia nowego projektu w programie Visual Studio, `AddConsole` metoda wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="c2209-369">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="c2209-370">Ten kod, który odwołuje się do `Logging` części *appSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="c2209-370">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="c2209-371">Ustawienia pokazano limit framework dzienniki, aby ostrzeżenia zezwalając aplikacji do logowania na poziomie debugowania, jak wyjaśniono w [filtrowanie dziennika](#log-filtering) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2209-371">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="c2209-372">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c2209-372">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="c2209-373">Debugowanie dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2209-373">Debug provider</span></span>

<span data-ttu-id="c2209-374">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakiet dostawcy zapisuje dane wyjściowe dziennika za pomocą [system.Diagnostics.Debug —](/dotnet/api/system.diagnostics.debug) klasy (`Debug.WriteLine` wywołania metody).</span><span class="sxs-lookup"><span data-stu-id="c2209-374">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="c2209-375">W systemie Linux, tego dostawcy zapisuje dzienniki na */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="c2209-375">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="c2209-376">[Przeciążenia AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) pozwalają przekazać minimalny poziom rejestrowania lub funkcję filtru.</span><span class="sxs-lookup"><span data-stu-id="c2209-376">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="c2209-377">Dostawca źródła zdarzeń</span><span class="sxs-lookup"><span data-stu-id="c2209-377">EventSource provider</span></span>

<span data-ttu-id="c2209-378">W przypadku aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszym, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zaimplementować pakiet dostawcy śledzenia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c2209-378">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="c2209-379">W Windows, używa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="c2209-379">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="c2209-380">Dostawca jest dla wielu platform, ale nie są dostępne żadne zdarzenie gromadzenia i wyświetlania narzędzia jeszcze dla systemu Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="c2209-380">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="c2209-381">Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [narzędzia PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="c2209-381">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="c2209-382">Istnieją inne narzędzia umożliwiające wyświetlanie dzienników zdarzeń systemu Windows, ale narzędzia PerfView zapewnia najlepsze środowisko do pracy z zdarzenia ETW emitowane przez platformę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c2209-382">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="c2209-383">Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, Dodaj parametry `*Microsoft-Extensions-Logging` do **dodatkowych dostawców** listy.</span><span class="sxs-lookup"><span data-stu-id="c2209-383">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="c2209-384">(Nie przegap gwiazdki na początku ciągu).</span><span class="sxs-lookup"><span data-stu-id="c2209-384">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia Perfview dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="c2209-386">Dostawca dziennika zdarzeń Windows</span><span class="sxs-lookup"><span data-stu-id="c2209-386">Windows EventLog provider</span></span>

<span data-ttu-id="c2209-387">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pakiet dostawcy wysyła dane wyjściowe dziennika w dzienniku zdarzeń Windows.</span><span class="sxs-lookup"><span data-stu-id="c2209-387">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="c2209-388">[Przeciążenia AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) umożliwiają przekazanej `EventLogSettings` lub minimalny poziom rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="c2209-388">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="c2209-389">TraceSource dostawcy</span><span class="sxs-lookup"><span data-stu-id="c2209-389">TraceSource provider</span></span>

<span data-ttu-id="c2209-390">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa dostawcy pakietu [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) bibliotek i dostawców.</span><span class="sxs-lookup"><span data-stu-id="c2209-390">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="c2209-391">[Przeciążenia AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) pozwalają przekazać przełącznik źródła i odbiornika śledzenia.</span><span class="sxs-lookup"><span data-stu-id="c2209-391">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="c2209-392">Aby użyć tego dostawcy, aplikacja ma do uruchamiania na .NET Framework (a nie .NET Core).</span><span class="sxs-lookup"><span data-stu-id="c2209-392">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="c2209-393">Umożliwia dostawcy kierowanie komunikatów w postaci z szeroką gamą [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takich jak [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) używane w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2209-393">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="c2209-394">Poniższy przykład umożliwia skonfigurowanie `TraceSource` dostawcy, która rejestruje `Warning` i wyższych komunikaty w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="c2209-394">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="c2209-395">Dostawca usługi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c2209-395">Azure App Service provider</span></span>

<span data-ttu-id="c2209-396">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pakiet dostawcy zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji w usłudze Azure App Service i do [magazynu obiektów blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c2209-396">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="c2209-397">Pakiet dostawcy jest dostępna dla aplikacji przeznaczonych dla platformy .NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="c2209-397">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2209-398">Jeśli przeznaczony dla platformy .NET Core, pamiętaj o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="c2209-398">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="c2209-399">Pakiet dostawcy znajduje się w programie ASP.NET Core [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) , ale nie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c2209-399">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="c2209-400">Nie jawnie wywołać [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="c2209-400">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="c2209-401">Dostawca automatycznie staje się dostępny do aplikacji, gdy aplikacja jest wdrożona w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c2209-401">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="c2209-402">Jeśli przeznaczonych dla platformy .NET Framework lub odwołuje się do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, Dodaj pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="c2209-402">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="c2209-403">Wywoływanie `AddAzureWebAppDiagnostics` na [element ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="c2209-403">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="c2209-404">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) przeciążenia umożliwia przekazywanie w [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) za pomocą którego można zastąpić domyślne ustawienia, takie jak rejestrowanie danych wyjściowych szablonu, nazwa obiektu blob i plików limit rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="c2209-404">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="c2209-405">(*Danych wyjściowych szablonu* jest szablon wiadomości, która jest stosowana do wszystkich dzienników na elemencie, który podajesz podczas wywoływania `ILogger` metody.)</span><span class="sxs-lookup"><span data-stu-id="c2209-405">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="c2209-406">Podczas wdrażania aplikacji usługi App Service aplikacja honoruje ustawienia w [dzienniki diagnostyczne](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) części **usługi App Service** strony w witrynie Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c2209-406">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="c2209-407">Jeśli te ustawienia zostaną zaktualizowane, zmiany zaczynają obowiązywać natychmiast bez konieczności ponownego uruchomienia lub ponownego wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2209-407">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Ustawienia rejestrowania platformy Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="c2209-409">Domyślną lokalizacją dla plików dziennika jest *D:\\macierzystego\\LogFiles\\aplikacji* folder i nazwę pliku domyślny jest *yyyymmdd.txt diagnostyki*.</span><span class="sxs-lookup"><span data-stu-id="c2209-409">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="c2209-410">Domyślnego limitu rozmiaru pliku to 10 MB, a maksymalną domyślną liczbę plików, które przechowywane jest 2.</span><span class="sxs-lookup"><span data-stu-id="c2209-410">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="c2209-411">Domyślna nazwa obiektu blob to *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="c2209-411">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="c2209-412">Aby uzyskać więcej informacji na temat zachowania domyślnego, zobacz [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="c2209-412">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="c2209-413">Dostawca działa tylko w przypadku, gdy projekt jest uruchamiany w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="c2209-413">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="c2209-414">Nie ma wpływu, gdy projekt jest uruchamiany lokalnie&mdash;nie zapisywać pliki lokalne lub lokalnym magazynem projektowym dla obiektów blob.</span><span class="sxs-lookup"><span data-stu-id="c2209-414">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="c2209-415">Rejestrowanie innych dostawców</span><span class="sxs-lookup"><span data-stu-id="c2209-415">Third-party logging providers</span></span>

<span data-ttu-id="c2209-416">Struktury rejestrowania innych firm, które działają z platformą ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c2209-416">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="c2209-417">[elmah.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="c2209-417">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="c2209-418">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="c2209-418">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="c2209-419">[JSNLog](http://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="c2209-419">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="c2209-420">[Loggr](http://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="c2209-420">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="c2209-421">[NLog](http://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="c2209-421">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="c2209-422">[Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="c2209-422">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="c2209-423">Niektóre środowiska innych producentów mogą wykonywać [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="c2209-423">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="c2209-424">Za pomocą środowiska innych producentów są podobne do przy użyciu jednej z wbudowanych dostawców:</span><span class="sxs-lookup"><span data-stu-id="c2209-424">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="c2209-425">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="c2209-425">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="c2209-426">Wywoływanie metody rozszerzenia na `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="c2209-426">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="c2209-427">Aby uzyskać więcej informacji można znaleźć w temacie każdego szablonu dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="c2209-427">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="c2209-428">Przesyłanie strumieniowe dzienników platformy Azure</span><span class="sxs-lookup"><span data-stu-id="c2209-428">Azure log streaming</span></span>

<span data-ttu-id="c2209-429">Przesyłanie strumieniowe dzienników platformy Azure umożliwia wyświetlenie dziennika aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="c2209-429">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="c2209-430">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="c2209-430">The application server</span></span>
* <span data-ttu-id="c2209-431">Serwer sieci web</span><span class="sxs-lookup"><span data-stu-id="c2209-431">The web server</span></span>
* <span data-ttu-id="c2209-432">Śledzenie żądań zakończonych niepowodzeniem</span><span class="sxs-lookup"><span data-stu-id="c2209-432">Failed request tracing</span></span>

<span data-ttu-id="c2209-433">Aby skonfigurować, przesyłanie strumieniowe dzienników platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="c2209-433">To configure Azure log streaming:</span></span>

* <span data-ttu-id="c2209-434">Przejdź do **dzienniki diagnostyczne** strony na stronie portalu aplikacji</span><span class="sxs-lookup"><span data-stu-id="c2209-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="c2209-435">Ustaw **rejestrowanie aplikacji (system plików)** pozycji włączone.</span><span class="sxs-lookup"><span data-stu-id="c2209-435">Set **Application Logging (Filesystem)** to on.</span></span>

![Strona portalu dzienniki diagnostyczne platformy Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="c2209-437">Przejdź do **przesyłanie strumieniowe dzienników** strony w celu wyświetlenia komunikatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2209-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="c2209-438">Zalogowani przez aplikację za pośrednictwem `ILogger` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c2209-438">They're logged by application through the `ILogger` interface.</span></span>

![Przesyłanie strumieniowe dzienników aplikacji z portalu Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="c2209-440">Rejestrowanie śledzenia w usłudze Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="c2209-440">Azure Application Insights trace logging</span></span>

<span data-ttu-id="c2209-441">[Usługi Application Insights](https://azure.microsoft.com/services/application-insights/) zestawu SDK jest zdolny do zbierania danych telemetrycznych śledzenia z dzienniki wygenerowane za pomocą infrastruktury rejestrowania platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2209-441">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="c2209-442">Aby uzyskać więcej informacji, zobacz [Wiki dotycząca usługi Application Insights/Microsoft-aspnetcore: rejestrowanie](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="c2209-442">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2209-443">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c2209-443">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
