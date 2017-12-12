---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Tworzenie klienta JavaScript | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b397c5a413ae213c9b79da1c0e0626efe21c7e21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-the-javascript-client"></a>Tworzenie klienta JavaScript
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji utworzysz klienta dla aplikacji, przy użyciu języka HTML, JavaScript oraz [Knockout.js](http://knockoutjs.com/) biblioteki. Firma Microsoft będzie kompilacji aplikacji klienckiej w etapach:

- Wyświetlanie listy książek.
- Wyświetlanie szczegółów książki.
- Dodawanie nowej książki.

Biblioteki Knockout korzysta ze wzorca Model-View-ViewModel (MVVM):

- **Modelu** jest po stronie serwera reprezentację danych w domenie business (w naszym case, książki i autorów).
- **Widoku** to warstwa prezentacji (HTML).
- **Model widoku** jest obiektu JavaScript, która przechowuje modeli. Model widoku jest abstrakcji kodu interfejsu użytkownika. Go nie ma informacji o reprezentacji w formacie HTML. Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak &quot;listę książek&quot;.

Widok jest powiązany z danymi model widoku. Aktualizacje na model widoku są automatycznie odzwierciedlane w widoku. Model widoku również pobiera zdarzenia w widoku, takich jak kliknięcie przycisku.

![](part-6/_static/image1.png)

Tej metody można łatwo zmienić układ i interfejsu użytkownika aplikacji, ponieważ powiązania, można zmienić bez ponownego tworzenia kodu. Na przykład może wyświetlić listę elementów jako `<ul>`, następnie zmienić go później do tabeli.

## <a name="add-the-knockout-library"></a>Dodawanie biblioteki Knockout

W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**. Następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](part-6/samples/sample1.cmd)]

To polecenie dodaje pliki odcinania w folderze skryptów.

## <a name="create-the-view-model"></a>Tworzenie modelu widoku

Dodaj plik JavaScript o nazwie app.js w folderze skryptów. (W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder skryptów, wybierz **Dodaj**, a następnie wybierz pozycję **plik JavaScript**.) Wklej następujący kod:

[!code-javascript[Main](part-6/samples/sample2.js)]

W Knockout `observable` klasa umożliwia powiązanie danych. Zmienić zawartość zauważalny, według powiadamia wszystkie formanty powiązane z danymi, więc one aktualizowane automatycznie. ( `observableArray` Klasy jest wersją tablicy *według*.) Do uruchomienia z naszych model widoku zawiera dwa dostrzegalne elementy:

- `books`zawiera listę książek.
- `error`zawiera komunikat o błędzie, jeśli wywołanie AJAX nie powiodło się.

`getAllBooks` Metody sprawia, że wywołanie AJAX do pobrania listy książek. Następnie wypchnięcia jej wyników na `books` tablicy.

`ko.applyBindings` Metoda jest częścią biblioteki Knockout. Pobiera model widoku jako parametru, a konfiguruje wiązania danych.

## <a name="add-a-script-bundle"></a>Dodaj pakiet skryptu

Tworzenie pakietów jest funkcją w programie ASP.NET 4.5, który ułatwia łączenie lub wielu plików pakietu w jednym pliku. Tworzenie pakietów zmniejsza liczbę żądań do serwera, który można zwiększyć czas ładowania strony.

Otwórz plik aplikacji\_Start/BundleConfig.cs. Dodaj następujący kod do metody RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

>[!div class="step-by-step"]
[Poprzednie](part-5.md)
[dalej](part-7.md)
