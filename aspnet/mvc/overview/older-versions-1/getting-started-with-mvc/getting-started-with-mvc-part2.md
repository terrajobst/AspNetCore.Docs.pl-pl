---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: shanselman
description: Zaktualizowaną wersję, jeśli w tym samouczku jest dostępna w tym miejscu za pomocą programu Visual Studio 2013. Nowe samouczku ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z t...
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: afe1182bca0fe58e1df162c5027df35992eeef38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809913"
---
<a name="adding-a-controller"></a>Dodawanie kontrolera
====================
przez [Scotta Hanselmana](https://github.com/shanselman)

> > [!NOTE]
> > Zaktualizowanej wersji, jeśli w tym samouczku jest dostępny [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nowe samouczku ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.
> 
> 
> Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.


MVC to Model, widok i kontroler. MVC to wzorzec do tworzenia aplikacji w taki sposób, że każda część ma odpowiedzialność, która różni się od siebie.

- Model: Dane aplikacji
- Widoki: Pliki szablonów aplikacji użyje do dynamicznego generowania odpowiedzi HTML.
- Kontrolery: Klas, które obsługują przychodzące żądania adres URL do aplikacji, pobrać modelu danych, a następnie określ Przeglądanie szablonów, które renderują odpowiedź z powrotem do klienta

Firma Microsoft będzie obejmujące wszystkie te pojęcia, w tym samouczku a pokazują, jak ich używać do tworzenia aplikacji.

Utworzymy nowy kontroler, klikając prawym przyciskiem myszy folder kontrolerów w Eksploratorze rozwiązania i wybierając polecenie Dodaj kontroler.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Nadaj nazwę nowemu kontrolerowi "HelloWorldController", a następnie kliknij przycisk Dodaj.

[![Dodaj kontroler, okno dialogowe](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Zwróć uwagę, w Eksploratorze rozwiązań po prawej stronie, który został utworzony nowy plik o nazwie HelloWorldController.cs, i teraz otworzyć tego pliku w **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Utworzyć dwie nowe metody, które wyglądają następująco wewnątrz nowej klasy publicznej HelloWorldController. Na przykład zostanie zwrócona ciąg HTML bezpośrednio z poziomu kontrolera.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Twój kontroler o nazwie HelloWorldController i nową metodę nosi nazwę indeksu. Uruchom aplikację ponownie, tak jak wcześniej (kliknij przycisk Odtwórz, lub naciśnij klawisz F5, aby to zrobić). Zmień ścieżkę w pasku adresu, po uruchomieniu przeglądarki `http://localhost:xx/HelloWorld` wybrał gdzie xx jest numer niezależnie od komputera. Przeglądarka powinna wyglądać tak jak poniższy zrzut ekranu. W naszym powyższej metody możemy zwracane, ciąg przekazywany do metody o nazwie "Zawartość". Powiedzieliśmy systemu po prostu zwraca kod HTML i tak!

ASP.NET MVC wywołuje różne klasy kontrolera (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Domyślne mapowanie logikę używaną przez platformę ASP.NET MVC używa formatu to kontrolować, jaki kod jest uruchamiany:

/ [Controller] / [Nazwa akcji] / [parametry]

Pierwszą część adresu URL określa klasę kontrolera do wykonania. Dlatego /HelloWorld mapuje klasy HelloWorldController. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego /HelloWorld/Index spowoduje, że metoda indeks() klasy HelloWorldcontroller do wykonania. Należy zauważyć, że mieliśmy do odwiedzenia /HelloWorld powyżej i metodę, która została niejawnego indeksu. Jest to spowodowane metodę o nazwie "Index" jest to domyślna metoda, która zostanie wywołana na kontrolerze, jeśli nie jest jawnie określona.

[![To jest Moja Akcja domyślna](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Teraz przejdźmy do `http://localhost:xx/HelloWorld/Welcome.` teraz naszych powitalnej metody wykonywane i zwrócony jego ciągu HTML.

Ponownie / [Controller] / [Nazwa akcji] / [parametry] kontroler zostanie HelloWorld i Witaj w tym przypadku jest to metoda. Mamy jeszcze nie parametry to jeszcze zrobione.

[![Jest to metoda akcji-Zapraszamy!](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Zmodyfikujmy naszego przykładu nieco tak, że możemy przekazać niektóre informacje z adresu URL do kontrolera, na przykład następująco: / HelloWorld/Witaj? nazwa = Scott&amp;numtimes = 4. Zmienić metodę powitalnej obejmujący dwa parametry i aktualizacji, takich jak go poniżej. Należy pamiętać, aby wskazać, że numTimes parametr powinien domyślnie 1, jeśli nie zostanie przekazany w używaliśmy funkcji opcjonalny parametr języka C#.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Uruchom aplikację i odwiedź `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` zmiana wartości nazwy i numtimes, jak chcesz. System automatycznie mapowane nazwane parametry ciągu zapytania w pasku adresu do parametrów w metodzie.

W obu tych przykładach kontroler zostało, wykonując całą pracę i ma zostać zwracanie HTML bezpośrednio. Zwykle nie chcemy naszych kontrolerów zwracanie HTML bezpośrednio — od czasu, który kończy się on bardzo skomplikowane, aby kod. Zamiast tego zazwyczaj użyjemy oddzielny plik szablonu widoku ułatwiający Generowanie odpowiedzi HTML. Przyjrzyjmy się jak możemy to zrobić. Zamknij przeglądarkę i powrócić do IDE.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part1.md)
> [dalej](getting-started-with-mvc-part3.md)
