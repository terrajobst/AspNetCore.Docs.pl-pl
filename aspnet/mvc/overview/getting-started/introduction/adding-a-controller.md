---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d5cc9db7b1eec139a37afb6427fd761342fcc1f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751956"
---
<a name="adding-a-controller"></a>Dodawanie kontrolera
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC to *model-view-controller*. MVC to wzorzec do tworzenia aplikacji, które są również wysokiej, zakresie testować i łatwa w obsłudze. Aplikacje oparte na MVC zawiera:

- **M** odels: klasy reprezentujące dane aplikacji, które korzystają z logikę walidacji do wymuszania reguł biznesowych dla tych danych.
- **V** iews: pliki szablonów, których Twoja aplikacja używa do dynamicznego generowania odpowiedzi HTML.
- **C** ontrollers: klas, które obsługują żądania przychodzące przeglądarki, pobrać modelu danych, a następnie określ Przeglądanie szablonów, które zwracają odpowiedzi do przeglądarki.

Firma Microsoft będzie obejmujące wszystkie te pojęcia w tej serii samouczków a pokazują, jak ich używać do tworzenia aplikacji.

> [!NOTE]
> W poprzednim kroku MVC domyślny szablon został wybrany. Spowoduje to utworzenie domu, konta i Zarządzanie kontrolerami domyślnie.

Zacznijmy od utworzenia klasy kontrolera. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie kliknij przycisk **Dodaj**, następnie **kontrolera**.


![](adding-a-controller/_static/image1.png)

W **Dodawanie szkieletu** okno dialogowe, kliknij przycisk **kontroler MVC 5 — pusty**, a następnie kliknij przycisk **Dodaj**.

![](adding-a-controller/_static/image2.png)  
 

Nadaj nowemu kontrolerowi nazwę "HelloWorldController", a następnie kliknij przycisk **Dodaj**.

![Dodawanie kontrolera](adding-a-controller/_static/image3.png)

Należy zauważyć w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs* i nowy folder *Views\HelloWorld*. Kontroler jest otwarty w środowisku IDE.

![](adding-a-controller/_static/image4.png)

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontrolera zwróci ciąg HTML jako przykład. Nosi nazwę kontrolera `HelloWorldController` i nosi nazwę pierwszej metody `Index`. Możemy wywołać go z poziomu przeglądarki. Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5). W przeglądarce, dołącz &quot;HelloWorld&quot; do ścieżki, w pasku adresu. (Na przykład na ilustracji poniżej jego `http://localhost:1234/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu. W powyższej metody kod zwrócony ciąg bezpośrednio. Otrzymaliśmy systemu, aby po prostu zwraca kod HTML i tak!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC wywołuje innego kontrolera klasy (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Logiki routingu domyślnego adresu URL używany przez program ASP.NET MVC używa formatu następująco, aby określić, jaki kod do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Ustaw format routingu w *aplikacji\_Start/RouteConfig.cs* pliku.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Podczas uruchamiania aplikacji, a nie podasz żadnych segmentów adresu URL, jego wartość domyślna to "Domowy" kontrolera i metody akcji "Index" określone w sekcji Ustawienia domyślne powyżej kodu.

Pierwszą część adresu URL określa klasę kontrolera do wykonania. Dlatego */HelloWorld* mapuje `HelloWorldController` klasy. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego */HelloWorld/indeks* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania. Należy zauważyć, że musimy wskaż */HelloWorld* i `Index` metoda została użyta domyślna. Jest to spowodowane metodę o nazwie `Index` jest domyślną metodą, która zostanie wywołana na kontrolerze, jeśli nie jest jawnie określona. Trzecia część segment adresu URL ( `Parameters`) dla danych trasy. Zobaczymy danych trasy w późniejszym czasie na w tym samouczku.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda powitalnej akcji... &quot;. Domyślne mapowanie MVC to `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL jest kontroler `HelloWorld` i `Welcome` jest metodą akcji. Pierwszy raz używasz `[Parameters]` wchodzi w skład jeszcze adresu URL.

![](adding-a-controller/_static/image6.png)

Zmodyfikujmy przykład nieco tak, aby przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Witaj? nazwa = Scott&amp;numtimes = 4*). Zmień swoje `Welcome` metodę, aby uwzględnić dwa parametry, jak pokazano poniżej. Należy pamiętać, że kod używa funkcji opcjonalny parametr języka C# z informacją, że `numTimes` parametr powinien domyślnie na 1, jeśli nie przekazano żadnej wartości tego parametru.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Uwaga dotycząca zabezpieczeń: Powyższy kod używa [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) chronić aplikację przed złośliwe dane wejściowe (to znaczy JavaScript). Aby uzyskać więcej informacji, zobacz [porady: ochrona względem skryptu luki w aplikacji sieci Web, stosując kodowanie HTML na ciągi](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Uruchom aplikację, a następnie przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Możesz spróbować różne wartości `name` i `numtimes` w adresie URL. [System powiązań modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu do parametrów w metodzie.

![](adding-a-controller/_static/image7.png)

W przykładzie powyżej segment adresu URL ( `Parameters`) nie jest używany, `name` i `numTimes` parametry są przekazywane jako [ciągów zapytania](http://en.wikipedia.org/wiki/Query_string). ? (znak zapytania) w powyższym adresie URL jest separatorem, a następnie wykonaj ciągi zapytań. &amp; Rozdziela ciągi zapytań.

Zastąp metodę powitalnej następującym kodem:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Uruchom aplikację, a następnie wprowadź następujący adres URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Tym razem trzeci segment adresu URL dopasowane parametru trasy `ID.` `Welcome` metody akcji zawiera parametr (`ID`) pasujących specyfikacji adresu URL w `RegisterRoutes` metody.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

W aplikacjach ASP.NET MVC jest bardziej typowego, aby przekazać parametry trasy danych (takich jak zrobiliśmy o identyfikatorze powyżej) od przekazywania ich jako ciągi zapytań. Można również dodać trasę do przekazywania zarówno `name` i `numtimes` w parametrach jako dane trasy w adresie URL. W *aplikacji\_Start\RouteConfig.cs* plików, Dodawanie trasy "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Uruchom aplikację, a następnie przejdź do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

W przypadku wielu aplikacji MVC trasy domyślnej działa prawidłowo. Dowiesz się w dalszej części tego samouczka, aby przekazać dane za pomocą integratora modelu, a nie będziesz mieć zmodyfikować domyślną trasę dla tego.

W tych przykładach kontrolera została wykonując &quot;VC&quot; część MVC — oznacza to, widok i kontroler pracy. Kontroler zwraca HTML bezpośrednio. Zazwyczaj nie ma kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo trudne kodu. Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwiający Generowanie odpowiedzi HTML. Przyjrzyjmy się dalej, w jaki sposób możemy to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](getting-started.md)
> [dalej](adding-a-view.md)
