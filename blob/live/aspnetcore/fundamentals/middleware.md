---
title: "Oprogramowanie pośredniczące platformy ASP.NET Core"
author: rick-anderson
description: "Więcej informacji na temat oprogramowania pośredniczącego platformy ASP.NET Core i żądania potoku."
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: af16046c97964e8e1c16a4f5989fcfa794741c4d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="b5355-103">Podstawowe informacje na temat platformy ASP.NET Core oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="b5355-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="b5355-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b5355-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b5355-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b5355-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="b5355-106">Co to jest oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="b5355-106">What is middleware</span></span>

<span data-ttu-id="b5355-107">Oprogramowanie pośredniczące to oprogramowanie, które są umieszczone w potoku aplikacji do obsługi żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b5355-107">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="b5355-108">Poszczególne składniki:</span><span class="sxs-lookup"><span data-stu-id="b5355-108">Each component:</span></span>

* <span data-ttu-id="b5355-109">Wybiera opcję Przekaż żądanie do następnego składnika w potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="b5355-110">Może wykonywać pracy przed i po wywołaniu następny składnik w potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="b5355-111">Obiekty delegowane żądania są używane do tworzenia potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="b5355-112">Obiekty delegowane żądania obsługi każdego żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5355-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="b5355-113">Żądanie delegatów są skonfigurowane przy użyciu [Uruchom](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mapy](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), i [użyj](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b5355-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="b5355-114">Obiekt delegowany oddzielne żądanie może być określony w wierszu jako metody anonimowej (nazywane oprogramowanie pośredniczące w wierszu) lub może być zdefiniowana w klasie wielokrotnego użytku.</span><span class="sxs-lookup"><span data-stu-id="b5355-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="b5355-115">Te klasy wielokrotnego użytku i metod anonimowych w wierszu są *oprogramowanie pośredniczące*, lub *składników oprogramowania pośredniczącego*.</span><span class="sxs-lookup"><span data-stu-id="b5355-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="b5355-116">Każdy składnik oprogramowania pośredniczącego w potoku żądania jest odpowiedzialny za wywoływanie następny składnik w potoku lub zwarcie łańcucha, w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="b5355-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="b5355-117">[Migrowanie moduły HTTP z oprogramowaniem pośredniczącym](../migration/http-modules.md) objaśniono różnicę między potoków żądania w ASP.NET Core i poprzednie wersje i zawiera więcej przykładów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="b5355-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="b5355-118">Tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="b5355-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="b5355-119">Potok żądań ASP.NET Core składa się z sekwencji delegatów żądania, jeden po drugim wywołać, ponieważ ten diagram przedstawia (wątek sposób wykonywania strzałki czarny):</span><span class="sxs-lookup"><span data-stu-id="b5355-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Żądanie przetwarzania wzorca przedstawiający żądania przychodzące, przetwarzania za pośrednictwem trzy middlewares i odpowiedzi, pozostawiając aplikacji.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="b5355-123">Każdy delegata mogą wykonywać operacje przed i po następnym delegata.</span><span class="sxs-lookup"><span data-stu-id="b5355-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="b5355-124">Delegat można też przekazuje żądanie do następnego delegata, która jest wywoływana zwarcie żądania potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="b5355-125">Zwarcie często jest pożądane, ponieważ takie rozwiązanie pomaga uniknąć niepotrzebnych pracy.</span><span class="sxs-lookup"><span data-stu-id="b5355-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="b5355-126">Na przykład oprogramowanie pośredniczące plików statycznych wrócić żądania plików statycznych i zwarcia pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="b5355-127">Delegatów obsługi wyjątków muszą można wywołać w potoku, więc ich catch wyjątków, które występują w późniejszym etapie w potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="b5355-128">Najprostsza możliwa aplikacji platformy ASP.NET Core ustawia delegata pojedynczego żądania, który obsługuje wszystkie żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="b5355-129">Ten przypadek nie zawiera rzeczywistego żądania potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="b5355-130">Zamiast tego jednej funkcji anonimowy jest wywoływana w odpowiedzi na każde żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5355-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="b5355-131">Pierwszy [aplikacji. Uruchom](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegata kończy potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="b5355-132">Tworzenia łańcucha wielu delegatów żądania razem z [aplikacji. Użyj](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="b5355-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="b5355-133">`next` Parametr reprezentuje dalej delegata w potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="b5355-134">(Należy pamiętać, że można zwarcia potoku przez *nie* wywoływania *dalej* parametru.) Zazwyczaj można wykonywać akcje przed i po następnej delegata, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="b5355-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="b5355-135">Nie wywołuj `next.Invoke` po wysłaniu odpowiedzi do klienta.</span><span class="sxs-lookup"><span data-stu-id="b5355-135">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="b5355-136">Zmienia się na `HttpResponse` po rozpoczęciu odpowiedzi spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="b5355-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="b5355-137">Na przykład zmiany, takie jak ustawianie nagłówków, kod stanu itd., spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="b5355-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="b5355-138">Zapisywanie w treści odpowiedzi po wywołaniu `next`:</span><span class="sxs-lookup"><span data-stu-id="b5355-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="b5355-139">Może spowodować naruszenie protokołu.</span><span class="sxs-lookup"><span data-stu-id="b5355-139">May cause a protocol violation.</span></span> <span data-ttu-id="b5355-140">Na przykład zapisywania więcej niż podanej `content-length`.</span><span class="sxs-lookup"><span data-stu-id="b5355-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="b5355-141">Może spowodować uszkodzenie formatu treści.</span><span class="sxs-lookup"><span data-stu-id="b5355-141">May corrupt the body format.</span></span> <span data-ttu-id="b5355-142">Na przykład zapisywanie stopkę HTML w pliku CSS.</span><span class="sxs-lookup"><span data-stu-id="b5355-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="b5355-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) przydatne podpowiada, aby wskazać, czy wysłaniu nagłówków i/lub treść została zapisana.</span><span class="sxs-lookup"><span data-stu-id="b5355-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="b5355-144">Szeregowanie</span><span class="sxs-lookup"><span data-stu-id="b5355-144">Ordering</span></span>

<span data-ttu-id="b5355-145">Dodano składników oprogramowania pośredniczącego w kolejności `Configure` metoda definiuje kolejności, w którym są wywoływane w odpowiedzi na żądania i odwrotna kolejność dla odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b5355-145">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="b5355-146">Ta kolejność jest krytyczny dla zabezpieczeń, wydajności i funkcji.</span><span class="sxs-lookup"><span data-stu-id="b5355-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="b5355-147">Konfigurowanie metody (pokazana poniżej) dodaje następujące składniki oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="b5355-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="b5355-148">Obsługa wyjątków/błędów</span><span class="sxs-lookup"><span data-stu-id="b5355-148">Exception/error handling</span></span>
2. <span data-ttu-id="b5355-149">Serwer plików statycznych</span><span class="sxs-lookup"><span data-stu-id="b5355-149">Static file server</span></span>
3. <span data-ttu-id="b5355-150">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="b5355-150">Authentication</span></span>
4. <span data-ttu-id="b5355-151">MVC</span><span class="sxs-lookup"><span data-stu-id="b5355-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5355-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5355-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5355-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5355-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="b5355-154">W powyższym kodzie `UseExceptionHandler` jest pierwszy składnik oprogramowania pośredniczącego dodane do potoku — w związku z tym go przechwytuje żadnych wyjątków, które występują w późniejszym wywołania.</span><span class="sxs-lookup"><span data-stu-id="b5355-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="b5355-155">Oprogramowanie pośredniczące plików statycznych jest wywoływana wcześnie w potoku, więc może obsługiwać żądania ani zwarcia bez pośrednictwa pozostałe składniki.</span><span class="sxs-lookup"><span data-stu-id="b5355-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="b5355-156">Udostępnia oprogramowanie pośredniczące plików statycznych **nie** sprawdzeń autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="b5355-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="b5355-157">Wszystkie pliki obsługiwane przez, włącznie z zawartymi w obszarze *wwwroot*, są dostępne publicznie.</span><span class="sxs-lookup"><span data-stu-id="b5355-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="b5355-158">Zobacz [Praca z pliki statyczne](xref:fundamentals/static-files) podejścia do zabezpieczania plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="b5355-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5355-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5355-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="b5355-160">Jeśli żądanie nie jest obsługiwany przez oprogramowanie pośredniczące plików statycznych, będzie przekazany z oprogramowaniem pośredniczącym tożsamości (`app.UseAuthentication`), który przeprowadza uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="b5355-160">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="b5355-161">Tożsamość nie zwarcia nieuwierzytelnione żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-161">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="b5355-162">Mimo że tożsamości uwierzytelniania żądań autoryzacji (i odrzucenia) występuje tylko po MVC wybierze określonych Razor strony lub kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="b5355-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5355-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5355-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b5355-164">Jeśli żądanie nie jest obsługiwany przez oprogramowanie pośredniczące plików statycznych, będzie przekazany z oprogramowaniem pośredniczącym tożsamości (`app.UseIdentity`), który przeprowadza uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="b5355-164">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="b5355-165">Tożsamość nie zwarcia nieuwierzytelnione żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-165">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="b5355-166">Mimo że tożsamości uwierzytelnia żądania, autoryzacji (i odrzucenia) występuje tylko wtedy, gdy wybiera MVC określonego kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="b5355-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="b5355-167">W poniższym przykładzie pokazano oprogramowanie pośredniczące kolejność, w którym żądań dotyczących plików statycznych są obsługiwane przez oprogramowanie pośredniczące plików statycznych przed oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b5355-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="b5355-168">Pliki statyczne nie są kompresowane z ta kolejność oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="b5355-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="b5355-169">Odpowiedzi MVC z [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) można skompresować.</span><span class="sxs-lookup"><span data-stu-id="b5355-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="b5355-170">Użyj, uruchamiania i mapowanie</span><span class="sxs-lookup"><span data-stu-id="b5355-170">Use, Run, and Map</span></span>

<span data-ttu-id="b5355-171">Można skonfigurować za pomocą potoku HTTP `Use`, `Run`, i `Map`.</span><span class="sxs-lookup"><span data-stu-id="b5355-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="b5355-172">`Use` Metody może zwarcia potoku (to znaczy, jeśli nie wywołuje `next` delegata żądania).</span><span class="sxs-lookup"><span data-stu-id="b5355-172">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="b5355-173">`Run`jest to Konwencja i niektórych składników oprogramowania pośredniczącego może narazić `Run[Middleware]` metod, które są uruchamiane na końcu potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="b5355-174">`Map*`rozszerzenia są używane jako Konwencji Rozgałęzienie potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="b5355-175">[Mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) odgałęzień potoku żądania na podstawie dopasowań z podanej ścieżki żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="b5355-176">Jeśli ścieżka żądania rozpoczyna się od podanej ścieżce, gałęzi jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="b5355-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="b5355-177">W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="b5355-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="b5355-178">Żądanie</span><span class="sxs-lookup"><span data-stu-id="b5355-178">Request</span></span> | <span data-ttu-id="b5355-179">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="b5355-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="b5355-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="b5355-180">localhost:1234</span></span> | <span data-ttu-id="b5355-181">Powitania od elementu delegate-Map.</span><span class="sxs-lookup"><span data-stu-id="b5355-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="b5355-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="b5355-182">localhost:1234/map1</span></span> | <span data-ttu-id="b5355-183">Mapa Test 1</span><span class="sxs-lookup"><span data-stu-id="b5355-183">Map Test 1</span></span> |
| <span data-ttu-id="b5355-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="b5355-184">localhost:1234/map2</span></span> | <span data-ttu-id="b5355-185">Mapa Test 2</span><span class="sxs-lookup"><span data-stu-id="b5355-185">Map Test 2</span></span> |
| <span data-ttu-id="b5355-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="b5355-186">localhost:1234/map3</span></span> | <span data-ttu-id="b5355-187">Powitania od elementu delegate-Map.</span><span class="sxs-lookup"><span data-stu-id="b5355-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="b5355-188">Gdy `Map` jest, następującą liczbę segmentów ścieżki dopasowane są usuwane z `HttpRequest.Path` i jest dołączany do `HttpRequest.PathBase` dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="b5355-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) odgałęzień potoku żądania na podstawie wyniku danego predykatu.</span><span class="sxs-lookup"><span data-stu-id="b5355-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="b5355-190">Predykat dowolnego typu `Func<HttpContext, bool>` służy do mapowania żądania do nowej gałęzi potoku.</span><span class="sxs-lookup"><span data-stu-id="b5355-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="b5355-191">W poniższym przykładzie predykat jest używana do wykrywania obecności zmiennej ciągu zapytania `branch`:</span><span class="sxs-lookup"><span data-stu-id="b5355-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="b5355-192">W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="b5355-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="b5355-193">Żądanie</span><span class="sxs-lookup"><span data-stu-id="b5355-193">Request</span></span> | <span data-ttu-id="b5355-194">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="b5355-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="b5355-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="b5355-195">localhost:1234</span></span> | <span data-ttu-id="b5355-196">Powitania od elementu delegate-Map.</span><span class="sxs-lookup"><span data-stu-id="b5355-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="b5355-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="b5355-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="b5355-198">Gałąź używane = wzorca</span><span class="sxs-lookup"><span data-stu-id="b5355-198">Branch used = master</span></span>|

<span data-ttu-id="b5355-199">`Map`obsługuje zagnieżdżania, na przykład:</span><span class="sxs-lookup"><span data-stu-id="b5355-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="b5355-200">`Map`można również odpowiada wielu segmentach jednocześnie, na przykład:</span><span class="sxs-lookup"><span data-stu-id="b5355-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="b5355-201">Wbudowane oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="b5355-201">Built-in middleware</span></span>

<span data-ttu-id="b5355-202">Platformy ASP.NET Core jest dostarczany z następujących składników oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="b5355-202">ASP.NET Core ships with the following middleware components:</span></span>

| <span data-ttu-id="b5355-203">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="b5355-203">Middleware</span></span> | <span data-ttu-id="b5355-204">Opis</span><span class="sxs-lookup"><span data-stu-id="b5355-204">Description</span></span> |
| ----- | ------- |
| [<span data-ttu-id="b5355-205">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="b5355-205">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="b5355-206">Zapewnia obsługę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b5355-206">Provides authentication support.</span></span> |
| [<span data-ttu-id="b5355-207">CORS</span><span class="sxs-lookup"><span data-stu-id="b5355-207">CORS</span></span>](xref:security/cors) | <span data-ttu-id="b5355-208">Konfiguruje współużytkowanie zasobów między źródłami.</span><span class="sxs-lookup"><span data-stu-id="b5355-208">Configures Cross-Origin Resource Sharing.</span></span> |
| [<span data-ttu-id="b5355-209">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="b5355-209">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="b5355-210">Zapewnia obsługę buforowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b5355-210">Provides support for caching responses.</span></span> |
| [<span data-ttu-id="b5355-211">Kompresja odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="b5355-211">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="b5355-212">Zapewnia obsługę kompresowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b5355-212">Provides support for compressing responses.</span></span> |
| [<span data-ttu-id="b5355-213">Routing</span><span class="sxs-lookup"><span data-stu-id="b5355-213">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="b5355-214">Definiuje i ogranicza trasy żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-214">Defines and constrains request routes.</span></span> |
| [<span data-ttu-id="b5355-215">Sesja</span><span class="sxs-lookup"><span data-stu-id="b5355-215">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="b5355-216">Zapewnia obsługę zarządzania sesjami użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b5355-216">Provides support for managing user sessions.</span></span> |
| [<span data-ttu-id="b5355-217">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="b5355-217">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="b5355-218">Zapewnia obsługę obsługujących pliki statyczne oraz przeglądanie katalogów.</span><span class="sxs-lookup"><span data-stu-id="b5355-218">Provides support for serving static files and directory browsing.</span></span> |
| [<span data-ttu-id="b5355-219">Oprogramowanie pośredniczące ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="b5355-219">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="b5355-220">Umożliwia ponowne zapisywanie adresów URL i przekierowywanie żądań.</span><span class="sxs-lookup"><span data-stu-id="b5355-220">Provides support for rewriting URLs and redirecting requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="b5355-221">Oprogramowanie pośredniczące zapisu</span><span class="sxs-lookup"><span data-stu-id="b5355-221">Writing middleware</span></span>

<span data-ttu-id="b5355-222">Oprogramowanie pośredniczące jest zazwyczaj hermetyzowany w klasie i udostępniany z metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b5355-222">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="b5355-223">Należy wziąć pod uwagę następujące oprogramowanie pośredniczące, które ustawia kulturę bieżącego żądania z ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="b5355-223">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="b5355-224">Uwaga: W powyższym przykładowym kodzie służy do zaprezentowania tworzenia składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="b5355-224">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="b5355-225">Zobacz [ lokalizacja i globalizacja](xref:fundamentals/localization) obsługę wbudowanych lokalizacja platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5355-225">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="b5355-226">Możesz przetestować oprogramowanie pośredniczące, przekazując kultury, na przykład `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="b5355-226">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="b5355-227">Poniższy kod powoduje delegata oprogramowanie pośredniczące do klasy:</span><span class="sxs-lookup"><span data-stu-id="b5355-227">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="b5355-228">Oprogramowanie pośredniczące za pośrednictwem udostępnia następujące metody rozszerzenia [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="b5355-228">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="b5355-229">Poniższy kod wywołuje oprogramowanie pośredniczące z `Configure`:</span><span class="sxs-lookup"><span data-stu-id="b5355-229">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="b5355-230">Oprogramowanie pośredniczące powinno wykonać [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/) przez udostępnianie jego zależności w jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b5355-230">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="b5355-231">Oprogramowanie pośredniczące jest tworzony raz na *istnienia aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="b5355-231">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="b5355-232">Zobacz *zależności żądania* poniżej Jeśli potrzebne do udostępnienia usług z oprogramowania pośredniczącego w ramach żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-232">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="b5355-233">Składniki oprogramowania pośredniczącego można rozwiązać zależności z iniekcji zależności za pomocą parametrów konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b5355-233">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="b5355-234">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)Ponadto może zaakceptować dodatkowe parametry bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="b5355-234">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="b5355-235">Zależności żądania</span><span class="sxs-lookup"><span data-stu-id="b5355-235">Per-request dependencies</span></span>

<span data-ttu-id="b5355-236">Ponieważ oprogramowanie pośredniczące jest tworzony podczas uruchamiania aplikacji, nie dla poszczególnych żądań, *zakres* okres istnienia usługi używane przez oprogramowanie pośredniczące konstruktory nie są współużytkowane z innych typów zależności wprowadzonym podczas każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b5355-236">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="b5355-237">Jeśli trzeba udostępnić *zakres* usługi między oprogramowania pośredniczącego i innych typów, należy dodać tych usług `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="b5355-237">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="b5355-238">`Invoke` Metody może zaakceptować dodatkowe parametry, które są wypełniane przy iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="b5355-238">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="b5355-239">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b5355-239">For example:</span></span>

```c#
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

## <a name="resources"></a><span data-ttu-id="b5355-240">Resources</span><span class="sxs-lookup"><span data-stu-id="b5355-240">Resources</span></span>

* [<span data-ttu-id="b5355-241">Przykładowy kod używany w tym dokumencie</span><span class="sxs-lookup"><span data-stu-id="b5355-241">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="b5355-242">Migrowanie moduły HTTP oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="b5355-242">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="b5355-243">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b5355-243">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="b5355-244">Funkcje na żądanie</span><span class="sxs-lookup"><span data-stu-id="b5355-244">Request Features</span></span>](request-features.md)
