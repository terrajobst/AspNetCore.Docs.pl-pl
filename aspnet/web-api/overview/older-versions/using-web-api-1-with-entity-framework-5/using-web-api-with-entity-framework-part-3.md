---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Część 3: Tworzenie kontrolera administratora | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7ad0ec27021514b447e569e479a9e9127e3f75fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757146"
---
<a name="part-3-creating-an-admin-controller"></a>Część 3: Tworzenie kontrolera administratora
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Dodawanie kontrolera administratora

W tej sekcji dodamy kontroler Web API, który obsługuje CRUD (tworzenia, odczytu, aktualizacji i usuwania) operacje na produkty. Kontroler będzie używać programu Entity Framework do komunikowania się z warstwą bazy danych. Tylko administratorzy będą mogli korzystać z tego kontrolera. Klienci będą uzyskiwać dostęp do produktów, za pomocą innego kontrolera.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj** i następnie **kontrolera**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

W **Dodaj kontroler** okno dialogowe, nazwy kontrolera `AdminController`. W obszarze **szablonu**, wybierz opcję &quot;Kontroler interfejsu API z akcjami odczytu/zapisu, używający narzędzia Entity Framework&quot;. W obszarze **klasa modelu**, wybierz pozycję "Product (ProductStore.Models)". W obszarze **kontekstu danych**, wybierz opcję "&lt;nowy kontekst danych&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Jeśli **klasa modelu** listy rozwijanej nie są wyświetlane wszystkie klasy modelu, upewnij się, skompilowany projekt. Entity Framework używa odbicia, więc wymaga skompilowanym zestawie.


Wybieranie "&lt;nowy kontekst danych&gt;" spowoduje otwarcie **nowy kontekst danych** okna dialogowego. Nazwa kontekstu danych `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Kliknij przycisk **OK** odrzucać **nowy kontekst danych** okna dialogowego. W **Dodaj kontroler** okno dialogowe, kliknij przycisk **Dodaj**.

Oto, co stało się dodane do projektu:

- Klasa o nazwie `OrdersContext` który pochodzi od klasy **DbContext**. Ta klasa dostarcza pośredniczącego między modelami POCO i bazy danych.
- Kontroler Web API o nazwie `AdminController`. Ten kontroler obsługuje operacje CRUD na `Product` wystąpień. Używa ona `OrdersContext` klasy do komunikowania się z platformą Entity Framework.
- Nowe parametry połączenia bazy danych w pliku Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Otwórz plik OrdersContext.cs. Należy zauważyć, że Konstruktor Określa nazwę parametrów połączenia bazy danych. Ta nazwa odnosi się do ciągu połączenia, który został dodany do pliku Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Dodaj następujące właściwości do `OrdersContext` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet** reprezentuje zestaw jednostek, które mogą być wyszukiwane. Oto Pełna lista dla `OrdersContext` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController` Klasa definiuje pięć metod, które implementują podstawowych funkcji CRUD. Każda metoda odnosi się do identyfikatora URI, które mogą być wywoływane przez klienta:

| Metoda kontrolera | Opis | Identyfikator URI | Metoda HTTP |
| --- | --- | --- | --- |
| GetProducts | Pobiera wszystkie produkty. | Interfejs API/produktów | POBIERZ |
| GetProduct | Umożliwia znalezienie produktu według identyfikatora. | InterfejsAPI/produkty/*identyfikator* | POBIERZ |
| PutProduct | Aktualizacje produktu. | InterfejsAPI/produkty/*identyfikator* | PUT |
| PostProduct | Tworzy nowy produkt. | Interfejs API/produktów | POST |
| DeleteProduct | Usuwa produkt. | InterfejsAPI/produkty/*identyfikator* | DELETE |

Każda metoda wywoła `OrdersContext` do wykonywania zapytań w bazie danych. Wywołanie metody, które zmodyfikować kolekcji (PUT, POST i DELETE) `db.SaveChanges` aby utrwalić zmiany w bazie danych. Kontrolery są tworzone na żądanie HTTP i następnie usunięty tak jest zachować zmiany, zanim metoda zwraca.

## <a name="add-a-database-initializer"></a>Dodaj inicjator bazy danych

Entity Framework ma ładny funkcja, która umożliwia wypełnienie bazy danych podczas uruchamiania i automatycznie odtworzy bazy danych, po każdym wprowadzeniu zmiany modeli. Ta funkcja jest przydatna podczas tworzenia aplikacji, ponieważ zawsze masz dane testowe nawet wtedy, gdy zmienisz modeli.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli, a następnie utwórz nową klasę o nazwie `OrdersContextInitializer`. Wklej następującą implementacją:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Przez dziedziczenie z **DropCreateDatabaseIfModelChanges** klasy, kolejne parametry programu Entity Framework w celu porzucenia bazy danych zawsze wtedy, gdy zmodyfikujemy klasy modelu. Gdy Entity Framework tworzy (lub odtwarza) bazy danych, wywołuje **inicjatora** metodę, aby wypełnić tabele. Używamy **inicjatora** metoda umożliwiająca dodanie niektórych produktów w przykładzie oraz przykładowe zamówienie.

Ta funkcja to idealne narzędzie do testowania, ale nie należy używać **DropCreateDatabaseIfModelChanges** klasy w środowisku produkcyjnym, ponieważ może utracić dane, jeśli ktoś zmienia klasę modelu.

Następnie otwórz plik Global.asax i Dodaj następujący kod do **aplikacji\_Start** metody:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Wyślij żądanie do kontrolera

Na tym etapie firma Microsoft nie zostały zapisane jakiegokolwiek kodu klienta, ale można wywołać interfejsu API za pomocą przeglądarki sieci web lub debugowania HTTP narzędzia, takie jak sieci web [Fiddler](http://www.fiddler2.com/fiddler2/). W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowanie. Spowoduje to otwarcie przeglądarki sieci web do `http://localhost:*portnum*/`, gdzie *portnum* niektóre numer portu to.

Wyślij żądanie HTTP do "`http://localhost:*portnum*/api/admin`. Pierwsze żądanie może być powolne, ponieważ Entify Framework musi utworzyć i Inicjowanie bazy danych. Odpowiedź powinna coś podobnego do następującego:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-2.md)
> [dalej](using-web-api-with-entity-framework-part-4.md)
