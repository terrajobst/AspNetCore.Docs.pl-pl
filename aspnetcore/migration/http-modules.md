---
title: Migrowanie moduły i programy obsługi HTTP do oprogramowania pośredniczącego platformy ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 9dd28b86966912cce87166feb37e65adf3dd6dcb
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902674"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="a1a58-102">Migrowanie moduły i programy obsługi HTTP do oprogramowania pośredniczącego platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1a58-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="a1a58-103">Przez [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="a1a58-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="a1a58-104">W tym artykule pokazano, jak przeprowadzić migrację istniejących ASP.NET [moduły HTTP do programów obsługi z system.webserver](/iis/configuration/system.webserver/) do platformy ASP.NET Core [oprogramowania pośredniczącego](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="a1a58-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="a1a58-105">Moduły i programy obsługi poprawiony</span><span class="sxs-lookup"><span data-stu-id="a1a58-105">Modules and handlers revisited</span></span>

<span data-ttu-id="a1a58-106">Przed przejściem do oprogramowania pośredniczącego platformy ASP.NET Core, najpierw przypomnijmy jak działają moduły HTTP do programów obsługi:</span><span class="sxs-lookup"><span data-stu-id="a1a58-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Program obsługi modułów](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="a1a58-108">**Programy obsługi są:**</span><span class="sxs-lookup"><span data-stu-id="a1a58-108">**Handlers are:**</span></span>

   * <span data-ttu-id="a1a58-109">Klasy, które implementują [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="a1a58-109">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="a1a58-110">Używane do obsługi żądań przy użyciu określonej nazwy pliku lub rozszerzenie, takich jak *.report*</span><span class="sxs-lookup"><span data-stu-id="a1a58-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="a1a58-111">[Skonfigurowane](/iis/configuration/system.webserver/handlers/) w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="a1a58-111">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="a1a58-112">**Dostępne są następujące moduły:**</span><span class="sxs-lookup"><span data-stu-id="a1a58-112">**Modules are:**</span></span>

   * <span data-ttu-id="a1a58-113">Klasy, które implementują [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="a1a58-113">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="a1a58-114">Wywoływane dla każdego żądania</span><span class="sxs-lookup"><span data-stu-id="a1a58-114">Invoked for every request</span></span>

   * <span data-ttu-id="a1a58-115">Możliwość zwarcie (zatrzymać dalsze przetwarzanie żądania)</span><span class="sxs-lookup"><span data-stu-id="a1a58-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="a1a58-116">Możliwość dodania do odpowiedzi HTTP, lub Utwórz własne</span><span class="sxs-lookup"><span data-stu-id="a1a58-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="a1a58-117">[Skonfigurowane](/iis/configuration/system.webserver/modules/) w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="a1a58-117">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="a1a58-118">**Kolejność, w którym modułów przetworzyć żądań przychodzących jest określana przez:**</span><span class="sxs-lookup"><span data-stu-id="a1a58-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="a1a58-119">[Cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx), czyli zdarzenia serii, wywoływane przez platformę ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)itp. Każdy moduł można utworzyć procedury obsługi dla jednego lub wielu zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="a1a58-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="a1a58-120">Zdarzenie, dla którego, kolejność, w którym są one konfigurowane w *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="a1a58-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="a1a58-121">Oprócz modułów, można dodać procedury obsługi zdarzeń cyklu życia do Twojej *Global.asax.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="a1a58-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="a1a58-122">Te procedury obsługi uruchomić po obsługi w modułach skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="a1a58-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="a1a58-123">Programy obsługi i moduły do oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="a1a58-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="a1a58-124">**Oprogramowanie pośredniczące są prostsze niż moduły HTTP do programów obsługi:**</span><span class="sxs-lookup"><span data-stu-id="a1a58-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="a1a58-125">Moduły, programy obsługi, *Global.asax.cs*, *Web.config* (z wyjątkiem konfiguracji usług IIS) i znikną cyklu życia aplikacji</span><span class="sxs-lookup"><span data-stu-id="a1a58-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="a1a58-126">Role, moduły i programy obsługi zostały przejęte przez oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="a1a58-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="a1a58-127">Oprogramowanie pośredniczące są skonfigurowane przy użyciu kodu, a nie w *pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="a1a58-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="a1a58-128">[Rozgałęzianie w potoku](xref:fundamentals/middleware/index#use-run-and-map) pozwala wysyłać żądania do określonej, opartego na oprogramowaniu pośredniczącym nie tylko adres URL, ale także dla nagłówków żądań, ciągi zapytań, itp.</span><span class="sxs-lookup"><span data-stu-id="a1a58-128">[Pipeline branching](xref:fundamentals/middleware/index#use-run-and-map) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="a1a58-129">**Oprogramowanie pośredniczące są bardzo podobne do modułów:**</span><span class="sxs-lookup"><span data-stu-id="a1a58-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="a1a58-130">Wywoływane w zasadzie dla każdego żądania</span><span class="sxs-lookup"><span data-stu-id="a1a58-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="a1a58-131">Możliwość zwarcie żądanie, według [nie przekazaniem żądania do następnego oprogramowania pośredniczącego](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="a1a58-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="a1a58-132">Możliwość tworzenia własnych odpowiedzi HTTP</span><span class="sxs-lookup"><span data-stu-id="a1a58-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="a1a58-133">**Oprogramowanie pośredniczące i moduły są przetwarzane w innej kolejności:**</span><span class="sxs-lookup"><span data-stu-id="a1a58-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="a1a58-134">Kolejność oprogramowania pośredniczącego zależy od kolejności, w którym wstawiane są one potokiem żądań, gdy zamówienie modułów opiera się przede wszystkim na [cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx) zdarzenia</span><span class="sxs-lookup"><span data-stu-id="a1a58-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="a1a58-135">Kolejność oprogramowania pośredniczącego w odpowiedzi jest odwrotnie niż to, w przypadku żądań, a kolejność modułów jest taka sama dla żądań i odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="a1a58-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="a1a58-136">Zobacz [tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="a1a58-136">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Oprogramowanie pośredniczące](http-modules/_static/middleware.png)

<span data-ttu-id="a1a58-138">Należy pamiętać o tym, jak na ilustracji powyżej oprogramowania pośredniczącego uwierzytelniania short-circuited żądania.</span><span class="sxs-lookup"><span data-stu-id="a1a58-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="a1a58-139">Migrowanie kodu modułu do oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="a1a58-139">Migrating module code to middleware</span></span>

<span data-ttu-id="a1a58-140">Istniejący moduł HTTP będzie wyglądać podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="a1a58-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="a1a58-141">Jak pokazano na [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) stronie pośredniczące platformy ASP.NET Core to klasa, która udostępnia `Invoke` wykonywanie metody `HttpContext` i zwracanie `Task`.</span><span class="sxs-lookup"><span data-stu-id="a1a58-141">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="a1a58-142">Nowego oprogramowania pośredniczącego będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="a1a58-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="a1a58-143">Powyższy szablon oprogramowania pośredniczącego została pobrana z sekcji [pisanie oprogramowania pośredniczącego](xref:fundamentals/middleware/index#write-middleware).</span><span class="sxs-lookup"><span data-stu-id="a1a58-143">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/index#write-middleware).</span></span>

<span data-ttu-id="a1a58-144">*MyMiddlewareExtensions* klasy pomocnika ułatwia konfigurowanie oprogramowania pośredniczącego w swojej `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="a1a58-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="a1a58-145">`UseMyMiddleware` Metoda dodaje klasa oprogramowania pośredniczącego do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="a1a58-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="a1a58-146">Usługi wymagane przez oprogramowanie pośredniczące Pobierz wprowadzony w Konstruktorze oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="a1a58-147">Modułu może przerwać żądanie, na przykład jeśli użytkownik nie ma uprawnień:</span><span class="sxs-lookup"><span data-stu-id="a1a58-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="a1a58-148">Oprogramowanie pośredniczące wykonuje tę nie wywołując `Invoke` na następne oprogramowanie pośredniczące w potoku.</span><span class="sxs-lookup"><span data-stu-id="a1a58-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="a1a58-149">Zachowaj należy pamiętać, że to nie pełni zakończyć żądania, ponieważ poprzednie middlewares nadal zostanie wywołany, gdy odpowiedź sprawia, że jego sposób za pośrednictwem potoku.</span><span class="sxs-lookup"><span data-stu-id="a1a58-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="a1a58-150">Podczas migracji funkcji Twojego modułu do nowego oprogramowania pośredniczącego, może się okazać, że Twój kod nie kompilacji, ponieważ `HttpContext` klasy zmienił się znacznie w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1a58-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="a1a58-151">[Później](#migrating-to-the-new-httpcontext), pokazano, jak przeprowadzić migrację do nowego obiektu ASP.NET Core HttpContext.</span><span class="sxs-lookup"><span data-stu-id="a1a58-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="a1a58-152">Migrowanie wstawienia modułu Potok żądań</span><span class="sxs-lookup"><span data-stu-id="a1a58-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="a1a58-153">Moduły HTTP zazwyczaj są dodawane do żądania potoku przy użyciu *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="a1a58-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="a1a58-154">Skonwertuje ją przez [dodanie nowego oprogramowania pośredniczącego](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) do potoku żądania w swojej `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="a1a58-154">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="a1a58-155">Dokładnie miejscu w potoku, w którym wstawisz nowego oprogramowania pośredniczącego zależy od zdarzenia, które system zrobił to automatycznie jako moduł (`BeginRequest`, `EndRequest`, itp.) i ich kolejność na liście modułów w *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="a1a58-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="a1a58-156">Jak wcześniej wspomniano, istnieje nie cyklu życia aplikacji w programie ASP.NET Core i kolejności przetwarzania odpowiedzi przez oprogramowanie pośredniczące różni się od kolejności, używany przez moduły.</span><span class="sxs-lookup"><span data-stu-id="a1a58-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="a1a58-157">To może spowodować, że szeregowania decyzji trudniejsze.</span><span class="sxs-lookup"><span data-stu-id="a1a58-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="a1a58-158">Jeśli kolejność staje się problem, można podzielić modułu wielu składników oprogramowania pośredniczącego, które może zostać określona niezależnie.</span><span class="sxs-lookup"><span data-stu-id="a1a58-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="a1a58-159">Migrowanie kodu programu obsługi do oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="a1a58-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="a1a58-160">Program obsługi HTTP, wygląda mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="a1a58-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="a1a58-161">W projekcie platformy ASP.NET Core będzie to przełożyć na oprogramowanie pośredniczące podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="a1a58-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="a1a58-162">To oprogramowanie pośredniczące jest bardzo podobny do oprogramowania pośredniczącego odpowiadający modułów.</span><span class="sxs-lookup"><span data-stu-id="a1a58-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="a1a58-163">Tylko rzeczywistego różnicą jest to, że w tym miejscu nie ma żadnych wywołanie `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="a1a58-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="a1a58-164">Ma to sens, ponieważ program obsługi ma pod koniec potokiem żądań, zostanie nie następne oprogramowanie pośredniczące do wywołania.</span><span class="sxs-lookup"><span data-stu-id="a1a58-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="a1a58-165">Migrowanie wstawienia Potok żądań obsługi</span><span class="sxs-lookup"><span data-stu-id="a1a58-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="a1a58-166">Konfigurowanie programu obsługi HTTP odbywa się w *Web.config* i wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="a1a58-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="a1a58-167">Można to skonwertować przez dodanie nowej procedury obsługi oprogramowania pośredniczącego do potoku żądania w swojej `Startup` klasy, podobnie jak przekonwertować z modułów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="a1a58-168">Problem za pomocą tego podejścia polega na tym, że prześle wszystkich żądań do nowego programu obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="a1a58-169">Jednak mają tylko z danym rozszerzeniem dotarcie żądania do oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="a1a58-170">Który pozwoli uzyskać te same funkcje, które miał za pomocą programu obsługi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a1a58-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="a1a58-171">Rozwiązanie polega na gałąź potoku dla żądań z danego rozszerzenia za pomocą `MapWhen` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a1a58-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="a1a58-172">Można to zrobić w taki sam `Configure` metody, w którym dodajesz innym oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="a1a58-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="a1a58-173">`MapWhen` przyjmuje następujące parametry:</span><span class="sxs-lookup"><span data-stu-id="a1a58-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="a1a58-174">Lambda, która pobiera `HttpContext` i zwraca `true` Jeśli żądanie powinno przestaną działać gałęzi.</span><span class="sxs-lookup"><span data-stu-id="a1a58-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="a1a58-175">Oznacza to, że można rozgałęziać żądań nie tylko na podstawie ich rozszerzeń, ale również na nagłówki żądania, parametry ciągu zapytania itp.</span><span class="sxs-lookup"><span data-stu-id="a1a58-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="a1a58-176">Lambda, która pobiera `IApplicationBuilder` i dodaje całe oprogramowanie pośredniczące dla gałęzi.</span><span class="sxs-lookup"><span data-stu-id="a1a58-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="a1a58-177">Oznacza to, że można dodać dodatkowe oprogramowanie pośredniczące do gałęzi przed obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="a1a58-178">Oprogramowanie pośredniczące dodawane do potoku przed gałęzi, zostanie wywołana na wszystkie żądania; gałąź będzie mieć żadnego wpływu na nich.</span><span class="sxs-lookup"><span data-stu-id="a1a58-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="a1a58-179">Opcje oprogramowania pośredniczącego przy użyciu wzorca opcje ładowania</span><span class="sxs-lookup"><span data-stu-id="a1a58-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="a1a58-180">Niektóre moduły i programy obsługi ma opcji konfiguracji, które są przechowywane w *Web.config*. Jednak w programie ASP.NET Core nowy model konfiguracji jest używany zamiast *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="a1a58-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="a1a58-181">Nowy [system konfiguracji](xref:fundamentals/configuration/index) zapewnia tych opcji, aby rozwiązać ten problem:</span><span class="sxs-lookup"><span data-stu-id="a1a58-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="a1a58-182">Opcje w oprogramowaniu pośredniczącym, wstrzyknąć bezpośrednio, jak pokazano w [następnej sekcji](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="a1a58-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="a1a58-183">Użyj [wzorzec opcje](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="a1a58-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="a1a58-184">Tworzenie klasy utrzymującej opcje oprogramowania pośredniczącego, na przykład:</span><span class="sxs-lookup"><span data-stu-id="a1a58-184">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="a1a58-185">Store wartości opcji</span><span class="sxs-lookup"><span data-stu-id="a1a58-185">Store the option values</span></span>

   <span data-ttu-id="a1a58-186">System konfiguracji służy do przechowywania wartości opcji dowolnym ma.</span><span class="sxs-lookup"><span data-stu-id="a1a58-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="a1a58-187">Jednak większość witryn użyj *appsettings.json*, więc przeniesiemy tego podejścia:</span><span class="sxs-lookup"><span data-stu-id="a1a58-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="a1a58-188">*MyMiddlewareOptionsSection* Oto nazwę sekcji.</span><span class="sxs-lookup"><span data-stu-id="a1a58-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="a1a58-189">Nie musi być taka sama jak nazwa klasy opcji.</span><span class="sxs-lookup"><span data-stu-id="a1a58-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="a1a58-190">Kojarzenie wartości opcji z klasy opcji</span><span class="sxs-lookup"><span data-stu-id="a1a58-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="a1a58-191">Wzorzec opcje używa framework iniekcji zależności platformy ASP.NET Core w celu skojarzenia typu opcji (takich jak `MyMiddlewareOptions`) przy użyciu `MyMiddlewareOptions` obiekt, który zapewnia dostęp do rzeczywistego opcji.</span><span class="sxs-lookup"><span data-stu-id="a1a58-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="a1a58-192">Aktualizacja usługi `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="a1a58-192">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="a1a58-193">Jeśli używasz *appsettings.json*, dodaj go do konstruktora konfiguracji w `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="a1a58-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="a1a58-194">Skonfiguruj usługę opcje:</span><span class="sxs-lookup"><span data-stu-id="a1a58-194">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="a1a58-195">Skojarz opcje z klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="a1a58-195">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="a1a58-196">Wstrzyknąć opcje do konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="a1a58-197">Jest to podobne do wprowadzanie opcji w kontroler.</span><span class="sxs-lookup"><span data-stu-id="a1a58-197">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="a1a58-198">[UseMiddleware](#http-modules-usemiddleware) metodę rozszerzenia, który dodaje oprogramowaniu pośredniczącym, aby `IApplicationBuilder` dba o iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="a1a58-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="a1a58-199">To nie jest ograniczona do `IOptions` obiektów.</span><span class="sxs-lookup"><span data-stu-id="a1a58-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="a1a58-200">Inny obiekt, który wymaga oprogramowania pośredniczącego może wprowadzony w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="a1a58-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="a1a58-201">Opcje oprogramowania pośredniczącego przy użyciu iniekcji bezpośrednie ładowanie</span><span class="sxs-lookup"><span data-stu-id="a1a58-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="a1a58-202">Wzorzec opcje ma tę zaletę, że tworzy luźne, sprzężenia między wartościami opcje i ich odbiorców.</span><span class="sxs-lookup"><span data-stu-id="a1a58-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="a1a58-203">Gdy klasa opcji został skojarzony z wartości rzeczywistej opcje, inne klasy mogą uzyskać dostęp do opcji przez strukturę iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="a1a58-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="a1a58-204">Nie ma potrzeby w celu przejścia wokół wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="a1a58-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="a1a58-205">To dzieli jednak jeśli chcesz użyć tego samego oprogramowania pośredniczącego dwa razy, przy użyciu różnych opcji.</span><span class="sxs-lookup"><span data-stu-id="a1a58-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="a1a58-206">Na przykład autoryzacji oprogramowanie pośredniczące używane w różnych oddziałach, umożliwiając różne role.</span><span class="sxs-lookup"><span data-stu-id="a1a58-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="a1a58-207">Dwa obiekty różnych opcji nie można skojarzyć z klasy jednej opcji.</span><span class="sxs-lookup"><span data-stu-id="a1a58-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="a1a58-208">Rozwiązanie jest uzyskanie obiektów opcje przy użyciu wartości z rzeczywistych opcje usługi `Startup` klasy i przekazać te bezpośrednio do poszczególnych wystąpień tego oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="a1a58-209">Dodaj drugi klucz, który *appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="a1a58-209">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="a1a58-210">Aby dodać drugi zestaw opcji, aby *appsettings.json* pliku, użyj nowego klucza umożliwiającą jej jednoznaczną identyfikację:</span><span class="sxs-lookup"><span data-stu-id="a1a58-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="a1a58-211">Pobieranie wartości opcji i przekazywać je do oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="a1a58-212">`Use...` Metody rozszerzenia, (które dodaje oprogramowania pośredniczącego do potoku) jest miejscem logiczne do przekazywania wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="a1a58-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="a1a58-213">Włącza oprogramowanie pośredniczące przyjmować parametr opcji.</span><span class="sxs-lookup"><span data-stu-id="a1a58-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="a1a58-214">Podaj przeciążenia `Use...` — metoda rozszerzenia (który przyjmuje parametr opcji i przekazuje go do `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="a1a58-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="a1a58-215">Gdy `UseMiddleware` jest wywoływana z parametrami, przekazuje parametry do konstruktora oprogramowania pośredniczącego podczas tworzenia wystąpienia obiektu oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="a1a58-216">Należy zauważyć, jak to otacza obiekt opcje w `OptionsWrapper` obiektu.</span><span class="sxs-lookup"><span data-stu-id="a1a58-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="a1a58-217">To implementuje `IOptions`, zgodnie z oczekiwaniami, konstruktora oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="a1a58-218">Migracja do nowego obiektu HttpContext.</span><span class="sxs-lookup"><span data-stu-id="a1a58-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="a1a58-219">Wcześniej, `Invoke` metoda w oprogramowania pośredniczącego przyjmuje parametr typu `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="a1a58-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="a1a58-220">`HttpContext` znacznie zmienił się w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1a58-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="a1a58-221">W tej sekcji pokazano, jak tłumaczenie najczęściej używanych właściwości [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) do nowego `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a1a58-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="a1a58-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="a1a58-222">HttpContext</span></span>

<span data-ttu-id="a1a58-223">**HttpContext.Items** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="a1a58-224">**Unikatowy identyfikator (odpowiednika System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="a1a58-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="a1a58-225">Zawiera unikatowy identyfikator dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="a1a58-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="a1a58-226">Bardzo przydatne do uwzględnienia w dziennikach.</span><span class="sxs-lookup"><span data-stu-id="a1a58-226">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="a1a58-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="a1a58-227">HttpContext.Request</span></span>

<span data-ttu-id="a1a58-228">**HttpContext.Request.HttpMethod** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="a1a58-229">**HttpContext.Request.QueryString** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="a1a58-230">**HttpContext.Request.Url** i **HttpContext.Request.RawUrl** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="a1a58-231">**HttpContext.Request.IsSecureConnection** translates to:</span><span class="sxs-lookup"><span data-stu-id="a1a58-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="a1a58-232">**HttpContext.Request.UserHostAddress** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="a1a58-233">**HttpContext.Request.Cookies** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="a1a58-234">**HttpContext.Request.RequestContext.RouteData** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="a1a58-235">**HttpContext.Request.Headers** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="a1a58-236">**HttpContext.Request.UserAgent** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="a1a58-237">**HttpContext.Request.UrlReferrer** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="a1a58-238">**HttpContext.Request.ContentType** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="a1a58-239">**HttpContext.Request.Form** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="a1a58-240">Odczyt wartości formularza, tylko wtedy, gdy zawartość podtyp *x--www-form-urlencoded* lub *dane formularza*.</span><span class="sxs-lookup"><span data-stu-id="a1a58-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="a1a58-241">**HttpContext.Request.InputStream** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="a1a58-242">Użyj tego kodu tylko w przypadku obsługi typu oprogramowanie pośredniczące, na końcu potoku.</span><span class="sxs-lookup"><span data-stu-id="a1a58-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="a1a58-243">Może odczytywać nieprzetworzonej treści, jak pokazano powyżej jednokrotnie dla danego żądania.</span><span class="sxs-lookup"><span data-stu-id="a1a58-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="a1a58-244">Oprogramowanie pośredniczące próby odczytu treści po pierwszym odczytu będzie odczytywał element body puste.</span><span class="sxs-lookup"><span data-stu-id="a1a58-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="a1a58-245">To nie ma zastosowania do odczytu formularza, jak pokazano wcześniej, ponieważ zostanie to zrobione buforu.</span><span class="sxs-lookup"><span data-stu-id="a1a58-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="a1a58-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="a1a58-246">HttpContext.Response</span></span>

<span data-ttu-id="a1a58-247">**HttpContext.Response.Status** i **HttpContext.Response.StatusDescription** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="a1a58-248">**HttpContext.Response.ContentEncoding** i **HttpContext.Response.ContentType** przełożyć na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="a1a58-249">**HttpContext.Response.ContentType** na jego własnej również przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="a1a58-250">**HttpContext.Response.Output** przekłada się na:</span><span class="sxs-lookup"><span data-stu-id="a1a58-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="a1a58-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="a1a58-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="a1a58-252">Wysyłaniu pliku omówiono [tutaj](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="a1a58-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="a1a58-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="a1a58-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="a1a58-254">Wysyłanie nagłówków odpowiedzi jest skomplikowane faktem, że jeśli ustawisz je po nic został zapisany treść odpowiedzi, mogą nie będą wysyłane.</span><span class="sxs-lookup"><span data-stu-id="a1a58-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="a1a58-255">Rozwiązanie jest można ustawić metodę wywołania zwrotnego, która zostanie wywołana po prawej stronie przed zapisaniem uruchamia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a1a58-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="a1a58-256">Najlepiej odbywa się na początku `Invoke` metody w oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a1a58-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="a1a58-257">Jest to metoda wywołania zwrotnego, określająca swoje nagłówki odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a1a58-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="a1a58-258">Poniższy kod ustawia metodę wywołania zwrotnego wywoływana `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="a1a58-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="a1a58-259">`SetHeaders` Metody wywołania zwrotnego będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="a1a58-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="a1a58-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="a1a58-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="a1a58-261">Pliki cookie są przesyłane do przeglądarki w *Set-Cookie* nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a1a58-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="a1a58-262">W rezultacie wysyłanie plików cookie wymaga tego samego wywołania zwrotnego, które jest używane do wysyłania nagłówków odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="a1a58-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="a1a58-263">`SetCookies` Metody wywołania zwrotnego będzie wyglądać podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="a1a58-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="a1a58-264">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a1a58-264">Additional resources</span></span>

* [<span data-ttu-id="a1a58-265">Funkcje obsługi protokołu HTTP i moduły HTTP danych — omówienie</span><span class="sxs-lookup"><span data-stu-id="a1a58-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="a1a58-266">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="a1a58-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="a1a58-267">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="a1a58-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="a1a58-268">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="a1a58-268">Middleware</span></span>](xref:fundamentals/middleware/index)
