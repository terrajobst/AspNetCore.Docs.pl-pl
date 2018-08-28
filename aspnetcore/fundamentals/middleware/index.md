---
title: Oprogramowanie pośredniczące platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat oprogramowania pośredniczącego platformy ASP.NET Core i potok żądań.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: e6dc76b7cb80e0dfda102df5aefb5d9ce9b821ed
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055809"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="66aa8-103">Oprogramowanie pośredniczące platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66aa8-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="66aa8-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="66aa8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="66aa8-105">Oprogramowanie pośredniczące to oprogramowanie, które jest umieszczone w potoku aplikacji do obsługi żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66aa8-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="66aa8-106">Poszczególne składniki:</span><span class="sxs-lookup"><span data-stu-id="66aa8-106">Each component:</span></span>

* <span data-ttu-id="66aa8-107">Pozwala wybrać, czy przekazywać żądania do następnego składnika w potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="66aa8-108">Można wykonywać pracę, przed i po wywołaniu następny składnik w potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="66aa8-109">Delegaty żądania są używane do tworzenia potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="66aa8-110">Delegaty żądania obsługi danego żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="66aa8-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="66aa8-111">Żądania, obiekty delegowane są skonfigurowane przy użyciu <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, i <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="66aa8-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="66aa8-112">Delegat pojedynczego żądania może być określony w tekście jako metodę anonimową (nazywane w tekście oprogramowania pośredniczącego), lub można zdefiniować klasy wielokrotnego użytku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="66aa8-113">Te klasy wielokrotnego użytku i metod anonimowych w tekście są *oprogramowania pośredniczącego*, nazywane również *składników oprogramowania pośredniczącego*.</span><span class="sxs-lookup"><span data-stu-id="66aa8-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="66aa8-114">Każdy składnik oprogramowania pośredniczącego w potoku żądania jest odpowiedzialny za wywoływanie następny składnik w potoku lub zwarcie potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="66aa8-115"><xref:migration/http-modules> wyjaśnia różnicę pomiędzy potoki żądania w programie ASP.NET Core i ASP.NET 4.x i udostępnia więcej przykładów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="66aa8-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="66aa8-116">Tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="66aa8-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="66aa8-117">Potok żądań ASP.NET Core składa się z sekwencji obiektów delegowanych żądania, o nazwie jedna po drugiej.</span><span class="sxs-lookup"><span data-stu-id="66aa8-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="66aa8-118">Poniższy diagram ilustruje pojęcia.</span><span class="sxs-lookup"><span data-stu-id="66aa8-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="66aa8-119">Wątek wykonywania następuje zamiast czarnych strzałek.</span><span class="sxs-lookup"><span data-stu-id="66aa8-119">The thread of execution follows the black arrows.</span></span>

