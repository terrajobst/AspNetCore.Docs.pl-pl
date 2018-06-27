---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: What's New in ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 89b065fccd0e4864f4a24c37b4caa29a1e127840
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961302"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="9ffa7-102">What's New in ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="9ffa7-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="9ffa7-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9ffa7-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="9ffa7-104">W tym temacie opisano, co jest nowego w wersji 2.2 interfejsu API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="9ffa7-105">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="9ffa7-105">Download</span></span>](#download)
- [<span data-ttu-id="9ffa7-106">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="9ffa7-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="9ffa7-107">Nowe funkcje w składniku ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="9ffa7-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="9ffa7-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="9ffa7-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="9ffa7-109">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="9ffa7-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="9ffa7-110">Obsługa klienta interfejsu API sieci Web dla systemu Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="9ffa7-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="9ffa7-111">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="9ffa7-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="9ffa7-112">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="9ffa7-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="9ffa7-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="9ffa7-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="9ffa7-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="9ffa7-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="9ffa7-115">Microsoft.AspNet.WebAPI 5.2.3 w wersji Beta</span><span class="sxs-lookup"><span data-stu-id="9ffa7-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="9ffa7-116">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="9ffa7-116">Download</span></span>

<span data-ttu-id="9ffa7-117">Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="9ffa7-118">Wykonaj wszystkie pakiety środowiska uruchomieniowego [Wersjonowania semantycznego](http://semver.org/) specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="9ffa7-119">Najnowszy pakiet 2.2 interfejsu API sieci Web ASP.NET ma następującą wersję: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="9ffa7-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="9ffa7-120">Można zainstalować lub zaktualizować te pakiety za pośrednictwem [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="9ffa7-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="9ffa7-121">Wydanie obejmuje również odpowiedniego zlokalizowane pakiety na NuGet.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="9ffa7-122">Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="9ffa7-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="9ffa7-123">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="9ffa7-123">Documentation</span></span>

<span data-ttu-id="9ffa7-124">Samouczki i inne informacje o 2.2 interfejsu API sieci Web platformy ASP.NET są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="9ffa7-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="9ffa7-125">Nowe funkcje w składniku ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="9ffa7-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="9ffa7-126">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="9ffa7-126">OData v4</span></span>

<span data-ttu-id="9ffa7-127">Ta wersja dodaje obsługę protokołu OData v4.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="9ffa7-128">Aby uzyskać więcej informacji, zobacz [Web API OData v4 dokumentacji.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="9ffa7-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="9ffa7-129">Oto niektóre najważniejsze funkcje i zmiany dotyczące protokołu OData v4:</span><span class="sxs-lookup"><span data-stu-id="9ffa7-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="9ffa7-130">Obsługa aliasów właściwości w modelu OData</span><span class="sxs-lookup"><span data-stu-id="9ffa7-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="9ffa7-131">Obsługa ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute i ConcurrencyCheckAttribute w ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="9ffa7-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="9ffa7-132">Zapewniają możliwość Podaj przyjazną tytuł dla akcji</span><span class="sxs-lookup"><span data-stu-id="9ffa7-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="9ffa7-133">Integracja z ODL UriParser</span><span class="sxs-lookup"><span data-stu-id="9ffa7-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="9ffa7-134">Obsługa [wyliczenia](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [zawierania](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) i [pojedynczego wystąpienia](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="9ffa7-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="9ffa7-135">Rzutowanie typów pierwotnych pomocy technicznej</span><span class="sxs-lookup"><span data-stu-id="9ffa7-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="9ffa7-136">Dodano obsługę funkcji OData</span><span class="sxs-lookup"><span data-stu-id="9ffa7-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="9ffa7-137">Obsługa aliasów parametrów wywołania funkcji</span><span class="sxs-lookup"><span data-stu-id="9ffa7-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="9ffa7-138">Obsługuje format camel case konwencję nazewnictwa w modelu</span><span class="sxs-lookup"><span data-stu-id="9ffa7-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="9ffa7-139">Obsługa cast() w $filter</span><span class="sxs-lookup"><span data-stu-id="9ffa7-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="9ffa7-140">Obsługa Otwórz typu złożonego</span><span class="sxs-lookup"><span data-stu-id="9ffa7-140">Support for open complex type</span></span>
- <span data-ttu-id="9ffa7-141">Usunięto EntitySetController i AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="9ffa7-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="9ffa7-142">Zmienione $link do $ref</span><span class="sxs-lookup"><span data-stu-id="9ffa7-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="9ffa7-143">Dodano obsługę routingu atrybutu</span><span class="sxs-lookup"><span data-stu-id="9ffa7-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="9ffa7-144">Korzysta z bibliotek usługi OData Core 6.4.0</span><span class="sxs-lookup"><span data-stu-id="9ffa7-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="9ffa7-145">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="9ffa7-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="9ffa7-146">Atrybut routingu teraz udostępnia punkt rozszerzalności o nazwie IDirectRouteProvider, który umożliwia pełną kontrolę nad jak tras atrybutów są odnajdywane i skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="9ffa7-147">IDirectRouteProvider jest odpowiedzialny za zapewnienie listę akcji i kontrolerów wraz z informacjami skojarzone trasy do określenia dokładnie konfiguracji routingu jest pożądany dla tych czynności.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="9ffa7-148">Implementacja IDirectRouteProvider można określić podczas wywoływania metody MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="9ffa7-149">Dostosowywanie IDirectRouteProvider będzie najprostszym rozszerzając naszych Domyślna implementacja DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="9ffa7-150">Ta klasa zawiera osobne wirtualnego metod, które można zmienić logikę wykrywania atrybuty, tworzenie wejścia dla trasy i odnajdywanie prefiks trasy i prefiks obszaru.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="9ffa7-151">Poniżej przedstawiono niektóre przykłady co można zrobić to nowy punkt rozszerzeń:</span><span class="sxs-lookup"><span data-stu-id="9ffa7-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="9ffa7-152">Obsługa dziedziczenia atrybutów trasy</span><span class="sxs-lookup"><span data-stu-id="9ffa7-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="9ffa7-153">Przykład:</span><span class="sxs-lookup"><span data-stu-id="9ffa7-153">Example:</span></span>

    <span data-ttu-id="9ffa7-154">W tym miejscu żądanie like "/ 10/api/wartości" pomyślnie zwróci "Powodzenie: 10"</span><span class="sxs-lookup"><span data-stu-id="9ffa7-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="9ffa7-155">Podaj nazwę trasy domyślnej trasy atrybutu wykonując niektórych Konwencji, którą chcesz.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="9ffa7-156">Domyślnie trasami atrybutów nie tworzy automatycznie nazw dla tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="9ffa7-157">Zmodyfikuj szablon trasy tras atrybutów w jednym miejscu centralnej zakończyć w tabeli tras.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="9ffa7-158">Obsługa klienta interfejsu API sieci Web Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="9ffa7-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="9ffa7-159">Pakiet NuGet klienta interfejsu API sieci Web można teraz używać do implementacji logiki klienta interfejsu API sieci Web przeznaczonych dla systemu Windows Phone 8.1 lub z poziomu aplikacji uniwersalnej.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="9ffa7-160">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="9ffa7-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="9ffa7-161">W tej sekcji opisano znane problemy i fundamentalne zmiany w 2.2 interfejsu API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="9ffa7-162">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="9ffa7-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="9ffa7-163">Konstruktor modelu</span><span class="sxs-lookup"><span data-stu-id="9ffa7-163">Model builder</span></span>

<span data-ttu-id="9ffa7-164">Problem: Przeciążonej funkcji nie może być udostępniany jako FunctionImport</span><span class="sxs-lookup"><span data-stu-id="9ffa7-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="9ffa7-165">Jeśli ma funkcji przeciążenia 2 i są one również FunctionImport, jak pokazano poniżej następnie żąda powoduje ~/GetAllConventionCustomers(CustomerName={customerName}) System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="9ffa7-166">Obejście problemu: Obejście tego problemu jest dodanie przeciążenia funkcji jako FunctionImports.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="9ffa7-167">OData Routing</span><span class="sxs-lookup"><span data-stu-id="9ffa7-167">OData Routing</span></span>

<span data-ttu-id="9ffa7-168">Literały ciągów, które obejmują adres URL zakodowane ukośnika (% 2F), a backslash(%5C) spowodować błąd 404, gdy są one używane w ścieżkach zasobów OData.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="9ffa7-169">Na przykład literały ciągu można w ścieżkach zasobów OData jako parametry funkcji lub wartości klucza zestawów jednostek.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="9ffa7-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="9ffa7-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="9ffa7-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="9ffa7-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="9ffa7-172">Usługi otrzymują takich żądań hostów spowoduje un ucieczki Sekwencje te specjalne przed przekazaniem ich do środowiska wykonawczego interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="9ffa7-173">Chroni przed atakami podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="9ffa7-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="9ffa7-174">Powoduje to, że stos sieci Web API OData do zwrócenia błędu 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="9ffa7-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="9ffa7-175">Aby uniknąć tego błędu, klient należy używać sekwencje podwójnego anulowania kreski ułamkowej (% 252F) i ukośnika odwrotnego (% 255C).</span><span class="sxs-lookup"><span data-stu-id="9ffa7-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="9ffa7-176">Nie odbywa się to dla ciągów zapytania, takie jak /Employees? $filter = nazwa eq nazwy % 2F</span><span class="sxs-lookup"><span data-stu-id="9ffa7-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="9ffa7-177">**Zanotuj niezmieniony ukośniki ("/") i ukośników odwrotnych (") są niedozwolone w literałach ciągu ścieżki zasobu OData. Ukośniki powinna występować tylko jako separatorów ścieżek i ukośników odwrotnych nie powinny być wyświetlane w ścieżce zasobów OData w ogóle. (Oba są w niektórych części ciągu zapytania OData).**</span><span class="sxs-lookup"><span data-stu-id="9ffa7-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="9ffa7-178">Obejście problemu: Można zastąpić metody Parse DefaultODataPathHandler ucieczki ukośnika i ukośnika w literałach ciągu przed faktycznie analizowanie ich.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="9ffa7-179">Znajduje się przykład tej metody w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="9ffa7-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="9ffa7-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="9ffa7-181">[Kolejność]</span><span class="sxs-lookup"><span data-stu-id="9ffa7-181">[Queryable]</span></span>

<span data-ttu-id="9ffa7-182">Atrybut [Queryable] jest przestarzały.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="9ffa7-183">Nowe OData v3 aplikacje powinny używać **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="9ffa7-184">**ODataHttpConfigurationExtensions.EnableQuerySupport** — metoda rozszerzenia doda **EnableQueryAttribute** do kolekcji filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="9ffa7-185">Jeśli wszystkie kontrolery mają **[Queryable]** atrybutu wywoływania `config.EnableQuerySupport()` spowoduje, że **[Queryable]** atrybutu niepowodzenie</span><span class="sxs-lookup"><span data-stu-id="9ffa7-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="9ffa7-186">Zalecanym sposobem, aby rozwiązać ten problem jest zastąpić wszystkie wystąpienia **klasie QueryableAttribute** z **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="9ffa7-187">Inne obejście ma użyć poniższego kodu, konfiguracji interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="9ffa7-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="9ffa7-188">Atrybut routingu</span><span class="sxs-lookup"><span data-stu-id="9ffa7-188">Attribute Routing</span></span>

<span data-ttu-id="9ffa7-189">Problem: Wiązania modelu typu złożonego, zostanie nadany atrybut FromUri zachowuje się inaczej przy użyciu atrybutu routingu.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="9ffa7-190">Poniższe łącze służy do śledzenia problemu i ma również szczegółowe informacje o obejście tego problemu.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="9ffa7-191">Problem: Szkieletów MVC/API sieci Web do projektu z 5.2.0 pakietów pakiety powoduje 5.1.2 dla tych, które już nie istnieje w projekcie</span><span class="sxs-lookup"><span data-stu-id="9ffa7-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="9ffa7-192">Aktualizowanie pakietów NuGet dla programu ASP.NET MVC 5.2 nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak ASP.NET rusztowania lub szablonu projektu aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="9ffa7-193">Korzystają z poprzedniej wersji pakietów środowiska uruchomieniowego ASP.NET (np. 5.1.2 w wersji Update 2).</span><span class="sxs-lookup"><span data-stu-id="9ffa7-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="9ffa7-194">W związku z tym szkieletów ASP.NET zainstaluje poprzedniej wersji wymaganych pakietów (np. 5.1.2 w wersji Update 2) Jeśli nie są one już dostępne w projektach.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="9ffa7-195">Jednak funkcja szkieletów ASP.NET w programie Visual Studio 2013 RTM lub Update 1 nie powoduje zastąpienia najnowszych pakietów w projektach.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="9ffa7-196">Jeśli używasz szkieletów ASP.NET po zaktualizowaniu pakietów projektów sieci Web interfejsu API w wersji 2.2 lub ASP.NET MVC 5.2, upewnij się, że wersje interfejsu API sieci Web i ASP.NET MVC są spójne.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="9ffa7-197">Poprawki i aktualizacje funkcji pomocniczych</span><span class="sxs-lookup"><span data-stu-id="9ffa7-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="9ffa7-198">Ta wersja zawiera również kilka poprawek usterek i funkcji pomocniczych aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="9ffa7-199">Pełną listę można znaleźć:</span><span class="sxs-lookup"><span data-stu-id="9ffa7-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="9ffa7-200">5.2 pakietu</span><span class="sxs-lookup"><span data-stu-id="9ffa7-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="9ffa7-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="9ffa7-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="9ffa7-202">Pakietu Microsoft.AspNet.OData 5.2.1 zawiera aktualizacje zależności NuGet, ale nie poprawki.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="9ffa7-203">Dzięki tej aktualizacji nie jest już strict zależności na Microsoft.OData.Core 6.4.0, ale co można uaktualnić do dowolnej wersji między 6.4.0 i 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="9ffa7-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="9ffa7-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="9ffa7-205">W tej wersji wprowadzono zmiany dla zależności `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="9ffa7-206">Aby uzyskać więcej informacji na temat nowości w tej wersji `Json.NET`, zobacz [iniekcji zależności struktury Json.NET 6.0 wersji 4 - scalić JSON,](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9ffa7-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="9ffa7-207">Ta wersja nie ma inne nowe funkcje i poprawki błędów w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="9ffa7-208">Następnie zaktualizowano wszystkich innych pakietów zależnych, które firma Microsoft należeć do są zależne od tej nowej wersji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="9ffa7-209">Microsoft.AspNet.WebAPI 5.2.3 w wersji Beta</span><span class="sxs-lookup"><span data-stu-id="9ffa7-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="9ffa7-210">Informacje o wersji [tutaj](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ffa7-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="9ffa7-211">Ta wersja zawiera tylko poprawki błędów.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-211">This release contains only bug fixes.</span></span> <span data-ttu-id="9ffa7-212">Można użyć [to zapytanie](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) lista problemy rozwiązane w tej wersji.</span><span class="sxs-lookup"><span data-stu-id="9ffa7-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
