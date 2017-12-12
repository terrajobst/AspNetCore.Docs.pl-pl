---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: "Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (VB) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Informacje o sposobie tworzenia testów jednostkowych dla akcji kontrolera. W tym samouczku Stephen Walther pokazano, jak sprawdzić, czy akcji kontrolera zwraca częśći..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: d92ee0c26787e5c482e8695001d8809d3ee9ee30
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (VB)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

[Pobierz plik PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Informacje o sposobie tworzenia testów jednostkowych dla akcji kontrolera. W tym samouczku Stephen Walther pokazano, jak sprawdzić, czy zwraca określonego widoku akcji kontrolera, zwraca określonego zestawu danych lub zwraca innego typu wyniku akcji.


Celem tego samouczka jest aby zademonstrować, jak możesz pisać testy jednostkowe dla kontrolerów programu ASP.NET MVC aplikacji. Omówiono sposób tworzenia trzy różne typy testów jednostkowych. Dowiesz się, jak przetestować widoku zwrócony przez akcji kontrolera, jak przetestować widoku danych zwróconych przez akcji kontrolera i testowanie, czy jednej akcji kontrolera przekieruje Cię do drugiej akcji kontrolera.

## <a name="creating-the-controller-under-test"></a>Tworzenie kontrolera w ramach testu

Zacznijmy od utworzenia kontrolera, który mamy zamierzają testu. Kontroler, o nazwie `ProductController`, znajduje się lista 1.

**1 — Lista`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

`ProductController` Zawiera dwie metody akcji, o nazwie `Index()` i `Details()`. Obie metody akcji zwracają widoku. Zwróć uwagę, że `Details()` akcji akceptuje parametr o nazwie identyfikatora.

## <a name="testing-the-view-returned-by-a-controller"></a>Testowanie widoku zwrócony przez kontroler

Załóżmy, że chcemy przetestować czy `ProductController` zwraca prawego widoku. Chcemy upewnić się, że w przypadku `ProductController.Details()` akcji jest wywoływana, jest zwracany w widoku szczegółów. Klasy testowej wyświetlania 2 zawiera testu jednostkowego do testowania zwrócone przez widok `ProductController.Details()` akcji.

**2 — Lista`ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

Klasa w wyświetlania 2 zawiera metodę testu o nazwie `TestDetailsView()`. Ta metoda zawiera trzy wiersze kodu. Pierwszy wiersz kodu tworzy nowe wystąpienie klasy `ProductController` klasy. Drugi wiersz kodu wywołuje kontrolera `Details()` metody akcji. Na koniec ostatniego wiersza kodu sprawdza, czy widok zwrócony przez `Details()` akcji jest widok szczegółów.

`ViewResult.ViewName` Właściwość reprezentuje nazwę widoku zwrócony przez kontroler. Jeden duży ostrzeżenie o testowaniu tej właściwości. Istnieją dwa sposoby, że kontroler może zwrócić widok. Kontroler jawnie może zwrócić widok następująco:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Alternatywnie nazwę widoku można wywnioskować na podstawie nazwę akcji kontrolera następująco:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Ta akcja kontrolera również zwraca widok o nazwie `Details`. Nazwa widoku jest jednak wywnioskować na podstawie nazwy akcji. Jeśli chcesz przetestować nazwy widoku, następnie jawnie wróć Nazwa widoku z akcji kontrolera.

Można uruchomić testu jednostkowego wyświetlania 2, wprowadzając kombinację klawiszy **Ctrl-R, A** lub przez kliknięcie przycisku **Uruchom wszystkie testy z rozwiązania** przycisk (zobacz rysunek 1). Jeśli test zakończył się pomyślnie, zostanie wyświetlone okno wyników testu na rysunku 2.


[![Uruchom wszystkie testy z rozwiązania](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Rysunek 01**: Uruchom wszystkie testy z rozwiązania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![Powodzenie!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Rysunek 02**: sukcesu! ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Testowanie widoku danych zwróconych przez kontrolera

Kontroler MVC przekazuje dane do widoku przy użyciu elementu o nazwie  *`View Data`* . Załóżmy na przykład chcesz wyświetlić szczegóły dotyczące danego produktu po wywołaniu `ProductController Details()` akcji. W takim przypadku można utworzyć wystąpienia `Product` klasy (zdefiniowany w modelu) i przekaż wystąpienie `Details` widoku dzięki wykorzystaniu `View Data`.

Zmodyfikowane `ProductController` w wyświetlania 3 zawiera zaktualizowanego `Details()` akcję, która zwraca produktu.

**3 — lista`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Najpierw `Details()` akcja tworzy nowe wystąpienie klasy `Product` klasa, która reprezentuje komputer przenośny. Następnie, wystąpienie `Product` klasy jest przekazywany jako drugi parametr `View()` metody.

Można pisać testy jednostkowe można sprawdzić, czy jest oczekiwane dane zawarte w widoku danych. Czy produkt reprezentujący komputer przenośny jest zwracany, gdy jest wywoływana w testach listę 4 testu jednostkowego `ProductController Details()` metody akcji.

**Wyświetlanie listy 4.`ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

W przypadku wyświetlania 4 `TestDetailsView()` metoda sprawdza dane widoku zwrócony przez wywołanie `Details()` metody. `ViewData` Jest udostępniany jako właściwość na `ViewResult` zwrócony przez wywołanie `Details()` metody. `ViewData.Model` Właściwość zawiera produktu przekazywane do widoku. Po prostu test sprawdza, czy produktu zawartego w widoku danych ma nazwę komputera przenośnego.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testowanie wynik akcji zwrócony przez kontroler

Bardziej złożone akcji kontrolera może być zwracanie różnych typów wyników akcji w zależności od wartości parametrów przekazanych do akcji kontrolera. Akcja kontrolera może zwracać różne typy wyników akcji w tym `ViewResult`, `RedirectToRouteResult`, lub `JsonResult`.

Na przykład zmodyfikowanych `Details()` zwraca akcji w listę 5 `Details` widoku, gdy przekazujesz prawidłowego identyfikatora produktu do akcji. W przypadku przekazania nieprawidłowy produktu identyfikator - Id o wartości mniej niż 1 — a następnie użytkownik zostanie przekierowany do `Index()` akcji.

**Wyświetlanie listy 5 —`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Możesz przetestować działanie `Details()` akcji z testu jednostkowego wyświetlania 6. Weryfikuje testu jednostkowego w 6 wyświetlania nastąpi przekierowanie do `Index` widoku, gdy Id o wartości -1 są przekazywane do `Details()` metody.

**Wyświetlanie listy 6 —`ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Podczas wywoływania `RedirectToAction()` metoda akcji kontrolera, zwraca akcji kontrolera `RedirectToRouteResult`. Test sprawdza czy `RedirectToRouteResult` przekierowuje użytkownika do akcji kontrolera, o nazwie `Index`.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia testów jednostkowych dla akcji kontrolera MVC. Po pierwsze przedstawiono sposób sprawdzania, czy widok z prawej strony jest zwracana w wyniku akcji kontrolera. Przedstawiono sposób używania `ViewResult.ViewName` właściwości można zweryfikować nazwy widoku.

Następnie możemy zbadać jak można sprawdzić zawartość `View Data`. Przedstawiono sposób sprawdzania, czy odpowiedniego produktu został zwrócony w `View Data` po wywołaniu metody akcji kontrolera.

Ponadto omówiono, jak można sprawdzić, czy różnych typów wyników akcji są zwracane z akcji kontrolera. Przedstawiono sposób sprawdzić, czy kontroler zwraca `ViewResult` lub `RedirectToRouteResult`.

>[!div class="step-by-step"]
[Poprzednie](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
