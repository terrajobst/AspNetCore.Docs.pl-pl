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
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566825"
---
<a name="whats-new-in-aspnet-web-api-22"></a>What's New in ASP.NET Web API 2.2
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano, co jest nowego w wersji 2.2 interfejsu API sieci Web ASP.NET.

- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Nowe funkcje w składniku ASP.NET Web API 2.2](#newf)

    - [Protokołu OData v4](#OData)
    - [Atrybut ulepszenia routingu](#ARI)
    - [Obsługa klienta interfejsu API sieci Web dla systemu Windows Phone 8.1](#phone)
- [Znane problemy i fundamentalne zmiany](#known-issues)
- [Poprawki błędów](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 w wersji Beta](#523)

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Wersjonowania semantycznego](http://semver.org/) specyfikacji. Najnowszy pakiet 2.2 interfejsu API sieci Web ASP.NET ma następującą wersję: "5.2.0". Można zainstalować lub zaktualizować te pakiety za pośrednictwem [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Wydanie obejmuje również odpowiedniego zlokalizowane pakiety na NuGet.

Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje o 2.2 interfejsu API sieci Web platformy ASP.NET są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nowe funkcje w składniku ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>Protokołu OData v4

Ta wersja dodaje obsługę protokołu OData v4. Aby uzyskać więcej informacji, zobacz [Web API OData v4 dokumentacji.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Oto niektóre najważniejsze funkcje i zmiany dotyczące protokołu OData v4:

- [Obsługa aliasów właściwości w modelu OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Obsługa ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute i ConcurrencyCheckAttribute w ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Zapewniają możliwość Podaj przyjazną tytuł dla akcji](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integracja z ODL UriParser
- Obsługa [wyliczenia](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [zawierania](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) i [pojedynczego wystąpienia](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Rzutowanie typów pierwotnych pomocy technicznej
- [Dodano obsługę funkcji OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Obsługa aliasów parametrów wywołania funkcji](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Obsługuje format camel case konwencję nazewnictwa w modelu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Obsługa cast() w $filter
- Obsługa Otwórz typu złożonego
- Usunięto EntitySetController i AsyncEntitySetController
- [Zmienione $link do $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Dodano obsługę routingu atrybutu](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Korzysta z bibliotek usługi OData Core 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Atrybut ulepszenia routingu

Atrybut routingu teraz udostępnia punkt rozszerzalności o nazwie IDirectRouteProvider, który umożliwia pełną kontrolę nad jak tras atrybutów są odnajdywane i skonfigurowane. IDirectRouteProvider jest odpowiedzialny za zapewnienie listę akcji i kontrolerów wraz z informacjami skojarzone trasy do określenia dokładnie konfiguracji routingu jest pożądany dla tych czynności. Implementacja IDirectRouteProvider można określić podczas wywoływania metody MapAttributes/MapHttpAttributeRoutes.

Dostosowywanie IDirectRouteProvider będzie najprostszym rozszerzając naszych Domyślna implementacja DefaultDirectRouteProvider. Ta klasa zawiera osobne wirtualnego metod, które można zmienić logikę wykrywania atrybuty, tworzenie wejścia dla trasy i odnajdywanie prefiks trasy i prefiks obszaru.

Poniżej przedstawiono niektóre przykłady co można zrobić to nowy punkt rozszerzeń:

1. Obsługa dziedziczenia atrybutów trasy

    Przykład:

    W tym miejscu żądanie like "/ 10/api/wartości" pomyślnie zwróci "Powodzenie: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Podaj nazwę trasy domyślnej trasy atrybutu wykonując niektórych Konwencji, którą chcesz. Domyślnie trasami atrybutów nie tworzy automatycznie nazw dla tras atrybutów.
3. Zmodyfikuj szablon trasy tras atrybutów w jednym miejscu centralnej zakończyć w tabeli tras.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Obsługa klienta interfejsu API sieci Web Windows Phone 8.1

Pakiet NuGet klienta interfejsu API sieci Web można teraz używać do implementacji logiki klienta interfejsu API sieci Web przeznaczonych dla systemu Windows Phone 8.1 lub z poziomu aplikacji uniwersalnej.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i fundamentalne zmiany w 2.2 interfejsu API sieci Web ASP.NET.

### <a name="odata-v4"></a>Protokołu OData v4

#### <a name="model-builder"></a>Konstruktor modelu

Problem: Przeciążonej funkcji nie może być udostępniany jako FunctionImport

Jeśli ma funkcji przeciążenia 2 i są one również FunctionImport, jak pokazano poniżej następnie żąda powoduje ~/GetAllConventionCustomers(CustomerName={customerName}) System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Obejście problemu: Obejście tego problemu jest dodanie przeciążenia funkcji jako FunctionImports.

#### <a name="odata-routing"></a>OData Routing

Literały ciągów, które obejmują adres URL zakodowane ukośnika (% 2F), a backslash(%5C) spowodować błąd 404, gdy są one używane w ścieżkach zasobów OData.

Na przykład literały ciągu można w ścieżkach zasobów OData jako parametry funkcji lub wartości klucza zestawów jednostek.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Usługi otrzymują takich żądań hostów spowoduje un ucieczki Sekwencje te specjalne przed przekazaniem ich do środowiska wykonawczego interfejsu API sieci Web. Chroni przed atakami podobne do poniższych:  
  
 http://www.contoso.com/..%2f..%2f/Windows/System32/cmd.exe?/c+dir+c:

Powoduje to, że stos sieci Web API OData do zwrócenia błędu 404 (nie znaleziono). Aby uniknąć tego błędu, klient należy używać sekwencje podwójnego anulowania kreski ułamkowej (% 252F) i ukośnika odwrotnego (% 255C). Nie odbywa się to dla ciągów zapytania, takie jak /Employees? $filter = nazwa eq nazwy % 2F

**Zanotuj niezmieniony ukośniki ("/") i ukośników odwrotnych (") są niedozwolone w literałach ciągu ścieżki zasobu OData. Ukośniki powinna występować tylko jako separatorów ścieżek i ukośników odwrotnych nie powinny być wyświetlane w ścieżce zasobów OData w ogóle. (Oba są w niektórych części ciągu zapytania OData).**

Obejście problemu: Można zastąpić metody Parse DefaultODataPathHandler ucieczki ukośnika i ukośnika w literałach ciągu przed faktycznie analizowanie ich. Znajduje się przykład tej metody w tym miejscu.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Kolejność]

Atrybut [Queryable] jest przestarzały. Nowe OData v3 aplikacje powinny używać **System.Web.Http.OData.EnableQueryAttribute**.

**ODataHttpConfigurationExtensions.EnableQuerySupport** — metoda rozszerzenia doda **EnableQueryAttribute** do kolekcji filtrów globalnych. Jeśli wszystkie kontrolery mają **[Queryable]** atrybutu wywoływania `config.EnableQuerySupport()` spowoduje, że **[Queryable]** atrybutu niepowodzenie

Zalecanym sposobem, aby rozwiązać ten problem jest zastąpić wszystkie wystąpienia **klasie QueryableAttribute** z **System.Web.Http.OData.EnableQueryAttribute**.

Inne obejście ma użyć poniższego kodu, konfiguracji interfejsu API sieci Web:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Atrybut routingu

Problem: Wiązania modelu typu złożonego, zostanie nadany atrybut FromUri zachowuje się inaczej przy użyciu atrybutu routingu.

Poniższe łącze służy do śledzenia problemu i ma również szczegółowe informacje o obejście tego problemu.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problem: Szkieletów MVC/API sieci Web do projektu z 5.2.0 pakietów pakiety powoduje 5.1.2 dla tych, które już nie istnieje w projekcie

Aktualizowanie pakietów NuGet dla programu ASP.NET MVC 5.2 nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak ASP.NET rusztowania lub szablonu projektu aplikacji sieci Web ASP.NET. Korzystają z poprzedniej wersji pakietów środowiska uruchomieniowego ASP.NET (np. 5.1.2 w wersji Update 2). W związku z tym szkieletów ASP.NET zainstaluje poprzedniej wersji wymaganych pakietów (np. 5.1.2 w wersji Update 2) Jeśli nie są one już dostępne w projektach. Jednak funkcja szkieletów ASP.NET w programie Visual Studio 2013 RTM lub Update 1 nie powoduje zastąpienia najnowszych pakietów w projektach. Jeśli używasz szkieletów ASP.NET po zaktualizowaniu pakietów projektów sieci Web interfejsu API w wersji 2.2 lub ASP.NET MVC 5.2, upewnij się, że wersje interfejsu API sieci Web i ASP.NET MVC są spójne.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Poprawki i aktualizacje funkcji pomocniczych

Ta wersja zawiera również kilka poprawek usterek i funkcji pomocniczych aktualizacji. Pełną listę można znaleźć:

- [5.2 pakietu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Pakietu Microsoft.AspNet.OData 5.2.1 zawiera aktualizacje zależności NuGet, ale nie poprawki. Dzięki tej aktualizacji nie jest już strict zależności na Microsoft.OData.Core 6.4.0, ale co można uaktualnić do dowolnej wersji między 6.4.0 i 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

W tej wersji wprowadzono zmiany dla zależności `Json.Net 6.0.4`. Aby uzyskać więcej informacji na temat nowości w tej wersji `Json.NET`, zobacz [iniekcji zależności struktury Json.NET 6.0 wersji 4 - scalić JSON,](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Ta wersja nie ma inne nowe funkcje i poprawki błędów w interfejsie API sieci Web. Następnie zaktualizowano wszystkich innych pakietów zależnych, które firma Microsoft należeć do są zależne od tej nowej wersji interfejsu API sieci Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 w wersji Beta

Informacje o wersji [tutaj](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Ta wersja zawiera tylko poprawki błędów. Można użyć [to zapytanie](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) lista problemy rozwiązane w tej wersji.
