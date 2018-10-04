---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu za pomocą kontrolera (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 47d6e611ea79cf8c217f1f793d342b065233081f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577889"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera (VB)
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/accessing-your-models-data-from-a-controller.md) po ukończeniu tego samouczka.


W tej sekcji utworzysz nową `MoviesController` klasy, a następnie napisać kod, który pobiera dane filmów i wyświetla go w przeglądarce, za pomocą szablonu widoku. Pamiętaj skompilować aplikację przed kontynuowaniem.

Kliknij prawym przyciskiem myszy *kontrolerów* folderze i utworzyć nową `MoviesController` kontrolera. Wybierz następujące opcje:

- Nazwa kontrolera: **MoviesController**. (Jest to wartość domyślna).
- Szablon: **kontroler z akcjami odczytu/zapisu oraz widokami używający narzędzia Entity Framework**.
- Klasa modelu: **Movie (MvcMovie.Models)**.
- Klasa kontekstu danych: **MovieDBContext (MvcMovie.Models)**.
- Widoki: **Razor (CSHTML)**. (Ustawienie domyślne).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Kliknij przycisk **Dodaj**. Visual Web Developer tworzy następujące pliki i foldery:

- *MoviesController.vb* pliku w projekcie *kontrolerów* folderu.
- A *filmy* folderu w projekcie *widoków* folderu.
- *Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, i *Index.vbhtml* w nowym *Views\Movies* folderu.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Mechanizm tworzenia szkieletu ASP.NET MVC 3 tworzone automatycznie CRUD (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoków. Masz teraz aplikację internetową w pełni funkcjonalne, która umożliwia tworzenie, listy, edytować i usuwać wpisy filmu.

Uruchom aplikację, a następnie przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL w pasku adresu przeglądarki. Ponieważ aplikacja powołuje się na domyślny routing (zdefiniowane w *Global.asax* pliku), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowany do domyślnego `Index` metody akcji `Movies` kontrolera. Innymi słowy żądanie przeglądarki `http://localhost:xxxxx/Movies` skutecznie jest taka sama jak żądanie przeglądarki `http://localhost:xxxxx/Movies/Index`. Wynik jest pusta lista filmy, ponieważ nie dodano żadnego jeszcze.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Tworzenie filmu

Wybierz **Utwórz nowy** łącza. Wprowadź informacje na temat filmów, a następnie kliknij przycisk **Utwórz** przycisku.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Klikając **Utwórz** przycisku powoduje, że formularz do opublikowania na serwerze, gdzie informacje filmu są zapisywane w bazie danych. Użytkownik jest następnie przekierowywane do */Movies* adresu URL, w którym można zobaczyć nowo utworzoną filmu na liście.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Utwórz kilka więcej wpisów filmu. Spróbuj **Edytuj**, **szczegóły**, i **Usuń** łącza, które są wszystkie funkcjonalności.

## <a name="examining-the-generated-code"></a>Badanie wygenerowanego kodu

Otwórz *Controllers\MoviesController.vb* plików i zbadaj wygenerowany `Index` metody. Część kontroler film z `Index` metoda znajdują się poniżej.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

Poniższy wiersz z `MoviesController` klasy tworzy kontekst bazy danych filmów, zgodnie z wcześniejszym opisem. Kontekst bazy danych filmów umożliwia zapytania, edytowanie i usuwanie filmów.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Żądanie `Movies` kontroler zwraca wszystkie wpisy w `Movies` tabeli bazy danych filmów, a następnie przekazuje wyniki do `Index` widoku.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silnie Typizowane modeli i @model — słowo kluczowe

Wcześniej w tym samouczku pokazano, jak kontroler można przekazać dane i obiekty za pomocą szablonu widoku `ViewBag` obiektu. `ViewBag` To obiekt dynamiczny, która zapewnia wygodny sposób z późnym wiązaniem do przekazywania informacji do widoku.

ASP.NET MVC udostępnia również możliwość przekazywania silnie typizowane dane i obiekty w szablonie widoku. Silnie typizowane to podejście umożliwia lepsze kompilacji Sprawdzanie kodu oraz ulepszoną funkcję IntelliSense w edytorze programu Visual Web Developer. Firma Microsoft korzysta z tym podejściem `MoviesController` klasy i *Index.vbhtml* szablon widoku.

Zwróć uwagę, jak kod tworzy [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) obiektu, kiedy wywoływanych przez nią `View` metody pomocnika w `Index` metody akcji. Kod następnie przekazuje to `Movies` listy z kontrolera do widoku:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Jeśli dołączysz `@ModelType` instrukcji w górnej części pliku szablonu widoku, można określić typu obiektu, który oczekuje, że widok. Podczas tworzenia kontrolera filmu Visual Web Developer automatycznie uwzględnione następujące `@model` instrukcji na górze *Index.vbhtml* pliku:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

To `@ModelType` dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Index.vbhtml* szablonu, kod w pętli filmy wykonując `foreach` instrukcji na silnie typizowaną `Model` obiektu:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Ponieważ `Model` obiektu zdecydowanie jest wpisane (jako `IEnumerable<Movie>` obiektu), każdy `item` obiektu w pętli jest wpisana jako `Movie`. Wśród innych korzyści oznacza to, możesz uzyskać w czasie kompilacji sprawdzania kodu i pełną obsługę technologii IntelliSense w edytorze kodu:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Praca z użyciem programów SQL Server Compact

Entity Framework Code First wykrył, że parametry połączenia bazy danych, który został dostarczony wskazywany `Movies` bazy danych, która nie istnieje jeszcze, więc Code First baza danych utworzona automatycznie. Możesz sprawdzić, czy został on utworzony przez wyszukiwanie *aplikacji\_danych* folderu. Jeśli nie widzisz *Movies.sdf* plików, kliknij **Pokaż wszystkie pliki** znajdujący się w **Eksploratora rozwiązań** narzędzi, kliknij przycisk **Odśwież** przycisk, a następnie rozwiń *aplikacji\_danych* folderu.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Kliknij dwukrotnie *Movies.sdf* otworzyć **Eksploratora serwera**. Następnie rozwiń **tabel** folderze, aby zobaczyć tabele, które zostały utworzone w bazie danych.

> [!NOTE]
> Jeśli wystąpi błąd, po dwukrotnym kliknięciu *Movies.sdf*, upewnij się, że zainstalowano **Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**. (Aby uzyskać łącza do oprogramowania, zobacz listę wymagań wstępnych, w części 1 w tej serii samouczków). Jeśli teraz zainstalować wersję musisz zamknąć i otworzyć ponownie Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Istnieją dwie tabele, jeden dla `Movie` zestawu jednostek i następnie `EdmMetadata` tabeli. `EdmMetadata` Tabela jest używana przez program Entity Framework w celu określenia, kiedy modelu i bazy danych nie są zsynchronizowane.

Kliknij prawym przyciskiem myszy `Movies` tabeli, a następnie wybierz pozycję **Pokaż dane tabeli** do wyświetlenia danych został utworzony.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Kliknij prawym przyciskiem myszy `Movies` tabeli, a następnie wybierz pozycję **edytować schemat tabeli**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Zwróć uwagę jak schemat `Movies` mapy do tabel `Movie` klasa została utworzona wcześniej. Entity Framework Code First automatycznie tworzony w tym schemacie na podstawie Twojej `Movie` klasy.

Gdy skończysz, zamknij połączenie. (Jeśli nie zamkniesz połączenie, możesz otrzymać błąd przy następnym uruchomieniu projektu).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Masz teraz bazę danych i prostego strony do wyświetlania zawartości z niego. W następnym samouczku utworzymy Sprawdź pozostałą część utworzony szkielet kodu i dodamy `SearchIndex` metody i `SearchIndex` widok, który umożliwia wyszukiwanie filmów w tej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-model.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)
