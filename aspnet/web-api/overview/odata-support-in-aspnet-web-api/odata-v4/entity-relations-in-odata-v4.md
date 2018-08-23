---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relacje jednostek w protokole OData v4 przy użyciu wzorca ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'Większość zestawów danych zdefiniować relacje między jednostkami: klienci mają zamówienia; książki mają autorzy; Jeśli te produkty mają dostawców. Przy użyciu protokołu OData, klienci mogą przejść za pośrednictwem...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 80173519f1c8abd77b4138b7d29f780ffc60a188
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756314"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relacje jednostek w protokole OData v4 przy użyciu wzorca ASP.NET Web API 2.2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> Większość zestawów danych zdefiniować relacje między jednostkami: klienci mają zamówienia; książki mają autorzy; Jeśli te produkty mają dostawców. Używanie protokołu OData, klienci mogą przejść za pośrednictwem relacji jednostek. Biorąc pod uwagę produktu, można znaleźć dostawcy. Można również tworzyć i usuwać relacje. Na przykład można ustawić dostawcę dla produktu.
> 
> W tym samouczku pokazano, jak obsługują te operacje w protokole OData v4 przy użyciu interfejsu API sieci Web platformy ASP.NET. Samouczek opiera się na samouczka [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Składnik Web API 2.1
> - Protokołu OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Dla protokołu OData w wersji 3, zobacz [Obsługa relacji jednostek w protokole OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Dodaj jednostkę dostawcy

> [!NOTE]
> Samouczek opiera się na samouczka [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2](create-an-odata-v4-endpoint.md).


Po pierwsze potrzebujemy powiązanej jednostki. Dodaj klasę o nazwie `Supplier` w folderze modeli.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Właściwość nawigacji, aby dodać `Product` klasy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Dodaj nową **DbSet** do `ProductsContext` klasy, tak aby Entity Framework będzie zawierać tabeli dostawcy w bazie danych.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

W WebApiConfig.cs, Dodaj &quot;dostawców&quot; zestawu jednostek do modelu danych jednostki:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Dodawanie kontrolera dostawców

Dodaj `SuppliersController` klasy do folderu kontrolerów.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Czy mogę nie pokazują, jak dodać operacje CRUD dla tego kontrolera. Kroki są takie same jak dla kontrolera produktów (zobacz [utworzenie punktu końcowego OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Pobieranie powiązanych jednostek

Aby uzyskać dostawcy, produktu, klient wysyła żądanie pobrania:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Aby zapewnić obsługę tego żądania, dodaj następującą metodę do `ProductsController` klasy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Ta metoda używa domyślnej konwencji nazewnictwa

- Nazwa metody: metody GetX, gdzie X jest właściwość nawigacji.
- Nazwa parametru: *klucza*

Jeśli wykonujesz tę konwencję nazewnictwa, interfejs API sieci Web automatycznie mapuje żądania HTTP do metody kontrolera.

Przykład HTTP żądania:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Odpowiedź na przykład HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Pobieranie kolekcji powiązanych

W poprzednim przykładzie produkt ma jeden dostawca. Właściwość nawigacji może również zwracać kolekcję. Poniższy kod umożliwia pobranie produktów dla dostawcy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

W tym przypadku metoda ta zwraca **IQueryable** zamiast **SingleResult&lt;T&gt;**

Przykład HTTP żądania:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Odpowiedź na przykład HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Tworzenie relacji między jednostkami

OData obsługuje tworzenie lub usuwanie relacji między dwiema jednostkami istniejących. W terminologii OData v4 relacja jest &quot;odwołania&quot;. (W protokole OData v3 relacja została wywołana *łącze*. Różnice protokołu nie ma znaczenia, w tym samouczku.)

Odwołanie ma swój własny identyfikator URI, za pomocą formularza `/Entity/NavigationProperty/$ref`. Na przykład Oto identyfikatora URI, aby rozwiązać odwołania między produktem i jego dostawcy:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Aby dodać relację, klient wysyła żądanie POST lub PUT z tym adresem.

- Umieść, jeśli właściwość nawigacji jest pojedynczą jednostką, takich jak `Product.Supplier`.
- Jeśli właściwość nawigacji jest kolekcją, takich jak `Supplier.Products`.

Treść żądania zawiera identyfikator URI inną jednostkę w relacji. Oto przykładowe żądanie:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

W tym przykładzie klient wysyła żądanie PUT `/Products(6)/Supplier/$ref`, czyli identyfikator URI $ref `Supplier` produktu o identyfikatorze = 6. Jeśli żądanie zakończy się powodzeniem, serwer wysyła odpowiedź 204 (Brak zawartości):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Poniżej przedstawiono metody kontrolera, aby dodać relację do `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*Element navigationProperty* parametr określa relacji, które można ustawić. (Jeśli istnieje więcej niż jednej właściwości nawigacji jednostki, możesz dodać więcej `case` instrukcji.)

*Łącze* parametr zawiera identyfikator URI dostawcy. Interfejs API sieci Web automatycznie analizuje treść żądania, aby uzyskać wartość tego parametru.

Aby wyszukać dostawcy, potrzebujemy Identyfikatora (lub klucza), który jest częścią *łącze* parametru. Aby to zrobić, użyj następującej metody pomocnika:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Zasadniczo ta metoda używa biblioteki OData Podziel składnik path identyfikatora URI na segmenty, Znajdź segment, który zawiera klucz i przekonwertować klucz do poprawnego typu.

## <a name="deleting-a-relationship-between-entities"></a>Usuwanie relacji między jednostkami

Aby usunąć relację, klient wysyła żądanie HTTP DELETE do identyfikatora URI $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Poniżej przedstawiono metody kontrolera, aby usunąć relację między produktem i dostawcy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

W tym przypadku `Product.Supplier` jest &quot;1&quot; end relacji 1-do wielu, dzięki czemu można usunąć relacji, po prostu przez ustawienie `Product.Supplier` do `null`.

W &quot;wiele&quot; końcu relacji klienta należy określić, które powiązanego obiektu do usunięcia. Aby to zrobić, klient wysyła identyfikator URI powiązanej jednostki w ciągu zapytania żądania. Na przykład, aby usunąć "Product 1" z "Dostawca 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Aby to umożliwić w interfejsie API sieci Web, musimy uwzględnić dodatkowego parametru w `DeleteRef` metody. Poniżej przedstawiono metody kontrolera, aby usunąć produkt z `Supplier.Products` relacji.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*Klucz* parametr jest klucz dla dostawcy i *relatedKey* parametr jest klucz produktu usunąć z `Products` relacji. Należy pamiętać, że interfejs API sieci Web automatycznie pobiera klucz z ciągu zapytania.
