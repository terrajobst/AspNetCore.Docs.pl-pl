---
title: "Migrowanie programów obsługi HTTP i modułów platformy ASP.NET Core oprogramowania pośredniczącego"
author: rick-anderson
description: 
keywords: Platformy ASP.NET Core
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.assetid: 9c826a76-fbd2-46b5-978d-6ca6df53531a
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: f217e5264742826f285444dcbaea4b28b97c4d7e
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="2af4f-103">Migrowanie programów obsługi HTTP i modułów platformy ASP.NET Core oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="2af4f-103">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="2af4f-104">Przez [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="2af4f-104">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="2af4f-105">W tym artykule pokazano, jak przeprowadzić migrację istniejących ASP.NET [moduły HTTP i programów obsługi system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) do platformy ASP.NET Core [oprogramowanie pośredniczące](../fundamentals/middleware.md).</span><span class="sxs-lookup"><span data-stu-id="2af4f-105">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](../fundamentals/middleware.md).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="2af4f-106">Moduły i uruchomić ponownie programów obsługi</span><span class="sxs-lookup"><span data-stu-id="2af4f-106">Modules and handlers revisited</span></span>

<span data-ttu-id="2af4f-107">Przed przystąpieniem do platformy ASP.NET Core oprogramowanie pośredniczące, umożliwia najpierw recap jak działają moduły HTTP i obsługi:</span><span class="sxs-lookup"><span data-stu-id="2af4f-107">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Obsługa modułów](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="2af4f-109">**Programy obsługi są:**</span><span class="sxs-lookup"><span data-stu-id="2af4f-109">**Handlers are:**</span></span>

   * <span data-ttu-id="2af4f-110">Klasy, które implementują [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="2af4f-110">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="2af4f-111">Umożliwia obsługę żądania za pomocą podanej nazwy pliku lub rozszerzenie, takich jak *.report*</span><span class="sxs-lookup"><span data-stu-id="2af4f-111">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="2af4f-112">[Skonfigurowane](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="2af4f-112">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="2af4f-113">**Moduły są:**</span><span class="sxs-lookup"><span data-stu-id="2af4f-113">**Modules are:**</span></span>

   * <span data-ttu-id="2af4f-114">Klasy, które implementują [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="2af4f-114">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="2af4f-115">Wywoływane dla każdego żądania</span><span class="sxs-lookup"><span data-stu-id="2af4f-115">Invoked for every request</span></span>

   * <span data-ttu-id="2af4f-116">Możliwość zwarcia (zatrzymać dalsze przetwarzanie żądania)</span><span class="sxs-lookup"><span data-stu-id="2af4f-116">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="2af4f-117">Można dodać do odpowiedzi HTTP lub utworzyć własne</span><span class="sxs-lookup"><span data-stu-id="2af4f-117">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="2af4f-118">[Skonfigurowane](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="2af4f-118">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="2af4f-119">**Kolejność, w którym modułów przetwarzania przychodzących żądań jest określany przez:**</span><span class="sxs-lookup"><span data-stu-id="2af4f-119">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="2af4f-120">[Cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx), która jest zdarzenia serii wywoływane przez platformę ASP.NET: [powstaniem zdarzenia BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)itp. Każdy moduł można utworzyć programu obsługi dla co najmniej jednego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="2af4f-120">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="2af4f-121">Dla tego samego zdarzenia kolejności, w której są konfigurowane w *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="2af4f-121">For the same event, the order in which they are configured in *Web.config*.</span></span>

<span data-ttu-id="2af4f-122">Oprócz modułów, można dodać obsługi zdarzeń cyklu życia z *Global.asax.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="2af4f-122">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="2af4f-123">Po obsługi w modułach skonfigurowanych Uruchom te programy obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="2af4f-123">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="2af4f-124">Z programów obsługi i modułów oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="2af4f-124">From handlers and modules to middleware</span></span>

<span data-ttu-id="2af4f-125">**Oprogramowanie pośredniczące są prostsze niż moduły HTTP i obsługi:**</span><span class="sxs-lookup"><span data-stu-id="2af4f-125">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="2af4f-126">Modułów, programów obsługi, *Global.asax.cs*, *Web.config* (z wyjątkiem konfiguracji usług IIS) i znikną cyklu życia aplikacji</span><span class="sxs-lookup"><span data-stu-id="2af4f-126">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="2af4f-127">Role, moduły i programy obsługi zostały przejęte przez oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="2af4f-127">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="2af4f-128">Oprogramowanie pośredniczące są skonfigurowane przy użyciu kodu, a nie w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="2af4f-128">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="2af4f-129">[Rozgałęzienie potoku](../fundamentals/middleware.md#middleware-run-map-use) umożliwia wysyłanie żądań do określonego oprogramowania pośredniczącego, oparte na nie tylko adres URL, ale także na nagłówków żądań, ciągi zapytań, itp.</span><span class="sxs-lookup"><span data-stu-id="2af4f-129">[Pipeline branching](../fundamentals/middleware.md#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="2af4f-130">**Oprogramowanie pośredniczące są bardzo podobne do modułów:**</span><span class="sxs-lookup"><span data-stu-id="2af4f-130">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="2af4f-131">Wywoływane w zasadzie dla każdego żądania</span><span class="sxs-lookup"><span data-stu-id="2af4f-131">Invoked in principle for every request</span></span>

   * <span data-ttu-id="2af4f-132">Możliwość zwarcia przez żądanie, [nie przekazanie żądania do następnego oprogramowania pośredniczącego](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="2af4f-132">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="2af4f-133">Można tworzyć własne odpowiedzi HTTP</span><span class="sxs-lookup"><span data-stu-id="2af4f-133">Able to create their own HTTP response</span></span>

<span data-ttu-id="2af4f-134">**Oprogramowanie pośredniczące i moduły są przetwarzane w innej kolejności:**</span><span class="sxs-lookup"><span data-stu-id="2af4f-134">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="2af4f-135">Kolejność oprogramowania pośredniczącego opiera się na kolejność, w którym wstawieniu do potoku żądania podczas kolejność modułów opiera się głównie na [cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx) zdarzeń</span><span class="sxs-lookup"><span data-stu-id="2af4f-135">Order of middleware is based on the order in which they are inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="2af4f-136">Kolejność oprogramowania pośredniczącego odpowiedzi jest odwrotnie niż dla żądania, podczas kolejność modułów jest taki sam dla żądań i odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="2af4f-136">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="2af4f-137">Zobacz [tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="2af4f-137">See [Creating a middleware pipeline with IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Oprogramowanie pośredniczące](http-modules/_static/middleware.png)

<span data-ttu-id="2af4f-139">Należy zwrócić uwagę, jak na ilustracji powyżej, oprogramowanie pośredniczące uwierzytelniania short-circuited żądania.</span><span class="sxs-lookup"><span data-stu-id="2af4f-139">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="2af4f-140">Migrowanie kodu modułu do oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="2af4f-140">Migrating module code to middleware</span></span>

<span data-ttu-id="2af4f-141">Istniejący moduł HTTP będzie wyglądać podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="2af4f-141">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="2af4f-142">Jak pokazano w [oprogramowanie pośredniczące](../fundamentals/middleware.md) strony, oprogramowanie pośredniczące platformy ASP.NET Core jest klasa, która przedstawia `Invoke` metody z argumentami `HttpContext` i zwracanie `Task`.</span><span class="sxs-lookup"><span data-stu-id="2af4f-142">As shown in the [Middleware](../fundamentals/middleware.md) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="2af4f-143">Nowego oprogramowania pośredniczącego będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="2af4f-143">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="2af4f-144">Powyższego szablonu oprogramowanie pośredniczące została pobrana z sekcji [zapisywania oprogramowanie pośredniczące](../fundamentals/middleware.md#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="2af4f-144">The above middleware template was taken from the section on [writing middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span></span>

<span data-ttu-id="2af4f-145">*MyMiddlewareExtensions* Klasa pomocy ułatwia konfigurowanie oprogramowania pośredniczącego w Twojej `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="2af4f-145">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="2af4f-146">`UseMyMiddleware` Metody dodaje klasy oprogramowania pośredniczącego do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="2af4f-146">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="2af4f-147">Usług wymaganych przez oprogramowanie pośredniczące uzyskać wprowadzonym w Konstruktorze przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="2af4f-147">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="2af4f-148">Modułu może zakończyć żądania, na przykład jeśli użytkownik nie ma uprawnień:</span><span class="sxs-lookup"><span data-stu-id="2af4f-148">Your module might terminate a request, for example if the user is not authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="2af4f-149">Oprogramowanie pośredniczące obsługuje to nie wywołując `Invoke` na następne oprogramowanie pośredniczące w potoku.</span><span class="sxs-lookup"><span data-stu-id="2af4f-149">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="2af4f-150">Należy pamiętać, że to nie pełni kończy żądanie, ponieważ poprzednie middlewares nadal zostanie wywołany, gdy odpowiedź sprawia, że jego sposób za pośrednictwem potoku.</span><span class="sxs-lookup"><span data-stu-id="2af4f-150">Keep in mind that this does not fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="2af4f-151">Funkcje z modułu w przypadku migracji do nowego oprogramowania pośredniczącego, może się okazać, że kod nie kompilacji, ponieważ `HttpContext` klasy zmienił się znacznie w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2af4f-151">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="2af4f-152">[Później](#migrating-to-the-new-httpcontext), zobaczysz jak przeprowadzić migrację do nowego element HttpContext Core ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2af4f-152">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="2af4f-153">Migrowanie modułu wstawiania do potoku żądania</span><span class="sxs-lookup"><span data-stu-id="2af4f-153">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="2af4f-154">Moduły HTTP zazwyczaj są dodawane do potoku żądania przy użyciu *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="2af4f-154">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="2af4f-155">Konwertuj to przez [Dodawanie nowego oprogramowania pośredniczącego](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) do potoku żądania w Twojej `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="2af4f-155">Convert this by [adding your new middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="2af4f-156">Dokładne miejsce w potoku, gdzie wstawić nowego oprogramowania pośredniczącego zależy od zdarzenia, które go obsługiwane jako modułu (`BeginRequest`, `EndRequest`, itd.) i ich kolejność na liście modułów w *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="2af4f-156">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="2af4f-157">Wcześniej wspomniano, nie bez cyklu życia aplikacji w ASP.NET Core a kolejności, w której odpowiedzi są przetwarzane przez oprogramowanie pośredniczące różni się od kolejności użytej przez moduły.</span><span class="sxs-lookup"><span data-stu-id="2af4f-157">As previously stated, there is no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="2af4f-158">To może należy zdecydować, czy porządkowania trudniejsze.</span><span class="sxs-lookup"><span data-stu-id="2af4f-158">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="2af4f-159">Jeśli kolejność staje się problem, można podzielić modułu na wiele składników oprogramowania pośredniczącego, które może zostać określona niezależnie.</span><span class="sxs-lookup"><span data-stu-id="2af4f-159">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="2af4f-160">Migrowanie kod obsługi oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="2af4f-160">Migrating handler code to middleware</span></span>

<span data-ttu-id="2af4f-161">Program obsługi HTTP wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="2af4f-161">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="2af4f-162">W projekcie platformy ASP.NET Core będzie to przełożyć na oprogramowanie pośredniczące podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="2af4f-162">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="2af4f-163">To oprogramowanie pośredniczące jest bardzo podobny do odpowiadającego modułów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-163">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="2af4f-164">Tylko rzeczywistych różnica jest to, że w tym miejscu jest Brak wywołania `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="2af4f-164">The only real difference is that here there is no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="2af4f-165">Ma to sens, ponieważ procedura obsługi jest na końcu potoku żądania zostanie nie następne oprogramowanie pośredniczące do wywołania.</span><span class="sxs-lookup"><span data-stu-id="2af4f-165">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="2af4f-166">Migrowanie wstawienia potoku żądania obsługi</span><span class="sxs-lookup"><span data-stu-id="2af4f-166">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="2af4f-167">Konfigurowanie programu obsługi HTTP odbywa się *Web.config* i wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="2af4f-167">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="2af4f-168">Można przekonwertować to przez dodanie nowych obsługi oprogramowania pośredniczącego do potoku żądania w Twojej `Startup` klasy, podobnie jak przekonwertować z modułów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-168">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="2af4f-169">Tego podejścia przy rozwiązywaniu problemu jest to, że wszystkie żądania będzie wysyłać do nowego oprogramowania pośredniczącego obsługi.</span><span class="sxs-lookup"><span data-stu-id="2af4f-169">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="2af4f-170">Jednak tylko żądania z danym rozszerzeniem nawiązać oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-170">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="2af4f-171">Która pozwoli uzyskać te same funkcje, jaką miał programu obsługi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2af4f-171">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="2af4f-172">Rozwiązanie polega na Rozgałęzienie potoku żądań dla danego rozszerzenia, przy użyciu `MapWhen` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="2af4f-172">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="2af4f-173">Można to zrobić w tym samym `Configure` metody, gdzie możesz dodać inne oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="2af4f-173">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="2af4f-174">`MapWhen`przyjmuje następujące parametry:</span><span class="sxs-lookup"><span data-stu-id="2af4f-174">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="2af4f-175">Wyrażenie lambda, która przyjmuje `HttpContext` i zwraca `true` w przypadku żądania powinien wyłączenia gałęzi.</span><span class="sxs-lookup"><span data-stu-id="2af4f-175">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="2af4f-176">Oznacza to, że można rozgałęzić żądania nie tylko na podstawie ich rozszerzenia, ale także nagłówków żądań parametrów ciągu zapytania, itp.</span><span class="sxs-lookup"><span data-stu-id="2af4f-176">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="2af4f-177">Wyrażenie lambda, która przyjmuje `IApplicationBuilder` i dodaje całe oprogramowanie pośredniczące dla gałęzi.</span><span class="sxs-lookup"><span data-stu-id="2af4f-177">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="2af4f-178">Oznacza to, że można dodać dodatkowe oprogramowanie pośredniczące do gałęzi przed obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-178">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="2af4f-179">Oprogramowanie pośredniczące dodane do potoku przed gałęzi, zostanie wywołany we wszystkich żądaniach; gałąź zostanie nie mają wpływu na nich.</span><span class="sxs-lookup"><span data-stu-id="2af4f-179">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="2af4f-180">Opcje oprogramowania pośredniczącego przy użyciu wzorca opcje ładowania</span><span class="sxs-lookup"><span data-stu-id="2af4f-180">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="2af4f-181">Niektóre moduły i programy obsługi mają opcje konfiguracji, które są przechowywane w *Web.config*. Jednak w przypadku platformy ASP.NET Core nowy model konfiguracji jest używany zamiast *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="2af4f-181">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="2af4f-182">Nowy [system konfiguracji](xref:fundamentals/configuration/index) umożliwia te opcje, aby rozwiązać ten problem:</span><span class="sxs-lookup"><span data-stu-id="2af4f-182">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="2af4f-183">Bezpośrednio wstrzyknąć opcje w oprogramowaniu pośredniczącym, jak pokazano w [następnej sekcji](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="2af4f-183">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="2af4f-184">Użyj [wzorzec opcje](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="2af4f-184">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1.  <span data-ttu-id="2af4f-185">Tworzenie klasy utrzymującej opcje oprogramowania pośredniczącego, na przykład:</span><span class="sxs-lookup"><span data-stu-id="2af4f-185">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="2af4f-186">Przechowywanie wartości opcji</span><span class="sxs-lookup"><span data-stu-id="2af4f-186">Store the option values</span></span>

    <span data-ttu-id="2af4f-187">System konfiguracji umożliwia przechowywanie wartości opcji dowolnym ma.</span><span class="sxs-lookup"><span data-stu-id="2af4f-187">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="2af4f-188">Jednak najbardziej Lokacje użyj *appsettings.json*, więc przeniesiemy tego podejścia:</span><span class="sxs-lookup"><span data-stu-id="2af4f-188">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="2af4f-189">*MyMiddlewareOptionsSection* Oto nazwę sekcji.</span><span class="sxs-lookup"><span data-stu-id="2af4f-189">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="2af4f-190">Nie musi być taka sama jak nazwa klasy opcje.</span><span class="sxs-lookup"><span data-stu-id="2af4f-190">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="2af4f-191">Kojarzenie wartości opcji z klasy opcji</span><span class="sxs-lookup"><span data-stu-id="2af4f-191">Associate the option values with the options class</span></span>

    <span data-ttu-id="2af4f-192">Wzorzec opcje używa framework iniekcji zależności platformy ASP.NET Core w celu skojarzenia typu opcje (takich jak `MyMiddlewareOptions`) z `MyMiddlewareOptions` obiektu, który ma rzeczywistego opcje.</span><span class="sxs-lookup"><span data-stu-id="2af4f-192">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="2af4f-193">Aktualizacja Twojego `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="2af4f-193">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="2af4f-194">Jeśli używasz *appsettings.json*, dodaj go do konstruktora konfiguracji w `Startup` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="2af4f-194">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="2af4f-195">Skonfiguruj usługę opcje:</span><span class="sxs-lookup"><span data-stu-id="2af4f-195">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="2af4f-196">Skojarz opcji z klasy opcje:</span><span class="sxs-lookup"><span data-stu-id="2af4f-196">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="2af4f-197">Wstaw opcje do Twojej konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-197">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="2af4f-198">Efekt jest podobny do iniekcję opcje do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="2af4f-198">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="2af4f-199">[UseMiddleware](#http-modules-usemiddleware) — metoda rozszerzenia, który dodaje oprogramowaniu pośredniczącym, aby `IApplicationBuilder` zajmuje się iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="2af4f-199">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="2af4f-200">To nie jest ograniczona do `IOptions` obiektów.</span><span class="sxs-lookup"><span data-stu-id="2af4f-200">This is not limited to `IOptions` objects.</span></span> <span data-ttu-id="2af4f-201">Drugi obiekt, który wymaga oprogramowania pośredniczącego mogą zostać dodane w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="2af4f-201">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="2af4f-202">Opcje oprogramowania pośredniczącego za pomocą iniekcji bezpośredniego ładowania</span><span class="sxs-lookup"><span data-stu-id="2af4f-202">Loading middleware options through direct injection</span></span>

<span data-ttu-id="2af4f-203">Wzorzec opcje ma tę zaletę, utworzonych utracić sprzężenia między wartości opcji i ich odbiorców.</span><span class="sxs-lookup"><span data-stu-id="2af4f-203">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="2af4f-204">Po klasy opcji został skojarzony z wartości rzeczywistych opcje, inne klasy mogą korzystać z opcji przez strukturę iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="2af4f-204">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="2af4f-205">Nie istnieje potrzeba do przekazania wokół wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="2af4f-205">There is no need to pass around options values.</span></span>

<span data-ttu-id="2af4f-206">To dzieli jednak jeśli chcesz użyć tego samego oprogramowania pośredniczącego dwa razy, przy użyciu różnych opcji.</span><span class="sxs-lookup"><span data-stu-id="2af4f-206">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="2af4f-207">Na przykład autoryzacji oprogramowanie pośredniczące używane w różnych gałęziach, dzięki czemu różne role.</span><span class="sxs-lookup"><span data-stu-id="2af4f-207">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="2af4f-208">Dwa obiekty różnych opcji nie można skojarzyć z klasy jednej opcji.</span><span class="sxs-lookup"><span data-stu-id="2af4f-208">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="2af4f-209">Rozwiązanie to uzyskać obiekty opcje przy użyciu wartości rzeczywistych opcje w Twojej `Startup` klasy i przekazywać je bezpośrednio na każde wystąpienie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-209">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="2af4f-210">Dodaj drugi klucz do *appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="2af4f-210">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="2af4f-211">Aby dodać drugi zestaw opcji, aby *appsettings.json* plików, użyj nowego klucza w celu jego jednoznacznej identyfikacji:</span><span class="sxs-lookup"><span data-stu-id="2af4f-211">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="2af4f-212">Pobieranie wartości opcji i przekazywanie ich do oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-212">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="2af4f-213">`Use...` — Metoda rozszerzenia (która dodaje oprogramowania pośredniczącego do potoku) to logiczne miejsce do przekazywania wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="2af4f-213">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="2af4f-214">Włącz oprogramowaniu pośredniczącym, aby mieć parametr opcji.</span><span class="sxs-lookup"><span data-stu-id="2af4f-214">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="2af4f-215">Udostępnij przeciążenie metody `Use...` — metoda rozszerzenia (która przyjmuje parametr opcje i przekazuje je do `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="2af4f-215">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="2af4f-216">Gdy `UseMiddleware` jest wywoływana z parametrami przekazuje parametry do Twojej konstruktora oprogramowania pośredniczącego podczas tworzenia wystąpień obiektu oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-216">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="2af4f-217">Należy zwrócić uwagę, jak to opakowuje obiektu opcje w `OptionsWrapper` obiektu.</span><span class="sxs-lookup"><span data-stu-id="2af4f-217">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="2af4f-218">To implementuje `IOptions`, zgodnie z oczekiwaniami konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-218">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="2af4f-219">Migracja do nowego element HttpContext</span><span class="sxs-lookup"><span data-stu-id="2af4f-219">Migrating to the new HttpContext</span></span>

<span data-ttu-id="2af4f-220">Wcześniej przedstawiono który `Invoke` metoda w oprogramowania pośredniczącego przyjmuje parametr typu `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="2af4f-220">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="2af4f-221">`HttpContext`znacznie została zmieniona w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2af4f-221">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="2af4f-222">W tej sekcji przedstawiono sposób tłumaczenia najczęściej używane właściwości [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) do nowego `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="2af4f-222">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="2af4f-223">Element HttpContext</span><span class="sxs-lookup"><span data-stu-id="2af4f-223">HttpContext</span></span>

<span data-ttu-id="2af4f-224">**HttpContext.Items** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-224">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="2af4f-225">**Unikatowy identyfikator (odpowiednika System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="2af4f-225">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="2af4f-226">Zawiera unikatowy identyfikator dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="2af4f-226">Gives you a unique id for each request.</span></span> <span data-ttu-id="2af4f-227">Bardzo przydatny w dziennikach.</span><span class="sxs-lookup"><span data-stu-id="2af4f-227">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="2af4f-228">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="2af4f-228">HttpContext.Request</span></span>

<span data-ttu-id="2af4f-229">**HttpContext.Request.HttpMethod** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-229">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="2af4f-230">**HttpContext.Request.QueryString** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-230">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="2af4f-231">**HttpContext.Request.Url** i **HttpContext.Request.RawUrl** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="2af4f-231">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="2af4f-232">**HttpContext.Request.IsSecureConnection** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-232">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="2af4f-233">**HttpContext.Request.UserHostAddress** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-233">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="2af4f-234">**HttpContext.Request.Cookies** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-234">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="2af4f-235">**HttpContext.Request.RequestContext.RouteData** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-235">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="2af4f-236">**HttpContext.Request.Headers** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-236">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="2af4f-237">**HttpContext.Request.UserAgent** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-237">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="2af4f-238">**HttpContext.Request.UrlReferrer** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-238">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="2af4f-239">**HttpContext.Request.ContentType** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-239">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="2af4f-240">**HttpContext.Request.Form** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-240">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="2af4f-241">Odczytać wartości formularza, tylko wtedy, gdy typ zawartości sub *x--www-form-urlencoded* lub *dane formularza*.</span><span class="sxs-lookup"><span data-stu-id="2af4f-241">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="2af4f-242">**HttpContext.Request.InputStream** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-242">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="2af4f-243">Ten kod należy używać tylko w obsługi typu oprogramowanie pośredniczące, na końcu potoku.</span><span class="sxs-lookup"><span data-stu-id="2af4f-243">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="2af4f-244">Można odczytać pierwotne treści, jak pokazano powyżej tylko raz dla żądania.</span><span class="sxs-lookup"><span data-stu-id="2af4f-244">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="2af4f-245">Oprogramowanie pośredniczące próby odczytu treści po pierwszym odczytu będzie odczytywał element body puste.</span><span class="sxs-lookup"><span data-stu-id="2af4f-245">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="2af4f-246">To nie ma zastosowania do odczytywania formularza, jak pokazano wcześniej, ponieważ która jest wykonywana z buforu.</span><span class="sxs-lookup"><span data-stu-id="2af4f-246">This does not apply to reading a form as shown earlier, because that is done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="2af4f-247">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="2af4f-247">HttpContext.Response</span></span>

<span data-ttu-id="2af4f-248">**HttpContext.Response.Status** i **HttpContext.Response.StatusDescription** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="2af4f-248">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="2af4f-249">**HttpContext.Response.ContentEncoding** i **HttpContext.Response.ContentType** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="2af4f-249">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="2af4f-250">**HttpContext.Response.ContentType** na jego własnej tłumaczy także do:</span><span class="sxs-lookup"><span data-stu-id="2af4f-250">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="2af4f-251">**HttpContext.Response.Output** umożliwia to:</span><span class="sxs-lookup"><span data-stu-id="2af4f-251">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="2af4f-252">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="2af4f-252">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="2af4f-253">Wysyłaniu pliku omówiono [tutaj](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="2af4f-253">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="2af4f-254">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="2af4f-254">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="2af4f-255">Wysyłanie nagłówków odpowiedzi jest złożona faktem, że ustawienie po niczego zostały zapisane w treści odpowiedzi, ich nie będą wysyłane.</span><span class="sxs-lookup"><span data-stu-id="2af4f-255">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="2af4f-256">Rozwiązanie jest ustalenie metody wywołania zwrotnego, która będzie wywoływana po prawej przed zapisaniem na uruchamia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2af4f-256">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="2af4f-257">Najlepiej odbywa się na początku `Invoke` metoda oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2af4f-257">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="2af4f-258">Jest ta metoda wywołania zwrotnego ustawiający nagłówkach odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2af4f-258">It is this callback method that sets your response headers.</span></span>

<span data-ttu-id="2af4f-259">Poniższy kod ustawia metodę wywołania zwrotnego o nazwie `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="2af4f-259">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="2af4f-260">`SetHeaders` Metody wywołania zwrotnego będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="2af4f-260">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="2af4f-261">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="2af4f-261">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="2af4f-262">Pliki cookie są przesyłane do przeglądarki w *Set-Cookie* nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2af4f-262">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="2af4f-263">W związku z tym wysyłanie plików cookie wymaga tego samego wywołania zwrotnego jako używane do wysyłania nagłówki odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="2af4f-263">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="2af4f-264">`SetCookies` Metody wywołania zwrotnego będzie wyglądać podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="2af4f-264">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="2af4f-265">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2af4f-265">Additional Resources</span></span>

* [<span data-ttu-id="2af4f-266">Omówienie moduły HTTP i programów obsługi HTTP</span><span class="sxs-lookup"><span data-stu-id="2af4f-266">HTTP Handlers and HTTP Modules Overview</span></span>](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [<span data-ttu-id="2af4f-267">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="2af4f-267">Configuration</span></span>](xref:fundamentals/configuration/index)

* [<span data-ttu-id="2af4f-268">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2af4f-268">Application Startup</span></span>](../fundamentals/startup.md)

* [<span data-ttu-id="2af4f-269">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="2af4f-269">Middleware</span></span>](../fundamentals/middleware.md)
