---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Obsługa opcji zapytań protokołu OData, we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 029a791074a5ab88f1c6d83311e88fc9f726fe6f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814944"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Obsługa opcji zapytań protokołu OData we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Usługa OData definiuje parametry, które mogą służyć do modyfikowania zapytanie OData. Klient wysyła te parametry do ciągu zapytania identyfikatora URI żądania. Na przykład aby posortować wyniki, klient używa parametru $orderby:

`http://localhost/Products?$orderby=Name`

Specyfikacja protokołu OData wywołuje te parametry *opcje kwerendy*. Można włączyć opcji zapytania OData dla każdego kontrolera interfejsu API sieci Web w projekcie &#8212; kontrolera musi być punktu końcowego OData. Zapewnia wygodny sposób dodawania funkcji, takich jak filtrowanie i sortowanie do dowolnej aplikacji interfejsu API sieci Web.

Przed włączeniem opcji zapytań, przeczytaj temat [wskazówki dotyczące zabezpieczeń OData](odata-security-guidance.md).

- [Włączenie opcji zapytań protokołu OData](#enable)
- [Przykładowe zapytania](#examples)
- [Stronicowania opartych na serwerze](#server-paging)
- [Ograniczanie opcje kwerendy](#limiting_query_options)
- [Bezpośrednie wywoływanie opcje kwerendy](#ODataQueryOptions)
- [Sprawdzanie poprawności zapytań](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Włączenie opcji zapytań protokołu OData

Internetowy interfejs API obsługuje następujące opcje zapytania OData:

| Opcja | Opis |
| --- | --- |
| $expand | Rozszerza wbudowany powiązanych jednostek. |
| $filter | Służy do przefiltrowania wyników, w oparciu o warunek logiczny. |
| $inlinecount | Informuje serwer, które mają zostać objęte łączna liczba zgodnych jednostek odpowiedzi. (Przydatne w przypadku stronicowania po stronie serwera.) |
| $orderby | Sortuje wyniki. |
| $select | Wybiera właściwości do uwzględnienia w odpowiedzi. |
| $skip | Pomija pierwszych n wyników. |
| $top | Zwraca pierwsze n wyniki. |

Aby użyć opcji zapytania OData, należy je jawnie włączyć. Można je włączyć ją globalnie dla całej aplikacji, lub włączyć je do określonych kontrolery lub konkretne akcje.

Aby włączyć ją globalnie opcje zapytania OData, należy wywołać **EnableQuerySupport** na **HttpConfiguration** klasy przy uruchamianiu:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport** metoda zapewnia opcje zapytania globalnie dla każdej akcji kontrolera, które zwraca **IQueryable** typu. Jeśli nie chcesz, aby opcje zapytania włączone dla całej aplikacji, można je włączyć dla akcji określonego kontrolera, dodając **[Queryable]** atrybutu do metody akcji.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Przykładowe zapytania

W tej sekcji przedstawiono typów kwerend, które są możliwe przy użyciu opcji zapytania OData. Aby uzyskać szczegółowe informacje na temat opcji zapytania, zapoznaj się z dokumentacją OData na [www.odata.org](http://www.odata.org/).

W przypadku informacji na temat $Rozwiń i $select, zobacz [przy użyciu $select, $expand and $value w protokole OData składnika Web API platformy ASP.NET](using-select-expand-and-value.md).

**Stronicowania opartych na klienta**

Dla zestawów dużych jednostek klient chcieć ograniczyć liczbę wyników. Na przykład klient może wyświetlać 10 wpisów w czasie, wraz z łączami "dalej", aby uzyskać następnej strony wyników. Aby to zrobić, klient używa opcji $top i $skip.

`http://localhost/Products?$top=10&$skip=20`

Opcja $top zapewnia maksymalną liczbę zwracanych pozycji, a opcja $skip zapewnia liczba wpisów do pominięcia. Poprzedni przykład pobiera wpisy 21 do 30.

**Filtrowanie**

Opcja $filter umożliwia klientowi filtrowanie wyników za pomocą wyrażenia logicznego. Wyrażenia filtru są bardzo wydajny; obejmują one operatory logiczne i arytmetyczne, parametry funkcji i funkcji daty.

| Zwróć wszystkie produkty z kategorii jest równa "Zabawki". | `http://localhost/Products?$filter=Category` EQ "Zabawki" |
| --- | --- |
| Zwróć wszystkie produkty z ceną mniej niż 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Operatory logiczne: zwraca wszystkie produkty których cena > = 5, a cena < = 15. | `http://localhost/Products?$filter=Price` GE 5 i le ceny 15 |
| Funkcje ciągów: zwraca wszystkie produkty z "zz" w nazwie. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Funkcje daty: zwraca wszystkie produkty z ReleaseDate po 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Sortowanie**

Aby posortować wyniki, użyj filtru $orderby.

| Sortuj według ceny. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Sortuj według ceny w kolejności (od najwyższej do najniższej). | `http://localhost/Products?$orderby=Price desc` |
| Sortuj według kategorii, a następnie Sortuj według ceny w porządku malejącym w kategoriach. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Stronicowania opartych na serwerze

Jeśli baza danych zawiera miliony rekordów, nie chcesz wysyłać je wszystkie w jednym ładunku. Aby tego uniknąć, serwer można ograniczyć liczbę wpisów, które wysyła w jednej odpowiedzi. Aby włączyć stronicowania serwera, należy ustawić **PageSize** właściwość **Queryable** atrybutu. Wartość jest maksymalna liczba wpisów do zwrócenia.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Jeśli kontroler zwraca OData format, treść odpowiedzi będzie zawierać link do następnej strony danych:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Klient może używać tego łącza do pobrania następnej strony. Aby dowiedzieć się, łączna liczba wpisów w zestawie wyników, klienta można ustawić za pomocą wartości opcji zapytania $inlinecount "allpages".

`http://localhost/Products?$inlinecount=allpages`

Wartość "allpages" informuje serwera, które mają zostać objęte łączna liczba odpowiedzi:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Następna strona łącza i liczbę inlinecount wymaga formatu OData. Przyczyną jest to, że usługa OData definiuje specjalne pola w treści odpowiedzi na potrzeby przechowywania łącze i liczba.


W przypadku formatów OData bez jest nadal możliwe do obsługi liczby linków i wbudowane Następna strona opakowując wyniki zapytania w **PageResult&lt;T&gt;**  obiektu. Jednak wymaga nieco więcej kodu. Oto przykład:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Oto przykład odpowiedź w formacie JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Ograniczanie opcje kwerendy

Opcje zapytania zaoferować klientowi produkt szczegółową kontrolę nad zapytanie, które jest uruchamiane na serwerze. W niektórych przypadkach możesz chcieć ograniczyć opcje dostępne ze względów bezpieczeństwa ani wydajności. **[Queryable]** atrybut ma niektórych wbudowanych właściwości dla tego. Poniżej przedstawiono kilka przykładów.

Zezwalaj na tylko $skip i $top, do obsługi stronicowania i od niczego więcej:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Zezwalaj na porządkowanie tylko przez określonych właściwości zapobiec sortowanie według właściwości, które nie są indeksowane w bazie danych:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Zezwalaj na funkcję logiczną "eq", ale inne funkcje logiczne:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Nie zezwalaj na wszystkie operatory arytmetyczne:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Opcje można ograniczyć globalnie tworząc **klasie QueryableAttribute** wystąpienia i przekazanie jej do **EnableQuerySupport** funkcji:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Bezpośrednie wywoływanie opcje kwerendy

Zamiast używania **[Queryable]** atrybut, można wywołać opcje zapytania bezpośrednio w kontrolerze. Aby to zrobić, Dodaj **ODataQueryOptions** parametr do metody kontrolera. W takim przypadku nie ma potrzeby **[Queryable]** atrybutu.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Internetowy interfejs API wypełnia **ODataQueryOptions** ciąg zapytania z identyfikatora URI. Stosuje zapytanie, polega na przekazaniu **IQueryable** do **ApplyTo** metody. Metoda ta zwraca innego **IQueryable**.

W przypadku zaawansowanych scenariuszy, jeśli nie masz **IQueryable** dostawcę zapytań, można sprawdzić **ODataQueryOptions** i tłumaczyć opcje zapytania do innego formularza. (Na przykład, zobacz wpis w blogu RaghuRam Nadiminti [tłumaczenie zapytań protokołu OData do HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), który zawiera również [przykładowe](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Sprawdzanie poprawności zapytań

**[Queryable]** atrybut weryfikuje zapytanie przed jej wykonanie. W kroku sprawdzania poprawności jest wykonywane w **QueryableAttribute.ValidateQuery** metody. Można również dostosować proces sprawdzania poprawności.

Zobacz też [wskazówki dotyczące zabezpieczeń OData](odata-security-guidance.md).

Najpierw zastąpienie jeden moduł weryfikacji to znaczy klas zdefiniowanych w **Web.Http.OData.Query.Validators** przestrzeni nazw. Na przykład następujące klasy modułu sprawdzania poprawności wyłącza opcję "desc" dla opcji $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Podklasy **[Queryable]** atrybutu, aby zastąpić **ValidateQuery** metody.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Następnie ustaw niestandardowy atrybut albo globalnie lub na kontrolerze:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Jeśli używasz **ODataQueryOptions** bezpośrednio, ustaw modułu sprawdzania poprawności na temat opcji:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
