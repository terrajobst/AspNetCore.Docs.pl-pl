---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Omówienie kontrolera ASP.NET MVC (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: W tym samouczku Stephen Walther przedstawiono kontrolery ASP.NET MVC. Jak utworzyć nowe kontrolery i zwracanie różnych typów akcji res...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 95e7c555a52c8c3b765a6fffab15276491cf5714
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869091"
---
<a name="aspnet-mvc-controller-overview-c"></a>Omówienie kontrolera ASP.NET MVC (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> W tym samouczku Stephen Walther przedstawiono kontrolery ASP.NET MVC. Jak utworzyć nowe kontrolery i zwracanie różnych typów wyników akcji.


W tym samouczku Eksploruje temat kontrolery ASP.NET MVC, akcji kontrolera i wyniki akcji. Po ukończeniu tego samouczka zostanie zrozumieć, jak kontrolery są używane do kontrolować sposób którą użytkownik wchodzi w interakcję z witryny sieci Web platformy ASP.NET MVC.

## <a name="understanding-controllers"></a>Opis kontrolerów

Kontrolerów MVC jest odpowiedzialny za odpowiada na żądania wysyłane przed witryny sieci Web platformy ASP.NET MVC. Każde żądanie przeglądarki jest mapowane na określony kontroler. Załóżmy na przykład, wprowadź następujący adres URL na pasku adresu przeglądarki:

`http://localhost/Product/Index/3`

W takim przypadku kontrolerze o nazwie ProductController jest wywoływana. ProductController jest odpowiedzialny za Generowanie odpowiedzi na żądanie. Na przykład kontroler może wrócić określonego widoku do przeglądarki lub kontroler może przekierować użytkownika do innego kontrolera.

Wyświetlanie listy 1 zawiera proste kontrolerze o nazwie ProductController.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Jak widać z zakresu od 1 do wyświetlania, kontrolera jest właśnie klasą (Visual Basic .NET lub C#). Kontroler jest klasą pochodzącą od klasy podstawowej System.Web.Mvc.Controller. Ponieważ kontrolera dziedziczy po tej klasie podstawowej, kontrolera bezpłatnie dziedziczy kilka metod przydatne (omówiono te metody później).

## <a name="understanding-controller-actions"></a>Opis akcji kontrolera

Kontroler przedstawia akcji kontrolera. Akcja jest metoda na kontrolerze, która jest wywoływana po wprowadzeniu określonego adresu URL na pasku adresu przeglądarki. Załóżmy na przykład, musisz wysłać żądanie dla następującego adresu URL:

`http://localhost/Product/Index/3`

W takim przypadku klasa ProductController wywoływana jest metoda indeks(). Metoda indeks() jest przykładem akcji kontrolera.

Akcja kontrolera musi być publiczną metodę klasy kontrolera. C#, domyślnie przedstawiono prywatnej metody. Należy pamiętać, że metoda publiczna, które dodajesz do klasy kontrolera jest ujawniona jako akcji kontrolera automatycznie (należy zachować ostrożność na ten temat ponieważ akcji kontrolera może być wywoływany dla wszystkich użytkowników na całym świecie, po prostu, wpisując adres URL prawego w pasku adresu przeglądarki).

Istnieją pewne dodatkowe wymagania, które muszą być spełnione przez akcji kontrolera. Metoda używana jako akcji kontrolera nie może być przeciążony. Ponadto akcji kontrolera nie może być metodą statyczną. Inną niż można użyć dowolnego — metoda akcji kontrolera.

## <a name="understanding-action-results"></a>Opis wyników akcji

Akcja kontrolera zwraca coś o nazwie *wynik akcji*. Wynik akcji jest akcją kontrolera zwraca w odpowiedzi na żądanie przeglądarki.

Platforma ASP.NET MVC obsługuje kilka typów wyników akcji w tym:

1. ViewResult - reprezentuje HTML i znaczników.
2. EmptyResult - reprezentuje żadnego wyniku.
3. RedirectResult - reprezentuje przekierowanie do nowego adresu URL.
4. JsonResult — przedstawia wynik JavaScript Object Notation, które mogą być używane w aplikacji AJAX.
5. JavaScriptResult - reprezentuje skryptu JavaScript.
6. ContentResult - reprezentuje wynik tekstu.
7. FileContentResult - reprezentuje do pobrania pliku (z zawartość binarną).
8. FilePathResult - reprezentuje plik do pobrania (ze ścieżką).
9. FileStreamResult - reprezentuje plik do pobrania (za pomocą strumienia pliku).

Wszystkie te wyniki akcji dziedziczyć po klasie ActionResult podstawowej.

W większości przypadków akcji kontrolera zwraca ViewResult. Na przykład w akcji kontrolera indeks wyświetlania 2 zwraca ViewResult.

**Wyświetlanie listy 2 - Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Po powrocie z operacji ViewResult HTML jest zwracany do przeglądarki. Metoda indeks() wyświetlania 2 zwraca widok o nazwie indeksu w przeglądarce.

Zwróć uwagę, w akcji indeks() wyświetlania 2 nie zwraca ViewResult(). Zamiast tego jest wywoływana metoda View() klasy podstawowej kontrolera. Zwykle użytkownik nie zwrócenia wyniku akcji bezpośrednio. Zamiast tego wywołania jest jedną z następujących metod klasy podstawowej kontrolera:

1. Wyświetl — zwraca wynik akcji ViewResult.
2. Przekieruj — zwraca wynik akcji RedirectResult.
3. RedirectToAction — zwraca wynik akcji RedirectToRouteResult.
4. Metody RedirectToRoute — zwraca wynik akcji RedirectToRouteResult.
5. JSON — zwraca wynik akcji JsonResult.
6. JavaScriptResult — zwraca JavaScriptResult.
7. Zawartości — zwraca wynik akcji ContentResult.
8. Plik — zwraca FileContentResult, FilePathResult lub FileStreamResult w zależności od parametry przekazane do metody.

Tak aby zwrócić widok do przeglądarki, należy wywołać metodę View(). Jeśli chcesz przekierowywać użytkowników z jednego kontrolera akcji do innego, należy wywołać metodę RedirectToAction(). Na przykład w akcji Details() wyświetlania 3 przedstawia widok albo przekierowuje użytkownika do akcji indeks() w zależności od tego, czy parametr Id ma wartość.

**Wyświetlanie listy 3 - CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Wynik akcji ContentResult jest specjalne. Wynik akcji ContentResult służy do zwrócenia wyniku akcji jako zwykły tekst. Na przykład metoda indeks() listę 4 zwraca komunikat jako zwykły tekst, a nie w formacie HTML.

**Wyświetlanie listy 4 - Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Po wywołaniu akcji StatusController.Index() widoku nie są zwracane. Zamiast tego nieprzetworzony tekst "Hello World!" jest zwracany do przeglądarki.

Jeśli akcja kontrolera zwraca wynik nie wynik akcji — na przykład datę lub liczbę całkowitą — następnie wynik jest ujęte w ContentResult automatycznie. Na przykład po wywołaniu akcji indeks() WorkController listę 5 daty jest zwracana jako ContentResult automatycznie.

**Wyświetlanie listy 5 - WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

W akcji indeks() listę 5 zwraca obiekt DateTime. Platforma ASP.NET MVC Konwertuje obiekt daty i godziny do ciągu i opakowuje wartość daty i godziny w ContentResult automatycznie. Przeglądarka odbiera datę i godzinę jako zwykły tekst.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było przedstawiono podstawowe pojęcia kontrolery ASP.NET MVC, akcji kontrolera i wyniki akcji kontrolera. W pierwszej sekcji przedstawiono sposób dodawania nowych kontrolerów do projektu programu ASP.NET MVC. Następnie przedstawiono sposób publicznej metody kontrolera są widoczne dla całość jako akcji kontrolera. Ponadto omówiono różne typy wyników akcji, które mogą zostać zwrócone z akcji kontrolera. W szczególności omówiono sposób zwracania ViewResult, RedirectToActionResult i ContentResult z akcji kontrolera.

> [!div class="step-by-step"]
> [Poprzednie](creating-an-action-vb.md)
> [dalej](creating-custom-routes-cs.md)
