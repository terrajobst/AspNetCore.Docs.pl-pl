---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Dodawanie kolumny do modelu | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 22a6c4e5a07e81d5876cc442e68926094e3a243d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751855"
---
<a name="adding-a-column-to-the-model"></a>Dodawanie kolumny do modelu
====================
przez [Scotta Hanselmana](https://github.com/shanselman)

> Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.


W tej sekcji użyjemy opisano jak firma Microsoft może dokonać zmian schematu w naszej bazie danych i obsługi zmian w naszej aplikacji.

Dodajmy "Ocena" kolumny do tabeli filmu. Wróć do IDE, a następnie kliknij pozycję Eksplorator bazy danych. Kliknij prawym przyciskiem myszy tabelę filmu, a następnie wybierz Otwórz definicję tabeli.

Dodaj kolumnę "Ocena", jak pokazano poniżej. Ponieważ firma Microsoft nie mają teraz wszystkie klasyfikacje kolumny można zezwolić na wartości null. Kliknij przycisk Zapisz.

[![Edytowanie tabel filmy](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Następnie wróć do Eksploratora rozwiązań i Otwórz plik Movies.edmx (która znajduje się w folderze \Models). Kliknij prawym przyciskiem myszy na powierzchni projektowej (białego obszaru) i wybierz Model aktualizacji z bazy danych.

[![Filmy — Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Spowoduje to uruchomienie kreatora"aktualizacji". Kliknij kartę odświeżania w nim, a następnie kliknij przycisk Zakończ. Klasa modelu nasz film zostaną zaktualizowane przy użyciu nowej kolumny.

![Kreator aktualizacji (2)](getting-started-with-mvc-part8/_static/image5.png)

Po kliknięciu przycisku Zakończ, zobaczysz, że nowa kolumna Ocena został dodany do jednostki filmu w naszym modelu.

[![Jednostki filmu](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Dodaliśmy kolumny w modelu bazy danych, ale widoków nie wiadomo o nim.

## <a name="update-views-with-model-changes"></a>Zaktualizuj widoków, wprowadzając zmiany w modelu

Istnieje kilka sposobów aktualizujemy nasze szablony widoku, aby odzwierciedlić nową kolumnę klasyfikacji. Ponieważ utworzyliśmy te widoki, generując je za pomocą okna dialogowego Dodaj widok, można je usunąć i ponownie utwórz je ponownie. Jednak zazwyczaj osób będzie zostały już wykonane modyfikacje ich Przeglądanie szablonów początkowej generowania szkieletu i trzeba dodać lub usunąć pola, ręcznie, tak jak robiliśmy z polem Identyfikator tworzenia.

Otwórz szablon \Views\Movies\Index.aspx i Dodaj &lt;th&gt;ocena&lt;/th&gt; porównanie tabeli filmu. Po dodaniu wydobywanie po gatunku. Następnie w tym samym położeniu kolumny, ale pod nią, Dodaj wiersz w danych wyjściowych naszych Nowa klasyfikacja.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Ostateczny szablon Index.aspx będzie wyglądać następująco:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Możemy następnie otworzyć szablon \Views\Movies\Create.aspx i dodawanie etykiety i pole tekstowe dla naszej nowej właściwości klasyfikacji:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Nasze ostateczny szablon Create.aspx będzie wyglądać następująco, a Zmieńmy tytuł i pomocniczy naszej przeglądarki &lt;h2&gt; tytułu na wartość podobną "Tworzenie filmu" gdy mamy tutaj!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Uruchom aplikację, a teraz masz nowe pole w bazie danych zostały dodane do tworzenia strony. Dodaj nowy film — tym razem z oceną — i kliknij przycisk Utwórz.

[![Utwórz film — Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Po kliknięciu pozycji Utwórz nastąpiło do strony indeksu w przypadku, gdy użytkownik nowego filmu znajduje się nowa kolumna oceny w bazie danych

[![Lista filmu — Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

W tym samouczku podstawowe stało się ułatwiające rozpoczęcie pracy, dzięki czemu kontrolerów, skojarzenie ich z widoków i przekazanie wokół zakodowanych danych. Następnie możemy utworzone i przeznaczone do bazy danych niektóre dane umieszczane w. Firma Microsoft może pobrać dane z bazy danych i on wyświetlany w tabeli HTML. Następnie dodaliśmy Utwórz formularz, który umożliwia użytkownikowi dodawanie danych do bazy danych samodzielnie z poziomu aplikacji sieci Web. Możemy dodać sprawdzanie poprawności, a następnie wykonywane sprawdzanie poprawności używany język JavaScript po stronie klienta. Na koniec możemy zmienić bazę danych, aby uwzględnić nową kolumnę danych, a następnie zaktualizowane nasze dwie strony, aby utworzyć i wyświetlić te nowe dane.

Teraz zachęcam Cię do przejdziemy do Nasz samouczek dotyczący poziomu pośredniego "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", a także wiele filmów wideo i zasobów w [ https://asp.net/mvc ](https://asp.net/mvc) się jeszcze bardziej poświęcone platformie ASP.NET MVC!

Ciesz się!

- Scott Hanselman — [ http://hanselman.com ](http://hanselman.com) i [ @shanselman ](http://twitter.com/shanselman) w serwisie Twitter.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part7.md)
