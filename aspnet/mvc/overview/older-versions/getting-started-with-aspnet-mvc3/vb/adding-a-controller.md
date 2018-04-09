---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Dodawanie kontrolera (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 9a433083c31c7929f7599e52800c887f301d7727
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller-vb"></a>Dodawanie kontrolera (VB)
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-a-controller.md) tego samouczka.


Oznacza MVC *model-view-controller*. MVC to wzorzec do tworzenia aplikacji w taki sposób, że każda część ma oddzielne odpowiedzialność:

- Model: Dane dla aplikacji.
- Widoki: Pliki szablonów będzie używać aplikacja do dynamicznego generowania odpowiedzi HTML.
- Kontrolery: Klas, które obsługują przychodzące żądania adresu URL do aplikacji, pobrać modelu danych, a następnie określ szablonów widoków, które renderują odpowiedzi do klienta.

Firma Microsoft będzie można obejmujące wszystkie te pojęcia w tym samouczku i opisano, jak ich używać do tworzenia aplikacji.

Tworzenie nowego kontrolera, klikając prawym przyciskiem myszy *kontrolerów* folderu w **Eksploratora rozwiązań** , a następnie wybierając **Dodaj kontroler**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Nowemu kontrolerowi nazwę &quot;HelloWorldController&quot; i kliknij przycisk **Dodaj**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Zwróć uwagę, w **Eksploratora rozwiązań** po prawej stronie utworzony dla Ciebie nowy plik o nazwie *HelloWorldController.cs* i że plik jest otwarty w IDE.

Wewnątrz nowe `public class HelloWorldController` bloków, Utwórz dwie nowe metody, które wyglądać podobnie do następującego kodu. Na przykład zostanie zwrócona ciąg HTML bezpośrednio z kontrolerem.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Kontroler nosi nazwę `HelloWorldController` i nosi nazwę nowego metodę `Index`. Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5). Gdy przeglądarka ma uruchomienia, dołącz &quot;HelloWorld&quot; do ścieżki w pasku adresu. (Na tym komputerze, ma `http://localhost:43246/HelloWorld`) przeglądarka będzie wyglądać podobnie jak na poniższym zrzucie ekranu. W powyższej metodzie kod zwrócony ciąg bezpośrednio. Firma Microsoft informację systemu tylko zwrócić kod HTML i jak!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC wywołuje klasy innego kontrolera (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL. Domyślna logika mapowania używane przez program ASP.NET MVC w formacie takie do kontrolowania, jaki kod jest wywoływany:

`/[Controller]/[ActionName]/[Parameters]`

Pierwsza część adresu URL określa klasy kontrolera do wykonania. Dlatego */HelloWorld* mapuje `HelloWorldController` klasy. Druga część adresu URL określa metody akcji w klasie, do wykonania. Dlatego */HelloWorld/indeksu* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania. Należy zauważyć, że Musieliśmy odwiedzać */HelloWorld* powyżej i `Index` użyto metody domyślnie. Jest to spowodowane metodę o nazwie `Index` jest domyślna metoda, która zostanie wywołana na kontrolerze, jeśli nie został jawnie określony.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda akcji Zapraszamy... &quot;. Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontrolera jest `HelloWorld` i `Welcome` jest metoda. Firma Microsoft nie były używane `[Parameters]` część jeszcze adresu URL.

![](adding-a-controller/_static/image6.png)

Umożliwia przykładzie nieco zmodyfikować tak, aby firma Microsoft może przekazać niektóre informacje o parametrach w z adresu URL do kontrolera (na przykład */HelloWorld/Zapraszamy? name = Scott&amp;numtimes = 4*). Zmień Twoje `Welcome` metodę w celu uwzględnienia dwóch parametrów, jak pokazano poniżej. Należy pamiętać, że firma Microsoft używano z informacją, że funkcja opcjonalny parametr VB `numTimes` parametr powinien domyślnie 1, jeśli nie przekazano żadnej wartości tego parametru.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Uruchom aplikację i przejdź do `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Możesz spróbować różnych wartości `name` i `numtimes`. System automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu w parametrach w metodę.

![](adding-a-controller/_static/image7.png)

W obu tych przykładach kontroler ma zostały wykonywania VC część MVC, który jest praca widok i kontroler. Kontroler bezpośrednio zwraca HTML. Zwykle nie chcemy kontrolerów bezpośrednio, zwracanie HTML, ponieważ staje się bardzo trudne kodu. Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwia generowanie odpowiedzi HTML. Oto jak możemy można to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](intro-to-aspnet-mvc-3.md)
> [dalej](adding-a-view.md)
