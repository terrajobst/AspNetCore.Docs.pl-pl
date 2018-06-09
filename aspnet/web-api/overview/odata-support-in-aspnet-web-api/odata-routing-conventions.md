---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Konwencje tras w składniku ASP.NET Web API 2 Odata | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano konwencje tras, które używa interfejsu API sieci Web dla punktów końcowych OData.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "28037324"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Konwencje tras w składniku ASP.NET Web API 2 Odata
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> W tym artykule opisano konwencje tras, które używa interfejsu API sieci Web dla punktów końcowych OData.


Gdy interfejs API sieci Web pobiera żądanie OData, mapuje żądanie nazwy kontrolera i nazwy akcji. Mapowanie jest na podstawie metody HTTP i identyfikator URI. Na przykład `GET /odata/Products(1)` mapuje `ProductsController.GetProduct`.

W części 1 w tym artykule opisano I wbudowane konwencje tras OData. Konwencje te zostały zaprojektowane specjalnie dla punktów końcowych OData i zastępują one domyślny system routingu interfejsu API sieci Web. (Zastępuje się stanie, jeśli wywołujesz **MapODataRoute**.)

W części 2 I przedstawiają sposób dodawania niestandardowych Konwencji tras. Obecnie wbudowane konwencje nie obejmują cały zakres URI OData, ale można rozszerzyć je, aby obsłużyć dodatkowe przypadki.

