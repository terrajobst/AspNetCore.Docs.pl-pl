---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Dodawanie widoku | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: f55e558dd056e86bdd2310894959aef02a9d8de2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752279"
---
<a name="adding-a-view"></a>Dodawanie widoku
====================
przez [Scotta Hanselmana](https://github.com/shanselman)

> Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.


W tej sekcji użyjemy Przyjrzyj się, jak firma Microsoft może mieć klasy Nasze HelloWorldController nie pozostawia żadnych śladów hermetyzacji Generowanie odpowiedzi HTML z powrotem do klienta przy użyciu pliku szablonu widoku.

Zacznijmy od naszych metodą indeksu przy użyciu szablonu widoku. Nasze metoda jest wywoływana, indeksu i jest w HelloWorldController. Obecnie nasz metoda indeks() zwraca ciąg zawierający komunikat, który jest ustalona w obrębie klasy kontrolera.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Zmienimy teraz metoda indeksu, zamiast tego wyglądać następująco:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Teraz Dodajmy Wyświetl szablon do naszego projektu, który możemy użyć naszego indeks() metody. Aby to zrobić, kliknij prawym przyciskiem myszy, przy użyciu myszy gdzieś w trakcie wykonywania metody indeksu, a następnie kliknij przycisk Dodaj widok...

![obraz](getting-started-with-mvc-part3/_static/image1.png)

Zostanie wyświetlone okno dialogowe "Dodaj widok", która zapewnia nam kilka opcji, jak chcemy utworzyć szablon widoku, który może być używany przez naszych Index — metoda. Na razie nie należy wprowadzić zmiany i po prostu kliknij przycisk Dodaj.

[![Okno dialogowe dodawania widoku](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Po kliknięciu przycisku Dodaj nowy folder i nowy plik pojawi się w folderze rozwiązania, jak pokazano tutaj. Teraz masz folder HelloWorld w widokach i plik Index.aspx wewnątrz tego folderu.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Nowy plik indeksu jest również już otwarte i gotowy do edycji. Dodaj tekst w pierwszym &lt;h2&gt;indeksu&lt;/h2&gt; , takich jak "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Uruchom aplikację i odwiedź [ `http://localhost:xx/HelloWorld` ](http://localhostxx) ponownie w przeglądarce. Metoda indeksu kontrolera w tym przykładzie nie wykonać pracę, ale wywołać "return View()", co oznacza, że chcemy użyć pliku szablonu widoku do renderowania odpowiedzi do klienta. Ponieważ firma Microsoft nie jawnie określono nazwę pliku szablonu widoku, który będzie używany, za pomocą Index.aspx Wyświetl plik w folderze \Views\HelloWorld ustawiana domyślnie platformy ASP.NET MVC. Teraz widzimy, ciąg, który możemy zakodowane w naszym widoku.

[![Indeks - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Wygląda dość dobrze. Jednak zauważyć, że tytuł w przeglądarce jest wyświetlany komunikat "Index" i big tytuł na stronie jest wyświetlany komunikat "Moja aplikacja MVC." Zmieńmy te z nich.

### <a name="changing-views-and-master-pages"></a>Zmienianie widoków i stron wzorcowych

Po pierwsze zmienimy tekst "Moja aplikacja MVC." Ten tekst jest udostępniana i pojawia się na każdej stronie. Faktycznie wygląda na to w jednym miejscu w naszym kodzie, nawet jeśli nie znajduje się na każdej stronie w naszej aplikacji. Przejdź do folderu /Views/Shared w Eksploratorze rozwiązań, a następnie otwórz plik Site.Master. Ten plik jest nazywany stronę wzorcową i jest udostępniony "powłoka" używanego przez naszych pozostałych stronach.

Należy zauważyć jakiś tekst, który mówi ContentPlaceholder "MainContent", w tym pliku.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Ten symbol zastępczy jest, gdzie wszystkie strony utworzone pojawią się, "zawinięty" na stronie głównej. Spróbuj zmienić tytuł, a następnie uruchom aplikację, a następnie odwiedź wielu stronach. Można zauważyć, że zmiana jednego pojawi się na wielu stronach.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Teraz każdej strony będzie miał nagłówka podstawowego — to H1 - "Moje filmu aplikacji MVC." Obsługująca białego tekstu na górze strony jest udostępniany na wszystkich stronach.

Oto Site.Master w całości z naszych zmieniliśmy tytuł:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Teraz Zmieńmy tytuł strony indeksu.

Open /HelloWorld/Index.aspx. Ma dwa miejsca, aby zmienić. Po pierwsze tytuł wyświetlany w tytule przeglądarki, a następnie dodatkowej nagłówek - będącego również H2 —. Wprowadzimy je nieco, dzięki czemu można zobaczyć które fragmentem kodu zmienia której części aplikacji.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Uruchom aplikację, a następnie odwiedź /Movies. Zauważ, że tytuł przeglądarki, podstawowego nagłówka i dodatkowych nagłówków zostały zmienione. To można łatwo stworzyć duże zmiany w aplikacji, używając niewielkie zmiany do widoku.

[![Lista filmu — Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nasze trochę "dane" (w tym przypadku "Hello World!" komunikat) ma wartość twardych jednak kodowane. Mamy V (widoki), a następnie mamy C (kontrolery), ale nie M (Model) jeszcze. Krótko omówimy sposób tworzenia bazy danych i pobierania danych modelu z niego.

## <a name="passing-a-viewmodel"></a>Przekazywanie ViewModel

Zanim będziemy przejdź do bazy danych i porozmawiać na temat modeli, jednak najpierw Omówmy "Modele widoków." Są to obiekty, które reprezentują szablon widoku wymaga do renderowania odpowiedzi HTML z powrotem do klienta. One są zwykle tworzone przekazywane przez klasę kontrolera do widoku szablonu i powinna zawierać tylko dane, które wymaga Wyświetl szablon — i nie więcej.

Wcześniej z naszego przykładu HelloWorld naszych metody akcji Welcome() trwało nazwy i parametru numTimes i wyprowadzić dane do przeglądarki. Zamiast kontrolera renderowania odpowiedź bezpośrednio w dalszym ciągu, zamiast tego stwórzmy klasę mały do przechowywania danych, a następnie przekazać go za pośrednictwem do szablonu widoku do renderowania ponownie odpowiedzi HTML korzystania z niego. W ten sposób kontrolera jest rozpatrywany wraz z jedną z rzeczy i Wyświetl szablon innego — dzięki czemu możemy zachować czyste "separacji" w ramach naszej aplikacji.

Wróć do pliku HelloWorldController.cs i Dodaj nową klasę "WelcomeViewModel" i zmień metodę Zapraszamy w kontrolerze. Oto kompletny HelloWorldController.cs przy użyciu nowej klasy, w tym samym pliku.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Mimo że jest w wielu wierszach, nasze powitalnej metody jest w rzeczywistości tylko dwie instrukcje kodu. Pierwsza instrukcja pakiety naszych dwóch parametrów do obiektu ViewModel i przekazuje drugi wynikowy obiekt na widok.

Teraz należy szablonu widoku Zapraszamy! W metodzie powitalnej kliknij prawym przyciskiem myszy i wybierz pozycję Dodaj widok. Tym razem utworzymy Sprawdź "Utwórz widok silnie typizowane" i wybierz klasy Nasze WelcomeViewModel z listy rozwijanej. Ten nowy widok będzie tylko wiedzieć o WelcomeViewModels i inne typy obiektów.

> *Uwaga: Musisz mieć skompilowany raz po dodanie Twojego WelcomeViewModel dla pojawienie się na liście rozwijanej.*


Oto jak powinien wyglądać Twoje okno dialogowe dodawania widoku. Kliknij przycisk Dodaj. ![Dodaj widok w kółkach](getting-started-with-mvc-part3/_static/image10.png)

Dodaj następujący kod w obszarze &lt;h2&gt; w Twojej nowej Welcome.aspx. Utworzymy wprowadzić pętli i zacznij dowolną liczbę razy użytkownik wyświetla informację, że powinniśmy!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Należy również zwrócić uwagę podczas pisania, ponieważ widok o WelcomeViewModel powiedzieliśmy to (są one zamężną, należy pamiętać,?), uzyskujemy przydatne Intellisense każdorazowo odwołujemy się nasze obiektu modelu, jak w poniższym zrzucie ekranu:

[![Kod źródłowy NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Uruchom aplikację i odwiedź `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` ponownie. Teraz Przenosimy dane z adresu URL, jest automatycznie przekazywana do kontrolera, kontrolera pakiety danych na ViewModel i przekazuje tego obiektu, na naszej widoku. Widok nie wyświetla dane w postaci kodu HTML do użytkownika.

[![Wprowadzenie — Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Cóż, to był rodzaju "M" dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy wyjaśniono konto i bazę danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part2.md)
> [dalej](getting-started-with-mvc-part4.md)
