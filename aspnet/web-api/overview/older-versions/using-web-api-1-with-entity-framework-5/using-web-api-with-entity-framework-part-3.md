---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Część 3: Tworzenie kontrolera Admin | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a>Część 3: Tworzenie kontrolera administratora
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Dodawanie kontrolera administratora

W tej sekcji dodamy kontrolera interfejsu API sieci Web, który obsługuje CRUD (tworzenia, odczytu, aktualizacji i usuwania) operacje na produkty. Kontroler użyje Entity Framework do komunikowania się z warstwy bazy danych. Tylko administratorzy będą mogli używać tego kontrolera. Klienci będą uzyskiwać dostęp do produktów za pomocą innego kontrolera.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery. Wybierz **dodać** , a następnie **kontrolera**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

W **Dodaj kontroler** okna dialogowego, nazwy kontrolera `AdminController`. W obszarze **szablonu**, wybierz pozycję &quot;Kontroler interfejsu API z akcjami odczytu/zapisu, przy użyciu programu Entity Framework&quot;. W obszarze **klasa modelu**, wybierz "Produktu (ProductStore.Models)". W obszarze **kontekstu danych**, wybierz pozycję "&lt;nowy kontekst danych&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Jeśli **klasa modelu** listy rozwijanej nie są wyświetlane wszystkie klasy modeli, upewnij się, że można skompilować projekt. Entity Framework używa odbicia, a więc musi skompilowanego zestawu.


Wybieranie "&lt;nowy kontekst danych&gt;" spowoduje otwarcie **nowy kontekst danych** okna dialogowego. Nazwa kontekstu danych `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Kliknij przycisk **OK** aby odrzucić **nowy kontekst danych** okna dialogowego. W **Dodaj kontroler** okna dialogowego, kliknij przycisk **Dodaj**.

Oto, co zostały dodane do projektu:

- Klasa o nazwie `OrdersContext` która pochodzi z **DbContext**. Ta klasa udostępnia sklejki między modelami POCO i bazy danych.
- Kontroler interfejsu API sieci Web o nazwie `AdminController`. Ten kontroler obsługuje operacje CRUD na `Product` wystąpień. Używa `OrdersContext` klasy do komunikowania się z programu Entity Framework.
- Nowe parametry połączenia bazy danych w pliku Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Otwórz plik OrdersContext.cs. Zwróć uwagę, że Konstruktor Określa nazwę parametrów połączenia bazy danych. Ta nazwa odwołuje się do ciągu połączenia, który został dodany do pliku Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Dodaj następujące właściwości `OrdersContext` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet** reprezentuje zestaw jednostek, które można wyszukiwać. Poniżej przedstawiono pełną listę `OrdersContext` klasy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController` Klasa definiuje pięć metod, które implementują podstawowe funkcje CRUD. Każda metoda odnosi się do identyfikatora URI, który można wywołać klienta:

| Kontroler — metoda | Opis | Identyfikator URI | Metoda HTTP |
| --- | --- | --- | --- |
| GetProducts | Pobiera wszystkie produkty. | Interfejs API/produktów | GET |
| GetProduct | Wyszukuje produktu według identyfikatora. | produkty/API/*id* | GET |
| PutProduct | Aktualizacje produktu. | produkty/API/*id* | UMIEŚĆ |
| PostProduct | Tworzy nowego produktu. | Interfejs API/produktów | POST |
| DeleteProduct | Usuwa produktu. | produkty/API/*id* | DELETE |

Każda metoda wywołuje do `OrdersContext` aby w bazie danych. Wywołanie metody, które modyfikują kolekcji (PUT, POST i DELETE) `db.SaveChanges` do utrwalania zmian w bazie danych. Kontrolery są tworzone na żądanie HTTP i następnie usunięty, dlatego należy zachować zmiany przed metoda zwraca.

## <a name="add-a-database-initializer"></a>Dodaj inicjatora bazy danych

Entity Framework ma nieuprzywilejowany funkcja, która umożliwia wypełnienie bazy danych podczas uruchamiania i automatycznie ponownie utworzyć bazy danych, zmianie modeli. Ta funkcja jest przydatna podczas tworzenia, ponieważ zawsze dane testowe, nawet w przypadku zmiany modelu.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli i Utwórz nową klasę o nazwie `OrdersContextInitializer`. Wklej następujący implementacji:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Poprzez dziedziczenie z **DropCreateDatabaseIfModelChanges** klasy, firma Microsoft informuje Entity Framework do usunięcia bazy danych, gdy firma Microsoft zmodyfikować klasy modelu. Kiedy Entity Framework utworzy (lub odtwarza) bazy danych, wywołuje **inicjatora** metodę, aby wypełnić tabel. Używamy **inicjatora** metody w celu dodania przykład niektórych produktów, oraz przykład zamówienia.

Ta funkcja stanowi doskonałe rozwiązanie do testowania, ale nie należy używać **DropCreateDatabaseIfModelChanges** klasy w środowisku produkcyjnym, ponieważ zmiana klasę modelu może spowodować utratę danych.

Następnie otwórz plik Global.asax i Dodaj następujący kod do **aplikacji\_Start** metody:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Wyślij żądanie do kontrolera

Na tym etapie firma Microsoft nie zostały zapisane żadnego kodu klienta, ale można wywołać interfejsu API przy użyciu przeglądarki sieci web lub debugowania HTTP takich jak narzędzia sieci web [Fiddler](http://www.fiddler2.com/fiddler2/). W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowania. Zostanie otwarta w przeglądarce sieci web `http://localhost:*portnum*/`, gdzie *portnum* niektórych numer portu.

Wyślij żądanie HTTP skierowane do "`http://localhost:*portnum*/api/admin`. Pierwsze żądanie może być powolne, ponieważ Entify Framework musi utworzyć i inicjatora bazy danych. Odpowiedź powinna coś podobnego do następującego:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-2.md)
> [dalej](using-web-api-with-entity-framework-part-4.md)
