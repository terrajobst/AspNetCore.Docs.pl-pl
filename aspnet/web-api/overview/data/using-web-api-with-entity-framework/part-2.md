---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Dodaj modele i kontrolerów | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879585"
---
<a name="add-models-and-controllers"></a>Dodaj modele i kontrolerów
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji dodasz klasy modeli, które definiują jednostki bazy danych. Następnie należy dodać kontrolerów interfejsu API sieci Web, które wykonują operacje CRUD na tych jednostek.

## <a name="add-model-classes"></a>Dodawanie klasy modeli

W tym samouczku utworzymy bazy danych przy użyciu "Code First" podejście do Entity Framework (EF). Code First zapisu klas C#, które odpowiadają w tabelach bazy danych i EF utworzy bazę danych. (Aby uzyskać więcej informacji, zobacz [Entity Framework programowanie podejścia](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Firma Microsoft Rozpocznij od zdefiniowania naszych obiektów domeny jako POCOs (stary zwykły obiektów CLR). Utworzymy POCOs następujące:

- Autor
- Book

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy kliknij folder modeli. Wybierz **Dodaj**, a następnie wybierz pozycję **klasy**. Nazwa klasy `Author`.

![](part-2/_static/image1.png)

Zamień wszystkie schematyczny kod w Author.cs poniższy kod.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Dodaj kolejną klasę o nazwie `Book`, z następującym kodem.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework użyje tych modeli w celu tworzenia tabel bazy danych. Dla każdego modelu `Id` Właściwości staną się kolumna klucza podstawowego tabeli bazy danych.

W klasie książki `AuthorId` definiuje klucza obcego do `Author` tabeli. (Dla uproszczenia I używam przy założeniu, że każdy książki ma jednego autora.) Klasa book zawiera także właściwości nawigacji do pokrewnych `Author`. Właściwość nawigacji umożliwia dostęp do pokrewny `Author` w kodzie. Coś więcej o właściwości nawigacji w część 4, [relacjami jednostek obsługi](part-4.md).

## <a name="add-web-api-controllers"></a>Dodaj kontrolery interfejsu API sieci Web

W tej sekcji dodamy kontrolery interfejsu API sieci Web, które obsługują operacje CRUD (tworzenia, odczytu, aktualizacji i usuwania). Kontrolery użyje Entity Framework do komunikowania się z warstwy bazy danych.

Po pierwsze można usunąć pliku Controllers/ValuesController.cs. Ten plik zawiera przykład kontroler interfejsu API sieci Web, ale nie będzie potrzebny w tym samouczku.

![](part-2/_static/image2.png)

Następnie należy skompilować projekt. Funkcja szkieletów interfejsu API sieci Web używa odbicia można znaleźć klasy modeli, dlatego potrzebuje skompilowanego zestawu.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery. Wybierz **Dodaj**, a następnie wybierz pozycję **kontrolera**.

![](part-2/_static/image3.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję "składnika Web API 2 kontroler z akcjami używający narzędzia Entity Framework". Kliknij przycisk **Dodaj**.

![](part-2/_static/image4.png)

W **Dodaj kontroler** okna dialogowego, wykonaj następujące czynności:

1. W **klasa modelu** listy rozwijanej wybierz `Author` klasy. (Jeśli nie widzisz wyświetlane na liście rozwijanej, upewnij się, że zostały utworzone w projekcie.)
2. Sprawdź "Użyj asynchronicznych akcji kontrolera".
3. Pozostaw nazwę kontrolera jako &quot;AuthorsController&quot;.
4. Kliknij przycisk plus (+) obok przycisku **klasa kontekstu danych**.

![](part-2/_static/image5.png)

W **nowy kontekst danych** okna dialogowego, pozostaw nazwę domyślną, a następnie kliknij przycisk **Dodaj**.

![](part-2/_static/image6.png)

Kliknij przycisk **Dodaj** do ukończenia **Dodaj kontroler** okna dialogowego. Okno dialogowe dodaje dwie klasy do projektu:

- `AuthorsController` Definiuje kontrolera interfejsu API sieci Web. Kontroler implementuje interfejs API REST, używanego przez klientów w celu wykonywania operacji CRUD na liście autorów.
- `BookServiceContext` zarządza obiekty obiektów w czasie wykonywania, obejmujące wypełnianie obiekty z danymi z bazy danych, śledzenie zmian i trwałych danych do bazy danych. Dziedziczy on z `DbContext`.

![](part-2/_static/image7.png)

W tym momencie ponownie skompilować projekt. Teraz przejdź przez te same kroki, aby dodać Kontroler interfejsu API dla `Book` jednostek. Teraz, wybierz opcję `Book` dla klasy modelu i wybierz istniejącą `BookServiceContext` klasy dla klasy kontekstu danych. (Nie tworzy nowy kontekst danych). Kliknij przycisk **Dodaj** Aby dodać kontroler.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Poprzednie](part-1.md)
> [dalej](part-3.md)
