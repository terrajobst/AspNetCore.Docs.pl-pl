---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Omówienie kontrolera ASP.NET MVC (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen przedstawiono kontrolery ASP.NET MVC. Dowiesz się, jak utworzyć nowe kontrolery i zwracać różne typy akcji res...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: e1834498d5a0f5d84f650faf32bb6352f2ad3e70
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370090"
---
<a name="aspnet-mvc-controller-overview-c"></a>Omówienie kontrolera ASP.NET MVC (C#)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> W tym samouczku Walther Autor: Stephen przedstawiono kontrolery ASP.NET MVC. Dowiesz się, jak utworzyć nowe kontrolery i zwracania różnych typów wyników akcji.


W tym samouczku przedstawiono temat kontrolerów, akcji kontrolera ASP.NET MVC i wyników akcji. Po ukończeniu tego samouczka należy zrozumieć, jak kontrolery są używane do kontrolowania sposobu, w których użytkownik wchodzi w interakcję z witryny sieci Web platformy ASP.NET MVC.

## <a name="understanding-controllers"></a>Opis kontrolerów

Kontrolerów MVC są odpowiedzialne za odpowiada na żądania skierowanego do witryny sieci Web platformy ASP.NET MVC. Każde żądanie przeglądarki jest mapowane na określony kontroler. Załóżmy na przykład, wprowadź następujący adres URL w pasku adresu przeglądarki:

`http://localhost/Product/Index/3`

W tym przypadku kontroler o nazwie ProductController jest wywoływana. ProductController jest odpowiedzialny za Generowanie odpowiedzi na żądanie przeglądarki. Na przykład kontroler może zwrócić określonego widoku, wróć do przeglądarki lub kontroler może być przekierowanie użytkownika do innego kontrolera.

Wyświetlanie listy 1 zawiera proste o nazwie ProductController kontrolera.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Jak widać z zakresu od 1 do wyświetlania listy kontrolera jest po prostu klasą (Visual Basic .NET lub C#). Kontroler jest klasą pochodzącą z klasy bazowej System.Web.Mvc.Controller. Ponieważ kontroler dziedziczy z tej klasy bazowej, kontrolerem dziedziczy kilka użytecznych metod bezpłatnie (omawiane metody te za chwilę).

## <a name="understanding-controller-actions"></a>Opis akcji kontrolera

Kontroler udostępnia akcji kontrolera. Akcja jest metoda na kontrolerze, która jest wywoływana po wprowadzeniu określonego adresu URL w pasku adresu przeglądarki. Załóżmy na przykład, przesyłania żądania do następującego adresu URL:

`http://localhost/Product/Index/3`

W takim przypadku klasa ProductController wywoływana jest metoda indeks(). Metoda indeks() jest przykładem akcji kontrolera.

Akcja kontrolera musi być publiczna metoda klasy kontrolera. C#, domyślnie przedstawiono prywatnej metody. Należy pamiętać, że metoda publiczna, dodawana do klasy kontrolera jest automatycznie widoczne jako akcji kontrolera (należy zachować ostrożność na ten temat ponieważ akcji kontrolera może być wywoływany przez wszystkich użytkowników w środowisku, po prostu wpisując polecenie właściwego adresu URL w pasku adresu przeglądarki).

Istnieją pewne wymagania dodatkowe, które muszą zostać spełnione przez akcji kontrolera. Nie można przeciążyć metodę akcji kontrolera. Ponadto akcji kontrolera nie może być metodą statyczną. Innych niż można użyć niemal dowolnej metody akcji kontrolera.

## <a name="understanding-action-results"></a>Opis wyników akcji

Akcja kontrolera zwraca coś, co jest nazywane *wynik akcji*. Wynik akcji jest akcją kontroler zwraca w odpowiedzi na żądanie przeglądarki.

Platforma ASP.NET MVC obsługuje kilka typów wyników akcji, w tym:

1. ViewResult — HTML reprezentuje i znaczników.
2. EmptyResult — reprezentuje żadnego wyniku.
3. RedirectResult — reprezentuje przekierowanie do nowego adresu URL.
4. JsonResult — reprezentuje wynik JavaScript Object Notation używanym w aplikacji interfejsu AJAX.
5. JavaScriptResult — reprezentuje skryptu JavaScript.
6. ContentResult — reprezentuje rezultat tekstu.
7. FileContentResult — reprezentuje plik do pobrania (z zawartości binarnej).
8. FilePathResult — reprezentuje plik do pobrania (przy użyciu ścieżki).
9. FileStreamResult — reprezentuje plik do pobrania (z strumienia pliku).

Wszystkie te wyniki akcji dziedziczą z klasy bazowej ActionResult.

W większości przypadków akcji kontrolera zwraca ViewResult. Na przykład akcji kontrolera indeksu w ofercie 2 zwraca ViewResult.

**Wyświetlanie listy 2 - Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Po powrocie z akcji ViewResult HTML jest zwracany do przeglądarki. Metoda indeks() w ofercie 2 zwraca widok o nazwie indeksu w przeglądarce.

Zwróć uwagę, czy akcja indeks() w ofercie 2 nie może zwracać ViewResult(). Zamiast tego zostanie wywołana metoda View() klasy bazowej kontrolera. Zazwyczaj użytkownik nie zwracają wynik akcji bezpośrednio. Zamiast tego można wywołać jedną z następujących metod klasy bazowej kontrolera:

1. Wyświetl — zwraca wynik akcji ViewResult.
2. Przekieruj — zwraca wynik akcji RedirectResult.
3. RedirectToAction — zwraca wynik akcji RedirectToRouteResult.
4. RedirectToRoute — zwraca wynik akcji RedirectToRouteResult.
5. JSON — zwraca wynik akcji JsonResult.
6. JavaScriptResult — zwraca JavaScriptResult.
7. Zawartości — zwraca wynik akcji ContentResult.
8. Plik — zwraca FileContentResult, FilePathResult lub FileStreamResult w zależności od parametrów przekazywany do metody.

Tak Jeśli chcesz przywrócić widok w przeglądarce, należy wywołać metodę View(). Jeśli chcesz przekierować użytkownika z jednego kontrolera akcji do innego, należy wywołać metodę RedirectToAction(). Na przykład akcja Details() w ofercie 3 przedstawia widok lub przekierowuje użytkownika do akcji indeks(), w zależności od tego, czy parametr Id ma wartość.

**Wyświetlanie listy 3 - CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Wynik akcji ContentResult to specjalne. Można użyć ContentResult wyniku akcji do zwrócenia wyniku akcji jako zwykły tekst. Na przykład metoda indeks() w ofercie 4 zwraca komunikat jako zwykły tekst, a nie kodu HTML.

**Wyświetlanie listy 4 - Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Wywołana Akcja StatusController.Index() widoku nie są zwracane. Zamiast tego nieprzetworzony tekst "Hello World!" jest zwracany do przeglądarki.

Akcja kontrolera zwraca wynik nie wynik akcji — na przykład datę lub liczbę całkowitą — następnie wynik jest otoczona ContentResult automatycznie. Na przykład podczas wywoływania akcji indeks() WorkController w ofercie 5 Data jest zwracana jako ContentResult automatycznie.

**Wyświetlanie listy 5 - WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

Akcja indeks() w ofercie 5 zwraca obiekt daty/godziny. Platforma ASP.NET MVC Konwertuje obiekt DateTime na ciąg i otacza wartość daty/godziny w ContentResult automatycznie. Przeglądarka odbiera datę i godzinę w postaci zwykłego tekstu.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było wprowadzenie do koncepcji kontrolerów, akcji kontrolera i wyników akcji kontrolera ASP.NET MVC. W pierwszej sekcji przedstawiono sposób dodawania nowych kontrolerów do projektu programu ASP.NET MVC. Następnie przedstawiono metod jak publiczny kontrolera są narażone na wszechświat jako akcji kontrolera. Na koniec Omówiliśmy różne typy wyników akcji, które mogą zostać zwrócone z akcji kontrolera. W szczególności omówiono sposób zwracania ViewResult, RedirectToActionResult i ContentResult z akcji kontrolera.

> [!div class="step-by-step"]
> [Poprzednie](creating-an-action-vb.md)
> [dalej](creating-custom-routes-cs.md)