- [Wbudowane Konwencji tras](#conventions)
- [Konwencje tras niestandardowych](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Wbudowane Konwencji tras

Przed I opisano konwencje tras OData w składniku Web API, warto poznać identyfikatory URI OData. [Identyfikatora URI OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) obejmuje:

- Element główny usługi
- Ścieżka zasobu
- Opcje zapytania

![](odata-routing-conventions/_static/image1.png)

W przypadku routingu ważnym elementem jest ścieżka zasobu. Ścieżka zasobu jest podzielona na segmenty. Na przykład `/Products(1)/Supplier` ma trzy segmenty:

- `Products` Odwołanie do zestawu jednostek o nazwie "Produktów".
- `1` jest kluczem jednostki, wybierając pojedynczej jednostki z zestawu.
- `Supplier` jest właściwością nawigacji, który wybiera powiązanej jednostki.

Dlatego tej ścieżki wybiera się dostawcę produktu 1.

> [!NOTE]
> Segmenty ścieżki OData nie zawsze pasują do segmentów identyfikatora URI. Na przykład "1" jest traktowany jako segment ścieżki.


**Nazwy kontrolera.** Nazwa kontrolera jest zawsze pochodną jednostki na katalog główny ścieżki zasobu. Na przykład, jeśli ścieżka zasobu jest `/Products(1)/Supplier`, interfejsu API sieci Web wygląda na kontrolerze o nazwie `ProductsController`.

**Nazwy akcji.** Nazwy akcji są uzyskiwane z segmentów ścieżki plus modelu entity data model (EDM) wymienionych w poniższych tabelach. W niektórych przypadkach dostępne są dwie opcje dla nazwy akcji. Na przykład "Get" lub &quot;GetProducts&quot;.

**Wykonywanie zapytania jednostek**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład działania |
| --- | --- | --- | --- |
| Pobierz /entityset | / Produktów | GetEntitySet lub Get | GetProducts |
| Pobierz /entityset(key) | /Products(1) | GetEntityType lub Get | GetProduct |
| Pobierz /entityset (klucz) / rzutowania | /Products(1)/Models.Book | GetEntityType lub Get | GetBook |

Aby uzyskać więcej informacji, zobacz [Tworzenie punktu końcowego OData tylko do odczytu](odata-v3/creating-an-odata-endpoint.md).

**Tworzenie, aktualizowanie i usuwanie jednostek**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład działania |
| --- | --- | --- | --- |
| POST /entityset | / Produktów | PostEntityType lub Post | PostProduct |
| Umieść /entityset(key) | /Products(1) | PutEntityType lub Put | PutProduct |
| Umieść /entityset (klucz) / rzutowania | /Products(1)/Models.Book | PutEntityType lub Put | PutBook |
| POPRAWKA /entityset(key) | /Products(1) | PatchEntityType lub poprawki | PatchProduct |
| POPRAWKA /entityset (klucz) / rzutowania | /Products(1)/Models.Book | PatchEntityType lub poprawki | PatchBook |
| Usuń /entityset(key) | /Products(1) | DeleteEntityType lub Delete | DeleteProduct |
| Usuń /entityset (klucz) / rzutowania | /Products(1)/Models.Book | DeleteEntityType lub Delete | DeleteBook |

**Wykonanie zapytania dotyczącego właściwości nawigacji**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład działania |
| --- | --- | --- | --- |
| /Entityset GET (klucz) / nawigacji | / (1) lub dostawcy produktów | GetNavigationFromEntityType lub GetNavigation | GetSupplierFromProduct |
| Pobierz /entityset (klucz) / rzutowania/nawigacji | /Products(1)/Models.Book/Author | GetNavigationFromEntityType lub GetNavigation | GetAuthorFromBook |

Aby uzyskać więcej informacji, zobacz [Praca z relacjami jednostek](odata-v3/working-with-entity-relations.md).

**Tworzenie i usuwanie łącza**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji |
| --- | --- | --- |
| /Entityset POST (klucz) / $links/nawigacji | / (1) / $produktów łącza lub dostawcy | CreateLink |
| Umieść /entityset (klucz) / $links/nawigacji | / (1) / $produktów łącza lub dostawcy | CreateLink |
| Usuń /entityset (klucz) / $links/nawigacji | / (1) / $produktów łącza lub dostawcy | DeleteLink |
| Usuń /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Aby uzyskać więcej informacji, zobacz [Praca z relacjami jednostek](odata-v3/working-with-entity-relations.md).

**Właściwości**

*Wymaga składnika Web API 2*

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład działania |
| --- | --- | --- | --- |
| /Entityset GET (klucz) / właściwości | /Products(1)/Name | GetPropertyFromEntityType lub GetProperty | GetNameFromProduct |
| Pobierz /entityset (klucz) / rzutowania lub właściwości | /Products(1)/Models.Book/Author | GetPropertyFromEntityType lub GetProperty | GetTitleFromBook |

**Akcje**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład działania |
| --- | --- | --- | --- |
| /Entityset POST (klucz) / działania | / (1) / szybkość produktów | ActionNameOnEntityType lub nazwa akcji | RateOnProduct |
| OPUBLIKUJ /entityset (klucz) / rzutowania/działania | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType lub nazwa akcji | CheckOutOnBook |

Aby uzyskać więcej informacji, zobacz [akcji OData](odata-v3/odata-actions.md).

**Podpisy — metoda**

Poniżej przedstawiono niektóre zasady podpisy metod:

- Jeśli ścieżka zawiera klucz, akcja ma parametr o nazwie *klucza*.
- Jeśli ścieżka zawiera klucz do właściwości nawigacji, akcja ma parametr o nazwie *relatedKey*.
- Dekoracji *klucza* i *relatedKey* parametrów z **[FromODataUri]** parametru.
- POST i PUT żądań zająć parametr typu jednostki.
- W żądaniach PATCH zająć parametr typu **Delta&lt;T&gt;**, gdzie *T* jest typem jednostki.

Odwołania Oto przykład pokazujący podpisy metod dla każdej wbudowanych Konwencji tras OData.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Konwencje tras niestandardowych

Obecnie wbudowane konwencje nie obejmują wszystkich możliwych identyfikatorów URI OData. Można dodawać nowych Konwencji zaimplementowanie **IODataRoutingConvention** interfejsu. Ten interfejs ma dwóch metod:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** zwraca nazwę kontrolera.
- **SelectAction** zwraca nazwę akcji.

Dla obu metod Jeśli Konwencji nie ma zastosowania do tego żądania, metoda powinna zwrócić wartość null.

**Element ODataPath** parametr reprezentuje przeanalizowany ścieżka zasobu OData. Zawiera listę **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** wystąpienia, po jednej dla każdego segmentu ścieżki zasobu. **ODataPathSegment** jest klasą abstrakcyjną; każdy typ segmentu jest reprezentowany przez klasę pochodną **ODataPathSegment**.

**ODataPath.TemplatePath** właściwość jest ciągiem, który reprezentuje połączenie wszystkich segmentów ścieżki. Na przykład, jeśli identyfikator URI jest `/Products(1)/Supplier`, jest szablon ścieżki &quot;~/entityset/key/navigation&quot;. Zwróć uwagę, że segmenty nie odnoszą się bezpośrednio do segmentów identyfikatora URI. Na przykład klucz jednostki (1) jest reprezentowany jako własnego **ODataPathSegment**.

Zazwyczaj implementacja **IODataRoutingConvention** wykonuje następujące czynności:

1. Porównaj szablonu ścieżki, aby sprawdzić, czy Konwencja ma zastosowanie do bieżącego żądania. Jeśli nie ma zastosowania, zwraca wartość null.
2. Jeśli ma zastosowanie Konwencji, użyj właściwości **ODataPathSegment** wystąpień do uzyskania nazwy kontrolera i akcji.
3. Dla akcji należy dodać żadnych wartości do słownika trasy, który należy powiązać z parametrami akcji (zazwyczaj kluczy jednostek).

Oto przykład określone. Wbudowane Konwencji tras nie obsługują przeprowadzane jest indeksowanie do kolekcji nawigacji. Innymi słowy jest brak Konwencji identyfikatory URI podobne do poniższych:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Oto niestandardowych Konwencji routingu do obsługi tego typu zapytań.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Uwagi:

1. I pochodzi od **EntitySetRoutingConvention**, ponieważ **SelectController** metody klasy jest odpowiedni dla tego nowego Konwencji routingu. Oznacza to, że nie należy ponownie wdrożyć **SelectController**.
2. Konwencja dotyczy tylko żądania GET, i tylko wtedy, gdy szablon ścieżki &quot;~/entityset/key/navigation/key&quot;.
3. Nazwa akcji jest &quot;uzyskać {EntityType}&quot;, gdzie *{EntityType}* jest typ kolekcji nawigacji. Na przykład &quot;GetSupplier&quot;. Korzystając z konwencją nazewnictwa, który chcesz &#8212; upewnij się, zgodne akcji kontrolera.
4. Akcja przyjmuje dwa parametry o nazwie *klucza* i *relatedKey*. (Aby uzyskać listę niektóre nazwy wstępnie zdefiniowanych parametrów, zobacz [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Następny krok polega na dodaniu nowej Konwencji do listy Konwencji tras. Dzieje się to podczas konfiguracji, jak pokazano w poniższym kodzie:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Poniżej przedstawiono niektóre inne przykładowe routingu Konwencji służyć do badania:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

I oczywiście interfejsu API sieci Web, sam jest open source, tak aby był widoczny [kod źródłowy](http://aspnetwebstack.codeplex.com/) dla wbudowanych Konwencji tras. Są one zdefiniowane w **System.Web.Http.OData.Routing.Conventions** przestrzeni nazw.
