---
title: Rejestrowanie w programie .NET Core i ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać struktury rejestrowania dostarczonej przez pakiet NuGet Microsoft. Extensions. Logging.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/13/2019
uid: fundamentals/logging/index
ms.openlocfilehash: eda5c9c0372e47f5670cf097b5db80ec227bcb47
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115955"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="0892e-103">Rejestrowanie w programie .NET Core i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0892e-103">Logging in .NET Core and ASP.NET Core</span></span>

<span data-ttu-id="0892e-104">Autorzy [Dykstra](https://github.com/tdykstra) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0892e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0892e-105">Platforma .NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm.</span><span class="sxs-lookup"><span data-stu-id="0892e-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="0892e-106">W tym artykule pokazano, jak używać interfejsu API rejestrowania z wbudowanymi dostawcami.</span><span class="sxs-lookup"><span data-stu-id="0892e-106">This article shows how to use the logging API with built-in providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0892e-107">Większość przykładów kodu pokazanych w tym artykule pochodzą z ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="0892e-108">Części tych fragmentów kodu dotyczące rejestrowania mają zastosowanie do dowolnej aplikacji .NET Core, która korzysta z [hosta ogólnego](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="0892e-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="0892e-109">Aby uzyskać informacje na temat korzystania z hosta generycznego w aplikacjach konsolowych innych niż sieci Web, zobacz [usługi hostowane](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="0892e-109">For information about how to use the Generic Host in non-web console apps, see [Hosted services](xref:fundamentals/host/hosted-services).</span></span>

<span data-ttu-id="0892e-110">Rejestrowanie kodu dla aplikacji bez hosta ogólnego różni się w sposób, w jaki [są dodawane dostawcy](#add-providers) i [są tworzone rejestratory](#create-logs).</span><span class="sxs-lookup"><span data-stu-id="0892e-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="0892e-111">Przykłady kodu niehosta są wyświetlane w tych częściach artykułu.</span><span class="sxs-lookup"><span data-stu-id="0892e-111">Non-host code examples are shown in those sections of the article.</span></span>

::: moniker-end

<span data-ttu-id="0892e-112">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0892e-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="0892e-113">Dodaj dostawców</span><span class="sxs-lookup"><span data-stu-id="0892e-113">Add providers</span></span>

<span data-ttu-id="0892e-114">Dostawca rejestrowania wyświetla lub przechowuje dzienniki.</span><span class="sxs-lookup"><span data-stu-id="0892e-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="0892e-115">Na przykład dostawca konsoli wyświetla dzienniki w konsoli programu, a Dostawca usługi Azure Application Insights przechowuje je na platformie Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0892e-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="0892e-116">Dzienniki mogą być wysyłane do wielu miejsc docelowych przez dodanie wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="0892e-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0892e-117">Aby dodać dostawcę w aplikacji korzystającej z hosta ogólnego, wywołaj metodę rozszerzenia `Add{provider name}` dostawcy w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0892e-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="0892e-118">W aplikacji konsolowej bez hosta Wywołaj metodę rozszerzenia `Add{provider name}` dostawcy podczas tworzenia `LoggerFactory`:</span><span class="sxs-lookup"><span data-stu-id="0892e-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="0892e-119">`LoggerFactory` i `AddConsole` wymagają instrukcji `using` dla `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="0892e-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="0892e-120">Domyślne ASP.NET Core wywołań szablonów projektu <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, które dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="0892e-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="0892e-121">Konsola</span><span class="sxs-lookup"><span data-stu-id="0892e-121">Console</span></span>
* <span data-ttu-id="0892e-122">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0892e-122">Debug</span></span>
* <span data-ttu-id="0892e-123">EventSource</span><span class="sxs-lookup"><span data-stu-id="0892e-123">EventSource</span></span>
* <span data-ttu-id="0892e-124">EventLog (tylko w przypadku uruchamiania w systemie Windows)</span><span class="sxs-lookup"><span data-stu-id="0892e-124">EventLog (only when running on Windows)</span></span>

<span data-ttu-id="0892e-125">Dostawców domyślnych można zastąpić własnymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="0892e-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="0892e-126">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>i Dodaj żądanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="0892e-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0 "

<span data-ttu-id="0892e-127">Aby dodać dostawcę, wywołaj metodę rozszerzenia `Add{provider name}` dostawcy w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0892e-127">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="0892e-128">Poprzedzający kod wymaga odwołań do `Microsoft.Extensions.Logging` i `Microsoft.Extensions.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="0892e-128">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="0892e-129">Domyślny szablon projektu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, który dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="0892e-129">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="0892e-130">Konsola</span><span class="sxs-lookup"><span data-stu-id="0892e-130">Console</span></span>
* <span data-ttu-id="0892e-131">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0892e-131">Debug</span></span>
* <span data-ttu-id="0892e-132">EventSource (rozpoczęcie w ASP.NET Core 2,2)</span><span class="sxs-lookup"><span data-stu-id="0892e-132">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="0892e-133">Jeśli używasz `CreateDefaultBuilder`, możesz zastąpić domyślnych dostawców własnymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="0892e-133">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="0892e-134">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>i Dodaj żądanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="0892e-134">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

<span data-ttu-id="0892e-135">Więcej informacji na temat [wbudowanych dostawców rejestrowania](#built-in-logging-providers) i [dostawców rejestrowania innych](#third-party-logging-providers) firm znajduje się w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="0892e-135">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="0892e-136">Tworzenie dzienników</span><span class="sxs-lookup"><span data-stu-id="0892e-136">Create logs</span></span>

<span data-ttu-id="0892e-137">Aby utworzyć dzienniki, użyj obiektu <xref:Microsoft.Extensions.Logging.ILogger%601>.</span><span class="sxs-lookup"><span data-stu-id="0892e-137">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="0892e-138">W aplikacji sieci Web lub hostowanej usłudze Pobierz `ILogger` z iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="0892e-138">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="0892e-139">W aplikacjach konsolowych innych niż host Użyj `LoggerFactory`, aby utworzyć `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="0892e-139">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="0892e-140">Poniższy ASP.NET Core przykład tworzy Rejestrator z `TodoApiSample.Pages.AboutModel` jako kategorii.</span><span class="sxs-lookup"><span data-stu-id="0892e-140">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="0892e-141">*Kategoria* dziennika jest ciągiem, który jest skojarzony z każdym dziennikiem.</span><span class="sxs-lookup"><span data-stu-id="0892e-141">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="0892e-142">Wystąpienie `ILogger<T>` zapewniane przez program DI tworzy dzienniki, które mają w pełni kwalifikowaną nazwę typu `T` jako kategorię.</span><span class="sxs-lookup"><span data-stu-id="0892e-142">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="0892e-143">Poniższy przykład aplikacji nieprzyjmującej konsoli tworzy Rejestrator z `LoggingConsoleApp.Program` jako kategorii.</span><span class="sxs-lookup"><span data-stu-id="0892e-143">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

::: moniker-end

<span data-ttu-id="0892e-144">W poniższych przykładach ASP.NET Core i aplikacji konsoli Rejestrator jest używany do tworzenia dzienników z `Information` jako poziom.</span><span class="sxs-lookup"><span data-stu-id="0892e-144">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="0892e-145">*Poziom* dziennika wskazuje ważność rejestrowanego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="0892e-145">The Log *level* indicates the severity of the logged event.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

<span data-ttu-id="0892e-146">[Poziomy](#log-level) i [Kategorie](#log-category) zostały wyjaśnione bardziej szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="0892e-146">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-3.0"

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="0892e-147">Tworzenie dzienników w klasie programu</span><span class="sxs-lookup"><span data-stu-id="0892e-147">Create logs in the Program class</span></span>

<span data-ttu-id="0892e-148">Aby napisać dzienniki w klasie `Program` aplikacji ASP.NET Core, Pobierz wystąpienie `ILogger` od DI po skompilowaniu hosta:</span><span class="sxs-lookup"><span data-stu-id="0892e-148">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="0892e-149">Rejestrowanie podczas konstruowania hosta nie jest bezpośrednio obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="0892e-149">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="0892e-150">Można jednak użyć osobnego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="0892e-150">However, a separate logger can be used.</span></span> <span data-ttu-id="0892e-151">W poniższym przykładzie Rejestrator [Serilog](https://serilog.net/) jest używany do logowania się `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0892e-151">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateHostBuilder`.</span></span> <span data-ttu-id="0892e-152">`AddSerilog` używa konfiguracji statycznej określonej w `Log.Logger`:</span><span class="sxs-lookup"><span data-stu-id="0892e-152">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return Host.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddRazorPages();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {   
                    logging.AddSerilog();
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="0892e-153">Tworzenie dzienników w klasie startowej</span><span class="sxs-lookup"><span data-stu-id="0892e-153">Create logs in the Startup class</span></span>

<span data-ttu-id="0892e-154">Aby napisać dzienniki w metodzie `Startup.Configure` aplikacji ASP.NET Core, należy uwzględnić parametr `ILogger` w sygnaturze metody:</span><span class="sxs-lookup"><span data-stu-id="0892e-154">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="0892e-155">Zapisywanie dzienników przed ukończeniem instalacji przez DI kontenera w metodzie `Startup.ConfigureServices` nie jest obsługiwane:</span><span class="sxs-lookup"><span data-stu-id="0892e-155">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="0892e-156">Iniekcja rejestratora do konstruktora `Startup` nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="0892e-156">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="0892e-157">Wstrzykiwanie rejestratora do sygnatury metody `Startup.ConfigureServices` nie jest obsługiwane</span><span class="sxs-lookup"><span data-stu-id="0892e-157">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="0892e-158">Przyczyną tego ograniczenia jest to, że rejestrowanie jest zależne od typu i konfiguracji, która z tego powodu zależy od DI.</span><span class="sxs-lookup"><span data-stu-id="0892e-158">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="0892e-159">Kontener DI nie jest ustawiany do momentu zakończenia `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0892e-159">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="0892e-160">Wstrzykiwanie konstruktora rejestratora do `Startup` działa we wcześniejszych wersjach ASP.NET Core, ponieważ dla hosta sieci Web jest tworzony oddzielny kontener DI.</span><span class="sxs-lookup"><span data-stu-id="0892e-160">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="0892e-161">Aby uzyskać informacje o tym, dlaczego dla hosta generycznego jest tworzony tylko jeden kontener, zobacz [zawiadomienie o rozdzieleniu zmian](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="0892e-161">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="0892e-162">Jeśli trzeba skonfigurować usługę, która zależy od `ILogger<T>`, można to zrobić za pomocą iniekcji konstruktora lub dostarczając metodę fabryki.</span><span class="sxs-lookup"><span data-stu-id="0892e-162">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="0892e-163">Podejście metody fabryki jest zalecane tylko wtedy, gdy nie ma żadnych innych opcji.</span><span class="sxs-lookup"><span data-stu-id="0892e-163">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="0892e-164">Załóżmy na przykład, że musisz wypełnić Właściwość usługą z:</span><span class="sxs-lookup"><span data-stu-id="0892e-164">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="0892e-165">Poprzedni wyróżniony kod jest `Func`, który jest uruchamiany po raz pierwszy do utworzenia wystąpienia `MyService`.</span><span class="sxs-lookup"><span data-stu-id="0892e-165">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="0892e-166">W ten sposób można uzyskać dostęp do dowolnych zarejestrowanych usług.</span><span class="sxs-lookup"><span data-stu-id="0892e-166">You can access any of the registered services in this way.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="0892e-167">Tworzenie dzienników w programie startowym</span><span class="sxs-lookup"><span data-stu-id="0892e-167">Create logs in Startup</span></span>

<span data-ttu-id="0892e-168">Aby napisać dzienniki w klasie `Startup`, Dołącz parametr `ILogger` do sygnatury konstruktora:</span><span class="sxs-lookup"><span data-stu-id="0892e-168">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="0892e-169">Tworzenie dzienników w klasie programu</span><span class="sxs-lookup"><span data-stu-id="0892e-169">Create logs in the Program class</span></span>

<span data-ttu-id="0892e-170">Aby napisać dzienniki w klasie `Program`, Pobierz wystąpienie `ILogger` z DI:</span><span class="sxs-lookup"><span data-stu-id="0892e-170">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="0892e-171">Rejestrowanie podczas konstruowania hosta nie jest bezpośrednio obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="0892e-171">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="0892e-172">Można jednak użyć osobnego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="0892e-172">However, a separate logger can be used.</span></span> <span data-ttu-id="0892e-173">W poniższym przykładzie Rejestrator [Serilog](https://serilog.net/) jest używany do logowania się `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0892e-173">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateWebHostBuilder`.</span></span> <span data-ttu-id="0892e-174">`AddSerilog` używa konfiguracji statycznej określonej w `Log.Logger`:</span><span class="sxs-lookup"><span data-stu-id="0892e-174">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

```csharp
using System;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return WebHost.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddMvc();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {
                    logging.AddSerilog();
                })
                .UseStartup<Startup>();
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="0892e-175">Brak metod rejestratora asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="0892e-175">No asynchronous logger methods</span></span>

<span data-ttu-id="0892e-176">Rejestracja powinna być tak szybka, że nie jest to koszt wydajności kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="0892e-176">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="0892e-177">Jeśli magazyn danych rejestrowania jest wolny, nie zapisuj go bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="0892e-177">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="0892e-178">Najpierw Rozważ zapisanie komunikatów dziennika do szybkiego sklepu, a następnie przeniesienie ich do wolnego magazynu później.</span><span class="sxs-lookup"><span data-stu-id="0892e-178">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="0892e-179">Na przykład jeśli rejestrujesz się do SQL Server, nie chcesz tego robić bezpośrednio w metodzie `Log`, ponieważ metody `Log` są synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="0892e-179">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="0892e-180">Zamiast tego można synchronicznie dodawać komunikaty dziennika do kolejki w pamięci, a proces roboczy w tle ściągał komunikaty z kolejki, aby wykonać asynchroniczne działanie wypychania danych do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0892e-180">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="0892e-181">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="0892e-181">Configuration</span></span>

<span data-ttu-id="0892e-182">Konfiguracja dostawcy rejestrowania jest świadczona przez co najmniej jednego dostawcę konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0892e-182">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="0892e-183">Formaty plików (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="0892e-183">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="0892e-184">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0892e-184">Command-line arguments.</span></span>
* <span data-ttu-id="0892e-185">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="0892e-185">Environment variables.</span></span>
* <span data-ttu-id="0892e-186">Obiekty platformy .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="0892e-186">In-memory .NET objects.</span></span>
* <span data-ttu-id="0892e-187">Magazyn niezaszyfrowanego [klucza tajnego](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="0892e-187">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="0892e-188">Zaszyfrowany magazyn użytkowników, taki jak [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="0892e-188">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="0892e-189">Dostawcy niestandardowi (instalowani lub utworzony).</span><span class="sxs-lookup"><span data-stu-id="0892e-189">Custom providers (installed or created).</span></span>

<span data-ttu-id="0892e-190">Na przykład konfiguracja rejestrowania jest ogólnie dostępna w sekcji `Logging` plików ustawień aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-190">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="0892e-191">Poniższy przykład pokazuje zawartość typowej wartości *appSettings. Plik Development. JSON* :</span><span class="sxs-lookup"><span data-stu-id="0892e-191">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="0892e-192">Właściwość `Logging` może mieć `LogLevel` i właściwości dostawcy dzienników (wyświetlana jest konsola).</span><span class="sxs-lookup"><span data-stu-id="0892e-192">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="0892e-193">Właściwość `LogLevel` w obszarze `Logging` określa minimalny [poziom](#log-level) rejestrowania wybranych kategorii.</span><span class="sxs-lookup"><span data-stu-id="0892e-193">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="0892e-194">W przykładzie `System` i `Microsoft` kategorii dziennika na poziomie `Information`, a wszystkie inne będą logować się na poziomie `Debug`.</span><span class="sxs-lookup"><span data-stu-id="0892e-194">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="0892e-195">Inne właściwości w obszarze `Logging` określają dostawców rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="0892e-195">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="0892e-196">Przykład dotyczy dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="0892e-196">The example is for the Console provider.</span></span> <span data-ttu-id="0892e-197">Jeśli dostawca obsługuje [zakresy dzienników](#log-scopes), `IncludeScopes` wskazuje, czy są one włączone.</span><span class="sxs-lookup"><span data-stu-id="0892e-197">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="0892e-198">Właściwość dostawcy (taka jak `Console` w przykładzie) może także określić właściwość `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="0892e-198">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="0892e-199">`LogLevel` w ramach dostawcy Określa poziomy do rejestrowania dla tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="0892e-199">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="0892e-200">Jeśli w `Logging.{providername}.LogLevel`są określone poziomy, zastępują one wszystko ustawione w `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="0892e-200">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

<span data-ttu-id="0892e-201">Informacje o implementowaniu dostawców konfiguracji znajdują się w temacie <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0892e-201">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="0892e-202">Przykładowe dane wyjściowe rejestrowania</span><span class="sxs-lookup"><span data-stu-id="0892e-202">Sample logging output</span></span>

<span data-ttu-id="0892e-203">W przypadku przykładowego kodu podanego w poprzedniej sekcji Dzienniki są wyświetlane w konsoli programu, gdy aplikacja jest uruchamiana z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0892e-203">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="0892e-204">Oto przykład danych wyjściowych konsoli:</span><span class="sxs-lookup"><span data-stu-id="0892e-204">Here's an example of console output:</span></span>

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

<span data-ttu-id="0892e-205">Poprzednie dzienniki zostały wygenerowane przez utworzenie żądania HTTP GET do przykładowej aplikacji w `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="0892e-205">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="0892e-206">Oto przykład tych samych dzienników, które są wyświetlane w oknie debugowania podczas uruchamiania przykładowej aplikacji w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0892e-206">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

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

<span data-ttu-id="0892e-207">Dzienniki utworzone za pomocą wywołań `ILogger` przedstawionych w poprzedniej sekcji zaczynają się od "TodoApiSample".</span><span class="sxs-lookup"><span data-stu-id="0892e-207">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="0892e-208">Dzienniki zaczynające się od kategorii "Microsoft" pochodzą z kodu ASP.NET Core Framework.</span><span class="sxs-lookup"><span data-stu-id="0892e-208">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="0892e-209">ASP.NET Core i kod aplikacji używają tego samego rejestrowania interfejsu API i dostawców.</span><span class="sxs-lookup"><span data-stu-id="0892e-209">ASP.NET Core and application code are using the same logging API and providers.</span></span>

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

<span data-ttu-id="0892e-210">Dzienniki utworzone za pomocą wywołań `ILogger` przedstawionych w poprzedniej sekcji zaczynają się od "TodoApi".</span><span class="sxs-lookup"><span data-stu-id="0892e-210">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="0892e-211">Dzienniki zaczynające się od kategorii "Microsoft" pochodzą z kodu ASP.NET Core Framework.</span><span class="sxs-lookup"><span data-stu-id="0892e-211">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="0892e-212">ASP.NET Core i kod aplikacji używają tego samego rejestrowania interfejsu API i dostawców.</span><span class="sxs-lookup"><span data-stu-id="0892e-212">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

<span data-ttu-id="0892e-213">W pozostałej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="0892e-213">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="0892e-214">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="0892e-214">NuGet packages</span></span>

<span data-ttu-id="0892e-215">Interfejsy `ILogger` i `ILoggerFactory` są w [Microsoft. Extensions. Logging. Abstracts](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)i domyślne implementacje dla nich są w [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="0892e-215">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="0892e-216">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="0892e-216">Log category</span></span>

<span data-ttu-id="0892e-217">Po utworzeniu obiektu `ILogger` określono dla niego *kategorię* .</span><span class="sxs-lookup"><span data-stu-id="0892e-217">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="0892e-218">Ta kategoria jest dołączona do każdego komunikatu dziennika utworzonego przez to wystąpienie `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="0892e-218">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="0892e-219">Kategoria może być dowolnym ciągiem, ale Konwencja ma używać nazwy klasy, takiej jak "TodoApi. controllers. TodoController".</span><span class="sxs-lookup"><span data-stu-id="0892e-219">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="0892e-220">Użyj `ILogger<T>`, aby pobrać wystąpienie `ILogger`, które używa w pełni kwalifikowanej nazwy typu `T` jako kategorii:</span><span class="sxs-lookup"><span data-stu-id="0892e-220">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="0892e-221">Aby jawnie określić kategorię, wywołaj `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="0892e-221">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="0892e-222">`ILogger<T>` jest równoznaczny z wywoływaniem `CreateLogger` z w pełni kwalifikowaną nazwą typu `T`.</span><span class="sxs-lookup"><span data-stu-id="0892e-222">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="0892e-223">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="0892e-223">Log level</span></span>

<span data-ttu-id="0892e-224">Każdy dziennik Określa wartość <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="0892e-224">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="0892e-225">Poziom dziennika wskazuje ważność lub ważność.</span><span class="sxs-lookup"><span data-stu-id="0892e-225">The log level indicates the severity or importance.</span></span> <span data-ttu-id="0892e-226">Przykładowo można napisać dziennik `Information`, gdy metoda zostanie zakończona normalnie, a dziennik `Warning`, gdy metoda zwraca *404 nie znaleziono* kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="0892e-226">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="0892e-227">Poniższy kod tworzy `Information` i `Warning` dzienników:</span><span class="sxs-lookup"><span data-stu-id="0892e-227">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="0892e-228">W powyższym kodzie pierwszy parametr jest [identyfikatorem zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="0892e-228">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="0892e-229">Drugi parametr jest szablonem wiadomości z symbolami zastępczymi dla wartości argumentów dostarczonych przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="0892e-229">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="0892e-230">Parametry metody zostały wyjaśnione w [sekcji szablon komunikatu](#log-message-template) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="0892e-230">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="0892e-231">Metody rejestrowania, które obejmują poziom w nazwie metody (na przykład `LogInformation` i `LogWarning`) są [metodami rozszerzenia dla ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="0892e-231">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="0892e-232">Te metody wywołują metodę `Log`, która przyjmuje parametr `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="0892e-232">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="0892e-233">Metodę `Log` można wywołać bezpośrednio zamiast jednej z tych metod rozszerzających, ale składnia jest stosunkowo skomplikowana.</span><span class="sxs-lookup"><span data-stu-id="0892e-233">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="0892e-234">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.Extensions.Logging.ILogger> i [kod źródłowy rozszerzeń rejestratora](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="0892e-234">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="0892e-235">ASP.NET Core definiuje następujące poziomy dziennika uporządkowane w tym miejscu od najniższej do najwyższej wagi.</span><span class="sxs-lookup"><span data-stu-id="0892e-235">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="0892e-236">Ślad = 0</span><span class="sxs-lookup"><span data-stu-id="0892e-236">Trace = 0</span></span>

  <span data-ttu-id="0892e-237">Aby uzyskać informacje, które są zazwyczaj cenne tylko dla debugowania.</span><span class="sxs-lookup"><span data-stu-id="0892e-237">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="0892e-238">Komunikaty te mogą zawierać poufne dane aplikacji, dlatego nie powinny być włączone w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0892e-238">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="0892e-239">*Domyślnie wyłączona.*</span><span class="sxs-lookup"><span data-stu-id="0892e-239">*Disabled by default.*</span></span>

* <span data-ttu-id="0892e-240">Debuguj = 1</span><span class="sxs-lookup"><span data-stu-id="0892e-240">Debug = 1</span></span>

  <span data-ttu-id="0892e-241">Informacje, które mogą być przydatne podczas tworzenia i debugowania.</span><span class="sxs-lookup"><span data-stu-id="0892e-241">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="0892e-242">Przykład: `Entering method Configure with flag set to true.` Włącz dzienniki poziomu `Debug` w środowisku produkcyjnym tylko w przypadku rozwiązywania problemów, ze względu na dużą ilość dzienników.</span><span class="sxs-lookup"><span data-stu-id="0892e-242">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="0892e-243">Informacje = 2</span><span class="sxs-lookup"><span data-stu-id="0892e-243">Information = 2</span></span>

  <span data-ttu-id="0892e-244">Do śledzenia ogólnego przepływu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-244">For tracking the general flow of the app.</span></span> <span data-ttu-id="0892e-245">Te dzienniki zwykle mają pewną wartość długoterminową.</span><span class="sxs-lookup"><span data-stu-id="0892e-245">These logs typically have some long-term value.</span></span> <span data-ttu-id="0892e-246">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="0892e-246">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="0892e-247">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="0892e-247">Warning = 3</span></span>

  <span data-ttu-id="0892e-248">Dla nietypowych lub nieoczekiwanych zdarzeń w przepływie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-248">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="0892e-249">Mogą to być błędy lub inne warunki, które nie powodują zatrzymania aplikacji, ale konieczne może być zbadanie.</span><span class="sxs-lookup"><span data-stu-id="0892e-249">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="0892e-250">Obsłużone wyjątki są typowym miejscem do korzystania z `Warning` poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="0892e-250">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="0892e-251">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="0892e-251">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="0892e-252">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="0892e-252">Error = 4</span></span>

  <span data-ttu-id="0892e-253">W przypadku błędów i wyjątków, których nie można obsłużyć.</span><span class="sxs-lookup"><span data-stu-id="0892e-253">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="0892e-254">Te komunikaty wskazują niepowodzenie w bieżącym działaniu lub operacji (np. bieżące żądanie HTTP), a nie awaria całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-254">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="0892e-255">Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="0892e-255">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="0892e-256">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="0892e-256">Critical = 5</span></span>

  <span data-ttu-id="0892e-257">Dla niepowodzeń, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="0892e-257">For failures that require immediate attention.</span></span> <span data-ttu-id="0892e-258">Przykłady: scenariusze utraty danych, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="0892e-258">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="0892e-259">Poziom dziennika służy do kontrolowania, ile danych wyjściowych dziennika jest zapisywana w określonym nośniku lub oknie wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="0892e-259">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="0892e-260">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="0892e-260">For example:</span></span>

* <span data-ttu-id="0892e-261">W środowisku produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="0892e-261">In production:</span></span>
  * <span data-ttu-id="0892e-262">Rejestrowanie na `Trace` za pomocą poziomów `Information` powoduje utworzenie dużej ilości szczegółowych komunikatów dzienników.</span><span class="sxs-lookup"><span data-stu-id="0892e-262">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="0892e-263">Aby kontrolować koszty i nie przekraczać limitów magazynowania danych, należy rejestrować `Trace` za pomocą komunikatów na poziomie `Information` na potrzeby magazynu z dużymi ilościami danych.</span><span class="sxs-lookup"><span data-stu-id="0892e-263">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="0892e-264">Rejestrowanie w `Warning` przez poziomy `Critical` zazwyczaj generuje mniejszą liczbę mniejszych komunikatów dzienników.</span><span class="sxs-lookup"><span data-stu-id="0892e-264">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="0892e-265">W związku z tym koszty i limity magazynu zazwyczaj nie są problemem, co skutkuje większą elastycznością wyboru magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="0892e-265">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="0892e-266">Podczas tworzenia:</span><span class="sxs-lookup"><span data-stu-id="0892e-266">During development:</span></span>
  * <span data-ttu-id="0892e-267">Rejestruj `Warning` za pomocą `Critical` komunikatów w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="0892e-267">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="0892e-268">Podczas rozwiązywania problemów Dodaj `Trace` za poorednictwem `Information` komunikatów.</span><span class="sxs-lookup"><span data-stu-id="0892e-268">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="0892e-269">W sekcji [filtrowanie dzienników](#log-filtering) w dalszej części tego artykułu wyjaśniono, jak kontrolować poziomy dzienników obsługiwane przez dostawcę.</span><span class="sxs-lookup"><span data-stu-id="0892e-269">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="0892e-270">ASP.NET Core zapisuje dzienniki dla zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="0892e-270">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="0892e-271">Przykłady dzienników znajdujące się wcześniej w tym artykule nie wykluczają dzienników poniżej `Information` poziomu, dlatego nie zostały utworzone dzienniki poziomu `Debug` lub `Trace`.</span><span class="sxs-lookup"><span data-stu-id="0892e-271">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="0892e-272">Oto przykład dzienników konsoli utworzonych przez uruchomienie przykładowej aplikacji skonfigurowanej do wyświetlania dzienników `Debug`:</span><span class="sxs-lookup"><span data-stu-id="0892e-272">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="0892e-273">Identyfikator zdarzenia dziennika</span><span class="sxs-lookup"><span data-stu-id="0892e-273">Log event ID</span></span>

<span data-ttu-id="0892e-274">Każdy dziennik może określać *Identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="0892e-274">Each log can specify an *event ID*.</span></span> <span data-ttu-id="0892e-275">Aplikacja Przykładowa wykonuje to przy użyciu lokalnie zdefiniowanej klasy `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="0892e-275">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="0892e-276">Identyfikator zdarzenia kojarzy zestaw zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="0892e-276">An event ID associates a set of events.</span></span> <span data-ttu-id="0892e-277">Na przykład wszystkie dzienniki związane z wyświetlaniem listy elementów na stronie mogą być 1001.</span><span class="sxs-lookup"><span data-stu-id="0892e-277">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="0892e-278">Dostawca rejestrowania może przechowywać identyfikator zdarzenia w polu identyfikatora, w komunikacie rejestrowania lub wcale.</span><span class="sxs-lookup"><span data-stu-id="0892e-278">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="0892e-279">Dostawca debugowania nie pokazuje identyfikatorów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="0892e-279">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="0892e-280">Dostawca konsoli pokazuje identyfikatory zdarzeń w nawiasach po kategorii:</span><span class="sxs-lookup"><span data-stu-id="0892e-280">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="0892e-281">Szablon komunikatu dziennika</span><span class="sxs-lookup"><span data-stu-id="0892e-281">Log message template</span></span>

<span data-ttu-id="0892e-282">Każdy dziennik Określa szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="0892e-282">Each log specifies a message template.</span></span> <span data-ttu-id="0892e-283">Szablon wiadomości może zawierać symbole zastępcze, dla których podano argumenty.</span><span class="sxs-lookup"><span data-stu-id="0892e-283">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="0892e-284">Użyj nazw dla symboli zastępczych, a nie liczby.</span><span class="sxs-lookup"><span data-stu-id="0892e-284">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="0892e-285">Kolejność symboli zastępczych, nie ich nazw, określa, które parametry są używane do dostarczania ich wartości.</span><span class="sxs-lookup"><span data-stu-id="0892e-285">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="0892e-286">W poniższym kodzie Zwróć uwagę, że nazwy parametrów są poza kolejnością w szablonie wiadomości:</span><span class="sxs-lookup"><span data-stu-id="0892e-286">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="0892e-287">Ten kod tworzy komunikat dziennika z wartościami parametrów w kolejności:</span><span class="sxs-lookup"><span data-stu-id="0892e-287">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="0892e-288">Struktura rejestrowania działa w ten sposób, aby dostawcy rejestrowania mogli zaimplementować [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="0892e-288">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="0892e-289">Same argumenty są przesyłane do systemu rejestrowania, a nie tylko dla sformatowanego szablonu wiadomości.</span><span class="sxs-lookup"><span data-stu-id="0892e-289">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="0892e-290">Te informacje umożliwiają dostawcom rejestrowania przechowywanie wartości parametrów jako pól.</span><span class="sxs-lookup"><span data-stu-id="0892e-290">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="0892e-291">Na przykład załóżmy, że wywołania metod rejestratora wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="0892e-291">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="0892e-292">W przypadku wysyłania dzienników do usługi Azure Table Storage każda jednostka tabeli platformy Azure może mieć właściwości `ID` i `RequestTime`, które upraszczają zapytania dotyczące danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="0892e-292">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="0892e-293">Zapytanie może znaleźć wszystkie dzienniki w określonym zakresie `RequestTime` bez analizowania limitu czasu wiadomości tekstowej.</span><span class="sxs-lookup"><span data-stu-id="0892e-293">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="0892e-294">Wyjątki rejestrowania</span><span class="sxs-lookup"><span data-stu-id="0892e-294">Logging exceptions</span></span>

<span data-ttu-id="0892e-295">Metody rejestratora mają przeciążenia umożliwiające przekazanie wyjątku, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0892e-295">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="0892e-296">Różni dostawcy obsługują informacje o wyjątkach na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="0892e-296">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="0892e-297">Oto przykład danych wyjściowych dostawcy debugowania z kodu pokazanego powyżej.</span><span class="sxs-lookup"><span data-stu-id="0892e-297">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="0892e-298">Filtrowanie dzienników</span><span class="sxs-lookup"><span data-stu-id="0892e-298">Log filtering</span></span>

<span data-ttu-id="0892e-299">Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="0892e-299">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="0892e-300">Wszystkie dzienniki poniżej minimalnego poziomu nie są przesyłane do tego dostawcy, więc nie są wyświetlane ani przechowywane.</span><span class="sxs-lookup"><span data-stu-id="0892e-300">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="0892e-301">Aby pominąć wszystkie dzienniki, określ `LogLevel.None` jako minimalny poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="0892e-301">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="0892e-302">Wartość całkowita `LogLevel.None` wynosi 6, która jest większa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="0892e-302">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="0892e-303">Utwórz reguły filtru w konfiguracji</span><span class="sxs-lookup"><span data-stu-id="0892e-303">Create filter rules in configuration</span></span>

<span data-ttu-id="0892e-304">Kod szablonu projektu wywołuje `CreateDefaultBuilder`, aby skonfigurować rejestrowanie dla konsoli i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="0892e-304">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="0892e-305">Metoda `CreateDefaultBuilder` konfiguruje rejestrowanie, aby wyszukać konfigurację w sekcji `Logging`, jak wyjaśniono [wcześniej w tym artykule](#configuration).</span><span class="sxs-lookup"><span data-stu-id="0892e-305">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="0892e-306">Dane konfiguracyjne określają minimalne poziomy dziennika według dostawcy i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0892e-306">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

::: moniker-end

<span data-ttu-id="0892e-307">Ten kod JSON tworzy sześć reguł filtrowania: jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jeden dla wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="0892e-307">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="0892e-308">Dla każdego dostawcy wybierana jest pojedyncza reguła podczas tworzenia obiektu `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="0892e-308">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="0892e-309">Filtrowanie reguł w kodzie</span><span class="sxs-lookup"><span data-stu-id="0892e-309">Filter rules in code</span></span>

<span data-ttu-id="0892e-310">Poniższy przykład pokazuje, jak zarejestrować reguły filtru w kodzie:</span><span class="sxs-lookup"><span data-stu-id="0892e-310">The following example shows how to register filter rules in code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

<span data-ttu-id="0892e-311">Druga `AddFilter` określa dostawcę debugowania za pomocą nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="0892e-311">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="0892e-312">Pierwszy `AddFilter` ma zastosowanie do wszystkich dostawców, ponieważ nie określa typu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="0892e-312">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="0892e-313">Jak są stosowane reguły filtrowania</span><span class="sxs-lookup"><span data-stu-id="0892e-313">How filtering rules are applied</span></span>

<span data-ttu-id="0892e-314">Dane konfiguracji i kod `AddFilter` przedstawiony w powyższych przykładach tworzą reguły przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="0892e-314">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="0892e-315">Pierwsze sześć pochodzi z przykładu konfiguracji, a ostatnie dwa pochodzą z przykładu kodu.</span><span class="sxs-lookup"><span data-stu-id="0892e-315">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="0892e-316">Wartość liczbowa</span><span class="sxs-lookup"><span data-stu-id="0892e-316">Number</span></span> | <span data-ttu-id="0892e-317">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="0892e-317">Provider</span></span>      | <span data-ttu-id="0892e-318">Kategorie zaczynające się od...</span><span class="sxs-lookup"><span data-stu-id="0892e-318">Categories that begin with ...</span></span>          | <span data-ttu-id="0892e-319">Minimalny poziom rejestrowania</span><span class="sxs-lookup"><span data-stu-id="0892e-319">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="0892e-320">1</span><span class="sxs-lookup"><span data-stu-id="0892e-320">1</span></span>      | <span data-ttu-id="0892e-321">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0892e-321">Debug</span></span>         | <span data-ttu-id="0892e-322">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="0892e-322">All categories</span></span>                          | <span data-ttu-id="0892e-323">Informacje</span><span class="sxs-lookup"><span data-stu-id="0892e-323">Information</span></span>       |
| <span data-ttu-id="0892e-324">2</span><span class="sxs-lookup"><span data-stu-id="0892e-324">2</span></span>      | <span data-ttu-id="0892e-325">Konsola</span><span class="sxs-lookup"><span data-stu-id="0892e-325">Console</span></span>       | <span data-ttu-id="0892e-326">Microsoft. AspNetCore. MVC. Razor. Internal</span><span class="sxs-lookup"><span data-stu-id="0892e-326">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="0892e-327">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="0892e-327">Warning</span></span>           |
| <span data-ttu-id="0892e-328">3</span><span class="sxs-lookup"><span data-stu-id="0892e-328">3</span></span>      | <span data-ttu-id="0892e-329">Konsola</span><span class="sxs-lookup"><span data-stu-id="0892e-329">Console</span></span>       | <span data-ttu-id="0892e-330">Microsoft. AspNetCore. MVC. Razor. Razor</span><span class="sxs-lookup"><span data-stu-id="0892e-330">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="0892e-331">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0892e-331">Debug</span></span>             |
| <span data-ttu-id="0892e-332">4</span><span class="sxs-lookup"><span data-stu-id="0892e-332">4</span></span>      | <span data-ttu-id="0892e-333">Konsola</span><span class="sxs-lookup"><span data-stu-id="0892e-333">Console</span></span>       | <span data-ttu-id="0892e-334">Microsoft. AspNetCore. MVC. Razor</span><span class="sxs-lookup"><span data-stu-id="0892e-334">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="0892e-335">Błąd</span><span class="sxs-lookup"><span data-stu-id="0892e-335">Error</span></span>             |
| <span data-ttu-id="0892e-336">5</span><span class="sxs-lookup"><span data-stu-id="0892e-336">5</span></span>      | <span data-ttu-id="0892e-337">Konsola</span><span class="sxs-lookup"><span data-stu-id="0892e-337">Console</span></span>       | <span data-ttu-id="0892e-338">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="0892e-338">All categories</span></span>                          | <span data-ttu-id="0892e-339">Informacje</span><span class="sxs-lookup"><span data-stu-id="0892e-339">Information</span></span>       |
| <span data-ttu-id="0892e-340">6</span><span class="sxs-lookup"><span data-stu-id="0892e-340">6</span></span>      | <span data-ttu-id="0892e-341">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="0892e-341">All providers</span></span> | <span data-ttu-id="0892e-342">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="0892e-342">All categories</span></span>                          | <span data-ttu-id="0892e-343">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0892e-343">Debug</span></span>             |
| <span data-ttu-id="0892e-344">7</span><span class="sxs-lookup"><span data-stu-id="0892e-344">7</span></span>      | <span data-ttu-id="0892e-345">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="0892e-345">All providers</span></span> | <span data-ttu-id="0892e-346">System</span><span class="sxs-lookup"><span data-stu-id="0892e-346">System</span></span>                                  | <span data-ttu-id="0892e-347">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0892e-347">Debug</span></span>             |
| <span data-ttu-id="0892e-348">8</span><span class="sxs-lookup"><span data-stu-id="0892e-348">8</span></span>      | <span data-ttu-id="0892e-349">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0892e-349">Debug</span></span>         | <span data-ttu-id="0892e-350">Microsoft</span><span class="sxs-lookup"><span data-stu-id="0892e-350">Microsoft</span></span>                               | <span data-ttu-id="0892e-351">szuka</span><span class="sxs-lookup"><span data-stu-id="0892e-351">Trace</span></span>             |

<span data-ttu-id="0892e-352">Po utworzeniu obiektu `ILogger` obiekt `ILoggerFactory` wybiera jedną regułę dla każdego dostawcy do zastosowania do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="0892e-352">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="0892e-353">Wszystkie komunikaty zapisywane przez wystąpienie `ILogger` są filtrowane na podstawie wybranych reguł.</span><span class="sxs-lookup"><span data-stu-id="0892e-353">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="0892e-354">Najbardziej konkretną regułą można wybrać dla każdego dostawcy i pary kategorii z dostępnych reguł.</span><span class="sxs-lookup"><span data-stu-id="0892e-354">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="0892e-355">Następujący algorytm jest używany dla każdego dostawcy, gdy `ILogger` jest tworzony dla danej kategorii:</span><span class="sxs-lookup"><span data-stu-id="0892e-355">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="0892e-356">Wybierz wszystkie reguły, które pasują do dostawcy lub jego aliasu.</span><span class="sxs-lookup"><span data-stu-id="0892e-356">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="0892e-357">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły z pustym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="0892e-357">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="0892e-358">W wyniku poprzedniego kroku wybierz pozycję reguły z najdłuższym prefiksem kategorii.</span><span class="sxs-lookup"><span data-stu-id="0892e-358">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="0892e-359">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="0892e-359">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="0892e-360">Jeśli wybrano wiele reguł, zrób to **ostatnie** .</span><span class="sxs-lookup"><span data-stu-id="0892e-360">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="0892e-361">Jeśli nie wybrano żadnych reguł, użyj `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="0892e-361">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="0892e-362">Na powyższej liście reguł Załóżmy, że tworzysz obiekt `ILogger` dla kategorii "Microsoft. AspNetCore. MVC. Razor. RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="0892e-362">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="0892e-363">Dla dostawcy debugowania obowiązują reguły 1, 6 i 8.</span><span class="sxs-lookup"><span data-stu-id="0892e-363">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="0892e-364">Reguła 8 jest najbardziej specyficzna, więc jest to jedna wybrana.</span><span class="sxs-lookup"><span data-stu-id="0892e-364">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="0892e-365">W przypadku dostawcy konsoli obowiązują reguły 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="0892e-365">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="0892e-366">Reguła 3 jest najbardziej specyficzna.</span><span class="sxs-lookup"><span data-stu-id="0892e-366">Rule 3 is most specific.</span></span>

<span data-ttu-id="0892e-367">Dane wystąpienie `ILogger` wysyła dzienniki poziomu `Trace` i powyżej do dostawcy debugowania.</span><span class="sxs-lookup"><span data-stu-id="0892e-367">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="0892e-368">Dzienniki poziomu `Debug` i nowsze są wysyłane do dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="0892e-368">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="0892e-369">Aliasy dostawców</span><span class="sxs-lookup"><span data-stu-id="0892e-369">Provider aliases</span></span>

<span data-ttu-id="0892e-370">Każdy dostawca definiuje *alias* , który może być używany w konfiguracji zamiast w pełni kwalifikowanej nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="0892e-370">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="0892e-371">W przypadku dostawców wbudowanych Użyj następujących aliasów:</span><span class="sxs-lookup"><span data-stu-id="0892e-371">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="0892e-372">Konsola</span><span class="sxs-lookup"><span data-stu-id="0892e-372">Console</span></span>
* <span data-ttu-id="0892e-373">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="0892e-373">Debug</span></span>
* <span data-ttu-id="0892e-374">EventSource</span><span class="sxs-lookup"><span data-stu-id="0892e-374">EventSource</span></span>
* <span data-ttu-id="0892e-375">Elemencie</span><span class="sxs-lookup"><span data-stu-id="0892e-375">EventLog</span></span>
* <span data-ttu-id="0892e-376">TraceSource</span><span class="sxs-lookup"><span data-stu-id="0892e-376">TraceSource</span></span>
* <span data-ttu-id="0892e-377">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="0892e-377">AzureAppServicesFile</span></span>
* <span data-ttu-id="0892e-378">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="0892e-378">AzureAppServicesBlob</span></span>
* <span data-ttu-id="0892e-379">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="0892e-379">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="0892e-380">Domyślny poziom minimalny</span><span class="sxs-lookup"><span data-stu-id="0892e-380">Default minimum level</span></span>

<span data-ttu-id="0892e-381">Istnieje ustawienie minimalnego poziomu, które działa tylko wtedy, gdy nie mają zastosowania żadne reguły z konfiguracji lub kodu dla danego dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="0892e-381">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="0892e-382">Poniższy przykład pokazuje, jak ustawić poziom minimalny:</span><span class="sxs-lookup"><span data-stu-id="0892e-382">The following example shows how to set the minimum level:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

<span data-ttu-id="0892e-383">Jeśli poziom minimalny nie został jawnie ustawiony, wartość domyślna to `Information`, co oznacza, że dzienniki `Trace` i `Debug` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="0892e-383">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="0892e-384">Funkcje filtrowania</span><span class="sxs-lookup"><span data-stu-id="0892e-384">Filter functions</span></span>

<span data-ttu-id="0892e-385">Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorii, które nie mają przypisanych do nich reguł przez konfigurację lub kod.</span><span class="sxs-lookup"><span data-stu-id="0892e-385">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="0892e-386">Kod w funkcji ma dostęp do typu dostawcy, kategorii i poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="0892e-386">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="0892e-387">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="0892e-387">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="0892e-388">Kategorie i poziomy systemu</span><span class="sxs-lookup"><span data-stu-id="0892e-388">System categories and levels</span></span>

<span data-ttu-id="0892e-389">Poniżej przedstawiono niektóre kategorie używane przez ASP.NET Core i Entity Framework Core, z informacjami o dziennikach, od których należy się spodziewać:</span><span class="sxs-lookup"><span data-stu-id="0892e-389">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="0892e-390">Kategoria</span><span class="sxs-lookup"><span data-stu-id="0892e-390">Category</span></span>                            | <span data-ttu-id="0892e-391">Uwagi</span><span class="sxs-lookup"><span data-stu-id="0892e-391">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="0892e-392">Microsoft. AspNetCore</span><span class="sxs-lookup"><span data-stu-id="0892e-392">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="0892e-393">Ogólna Diagnostyka ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0892e-393">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="0892e-394">Microsoft. AspNetCore. dataprotection</span><span class="sxs-lookup"><span data-stu-id="0892e-394">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="0892e-395">Które klucze zostały wzięte pod uwagę, znaleziono i użyte.</span><span class="sxs-lookup"><span data-stu-id="0892e-395">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="0892e-396">Microsoft. AspNetCore. HostFiltering</span><span class="sxs-lookup"><span data-stu-id="0892e-396">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="0892e-397">Dozwolone hosty.</span><span class="sxs-lookup"><span data-stu-id="0892e-397">Hosts allowed.</span></span> |
| <span data-ttu-id="0892e-398">Microsoft. AspNetCore. hosting</span><span class="sxs-lookup"><span data-stu-id="0892e-398">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="0892e-399">Jak długo trwa wykonywanie żądań HTTP i czas ich uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="0892e-399">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="0892e-400">Które hostowanie zestawów uruchamiania zostało załadowane.</span><span class="sxs-lookup"><span data-stu-id="0892e-400">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="0892e-401">Microsoft. AspNetCore. MVC</span><span class="sxs-lookup"><span data-stu-id="0892e-401">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="0892e-402">Diagnostyka MVC i Razor.</span><span class="sxs-lookup"><span data-stu-id="0892e-402">MVC and Razor diagnostics.</span></span> <span data-ttu-id="0892e-403">Powiązanie modelu, wykonywanie filtru, kompilacja widoku, wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="0892e-403">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="0892e-404">Microsoft. AspNetCore. Routing</span><span class="sxs-lookup"><span data-stu-id="0892e-404">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="0892e-405">Informacje o trasie.</span><span class="sxs-lookup"><span data-stu-id="0892e-405">Route matching information.</span></span> |
| <span data-ttu-id="0892e-406">Microsoft. AspNetCore. Server</span><span class="sxs-lookup"><span data-stu-id="0892e-406">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="0892e-407">Reagowanie na uruchamianie, zatrzymywanie i utrzymywanie aktywności.</span><span class="sxs-lookup"><span data-stu-id="0892e-407">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="0892e-408">Informacje o certyfikacie HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0892e-408">HTTPS certificate information.</span></span> |
| <span data-ttu-id="0892e-409">Microsoft. AspNetCore. StaticFiles</span><span class="sxs-lookup"><span data-stu-id="0892e-409">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="0892e-410">Obsługiwane pliki.</span><span class="sxs-lookup"><span data-stu-id="0892e-410">Files served.</span></span> |
| <span data-ttu-id="0892e-411">Microsoft. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="0892e-411">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="0892e-412">Ogólna Diagnostyka Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0892e-412">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="0892e-413">Aktywność i Konfiguracja bazy danych, wykrywanie zmian, migracje.</span><span class="sxs-lookup"><span data-stu-id="0892e-413">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="0892e-414">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="0892e-414">Log scopes</span></span>

 <span data-ttu-id="0892e-415">*Zakres* może grupować zestaw operacji logicznych.</span><span class="sxs-lookup"><span data-stu-id="0892e-415">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="0892e-416">Takie grupowanie może służyć do dołączania tych samych danych do każdego dziennika, który został utworzony jako część zestawu.</span><span class="sxs-lookup"><span data-stu-id="0892e-416">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="0892e-417">Na przykład każdy dziennik utworzony w ramach przetwarzania transakcji może zawierać identyfikator transakcji.</span><span class="sxs-lookup"><span data-stu-id="0892e-417">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="0892e-418">Zakres jest typem `IDisposable`, który jest zwracany przez metodę <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> i obowiązuje do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="0892e-418">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="0892e-419">Użyj zakresu przez Zawijanie wywołań rejestratora w bloku `using`:</span><span class="sxs-lookup"><span data-stu-id="0892e-419">Use a scope by wrapping logger calls in a `using` block:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

<span data-ttu-id="0892e-420">Poniższy kod włącza zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="0892e-420">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="0892e-421">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0892e-421">*Program.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0892e-422">Skonfigurowanie opcji rejestratora `IncludeScopes` konsoli jest wymagane do włączenia rejestrowania na podstawie zakresu.</span><span class="sxs-lookup"><span data-stu-id="0892e-422">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="0892e-423">Informacje o konfiguracji znajdują się w sekcji [Konfiguracja](#configuration) .</span><span class="sxs-lookup"><span data-stu-id="0892e-423">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="0892e-424">Każdy komunikat dziennika zawiera informacje o zakresie:</span><span class="sxs-lookup"><span data-stu-id="0892e-424">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="0892e-425">Wbudowani dostawcy rejestrowania</span><span class="sxs-lookup"><span data-stu-id="0892e-425">Built-in logging providers</span></span>

<span data-ttu-id="0892e-426">ASP.NET Core dostarcza następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="0892e-426">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="0892e-427">Console</span><span class="sxs-lookup"><span data-stu-id="0892e-427">Console</span></span>](#console-provider)
* [<span data-ttu-id="0892e-428">Rozpocząć</span><span class="sxs-lookup"><span data-stu-id="0892e-428">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="0892e-429">EventSource</span><span class="sxs-lookup"><span data-stu-id="0892e-429">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="0892e-430">Elemencie</span><span class="sxs-lookup"><span data-stu-id="0892e-430">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="0892e-431">TraceSource</span><span class="sxs-lookup"><span data-stu-id="0892e-431">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="0892e-432">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="0892e-432">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="0892e-433">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="0892e-433">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="0892e-434">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="0892e-434">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="0892e-435">Aby uzyskać informacje na temat strumienia stdout i rejestrowania debugowania za pomocą modułu ASP.NET Core, zobacz <xref:test/troubleshoot-azure-iis> i <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="0892e-435">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="0892e-436">Dostawca konsoli</span><span class="sxs-lookup"><span data-stu-id="0892e-436">Console provider</span></span>

<span data-ttu-id="0892e-437">Pakiet [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) Provider wysyła dane wyjściowe dziennika do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="0892e-437">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="0892e-438">Aby wyświetlić dane wyjściowe rejestrowania konsoli, Otwórz wiersz polecenia w folderze projektu i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0892e-438">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="0892e-439">Dostawca debugowania</span><span class="sxs-lookup"><span data-stu-id="0892e-439">Debug provider</span></span>

<span data-ttu-id="0892e-440">Pakiet [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) Provider zapisuje dane wyjściowe dziennika przy użyciu klasy [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug) (`Debug.WriteLine` wywołania metod).</span><span class="sxs-lookup"><span data-stu-id="0892e-440">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="0892e-441">W systemie Linux ten dostawca zapisuje dzienniki do */var/log/Message*.</span><span class="sxs-lookup"><span data-stu-id="0892e-441">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="0892e-442">Dostawca źródła zdarzeń</span><span class="sxs-lookup"><span data-stu-id="0892e-442">Event Source provider</span></span>

<span data-ttu-id="0892e-443">Pakiet dostawcy [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) zapisuje na międzyplatformę źródła zdarzeń przy użyciu nazwy `Microsoft-Extensions-Logging`.</span><span class="sxs-lookup"><span data-stu-id="0892e-443">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="0892e-444">W systemie Windows Dostawca używa [funkcji ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="0892e-444">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="0892e-445">Dostawca źródła zdarzeń jest automatycznie dodawany, gdy `CreateDefaultBuilder` jest wywoływana w celu skompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="0892e-445">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

::: moniker range=">= aspnetcore-3.0"

#### <a name="dotnet-trace-tooling"></a><span data-ttu-id="0892e-446">narzędzia śledzenia dotnet</span><span class="sxs-lookup"><span data-stu-id="0892e-446">dotnet trace tooling</span></span>

<span data-ttu-id="0892e-447">Narzędzie do [śledzenia dotnet](/dotnet/core/diagnostics/dotnet-trace) to międzyplatformowe narzędzie globalne interfejsu wiersza polecenia, które umożliwia zbieranie śladów programu .NET Core uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="0892e-447">The [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) tool is a cross-platform CLI global tool that enables the collection of .NET Core traces of a running process.</span></span> <span data-ttu-id="0892e-448">Narzędzie zbiera dane dostawcy <xref:Microsoft.Extensions.Logging.EventSource> przy użyciu <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span><span class="sxs-lookup"><span data-stu-id="0892e-448">The tool collects <xref:Microsoft.Extensions.Logging.EventSource> provider data using a <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span></span>

<span data-ttu-id="0892e-449">Zainstaluj narzędzia śledzenia dotnet przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="0892e-449">Install the dotnet trace tooling with the following command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-trace
```

<span data-ttu-id="0892e-450">Użyj narzędzi śledzenia dotnet, aby zebrać ślad z aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0892e-450">Use the dotnet trace tooling to collect a trace from an app:</span></span>

1. <span data-ttu-id="0892e-451">Jeśli aplikacja nie kompiluje hosta z `CreateDefaultBuilder`, Dodaj [dostawcę źródła zdarzeń](#event-source-provider) do konfiguracji rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-451">If the app doesn't build the host with `CreateDefaultBuilder`, add the [Event Source provider](#event-source-provider) to the app's logging configuration.</span></span>

1. <span data-ttu-id="0892e-452">Uruchom aplikację za pomocą polecenia `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="0892e-452">Run the app with the `dotnet run` command.</span></span>

1. <span data-ttu-id="0892e-453">Określ identyfikator procesu (PID) aplikacji .NET Core:</span><span class="sxs-lookup"><span data-stu-id="0892e-453">Determine the process identifier (PID) of the .NET Core app:</span></span>

   * <span data-ttu-id="0892e-454">W systemie Windows należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="0892e-454">On Windows, use one of the following approaches:</span></span>
     * <span data-ttu-id="0892e-455">Menedżer zadań (Ctrl + Alt + Del)</span><span class="sxs-lookup"><span data-stu-id="0892e-455">Task Manager (Ctrl+Alt+Del)</span></span>
     * [<span data-ttu-id="0892e-456">tasklist — polecenie</span><span class="sxs-lookup"><span data-stu-id="0892e-456">tasklist command</span></span>](/windows-server/administration/windows-commands/tasklist)
     * [<span data-ttu-id="0892e-457">Get-Process — polecenie programu PowerShell</span><span class="sxs-lookup"><span data-stu-id="0892e-457">Get-Process Powershell command</span></span>](/powershell/module/microsoft.powershell.management/get-process)
   * <span data-ttu-id="0892e-458">W systemie Linux Użyj [polecenia pidof](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span><span class="sxs-lookup"><span data-stu-id="0892e-458">On Linux, use the [pidof command](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span></span>

   <span data-ttu-id="0892e-459">Znajdź identyfikator PID procesu, który ma taką samą nazwę jak zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-459">Find the PID for the process that has the same name as the app's assembly.</span></span>

1. <span data-ttu-id="0892e-460">Wykonaj `dotnet trace` polecenie.</span><span class="sxs-lookup"><span data-stu-id="0892e-460">Execute the `dotnet trace` command.</span></span>

   <span data-ttu-id="0892e-461">Ogólna składnia poleceń:</span><span class="sxs-lookup"><span data-stu-id="0892e-461">General command syntax:</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   <span data-ttu-id="0892e-462">W przypadku korzystania z powłoki poleceń programu PowerShell należy ująć wartość `--providers` w pojedynczy cudzysłów (`'`):</span><span class="sxs-lookup"><span data-stu-id="0892e-462">When using a PowerShell command shell, enclose the `--providers` value in single quotes (`'`):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   <span data-ttu-id="0892e-463">Na platformach innych niż Windows Dodaj opcję `-f speedscope`, aby zmienić format wyjściowego pliku śledzenia na `speedscope`.</span><span class="sxs-lookup"><span data-stu-id="0892e-463">On non-Windows platforms, add the `-f speedscope` option to change the format of the output trace file to `speedscope`.</span></span>

   | <span data-ttu-id="0892e-464">Kodu</span><span class="sxs-lookup"><span data-stu-id="0892e-464">Keyword</span></span> | <span data-ttu-id="0892e-465">Opis</span><span class="sxs-lookup"><span data-stu-id="0892e-465">Description</span></span> |
   | :-----: | ----------- |
   | <span data-ttu-id="0892e-466">1</span><span class="sxs-lookup"><span data-stu-id="0892e-466">1</span></span>       | <span data-ttu-id="0892e-467">Rejestruj meta zdarzenia dotyczące `LoggingEventSource`.</span><span class="sxs-lookup"><span data-stu-id="0892e-467">Log meta events about the `LoggingEventSource`.</span></span> <span data-ttu-id="0892e-468">Nie rejestruje zdarzeń z `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="0892e-468">Doesn't log events from `ILogger`).</span></span> |
   | <span data-ttu-id="0892e-469">2</span><span class="sxs-lookup"><span data-stu-id="0892e-469">2</span></span>       | <span data-ttu-id="0892e-470">Włącza `Message` zdarzenie, gdy zostanie wywołane `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="0892e-470">Turns on the `Message` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="0892e-471">Zawiera informacje w sposób programistyczny (nie sformatowany).</span><span class="sxs-lookup"><span data-stu-id="0892e-471">Provides information in a programmatic (not formatted) way.</span></span> |
   | <span data-ttu-id="0892e-472">4</span><span class="sxs-lookup"><span data-stu-id="0892e-472">4</span></span>       | <span data-ttu-id="0892e-473">Włącza `FormatMessage` zdarzenie, gdy zostanie wywołane `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="0892e-473">Turns on the `FormatMessage` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="0892e-474">Udostępnia sformatowaną wersję ciągu informacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-474">Provides the formatted string version of the information.</span></span> |
   | <span data-ttu-id="0892e-475">8</span><span class="sxs-lookup"><span data-stu-id="0892e-475">8</span></span>       | <span data-ttu-id="0892e-476">Włącza `MessageJson` zdarzenie, gdy zostanie wywołane `ILogger.Log()`.</span><span class="sxs-lookup"><span data-stu-id="0892e-476">Turns on the `MessageJson` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="0892e-477">Zawiera reprezentację argumentów w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="0892e-477">Provides a JSON representation of the arguments.</span></span> |

   | <span data-ttu-id="0892e-478">Poziom zdarzenia</span><span class="sxs-lookup"><span data-stu-id="0892e-478">Event Level</span></span> | <span data-ttu-id="0892e-479">Opis</span><span class="sxs-lookup"><span data-stu-id="0892e-479">Description</span></span>     |
   | :---------: | --------------- |
   | <span data-ttu-id="0892e-480">0</span><span class="sxs-lookup"><span data-stu-id="0892e-480">0</span></span>           | `LogAlways`     |
   | <span data-ttu-id="0892e-481">1</span><span class="sxs-lookup"><span data-stu-id="0892e-481">1</span></span>           | `Critical`      |
   | <span data-ttu-id="0892e-482">2</span><span class="sxs-lookup"><span data-stu-id="0892e-482">2</span></span>           | `Error`         |
   | <span data-ttu-id="0892e-483">3</span><span class="sxs-lookup"><span data-stu-id="0892e-483">3</span></span>           | `Warning`       |
   | <span data-ttu-id="0892e-484">4</span><span class="sxs-lookup"><span data-stu-id="0892e-484">4</span></span>           | `Informational` |
   | <span data-ttu-id="0892e-485">5</span><span class="sxs-lookup"><span data-stu-id="0892e-485">5</span></span>           | `Verbose`       |

   <span data-ttu-id="0892e-486">`FilterSpecs` wpisy `{Logger Category}` i `{Event Level}` reprezentują dodatkowe warunki filtrowania dzienników.</span><span class="sxs-lookup"><span data-stu-id="0892e-486">`FilterSpecs` entries for `{Logger Category}` and `{Event Level}` represent additional log filtering conditions.</span></span> <span data-ttu-id="0892e-487">Oddziel pozycje `FilterSpecs` średnikiem (`;`).</span><span class="sxs-lookup"><span data-stu-id="0892e-487">Separate `FilterSpecs` entries with a semicolon (`;`).</span></span>

   <span data-ttu-id="0892e-488">Przykład użycia powłoki poleceń systemu Windows (**Brak** pojedynczego cudzysłowu wokół wartości `--providers`):</span><span class="sxs-lookup"><span data-stu-id="0892e-488">Example using a Windows command shell (**no** single quotes around the `--providers` value):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   <span data-ttu-id="0892e-489">Poprzednie polecenie aktywuje:</span><span class="sxs-lookup"><span data-stu-id="0892e-489">The preceding command activates:</span></span>

   * <span data-ttu-id="0892e-490">Rejestrator źródła zdarzeń do tworzenia sformatowanych ciągów (`4`) dla błędów (`2`).</span><span class="sxs-lookup"><span data-stu-id="0892e-490">The Event Source logger to produce formatted strings (`4`) for errors (`2`).</span></span>
   * <span data-ttu-id="0892e-491">Rejestrowanie `Microsoft.AspNetCore.Hosting` na poziomie rejestrowania `Informational` (`4`).</span><span class="sxs-lookup"><span data-stu-id="0892e-491">`Microsoft.AspNetCore.Hosting` logging at the `Informational` logging level (`4`).</span></span>

1. <span data-ttu-id="0892e-492">Zatrzymaj narzędzia śledzenia dotnet, naciskając klawisz ENTER lub CTRL + C.</span><span class="sxs-lookup"><span data-stu-id="0892e-492">Stop the dotnet trace tooling by pressing the Enter key or Ctrl+C.</span></span>

   <span data-ttu-id="0892e-493">Ślad jest zapisywany z nazwą *Trace. webtrace* w folderze, w którym jest wykonywane polecenie `dotnet trace`.</span><span class="sxs-lookup"><span data-stu-id="0892e-493">The trace is saved with the name *trace.nettrace* in the folder where the `dotnet trace` command is executed.</span></span>

1. <span data-ttu-id="0892e-494">Otwórz ślad z [Narzędzia PerfView](#perfview).</span><span class="sxs-lookup"><span data-stu-id="0892e-494">Open the trace with [Perfview](#perfview).</span></span> <span data-ttu-id="0892e-495">Otwórz plik *Trace. webtrace* i zbadaj zdarzenia śledzenia.</span><span class="sxs-lookup"><span data-stu-id="0892e-495">Open the *trace.nettrace* file and explore the trace events.</span></span>

<span data-ttu-id="0892e-496">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="0892e-496">For more information, see:</span></span>

* <span data-ttu-id="0892e-497">[Śledzenie dla narzędzia do analizy wydajności (program dotnet-Trace)](/dotnet/core/diagnostics/dotnet-trace) (dokumentacja platformy .NET Core)</span><span class="sxs-lookup"><span data-stu-id="0892e-497">[Trace for performance analysis utility (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core documentation)</span></span>
* <span data-ttu-id="0892e-498">[Śledzenie dla narzędzia analizy wydajności (dotnet-Trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (dokumentacja repozytorium dotnet/Diagnostics)</span><span class="sxs-lookup"><span data-stu-id="0892e-498">[Trace for performance analysis utility (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (dotnet/diagnostics GitHub repository documentation)</span></span>
* <span data-ttu-id="0892e-499">[LoggingEventSource — Klasa](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (przeglądarka interfejsu API platformy .NET)</span><span class="sxs-lookup"><span data-stu-id="0892e-499">[LoggingEventSource Class](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API Browser)</span></span>
* <xref:System.Diagnostics.Tracing.EventLevel>
* <span data-ttu-id="0892e-500">[LoggingEventSource Reference Source (3,0 &ndash;)](https://github.com/aspnet/Extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) , aby uzyskać Źródło odwołania dla innej wersji, Zmień gałąź na `release/{Version}`, gdzie `{Version}` jest wersją ASP.NET Core żądana.</span><span class="sxs-lookup"><span data-stu-id="0892e-500">[LoggingEventSource reference source (3.0)](https://github.com/aspnet/Extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash; To obtain reference source for a different version, change the branch to `release/{Version}`, where `{Version}` is the version of ASP.NET Core desired.</span></span>
* <span data-ttu-id="0892e-501">[Narzędzia perfview](#perfview) &ndash; przydatny do wyświetlania śladów źródła zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="0892e-501">[Perfview](#perfview) &ndash; Useful for viewing Event Source traces.</span></span>

#### <a name="perfview"></a><span data-ttu-id="0892e-502">Narzędzia PerfView</span><span class="sxs-lookup"><span data-stu-id="0892e-502">Perfview</span></span>

::: moniker-end

<span data-ttu-id="0892e-503">Użyj [Narzędzia Narzędzia PerfView](https://github.com/Microsoft/perfview) do zbierania i wyświetlania dzienników.</span><span class="sxs-lookup"><span data-stu-id="0892e-503">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="0892e-504">Istnieją inne narzędzia do wyświetlania dzienników ETW, ale narzędzia PerfView zapewnia najlepsze środowisko pracy z zdarzeniami ETW emitowanymi przez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0892e-504">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="0892e-505">Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, należy dodać ciąg `*Microsoft-Extensions-Logging` do listy **dodatkowych dostawców** .</span><span class="sxs-lookup"><span data-stu-id="0892e-505">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="0892e-506">(Nie przegap gwiazdki na początku ciągu znaków).</span><span class="sxs-lookup"><span data-stu-id="0892e-506">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia PerfView dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="0892e-508">Dostawca dziennika zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="0892e-508">Windows EventLog provider</span></span>

<span data-ttu-id="0892e-509">Pakiet dostawcy [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) wysyła dane wyjściowe dziennika do dziennika zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="0892e-509">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="0892e-510">[Przeciążenia addeventlog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) umożliwiają przekazywanie <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span><span class="sxs-lookup"><span data-stu-id="0892e-510">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span>

### <a name="tracesource-provider"></a><span data-ttu-id="0892e-511">Dostawca TraceSource</span><span class="sxs-lookup"><span data-stu-id="0892e-511">TraceSource provider</span></span>

<span data-ttu-id="0892e-512">Pakiet dostawcy [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa bibliotek i dostawców <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="0892e-512">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="0892e-513">[Przeciążenia AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) umożliwiają przekazywanie danych w przełączniku źródłowym i odbiorniku śledzenia.</span><span class="sxs-lookup"><span data-stu-id="0892e-513">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="0892e-514">Aby można było korzystać z tego dostawcy, aplikacja musi działać na .NET Framework (a nie na platformie .NET Core).</span><span class="sxs-lookup"><span data-stu-id="0892e-514">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="0892e-515">Dostawca może kierować komunikaty do różnych [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takich jak <xref:System.Diagnostics.TextWriterTraceListener> używane w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-515">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="0892e-516">Dostawca Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0892e-516">Azure App Service provider</span></span>

<span data-ttu-id="0892e-517">Pakiet [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) Provider zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji Azure App Service i w usłudze [BLOB Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0892e-517">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0892e-518">Pakiet dostawcy nie jest uwzględniony w udostępnionej infrastrukturze.</span><span class="sxs-lookup"><span data-stu-id="0892e-518">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="0892e-519">Aby użyć dostawcy, Dodaj pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="0892e-519">To use the provider, add the provider package to the project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="0892e-520">Pakiet dostawcy nie jest uwzględniony w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0892e-520">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="0892e-521">Podczas określania wartości docelowej .NET Framework lub odwoływania się do `Microsoft.AspNetCore.App` pakietu, Dodaj pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="0892e-521">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0892e-522">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0892e-522">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="0892e-523">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0892e-523">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="0892e-524">Przeciążenie <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> umożliwia przekazywanie <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="0892e-524">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="0892e-525">Obiekt Settings może przesłonić ustawienia domyślne, takie jak szablon danych wyjściowych rejestrowania, nazwa obiektu BLOB i limit rozmiaru pliku.</span><span class="sxs-lookup"><span data-stu-id="0892e-525">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="0892e-526">(*Szablon danych wyjściowych* to szablon wiadomości, który jest stosowany do wszystkich dzienników oprócz tego, co jest dostępne w wywołaniu metody `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="0892e-526">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

<span data-ttu-id="0892e-527">Po wdrożeniu w aplikacji App Service, aplikacja będzie przestrzegać ustawień w sekcji [dzienniki App Service](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) na stronie **App Service** Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0892e-527">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="0892e-528">Po zaktualizowaniu następujących ustawień zmiany zaczynają obowiązywać natychmiast, bez konieczności ponownego uruchomienia lub ponownej wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-528">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="0892e-529">**Rejestrowanie aplikacji (system plików)**</span><span class="sxs-lookup"><span data-stu-id="0892e-529">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="0892e-530">**Rejestrowanie aplikacji (BLOB)**</span><span class="sxs-lookup"><span data-stu-id="0892e-530">**Application Logging (Blob)**</span></span>

<span data-ttu-id="0892e-531">Domyślna lokalizacja plików dziennika znajduje się w folderze *D:\\home\\plik_dziennikas\\folder aplikacji* , a domyślna nazwa pliku to *Diagnostics-YYYYMMDD. txt*.</span><span class="sxs-lookup"><span data-stu-id="0892e-531">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="0892e-532">Domyślny limit rozmiaru pliku wynosi 10 MB, a domyślna maksymalna liczba zachowywanych plików to 2.</span><span class="sxs-lookup"><span data-stu-id="0892e-532">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="0892e-533">Domyślną nazwą obiektu BLOB jest *{App-Name} {timestamp}/yyyy/mm/dd/hh/{GUID}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="0892e-533">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="0892e-534">Dostawca działa tylko wtedy, gdy projekt jest uruchomiony w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="0892e-534">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="0892e-535">Nie ma ono wpływu, gdy projekt jest uruchamiany lokalnie&mdash;nie zapisuje w plikach lokalnych ani w lokalnym magazynie programistycznym dla obiektów BLOB.</span><span class="sxs-lookup"><span data-stu-id="0892e-535">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="0892e-536">Przesyłanie strumieniowe dzienników Azure</span><span class="sxs-lookup"><span data-stu-id="0892e-536">Azure log streaming</span></span>

<span data-ttu-id="0892e-537">Usługa przesyłania strumieniowego w usłudze Azure log umożliwia wyświetlanie dziennika aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="0892e-537">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="0892e-538">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="0892e-538">The app server</span></span>
* <span data-ttu-id="0892e-539">Serwer sieci Web</span><span class="sxs-lookup"><span data-stu-id="0892e-539">The web server</span></span>
* <span data-ttu-id="0892e-540">Śledzenie nieudanych żądań</span><span class="sxs-lookup"><span data-stu-id="0892e-540">Failed request tracing</span></span>

<span data-ttu-id="0892e-541">Aby skonfigurować przesyłanie strumieniowe dzienników Azure:</span><span class="sxs-lookup"><span data-stu-id="0892e-541">To configure Azure log streaming:</span></span>

* <span data-ttu-id="0892e-542">Przejdź do strony **dzienników App Service** ze strony portalu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-542">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="0892e-543">Ustaw **Rejestrowanie aplikacji (system plików)** na **włączone**.</span><span class="sxs-lookup"><span data-stu-id="0892e-543">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="0892e-544">Wybierz **poziom**dziennika.</span><span class="sxs-lookup"><span data-stu-id="0892e-544">Choose the log **Level**.</span></span>

<span data-ttu-id="0892e-545">Przejdź do strony **strumień dziennika** , aby wyświetlić komunikaty aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0892e-545">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="0892e-546">Są one rejestrowane przez aplikację za pomocą interfejsu `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="0892e-546">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="0892e-547">Rejestrowanie śledzenia Application Insights platformy Azure</span><span class="sxs-lookup"><span data-stu-id="0892e-547">Azure Application Insights trace logging</span></span>

<span data-ttu-id="0892e-548">Pakiet [Microsoft. Extensions. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) Provider zapisuje dzienniki na platformie Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0892e-548">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="0892e-549">Application Insights to usługa, która monitoruje aplikację internetową i udostępnia narzędzia do wykonywania zapytań i analizowania danych telemetrycznych.</span><span class="sxs-lookup"><span data-stu-id="0892e-549">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="0892e-550">Jeśli używasz tego dostawcy, możesz wysyłać zapytania i analizować dzienniki przy użyciu narzędzi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0892e-550">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="0892e-551">Dostawca rejestrowania jest dołączony jako zależność [Microsoft. ApplicationInsights. AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), który jest pakietem, który zapewnia wszystkie dostępne dane telemetryczne dla ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0892e-551">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="0892e-552">Jeśli używasz tego pakietu, nie musisz instalować pakietu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="0892e-552">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="0892e-553">Nie używaj pakietu [Microsoft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) Package&mdash;, który jest przeznaczony dla ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="0892e-553">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="0892e-554">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="0892e-554">For more information, see the following resources:</span></span>

* [<span data-ttu-id="0892e-555">Przegląd Application Insights</span><span class="sxs-lookup"><span data-stu-id="0892e-555">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="0892e-556">[Application Insights dla ASP.NET Core aplikacji](/azure/azure-monitor/app/asp-net-core) — Zacznij tutaj, jeśli chcesz zaimplementować cały zakres Application Insights telemetrii wraz z rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="0892e-556">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="0892e-557">[ApplicationInsightsLoggerProvider for .NET Core ILogger](/azure/azure-monitor/app/ilogger) — Rozpocznij tutaj, jeśli chcesz zaimplementować dostawcę rejestrowania bez pozostałej części telemetrii Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0892e-557">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="0892e-558">[Karty rejestrowania Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="0892e-558">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="0892e-559">[Zainstaluj, skonfiguruj i zainicjuj samouczek Application INSIGHTS SDK](/learn/modules/instrument-web-app-code-with-application-insights) — interaktywny w witrynie Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="0892e-559">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="0892e-560">Dostawcy rejestrowania innych firm</span><span class="sxs-lookup"><span data-stu-id="0892e-560">Third-party logging providers</span></span>

<span data-ttu-id="0892e-561">Platformy rejestrowania innych firm, które współpracują z ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0892e-561">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="0892e-562">[ELMAH.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0892e-562">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="0892e-563">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="0892e-563">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="0892e-564">[JSNLog](https://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="0892e-564">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="0892e-565">[KissLog.NET](https://kisslog.net/) ([repozytorium GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="0892e-565">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="0892e-566">[Log4Net](https://logging.apache.org/log4net/) ([repozytorium GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="0892e-566">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="0892e-567">[Loggr](https://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0892e-567">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="0892e-568">[NLOG](https://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0892e-568">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="0892e-569">[Sentry](https://sentry.io/welcome/) ([repozytorium GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="0892e-569">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="0892e-570">[Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="0892e-570">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="0892e-571">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repozytorium GitHub](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="0892e-571">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="0892e-572">Niektóre struktury innych firm mogą wykonywać [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="0892e-572">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="0892e-573">Korzystanie z struktury innej firmy jest podobne do korzystania z jednego z wbudowanych dostawców:</span><span class="sxs-lookup"><span data-stu-id="0892e-573">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="0892e-574">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="0892e-574">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="0892e-575">Wywoływanie metody rozszerzenia `ILoggerFactory` dostarczonej przez strukturę rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="0892e-575">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="0892e-576">Aby uzyskać więcej informacji, zobacz dokumentację każdego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="0892e-576">For more information, see each provider's documentation.</span></span> <span data-ttu-id="0892e-577">Dostawcy rejestrowania innych firm nie są obsługiwani przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0892e-577">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0892e-578">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0892e-578">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
