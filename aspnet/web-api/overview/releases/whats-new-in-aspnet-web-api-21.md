---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: What's New in ASP.NET Web API 2.1 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="73907-102">What's New in ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="73907-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="73907-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="73907-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="73907-104">W tym temacie opisano, jakie nowości 2.1 interfejsu API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="73907-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="73907-105">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="73907-105">Download</span></span>](#download)
- [<span data-ttu-id="73907-106">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="73907-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="73907-107">Nowe funkcje w składniku ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="73907-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="73907-108">Obsługa błędów globalne</span><span class="sxs-lookup"><span data-stu-id="73907-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="73907-109">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="73907-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="73907-110">Ulepszenia strona pomocy</span><span class="sxs-lookup"><span data-stu-id="73907-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="73907-111">Obsługa IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="73907-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="73907-112">Program formatujący typ nośnika BSON</span><span class="sxs-lookup"><span data-stu-id="73907-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="73907-113">Lepszą obsługę filtry Async</span><span class="sxs-lookup"><span data-stu-id="73907-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="73907-114">Analiza kodu dla klienta formatowania biblioteki zapytania</span><span class="sxs-lookup"><span data-stu-id="73907-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="73907-115">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="73907-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="73907-116">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="73907-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="73907-117">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="73907-117">Download</span></span>

<span data-ttu-id="73907-118">Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="73907-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="73907-119">Wykonaj wszystkie pakiety środowiska uruchomieniowego [Wersjonowania semantycznego](http://semver.org/) specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="73907-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="73907-120">Najnowszy pakiet ASP.NET Web API 2.1 RTM ma następującą wersję: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="73907-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="73907-121">Można zainstalować lub zaktualizować te pakiety za pośrednictwem [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="73907-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="73907-122">Wydanie obejmuje również odpowiedniego zlokalizowane pakiety na NuGet.</span><span class="sxs-lookup"><span data-stu-id="73907-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="73907-123">Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="73907-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="73907-124">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="73907-124">Documentation</span></span>

<span data-ttu-id="73907-125">Samouczki i inne informacje na temat programu ASP.NET Web API 2.1 RTM są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="73907-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="73907-126">Nowe funkcje w składniku ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="73907-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="73907-127">Obsługa błędów globalne</span><span class="sxs-lookup"><span data-stu-id="73907-127">Global Error Handling</span></span>

<span data-ttu-id="73907-128">Wszystkie nieobsługiwane wyjątki teraz mogą być rejestrowane za pośrednictwem jednego mechanizmu centralnej i można dostosować zachowanie nieobsługiwanych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="73907-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="73907-129">Platforma obsługuje wiele rejestratorów wyjątków, które można znaleźć w nieobsługiwany wyjątek i informacje o kontekście, w którym wystąpił, takich jak żądanie przetwarzane, w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="73907-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="73907-130">Na przykład w poniższym kodzie użyto System.Diagnostics.TraceSource rejestrowania wszystkich nieobsługiwanych wyjątków:</span><span class="sxs-lookup"><span data-stu-id="73907-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="73907-131">Można również zastąpić domyślny program obsługi wyjątku, dzięki czemu można całkowicie dostosować komunikat odpowiedzi HTTP, który jest wysyłany, gdy wystąpił nieobsługiwany wyjątek występuje.</span><span class="sxs-lookup"><span data-stu-id="73907-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="73907-132">Firma Microsoft umieściła [próbki](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) który rejestruje wszystkie nieobsługiwane wyjątki za pomocą popularnych framework ELMAH.</span><span class="sxs-lookup"><span data-stu-id="73907-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="73907-133">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="73907-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="73907-134">Atrybut routingu teraz obsługuje ograniczenia, włączanie wersji i wybór tras oparte na protokole nagłówka.</span><span class="sxs-lookup"><span data-stu-id="73907-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="73907-135">Ponadto wiele aspektów tras atrybutów są teraz można dostosować za pomocą **IDirectRouteFactory** interfejsu i **RouteFactoryAttribute** klasy.</span><span class="sxs-lookup"><span data-stu-id="73907-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="73907-136">Prefiks trasy jest teraz rozszerzalny za pośrednictwem **IRoutePrefix** interfejsu i **RoutePrefixAttribute** klasy.</span><span class="sxs-lookup"><span data-stu-id="73907-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="73907-137">Firma Microsoft umieściła [próbki](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) używającą ograniczenia dynamicznie filtrowania kontrolerów nagłówka HTTP "api-version".</span><span class="sxs-lookup"><span data-stu-id="73907-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="73907-138">Ulepszenia strona pomocy</span><span class="sxs-lookup"><span data-stu-id="73907-138">Help Page Improvements</span></span>

<span data-ttu-id="73907-139">Składnik Web API 2.1 zawiera następujące rozszerzenia [strony pomocy interfejsu API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="73907-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="73907-140">Dokumentacja poszczególnych właściwości parametrów lub zwracanych typów akcji.</span><span class="sxs-lookup"><span data-stu-id="73907-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="73907-141">Dokumentacja adnotacji modelu danych.</span><span class="sxs-lookup"><span data-stu-id="73907-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="73907-142">Projekt interfejsu użytkownika strony pomocy Zaktualizowano również, aby uwzględnić te zmiany.</span><span class="sxs-lookup"><span data-stu-id="73907-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="73907-143">Obsługa IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="73907-143">IgnoreRoute Support</span></span>

<span data-ttu-id="73907-144">Sieci Web obsługuje interfejs API 2.1 ignorowanie wzorców adresów URL w składniku Web API routingu, za pomocą zestawu **IgnoreRoute** metody rozszerzenia na **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="73907-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="73907-145">Te metody spowodować interfejsu API sieci Web zignorować wszelkie adresy URL, które pasują do określonego szablonu i umożliwić hosta do zastosowania w razie potrzeby dodatkowego przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="73907-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="73907-146">Poniższy przykład powoduje ignorowanie identyfikatory URI, które zaczynają się &quot;zawartości&quot; segmentu:</span><span class="sxs-lookup"><span data-stu-id="73907-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="73907-147">Program formatujący typ nośnika BSON</span><span class="sxs-lookup"><span data-stu-id="73907-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="73907-148">Sieci Web obsługuje teraz interfejsu API [BSON](http://bsonspec.org/) format danych przesyłanych w sieci, zarówno na kliencie, jak i na serwerze.</span><span class="sxs-lookup"><span data-stu-id="73907-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="73907-149">Aby włączyć BSON po stronie serwera, Dodaj **BsonMediaTypeFormatter** do kolekcji elementów formatujących:</span><span class="sxs-lookup"><span data-stu-id="73907-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="73907-150">Oto, jak klienta .NET mogą korzystać z formatu BSON:</span><span class="sxs-lookup"><span data-stu-id="73907-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="73907-151">Firma Microsoft umieściła [próbki](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) którym wyświetlana po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="73907-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="73907-152">Aby uzyskać więcej informacji, zobacz [wsparcia formatu BSON 2.1 interfejsu API sieci Web](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="73907-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="73907-153">Lepszą obsługę filtry Async</span><span class="sxs-lookup"><span data-stu-id="73907-153">Better Support for Async Filters</span></span>

<span data-ttu-id="73907-154">Interfejs API sieci Web obsługuje teraz łatwo tworzyć filtry, które są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="73907-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="73907-155">Ta funkcja jest przydatna jest filtr potrzebnych do wykonania akcji asynchronicznej, takich jak dostęp w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="73907-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="73907-156">Wcześniej Aby utworzyć filtr asynchroniczne, trzeba było implementować interfejs filtru samodzielnie, ponieważ klas podstawowych filtru dostępne tylko metod synchronicznych.</span><span class="sxs-lookup"><span data-stu-id="73907-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="73907-157">Teraz można zastąpić wirtualnego `On*Async` klasy podstawowej metody filtru.</span><span class="sxs-lookup"><span data-stu-id="73907-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="73907-158">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="73907-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="73907-159">**AuthorizationFilterAttribute**, **ActionFilterAttribute**, i **ExceptionFilterAttribute** wszystkie klasy obsługuje asynchroniczne 2.1 interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="73907-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="73907-160">Analiza kodu dla klienta formatowania biblioteki zapytania</span><span class="sxs-lookup"><span data-stu-id="73907-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="73907-161">Wcześniej **System.Net.Http.Formatting** obsługiwane podczas analizowania i aktualizowanie zapytania identyfikatora URI dla kodu po stronie serwera, ale równoważne przenośnej biblioteki brakowało tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="73907-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="73907-162">W sieci Web interfejsu API 2.1 aplikacja kliencka można teraz łatwo przeanalizować i aktualizować ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="73907-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="73907-163">Poniższe przykłady przedstawiają sposób przeanalizować, modyfikowania i generowania zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="73907-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="73907-164">(W przykładach przedstawiono aplikację konsolową dla uproszczenia).</span><span class="sxs-lookup"><span data-stu-id="73907-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="73907-165">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="73907-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="73907-166">W tej sekcji opisano znane problemy i fundamentalne zmiany w RTM 2.1 interfejsu API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="73907-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="73907-167">Atrybut routingu</span><span class="sxs-lookup"><span data-stu-id="73907-167">Attribute Routing</span></span>

<span data-ttu-id="73907-168">Niejednoznaczności w dopasowań routingu atrybutu teraz raport błędu zamiast wybranie pierwszego dopasowania.</span><span class="sxs-lookup"><span data-stu-id="73907-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="73907-169">Tras atrybutów mogą używać tychże *{controller}* parametru i korzystania z *{action}* parametru na trasach dotyczącymi akcji.</span><span class="sxs-lookup"><span data-stu-id="73907-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="73907-170">Te parametry najprawdopodobniej spowoduje, że niejednoznaczności.</span><span class="sxs-lookup"><span data-stu-id="73907-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="73907-171">Funkcja szkieletów MVC/interfejs API sieci Web do projektu z 5.1 wynikiem pakiety w wersji 5.0 pakiety dla tych, które już nie istnieje w projekcie</span><span class="sxs-lookup"><span data-stu-id="73907-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="73907-172">Aktualizowanie pakietów NuGet dla programu ASP.NET Web API 2.1 RTM nie powoduje aktualizacji Visual Studio tools, takich jak ASP.NET rusztowania lub szablonu projektu aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="73907-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="73907-173">Korzystają z poprzedniej wersji pakietów środowiska uruchomieniowego ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="73907-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="73907-174">W związku z tym szkieletów ASP.NET zainstaluje poprzedniej wersji wymaganych pakietów (5.0.0.0), jeśli nie są one już dostępne w projektach.</span><span class="sxs-lookup"><span data-stu-id="73907-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="73907-175">Jednak funkcja szkieletów ASP.NET w programie Visual Studio 2013 RTM lub Update 1 nie powoduje zastąpienia najnowszych pakietów w projektach.</span><span class="sxs-lookup"><span data-stu-id="73907-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="73907-176">Użycie programu ASP.NET szkieletów po zaktualizowaniu pakietów 2.1 interfejsu API sieci Web lub ASP.NET MVC w wersji 5.1, upewnij się, że wersje interfejsu API sieci Web i MVC są spójne.</span><span class="sxs-lookup"><span data-stu-id="73907-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="73907-177">Zmienia nazwę typu</span><span class="sxs-lookup"><span data-stu-id="73907-177">Type Renames</span></span>

<span data-ttu-id="73907-178">Niektóre typy używane dla routingu rozszerzalności atrybutu nazwy zostały zmienione z wersji RC do wersji 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="73907-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="73907-179">Stara nazwa typu (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="73907-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="73907-180">Nowy typ Name (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="73907-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="73907-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="73907-181">IDirectRouteProvider</span></span> | <span data-ttu-id="73907-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="73907-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="73907-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="73907-183">RouteProviderAttribute</span></span> | <span data-ttu-id="73907-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="73907-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="73907-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="73907-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="73907-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="73907-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="73907-187">Filtry wyjątków nie dekodowania agregacji wyjątki zgłaszane w asynchronicznych akcji</span><span class="sxs-lookup"><span data-stu-id="73907-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="73907-188">Wcześniej Jeśli operacji asynchronicznej zwrócił **AggregateException**, filtra wyjątku czy dekodowania wyjątku, i **OnException** otrzyma podstawowego wyjątku.</span><span class="sxs-lookup"><span data-stu-id="73907-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="73907-189">2.1, filtru wyjątków nie Cofnij zawijanie, i **OnException** pobiera oryginalny **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="73907-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="73907-190">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="73907-190">Bug Fixes</span></span>

<span data-ttu-id="73907-191">Ta wersja zawiera również kilka poprawek.</span><span class="sxs-lookup"><span data-stu-id="73907-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="73907-192">Pełną listę można znaleźć:</span><span class="sxs-lookup"><span data-stu-id="73907-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="73907-193">5.1.0 pakiet</span><span class="sxs-lookup"><span data-stu-id="73907-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="73907-194">5.1.1 pakietu</span><span class="sxs-lookup"><span data-stu-id="73907-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="73907-195">5.1.2 pakiet zawiera aktualizacje funkcji IntelliSense, ale nie poprawki.</span><span class="sxs-lookup"><span data-stu-id="73907-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
