---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 3864bab284661b0c44f9e4cb363c2d60eccc7c66
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a>Dodawanie kontrolera
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Oznacza MVC *model-view-controller*. MVC to wzorzec do tworzenia aplikacji, które są dobrze zaprojektowanej architektury, testować i łatwe w konserwacji. Aplikacje oparte na MVC zawiera:

- **M** odels: klasy reprezentujące dane aplikacji, które korzystają z logikę weryfikacji do wymuszania reguł biznesowych dla tych danych.
- **V** iews: plików szablonów, które Twoja aplikacja używa do dynamicznego generowania odpowiedzi HTML.
- **C** ontrollers: klas, które obsługują przychodzące żądania przeglądarki, pobrać modelu danych, a następnie określ szablonów widoków, które zwracają odpowiedzi do przeglądarki.

Firma Microsoft będzie można obejmujące wszystkie te pojęcia w tym samouczku i opisano, jak ich używać do tworzenia aplikacji.

> [!NOTE]
> W poprzednim kroku MVC domyślny szablon został wybrany. Spowoduje to utworzenie Narzędzia główne, konta i Zarządzaj kontrolerami domyślnie.

Zacznijmy od utworzenia klasy kontrolera. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie kliknij przycisk **Dodaj**, następnie **kontrolera**.


![](adding-a-controller/_static/image1.png)

W **Dodawanie szkieletu** okno dialogowe, kliknij przycisk **kontroler MVC 5 — pusty**, a następnie kliknij przycisk **Dodaj**.

![](adding-a-controller/_static/image2.png)  
 

Nowemu kontrolerowi nazwę "HelloWorldController", a następnie kliknij przycisk **Dodaj**.

![Dodawanie kontrolera](adding-a-controller/_static/image3.png)

Zwróć uwagę, w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs* i nowy folder *Views\HelloWorld*. Kontroler jest otwarty w IDE.

![](adding-a-controller/_static/image4.png)

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Jako przykład metod kontrolera zwraca ciąg HTML. Nosi nazwę kontrolera `HelloWorldController` i nosi nazwę pierwsza metoda `Index`. Teraz wywołać go w przeglądarce. Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5). W przeglądarce, dołącz &quot;HelloWorld&quot; do ścieżki w pasku adresu. (Na przykład na ilustracji poniżej jego `http://localhost:1234/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu. W powyższej metodzie kod zwrócony ciąg bezpośrednio. Informację systemu tylko zwrócić kod HTML i jak!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC wywołuje klasy innego kontrolera (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Logiki routingu domyślny adres URL używany przez program ASP.NET MVC używa formatu podobny do tego, aby określić, jaki kod do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Określanie formatu routingu w *aplikacji\_Start/RouteConfig.cs* pliku.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Po uruchomieniu aplikacji i nie podawaj wszystkie segmenty adresu URL, domyślne "Home" kontrolera i metody akcji "Index" określony w sekcji Ustawienia domyślne kodu powyżej.

Pierwsza część adresu URL określa klasy kontrolera do wykonania. Dlatego */HelloWorld* mapuje `HelloWorldController` klasy. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego */HelloWorld/indeksu* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania. Należy zauważyć, że Musieliśmy aby przejść do */HelloWorld* i `Index` użyto metody domyślnie. Jest to spowodowane metodę o nazwie `Index` jest domyślna metoda, która zostanie wywołana na kontrolerze, jeśli nie został jawnie określony. Trzeci część segment adresu URL ( `Parameters`) dla danych trasy. Firma Microsoft będzie później Zobacz dane trasy w tym samouczku.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda akcji Zapraszamy... &quot;. Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontrolera jest `HelloWorld` i `Welcome` jest metodą akcji. Nie były używane `[Parameters]` część jeszcze adresu URL.

![](adding-a-controller/_static/image6.png)

Załóżmy przykładzie nieco zmodyfikować tak, aby niektóre informacje o parametrach z adresu URL można przekazać do kontrolera (na przykład */HelloWorld/Zapraszamy? nazwa = Scott&amp;numtimes = 4*). Zmień Twoje `Welcome` metodę w celu uwzględnienia dwóch parametrów, jak pokazano poniżej. Należy pamiętać, że kod jest używana funkcja opcjonalny parametr C# z informacją, że `numTimes` parametr powinien domyślnie 1, jeśli nie przekazano żadnej wartości tego parametru.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Uwaga dotycząca zabezpieczeń: Powyższy kod używa [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) do ochrony aplikacji przed złośliwymi danych wejściowych (to znaczy JavaScript). Aby uzyskać więcej informacji, zobacz [porady: ochrona przed skryptu wykorzystuje luki w aplikacji sieci Web przez stosowanie kodowanie HTML do ciągów](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Uruchom aplikację i przejdź do adresu URL przykład (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Możesz spróbować różnych wartości `name` i `numtimes` w adresie URL. [System powiązanie modelu platformy ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu w parametrach w metodę.

![](adding-a-controller/_static/image7.png)

W przykładzie powyżej segment adresu URL ( `Parameters`) nie jest używany, `name` i `numTimes` parametry są przekazywane jako [ciągów zapytania](http://en.wikipedia.org/wiki/Query_string). ? (znak zapytania) w powyższy adres URL jest separatorem, i wykonaj ciągi zapytań. &amp; Rozdziela ciągi zapytań.

Zastąp metodę powitalnej następujący kod:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Uruchom aplikację i wprowadź następujący adres URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Teraz trzeci segment adresu URL dopasowane parametru trasy `ID.` `Welcome` metody akcji zawiera parametr (`ID`) zgodnych ze specyfikacją adres URL w `RegisterRoutes` metody.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

W aplikacjach ASP.NET MVC jest bardziej typowego do przekazania parametrów jako dane trasy (takich jak robiliśmy o identyfikatorze powyżej) niż przekazywanie ich jako ciągi zapytań. Można również dodać trasy do przekazywania zarówno `name` i `numtimes` w parametrach jako dane trasy w adresie URL. W *aplikacji\_Start\RouteConfig.cs* plików, Dodawanie trasy "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Uruchom aplikację i przejdź do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Wiele aplikacji MVC trasa domyślna działa prawidłowo. Dowiesz się w dalszej części tego samouczka do przekazywania danych za pomocą integratora modelu, a nie będziesz mieć do modyfikowania dla tej trasy domyślnej.

W tym przykładzie został ten kontroler &quot;VC&quot; część MVC — to znaczy pracy widok i kontroler. Kontroler bezpośrednio zwraca HTML. Zwykle nie mają kontrolerów bezpośrednio, zwracanie HTML, ponieważ staje się bardzo trudne kodu. Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwia generowanie odpowiedzi HTML. Oto dalej w sposób firma Microsoft można to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](getting-started.md)
> [dalej](adding-a-view.md)
