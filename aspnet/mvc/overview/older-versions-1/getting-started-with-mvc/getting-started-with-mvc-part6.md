---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Dodawanie metody i tworzenie widoku | Dokumentacja firmy Microsoft
author: shanselman
description: "Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 36b3d6ef0432292f21ecd8f29ea2d88ee8867436
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-create-method-and-create-view"></a>Dodawanie metody i tworzenie widoku
====================
przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.


W tej sekcji zamierzamy Obsługa niezbędne umożliwić użytkownikom tworzenie nowości w naszej bazie danych. Firma Microsoft będzie w tym celu wykonania akcji filmów/Utwórz adres URL.

Implementowanie filmów/Utwórz adres URL jest procesem dwóch kroku. Gdy użytkownik odwiedza najpierw filmów/Utwórz adres URL chcemy ich wyświetlania formularza HTML, który można wypełnić wprowadzenia nowych filmu. Następnie gdy użytkownik przesyła formularz i wpisów, które dane z powrotem do serwera, chcemy się pobrać oczekujących na opublikowanie zawartość i zapisz go w naszej bazie danych.

W naszym klasy MoviesController będzie wprowadzania tych dwóch kroków w ramach dwóch metod Create(). Przedstawia jedną metodę &lt;formularza&gt; czy użytkownik powinien wypełnić do utworzenia nowego filmu. Druga metoda obsługi przetwarzania przesłane dane, gdy użytkownik przesyła &lt;formularza&gt; z powrotem do serwera, a następnie zapisz nowy filmu w naszej bazie danych.

Poniżej znajduje się kod zostanie dodany do naszej klasy MoviesController implementacji:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Powyższy kod zawiera wszystkie kodu, który będą potrzebne w ramach kontrolera.

Umożliwia teraz wdrożyć szablon tworzenia widoku, który zostanie użyta do wyświetlania formularza do użytkownika. Firma Microsoft będzie pierwszy metody Create kliknij prawym przyciskiem myszy i wybierz "Dodaj widok", aby utworzyć szablon widoku dla naszego formularza filmu.

Wybierzemy możemy są, przechodząc do przekazania Wyświetl szablon "Filmu" jak jego Klasa danych widoku, które wskazują, że chcemy "utworzyć szkielet" szablon "Utwórz".

[![Dodaj widok](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Po kliknięciu przycisku Dodaj \Movies\Create.aspx wyświetlanie szablonu zostaną utworzone automatycznie. Ponieważ wybrano "Utwórz" z listy rozwijanej "Wyświetl zawartość", okno dialogowe dodawania widoku automatycznie "szkieletu" zawartość domyślne firmie Microsoft. Rusztowania utworzone HTML &lt;formularza&gt;, miejsce na błąd sprawdzania poprawności komunikaty Przejdź, i ponieważ szkieletów obsługującemu filmów, dla każdej właściwości klasy Nasze utworzony etykiety i pola.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Ponieważ naszej bazie danych jest automatycznie daje filmu Identyfikatora, umożliwia usunięcie tego modelu referencyjnego. Identyfikator z naszych tworzenia widoku. Usuń 7 wierszy po &lt;legendy&gt;pola&lt;/legend&gt; zgodnie z ich pokazać w polu Identyfikatora firma Microsoft nie ma.

Umożliwia teraz tworzenie nowych filmu i dodaj go do bazy danych. Firma Microsoft będzie to zrobić, uruchamiając ponownie aplikację i odwiedź "/ filmy" adres URL i kliknij przycisk "Utwórz" łącze do dodawania nowych filmu.

[![Tworzenie - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Kliknięcie przycisku Utwórz, firma Microsoft będzie można zamieszczając Wstecz (za pośrednictwem protokołu HTTP POST) danych w tym formularzu do metody /Movies/Create, którą właśnie utworzyliśmy. Podobnie jak podczas system automatycznie trwało parametru "numTimes" i "name" poza adres URL i mechanizmowi parametry dla metody wcześniej system automatycznie pobrać pola formularza z ogłoszenie (POST) i mapować je do obiektu. W tym przypadku wartości pól w formacie HTML, takie jak "ReleaseDate" i "Title", zostaną automatycznie wprowadzone w prawidłowe właściwości nowe wystąpienie klasy filmu.

Przyjrzyjmy się drugi metody Create z naszych MoviesController ponownie. Zwróć uwagę, jak zajmuje obiektu "Filmu" jako argumentu:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Ten obiekt filmu następnie został przekazany do wersji [HttpPost] naszych Utwórz metody akcji i firma Microsoft zapisany w bazie danych, a następnie przekierowany użytkownika z powrotem do metody akcji indeks(), który będzie widoczny zapisane wyniki w listy filmów:

[![Listy filmów - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Firma Microsoft nie są sprawdzania, jeśli naszych filmów są poprawne, ale i bazy danych nie zezwalają na Zapisz film z bez tytułu. Byłoby nieuprzywilejowany możemy podać użytkownik, który wygenerował błąd przed bazy danych. Firma Microsoft zrobić dalej to przez dodanie obsługi sprawdzania poprawności do naszej aplikacji.

>[!div class="step-by-step"]
[Poprzednie](getting-started-with-mvc-part5.md)
[dalej](getting-started-with-mvc-part7.md)
