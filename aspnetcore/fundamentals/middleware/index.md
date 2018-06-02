---
title: Oprogramowanie pośredniczące platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat oprogramowania pośredniczącego platformy ASP.NET Core i żądania potoku.
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 381c1a5cecee945559ea0dabd0aa086c8d52b43a
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729171"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="4deeb-103">Oprogramowanie pośredniczące platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4deeb-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="4deeb-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4deeb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4deeb-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4deeb-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="4deeb-106">Co to jest oprogramowanie pośredniczące?</span><span class="sxs-lookup"><span data-stu-id="4deeb-106">What is middleware?</span></span>

<span data-ttu-id="4deeb-107">Oprogramowanie pośredniczące to oprogramowanie, które są umieszczone w potoku aplikacji do obsługi żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4deeb-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="4deeb-108">Poszczególne składniki:</span><span class="sxs-lookup"><span data-stu-id="4deeb-108">Each component:</span></span>

* <span data-ttu-id="4deeb-109">Wybiera opcję Przekaż żądanie do następnego składnika w potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="4deeb-110">Może wykonywać pracy przed i po wywołaniu następny składnik w potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="4deeb-111">Obiekty delegowane żądania są używane do tworzenia potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="4deeb-112">Obiekty delegowane żądania obsługi każdego żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="4deeb-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="4deeb-113">Żądanie delegatów są skonfigurowane przy użyciu [Uruchom](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [mapy](/dotnet/api/microsoft.aspnetcore.builder.mapextensions), i [użyj](/dotnet/api/microsoft.aspnetcore.builder.useextensions) metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="4deeb-113">Request delegates are configured using [Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions), and [Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="4deeb-114">Obiekt delegowany oddzielne żądanie może być określony w wierszu jako metody anonimowej (nazywane oprogramowanie pośredniczące w wierszu) lub może być zdefiniowana w klasie wielokrotnego użytku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="4deeb-115">Te klasy wielokrotnego użytku i metod anonimowych w wierszu są *oprogramowanie pośredniczące*, lub *składników oprogramowania pośredniczącego*.</span><span class="sxs-lookup"><span data-stu-id="4deeb-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="4deeb-116">Każdy składnik oprogramowania pośredniczącego w potoku żądania jest odpowiedzialny za wywoływanie następny składnik w potoku lub zwarcie łańcucha, w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="4deeb-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="4deeb-117">[Migrowanie moduły HTTP z oprogramowaniem pośredniczącym](xref:migration/http-modules) objaśniono różnicę między potoków żądania w ASP.NET Core i ASP.NET 4.x i zawiera więcej przykładów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="4deeb-117">[Migrate HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="4deeb-118">Tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="4deeb-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="4deeb-119">Potok żądań ASP.NET Core składa się z sekwencji delegatów żądania, jeden po drugim wywołać, ponieważ ten diagram przedstawia (wątek sposób wykonywania strzałki czarny):</span><span class="sxs-lookup"><span data-stu-id="4deeb-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Żądanie przetwarzania wzorca przedstawiający żądania przychodzące, przetwarzania za pośrednictwem trzy middlewares i odpowiedzi, pozostawiając aplikacji.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="4deeb-123">Każdy delegata mogą wykonywać operacje przed i po następnym delegata.</span><span class="sxs-lookup"><span data-stu-id="4deeb-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="4deeb-124">Delegat można też przekazuje żądanie do następnego delegata, która jest wywoływana zwarcie żądania potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="4deeb-125">Zwarcie często jest pożądane, ponieważ takie rozwiązanie pomaga uniknąć niepotrzebnych pracy.</span><span class="sxs-lookup"><span data-stu-id="4deeb-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="4deeb-126">Na przykład oprogramowanie pośredniczące plików statycznych wrócić żądania plików statycznych i zwarcia pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="4deeb-127">Delegatów obsługi wyjątków muszą można wywołać w potoku, więc ich catch wyjątków, które występują w późniejszym etapie w potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="4deeb-128">Najprostsza możliwa aplikacji platformy ASP.NET Core ustawia delegata pojedynczego żądania, który obsługuje wszystkie żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="4deeb-129">Ten przypadek nie zawiera rzeczywistego żądania potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="4deeb-130">Zamiast tego jednej funkcji anonimowy jest wywoływana w odpowiedzi na każde żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="4deeb-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="4deeb-131">Pierwszy [aplikacji. Uruchom](/dotnet/api/microsoft.aspnetcore.builder.runextensions) delegata kończy potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-131">The first [app.Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="4deeb-132">Tworzenia łańcucha wielu delegatów żądania razem z [aplikacji. Użyj](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="4deeb-132">You can chain multiple request delegates together with [app.Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="4deeb-133">`next` Parametr reprezentuje dalej delegata w potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="4deeb-134">(Należy pamiętać, że można zwarcia potoku przez *nie* wywoływania *dalej* parametru.) Zazwyczaj można wykonywać akcje przed i po następnej delegata, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4deeb-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="4deeb-135">Nie wywołuj `next.Invoke` po wysłaniu odpowiedzi do klienta.</span><span class="sxs-lookup"><span data-stu-id="4deeb-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="4deeb-136">Zmienia się na `HttpResponse` po rozpoczęciu odpowiedzi spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="4deeb-137">Na przykład zmiany, takie jak ustawianie nagłówków, kod stanu itd., spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="4deeb-138">Zapisywanie w treści odpowiedzi po wywołaniu `next`:</span><span class="sxs-lookup"><span data-stu-id="4deeb-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="4deeb-139">Może spowodować naruszenie protokołu.</span><span class="sxs-lookup"><span data-stu-id="4deeb-139">May cause a protocol violation.</span></span> <span data-ttu-id="4deeb-140">Na przykład zapisywania więcej niż podanej `content-length`.</span><span class="sxs-lookup"><span data-stu-id="4deeb-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="4deeb-141">Może spowodować uszkodzenie formatu treści.</span><span class="sxs-lookup"><span data-stu-id="4deeb-141">May corrupt the body format.</span></span> <span data-ttu-id="4deeb-142">Na przykład zapisywanie stopkę HTML w pliku CSS.</span><span class="sxs-lookup"><span data-stu-id="4deeb-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="4deeb-143">[HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) przydatne podpowiada, aby wskazać, czy wysłaniu nagłówków i/lub treść została zapisana.</span><span class="sxs-lookup"><span data-stu-id="4deeb-143">[HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="4deeb-144">Szeregowanie</span><span class="sxs-lookup"><span data-stu-id="4deeb-144">Ordering</span></span>

