---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: "Śledzenie w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Pokazuje, jak włączyć śledzenie w interfejsie API sieci Web ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7392ae5d9bc4c3aab45a9373099a0ee18e873a4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="33457-103">Śledzenie w składniku ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="33457-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="33457-104">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="33457-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="33457-105">Próbujesz debugowanie aplikacji sieci web, nie ma nie zastąpi dobry zestaw dzienników śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33457-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="33457-106">Ten samouczek pokazuje, jak włączyć śledzenie w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33457-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="33457-107">Ta funkcja służy do śledzenia strukturę interfejsu API sieci Web do czego przed i po wywołuje w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="33457-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="33457-108">Można również użyć do śledzenia swoim własnym kodem.</span><span class="sxs-lookup"><span data-stu-id="33457-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="33457-109">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="33457-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="33457-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (współdziała również z programu Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="33457-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="33457-111">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="33457-111">Web API 2</span></span>
> - [<span data-ttu-id="33457-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="33457-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="33457-113">Włącz śledzenie w składniku Web API System.Diagnostics</span><span class="sxs-lookup"><span data-stu-id="33457-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="33457-114">Najpierw utworzymy nowy projekt aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33457-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="33457-115">W programie Visual Studio z **pliku** menu, wybierz opcję **nowy**, następnie **projektu**.</span><span class="sxs-lookup"><span data-stu-id="33457-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="33457-116">W obszarze **szablony**, **Web**, wybierz pozycję **aplikacji sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="33457-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="33457-117">Wybierz szablon projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="33457-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="33457-118">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, następnie **konsoli Zarządzanie pakietów**.</span><span class="sxs-lookup"><span data-stu-id="33457-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="33457-119">W oknie Konsola Menedżera pakietów wpisz następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="33457-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="33457-120">Pierwsze polecenie instaluje najnowszy pakiet śledzenia interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="33457-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="33457-121">Aktualizuje również pakietami podstawowymi interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="33457-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="33457-122">Drugie polecenie aktualizuje pakiet WebApi.WebHost do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="33457-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="33457-123">Jeśli chcesz przeanalizować określonej wersji interfejsu API sieci Web, Użyj flagi wersji, podczas instalowania pakietu śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33457-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="33457-124">Otwórz plik WebApiConfig.cs w aplikacji\_folder początkowy.</span><span class="sxs-lookup"><span data-stu-id="33457-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="33457-125">Dodaj następujący kod do **zarejestrować** metody.</span><span class="sxs-lookup"><span data-stu-id="33457-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="33457-126">Ten kod dodaje [klasę SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) klasy potok składnika Web API.</span><span class="sxs-lookup"><span data-stu-id="33457-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="33457-127">**Klasę SystemDiagnosticsTraceWriter** klasy zapisuje śladów do [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="33457-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="33457-128">Aby wyświetlić dane śledzenia, należy uruchomić aplikację w debugerze.</span><span class="sxs-lookup"><span data-stu-id="33457-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="33457-129">W przeglądarce przejdź do `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="33457-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="33457-130">Instrukcje śledzenia są zapisywane w oknie danych wyjściowych w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33457-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="33457-131">(Z **widoku** menu, wybierz opcję **dane wyjściowe**).</span><span class="sxs-lookup"><span data-stu-id="33457-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="33457-132">Ponieważ **klasę SystemDiagnosticsTraceWriter** zapisuje dane śledzenia do **System.Diagnostics.Trace**, możesz zarejestrować obiekty nasłuchujące śledzenia dodatkowe; na przykład, aby zapisać śladów do pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="33457-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="33457-133">Aby uzyskać więcej informacji na temat zapisywania śledzenia, zobacz [obiektów nasłuchujących śledzenia](https://msdn.microsoft.com/library/4y5y10s7.aspx) temacie w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="33457-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="33457-134">Konfigurowanie klasę SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="33457-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="33457-135">Poniższy kod przedstawia sposób konfigurowania moduł zapisujący śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33457-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="33457-136">Istnieją dwa ustawienia, które mogą kontrolować:</span><span class="sxs-lookup"><span data-stu-id="33457-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="33457-137">IsVerbose: W przypadku wartości FAŁSZ każdego śledzenia zawiera minimalne informacje.</span><span class="sxs-lookup"><span data-stu-id="33457-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="33457-138">Jeśli PRAWDA, ślady zawierają więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="33457-138">If true, traces include more information.</span></span>
- <span data-ttu-id="33457-139">MinimumLevel: Ustawia minimalny poziom śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33457-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="33457-140">Poziomy śledzenia, w kolejności, to debugowania, informacje o Ostrzegaj, błąd i błąd krytyczny.</span><span class="sxs-lookup"><span data-stu-id="33457-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="33457-141">Dodawanie śladów do swojej aplikacji interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="33457-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="33457-142">Dodawanie moduł zapisujący śledzenia umożliwia natychmiastowy dostęp do śledzenia utworzone przez potok składnika Web API.</span><span class="sxs-lookup"><span data-stu-id="33457-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="33457-143">Moduł zapisujący śledzenia można także użyć do śledzenia swoim własnym kodem:</span><span class="sxs-lookup"><span data-stu-id="33457-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="33457-144">Aby uzyskać moduł zapisujący śledzenia, należy wywołać **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="33457-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="33457-145">Z kontrolera, ta metoda jest dostępna za pośrednictwem **ApiController.Configuration** właściwości.</span><span class="sxs-lookup"><span data-stu-id="33457-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="33457-146">Aby napisać śledzenia, należy wywołać **ITraceWriter.Trace** metody bezpośrednio, ale [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) klasa definiuje niektóre metody rozszerzenia, które są bardziej przyjazna dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="33457-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="33457-147">Na przykład **informacji** metod przedstawionych powyżej tworzy śledzenia z poziomu śledzenia **informacji**.</span><span class="sxs-lookup"><span data-stu-id="33457-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="33457-148">Infrastruktura śledzenia interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="33457-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="33457-149">W tej sekcji opisano, jak zapisać moduł zapisujący śledzenia niestandardowego interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="33457-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="33457-150">Pakietu Microsoft.AspNet.WebApi.Tracing jest oparty na bardziej ogólne Infrastruktura śledzenia w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="33457-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="33457-151">Zamiast Microsoft.AspNet.WebApi.Tracing, można także podłączyć niektórych innych biblioteki śledzenie/logowania, takie jak [NLog](http://nlog-project.org/) lub [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="33457-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="33457-152">Aby zbierać dane śledzenia, należy zaimplementować **ITraceWriter** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="33457-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="33457-153">Poniżej przedstawiono prosty przykład:</span><span class="sxs-lookup"><span data-stu-id="33457-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="33457-154">**ITraceWriter.Trace** metoda tworzy śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33457-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="33457-155">Obiekt wywołujący określa poziom kategorii i śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33457-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="33457-156">Kategoria może być dowolnym ciągiem zdefiniowane przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="33457-156">The category can be any user-defined string.</span></span> <span data-ttu-id="33457-157">Implementacji **śledzenia** należy wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="33457-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="33457-158">Utwórz nową **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="33457-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="33457-159">Go zainicjować z żądania, kategorii i poziomie śledzenia, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="33457-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="33457-160">Te wartości są dostarczane przez obiekt wywołujący.</span><span class="sxs-lookup"><span data-stu-id="33457-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="33457-161">Wywołanie *traceAction* delegowanie.</span><span class="sxs-lookup"><span data-stu-id="33457-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="33457-162">Wewnątrz tego delegata wywołujący powinien wypełnić w pozostałej części **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="33457-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="33457-163">Zapis **TraceRecord**, za pomocą żadnych technika rejestrowania, który chcesz.</span><span class="sxs-lookup"><span data-stu-id="33457-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="33457-164">Tu przykładzie wywołuje po prostu **System.Diagnostics.Trace**.</span><span class="sxs-lookup"><span data-stu-id="33457-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="33457-165">Ustawienie moduł zapisujący śledzenia</span><span class="sxs-lookup"><span data-stu-id="33457-165">Setting the Trace Writer</span></span>

<span data-ttu-id="33457-166">Aby włączyć śledzenie, należy skonfigurować interfejs API sieci Web do użycia z **ITraceWriter** implementacji.</span><span class="sxs-lookup"><span data-stu-id="33457-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="33457-167">W tym za pomocą **HttpConfiguration** obiektów, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="33457-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="33457-168">Moduł zapisujący śledzenia tylko jedna może być aktywny.</span><span class="sxs-lookup"><span data-stu-id="33457-168">Only one trace writer can be active.</span></span> <span data-ttu-id="33457-169">Domyślnie ustawia interfejsu API sieci Web &quot;pusta&quot; śledzenia, które nie działają.</span><span class="sxs-lookup"><span data-stu-id="33457-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="33457-170">( &quot;Pusta&quot; śledzenia istnieje, dzięki czemu kod śledzenia nie trzeba sprawdzić, czy moduł zapisujący śledzenia jest **null** przed zapisaniem śladu.)</span><span class="sxs-lookup"><span data-stu-id="33457-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="33457-171">Interfejs API, śledzenie działa jak sieci Web</span><span class="sxs-lookup"><span data-stu-id="33457-171">How Web API Tracing Works</span></span>

<span data-ttu-id="33457-172">Śledzenie wykorzystania interfejsu API sieci Web w interfejsu API sieci Web używa *fasad* wzorzec: gdy śledzenie jest włączone, interfejsu API sieci Web opakowuje różnych części Potok żądań z klasami, które wykonywać wywołania śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33457-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="33457-173">Na przykład podczas wybierania kontrolera, używa potoku **IHttpControllerSelector** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="33457-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="33457-174">Z włączonym śledzeniem pipleline wstawia klasy, która implementuje **IHttpControllerSelector** , ale wywołania do rzeczywistego wykonania:</span><span class="sxs-lookup"><span data-stu-id="33457-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Śledzenie sieci Web interfejsu API korzysta ze wzorca fasad.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="33457-176">Zalety tego projektu:</span><span class="sxs-lookup"><span data-stu-id="33457-176">The benefits of this design include:</span></span>

- <span data-ttu-id="33457-177">Jeśli moduł zapisujący śledzenia nie należy dodawać, składniki śledzenia nie jest utworzona i nie wpływają wydajności.</span><span class="sxs-lookup"><span data-stu-id="33457-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="33457-178">Jeśli takie jak zastąpić domyślne usługi **IHttpControllerSelector** z własnych niestandardowych implementacji śledzenia nie występuje, ponieważ śledzenie jest wykonywana przez obiekt otoki.</span><span class="sxs-lookup"><span data-stu-id="33457-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="33457-179">Można również zastąpić całej struktury śledzenia interfejsu API sieci Web własne niestandardowe framework, zastępując wartość domyślna **ITraceManager** usługi:</span><span class="sxs-lookup"><span data-stu-id="33457-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="33457-180">Implementowanie **ITraceManager.Initialize** zainicjować systemu śledzenia.</span><span class="sxs-lookup"><span data-stu-id="33457-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="33457-181">Należy pamiętać, że spowoduje to zastąpienie *cały* framework śledzenia, łącznie ze wszystkimi kod śledzenia, który jest wbudowany w interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="33457-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
