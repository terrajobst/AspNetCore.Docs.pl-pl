---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>Dodawanie kontrolera
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


Oznacza MVC *model-view-controller*. MVC to wzorzec do tworzenia aplikacji, które są dobrze zaprojektowanej architektury, testować i łatwe w konserwacji. Aplikacje oparte na MVC zawiera:

- **M** odels: klasy reprezentujące dane aplikacji, które korzystają z logikę weryfikacji do wymuszania reguł biznesowych dla tych danych.
- **V** iews: plików szablonów, które Twoja aplikacja używa do dynamicznego generowania odpowiedzi HTML.
- **C** ontrollers: klas, które obsługują przychodzące żądania przeglądarki, pobrać modelu danych, a następnie określ szablonów widoków, które zwracają odpowiedzi do przeglądarki.

Firma Microsoft będzie można obejmujące wszystkie te pojęcia w tym samouczku i opisano, jak ich używać do tworzenia aplikacji.

Zacznijmy od utworzenia klasy kontrolera. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**.

![](adding-a-controller/_static/image1.png)

Nowemu kontrolerowi nazwę &quot;HelloWorldController&quot;. Pozostaw domyślny szablon jako **kontroler MVC pusty** i kliknij przycisk **Dodaj**.

![Dodawanie kontrolera](adding-a-controller/_static/image2.png)

Zwróć uwagę, w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs*. Plik jest otwarty w IDE.

![](adding-a-controller/_static/image3.png)

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Jako przykład metod kontrolera zwraca ciąg HTML. Nosi nazwę kontrolera `HelloWorldController` i nosi nazwę pierwsza metoda powyżej `Index`. Teraz wywołać go w przeglądarce. Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5). W przeglądarce, dołącz &quot;HelloWorld&quot; do ścieżki w pasku adresu. (Na przykład na ilustracji poniżej jego `http://localhost:1234/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu. W powyższej metodzie kod zwrócony ciąg bezpośrednio. Informację systemu tylko zwrócić kod HTML i jak!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC wywołuje klasy innego kontrolera (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Logiki routingu domyślny adres URL używany przez program ASP.NET MVC używa formatu podobny do tego, aby określić, jaki kod do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Pierwsza część adresu URL określa klasy kontrolera do wykonania. Dlatego */HelloWorld* mapuje `HelloWorldController` klasy. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego */HelloWorld/indeksu* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania. Należy zauważyć, że Musieliśmy aby przejść do */HelloWorld* i `Index` użyto metody domyślnie. Jest to spowodowane metodę o nazwie `Index` jest domyślna metoda, która zostanie wywołana na kontrolerze, jeśli nie został jawnie określony.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda akcji Zapraszamy... &quot;. Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontrolera jest `HelloWorld` i `Welcome` jest metodą akcji. Nie były używane `[Parameters]` część jeszcze adresu URL.

![](adding-a-controller/_static/image5.png)

Załóżmy przykładzie nieco zmodyfikować tak, aby niektóre informacje o parametrach z adresu URL można przekazać do kontrolera (na przykład */HelloWorld/Zapraszamy? nazwa = Scott&amp;numtimes = 4*). Zmień Twoje `Welcome` metodę w celu uwzględnienia dwóch parametrów, jak pokazano poniżej. Należy pamiętać, że kod jest używana funkcja opcjonalny parametr C# z informacją, że `numTimes` parametr powinien domyślnie 1, jeśli nie przekazano żadnej wartości tego parametru.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uruchom aplikację i przejdź do adresu URL przykład (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Możesz spróbować różnych wartości `name` i `numtimes` w adresie URL. [System powiązanie modelu platformy ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu w parametrach w metodę.

![](adding-a-controller/_static/image6.png)

W obu tych przykładach został ten kontroler &quot;VC&quot; część MVC — to znaczy pracy widok i kontroler. Kontroler bezpośrednio zwraca HTML. Zwykle nie mają kontrolerów bezpośrednio, zwracanie HTML, ponieważ staje się bardzo trudne kodu. Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwia generowanie odpowiedzi HTML. Oto dalej w sposób firma Microsoft można to zrobić.

>[!div class="step-by-step"]
[Poprzednie](intro-to-aspnet-mvc-4.md)
[dalej](adding-a-view.md)