![Wyświetlanie żądań przychodzących, przetwarzania za pomocą trzech middlewares i odpowiedzi, wychodzenia z aplikacji wzorca przetwarzania żądania.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="66aa8-123">Każdy delegat mogą wykonywać operacje, przed i po następnym delegata.</span><span class="sxs-lookup"><span data-stu-id="66aa8-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="66aa8-124">Obiekt delegowany można też przekazuje żądania do następnej delegata, która jest wywoływana *zwarcie Potok żądań*.</span><span class="sxs-lookup"><span data-stu-id="66aa8-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="66aa8-125">Zwarcie jest często pożądane, ponieważ takie rozwiązanie pomaga uniknąć niepotrzebnych pracy.</span><span class="sxs-lookup"><span data-stu-id="66aa8-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="66aa8-126">Na przykład statyczne pliki oprogramowania pośredniczącego wrócić żądanie dotyczące pliku statycznego i zwarcie pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="66aa8-127">Delegatów obsługi wyjątków są nazywane na wczesnym etapie potoku, dlatego ich może przechwytywać wyjątki, które występują w późniejszym etapie w potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="66aa8-128">Najprostsza możliwa aplikacji ASP.NET Core ustawia delegat pojedynczego żądania, który obsługuje wszystkie żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="66aa8-129">Ten przypadek nie zawiera rzeczywistego żądania potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="66aa8-130">Zamiast tego jednego funkcja anonimowa jest wywoływana w odpowiedzi na każde żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="66aa8-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="66aa8-131">Pierwszy <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegata kończy potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="66aa8-132">Utworzyć łańcuch wielu delegatów żądanie, wraz z <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="66aa8-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="66aa8-133">`next` Parametr reprezentuje dalej delegata w potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="66aa8-134">Można zwarcie potok według *nie* wywoływania *dalej* parametru.</span><span class="sxs-lookup"><span data-stu-id="66aa8-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="66aa8-135">Zazwyczaj można wykonywać akcje przed i po następnym delegata, tak jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="66aa8-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="66aa8-136">Nie wywołuj `next.Invoke` po wysłaniu odpowiedzi do klienta.</span><span class="sxs-lookup"><span data-stu-id="66aa8-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="66aa8-137">Zmienia się na <xref:Microsoft.AspNetCore.Http.HttpResponse> po rozpoczęciu odpowiedzi zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="66aa8-138">Na przykład zmiany, takie jak ustawianie nagłówków i kod stanu zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="66aa8-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="66aa8-139">Zapisywanie w treści odpowiedzi po wywołaniu `next`:</span><span class="sxs-lookup"><span data-stu-id="66aa8-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="66aa8-140">Może to spowodować naruszenie protokołu.</span><span class="sxs-lookup"><span data-stu-id="66aa8-140">May cause a protocol violation.</span></span> <span data-ttu-id="66aa8-141">Na przykład zapisywanie ponad podanej `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="66aa8-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="66aa8-142">Może spowodować uszkodzenie format treści.</span><span class="sxs-lookup"><span data-stu-id="66aa8-142">May corrupt the body format.</span></span> <span data-ttu-id="66aa8-143">Na przykład zapis stopki HTML do pliku CSS.</span><span class="sxs-lookup"><span data-stu-id="66aa8-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="66aa8-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> jest to przydatne wskazówka, aby wskazać, czy nagłówki zostały wysłane, czy jednostka została zapisana.</span><span class="sxs-lookup"><span data-stu-id="66aa8-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="66aa8-145">Kolejność</span><span class="sxs-lookup"><span data-stu-id="66aa8-145">Order</span></span>

<span data-ttu-id="66aa8-146">Kolejność dodaną składników oprogramowania pośredniczącego w `Startup.Configure` metoda definiuje kolejność, w którym są wywoływane składników oprogramowania pośredniczącego na żądania i odwrotnej kolejności dla odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66aa8-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="66aa8-147">Kolejność jest krytyczny dla bezpieczeństwa, wydajności i funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="66aa8-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="66aa8-148">Następujące `Configure` metoda dodaje następujące składniki oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="66aa8-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="66aa8-149">Obsługa wyjątku/błędów</span><span class="sxs-lookup"><span data-stu-id="66aa8-149">Exception/error handling</span></span>
2. <span data-ttu-id="66aa8-150">Serwer plików statycznych</span><span class="sxs-lookup"><span data-stu-id="66aa8-150">Static file server</span></span>
3. <span data-ttu-id="66aa8-151">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="66aa8-151">Authentication</span></span>
4. <span data-ttu-id="66aa8-152">MVC</span><span class="sxs-lookup"><span data-stu-id="66aa8-152">MVC</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="66aa8-153">W poprzednim kodzie przykład, każda metoda rozszerzenia oprogramowanie pośredniczące jest uwidaczniany w <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> za pośrednictwem <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="66aa8-153">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="66aa8-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> Pierwszy składnik oprogramowania pośredniczącego jest dodawany do potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="66aa8-155">W związku z tym oprogramowanie pośredniczące programu obsługi wyjątków przechwytuje wszystkie wyjątki, które występują w nowszym wywołuje.</span><span class="sxs-lookup"><span data-stu-id="66aa8-155">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="66aa8-156">Statyczne oprogramowania pośredniczącego plików jest wywoływana na wczesnym etapie potoku, dzięki czemu mogą obsługiwać żądania i zwarcie bez pośrednictwa pozostałe składniki.</span><span class="sxs-lookup"><span data-stu-id="66aa8-156">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="66aa8-157">Statyczne oprogramowanie pośredniczące plikach udostępnia **nie** sprawdzeń autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="66aa8-157">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="66aa8-158">Wszystkich plików obsługiwanych przez, łącznie z tymi w ramach *wwwroot*, są publicznie dostępne.</span><span class="sxs-lookup"><span data-stu-id="66aa8-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="66aa8-159">Podejścia do zabezpieczania plików statycznych, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="66aa8-159">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="66aa8-160">Jeśli żądanie nie jest obsługiwany przez statyczne oprogramowanie pośredniczące plików, będzie przekazany z oprogramowaniem pośredniczącym uwierzytelniania (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), który przeprowadza uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="66aa8-160">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="66aa8-161">Uwierzytelnianie nie powodują pominięcie nieuwierzytelnione żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-161">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="66aa8-162">Mimo że uwierzytelniania oprogramowania pośredniczącego uwierzytelniania na podstawie żądań autoryzacji (i odrzucenia) występuje tylko wtedy, gdy MVC wybiera określonych akcji i kontrolerów MVC lub strony Razor.</span><span class="sxs-lookup"><span data-stu-id="66aa8-162">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="66aa8-163">Jeśli żądanie nie jest obsługiwany przez statyczne pliki oprogramowanie pośredniczące, przekazaniem do oprogramowania pośredniczącego tożsamości (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), który przeprowadza uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="66aa8-163">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="66aa8-164">Tożsamość nie powodują pominięcie nieuwierzytelnione żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-164">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="66aa8-165">Mimo że tożsamość uwierzytelniania na podstawie żądań autoryzacji (i odrzucenia) występuje tylko wtedy, gdy MVC wybiera określonego kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="66aa8-165">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="66aa8-166">Poniższy przykład pokazuje kolejność oprogramowania pośredniczącego, w którym żądań dotyczących plików statycznych są obsługiwane przez statyczne pliki oprogramowania pośredniczącego, zanim oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66aa8-166">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="66aa8-167">Pliki statyczne nie są kompresowane z tym zamówieniem oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="66aa8-167">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="66aa8-168">Odpowiedzi MVC z <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> można skompresować.</span><span class="sxs-lookup"><span data-stu-id="66aa8-168">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="66aa8-169">Użyj, uruchom i mapy</span><span class="sxs-lookup"><span data-stu-id="66aa8-169">Use, Run, and Map</span></span>

<span data-ttu-id="66aa8-170">Konfigurowanie przy użyciu potoku HTTP `Use`, `Run`, i `Map`.</span><span class="sxs-lookup"><span data-stu-id="66aa8-170">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="66aa8-171">`Use` Metoda może zwarcie potoku (to znaczy, jeśli on nie wywołać `next` delegata żądania).</span><span class="sxs-lookup"><span data-stu-id="66aa8-171">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="66aa8-172">`Run` jest to Konwencja i niektórych składników oprogramowania pośredniczącego może narazić `Run[Middleware]` metod, które są uruchamiane na końcu potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-172">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="66aa8-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> rozszerzenia są używane jako Konwencję do tworzenia odgałęzień potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="66aa8-174">`Map*` gałęzie potoku żądania na podstawie dopasowań podanej ścieżki żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-174">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="66aa8-175">Jeśli ścieżka żądania rozpoczyna się od podanej ścieżce, jest wykonywana gałąź.</span><span class="sxs-lookup"><span data-stu-id="66aa8-175">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="66aa8-176">W poniższej tabeli przedstawiono żądań i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu.</span><span class="sxs-lookup"><span data-stu-id="66aa8-176">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="66aa8-177">Żądanie</span><span class="sxs-lookup"><span data-stu-id="66aa8-177">Request</span></span>             | <span data-ttu-id="66aa8-178">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="66aa8-178">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="66aa8-179">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="66aa8-179">localhost:1234</span></span>      | <span data-ttu-id="66aa8-180">Pozdrowienia ze-Map delegata.</span><span class="sxs-lookup"><span data-stu-id="66aa8-180">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="66aa8-181">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="66aa8-181">localhost:1234/map1</span></span> | <span data-ttu-id="66aa8-182">Mapa badania 1</span><span class="sxs-lookup"><span data-stu-id="66aa8-182">Map Test 1</span></span>                   |
| <span data-ttu-id="66aa8-183">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="66aa8-183">localhost:1234/map2</span></span> | <span data-ttu-id="66aa8-184">Mapa badania 2</span><span class="sxs-lookup"><span data-stu-id="66aa8-184">Map Test 2</span></span>                   |
| <span data-ttu-id="66aa8-185">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="66aa8-185">localhost:1234/map3</span></span> | <span data-ttu-id="66aa8-186">Pozdrowienia ze-Map delegata.</span><span class="sxs-lookup"><span data-stu-id="66aa8-186">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="66aa8-187">Gdy `Map` jest używane, ścieżka dopasowane jej ewentualny związek są usuwane z `HttpRequest.Path` i dołączonych do `HttpRequest.PathBase` dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-187">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="66aa8-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) odgałęzień potoku żądania na podstawie wyniku danego predykatu.</span><span class="sxs-lookup"><span data-stu-id="66aa8-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="66aa8-189">Wszelkie predykatu typu `Func<HttpContext, bool>` może być używany do mapowania żądania do nowej gałęzi w potoku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-189">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="66aa8-190">W poniższym przykładzie, predykat jest używany do wykrywania obecności zmiennej ciągu zapytania `branch`:</span><span class="sxs-lookup"><span data-stu-id="66aa8-190">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="66aa8-191">W poniższej tabeli przedstawiono żądań i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu.</span><span class="sxs-lookup"><span data-stu-id="66aa8-191">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="66aa8-192">Żądanie</span><span class="sxs-lookup"><span data-stu-id="66aa8-192">Request</span></span>                       | <span data-ttu-id="66aa8-193">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="66aa8-193">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="66aa8-194">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="66aa8-194">localhost:1234</span></span>                | <span data-ttu-id="66aa8-195">Pozdrowienia ze-Map delegata.</span><span class="sxs-lookup"><span data-stu-id="66aa8-195">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="66aa8-196">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="66aa8-196">localhost:1234/?branch=master</span></span> | <span data-ttu-id="66aa8-197">Używana gałąź = master</span><span class="sxs-lookup"><span data-stu-id="66aa8-197">Branch used = master</span></span>         |

<span data-ttu-id="66aa8-198">`Map` obsługuje zagnieżdżania, na przykład:</span><span class="sxs-lookup"><span data-stu-id="66aa8-198">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="66aa8-199">`Map` można również przeprowadzać dopasowywanie wiele segmentów jednocześnie:</span><span class="sxs-lookup"><span data-stu-id="66aa8-199">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="66aa8-200">Wbudowane oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="66aa8-200">Built-in middleware</span></span>

<span data-ttu-id="66aa8-201">Platforma ASP.NET Core jest dostarczany z następujących składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="66aa8-201">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="66aa8-202">*Kolejności* kolumna zawiera uwagi dotyczące umieszczenia oprogramowanie pośredniczące w potoku żądanie i na jakich warunkach oprogramowanie pośredniczące może zakończyć żądania i uniemożliwić innym oprogramowaniu pośredniczącym przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-202">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="66aa8-203">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="66aa8-203">Middleware</span></span> | <span data-ttu-id="66aa8-204">Opis</span><span class="sxs-lookup"><span data-stu-id="66aa8-204">Description</span></span> | <span data-ttu-id="66aa8-205">Kolejność</span><span class="sxs-lookup"><span data-stu-id="66aa8-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="66aa8-206">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="66aa8-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="66aa8-207">Zapewnia obsługę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-207">Provides authentication support.</span></span> | <span data-ttu-id="66aa8-208">Przed `HttpContext.User` jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="66aa8-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="66aa8-209">Terminala dla wywołania zwrotne OAuth.</span><span class="sxs-lookup"><span data-stu-id="66aa8-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="66aa8-210">CORS</span><span class="sxs-lookup"><span data-stu-id="66aa8-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="66aa8-211">Konfiguruje, Cross-Origin Resource Sharing.</span><span class="sxs-lookup"><span data-stu-id="66aa8-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="66aa8-212">Przed składników, które korzystają z mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="66aa8-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="66aa8-213">Diagnostyka</span><span class="sxs-lookup"><span data-stu-id="66aa8-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="66aa8-214">Konfiguruje diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="66aa8-214">Configures diagnostics.</span></span> | <span data-ttu-id="66aa8-215">Przed składniki, które generują błędy.</span><span class="sxs-lookup"><span data-stu-id="66aa8-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="66aa8-216">Nagłówki przekazywane</span><span class="sxs-lookup"><span data-stu-id="66aa8-216">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="66aa8-217">Przekazuje nagłówki przekazywane do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="66aa8-218">Przed składników korzystających z zaktualizowanymi polami (przykłady: schematu i hosta, adres IP klienta, metoda).</span><span class="sxs-lookup"><span data-stu-id="66aa8-218">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="66aa8-219">Zastąpienie metody HTTP</span><span class="sxs-lookup"><span data-stu-id="66aa8-219">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="66aa8-220">Zezwala na przychodzące żądania POST przesłonić metodę.</span><span class="sxs-lookup"><span data-stu-id="66aa8-220">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="66aa8-221">Przed składniki, które zużywają zaktualizowana metoda.</span><span class="sxs-lookup"><span data-stu-id="66aa8-221">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="66aa8-222">Przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="66aa8-222">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="66aa8-223">Przekieruj wszystkie żądania HTTP do HTTPS (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="66aa8-223">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="66aa8-224">Przed składniki, których wartość użycia adresu URL.</span><span class="sxs-lookup"><span data-stu-id="66aa8-224">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="66aa8-225">Zabezpieczenia transportu Strict HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="66aa8-225">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="66aa8-226">Oprogramowanie ulepszenie pośredniczące zabezpieczeń, który dodaje nagłówek odpowiedzi specjalne (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="66aa8-226">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="66aa8-227">Przed wysłaniem odpowiedzi i po składniki, które modyfikują żądania (na przykład nagłówki przekazywane, ponownego zapisywania adresów URL).</span><span class="sxs-lookup"><span data-stu-id="66aa8-227">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="66aa8-228">MVC</span><span class="sxs-lookup"><span data-stu-id="66aa8-228">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="66aa8-229">Przetwarza żądania przy użyciu stron MVC i Razor (platformy ASP.NET Core w wersji 2.0 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="66aa8-229">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="66aa8-230">Terminal, jeśli żądanie jest zgodny z trasą.</span><span class="sxs-lookup"><span data-stu-id="66aa8-230">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="66aa8-231">OWIN</span><span class="sxs-lookup"><span data-stu-id="66aa8-231">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="66aa8-232">Współdziałanie z aplikacji opartych na OWIN, serwerów i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="66aa8-232">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="66aa8-233">Terminal, jeśli oprogramowanie pośredniczące OWIN w pełni przetwarza żądanie.</span><span class="sxs-lookup"><span data-stu-id="66aa8-233">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="66aa8-234">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="66aa8-234">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="66aa8-235">Zapewnia obsługę buforowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66aa8-235">Provides support for caching responses.</span></span> | <span data-ttu-id="66aa8-236">Przed składniki, które wymagają buforowania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-236">Before components that require caching.</span></span> |
| [<span data-ttu-id="66aa8-237">Kompresja odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="66aa8-237">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="66aa8-238">Zapewnia obsługę kompresowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66aa8-238">Provides support for compressing responses.</span></span> | <span data-ttu-id="66aa8-239">Przed składniki, które wymagają kompresji.</span><span class="sxs-lookup"><span data-stu-id="66aa8-239">Before components that require compression.</span></span> |
| [<span data-ttu-id="66aa8-240">Lokalizacja żądania</span><span class="sxs-lookup"><span data-stu-id="66aa8-240">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="66aa8-241">Zapewnia obsługę lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="66aa8-241">Provides localization support.</span></span> | <span data-ttu-id="66aa8-242">Przed składniki poufnych lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="66aa8-242">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="66aa8-243">Routing</span><span class="sxs-lookup"><span data-stu-id="66aa8-243">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="66aa8-244">Definiuje i ogranicza trasy żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-244">Defines and constrains request routes.</span></span> | <span data-ttu-id="66aa8-245">Terminalu zgodnych tras.</span><span class="sxs-lookup"><span data-stu-id="66aa8-245">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="66aa8-246">Sesja</span><span class="sxs-lookup"><span data-stu-id="66aa8-246">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="66aa8-247">Zapewnia obsługę zarządzania sesjami użytkownika.</span><span class="sxs-lookup"><span data-stu-id="66aa8-247">Provides support for managing user sessions.</span></span> | <span data-ttu-id="66aa8-248">Przed składniki, które wymagają sesji.</span><span class="sxs-lookup"><span data-stu-id="66aa8-248">Before components that require Session.</span></span> |
| [<span data-ttu-id="66aa8-249">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="66aa8-249">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="66aa8-250">Zapewnia obsługę obsługujących pliki statyczne oraz przeglądanie katalogów.</span><span class="sxs-lookup"><span data-stu-id="66aa8-250">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="66aa8-251">Terminal, gdy żądanie pasuje do pliku.</span><span class="sxs-lookup"><span data-stu-id="66aa8-251">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="66aa8-252">Ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="66aa8-252">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="66aa8-253">Zapewnia obsługę ponownego zapisywania adresów URL przekierowywania żądań.</span><span class="sxs-lookup"><span data-stu-id="66aa8-253">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="66aa8-254">Przed składniki, których wartość użycia adresu URL.</span><span class="sxs-lookup"><span data-stu-id="66aa8-254">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="66aa8-255">Obiekty WebSocket</span><span class="sxs-lookup"><span data-stu-id="66aa8-255">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="66aa8-256">Włącza protokół Websocket.</span><span class="sxs-lookup"><span data-stu-id="66aa8-256">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="66aa8-257">Przed składniki, które są wymagane w celu umożliwienia akceptowania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="66aa8-257">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="66aa8-258">Pisanie oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="66aa8-258">Write middleware</span></span>

<span data-ttu-id="66aa8-259">Oprogramowanie pośredniczące jest zazwyczaj hermetyzowane w klasie i widoczne z metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="66aa8-259">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="66aa8-260">Należy wziąć pod uwagę następujące oprogramowanie pośredniczące, które ustawia kulturę bieżącego żądania z ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="66aa8-260">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="66aa8-261">Poprzedni przykładowy kod jest używany do zademonstrowania tworzenia składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="66aa8-261">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="66aa8-262">Obsługę wbudowanych lokalizacja platformy ASP.NET Core, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="66aa8-262">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="66aa8-263">Możesz przetestować oprogramowanie pośredniczące, przekazując w kulturze, na przykład `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="66aa8-263">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="66aa8-264">Poniższy kod powoduje delegata oprogramowania pośredniczącego do klasy:</span><span class="sxs-lookup"><span data-stu-id="66aa8-264">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="66aa8-265">Oprogramowanie pośredniczące `Task` nazwę metody musi być `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="66aa8-265">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="66aa8-266">W programie ASP.NET Core 2.0 lub nowszej, może to być albo `Invoke` lub `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="66aa8-266">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="66aa8-267">Następujące metody rozszerzenia udostępnia oprogramowanie pośredniczące za pośrednictwem <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="66aa8-267">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="66aa8-268">Poniższy kod wywołuje oprogramowanie pośredniczące z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="66aa8-268">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="66aa8-269">Oprogramowanie pośredniczące powinno wykonać [jawne zależności zasady](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) uwidaczniając jego zależności w jego konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="66aa8-269">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="66aa8-270">Oprogramowanie pośredniczące jest tworzona raz na *okres istnienia aplikacji*.</span><span class="sxs-lookup"><span data-stu-id="66aa8-270">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="66aa8-271">Zobacz [zależności żądania](#per-request-dependencies) sekcji, jeśli zachodzi potrzeba udostępniania usług za pomocą oprogramowania pośredniczącego w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="66aa8-271">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="66aa8-272">Składniki oprogramowania pośredniczącego może rozpoznać ich zależności z [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) za pośrednictwem parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="66aa8-272">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="66aa8-273">[UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) mogą również akceptować dodatkowe parametry bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="66aa8-273">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="66aa8-274">Zależności żądania</span><span class="sxs-lookup"><span data-stu-id="66aa8-274">Per-request dependencies</span></span>

<span data-ttu-id="66aa8-275">Ponieważ oprogramowanie pośredniczące jest konstruowany przy uruchamianiu aplikacji, nie dla poszczególnych żądań, *zakresie* okres istnienia, obejmujący usługi używane przez oprogramowanie pośredniczące konstruktory nie są udostępniane przy użyciu innych typów zależności, wprowadzony podczas każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="66aa8-275">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="66aa8-276">Jeśli musisz udostępnić *zakresie* usługi między oprogramowania pośredniczącego a innymi typami danych, należy dodać tych usług `Invoke` podpis metody.</span><span class="sxs-lookup"><span data-stu-id="66aa8-276">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="66aa8-277">`Invoke` Metoda może obsługiwać dodatkowe parametry, które są wypełniane przez DI:</span><span class="sxs-lookup"><span data-stu-id="66aa8-277">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="66aa8-278">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="66aa8-278">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
