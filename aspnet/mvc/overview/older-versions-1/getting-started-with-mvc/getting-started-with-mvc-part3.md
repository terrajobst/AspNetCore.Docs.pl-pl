---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Dodawanie widoku | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 978d7980274c072ed559b54ed69ab86245b6c5a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-view"></a>Dodawanie widoku
====================
przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.


W tej sekcji zamierzamy przyjrzeć się jak mamy klasy Nasze HelloWorldController prawidłowo Hermetyzowanie generowania odpowiedzi HTML z powrotem do klienta przy użyciu pliku szablonu widoku.

Zacznijmy od naszych metodą indeksu przy użyciu szablonu widoku. Nasze metoda jest wywoływana indeksu i znajduje się w HelloWorldController. Obecnie nasze indeks() metoda zwraca ciąg z komunikatem jest zapisane na stałe należące do klasy kontrolera.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Możemy teraz zmienić metodę indeksu, aby zamiast tego wyglądać następująco:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Teraz Dodajmy szablonu widoku naszych projekt, który możemy użyć metody naszych indeks(). Aby to zrobić, prawym przyciskiem myszy gdzieś w środku metody indeksu, a następnie kliknij przycisk Dodaj widok...

![obraz](getting-started-with-mvc-part3/_static/image1.png)

Zostanie wyświetlone okno dialogowe "Dodaj widok", co zapewnia nam kilka opcji jak chcemy, aby utworzyć szablon widoku, który mogą być używane przez naszych metody indeksu. Obecnie nie wprowadzanie zmian i kliknij przycisk Dodaj.

