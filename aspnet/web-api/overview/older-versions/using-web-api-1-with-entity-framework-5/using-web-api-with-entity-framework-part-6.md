---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "Część 6: Tworzenie produktu i kolejność kontrolerów | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 3b33543f02479b97112a63eb3879967ae31ccfb3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="part-6-creating-product-and-order-controllers"></a>Część 6: Tworzenie produktu i kolejność kontrolerów
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Dodawanie kontrolera produktów

Kontroler administratora jest dla użytkowników, którzy mają uprawnienia administratora. Klientów, z drugiej strony, można wyświetlić produktów, ale nie można utworzyć, zaktualizować lub usuń je.

Firma Microsoft łatwo ograniczyć dostęp do metody Post, Put i Delete, pozostawiając Otwórz metody Get. Ale przyjrzeć się dane, które są zwracane do produktu:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost` Właściwości nie powinny być widoczne dla klientów! Rozwiązanie jest określenie *obiektu transferu danych* (DTO), który zawiera podzbiór właściwości, które powinny być widoczne dla klientów. Używamy LINQ do projektu `Product` wystąpień do `ProductDTO` wystąpień.

Dodaj klasę o nazwie `ProductDTO` do folderu modeli.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Teraz Dodaj kontroler. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery. Wybierz **Dodaj**, a następnie wybierz pozycję **kontrolera**. W **Dodaj kontroler** okna dialogowego, nazwy kontrolera &quot;ProductsController&quot;. W obszarze **szablonu**, wybierz pozycję **Kontroler interfejsu API pusty**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Zastąp wszystkie elementy w pliku źródłowym następujący kod:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Nadal używa kontrolera `OrdersContext` aby w bazie danych. Zamiast zwracać, ale `Product` wystąpień bezpośrednio, nazywamy `MapProducts` do projektu, na `ProductDTO` wystąpień:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts` Metoda zwraca **IQueryable**, dlatego firma Microsoft tworzą wynik z innych parametrów zapytania. Widać to w `GetProduct` metodę, która dodaje **gdzie** klauzuli zapytania:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Dodawanie kontrolera zlecenia

Następnie dodaj kontroler, który umożliwia użytkownikom tworzenie i wyświetlanie zleceń.

Zaczniemy DTO innego. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli, a następnie Dodaj klasę o nazwie `OrderDTO` Użyj następujących implementacji:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Teraz Dodaj kontroler. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery. Wybierz **Dodaj**, a następnie wybierz pozycję **kontrolera**. W **Dodaj kontroler** okna dialogowego, ustaw następujące opcje:

- W obszarze **nazwy kontrolera**, wprowadź "OrdersController".
- W obszarze **szablonu**, wybierz pozycję "Kontroler interfejsu API z akcjami odczytu/zapisu, przy użyciu programu Entity Framework".
- W obszarze **klasa modelu**, wybierz pozycję &quot;zamówienia (ProductStore.Models)&quot;.
- W obszarze **klasa kontekstu danych**, wybierz pozycję &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Kliknij przycisk **Dodaj**. Spowoduje to dodanie pliku o nazwie OrdersController.cs. Następnie należy zmodyfikować domyślną implementację kontrolera.

Należy najpierw usunąć `PutOrder` i `DeleteOrder` metody. Dla tego przykładu klienci nie można zmodyfikować lub usunąć istniejące zlecenia. W rzeczywistej aplikacji będzie potrzebny partii logiki zaplecza do obsługi tych przypadkach. (Na przykład kolejność już dostarczono?)

Zmień `GetOrders` metodę, aby zwrócić tylko zleceń, które należą do użytkownika:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Zmień `GetOrder` metody w następujący sposób:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Poniżej przedstawiono zmiany wprowadzone do metody:

- Wartość zwracana jest `OrderDTO` wystąpienia, zamiast `Order`.
- Firma Microsoft kwerendy bazy danych dla zlecenia, używamy [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metodę, aby pobrać pokrewny `OrderDetail` i `Product` jednostek.
- Firma Microsoft spłaszczanie wynik przy użyciu projekcji.

Odpowiedź HTTP będzie zawierać tablicę produkty z ilości:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Ten format jest ułatwia klientom korzystać niż oryginalny wykres obiektu zawiera zagnieżdżone jednostki (kolejności, szczegóły i produktów).

Metoda ostatniego uważają, że `PostOrder`. Teraz, ta metoda przyjmuje `Order` wystąpienia. Jednak rozważyć, co się stanie, jeśli klient wyśle treści żądania następująco:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Jest to dobrze kolejności i Entity Framework będzie happily wstawić je do bazy danych. Ale zawiera jednostki produktu, która wcześniej nie istnieje. Klient właśnie utworzony nowy produkt w naszej bazie danych! Będzie zaskoczeniem do działu fullfilment kolejności, po zgłoszeniu kolejność posiada koala. Wnioski, należy zachować ostrożność naprawdę dane, które akceptują w żądaniu POST i PUT.

Aby uniknąć tego problemu, należy zmienić `PostOrder` metody podjęcie `OrderDTO` wystąpienia. Użyj `OrderDTO` do utworzenia `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Należy zauważyć, że używamy `ProductID` i `Quantity` właściwości i firma Microsoft ignorować wszystkie wartości, które klient wysłany nazwa produktu lub ceny. Jeśli identyfikator produktu nie jest prawidłowy, spowodują naruszenie ograniczenia klucza obcego w bazie danych i Wstaw zakończy się niepowodzeniem, jak powinno.

Poniżej przedstawiono pełną `PostOrder` metody:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Na koniec należy dodać **autoryzacji** atrybutu na kontrolerze:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Teraz można tworzyć tylko zarejestrowani użytkownicy lub wyświetlić zamówienia.

>[!div class="step-by-step"]
[Poprzednie](using-web-api-with-entity-framework-part-5.md)
[dalej](using-web-api-with-entity-framework-part-7.md)
