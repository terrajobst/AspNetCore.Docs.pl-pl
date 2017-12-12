---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: "Tworzenie aplikacji bazy danych filmu w ciągu 15 minut z platformą ASP.NET MVC (C#) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Stephen Walther kompilacje całego opartej na bazie danych aplikacji ASP.NET MVC od początku do zakończenia. W tym samouczku jest doskonałym wprowadzenie dla osób, które są nowe..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 8294b5a8824c6a27e958e1ea78b7909a134447d2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Tworzenie aplikacji bazy danych filmu w ciągu 15 minut z platformą ASP.NET MVC (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

[Pobierz kod](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther kompilacje całego opartej na bazie danych aplikacji ASP.NET MVC od początku do zakończenia. W tym samouczku jest doskonałym wprowadzenie dla użytkowników, którzy dopiero zaczynasz korzystać z struktura MVC ASP.NET i którzy potrzebują się procesu tworzenia aplikacji platformy ASP.NET MVC.


Ten samouczek ma na celu zapewniają w pewnym sensie "co to jest przykład" do tworzenia aplikacji platformy ASP.NET MVC. W tym samouczku I wielkich za pośrednictwem tworzenia całej aplikacji ASP.NET MVC od początku do zakończenia. I opisano sposób tworzenia prostej aplikacji opartej na bazie danych, która ilustruje sposób można wyświetlić listę, tworzenia i edytowania rekordów bazy danych.

W celu uproszczenia procesu tworzenia aplikacji, firma Microsoft będzie korzystać z funkcji szkieletów programu Visual Studio 2008. Powiadomimy Visual Studio generowania kodu początkowej i zawartości dla naszych kontrolerów, modeli i widoków.

Użytkownicy mający doświadczenie z Active Server Pages lub ASP.NET, następnie należy odnaleźć platformy ASP.NET MVC bardzo znanych. Widoki ASP.NET MVC są bardzo podobne stron w aplikacji Active Server Pages. I, podobnie jak tradycyjnych aplikacji składnika ASP.NET Web Forms, ASP.NET MVC zapewnia pełny dostęp do bogatego zestawu języków i klas dostarczonych przez program .NET framework.

Mamy nadzieję, że moje jest, że w tym samouczku zapewni zorientować się, jak środowisko tworzenia aplikacji platformy ASP.NET MVC jest podobne i inne niż środowisko tworzenia aplikacji Active Server Pages lub formularzy sieci Web ASP.NET.

## <a name="overview-of-the-movie-database-application"></a>Omówienie filmu aplikacji bazy danych

Ponieważ naszym celem jest zapewnić proste, budujemy bardzo prostą aplikację filmu bazy danych. Nasze prostą aplikację filmu bazy danych pozwoli nam wykonać trzy czynności:

1. Zestaw rekordów bazy danych film
2. Tworzenie nowego rekordu bazy danych film
3. Edytowanie istniejącego rekordu bazy danych film

Ponownie ponieważ chcemy zapewnić proste, firma Microsoft będzie korzystać z minimalną liczbę funkcji niezbędnych do tworzenia aplikacji platformy ASP.NET MVC. Na przykład firma Microsoft nie będą mogły skorzystać z Test-Driven programowanie.

Aby można było utworzyć naszej aplikacji, musimy wykonać wszystkie z następujących czynności:

1. Utwórz projekt aplikacji sieci Web platformy ASP.NET MVC
2. Utwórz bazę danych
3. Tworzenie modelu bazy danych
4. Tworzenie kontrolera ASP.NET MVC
5. Utwórz widoki ASP.NET MVC

## <a name="preliminaries"></a>Wymagania wstępne

Potrzebujesz programu Visual Studio 2008 lub Visual Web Developer 2008 Express, do tworzenia aplikacji platformy ASP.NET MVC. Należy również pobrać platformę ASP.NET MVC.

Jeśli nie masz programu Visual Studio 2008, można pobrać 90-dniowa wersja próbna programu Visual Studio 2008 z tej witryny sieci Web:

[https://msdn.microsoft.com/en-us/VS2008/Products/cc268305.aspx](https://msdn.microsoft.com/en-us/vs2008/products/cc268305.aspx)

Alternatywnie można tworzyć ASP.NET MVC aplikacji z programu Visual Web Developer Express 2008. Jeśli zdecydujesz się używać programu Visual Web Developer Express musi mieć dodatku Service Pack 1. Visual Web Developer 2008 Express z dodatkiem Service Pack 1 można pobrać z tej witryny sieci Web:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Po zainstalowaniu programu Visual Studio 2008 lub Visual Web Developer 2008, musisz zainstalować platformę ASP.NET MVC. Platforma ASP.NET MVC można pobrać z następującej stronie internetowej:

[https://www.asp.NET/MVC/](../../../index.md)

> [!NOTE] 
> 
> Zamiast struktury programu ASP.NET i platforma ASP.NET MVC oddzielnie, można korzystać z Instalatora platformy sieci Web. Instalator platformy sieci Web jest aplikacja, która umożliwia łatwe zarządzanie zainstalowanych aplikacji komputera:
> 
> [https://www.microsoft.com/Web/Gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC

Zacznijmy od utworzenia nowego projektu aplikacji sieci Web platformy ASP.NET MVC w programie Visual Studio 2008. Wybierz opcję menu **plik, nowy projekt** i pojawi się okno dialogowe nowego projektu na rysunku 1. Wybierz C# jako języka programowania, a następnie wybierz szablon projektu aplikacji sieci Web platformy ASP.NET MVC. Nadaj nazwę MovieApp projekt i kliknij przycisk OK.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Rysunek 01**: okno dialogowe Nowy projekt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


Upewnij się, że program .NET Framework 3.5 należy wybrać z listy rozwijanej w górnej części okna dialogowego Nowy projekt lub szablonu projektu aplikacji sieci Web platformy ASP.NET MVC, nie będą wyświetlane.


Podczas tworzenia nowego projektu aplikacji sieci Web MVC programu Visual Studio wyświetli monit o utworzenie oddzielnych jednostkowy projekt testowy. Wyświetli się okno dialogowe na rysunku 2. Ponieważ firma Microsoft nie będzie można tworzenie testów w tym samouczku z powodu ograniczeń czasowych (i tak, powinien uważamy nieco winny na ten temat) wybierz **nr** opcję i kliknij przycisk **OK** przycisku.

> [!NOTE] 
> 
> Visual Web Developer nie wspiera projekty testowe.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Rysunek 02**: okna dialogowego Utwórz jednostkowy projekt testowy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


Aplikacji platformy ASP.NET MVC zawiera standardowy zestaw folderów: folder modele, widoki i kontrolery. Spowoduje to standardowy zestaw folderów w oknie Eksploratora rozwiązań. Potrzebujemy dodać pliki do każdego z folderów modele, widoki i kontrolery do zbudowania aplikacji filmu bazy danych.

Podczas tworzenia nowej aplikacji MVC za pomocą programu Visual Studio, możesz uzyskać przykładowej aplikacji. Ponieważ chcemy rozpocząć od początku, należy usunąć zawartość dla tej aplikacji przykładowej. Należy usunąć następujący plik i następujący folder:

- Controllers\HomeController.CS
- Views\Home

## <a name="creating-the-database"></a>Tworzenie bazy danych

Należy utworzyć bazę danych do przechowywania naszych filmu rekordów bazy danych. Szczęście program Visual Studio obejmuje bezpłatna baza danych o nazwie SQL Server Express. Wykonaj następujące kroki, aby utworzyć bazę danych:

1. Kliknij prawym przyciskiem myszy aplikację\_folderu danych w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz **danych** kategorii i wybierz **bazy danych programu SQL Server** szablonu (patrz rysunek 3).
3. Nazwa nowej bazy danych *MoviesDB.mdf* i kliknij przycisk **Dodaj** przycisku.

Po utworzeniu bazy danych można połączyć do bazy danych, klikając dwukrotnie plik MoviesDB.mdf znajdujący się w aplikacji\_folderem danych. Dwukrotne kliknięcie pliku MoviesDB.mdf powoduje otwarcie okna Eksploratora serwera.

> [!NOTE] 
> 
> Okno Eksploratora serwera nosi nazwę okna Eksploratora bazy danych w przypadku programu Visual Web Developer.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Rysunek 03**: tworzenie bazy danych programu Microsoft SQL Server ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


Następnie należy utworzyć nową tabelę bazy danych. W oknie Eksploratora serwera, kliknij prawym przyciskiem folder Tabele i wybierz opcję menu **Dodaj nową tabelę**. Wybranie tej opcji menu otwiera projektanta tabel bazy danych. Utwórz następujące kolumny bazy danych:

<a id="0.1_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Dopuszcza wartości null** |
| --- | --- | --- |
| Identyfikator | int | False |
| Tytuł | Nvarchar(100) | False |
| Dyrektor | Nvarchar(100) | False |
| DateReleased | DataGodzina | False |


Pierwsza kolumna, w kolumnie identyfikator ma dwie właściwości specjalnych. Najpierw należy oznaczyć jako kolumna klucza podstawowego w kolumnie identyfikator. Po wybraniu w kolumnie identyfikator, kliknij przycisk **klucz podstawowy** przycisk (jest ikonę, która wygląda jak klucz). Po drugie należy oznaczyć jako kolumnę tożsamości w kolumnie identyfikator. W oknie właściwości kolumny przewiń w dół do sekcji specyfikacji tożsamości i rozwiń go. Zmień **tożsamości jest** na wartość **tak**. Po ukończeniu tabeli powinien wyglądać na rysunku 4.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Rysunek 04**: filmy tabeli bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


Ostatnim krokiem jest, aby zapisać nową tabelę. Kliknij przycisk Zapisz (ikona dyskietek) i nadaj nowej tabeli filmy nazwy.

Po utworzeniu tabeli należy dodać niektóre rekordy filmu do tabeli. Kliknij prawym przyciskiem myszy tabelę filmów w oknie Eksploratora serwera i wybierz opcję menu **Pokaż dane tabeli**. Wprowadź listę ulubionych filmów (patrz rysunek 5).


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Rysunek 05**: wprowadzanie rekordów movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>Tworzenie modelu

Następnie należy utworzyć zestaw klas do reprezentowania naszej bazie danych. Należy utworzyć model bazy danych. Firma Microsoft będzie korzystać z programu Microsoft Entity Framework, można automatycznie wygenerować klasy dla modelu bazy danych.

> [!NOTE] 
> 
> Platforma ASP.NET MVC nie jest związana z programu Entity Framework firmy Microsoft. Można utworzyć bazy danych modelu klasy dzięki wykorzystaniu różnych obiektów relacyjnych mapowania (lub / M) narzędzia w tym składniku LINQ to SQL, Subsonic i NHibernate.


Wykonaj następujące kroki, aby uruchomić Kreatora modelu danych jednostki:

1. Kliknij prawym przyciskiem myszy folder modeli w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz **danych** kategorii i wybierz **modelu danych jednostki ADO.NET** szablonu.
3. Nadaj nazwę modelu danych *MoviesDBModel.edmx* i kliknij przycisk **Dodaj** przycisku.

Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator modelu danych jednostki (patrz rysunek 6). Wykonaj następujące kroki, aby zakończyć działanie kreatora:

1. W **wybierz zawartość modelu** krok, wybierz opcję **generowania z bazy danych** opcji.
2. W **wybierz połączenie danych** krok, użyj *MoviesDB.mdf* połączenie danych i nazwy *MoviesDBEntities* dla ustawień połączenia. Kliknij przycisk **dalej** przycisku.
3. W **wybierz obiekty bazy danych użytkownika** krok, rozwiń węzeł tabel, zaznacz tabelę filmów. Wprowadź przestrzeń nazw *MovieApp.Models* i kliknij przycisk **Zakończ** przycisku.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Rysunek 06**: generowania modelu bazy danych przy użyciu Kreatora modelu danych jednostki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Po zakończeniu pracy Kreatora modelu danych jednostki, zostanie otwarty projektant modelu danych jednostki. Projektant powinien być wyświetlany w tabeli bazy danych filmów (patrz rysunek 7).


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Rysunek 07**: Projektant modelu danych jednostki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


Musimy upewnić jednej zmiany przed kontynuowaniem. Kreator danych jednostek generuje klasę modelu o nazwie *filmy* reprezentujący filmy tabeli bazy danych. Ponieważ użyjemy klasy filmów do reprezentowania określonego filmu, należy zmodyfikować nazwę klasy, która ma być *film* zamiast *filmy* (pojedynczej zamiast mnogiej).

Kliknij dwukrotnie nazwę klasy na powierzchni projektanta i Zmień nazwę klasy z filmów filmu. Po wprowadzeniu tej zmiany, kliknij przycisk **zapisać** przycisk (ikona dyskietki) do generowania klasy filmu.

## <a name="creating-the-aspnet-mvc-controller"></a>Tworzenie kontrolera ASP.NET MVC

Następnym krokiem jest utworzenie kontrolera ASP.NET MVC. Kontroler jest odpowiedzialny za kontrolowanie sposobu użytkownik korzysta z aplikacji platformy ASP.NET MVC.

Wykonaj następujące kroki:

1. W oknie Eksploratora rozwiązań kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz opcję menu **Dodaj, kontrolera**.
2. W oknie dialogowym Dodaj kontroler, wprowadź nazwę *HomeController* i zaznacz pole wyboru z etykietą **Dodaj metody akcji dla scenariuszy tworzenia, aktualizacji i szczegółów** (patrz rysunek 8).
3. Kliknij przycisk **Dodaj** przycisk, aby dodać nowy kontroler do projektu.

Po wykonaniu tych kroków jest tworzony kontrolera 1 wyświetlania. Zwróć uwagę, że zawiera on metody o nazwie indeksu, uzyskać szczegółowe informacje, Utwórz i edytowania. W poniższych sekcjach dodamy niezbędne kod w celu uzyskania tych metod do pracy.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Rysunek 08**: dodawania nowego kontrolera ASP.NET MVC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**Wyświetlanie listy 1 – Controllers\HomeController.cs**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Lista rekordów bazy danych

Metoda indeks() kontrolera głównej jest domyślną metodą aplikacji platformy ASP.NET MVC. Po uruchomieniu aplikacji platformy ASP.NET MVC, metoda indeks() jest pierwszy wywoływanej metody kontrolera.

Aby wyświetlić listę rekordów z tabeli bazy danych filmy użyjemy metody indeks(). Firma Microsoft będzie korzystać z bazy danych modelu klasy, które utworzony wcześniej można pobrać za pomocą metody indeks() filmu rekordów bazy danych.

Tak, aby zawierał prywatnej nowe pole o nazwie zostały jest zmodyfikowane klasy HomeController wyświetlania 2 \_bazy danych. Klasa MoviesDBEntities reprezentuje nasz model bazy danych i użyjemy tej klasy do komunikowania się z naszej bazie danych.

Po zmodyfikowaniu również metoda indeks() wyświetlania 2. Metoda indeks() używa klasy MoviesDBEntities pobrać wszystkie rekordy film filmy tabeli bazy danych. Wyrażenie  *\_bazy danych. MovieSet.ToList()* zwraca listę wszystkich rekordów film z filmów tabeli bazy danych.

Lista filmów są przekazywane do widoku. Wszystko, co jest przekazywane do metody View() jest przekazywane do widoku danych widoku.

**Lista 2 — Controllers/HomeController.cs (metoda modyfikacji indeksu)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

Metoda indeks() zwraca widok o nazwie indeksu. Należy utworzyć ten widok, aby wyświetlić listę filmu rekordów bazy danych. Wykonaj następujące kroki:


Należy skompilować projekt (wybierz opcję menu **kompilacji Kompiluj rozwiązanie**) przed otwarciem **Dodaj widok** okna dialogowego lub klasy nie będą wyświetlane w **wyświetlić klasy danych** Lista rozwijana.


1. Kliknij prawym przyciskiem myszy w edytorze kodu metody indeks() i wybierz opcję menu **Dodaj widok** (patrz rysunek 9).
2. W oknie dialogowym Dodaj widok, sprawdź, czy pole wyboru z etykietą **utworzyć widok jednoznacznie** jest zaznaczony.
3. Z **wyświetlania zawartości** listy rozwijanej wybierz wartość *listy*.
4. Z **wyświetlić klasy danych** listy rozwijanej wybierz wartość *MovieApp.Models.Movie*.
5. Kliknij przycisk Dodaj, aby utworzyć nowy wyświetlania (patrz rysunek 10).

Po wykonaniu tych kroków nowy widok o nazwie Index.aspx jest dodawana do Views\Home folder. Zawartość widoku indeksu są uwzględnione w wersji 3 wyświetlania.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Rysunek 09**: Dodawanie widoku z akcji kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Na rysunku nr 10**: Tworzenie nowego widoku w oknie dialogowym Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**Wyświetlanie listy 3 — Views\Home\Index.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

Widok indeksu Wyświetla wszystkie rekordy film z filmów tabeli bazy danych w tabeli HTML. Widok zawiera pętli foreach, który iteruje po każdej filmu reprezentowany przez właściwość ViewData.Model. Jeśli aplikacja jest uruchamiane za naciskać klawisz F5, zostanie wyświetlone strony sieci web w rysunek 11.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Rysunek 11**: widok Index ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>Tworzenie nowych rekordów bazy danych

Widok indeksu utworzony w poprzedniej sekcji zawiera łącze do tworzenia nowych rekordów bazy danych. Spróbuj teraz i implementują logikę i tworzenie widoku niezbędne do tworzenia nowych rekordów bazy danych filmu.

Kontroler główna zawiera dwie metody o nazwie Create(). Pierwsza metoda Create() nie ma parametrów. To przeciążenie metody Create() służy do wyświetlania formularza HTML do tworzenia nowego rekordu bazy danych filmu.

Druga metoda Create() ma parametr FormCollection. To przeciążenie metody Create() jest wywoływana, gdy formularz HTML do tworzenia nowych film jest przesyłana do serwera. Należy zauważyć, że ta druga metoda Create() ma atrybut AcceptVerbs, który uniemożliwia metoda wywoływana, chyba że operacji POST protokołu HTTP.

Ta druga metoda Create() został zmodyfikowany w klasie HomeController zaktualizowane w listę 4. Nowa wersja metody Create() przyjmuje parametr filmu i zawiera logikę wstawianie nowych filmu do filmy tabeli bazy danych.

> [!NOTE] 
> 
> Zwróć uwagę, atrybut wiązania. Ponieważ nie chcemy zaktualizować właściwości Id film z formularza HTML, potrzebujemy jawnie wykluczyć tę właściwość.


**Wyświetlanie 4 — Controllers\HomeController.cs (zmodyfikowane metody Create)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio można łatwo utworzyć formularz służący do tworzenia nowej bazy danych filmu rekordów (patrz rysunek 12). Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy metodę Create() w edytorze kodu i wybierz opcję menu **Dodaj widok**.
2. Sprawdź, czy pole wyboru z etykietą **utworzyć widok jednoznacznie** jest zaznaczony.
3. Z **wyświetlania zawartości** listy rozwijanej wybierz wartość *Utwórz*.
4. Z **wyświetlić klasy danych** listy rozwijanej wybierz wartość *MovieApp.Models.Movie*.
5. Kliknij przycisk **Dodaj** przycisk, aby utworzyć nowy widok.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Rysunek 12**: Dodawanie widoku Create ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


Visual Studio automatycznie generuje widok w listę 5. Ten widok zawiera formularza HTML, który zawiera pola, które odpowiadają każdej z właściwości klasy filmu.

**Wyświetlanie listy 5 — Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> Formularza HTML, wygenerowane za pomocą okna dialogowego Dodawanie widoku generuje identyfikator pola formularza. Ponieważ w kolumnie Identyfikator kolumny tożsamości, nie należy tego pola formularza i można bezpiecznie usunąć go.


Po dodaniu widoku tworzenie nowych rekordów filmu można dodać do bazy danych. Uruchom aplikację, naciskając klawisz F5, a następnie kliknij polecenie Utwórz nowy linku umożliwia wyświetlenie formularza na rysunku 13. Jeśli zakończyć i Prześlij formularz, zostanie utworzony nowy rekord filmu bazy danych.

Zwróć uwagę, automatyczne pobieranie walidacji formularza. Jeśli udzielisz wprowadź datę wersji filmu lub wprowadź Data wydania nieprawidłowy, formularza zostanie wyświetlony ponownie i zostanie wyróżniona pola Data wydania.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Rysunek 13**: Tworzenie nowego rekordu bazy danych movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>Edytowanie istniejących rekordów bazy danych

W poprzednich sekcjach omówiono sposób listy i tworzenia nowych rekordów bazy danych. W tej sekcji końcowego omówiono sposób edycji istniejących rekordów bazy danych.

Najpierw należy wygenerować formularza edycji. Ten krok jest proste, ponieważ program Visual Studio wygeneruje formularzu edytowania firmie Microsoft w automatycznie. Otwórz klasy HomeController.cs w edytorze kodu programu Visual Studio i wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy metodę Edit() w edytorze kodu i wybierz opcję menu **Dodaj widok** (patrz rysunek 14).
2. Zaznacz pole wyboru z etykietą **utworzyć widok jednoznacznie**.
3. Z **wyświetlania zawartości** listy rozwijanej wybierz wartość *Edytuj*.
4. Z **wyświetlić klasy danych** listy rozwijanej wybierz wartość *MovieApp.Models.Movie*.
5. Kliknij przycisk **Dodaj** przycisk, aby utworzyć nowy widok.

Wykonanie tych kroków dodaje nowy widok o nazwie Edit.aspx Views\Home folder. Ten widok zawiera formularza HTML do edytowania rekordu filmu.


[![Okno dialogowe nowego projektu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Rysunek 14**: Dodawanie widoku edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> Widok edycji zawiera pola formularza HTML, który odpowiada właściwości Id filmu. Ponieważ nie mają edycji wartość właściwości identyfikator osoby, należy usunąć to pole formularza.


Na koniec należy zmodyfikować kontrolera głównej, tak aby obsługuje edytowania rekordu bazy danych. Zaktualizowano klasy HomeController znajduje się w 6 wyświetlania.

**Wyświetlanie listy 6 — Controllers\HomeController.cs (Edycja metody)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

Dodatkową logikę do obu przeciążenia metody Edit() zostały dodane w wyświetlania 6. Pierwsza metoda Edit() zwraca filmu rekordu bazy danych, który odpowiada na parametr identyfikatora przekazany do metody. Drugi przeciążenia wykonuje aktualizacje do rekordu filmu w bazie danych.

Zwróć uwagę, że należy pobrać oryginalnego film, a następnie wywołać ApplyPropertyChanges(), aby zaktualizować filmu istniejących w bazie danych.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było pozwalają zorientować się proces tworzenia aplikacji platformy ASP.NET MVC. Mam nadzieję, że wykryte, że tworzenie aplikacji sieci web platformy ASP.NET MVC jest bardzo podobny do obsługi tworzenia aplikacji Active Server Pages ani program ASP.NET.

W tym samouczku będziemy zbadać tylko najbardziej podstawowe funkcje platformę ASP.NET MVC. W przyszłości samouczki możemy Poznaj głębiej tematy, takimi jak kontrolery, akcji kontrolera, widoki, dane widoku i pomocników HTML.

>[!div class="step-by-step"]
[Dalej](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