<span data-ttu-id="4deeb-145">Dodano składników oprogramowania pośredniczącego w kolejności `Configure` metoda definiuje kolejność, w którym jest wywoływana w odpowiedzi na żądania i odwrotna kolejność dla odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4deeb-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="4deeb-146">Ta kolejność jest krytyczny dla zabezpieczeń, wydajności i funkcji.</span><span class="sxs-lookup"><span data-stu-id="4deeb-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="4deeb-147">Konfigurowanie metody (pokazana poniżej) dodaje następujące składniki oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="4deeb-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="4deeb-148">Obsługa wyjątków/błędów</span><span class="sxs-lookup"><span data-stu-id="4deeb-148">Exception/error handling</span></span>
2. <span data-ttu-id="4deeb-149">Serwer plików statycznych</span><span class="sxs-lookup"><span data-stu-id="4deeb-149">Static file server</span></span>
3. <span data-ttu-id="4deeb-150">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="4deeb-150">Authentication</span></span>
4. <span data-ttu-id="4deeb-151">MVC</span><span class="sxs-lookup"><span data-stu-id="4deeb-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4deeb-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4deeb-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4deeb-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4deeb-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="4deeb-154">W powyższym kodzie `UseExceptionHandler` jest pierwszy składnik oprogramowania pośredniczącego dodane do potoku — w związku z tym go przechwytuje żadnych wyjątków, które występują w późniejszym wywołania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="4deeb-155">Oprogramowanie pośredniczące plików statycznych jest wywoływana wcześnie w potoku, więc może obsługiwać żądania ani zwarcia bez pośrednictwa pozostałe składniki.</span><span class="sxs-lookup"><span data-stu-id="4deeb-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="4deeb-156">Udostępnia oprogramowanie pośredniczące plików statycznych **nie** sprawdzeń autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4deeb-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="4deeb-157">Wszystkie pliki obsługiwane przez, włącznie z zawartymi w obszarze *wwwroot*, są dostępne publicznie.</span><span class="sxs-lookup"><span data-stu-id="4deeb-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="4deeb-158">Zobacz [pliki statyczne](xref:fundamentals/static-files) podejścia do zabezpieczania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="4deeb-158">See [Static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4deeb-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4deeb-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="4deeb-160">Jeśli żądanie nie jest obsługiwane przez oprogramowanie pośredniczące plików statycznych, będzie przekazany z oprogramowaniem pośredniczącym tożsamości (`app.UseAuthentication`), który przeprowadza uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="4deeb-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="4deeb-161">Tożsamość nie zwarcia nieuwierzytelnione żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="4deeb-162">Mimo że tożsamości uwierzytelniania żądań autoryzacji (i odrzucenia) występuje tylko po MVC wybierze określonych Razor strony lub kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="4deeb-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4deeb-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4deeb-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4deeb-164">Jeśli żądanie nie jest obsługiwane przez oprogramowanie pośredniczące plików statycznych, będzie przekazany z oprogramowaniem pośredniczącym tożsamości (`app.UseIdentity`), który przeprowadza uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="4deeb-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="4deeb-165">Tożsamość nie zwarcia nieuwierzytelnione żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="4deeb-166">Mimo że tożsamości uwierzytelnia żądania, autoryzacji (i odrzucenia) występuje tylko wtedy, gdy wybiera MVC określonego kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="4deeb-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="4deeb-167">W poniższym przykładzie pokazano oprogramowanie pośredniczące kolejność, w którym żądań dotyczących plików statycznych są obsługiwane przez oprogramowanie pośredniczące plików statycznych przed oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4deeb-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="4deeb-168">Pliki statyczne nie są kompresowane z ta kolejność oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="4deeb-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="4deeb-169">Odpowiedzi MVC z [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) można skompresować.</span><span class="sxs-lookup"><span data-stu-id="4deeb-169">The MVC responses from [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="4deeb-170">Użyj, uruchamiania i mapowanie</span><span class="sxs-lookup"><span data-stu-id="4deeb-170">Use, Run, and Map</span></span>

