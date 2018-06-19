---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: shanselman
description: Zaktualizowanej wersji, jeśli w tym samouczku jest dostępna, w tym miejscu przy użyciu programu Visual Studio 2013. Samouczek nowej używa platformy ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: c6ecd1ffdd53a629d0079d57b85c7f6db2f316ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869026"
---
<a name="adding-a-controller"></a>Dodawanie kontrolera
====================
przez [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Zaktualizowanej wersji, jeśli jest dostępna w tym samouczku [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nowe samouczku platformy ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.
> 
> 
> Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.


MVC to Model, widok, kontrolera. MVC to wzorzec do tworzenia aplikacji w taki sposób, że każda część ma odpowiedzialność, która różni się od siebie.

- Model: Dane aplikacji
- Widoki: Pliki szablonów będzie używać aplikacja do dynamicznego generowania odpowiedzi HTML.
- Kontrolery: Klas, które obsługują przychodzące żądania adresu URL do aplikacji, pobrać modelu danych, a następnie określ szablonów widoków, które renderują odpowiedź z powrotem do klienta

Firma Microsoft będzie można obejmujące wszystkie te pojęcia w tym samouczku i opisano, jak ich używać do tworzenia aplikacji.

Prawym przyciskiem myszy folder kontrolery w Eksploratorze rozwiązań i wybierając pozycję Dodaj kontroler, Utwórzmy nowy kontroler.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

"HelloWorldController" nowemu kontrolerowi nazwę, a następnie kliknij przycisk Dodaj.

[![Dodawanie kontrolera okna dialogowego](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Powiadomienie w Eksploratorze rozwiązań z prawej strony, który został utworzony nowy plik o nazwie HelloWorldController.cs i teraz otworzyć tego pliku w **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Utwórz dwa nowych metod, które wyglądają następująco wewnątrz nowej klasy publicznej HelloWorldController. Na przykład zostanie zwrócona ciąg HTML bezpośrednio z kontrolera.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Kontroler ma nazwę HelloWorldController i nowych metodę nosi nazwę indeksu. Uruchom aplikację ponownie, tak jak poprzednio (kliknij przycisk Odtwórz, lub naciśnij klawisz F5, aby to zrobić). Po ma uruchomienia przeglądarki, zmień ścieżkę w pasku adresu `http://localhost:xx/HelloWorld` wybrał gdzie xx jest numer niezależnie od komputera. Przeglądarka powinna wyglądać tak jak na poniższym zrzucie ekranu. W naszym metod powyżej firma Microsoft zwrócony ciąg przekazany do metody o nazwie "Zawartość". Firma Microsoft informację system tylko zwraca kod HTML i jak!

ASP.NET MVC wywołuje różnych klas kontrolera (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Domyślna logika mapowania używane przez program ASP.NET MVC używa formatu to do kontrolowania, jaki kod jest uruchamiany:

/ [Kontrolera] / [Nazwa akcji] / [parametry]

Pierwsza część adresu URL określa klasy kontrolera do wykonania. Dlatego /HelloWorld mapuje klasy HelloWorldController. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego /HelloWorld/Index spowoduje, że metoda indeks() klasy HelloWorldcontroller do wykonania. Zwróć uwagę, że Musieliśmy do odwiedzenia /HelloWorld powyżej i metoda został on zasugerowany indeksu. Jest to spowodowane metodę o nazwie "Index" jest domyślna metoda, która zostanie wywołana na kontrolerze, jeśli nie został jawnie określony.

[![Jest to Mój Akcja domyślna](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Teraz załóżmy odwiedź `http://localhost:xx/HelloWorld/Welcome.` teraz naszych powitalnej metody wykonane i zwrócony jego ciągu HTML.

Ponownie / [kontrolera] / [Nazwa akcji] / [parametry], aby kontroler HelloWorld i w tym przypadku jest to metoda — Zapraszamy!. Firma Microsoft nie zostało jeszcze wykonane parametrów.

[![Jest to metoda akcji-Zapraszamy!](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Załóżmy naszej próbki nieco zmodyfikować tak, aby firma Microsoft może przekazać niektóre informacje z adresu URL do kontrolera, na przykład w następujący sposób: / HelloWorld/Zapraszamy? name = Scott&amp;numtimes = 4. Należy wybrać metodę powitalnej uwzględnienie dwóch parametrów i aktualizacji go, takich jak poniżej. Należy pamiętać, że firma Microsoft używano aby wskazać, że numTimes parametr powinien domyślnie 1, jeśli nie zostanie przekazany w C# opcjonalny parametr funkcji.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Uruchom aplikację, a następnie odwiedź `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` zmiana wartości nazwy i numtimes według uznania. System automatycznie mapowane parametry nazwane z ciągu zapytania w pasku adresu w parametrach w metodę.

W obu tych przykładach kontrolera jest wykonanie całej pracy i ma zostać zwracanie HTML bezpośrednio. Zwykle nie chcemy naszych kontrolerów bezpośrednio - zwracanie HTML, ponieważ który kończy się być bardzo skomplikowane, aby kod. Zamiast tego zazwyczaj użyjemy oddzielny plik szablonu widoku ułatwia generowanie odpowiedzi HTML. Oto jak możemy można to zrobić. Zamknij przeglądarkę i wrócić do środowiska IDE.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part1.md)
> [dalej](getting-started-with-mvc-part3.md)
