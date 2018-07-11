---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Konwencje routingu we wzorcu ASP.NET Web API 2 Odata | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano konwencje routingu, które korzysta z interfejsu API sieci Web dla punktów końcowych OData.
ms.author: aspnetcontent
ms.date: 07/31/2013
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 645e820d3b03d1e3d2ac088973f6296efa5c2f40
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818268"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Konwencje routingu we wzorcu ASP.NET Web API 2 Odata
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> W tym artykule opisano konwencje routingu, które korzysta z interfejsu API sieci Web dla punktów końcowych OData.


Gdy internetowy interfejs API odbiera żądanie OData, jest on mapowany żądanie nazwy kontrolera i nazwy akcji. Mapowanie jest oparte na metody HTTP i identyfikatora URI. Na przykład `GET /odata/Products(1)` mapuje `ProductsController.GetProduct`.

W części 1 tego artykułu I Opisz wbudowanych konwencje tras OData. Konwencje te są przeznaczone specjalnie dla punktów końcowych OData, a następnie zastępuje domyślny system routingu internetowego interfejsu API. (Zastępuje się dzieje, gdy wywołujesz **MapODataRoute**.)

W części 2 I przedstawiają sposób dodawania niestandardowej Konwencji tras. Obecnie wbudowane konwencje nie obejmuje cały zakres OData na identyfikatory URI, ale można rozszerzyć je, aby obsłużyć dodatkowe przypadki.