<span data-ttu-id="4deeb-171">Można skonfigurować za pomocą potoku HTTP `Use`, `Run`, i `Map`.</span><span class="sxs-lookup"><span data-stu-id="4deeb-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="4deeb-172">`Use` Metody może zwarcia potoku (to znaczy, jeśli go nie wywołać `next` delegata żądania).</span><span class="sxs-lookup"><span data-stu-id="4deeb-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="4deeb-173">`Run` jest to Konwencja i niektórych składników oprogramowania pośredniczącego może narazić `Run[Middleware]` metod, które są uruchamiane na końcu potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="4deeb-174">`Map*` rozszerzenia są używane jako Konwencji Rozgałęzienie potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="4deeb-175">[Mapa](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) odgałęzień potoku żądania na podstawie dopasowań z podanej ścieżki żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-175">[Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="4deeb-176">Jeśli ścieżka żądania rozpoczyna się od podanej ścieżce, gałęzi jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="4deeb-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="4deeb-177">W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="4deeb-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="4deeb-178">Żądanie</span><span class="sxs-lookup"><span data-stu-id="4deeb-178">Request</span></span> | <span data-ttu-id="4deeb-179">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="4deeb-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="4deeb-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="4deeb-180">localhost:1234</span></span> | <span data-ttu-id="4deeb-181">Powitania od elementu delegate-Map.</span><span class="sxs-lookup"><span data-stu-id="4deeb-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="4deeb-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="4deeb-182">localhost:1234/map1</span></span> | <span data-ttu-id="4deeb-183">Mapa Test 1</span><span class="sxs-lookup"><span data-stu-id="4deeb-183">Map Test 1</span></span> |
| <span data-ttu-id="4deeb-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="4deeb-184">localhost:1234/map2</span></span> | <span data-ttu-id="4deeb-185">Mapa Test 2</span><span class="sxs-lookup"><span data-stu-id="4deeb-185">Map Test 2</span></span> |
| <span data-ttu-id="4deeb-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="4deeb-186">localhost:1234/map3</span></span> | <span data-ttu-id="4deeb-187">Powitania od elementu delegate-Map.</span><span class="sxs-lookup"><span data-stu-id="4deeb-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="4deeb-188">Gdy `Map` jest, następującą liczbę segmentów ścieżki dopasowane są usuwane z `HttpRequest.Path` i jest dołączany do `HttpRequest.PathBase` dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="4deeb-189">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) odgałęzień potoku żądania na podstawie wyniku danego predykatu.</span><span class="sxs-lookup"><span data-stu-id="4deeb-189">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="4deeb-190">Predykat dowolnego typu `Func<HttpContext, bool>` służy do mapowania żądania do nowej gałęzi potoku.</span><span class="sxs-lookup"><span data-stu-id="4deeb-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="4deeb-191">W poniższym przykładzie predykat jest używana do wykrywania obecności zmiennej ciągu zapytania `branch`:</span><span class="sxs-lookup"><span data-stu-id="4deeb-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="4deeb-192">W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="4deeb-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="4deeb-193">Żądanie</span><span class="sxs-lookup"><span data-stu-id="4deeb-193">Request</span></span> | <span data-ttu-id="4deeb-194">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="4deeb-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="4deeb-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="4deeb-195">localhost:1234</span></span> | <span data-ttu-id="4deeb-196">Powitania od elementu delegate-Map.</span><span class="sxs-lookup"><span data-stu-id="4deeb-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="4deeb-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="4deeb-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="4deeb-198">Gałąź używane = wzorca</span><span class="sxs-lookup"><span data-stu-id="4deeb-198">Branch used = master</span></span>|

