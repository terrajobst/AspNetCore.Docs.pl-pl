---
title: Logowanie do platformy ASP.NET Core
author: ardalis
description: Więcej informacji na temat struktury rejestrowania w ASP.NET Core. Odnajdywanie dostawców wbudowanych rejestrowania i Dowiedz się więcej o innych popularnych dostawców.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: 969ad303c3fee06aa40d43140153ffbf58b735db
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126290"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="a7bea-104">Logowanie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7bea-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="a7bea-105">Przez [Steve Smith](https://ardalis.com/) i [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a7bea-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a7bea-106">Platformy ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnych dostawców rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="a7bea-107">Dostawców wbudowanych umożliwiają wysyłanie dzienników do jednego lub więcej miejsc docelowych, a można podłączyć w ramach rejestrowania innych firm.</span><span class="sxs-lookup"><span data-stu-id="a7bea-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="a7bea-108">W tym artykule pokazano, jak korzystać z interfejsu API wbudowanych rejestrowania i dostawców w kodzie.</span><span class="sxs-lookup"><span data-stu-id="a7bea-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a7bea-109">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a7bea-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a7bea-110">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a7bea-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

## <a name="how-to-create-logs"></a><span data-ttu-id="a7bea-111">Jak utworzyć dzienników</span><span class="sxs-lookup"><span data-stu-id="a7bea-111">How to create logs</span></span>

