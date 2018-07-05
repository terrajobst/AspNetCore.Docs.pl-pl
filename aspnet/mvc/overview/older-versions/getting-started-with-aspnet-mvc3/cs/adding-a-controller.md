---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Dodawanie kontrolera (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Serivice dodatkiem Service Pack 1, które...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: ba6cc715f8c8eaf624ab5314e3afbfd68da11485
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370895"
---
<a name="adding-a-controller-c"></a>Dodawanie kontrolera (C#)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.
> 
> 
> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer, przy użyciu kodu źródłowego języka C# jest dostępny powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.


MVC to *model-view-controller*. MVC to wzorzec do tworzenia aplikacji, które są również wysokiej i łatwa w obsłudze. Aplikacje oparte na MVC zawiera:

- Kontrolery: Klas, które obsłużyć żądań przychodzących do aplikacji, pobrać modelu danych, a następnie określ Przeglądanie szablonów, które zwracają odpowiedź do klienta.
- Modele: Klasy reprezentujące dane aplikacji, które korzystają z logikę walidacji do wymuszania reguł biznesowych dla tych danych.
- Widoki: Pliki szablonów używanych przez aplikację w celu dynamicznego generowania odpowiedzi HTML.

Firma Microsoft będzie obejmujące wszystkie te pojęcia w tej serii samouczków a pokazują, jak ich używać do tworzenia aplikacji.

Zacznijmy od utworzenia klasy kontrolera. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nadaj nazwę nowemu kontrolerowi "HelloWorldController". Pozostaw domyślny szablon jako **pusty kontroler** i kliknij przycisk **Dodaj**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Należy zauważyć w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs*. Plik jest otwarty w środowisku IDE.

![](adding-a-controller/_static/image5.png)

Wewnątrz `public class HelloWorldController` blokowania, Utwórz dwie metody, które wyglądają podobnie do poniższego kodu. Kontroler zwraca ciąg HTML jako przykład.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Nosi nazwę usługi kontrolera `HelloWorldController` i nosi nazwę pierwszej metody powyżej `Index`. Możemy wywołać go z poziomu przeglądarki. Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5). W przeglądarce należy dołączyć "nazwę HelloWorld" w ścieżce w pasku adresu. (Na przykład na ilustracji poniżej jego `http://localhost:43246/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu. W powyższej metody kod zwrócony ciąg bezpośrednio. Otrzymaliśmy systemu, aby po prostu zwraca kod HTML i tak!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC wywołuje innego kontrolera klasy (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Domyślnej logiki mapowania używane przez platformę ASP.NET MVC używa formatu następująco, aby określić, jaki kod do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Pierwszą część adresu URL określa klasę kontrolera do wykonania. Dlatego */HelloWorld* mapuje `HelloWorldController` klasy. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego */HelloWorld/indeks* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania. Należy zauważyć, że musimy wskaż */HelloWorld* i `Index` metoda została użyta domyślna. Jest to spowodowane metodę o nazwie `Index` jest domyślną metodą, która zostanie wywołana na kontrolerze, jeśli nie jest jawnie określona.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg "To jest metoda powitalnej akcji...". Domyślne mapowanie MVC to `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL jest kontroler `HelloWorld` i `Welcome` jest metodą akcji. Pierwszy raz używasz `[Parameters]` wchodzi w skład jeszcze adresu URL.

![](adding-a-controller/_static/image7.png)

Zmodyfikujmy przykład nieco tak, aby przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Witaj? nazwa = Scott&amp;numtimes = 4*). Zmień swoje `Welcome` metodę, aby uwzględnić dwa parametry, jak pokazano poniżej. Należy pamiętać, że kod używa funkcji opcjonalny parametr języka C# z informacją, że `numTimes` parametr powinien domyślnie na 1, jeśli nie przekazano żadnej wartości tego parametru.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uruchom aplikację, a następnie przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Możesz spróbować różne wartości `name` i `numtimes` w adresie URL. System automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu do parametrów w metodzie.

![](adding-a-controller/_static/image8.png)

W obu tych przykładach kontroler ma zostały wykonując "VC" część MVC — oznacza to, widok i kontroler pracy. Kontroler zwraca HTML bezpośrednio. Zazwyczaj nie ma kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo trudne kodu. Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwiający Generowanie odpowiedzi HTML. Przyjrzyjmy się dalej, w jaki sposób możemy to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](intro-to-aspnet-mvc-3.md)
> [dalej](adding-a-view.md)
