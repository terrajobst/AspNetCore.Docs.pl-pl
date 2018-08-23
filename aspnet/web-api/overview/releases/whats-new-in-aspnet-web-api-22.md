---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Co nowego we wzorcu ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: ef08a3bb397ff54795ca6c70cdcc35206cf122f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756310"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="7615d-102">Co nowego we wzorcu ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="7615d-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="7615d-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7615d-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="7615d-104">W tym temacie opisano what's new for ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="7615d-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="7615d-105">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="7615d-105">Download</span></span>](#download)
- [<span data-ttu-id="7615d-106">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="7615d-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="7615d-107">Nowe funkcje we wzorcu ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="7615d-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="7615d-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="7615d-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="7615d-109">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="7615d-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="7615d-110">Obsługa klienta interfejsu API sieci Web dla systemu Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="7615d-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="7615d-111">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="7615d-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="7615d-112">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="7615d-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="7615d-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="7615d-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="7615d-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="7615d-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="7615d-115">Beta Microsoft.AspNet.WebAPI 5.2.3</span><span class="sxs-lookup"><span data-stu-id="7615d-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="7615d-116">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="7615d-116">Download</span></span>

<span data-ttu-id="7615d-117">Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="7615d-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="7615d-118">Wykonaj wszystkie pakiety środowiska uruchomieniowego [Semantic Versioning](http://semver.org/) specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="7615d-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="7615d-119">Najnowszy pakiet ASP.NET Web API 2.2 ma następującą wersję: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="7615d-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="7615d-120">Możesz zainstalować lub zaktualizować te pakiety przy użyciu [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="7615d-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="7615d-121">Wersja ta oferuje również odpowiedni zlokalizowanych pakietów dla narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="7615d-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="7615d-122">Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="7615d-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="7615d-123">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="7615d-123">Documentation</span></span>

<span data-ttu-id="7615d-124">W samouczkach i innych informacji na temat platformy ASP.NET Web API 2.2 są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="7615d-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="7615d-125">Nowe funkcje we wzorcu ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="7615d-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="7615d-126">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="7615d-126">OData v4</span></span>

<span data-ttu-id="7615d-127">W tej wersji dodano obsługę protokołu OData 4.</span><span class="sxs-lookup"><span data-stu-id="7615d-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="7615d-128">Aby uzyskać więcej informacji, zobacz [Web API OData v4 dokumentacji.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="7615d-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="7615d-129">Poniżej przedstawiono niektóre najważniejsze funkcje i zmiany dotyczące protokołu OData v4:</span><span class="sxs-lookup"><span data-stu-id="7615d-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="7615d-130">Obsługa aliasów właściwości w modelu OData</span><span class="sxs-lookup"><span data-stu-id="7615d-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="7615d-131">Obsługa ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute i ConcurrencyCheckAttribute w ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="7615d-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="7615d-132">Zapewnia możliwość dostarczania tytuł przyjazne dla akcji</span><span class="sxs-lookup"><span data-stu-id="7615d-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="7615d-133">Integracja z usługą ODL UriParser</span><span class="sxs-lookup"><span data-stu-id="7615d-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="7615d-134">Obsługa [wyliczenia](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [zawierania](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) i [pojedynczego wystąpienia](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="7615d-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="7615d-135">Rzutowanie typów pierwotnych pomocy technicznej</span><span class="sxs-lookup"><span data-stu-id="7615d-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="7615d-136">Dodano obsługę funkcji OData</span><span class="sxs-lookup"><span data-stu-id="7615d-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="7615d-137">Obsługa aliasów parametrów wywołania funkcji</span><span class="sxs-lookup"><span data-stu-id="7615d-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="7615d-138">Obsługuje pisane konwencji nazewnictwa przypadków w modelu</span><span class="sxs-lookup"><span data-stu-id="7615d-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="7615d-139">Obsługa cast() w $filter</span><span class="sxs-lookup"><span data-stu-id="7615d-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="7615d-140">Obsługa Otwórz typ złożony</span><span class="sxs-lookup"><span data-stu-id="7615d-140">Support for open complex type</span></span>
- <span data-ttu-id="7615d-141">Usunięto EntitySetController i AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="7615d-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="7615d-142">Zmienione $link do $ref</span><span class="sxs-lookup"><span data-stu-id="7615d-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="7615d-143">Dodanie obsługi routingu atrybutu</span><span class="sxs-lookup"><span data-stu-id="7615d-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="7615d-144">Korzysta z bibliotek usługi OData Core 6.4.0</span><span class="sxs-lookup"><span data-stu-id="7615d-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="7615d-145">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="7615d-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="7615d-146">Teraz trasowanie atrybutów udostępnia punkt rozszerzalności o nazwie IDirectRouteProvider, która umożliwia pełną kontrolę nad jak tras atrybutów są odnajdywane i skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="7615d-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="7615d-147">IDirectRouteProvider jest odpowiedzialny za zapewnienie listę akcji i kontrolerów wraz z informacjami skojarzone trasy do określenia dokładnie wymaganego konfigurację routingu dla tych akcji.</span><span class="sxs-lookup"><span data-stu-id="7615d-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="7615d-148">Implementacja IDirectRouteProvider może być określona podczas wywoływania MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="7615d-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="7615d-149">Dostosowywanie IDirectRouteProvider będzie najprostszym, rozszerzając nasze Domyślna implementacja DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="7615d-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="7615d-150">Ta klasa udostępnia oddzielne możliwym do zastąpienia metod wirtualnych, aby zmienić logikę odnajdywania atrybutów, tworząc wejścia dla trasy i odnajdywanie prefiks trasy i prefiks obszaru.</span><span class="sxs-lookup"><span data-stu-id="7615d-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="7615d-151">Poniżej przedstawiono kilka przykładów, co można zrobić za pomocą tego nowego punktu rozszerzalności:</span><span class="sxs-lookup"><span data-stu-id="7615d-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="7615d-152">Obsługa dziedziczenia atrybuty trasy</span><span class="sxs-lookup"><span data-stu-id="7615d-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="7615d-153">Przykład:</span><span class="sxs-lookup"><span data-stu-id="7615d-153">Example:</span></span>

    <span data-ttu-id="7615d-154">W tym miejscu żądania like "/ api/wartości/10" pomyślnie zwróci "SUKCES: 10"</span><span class="sxs-lookup"><span data-stu-id="7615d-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="7615d-155">Podaj nazwę trasy domyślne trasy atrybutu postępując zgodnie z niektórych Konwencji, którą chcesz.</span><span class="sxs-lookup"><span data-stu-id="7615d-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="7615d-156">Domyślnie trasowanie atrybutów nie tworzy automatycznie nazw dla tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="7615d-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="7615d-157">Przed znajdą się one w tabeli tras, należy zmodyfikować szablon trasy tras atrybutów w jednej centralnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="7615d-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="7615d-158">Obsługa klienta interfejsu API sieci Web dla Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="7615d-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="7615d-159">Można teraz używać pakietu NuGet klienta interfejsu API sieci Web zaimplementować logikę klienta interfejsu API sieci Web podczas określania wartości docelowej systemu Windows Phone 8.1 lub z poziomu aplikacji uniwersalnej.</span><span class="sxs-lookup"><span data-stu-id="7615d-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="7615d-160">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="7615d-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="7615d-161">W tej sekcji opisano znane problemy i przełomowe zmiany w programie ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="7615d-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="7615d-162">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="7615d-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="7615d-163">Konstruktor modelu</span><span class="sxs-lookup"><span data-stu-id="7615d-163">Model builder</span></span>

<span data-ttu-id="7615d-164">Problem: Przeciążonych funkcji nie może być udostępniany jako element FunctionImport</span><span class="sxs-lookup"><span data-stu-id="7615d-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="7615d-165">Jeśli istnieją 2 funkcje przeciążone, a następnie są one również element FunctionImport, jak pokazano poniżej następnie żądanie ~/GetAllConventionCustomers(CustomerName={customerName}) skutkuje System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="7615d-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="7615d-166">Obejście: Sposobu obejścia tego problemu jest dodanie przeciążenia funkcji jako FunctionImports.</span><span class="sxs-lookup"><span data-stu-id="7615d-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="7615d-167">Routing protokołu OData</span><span class="sxs-lookup"><span data-stu-id="7615d-167">OData Routing</span></span>

<span data-ttu-id="7615d-168">Literały ciągów, które obejmują adres URL zakodowane ukośnika (% 2F), a backslash(%5C) spowodować błąd 404, gdy są one używane w ścieżkach zasobów OData.</span><span class="sxs-lookup"><span data-stu-id="7615d-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="7615d-169">Na przykład literałów ciągów może służyć w ścieżkach zasobów OData jako wartości kluczy zestawy jednostek lub parametrów funkcji.</span><span class="sxs-lookup"><span data-stu-id="7615d-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="7615d-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="7615d-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="7615d-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="7615d-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="7615d-172">Usługi otrzymywać takie żądania hosty będą un-znak ucieczki tych ucieczki sekwencji przed przekazaniem ich do internetowego interfejsu API środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="7615d-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="7615d-173">Chroni to przed atakami, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="7615d-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="7615d-174">Powoduje to stosu Web API OData zwrócić błąd 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="7615d-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="7615d-175">Aby uniknąć tego błędu, klienta należy używać podwójnego unikowe kreski ułamkowej (% 252F) i kreski ułamkowej odwróconej (% 255C).</span><span class="sxs-lookup"><span data-stu-id="7615d-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="7615d-176">Nie jest to realizowane dla ciągów zapytań, takich jak /Employees? $filter = nazwa eq "Nazwa % 2F"</span><span class="sxs-lookup"><span data-stu-id="7615d-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="7615d-177">**Należy pamiętać, niezmieniony ukośniki ("/") i ukośniki odwrotne (") nie są dozwolone w literałów ciągów ścieżkę zasobów OData. Ukośniki powinna zostać wyświetlona tylko jako separatorami ścieżki i ukośników odwrotnych nie powinien pojawić się w ścieżkę zasobów OData w ogóle. (Oba są użyteczne w niektórych obszarach ciągu zapytania OData).**</span><span class="sxs-lookup"><span data-stu-id="7615d-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="7615d-178">Obejście: Można zastąpić metody Parse DefaultODataPathHandler jako znak ucieczki ukośnika i kreski ułamkowej odwróconej w literałach ciągu przed faktycznie analizowanie ich.</span><span class="sxs-lookup"><span data-stu-id="7615d-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="7615d-179">Przykładem tego podejścia, w tym miejscu można znaleźć.</span><span class="sxs-lookup"><span data-stu-id="7615d-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="7615d-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="7615d-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="7615d-181">[Odpytywalny]</span><span class="sxs-lookup"><span data-stu-id="7615d-181">[Queryable]</span></span>

<span data-ttu-id="7615d-182">Atrybut [Queryable] jest przestarzały.</span><span class="sxs-lookup"><span data-stu-id="7615d-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="7615d-183">Nowe OData v3 aplikacje powinny używać **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="7615d-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="7615d-184">**ODataHttpConfigurationExtensions.EnableQuerySupport** — metoda rozszerzenia jest teraz dodaje **EnableQueryAttribute** do kolekcji filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="7615d-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="7615d-185">Jeśli ma żadnych kontrolerów **[Queryable]** atrybutu i wywoływania `config.EnableQuerySupport()` spowoduje, że **[Queryable]** atrybutu, aby zakończyć się niepowodzeniem</span><span class="sxs-lookup"><span data-stu-id="7615d-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="7615d-186">Zalecanym sposobem, aby rozwiązać ten problem jest zastąpić wszystkie wystąpienia **klasie QueryableAttribute** z **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="7615d-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="7615d-187">Jeszcze inne obejście jest Użyj poniższego kodu, w konfiguracji interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="7615d-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="7615d-188">Routing atrybutów</span><span class="sxs-lookup"><span data-stu-id="7615d-188">Attribute Routing</span></span>

<span data-ttu-id="7615d-189">Problem: Wiązanie modelu typu złożonego, który zostanie nadany atrybut FromUri zachowuje się inaczej korzystając z trasami atrybutów.</span><span class="sxs-lookup"><span data-stu-id="7615d-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="7615d-190">Poniższe łącze służy do śledzenia problemu i ma również szczegółowe informacje o obejście tego problemu.</span><span class="sxs-lookup"><span data-stu-id="7615d-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="7615d-191">Problem: Interfejs API sieci Web i MVC szkieletu do projektu przy użyciu 5.2.0 pakietów skutkuje 5.1.2 pakietów dla tych, które jeszcze nie istnieją w projekcie</span><span class="sxs-lookup"><span data-stu-id="7615d-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="7615d-192">Trwa aktualizowanie pakietów NuGet dla platformy ASP.NET MVC 5.2 nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak funkcja tworzenia szkieletu ASP.NET lub szablon projektu aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7615d-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="7615d-193">Używają poprzedniej wersji pakiety środowiska uruchomieniowego platformy ASP.NET (np. 5.1.2 w wersji Update 2).</span><span class="sxs-lookup"><span data-stu-id="7615d-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="7615d-194">Co w efekcie funkcja tworzenia szkieletu ASP.NET zainstaluje poprzedniej wersji (np. 5.1.2 w wersji Update 2) wymagane pakiety, jeśli nie są jeszcze dostępne w projektach.</span><span class="sxs-lookup"><span data-stu-id="7615d-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="7615d-195">Funkcja tworzenia szkieletu ASP.NET w Visual Studio 2013 RTM i Update 1 nie zastąpi jednak najnowszych pakietów w swoich projektach.</span><span class="sxs-lookup"><span data-stu-id="7615d-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="7615d-196">Jeśli używasz funkcja tworzenia szkieletu ASP.NET po zaktualizowaniu pakietów projektów sieci Web API 2.2 lub platformy ASP.NET MVC 5.2, upewnij się, że wersje interfejsu API sieci Web platformy ASP.NET MVC są spójne.</span><span class="sxs-lookup"><span data-stu-id="7615d-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="7615d-197">Poprawki i aktualizacje funkcji pomocniczych</span><span class="sxs-lookup"><span data-stu-id="7615d-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="7615d-198">Ta wersja zawiera także kilka poprawek usterek i funkcji drobne aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="7615d-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="7615d-199">Pełną listę można znaleźć:</span><span class="sxs-lookup"><span data-stu-id="7615d-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="7615d-200">5.2 pakietu</span><span class="sxs-lookup"><span data-stu-id="7615d-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="7615d-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="7615d-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="7615d-202">Pakiet Microsoft.AspNet.OData 5.2.1 zawiera aktualizacje zależności NuGet, ale nie poprawki.</span><span class="sxs-lookup"><span data-stu-id="7615d-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="7615d-203">Dzięki tej aktualizacji nie jest już strict zależności na Microsoft.OData.Core 6.4.0, ale jeden można uaktualnić do dowolnej wersji między 6.4.0 i 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="7615d-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="7615d-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="7615d-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="7615d-205">W tej wersji wprowadzono zmiany zależności dla `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="7615d-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="7615d-206">Aby uzyskać więcej informacji na temat nowości w tej wersji `Json.NET`, zobacz [Json.NET 6.0 Release 4 — scalanie kodu JSON, wstrzykiwanie zależności](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7615d-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="7615d-207">W tej wersji nie zawarto żadnych nowych funkcji ani poprawek błędów w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7615d-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="7615d-208">Zaktualizowaliśmy wszystkie inne zależne pakiety, możemy do są zależne od tej nowej wersji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7615d-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="7615d-209">Beta Microsoft.AspNet.WebAPI 5.2.3</span><span class="sxs-lookup"><span data-stu-id="7615d-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="7615d-210">Informacje o wersji [tutaj](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="7615d-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="7615d-211">Ta wersja zawiera tylko poprawki.</span><span class="sxs-lookup"><span data-stu-id="7615d-211">This release contains only bug fixes.</span></span> <span data-ttu-id="7615d-212">Możesz użyć [to zapytanie](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) Aby wyświetlić listę problemów rozwiązanych w tej wersji.</span><span class="sxs-lookup"><span data-stu-id="7615d-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