[![Okno dialogowe dodawania widoku](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Po kliknięciu przycisku Dodaj nowy folder i nowy plik będą wyświetlane w folderze rozwiązania, jak pokazano poniżej. Masz teraz HelloWorld folderze Widoki i pliku Index.aspx wewnątrz tego folderu.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Nowy plik indeksu jest również już otwarty i jest gotowy do edycji. Dodaj tekst w pierwszym &lt;h2&gt;indeksu&lt;/h2&gt; , takich jak "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Uruchom aplikację, a następnie odwiedź [ `http://localhost:xx/HelloWorld` ](http://localhostxx) ponownie w przeglądarce. W naszym kontrolera, w tym przykładzie metody indeksu nie wykonać pracę, ale wywołać "return View()", który wskazany możemy użyć pliku szablonu widoku do renderowania odpowiedzi do klienta. Ponieważ firma Microsoft nie jawnie określono nazwę pliku szablonu widok do użycia, domyślnie przy użyciu pliku widoku Index.aspx znajdujących się w folderze \Views\HelloWorld ASP.NET MVC. Teraz widoczny ciąg, do którego możemy ustalony w naszym widoku.

[![Indeks — Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Wygląda bardzo dobre. Jednak zauważyć, że tytuł przeglądarki mówi, "Index", a duży tytuł na stronie mówi "Moja aplikacja MVC." Zmieńmy te.

### <a name="changing-views-and-master-pages"></a>Zmiana widoków i stron wzorcowych

Po pierwsze Zmieńmy tekst "Moja aplikacja MVC." Ten tekst jest udostępniana i pojawia się na każdej stronie. Faktycznie pojawia się tylko w jednym miejscu w naszym kodzie, mimo że znajduje się na każdej stronie w naszej aplikacji. Przejdź do folderu /Views/Shared w Eksploratorze rozwiązań i Otwórz plik Site.Master. Ten plik jest nazywany strony wzorcowej i jest on udostępniony "powłoka" używanego dla wszystkich innych stronach.

Zwróć uwagę, niektóre tekście elementu ContentPlaceholder "Znacznika" w tym pliku.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Ten symbol zastępczy jest, gdzie wszystkich stron tworzonych będą widoczne, "zawinięty", na stronie głównej. Spróbuj zmienić tytuł, a następnie uruchom aplikację, a następnie odwiedź wiele stron. Można zauważyć, że zmiana jednego pojawia się na wielu stronach.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Po każdej stronie będzie mieć podstawowego nagłówek — to H1 - "Mój MVC Movie aplikacji." Obsługująca białego tekstu na górze strony jest współużytkowana przez wszystkie strony.

Oto Site.Master w całości z naszych zmieniono tytuł:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Teraz Zmieńmy tytuł strony indeksu.

Open /HelloWorld/Index.aspx. Brak dwóch miejscach, aby zmienić. Po pierwsze tytuł wyświetlany w tytule przeglądarki, a następnie dodatkowej nagłówku -, który jest również H2 —. Będzie wprowadzić je nieco inne pozwala zobaczyć, które fragmentem kodu zmienia której części aplikacji

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Uruchom aplikację, a następnie odwiedź /Movies. Należy zauważyć, że tytuł przeglądarki, nagłówek głównej i dodatkowej nagłówki zostały zmienione. Jest łatwy do wprowadzania dużych zmian w aplikacji za pomocą niewielkich zmian do widoku.

[![Listy filmów - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nasze niewielki "data" (w tym przypadku "Witaj świecie!" komunikat) był twardych jednak na stałe. Mamy V (widoki) i mamy C (kontrolery), ale nie M (Model) jeszcze. Wkrótce omówimy jak utworzyć bazę danych i pobrać modelu danych.

## <a name="passing-a-viewmodel"></a>Przekazywanie ViewModel

Przed możemy przejdź do bazy danych i porozmawiać na temat modeli, umożliwia najpierw porozmawiać na temat "ViewModels." Są to obiekty reprezentujące Wyświetl szablon wymaga do renderowania odpowiedzi HTML z powrotem do klienta. One są zwykle tworzone przekazany przez klasę kontrolera do szablonu widoku i powinien zawierać tylko dane szablon widoku wymaga - i nie więcej.

Wcześniej z naszej próbki HelloWorld naszych metody akcji Welcome() trwało nazwy i parametru numTimes i output go w przeglądarce. Zamiast kontrolera renderowania odpowiedź bezpośrednio w dalszym ciągu, zamiast tego upewnijmy klasę mała do przechowywania danych, a następnie przekazać go za pośrednictwem do szablonu widoku do renderowania wstecz odpowiedzi HTML przy użyciu go. W ten sposób kontroler dotyczy rzecz i Wyświetl szablon innego — co pozwala na zachowanie czystą "separacji" w naszej aplikacji.

Wróć do pliku HelloWorldController.cs i Dodaj nową klasę "WelcomeViewModel" i zmień metodę Zapraszamy w kontrolerze. Poniżej przedstawiono pełną HelloWorldController.cs z nową klasę w tym samym pliku.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Nawet jeśli komputer jest w wielu wierszach, naszych powitalnej metoda jest naprawdę tylko dwa instrukcje kodu. Pierwsza instrukcja pakiety naszych dwóch parametrów do obiektu ViewModel, a drugi przekazuje obiekt wynikowy na widoku.

Teraz należy szablonu widoku Zapraszamy! W metodzie powitalnej kliknij prawym przyciskiem myszy i wybierz polecenie Dodaj widok. Teraz, firma Microsoft będzie Sprawdź "Utwórz widok jednoznacznie" i wybierz klasy Nasze WelcomeViewModel z listy rozwijanej. Ten nowy widok tylko będzie wiadomo o WelcomeViewModels i inne typy obiektów.

> *Uwaga: Musisz mieć skompilowany raz, po dodaniu WelcomeViewModel Twojego dla wyświetlani na liście rozwijanej.*


Oto, jak powinna wyglądać Twoje okno dialogowe dodawania widoku. Kliknij przycisk Dodaj. ![Dodaj widok kółku](getting-started-with-mvc-part3/_static/image10.png)

Dodaj ten kod w obszarze &lt;h2&gt; w Twoje nowe Welcome.aspx. Firma Microsoft będzie pętlę wprowadzić i zacznij tyle razy, ile użytkownik mówi się, że firma Microsoft powinien!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Należy również zwrócić uwagę podczas pisania który ponieważ informację firma Microsoft, to widok o WelcomeViewModel (są one traktowaniem, pamiętaj?) czy uzyskujemy przydatne Intellisense zawsze możemy odwołuje się do naszej modelu obiektu wyświetlanego na zrzucie ekranu poniżej:

[![NumTime Source Code](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Uruchom aplikację, a następnie odwiedź `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` ponownie. Teraz Przenosimy dane z adresu URL, jest on automatycznie przekazywane do kontrolera, kontrolera pakietów zapasową danych do ViewModel i przekazuje ten obiekt na naszych widoku. Widok nie wyświetla dane w postaci kodu HTML dla użytkownika.

[![Witamy! — Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Dobrze, która jest typu "M" dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy samouczka jest i utworzyć bazę danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part2.md)
> [dalej](getting-started-with-mvc-part4.md)
