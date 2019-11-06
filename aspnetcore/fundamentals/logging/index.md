---
title: Rejestrowanie w programie .NET Core i ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać struktury rejestrowania dostarczonej przez pakiet NuGet Microsoft. Extensions. Logging.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 2cb19d251ad69ebd7d18480c14857e948c69b747
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659971"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="98640-103">Rejestrowanie w programie .NET Core i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98640-103">Logging in .NET Core and ASP.NET Core</span></span>

<span data-ttu-id="98640-104">Autorzy [Dykstra](https://github.com/tdykstra) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="98640-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="98640-105">Platforma .NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm.</span><span class="sxs-lookup"><span data-stu-id="98640-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="98640-106">W tym artykule pokazano, jak używać interfejsu API rejestrowania z wbudowanymi dostawcami.</span><span class="sxs-lookup"><span data-stu-id="98640-106">This article shows how to use the logging API with built-in providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="98640-107">Większość przykładów kodu pokazanych w tym artykule pochodzą z ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="98640-108">Części tych fragmentów kodu dotyczące rejestrowania mają zastosowanie do dowolnej aplikacji .NET Core, która korzysta z [hosta ogólnego](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="98640-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="98640-109">Aby uzyskać informacje na temat korzystania z hosta generycznego w aplikacjach konsolowych innych niż sieci Web, zobacz [usługi hostowane](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="98640-109">For information about how to use the Generic Host in non-web console apps, see [Hosted services](xref:fundamentals/host/hosted-services).</span></span>

<span data-ttu-id="98640-110">Rejestrowanie kodu dla aplikacji bez hosta ogólnego różni się w sposób, w jaki [są dodawane dostawcy](#add-providers) i [są tworzone rejestratory](#create-logs).</span><span class="sxs-lookup"><span data-stu-id="98640-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="98640-111">Przykłady kodu niehosta są wyświetlane w tych częściach artykułu.</span><span class="sxs-lookup"><span data-stu-id="98640-111">Non-host code examples are shown in those sections of the article.</span></span>

::: moniker-end

<span data-ttu-id="98640-112">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98640-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="98640-113">Dodaj dostawców</span><span class="sxs-lookup"><span data-stu-id="98640-113">Add providers</span></span>

<span data-ttu-id="98640-114">Dostawca rejestrowania wyświetla lub przechowuje dzienniki.</span><span class="sxs-lookup"><span data-stu-id="98640-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="98640-115">Na przykład dostawca konsoli wyświetla dzienniki w konsoli programu, a Dostawca usługi Azure Application Insights przechowuje je na platformie Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="98640-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="98640-116">Dzienniki mogą być wysyłane do wielu miejsc docelowych przez dodanie wielu dostawców.</span><span class="sxs-lookup"><span data-stu-id="98640-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="98640-117">Aby dodać dostawcę w aplikacji korzystającej z hosta ogólnego, wywołaj metodę rozszerzenia `Add{provider name}` dostawcy w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="98640-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="98640-118">W aplikacji konsolowej bez hosta Wywołaj metodę rozszerzenia `Add{provider name}` dostawcy podczas tworzenia `LoggerFactory`:</span><span class="sxs-lookup"><span data-stu-id="98640-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="98640-119">`LoggerFactory` i `AddConsole` wymagają instrukcji `using` dla `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="98640-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="98640-120">Domyślne ASP.NET Core wywołań szablonów projektu <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, które dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="98640-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="98640-121">Konsola</span><span class="sxs-lookup"><span data-stu-id="98640-121">Console</span></span>
* <span data-ttu-id="98640-122">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="98640-122">Debug</span></span>
* <span data-ttu-id="98640-123">EventSource</span><span class="sxs-lookup"><span data-stu-id="98640-123">EventSource</span></span>
* <span data-ttu-id="98640-124">EventLog (tylko w przypadku uruchamiania w systemie Windows)</span><span class="sxs-lookup"><span data-stu-id="98640-124">EventLog (only when running on Windows)</span></span>

<span data-ttu-id="98640-125">Dostawców domyślnych można zastąpić własnymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="98640-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="98640-126">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>i Dodaj żądanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="98640-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0 "

<span data-ttu-id="98640-127">Aby dodać dostawcę, wywołaj metodę rozszerzenia `Add{provider name}` dostawcy w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="98640-127">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="98640-128">Poprzedzający kod wymaga odwołań do `Microsoft.Extensions.Logging` i `Microsoft.Extensions.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="98640-128">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="98640-129">Domyślny szablon projektu wywołuje <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, który dodaje następujących dostawców rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="98640-129">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="98640-130">Konsola</span><span class="sxs-lookup"><span data-stu-id="98640-130">Console</span></span>
* <span data-ttu-id="98640-131">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="98640-131">Debug</span></span>
* <span data-ttu-id="98640-132">EventSource (rozpoczęcie w ASP.NET Core 2,2)</span><span class="sxs-lookup"><span data-stu-id="98640-132">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="98640-133">Jeśli używasz `CreateDefaultBuilder`, możesz zastąpić domyślnych dostawców własnymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="98640-133">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="98640-134">Wywołaj <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>i Dodaj żądanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="98640-134">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

<span data-ttu-id="98640-135">Więcej informacji na temat [wbudowanych dostawców rejestrowania](#built-in-logging-providers) i [dostawców rejestrowania innych](#third-party-logging-providers) firm znajduje się w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="98640-135">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="98640-136">Tworzenie dzienników</span><span class="sxs-lookup"><span data-stu-id="98640-136">Create logs</span></span>

<span data-ttu-id="98640-137">Aby utworzyć dzienniki, użyj obiektu <xref:Microsoft.Extensions.Logging.ILogger%601>.</span><span class="sxs-lookup"><span data-stu-id="98640-137">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="98640-138">W aplikacji sieci Web lub hostowanej usłudze Pobierz `ILogger` z iniekcji zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="98640-138">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="98640-139">W aplikacjach konsolowych innych niż host Użyj `LoggerFactory`, aby utworzyć `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="98640-139">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="98640-140">Poniższy ASP.NET Core przykład tworzy Rejestrator z `TodoApiSample.Pages.AboutModel` jako kategorii.</span><span class="sxs-lookup"><span data-stu-id="98640-140">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="98640-141">*Kategoria* dziennika jest ciągiem, który jest skojarzony z każdym dziennikiem.</span><span class="sxs-lookup"><span data-stu-id="98640-141">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="98640-142">Wystąpienie `ILogger<T>` dostarczone przez program DI tworzy dzienniki, które mają w pełni kwalifikowaną nazwę typu `T` jako kategorię.</span><span class="sxs-lookup"><span data-stu-id="98640-142">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="98640-143">Poniższy przykład aplikacji nieprzyjmującej konsoli tworzy Rejestrator z `LoggingConsoleApp.Program` jako kategorię.</span><span class="sxs-lookup"><span data-stu-id="98640-143">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

::: moniker-end

<span data-ttu-id="98640-144">W poniższych przykładach ASP.NET Core i aplikacji konsoli Rejestrator jest używany do tworzenia dzienników z `Information` jako poziom.</span><span class="sxs-lookup"><span data-stu-id="98640-144">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="98640-145">*Poziom* dziennika wskazuje ważność rejestrowanego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="98640-145">The Log *level* indicates the severity of the logged event.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

<span data-ttu-id="98640-146">[Poziomy](#log-level) i [Kategorie](#log-category) zostały wyjaśnione bardziej szczegółowo w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="98640-146">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-3.0"

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="98640-147">Tworzenie dzienników w klasie programu</span><span class="sxs-lookup"><span data-stu-id="98640-147">Create logs in the Program class</span></span>

<span data-ttu-id="98640-148">Aby napisać dzienniki w klasie `Program` aplikacji ASP.NET Core, Pobierz wystąpienie `ILogger` od DI po skompilowaniu hosta:</span><span class="sxs-lookup"><span data-stu-id="98640-148">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="98640-149">Rejestrowanie podczas konstruowania hosta nie jest bezpośrednio obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="98640-149">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="98640-150">Można jednak użyć osobnego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="98640-150">However, a separate logger can be used.</span></span> <span data-ttu-id="98640-151">W poniższym przykładzie Rejestrator [Serilog](https://serilog.net/) jest używany do logowania się `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="98640-151">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateHostBuilder`.</span></span> <span data-ttu-id="98640-152">`AddSerilog` używa konfiguracji statycznej określonej w `Log.Logger`:</span><span class="sxs-lookup"><span data-stu-id="98640-152">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="98640-153">Tworzenie dzienników w klasie startowej</span><span class="sxs-lookup"><span data-stu-id="98640-153">Create logs in the Startup class</span></span>

<span data-ttu-id="98640-154">Aby napisać dzienniki w metodzie `Startup.Configure` aplikacji ASP.NET Core, należy uwzględnić parametr `ILogger` w podpisie metody:</span><span class="sxs-lookup"><span data-stu-id="98640-154">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="98640-155">Zapisywanie dzienników przed ukończeniem instalacji przez DI kontenera w metodzie `Startup.ConfigureServices` nie jest obsługiwane:</span><span class="sxs-lookup"><span data-stu-id="98640-155">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="98640-156">Iniekcja rejestratora do konstruktora `Startup` nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="98640-156">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="98640-157">Iniekcja rejestratora do sygnatury metody `Startup.ConfigureServices` nie jest obsługiwana</span><span class="sxs-lookup"><span data-stu-id="98640-157">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="98640-158">Przyczyną tego ograniczenia jest to, że rejestrowanie jest zależne od typu i konfiguracji, która z tego powodu zależy od DI.</span><span class="sxs-lookup"><span data-stu-id="98640-158">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="98640-159">Kontener DI nie jest ustawiany do momentu zakończenia `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="98640-159">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="98640-160">Iniekcja konstruktora rejestratora do `Startup` działa we wcześniejszych wersjach ASP.NET Core, ponieważ dla hosta sieci Web jest tworzony oddzielny kontener DI.</span><span class="sxs-lookup"><span data-stu-id="98640-160">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="98640-161">Aby uzyskać informacje o tym, dlaczego dla hosta generycznego jest tworzony tylko jeden kontener, zobacz [zawiadomienie o rozdzieleniu zmian](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="98640-161">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="98640-162">Jeśli trzeba skonfigurować usługę, która zależy od `ILogger<T>`, można to zrobić za pomocą iniekcji konstruktora lub dostarczając metodę fabryki.</span><span class="sxs-lookup"><span data-stu-id="98640-162">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="98640-163">Podejście metody fabryki jest zalecane tylko wtedy, gdy nie ma żadnych innych opcji.</span><span class="sxs-lookup"><span data-stu-id="98640-163">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="98640-164">Załóżmy na przykład, że musisz wypełnić Właściwość usługą z:</span><span class="sxs-lookup"><span data-stu-id="98640-164">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="98640-165">Poprzedni wyróżniony kod jest `Func`, który jest uruchamiany po raz pierwszy do utworzenia wystąpienia `MyService`.</span><span class="sxs-lookup"><span data-stu-id="98640-165">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="98640-166">W ten sposób można uzyskać dostęp do dowolnych zarejestrowanych usług.</span><span class="sxs-lookup"><span data-stu-id="98640-166">You can access any of the registered services in this way.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="98640-167">Tworzenie dzienników w programie startowym</span><span class="sxs-lookup"><span data-stu-id="98640-167">Create logs in Startup</span></span>

<span data-ttu-id="98640-168">Aby napisać dzienniki w klasie `Startup`, należy uwzględnić parametr `ILogger` w sygnaturze konstruktora:</span><span class="sxs-lookup"><span data-stu-id="98640-168">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="98640-169">Tworzenie dzienników w klasie programu</span><span class="sxs-lookup"><span data-stu-id="98640-169">Create logs in the Program class</span></span>

<span data-ttu-id="98640-170">Aby napisać dzienniki w klasie `Program`, Pobierz wystąpienie `ILogger` od DI:</span><span class="sxs-lookup"><span data-stu-id="98640-170">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="98640-171">Rejestrowanie podczas konstruowania hosta nie jest bezpośrednio obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="98640-171">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="98640-172">Można jednak użyć osobnego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="98640-172">However, a separate logger can be used.</span></span> <span data-ttu-id="98640-173">W poniższym przykładzie Rejestrator [Serilog](https://serilog.net/) jest używany do logowania się `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="98640-173">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateWebHostBuilder`.</span></span> <span data-ttu-id="98640-174">`AddSerilog` używa konfiguracji statycznej określonej w `Log.Logger`:</span><span class="sxs-lookup"><span data-stu-id="98640-174">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="98640-175">Brak metod rejestratora asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="98640-175">No asynchronous logger methods</span></span>

<span data-ttu-id="98640-176">Rejestracja powinna być tak szybka, że nie jest to koszt wydajności kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="98640-176">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="98640-177">Jeśli magazyn danych rejestrowania jest wolny, nie zapisuj go bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="98640-177">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="98640-178">Najpierw Rozważ zapisanie komunikatów dziennika do szybkiego sklepu, a następnie przeniesienie ich do wolnego magazynu później.</span><span class="sxs-lookup"><span data-stu-id="98640-178">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="98640-179">Na przykład jeśli rejestrujesz się do SQL Server, nie chcesz tego robić bezpośrednio w metodzie `Log`, ponieważ metody `Log` są synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="98640-179">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="98640-180">Zamiast tego można synchronicznie dodawać komunikaty dziennika do kolejki w pamięci, a proces roboczy w tle ściągał komunikaty z kolejki, aby wykonać asynchroniczne działanie wypychania danych do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="98640-180">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="98640-181">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="98640-181">Configuration</span></span>

<span data-ttu-id="98640-182">Konfiguracja dostawcy rejestrowania jest świadczona przez co najmniej jednego dostawcę konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="98640-182">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="98640-183">Formaty plików (INI, JSON i XML).</span><span class="sxs-lookup"><span data-stu-id="98640-183">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="98640-184">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="98640-184">Command-line arguments.</span></span>
* <span data-ttu-id="98640-185">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="98640-185">Environment variables.</span></span>
* <span data-ttu-id="98640-186">Obiekty platformy .NET w pamięci.</span><span class="sxs-lookup"><span data-stu-id="98640-186">In-memory .NET objects.</span></span>
* <span data-ttu-id="98640-187">Magazyn niezaszyfrowanego [klucza tajnego](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="98640-187">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="98640-188">Zaszyfrowany magazyn użytkowników, taki jak [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="98640-188">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="98640-189">Dostawcy niestandardowi (instalowani lub utworzony).</span><span class="sxs-lookup"><span data-stu-id="98640-189">Custom providers (installed or created).</span></span>

<span data-ttu-id="98640-190">Na przykład konfiguracja rejestrowania jest zwykle udostępniana przez sekcję `Logging` plików ustawień aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-190">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="98640-191">Poniższy przykład pokazuje zawartość typowej wartości *appSettings. Plik Development. JSON* :</span><span class="sxs-lookup"><span data-stu-id="98640-191">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="98640-192">Właściwość `Logging` może mieć `LogLevel` i właściwości dostawcy dzienników (wyświetlana jest konsola).</span><span class="sxs-lookup"><span data-stu-id="98640-192">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="98640-193">Właściwość `LogLevel` w obszarze `Logging` określa minimalny [poziom](#log-level) rejestrowania wybranych kategorii.</span><span class="sxs-lookup"><span data-stu-id="98640-193">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="98640-194">W przykładzie `System` i `Microsoft` kategorii są rejestrowane na poziomie `Information`, a wszystkie inne będą logować się na poziomie `Debug`.</span><span class="sxs-lookup"><span data-stu-id="98640-194">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="98640-195">Inne właściwości w obszarze `Logging` określają dostawców rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="98640-195">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="98640-196">Przykład dotyczy dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="98640-196">The example is for the Console provider.</span></span> <span data-ttu-id="98640-197">Jeśli dostawca obsługuje [zakresy dzienników](#log-scopes), `IncludeScopes` wskazuje, czy są one włączone.</span><span class="sxs-lookup"><span data-stu-id="98640-197">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="98640-198">Właściwość dostawcy (taka jak `Console` w przykładzie) może również określać Właściwość `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="98640-198">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="98640-199">`LogLevel` w obszarze dostawcy Określa poziomy do rejestrowania dla tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="98640-199">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="98640-200">Jeśli poziomy są określone w `Logging.{providername}.LogLevel`, zastępują wszystkie elementy ustawione w `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="98640-200">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

<span data-ttu-id="98640-201">Aby uzyskać informacje na temat implementowania dostawców konfiguracji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="98640-201">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="98640-202">Przykładowe dane wyjściowe rejestrowania</span><span class="sxs-lookup"><span data-stu-id="98640-202">Sample logging output</span></span>

<span data-ttu-id="98640-203">W przypadku przykładowego kodu podanego w poprzedniej sekcji Dzienniki są wyświetlane w konsoli programu, gdy aplikacja jest uruchamiana z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="98640-203">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="98640-204">Oto przykład danych wyjściowych konsoli:</span><span class="sxs-lookup"><span data-stu-id="98640-204">Here's an example of console output:</span></span>

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

<span data-ttu-id="98640-205">Poprzednie dzienniki zostały wygenerowane przez utworzenie żądania HTTP GET do przykładowej aplikacji w `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="98640-205">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="98640-206">Oto przykład tych samych dzienników, które są wyświetlane w oknie debugowania podczas uruchamiania przykładowej aplikacji w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="98640-206">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

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

<span data-ttu-id="98640-207">Dzienniki utworzone za pomocą wywołań `ILogger` przedstawionych w poprzedniej sekcji zaczynają się od "TodoApiSample".</span><span class="sxs-lookup"><span data-stu-id="98640-207">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="98640-208">Dzienniki zaczynające się od kategorii "Microsoft" pochodzą z kodu ASP.NET Core Framework.</span><span class="sxs-lookup"><span data-stu-id="98640-208">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="98640-209">ASP.NET Core i kod aplikacji używają tego samego rejestrowania interfejsu API i dostawców.</span><span class="sxs-lookup"><span data-stu-id="98640-209">ASP.NET Core and application code are using the same logging API and providers.</span></span>

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

<span data-ttu-id="98640-210">Dzienniki utworzone za pomocą wywołań `ILogger` przedstawionych w poprzedniej sekcji zaczynają się od "TodoApi".</span><span class="sxs-lookup"><span data-stu-id="98640-210">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="98640-211">Dzienniki zaczynające się od kategorii "Microsoft" pochodzą z kodu ASP.NET Core Framework.</span><span class="sxs-lookup"><span data-stu-id="98640-211">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="98640-212">ASP.NET Core i kod aplikacji używają tego samego rejestrowania interfejsu API i dostawców.</span><span class="sxs-lookup"><span data-stu-id="98640-212">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

<span data-ttu-id="98640-213">W pozostałej części tego artykułu opisano niektóre szczegóły i opcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="98640-213">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="98640-214">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="98640-214">NuGet packages</span></span>

<span data-ttu-id="98640-215">Interfejsy `ILogger` i `ILoggerFactory` są w [Microsoft. Extensions. Logging. Abstracts](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)i domyślne implementacje dla nich są w [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="98640-215">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="98640-216">Kategoria dziennika</span><span class="sxs-lookup"><span data-stu-id="98640-216">Log category</span></span>

<span data-ttu-id="98640-217">Po utworzeniu obiektu `ILogger` jest dla niego określona *Kategoria* .</span><span class="sxs-lookup"><span data-stu-id="98640-217">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="98640-218">Ta kategoria jest dołączona do każdego komunikatu dziennika utworzonego przez to wystąpienie `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="98640-218">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="98640-219">Kategoria może być dowolnym ciągiem, ale Konwencja ma używać nazwy klasy, takiej jak "TodoApi. controllers. TodoController".</span><span class="sxs-lookup"><span data-stu-id="98640-219">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="98640-220">Użyj `ILogger<T>`, aby uzyskać wystąpienie `ILogger` używające w pełni kwalifikowanej nazwy typu `T` jako kategorii:</span><span class="sxs-lookup"><span data-stu-id="98640-220">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="98640-221">Aby jawnie określić kategorię, wywołaj `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="98640-221">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="98640-222">`ILogger<T>` jest równoważne wywołaniu `CreateLogger` z w pełni kwalifikowaną nazwą typu `T`.</span><span class="sxs-lookup"><span data-stu-id="98640-222">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="98640-223">Poziom dziennika</span><span class="sxs-lookup"><span data-stu-id="98640-223">Log level</span></span>

<span data-ttu-id="98640-224">Każdy dziennik Określa wartość <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="98640-224">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="98640-225">Poziom dziennika wskazuje ważność lub ważność.</span><span class="sxs-lookup"><span data-stu-id="98640-225">The log level indicates the severity or importance.</span></span> <span data-ttu-id="98640-226">Przykładowo można napisać dziennik `Information`, gdy metoda zostanie zakończona normalnie, a dziennik `Warning`, gdy metoda zwraca *404 nie znaleziono* kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="98640-226">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="98640-227">Poniższy kod tworzy `Information` i `Warning` dzienników:</span><span class="sxs-lookup"><span data-stu-id="98640-227">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="98640-228">W powyższym kodzie pierwszy parametr jest [identyfikatorem zdarzenia dziennika](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="98640-228">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="98640-229">Drugi parametr jest szablonem wiadomości z symbolami zastępczymi dla wartości argumentów dostarczonych przez pozostałe parametry metody.</span><span class="sxs-lookup"><span data-stu-id="98640-229">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="98640-230">Parametry metody zostały wyjaśnione w [sekcji szablon komunikatu](#log-message-template) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="98640-230">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="98640-231">Metody rejestrowania, które obejmują poziom w nazwie metody (na przykład `LogInformation` i `LogWarning`) są [metodami rozszerzenia dla ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="98640-231">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="98640-232">Te metody wywołują metodę `Log`, która przyjmuje parametr `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="98640-232">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="98640-233">Metodę `Log` można wywołać bezpośrednio zamiast jednej z tych metod rozszerzających, ale składnia jest stosunkowo skomplikowana.</span><span class="sxs-lookup"><span data-stu-id="98640-233">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="98640-234">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.Extensions.Logging.ILogger> i [kod źródłowy rozszerzeń rejestratora](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="98640-234">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="98640-235">ASP.NET Core definiuje następujące poziomy dziennika uporządkowane w tym miejscu od najniższej do najwyższej wagi.</span><span class="sxs-lookup"><span data-stu-id="98640-235">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="98640-236">Ślad = 0</span><span class="sxs-lookup"><span data-stu-id="98640-236">Trace = 0</span></span>

  <span data-ttu-id="98640-237">Aby uzyskać informacje, które są zazwyczaj cenne tylko dla debugowania.</span><span class="sxs-lookup"><span data-stu-id="98640-237">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="98640-238">Komunikaty te mogą zawierać poufne dane aplikacji, dlatego nie powinny być włączone w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="98640-238">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="98640-239">*Domyślnie wyłączona.*</span><span class="sxs-lookup"><span data-stu-id="98640-239">*Disabled by default.*</span></span>

* <span data-ttu-id="98640-240">Debuguj = 1</span><span class="sxs-lookup"><span data-stu-id="98640-240">Debug = 1</span></span>

  <span data-ttu-id="98640-241">Informacje, które mogą być przydatne podczas tworzenia i debugowania.</span><span class="sxs-lookup"><span data-stu-id="98640-241">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="98640-242">Przykład: `Entering method Configure with flag set to true.` Włącz dzienniki poziomu `Debug` w środowisku produkcyjnym tylko podczas rozwiązywania problemów, ze względu na dużą ilość dzienników.</span><span class="sxs-lookup"><span data-stu-id="98640-242">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="98640-243">Informacje = 2</span><span class="sxs-lookup"><span data-stu-id="98640-243">Information = 2</span></span>

  <span data-ttu-id="98640-244">Do śledzenia ogólnego przepływu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-244">For tracking the general flow of the app.</span></span> <span data-ttu-id="98640-245">Te dzienniki zwykle mają pewną wartość długoterminową.</span><span class="sxs-lookup"><span data-stu-id="98640-245">These logs typically have some long-term value.</span></span> <span data-ttu-id="98640-246">Przykład: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="98640-246">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="98640-247">Ostrzeżenie = 3</span><span class="sxs-lookup"><span data-stu-id="98640-247">Warning = 3</span></span>

  <span data-ttu-id="98640-248">Dla nietypowych lub nieoczekiwanych zdarzeń w przepływie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-248">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="98640-249">Mogą to być błędy lub inne warunki, które nie powodują zatrzymania aplikacji, ale konieczne może być zbadanie.</span><span class="sxs-lookup"><span data-stu-id="98640-249">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="98640-250">Obsłużone wyjątki są typowym miejscem do korzystania z poziomu dziennika `Warning`.</span><span class="sxs-lookup"><span data-stu-id="98640-250">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="98640-251">Przykład: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="98640-251">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="98640-252">Błąd = 4</span><span class="sxs-lookup"><span data-stu-id="98640-252">Error = 4</span></span>

  <span data-ttu-id="98640-253">W przypadku błędów i wyjątków, których nie można obsłużyć.</span><span class="sxs-lookup"><span data-stu-id="98640-253">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="98640-254">Te komunikaty wskazują niepowodzenie w bieżącym działaniu lub operacji (np. bieżące żądanie HTTP), a nie awaria całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-254">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="98640-255">Przykładowy komunikat dziennika: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="98640-255">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="98640-256">Krytyczne = 5</span><span class="sxs-lookup"><span data-stu-id="98640-256">Critical = 5</span></span>

  <span data-ttu-id="98640-257">Dla niepowodzeń, które wymagają natychmiastowej uwagi.</span><span class="sxs-lookup"><span data-stu-id="98640-257">For failures that require immediate attention.</span></span> <span data-ttu-id="98640-258">Przykłady: scenariusze utraty danych, brak miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="98640-258">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="98640-259">Poziom dziennika służy do kontrolowania, ile danych wyjściowych dziennika jest zapisywana w określonym nośniku lub oknie wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="98640-259">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="98640-260">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="98640-260">For example:</span></span>

* <span data-ttu-id="98640-261">W środowisku produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="98640-261">In production:</span></span>
  * <span data-ttu-id="98640-262">Rejestrowanie na poziomie `Trace` za pomocą poziomów `Information` powoduje utworzenie dużej ilości szczegółowych komunikatów dziennika.</span><span class="sxs-lookup"><span data-stu-id="98640-262">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="98640-263">Aby kontrolować koszty i nie przekraczać limitów magazynowania danych, należy rejestrować `Trace` przez komunikaty poziomu `Information` do magazynu o wysokim poziomie ilości danych.</span><span class="sxs-lookup"><span data-stu-id="98640-263">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="98640-264">Rejestrowanie w `Warning` do `Critical` poziomów zwykle generuje mniejszą liczbę mniejszych komunikatów dzienników.</span><span class="sxs-lookup"><span data-stu-id="98640-264">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="98640-265">W związku z tym koszty i limity magazynu zazwyczaj nie są problemem, co skutkuje większą elastycznością wyboru magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="98640-265">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="98640-266">Podczas tworzenia:</span><span class="sxs-lookup"><span data-stu-id="98640-266">During development:</span></span>
  * <span data-ttu-id="98640-267">Rejestruj `Warning` za pomocą komunikatów `Critical` do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="98640-267">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="98640-268">Dodaj `Trace` do `Information` komunikatów podczas rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="98640-268">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="98640-269">W sekcji [filtrowanie dzienników](#log-filtering) w dalszej części tego artykułu wyjaśniono, jak kontrolować poziomy dzienników obsługiwane przez dostawcę.</span><span class="sxs-lookup"><span data-stu-id="98640-269">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="98640-270">ASP.NET Core zapisuje dzienniki dla zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="98640-270">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="98640-271">W przykładach dzienników wymienionych wcześniej w tym artykule wykluczono dzienniki poniżej `Information` poziomu, dlatego nie zostały utworzone dzienniki `Debug` lub `Trace`.</span><span class="sxs-lookup"><span data-stu-id="98640-271">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="98640-272">Oto przykład dzienników konsoli utworzonych przez uruchomienie przykładowej aplikacji skonfigurowanej do wyświetlania dzienników `Debug`:</span><span class="sxs-lookup"><span data-stu-id="98640-272">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="98640-273">Identyfikator zdarzenia dziennika</span><span class="sxs-lookup"><span data-stu-id="98640-273">Log event ID</span></span>

<span data-ttu-id="98640-274">Każdy dziennik może określać *Identyfikator zdarzenia*.</span><span class="sxs-lookup"><span data-stu-id="98640-274">Each log can specify an *event ID*.</span></span> <span data-ttu-id="98640-275">Aplikacja Przykładowa wykonuje to przy użyciu lokalnie zdefiniowanej klasy `LoggingEvents`:</span><span class="sxs-lookup"><span data-stu-id="98640-275">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="98640-276">Identyfikator zdarzenia kojarzy zestaw zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="98640-276">An event ID associates a set of events.</span></span> <span data-ttu-id="98640-277">Na przykład wszystkie dzienniki związane z wyświetlaniem listy elementów na stronie mogą być 1001.</span><span class="sxs-lookup"><span data-stu-id="98640-277">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="98640-278">Dostawca rejestrowania może przechowywać identyfikator zdarzenia w polu identyfikatora, w komunikacie rejestrowania lub wcale.</span><span class="sxs-lookup"><span data-stu-id="98640-278">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="98640-279">Dostawca debugowania nie pokazuje identyfikatorów zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="98640-279">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="98640-280">Dostawca konsoli pokazuje identyfikatory zdarzeń w nawiasach po kategorii:</span><span class="sxs-lookup"><span data-stu-id="98640-280">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="98640-281">Szablon komunikatu dziennika</span><span class="sxs-lookup"><span data-stu-id="98640-281">Log message template</span></span>

<span data-ttu-id="98640-282">Każdy dziennik Określa szablon wiadomości.</span><span class="sxs-lookup"><span data-stu-id="98640-282">Each log specifies a message template.</span></span> <span data-ttu-id="98640-283">Szablon wiadomości może zawierać symbole zastępcze, dla których podano argumenty.</span><span class="sxs-lookup"><span data-stu-id="98640-283">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="98640-284">Użyj nazw dla symboli zastępczych, a nie liczby.</span><span class="sxs-lookup"><span data-stu-id="98640-284">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="98640-285">Kolejność symboli zastępczych, nie ich nazw, określa, które parametry są używane do dostarczania ich wartości.</span><span class="sxs-lookup"><span data-stu-id="98640-285">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="98640-286">W poniższym kodzie Zwróć uwagę, że nazwy parametrów są poza kolejnością w szablonie wiadomości:</span><span class="sxs-lookup"><span data-stu-id="98640-286">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="98640-287">Ten kod tworzy komunikat dziennika z wartościami parametrów w kolejności:</span><span class="sxs-lookup"><span data-stu-id="98640-287">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="98640-288">Struktura rejestrowania działa w ten sposób, aby dostawcy rejestrowania mogli zaimplementować [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="98640-288">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="98640-289">Same argumenty są przesyłane do systemu rejestrowania, a nie tylko dla sformatowanego szablonu wiadomości.</span><span class="sxs-lookup"><span data-stu-id="98640-289">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="98640-290">Te informacje umożliwiają dostawcom rejestrowania przechowywanie wartości parametrów jako pól.</span><span class="sxs-lookup"><span data-stu-id="98640-290">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="98640-291">Na przykład załóżmy, że wywołania metod rejestratora wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="98640-291">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="98640-292">W przypadku wysyłania dzienników do usługi Azure Table Storage każda jednostka tabeli platformy Azure może mieć właściwości `ID` i `RequestTime`, które upraszczają zapytania dotyczące danych dziennika.</span><span class="sxs-lookup"><span data-stu-id="98640-292">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="98640-293">Zapytanie może znaleźć wszystkie dzienniki w określonym zakresie `RequestTime` bez analizowania limitu czasu wiadomości tekstowej.</span><span class="sxs-lookup"><span data-stu-id="98640-293">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="98640-294">Wyjątki rejestrowania</span><span class="sxs-lookup"><span data-stu-id="98640-294">Logging exceptions</span></span>

<span data-ttu-id="98640-295">Metody rejestratora mają przeciążenia umożliwiające przekazanie wyjątku, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="98640-295">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="98640-296">Różni dostawcy obsługują informacje o wyjątkach na różne sposoby.</span><span class="sxs-lookup"><span data-stu-id="98640-296">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="98640-297">Oto przykład danych wyjściowych dostawcy debugowania z kodu pokazanego powyżej.</span><span class="sxs-lookup"><span data-stu-id="98640-297">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="98640-298">Filtrowanie dzienników</span><span class="sxs-lookup"><span data-stu-id="98640-298">Log filtering</span></span>

<span data-ttu-id="98640-299">Można określić minimalny poziom rejestrowania dla określonego dostawcy i kategorii lub dla wszystkich dostawców lub wszystkich kategorii.</span><span class="sxs-lookup"><span data-stu-id="98640-299">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="98640-300">Wszystkie dzienniki poniżej minimalnego poziomu nie są przesyłane do tego dostawcy, więc nie są wyświetlane ani przechowywane.</span><span class="sxs-lookup"><span data-stu-id="98640-300">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="98640-301">Aby pominąć wszystkie dzienniki, określ `LogLevel.None` jako minimalny poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="98640-301">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="98640-302">Wartość całkowita `LogLevel.None` to 6, która jest większa niż `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="98640-302">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="98640-303">Utwórz reguły filtru w konfiguracji</span><span class="sxs-lookup"><span data-stu-id="98640-303">Create filter rules in configuration</span></span>

<span data-ttu-id="98640-304">Kod szablonu projektu wywołuje `CreateDefaultBuilder` w celu skonfigurowania rejestrowania dla konsoli i dostawców debugowania.</span><span class="sxs-lookup"><span data-stu-id="98640-304">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="98640-305">Metoda `CreateDefaultBuilder` konfiguruje rejestrowanie, aby wyszukać konfigurację w sekcji `Logging`, jak wyjaśniono [wcześniej w tym artykule](#configuration).</span><span class="sxs-lookup"><span data-stu-id="98640-305">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="98640-306">Dane konfiguracyjne określają minimalne poziomy dziennika według dostawcy i kategorii, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="98640-306">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

::: moniker-end

<span data-ttu-id="98640-307">Ten kod JSON tworzy sześć reguł filtrowania: jeden dla dostawcy debugowania, cztery dla dostawcy konsoli i jeden dla wszystkich dostawców.</span><span class="sxs-lookup"><span data-stu-id="98640-307">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="98640-308">Dla każdego dostawcy wybierana jest pojedyncza reguła, gdy zostanie utworzony obiekt `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="98640-308">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="98640-309">Filtrowanie reguł w kodzie</span><span class="sxs-lookup"><span data-stu-id="98640-309">Filter rules in code</span></span>

<span data-ttu-id="98640-310">Poniższy przykład pokazuje, jak zarejestrować reguły filtru w kodzie:</span><span class="sxs-lookup"><span data-stu-id="98640-310">The following example shows how to register filter rules in code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

<span data-ttu-id="98640-311">Drugi `AddFilter` określa dostawcę debugowania za pomocą nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="98640-311">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="98640-312">Pierwszy `AddFilter` dotyczy wszystkich dostawców, ponieważ nie określa typu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="98640-312">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="98640-313">Jak są stosowane reguły filtrowania</span><span class="sxs-lookup"><span data-stu-id="98640-313">How filtering rules are applied</span></span>

<span data-ttu-id="98640-314">Dane konfiguracji i kod `AddFilter` pokazane w powyższych przykładach tworzą reguły przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="98640-314">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="98640-315">Pierwsze sześć pochodzi z przykładu konfiguracji, a ostatnie dwa pochodzą z przykładu kodu.</span><span class="sxs-lookup"><span data-stu-id="98640-315">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="98640-316">Wartość liczbowa</span><span class="sxs-lookup"><span data-stu-id="98640-316">Number</span></span> | <span data-ttu-id="98640-317">Dostawcy</span><span class="sxs-lookup"><span data-stu-id="98640-317">Provider</span></span>      | <span data-ttu-id="98640-318">Kategorie zaczynające się od...</span><span class="sxs-lookup"><span data-stu-id="98640-318">Categories that begin with ...</span></span>          | <span data-ttu-id="98640-319">Minimalny poziom rejestrowania</span><span class="sxs-lookup"><span data-stu-id="98640-319">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="98640-320">1</span><span class="sxs-lookup"><span data-stu-id="98640-320">1</span></span>      | <span data-ttu-id="98640-321">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="98640-321">Debug</span></span>         | <span data-ttu-id="98640-322">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="98640-322">All categories</span></span>                          | <span data-ttu-id="98640-323">Informacje</span><span class="sxs-lookup"><span data-stu-id="98640-323">Information</span></span>       |
| <span data-ttu-id="98640-324">2</span><span class="sxs-lookup"><span data-stu-id="98640-324">2</span></span>      | <span data-ttu-id="98640-325">Konsola</span><span class="sxs-lookup"><span data-stu-id="98640-325">Console</span></span>       | <span data-ttu-id="98640-326">Microsoft. AspNetCore. MVC. Razor. Internal</span><span class="sxs-lookup"><span data-stu-id="98640-326">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="98640-327">Ostrzeżenie</span><span class="sxs-lookup"><span data-stu-id="98640-327">Warning</span></span>           |
| <span data-ttu-id="98640-328">3</span><span class="sxs-lookup"><span data-stu-id="98640-328">3</span></span>      | <span data-ttu-id="98640-329">Konsola</span><span class="sxs-lookup"><span data-stu-id="98640-329">Console</span></span>       | <span data-ttu-id="98640-330">Microsoft. AspNetCore. MVC. Razor. Razor</span><span class="sxs-lookup"><span data-stu-id="98640-330">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="98640-331">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="98640-331">Debug</span></span>             |
| <span data-ttu-id="98640-332">4</span><span class="sxs-lookup"><span data-stu-id="98640-332">4</span></span>      | <span data-ttu-id="98640-333">Konsola</span><span class="sxs-lookup"><span data-stu-id="98640-333">Console</span></span>       | <span data-ttu-id="98640-334">Microsoft. AspNetCore. MVC. Razor</span><span class="sxs-lookup"><span data-stu-id="98640-334">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="98640-335">Błąd</span><span class="sxs-lookup"><span data-stu-id="98640-335">Error</span></span>             |
| <span data-ttu-id="98640-336">5</span><span class="sxs-lookup"><span data-stu-id="98640-336">5</span></span>      | <span data-ttu-id="98640-337">Konsola</span><span class="sxs-lookup"><span data-stu-id="98640-337">Console</span></span>       | <span data-ttu-id="98640-338">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="98640-338">All categories</span></span>                          | <span data-ttu-id="98640-339">Informacje</span><span class="sxs-lookup"><span data-stu-id="98640-339">Information</span></span>       |
| <span data-ttu-id="98640-340">6</span><span class="sxs-lookup"><span data-stu-id="98640-340">6</span></span>      | <span data-ttu-id="98640-341">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="98640-341">All providers</span></span> | <span data-ttu-id="98640-342">Wszystkie kategorie</span><span class="sxs-lookup"><span data-stu-id="98640-342">All categories</span></span>                          | <span data-ttu-id="98640-343">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="98640-343">Debug</span></span>             |
| <span data-ttu-id="98640-344">7</span><span class="sxs-lookup"><span data-stu-id="98640-344">7</span></span>      | <span data-ttu-id="98640-345">Wszyscy dostawcy</span><span class="sxs-lookup"><span data-stu-id="98640-345">All providers</span></span> | <span data-ttu-id="98640-346">System</span><span class="sxs-lookup"><span data-stu-id="98640-346">System</span></span>                                  | <span data-ttu-id="98640-347">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="98640-347">Debug</span></span>             |
| <span data-ttu-id="98640-348">8</span><span class="sxs-lookup"><span data-stu-id="98640-348">8</span></span>      | <span data-ttu-id="98640-349">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="98640-349">Debug</span></span>         | <span data-ttu-id="98640-350">Microsoft</span><span class="sxs-lookup"><span data-stu-id="98640-350">Microsoft</span></span>                               | <span data-ttu-id="98640-351">Szuka</span><span class="sxs-lookup"><span data-stu-id="98640-351">Trace</span></span>             |

<span data-ttu-id="98640-352">Po utworzeniu obiektu `ILogger` obiekt `ILoggerFactory` wybiera jedną regułę dla każdego dostawcy do zastosowania do tego rejestratora.</span><span class="sxs-lookup"><span data-stu-id="98640-352">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="98640-353">Wszystkie komunikaty zapisywane przez wystąpienie `ILogger` są filtrowane na podstawie wybranych reguł.</span><span class="sxs-lookup"><span data-stu-id="98640-353">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="98640-354">Najbardziej konkretną regułą można wybrać dla każdego dostawcy i pary kategorii z dostępnych reguł.</span><span class="sxs-lookup"><span data-stu-id="98640-354">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="98640-355">Następujący algorytm jest używany dla każdego dostawcy, gdy dla danej kategorii jest tworzony `ILogger`:</span><span class="sxs-lookup"><span data-stu-id="98640-355">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="98640-356">Wybierz wszystkie reguły, które pasują do dostawcy lub jego aliasu.</span><span class="sxs-lookup"><span data-stu-id="98640-356">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="98640-357">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły z pustym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="98640-357">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="98640-358">W wyniku poprzedniego kroku wybierz pozycję reguły z najdłuższym prefiksem kategorii.</span><span class="sxs-lookup"><span data-stu-id="98640-358">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="98640-359">Jeśli nie zostanie znalezione dopasowanie, zaznacz wszystkie reguły, które nie określają kategorii.</span><span class="sxs-lookup"><span data-stu-id="98640-359">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="98640-360">Jeśli wybrano wiele reguł, zrób to **ostatnie** .</span><span class="sxs-lookup"><span data-stu-id="98640-360">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="98640-361">Jeśli nie wybrano żadnych reguł, użyj `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="98640-361">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="98640-362">Na powyższej liście reguł Załóżmy, że tworzysz obiekt `ILogger` dla kategorii "Microsoft. AspNetCore. MVC. Razor. RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="98640-362">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="98640-363">Dla dostawcy debugowania obowiązują reguły 1, 6 i 8.</span><span class="sxs-lookup"><span data-stu-id="98640-363">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="98640-364">Reguła 8 jest najbardziej specyficzna, więc jest to jedna wybrana.</span><span class="sxs-lookup"><span data-stu-id="98640-364">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="98640-365">W przypadku dostawcy konsoli obowiązują reguły 3, 4, 5 i 6.</span><span class="sxs-lookup"><span data-stu-id="98640-365">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="98640-366">Reguła 3 jest najbardziej specyficzna.</span><span class="sxs-lookup"><span data-stu-id="98640-366">Rule 3 is most specific.</span></span>

<span data-ttu-id="98640-367">Wystąpienie `ILogger` wysyła dzienniki poziomu `Trace` i powyżej do dostawcy debugowania.</span><span class="sxs-lookup"><span data-stu-id="98640-367">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="98640-368">Dzienniki poziomu `Debug` i nowsze są wysyłane do dostawcy konsoli.</span><span class="sxs-lookup"><span data-stu-id="98640-368">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="98640-369">Aliasy dostawców</span><span class="sxs-lookup"><span data-stu-id="98640-369">Provider aliases</span></span>

<span data-ttu-id="98640-370">Każdy dostawca definiuje *alias* , który może być używany w konfiguracji zamiast w pełni kwalifikowanej nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="98640-370">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="98640-371">W przypadku dostawców wbudowanych Użyj następujących aliasów:</span><span class="sxs-lookup"><span data-stu-id="98640-371">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="98640-372">Konsola</span><span class="sxs-lookup"><span data-stu-id="98640-372">Console</span></span>
* <span data-ttu-id="98640-373">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="98640-373">Debug</span></span>
* <span data-ttu-id="98640-374">EventSource</span><span class="sxs-lookup"><span data-stu-id="98640-374">EventSource</span></span>
* <span data-ttu-id="98640-375">Elemencie</span><span class="sxs-lookup"><span data-stu-id="98640-375">EventLog</span></span>
* <span data-ttu-id="98640-376">TraceSource</span><span class="sxs-lookup"><span data-stu-id="98640-376">TraceSource</span></span>
* <span data-ttu-id="98640-377">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="98640-377">AzureAppServicesFile</span></span>
* <span data-ttu-id="98640-378">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="98640-378">AzureAppServicesBlob</span></span>
* <span data-ttu-id="98640-379">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="98640-379">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="98640-380">Domyślny poziom minimalny</span><span class="sxs-lookup"><span data-stu-id="98640-380">Default minimum level</span></span>

<span data-ttu-id="98640-381">Istnieje ustawienie minimalnego poziomu, które działa tylko wtedy, gdy nie mają zastosowania żadne reguły z konfiguracji lub kodu dla danego dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="98640-381">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="98640-382">Poniższy przykład pokazuje, jak ustawić poziom minimalny:</span><span class="sxs-lookup"><span data-stu-id="98640-382">The following example shows how to set the minimum level:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

<span data-ttu-id="98640-383">Jeśli poziom minimalny nie został jawnie ustawiony, wartość domyślna to `Information`, co oznacza, że `Trace` i `Debug` dzienników są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="98640-383">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="98640-384">Funkcje filtrowania</span><span class="sxs-lookup"><span data-stu-id="98640-384">Filter functions</span></span>

<span data-ttu-id="98640-385">Funkcja filtru jest wywoływana dla wszystkich dostawców i kategorii, które nie mają przypisanych do nich reguł przez konfigurację lub kod.</span><span class="sxs-lookup"><span data-stu-id="98640-385">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="98640-386">Kod w funkcji ma dostęp do typu dostawcy, kategorii i poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="98640-386">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="98640-387">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="98640-387">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="98640-388">Kategorie i poziomy systemu</span><span class="sxs-lookup"><span data-stu-id="98640-388">System categories and levels</span></span>

<span data-ttu-id="98640-389">Poniżej przedstawiono niektóre kategorie używane przez ASP.NET Core i Entity Framework Core, z informacjami o dziennikach, od których należy się spodziewać:</span><span class="sxs-lookup"><span data-stu-id="98640-389">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="98640-390">Kategoria</span><span class="sxs-lookup"><span data-stu-id="98640-390">Category</span></span>                            | <span data-ttu-id="98640-391">Uwagi</span><span class="sxs-lookup"><span data-stu-id="98640-391">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="98640-392">Microsoft. AspNetCore</span><span class="sxs-lookup"><span data-stu-id="98640-392">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="98640-393">Ogólna Diagnostyka ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98640-393">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="98640-394">Microsoft. AspNetCore. dataprotection</span><span class="sxs-lookup"><span data-stu-id="98640-394">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="98640-395">Które klucze zostały wzięte pod uwagę, znaleziono i użyte.</span><span class="sxs-lookup"><span data-stu-id="98640-395">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="98640-396">Microsoft. AspNetCore. HostFiltering</span><span class="sxs-lookup"><span data-stu-id="98640-396">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="98640-397">Dozwolone hosty.</span><span class="sxs-lookup"><span data-stu-id="98640-397">Hosts allowed.</span></span> |
| <span data-ttu-id="98640-398">Microsoft. AspNetCore. hosting</span><span class="sxs-lookup"><span data-stu-id="98640-398">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="98640-399">Jak długo trwa wykonywanie żądań HTTP i czas ich uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="98640-399">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="98640-400">Które hostowanie zestawów uruchamiania zostało załadowane.</span><span class="sxs-lookup"><span data-stu-id="98640-400">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="98640-401">Microsoft. AspNetCore. MVC</span><span class="sxs-lookup"><span data-stu-id="98640-401">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="98640-402">Diagnostyka MVC i Razor.</span><span class="sxs-lookup"><span data-stu-id="98640-402">MVC and Razor diagnostics.</span></span> <span data-ttu-id="98640-403">Powiązanie modelu, wykonywanie filtru, kompilacja widoku, wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="98640-403">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="98640-404">Microsoft. AspNetCore. Routing</span><span class="sxs-lookup"><span data-stu-id="98640-404">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="98640-405">Informacje o trasie.</span><span class="sxs-lookup"><span data-stu-id="98640-405">Route matching information.</span></span> |
| <span data-ttu-id="98640-406">Microsoft. AspNetCore. Server</span><span class="sxs-lookup"><span data-stu-id="98640-406">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="98640-407">Reagowanie na uruchamianie, zatrzymywanie i utrzymywanie aktywności.</span><span class="sxs-lookup"><span data-stu-id="98640-407">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="98640-408">Informacje o certyfikacie HTTPS.</span><span class="sxs-lookup"><span data-stu-id="98640-408">HTTPS certificate information.</span></span> |
| <span data-ttu-id="98640-409">Microsoft. AspNetCore. StaticFiles</span><span class="sxs-lookup"><span data-stu-id="98640-409">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="98640-410">Obsługiwane pliki.</span><span class="sxs-lookup"><span data-stu-id="98640-410">Files served.</span></span> |
| <span data-ttu-id="98640-411">Microsoft. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="98640-411">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="98640-412">Ogólna Diagnostyka Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="98640-412">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="98640-413">Aktywność i Konfiguracja bazy danych, wykrywanie zmian, migracje.</span><span class="sxs-lookup"><span data-stu-id="98640-413">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="98640-414">Zakresy dziennika</span><span class="sxs-lookup"><span data-stu-id="98640-414">Log scopes</span></span>

 <span data-ttu-id="98640-415">*Zakres* może grupować zestaw operacji logicznych.</span><span class="sxs-lookup"><span data-stu-id="98640-415">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="98640-416">Takie grupowanie może służyć do dołączania tych samych danych do każdego dziennika, który został utworzony jako część zestawu.</span><span class="sxs-lookup"><span data-stu-id="98640-416">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="98640-417">Na przykład każdy dziennik utworzony w ramach przetwarzania transakcji może zawierać identyfikator transakcji.</span><span class="sxs-lookup"><span data-stu-id="98640-417">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="98640-418">Zakres jest typem `IDisposable`, który jest zwracany przez metodę <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> i obowiązuje do momentu jego usunięcia.</span><span class="sxs-lookup"><span data-stu-id="98640-418">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="98640-419">Użyj zakresu przez Zawijanie wywołań rejestratora w bloku `using`:</span><span class="sxs-lookup"><span data-stu-id="98640-419">Use a scope by wrapping logger calls in a `using` block:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

<span data-ttu-id="98640-420">Poniższy kod włącza zakresy dla dostawcy konsoli:</span><span class="sxs-lookup"><span data-stu-id="98640-420">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="98640-421">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="98640-421">*Program.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="98640-422">Skonfigurowanie opcji rejestratora konsoli `IncludeScopes` jest wymagane do włączenia rejestrowania na podstawie zakresu.</span><span class="sxs-lookup"><span data-stu-id="98640-422">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="98640-423">Informacje o konfiguracji znajdują się w sekcji [Konfiguracja](#configuration) .</span><span class="sxs-lookup"><span data-stu-id="98640-423">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="98640-424">Każdy komunikat dziennika zawiera informacje o zakresie:</span><span class="sxs-lookup"><span data-stu-id="98640-424">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="98640-425">Wbudowani dostawcy rejestrowania</span><span class="sxs-lookup"><span data-stu-id="98640-425">Built-in logging providers</span></span>

<span data-ttu-id="98640-426">ASP.NET Core dostarcza następujących dostawców:</span><span class="sxs-lookup"><span data-stu-id="98640-426">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="98640-427">Console</span><span class="sxs-lookup"><span data-stu-id="98640-427">Console</span></span>](#console-provider)
* [<span data-ttu-id="98640-428">Rozpocząć</span><span class="sxs-lookup"><span data-stu-id="98640-428">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="98640-429">EventSource</span><span class="sxs-lookup"><span data-stu-id="98640-429">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="98640-430">Elemencie</span><span class="sxs-lookup"><span data-stu-id="98640-430">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="98640-431">TraceSource</span><span class="sxs-lookup"><span data-stu-id="98640-431">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="98640-432">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="98640-432">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="98640-433">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="98640-433">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="98640-434">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="98640-434">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="98640-435">Aby uzyskać informacje na temat strumienia stdout i rejestrowania debugowania za pomocą modułu ASP.NET Core, zobacz <xref:test/troubleshoot-azure-iis> i <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="98640-435">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="98640-436">Dostawca konsoli</span><span class="sxs-lookup"><span data-stu-id="98640-436">Console provider</span></span>

<span data-ttu-id="98640-437">Pakiet [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) Provider wysyła dane wyjściowe dziennika do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="98640-437">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="98640-438">Aby wyświetlić dane wyjściowe rejestrowania konsoli, Otwórz wiersz polecenia w folderze projektu i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="98640-438">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="98640-439">Dostawca debugowania</span><span class="sxs-lookup"><span data-stu-id="98640-439">Debug provider</span></span>

<span data-ttu-id="98640-440">Pakiet [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) Provider zapisuje dane wyjściowe dziennika przy użyciu klasy [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug) (wywołania metody `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="98640-440">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="98640-441">W systemie Linux ten dostawca zapisuje dzienniki do */var/log/Message*.</span><span class="sxs-lookup"><span data-stu-id="98640-441">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="eventsource-provider"></a><span data-ttu-id="98640-442">Dostawca EventSource</span><span class="sxs-lookup"><span data-stu-id="98640-442">EventSource provider</span></span>

<span data-ttu-id="98640-443">W przypadku aplikacji przeznaczonych do ASP.NET Core 1.1.0 lub nowszych pakiet dostawcy [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) może zaimplementować śledzenie zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="98640-443">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="98640-444">W systemie Windows używa [funkcji ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="98640-444">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="98640-445">Dostawca jest międzyplatformowy, ale nie ma jeszcze kolekcji zdarzeń i narzędzi do wyświetlania dla systemu Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="98640-445">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="98640-446">Dobrym sposobem na zbieranie i wyświetlanie dzienników jest użycie [Narzędzia Narzędzia PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="98640-446">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="98640-447">Istnieją inne narzędzia do wyświetlania dzienników ETW, ale narzędzia PerfView zapewnia najlepsze środowisko pracy z zdarzeniami ETW emitowanymi przez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98640-447">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="98640-448">Aby skonfigurować narzędzia PerfView do zbierania zdarzeń rejestrowanych przez tego dostawcę, należy dodać ciąg `*Microsoft-Extensions-Logging` do listy **dodatkowych dostawców** .</span><span class="sxs-lookup"><span data-stu-id="98640-448">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="98640-449">(Nie przegap gwiazdki na początku ciągu znaków).</span><span class="sxs-lookup"><span data-stu-id="98640-449">(Don't miss the asterisk at the start of the string.)</span></span>

![Narzędzia PerfView dodatkowych dostawców](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="98640-451">Dostawca dziennika zdarzeń systemu Windows</span><span class="sxs-lookup"><span data-stu-id="98640-451">Windows EventLog provider</span></span>

<span data-ttu-id="98640-452">Pakiet dostawcy [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) wysyła dane wyjściowe dziennika do dziennika zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="98640-452">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="98640-453">[Przeciążenia addeventlog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) umożliwiają przekazywanie <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span><span class="sxs-lookup"><span data-stu-id="98640-453">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span>

### <a name="tracesource-provider"></a><span data-ttu-id="98640-454">Dostawca TraceSource</span><span class="sxs-lookup"><span data-stu-id="98640-454">TraceSource provider</span></span>

<span data-ttu-id="98640-455">Pakiet dostawcy [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) używa bibliotek i dostawców <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="98640-455">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="98640-456">[Przeciążenia AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) umożliwiają przekazywanie danych w przełączniku źródłowym i odbiorniku śledzenia.</span><span class="sxs-lookup"><span data-stu-id="98640-456">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="98640-457">Aby można było korzystać z tego dostawcy, aplikacja musi działać na .NET Framework (a nie na platformie .NET Core).</span><span class="sxs-lookup"><span data-stu-id="98640-457">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="98640-458">Dostawca może kierować komunikaty do różnych [odbiorników](/dotnet/framework/debug-trace-profile/trace-listeners), takich jak <xref:System.Diagnostics.TextWriterTraceListener> używany w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-458">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="98640-459">Dostawca Azure App Service</span><span class="sxs-lookup"><span data-stu-id="98640-459">Azure App Service provider</span></span>

<span data-ttu-id="98640-460">Pakiet [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) Provider zapisuje dzienniki w plikach tekstowych w systemie plików aplikacji Azure App Service i w usłudze [BLOB Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) na koncie usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="98640-460">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="98640-461">Pakiet dostawcy nie jest uwzględniony w udostępnionej infrastrukturze.</span><span class="sxs-lookup"><span data-stu-id="98640-461">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="98640-462">Aby użyć dostawcy, Dodaj pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="98640-462">To use the provider, add the provider package to the project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="98640-463">Pakiet dostawcy nie jest uwzględniony w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="98640-463">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="98640-464">Podczas określania wartości docelowej .NET Framework lub odwoływania się do pakietu `Microsoft.AspNetCore.App` należy dodać pakiet dostawcy do projektu.</span><span class="sxs-lookup"><span data-stu-id="98640-464">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="98640-465">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="98640-465">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="98640-466">Aby skonfigurować ustawienia dostawcy, użyj <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> i <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="98640-466">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="98640-467">Przeciążenie <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> umożliwia przekazanie <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="98640-467">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="98640-468">Obiekt Settings może przesłonić ustawienia domyślne, takie jak szablon danych wyjściowych rejestrowania, nazwa obiektu BLOB i limit rozmiaru pliku.</span><span class="sxs-lookup"><span data-stu-id="98640-468">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="98640-469">(*Szablon danych wyjściowych* to szablon wiadomości, który jest stosowany do wszystkich dzienników oprócz tego, co jest dostępne w wywołaniu metody `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="98640-469">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

<span data-ttu-id="98640-470">Po wdrożeniu w aplikacji App Service, aplikacja będzie przestrzegać ustawień w sekcji [dzienniki App Service](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) na stronie **App Service** Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="98640-470">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="98640-471">Po zaktualizowaniu następujących ustawień zmiany zaczynają obowiązywać natychmiast, bez konieczności ponownego uruchomienia lub ponownej wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-471">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="98640-472">**Rejestrowanie aplikacji (system plików)**</span><span class="sxs-lookup"><span data-stu-id="98640-472">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="98640-473">**Rejestrowanie aplikacji (BLOB)**</span><span class="sxs-lookup"><span data-stu-id="98640-473">**Application Logging (Blob)**</span></span>

<span data-ttu-id="98640-474">Domyślna lokalizacja plików dziennika znajduje się w folderze *D:\\home\\plik_dziennikas\\folder aplikacji* , a domyślna nazwa pliku to *Diagnostics-YYYYMMDD. txt*.</span><span class="sxs-lookup"><span data-stu-id="98640-474">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="98640-475">Domyślny limit rozmiaru pliku wynosi 10 MB, a domyślna maksymalna liczba zachowywanych plików to 2.</span><span class="sxs-lookup"><span data-stu-id="98640-475">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="98640-476">Domyślną nazwą obiektu BLOB jest *{App-Name} {timestamp}/yyyy/mm/dd/hh/{GUID}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="98640-476">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="98640-477">Dostawca działa tylko wtedy, gdy projekt jest uruchomiony w środowisku platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="98640-477">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="98640-478">Nie ma ono wpływu, gdy projekt jest uruchamiany lokalnie&mdash;nie zapisuje w plikach lokalnych ani w lokalnym magazynie programistycznym dla obiektów BLOB.</span><span class="sxs-lookup"><span data-stu-id="98640-478">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="98640-479">Przesyłanie strumieniowe dzienników Azure</span><span class="sxs-lookup"><span data-stu-id="98640-479">Azure log streaming</span></span>

<span data-ttu-id="98640-480">Usługa przesyłania strumieniowego w usłudze Azure log umożliwia wyświetlanie dziennika aktywności w czasie rzeczywistym z:</span><span class="sxs-lookup"><span data-stu-id="98640-480">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="98640-481">Serwer aplikacji</span><span class="sxs-lookup"><span data-stu-id="98640-481">The app server</span></span>
* <span data-ttu-id="98640-482">Serwer sieci Web</span><span class="sxs-lookup"><span data-stu-id="98640-482">The web server</span></span>
* <span data-ttu-id="98640-483">Śledzenie nieudanych żądań</span><span class="sxs-lookup"><span data-stu-id="98640-483">Failed request tracing</span></span>

<span data-ttu-id="98640-484">Aby skonfigurować przesyłanie strumieniowe dzienników Azure:</span><span class="sxs-lookup"><span data-stu-id="98640-484">To configure Azure log streaming:</span></span>

* <span data-ttu-id="98640-485">Przejdź do strony **dzienników App Service** ze strony portalu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-485">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="98640-486">Ustaw **Rejestrowanie aplikacji (system plików)** na **włączone**.</span><span class="sxs-lookup"><span data-stu-id="98640-486">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="98640-487">Wybierz **poziom**dziennika.</span><span class="sxs-lookup"><span data-stu-id="98640-487">Choose the log **Level**.</span></span>

<span data-ttu-id="98640-488">Przejdź do strony **strumień dziennika** , aby wyświetlić komunikaty aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98640-488">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="98640-489">Są one rejestrowane przez aplikację za pomocą interfejsu `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="98640-489">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="98640-490">Rejestrowanie śledzenia Application Insights platformy Azure</span><span class="sxs-lookup"><span data-stu-id="98640-490">Azure Application Insights trace logging</span></span>

<span data-ttu-id="98640-491">Pakiet [Microsoft. Extensions. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) Provider zapisuje dzienniki na platformie Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="98640-491">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="98640-492">Application Insights to usługa, która monitoruje aplikację internetową i udostępnia narzędzia do wykonywania zapytań i analizowania danych telemetrycznych.</span><span class="sxs-lookup"><span data-stu-id="98640-492">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="98640-493">Jeśli używasz tego dostawcy, możesz wysyłać zapytania i analizować dzienniki przy użyciu narzędzi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="98640-493">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="98640-494">Dostawca rejestrowania jest dołączony jako zależność [Microsoft. ApplicationInsights. AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), który jest pakietem, który zapewnia wszystkie dostępne dane telemetryczne dla ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98640-494">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="98640-495">Jeśli używasz tego pakietu, nie musisz instalować pakietu dostawcy.</span><span class="sxs-lookup"><span data-stu-id="98640-495">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="98640-496">Nie używaj pakietu [Microsoft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) Package&mdash;, który jest przeznaczony dla ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="98640-496">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="98640-497">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="98640-497">For more information, see the following resources:</span></span>

* [<span data-ttu-id="98640-498">Przegląd Application Insights</span><span class="sxs-lookup"><span data-stu-id="98640-498">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="98640-499">[Application Insights dla ASP.NET Core aplikacji](/azure/azure-monitor/app/asp-net-core) — Zacznij tutaj, jeśli chcesz zaimplementować cały zakres Application Insights telemetrii wraz z rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="98640-499">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="98640-500">[ApplicationInsightsLoggerProvider for .NET Core ILogger](/azure/azure-monitor/app/ilogger) — Rozpocznij tutaj, jeśli chcesz zaimplementować dostawcę rejestrowania bez pozostałej części telemetrii Application Insights.</span><span class="sxs-lookup"><span data-stu-id="98640-500">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="98640-501">[Karty rejestrowania Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="98640-501">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="98640-502">[Zainstaluj, skonfiguruj i zainicjuj samouczek Application INSIGHTS SDK](/learn/modules/instrument-web-app-code-with-application-insights) — interaktywny w witrynie Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="98640-502">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="98640-503">Dostawcy rejestrowania innych firm</span><span class="sxs-lookup"><span data-stu-id="98640-503">Third-party logging providers</span></span>

<span data-ttu-id="98640-504">Platformy rejestrowania innych firm, które współpracują z ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="98640-504">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="98640-505">[ELMAH.IO](https://elmah.io/) ([repozytorium GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="98640-505">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="98640-506">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([repozytorium GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="98640-506">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="98640-507">[JSNLog](https://jsnlog.com/) ([repozytorium GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="98640-507">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="98640-508">[KissLog.NET](https://kisslog.net/) ([repozytorium GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="98640-508">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="98640-509">[Log4Net](https://logging.apache.org/log4net/) ([repozytorium GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="98640-509">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="98640-510">[Loggr](https://loggr.net/) ([repozytorium GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="98640-510">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="98640-511">[NLOG](https://nlog-project.org/) ([repozytorium GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="98640-511">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="98640-512">[Sentry](https://sentry.io/welcome/) ([repozytorium GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="98640-512">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="98640-513">[Serilog](https://serilog.net/) ([repozytorium GitHub](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="98640-513">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="98640-514">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repozytorium GitHub](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="98640-514">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="98640-515">Niektóre struktury innych firm mogą wykonywać [Rejestrowanie semantyczne, znane także jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="98640-515">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="98640-516">Korzystanie z struktury innej firmy jest podobne do korzystania z jednego z wbudowanych dostawców:</span><span class="sxs-lookup"><span data-stu-id="98640-516">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="98640-517">Dodaj pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="98640-517">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="98640-518">Wywoływanie metody rozszerzenia `ILoggerFactory` dostarczonej przez strukturę rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="98640-518">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="98640-519">Aby uzyskać więcej informacji, zobacz dokumentację każdego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="98640-519">For more information, see each provider's documentation.</span></span> <span data-ttu-id="98640-520">Dostawcy rejestrowania innych firm nie są obsługiwani przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="98640-520">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="98640-521">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="98640-521">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
