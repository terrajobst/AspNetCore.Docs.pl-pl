---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Dodawanie modeli i kontrolerów | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 162ef2cd4ba11040e1bc938617a36495489ba5bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755173"
---
<a name="add-models-and-controllers"></a>Dodawanie modeli i kontrolerów
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji dodasz klasy modeli, które definiują jednostek bazy danych. Następnie dodasz kontrolerów internetowych interfejsów API, które wykonują operacje CRUD na tych jednostkach.

## <a name="add-model-classes"></a>Dodawanie klasy modelu

W tym samouczku utworzymy bazy danych przy użyciu podejścia "Code First", aby Entity Framework (EF). Code First zapisu klas języka C#, które odnoszą się do tabel bazy danych i EF tworzy bazę danych. (Aby uzyskać więcej informacji, zobacz [podejścia do projektowania struktury jednostki](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Rozpoczniemy pracę przez zdefiniowanie naszych obiektów domeny jako POCOs (stary zwykły obiektów CLR). Utworzymy POCOs następujące:

- Autor
- Książki

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy kliknij folder modeli. Wybierz **Dodaj**, a następnie wybierz **klasy**. Nazwa klasy `Author`.

![](part-2/_static/image1.png)

Zamień wszystkie schematyczny kod w Author.cs następujący kod.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Dodaj klasę o nazwie `Book`, używając następującego kodu.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework użyje tych modeli do tworzenia tabel bazy danych. Dla każdego modelu `Id` Właściwości staną się kolumna klucza podstawowego w tabeli bazy danych.

W klasie książki `AuthorId` definiuje klucz obcy do `Author` tabeli. (Dla uproszczenia I jestem przy założeniu, że poszczególne książki ma jednego autora.) Klasa książki zawiera także właściwości nawigacji w celu powiązane `Author`. Właściwość nawigacji umożliwia dostęp do odnośnych `Author` w kodzie. Więcej informacji na temat właściwości nawigacji, część 4, mówię [Obsługa relacji jednostek](part-4.md).

## <a name="add-web-api-controllers"></a>Dodaj kontrolery interfejsu API sieci Web

W tej sekcji dodamy kontrolerów internetowych interfejsów API, które obsługują operacje CRUD (tworzenia, odczytu, aktualizacji i usuwania). Kontrolery będzie używać programu Entity Framework do komunikowania się z warstwą bazy danych.

Po pierwsze można usunąć pliku Controllers/ValuesController.cs. Ten plik zawiera przykład kontroler interfejsu API sieci Web, ale nie są potrzebne w tym samouczku.

![](part-2/_static/image2.png)

Następnie Skompiluj projekt. Tworzenie szkieletu interfejsu API sieci Web używa odbicia, można znaleźć klasy modeli, dlatego potrzebuje skompilowanym zestawie.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj**, a następnie wybierz **kontrolera**.

![](part-2/_static/image3.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję "Web API 2 kontroler z akcjami używający narzędzia Entity Framework". Kliknij przycisk **Dodaj**.

![](part-2/_static/image4.png)

W **Dodaj kontroler** okna dialogowego, wykonaj następujące czynności:

1. W **klasa modelu** listy rozwijanej wybierz `Author` klasy. (Jeśli widzisz go na liście rozwijanej, upewnij się, że utworzony projekt.)
2. Sprawdź "Użyj asynchronicznych akcji kontrolera".
3. Pozostaw nazwę kontrolera &quot;AuthorsController&quot;.
4. Kliknij przycisk plus (+) znajdujący się obok **klasa kontekstu danych**.

![](part-2/_static/image5.png)

W **nowy kontekst danych** okno dialogowe, pozostaw nazwę domyślną, a następnie kliknij przycisk **Dodaj**.

![](part-2/_static/image6.png)

Kliknij przycisk **Dodaj** do ukończenia **Dodaj kontroler** okna dialogowego. Okno dialogowe powoduje dodanie dwóch klas do projektu:

- `AuthorsController` Definiuje kontrolera interfejsu API sieci Web. Kontroler implementuje interfejs API REST, używanego przez klientów w celu wykonywania operacji CRUD na liście autorów.
- `BookServiceContext` zarządza obiekty jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, śledzenie zmian i przechowywanie danych w bazie danych. Dziedziczy `DbContext`.

![](part-2/_static/image7.png)

W tym momencie ponownie skompiluj projekt. Teraz przechodzić przez te same kroki, aby dodać Kontroler interfejsu API dla `Book` jednostek. Tym razem wybierz pozycję `Book` klasy modelu, a następnie wybierz istniejący `BookServiceContext` klasy dla klasy kontekstu danych. (Nie twórz nowy kontekst danych). Kliknij przycisk **Dodaj** można dodać kontrolera.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Poprzednie](part-1.md)
> [dalej](part-3.md)
