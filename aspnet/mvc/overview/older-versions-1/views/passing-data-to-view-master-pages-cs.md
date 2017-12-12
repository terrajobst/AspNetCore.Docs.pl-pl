---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: Przekazywanie danych do strony wzorcowej widoku (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: "Celem tego samouczka jest wyjaśnienie, jak można przekazać dane z kontrolera do widoku strony wzorcowej. Omówione dwie strategie przekazywania danych do widoku m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: b8bc8ce0690d2e45877be75011d8883facbc74a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="passing-data-to-view-master-pages-c"></a>Przekazywanie danych do strony wzorcowej widoku (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> Celem tego samouczka jest wyjaśnienie, jak można przekazać dane z kontrolera do widoku strony wzorcowej. Omówione dwie strategie przekazywania danych do strony wzorcowej widoku. Najpierw omówimy łatwe rozwiązania, który daje w aplikacji, która jest trudne w utrzymaniu. Następnie omówione dużo lepszym rozwiązaniem, które wymaga nieco więcej pracy początkowej, ale powoduje utrzymaniu znacznie więcej aplikacji.


## <a name="passing-data-to-view-master-pages"></a>Przekazywanie danych do strony wzorcowej widoku

Celem tego samouczka jest wyjaśnienie, jak można przekazać dane z kontrolera do widoku strony wzorcowej. Omówione dwie strategie przekazywania danych do strony wzorcowej widoku. Najpierw omówimy łatwe rozwiązania, który daje w aplikacji, która jest trudne w utrzymaniu. Następnie omówione dużo lepszym rozwiązaniem, które wymaga nieco więcej pracy początkowej, ale powoduje utrzymaniu znacznie więcej aplikacji.

### <a name="the-problem"></a>Problem

Załóżmy, że tworzysz filmu aplikacji bazy danych i mają być wyświetlane na liście kategorii filmu na każdej stronie w aplikacji (zobacz rysunek 1). Załóżmy, ponadto, czy lista kategorii film jest przechowywana w bazie danych. W takim przypadku powinna mieć do pobrania kategorii z bazy danych i renderowania listy kategorii filmu w widoku strony wzorcowej.


[![Wyświetlanie kategorii filmu w widoku strony wzorcowej](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**Rysunek 01**: wyświetlanie kategorii filmu w widoku strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](passing-data-to-view-master-pages-cs/_static/image3.png))


Oto problem. Sposób pobierania listy kategorii filmu z strony wzorcowej? Jest kuszące bezpośrednio wywołać metody klasach modeli na stronie głównej. Innymi słowy jest kuszące, aby uwzględnić kod do pobierania danych z prawej stronie głównej bazy danych. Jednak pomijanie kontrolerów MVC dostęp do bazy danych naruszyłoby czyste rozdzielenie dotyczy jest jednym z podstawowych zalet tworzenia aplikacji MVC.

W aplikacji MVC ma wszystkie interakcje między widoków MVC i modelu MVC, które mają być obsługiwane przez kontrolerów MVC. Powoduje to separacji w utrzymaniu, dostosowywalne i testować aplikacji.

W aplikacji MVC wszystkie dane przekazywane do widoku — w tym widoku strony wzorcowej — powinny być przekazywane do widoku przez akcji kontrolera. Ponadto dane powinien zostać przekazany dzięki wykorzystaniu danych widoku. W pozostałej części tego samouczka I Sprawdź na dwa sposoby przekazywania danych widoku do strony wzorcowej widoku.

### <a name="the-simple-solution"></a>Prostym rozwiązaniem

Zacznijmy z Najprostszym rozwiązaniem w celu przekazywania danych widoku z kontrolera do widoku strony wzorcowej. Najprostszym rozwiązaniem jest przekazywanie danych widoku dla strony wzorcowej każdej akcji kontrolera.

Należy wziąć pod uwagę kontrolera 1 wyświetlania. Udostępnia ona dwie akcje o nazwie `Index()` i `Details()`. `Index()` Metoda akcji zwraca co film filmy tabeli bazy danych. `Details()` Metoda akcji zwraca co filmu w filmu określonej kategorii.

**1 — Lista`Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Należy zauważyć, że zarówno indeks() i akcje Details() dodać dwóch elementów do wyświetlania danych. Akcja indeks() dodaje dwa klucze: kategorie i filmy. Klucz kategorie reprezentuje listę filmu kategorie wyświetlane w widoku strony wzorcowej. Klucz filmy reprezentuje listę filmów wyświetlanych przez indeks strony widoku.

Akcja Details() dodaje również dwa klucze o nazwie kategorii i filmy. Klucz kategorie jeszcze raz reprezentuje listę filmu kategorie wyświetlane w widoku strony wzorcowej. Klucz filmy reprezentuje listę filmów w określonej kategorii wyświetlanych przez strony widoku szczegółów (patrz rysunek 2).


[![W widoku szczegółów](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**Rysunek 02**: widok szczegółów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](passing-data-to-view-master-pages-cs/_static/image6.png))


Widok indeks znajduje się w wyświetlania 2. Iteruje po prostu Lista filmów reprezentowany przez element filmów w widoku danych.

**2 — Lista`Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

Strony wzorcowej widoku znajduje się w 3 wyświetlania. Widok strony wzorcowej iteracje i renderuje wszystkie kategorie filmu reprezentowany przez element kategorii z danymi widoku.

**3 — lista`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Wszystkie dane są przekazywane do widoku oraz widoku strony wzorcowej za pośrednictwem widoku danych. To jest prawidłowy sposób, aby przekazać dane do strony wzorcowej.

Tak co to jest problem z tego rozwiązania? Problem polega na tym, że to rozwiązanie narusza zasady suchej (nie powtarzaj siebie). Każdy akcji kontrolera należy dodać tej samej listy kategorii film, aby wyświetlić dane. Mające zduplikowany kod w aplikacji utrudnia aplikacji znacznie Obsługa dostosowania i modyfikować.

### <a name="the-good-solution"></a>Dobre rozwiązanie

W tej sekcji omówione alternatywnych i lepsze, rozwiązanie do przekazywania danych z akcji kontrolera do widoku strony wzorcowej. Zamiast opcji dodawania kategorii filmu dla strony wzorcowej w każdej akcji kontrolera, możemy dodać filmu kategorie do widoku danych tylko raz. Wszystkie dane widoku, używany przez strony wzorcowej widoku jest dodawany do kontrolera aplikacji.

Klasa ApplicationController znajduje się w listę 4.

**Wyświetlanie listy 4.`Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Istnieją trzy elementy, które powinny uwagi dotyczące kontrolera aplikacji na wyświetlanie 4. Najpierw należy zauważyć, że klasa dziedziczy z klasy podstawowej System.Web.Mvc.Controller. Kontroler aplikacji jest klasy kontrolera.

Po drugie Zwróć uwagę, że klasa kontrolera aplikacji jest klasą abstrakcyjną. Klasy abstrakcyjnej jest klasa, która musi być implementowana przez konkretną klasę. Ponieważ kontrolera aplikacji jest klasą abstrakcyjną, nie można wywołać nie wszystkie metody zdefiniowanej w klasie bezpośrednio. Jeśli będziesz próbować wywołać klasy aplikacji bezpośrednio następnie zostanie wyświetlony komunikat o błędzie nie można odnaleźć zasobu.

Trzecie Zwróć uwagę, że kontroler aplikacji zawiera konstruktora, który dodaje listy kategorii film, aby wyświetlić dane. Każda klasa kontrolera, który dziedziczy z kontrolera aplikacji automatycznie wywołuje konstruktor kontrolera aplikacji. Zawsze, gdy wywoływana żadnych działań na każdym kontrolerze, który dziedziczy z kontrolera aplikacji, kategorie film jest dołączony do danych widoku automatycznie.

Kontroler filmów w listę 5 dziedziczy kontrolera aplikacji.

**Wyświetlanie listy 5 —`Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

Kontroler filmów, podobnie jak kontrolera głównej opisanych w poprzedniej sekcji, udostępnia dwie metody akcji, o nazwie `Index()` i `Details()`. Należy zauważyć, że na liście kategorii filmu wyświetlanych przez strony wzorcowej widoku nie jest dodawane do wyświetlania danych w jednym `Index()` lub `Details()` metody. Ponieważ kontrolera filmy dziedziczy kontrolera aplikacji, lista kategorii film jest dodawana do wyświetlania danych automatycznie.

Zwróć uwagę, to rozwiązanie w celu dodanie danych widoku dla strony wzorcowej widoku nie narusza zasady suchej (nie powtarzaj siebie). Kod do dodawania listy kategorii film, aby wyświetlić dane znajduje się w lokalizacji tylko jeden: Konstruktor kontrolera aplikacji.

### <a name="summary"></a>Podsumowanie

W tym samouczku omówiono dwa podejścia do przekazywania danych widoku z kontrolera do widoku strony wzorcowej. Po pierwsze firma Microsoft zbadać prostą, ale trudne w utrzymaniu podejście. W pierwszej sekcji omówiono sposób dodawania danych widoku dla strony wzorcowej widoku w każdej akcji każdego kontrolera w aplikacji. Możemy stwierdzić, było nieprawidłowe podejście ponieważ narusza on zasady suchej (nie powtarzaj samodzielnie).

Następnie możemy się zbadana znacznie lepszą strategię dodawania danych do wyświetlania danych wymagane przez strony wzorcowej widoku. Zamiast opcji dodawania danych widoku w każdej akcji kontrolera, dodaliśmy danych widoku tylko raz w ramach kontrolera aplikacji. W ten sposób można uniknąć zduplikowanych kodu podczas przekazywania danych do strony wzorcowej widoku w aplikacji platformy ASP.NET MVC.

>[!div class="step-by-step"]
[Poprzednie](creating-page-layouts-with-view-master-pages-cs.md)
[dalej](asp-net-mvc-views-overview-vb.md)
