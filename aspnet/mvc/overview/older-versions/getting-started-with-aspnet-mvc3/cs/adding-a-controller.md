---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Dodawanie kontrolera (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Serivice dodatkiem Service Pack 1, które...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868298"
---
<a name="adding-a-controller-c"></a>Dodawanie kontrolera (C#)
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.
> 
> 
> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodu źródłowego C# jest dostępna powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przełącz się do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.


Oznacza MVC *model-view-controller*. MVC to wzorzec do tworzenia aplikacji, które są dobrze zaprojektowanej architektury i łatwe w konserwacji. Aplikacje oparte na MVC zawiera:

- Kontrolery: Klas, które obsłużyć żądań przychodzących do aplikacji, pobrać modelu danych, a następnie określ szablonów widoków, które zwracają odpowiedź do klienta.
- Modele: Klasy reprezentujące dane aplikacji, które korzystają z logikę weryfikacji do wymuszania reguł biznesowych dla tych danych.
- Widoków: Pliki szablonów aplikacja używa do dynamicznego generowania odpowiedzi HTML.

Firma Microsoft będzie można obejmujące wszystkie te pojęcia w tym samouczku i opisano, jak ich używać do tworzenia aplikacji.

Zacznijmy od utworzenia klasy kontrolera. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nadaj nazwę nowemu kontrolerowi "HelloWorldController". Pozostaw domyślny szablon jako **pusty kontroler** i kliknij przycisk **Dodaj**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Zwróć uwagę, w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs*. Plik jest otwarty w IDE.

![](adding-a-controller/_static/image5.png)

Wewnątrz `public class HelloWorldController` bloków, Utwórz dwie metody, których wyglądać podobnie do następującego kodu. Kontroler zwróci ciąg HTML jako przykład.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Kontroler nosi nazwę `HelloWorldController` i nosi nazwę pierwsza metoda powyżej `Index`. Teraz wywołać go w przeglądarce. Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5). W przeglądarce Dołącz "HelloWorld" do ścieżki w pasku adresu. (Na przykład na ilustracji poniżej jego `http://localhost:43246/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu. W powyższej metodzie kod zwrócony ciąg bezpośrednio. Informację systemu tylko zwrócić kod HTML i jak!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC wywołuje klasy innego kontrolera (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Domyślna logika mapowania używane przez program ASP.NET MVC w formacie takie ustalenie, jaki kod do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Pierwsza część adresu URL określa klasy kontrolera do wykonania. Dlatego */HelloWorld* mapuje `HelloWorldController` klasy. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego */HelloWorld/indeksu* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania. Należy zauważyć, że Musieliśmy aby przejść do */HelloWorld* i `Index` użyto metody domyślnie. Jest to spowodowane metodę o nazwie `Index` jest domyślna metoda, która zostanie wywołana na kontrolerze, jeśli nie został jawnie określony.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg "Jest to metoda akcji Zapraszamy...". Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontrolera jest `HelloWorld` i `Welcome` jest metodą akcji. Nie były używane `[Parameters]` część jeszcze adresu URL.

![](adding-a-controller/_static/image7.png)

Załóżmy przykładzie nieco zmodyfikować tak, aby niektóre informacje o parametrach z adresu URL można przekazać do kontrolera (na przykład */HelloWorld/Zapraszamy? nazwa = Scott&amp;numtimes = 4*). Zmień Twoje `Welcome` metodę w celu uwzględnienia dwóch parametrów, jak pokazano poniżej. Należy pamiętać, że kod jest używana funkcja opcjonalny parametr C# z informacją, że `numTimes` parametr powinien domyślnie 1, jeśli nie przekazano żadnej wartości tego parametru.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uruchom aplikację i przejdź do adresu URL przykład (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Możesz spróbować różnych wartości `name` i `numtimes` w adresie URL. System automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu w parametrach w metodę.

![](adding-a-controller/_static/image8.png)

W obu tych przykładach kontroler ma zostały czynności "VC" część MVC — to znaczy pracy widok i kontroler. Kontroler bezpośrednio zwraca HTML. Zwykle nie mają kontrolerów bezpośrednio, zwracanie HTML, ponieważ staje się bardzo trudne kodu. Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwia generowanie odpowiedzi HTML. Oto dalej w sposób firma Microsoft można to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](intro-to-aspnet-mvc-3.md)
> [dalej](adding-a-view.md)
