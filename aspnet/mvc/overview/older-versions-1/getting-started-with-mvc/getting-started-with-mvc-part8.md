---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Dodawanie kolumny do modelu | Dokumentacja firmy Microsoft
author: shanselman
description: "Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 17ee105f596319423ac83cf718683ed293f952f3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-column-to-the-model"></a>Dodawanie kolumny do modelu
====================
przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.


W tej sekcji zamierzamy przeprowadzenie jak możemy wprowadzić zmiany w schemacie naszej bazie danych i obsługi zmian w naszej aplikacji.

Możemy dodać kolumny "Klasyfikacja" do tabeli filmu. Wróć do środowiska IDE, a następnie kliknij przycisk pomocy Eksploratora bazy danych. W tabeli filmu kliknij prawym przyciskiem myszy i wybierz Otwórz definicję tabeli.

Dodaj kolumnę "Klasyfikacji", jak pokazano poniżej. Ponieważ nie mamy teraz wszystkie klasyfikacje, kolumnie mogą dopuszczać wartości null. Kliknij przycisk Zapisz.

[![Edytowanie tabeli filmy](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Następnie wróć do Eksploratora rozwiązań i otwarcie pliku Movies.edmx (który znajduje się w folderze \Models). Kliknij prawym przyciskiem myszy powierzchnię projektu (obszar biały) i wybierz Model aktualizacji z bazy danych.

[![Filmów - Microsoft Visual Web Developer Express 2010 (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Spowoduje to uruchomienie "Kreatora aktualizacji". Kliknij kartę odświeżania w nim, a następnie kliknij przycisk Zakończ. Klasy modelu nasze filmu zostaną zaktualizowane o nową kolumnę.

![Kreator aktualizacji (2)](getting-started-with-mvc-part8/_static/image5.png)

Po kliknięciu przycisku Zakończ, można zauważyć, że nowa kolumna klasyfikacji został dodany do filmu jednostki w modelu.

[![Film jednostki](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Dodaliśmy kolumny w modelu bazy danych, ale widoki nie wiadomo o nim.

## <a name="update-views-with-model-changes"></a>Widoki aktualizacji z zmiany modelu

Istnieje kilka sposobów firma Microsoft może zaktualizować naszych szablonów widoku, aby odzwierciedlić nową kolumnę klasyfikacji. Ponieważ utworzyliśmy tych widoków generując za pomocą okna dialogowego Dodaj widok, firma Microsoft może je usunąć i ponownie utwórz je ponownie. Jednak zwykle osób będzie już zostały wprowadzone zmiany, aby ich wyświetlanie szablonów z początkowej generowania szkieletu i będzie można dodać lub usunąć pola ręcznie, tak jak robiliśmy z polem Identyfikator Utwórz.

Otwarcie szablonu \Views\Movies\Index.aspx i Dodaj &lt;th&gt;klasyfikacji&lt;/th&gt; nagłówek tabeli filmu. Po dodaniu min po rodzaju. Następnie w tym samym położeniu kolumny, ale przedstawione, Dodaj linię do wyjściowego naszej nowej klasyfikacji.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Nasze ostateczny szablon Index.aspx będzie wyglądać następująco:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Załóżmy następnie otworzyć szablon \Views\Movies\Create.aspx i Dodaj etykiety i pola tekstowego do naszej nowej właściwości klasyfikacji:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Nasze ostateczny szablon Create.aspx będzie wyglądać następująco i Zmieńmy naszej przeglądarki tytuł, jak i pomocniczy &lt;h2&gt; tytuł, aby przypominać "Tworzenie filmu" są nam w tym miejscu!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Uruchom aplikację i teraz już wiem nowego pola w bazie danych dodawanej do tworzenia strony. Dodawanie nowych filmu — tym razem z klasyfikacją — i kliknij przycisk Utwórz.

[![Tworzenie filmu — Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Po kliknięciu przycisku Utwórz są wysyłane do strony indeksu w przypadku, gdy użytkownik nowych filmu znajduje się nowa kolumna klasyfikacji w bazie danych

[![Listy filmów - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

W tym samouczku podstawowe otrzymano pracę wprowadzania kontrolerów, kojarzenie ich z widokami i przekazywanie wokół ustalony danych. Następnie możemy utworzyć i zaprojektowane bazy danych i umieść niektóre dane w. Możemy pobrać dane z bazy danych i on wyświetlany w tabeli HTML. Następnie dodano Tworzenie formularza, który zezwala użytkownikowi na dodanie danych do bazy danych się z aplikacji sieci Web. Możemy dodać sprawdzania poprawności, a następnie wprowadzone weryfikacji używany język JavaScript po stronie klienta. Na koniec możemy zmienić bazy danych w celu uwzględnienia nowej kolumny danych, a następnie zaktualizować naszych dwie strony, aby utworzyć i wyświetlić te nowe dane.

Można teraz zachęca do przejść do poziomu pośredniego z naszym samouczkiem "[magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" oraz wiele plików wideo i zasobów w [https://asp.net/mvc](https://asp.net/mvc) nawet więcej informacji o platformie ASP.NET MVC!

Owocnej pracy.

- Scott Hanselman - [http://hanselman.com](http://hanselman.com) i [ @shanselman ](http://twitter.com/shanselman) w serwisie Twitter.

>[!div class="step-by-step"]
[Poprzednie](getting-started-with-mvc-part7.md)
