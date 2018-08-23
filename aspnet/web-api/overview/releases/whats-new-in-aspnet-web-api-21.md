---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Co nowego we wzorcu ASP.NET Web API 2.1 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 6c5a73b95955663d76238e88009ca700f9ace010
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756782"
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="364c5-102">Co nowego we wzorcu ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="364c5-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="364c5-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="364c5-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="364c5-104">W tym temacie opisano what's new for ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="364c5-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="364c5-105">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="364c5-105">Download</span></span>](#download)
- [<span data-ttu-id="364c5-106">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="364c5-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="364c5-107">Nowe funkcje we wzorcu ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="364c5-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="364c5-108">Obsługa błędów globalne</span><span class="sxs-lookup"><span data-stu-id="364c5-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="364c5-109">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="364c5-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="364c5-110">Ulepszenia strony pomocy</span><span class="sxs-lookup"><span data-stu-id="364c5-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="364c5-111">Obsługa IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="364c5-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="364c5-112">Element formatujący typu nośnika BSON</span><span class="sxs-lookup"><span data-stu-id="364c5-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="364c5-113">Lepsza obsługa Async filtry</span><span class="sxs-lookup"><span data-stu-id="364c5-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="364c5-114">Zapytanie analizy dla klienta, formatowanie biblioteki</span><span class="sxs-lookup"><span data-stu-id="364c5-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="364c5-115">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="364c5-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="364c5-116">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="364c5-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="364c5-117">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="364c5-117">Download</span></span>

<span data-ttu-id="364c5-118">Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="364c5-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="364c5-119">Wykonaj wszystkie pakiety środowiska uruchomieniowego [Semantic Versioning](http://semver.org/) specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="364c5-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="364c5-120">Najnowszy pakiet ASP.NET Web API 2.1 RTM ma następującą wersję: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="364c5-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="364c5-121">Możesz zainstalować lub zaktualizować te pakiety przy użyciu [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="364c5-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="364c5-122">Wersja ta oferuje również odpowiedni zlokalizowanych pakietów dla narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="364c5-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="364c5-123">Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="364c5-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="364c5-124">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="364c5-124">Documentation</span></span>

<span data-ttu-id="364c5-125">W samouczkach i innych informacji na temat platformy ASP.NET Web API 2.1 RTM są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="364c5-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="364c5-126">Nowe funkcje we wzorcu ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="364c5-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="364c5-127">Obsługa błędów globalne</span><span class="sxs-lookup"><span data-stu-id="364c5-127">Global Error Handling</span></span>

<span data-ttu-id="364c5-128">Wszystkie nieobsłużone wyjątki, teraz mogą być rejestrowane za pośrednictwem jednego mechanizmu centralnej i można dostosować zachowanie dla nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="364c5-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="364c5-129">Platforma obsługuje wiele rejestratorów wyjątków, które Zobacz nieobsługiwany wyjątek i informacje o kontekście, w którym się pojawił, takie jak żądanie przetwarzane, w tym czasie.</span><span class="sxs-lookup"><span data-stu-id="364c5-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="364c5-130">Na przykład w poniższym kodzie użyto System.Diagnostics.TraceSource, aby rejestrować wszystkie nieobsłużone wyjątki:</span><span class="sxs-lookup"><span data-stu-id="364c5-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="364c5-131">Może również zastąpić domyślny program obsługi wyjątków, tak aby było w pełni dostosować komunikat odpowiedzi HTTP, która jest wysyłana, gdy wystąpił nieobsługiwany wyjątek występuje.</span><span class="sxs-lookup"><span data-stu-id="364c5-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="364c5-132">Udostępniliśmy [przykładowe](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) która rejestruje wszystkie nieobsłużone wyjątki przy użyciu popularnego środowiska ELMAH.</span><span class="sxs-lookup"><span data-stu-id="364c5-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="364c5-133">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="364c5-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="364c5-134">Teraz trasowanie atrybutów obsługuje ograniczenia, umożliwiające wybór opartej na nagłówkach trasy i przechowywania wersji.</span><span class="sxs-lookup"><span data-stu-id="364c5-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="364c5-135">Ponadto wiele aspektów tras atrybutów są teraz można dostosowywać za pomocą **IDirectRouteFactory** interfejsu i **RouteFactoryAttribute** klasy.</span><span class="sxs-lookup"><span data-stu-id="364c5-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="364c5-136">Prefiks trasy jest teraz rozszerzalny za pośrednictwem **IRoutePrefix** interfejsu i **RoutePrefixAttribute** klasy.</span><span class="sxs-lookup"><span data-stu-id="364c5-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="364c5-137">Udostępniliśmy [przykładowe](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) używającej ograniczenia filtrującą dane dynamiczne kontrolerów według nagłówek HTTP "api-version".</span><span class="sxs-lookup"><span data-stu-id="364c5-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="364c5-138">Ulepszenia strony pomocy</span><span class="sxs-lookup"><span data-stu-id="364c5-138">Help Page Improvements</span></span>

<span data-ttu-id="364c5-139">Składnik Web API 2.1 zawiera następujące rozszerzenia [strony pomocy dla interfejsu API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="364c5-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="364c5-140">Dokumentacja poszczególnych właściwości parametry lub zwracane typy akcji.</span><span class="sxs-lookup"><span data-stu-id="364c5-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="364c5-141">Dokumentacja adnotacje modelu danych.</span><span class="sxs-lookup"><span data-stu-id="364c5-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="364c5-142">Projekt interfejsu użytkownika strony pomocy również został zaktualizowany, aby uwzględnić te zmiany.</span><span class="sxs-lookup"><span data-stu-id="364c5-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="364c5-143">Obsługa IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="364c5-143">IgnoreRoute Support</span></span>

<span data-ttu-id="364c5-144">Web API 2.1 obsługuje ignorowanie wzorce adresów URL w routingu internetowego interfejsu API, za pomocą zestawu **IgnoreRoute** metod rozszerzenia **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="364c5-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="364c5-145">Te metody powodują interfejsu API sieci Web ignorować wszystkie adresy URL, które pasują do określonego szablonu i zezwala na hostów w celu zastosowania dodatkowego przetwarzania, jeśli to stosowne.</span><span class="sxs-lookup"><span data-stu-id="364c5-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="364c5-146">Poniższy przykład powoduje ignorowanie identyfikatory URI, które zaczynają się &quot;zawartości&quot; segmencie:</span><span class="sxs-lookup"><span data-stu-id="364c5-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="364c5-147">Element formatujący typu nośnika BSON</span><span class="sxs-lookup"><span data-stu-id="364c5-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="364c5-148">Sieci Web interfejs API obsługuje teraz [BSON](http://bsonspec.org/) formatu, zarówno na kliencie, jak i na serwerze.</span><span class="sxs-lookup"><span data-stu-id="364c5-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="364c5-149">Aby włączyć BSON po stronie serwera, Dodaj **BsonMediaTypeFormatter** do kolekcji elementów formatujących:</span><span class="sxs-lookup"><span data-stu-id="364c5-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="364c5-150">Poniżej przedstawiono, jak klienta platformy .NET mogą używać formatu BSON:</span><span class="sxs-lookup"><span data-stu-id="364c5-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="364c5-151">Udostępniliśmy [przykładowe](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) pokazujący po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="364c5-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="364c5-152">Aby uzyskać więcej informacji, zobacz [Obsługa formatu BSON w sieci Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="364c5-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="364c5-153">Lepsza obsługa Async filtry</span><span class="sxs-lookup"><span data-stu-id="364c5-153">Better Support for Async Filters</span></span>

<span data-ttu-id="364c5-154">Internetowy interfejs API obsługuje teraz łatwy sposób tworzenia filtrów, które są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="364c5-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="364c5-155">Ta funkcja jest przydatna jest filtr potrzebuje do wykonywania akcji asynchronicznych, takich jak dostęp w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="364c5-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="364c5-156">Wcześniej Aby utworzyć filtr async, trzeba było implementacji interfejsu filtru, samodzielnie, ponieważ klas bazowych filtr dostępne tylko synchroniczne metody.</span><span class="sxs-lookup"><span data-stu-id="364c5-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="364c5-157">Teraz możesz zastąpić wirtualnego `On*Async` metody filtr klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="364c5-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="364c5-158">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="364c5-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="364c5-159">**AuthorizationFilterAttribute**, **ActionFilterAttribute**, i **ExceptionFilterAttribute** wszystkie klasy obsługuje asynchroniczne w sieci Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="364c5-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="364c5-160">Zapytanie analizy dla klienta, formatowanie biblioteki</span><span class="sxs-lookup"><span data-stu-id="364c5-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="364c5-161">Wcześniej **System.Net.Http.Formatting** obsługiwane podczas analizowania i aktualizowanie zapytania identyfikatora URI dla kodu po stronie serwera, ale równoważne biblioteki przenośnej brakowało tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="364c5-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="364c5-162">W sieci Web API 2.1 aplikacja kliencka można teraz łatwo analizować i aktualizować ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="364c5-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="364c5-163">Poniższe przykłady pokazują, jak analizować, modyfikować i wygenerować zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="364c5-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="364c5-164">(W przykładach przedstawiono aplikację konsolową w języku dla uproszczenia).</span><span class="sxs-lookup"><span data-stu-id="364c5-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="364c5-165">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="364c5-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="364c5-166">W tej sekcji opisano znane problemy i przełomowe zmiany w ASP.NET Web API 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="364c5-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="364c5-167">Routing atrybutów</span><span class="sxs-lookup"><span data-stu-id="364c5-167">Attribute Routing</span></span>

<span data-ttu-id="364c5-168">Niejasności w atrybucie dopasowania routingu teraz zgłosić błąd, a nie wybierając pierwsze dopasowanie.</span><span class="sxs-lookup"><span data-stu-id="364c5-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="364c5-169">Tras atrybutów mogą używać tychże *{controller}* parametru i przy użyciu *{action}* parametru na trasach umieścić w akcji.</span><span class="sxs-lookup"><span data-stu-id="364c5-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="364c5-170">Te parametry bardzo prawdopodobne spowodowałby niejednoznaczności.</span><span class="sxs-lookup"><span data-stu-id="364c5-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="364c5-171">Tworzenie szkieletów MVC/interfejsu API sieci Web do projektu przy użyciu 5.1 pakietów skutkuje 5.0 pakietów dla tych, które jeszcze nie istnieją w projekcie</span><span class="sxs-lookup"><span data-stu-id="364c5-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="364c5-172">Trwa aktualizowanie pakietów NuGet dla platformy ASP.NET Web API 2.1 RTM nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak funkcja tworzenia szkieletu ASP.NET lub szablon projektu aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="364c5-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="364c5-173">Używają poprzedniej wersji pakiety środowiska uruchomieniowego platformy ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="364c5-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="364c5-174">Co w efekcie funkcja tworzenia szkieletu ASP.NET zainstaluje starszą wersję (5.0.0.0) wymagane pakiety, jeśli nie są jeszcze dostępne w projektach.</span><span class="sxs-lookup"><span data-stu-id="364c5-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="364c5-175">Funkcja tworzenia szkieletu ASP.NET w Visual Studio 2013 RTM i Update 1 nie zastąpi jednak najnowszych pakietów w swoich projektach.</span><span class="sxs-lookup"><span data-stu-id="364c5-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="364c5-176">Jeśli używasz funkcja tworzenia szkieletu ASP.NET po zaktualizowaniu pakietów do sieci Web API 2.1 lub platformy ASP.NET MVC 5.1, upewnij się, że wersje interfejsu API sieci Web i MVC są spójne.</span><span class="sxs-lookup"><span data-stu-id="364c5-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="364c5-177">Zmienia nazwę typu</span><span class="sxs-lookup"><span data-stu-id="364c5-177">Type Renames</span></span>

<span data-ttu-id="364c5-178">Niektóre typy używane do rozszerzalności routingu atrybutu nazwy zostały zmienione z wersji RC do 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="364c5-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="364c5-179">Stara nazwa typu (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="364c5-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="364c5-180">Nowy typ nazwy (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="364c5-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="364c5-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="364c5-181">IDirectRouteProvider</span></span> | <span data-ttu-id="364c5-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="364c5-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="364c5-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="364c5-183">RouteProviderAttribute</span></span> | <span data-ttu-id="364c5-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="364c5-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="364c5-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="364c5-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="364c5-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="364c5-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="364c5-187">Filtry wyjątków nie Odkodowywanie agregacji wyjątki zgłaszane w działaniach asynchronicznych</span><span class="sxs-lookup"><span data-stu-id="364c5-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="364c5-188">Wcześniej Jeśli akcję async spowodowała **aggregateexception —**, filtra wyjątku czy Odkodowywanie wyjątek, i **OnException** otrzymamy wyjątek podstawowy.</span><span class="sxs-lookup"><span data-stu-id="364c5-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="364c5-189">2.1, filtra wyjątku być nie Odkodowywanie, oraz **OnException** pobiera oryginalny **aggregateexception —**.</span><span class="sxs-lookup"><span data-stu-id="364c5-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="364c5-190">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="364c5-190">Bug Fixes</span></span>

<span data-ttu-id="364c5-191">Ta wersja zawiera także kilka poprawek usterek.</span><span class="sxs-lookup"><span data-stu-id="364c5-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="364c5-192">Pełną listę można znaleźć:</span><span class="sxs-lookup"><span data-stu-id="364c5-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="364c5-193">5.1.0 pakiet</span><span class="sxs-lookup"><span data-stu-id="364c5-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="364c5-194">5.1.1 pakiet</span><span class="sxs-lookup"><span data-stu-id="364c5-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="364c5-195">5.1.2 pakiet zawiera aktualizacje funkcji IntelliSense, ale nie poprawki.</span><span class="sxs-lookup"><span data-stu-id="364c5-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
