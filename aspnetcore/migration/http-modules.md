---
title: Przenieść programów obsługi HTTP i modułów platformy ASP.NET Core oprogramowania pośredniczącego
author: rick-anderson
description: ''
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: cbdef871ffc3269e3118d23ed20306a71b9df030
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741053"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="396e9-102">Przenieść programów obsługi HTTP i modułów platformy ASP.NET Core oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="396e9-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="396e9-103">Przez [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="396e9-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="396e9-104">W tym artykule pokazano, jak przeprowadzić migrację istniejących ASP.NET [moduły HTTP i programów obsługi system.webserver](/iis/configuration/system.webserver/) do platformy ASP.NET Core [oprogramowanie pośredniczące](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="396e9-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="396e9-105">Moduły i uruchomić ponownie programów obsługi</span><span class="sxs-lookup"><span data-stu-id="396e9-105">Modules and handlers revisited</span></span>

<span data-ttu-id="396e9-106">Przed przystąpieniem do platformy ASP.NET Core oprogramowanie pośredniczące, umożliwia najpierw recap jak działają moduły HTTP i obsługi:</span><span class="sxs-lookup"><span data-stu-id="396e9-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Obsługa modułów](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="396e9-108">**Programy obsługi są:**</span><span class="sxs-lookup"><span data-stu-id="396e9-108">**Handlers are:**</span></span>

   * <span data-ttu-id="396e9-109">Klasy, które implementują [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="396e9-109">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="396e9-110">Umożliwia obsługę żądania za pomocą podanej nazwy pliku lub rozszerzenie, takich jak *.report*</span><span class="sxs-lookup"><span data-stu-id="396e9-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="396e9-111">[Skonfigurowane](/iis/configuration/system.webserver/handlers/) w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="396e9-111">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="396e9-112">**Moduły są:**</span><span class="sxs-lookup"><span data-stu-id="396e9-112">**Modules are:**</span></span>

   * <span data-ttu-id="396e9-113">Klasy, które implementują [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="396e9-113">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="396e9-114">Wywoływane dla każdego żądania</span><span class="sxs-lookup"><span data-stu-id="396e9-114">Invoked for every request</span></span>

   * <span data-ttu-id="396e9-115">Możliwość zwarcia (zatrzymać dalsze przetwarzanie żądania)</span><span class="sxs-lookup"><span data-stu-id="396e9-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="396e9-116">Można dodać do odpowiedzi HTTP lub utworzyć własne</span><span class="sxs-lookup"><span data-stu-id="396e9-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="396e9-117">[Skonfigurowane](/iis/configuration/system.webserver/modules/) w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="396e9-117">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="396e9-118">**Kolejność, w którym modułów przetwarzania przychodzących żądań jest określany przez:**</span><span class="sxs-lookup"><span data-stu-id="396e9-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="396e9-119">[Cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx), która jest zdarzenia serii wywoływane przez platformę ASP.NET: [powstaniem zdarzenia BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)itp. Każdy moduł można utworzyć programu obsługi dla co najmniej jednego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="396e9-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="396e9-120">Dla tego samego zdarzenia kolejności, w której jest skonfigurowany w *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="396e9-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="396e9-121">Oprócz modułów, można dodać obsługi zdarzeń cyklu życia z *Global.asax.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="396e9-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="396e9-122">Po obsługi w modułach skonfigurowanych Uruchom te programy obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="396e9-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="396e9-123">Z programów obsługi i modułów oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="396e9-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="396e9-124">**Oprogramowanie pośredniczące są prostsze niż moduły HTTP i obsługi:**</span><span class="sxs-lookup"><span data-stu-id="396e9-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="396e9-125">Modułów, programów obsługi, *Global.asax.cs*, *Web.config* (z wyjątkiem konfiguracji usług IIS) i znikną cyklu życia aplikacji</span><span class="sxs-lookup"><span data-stu-id="396e9-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="396e9-126">Role, moduły i programy obsługi zostały przejęte przez oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="396e9-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="396e9-127">Oprogramowanie pośredniczące są skonfigurowane przy użyciu kodu, a nie w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="396e9-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="396e9-128">[Rozgałęzienie potoku](xref:fundamentals/middleware/index#middleware-run-map-use) umożliwia wysyłanie żądań do określonego oprogramowania pośredniczącego, oparte na nie tylko adres URL, ale także na nagłówków żądań, ciągi zapytań, itp.</span><span class="sxs-lookup"><span data-stu-id="396e9-128">[Pipeline branching](xref:fundamentals/middleware/index#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="396e9-129">**Oprogramowanie pośredniczące są bardzo podobne do modułów:**</span><span class="sxs-lookup"><span data-stu-id="396e9-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="396e9-130">Wywoływane w zasadzie dla każdego żądania</span><span class="sxs-lookup"><span data-stu-id="396e9-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="396e9-131">Możliwość zwarcia przez żądanie, [nie przekazanie żądania do następnego oprogramowania pośredniczącego](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="396e9-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="396e9-132">Można tworzyć własne odpowiedzi HTTP</span><span class="sxs-lookup"><span data-stu-id="396e9-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="396e9-133">**Oprogramowanie pośredniczące i moduły są przetwarzane w innej kolejności:**</span><span class="sxs-lookup"><span data-stu-id="396e9-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="396e9-134">Kolejność oprogramowania pośredniczącego opiera się na kolejność, w którym wstawiane są one z potokiem żądań, gdy kolejność modułów opiera się głównie na [cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx) zdarzeń</span><span class="sxs-lookup"><span data-stu-id="396e9-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="396e9-135">Kolejność oprogramowania pośredniczącego odpowiedzi jest odwrotnie niż dla żądania, podczas kolejność modułów jest taki sam dla żądań i odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="396e9-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="396e9-136">Zobacz [tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="396e9-136">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Oprogramowanie pośredniczące](http-modules/_static/middleware.png)

<span data-ttu-id="396e9-138">Należy zwrócić uwagę, jak na ilustracji powyżej, oprogramowanie pośredniczące uwierzytelniania short-circuited żądania.</span><span class="sxs-lookup"><span data-stu-id="396e9-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="396e9-139">Migrowanie kodu modułu do oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="396e9-139">Migrating module code to middleware</span></span>

<span data-ttu-id="396e9-140">Istniejący moduł HTTP będzie wyglądać podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="396e9-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="396e9-141">Jak pokazano w [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) strony, oprogramowanie pośredniczące platformy ASP.NET Core jest klasa, która przedstawia `Invoke` metody z argumentami `HttpContext` i zwracanie `Task`.</span><span class="sxs-lookup"><span data-stu-id="396e9-141">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="396e9-142">Nowego oprogramowania pośredniczącego będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="396e9-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="396e9-143">Powyższy szablon oprogramowanie pośredniczące została pobrana z sekcji [zapisywania oprogramowanie pośredniczące](xref:fundamentals/middleware/index#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="396e9-143">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).</span></span>

<span data-ttu-id="396e9-144">*MyMiddlewareExtensions* Klasa pomocy ułatwia konfigurowanie oprogramowania pośredniczącego w Twojej `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="396e9-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="396e9-145">`UseMyMiddleware` Metody dodaje klasy oprogramowania pośredniczącego do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="396e9-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="396e9-146">Usług wymaganych przez oprogramowanie pośredniczące uzyskać wprowadzonym w Konstruktorze przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="396e9-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="396e9-147">Modułu może zakończyć żądania, na przykład jeśli użytkownik nie ma uprawnień:</span><span class="sxs-lookup"><span data-stu-id="396e9-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="396e9-148">Oprogramowanie pośredniczące obsługuje to nie wywołując `Invoke` na następne oprogramowanie pośredniczące w potoku.</span><span class="sxs-lookup"><span data-stu-id="396e9-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="396e9-149">Należy pamiętać, że to nie pełni zakończyć żądania, ponieważ poprzednie middlewares nadal zostanie wywołany, gdy odpowiedź sprawia, że jego sposób za pośrednictwem potoku.</span><span class="sxs-lookup"><span data-stu-id="396e9-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="396e9-150">Funkcje z modułu w przypadku migracji do nowego oprogramowania pośredniczącego, może się okazać, że kod nie kompilacji, ponieważ `HttpContext` klasy zmienił się znacznie w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="396e9-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="396e9-151">[Później](#migrating-to-the-new-httpcontext), zobaczysz jak przeprowadzić migrację do nowego element HttpContext Core ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="396e9-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="396e9-152">Migrowanie modułu wstawiania do potoku żądania</span><span class="sxs-lookup"><span data-stu-id="396e9-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="396e9-153">Moduły HTTP zazwyczaj są dodawane do potoku żądania przy użyciu *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="396e9-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="396e9-154">Konwertuj to przez [Dodawanie nowego oprogramowania pośredniczącego](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) do potoku żądania w Twojej `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="396e9-154">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="396e9-155">Dokładne miejsce w potoku, gdzie wstawić nowego oprogramowania pośredniczącego zależy od zdarzenia, które go obsługiwane jako modułu (`BeginRequest`, `EndRequest`, itd.) i ich kolejność na liście modułów w *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="396e9-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="396e9-156">Wcześniej wspomniano, nie bez cyklu życia aplikacji w ASP.NET Core a kolejności, w której odpowiedzi są przetwarzane przez oprogramowanie pośredniczące różni się od kolejności użytej przez moduły.</span><span class="sxs-lookup"><span data-stu-id="396e9-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="396e9-157">To może należy zdecydować, czy porządkowania trudniejsze.</span><span class="sxs-lookup"><span data-stu-id="396e9-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="396e9-158">Jeśli kolejność staje się problem, można podzielić modułu na wiele składników oprogramowania pośredniczącego, które może zostać określona niezależnie.</span><span class="sxs-lookup"><span data-stu-id="396e9-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="396e9-159">Migrowanie kod obsługi oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="396e9-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="396e9-160">Program obsługi HTTP wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="396e9-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="396e9-161">W projekcie platformy ASP.NET Core będzie to przełożyć na oprogramowanie pośredniczące podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="396e9-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="396e9-162">To oprogramowanie pośredniczące jest bardzo podobny do odpowiadającego modułów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="396e9-163">Tylko rzeczywistych różnica jest to, że w tym miejscu jest Brak wywołania `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="396e9-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="396e9-164">Ma to sens, ponieważ procedura obsługi jest na końcu potoku żądania zostanie nie następne oprogramowanie pośredniczące do wywołania.</span><span class="sxs-lookup"><span data-stu-id="396e9-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="396e9-165">Migrowanie wstawienia potoku żądania obsługi</span><span class="sxs-lookup"><span data-stu-id="396e9-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="396e9-166">Konfigurowanie programu obsługi HTTP odbywa się *Web.config* i wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="396e9-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="396e9-167">Można przekonwertować to przez dodanie nowych obsługi oprogramowania pośredniczącego do potoku żądania w Twojej `Startup` klasy, podobnie jak przekonwertować z modułów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="396e9-168">Tego podejścia przy rozwiązywaniu problemu jest to, że wszystkie żądania będzie wysyłać do nowego oprogramowania pośredniczącego obsługi.</span><span class="sxs-lookup"><span data-stu-id="396e9-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="396e9-169">Jednak tylko żądania z danym rozszerzeniem nawiązać oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="396e9-170">Która pozwoli uzyskać te same funkcje, jaką miał programu obsługi HTTP.</span><span class="sxs-lookup"><span data-stu-id="396e9-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="396e9-171">Rozwiązanie polega na Rozgałęzienie potoku żądań dla danego rozszerzenia, przy użyciu `MapWhen` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="396e9-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="396e9-172">Można to zrobić w tym samym `Configure` metody, gdzie możesz dodać inne oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="396e9-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="396e9-173">`MapWhen` przyjmuje następujące parametry:</span><span class="sxs-lookup"><span data-stu-id="396e9-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="396e9-174">Wyrażenie lambda, która przyjmuje `HttpContext` i zwraca `true` w przypadku żądania powinien wyłączenia gałęzi.</span><span class="sxs-lookup"><span data-stu-id="396e9-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="396e9-175">Oznacza to, że można rozgałęzić żądania nie tylko na podstawie ich rozszerzenia, ale także nagłówków żądań parametrów ciągu zapytania, itp.</span><span class="sxs-lookup"><span data-stu-id="396e9-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="396e9-176">Wyrażenie lambda, która przyjmuje `IApplicationBuilder` i dodaje całe oprogramowanie pośredniczące dla gałęzi.</span><span class="sxs-lookup"><span data-stu-id="396e9-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="396e9-177">Oznacza to, że można dodać dodatkowe oprogramowanie pośredniczące do gałęzi przed obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="396e9-178">Oprogramowanie pośredniczące dodane do potoku przed gałęzi, zostanie wywołany we wszystkich żądaniach; gałąź zostanie nie mają wpływu na nich.</span><span class="sxs-lookup"><span data-stu-id="396e9-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="396e9-179">Opcje oprogramowania pośredniczącego przy użyciu wzorca opcje ładowania</span><span class="sxs-lookup"><span data-stu-id="396e9-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="396e9-180">Niektóre moduły i programy obsługi mają opcje konfiguracji, które są przechowywane w *Web.config*. Jednak w przypadku platformy ASP.NET Core nowy model konfiguracji jest używany zamiast *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="396e9-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="396e9-181">Nowy [system konfiguracji](xref:fundamentals/configuration/index) umożliwia te opcje, aby rozwiązać ten problem:</span><span class="sxs-lookup"><span data-stu-id="396e9-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="396e9-182">Bezpośrednio wstrzyknąć opcje w oprogramowaniu pośredniczącym, jak pokazano w [następnej sekcji](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="396e9-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="396e9-183">Użyj [wzorzec opcje](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="396e9-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="396e9-184">Tworzenie klasy utrzymującej opcje oprogramowania pośredniczącego, na przykład:</span><span class="sxs-lookup"><span data-stu-id="396e9-184">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="396e9-185">Przechowywanie wartości opcji</span><span class="sxs-lookup"><span data-stu-id="396e9-185">Store the option values</span></span>

   <span data-ttu-id="396e9-186">System konfiguracji umożliwia przechowywanie wartości opcji dowolnym ma.</span><span class="sxs-lookup"><span data-stu-id="396e9-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="396e9-187">Jednak najbardziej Lokacje użyj *appsettings.json*, więc przeniesiemy tego podejścia:</span><span class="sxs-lookup"><span data-stu-id="396e9-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="396e9-188">*MyMiddlewareOptionsSection* Oto nazwę sekcji.</span><span class="sxs-lookup"><span data-stu-id="396e9-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="396e9-189">Nie musi być taka sama jak nazwa klasy opcje.</span><span class="sxs-lookup"><span data-stu-id="396e9-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="396e9-190">Kojarzenie wartości opcji z klasy opcji</span><span class="sxs-lookup"><span data-stu-id="396e9-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="396e9-191">Wzorzec opcje używa framework iniekcji zależności platformy ASP.NET Core w celu skojarzenia typu opcje (takich jak `MyMiddlewareOptions`) z `MyMiddlewareOptions` obiektu, który ma rzeczywistego opcje.</span><span class="sxs-lookup"><span data-stu-id="396e9-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="396e9-192">Aktualizacja Twojego `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="396e9-192">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="396e9-193">Jeśli używasz *appsettings.json*, dodaj go do konstruktora konfiguracji w `Startup` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="396e9-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="396e9-194">Skonfiguruj usługę opcje:</span><span class="sxs-lookup"><span data-stu-id="396e9-194">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="396e9-195">Skojarz opcji z klasy opcje:</span><span class="sxs-lookup"><span data-stu-id="396e9-195">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="396e9-196">Wstaw opcje do Twojej konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="396e9-197">Efekt jest podobny do iniekcję opcje do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="396e9-197">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="396e9-198">[UseMiddleware](#http-modules-usemiddleware) — metoda rozszerzenia, który dodaje oprogramowaniu pośredniczącym, aby `IApplicationBuilder` zajmuje się iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="396e9-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="396e9-199">To nie jest ograniczona do `IOptions` obiektów.</span><span class="sxs-lookup"><span data-stu-id="396e9-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="396e9-200">Drugi obiekt, który wymaga oprogramowania pośredniczącego mogą zostać dodane w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="396e9-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="396e9-201">Opcje oprogramowania pośredniczącego za pomocą iniekcji bezpośredniego ładowania</span><span class="sxs-lookup"><span data-stu-id="396e9-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="396e9-202">Wzorzec opcje ma tę zaletę, utworzonych utracić sprzężenia między wartości opcji i ich odbiorców.</span><span class="sxs-lookup"><span data-stu-id="396e9-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="396e9-203">Po klasy opcji został skojarzony z wartości rzeczywistych opcje, inne klasy mogą korzystać z opcji przez strukturę iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="396e9-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="396e9-204">Nie istnieje potrzeba do przekazania wokół wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="396e9-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="396e9-205">To dzieli jednak jeśli chcesz użyć tego samego oprogramowania pośredniczącego dwa razy, przy użyciu różnych opcji.</span><span class="sxs-lookup"><span data-stu-id="396e9-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="396e9-206">Na przykład autoryzacji oprogramowanie pośredniczące używane w różnych gałęziach, dzięki czemu różne role.</span><span class="sxs-lookup"><span data-stu-id="396e9-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="396e9-207">Dwa obiekty różnych opcji nie można skojarzyć z klasy jednej opcji.</span><span class="sxs-lookup"><span data-stu-id="396e9-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="396e9-208">Rozwiązanie to uzyskać obiekty opcje przy użyciu wartości rzeczywistych opcje w Twojej `Startup` klasy i przekazywać je bezpośrednio na każde wystąpienie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="396e9-209">Dodaj drugi klucz do *appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="396e9-209">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="396e9-210">Aby dodać drugi zestaw opcji, aby *appsettings.json* plików, użyj nowego klucza w celu jego jednoznacznej identyfikacji:</span><span class="sxs-lookup"><span data-stu-id="396e9-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="396e9-211">Pobieranie wartości opcji i przekazywanie ich do oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="396e9-212">`Use...` — Metoda rozszerzenia (która dodaje oprogramowania pośredniczącego do potoku) to logiczne miejsce do przekazywania wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="396e9-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="396e9-213">Włącz oprogramowaniu pośredniczącym, aby mieć parametr opcji.</span><span class="sxs-lookup"><span data-stu-id="396e9-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="396e9-214">Udostępnij przeciążenie metody `Use...` — metoda rozszerzenia (która przyjmuje parametr opcje i przekazuje je do `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="396e9-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="396e9-215">Gdy `UseMiddleware` jest wywoływana z parametrami przekazuje parametry do Twojej konstruktora oprogramowania pośredniczącego podczas tworzenia wystąpień obiektu oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="396e9-216">Należy zwrócić uwagę, jak to opakowuje obiektu opcje w `OptionsWrapper` obiektu.</span><span class="sxs-lookup"><span data-stu-id="396e9-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="396e9-217">To implementuje `IOptions`, zgodnie z oczekiwaniami konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="396e9-218">Migracja do nowego element HttpContext</span><span class="sxs-lookup"><span data-stu-id="396e9-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="396e9-219">Wcześniej przedstawiono który `Invoke` metoda w oprogramowania pośredniczącego przyjmuje parametr typu `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="396e9-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="396e9-220">`HttpContext` znacznie została zmieniona w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="396e9-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="396e9-221">W tej sekcji przedstawiono sposób tłumaczenia najczęściej używane właściwości [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) do nowego `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="396e9-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="396e9-222">Element HttpContext</span><span class="sxs-lookup"><span data-stu-id="396e9-222">HttpContext</span></span>

<span data-ttu-id="396e9-223">**HttpContext.Items** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="396e9-224">**Unikatowy identyfikator (odpowiednika System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="396e9-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="396e9-225">Zawiera unikatowy identyfikator dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="396e9-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="396e9-226">Bardzo przydatny w dziennikach.</span><span class="sxs-lookup"><span data-stu-id="396e9-226">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="396e9-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="396e9-227">HttpContext.Request</span></span>

<span data-ttu-id="396e9-228">**HttpContext.Request.HttpMethod** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="396e9-229">**HttpContext.Request.QueryString** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="396e9-230">**HttpContext.Request.Url** i **HttpContext.Request.RawUrl** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="396e9-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="396e9-231">**HttpContext.Request.IsSecureConnection** translates to:</span><span class="sxs-lookup"><span data-stu-id="396e9-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="396e9-232">**HttpContext.Request.UserHostAddress** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="396e9-233">**HttpContext.Request.Cookies** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="396e9-234">**HttpContext.Request.RequestContext.RouteData** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="396e9-235">**HttpContext.Request.Headers** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="396e9-236">**HttpContext.Request.UserAgent** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="396e9-237">**HttpContext.Request.UrlReferrer** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="396e9-238">**HttpContext.Request.ContentType** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="396e9-239">**HttpContext.Request.Form** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="396e9-240">Odczytać wartości formularza, tylko wtedy, gdy typ zawartości sub *x--www-form-urlencoded* lub *dane formularza*.</span><span class="sxs-lookup"><span data-stu-id="396e9-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="396e9-241">**HttpContext.Request.InputStream** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="396e9-242">Ten kod należy używać tylko w obsługi typu oprogramowanie pośredniczące, na końcu potoku.</span><span class="sxs-lookup"><span data-stu-id="396e9-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="396e9-243">Można odczytać pierwotne treści, jak pokazano powyżej tylko raz dla żądania.</span><span class="sxs-lookup"><span data-stu-id="396e9-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="396e9-244">Oprogramowanie pośredniczące próby odczytu treści po pierwszym odczytu będzie odczytywał element body puste.</span><span class="sxs-lookup"><span data-stu-id="396e9-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="396e9-245">To nie ma zastosowania do odczytywania formularza, jak pokazano wcześniej, ponieważ która jest wykonywana z buforu.</span><span class="sxs-lookup"><span data-stu-id="396e9-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="396e9-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="396e9-246">HttpContext.Response</span></span>

<span data-ttu-id="396e9-247">**HttpContext.Response.Status** i **HttpContext.Response.StatusDescription** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="396e9-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="396e9-248">**HttpContext.Response.ContentEncoding** i **HttpContext.Response.ContentType** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="396e9-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="396e9-249">**HttpContext.Response.ContentType** na jego własnej tłumaczy także do:</span><span class="sxs-lookup"><span data-stu-id="396e9-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="396e9-250">**HttpContext.Response.Output** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="396e9-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="396e9-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="396e9-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="396e9-252">Wysyłaniu pliku omówiono [tutaj](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="396e9-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="396e9-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="396e9-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="396e9-254">Wysyłanie nagłówków odpowiedzi jest złożona faktem, że ustawienie po niczego zostały zapisane w treści odpowiedzi, ich nie będą wysyłane.</span><span class="sxs-lookup"><span data-stu-id="396e9-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="396e9-255">Rozwiązanie jest ustalenie metody wywołania zwrotnego, która będzie wywoływana po prawej przed zapisaniem na uruchamia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="396e9-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="396e9-256">Najlepiej odbywa się na początku `Invoke` metoda oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="396e9-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="396e9-257">Jest ta metoda wywołania zwrotnego ustawiający nagłówkach odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="396e9-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="396e9-258">Poniższy kod ustawia metodę wywołania zwrotnego o nazwie `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="396e9-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="396e9-259">`SetHeaders` Metody wywołania zwrotnego będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="396e9-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="396e9-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="396e9-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="396e9-261">Pliki cookie są przesyłane do przeglądarki w *Set-Cookie* nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="396e9-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="396e9-262">W związku z tym wysyłanie plików cookie wymaga tego samego wywołania zwrotnego jako używane do wysyłania nagłówki odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="396e9-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="396e9-263">`SetCookies` Metody wywołania zwrotnego będzie wyglądać podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="396e9-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="396e9-264">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="396e9-264">Additional resources</span></span>

* [<span data-ttu-id="396e9-265">Omówienie moduły HTTP i programów obsługi HTTP</span><span class="sxs-lookup"><span data-stu-id="396e9-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="396e9-266">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="396e9-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="396e9-267">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="396e9-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="396e9-268">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="396e9-268">Middleware</span></span>](xref:fundamentals/middleware/index)
