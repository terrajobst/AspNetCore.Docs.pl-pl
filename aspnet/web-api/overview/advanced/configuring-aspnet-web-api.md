---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "Konfigurowanie składnika ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="ba31a-102">Konfigurowanie składnika ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="ba31a-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="ba31a-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ba31a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ba31a-104">W tym temacie opisano sposób konfigurowania interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ba31a-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="ba31a-105">Ustawienia konfiguracji</span><span class="sxs-lookup"><span data-stu-id="ba31a-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="ba31a-106">Konfigurowanie witryny sieci Web interfejsu API z hostingu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ba31a-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="ba31a-107">Konfigurowanie witryny sieci Web interfejsu API z własnym hostingu OWIN</span><span class="sxs-lookup"><span data-stu-id="ba31a-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="ba31a-108">Usługi interfejsu API sieci Web globalne</span><span class="sxs-lookup"><span data-stu-id="ba31a-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="ba31a-109">Konfiguracja dla kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="ba31a-110">Ustawienia konfiguracji</span><span class="sxs-lookup"><span data-stu-id="ba31a-110">Configuration Settings</span></span>

<span data-ttu-id="ba31a-111">Ustawienia konfiguracji interfejsu API sieci Web są zdefiniowane w [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) klasy.</span><span class="sxs-lookup"><span data-stu-id="ba31a-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="ba31a-112">Element członkowski</span><span class="sxs-lookup"><span data-stu-id="ba31a-112">Member</span></span> | <span data-ttu-id="ba31a-113">Opis</span><span class="sxs-lookup"><span data-stu-id="ba31a-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ba31a-114">**Klasy DependencyResolver**</span><span class="sxs-lookup"><span data-stu-id="ba31a-114">**DependencyResolver**</span></span> | <span data-ttu-id="ba31a-115">Umożliwia iniekcji zależności dla kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="ba31a-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="ba31a-116">Zobacz [za pomocą mechanizmu rozpoznawania zależności interfejsu API sieci Web](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="ba31a-117">**Filtry**</span><span class="sxs-lookup"><span data-stu-id="ba31a-117">**Filters**</span></span> | <span data-ttu-id="ba31a-118">Filtry akcji.</span><span class="sxs-lookup"><span data-stu-id="ba31a-118">Action filters.</span></span> |
| <span data-ttu-id="ba31a-119">**Elementy formatujące**</span><span class="sxs-lookup"><span data-stu-id="ba31a-119">**Formatters**</span></span> | <span data-ttu-id="ba31a-120">[Programy formatujące typy nośnika](../formats-and-model-binding/media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="ba31a-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="ba31a-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="ba31a-122">Określa, czy serwer ma uwzględniać szczegóły błędu, takie jak komunikaty wyjątku i ślady stosu, w komunikatów odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="ba31a-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="ba31a-123">Zobacz [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span><span class="sxs-lookup"><span data-stu-id="ba31a-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="ba31a-124">**Inicjator**</span><span class="sxs-lookup"><span data-stu-id="ba31a-124">**Initializer**</span></span> | <span data-ttu-id="ba31a-125">Funkcja, która wykonuje końcowego inicjowania elementu **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ba31a-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="ba31a-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="ba31a-126">**MessageHandlers**</span></span> | <span data-ttu-id="ba31a-127">[Programy obsługi komunikatów HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="ba31a-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="ba31a-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="ba31a-129">Kolekcja reguł dla wiązania parametrów na akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="ba31a-130">**Właściwości**</span><span class="sxs-lookup"><span data-stu-id="ba31a-130">**Properties**</span></span> | <span data-ttu-id="ba31a-131">Zbiór właściwości ogólnych.</span><span class="sxs-lookup"><span data-stu-id="ba31a-131">A generic property bag.</span></span> |
| <span data-ttu-id="ba31a-132">**Trasy**</span><span class="sxs-lookup"><span data-stu-id="ba31a-132">**Routes**</span></span> | <span data-ttu-id="ba31a-133">Kolekcja tras.</span><span class="sxs-lookup"><span data-stu-id="ba31a-133">The collection of routes.</span></span> <span data-ttu-id="ba31a-134">Zobacz [routingu w składniku ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="ba31a-135">**Usługi**</span><span class="sxs-lookup"><span data-stu-id="ba31a-135">**Services**</span></span> | <span data-ttu-id="ba31a-136">Kolekcja usług.</span><span class="sxs-lookup"><span data-stu-id="ba31a-136">The collection of services.</span></span> <span data-ttu-id="ba31a-137">Zobacz [usług](#services).</span><span class="sxs-lookup"><span data-stu-id="ba31a-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="ba31a-138">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ba31a-138">Prerequisites</span></span>

<span data-ttu-id="ba31a-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional lub Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="ba31a-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="ba31a-140">Konfigurowanie witryny sieci Web interfejsu API z hostingu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ba31a-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="ba31a-141">W aplikacji ASP.NET, należy skonfigurować interfejs API sieci Web przez wywołanie metody [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) w **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="ba31a-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="ba31a-142">**Konfiguruj** metoda przyjmuje delegata z pojedynczym parametrem typu **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ba31a-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="ba31a-143">Wykonaj wszystkie Twoje konfiguracyjny wewnątrz obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="ba31a-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="ba31a-144">Oto przykład przy użyciu anonimowego delegata:</span><span class="sxs-lookup"><span data-stu-id="ba31a-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="ba31a-145">W programie Visual Studio 2017 r, szablon projektu "Aplikacja sieci Web ASP.NET" automatycznie konfiguruje kod konfiguracji po wybraniu "Interfejsu API sieci Web" w **nowy projekt ASP.NET** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="ba31a-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="ba31a-146">Szablon projektu tworzy plik o nazwie WebApiConfig.cs wewnątrz aplikacji\_folder początkowy.</span><span class="sxs-lookup"><span data-stu-id="ba31a-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="ba31a-147">Ten plik kodu definiuje delegata, w którym należy umieścić kodu konfiguracji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ba31a-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="ba31a-148">Szablon projektu również dodaje kod, który wywołuje delegata z **aplikacji\_Start**.</span><span class="sxs-lookup"><span data-stu-id="ba31a-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="ba31a-149">Konfigurowanie witryny sieci Web interfejsu API z własnym hostingu OWIN</span><span class="sxs-lookup"><span data-stu-id="ba31a-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="ba31a-150">Jeśli jesteś własnym hostingu z oprogramowaniem OWIN, Utwórz nową **HttpConfiguration** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ba31a-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="ba31a-151">Wykonywać żadnej konfiguracji dla tego wystąpienia, a następnie przekazać wystąpienia **Owin.UseWebApi** — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="ba31a-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="ba31a-152">Samouczek [OWIN Użyj Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) zawiera wszystkie czynności.</span><span class="sxs-lookup"><span data-stu-id="ba31a-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="ba31a-153">Usługi interfejsu API sieci Web globalne</span><span class="sxs-lookup"><span data-stu-id="ba31a-153">Global Web API Services</span></span>

<span data-ttu-id="ba31a-154">**HttpConfiguration.Services** kolekcja zawiera zbiór usług globalnych, które używa interfejsu API sieci Web do wykonywania różnych zadań, takich jak kontroler wybór i negocjującej zawartość.</span><span class="sxs-lookup"><span data-stu-id="ba31a-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="ba31a-155">**Usług** kolekcji nie jest mechanizm ogólnego przeznaczenia iniekcji odnajdowania lub zależność usługi.</span><span class="sxs-lookup"><span data-stu-id="ba31a-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="ba31a-156">Przechowywane są tylko typy usług, które są znane w ramach interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ba31a-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="ba31a-157">**Usług** kolekcja jest zainicjowana za pomocą domyślnego zestawu usług i możesz podać własne implementacji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="ba31a-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="ba31a-158">Niektóre usługi obsługuje wiele wystąpień, podczas gdy inne osoby mogą mieć tylko jedno wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="ba31a-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="ba31a-159">(Jednak można też podać usługi na poziomie kontrolera; zobacz [konfiguracji poszczególnych kontrolerów](#percontrollerconfig).</span><span class="sxs-lookup"><span data-stu-id="ba31a-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="ba31a-160">Usługi w jednym wystąpieniu</span><span class="sxs-lookup"><span data-stu-id="ba31a-160">Single-Instance Services</span></span>


| <span data-ttu-id="ba31a-161">Usługa</span><span class="sxs-lookup"><span data-stu-id="ba31a-161">Service</span></span> | <span data-ttu-id="ba31a-162">Opis</span><span class="sxs-lookup"><span data-stu-id="ba31a-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ba31a-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="ba31a-163">**IActionValueBinder**</span></span> | <span data-ttu-id="ba31a-164">Pobiera wiązanie parametru.</span><span class="sxs-lookup"><span data-stu-id="ba31a-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="ba31a-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="ba31a-165">**IApiExplorer**</span></span> | <span data-ttu-id="ba31a-166">Pobiera opisy interfejsów API udostępniany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="ba31a-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="ba31a-167">Zobacz [tworzenia strony pomocy dla interfejsu API sieci Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="ba31a-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="ba31a-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="ba31a-169">Pobiera listę zestawów dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ba31a-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="ba31a-170">Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="ba31a-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="ba31a-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="ba31a-172">Weryfikuje model, który będzie odczytywać treść żądania przez element formatujący typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="ba31a-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="ba31a-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="ba31a-173">**IContentNegotiator**</span></span> | <span data-ttu-id="ba31a-174">Przeprowadza negocjowanie zawartości.</span><span class="sxs-lookup"><span data-stu-id="ba31a-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="ba31a-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="ba31a-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="ba31a-176">Zawiera dokumentacja interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="ba31a-176">Provides documentation for APIs.</span></span> <span data-ttu-id="ba31a-177">Wartość domyślna to **null**.</span><span class="sxs-lookup"><span data-stu-id="ba31a-177">The default is **null**.</span></span> <span data-ttu-id="ba31a-178">Zobacz [tworzenia strony pomocy dla interfejsu API sieci Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="ba31a-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="ba31a-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="ba31a-180">Wskazuje, czy host powinien buforować treść jednostki wiadomości HTTP.</span><span class="sxs-lookup"><span data-stu-id="ba31a-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="ba31a-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="ba31a-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="ba31a-182">Wywołuje akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-182">Invokes a controller action.</span></span> <span data-ttu-id="ba31a-183">Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="ba31a-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="ba31a-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="ba31a-185">Wybiera akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-185">Selects a controller action.</span></span> <span data-ttu-id="ba31a-186">Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="ba31a-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="ba31a-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="ba31a-188">Aktywuje kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-188">Activates a controller.</span></span> <span data-ttu-id="ba31a-189">Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="ba31a-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="ba31a-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="ba31a-191">Wybiera kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-191">Selects a controller.</span></span> <span data-ttu-id="ba31a-192">Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="ba31a-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="ba31a-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="ba31a-194">Zawiera listę typów kontrolera interfejsu API sieci Web w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ba31a-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="ba31a-195">Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="ba31a-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="ba31a-196">**ITraceManager**</span></span> | <span data-ttu-id="ba31a-197">Inicjuje struktury śledzenia.</span><span class="sxs-lookup"><span data-stu-id="ba31a-197">Initializes the tracing framework.</span></span> <span data-ttu-id="ba31a-198">Zobacz [śledzenie w składniku ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="ba31a-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="ba31a-199">**ITraceWriter**</span></span> | <span data-ttu-id="ba31a-200">Udostępnia moduł zapisujący śledzenia.</span><span class="sxs-lookup"><span data-stu-id="ba31a-200">Provides a trace writer.</span></span> <span data-ttu-id="ba31a-201">Wartość domyślna to moduł zapisujący śledzenia "pusta".</span><span class="sxs-lookup"><span data-stu-id="ba31a-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="ba31a-202">Zobacz [śledzenie w składniku ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ba31a-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="ba31a-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="ba31a-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="ba31a-204">Udostępnia pamięć podręczną z modułów weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="ba31a-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="ba31a-205">Wiele wystąpień usług</span><span class="sxs-lookup"><span data-stu-id="ba31a-205">Multiple-Instance Services</span></span>


| <span data-ttu-id="ba31a-206">Usługa</span><span class="sxs-lookup"><span data-stu-id="ba31a-206">Service</span></span> | <span data-ttu-id="ba31a-207">Opis</span><span class="sxs-lookup"><span data-stu-id="ba31a-207">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ba31a-208">**IFilterProvider**</span><span class="sxs-lookup"><span data-stu-id="ba31a-208">**IFilterProvider**</span></span> | <span data-ttu-id="ba31a-209">Zwraca listę filtrów dla akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-209">Returns a list of filters for a controller action.</span></span> |
| <span data-ttu-id="ba31a-210">**ModelBinderProvider**</span><span class="sxs-lookup"><span data-stu-id="ba31a-210">**ModelBinderProvider**</span></span> | <span data-ttu-id="ba31a-211">Zwraca integratora modelu dla danego typu.</span><span class="sxs-lookup"><span data-stu-id="ba31a-211">Returns a model binder for a given type.</span></span> |
| <span data-ttu-id="ba31a-212">**ModelMetadataProvider**</span><span class="sxs-lookup"><span data-stu-id="ba31a-212">**ModelMetadataProvider**</span></span> | <span data-ttu-id="ba31a-213">Udostępnia metadane dla modelu.</span><span class="sxs-lookup"><span data-stu-id="ba31a-213">Provides metadata for a model.</span></span> |
| <span data-ttu-id="ba31a-214">**ModelValidatorProvider**</span><span class="sxs-lookup"><span data-stu-id="ba31a-214">**ModelValidatorProvider**</span></span> | <span data-ttu-id="ba31a-215">Udostępnia moduł weryfikacji dla modelu.</span><span class="sxs-lookup"><span data-stu-id="ba31a-215">Provides a validator for a model.</span></span> |
| <span data-ttu-id="ba31a-216">**ValueProviderFactory**</span><span class="sxs-lookup"><span data-stu-id="ba31a-216">**ValueProviderFactory**</span></span> | <span data-ttu-id="ba31a-217">Tworzy dostawcę wartości.</span><span class="sxs-lookup"><span data-stu-id="ba31a-217">Creates a value provider.</span></span> <span data-ttu-id="ba31a-218">Aby uzyskać więcej informacji, zobacz wstrzymania Jan blogu [jak utworzyć dostawcę wartości niestandardowe w WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="ba31a-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |<span data-ttu-id="ba31a-219">.</span><span class="sxs-lookup"><span data-stu-id="ba31a-219">.</span></span>

<span data-ttu-id="ba31a-220">Aby dodać usługę wielowystąpieniowy implementacja niestandardowa, należy wywołać **Dodaj** lub **Wstaw** na **usług** kolekcji:</span><span class="sxs-lookup"><span data-stu-id="ba31a-220">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="ba31a-221">Aby zastąpić jednego wystąpienia usługi implementacja niestandardowa, należy wywołać **Zastąp** na **usług** kolekcji:</span><span class="sxs-lookup"><span data-stu-id="ba31a-221">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="ba31a-222">Konfiguracja dla kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-222">Per-Controller Configuration</span></span>

<span data-ttu-id="ba31a-223">Można zastąpić następujące ustawienia na podstawie na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="ba31a-223">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="ba31a-224">Programy formatujące typy nośnika</span><span class="sxs-lookup"><span data-stu-id="ba31a-224">Media-type formatters</span></span>
- <span data-ttu-id="ba31a-225">Parametr powiązanie reguły</span><span class="sxs-lookup"><span data-stu-id="ba31a-225">Parameter binding rules</span></span>
- <span data-ttu-id="ba31a-226">Usługi</span><span class="sxs-lookup"><span data-stu-id="ba31a-226">Services</span></span>

<span data-ttu-id="ba31a-227">Aby to zrobić, należy zdefiniować atrybutu niestandardowego, który implementuje **IControllerConfiguration** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ba31a-227">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="ba31a-228">Następnie zastosuj atrybut do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ba31a-228">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="ba31a-229">Poniższy przykład zastępuje elementy formatujące domyślny typ nośnika elementu formatującego niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="ba31a-229">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="ba31a-230">**IControllerConfiguration.Initialize** metoda przyjmuje dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="ba31a-230">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="ba31a-231">**HttpControllerSettings** obiektu</span><span class="sxs-lookup"><span data-stu-id="ba31a-231">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="ba31a-232">**HttpControllerDescriptor** obiektu</span><span class="sxs-lookup"><span data-stu-id="ba31a-232">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="ba31a-233">**HttpControllerDescriptor** zawiera opis kontrolera, który można sprawdzić w celach informacyjnych (powiedzieć, aby odróżnić dwa kontrolery).</span><span class="sxs-lookup"><span data-stu-id="ba31a-233">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="ba31a-234">Użyj **HttpControllerSettings** obiekt, aby skonfigurować kontroler.</span><span class="sxs-lookup"><span data-stu-id="ba31a-234">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="ba31a-235">Ten obiekt zawiera podzbiór parametry konfiguracji, które można zastąpić na podstawie poszczególnych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="ba31a-235">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="ba31a-236">Wszystkie ustawienia, które nie zmieniają domyślnie globalnej **HttpConfiguration** obiektu.</span><span class="sxs-lookup"><span data-stu-id="ba31a-236">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