- [Wbudowane Konwencji tras](#conventions)
- [Konwencje tras niestandardowych](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Wbudowane Konwencji tras

Przed I opisano konwencje tras OData w interfejsie API sieci Web, jest pomocne w zrozumieniu identyfikatorów URI OData. [Identyfikatora URI OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) obejmuje:

- Element główny usługi
- Ścieżka zasobu
- Opcje zapytania

![](odata-routing-conventions/_static/image1.png)

W przypadku routingu ważną część to ścieżka zasobu. Ścieżka zasobu jest podzielony na segmenty. Na przykład `/Products(1)/Supplier` ma trzy segmenty:

- `Products` odnosi się do zbioru jednostek o nazwie "Produkty".
- `1` jest to klucz jednostki, wybierając pojedynczej jednostki z zestawu.
- `Supplier` jest właściwość nawigacji, który wybiera powiązanej jednostki.

Zatem ta ścieżka wybiera się z dostawcą danego produktu 1.

> [!NOTE]
> Segmenty ścieżki OData nie zawsze pasują do segmentów identyfikatora URI. Na przykład "1" jest uważany za segmentu ścieżki.


**Nazwy kontrolera.** Nazwa kontrolera jest zawsze pochodną jednostki ustawione na poziom główny ścieżki zasobu. Na przykład, jeśli ścieżka zasobu jest `/Products(1)/Supplier`, internetowy interfejs API wygląda dla kontrolera, o nazwie `ProductsController`.

**Nazwy akcji.** Nazwy akcji pochodzą od segmenty ścieżki, a także modelu entity data model (EDM) struktury, zgodnie z opisem w poniższych tabelach. W niektórych przypadkach dostępne są dwie opcje dla nazwy akcji. Na przykład "Get" lub &quot;GetProducts&quot;.

**Podczas badania jednostki**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| Pobierz /entityset | / Produktów | GetEntitySet lub Get | GetProducts |
| Pobierz /entityset(key) | /Products(1) | GetEntityType lub Get | GetProduct |
| Pobierz /entityset (klucz) / rzutowania | /Products(1)/Models.Book | GetEntityType lub Get | GetBook |

Aby uzyskać więcej informacji, zobacz [Tworzenie punktu końcowego OData tylko do odczytu](odata-v3/creating-an-odata-endpoint.md).

**Tworzenie, aktualizowanie i usuwanie jednostek**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| OPUBLIKUJ /entityset | / Produktów | PostEntityType lub Post | PostProduct |
| Umieść /entityset(key) | /Products(1) | PutEntityType lub Put | PutProduct |
| Umieść /entityset (klucz) / rzutowania | /Products(1)/Models.Book | PutEntityType lub Put | PutBook |
| /Entityset(key) poprawki | /Products(1) | PatchEntityType lub poprawki | PatchProduct |
| POPRAWKA /entityset (klucz) / rzutowania | /Products(1)/Models.Book | PatchEntityType lub poprawki | PatchBook |
| Usuń /entityset(key) | /Products(1) | DeleteEntityType lub Delete | DeleteProduct |
| Usuń /entityset (klucz) / rzutowania | /Products(1)/Models.Book | DeleteEntityType lub Delete | DeleteBook |

**Badanie właściwości nawigacji**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| Pobierz /entityset (klucz) / nawigacji | / (1) lub dostawcy produktów | GetNavigationFromEntityType lub GetNavigation | GetSupplierFromProduct |
| Pobierz /entityset (klucz) / rzutowania/nawigacji | /Products(1)/Models.Book/Author | GetNavigationFromEntityType lub GetNavigation | GetAuthorFromBook |

Aby uzyskać więcej informacji, zobacz [Praca z relacjami jednostek](odata-v3/working-with-entity-relations.md).

**Tworzenie i usuwanie łącza**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji |
| --- | --- | --- |
| WPIS /entityset (klucz) / $links/nawigacji | / (1) / $produktów łącza lub dostawcy | CreateLink |
| Umieść /entityset (klucz) / $links/nawigacji | / (1) / $produktów łącza lub dostawcy | CreateLink |
| Usuń /entityset (klucz) / $links/nawigacji | / (1) / $produktów łącza lub dostawcy | DeleteLink |
| Usuń /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Aby uzyskać więcej informacji, zobacz [Praca z relacjami jednostek](odata-v3/working-with-entity-relations.md).

**Właściwości**

*Wymaga interfejsu Web API 2*

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| Pobierz /entityset (klucz) / właściwości | /Products(1)/Name | GetPropertyFromEntityType lub GetProperty | GetNameFromProduct |
| Pobierz /entityset (klucz) / rzutowania/właściwości | /Products(1)/Models.Book/Author | GetPropertyFromEntityType lub GetProperty | GetTitleFromBook |

**Akcje**

| Żądanie | Przykład identyfikatora URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| WPIS /entityset (klucz) / akcji | / (1) / szybkość produktów | ActionNameOnEntityType lub nazwa akcji | RateOnProduct |
| OPUBLIKUJ /entityset (klucz) / rzutowania/akcji | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType lub nazwa akcji | CheckOutOnBook |

Aby uzyskać więcej informacji, zobacz [akcji OData](odata-v3/odata-actions.md).

**Podpisy metod**

Poniżej przedstawiono niektóre reguły dla podpisów metody:

- Jeśli ścieżka zawiera klucz, działania powinny mieć parametr o nazwie *klucz*.
- Jeśli ścieżka zawiera klucz do właściwości nawigacji, działania powinny mieć parametr o nazwie *relatedKey*.
- Dekoracji *klucz* i *relatedKey* parametrów za pomocą **[FromODataUri]** parametru.
- POST i PUT żądań przyjmować parametr typu jednostki.
- W żądaniach PATCH przyjmować parametr typu **Delta&lt;T&gt;**, gdzie *T* jest typem jednostki.

Odwołanie Oto przykład pokazujący podpisy metod dla każdego wbudowanych Konwencji routingu OData.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Konwencje tras niestandardowych

Obecnie wbudowane konwencje nie obejmują wszystkich możliwych identyfikatorów URI OData. Możesz dodać nowe konwencje implementując **IODataRoutingConvention** interfejsu. Ten interfejs ma dwie metody:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** zwraca nazwę kontrolera.
- **SelectAction** zwraca nazwę akcji.

Dla obu metod Jeśli Konwencji nie ma zastosowania do tego żądania, metoda powinna zwrócić wartość null.

**Element ODataPath** parametr reprezentuje przeanalizowany ścieżkę zasobów OData. Zawiera on listę **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** wystąpienia, jedno dla każdego segmentu ścieżki zasobu. **ODataPathSegment** jest klasą abstrakcyjną; każdy typ segmentu jest reprezentowany przez klasę, która jest pochodną **ODataPathSegment**.

**ODataPath.TemplatePath** właściwość ma postać ciągu, który reprezentuje łączenie wszystkich segmentów ścieżki. Na przykład, jeśli identyfikator URI jest `/Products(1)/Supplier`, szablon ścieżki jest &quot;~/entityset/key/navigation&quot;. Należy zauważyć, że segmenty nie odpowiadają bezpośrednio do identyfikatora URI segmentów. Na przykład klucz jednostki (1) jest reprezentowany jako swój własny **ODataPathSegment**.

Zazwyczaj implementację **IODataRoutingConvention** wykonuje następujące czynności:

1. Porównaj z szablonem ścieżki, aby sprawdzić, czy ta Konwencja ma zastosowanie do bieżącego żądania. Jeśli nie ma zastosowania, zwracać wartość null.
2. Jeśli ma zastosowanie Konwencji, użyj właściwości **ODataPathSegment** wystąpień do uzyskania nazwy kontrolera i akcji.
3. Dla akcji należy dodać żadnych wartości do słownika trasy, która powinna być powiązana z parametrów akcji (zazwyczaj klucze jednostek).

Przyjrzyjmy się konkretnemu przykładowi. Wbudowane Konwencji tras nie obsługują indeksowanie w kolekcji nawigacji. Innymi słowy jest nie Konwencję identyfikatory URI podobne do następującego:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Oto Konwencję tras niestandardowych do obsługi tego typu zapytań.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Uwagi:

1. Czy mogę pochodzić od **EntitySetRoutingConvention**, ponieważ **SelectController** metody klasy jest odpowiedni dla tej nowej Konwencji routingu. Oznacza to, że nie potrzebuję potrzebę ponownego zaimplementowania **SelectController**.
2. Konwencja dotyczy tylko żądania GET, i tylko wtedy, gdy szablon ścieżki &quot;~/entityset/key/navigation/key&quot;.
3. Nazwa akcji jest &quot;pobieranie {elementu EntityType}&quot;, gdzie *{EntityType}* jest typem kolekcji nawigacji. Na przykład &quot;GetSupplier&quot;. Można użyć konwencji nazewnictwa, który chcesz &#8212; upewnij się, dopasowania akcji kontrolera.
4. Akcja przyjmuje dwa parametry o nazwie *klucz* i *relatedKey*. (Aby uzyskać listę niektórych nazw wstępnie zdefiniowanych parametrów, zobacz [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Następny krok polega na dodaniu nowej Konwencji do listy Konwencji tras. Dzieje się podczas konfiguracji, tak jak pokazano w poniższym kodzie:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Poniżej przedstawiono niektóre inne przykładowe Konwencji tras, które być przydatne do badania:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

I oczywiście sam interfejs Web API typu open-source, dzięki czemu możesz zobaczyć [kod źródłowy](http://aspnetwebstack.codeplex.com/) dla wbudowanych Konwencji tras. Są one zdefiniowane w **System.Web.Http.OData.Routing.Conventions** przestrzeni nazw.
