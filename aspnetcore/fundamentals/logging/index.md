---
title: Logowanie do platformy ASP.NET Core
author: ardalis
description: Więcej informacji na temat struktury rejestrowania w ASP.NET Core. Odnajdywanie dostawców wbudowanych rejestrowania i Dowiedz się więcej o innych popularnych dostawców.
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: 5e7e0fe0744a8dc3f3dd6097a059d77f2c578f77
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652204"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="bb7a0-104">Logowanie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bb7a0-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="bb7a0-105">Przez [Steve Smith](https://ardalis.com/) i [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="bb7a0-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="bb7a0-106">Platformy ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnych dostawców rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="bb7a0-107">Dostawców wbudowanych umożliwiają wysyłanie dzienników do jednego lub więcej miejsc docelowych, a można podłączyć w ramach rejestrowania innych firm.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="bb7a0-108">W tym artykule pokazano, jak korzystać z interfejsu API wbudowanych rejestrowania i dostawców w kodzie.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bb7a0-110">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bb7a0-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bb7a0-112">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bb7a0-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="bb7a0-113">Jak utworzyć dzienników</span><span class="sxs-lookup"><span data-stu-id="bb7a0-113">How to create logs</span></span>

<span data-ttu-id="bb7a0-114">Aby utworzyć dzienników, zaimplementuj [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) obiekt z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="bb7a0-115">Następnie wywołania metody rejestrowania dla tego obiektu rejestratora:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="bb7a0-116">W tym przykładzie tworzy dzienniki z `TodoController` klasy *kategorii*.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="bb7a0-117">Kategorie są objaśnione [dalszej części tego artykułu](#log-category).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="bb7a0-118">Platformy ASP.NET Core nie async rejestratora udostępniają metody służące ponieważ rejestrowania należy rozważnie nie warto kosztów asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="bb7a0-119">Jeśli pracujesz w sytuacji, w przypadku, gdy nie jest prawdziwe, należy rozważyć zmianę sposobu logowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="bb7a0-120">Jeśli w magazynie danych przebiega powoli, najpierw zapisać komunikaty dziennika do szybkiego magazynu, a następnie przenieść je do magazynu powolne później.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="bb7a0-121">Na przykład Zaloguj się do kolejki komunikatów, która ma odczytu i utrwalone w magazynie powolne przez inny proces.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="bb7a0-122">Jak dodać dostawców</span><span class="sxs-lookup"><span data-stu-id="bb7a0-122">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="bb7a0-124">Dostawcy logowania ma utworzonych za pomocą wiadomości `ILogger` obiektów, wyświetla i przechowuje je.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="bb7a0-125">Na przykład konsola dostawca wyświetla komunikaty w konsoli i dostawcy usługi Azure App Service można przechowywać w magazynie obiektów blob Azure.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="bb7a0-126">Aby użyć dostawcy, należy wywołać dostawcy `Add<ProviderName>` — metoda rozszerzenia w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="bb7a0-127">Domyślny szablon projektu umożliwia rejestrowanie z [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) metody:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-127">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="bb7a0-129">Dostawcy logowania ma utworzonych za pomocą wiadomości `ILogger` obiektów, wyświetla i przechowuje je.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="bb7a0-130">Na przykład konsola dostawca wyświetla komunikaty w konsoli i dostawcy usługi Azure App Service można przechowywać w magazynie obiektów blob Azure.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="bb7a0-131">Do korzystania z dostawcy, instalowanie pakietu NuGet i wywołanie metody rozszerzenia dostawcy na wystąpienie [element ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="bb7a0-132">Platformy ASP.NET Core [iniekcji zależności](xref:fundamentals/dependency-injection) (Podpisane) zapewnia `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="bb7a0-133">`AddConsole` i `AddDebug` metody rozszerzenia są definiowane w [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) i [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pakietów.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="bb7a0-134">Każda metoda rozszerzenia wywołuje `ILoggerFactory.AddProvider` metody, przekazując wystąpienie dostawcy.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="bb7a0-135">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) dodaje rejestrowanie dostawców w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-135">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="bb7a0-136">Uzyskanie danych wyjściowych dziennika z kodu, która wykonuje wcześniej, dodać rejestrowanie dostawców w `Startup` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-136">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

---

<span data-ttu-id="bb7a0-137">Znajdują się informacje o poszczególnych [rejestrowania wbudowanego dostawcy](#built-in-logging-providers) i łącza do [dostawców innych firm rejestrowania](#third-party-logging-providers) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="bb7a0-138">Przykładowe dane wyjściowe rejestrowania</span><span class="sxs-lookup"><span data-stu-id="bb7a0-138">Sample logging output</span></span>

<span data-ttu-id="bb7a0-139">Z przykładowy kod wyświetlany w poprzedniej sekcji zobaczysz dzienniki w konsoli po uruchomieniu z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="bb7a0-140">Oto przykładowe dane wyjściowe konsoli:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-140">Here's an example of console output:</span></span>

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

<span data-ttu-id="bb7a0-141">Te dzienniki zostały utworzone, przechodząc do `http://localhost:5000/api/todo/0`, który wyzwala wykonanie obu `ILogger` wywołania wyświetlone w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="bb7a0-142">Oto przykład tego samego dzienniki znajdujące się w oknie Debugowanie po uruchomieniu przykładowej aplikacji w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="bb7a0-143">Dzienniki, które zostały utworzone przez `ILogger` wywołania wyświetlone w poprzedniej sekcji rozpoczynać się od "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="bb7a0-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="bb7a0-144">Dzienniki, które zaczynają się od "Microsoft" kategorie są z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="bb7a0-145">Kod aplikacji i ASP.NET Core, sama korzystają z tego samego interfejsu API rejestrowania i tego samego dostawcy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="bb7a0-146">W dalszej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="bb7a0-147">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="bb7a0-147">NuGet packages</span></span>

<span data-ttu-id="bb7a0-148">`ILogger` i `ILoggerFactory` interfejsy są w [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), a w domyślnej implementacji dla nich [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="bb7a0-149">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="bb7a0-149">Log category</span></span>

<span data-ttu-id="bb7a0-150">A *kategorii* znajduje się w każdym dzienniku utworzony.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="bb7a0-151">Określ kategorię, po utworzeniu `ILogger` obiektu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="bb7a0-152">Kategoria może być dowolnym ciągiem, ale Konwencji jest użycie w pełni kwalifikowana nazwa klasy, z którego dzienniki są zapisywane.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="bb7a0-153">Na przykład: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="bb7a0-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="bb7a0-154">Można określić kategorię jako ciąg lub użyj metody rozszerzenia, która pochodzi z typu kategorii.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="bb7a0-155">Aby określić kategorię jako ciąg, należy wywołać `CreateLogger` na `ILoggerFactory` wystąpienie, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="bb7a0-156">W większości przypadków, będzie łatwiejszy do używania `ILogger<T>`, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="bb7a0-157">Jest to odpowiednik wywołania `CreateLogger` z w pełni kwalifikowaną nazwę typu `T`.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="bb7a0-158">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="bb7a0-158">Log level</span></span>

<span data-ttu-id="bb7a0-159">Zawsze możesz zapisać dziennik, należy określić jego [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-159">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="bb7a0-160">Poziom dziennika wskazuje poziom ważności lub ważność.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="bb7a0-161">Na przykład można napisać `Information` Rejestruj, gdy metoda kończy się zwykle `Warning` Zaloguj się przy metoda zwraca kod powrotu 404 oraz `Error` rejestrowania podczas catch wystąpił nieoczekiwany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="bb7a0-162">W poniższym przykładzie kodu, nazwy metody (na przykład `LogWarning`) określ poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="bb7a0-163">Pierwszym parametrem jest [identyfikator zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="bb7a0-164">Drugi parametr jest [szablon wiadomości](#log-message-template) z symbole zastępcze wartości argumentów dostarczonych przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="bb7a0-165">Parametry metody są szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="bb7a0-166">Metody dziennika, które obejmują poziom w nazwie metody są [metody rozszerzenia dla ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-166">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="bb7a0-167">W tle wywołania tych metod `Log` metody pobierającej `LogLevel` parametru.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="bb7a0-168">Możesz wywołać `Log` bezpośrednio zamiast jeden z tych metod rozszerzenia, ale składnia jest stosunkowo skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="bb7a0-169">Aby uzyskać więcej informacji, zobacz [interfejsu ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) i [kod źródłowy rozszerzenia rejestratora](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-169">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="bb7a0-170">Platformy ASP.NET Core definiuje następujące [dziennika poziomy](/dotnet/api/microsoft.extensions.logging.loglevel), uporządkowanych w tym miejscu z co najmniej do najwyższej ważności.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-170">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="bb7a0-171">Śledzenia = 0</span><span class="sxs-lookup"><span data-stu-id="bb7a0-171">Trace = 0</span></span>

  <span data-ttu-id="bb7a0-172">Aby uzyskać informacje, których jest przydatna tylko dla deweloperów, debugowanie problemu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-172">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="bb7a0-173">Te komunikaty mogą zawierać dane poufne aplikacji i dlatego nie można włączyć w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-173">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="bb7a0-174">*Domyślnie wyłączone.*</span><span class="sxs-lookup"><span data-stu-id="bb7a0-174">*Disabled by default.*</span></span> <span data-ttu-id="bb7a0-175">Przykład: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="bb7a0-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="bb7a0-176">Debugowanie = 1</span><span class="sxs-lookup"><span data-stu-id="bb7a0-176">Debug = 1</span></span>

  <span data-ttu-id="bb7a0-177">Aby uzyskać informacje, których ma krótkoterminowej przydatność podczas projektowania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="bb7a0-178">Przykład: `Entering method Configure with flag set to true.` zwykle nie włączyć `Debug` poziomu rejestruje w środowisku produkcyjnym, chyba że występuje problem z powodu dużej liczby dzienników.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-178">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="bb7a0-179">Informacje o = 2</span><span class="sxs-lookup"><span data-stu-id="bb7a0-179">Information = 2</span></span>

  <span data-ttu-id="bb7a0-180">Pozwala to na śledzenie ogólny przebieg aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="bb7a0-181">Te dzienniki zwykle mają niektóre wartości długoterminowej.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="bb7a0-182">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="bb7a0-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="bb7a0-183">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="bb7a0-183">Warning = 3</span></span>

  <span data-ttu-id="bb7a0-184">Dla nieprawidłowego lub nieoczekiwanego zdarzenia w procesie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="bb7a0-185">Mogą one zawierać błędy lub inne warunki, które nie powodują przerwania aplikacji, ale które mogą wymagać należy zbadać.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-185">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="bb7a0-186">Obsługiwany wyjątki są spójne użyj `Warning` poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="bb7a0-187">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="bb7a0-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="bb7a0-188">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="bb7a0-188">Error = 4</span></span>

  <span data-ttu-id="bb7a0-189">Błędy i wyjątki, które nie mogą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="bb7a0-190">Te komunikaty wskazują błędu bieżącego działania lub działania (takie jak bieżące żądanie HTTP), nie awarii całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="bb7a0-191">Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="bb7a0-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="bb7a0-192">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="bb7a0-192">Critical = 5</span></span>

  <span data-ttu-id="bb7a0-193">Niepowodzeń, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-193">For failures that require immediate attention.</span></span> <span data-ttu-id="bb7a0-194">Przykłady: danych scenariuszy utraty, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="bb7a0-195">Poziom dziennika służy do kontrolowania, jak dużo danych wyjściowych dziennika są zapisywane na nośniku magazynujące lub Wyświetl okno.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="bb7a0-196">Na przykład w środowisku produkcyjnym może być wszystkie dzienniki `Information` poziomu i zmniejszyć, aby przejść do magazynu danych woluminu, a wszystkie dzienniki `Warning` poziomu lub nowszej, aby przejść do magazynu danych wartości.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="bb7a0-197">Podczas tworzenia, zwykle może wysyłać dzienniki `Warning` lub wyższej ważności do konsoli.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="bb7a0-198">Następnie podczas należy rozwiązać, można dodać `Debug` poziom.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="bb7a0-199">[Filtrowania dziennika](#log-filtering) później w tym artykule wyjaśniono, jak kontrolować poziomy rejestrowania, które obsługuje dostawcę.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="bb7a0-200">Zapisuje w ramach platformy ASP.NET Core `Debug` poziomu dzienników zdarzeń framework.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="bb7a0-201">Przykłady dziennika wcześniej w tym artykule wykluczone dzienniki poniżej `Information` poziomu, więc nie ma `Debug` poziomu dzienniki były wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="bb7a0-202">Oto przykład dzienniki konsoli po uruchomieniu aplikacji przykładowej, jest skonfigurowana do wyświetlania `Debug` i wyższe dzienniki dla dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="bb7a0-203">Identyfikator zdarzenia logowania</span><span class="sxs-lookup"><span data-stu-id="bb7a0-203">Log event ID</span></span>

<span data-ttu-id="bb7a0-204">Zawsze możesz zapisać dziennik, można określić *identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="bb7a0-205">Przykładowa aplikacja robi to przy użyciu zdefiniowane lokalnie `LoggingEvents` klasy:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="bb7a0-206">Identyfikator zdarzenia jest wartość całkowita używanego do kojarzenia zestaw zarejestrowane zdarzenia ze sobą.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="bb7a0-207">Na przykład dziennika dodania elementu do koszyk może być zdarzenie o identyfikatorze 1000 oraz dziennika kończenia zakupu można identyfikator zdarzenia 1001.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="bb7a0-208">W danych wyjściowych rejestrowania identyfikator zdarzenia może przechowywanych w polu lub zawarte w wiadomości tekstowej, w zależności od dostawcy.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="bb7a0-209">Dostawca debugowania nie Wyświetla identyfikatory zdarzeń, ale dostawca konsoli Pokazuje je w nawiasach po kategorii:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="bb7a0-210">Szablon wiadomości dziennika</span><span class="sxs-lookup"><span data-stu-id="bb7a0-210">Log message template</span></span>

<span data-ttu-id="bb7a0-211">Zawsze, gdy zapisu komunikatu dziennika, musisz podać szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="bb7a0-212">Szablon wiadomości może być ciągiem lub może zawierać nazwanego symbole zastępcze w argumentu, które są umieszczone wartości.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="bb7a0-213">Ciąg formatu nie jest szablon i symbole zastępcze powinno być nazwanym, nie.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="bb7a0-214">Kolejność symbole zastępcze, a nie ich nazw, określa, które parametry są używane do zapewnienia ich wartości.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="bb7a0-215">Jeśli masz następujący kod:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="bb7a0-216">Wynikowa komunikatu dziennika wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="bb7a0-217">Struktury rejestrowania komunikatów formatowania w ten sposób, aby umożliwić rejestrowanie dostawców do zaimplementowania [semantycznego rejestrowania, nazywany również strukturalnych rejestrowania](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="bb7a0-218">Ponieważ same argumenty są przekazywane do rejestrowania systemu, nie tylko szablon sformatowany komunikat rejestrowania dostawców można przechowywać wartości parametrów za pomocą pól oprócz szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="bb7a0-219">Jeśli jest kierowanie dziennika dane wyjściowe do magazynu tabel platformy Azure i wywołania metody Rejestrator wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="bb7a0-220">Każda jednostka tabel Azure może mieć `ID` i `RequestTime` właściwości, które upraszcza zapytania na danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="bb7a0-221">Możesz znaleźć wszystkie dzienniki w ramach określonego `RequestTime` zakres bez konieczności przeanalizować limit czasu wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="bb7a0-222">Rejestrowaniem wyjątków</span><span class="sxs-lookup"><span data-stu-id="bb7a0-222">Logging exceptions</span></span>

<span data-ttu-id="bb7a0-223">Metody rejestratora mają przeciążenia, które umożliwiają przekazywanie wyjątku, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="bb7a0-224">Różnych dostawców obsługi informacji o wyjątkach na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="bb7a0-225">Oto przykład danych wyjściowych debugowania dostawcy z kodem przedstawionym powyżej.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="bb7a0-226">Filtrowanie dziennika</span><span class="sxs-lookup"><span data-stu-id="bb7a0-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="bb7a0-228">Można określić poziom dziennika minimalną dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="bb7a0-229">Żadnych dzienników poniżej minimalnego poziomu nie są przekazywane do tego dostawcy, tak, aby nie pobrać wyświetlane ani przechowywane.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="bb7a0-230">Jeśli chcesz pominąć wszystkie dzienniki, można określić `LogLevel.None` jako poziom dziennika minimalnej.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="bb7a0-231">Wartość całkowita `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="bb7a0-232">**Tworzenie reguły filtrów w konfiguracji**</span><span class="sxs-lookup"><span data-stu-id="bb7a0-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="bb7a0-233">Szablony projektów utworzyć kod, który wywołuje `CreateDefaultBuilder` Aby skonfigurować rejestrowanie dla dostawców konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="bb7a0-234">`CreateDefaultBuilder` Metody konfiguruje również rejestrowanie do wyszukania konfiguracji w `Logging` sekcji przy użyciu kodu podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="bb7a0-235">Dane konfiguracji Określa poziomy dziennika minimalna przez dostawcę i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="bb7a0-236">Ten kod JSON tworzy sześciu reguły filtrowania, jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jedną, która ma zastosowanie do wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="bb7a0-237">Pojawi się później, jak tylko jeden z tych zasad jest wybierany dla każdego dostawcy podczas `ILogger` tworzony jest obiekt.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="bb7a0-238">**Reguły filtrowania w kodzie**</span><span class="sxs-lookup"><span data-stu-id="bb7a0-238">**Filter rules in code**</span></span>

<span data-ttu-id="bb7a0-239">Można zarejestrować reguły filtrów w kodzie, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="bb7a0-240">Drugi `AddFilter` określa dostawcę, który debugowania za pomocą jego nazwy.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="bb7a0-241">Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określono typu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="bb7a0-242">**Sposób filtrowania zasady są stosowane.**</span><span class="sxs-lookup"><span data-stu-id="bb7a0-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="bb7a0-243">Dane konfiguracji i `AddFilter` kodem przedstawionym w powyższych przykładach tworzenia reguł pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="bb7a0-244">Pierwszy sześciu pochodzą z przykład konfiguracji i ostatnie dwa pochodzi z przykładów kodu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="bb7a0-245">Wartość liczbowa</span><span class="sxs-lookup"><span data-stu-id="bb7a0-245">Number</span></span> | <span data-ttu-id="bb7a0-246">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="bb7a0-246">Provider</span></span>      | <span data-ttu-id="bb7a0-247">Kategorie, które zaczynają się od...</span><span class="sxs-lookup"><span data-stu-id="bb7a0-247">Categories that begin with ...</span></span>          | <span data-ttu-id="bb7a0-248">Poziom dziennika minimalna</span><span class="sxs-lookup"><span data-stu-id="bb7a0-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="bb7a0-249">1</span><span class="sxs-lookup"><span data-stu-id="bb7a0-249">1</span></span>      | <span data-ttu-id="bb7a0-250">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-250">Debug</span></span>         | <span data-ttu-id="bb7a0-251">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-251">All categories</span></span>                          | <span data-ttu-id="bb7a0-252">Informacje</span><span class="sxs-lookup"><span data-stu-id="bb7a0-252">Information</span></span>       |
| <span data-ttu-id="bb7a0-253">2</span><span class="sxs-lookup"><span data-stu-id="bb7a0-253">2</span></span>      | <span data-ttu-id="bb7a0-254">Konsola</span><span class="sxs-lookup"><span data-stu-id="bb7a0-254">Console</span></span>       | <span data-ttu-id="bb7a0-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="bb7a0-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="bb7a0-256">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-256">Warning</span></span>           |
| <span data-ttu-id="bb7a0-257">3</span><span class="sxs-lookup"><span data-stu-id="bb7a0-257">3</span></span>      | <span data-ttu-id="bb7a0-258">Konsola</span><span class="sxs-lookup"><span data-stu-id="bb7a0-258">Console</span></span>       | <span data-ttu-id="bb7a0-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="bb7a0-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="bb7a0-260">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-260">Debug</span></span>             |
| <span data-ttu-id="bb7a0-261">4</span><span class="sxs-lookup"><span data-stu-id="bb7a0-261">4</span></span>      | <span data-ttu-id="bb7a0-262">Konsola</span><span class="sxs-lookup"><span data-stu-id="bb7a0-262">Console</span></span>       | <span data-ttu-id="bb7a0-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="bb7a0-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="bb7a0-264">Błąd</span><span class="sxs-lookup"><span data-stu-id="bb7a0-264">Error</span></span>             |
| <span data-ttu-id="bb7a0-265">5</span><span class="sxs-lookup"><span data-stu-id="bb7a0-265">5</span></span>      | <span data-ttu-id="bb7a0-266">Konsola</span><span class="sxs-lookup"><span data-stu-id="bb7a0-266">Console</span></span>       | <span data-ttu-id="bb7a0-267">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-267">All categories</span></span>                          | <span data-ttu-id="bb7a0-268">Informacje</span><span class="sxs-lookup"><span data-stu-id="bb7a0-268">Information</span></span>       |
| <span data-ttu-id="bb7a0-269">6</span><span class="sxs-lookup"><span data-stu-id="bb7a0-269">6</span></span>      | <span data-ttu-id="bb7a0-270">Wszystkich dostawców</span><span class="sxs-lookup"><span data-stu-id="bb7a0-270">All providers</span></span> | <span data-ttu-id="bb7a0-271">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-271">All categories</span></span>                          | <span data-ttu-id="bb7a0-272">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-272">Debug</span></span>             |
| <span data-ttu-id="bb7a0-273">7</span><span class="sxs-lookup"><span data-stu-id="bb7a0-273">7</span></span>      | <span data-ttu-id="bb7a0-274">Wszystkich dostawców</span><span class="sxs-lookup"><span data-stu-id="bb7a0-274">All providers</span></span> | <span data-ttu-id="bb7a0-275">System</span><span class="sxs-lookup"><span data-stu-id="bb7a0-275">System</span></span>                                  | <span data-ttu-id="bb7a0-276">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-276">Debug</span></span>             |
| <span data-ttu-id="bb7a0-277">8</span><span class="sxs-lookup"><span data-stu-id="bb7a0-277">8</span></span>      | <span data-ttu-id="bb7a0-278">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-278">Debug</span></span>         | <span data-ttu-id="bb7a0-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="bb7a0-279">Microsoft</span></span>                               | <span data-ttu-id="bb7a0-280">śledzenia</span><span class="sxs-lookup"><span data-stu-id="bb7a0-280">Trace</span></span>             |

<span data-ttu-id="bb7a0-281">Po utworzeniu `ILogger` obiektu do zapisania dzienniki, `ILoggerFactory` obiektu wybiera jedną regułę na dostawcy, aby zastosować do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="bb7a0-282">Wszystkie komunikaty napisane przez to `ILogger` obiektu są filtrowane w zależności od wybranej reguły.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="bb7a0-283">Reguła specyficzny dla każdej pary kategorii i dostawcy możliwe jest wybierana z dostępne reguły.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="bb7a0-284">Następujący algorytm jest używana dla każdego dostawcy podczas `ILogger` jest tworzony dla danej kategorii:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="bb7a0-285">Wybierz wszystkie reguły zgodne dostawca lub jego alias.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="bb7a0-286">Jeśli żaden nie zostaną znalezione, wybierz wszystkie reguły z dostawcą puste.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="bb7a0-287">W wyniku poprzedniego kroku wybierz reguły z najdłuższy dopasowanie prefiksu kategorii.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="bb7a0-288">Jeśli żaden nie zostaną znalezione, wybierz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="bb7a0-289">Jeśli wybrano wiele reguł **ostatniego** jeden.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="bb7a0-290">Jeśli nie wybrano żadnych reguł, użyj `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-290">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="bb7a0-291">Na przykład, załóżmy, że powyższej listy reguł i utworzysz `ILogger` obiektu dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="bb7a0-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="bb7a0-292">W przypadku dostawcy usług debugowania mają zastosowanie zasady 1, 6, 8.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="bb7a0-293">Reguła 8 jest najbardziej konkretny, więc jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="bb7a0-294">Dostawca konsoli zastosować zasady 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="bb7a0-295">Reguła 3 jest specyficzny.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="bb7a0-296">Po utworzeniu dzienniki z `ILogger` dla kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" dzienniki z `Trace` poziomu i nowszych nastąpi przejście do debugowania dostawcy i dzienniki `Debug` poziomu i nowszych nastąpi przejście do dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="bb7a0-297">**Aliasy dostawcy**</span><span class="sxs-lookup"><span data-stu-id="bb7a0-297">**Provider aliases**</span></span>

<span data-ttu-id="bb7a0-298">Nazwa typu można używać, aby określić dostawcę konfiguracji, ale każdy dostawca definiuje krótszą *alias* łatwiej do użycia.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="bb7a0-299">W przypadku dostawców wbudowanych, użyj następujących aliasów:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="bb7a0-300">Konsola</span><span class="sxs-lookup"><span data-stu-id="bb7a0-300">Console</span></span>
- <span data-ttu-id="bb7a0-301">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="bb7a0-301">Debug</span></span>
- <span data-ttu-id="bb7a0-302">Dziennik zdarzeń</span><span class="sxs-lookup"><span data-stu-id="bb7a0-302">EventLog</span></span>
- <span data-ttu-id="bb7a0-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="bb7a0-303">AzureAppServices</span></span>
- <span data-ttu-id="bb7a0-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="bb7a0-304">TraceSource</span></span>
- <span data-ttu-id="bb7a0-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="bb7a0-305">EventSource</span></span>

<span data-ttu-id="bb7a0-306">**Minimalny poziom domyślny**</span><span class="sxs-lookup"><span data-stu-id="bb7a0-306">**Default minimum level**</span></span>

<span data-ttu-id="bb7a0-307">Brak minimalnej ustawienie poziomu, które obowiązuje tylko wtedy, gdy żadne reguły z konfiguracji lub kod odnoszą się do danego dostawcy oraz kategorii.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-307">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="bb7a0-308">Poniższy przykład pokazuje, jak ustawić minimalny poziom:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="bb7a0-309">Jeśli nie zostanie jawnie ustawiona na poziomie minimalnym, wartością domyślną jest `Information`, co oznacza, że `Trace` i `Debug` dzienniki są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="bb7a0-310">**Funkcje filtrowania**</span><span class="sxs-lookup"><span data-stu-id="bb7a0-310">**Filter functions**</span></span>

<span data-ttu-id="bb7a0-311">W funkcji Filtr można zastosować reguł filtrowania, można napisać kod.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="bb7a0-312">Funkcja filtru jest wywoływany dla wszystkich dostawców i kategorie, które nie mają reguł przypisanego przez konfiguracji lub kodu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-312">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="bb7a0-313">Kod w funkcji ma dostęp do typ dostawcy, kategorii i poziom dziennika, aby zdecydować, czy wiadomość powinny być rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="bb7a0-314">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="bb7a0-316">Niektórzy dostawcy rejestrowania pozwalają określić, gdy zapisywane na nośniku magazynowania lub ignorowane dzienniki na podstawie poziomu dziennika i kategorii.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="bb7a0-317">`AddConsole` i `AddDebug` metody rozszerzenia udostępniają przeciążenia, które umożliwiają przekazywanie w kryteriach filtrowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="bb7a0-318">Następujący przykładowy kod powoduje, że dostawca konsoli do ignorowania dzienniki poniżej `Warning` poziom, gdy dostawca debugowania ignoruje dzienników tworzone w ramach.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="bb7a0-319">`AddEventLog` Metoda ma przeciążenia, które przyjmuje `EventLogSettings` wystąpienia, która może zawierać funkcji filtrowania w jego `Filter` właściwości.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="bb7a0-320">Dostawcy TraceSource nie zapewnia żadnego z tych przeciążeń, ponieważ jej poziom rejestrowania i inne parametry są oparte na `SourceSwitch` i `TraceListener` go używa.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-320">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="bb7a0-321">Można ustawić reguły filtrowania dla wszystkich dostawców, które są zarejestrowane w usłudze `ILoggerFactory` wystąpienia przy użyciu `WithFilter` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="bb7a0-322">W poniższym przykładzie ogranicza dzienniki framework (kategoria rozpoczyna się od "Microsoft" lub "System") z ostrzeżeniami, a w dzienniku aplikacji na poziomie debugowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="bb7a0-323">Jeśli chcesz użyć filtrowania, aby uniemożliwić wszystkie dzienniki zapisywane dla określonej kategorii, można określić `LogLevel.None` jako poziom dziennika minimalną dla tej kategorii.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="bb7a0-324">Wartość całkowita `LogLevel.None` to 6, która jest wyższa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="bb7a0-325">`WithFilter` — Metoda rozszerzenia są dostarczane przez [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="bb7a0-326">Metoda zwraca nową `ILoggerFactory` wystąpienia, która będzie filtrować wiadomości dziennika przekazany do wszystkich dostawców rejestratora zarejestrowany z nim.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="bb7a0-327">Nie wpływa na inne `ILoggerFactory` wystąpienia, w tym oryginalnej `ILoggerFactory` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-327">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="bb7a0-328">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="bb7a0-328">Log scopes</span></span>

<span data-ttu-id="bb7a0-329">Można grupować zestaw operacji logicznych w *zakres* aby można było dołączyć tych samych danych do każdego dziennika, który został utworzony jako część tego zbioru.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="bb7a0-330">Na przykład może być co dziennika utworzona w ramach przetwarzania transakcji, aby uwzględnić identyfikator transakcji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="bb7a0-331">Zakres jest `IDisposable` typu, który jest zwracany przez [ILogger.BeginScope&lt;stanu dławienia&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) — metoda i trwa do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-331">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="bb7a0-332">Użyj zakresu przez zawijania odwołuje Twoje rejestratora `using` zablokować, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="bb7a0-333">Poniższy kod umożliwia zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="bb7a0-335">W *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="bb7a0-336">Konfigurowanie `IncludeScopes` opcja rejestratora konsoli jest wymagana, aby włączyć rejestrowanie zakresu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="bb7a0-337">Konfiguracja `IncludeScopes` przy użyciu *appsettings* pliki konfiguracji będą dostępne w wersji platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="bb7a0-339">W *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="bb7a0-340">Każdy komunikat dziennika zawiera informacje o zakresie:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="bb7a0-341">Rejestrowanie wbudowanych dostawców</span><span class="sxs-lookup"><span data-stu-id="bb7a0-341">Built-in logging providers</span></span>

<span data-ttu-id="bb7a0-342">Platformy ASP.NET Core dostarczany następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="bb7a0-343">Console</span><span class="sxs-lookup"><span data-stu-id="bb7a0-343">Console</span></span>](#console-provider)
* [<span data-ttu-id="bb7a0-344">Debugowania</span><span class="sxs-lookup"><span data-stu-id="bb7a0-344">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="bb7a0-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="bb7a0-345">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="bb7a0-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="bb7a0-346">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="bb7a0-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="bb7a0-347">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="bb7a0-348">Usługa aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="bb7a0-348">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="bb7a0-349">Dostawca konsoli</span><span class="sxs-lookup"><span data-stu-id="bb7a0-349">Console provider</span></span>

<span data-ttu-id="bb7a0-350">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakiet dostawcy wysyła dane wyjściowe dziennika do konsoli.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-351">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="bb7a0-353">[Przeciążenia AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let przekazywane w poziom dziennika minimalne, funkcji filtru i wartość logiczną wskazującą, czy zakresy są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-353">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="bb7a0-354">Innym rozwiązaniem jest przekazanie w `IConfiguration` obiektu, który można określić zakresy pomocy technicznej i poziomy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="bb7a0-355">Planując Konsola dostawca do użycia w środowisku produkcyjnym, należy pamiętać, ma znaczący wpływ na wydajność.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="bb7a0-356">Podczas tworzenia nowego projektu programu Visual Studio `AddConsole` metody wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="bb7a0-357">Ten kod odwołuje się do `Logging` sekcji *appSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="bb7a0-358">Ustawienia wyświetlane limit framework dzienniki, aby ostrzeżenia, umożliwiając aplikacji do logowania się na poziomie debugowania, zgodnie z objaśnieniem w [filtrowania dziennika](#log-filtering) sekcji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="bb7a0-359">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

### <a name="debug-provider"></a><span data-ttu-id="bb7a0-360">Debugowanie dostawcy</span><span class="sxs-lookup"><span data-stu-id="bb7a0-360">Debug provider</span></span>

<span data-ttu-id="bb7a0-361">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakiet dostawcy zapisuje dane wyjściowe dziennika przy użyciu [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) klasy (`Debug.WriteLine` wywołania metody).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="bb7a0-362">W systemie Linux, tego dostawcy zapisuje dzienniki, aby */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="bb7a0-365">[Przeciążenia AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) umożliwiają przekazywanie poziom dziennika minimalną lub funkcji filtru.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-365">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

### <a name="eventsource-provider"></a><span data-ttu-id="bb7a0-366">Dostawca źródła zdarzeń</span><span class="sxs-lookup"><span data-stu-id="bb7a0-366">EventSource provider</span></span>

<span data-ttu-id="bb7a0-367">Dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1.0 lub nowszej, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zaimplementować pakiet dostawcy śledzenia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="bb7a0-368">W systemie Windows, używa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="bb7a0-369">Dostawca jest między platformami, ale żadne zdarzenie kolekcji i wyświetlanie narzędzi jeszcze dla systemu Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="bb7a0-372">Dobrym sposobem na zbieranie i przeglądanie dzienników jest użycie [narzędzie narzędzia PerfView](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-372">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="bb7a0-373">Istnieją inne narzędzia umożliwiające wyświetlanie dzienników zdarzeń systemu Windows, ale narzędzia PerfView zapewnia najlepsze środowisko do pracy z zdarzenia ETW emitowane przez platformę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="bb7a0-374">Aby skonfigurować narzędzia PerfView zbierania zdarzenia zarejestrowane przez tego dostawcę, Dodaj ciąg `*Microsoft-Extensions-Logging` do **dodatkowych dostawców** listy.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="bb7a0-375">(Nie zostały pominięte gwiazdki na początku ciąg.)</span><span class="sxs-lookup"><span data-stu-id="bb7a0-375">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia Perfview dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="bb7a0-377">Dostawca dziennika zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="bb7a0-377">Windows EventLog provider</span></span>

<span data-ttu-id="bb7a0-378">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pakiet dostawcy wysyła dane wyjściowe dziennika w dzienniku zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-378">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-379">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-379">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-380">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-380">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="bb7a0-381">[Przeciążenia AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let przekazywane w `EventLogSettings` lub poziom dziennika minimalnej.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-381">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

### <a name="tracesource-provider"></a><span data-ttu-id="bb7a0-382">TraceSource dostawcy</span><span class="sxs-lookup"><span data-stu-id="bb7a0-382">TraceSource provider</span></span>

<span data-ttu-id="bb7a0-383">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa dostawcy pakietu [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) bibliotek i dostawców.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-383">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-384">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-384">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-385">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-385">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="bb7a0-386">[Przeciążenia AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) umożliwiają przekazywanie przełącznik źródła i nasłuchującego śledzenia.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-386">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="bb7a0-387">Aby użyć tego dostawcy, aplikacja musi działać .NET Framework (zamiast .NET Core).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-387">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="bb7a0-388">Umożliwia dostawcy rozesłać komunikaty do różnych [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takich jak [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) używane w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-388">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="bb7a0-389">Poniższy przykład konfiguruje `TraceSource` dostawcy, który rejestruje `Warning` i wyższe wiadomości dla okna konsoli.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-389">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="bb7a0-390">Dostawca usługi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bb7a0-390">Azure App Service provider</span></span>

<span data-ttu-id="bb7a0-391">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pakiet dostawcy zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji w usłudze Azure App Service i do [magazynu obiektów blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie magazynu Azure.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-391">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="bb7a0-392">Dostawca jest dostępna tylko dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-392">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb7a0-393">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bb7a0-394">Jeśli przeznaczonych dla platformy .NET Core, nie należy zainstalować pakiet dostawcy lub jawnie wywołać [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-394">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="bb7a0-395">Dostawca jest automatycznie dostępne dla aplikacji, gdy aplikacja jest wdrożona w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-395">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="bb7a0-396">Jeśli przeznaczonych dla platformy .NET Framework, Dodaj pakiet dostawcy do projektu i wywołać `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-396">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb7a0-397">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb7a0-397">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="bb7a0-398">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) przeciążenia umożliwia przekazywanie w [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) z którym mogą zastąpić ustawienia domyślne, takie jak rejestrowanie danych wyjściowych szablonu, nazwa obiektu blob i plików limit rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-398">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="bb7a0-399">(*Danych wyjściowych szablonu* jest szablon wiadomości, która jest stosowana do wszystkich dzienników na elemencie podane podczas wywoływania `ILogger` metody.)</span><span class="sxs-lookup"><span data-stu-id="bb7a0-399">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="bb7a0-400">Podczas wdrażania aplikacji usługi app Service, aplikacja będzie honorować ustawień w [dzienników diagnostycznych](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) sekcji **usługi aplikacji** strony portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-400">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="bb7a0-401">Jeśli te ustawienia zostaną zaktualizowane, zmiany zaczynają obowiązywać natychmiast bez konieczności ponownego uruchamiania lub ponownego wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-401">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Ustawienia rejestrowania usługi Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="bb7a0-403">Domyślna lokalizacja dla plików dziennika jest *D:\\macierzystego\\LogFiles\\aplikacji* folderu, a domyślna nazwa pliku jest *yyyymmdd.txt diagnostyki*.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-403">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="bb7a0-404">Domyślnego limitu rozmiaru pliku to 10 MB, a domyślna maksymalna liczba pliki przechowywane wynosi 2.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-404">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="bb7a0-405">Domyślna nazwa obiektu blob jest *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-405">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="bb7a0-406">Aby uzyskać więcej informacji na temat domyślnego zachowania, zobacz [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-406">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="bb7a0-407">Dostawca działa tylko w przypadku, gdy projekt jest uruchamiana w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-407">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="bb7a0-408">Nie ma wpływu, gdy projekt jest uruchamiana lokalnie&mdash;nie zapisu plików lokalnych lub rozwoju lokalnego magazynu obiektów blob.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-408">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="bb7a0-409">Rejestrowanie innych dostawców</span><span class="sxs-lookup"><span data-stu-id="bb7a0-409">Third-party logging providers</span></span>

<span data-ttu-id="bb7a0-410">Struktury rejestrowania innych firm, które współpracują z platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-410">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="bb7a0-411">[elmah.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="bb7a0-411">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="bb7a0-412">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="bb7a0-412">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="bb7a0-413">[JSNLog](http://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="bb7a0-413">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="bb7a0-414">[Loggr](http://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="bb7a0-414">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="bb7a0-415">[NLog](http://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="bb7a0-415">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="bb7a0-416">[Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="bb7a0-416">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="bb7a0-417">Niektórych struktur innych firm mogą wykonywać [semantycznego rejestrowania, nazywany również strukturalnych rejestrowania](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="bb7a0-417">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="bb7a0-418">Przy użyciu platformy innej firmy jest podobny do przy użyciu jednego z dostawców wbudowanych:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-418">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="bb7a0-419">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-419">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="bb7a0-420">Wywoływanie metody rozszerzenia na `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-420">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="bb7a0-421">Aby uzyskać więcej informacji zobacz dokumentację każdego framework.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-421">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="bb7a0-422">Strumieniowe przesyłanie dzienników Azure</span><span class="sxs-lookup"><span data-stu-id="bb7a0-422">Azure log streaming</span></span>

<span data-ttu-id="bb7a0-423">Strumieniowe przesyłanie dzienników Azure umożliwia wyświetlenie dziennika aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-423">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="bb7a0-424">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="bb7a0-424">The application server</span></span>
* <span data-ttu-id="bb7a0-425">Serwer sieci web</span><span class="sxs-lookup"><span data-stu-id="bb7a0-425">The web server</span></span>
* <span data-ttu-id="bb7a0-426">Śledzenie nieudanych żądań</span><span class="sxs-lookup"><span data-stu-id="bb7a0-426">Failed request tracing</span></span>

<span data-ttu-id="bb7a0-427">Aby skonfigurować przesyłania strumieniowego dzienników Azure:</span><span class="sxs-lookup"><span data-stu-id="bb7a0-427">To configure Azure log streaming:</span></span>

* <span data-ttu-id="bb7a0-428">Przejdź do **dzienników diagnostycznych** strony ze strony portalu aplikacji</span><span class="sxs-lookup"><span data-stu-id="bb7a0-428">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="bb7a0-429">Ustaw **rejestrowanie aplikacji (systemu plików)** do włączenia.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-429">Set **Application Logging (Filesystem)** to on.</span></span>

![Strona Azure portalu dzienników diagnostycznych](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="bb7a0-431">Przejdź do **dzienników przesyłania strumieniowego** strony w celu wyświetlenia komunikatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-431">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="bb7a0-432">Są one zarejestrowane przez aplikację za pomocą `ILogger` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="bb7a0-432">They're logged by application through the `ILogger` interface.</span></span>

![Przesyłanie strumieniowe dziennika aplikacji z portalu Azure](index/_static/azure-log-streaming.png)

## <a name="additional-resources"></a><span data-ttu-id="bb7a0-434">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bb7a0-434">Additional resources</span></span>

[<span data-ttu-id="bb7a0-435">Rejestrowanie wysokiej wydajności z LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="bb7a0-435">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
