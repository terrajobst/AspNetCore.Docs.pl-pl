---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 772f3c87d6b73b324164bf9619d332a9c01d7476
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756136"
---
<a name="adding-a-controller"></a>Dodawanie kontrolera
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


MVC to *model-view-controller*. MVC to wzorzec do tworzenia aplikacji, które są również wysokiej, zakresie testować i łatwa w obsłudze. Aplikacje oparte na MVC zawiera:

- **M** odels: klasy reprezentujące dane aplikacji, które korzystają z logikę walidacji do wymuszania reguł biznesowych dla tych danych.
- **V** iews: pliki szablonów, których Twoja aplikacja używa do dynamicznego generowania odpowiedzi HTML.
- **C** ontrollers: klas, które obsługują żądania przychodzące przeglądarki, pobrać modelu danych, a następnie określ Przeglądanie szablonów, które zwracają odpowiedzi do przeglądarki.

Firma Microsoft będzie obejmujące wszystkie te pojęcia w tej serii samouczków a pokazują, jak ich używać do tworzenia aplikacji.

Zacznijmy od utworzenia klasy kontrolera. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**.

![](adding-a-controller/_static/image1.png)

Nadaj nazwę nowemu kontrolerowi &quot;HelloWorldController&quot;. Pozostaw domyślny szablon jako **pusty kontroler MVC** i kliknij przycisk **Dodaj**.

![Dodawanie kontrolera](adding-a-controller/_static/image2.png)

Należy zauważyć w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs*. Plik jest otwarty w środowisku IDE.

![](adding-a-controller/_static/image3.png)

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontrolera zwróci ciąg HTML jako przykład. Nosi nazwę kontrolera `HelloWorldController` i nosi nazwę pierwszej metody powyżej `Index`. Możemy wywołać go z poziomu przeglądarki. Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5). W przeglądarce, dołącz &quot;HelloWorld&quot; do ścieżki, w pasku adresu. (Na przykład na ilustracji poniżej jego `http://localhost:1234/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu. W powyższej metody kod zwrócony ciąg bezpośrednio. Otrzymaliśmy systemu, aby po prostu zwraca kod HTML i tak!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC wywołuje innego kontrolera klasy (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Logiki routingu domyślnego adresu URL używany przez program ASP.NET MVC używa formatu następująco, aby określić, jaki kod do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Pierwszą część adresu URL określa klasę kontrolera do wykonania. Dlatego */HelloWorld* mapuje `HelloWorldController` klasy. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego */HelloWorld/indeks* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania. Należy zauważyć, że musimy wskaż */HelloWorld* i `Index` metoda została użyta domyślna. Jest to spowodowane metodę o nazwie `Index` jest domyślną metodą, która zostanie wywołana na kontrolerze, jeśli nie jest jawnie określona.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda powitalnej akcji... &quot;. Domyślne mapowanie MVC to `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL jest kontroler `HelloWorld` i `Welcome` jest metodą akcji. Pierwszy raz używasz `[Parameters]` wchodzi w skład jeszcze adresu URL.

![](adding-a-controller/_static/image5.png)

Zmodyfikujmy przykład nieco tak, aby przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Witaj? nazwa = Scott&amp;numtimes = 4*). Zmień swoje `Welcome` metodę, aby uwzględnić dwa parametry, jak pokazano poniżej. Należy pamiętać, że kod używa funkcji opcjonalny parametr języka C# z informacją, że `numTimes` parametr powinien domyślnie na 1, jeśli nie przekazano żadnej wartości tego parametru.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uruchom aplikację, a następnie przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Możesz spróbować różne wartości `name` i `numtimes` w adresie URL. [System powiązań modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu do parametrów w metodzie.

![](adding-a-controller/_static/image6.png)

W obu tych przykładach kontrolera została wykonując &quot;VC&quot; część MVC — oznacza to, widok i kontroler pracy. Kontroler zwraca HTML bezpośrednio. Zazwyczaj nie ma kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo trudne kodu. Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwiający Generowanie odpowiedzi HTML. Przyjrzyjmy się dalej, w jaki sposób możemy to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](intro-to-aspnet-mvc-4.md)
> [dalej](adding-a-view.md)
