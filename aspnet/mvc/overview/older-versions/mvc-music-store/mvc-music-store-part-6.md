---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Część 6: Korzystanie z adnotacji danych do sprawdzania poprawności modelu | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 6 obejmuje za pomocą adnotacji danych dla modelu V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872419"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Część 6: Przy użyciu adnotacje danych na potrzeby sprawdzania poprawności modelu
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 6 obejmuje za pomocą adnotacji danych do weryfikacji modelu.


Mamy problem główne z naszych tworzenie i edytowanie formularzy: nie robią wszystkich sprawdzania poprawności. Możemy czynności, takie jak pozostaw puste wymagane pola lub typu litery w polu Cena i jest pierwszy błąd, który zajmiemy się z bazy danych.

Firma Microsoft łatwe dodawanie walidacji do naszej aplikacji przez dodanie adnotacji danych do naszej klasy modelu. Adnotacje danych umożliwiają opisano reguły, którą chcemy udostępnić stosowane do naszej właściwości modelu i ASP.NET MVC zajmie się ich wymuszania i wyświetlanie odpowiednie wiadomości do użytkowników.

## <a name="adding-validation-to-our-album-forms"></a>Dodawanie walidacji do naszej albumu formularzy

Użyjemy następujące atrybuty adnotacji danych:

- **Wymagane** — wskazuje, że właściwość jest polem obowiązkowym
- **Nazwa wyświetlana** — definiuje tekstu, firma Microsoft ma być użyty na pola formularza i komunikatów dotyczących sprawdzania poprawności
- **StringLength** — określa maksymalną długość pola ciągu
- **Zakres** — zapewnia maksymalne i minimalne wartości pola numerycznego
- **Powiąż** — zawiera listę pól do wykluczanie lub uwzględnianie podczas wiązania parametru lub formularza wartości właściwości modelu
- **ScaffoldColumn** — umożliwia ukrywanie pól z edytora formularzy

*Uwaga: Więcej informacji o weryfikacji modelu przy użyciu adnotacji danych atrybutów, w dokumentacji MSDN w*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Otwórz klasy albumu i dodaj następujące *przy użyciu* instrukcje do góry.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Następnie zaktualizuj właściwości, aby dodać atrybuty ekranu i sprawdzania poprawności, jak pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Są nam również zmieniono wykonawcy i Genre do właściwości wirtualnych. Dzięki temu Entity Framework do opóźnionego ładowania ich w razie potrzeby.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Po o dodać tych atrybutów do modelu albumu, naszych ekranu tworzenie i edytowanie natychmiast rozpocząć sprawdzanie poprawności pól i przy użyciu nazw wyświetlanych możemy wybraną (np. albumów graficznych adresu Url zamiast AlbumArtUrl). Uruchom aplikację i przejdź do /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Następnie firma Microsoft będzie Podziel niektórych reguł sprawdzania poprawności. Wprowadź wartości 0 i tytuł jest pusta. Po kliknięciu przycisku Utwórz przycisk, zostanie wyświetlone wyświetlane komunikaty o błędach weryfikacji przedstawiający pola, które nie spełnia warunków reguły sprawdzania poprawności się, że ma zdefiniowanego formularza.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testowanie weryfikacji po stronie klienta

Weryfikacja po stronie serwera jest bardzo ważne z perspektywy aplikacji, ponieważ użytkownicy mogą omijać weryfikacji po stronie klienta. Formularze sieci Web, które tylko zaimplementować weryfikację po stronie serwera mogą jednak zachowywać trzy istotne problemy.

1. Użytkownik będzie musiał odczekaj formularza do zaksięgowania zweryfikowane na serwerze, a odpowiedź do wysłania do przeglądarki.
2. Użytkownik nie otrzymywać natychmiast uzyskuje opinie, podczas ich Popraw pola tak, aby teraz przekazuje reguł sprawdzania poprawności.
3. Firma Microsoft tym jak zmarnowała zasobów serwera w celu wykonywania logiki sprawdzania poprawności zamiast korzystania z przeglądarki użytkownika.

Na szczęście szablony szkieletu ASP.NET MVC 3 mają weryfikacji po stronie klienta wbudowane, wymagających ani żadne dodatkowe czynności.

Wpisz pojedyncze litery w polu Tytuł spełnia wymagań sprawdzania poprawności, więc komunikatu weryfikacji jest od razu usunięte.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-5.md)
> [dalej](mvc-music-store-part-7.md)
