---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Część 6: Tworzenie kontrolerów produktów i zamówień | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756046"
---
<a name="part-6-creating-product-and-order-controllers"></a>Część 6: Tworzenie kontrolerów produktów i zamówień
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Dodawanie kontrolera produktów

Kontrolera administratora jest przeznaczony dla użytkowników, którzy mają uprawnienia administratora. Klientów, z drugiej strony, Wyświetl produkty, ale nie można utworzyć, zaktualizować lub je usunąć.

Firma Microsoft może łatwo ograniczyć dostęp do metody Post, Put i Delete, przy równoczesnym pozostawieniu otwartego metody Get. Można jednak przyjrzeć się danym, która jest zwracana dla produktu:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost` Właściwości nie powinny być widoczne dla klientów! Rozwiązanie polega na zdefiniowaniu *obiekt transferu danych* (DTO), który zawiera podzbiór właściwości, które powinny być widoczne dla klientów. Firma Microsoft użyje LINQ do projektu `Product` wystąpień do `ProductDTO` wystąpień.

Dodaj klasę o nazwie `ProductDTO` do folderu modeli.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Teraz Dodaj kontroler. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj**, a następnie wybierz **kontrolera**. W **Dodaj kontroler** okno dialogowe, nazwy kontrolera &quot;ProductsController&quot;. W obszarze **szablonu**, wybierz opcję **kontrolera pusty interfejs API**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Zastąp całą zawartość pliku źródłowego, z następującym kodem:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Nadal używa kontrolera `OrdersContext` do wykonywania zapytań w bazie danych. Jednak zamiast zwracać `Product` wystąpień bezpośrednio, nazywamy `MapProducts` do projektu je na `ProductDTO` wystąpień:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts` Metoda zwraca **IQueryable**, dzięki czemu możemy narzędzia compose wyników za pomocą inne parametry zapytania. Widać to w `GetProduct` metody, która dodaje **gdzie** klauzuli kwerendy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Dodawanie kontrolera Orders

Następnie dodaj kontroler, który pozwala użytkownikom tworzyć i wyświetlać zamówienia.

Rozpoczniemy od innego DTO. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli, a następnie Dodaj klasę o nazwie `OrderDTO` Użyj następującą implementacją:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Teraz Dodaj kontroler. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj**, a następnie wybierz **kontrolera**. W **Dodaj kontroler** okna dialogowego, ustaw następujące opcje:

- W obszarze **nazwy kontrolera**, wprowadź "OrdersController".
- W obszarze **szablonu**, wybierz opcję "Kontroler interfejsu API z akcjami odczytu/zapisu, używający narzędzia Entity Framework".
- W obszarze **klasa modelu**, wybierz opcję &quot;zamówienia (ProductStore.Models)&quot;.
- W obszarze **klasa kontekstu danych**, wybierz opcję &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Kliknij przycisk **Dodaj**. Spowoduje to dodanie pliku o nazwie OrdersController.cs. Następnie należy zmodyfikować domyślną implementację elementu kontrolera.

Najpierw usuń `PutOrder` i `DeleteOrder` metody. W tym przykładzie klienci nie mogą modyfikować ani usuwać istniejące zamówienia. W rzeczywistej aplikacji będziesz potrzebować mnóstwo logiki zaplecza w takich sytuacjach. (Na przykład został już wysłaniu zamówienia?)

Zmiana `GetOrders` metodę, aby zwrócić tylko zamówienia, które należą do użytkownika:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Zmiana `GetOrder` metody w następujący sposób:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Poniżej przedstawiono zmiany wprowadzone do metody:

- Wartość zwracana jest `OrderDTO` wystąpienia, zamiast `Order`.
- Firma Microsoft zapytania bazy danych dla zamówienia, używamy [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) — w celu pobrania powiązane `OrderDetail` i `Product` jednostek.
- Firma Microsoft spłaszczanie wynik za pomocą projekcji.

Odpowiedź HTTP będzie zawierać tablicę produkty z ilości:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Ten format jest łatwiejsze w przypadku klientów z niż oryginalny wykresu obiektu, który zawiera zagnieżdżony jednostki (kolejność, szczegóły i produktów).

Last — metoda wziąć pod uwagę jej `PostOrder`. W tej chwili, ta metoda przyjmuje `Order` wystąpienia. Jednak należy wziąć pod uwagę, co się stanie, jeśli klient wysyła treści żądania w następujący sposób:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Jest to dobrze kolejności i Entity Framework trafem korzysta wstawi je do bazy danych. Ale zawiera jednostkę produktu, która wcześniej nie istniał. Klient właśnie utworzony nowy produkt w naszej bazie danych! Będzie to Zaskoczenie do działu fullfilment kolejności, po zgłoszeniu zamówienie koala polarnych. Wnioski, należy zachować ostrożność bardzo dane, które akceptują żądania POST lub PUT.

Aby uniknąć tego problemu, należy zmienić `PostOrder` metodę, aby wykonać `OrderDTO` wystąpienia. Użyj `OrderDTO` utworzyć `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Należy zauważyć, że używamy `ProductID` i `Quantity` właściwości, a firma Microsoft Ignoruj dowolnych wartości, które klient wysyłał dla nazwy produktu i cenę. Jeśli identyfikator produktu nie jest prawidłowy, spowodują naruszenie, ograniczenie klucza obcego w bazie danych, a następnie Wstaw zakończy się niepowodzeniem, zgodnie z oczekiwaniami.

Poniżej przedstawiono pełne `PostOrder` metody:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Na koniec należy dodać **Autoryzuj** atrybutu do kontrolera:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Teraz tylko zarejestrowani użytkownicy można utworzyć lub wyświetlić zamówienia.

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-5.md)
> [dalej](using-web-api-with-entity-framework-part-7.md)
