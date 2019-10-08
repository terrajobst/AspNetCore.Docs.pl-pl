---
title: Rejestrowanie w programie .NET Core i ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać struktury rejestrowania dostarczonej przez pakiet NuGet Microsoft. Extensions. Logging.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 9f7b39cc1c557356b75608817db4e8d6f61af794
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007027"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="db8d5-103">Rejestrowanie w programie .NET Core i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db8d5-103">Logging in .NET Core and ASP.NET Core</span></span>

<span data-ttu-id="db8d5-104">Autorzy [Dykstra](https://github.com/tdykstra) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="db8d5-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="db8d5-105">Platforma .NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm.</span><span class="sxs-lookup"><span data-stu-id="db8d5-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="db8d5-106">W tym artykule pokazano, jak używać interfejsu API rejestrowania z wbudowanymi dostawcami.</span><span class="sxs-lookup"><span data-stu-id="db8d5-106">This article shows how to use the logging API with built-in providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="db8d5-107">Większość przykładów kodu pokazanych w tym artykule pochodzą z ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="db8d5-108">Części tych fragmentów kodu dotyczące rejestrowania mają zastosowanie do dowolnej aplikacji .NET Core, która korzysta z [hosta ogólnego](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="db8d5-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="db8d5-109">Aby uzyskać informacje na temat korzystania z hosta generycznego w aplikacjach konsolowych innych niż sieci Web, zobacz [usługi hostowane](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="db8d5-109">For information about how to use the Generic Host in non-web console apps, see [Hosted services](xref:fundamentals/host/hosted-services).</span></span>

<span data-ttu-id="db8d5-110">Rejestrowanie kodu dla aplikacji bez hosta ogólnego różni się w sposób, w jaki [są dodawane dostawcy](#add-providers) i [są tworzone rejestratory](#create-logs).</span><span class="sxs-lookup"><span data-stu-id="db8d5-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="db8d5-111">Przykłady kodu niehosta są wyświetlane w tych częściach artykułu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-111">Non-host code examples are shown in those sections of the article.</span></span>

::: moniker-end

<span data-ttu-id="db8d5-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="db8d5-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="db8d5-113">Dodaj dostawców</span><span class="sxs-lookup"><span data-stu-id="db8d5-113">Add providers</span></span>

<span data-ttu-id="db8d5-114">Dostawca rejestrowania wyświetla lub przechowuje dzienniki.</span><span class="sxs-lookup"><span data-stu-id="db8d5-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="db8d5-115">Na przykład dostawca konsoli wyświetla dzienniki w konsoli programu, a Dostawca usługi Azure Application Insights przechowuje je na platformie Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="db8d5-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="db8d5-116">Dzienniki mogą być wysyłane do wielu miejsc docelowych przez dodanie wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="db8d5-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="db8d5-117">Aby dodać dostawcę w aplikacji korzystającej z hosta ogólnego, wywołaj metodę rozszerzenia `Add{provider name}` dostawcy w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="db8d5-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="db8d5-118">W aplikacji konsolowej bez hosta Wywołaj metodę rozszerzenia `Add{provider name}` dostawcy podczas tworzenia `LoggerFactory`:</span><span class="sxs-lookup"><span data-stu-id="db8d5-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="db8d5-119">`LoggerFactory` i `AddConsole` wymagają instrukcji `using` dla `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="db8d5-120">Domyślne ASP.NET Core szablonów projektu wywołują <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, które dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="db8d5-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="db8d5-121">Konsola</span><span class="sxs-lookup"><span data-stu-id="db8d5-121">Console</span></span>
* <span data-ttu-id="db8d5-122">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-122">Debug</span></span>
* <span data-ttu-id="db8d5-123">EventSource</span><span class="sxs-lookup"><span data-stu-id="db8d5-123">EventSource</span></span>
* <span data-ttu-id="db8d5-124">EventLog (tylko w przypadku uruchamiania w systemie Windows)</span><span class="sxs-lookup"><span data-stu-id="db8d5-124">EventLog (only when running on Windows)</span></span>

<span data-ttu-id="db8d5-125">Dostawców domyślnych można zastąpić własnymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="db8d5-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="db8d5-126">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> i Dodaj żądanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="db8d5-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0 "

<span data-ttu-id="db8d5-127">Aby dodać dostawcę, wywołaj metodę rozszerzenia `Add{provider name}` dostawcy w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="db8d5-127">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="db8d5-128">Poprzedzający kod wymaga odwołań do `Microsoft.Extensions.Logging` i `Microsoft.Extensions.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-128">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="db8d5-129">Domyślny szablon projektu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, które dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="db8d5-129">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="db8d5-130">Konsola</span><span class="sxs-lookup"><span data-stu-id="db8d5-130">Console</span></span>
* <span data-ttu-id="db8d5-131">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-131">Debug</span></span>
* <span data-ttu-id="db8d5-132">EventSource (rozpoczęcie w ASP.NET Core 2,2)</span><span class="sxs-lookup"><span data-stu-id="db8d5-132">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="db8d5-133">Jeśli używasz `CreateDefaultBuilder`, możesz zastąpić domyślnych dostawców własnymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="db8d5-133">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="db8d5-134">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> i Dodaj żądanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="db8d5-134">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

<span data-ttu-id="db8d5-135">Więcej informacji na temat [wbudowanych dostawców rejestrowania](#built-in-logging-providers) i [dostawców rejestrowania innych](#third-party-logging-providers) firm znajduje się w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-135">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="db8d5-136">Tworzenie dzienników</span><span class="sxs-lookup"><span data-stu-id="db8d5-136">Create logs</span></span>

<span data-ttu-id="db8d5-137">Aby utworzyć dzienniki, użyj obiektu <xref:Microsoft.Extensions.Logging.ILogger%601>.</span><span class="sxs-lookup"><span data-stu-id="db8d5-137">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="db8d5-138">W aplikacji sieci Web lub hostowanej usłudze Pobierz `ILogger` z iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="db8d5-138">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="db8d5-139">W aplikacjach konsolowych innych niż host Użyj `LoggerFactory`, aby utworzyć `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-139">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="db8d5-140">Poniższy ASP.NET Core przykład tworzy Rejestrator z `TodoApiSample.Pages.AboutModel` jako kategorii.</span><span class="sxs-lookup"><span data-stu-id="db8d5-140">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="db8d5-141">*Kategoria* dziennika jest ciągiem, który jest skojarzony z każdym dziennikiem.</span><span class="sxs-lookup"><span data-stu-id="db8d5-141">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="db8d5-142">Wystąpienie `ILogger<T>` dostarczone przez program DI tworzy dzienniki, które mają w pełni kwalifikowaną nazwę typu `T` jako kategorię.</span><span class="sxs-lookup"><span data-stu-id="db8d5-142">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="db8d5-143">Poniższy przykład aplikacji nieprzyjmującej konsoli tworzy Rejestrator z `LoggingConsoleApp.Program` jako kategorię.</span><span class="sxs-lookup"><span data-stu-id="db8d5-143">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

::: moniker-end

<span data-ttu-id="db8d5-144">W poniższych przykładach ASP.NET Core i aplikacji konsoli Rejestrator jest używany do tworzenia dzienników z `Information` jako poziom.</span><span class="sxs-lookup"><span data-stu-id="db8d5-144">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="db8d5-145">*Poziom* dziennika wskazuje ważność rejestrowanego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="db8d5-145">The Log *level* indicates the severity of the logged event.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

<span data-ttu-id="db8d5-146">[Poziomy](#log-level) i [Kategorie](#log-category) zostały wyjaśnione bardziej szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-146">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-3.0"

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="db8d5-147">Tworzenie dzienników w klasie programu</span><span class="sxs-lookup"><span data-stu-id="db8d5-147">Create logs in the Program class</span></span>

<span data-ttu-id="db8d5-148">Aby napisać dzienniki w klasie `Program` aplikacji ASP.NET Core, Pobierz wystąpienie `ILogger` od DI po skompilowaniu hosta:</span><span class="sxs-lookup"><span data-stu-id="db8d5-148">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="db8d5-149">Tworzenie dzienników w klasie startowej</span><span class="sxs-lookup"><span data-stu-id="db8d5-149">Create logs in the Startup class</span></span>

<span data-ttu-id="db8d5-150">Aby napisać dzienniki w metodzie `Startup.Configure` aplikacji ASP.NET Core, należy uwzględnić parametr `ILogger` w podpisie metody:</span><span class="sxs-lookup"><span data-stu-id="db8d5-150">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="db8d5-151">Zapisywanie dzienników przed ukończeniem instalacji przez DI kontenera w metodzie `Startup.ConfigureServices` nie jest obsługiwane:</span><span class="sxs-lookup"><span data-stu-id="db8d5-151">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="db8d5-152">Iniekcja rejestratora do konstruktora `Startup` nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="db8d5-152">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="db8d5-153">Iniekcja rejestratora do sygnatury metody `Startup.ConfigureServices` nie jest obsługiwana</span><span class="sxs-lookup"><span data-stu-id="db8d5-153">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="db8d5-154">Przyczyną tego ograniczenia jest to, że rejestrowanie jest zależne od typu i konfiguracji, która z tego powodu zależy od DI.</span><span class="sxs-lookup"><span data-stu-id="db8d5-154">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="db8d5-155">Kontener DI nie jest ustawiany do momentu zakończenia `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-155">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="db8d5-156">Iniekcja konstruktora rejestratora do `Startup` działa we wcześniejszych wersjach ASP.NET Core, ponieważ dla hosta sieci Web jest tworzony oddzielny kontener DI.</span><span class="sxs-lookup"><span data-stu-id="db8d5-156">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="db8d5-157">Aby uzyskać informacje o tym, dlaczego dla hosta generycznego jest tworzony tylko jeden kontener, zobacz [zawiadomienie o rozdzieleniu zmian](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="db8d5-157">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="db8d5-158">Jeśli trzeba skonfigurować usługę, która zależy od `ILogger<T>`, można to zrobić za pomocą iniekcji konstruktora lub dostarczając metodę fabryki.</span><span class="sxs-lookup"><span data-stu-id="db8d5-158">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="db8d5-159">Podejście metody fabryki jest zalecane tylko wtedy, gdy nie ma żadnych innych opcji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-159">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="db8d5-160">Załóżmy na przykład, że musisz wypełnić Właściwość usługą z:</span><span class="sxs-lookup"><span data-stu-id="db8d5-160">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="db8d5-161">Poprzedni wyróżniony kod jest `Func`, który jest uruchamiany po raz pierwszy do utworzenia wystąpienia `MyService`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-161">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="db8d5-162">W ten sposób można uzyskać dostęp do dowolnych zarejestrowanych usług.</span><span class="sxs-lookup"><span data-stu-id="db8d5-162">You can access any of the registered services in this way.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="db8d5-163">Tworzenie dzienników w programie startowym</span><span class="sxs-lookup"><span data-stu-id="db8d5-163">Create logs in Startup</span></span>

<span data-ttu-id="db8d5-164">Aby napisać dzienniki w klasie `Startup`, należy uwzględnić parametr `ILogger` w sygnaturze konstruktora:</span><span class="sxs-lookup"><span data-stu-id="db8d5-164">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="db8d5-165">Tworzenie dzienników w klasie programu</span><span class="sxs-lookup"><span data-stu-id="db8d5-165">Create logs in the Program class</span></span>

<span data-ttu-id="db8d5-166">Aby napisać dzienniki w klasie `Program`, Pobierz wystąpienie `ILogger` od DI:</span><span class="sxs-lookup"><span data-stu-id="db8d5-166">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="db8d5-167">Brak metod rejestratora asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="db8d5-167">No asynchronous logger methods</span></span>

<span data-ttu-id="db8d5-168">Rejestracja powinna być tak szybka, że nie jest to koszt wydajności kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="db8d5-168">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="db8d5-169">Jeśli magazyn danych rejestrowania jest wolny, nie zapisuj go bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="db8d5-169">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="db8d5-170">Najpierw Rozważ zapisanie komunikatów dziennika do szybkiego sklepu, a następnie przeniesienie ich do wolnego magazynu później.</span><span class="sxs-lookup"><span data-stu-id="db8d5-170">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="db8d5-171">Na przykład jeśli rejestrujesz się do SQL Server, nie chcesz tego robić bezpośrednio w metodzie `Log`, ponieważ metody `Log` są synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="db8d5-171">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="db8d5-172">Zamiast tego można synchronicznie dodawać komunikaty dziennika do kolejki w pamięci, a proces roboczy w tle ściągał komunikaty z kolejki, aby wykonać asynchroniczne działanie wypychania danych do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="db8d5-172">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="db8d5-173">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-173">Configuration</span></span>

<span data-ttu-id="db8d5-174">Konfiguracja dostawcy rejestrowania jest świadczona przez co najmniej jednego dostawcę konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="db8d5-174">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="db8d5-175">Formaty plików (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="db8d5-175">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="db8d5-176">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="db8d5-176">Command-line arguments.</span></span>
* <span data-ttu-id="db8d5-177">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="db8d5-177">Environment variables.</span></span>
* <span data-ttu-id="db8d5-178">Obiekty platformy .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="db8d5-178">In-memory .NET objects.</span></span>
* <span data-ttu-id="db8d5-179">Magazyn niezaszyfrowanego [klucza tajnego](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="db8d5-179">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="db8d5-180">Zaszyfrowany magazyn użytkowników, taki jak [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="db8d5-180">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="db8d5-181">Dostawcy niestandardowi (instalowani lub utworzony).</span><span class="sxs-lookup"><span data-stu-id="db8d5-181">Custom providers (installed or created).</span></span>

<span data-ttu-id="db8d5-182">Na przykład konfiguracja rejestrowania jest zwykle udostępniana przez sekcję `Logging` plików ustawień aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-182">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="db8d5-183">Poniższy przykład pokazuje zawartość typowej wartości *appSettings. Plik Development. JSON* :</span><span class="sxs-lookup"><span data-stu-id="db8d5-183">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="db8d5-184">Właściwość `Logging` może mieć `LogLevel` i właściwości dostawcy dzienników (wyświetlana jest konsola).</span><span class="sxs-lookup"><span data-stu-id="db8d5-184">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="db8d5-185">Właściwość `LogLevel` w obszarze `Logging` określa minimalny [poziom](#log-level) rejestrowania wybranych kategorii.</span><span class="sxs-lookup"><span data-stu-id="db8d5-185">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="db8d5-186">W przykładzie `System` i `Microsoft` kategorii są rejestrowane na poziomie `Information`, a wszystkie inne będą logować się na poziomie `Debug`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-186">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="db8d5-187">Inne właściwości w obszarze `Logging` określają dostawców rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="db8d5-187">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="db8d5-188">Przykład dotyczy dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="db8d5-188">The example is for the Console provider.</span></span> <span data-ttu-id="db8d5-189">Jeśli dostawca obsługuje [zakresy dzienników](#log-scopes), `IncludeScopes` wskazuje, czy są one włączone.</span><span class="sxs-lookup"><span data-stu-id="db8d5-189">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="db8d5-190">Właściwość dostawcy (taka jak `Console` w przykładzie) może również określać Właściwość `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-190">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="db8d5-191">`LogLevel` w obszarze dostawcy Określa poziomy do rejestrowania dla tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="db8d5-191">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="db8d5-192">Jeśli poziomy są określone w `Logging.{providername}.LogLevel`, zastępują wszystkie elementy ustawione w `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-192">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

<span data-ttu-id="db8d5-193">Aby uzyskać informacje na temat implementowania dostawców konfiguracji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="db8d5-193">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="db8d5-194">Przykładowe dane wyjściowe rejestrowania</span><span class="sxs-lookup"><span data-stu-id="db8d5-194">Sample logging output</span></span>

<span data-ttu-id="db8d5-195">W przypadku przykładowego kodu podanego w poprzedniej sekcji Dzienniki są wyświetlane w konsoli programu, gdy aplikacja jest uruchamiana z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="db8d5-195">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="db8d5-196">Oto przykład danych wyjściowych konsoli:</span><span class="sxs-lookup"><span data-stu-id="db8d5-196">Here's an example of console output:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 84.26180000000001ms 307
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/api/todo/0
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

<span data-ttu-id="db8d5-197">Poprzednie dzienniki zostały wygenerowane przez utworzenie żądania HTTP GET do przykładowej aplikacji w `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-197">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="db8d5-198">Oto przykład tych samych dzienników, które są wyświetlane w oknie debugowania podczas uruchamiania przykładowej aplikacji w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="db8d5-198">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request starting HTTP/2.0 GET https://localhost:44328/api/todo/0  
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
TodoApiSample.Controllers.TodoController: Information: Getting item 0
TodoApiSample.Controllers.TodoController: Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult: Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 34.167ms
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request finished in 98.41300000000001ms 404
```

<span data-ttu-id="db8d5-199">Dzienniki utworzone za pomocą wywołań `ILogger` przedstawionych w poprzedniej sekcji zaczynają się od "TodoApiSample".</span><span class="sxs-lookup"><span data-stu-id="db8d5-199">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="db8d5-200">Dzienniki zaczynające się od kategorii "Microsoft" pochodzą z kodu ASP.NET Core Framework.</span><span class="sxs-lookup"><span data-stu-id="db8d5-200">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="db8d5-201">ASP.NET Core i kod aplikacji używają tego samego rejestrowania interfejsu API i dostawców.</span><span class="sxs-lookup"><span data-stu-id="db8d5-201">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="db8d5-202">Dzienniki utworzone za pomocą wywołań `ILogger` przedstawionych w poprzedniej sekcji zaczynają się od "TodoApi".</span><span class="sxs-lookup"><span data-stu-id="db8d5-202">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="db8d5-203">Dzienniki zaczynające się od kategorii "Microsoft" pochodzą z kodu ASP.NET Core Framework.</span><span class="sxs-lookup"><span data-stu-id="db8d5-203">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="db8d5-204">ASP.NET Core i kod aplikacji używają tego samego rejestrowania interfejsu API i dostawców.</span><span class="sxs-lookup"><span data-stu-id="db8d5-204">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

<span data-ttu-id="db8d5-205">W pozostałej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="db8d5-205">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="db8d5-206">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="db8d5-206">NuGet packages</span></span>

<span data-ttu-id="db8d5-207">Interfejsy `ILogger` i `ILoggerFactory` są w [Microsoft. Extensions. Logging. Abstracts](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)i domyślne implementacje dla nich są w [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="db8d5-207">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="db8d5-208">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="db8d5-208">Log category</span></span>

<span data-ttu-id="db8d5-209">Po utworzeniu obiektu `ILogger` jest dla niego określona *Kategoria* .</span><span class="sxs-lookup"><span data-stu-id="db8d5-209">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="db8d5-210">Ta kategoria jest dołączona do każdego komunikatu dziennika utworzonego przez to wystąpienie `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-210">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="db8d5-211">Kategoria może być dowolnym ciągiem, ale Konwencja ma używać nazwy klasy, takiej jak "TodoApi. controllers. TodoController".</span><span class="sxs-lookup"><span data-stu-id="db8d5-211">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="db8d5-212">Użyj `ILogger<T>`, aby uzyskać wystąpienie `ILogger` używające w pełni kwalifikowanej nazwy typu `T` jako kategorii:</span><span class="sxs-lookup"><span data-stu-id="db8d5-212">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="db8d5-213">Aby jawnie określić kategorię, wywołaj `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="db8d5-213">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="db8d5-214">`ILogger<T>` jest równoważne wywołaniu `CreateLogger` z w pełni kwalifikowaną nazwą typu `T`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-214">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="db8d5-215">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="db8d5-215">Log level</span></span>

<span data-ttu-id="db8d5-216">Każdy dziennik Określa wartość <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="db8d5-216">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="db8d5-217">Poziom dziennika wskazuje ważność lub ważność.</span><span class="sxs-lookup"><span data-stu-id="db8d5-217">The log level indicates the severity or importance.</span></span> <span data-ttu-id="db8d5-218">Przykładowo można napisać dziennik `Information`, gdy metoda zostanie zakończona normalnie, a dziennik `Warning`, gdy metoda zwraca *404 nie znaleziono* kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-218">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="db8d5-219">Poniższy kod tworzy `Information` i `Warning` dzienników:</span><span class="sxs-lookup"><span data-stu-id="db8d5-219">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="db8d5-220">W powyższym kodzie pierwszy parametr jest [identyfikatorem zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="db8d5-220">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="db8d5-221">Drugi parametr jest szablonem wiadomości z symbolami zastępczymi dla wartości argumentów dostarczonych przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="db8d5-221">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="db8d5-222">Parametry metody zostały wyjaśnione w [sekcji szablon komunikatu](#log-message-template) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-222">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="db8d5-223">Metody rejestrowania, które obejmują poziom w nazwie metody (na przykład `LogInformation` i `LogWarning`) są [metodami rozszerzenia dla ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="db8d5-223">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="db8d5-224">Te metody wywołują metodę `Log`, która przyjmuje parametr `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-224">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="db8d5-225">Metodę `Log` można wywołać bezpośrednio zamiast jednej z tych metod rozszerzających, ale składnia jest stosunkowo skomplikowana.</span><span class="sxs-lookup"><span data-stu-id="db8d5-225">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="db8d5-226">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.Extensions.Logging.ILogger> i [kod źródłowy rozszerzeń rejestratora](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="db8d5-226">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="db8d5-227">ASP.NET Core definiuje następujące poziomy dziennika uporządkowane w tym miejscu od najniższej do najwyższej wagi.</span><span class="sxs-lookup"><span data-stu-id="db8d5-227">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="db8d5-228">Ślad = 0</span><span class="sxs-lookup"><span data-stu-id="db8d5-228">Trace = 0</span></span>

  <span data-ttu-id="db8d5-229">Aby uzyskać informacje, które są zazwyczaj cenne tylko dla debugowania.</span><span class="sxs-lookup"><span data-stu-id="db8d5-229">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="db8d5-230">Komunikaty te mogą zawierać poufne dane aplikacji, dlatego nie powinny być włączone w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="db8d5-230">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="db8d5-231">*Domyślnie wyłączona.*</span><span class="sxs-lookup"><span data-stu-id="db8d5-231">*Disabled by default.*</span></span>

* <span data-ttu-id="db8d5-232">Debuguj = 1</span><span class="sxs-lookup"><span data-stu-id="db8d5-232">Debug = 1</span></span>

  <span data-ttu-id="db8d5-233">Informacje, które mogą być przydatne podczas tworzenia i debugowania.</span><span class="sxs-lookup"><span data-stu-id="db8d5-233">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="db8d5-234">Przykład: `Entering method Configure with flag set to true.` Włącz dzienniki poziomu `Debug` w środowisku produkcyjnym tylko podczas rozwiązywania problemów, ze względu na dużą ilość dzienników.</span><span class="sxs-lookup"><span data-stu-id="db8d5-234">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="db8d5-235">Informacje = 2</span><span class="sxs-lookup"><span data-stu-id="db8d5-235">Information = 2</span></span>

  <span data-ttu-id="db8d5-236">Do śledzenia ogólnego przepływu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-236">For tracking the general flow of the app.</span></span> <span data-ttu-id="db8d5-237">Te dzienniki zwykle mają pewną wartość długoterminową.</span><span class="sxs-lookup"><span data-stu-id="db8d5-237">These logs typically have some long-term value.</span></span> <span data-ttu-id="db8d5-238">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="db8d5-238">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="db8d5-239">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="db8d5-239">Warning = 3</span></span>

  <span data-ttu-id="db8d5-240">Dla nietypowych lub nieoczekiwanych zdarzeń w przepływie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-240">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="db8d5-241">Mogą to być błędy lub inne warunki, które nie powodują zatrzymania aplikacji, ale konieczne może być zbadanie.</span><span class="sxs-lookup"><span data-stu-id="db8d5-241">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="db8d5-242">Obsłużone wyjątki są typowym miejscem do korzystania z poziomu dziennika `Warning`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-242">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="db8d5-243">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="db8d5-243">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="db8d5-244">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="db8d5-244">Error = 4</span></span>

  <span data-ttu-id="db8d5-245">W przypadku błędów i wyjątków, których nie można obsłużyć.</span><span class="sxs-lookup"><span data-stu-id="db8d5-245">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="db8d5-246">Te komunikaty wskazują niepowodzenie w bieżącym działaniu lub operacji (np. bieżące żądanie HTTP), a nie awaria całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-246">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="db8d5-247">Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="db8d5-247">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="db8d5-248">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="db8d5-248">Critical = 5</span></span>

  <span data-ttu-id="db8d5-249">Dla niepowodzeń, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="db8d5-249">For failures that require immediate attention.</span></span> <span data-ttu-id="db8d5-250">Przykłady: scenariusze utraty danych, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="db8d5-250">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="db8d5-251">Poziom dziennika służy do kontrolowania, ile danych wyjściowych dziennika jest zapisywana w określonym nośniku lub oknie wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="db8d5-251">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="db8d5-252">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="db8d5-252">For example:</span></span>

* <span data-ttu-id="db8d5-253">W środowisku produkcyjnym Wyślij `Trace` do `Information` poziomu do magazynu danych woluminu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-253">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="db8d5-254">Wyślij `Warning` do `Critical` do magazynu danych wartości.</span><span class="sxs-lookup"><span data-stu-id="db8d5-254">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="db8d5-255">Podczas programowania Wyślij `Warning` do `Critical` do konsoli i Dodaj `Trace` do `Information` podczas rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="db8d5-255">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="db8d5-256">W sekcji [filtrowanie dzienników](#log-filtering) w dalszej części tego artykułu wyjaśniono, jak kontrolować poziomy dzienników obsługiwane przez dostawcę.</span><span class="sxs-lookup"><span data-stu-id="db8d5-256">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="db8d5-257">ASP.NET Core zapisuje dzienniki dla zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="db8d5-257">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="db8d5-258">W przykładach dzienników wymienionych wcześniej w tym artykule wykluczono dzienniki poniżej `Information` poziomu, dlatego nie zostały utworzone dzienniki `Debug` lub `Trace`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-258">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="db8d5-259">Oto przykład dzienników konsoli utworzonych przez uruchomienie przykładowej aplikacji skonfigurowanej do wyświetlania dzienników `Debug`:</span><span class="sxs-lookup"><span data-stu-id="db8d5-259">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of authorization filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of resource filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of action filters (in the following order): Microsoft.AspNetCore.Mvc.Filters.ControllerActionFilter (Order: -2147483648), Microsoft.AspNetCore.Mvc.ModelBinding.UnsupportedContentTypeFilter (Order: -3000)
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of exception filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of result filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[22]
      Attempting to bind parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[44]
      Attempting to bind parameter 'id' of type 'System.String' using the name 'id' in request data ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[45]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[23]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[26]
      Attempting to validate the bound parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[27]
      Done attempting to validate the bound parameter 'id' of type 'System.String'.
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[2]
      Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 32.690400000000004ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 176.9103ms 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

## <a name="log-event-id"></a><span data-ttu-id="db8d5-260">Identyfikator zdarzenia dziennika</span><span class="sxs-lookup"><span data-stu-id="db8d5-260">Log event ID</span></span>

<span data-ttu-id="db8d5-261">Każdy dziennik może określać *Identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="db8d5-261">Each log can specify an *event ID*.</span></span> <span data-ttu-id="db8d5-262">Aplikacja Przykładowa wykonuje to przy użyciu lokalnie zdefiniowanej klasy `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="db8d5-262">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="db8d5-263">Identyfikator zdarzenia kojarzy zestaw zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="db8d5-263">An event ID associates a set of events.</span></span> <span data-ttu-id="db8d5-264">Na przykład wszystkie dzienniki związane z wyświetlaniem listy elementów na stronie mogą być 1001.</span><span class="sxs-lookup"><span data-stu-id="db8d5-264">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="db8d5-265">Dostawca rejestrowania może przechowywać identyfikator zdarzenia w polu identyfikatora, w komunikacie rejestrowania lub wcale.</span><span class="sxs-lookup"><span data-stu-id="db8d5-265">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="db8d5-266">Dostawca debugowania nie pokazuje identyfikatorów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="db8d5-266">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="db8d5-267">Dostawca konsoli pokazuje identyfikatory zdarzeń w nawiasach po kategorii:</span><span class="sxs-lookup"><span data-stu-id="db8d5-267">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="db8d5-268">Szablon komunikatu dziennika</span><span class="sxs-lookup"><span data-stu-id="db8d5-268">Log message template</span></span>

<span data-ttu-id="db8d5-269">Każdy dziennik Określa szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="db8d5-269">Each log specifies a message template.</span></span> <span data-ttu-id="db8d5-270">Szablon wiadomości może zawierać symbole zastępcze, dla których podano argumenty.</span><span class="sxs-lookup"><span data-stu-id="db8d5-270">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="db8d5-271">Użyj nazw dla symboli zastępczych, a nie liczby.</span><span class="sxs-lookup"><span data-stu-id="db8d5-271">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="db8d5-272">Kolejność symboli zastępczych, nie ich nazw, określa, które parametry są używane do dostarczania ich wartości.</span><span class="sxs-lookup"><span data-stu-id="db8d5-272">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="db8d5-273">W poniższym kodzie Zwróć uwagę, że nazwy parametrów są poza kolejnością w szablonie wiadomości:</span><span class="sxs-lookup"><span data-stu-id="db8d5-273">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="db8d5-274">Ten kod tworzy komunikat dziennika z wartościami parametrów w kolejności:</span><span class="sxs-lookup"><span data-stu-id="db8d5-274">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="db8d5-275">Struktura rejestrowania działa w ten sposób, aby dostawcy rejestrowania mogli zaimplementować [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="db8d5-275">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="db8d5-276">Same argumenty są przesyłane do systemu rejestrowania, a nie tylko dla sformatowanego szablonu wiadomości.</span><span class="sxs-lookup"><span data-stu-id="db8d5-276">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="db8d5-277">Te informacje umożliwiają dostawcom rejestrowania przechowywanie wartości parametrów jako pól.</span><span class="sxs-lookup"><span data-stu-id="db8d5-277">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="db8d5-278">Na przykład załóżmy, że wywołania metod rejestratora wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="db8d5-278">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="db8d5-279">W przypadku wysyłania dzienników do usługi Azure Table Storage każda jednostka tabeli platformy Azure może mieć właściwości `ID` i `RequestTime`, które upraszczają zapytania dotyczące danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="db8d5-279">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="db8d5-280">Zapytanie może znaleźć wszystkie dzienniki w określonym zakresie `RequestTime` bez analizowania limitu czasu wiadomości tekstowej.</span><span class="sxs-lookup"><span data-stu-id="db8d5-280">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="db8d5-281">Wyjątki rejestrowania</span><span class="sxs-lookup"><span data-stu-id="db8d5-281">Logging exceptions</span></span>

<span data-ttu-id="db8d5-282">Metody rejestratora mają przeciążenia umożliwiające przekazanie wyjątku, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="db8d5-282">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="db8d5-283">Różni dostawcy obsługują informacje o wyjątkach na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="db8d5-283">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="db8d5-284">Oto przykład danych wyjściowych dostawcy debugowania z kodu pokazanego powyżej.</span><span class="sxs-lookup"><span data-stu-id="db8d5-284">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="db8d5-285">Filtrowanie dzienników</span><span class="sxs-lookup"><span data-stu-id="db8d5-285">Log filtering</span></span>

<span data-ttu-id="db8d5-286">Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="db8d5-286">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="db8d5-287">Wszystkie dzienniki poniżej minimalnego poziomu nie są przesyłane do tego dostawcy, więc nie są wyświetlane ani przechowywane.</span><span class="sxs-lookup"><span data-stu-id="db8d5-287">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="db8d5-288">Aby pominąć wszystkie dzienniki, określ `LogLevel.None` jako minimalny poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="db8d5-288">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="db8d5-289">Wartość całkowita `LogLevel.None` to 6, która jest większa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="db8d5-289">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="db8d5-290">Utwórz reguły filtru w konfiguracji</span><span class="sxs-lookup"><span data-stu-id="db8d5-290">Create filter rules in configuration</span></span>

<span data-ttu-id="db8d5-291">Kod szablonu projektu wywołuje `CreateDefaultBuilder` w celu skonfigurowania rejestrowania dla konsoli i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="db8d5-291">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="db8d5-292">Metoda `CreateDefaultBuilder` konfiguruje rejestrowanie, aby wyszukać konfigurację w sekcji `Logging`, jak wyjaśniono [wcześniej w tym artykule](#configuration).</span><span class="sxs-lookup"><span data-stu-id="db8d5-292">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="db8d5-293">Dane konfiguracyjne określają minimalne poziomy dziennika według dostawcy i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="db8d5-293">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

::: moniker-end

<span data-ttu-id="db8d5-294">Ten kod JSON tworzy sześć reguł filtrowania: jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jeden dla wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="db8d5-294">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="db8d5-295">Dla każdego dostawcy wybierana jest pojedyncza reguła, gdy zostanie utworzony obiekt `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-295">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="db8d5-296">Filtrowanie reguł w kodzie</span><span class="sxs-lookup"><span data-stu-id="db8d5-296">Filter rules in code</span></span>

<span data-ttu-id="db8d5-297">Poniższy przykład pokazuje, jak zarejestrować reguły filtru w kodzie:</span><span class="sxs-lookup"><span data-stu-id="db8d5-297">The following example shows how to register filter rules in code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

<span data-ttu-id="db8d5-298">Drugi `AddFilter` określa dostawcę debugowania za pomocą nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-298">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="db8d5-299">Pierwszy `AddFilter` dotyczy wszystkich dostawców, ponieważ nie określa typu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="db8d5-299">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="db8d5-300">Jak są stosowane reguły filtrowania</span><span class="sxs-lookup"><span data-stu-id="db8d5-300">How filtering rules are applied</span></span>

<span data-ttu-id="db8d5-301">Dane konfiguracji i kod `AddFilter` pokazane w powyższych przykładach tworzą reguły przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="db8d5-301">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="db8d5-302">Pierwsze sześć pochodzi z przykładu konfiguracji, a ostatnie dwa pochodzą z przykładu kodu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-302">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="db8d5-303">Number</span><span class="sxs-lookup"><span data-stu-id="db8d5-303">Number</span></span> | <span data-ttu-id="db8d5-304">Dostawca</span><span class="sxs-lookup"><span data-stu-id="db8d5-304">Provider</span></span>      | <span data-ttu-id="db8d5-305">Kategorie zaczynające się od...</span><span class="sxs-lookup"><span data-stu-id="db8d5-305">Categories that begin with ...</span></span>          | <span data-ttu-id="db8d5-306">Minimalny poziom rejestrowania</span><span class="sxs-lookup"><span data-stu-id="db8d5-306">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="db8d5-307">1</span><span class="sxs-lookup"><span data-stu-id="db8d5-307">1</span></span>      | <span data-ttu-id="db8d5-308">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-308">Debug</span></span>         | <span data-ttu-id="db8d5-309">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="db8d5-309">All categories</span></span>                          | <span data-ttu-id="db8d5-310">Information</span><span class="sxs-lookup"><span data-stu-id="db8d5-310">Information</span></span>       |
| <span data-ttu-id="db8d5-311">2</span><span class="sxs-lookup"><span data-stu-id="db8d5-311">2</span></span>      | <span data-ttu-id="db8d5-312">Konsola</span><span class="sxs-lookup"><span data-stu-id="db8d5-312">Console</span></span>       | <span data-ttu-id="db8d5-313">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="db8d5-313">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="db8d5-314">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="db8d5-314">Warning</span></span>           |
| <span data-ttu-id="db8d5-315">3</span><span class="sxs-lookup"><span data-stu-id="db8d5-315">3</span></span>      | <span data-ttu-id="db8d5-316">Konsola</span><span class="sxs-lookup"><span data-stu-id="db8d5-316">Console</span></span>       | <span data-ttu-id="db8d5-317">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="db8d5-317">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="db8d5-318">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-318">Debug</span></span>             |
| <span data-ttu-id="db8d5-319">4</span><span class="sxs-lookup"><span data-stu-id="db8d5-319">4</span></span>      | <span data-ttu-id="db8d5-320">Konsola</span><span class="sxs-lookup"><span data-stu-id="db8d5-320">Console</span></span>       | <span data-ttu-id="db8d5-321">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="db8d5-321">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="db8d5-322">Błąd</span><span class="sxs-lookup"><span data-stu-id="db8d5-322">Error</span></span>             |
| <span data-ttu-id="db8d5-323">5</span><span class="sxs-lookup"><span data-stu-id="db8d5-323">5</span></span>      | <span data-ttu-id="db8d5-324">Konsola</span><span class="sxs-lookup"><span data-stu-id="db8d5-324">Console</span></span>       | <span data-ttu-id="db8d5-325">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="db8d5-325">All categories</span></span>                          | <span data-ttu-id="db8d5-326">Information</span><span class="sxs-lookup"><span data-stu-id="db8d5-326">Information</span></span>       |
| <span data-ttu-id="db8d5-327">6</span><span class="sxs-lookup"><span data-stu-id="db8d5-327">6</span></span>      | <span data-ttu-id="db8d5-328">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="db8d5-328">All providers</span></span> | <span data-ttu-id="db8d5-329">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="db8d5-329">All categories</span></span>                          | <span data-ttu-id="db8d5-330">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-330">Debug</span></span>             |
| <span data-ttu-id="db8d5-331">7</span><span class="sxs-lookup"><span data-stu-id="db8d5-331">7</span></span>      | <span data-ttu-id="db8d5-332">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="db8d5-332">All providers</span></span> | <span data-ttu-id="db8d5-333">System</span><span class="sxs-lookup"><span data-stu-id="db8d5-333">System</span></span>                                  | <span data-ttu-id="db8d5-334">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-334">Debug</span></span>             |
| <span data-ttu-id="db8d5-335">8</span><span class="sxs-lookup"><span data-stu-id="db8d5-335">8</span></span>      | <span data-ttu-id="db8d5-336">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-336">Debug</span></span>         | <span data-ttu-id="db8d5-337">Microsoft</span><span class="sxs-lookup"><span data-stu-id="db8d5-337">Microsoft</span></span>                               | <span data-ttu-id="db8d5-338">Szuka</span><span class="sxs-lookup"><span data-stu-id="db8d5-338">Trace</span></span>             |

<span data-ttu-id="db8d5-339">Po utworzeniu obiektu `ILogger` obiekt `ILoggerFactory` wybiera jedną regułę dla każdego dostawcy do zastosowania do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="db8d5-339">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="db8d5-340">Wszystkie komunikaty zapisywane przez wystąpienie `ILogger` są filtrowane na podstawie wybranych reguł.</span><span class="sxs-lookup"><span data-stu-id="db8d5-340">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="db8d5-341">Najbardziej konkretną regułą można wybrać dla każdego dostawcy i pary kategorii z dostępnych reguł.</span><span class="sxs-lookup"><span data-stu-id="db8d5-341">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="db8d5-342">Następujący algorytm jest używany dla każdego dostawcy, gdy dla danej kategorii jest tworzony `ILogger`:</span><span class="sxs-lookup"><span data-stu-id="db8d5-342">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="db8d5-343">Wybierz wszystkie reguły, które pasują do dostawcy lub jego aliasu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-343">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="db8d5-344">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły z pustym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="db8d5-344">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="db8d5-345">W wyniku poprzedniego kroku wybierz pozycję reguły z najdłuższym prefiksem kategorii.</span><span class="sxs-lookup"><span data-stu-id="db8d5-345">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="db8d5-346">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="db8d5-346">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="db8d5-347">Jeśli wybrano wiele reguł, zrób to **ostatnie** .</span><span class="sxs-lookup"><span data-stu-id="db8d5-347">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="db8d5-348">Jeśli nie wybrano żadnych reguł, użyj `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-348">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="db8d5-349">Na powyższej liście reguł Załóżmy, że tworzysz obiekt `ILogger` dla kategorii "Microsoft. AspNetCore. MVC. Razor. RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="db8d5-349">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="db8d5-350">Dla dostawcy debugowania obowiązują reguły 1, 6 i 8.</span><span class="sxs-lookup"><span data-stu-id="db8d5-350">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="db8d5-351">Reguła 8 jest najbardziej specyficzna, więc jest to jedna wybrana.</span><span class="sxs-lookup"><span data-stu-id="db8d5-351">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="db8d5-352">W przypadku dostawcy konsoli obowiązują reguły 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="db8d5-352">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="db8d5-353">Reguła 3 jest najbardziej specyficzna.</span><span class="sxs-lookup"><span data-stu-id="db8d5-353">Rule 3 is most specific.</span></span>

<span data-ttu-id="db8d5-354">Wystąpienie `ILogger` wysyła dzienniki poziomu `Trace` i powyżej do dostawcy debugowania.</span><span class="sxs-lookup"><span data-stu-id="db8d5-354">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="db8d5-355">Dzienniki poziomu `Debug` i nowsze są wysyłane do dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="db8d5-355">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="db8d5-356">Aliasy dostawców</span><span class="sxs-lookup"><span data-stu-id="db8d5-356">Provider aliases</span></span>

<span data-ttu-id="db8d5-357">Każdy dostawca definiuje *alias* , który może być używany w konfiguracji zamiast w pełni kwalifikowanej nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-357">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="db8d5-358">W przypadku dostawców wbudowanych Użyj następujących aliasów:</span><span class="sxs-lookup"><span data-stu-id="db8d5-358">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="db8d5-359">Konsola</span><span class="sxs-lookup"><span data-stu-id="db8d5-359">Console</span></span>
* <span data-ttu-id="db8d5-360">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-360">Debug</span></span>
* <span data-ttu-id="db8d5-361">EventSource</span><span class="sxs-lookup"><span data-stu-id="db8d5-361">EventSource</span></span>
* <span data-ttu-id="db8d5-362">Elemencie</span><span class="sxs-lookup"><span data-stu-id="db8d5-362">EventLog</span></span>
* <span data-ttu-id="db8d5-363">TraceSource</span><span class="sxs-lookup"><span data-stu-id="db8d5-363">TraceSource</span></span>
* <span data-ttu-id="db8d5-364">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="db8d5-364">AzureAppServicesFile</span></span>
* <span data-ttu-id="db8d5-365">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="db8d5-365">AzureAppServicesBlob</span></span>
* <span data-ttu-id="db8d5-366">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="db8d5-366">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="db8d5-367">Domyślny poziom minimalny</span><span class="sxs-lookup"><span data-stu-id="db8d5-367">Default minimum level</span></span>

<span data-ttu-id="db8d5-368">Istnieje ustawienie minimalnego poziomu, które działa tylko wtedy, gdy nie mają zastosowania żadne reguły z konfiguracji lub kodu dla danego dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="db8d5-368">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="db8d5-369">Poniższy przykład pokazuje, jak ustawić poziom minimalny:</span><span class="sxs-lookup"><span data-stu-id="db8d5-369">The following example shows how to set the minimum level:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

<span data-ttu-id="db8d5-370">Jeśli poziom minimalny nie został jawnie ustawiony, wartość domyślna to `Information`, co oznacza, że `Trace` i `Debug` dzienników są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="db8d5-370">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="db8d5-371">Funkcje filtrowania</span><span class="sxs-lookup"><span data-stu-id="db8d5-371">Filter functions</span></span>

<span data-ttu-id="db8d5-372">Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorii, które nie mają przypisanych do nich reguł przez konfigurację lub kod.</span><span class="sxs-lookup"><span data-stu-id="db8d5-372">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="db8d5-373">Kod w funkcji ma dostęp do typu dostawcy, kategorii i poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="db8d5-373">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="db8d5-374">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="db8d5-374">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="db8d5-375">Kategorie i poziomy systemu</span><span class="sxs-lookup"><span data-stu-id="db8d5-375">System categories and levels</span></span>

<span data-ttu-id="db8d5-376">Poniżej przedstawiono niektóre kategorie używane przez ASP.NET Core i Entity Framework Core, z informacjami o dziennikach, od których należy się spodziewać:</span><span class="sxs-lookup"><span data-stu-id="db8d5-376">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="db8d5-377">Category</span><span class="sxs-lookup"><span data-stu-id="db8d5-377">Category</span></span>                            | <span data-ttu-id="db8d5-378">Uwagi</span><span class="sxs-lookup"><span data-stu-id="db8d5-378">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="db8d5-379">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="db8d5-379">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="db8d5-380">Ogólna Diagnostyka ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db8d5-380">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="db8d5-381">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="db8d5-381">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="db8d5-382">Które klucze zostały wzięte pod uwagę, znaleziono i użyte.</span><span class="sxs-lookup"><span data-stu-id="db8d5-382">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="db8d5-383">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="db8d5-383">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="db8d5-384">Dozwolone hosty.</span><span class="sxs-lookup"><span data-stu-id="db8d5-384">Hosts allowed.</span></span> |
| <span data-ttu-id="db8d5-385">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="db8d5-385">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="db8d5-386">Jak długo trwa wykonywanie żądań HTTP i czas ich uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="db8d5-386">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="db8d5-387">Które hostowanie zestawów uruchamiania zostało załadowane.</span><span class="sxs-lookup"><span data-stu-id="db8d5-387">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="db8d5-388">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="db8d5-388">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="db8d5-389">Diagnostyka MVC i Razor.</span><span class="sxs-lookup"><span data-stu-id="db8d5-389">MVC and Razor diagnostics.</span></span> <span data-ttu-id="db8d5-390">Powiązanie modelu, wykonywanie filtru, kompilacja widoku, wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-390">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="db8d5-391">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="db8d5-391">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="db8d5-392">Informacje o trasie.</span><span class="sxs-lookup"><span data-stu-id="db8d5-392">Route matching information.</span></span> |
| <span data-ttu-id="db8d5-393">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="db8d5-393">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="db8d5-394">Reagowanie na uruchamianie, zatrzymywanie i utrzymywanie aktywności.</span><span class="sxs-lookup"><span data-stu-id="db8d5-394">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="db8d5-395">Informacje o certyfikacie HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db8d5-395">HTTPS certificate information.</span></span> |
| <span data-ttu-id="db8d5-396">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="db8d5-396">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="db8d5-397">Obsługiwane pliki.</span><span class="sxs-lookup"><span data-stu-id="db8d5-397">Files served.</span></span> |
| <span data-ttu-id="db8d5-398">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="db8d5-398">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="db8d5-399">Ogólna Diagnostyka Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="db8d5-399">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="db8d5-400">Aktywność i Konfiguracja bazy danych, wykrywanie zmian, migracje.</span><span class="sxs-lookup"><span data-stu-id="db8d5-400">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="db8d5-401">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="db8d5-401">Log scopes</span></span>

 <span data-ttu-id="db8d5-402">*Zakres* może grupować zestaw operacji logicznych.</span><span class="sxs-lookup"><span data-stu-id="db8d5-402">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="db8d5-403">Takie grupowanie może służyć do dołączania tych samych danych do każdego dziennika, który został utworzony jako część zestawu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-403">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="db8d5-404">Na przykład każdy dziennik utworzony w ramach przetwarzania transakcji może zawierać identyfikator transakcji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-404">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="db8d5-405">Zakres jest typem `IDisposable`, który jest zwracany przez metodę <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> i obowiązuje do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="db8d5-405">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="db8d5-406">Użyj zakresu przez Zawijanie wywołań rejestratora w bloku `using`:</span><span class="sxs-lookup"><span data-stu-id="db8d5-406">Use a scope by wrapping logger calls in a `using` block:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

<span data-ttu-id="db8d5-407">Poniższy kod włącza zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="db8d5-407">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="db8d5-408">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="db8d5-408">*Program.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="db8d5-409">Skonfigurowanie opcji rejestratora konsoli `IncludeScopes` jest wymagane do włączenia rejestrowania na podstawie zakresu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-409">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="db8d5-410">Informacje o konfiguracji znajdują się w sekcji [Konfiguracja](#configuration) .</span><span class="sxs-lookup"><span data-stu-id="db8d5-410">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="db8d5-411">Każdy komunikat dziennika zawiera informacje o zakresie:</span><span class="sxs-lookup"><span data-stu-id="db8d5-411">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="db8d5-412">Wbudowani dostawcy rejestrowania</span><span class="sxs-lookup"><span data-stu-id="db8d5-412">Built-in logging providers</span></span>

<span data-ttu-id="db8d5-413">ASP.NET Core dostarcza następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="db8d5-413">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="db8d5-414">Console</span><span class="sxs-lookup"><span data-stu-id="db8d5-414">Console</span></span>](#console-provider)
* [<span data-ttu-id="db8d5-415">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="db8d5-415">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="db8d5-416">EventSource</span><span class="sxs-lookup"><span data-stu-id="db8d5-416">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="db8d5-417">EventLog</span><span class="sxs-lookup"><span data-stu-id="db8d5-417">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="db8d5-418">TraceSource</span><span class="sxs-lookup"><span data-stu-id="db8d5-418">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="db8d5-419">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="db8d5-419">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="db8d5-420">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="db8d5-420">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="db8d5-421">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="db8d5-421">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="db8d5-422">Aby uzyskać informacje na temat strumienia stdout i rejestrowania debugowania za pomocą modułu ASP.NET Core, zobacz <xref:test/troubleshoot-azure-iis> i <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="db8d5-422">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="db8d5-423">Dostawca konsoli</span><span class="sxs-lookup"><span data-stu-id="db8d5-423">Console provider</span></span>

<span data-ttu-id="db8d5-424">Pakiet [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) Provider wysyła dane wyjściowe dziennika do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-424">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="db8d5-425">Aby wyświetlić dane wyjściowe rejestrowania konsoli, Otwórz wiersz polecenia w folderze projektu i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="db8d5-425">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="db8d5-426">Dostawca debugowania</span><span class="sxs-lookup"><span data-stu-id="db8d5-426">Debug provider</span></span>

<span data-ttu-id="db8d5-427">Pakiet [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) Provider zapisuje dane wyjściowe dziennika przy użyciu klasy [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug) (wywołania metody `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="db8d5-427">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="db8d5-428">W systemie Linux ten dostawca zapisuje dzienniki do */var/log/Message*.</span><span class="sxs-lookup"><span data-stu-id="db8d5-428">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="eventsource-provider"></a><span data-ttu-id="db8d5-429">Dostawca EventSource</span><span class="sxs-lookup"><span data-stu-id="db8d5-429">EventSource provider</span></span>

<span data-ttu-id="db8d5-430">W przypadku aplikacji przeznaczonych do ASP.NET Core 1.1.0 lub nowszych pakiet dostawcy [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) może zaimplementować śledzenie zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="db8d5-430">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="db8d5-431">W systemie Windows używa [funkcji ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="db8d5-431">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="db8d5-432">Dostawca jest międzyplatformowy, ale nie ma jeszcze kolekcji zdarzeń i narzędzi do wyświetlania dla systemu Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="db8d5-432">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="db8d5-433">Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [Narzędzia Narzędzia PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="db8d5-433">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="db8d5-434">Istnieją inne narzędzia do wyświetlania dzienników ETW, ale narzędzia PerfView zapewnia najlepsze środowisko pracy z zdarzeniami ETW emitowanymi przez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db8d5-434">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="db8d5-435">Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, należy dodać ciąg `*Microsoft-Extensions-Logging` do listy **dodatkowych dostawców** .</span><span class="sxs-lookup"><span data-stu-id="db8d5-435">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="db8d5-436">(Nie przegap gwiazdki na początku ciągu znaków).</span><span class="sxs-lookup"><span data-stu-id="db8d5-436">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia PerfView dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="db8d5-438">Dostawca dziennika zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="db8d5-438">Windows EventLog provider</span></span>

<span data-ttu-id="db8d5-439">Pakiet dostawcy [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) wysyła dane wyjściowe dziennika do dziennika zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="db8d5-439">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="db8d5-440">[Przeciążenia addeventlog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) umożliwiają przekazywanie <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span><span class="sxs-lookup"><span data-stu-id="db8d5-440">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span>

### <a name="tracesource-provider"></a><span data-ttu-id="db8d5-441">Dostawca TraceSource</span><span class="sxs-lookup"><span data-stu-id="db8d5-441">TraceSource provider</span></span>

<span data-ttu-id="db8d5-442">Pakiet dostawcy [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa bibliotek i dostawców <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="db8d5-442">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="db8d5-443">[Przeciążenia AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) umożliwiają przekazywanie danych w przełączniku źródłowym i odbiorniku śledzenia.</span><span class="sxs-lookup"><span data-stu-id="db8d5-443">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="db8d5-444">Aby można było korzystać z tego dostawcy, aplikacja musi działać na .NET Framework (a nie na platformie .NET Core).</span><span class="sxs-lookup"><span data-stu-id="db8d5-444">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="db8d5-445">Dostawca może kierować komunikaty do różnych [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takich jak <xref:System.Diagnostics.TextWriterTraceListener> używany w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-445">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="db8d5-446">Dostawca Azure App Service</span><span class="sxs-lookup"><span data-stu-id="db8d5-446">Azure App Service provider</span></span>

<span data-ttu-id="db8d5-447">Pakiet [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) Provider zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji Azure App Service i w usłudze [BLOB Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="db8d5-447">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="db8d5-448">Pakiet dostawcy nie jest uwzględniony w udostępnionej infrastrukturze.</span><span class="sxs-lookup"><span data-stu-id="db8d5-448">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="db8d5-449">Aby użyć dostawcy, Dodaj pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-449">To use the provider, add the provider package to the project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="db8d5-450">Pakiet dostawcy nie jest uwzględniony w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="db8d5-450">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="db8d5-451">Podczas określania wartości docelowej .NET Framework lub odwoływania się do pakietu `Microsoft.AspNetCore.App` należy dodać pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-451">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="db8d5-452">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="db8d5-452">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="db8d5-453">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="db8d5-453">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="db8d5-454">Przeciążenie <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> umożliwia przekazanie <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="db8d5-454">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="db8d5-455">Obiekt Settings może przesłonić ustawienia domyślne, takie jak szablon danych wyjściowych rejestrowania, nazwa obiektu BLOB i limit rozmiaru pliku.</span><span class="sxs-lookup"><span data-stu-id="db8d5-455">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="db8d5-456">(*Szablon danych wyjściowych* to szablon wiadomości, który jest stosowany do wszystkich dzienników oprócz tego, co jest dostępne w wywołaniu metody `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="db8d5-456">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

<span data-ttu-id="db8d5-457">Po wdrożeniu w aplikacji App Service, aplikacja będzie przestrzegać ustawień w sekcji [dzienniki App Service](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) na stronie **App Service** Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="db8d5-457">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="db8d5-458">Po zaktualizowaniu następujących ustawień zmiany zaczynają obowiązywać natychmiast, bez konieczności ponownego uruchomienia lub ponownej wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-458">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="db8d5-459">**Rejestrowanie aplikacji (system plików)**</span><span class="sxs-lookup"><span data-stu-id="db8d5-459">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="db8d5-460">**Rejestrowanie aplikacji (BLOB)**</span><span class="sxs-lookup"><span data-stu-id="db8d5-460">**Application Logging (Blob)**</span></span>

<span data-ttu-id="db8d5-461">Domyślną lokalizacją plików dziennika jest folder *D: \\home @ no__t-2LogFiles @ no__t-3Application* , a domyślna nazwa pliku to *Diagnostics-YYYYMMDD. txt*.</span><span class="sxs-lookup"><span data-stu-id="db8d5-461">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="db8d5-462">Domyślny limit rozmiaru pliku wynosi 10 MB, a domyślna maksymalna liczba zachowywanych plików to 2.</span><span class="sxs-lookup"><span data-stu-id="db8d5-462">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="db8d5-463">Domyślną nazwą obiektu BLOB jest *{App-Name} {timestamp}/yyyy/mm/dd/hh/{GUID}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="db8d5-463">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="db8d5-464">Dostawca działa tylko wtedy, gdy projekt jest uruchomiony w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="db8d5-464">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="db8d5-465">Nie ma ono wpływu, gdy projekt jest uruchamiany lokalnie @ no__t-0it nie zapisuje do plików lokalnych lub lokalnego magazynu tworzenia dla obiektów BLOB.</span><span class="sxs-lookup"><span data-stu-id="db8d5-465">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="db8d5-466">Przesyłanie strumieniowe dzienników Azure</span><span class="sxs-lookup"><span data-stu-id="db8d5-466">Azure log streaming</span></span>

<span data-ttu-id="db8d5-467">Usługa przesyłania strumieniowego w usłudze Azure log umożliwia wyświetlanie dziennika aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="db8d5-467">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="db8d5-468">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="db8d5-468">The app server</span></span>
* <span data-ttu-id="db8d5-469">Serwer sieci Web</span><span class="sxs-lookup"><span data-stu-id="db8d5-469">The web server</span></span>
* <span data-ttu-id="db8d5-470">Śledzenie nieudanych żądań</span><span class="sxs-lookup"><span data-stu-id="db8d5-470">Failed request tracing</span></span>

<span data-ttu-id="db8d5-471">Aby skonfigurować przesyłanie strumieniowe dzienników Azure:</span><span class="sxs-lookup"><span data-stu-id="db8d5-471">To configure Azure log streaming:</span></span>

* <span data-ttu-id="db8d5-472">Przejdź do strony **dzienników App Service** ze strony portalu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-472">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="db8d5-473">Ustaw **Rejestrowanie aplikacji (system plików)** na **włączone**.</span><span class="sxs-lookup"><span data-stu-id="db8d5-473">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="db8d5-474">Wybierz **poziom**dziennika.</span><span class="sxs-lookup"><span data-stu-id="db8d5-474">Choose the log **Level**.</span></span>

<span data-ttu-id="db8d5-475">Przejdź do strony **strumień dziennika** , aby wyświetlić komunikaty aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8d5-475">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="db8d5-476">Są one rejestrowane przez aplikację za pomocą interfejsu `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="db8d5-476">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="db8d5-477">Rejestrowanie śledzenia Application Insights platformy Azure</span><span class="sxs-lookup"><span data-stu-id="db8d5-477">Azure Application Insights trace logging</span></span>

<span data-ttu-id="db8d5-478">Pakiet [Microsoft. Extensions. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) Provider zapisuje dzienniki na platformie Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="db8d5-478">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="db8d5-479">Application Insights to usługa, która monitoruje aplikację internetową i udostępnia narzędzia do wykonywania zapytań i analizowania danych telemetrycznych.</span><span class="sxs-lookup"><span data-stu-id="db8d5-479">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="db8d5-480">Jeśli używasz tego dostawcy, możesz wysyłać zapytania i analizować dzienniki przy użyciu narzędzi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="db8d5-480">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="db8d5-481">Dostawca rejestrowania jest dołączony jako zależność [Microsoft. ApplicationInsights. AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), który jest pakietem, który zapewnia wszystkie dostępne dane telemetryczne dla ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db8d5-481">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="db8d5-482">Jeśli używasz tego pakietu, nie musisz instalować pakietu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="db8d5-482">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="db8d5-483">Nie używaj pakietu [Microsoft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) Package @ no__t-1that's for ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="db8d5-483">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="db8d5-484">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="db8d5-484">For more information, see the following resources:</span></span>

* [<span data-ttu-id="db8d5-485">Przegląd Application Insights</span><span class="sxs-lookup"><span data-stu-id="db8d5-485">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="db8d5-486">[Application Insights dla ASP.NET Core aplikacji](/azure/azure-monitor/app/asp-net-core) — Zacznij tutaj, jeśli chcesz zaimplementować cały zakres Application Insights telemetrii wraz z rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="db8d5-486">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="db8d5-487">[ApplicationInsightsLoggerProvider for .NET Core ILogger](/azure/azure-monitor/app/ilogger) — Rozpocznij tutaj, jeśli chcesz zaimplementować dostawcę rejestrowania bez pozostałej części telemetrii Application Insights.</span><span class="sxs-lookup"><span data-stu-id="db8d5-487">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="db8d5-488">[Karty rejestrowania Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="db8d5-488">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="db8d5-489">[Zainstaluj, skonfiguruj i zainicjuj samouczek Application INSIGHTS SDK](/learn/modules/instrument-web-app-code-with-application-insights) — interaktywny w witrynie Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="db8d5-489">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="db8d5-490">Dostawcy rejestrowania innych firm</span><span class="sxs-lookup"><span data-stu-id="db8d5-490">Third-party logging providers</span></span>

<span data-ttu-id="db8d5-491">Platformy rejestrowania innych firm, które współpracują z ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="db8d5-491">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="db8d5-492">[ELMAH.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="db8d5-492">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="db8d5-493">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="db8d5-493">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="db8d5-494">[JSNLog](https://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="db8d5-494">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="db8d5-495">[KissLog.NET](https://kisslog.net/) ([repozytorium GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="db8d5-495">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="db8d5-496">[Log4Net](https://logging.apache.org/log4net/) ([repozytorium GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="db8d5-496">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="db8d5-497">[Loggr](https://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="db8d5-497">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="db8d5-498">[NLOG](https://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="db8d5-498">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="db8d5-499">[Sentry](https://sentry.io/welcome/) ([repozytorium GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="db8d5-499">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="db8d5-500">[Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="db8d5-500">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="db8d5-501">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repozytorium GitHub](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="db8d5-501">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="db8d5-502">Niektóre struktury innych firm mogą wykonywać [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="db8d5-502">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="db8d5-503">Korzystanie z struktury innej firmy jest podobne do korzystania z jednego z wbudowanych dostawców:</span><span class="sxs-lookup"><span data-stu-id="db8d5-503">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="db8d5-504">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="db8d5-504">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="db8d5-505">Wywoływanie metody rozszerzenia `ILoggerFactory` dostarczonej przez strukturę rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="db8d5-505">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="db8d5-506">Aby uzyskać więcej informacji, zobacz dokumentację każdego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="db8d5-506">For more information, see each provider's documentation.</span></span> <span data-ttu-id="db8d5-507">Dostawcy rejestrowania innych firm nie są obsługiwani przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="db8d5-507">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db8d5-508">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="db8d5-508">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