<span data-ttu-id="a7bea-112">Aby utworzyć dzienników, zaimplementuj [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) obiekt z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera:</span><span class="sxs-lookup"><span data-stu-id="a7bea-112">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="a7bea-113">Następnie wywołania metody rejestrowania dla tego obiektu rejestratora:</span><span class="sxs-lookup"><span data-stu-id="a7bea-113">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="a7bea-114">W tym przykładzie tworzy dzienniki z `TodoController` klasy *kategorii*.</span><span class="sxs-lookup"><span data-stu-id="a7bea-114">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="a7bea-115">Kategorie są objaśnione [dalszej części tego artykułu](#log-category).</span><span class="sxs-lookup"><span data-stu-id="a7bea-115">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="a7bea-116">Platformy ASP.NET Core nie async rejestratora udostępniają metody służące ponieważ rejestrowania należy rozważnie nie warto kosztów asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="a7bea-116">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="a7bea-117">Jeśli pracujesz w sytuacji, w przypadku, gdy nie jest prawdziwe, należy rozważyć zmianę sposobu logowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-117">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="a7bea-118">Jeśli w magazynie danych przebiega powoli, najpierw zapisać komunikaty dziennika do szybkiego magazynu, a następnie przenieść je do magazynu powolne później.</span><span class="sxs-lookup"><span data-stu-id="a7bea-118">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="a7bea-119">Na przykład Zaloguj się do kolejki komunikatów, która ma odczytu i utrwalone w magazynie powolne przez inny proces.</span><span class="sxs-lookup"><span data-stu-id="a7bea-119">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="a7bea-120">Jak dodać dostawców</span><span class="sxs-lookup"><span data-stu-id="a7bea-120">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a7bea-121">Dostawcy logowania ma utworzonych za pomocą wiadomości `ILogger` obiektów, wyświetla i przechowuje je.</span><span class="sxs-lookup"><span data-stu-id="a7bea-121">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="a7bea-122">Na przykład konsola dostawca wyświetla komunikaty w konsoli i dostawcy usługi Azure App Service można przechowywać w magazynie obiektów blob Azure.</span><span class="sxs-lookup"><span data-stu-id="a7bea-122">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="a7bea-123">Aby użyć dostawcy, należy wywołać dostawcy `Add<ProviderName>` — metoda rozszerzenia w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7bea-123">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="a7bea-124">Domyślny szablon projektu umożliwia rejestrowanie z [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) metody:</span><span class="sxs-lookup"><span data-stu-id="a7bea-124">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a7bea-125">Dostawcy logowania ma utworzonych za pomocą wiadomości `ILogger` obiektów, wyświetla i przechowuje je.</span><span class="sxs-lookup"><span data-stu-id="a7bea-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="a7bea-126">Na przykład konsola dostawca wyświetla komunikaty w konsoli i dostawcy usługi Azure App Service można przechowywać w magazynie obiektów blob Azure.</span><span class="sxs-lookup"><span data-stu-id="a7bea-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="a7bea-127">Do korzystania z dostawcy, instalowanie pakietu NuGet i wywołanie metody rozszerzenia dostawcy na wystąpienie [element ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a7bea-127">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="a7bea-128">Platformy ASP.NET Core [iniekcji zależności](xref:fundamentals/dependency-injection) (Podpisane) zapewnia `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a7bea-128">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="a7bea-129">`AddConsole` i `AddDebug` metody rozszerzenia są definiowane w [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pakietów.</span><span class="sxs-lookup"><span data-stu-id="a7bea-129">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="a7bea-130">Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` metody, przekazując wystąpienie dostawcy.</span><span class="sxs-lookup"><span data-stu-id="a7bea-130">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="a7bea-131">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) dodaje rejestrowanie dostawców w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="a7bea-131">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="a7bea-132">Uzyskanie danych wyjściowych dziennika z kodu, która wykonuje wcześniej, dodać rejestrowanie dostawców w `Startup` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="a7bea-132">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="a7bea-133">Znajdują się informacje o poszczególnych [rejestrowania wbudowanego dostawcy](#built-in-logging-providers) i łącza do [dostawców innych firm rejestrowania](#third-party-logging-providers) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-133">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="settings-file-configuration"></a><span data-ttu-id="a7bea-134">Ustawienia pliku konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a7bea-134">Settings file configuration</span></span>

<span data-ttu-id="a7bea-135">Każdy z powyższych przykładach w [sposobu dodawania dostawcy](#how-to-add-providers) sekcji ładuje konfiguracji dostawcy rejestrowania z `Logging` sekcji pliki ustawień aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-135">Each of the preceding examples in the [How to add providers](#how-to-add-providers) section loads logging provider configuration from the `Logging` section of app settings files.</span></span> <span data-ttu-id="a7bea-136">W poniższym przykładzie przedstawiono typowe zawartość *appsettings. Development.JSON* pliku:</span><span class="sxs-lookup"><span data-stu-id="a7bea-136">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="a7bea-137">`LogLevel` klucze oznacza nazwę dziennika.</span><span class="sxs-lookup"><span data-stu-id="a7bea-137">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="a7bea-138">`Default` Klucz dotyczy dzienniki nie są jawnie wymienione.</span><span class="sxs-lookup"><span data-stu-id="a7bea-138">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="a7bea-139">Reprezentuje wartość [poziom dziennika](#log-level) stosowane do danej dziennika.</span><span class="sxs-lookup"><span data-stu-id="a7bea-139">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="a7bea-140">Tego zestawu kluczy dziennika `IncludeScopes` (`Console` w przykładzie), określ, czy [dziennika zakresy](#log-scopes) są włączone dla wskazanych dziennika.</span><span class="sxs-lookup"><span data-stu-id="a7bea-140">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="a7bea-141">`LogLevel` klucze oznacza nazwę dziennika.</span><span class="sxs-lookup"><span data-stu-id="a7bea-141">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="a7bea-142">`Default` Klucz dotyczy dzienniki nie są jawnie wymienione.</span><span class="sxs-lookup"><span data-stu-id="a7bea-142">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="a7bea-143">Reprezentuje wartość [poziom dziennika](#log-level) stosowane do danej dziennika.</span><span class="sxs-lookup"><span data-stu-id="a7bea-143">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

## <a name="sample-logging-output"></a><span data-ttu-id="a7bea-144">Przykładowe dane wyjściowe rejestrowania</span><span class="sxs-lookup"><span data-stu-id="a7bea-144">Sample logging output</span></span>

<span data-ttu-id="a7bea-145">Z przykładowy kod wyświetlany w poprzedniej sekcji zobaczysz dzienniki w konsoli po uruchomieniu z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="a7bea-145">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="a7bea-146">Oto przykładowe dane wyjściowe konsoli:</span><span class="sxs-lookup"><span data-stu-id="a7bea-146">Here's an example of console output:</span></span>

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

<span data-ttu-id="a7bea-147">Te dzienniki zostały utworzone, przechodząc do `http://localhost:5000/api/todo/0`, który wyzwala wykonanie obu `ILogger` wywołania wyświetlone w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-147">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="a7bea-148">Oto przykład tego samego dzienniki znajdujące się w oknie Debugowanie po uruchomieniu przykładowej aplikacji w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a7bea-148">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="a7bea-149">Dzienniki, które zostały utworzone przez `ILogger` wywołania wyświetlone w poprzedniej sekcji rozpoczynać się od "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="a7bea-149">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="a7bea-150">Dzienniki, które zaczynają się od "Microsoft" kategorie są z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7bea-150">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="a7bea-151">Kod aplikacji i ASP.NET Core, sama korzystają z tego samego interfejsu API rejestrowania i tego samego dostawcy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-151">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="a7bea-152">W dalszej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-152">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="a7bea-153">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="a7bea-153">NuGet packages</span></span>

<span data-ttu-id="a7bea-154">`ILogger` i `ILoggerFactory` interfejsy są w [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), a w domyślnej implementacji dla nich [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="a7bea-154">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="a7bea-155">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="a7bea-155">Log category</span></span>

<span data-ttu-id="a7bea-156">A *kategorii* znajduje się w każdym dzienniku utworzony.</span><span class="sxs-lookup"><span data-stu-id="a7bea-156">A *category* is included with each log that you create.</span></span> <span data-ttu-id="a7bea-157">Określ kategorię, po utworzeniu `ILogger` obiektu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-157">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="a7bea-158">Kategoria może być dowolnym ciągiem, ale Konwencji jest użycie w pełni kwalifikowana nazwa klasy, z którego dzienniki są zapisywane.</span><span class="sxs-lookup"><span data-stu-id="a7bea-158">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="a7bea-159">Na przykład: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="a7bea-159">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="a7bea-160">Można określić kategorię jako ciąg lub użyj metody rozszerzenia, która pochodzi z typu kategorii.</span><span class="sxs-lookup"><span data-stu-id="a7bea-160">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="a7bea-161">Aby określić kategorię jako ciąg, należy wywołać `CreateLogger` na `ILoggerFactory` wystąpienie, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a7bea-161">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="a7bea-162">W większości przypadków, będzie łatwiejszy do używania `ILogger<T>`, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="a7bea-162">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="a7bea-163">Jest to odpowiednik wywołania `CreateLogger` z w pełni kwalifikowaną nazwę typu `T`.</span><span class="sxs-lookup"><span data-stu-id="a7bea-163">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="a7bea-164">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="a7bea-164">Log level</span></span>

<span data-ttu-id="a7bea-165">Zawsze możesz zapisać dziennik, należy określić jego [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="a7bea-165">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="a7bea-166">Poziom dziennika wskazuje poziom ważności lub ważność.</span><span class="sxs-lookup"><span data-stu-id="a7bea-166">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="a7bea-167">Na przykład można napisać `Information` Rejestruj, gdy metoda kończy się zwykle `Warning` Zaloguj się przy metoda zwraca kod powrotu 404 oraz `Error` rejestrowania podczas catch wystąpił nieoczekiwany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="a7bea-167">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="a7bea-168">W poniższym przykładzie kodu, nazwy metody (na przykład `LogWarning`) określ poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="a7bea-168">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="a7bea-169">Pierwszym parametrem jest [identyfikator zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="a7bea-169">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="a7bea-170">Drugi parametr jest [szablon wiadomości](#log-message-template) z symbole zastępcze wartości argumentów dostarczonych przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="a7bea-170">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="a7bea-171">Parametry metody są szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-171">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="a7bea-172">Metody dziennika, które obejmują poziom w nazwie metody są [metody rozszerzenia dla ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="a7bea-172">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="a7bea-173">W tle wywołania tych metod `Log` metody pobierającej `LogLevel` parametru.</span><span class="sxs-lookup"><span data-stu-id="a7bea-173">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="a7bea-174">Możesz wywołać `Log` bezpośrednio zamiast jeden z tych metod rozszerzenia, ale składnia jest stosunkowo skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="a7bea-174">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="a7bea-175">Aby uzyskać więcej informacji, zobacz [interfejsu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) i [kod źródłowy rozszerzenia rejestratora](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="a7bea-175">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="a7bea-176">Platformy ASP.NET Core definiuje następujące [dziennika poziomy](/dotnet/api/microsoft.extensions.logging.loglevel), uporządkowanych w tym miejscu z co najmniej do najwyższej ważności.</span><span class="sxs-lookup"><span data-stu-id="a7bea-176">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="a7bea-177">Śledzenia = 0</span><span class="sxs-lookup"><span data-stu-id="a7bea-177">Trace = 0</span></span>

  <span data-ttu-id="a7bea-178">Aby uzyskać informacje, których jest przydatna tylko dla deweloperów, debugowanie problemu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-178">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="a7bea-179">Te komunikaty mogą zawierać dane poufne aplikacji i dlatego nie można włączyć w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="a7bea-179">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="a7bea-180">*Domyślnie wyłączone.*</span><span class="sxs-lookup"><span data-stu-id="a7bea-180">*Disabled by default.*</span></span> <span data-ttu-id="a7bea-181">Przykład: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="a7bea-181">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="a7bea-182">Debugowanie = 1</span><span class="sxs-lookup"><span data-stu-id="a7bea-182">Debug = 1</span></span>

  <span data-ttu-id="a7bea-183">Aby uzyskać informacje, których ma krótkoterminowej przydatność podczas projektowania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-183">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="a7bea-184">Przykład: `Entering method Configure with flag set to true.` zwykle nie włączyć `Debug` poziomu rejestruje w środowisku produkcyjnym, chyba że występuje problem z powodu dużej liczby dzienników.</span><span class="sxs-lookup"><span data-stu-id="a7bea-184">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="a7bea-185">Informacje o = 2</span><span class="sxs-lookup"><span data-stu-id="a7bea-185">Information = 2</span></span>

  <span data-ttu-id="a7bea-186">Pozwala to na śledzenie ogólny przebieg aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-186">For tracking the general flow of the application.</span></span> <span data-ttu-id="a7bea-187">Te dzienniki zwykle mają niektóre wartości długoterminowej.</span><span class="sxs-lookup"><span data-stu-id="a7bea-187">These logs typically have some long-term value.</span></span> <span data-ttu-id="a7bea-188">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="a7bea-188">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="a7bea-189">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="a7bea-189">Warning = 3</span></span>

  <span data-ttu-id="a7bea-190">Dla nieprawidłowego lub nieoczekiwanego zdarzenia w procesie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-190">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="a7bea-191">Mogą one zawierać błędy lub inne warunki, które nie powodują przerwania aplikacji, ale które mogą wymagać należy zbadać.</span><span class="sxs-lookup"><span data-stu-id="a7bea-191">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="a7bea-192">Obsługiwany wyjątki są spójne użyj `Warning` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="a7bea-192">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="a7bea-193">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="a7bea-193">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="a7bea-194">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="a7bea-194">Error = 4</span></span>

  <span data-ttu-id="a7bea-195">Błędy i wyjątki, które nie mogą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a7bea-195">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="a7bea-196">Te komunikaty wskazują błędu bieżącego działania lub działania (takie jak bieżące żądanie HTTP), nie awarii całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-196">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="a7bea-197">Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="a7bea-197">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="a7bea-198">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="a7bea-198">Critical = 5</span></span>

  <span data-ttu-id="a7bea-199">Niepowodzeń, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="a7bea-199">For failures that require immediate attention.</span></span> <span data-ttu-id="a7bea-200">Przykłady: danych scenariuszy utraty, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="a7bea-200">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="a7bea-201">Poziom dziennika służy do kontrolowania, jak dużo danych wyjściowych dziennika są zapisywane na nośniku magazynujące lub Wyświetl okno.</span><span class="sxs-lookup"><span data-stu-id="a7bea-201">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="a7bea-202">Na przykład w środowisku produkcyjnym może być wszystkie dzienniki `Information` poziomu i zmniejszyć, aby przejść do magazynu danych woluminu, a wszystkie dzienniki `Warning` poziomu lub nowszej, aby przejść do magazynu danych wartości.</span><span class="sxs-lookup"><span data-stu-id="a7bea-202">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="a7bea-203">Podczas tworzenia, zwykle może wysyłać dzienniki `Warning` lub wyższej ważności do konsoli.</span><span class="sxs-lookup"><span data-stu-id="a7bea-203">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="a7bea-204">Następnie podczas należy rozwiązać, można dodać `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="a7bea-204">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="a7bea-205">[Filtrowania dziennika](#log-filtering) później w tym artykule wyjaśniono, jak kontrolować poziomy rejestrowania, które obsługuje dostawcę.</span><span class="sxs-lookup"><span data-stu-id="a7bea-205">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="a7bea-206">Zapisuje w ramach platformy ASP.NET Core `Debug` poziomu dzienników zdarzeń framework.</span><span class="sxs-lookup"><span data-stu-id="a7bea-206">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="a7bea-207">Przykłady dziennika wcześniej w tym artykule wykluczone dzienniki poniżej `Information` poziomu, więc nie ma `Debug` poziomu dzienniki były wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="a7bea-207">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="a7bea-208">Oto przykład dzienniki konsoli po uruchomieniu aplikacji przykładowej, jest skonfigurowana do wyświetlania `Debug` i wyższe dzienniki dla dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="a7bea-208">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="a7bea-209">Identyfikator zdarzenia logowania</span><span class="sxs-lookup"><span data-stu-id="a7bea-209">Log event ID</span></span>

<span data-ttu-id="a7bea-210">Zawsze możesz zapisać dziennik, można określić *identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="a7bea-210">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="a7bea-211">Przykładowa aplikacja robi to przy użyciu zdefiniowane lokalnie `LoggingEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="a7bea-211">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="a7bea-212">Identyfikator zdarzenia jest wartość całkowita używanego do kojarzenia zestaw zarejestrowane zdarzenia ze sobą.</span><span class="sxs-lookup"><span data-stu-id="a7bea-212">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="a7bea-213">Na przykład dziennika dodania elementu do koszyk może być zdarzenie o identyfikatorze 1000 oraz dziennika kończenia zakupu można identyfikator zdarzenia 1001.</span><span class="sxs-lookup"><span data-stu-id="a7bea-213">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="a7bea-214">W danych wyjściowych rejestrowania identyfikator zdarzenia może przechowywanych w polu lub zawarte w wiadomości tekstowej, w zależności od dostawcy.</span><span class="sxs-lookup"><span data-stu-id="a7bea-214">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="a7bea-215">Dostawca debugowania nie Wyświetla identyfikatory zdarzeń, ale dostawca konsoli Pokazuje je w nawiasach po kategorii:</span><span class="sxs-lookup"><span data-stu-id="a7bea-215">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="a7bea-216">Szablon wiadomości dziennika</span><span class="sxs-lookup"><span data-stu-id="a7bea-216">Log message template</span></span>

<span data-ttu-id="a7bea-217">Zawsze, gdy zapisu komunikatu dziennika, musisz podać szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a7bea-217">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="a7bea-218">Szablon wiadomości może być ciągiem lub może zawierać nazwanego symbole zastępcze w argumentu, które są umieszczone wartości.</span><span class="sxs-lookup"><span data-stu-id="a7bea-218">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="a7bea-219">Ciąg formatu nie jest szablon i symbole zastępcze powinno być nazwanym, nie.</span><span class="sxs-lookup"><span data-stu-id="a7bea-219">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="a7bea-220">Kolejność symbole zastępcze, a nie ich nazw, określa, które parametry są używane do zapewnienia ich wartości.</span><span class="sxs-lookup"><span data-stu-id="a7bea-220">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="a7bea-221">Jeśli masz następujący kod:</span><span class="sxs-lookup"><span data-stu-id="a7bea-221">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="a7bea-222">Wynikowa komunikatu dziennika wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="a7bea-222">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="a7bea-223">Struktury rejestrowania komunikatów formatowania w ten sposób, aby umożliwić rejestrowanie dostawców do zaimplementowania [semantycznego rejestrowania, nazywany również strukturalnych rejestrowania](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="a7bea-223">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="a7bea-224">Ponieważ same argumenty są przekazywane do rejestrowania systemu, nie tylko szablon sformatowany komunikat rejestrowania dostawców można przechowywać wartości parametrów za pomocą pól oprócz szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a7bea-224">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="a7bea-225">Jeśli jest kierowanie dziennika dane wyjściowe do magazynu tabel platformy Azure i wywołania metody Rejestrator wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="a7bea-225">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="a7bea-226">Każda jednostka tabel Azure może mieć `ID` i `RequestTime` właściwości, które upraszcza zapytania na danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="a7bea-226">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="a7bea-227">Możesz znaleźć wszystkie dzienniki w ramach określonego `RequestTime` zakres bez konieczności przeanalizować limit czasu wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="a7bea-227">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="a7bea-228">Rejestrowaniem wyjątków</span><span class="sxs-lookup"><span data-stu-id="a7bea-228">Logging exceptions</span></span>

<span data-ttu-id="a7bea-229">Metody rejestratora mają przeciążenia, które umożliwiają przekazywanie wyjątku, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a7bea-229">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="a7bea-230">Różnych dostawców obsługi informacji o wyjątkach na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="a7bea-230">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="a7bea-231">Oto przykład danych wyjściowych debugowania dostawcy z kodem przedstawionym powyżej.</span><span class="sxs-lookup"><span data-stu-id="a7bea-231">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="a7bea-232">Filtrowanie dziennika</span><span class="sxs-lookup"><span data-stu-id="a7bea-232">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a7bea-233">Można określić poziom dziennika minimalną dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="a7bea-233">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="a7bea-234">Żadnych dzienników poniżej minimalnego poziomu nie są przekazywane do tego dostawcy, tak, aby nie pobrać wyświetlane ani przechowywane.</span><span class="sxs-lookup"><span data-stu-id="a7bea-234">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="a7bea-235">Jeśli chcesz pominąć wszystkie dzienniki, można określić `LogLevel.None` jako poziom dziennika minimalnej.</span><span class="sxs-lookup"><span data-stu-id="a7bea-235">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="a7bea-236">Wartość całkowita `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="a7bea-236">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="a7bea-237">**Tworzenie reguły filtrów w konfiguracji**</span><span class="sxs-lookup"><span data-stu-id="a7bea-237">**Create filter rules in configuration**</span></span>

<span data-ttu-id="a7bea-238">Szablony projektów utworzyć kod, który wywołuje `CreateDefaultBuilder` Aby skonfigurować rejestrowanie dla dostawców konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-238">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="a7bea-239">`CreateDefaultBuilder` Metody konfiguruje również rejestrowanie do wyszukania konfiguracji w `Logging` sekcji przy użyciu kodu podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="a7bea-239">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="a7bea-240">Dane konfiguracji Określa poziomy dziennika minimalna przez dostawcę i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a7bea-240">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="a7bea-241">Ten kod JSON tworzy sześciu reguły filtrowania, jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jedną, która ma zastosowanie do wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="a7bea-241">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="a7bea-242">Pojawi się później, jak tylko jeden z tych zasad jest wybierany dla każdego dostawcy podczas `ILogger` tworzony jest obiekt.</span><span class="sxs-lookup"><span data-stu-id="a7bea-242">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="a7bea-243">**Reguły filtrowania w kodzie**</span><span class="sxs-lookup"><span data-stu-id="a7bea-243">**Filter rules in code**</span></span>

<span data-ttu-id="a7bea-244">Można zarejestrować reguły filtrów w kodzie, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a7bea-244">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="a7bea-245">Drugi `AddFilter` określa dostawcę, który debugowania za pomocą jego nazwy.</span><span class="sxs-lookup"><span data-stu-id="a7bea-245">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="a7bea-246">Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określono typu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="a7bea-246">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="a7bea-247">**Sposób filtrowania zasady są stosowane.**</span><span class="sxs-lookup"><span data-stu-id="a7bea-247">**How filtering rules are applied**</span></span>

<span data-ttu-id="a7bea-248">Dane konfiguracji i `AddFilter` kodem przedstawionym w powyższych przykładach tworzenia reguł pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="a7bea-248">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="a7bea-249">Pierwszy sześciu pochodzą z przykład konfiguracji i ostatnie dwa pochodzi z przykładów kodu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-249">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="a7bea-250">Wartość liczbowa</span><span class="sxs-lookup"><span data-stu-id="a7bea-250">Number</span></span> | <span data-ttu-id="a7bea-251">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="a7bea-251">Provider</span></span>      | <span data-ttu-id="a7bea-252">Kategorie, które zaczynają się od...</span><span class="sxs-lookup"><span data-stu-id="a7bea-252">Categories that begin with ...</span></span>          | <span data-ttu-id="a7bea-253">Poziom dziennika minimalna</span><span class="sxs-lookup"><span data-stu-id="a7bea-253">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="a7bea-254">1</span><span class="sxs-lookup"><span data-stu-id="a7bea-254">1</span></span>      | <span data-ttu-id="a7bea-255">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="a7bea-255">Debug</span></span>         | <span data-ttu-id="a7bea-256">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="a7bea-256">All categories</span></span>                          | <span data-ttu-id="a7bea-257">Informacje</span><span class="sxs-lookup"><span data-stu-id="a7bea-257">Information</span></span>       |
| <span data-ttu-id="a7bea-258">2</span><span class="sxs-lookup"><span data-stu-id="a7bea-258">2</span></span>      | <span data-ttu-id="a7bea-259">Konsola</span><span class="sxs-lookup"><span data-stu-id="a7bea-259">Console</span></span>       | <span data-ttu-id="a7bea-260">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="a7bea-260">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="a7bea-261">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="a7bea-261">Warning</span></span>           |
| <span data-ttu-id="a7bea-262">3</span><span class="sxs-lookup"><span data-stu-id="a7bea-262">3</span></span>      | <span data-ttu-id="a7bea-263">Konsola</span><span class="sxs-lookup"><span data-stu-id="a7bea-263">Console</span></span>       | <span data-ttu-id="a7bea-264">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="a7bea-264">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="a7bea-265">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="a7bea-265">Debug</span></span>             |
| <span data-ttu-id="a7bea-266">4</span><span class="sxs-lookup"><span data-stu-id="a7bea-266">4</span></span>      | <span data-ttu-id="a7bea-267">Konsola</span><span class="sxs-lookup"><span data-stu-id="a7bea-267">Console</span></span>       | <span data-ttu-id="a7bea-268">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="a7bea-268">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="a7bea-269">Błąd</span><span class="sxs-lookup"><span data-stu-id="a7bea-269">Error</span></span>             |
| <span data-ttu-id="a7bea-270">5</span><span class="sxs-lookup"><span data-stu-id="a7bea-270">5</span></span>      | <span data-ttu-id="a7bea-271">Konsola</span><span class="sxs-lookup"><span data-stu-id="a7bea-271">Console</span></span>       | <span data-ttu-id="a7bea-272">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="a7bea-272">All categories</span></span>                          | <span data-ttu-id="a7bea-273">Informacje</span><span class="sxs-lookup"><span data-stu-id="a7bea-273">Information</span></span>       |
| <span data-ttu-id="a7bea-274">6</span><span class="sxs-lookup"><span data-stu-id="a7bea-274">6</span></span>      | <span data-ttu-id="a7bea-275">Wszystkich dostawców</span><span class="sxs-lookup"><span data-stu-id="a7bea-275">All providers</span></span> | <span data-ttu-id="a7bea-276">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="a7bea-276">All categories</span></span>                          | <span data-ttu-id="a7bea-277">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="a7bea-277">Debug</span></span>             |
| <span data-ttu-id="a7bea-278">7</span><span class="sxs-lookup"><span data-stu-id="a7bea-278">7</span></span>      | <span data-ttu-id="a7bea-279">Wszystkich dostawców</span><span class="sxs-lookup"><span data-stu-id="a7bea-279">All providers</span></span> | <span data-ttu-id="a7bea-280">System</span><span class="sxs-lookup"><span data-stu-id="a7bea-280">System</span></span>                                  | <span data-ttu-id="a7bea-281">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="a7bea-281">Debug</span></span>             |
| <span data-ttu-id="a7bea-282">8</span><span class="sxs-lookup"><span data-stu-id="a7bea-282">8</span></span>      | <span data-ttu-id="a7bea-283">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="a7bea-283">Debug</span></span>         | <span data-ttu-id="a7bea-284">Microsoft</span><span class="sxs-lookup"><span data-stu-id="a7bea-284">Microsoft</span></span>                               | <span data-ttu-id="a7bea-285">Śledzenia</span><span class="sxs-lookup"><span data-stu-id="a7bea-285">Trace</span></span>             |

<span data-ttu-id="a7bea-286">Po utworzeniu `ILogger` obiektu do zapisania dzienniki, `ILoggerFactory` obiektu wybiera jedną regułę na dostawcy, aby zastosować do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="a7bea-286">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="a7bea-287">Wszystkie komunikaty napisane przez to `ILogger` obiektu są filtrowane w zależności od wybranej reguły.</span><span class="sxs-lookup"><span data-stu-id="a7bea-287">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="a7bea-288">Reguła specyficzny dla każdej pary kategorii i dostawcy możliwe jest wybierana z dostępne reguły.</span><span class="sxs-lookup"><span data-stu-id="a7bea-288">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="a7bea-289">Następujący algorytm jest używana dla każdego dostawcy podczas `ILogger` jest tworzony dla danej kategorii:</span><span class="sxs-lookup"><span data-stu-id="a7bea-289">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="a7bea-290">Wybierz wszystkie reguły zgodne dostawca lub jego alias.</span><span class="sxs-lookup"><span data-stu-id="a7bea-290">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="a7bea-291">Jeśli żaden nie zostaną znalezione, wybierz wszystkie reguły z dostawcą puste.</span><span class="sxs-lookup"><span data-stu-id="a7bea-291">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="a7bea-292">W wyniku poprzedniego kroku wybierz reguły z najdłuższy dopasowanie prefiksu kategorii.</span><span class="sxs-lookup"><span data-stu-id="a7bea-292">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="a7bea-293">Jeśli żaden nie zostaną znalezione, wybierz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="a7bea-293">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="a7bea-294">Jeśli wybrano wiele reguł **ostatniego** jeden.</span><span class="sxs-lookup"><span data-stu-id="a7bea-294">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="a7bea-295">Jeśli nie wybrano żadnych reguł, użyj `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="a7bea-295">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="a7bea-296">Na przykład, załóżmy, że powyższej listy reguł i utworzysz `ILogger` obiektu dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="a7bea-296">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="a7bea-297">W przypadku dostawcy usług debugowania mają zastosowanie zasady 1, 6, 8.</span><span class="sxs-lookup"><span data-stu-id="a7bea-297">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="a7bea-298">Reguła 8 jest najbardziej konkretny, więc jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="a7bea-298">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="a7bea-299">Dostawca konsoli zastosować zasady 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="a7bea-299">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="a7bea-300">Reguła 3 jest specyficzny.</span><span class="sxs-lookup"><span data-stu-id="a7bea-300">Rule 3 is most specific.</span></span>

<span data-ttu-id="a7bea-301">Po utworzeniu dzienniki z `ILogger` dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" dzienniki z `Trace` poziomu i nowszych nastąpi przejście do debugowania dostawcy i dzienniki `Debug` poziomu i nowszych nastąpi przejście do dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="a7bea-301">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="a7bea-302">**Aliasy dostawcy**</span><span class="sxs-lookup"><span data-stu-id="a7bea-302">**Provider aliases**</span></span>

<span data-ttu-id="a7bea-303">Nazwa typu można używać, aby określić dostawcę konfiguracji, ale każdy dostawca definiuje krótszą *alias* łatwiej do użycia.</span><span class="sxs-lookup"><span data-stu-id="a7bea-303">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="a7bea-304">W przypadku dostawców wbudowanych, użyj następujących aliasów:</span><span class="sxs-lookup"><span data-stu-id="a7bea-304">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="a7bea-305">Konsola</span><span class="sxs-lookup"><span data-stu-id="a7bea-305">Console</span></span>
- <span data-ttu-id="a7bea-306">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="a7bea-306">Debug</span></span>
- <span data-ttu-id="a7bea-307">Dziennik zdarzeń</span><span class="sxs-lookup"><span data-stu-id="a7bea-307">EventLog</span></span>
- <span data-ttu-id="a7bea-308">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="a7bea-308">AzureAppServices</span></span>
- <span data-ttu-id="a7bea-309">TraceSource</span><span class="sxs-lookup"><span data-stu-id="a7bea-309">TraceSource</span></span>
- <span data-ttu-id="a7bea-310">EventSource</span><span class="sxs-lookup"><span data-stu-id="a7bea-310">EventSource</span></span>

<span data-ttu-id="a7bea-311">**Minimalny poziom domyślny**</span><span class="sxs-lookup"><span data-stu-id="a7bea-311">**Default minimum level**</span></span>

<span data-ttu-id="a7bea-312">Brak minimalnej ustawienie poziomu, które obowiązuje tylko wtedy, gdy żadne reguły z konfiguracji lub kod odnoszą się do danego dostawcy oraz kategorii.</span><span class="sxs-lookup"><span data-stu-id="a7bea-312">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="a7bea-313">Poniższy przykład pokazuje, jak ustawić minimalny poziom:</span><span class="sxs-lookup"><span data-stu-id="a7bea-313">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="a7bea-314">Jeśli nie zostanie jawnie ustawiona na poziomie minimalnym, wartością domyślną jest `Information`, co oznacza, że `Trace` i `Debug` dzienniki są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="a7bea-314">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="a7bea-315">**Funkcje filtrowania**</span><span class="sxs-lookup"><span data-stu-id="a7bea-315">**Filter functions**</span></span>

<span data-ttu-id="a7bea-316">W funkcji Filtr można zastosować reguł filtrowania, można napisać kod.</span><span class="sxs-lookup"><span data-stu-id="a7bea-316">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="a7bea-317">Funkcja filtru jest wywoływany dla wszystkich dostawców i kategorie, które nie mają reguł przypisanego przez konfiguracji lub kodu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-317">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="a7bea-318">Kod w funkcji ma dostęp do typ dostawcy, kategorii i poziom dziennika, aby zdecydować, czy wiadomość powinny być rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="a7bea-318">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="a7bea-319">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a7bea-319">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a7bea-320">Niektórzy dostawcy rejestrowania pozwalają określić, gdy zapisywane na nośniku magazynowania lub ignorowane dzienniki na podstawie poziomu dziennika i kategorii.</span><span class="sxs-lookup"><span data-stu-id="a7bea-320">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="a7bea-321">`AddConsole` i `AddDebug` metody rozszerzenia udostępniają przeciążenia, które umożliwiają przekazywanie w kryteriach filtrowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-321">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="a7bea-322">Następujący przykładowy kod powoduje, że dostawca konsoli do ignorowania dzienniki poniżej `Warning` poziom, gdy dostawca debugowania ignoruje dzienników tworzone w ramach.</span><span class="sxs-lookup"><span data-stu-id="a7bea-322">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="a7bea-323">`AddEventLog` Metoda ma przeciążenia, które przyjmuje `EventLogSettings` wystąpienia, która może zawierać funkcji filtrowania w jego `Filter` właściwości.</span><span class="sxs-lookup"><span data-stu-id="a7bea-323">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="a7bea-324">Dostawcy TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jej poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` go używa.</span><span class="sxs-lookup"><span data-stu-id="a7bea-324">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="a7bea-325">Można ustawić reguły filtrowania dla wszystkich dostawców, które są zarejestrowane w usłudze `ILoggerFactory` wystąpienia przy użyciu `WithFilter` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a7bea-325">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="a7bea-326">W poniższym przykładzie ogranicza dzienniki framework (kategoria rozpoczyna się od "Microsoft" lub "System") z ostrzeżeniami, a w dzienniku aplikacji na poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-326">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="a7bea-327">Jeśli chcesz użyć filtrowania, aby uniemożliwić wszystkie dzienniki zapisywane dla określonej kategorii, można określić `LogLevel.None` jako poziom dziennika minimalną dla tej kategorii.</span><span class="sxs-lookup"><span data-stu-id="a7bea-327">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="a7bea-328">Wartość całkowita `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="a7bea-328">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="a7bea-329">`WithFilter` — Metoda rozszerzenia są dostarczane przez [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="a7bea-329">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="a7bea-330">Metoda zwraca nową `ILoggerFactory` wystąpienia, która będzie filtrować wiadomości dziennika przekazany do wszystkich dostawców rejestratora zarejestrowany z nim.</span><span class="sxs-lookup"><span data-stu-id="a7bea-330">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="a7bea-331">Nie wpływa na inne `ILoggerFactory` wystąpienia, w tym oryginalnej `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a7bea-331">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="a7bea-332">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="a7bea-332">Log scopes</span></span>

<span data-ttu-id="a7bea-333">Można grupować zestaw operacji logicznych w *zakres* aby można było dołączyć tych samych danych do każdego dziennika, który został utworzony jako część tego zbioru.</span><span class="sxs-lookup"><span data-stu-id="a7bea-333">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="a7bea-334">Na przykład może być co dziennika utworzona w ramach przetwarzania transakcji, aby uwzględnić identyfikator transakcji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-334">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="a7bea-335">Zakres jest `IDisposable` typu, który jest zwracany przez [ILogger.BeginScope&lt;stanu dławienia&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) — metoda i trwa do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="a7bea-335">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="a7bea-336">Użyj zakresu przez zawijania odwołuje Twoje rejestratora `using` zablokować, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="a7bea-336">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="a7bea-337">Poniższy kod umożliwia zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="a7bea-337">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="a7bea-338">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7bea-338">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="a7bea-339">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-339">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="a7bea-340">`IncludeScopes` można skonfigurować za pomocą *appsettings* plików konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="a7bea-340">`IncludeScopes` can be configured via *appsettings* configuration files.</span></span> <span data-ttu-id="a7bea-341">Aby uzyskać więcej informacji, zobacz [ustawienia konfiguracji pliku](#settings-file-configuration) sekcji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-341">For more information, see the [Settings file configuration](#settings-file-configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7bea-342">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7bea-342">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="a7bea-343">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-343">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a7bea-344">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7bea-344">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="a7bea-345">Każdy komunikat dziennika zawiera informacje o zakresie:</span><span class="sxs-lookup"><span data-stu-id="a7bea-345">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="a7bea-346">Rejestrowanie wbudowanych dostawców</span><span class="sxs-lookup"><span data-stu-id="a7bea-346">Built-in logging providers</span></span>

<span data-ttu-id="a7bea-347">Platformy ASP.NET Core dostarczany następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="a7bea-347">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="a7bea-348">Console</span><span class="sxs-lookup"><span data-stu-id="a7bea-348">Console</span></span>](#console-provider)
* [<span data-ttu-id="a7bea-349">Debugowania</span><span class="sxs-lookup"><span data-stu-id="a7bea-349">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="a7bea-350">EventSource</span><span class="sxs-lookup"><span data-stu-id="a7bea-350">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="a7bea-351">EventLog</span><span class="sxs-lookup"><span data-stu-id="a7bea-351">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="a7bea-352">TraceSource</span><span class="sxs-lookup"><span data-stu-id="a7bea-352">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="a7bea-353">Usługa aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="a7bea-353">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="a7bea-354">Dostawca konsoli</span><span class="sxs-lookup"><span data-stu-id="a7bea-354">Console provider</span></span>

<span data-ttu-id="a7bea-355">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakiet dostawcy wysyła dane wyjściowe dziennika do konsoli.</span><span class="sxs-lookup"><span data-stu-id="a7bea-355">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="a7bea-356">[Przeciążenia AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let przekazywane w poziom dziennika minimalne, funkcji filtru i wartość logiczną wskazującą, czy zakresy są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a7bea-356">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="a7bea-357">Innym rozwiązaniem jest przekazanie w `IConfiguration` obiektu, który można określić zakresy pomocy technicznej i poziomy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a7bea-357">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="a7bea-358">Planując Konsola dostawca do użycia w środowisku produkcyjnym, należy pamiętać, ma znaczący wpływ na wydajność.</span><span class="sxs-lookup"><span data-stu-id="a7bea-358">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="a7bea-359">Podczas tworzenia nowego projektu programu Visual Studio `AddConsole` metody wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="a7bea-359">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="a7bea-360">Ten kod odwołuje się do `Logging` sekcji *appSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="a7bea-360">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="a7bea-361">Ustawienia wyświetlane limit framework dzienniki, aby ostrzeżenia, umożliwiając aplikacji do logowania się na poziomie debugowania, zgodnie z objaśnieniem w [filtrowania dziennika](#log-filtering) sekcji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-361">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="a7bea-362">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a7bea-362">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="a7bea-363">Debugowanie dostawcy</span><span class="sxs-lookup"><span data-stu-id="a7bea-363">Debug provider</span></span>

<span data-ttu-id="a7bea-364">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakiet dostawcy zapisuje dane wyjściowe dziennika przy użyciu [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) klasy (`Debug.WriteLine` wywołania metody).</span><span class="sxs-lookup"><span data-stu-id="a7bea-364">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="a7bea-365">W systemie Linux, tego dostawcy zapisuje dzienniki, aby */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="a7bea-365">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="a7bea-366">[Przeciążenia AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) umożliwiają przekazywanie poziom dziennika minimalną lub funkcji filtru.</span><span class="sxs-lookup"><span data-stu-id="a7bea-366">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="a7bea-367">Dostawca źródła zdarzeń</span><span class="sxs-lookup"><span data-stu-id="a7bea-367">EventSource provider</span></span>

<span data-ttu-id="a7bea-368">Dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszej, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zaimplementować pakiet dostawcy śledzenia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="a7bea-368">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="a7bea-369">W systemie Windows, używa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="a7bea-369">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="a7bea-370">Dostawca jest między platformami, ale żadne zdarzenie kolekcji i wyświetlanie narzędzi jeszcze dla systemu Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="a7bea-370">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

<span data-ttu-id="a7bea-371">Dobrym sposobem na zbieranie i przeglądanie dzienników jest użycie [narzędzie narzędzia PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="a7bea-371">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="a7bea-372">Istnieją inne narzędzia umożliwiające wyświetlanie dzienników zdarzeń systemu Windows, ale narzędzia PerfView zapewnia najlepsze środowisko do pracy z zdarzenia ETW emitowane przez platformę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a7bea-372">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="a7bea-373">Aby skonfigurować narzędzia PerfView zbierania zdarzenia zarejestrowane przez tego dostawcę, Dodaj ciąg `*Microsoft-Extensions-Logging` do **dodatkowych dostawców** listy.</span><span class="sxs-lookup"><span data-stu-id="a7bea-373">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="a7bea-374">(Nie zostały pominięte gwiazdki na początku ciąg.)</span><span class="sxs-lookup"><span data-stu-id="a7bea-374">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia Perfview dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="a7bea-376">Dostawca dziennika zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="a7bea-376">Windows EventLog provider</span></span>

<span data-ttu-id="a7bea-377">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pakiet dostawcy wysyła dane wyjściowe dziennika w dzienniku zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="a7bea-377">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="a7bea-378">[Przeciążenia AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let przekazywane w `EventLogSettings` lub poziom dziennika minimalnej.</span><span class="sxs-lookup"><span data-stu-id="a7bea-378">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="a7bea-379">TraceSource dostawcy</span><span class="sxs-lookup"><span data-stu-id="a7bea-379">TraceSource provider</span></span>

<span data-ttu-id="a7bea-380">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa dostawcy pakietu [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) bibliotek i dostawców.</span><span class="sxs-lookup"><span data-stu-id="a7bea-380">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="a7bea-381">[Przeciążenia AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) umożliwiają przekazywanie przełącznik źródła i nasłuchującego śledzenia.</span><span class="sxs-lookup"><span data-stu-id="a7bea-381">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="a7bea-382">Aby użyć tego dostawcy, aplikacja musi działać .NET Framework (zamiast .NET Core).</span><span class="sxs-lookup"><span data-stu-id="a7bea-382">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="a7bea-383">Umożliwia dostawcy rozesłać komunikaty do różnych [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takich jak [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) używane w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-383">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="a7bea-384">Poniższy przykład konfiguruje `TraceSource` dostawcy, który rejestruje `Warning` i wyższe wiadomości dla okna konsoli.</span><span class="sxs-lookup"><span data-stu-id="a7bea-384">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="a7bea-385">Dostawca usługi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a7bea-385">Azure App Service provider</span></span>

<span data-ttu-id="a7bea-386">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pakiet dostawcy zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji w usłudze Azure App Service i do [magazynu obiektów blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie magazynu Azure.</span><span class="sxs-lookup"><span data-stu-id="a7bea-386">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="a7bea-387">Dostawca jest dostępna tylko dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="a7bea-387">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a7bea-388">Jeśli przeznaczonych dla platformy .NET Core, nie należy zainstalować pakiet dostawcy lub jawnie wywołać [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="a7bea-388">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="a7bea-389">Dostawca jest automatycznie dostępne dla aplikacji, gdy aplikacja jest wdrożona w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a7bea-389">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="a7bea-390">Jeśli przeznaczonych dla platformy .NET Framework, Dodaj pakiet dostawcy do projektu i wywołać `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="a7bea-390">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="a7bea-391">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) przeciążenia umożliwia przekazywanie w [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) z którym mogą zastąpić ustawienia domyślne, takie jak rejestrowanie danych wyjściowych szablonu, nazwa obiektu blob i plików limit rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="a7bea-391">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="a7bea-392">(*Danych wyjściowych szablonu* jest szablon wiadomości, która jest stosowana do wszystkich dzienników na elemencie podane podczas wywoływania `ILogger` metody.)</span><span class="sxs-lookup"><span data-stu-id="a7bea-392">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

::: moniker-end

<span data-ttu-id="a7bea-393">Podczas wdrażania aplikacji usługi app Service, aplikacja będzie honorować ustawień w [dzienników diagnostycznych](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) sekcji **usługi aplikacji** strony portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="a7bea-393">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="a7bea-394">Jeśli te ustawienia zostaną zaktualizowane, zmiany zaczynają obowiązywać natychmiast bez konieczności ponownego uruchamiania lub ponownego wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-394">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Ustawienia rejestrowania usługi Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="a7bea-396">Domyślna lokalizacja dla plików dziennika jest *D:\\macierzystego\\LogFiles\\aplikacji* folderu, a domyślna nazwa pliku jest *yyyymmdd.txt diagnostyki*.</span><span class="sxs-lookup"><span data-stu-id="a7bea-396">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="a7bea-397">Domyślnego limitu rozmiaru pliku to 10 MB, a domyślna maksymalna liczba pliki przechowywane wynosi 2.</span><span class="sxs-lookup"><span data-stu-id="a7bea-397">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="a7bea-398">Domyślna nazwa obiektu blob jest *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="a7bea-398">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="a7bea-399">Aby uzyskać więcej informacji na temat domyślnego zachowania, zobacz [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="a7bea-399">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="a7bea-400">Dostawca działa tylko w przypadku, gdy projekt jest uruchamiana w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="a7bea-400">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="a7bea-401">Nie ma wpływu, gdy projekt jest uruchamiana lokalnie&mdash;nie zapisu plików lokalnych lub rozwoju lokalnego magazynu obiektów blob.</span><span class="sxs-lookup"><span data-stu-id="a7bea-401">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="a7bea-402">Rejestrowanie innych dostawców</span><span class="sxs-lookup"><span data-stu-id="a7bea-402">Third-party logging providers</span></span>

<span data-ttu-id="a7bea-403">Struktury rejestrowania innych firm, które współpracują z platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a7bea-403">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="a7bea-404">[elmah.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="a7bea-404">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="a7bea-405">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="a7bea-405">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="a7bea-406">[JSNLog](http://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="a7bea-406">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="a7bea-407">[Loggr](http://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="a7bea-407">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="a7bea-408">[NLog](http://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="a7bea-408">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="a7bea-409">[Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="a7bea-409">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="a7bea-410">Niektórych struktur innych firm mogą wykonywać [semantycznego rejestrowania, nazywany również strukturalnych rejestrowania](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="a7bea-410">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="a7bea-411">Przy użyciu platformy innej firmy jest podobny do przy użyciu jednego z dostawców wbudowanych:</span><span class="sxs-lookup"><span data-stu-id="a7bea-411">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="a7bea-412">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-412">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="a7bea-413">Wywoływanie metody rozszerzenia na `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="a7bea-413">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="a7bea-414">Aby uzyskać więcej informacji zobacz dokumentację każdego framework.</span><span class="sxs-lookup"><span data-stu-id="a7bea-414">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="a7bea-415">Strumieniowe przesyłanie dzienników Azure</span><span class="sxs-lookup"><span data-stu-id="a7bea-415">Azure log streaming</span></span>

<span data-ttu-id="a7bea-416">Strumieniowe przesyłanie dzienników Azure umożliwia wyświetlenie dziennika aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="a7bea-416">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="a7bea-417">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="a7bea-417">The application server</span></span>
* <span data-ttu-id="a7bea-418">Serwer sieci web</span><span class="sxs-lookup"><span data-stu-id="a7bea-418">The web server</span></span>
* <span data-ttu-id="a7bea-419">Śledzenie nieudanych żądań</span><span class="sxs-lookup"><span data-stu-id="a7bea-419">Failed request tracing</span></span>

<span data-ttu-id="a7bea-420">Aby skonfigurować przesyłania strumieniowego dzienników Azure:</span><span class="sxs-lookup"><span data-stu-id="a7bea-420">To configure Azure log streaming:</span></span>

* <span data-ttu-id="a7bea-421">Przejdź do **dzienników diagnostycznych** strony ze strony portalu aplikacji</span><span class="sxs-lookup"><span data-stu-id="a7bea-421">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="a7bea-422">Ustaw **rejestrowanie aplikacji (systemu plików)** do włączenia.</span><span class="sxs-lookup"><span data-stu-id="a7bea-422">Set **Application Logging (Filesystem)** to on.</span></span>

![Strona Azure portalu dzienników diagnostycznych](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="a7bea-424">Przejdź do **dzienników przesyłania strumieniowego** strony w celu wyświetlenia komunikatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7bea-424">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="a7bea-425">Są one zarejestrowane przez aplikację za pomocą `ILogger` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7bea-425">They're logged by application through the `ILogger` interface.</span></span>

![Przesyłanie strumieniowe dziennika aplikacji z portalu Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="a7bea-427">Azure rejestrowanie śledzenia usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="a7bea-427">Azure Application Insights trace logging</span></span>

<span data-ttu-id="a7bea-428">[Usługi Application Insights](https://azure.microsoft.com/services/application-insights/) zestawu SDK jest w stanie zbierając dane telemetryczne śledzenia z dzienniki generowane przez infrastrukturę rejestrowanie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7bea-428">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="a7bea-429">Aby uzyskać więcej informacji, zobacz [Microsoft/ApplicationInsights-aspnetcore typu Wiki: rejestrowanie](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="a7bea-429">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7bea-430">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a7bea-430">Additional resources</span></span>

[<span data-ttu-id="a7bea-431">Rejestrowanie wysokiej wydajności z LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="a7bea-431">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
