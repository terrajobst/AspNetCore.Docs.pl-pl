---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Tworzenie klienta JavaScript | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b0b8ef9bd44bbce5102f2b12717e330f72a9e0c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400937"
---
<a name="create-the-javascript-client"></a>Tworzenie klienta JavaScript
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji utworzysz klienta dla aplikacji, za pomocą kodu HTML, JavaScript oraz [struktura Knockout.js](http://knockoutjs.com/) biblioteki. Utworzymy aplikację kliencką w etapach:

- Wyświetlanie listy książek.
- Wyświetlanie szczegółów książki.
- Dodawanie nowej książki.

Bibliotekę Knockout używa wzorca Model-View-ViewModel (MVVM):

- **Modelu** jest po stronie serwera reprezentacja danych w domenie firmy (w naszym przypadku, książek i autorzy).
- **Widoku** to warstwa prezentacji (HTML).
- **Model widoku** jest obiekt JavaScript, który przechowuje modeli. Model widoku jest abstrakcji kodu interfejsu użytkownika. Go nie zna reprezentacji w formacie HTML. Zamiast tego, reprezentuje abstrakcyjnej funkcji widoku, takie jak &quot;listy książek&quot;.

Widok jest powiązany z danymi model widoku. Model widoku aktualizacje są automatycznie odzwierciedlane w widoku. Model widoku również pobiera zdarzenia w widoku, takich jak kliknięcie przycisku.

![](part-6/_static/image1.png)

Tej metody można łatwo zmienić układ i interfejsu użytkownika aplikacji, ponieważ powiązania, można zmienić bez konieczności ponownego zapisu żadnego kodu. Na przykład może wyświetlać listę elementów jako `<ul>`, zmienić ją później do tabeli.

## <a name="add-the-knockout-library"></a>Dodaj bibliotekę Knockout

W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**. Następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](part-6/samples/sample1.cmd)]

To polecenie dodaje pliki Knockout na folder skryptów.

## <a name="create-the-view-model"></a>Tworzenie modelu widoku

Dodaj plik języka JavaScript o nazwie app.js na folder skryptów. (W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder skryptów, wybierz **Dodaj**, a następnie wybierz **plik JavaScript**.) Wklej następujący kod:

[!code-javascript[Main](part-6/samples/sample2.js)]

W Knockout `observable` klasa umożliwia powiązanie danych. Zawartość możliwość obserwowania zmiany, możliwość obserwowania powiadamia wszystkie formanty powiązane z danymi, dzięki czemu mogą aktualizować same. ( `observableArray` Klasy jest tablica wersją *obserwowalnymi*.) Na początek z nasz model widoku ma dwa dostrzegalne elementy:

- `books` zawiera listy książek.
- `error` zawiera komunikat o błędzie w przypadku niepowodzenia wywołania AJAX.

`getAllBooks` Metoda przeprowadza wywołanie AJAX w celu uzyskania listy książek. A następnie umieszcza ono wynik na `books` tablicy.

`ko.applyBindings` Metodą jest częścią biblioteki Knockout. Jej modelu widoku jako parametr przyjmuje i konfiguruje powiązanie danych.

## <a name="add-a-script-bundle"></a>Dodaj pakiet skryptu

Tworzenie pakietów to funkcja w programie ASP.NET 4.5, który można łatwo połączyć lub wiele plików pakietu w jednym pliku. Tworzenie pakietów pozwala zmniejszyć liczbę żądań do serwera, co może poprawić czas ładowania strony.

Otwórz plik App\_Start/BundleConfig.cs. Dodaj następujący kod do metody RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Poprzednie](part-5.md)
> [dalej](part-7.md)
