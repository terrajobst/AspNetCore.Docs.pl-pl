---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: "Relacjami jednostek w protokołu OData v4 używanie składnika ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Większość zestawów danych definiować relacje między obiektami: klienci mają zleceń; książki ma autorów; produkty mają dostawców. Używanie protokołu OData, klientów można przechodzić przez..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relacjami jednostek w protokołu OData v4 używanie składnika ASP.NET Web API 2.2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> Większość zestawów danych definiować relacje między obiektami: klienci mają zleceń; książki ma autorów; produkty mają dostawców. Używanie protokołu OData, klienci mogą przechodzić za pośrednictwem relacjami jednostek. Biorąc pod uwagę produktu, można znaleźć dostawcy. Można również utworzyć lub Usuń relacje. Na przykład można ustawić dostawcy produktu.
> 
> W tym samouczku przedstawiono sposób obsługi tych operacji w protokołu OData v4 przy użyciu interfejsu API sieci Web platformy ASP.NET. Samouczek opiera się na samouczka [utworzyć OData v4 punktu końcowego Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - 2.1 interfejsu API sieci Web
> - Protokołu OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Dla OData w wersji 3, zobacz [obsługi relacjami jednostek w OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Dodawanie jednostki dostawcy

> [!NOTE]
> Samouczek opiera się na samouczka [utworzyć OData v4 punktu końcowego Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).


Najpierw należy powiązanej jednostki. Dodaj klasę o nazwie `Supplier` w folderze modeli.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Właściwość nawigacji, aby dodać `Product` klasy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Dodaj nową **DbSet** do `ProductsContext` klasy, tak aby zawiera tabeli dostawcy programu Entity Framework w bazie danych.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

W WebApiConfig.cs, Dodaj &quot;dostawców&quot; zestawu jednostek do modelu danych jednostki:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Dodawanie kontrolera dostawców

Dodaj `SuppliersController` klasy do folderu kontrolerów.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

I nie będzie pokazują, jak dodać operacje CRUD dla tego kontrolera. Kroki są takie same jak dla kontrolera produktów (zobacz [utworzyć punktu końcowego OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Pobieranie powiązanych jednostek

Aby uzyskać dostawcę produktu, klient wysyła żądanie pobrania:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Aby zapewnić obsługę tego żądania, dodaj następującą metodę do `ProductsController` klasy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Ta metoda używa domyślnej konwencji nazewnictwa

- Nazwa metody: GetX, gdzie X jest właściwością nawigacji.
- Nazwa parametru: *klucza*

Jeśli wykonujesz tę konwencję nazewnictwa interfejsu API sieci Web automatycznie mapuje żądania HTTP do metody kontrolera.

Przykład HTTP żądania:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Przykład HTTP odpowiedzi:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Pobierania kolekcję pokrewne

W poprzednim przykładzie produkt ma jeden dostawca. Właściwość nawigacji można także wrócić kolekcji. Poniższy kod pobiera produktów dla dostawcy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

W tym przypadku metoda zwraca **IQueryable** zamiast **SingleResult&lt;T&gt;**

Przykład HTTP żądania:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Przykład HTTP odpowiedzi:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Tworzenie relacji między jednostkami

OData obsługuje tworzenie lub usuwanie relacji między dwiema jednostkami istniejących. W terminologii OData v4 relacji jest &quot;odwołania&quot;. (W wersji 3 OData, wywołano relacji *łącze*. Różnice protokołu nie ma znaczenia w tym samouczku.)

Odwołanie ma własny identyfikator URI, za pomocą formularza `/Entity/NavigationProperty/$ref`. Na przykład w tym miejscu jest identyfikator URI do adresów odwołanie między produktu i jego dostawcy:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Aby dodać relację, klient wysyła żądanie POST i PUT na ten adres.

- USTAWIĆ właściwość nawigacji jest pojedyncza jednostka, takich jak `Product.Supplier`.
- Jeśli właściwość nawigacji jest kolekcją, takich jak `Supplier.Products`.

Treść żądania zawiera identyfikator URI inne jednostki w relacji. Poniżej przedstawiono przykładowe żądanie:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

W tym przykładzie klient wysyła żądanie PUT `/Products(6)/Supplier/$ref`, który jest identyfikatorem URI $ref dla `Supplier` produktu o identyfikatorze = 6. Jeśli żądanie zakończy się powodzeniem, serwer wysyła 204 odpowiedzi (bez zawartości):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Oto metoda kontrolera, można dodać relacji do `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty* parametr określa relacji, które można ustawić. (Jeśli istnieje więcej niż jedna właściwość nawigacji na jednostce, możesz dodać więcej `case` instrukcji.)

*Łącze* parametr zawiera identyfikator URI dostawcy. Interfejs API sieci Web automatycznie analizuje treść żądania, aby uzyskać wartość tego parametru.

Aby wyszukać dostawcy, potrzebujemy identyfikator (lub kluczy), który jest częścią *łącze* parametru. Aby to zrobić, użyj następującej metody pomocnika:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Zasadniczo ta metoda korzysta z biblioteki OData ścieżka identyfikatora URI jest podzielona na segmenty, Znajdź segment, który zawiera klucz i przekonwertować klucz do poprawnego typu.

## <a name="deleting-a-relationship-between-entities"></a>Usuwanie relacji między jednostkami

Aby usunąć relacji, klient wysyła żądanie HTTP DELETE do $ref identyfikatora URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Oto metoda kontrolera, można usunąć relacji między produktu i dostawcy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

W takim przypadku `Product.Supplier` jest &quot;1&quot; końca relacji 1-do wielu, dlatego w przypadku usunięcia relacji tylko przez ustawienie `Product.Supplier` do `null`.

W &quot;wiele&quot; końca relacji klienta należy określić, który powiązanego obiektu do usunięcia. Aby to zrobić, klient wysyła identyfikator URI obiektu pokrewnego w ciągu zapytania żądania. Na przykład, aby usunąć "Produktu 1" z "dostawcy 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Aby zapewnić tę obsługę w składniku Web API, musimy dołączyć dodatkowy parametr w `DeleteRef` metody. Oto metoda kontrolera, aby usunąć produkt z `Supplier.Products` relacji.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*Klucza* parametr jest klucz dla dostawcy, a *relatedKey* parametru jest klucz produktu usunąć z `Products` relacji. Należy pamiętać, że interfejs API sieci Web automatycznie pobiera klucz z ciągu zapytania.
