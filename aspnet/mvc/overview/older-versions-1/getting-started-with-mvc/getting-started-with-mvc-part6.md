---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Dodawanie metody Create i tworzenie widoku | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 546c3e0a24ecd0d916c79e9ad12f62b926c760c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756525"
---
<a name="adding-a-create-method-and-create-view"></a>Dodawanie metody Create i tworzenie widoku
====================
przez [Scotta Hanselmana](https://github.com/shanselman)

> Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.


W tej sekcji użyjemy do zaimplementowania obsługi, które są niezbędne umożliwić użytkownikom tworzenie nowych filmów w naszej bazie danych. Możemy to zrobić poprzez Implementowanie akcji filmy/tworzenia adresu URL.

Implementowanie filmy/tworzenia adresu URL jest procesem dwóch kroków. Gdy użytkownik odwiedzi najpierw filmy/tworzenia adresu URL chcemy je pokazać formularza HTML, który można wypełnić aby wprowadzić nowy film. Następnie gdy użytkownik przesyła formularz i wpisy, dane z powrotem do serwera, chcemy pobrać przesłanych zawartość i zapisz go w naszej bazie danych.

Firma Microsoft będzie implementowana następujące dwa kroki w ramach dwóch metod Create() w ramach naszych klasy MoviesController. Jedną z metod będzie wyświetlana &lt;formularza&gt; , użytkownik powinien Wypełnij do utworzenia nowego filmu. Druga metoda będzie obsługiwać przetwarzanie przesłane dane, gdy użytkownik przesyła &lt;formularza&gt; z powrotem do serwera, a następnie zapisz nowy film w naszej bazie danych.

Poniżej znajduje się kod zostanie dodany do naszych klasy MoviesController implementacji:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Powyższy kod zawiera cały kod, który będziemy potrzebować w ramach kontrolera.

Przejdźmy teraz wdrożyć szablon Utwórz widok, który użyjemy, aby wyświetlić formularz do użytkownika. Firma Microsoft będzie w przypadku pierwszej metody tworzenia kliknij prawym przyciskiem myszy i wybierz pozycję "Dodaj widok", aby utworzyć szablon widoku dla naszego formularza filmu.

Wybierzemy, firma Microsoft zamierza przekazać Wyświetl szablon "Filmu" jako klasą danych widoku i wskazują, że chcemy "tworzenia szkieletu" szablon "Utwórz".

[![Dodawanie widoku](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Po kliknięciu przycisku Dodaj zostanie utworzony szablon widoku \Movies\Create.aspx dla Ciebie. Ponieważ Wybraliśmy "Utwórz" z menu rozwijanego "Wyświetl zawartość" okno dialogowe dodawania widoku automatycznie "szkieletu" niektóre domyślnej zawartości dla nas. Szkieletu utworzone HTML &lt;formularza&gt;, miejsce na błąd sprawdzania poprawności wiadomości do i od tworzenia szkieletów obsługującemu filmów, dla każdej właściwości klasy Nasze go utworzone etykiety i pola.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Ponieważ identyfikator naszej bazie danych automatycznie daje filmu, usuńmy tych pól modelu odniesienia. Identyfikator z widoku nasze Utwórz. Usuń wiersze 7 po &lt;legendy&gt;pola&lt;/legend&gt; którzy pokazują, pole ID, nie chcemy.

Załóżmy teraz utworzyć nowy film i dodać go do bazy danych. Utworzymy to zrobić, uruchamiając ponownie aplikację i odwiedź stronę "/ filmy" adres URL i kliknij przycisk "Utwórz" link można dodać nowy film.

[![Tworzenie — Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Po kliknięciu przycisku Utwórz, firma Microsoft będzie można księgowania Wstecz (za pośrednictwem protokołu HTTP POST) danych w tym formularzu metodę /Movies/Create, który właśnie utworzyliśmy. Podobnie jak podczas systemu trwało parametr "numTimes" i "name" spoza adres URL i automatycznie mechanizmowi parametrów dla metody wcześniej system będzie automatycznie umieść pola formularza z WPIS i mapować je do obiektu. W tym przypadku wartości z pól w formacie HTML, takich jak "ReleaseDate" i "Title", będą automatycznie umieszczone prawidłowe właściwości nowe wystąpienie klasy filmu.

Przyjrzyjmy się druga metoda tworzenia z naszych MoviesController ponownie. Zwróć uwagę, jak zajmuje się obiekt "Filmu" jako argumentu:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Ten obiekt filmu następnie został przekazany do naszych Utwórz metody akcji w wersji [HttpPost], a firma Microsoft zapisany w bazie danych, a następnie przekierowanie użytkownika z powrotem do metody akcji indeks(), z którym będą wyświetlane zapisany wynik na liście filmów:

[![Lista filmu — Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Firma Microsoft nie są ewidencjonowane nasze filmy są poprawne, jednak i bazy danych nie pozwalają na zapisywanie filmu przy użyciu bez tytułu. Dobrze byłoby, jeśli firma Microsoft może Monituj użytkownika, że przed bazy danych Zgłoszono błąd. Robimy to dalej przez dodanie obsługi sprawdzania poprawności do naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part5.md)
> [dalej](getting-started-with-mvc-part7.md)
