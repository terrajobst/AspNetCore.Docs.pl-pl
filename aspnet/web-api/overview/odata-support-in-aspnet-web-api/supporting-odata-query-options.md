---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: "Obsługa opcje zapytania OData w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Obsługa opcje zapytania OData w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

OData definiuje parametry, które mogą służyć do modyfikowania zapytania OData. Klient wysyła te parametry w ciągu zapytania identyfikatora URI żądania. Na przykład aby sortować wyniki, klient używa parametru $orderby:

`http://localhost/Products?$orderby=Name`

Specyfikację OData wywołuje te parametry *opcje kwerendy*. Można włączyć opcji zapytania OData dla każdego kontrolera interfejsu API sieci Web w projekcie &#8212; kontroler nie musi być punkt końcowy OData. Zapewnia to wygodny sposób, aby dodać funkcje, takie jak filtrowanie i sortowanie do dowolnej aplikacji interfejsu API sieci Web.

Przed włączeniem opcje zapytania, przeczytaj temat [wskazówki dotyczące zabezpieczeń OData](odata-security-guidance.md).

- [Włączanie opcji zapytania OData](#enable)
- [Przykładowe zapytania](#examples)
- [Stronicowania obsługiwanego przez serwer](#server-paging)
- [Ograniczanie opcje zapytania](#limiting_query_options)
- [Bezpośrednie wywoływanie opcje zapytania](#ODataQueryOptions)
- [Sprawdzanie poprawności zapytań](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Włączanie opcji zapytania OData

Interfejs API sieci Web obsługuje następujące opcje zapytania OData:

| Opcja | Opis |
| --- | --- |
| Rozwiń węzeł $ | Rozwija wbudowanego powiązanych jednostek. |
| $filter | Filtruje wyniki, w oparciu o warunek typu Boolean. |
| $inlinecount | Określa, że serwer do uwzględnienia w odpowiedzi łączna liczba zgodnych jednostek. (Przydatne w przypadku stronicowania po stronie serwera.) |
| $orderby | Wyniki są sortowane. |
| $select | Wybiera właściwości, które do uwzględnienia w odpowiedzi. |
| $skip | Pomija pierwsze n wyniki. |
| $top | Zwraca pierwsze n wyniki. |

Aby użyć opcji zapytania OData, należy je jawnie włączyć. Możesz je włączyć globalnie dla całej aplikacji lub włączyć je do określonych kontrolerów lub określonych akcji.

Aby włączyć globalnie opcje zapytania OData, należy wywołać **EnableQuerySupport** na **HttpConfiguration** klasy podczas uruchamiania:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport** metoda zapewnia opcje zapytania globalnie dla każdej akcji kontrolera, która zwraca **IQueryable** typu. Jeśli nie chcesz, aby opcje zapytania włączony dla całej aplikacji, można włączyć je do akcji kontrolera określonej przez dodanie **[Queryable]** atrybut do metody akcji.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Przykładowe zapytania

W tej sekcji przedstawiono typy zapytań, które są możliwe przy użyciu opcji zapytania OData. Aby uzyskać szczegółowe informacje na temat opcji zapytania, zapoznaj się z dokumentacją OData, w [www.odata.org](http://www.odata.org/).

Informacje o $Rozwiń i $select, zobacz [przy użyciu $select, rozwinąć $ i $value w programie ASP.NET Web API OData](using-select-expand-and-value.md).

**Stronicowania obsługiwanego przez klienta**

Dla zestawów duża jednostka klienta mogą chcieć ograniczyć liczbę wyników. Na przykład klienta mogą być wyświetlane wpisy 10 jednocześnie, wraz z łączami "dalej" do pobrania następnej strony wyników. Aby to zrobić, klient używa opcji $top i $skip.

`http://localhost/Products?$top=10&$skip=20`

Opcja $top zapewnia maksymalna liczba wpisów do zwrócenia, a opcja $skip zapewnia liczba wpisów, aby pominąć. Poprzedni przykład pobiera wpisy 21 do 30.

**Filtrowanie**

Opcji $filter umożliwia klientowi filtrować wyniki, stosując wyrażenie logiczne. Wyrażenia filtru są bardzo zaawansowane; obejmują one operatorów logicznych i arytmetyczne, ciąg funkcji i funkcji daty.

| Zwróć wszystkie produkty z kategorii jest równa "Toys". | `http://localhost/Products?$filter=Category`EQ "Toys" |
| --- | --- |
| Zwróć wszystkie produkty z cen mniej niż 10. | `http://localhost/Products?$filter=Price`lt 10 |
| Operatory logiczne: zwraca wszystkie produkty gdzie ceny > = 5 i cen < = 15. | `http://localhost/Products?$filter=Price`GE 5 i cen le 15 |
| Funkcje ciągów: zwraca wszystkie produkty z "zz" w nazwie. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Funkcje daty: zwraca wszystkie produkty z ReleaseDate po 2005. | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**Sortowanie**

Sortowanie wyników, użyj filtru $orderby.

| Sortuj według ceny. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Sortuj według cen w porządku malejącym (najwyższy malejąco). | `http://localhost/Products?$orderby=Price desc` |
| Sortuj według kategorii, a następnie sortować cen malejąco według kategorii. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Stronicowania obsługiwanego przez serwer

Jeśli baza danych zawiera miliony rekordów, nie chcesz wysłać ich wszystkich w jednym ładunku. Aby tego uniknąć, serwer może ograniczyć liczbę wpisów, które wysyła w pojedynczą odpowiedź. Aby włączyć stronicowania na serwerze, należy ustawić **PageSize** właściwości w **Queryable** atrybutu. Wartość jest maksymalna liczba wpisów do zwrócenia.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Jeśli kontroler zwraca OData format, treść odpowiedzi będzie zawierać łącze do następnej strony danych:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Klient może używać to łącze do pobrania następnej strony. Aby dowiedzieć się liczba wpisów w zestawie wyników, klienta można ustawić o wartości opcji zapytania $inlinecount "allpages".

`http://localhost/Products?$inlinecount=allpages`

Wartość "allpages" informuje serwer do uwzględnienia w odpowiedzi łączna liczba:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Łącza do następnej strony i liczbę inlinecount wymaga OData. Przyczyną jest to, że OData definiuje specjalne pola w treści odpowiedzi, aby pomieścić link i liczba.


W przypadku formatów OData z systemem innym niż jest nadal możliwe do obsługi liczby łącza i wbudowanego następnej strony zawijania wyniki zapytania w **element PageResult&lt;T&gt;**  obiektu. Jednak wymaga nieco więcej kodu. Oto przykład:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Oto przykład JSON odpowiedzi:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Ograniczanie opcje zapytania

Opcje zapytania nadaj klienta dużo kontrolę nad zapytania, który jest uruchamiany na serwerze. W niektórych przypadkach można ograniczyć dostępne opcje ze względów bezpieczeństwa ani wydajności. **[Queryable]** atrybutu są niektóre wbudowane właściwości dla tego. Oto kilka przykładów.

Zezwalaj tylko $skip ani $top, do obsługi stronicowania i nic innego:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Kolejność tylko przez niektóre właściwości zapobiec sortowania dla właściwości, które nie są indeksowane w bazie danych:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Zezwalaj na funkcję logicznych "eq", ale inne funkcje logiczne:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Nie zezwalaj na wszystkie operatory arytmetyczne:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Można ograniczyć opcje globalnie tworząc **klasie QueryableAttribute** wystąpienia i przekazanie jej do **EnableQuerySupport** funkcji:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Bezpośrednie wywoływanie opcje zapytania

Zamiast **[Queryable]** atrybutu, opcji zapytania można wywoływać bezpośrednio w kontrolerze. Aby to zrobić, należy dodać **ODataQueryOptions** parametru do metody kontrolera. W takim przypadku nie trzeba **[Queryable]** atrybutu.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Interfejs API sieci Web wypełnia **ODataQueryOptions** z identyfikatora URI z ciągu zapytania. Aby zastosować zapytanie, należy przekazać **IQueryable** do **metody ApplyTo** metody. Metoda zwraca innego **IQueryable**.

Dla zaawansowanych scenariuszy, jeśli nie masz **IQueryable** zapytanie do dostawcy, można sprawdzić **ODataQueryOptions** i odpowiednie opcje zapytania do innego formularza. (Na przykład, zobacz RaghuRam Nadiminti blogu [zapytań tłumaczenia OData do HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), który obejmuje również [próbki](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Sprawdzanie poprawności zapytań

**[Queryable]** atrybutu weryfikuje zapytanie przed jej wykonanie. Krok sprawdzania poprawności jest wykonywane w **QueryableAttribute.ValidateQuery** metody. Można również dostosować procesu weryfikacji.

Zobacz też [wskazówki dotyczące zabezpieczeń OData](odata-security-guidance.md).

Najpierw zastąpienie jednego modułu sprawdzania poprawności klasy czyli zdefiniowane w **Web.Http.OData.Query.Validators** przestrzeni nazw. Na przykład następujące klasy modułu sprawdzania poprawności wyłącza opcję "opis" dla opcji $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Podklasy **[Queryable]** atrybutu, aby zastąpić **ValidateQuery** metody.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Następnie ustaw atrybut niestandardowy albo globalnie lub na kontrolerze:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Jeśli używasz **ODataQueryOptions** bezpośrednio, ustaw modułu sprawdzania poprawności opcji:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
