---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Co nowego we wzorcu ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 51b01c4b9ee8d12e4e3925193e308d3ca6c5f2b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377444"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Co nowego we wzorcu ASP.NET Web API 2.2
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano what's new for ASP.NET Web API 2.2.

- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Nowe funkcje we wzorcu ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Atrybut ulepszenia routingu](#ARI)
    - [Obsługa klienta interfejsu API sieci Web dla systemu Windows Phone 8.1](#phone)
- [Znane problemy i fundamentalne zmiany](#known-issues)
- [Poprawki błędów](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Beta Microsoft.AspNet.WebAPI 5.2.3](#523)

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Semantic Versioning](http://semver.org/) specyfikacji. Najnowszy pakiet ASP.NET Web API 2.2 ma następującą wersję: "5.2.0". Możesz zainstalować lub zaktualizować te pakiety przy użyciu [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Wersja ta oferuje również odpowiedni zlokalizowanych pakietów dla narzędzia NuGet.

Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

W samouczkach i innych informacji na temat platformy ASP.NET Web API 2.2 są dostępne w witrynie sieci web platformy ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nowe funkcje we wzorcu ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>Protokołu OData v4

W tej wersji dodano obsługę protokołu OData 4. Aby uzyskać więcej informacji, zobacz [Web API OData v4 dokumentacji.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Poniżej przedstawiono niektóre najważniejsze funkcje i zmiany dotyczące protokołu OData v4:

- [Obsługa aliasów właściwości w modelu OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Obsługa ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute i ConcurrencyCheckAttribute w ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Zapewnia możliwość dostarczania tytuł przyjazne dla akcji](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integracja z usługą ODL UriParser
- Obsługa [wyliczenia](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [zawierania](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) i [pojedynczego wystąpienia](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Rzutowanie typów pierwotnych pomocy technicznej
- [Dodano obsługę funkcji OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Obsługa aliasów parametrów wywołania funkcji](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Obsługuje pisane konwencji nazewnictwa przypadków w modelu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Obsługa cast() w $filter
- Obsługa Otwórz typ złożony
- Usunięto EntitySetController i AsyncEntitySetController
- [Zmienione $link do $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Dodanie obsługi routingu atrybutu](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Korzysta z bibliotek usługi OData Core 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Atrybut ulepszenia routingu

Teraz trasowanie atrybutów udostępnia punkt rozszerzalności o nazwie IDirectRouteProvider, która umożliwia pełną kontrolę nad jak tras atrybutów są odnajdywane i skonfigurowane. IDirectRouteProvider jest odpowiedzialny za zapewnienie listę akcji i kontrolerów wraz z informacjami skojarzone trasy do określenia dokładnie wymaganego konfigurację routingu dla tych akcji. Implementacja IDirectRouteProvider może być określona podczas wywoływania MapAttributes/MapHttpAttributeRoutes.

Dostosowywanie IDirectRouteProvider będzie najprostszym, rozszerzając nasze Domyślna implementacja DefaultDirectRouteProvider. Ta klasa udostępnia oddzielne możliwym do zastąpienia metod wirtualnych, aby zmienić logikę odnajdywania atrybutów, tworząc wejścia dla trasy i odnajdywanie prefiks trasy i prefiks obszaru.

Poniżej przedstawiono kilka przykładów, co można zrobić za pomocą tego nowego punktu rozszerzalności:

1. Obsługa dziedziczenia atrybuty trasy

    Przykład:

    W tym miejscu żądania like "/ api/wartości/10" pomyślnie zwróci "SUKCES: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Podaj nazwę trasy domyślne trasy atrybutu postępując zgodnie z niektórych Konwencji, którą chcesz. Domyślnie trasowanie atrybutów nie tworzy automatycznie nazw dla tras atrybutów.
3. Przed znajdą się one w tabeli tras, należy zmodyfikować szablon trasy tras atrybutów w jednej centralnej lokalizacji.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Obsługa klienta interfejsu API sieci Web dla Windows Phone 8.1

Można teraz używać pakietu NuGet klienta interfejsu API sieci Web zaimplementować logikę klienta interfejsu API sieci Web podczas określania wartości docelowej systemu Windows Phone 8.1 lub z poziomu aplikacji uniwersalnej.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i przełomowe zmiany w programie ASP.NET Web API 2.2.

### <a name="odata-v4"></a>Protokołu OData v4

#### <a name="model-builder"></a>Konstruktor modelu

Problem: Przeciążonych funkcji nie może być udostępniany jako element FunctionImport

Jeśli istnieją 2 funkcje przeciążone, a następnie są one również element FunctionImport, jak pokazano poniżej następnie żądanie ~/GetAllConventionCustomers(CustomerName={customerName}) skutkuje System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Obejście: Sposobu obejścia tego problemu jest dodanie przeciążenia funkcji jako FunctionImports.

#### <a name="odata-routing"></a>Routing protokołu OData

Literały ciągów, które obejmują adres URL zakodowane ukośnika (% 2F), a backslash(%5C) spowodować błąd 404, gdy są one używane w ścieżkach zasobów OData.

Na przykład literałów ciągów może służyć w ścieżkach zasobów OData jako wartości kluczy zestawy jednostek lub parametrów funkcji.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Usługi otrzymywać takie żądania hosty będą un-znak ucieczki tych ucieczki sekwencji przed przekazaniem ich do internetowego interfejsu API środowiska uruchomieniowego. Chroni to przed atakami, jak pokazano poniżej:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

Powoduje to stosu Web API OData zwrócić błąd 404 (nie znaleziono). Aby uniknąć tego błędu, klienta należy używać podwójnego unikowe kreski ułamkowej (% 252F) i kreski ułamkowej odwróconej (% 255C). Nie jest to realizowane dla ciągów zapytań, takich jak /Employees? $filter = nazwa eq "Nazwa % 2F"

**Należy pamiętać, niezmieniony ukośniki ("/") i ukośniki odwrotne (") nie są dozwolone w literałów ciągów ścieżkę zasobów OData. Ukośniki powinna zostać wyświetlona tylko jako separatorami ścieżki i ukośników odwrotnych nie powinien pojawić się w ścieżkę zasobów OData w ogóle. (Oba są użyteczne w niektórych obszarach ciągu zapytania OData).**

Obejście: Można zastąpić metody Parse DefaultODataPathHandler jako znak ucieczki ukośnika i kreski ułamkowej odwróconej w literałach ciągu przed faktycznie analizowanie ich. Przykładem tego podejścia, w tym miejscu można znaleźć.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Odpytywalny]

Atrybut [Queryable] jest przestarzały. Nowe OData v3 aplikacje powinny używać **System.Web.Http.OData.EnableQueryAttribute**.

**ODataHttpConfigurationExtensions.EnableQuerySupport** — metoda rozszerzenia jest teraz dodaje **EnableQueryAttribute** do kolekcji filtrów globalnych. Jeśli ma żadnych kontrolerów **[Queryable]** atrybutu i wywoływania `config.EnableQuerySupport()` spowoduje, że **[Queryable]** atrybutu, aby zakończyć się niepowodzeniem

Zalecanym sposobem, aby rozwiązać ten problem jest zastąpić wszystkie wystąpienia **klasie QueryableAttribute** z **System.Web.Http.OData.EnableQueryAttribute**.

Jeszcze inne obejście jest Użyj poniższego kodu, w konfiguracji interfejsu API sieci Web:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Routing atrybutów

Problem: Wiązanie modelu typu złożonego, który zostanie nadany atrybut FromUri zachowuje się inaczej korzystając z trasami atrybutów.

Poniższe łącze służy do śledzenia problemu i ma również szczegółowe informacje o obejście tego problemu.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problem: Interfejs API sieci Web i MVC szkieletu do projektu przy użyciu 5.2.0 pakietów skutkuje 5.1.2 pakietów dla tych, które jeszcze nie istnieją w projekcie

Trwa aktualizowanie pakietów NuGet dla platformy ASP.NET MVC 5.2 nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak funkcja tworzenia szkieletu ASP.NET lub szablon projektu aplikacji sieci Web ASP.NET. Używają poprzedniej wersji pakiety środowiska uruchomieniowego platformy ASP.NET (np. 5.1.2 w wersji Update 2). Co w efekcie funkcja tworzenia szkieletu ASP.NET zainstaluje poprzedniej wersji (np. 5.1.2 w wersji Update 2) wymagane pakiety, jeśli nie są jeszcze dostępne w projektach. Funkcja tworzenia szkieletu ASP.NET w Visual Studio 2013 RTM i Update 1 nie zastąpi jednak najnowszych pakietów w swoich projektach. Jeśli używasz funkcja tworzenia szkieletu ASP.NET po zaktualizowaniu pakietów projektów sieci Web API 2.2 lub platformy ASP.NET MVC 5.2, upewnij się, że wersje interfejsu API sieci Web platformy ASP.NET MVC są spójne.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Poprawki i aktualizacje funkcji pomocniczych

Ta wersja zawiera także kilka poprawek usterek i funkcji drobne aktualizacje. Pełną listę można znaleźć:

- [5.2 pakietu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Pakiet Microsoft.AspNet.OData 5.2.1 zawiera aktualizacje zależności NuGet, ale nie poprawki. Dzięki tej aktualizacji nie jest już strict zależności na Microsoft.OData.Core 6.4.0, ale jeden można uaktualnić do dowolnej wersji między 6.4.0 i 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

W tej wersji wprowadzono zmiany zależności dla `Json.Net 6.0.4`. Aby uzyskać więcej informacji na temat nowości w tej wersji `Json.NET`, zobacz [Json.NET 6.0 Release 4 — scalanie kodu JSON, wstrzykiwanie zależności](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). W tej wersji nie zawarto żadnych nowych funkcji ani poprawek błędów w interfejsie API sieci Web. Zaktualizowaliśmy wszystkie inne zależne pakiety, możemy do są zależne od tej nowej wersji interfejsu API sieci Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Beta Microsoft.AspNet.WebAPI 5.2.3

Informacje o wersji [tutaj](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Ta wersja zawiera tylko poprawki. Możesz użyć [to zapytanie](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) Aby wyświetlić listę problemów rozwiązanych w tej wersji.
