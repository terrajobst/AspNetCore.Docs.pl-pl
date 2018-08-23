---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Część 6: Przy użyciu adnotacje danych na potrzeby weryfikacji modelu | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 6 obejmuje korzystanie z adnotacji danych dla modelu V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b666c3cef0b09c6d68cee581a3c27c08e3357cca
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753302"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Część 6: Przy użyciu adnotacje danych na potrzeby weryfikacji modelu
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 6 obejmuje przy użyciu adnotacje danych na potrzeby weryfikacji modelu.


Mamy poważnym problemem z naszych formularzami tworzyć i edytować: nie robią wszystkich sprawdzania poprawności. Możemy to zrobić na przykład pozostawić wymagane pola puste lub liter, wpisz w polu Cena, a pierwszy błąd, który zobaczymy to z bazy danych.

Firma Microsoft można łatwo dodać sprawdzanie poprawności do naszej aplikacji, dodając adnotacje danych do naszych zajęć modelu. Adnotacje danych umożliwiają nam opisano reguły, którą chcemy stosowane do naszych właściwości modelu i platformy ASP.NET MVC zajmie się ich wymuszania i wyświetlania odpowiednie komunikaty naszych użytkowników.

## <a name="adding-validation-to-our-album-forms"></a>Dodawanie walidacji do naszych albumu formularzy

Użyjemy następujących atrybutów danych adnotacji:

- **Wymagane** — wskazuje, że właściwość jest polem wymaganym
- **DisplayName** — Określa tekst, chcemy, aby używane pola formularza i komunikatów dotyczących sprawdzania poprawności
- **StringLength** — określa maksymalną długość pola ciągu
- **Zakres** — zapewnia maksymalne i minimalne wartości pola numerycznego
- **Powiąż** — zawiera listę pól, aby wykluczyć lub uwzględnić podczas tworzenia wiązania parametru lub formularz wartości do właściwości modelu
- **ScaffoldColumn** — umożliwia ukrycie pola w edytorze formularzy

*Uwaga: Aby uzyskać więcej informacji o weryfikacji modelu przy użyciu adnotacji danych atrybutów, zobacz dokumentację MSDN w*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Otwórz klasę albumu i dodaj następującą *przy użyciu* instrukcji u góry.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Następnie zaktualizuj właściwości, aby dodać wyświetlanie i sprawdzanie poprawności atrybutów, jak pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Są nam również zmieniliśmy wykonawcy i gatunku do właściwości wirtualnego. Dzięki temu Entity Framework z opóźnieniem ładować je zgodnie z potrzebami.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Po mających te atrybuty są dodawane do nasz model fotograficzne, nasze ekranu tworzyć i edytować natychmiast rozpocząć sprawdzanie poprawności pól i przy użyciu nazw wyświetlanych Wybraliśmy (np. albumu grafikę adresu Url zamiast AlbumArtUrl). Uruchom aplikację, a następnie przejdź do /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Następnie możemy zepsuć niektórych reguł sprawdzania poprawności. Wprowadź cenie 0, a tytuł jest pusta. Po kliknięciu przycisku Utwórz widzimy formularzu wyświetlane przy użyciu komunikatów o błędach weryfikacji wyświetlane pola, które nie spełnia warunków reguły sprawdzania poprawności, gdy zdefiniowaliśmy.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testowanie poprawności po stronie klienta

Weryfikacja po stronie serwera jest bardzo ważne z perspektywy aplikacji, ponieważ użytkownicy mogą omijać weryfikacji po stronie klienta. Jednakże formularze sieci Web, które tylko zaimplementować weryfikację po stronie serwera następującej liczby etapów stwierdzono trzy istotne problemy.

1. Użytkownik będzie musiał czekać do formularza, który ma zostać opublikowany, sprawdzania poprawności na serwerze, a odpowiedź do wysłania do przeglądarki.
2. Użytkownik nie natychmiast otrzymać opinię podczas Popraw polem, tak, aby teraz przekazuje reguł sprawdzania poprawności.
3. Firma Microsoft jest marnowania zasobów serwera, aby wykonać logikę weryfikacji zamiast korzystanie z przeglądarki użytkownika.

Na szczęście szablony szkieletu ASP.NET MVC 3 mają weryfikacji po stronie klienta wbudowane, wymagania żadne dodatkowe czynności w inny sposób.

Wpisz pojedynczą literą, w polu Tytuł spełnia wymagania sprawdzania poprawności, więc komunikat sprawdzania poprawności jest od razu usunięte.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-5.md)
> [dalej](mvc-music-store-part-7.md)