<span data-ttu-id="4deeb-199">`Map` obsługuje zagnieżdżania, na przykład:</span><span class="sxs-lookup"><span data-stu-id="4deeb-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="4deeb-200">`Map` można również odpowiada wielu segmentach jednocześnie, na przykład:</span><span class="sxs-lookup"><span data-stu-id="4deeb-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="4deeb-201">Wbudowane oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="4deeb-201">Built-in middleware</span></span>

<span data-ttu-id="4deeb-202">Platformy ASP.NET Core jest dostarczany z następujących składników oprogramowania pośredniczącego, a także opis kolejności, w którym powinny zostać dodane:</span><span class="sxs-lookup"><span data-stu-id="4deeb-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="4deeb-203">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="4deeb-203">Middleware</span></span> | <span data-ttu-id="4deeb-204">Opis</span><span class="sxs-lookup"><span data-stu-id="4deeb-204">Description</span></span> | <span data-ttu-id="4deeb-205">Kolejność</span><span class="sxs-lookup"><span data-stu-id="4deeb-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="4deeb-206">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="4deeb-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="4deeb-207">Zapewnia obsługę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-207">Provides authentication support.</span></span> | <span data-ttu-id="4deeb-208">Przed `HttpContext.User` jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="4deeb-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="4deeb-209">Terminalu wywołania zwrotne OAuth.</span><span class="sxs-lookup"><span data-stu-id="4deeb-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="4deeb-210">CORS</span><span class="sxs-lookup"><span data-stu-id="4deeb-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="4deeb-211">Konfiguruje współużytkowanie zasobów między źródłami.</span><span class="sxs-lookup"><span data-stu-id="4deeb-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="4deeb-212">Przed składników, które korzystają z CORS.</span><span class="sxs-lookup"><span data-stu-id="4deeb-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="4deeb-213">Diagnostyka</span><span class="sxs-lookup"><span data-stu-id="4deeb-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="4deeb-214">Konfiguruje diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="4deeb-214">Configures diagnostics.</span></span> | <span data-ttu-id="4deeb-215">Przed składniki, które generują błędy.</span><span class="sxs-lookup"><span data-stu-id="4deeb-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="4deeb-216">Zastąpienia przekazywane nagłówki/HTTP</span><span class="sxs-lookup"><span data-stu-id="4deeb-216">Forwarded Headers/HTTP Overrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="4deeb-217">Przekazuje proxy nagłówki na bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="4deeb-218">Przed składniki używające zaktualizowanych pól (przykłady: schemat, hosta, ClientIP, metoda).</span><span class="sxs-lookup"><span data-stu-id="4deeb-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="4deeb-219">Przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="4deeb-219">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="4deeb-220">Przekieruj żądania HTTP, HTTPS (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="4deeb-220">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="4deeb-221">Przed składniki używające adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4deeb-221">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="4deeb-222">Zabezpieczenia transportu Strict HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="4deeb-222">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="4deeb-223">Pośredniczącym ulepszenie zabezpieczeń dodaje nagłówek odpowiedzi specjalne (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="4deeb-223">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="4deeb-224">Przed wysłaniem odpowiedzi oraz po wykonaniu składników, które modyfikują żądań (na przykład przekazywane nagłówków, ponowne zapisywanie adresów URL).</span><span class="sxs-lookup"><span data-stu-id="4deeb-224">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="4deeb-225">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="4deeb-225">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="4deeb-226">Zapewnia obsługę buforowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4deeb-226">Provides support for caching responses.</span></span> | <span data-ttu-id="4deeb-227">Przed składniki, które wymagają buforowania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-227">Before components that require caching.</span></span> |
| [<span data-ttu-id="4deeb-228">Kompresja odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="4deeb-228">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="4deeb-229">Zapewnia obsługę kompresowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4deeb-229">Provides support for compressing responses.</span></span> | <span data-ttu-id="4deeb-230">Przed składniki, które wymagają kompresji.</span><span class="sxs-lookup"><span data-stu-id="4deeb-230">Before components that require compression.</span></span> |
| [<span data-ttu-id="4deeb-231">Lokalizacja żądania</span><span class="sxs-lookup"><span data-stu-id="4deeb-231">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="4deeb-232">Zapewnia obsługę lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4deeb-232">Provides localization support.</span></span> | <span data-ttu-id="4deeb-233">Przed składniki poufnych lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4deeb-233">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="4deeb-234">Routing</span><span class="sxs-lookup"><span data-stu-id="4deeb-234">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="4deeb-235">Definiuje i ogranicza trasy żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-235">Defines and constrains request routes.</span></span> | <span data-ttu-id="4deeb-236">Terminalu zgodnych tras.</span><span class="sxs-lookup"><span data-stu-id="4deeb-236">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="4deeb-237">Sesja</span><span class="sxs-lookup"><span data-stu-id="4deeb-237">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="4deeb-238">Zapewnia obsługę zarządzania sesjami użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4deeb-238">Provides support for managing user sessions.</span></span> | <span data-ttu-id="4deeb-239">Przed składniki, które wymagają sesji.</span><span class="sxs-lookup"><span data-stu-id="4deeb-239">Before components that require Session.</span></span> |
| [<span data-ttu-id="4deeb-240">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="4deeb-240">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="4deeb-241">Zapewnia obsługę obsługujących pliki statyczne oraz przeglądanie katalogów.</span><span class="sxs-lookup"><span data-stu-id="4deeb-241">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="4deeb-242">Terminal, jeśli żądanie pasuje do plików.</span><span class="sxs-lookup"><span data-stu-id="4deeb-242">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="4deeb-243">Ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="4deeb-243">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="4deeb-244">Umożliwia ponowne zapisywanie adresów URL i przekierowywanie żądań.</span><span class="sxs-lookup"><span data-stu-id="4deeb-244">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="4deeb-245">Przed składniki używające adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4deeb-245">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="4deeb-246">Obiekty WebSocket</span><span class="sxs-lookup"><span data-stu-id="4deeb-246">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="4deeb-247">Włącza protokołu WebSockets.</span><span class="sxs-lookup"><span data-stu-id="4deeb-247">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="4deeb-248">Przed składników, które są wymagane, aby akceptował żądania protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4deeb-248">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="4deeb-249">Oprogramowanie pośredniczące zapisu</span><span class="sxs-lookup"><span data-stu-id="4deeb-249">Writing middleware</span></span>

<span data-ttu-id="4deeb-250">Oprogramowanie pośredniczące jest zazwyczaj hermetyzowany w klasie i udostępniany z metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="4deeb-250">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="4deeb-251">Należy wziąć pod uwagę następujące oprogramowanie pośredniczące, które ustawia kulturę bieżącego żądania z ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="4deeb-251">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="4deeb-252">Uwaga: W powyższym przykładowym kodzie służy do zaprezentowania tworzenia składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="4deeb-252">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="4deeb-253">Zobacz [ lokalizacja i globalizacja](xref:fundamentals/localization) obsługę wbudowanych lokalizacja platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4deeb-253">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="4deeb-254">Możesz przetestować oprogramowanie pośredniczące, przekazując kultury, na przykład `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="4deeb-254">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="4deeb-255">Poniższy kod powoduje delegata oprogramowanie pośredniczące do klasy:</span><span class="sxs-lookup"><span data-stu-id="4deeb-255">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> <span data-ttu-id="4deeb-256">W przypadku platformy ASP.NET Core 1.x, oprogramowanie pośredniczące `Task` Nazwa metody musi być `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="4deeb-256">In ASP.NET Core 1.x, the middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="4deeb-257">W programie ASP.NET Core 2.0 lub nowszej, może to być albo `Invoke` lub `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="4deeb-257">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

<span data-ttu-id="4deeb-258">Oprogramowanie pośredniczące za pośrednictwem udostępnia następujące metody rozszerzenia [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="4deeb-258">The following extension method exposes the middleware through [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="4deeb-259">Poniższy kod wywołuje oprogramowanie pośredniczące z `Configure`:</span><span class="sxs-lookup"><span data-stu-id="4deeb-259">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="4deeb-260">Oprogramowanie pośredniczące powinno wykonać [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/) przez udostępnianie jego zależności w jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="4deeb-260">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="4deeb-261">Oprogramowanie pośredniczące jest tworzony raz na *istnienia aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="4deeb-261">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="4deeb-262">Zobacz *zależności żądania* poniżej Jeśli potrzebne do udostępnienia usług z oprogramowania pośredniczącego w ramach żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-262">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="4deeb-263">Składniki oprogramowania pośredniczącego można rozwiązać zależności z iniekcji zależności za pomocą parametrów konstruktora.</span><span class="sxs-lookup"><span data-stu-id="4deeb-263">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="4deeb-264">[`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) Ponadto może zaakceptować dodatkowe parametry bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="4deeb-264">[`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="4deeb-265">Zależności żądania</span><span class="sxs-lookup"><span data-stu-id="4deeb-265">Per-request dependencies</span></span>

<span data-ttu-id="4deeb-266">Ponieważ oprogramowanie pośredniczące jest tworzony podczas uruchamiania aplikacji, nie dla poszczególnych żądań, *zakres* okres istnienia usługi używane przez oprogramowanie pośredniczące konstruktory nie są współużytkowane z innych typów zależności wprowadzonym podczas każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="4deeb-266">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="4deeb-267">Jeśli trzeba udostępnić *zakres* usługi między oprogramowania pośredniczącego i innych typów, należy dodać tych usług `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="4deeb-267">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="4deeb-268">`Invoke` Metody może zaakceptować dodatkowe parametry, które są wypełniane przy iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="4deeb-268">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="4deeb-269">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4deeb-269">For example:</span></span>

```csharp
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="4deeb-270">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4deeb-270">Additional resources</span></span>

* [<span data-ttu-id="4deeb-271">Migrowanie moduły HTTP do oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="4deeb-271">Migrate HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="4deeb-272">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4deeb-272">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="4deeb-273">Funkcje na żądanie</span><span class="sxs-lookup"><span data-stu-id="4deeb-273">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="4deeb-274">Aktywacji opartej na fabryki oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="4deeb-274">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="4deeb-275">Oprogramowanie pośredniczące aktywacji z kontenerem innych firm</span><span class="sxs-lookup"><span data-stu-id="4deeb-275">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
