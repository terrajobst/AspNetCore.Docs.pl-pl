---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: "Uzyskiwanie dostępu do danych do modelu kontrolera (VB) | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f1a1db907aa1d0a62af9b363fabfc74ac11acc68
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>Uzyskiwanie dostępu do danych do modelu kontrolera (VB)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/accessing-your-models-data-from-a-controller.md) tego samouczka.


W tej sekcji utworzysz nową `MoviesController` klasy i pisania kodu, który pobiera dane filmu i wyświetla je w przeglądarce, za pomocą szablonu widoku. Pamiętaj skompilować aplikację przed kontynuowaniem.

Kliknij prawym przyciskiem myszy *kontrolerów* folderze i utworzyć nową `MoviesController` kontrolera. Wybierz następujące opcje:

- Nazwa kontrolera: **MoviesController**. (Jest to wartość domyślna).
- Szablon: **kontrolera z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework**.
- Klasa modelu: **Movie (MvcMovie.Models)**.
- Klasa kontekstu danych: **MovieDBContext (MvcMovie.Models)**.
- Widoki: **Razor (CSHTML)**. (Ustawienie domyślne).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Kliknij przycisk **Dodaj**. Visual Web Developer tworzy następujące pliki i foldery:

- *MoviesController.vb* plik do projektu *kontrolerów* folderu.
- A *filmy* folderu do projektu *widoków* folderu.
- *Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, i *Index.vbhtml* w nowym *Views\Movies* folderu.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Mechanizm szkieletów ASP.NET MVC 3 tworzone automatycznie CRUD (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoków. Masz teraz aplikacji sieci web w pełni funkcjonalne, która umożliwia tworzenie, listy, edytować i usuwać wpisy filmu.

Uruchom aplikację i przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL na pasku adresu przeglądarki. Ponieważ aplikacja jest polegania na domyślny routing (zdefiniowany w *Global.asax* pliku), żądanie `http://localhost:xxxxx/Movies` jest kierowany do domyślnej `Index` metody akcji `Movies` kontrolera. Innymi słowy żądanie `http://localhost:xxxxx/Movies` jest taka sama jak żądanie `http://localhost:xxxxx/Movies/Index`. Wynik jest wyświetlana pusta lista filmów, ponieważ nie dodano żadnych serwerów jeszcze.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz **Utwórz nowy** łącza. Wprowadź niektóre szczegóły na temat film, a następnie kliknij przycisk **Utwórz** przycisku.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Kliknięcie przycisku **Utwórz** kliknięcie przycisku powoduje formularza można opublikować na serwerze, gdzie informacje filmu są zapisywane w bazie danych. Użytkownik jest przekierowywane do */Movies* adres URL, w którym można obejrzeć film nowo utworzony na liście.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Utwórz kilka więcej wpisów filmu. Spróbuj **Edytuj**, **szczegóły**, i **usunąć** łącza, które są wszystkie funkcjonalności.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz *Controllers\MoviesController.vb* pliku i sprawdź, czy wygenerowany `Index` metody. Część kontroler film z `Index` metody są wyświetlane poniżej.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

Poniższy wiersz z `MoviesController` klasy tworzy filmu kontekst bazy danych, jak opisano wcześniej. Film kontekst bazy danych służy do zapytań, edytowanie i usuwanie filmów.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Żądanie `Movies` kontrolera zwraca wszystkie wpisy w `Movies` tabeli filmu bazy danych, a następnie przekazuje wyniki w `Index` widoku.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silnie Typizowane modeli i @model — słowo kluczowe

Wcześniej w tym samouczku widać, jak kontroler może przekazać dane i obiekty do szablonu widoku przy użyciu `ViewBag` obiektu. `ViewBag` Jest to obiekt dynamiczny, które oferują wygodny sposób późnym wiązaniem do przekazywania informacji do widoku.

ASP.NET MVC dostarcza również możliwość przekazywania silnie typizowanej danych lub obiektów do szablonu widoku. To silnie typizowane metody umożliwia lepsze kompilacji Sprawdzanie kodu i bardziej zaawansowane funkcje IntelliSense w edytorze programu Visual Web Developer. Firma Microsoft korzysta z tej metody z `MoviesController` klasy i *Index.vbhtml* widok szablonu.

Zwróć uwagę, jak kod tworzy [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) obiektu, gdy wywołuje `View` metody pomocnika w `Index` metody akcji. Kod następnie przekazuje to `Movies` listy z kontrolera do widoku:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

W tym `@ModelType` instrukcji w górnej części pliku szablonu widoku, można określić typu obiektu, który oczekuje widoku. Podczas tworzenia kontrolera filmu Visual Web Developer automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Index.vbhtml* pliku:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

To `@ModelType` dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Index.vbhtml* szablonu, kod w pętli filmów za pomocą ćwiczeń `foreach` instrukcji na silnie typizowaną `Model` obiektu:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Ponieważ `Model` obiektu jest silnie typizowane (jako `IEnumerable<Movie>` obiektu), każdy `item` jest typu obiektu w pętli `Movie`. Wśród innych korzyści oznacza to, Pobierz sprawdzanie kompilacji kodu, a następnie pełne obsługę funkcji IntelliSense w edytorze kodu:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Praca z programu SQL Server Compact

Entity Framework Code First wykrył, że podany ciąg połączenia bazy danych wskazywanego `Movies` bazy danych, która nie istnieje jeszcze, więc Code First baza danych utworzona automatycznie. Możesz sprawdzić, czy jest on utworzony przez wyszukiwanie *aplikacji\_danych* folderu. Jeśli nie widzisz *Movies.sdf* plików, kliknij **Pokaż wszystkie pliki** przycisk **Eksploratora rozwiązań** narzędzi, kliknij przycisk **Odśwież** przycisk, a następnie rozwiń *aplikacji\_danych* folderu.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Kliknij dwukrotnie *Movies.sdf* otworzyć **Eksploratora serwera**. Następnie rozwiń węzeł **tabel** folderze, aby zobaczyć tabele, które zostały utworzone w bazie danych.

> [!NOTE]
> Jeśli błąd pojawia się po dwukrotnym kliknięciu *Movies.sdf*, upewnij się, że zainstalowano **Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**. (Aby łącza do oprogramowania, zobacz listę warunków wstępnych w tej serii samouczek, część 1). Po zainstalowaniu wersji teraz, musisz zamknąć i otworzyć ponownie Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Istnieją dwie tabele, jeden dla `Movie` zestaw jednostek, a następnie `EdmMetadata` tabeli. `EdmMetadata` Tabela jest używana przez program Entity Framework, aby określić, kiedy modelu i bazy danych nie są zsynchronizowane.

Kliknij prawym przyciskiem myszy `Movies` tabeli i wybierz **Pokaż dane tabeli** do wyświetlania danych został utworzony.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Kliknij prawym przyciskiem myszy `Movies` tabeli i wybierz **Edytuj schemat tabeli**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Powiadomienie jak schemat `Movies` mapy do tabel `Movie` klasy utworzony wcześniej. Entity Framework Code First automatycznie utworzyć ten schemat na podstawie Twojej `Movie` klasy.

Po zakończeniu zamknij połączenie. (Jeśli nie zamkniesz połączenie, może być błąd pojawia się przy następnym uruchomieniu projektu).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Istnieje już baza danych i proste listy na stronie zawartości z niego. W następnym samouczku będzie firma Microsoft bada reszty szkieletu kodu i dodać `SearchIndex` — metoda i `SearchIndex` widoku, który umożliwia wyszukiwanie filmów w tej bazie danych.

>[!div class="step-by-step"]
[Poprzednie](adding-a-model.md)
[dalej](examining-the-edit-methods-and-edit-view.md)
