---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: Tworzenie aplikacji bazy danych filmów w ciągu 15 minut przy użyciu platformy ASP.NET MVC (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther kompilacji całego opartego na bazie danych aplikacji ASP.NET MVC od początku do zakończenia. Ten samouczek stanowi znakomite wprowadzenie dla osób, które są nowe t...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: a64d140eba4ebf489486af1a0be6269a8b904c13
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372598"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Tworzenie aplikacji bazy danych filmów w ciągu 15 minut przy użyciu platformy ASP.NET MVC (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

[Pobierz program Code](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Autor: Stephen Walther kompilacji całego opartego na bazie danych aplikacji ASP.NET MVC od początku do zakończenia. Ten samouczek stanowi znakomite wprowadzenie dla osób, którzy są nowe struktury programu ASP.NET MVC i którzy chcą poznać proces tworzenia aplikacji ASP.NET MVC.


Ten samouczek ma na celu dać Ci przedsmak "co to jest takie jak" do tworzenia aplikacji ASP.NET MVC. W tym samouczku I rażenia procesem tworzenia całej aplikacji ASP.NET MVC od początku do zakończenia. Czy mogę pokazują sposób tworzenia prostej aplikacji opartej na bazie danych, który ilustruje, jak można wyświetlać, tworzenia i edytowania rekordów bazy danych.

Aby uprościć proces tworzenia aplikacji, firma Microsoft będzie korzystać z funkcji tworzenia szkieletów programu Visual Studio 2008. Poinformujemy programu Visual Studio generować kod początkowy i zawartości dla naszych, modeli, widoków i kontrolerów.

Użytkownicy mający doświadczenie z stron ASP lub ASP.NET, następnie powinien znajdować się platformy ASP.NET MVC znajomo. Widoki ASP.NET MVC są bardzo podobne stron w aplikacji Active Server Pages. I podobnie jak w przypadku tradycyjnych aplikacji formularzy sieci Web ASP.NET ASP.NET MVC zapewnia pełny dostęp do bogatego zestawu języków i klas dostarczonych przez program .NET framework.

Mamy nadzieję, że Moja jest, w tym samouczku zapewni zorientować się, jak środowisko tworzenia aplikacji ASP.NET MVC jest inny niż środowisko tworzenia aplikacji stron ASP lub ASP.NET Web Forms i podobne.

## <a name="overview-of-the-movie-database-application"></a>Omówienie aplikacji bazy danych filmów

Ponieważ naszym celem jest, aby zachować ich prostotę, utworzymy bardzo prostą aplikację bazy danych filmów. Nasze prostą aplikację bazy danych filmów pozwoli wykonać trzy czynności:

1. Zestaw rekordów bazy danych filmów
2. Utwórz nowy rekord bazy danych filmów
3. Edytowanie istniejącego rekordu bazy danych filmów

Ponownie ponieważ chcemy zachować ich prostotę, firma Microsoft będzie korzystać z minimalną liczbą funkcje potrzebne do tworzenia naszej aplikacji na platformę ASP.NET MVC. Na przykład firma Microsoft nie będą mogły skorzystać z Test-Driven rozwoju.

Aby można było utworzyć naszą aplikację, należy wykonać wszystkie kroki przedstawione poniżej:

1. Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC
2. Tworzenie bazy danych
3. Utwórz model bazy danych
4. Tworzenie kontrolera ASP.NET MVC
5. Tworzenie widoków ASP.NET MVC

## <a name="preliminaries"></a>Wymagania wstępne

Potrzebujesz programu Visual Studio 2008 lub Visual Web Developer 2008 Express, do tworzenia aplikacji ASP.NET MVC. Należy również pobrać platformę ASP.NET MVC.

Jeśli nie masz programu Visual Studio 2008, można pobrać 90-dniową wersję próbną programu Visual Studio 2008 z tej witryny sieci Web:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternatywnie można utworzyć platformy ASP.NET MVC aplikacji za pomocą programu Visual Web Developer Express 2008. Jeśli zdecydujesz się używać programu Visual Web Developer Express musi mieć w dodatku Service Pack 1. Visual Web Developer 2008 Express z dodatkiem Service Pack 1 można pobrać z tej witryny sieci Web:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Po zainstalowaniu programu Visual Studio 2008 lub Visual Web Developer 2008, należy zainstalować platformę ASP.NET MVC. Platforma ASP.NET MVC można pobrać z następującej witrynie sieci Web:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Zamiast pobierania indywidualnie środowiska ASP.NET framework i platformę ASP.NET MVC, możesz korzystać z zalet Instalatora platformy sieci Web. Instalator platformy sieci Web jest aplikacja, która umożliwia łatwe zarządzanie zainstalowanych aplikacji na komputerze:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Tworzenie projektu aplikacji sieci Web programu ASP.NET MVC

Zacznijmy od utworzenia nowego projektu aplikacji sieci Web programu ASP.NET MVC w programie Visual Studio 2008. Wybierz opcję menu **plik, nowy projekt** i pojawi się okno dialogowe Nowy projekt na rysunku 1. Wybierz język programowania Visual Basic, a następnie wybierz szablon projektu aplikacji sieci Web programu ASP.NET MVC. Nadaj projektowi nazwę MovieApp, a następnie kliknij przycisk OK.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Rysunek 01**: okno dialogowe Nowy projekt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


Upewnij się, że program .NET Framework 3.5 należy wybrać z listy rozwijanej w górnej części okna dialogowego Nowy projekt lub nie będą wyświetlane przez szablon projektu aplikacji sieci Web programu ASP.NET MVC.


Gdy utworzysz nowy projekt aplikacji sieci Web MVC, w Visual Studio zostanie wyświetlony monit o utworzenie projektu testu jednostkowego oddzielne. Zostanie wyświetlone okno dialogowe, na rysunku 2. Ponieważ firma Microsoft nie będzie można tworzenie testów w ramach tego samouczka z powodu ograniczeń czasowych (i tak powinien uważamy nieco winnych na ten temat) wybierz **nie** opcji, a następnie kliknij przycisk **OK** przycisku.

> [!NOTE] 
> 
> Visual Web Developer nie obsługuje projektów testów.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Rysunek 02**: projekt testu jednostkowego utworzyć okno dialogowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


Aplikacji ASP.NET MVC zawiera standardowy zestaw folderów: folder modeli, widoków i kontrolerów. Widać to standardowy zestaw folderów w oknie Eksploratora rozwiązań. Musimy dodać pliki do każdego z folderów modeli, widoków i kontrolerów do kompilowania aplikacji bazy danych filmów.

Podczas tworzenia nowej aplikacji MVC z programem Visual Studio, otrzymasz przykładową aplikację. Ponieważ chcemy zacząć od podstaw, należy usunąć zawartość dla tej aplikacji przykładowej. Musisz usunąć następującego pliku i folderu:

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Tworzenie bazy danych

Musimy utworzyć bazę danych do przechowywania nasz film rekordów bazy danych. Na szczęście program Visual Studio obejmuje bezpłatną bazę danych o nazwie SQL Server Express. Wykonaj następujące kroki, aby utworzyć bazę danych:

1. Kliknij prawym przyciskiem myszy aplikację\_folderu danych w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz **danych** kategorii, a następnie wybierz **bazy danych SQL Server** szablonu (zobacz rysunek 3).
3. Nadaj nazwę nowej bazy danych *MoviesDB.mdf* i kliknij przycisk **Dodaj** przycisku.

Po utworzeniu bazy danych, możesz się z bazą danych, klikając dwukrotnie plik MoviesDB.mdf znajduje się w aplikacji\_folderu danych. Dwukrotne kliknięcie pliku MoviesDB.mdf spowoduje otwarcie okna Eksploratora serwera.

> [!NOTE] 
> 
> Okno Eksploratora serwera nosi nazwę okna Eksplorator bazy danych w przypadku Visual Web Developer.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Rysunek 03**: tworzenie bazy danych programu Microsoft SQL Server ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


Następnie należy utworzyć nową tabelę bazy danych. Z poziomu okna Eksploratora serwera, kliknij prawym przyciskiem myszy folder Tabele i wybierz opcję menu **Dodaj nową tabelę**. Wybranie tej opcji menu zostanie otwarty projektant tabel bazy danych. Utwórz następujące kolumny bazy danych:

<a id="0.2_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Id | int | False |
| Tytuł | Nvarchar(100) | False |
| Dyrektor ds. | Nvarchar(100) | False |
| DateReleased | DataGodzina | False |


Pierwszej kolumny, a kolumna identyfikatora ma dwie właściwości specjalne. Najpierw należy oznaczyć jako kolumna klucza podstawowego kolumny identyfikatora. Po wybraniu kolumny identyfikatora, kliknij przycisk **Ustaw klucz podstawowy** przycisku (jest to ikona, który wygląda jak klucz). Po drugie, musisz oznaczyć jako kolumnę tożsamości kolumny identyfikatora. W oknie dialogowym właściwości kolumny przewiń w dół do sekcji Specyfikacja tożsamości i rozwiń go. Zmiana **tożsamości jest** właściwości na wartość **tak**. Gdy to zrobisz, tabela powinien wyglądać jak rysunek 4.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Rysunek 04**: tabela bazy danych filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


Ostatnim krokiem jest, aby zapisać nową tabelę. Kliknij przycisk Zapisz (ikona stacja dyskietek) i nadaj nowej tabeli filmy nazwy.

Po zakończeniu tworzenia tabeli należy dodać niektóre rekordy film do tabeli. Kliknij prawym przyciskiem myszy tabelę filmy, w oknie Eksploratora serwera, a następnie wybierz opcję menu **Pokaż dane tabeli**. Wprowadź listę filmów Ulubione (zobacz rysunek 5).


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Rysunek 05**: wprowadzanie filmu rekordów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>Tworzenie modelu

Następnie należy utworzyć zestaw klas do reprezentowania naszej bazie danych. Należy utworzyć model bazy danych. Firma Microsoft będzie korzystać z automatycznie Generuj klasy dla nasz model bazy danych w programie Microsoft Entity Framework.

> [!NOTE] 
> 
> Platforma ASP.NET MVC nie jest związany z Microsoft Entity Framework. Można utworzyć bazy danych klasy modeli, wykorzystując różne relacyjnego mapowania obiektów (lub / M) narzędzi takich jak LINQ to SQL, Subsonic i NHibernate.


Wykonaj następujące kroki, aby uruchomić Kreator modelu danych jednostki:

1. Kliknij prawym przyciskiem myszy folderu modeli w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz **danych** kategorii, a następnie wybierz **ADO.NET Entity Data Model** szablonu.
3. Nadaj nazwę modelu danych *MoviesDBModel.edmx* i kliknij przycisk **Dodaj** przycisku.

Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator modelu Entity Data Model (patrz rysunek 6). Wykonaj następujące kroki, aby zakończyć działanie kreatora:

1. W **wybierz zawartość modelu** kroku, wybierz pozycję **Generuj z bazy danych** opcji.
2. W **wybierz połączenie danych** kroku, należy użyć *MoviesDB.mdf* połączenia danych i nazwę *MoviesDBEntities* dla ustawień połączenia. Kliknij przycisk **dalej** przycisku.
3. W **wybierz obiekty bazy danych** kroku, rozwiń węzeł tabele, wybierz tabelę filmów. Wprowadź przestrzeń nazw *MovieApp.Models* i kliknij przycisk **Zakończ** przycisku.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Rysunek 06**: Generowanie modelu bazy danych za pomocą Kreator modelu Entity Data Model ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


Po zakończeniu działania Kreator modelu Entity Data Model, zostanie otwarty projektant modelu danych jednostki. Projektant powinien być wyświetlany w tabeli bazy danych filmów (zobacz rysunek 7).


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Rysunek 07**: projektancie Entity Data Model ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


Należy wprowadzić zmianę jeden, przed kontynuowaniem. Kreator modelu Entity Data generuje klasę modelu o nazwie filmy, który reprezentuje tabeli bazy danych filmów. Ponieważ będziemy korzystać do reprezentowania filmu konkretnej klasy filmy, należy zmodyfikować nazwę klasy, która ma być *filmu* zamiast *filmy* (pojedynczej zamiast liczba mnoga).

Kliknij dwukrotnie nazwę klasy na powierzchni projektowej i Zmień nazwę klasy z filmy filmu. Po wprowadzeniu tej zmiany, kliknij przycisk **Zapisz** przycisk (ikona dysku), aby wygenerować klasę filmu.

## <a name="creating-the-aspnet-mvc-controller"></a>Tworzenie kontrolera ASP.NET MVC

Następnym krokiem jest utworzenie kontrolera ASP.NET MVC. Kontroler jest odpowiedzialny za kontrolowanie sposobu interakcji użytkownika z aplikacją ASP.NET MVC.

Wykonaj następujące kroki:

1. W oknie Eksploratora rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz opcję menu **Dodaj, kontroler**.
2. W oknie dialogowym Dodaj kontroler, wprowadź nazwę *HomeController* i zaznacz pola wyboru **dodają metody akcji na potrzeby scenariuszy tworzenia, aktualizowania lub szczegóły** (zobacz rysunek 8).
3. Kliknij przycisk **Dodaj** przycisk, aby dodać nowy kontroler do projektu.

Po wykonaniu tych kroków, kontrolera w ofercie 1 jest tworzony. Należy zauważyć, że zawiera on metody o nazwie Index, uzyskać szczegółowe informacje, Utwórz i edytowania. W poniższych sekcjach dodamy niezbędny kod, aby uzyskać te metody do pracy.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Rysunek 08**: dodanie nowego kontrolera MVC platformy ASP.NET ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**Wyświetlanie listy 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Lista rekordów bazy danych

Metoda indeks() kontrolera głównego jest domyślną metodą dla aplikacji ASP.NET MVC. Po uruchomieniu aplikacji ASP.NET MVC, metoda indeks() jest pierwsza metoda kontrolera, która jest wywoływana.

Użyjemy metoda indeks(), aby wyświetlić listę rekordów z tabeli bazy danych filmów. Firma Microsoft będzie korzystać z bazy danych klasy modeli, które utworzony wcześniej w celu pobrania rekordów bazy danych filmów za pomocą metody indeks().

Czy mogę zostały zmodyfikowane klasy HomeController w ofercie 2 tak, aby zawierał nowy prywatnego pola o nazwie \_bazy danych. Klasa MoviesDBEntities reprezentuje nasz model bazy danych, a następnie użyjemy tej klasy do komunikowania się z naszym bazy danych.

Również są zmodyfikowaniu metoda indeks() w ofercie 2. Metoda indeks() używa klasy MoviesDBEntities w celu pobrania wszystkich rekordów film z tabeli bazy danych filmów. Wyrażenie  *\_bazy danych. MovieSet.ToList()* zwraca listę wszystkich rekordów film z tabeli bazy danych filmów.

Na liście filmów jest przekazywane do widoku. Wszystkie elementy, które są przekazywane do metody View() pobiera przekazywane do widoku danych widoku.

**Wyświetlanie listy 2 — Controllers/HomeController.vb (modyfikowana metoda indeksu)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Metoda indeks() zwraca widok o nazwie indeksu. Należy utworzyć ten widok, aby wyświetlić listę rekordów bazy danych filmów. Wykonaj następujące kroki:


Należy skompilować projekt (wybierz opcję menu **twórz, Kompiluj rozwiązanie**) przed otwarciem **Dodaj widok** okna dialogowego lub klasy, nie będą wyświetlane w **wyświetlić klasy danych** Lista rozwijana.


1. Kliknij prawym przyciskiem myszy metodę indeks() w edytorze kodu, a następnie wybierz opcję menu **Dodaj widok** (patrz rysunek 9).
2. W oknie dialogowym Dodaj widok, sprawdź, czy pole wyboru etykietą **utworzyć widok silnie typizowane** jest zaznaczone.
3. Z **wyświetlanie zawartości** listy rozwijanej wybierz wartość *listy*.
4. Z **wyświetlić klasy danych** listy rozwijanej wybierz wartość *MovieApp.Movie*.
5. Kliknij przycisk Dodaj, aby utworzyć nowy wyświetlić (zobacz rysunek 10).

Po wykonaniu tych kroków, nowy widok o nazwie Index.aspx zostanie dodany do folderu Views\Home. Zawartość widoku indeksu są uwzględnione w ofercie 3.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Rysunek 09**: Dodawanie widoku z akcji kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Na rysunku nr 10**: Tworzenie nowego widoku przy użyciu okna dialogowego Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

Widok indeksu Wyświetla wszystkie rekordy film z tabeli bazy danych filmów w tabeli HTML. Widok zawiera dla każdej pętli, który iteruje po każdego filmu, reprezentowane przez właściwość ViewData.Model. Po uruchomieniu aplikacji, naciskając klawisz F5, następnie zobaczysz stronę sieci web w rysunek 11.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Rysunek 11**: widoku indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>Tworzenie nowych rekordów bazy danych

Widok indeksu, które zostały utworzone w poprzedniej sekcji zawiera łącze do tworzenia nowych rekordów bazy danych. Rozpocznijmy i implementują logikę i tworzenie widoku niezbędne do tworzenia nowych rekordów bazy danych filmów.

Kontrolera głównego zawiera dwie metody o nazwie Create(). Pierwsza metoda Create() nie ma parametrów. Tego przeciążenia metody Create() służy do wyświetlania formularza HTML do tworzenia nowego rekordu bazy danych filmów.

Druga metoda Create() ma parametr FormCollection. Tego przeciążenia metody Create() jest wywoływana po opublikowaniu formularza HTML do tworzenia nowego filmu na serwerze. Należy zauważyć, że ta druga metoda Create() ma atrybut AcceptVerbs, który uniemożliwia metoda wywoływana, o ile nie jest wykonywana operacja Post protokołu HTTP.

Ta druga metoda Create() został zmodyfikowany w klasie HomeController zaktualizowane w ofercie 4. Nowa wersja metody Create() akceptuje parametr film i zawiera logikę wstawiania nowego filmu w tabeli bazy danych filmów.

> [!NOTE] 
> 
> Zwróć uwagę, atrybut do powiązania. Ponieważ nie chcemy zaktualizować właściwość Id film z formularza HTML, musimy jawnie wykluczone tej właściwości.


**Lista 4 – Controllers\HomeController.vb (modyfikowana metoda tworzenia)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Program Visual Studio ułatwia tworzenie formularza do tworzenia nowej bazy danych filmów rejestrowania (zobacz rysunek 12). Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy metodę Create() w edytorze kodu, a następnie wybierz opcję menu **Dodaj widok**.
2. Sprawdź, czy pole wyboru etykietą **utworzyć widok silnie typizowane** jest zaznaczone.
3. Z **wyświetlanie zawartości** listy rozwijanej wybierz wartość *Utwórz*.
4. Z **wyświetlić klasy danych** listy rozwijanej wybierz wartość *MovieApp.Movie*.
5. Kliknij przycisk **Dodaj** przycisk, aby utworzyć nowy widok.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Rysunek 12**: Dodawanie widoku Create ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Program Visual Studio automatycznie generuje widok w ofercie 5. Ten widok zawiera formularza HTML, który zawiera pola, które odpowiadają każdej z właściwości klasy filmu.

**Wyświetlanie listy 5 — Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Formularza HTML, generowane przez okno dialogowe dodawania widoku generuje identyfikator pola formularza. Ponieważ ta kolumna Id jest kolumną tożsamości, nie potrzebujemy tego pola formularza i można go bezpiecznie usunąć.


Po dodaniu Utwórz widok, można dodać nowe rekordy filmu w bazie danych. Uruchom aplikację, naciskając klawisz F5, a następnie kliknij przycisk Utwórz nowe łącze, aby wyświetlić formularz na rysunku 13. Jeśli zakończyć i Prześlij formularz, zostanie utworzony nowy rekord bazy danych filmów.

Zwróć uwagę, automatyczne pobieranie weryfikacji formularza. Jeśli udzielisz wprowadź datę wydania dla filmu lub wprowadź datę wydania nieprawidłowy, formularza zostanie wyświetlony ponownie, a pole daty wydania jest wyróżniona.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Rysunek 13**: Tworzenie nowego rekordu bazy danych filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>Edytowanie istniejących rekordów bazy danych

W poprzednich sekcjach omówiono, jak listy i tworzenia nowych rekordów bazy danych. W tej sekcji końcowej omówimy sposób edycji istniejących rekordów bazy danych.

Najpierw należy wygenerować formularz edycji. Ten krok jest proste, ponieważ program Visual Studio wygeneruje formularz edycji dla nas automatycznie. Otwórz klasę HomeController.vb w edytorze kodu programu Visual Studio, a następnie wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy metodę Edit() w edytorze kodu, a następnie wybierz opcję menu **Dodaj widok** (zobacz rysunek 14).
2. Zaznacz pola wyboru **utworzyć widok silnie typizowane**.
3. Z **wyświetlanie zawartości** listy rozwijanej wybierz wartość *Edytuj*.
4. Z **wyświetlić klasy danych** listy rozwijanej wybierz wartość *MovieApp.Movie*.
5. Kliknij przycisk **Dodaj** przycisk, aby utworzyć nowy widok.

Wykonanie tych kroków dodaje nowy widok o nazwie Edit.aspx w folderze Views\Home. Ten widok zawiera formularza HTML do edytowania rekordu filmu.


[![Okno dialogowe Nowy projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Rysunek 14**: Dodawanie widoku edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> Widok edycji zawiera pola formularza HTML, odpowiadającą Właściwość Id filmu. Ponieważ nie chcesz, aby osoby edycji wartości właściwości identyfikatora, należy usunąć tego pola formularza.


Na koniec należy zmodyfikować kontrolera głównego obsługuje edytowania rekordu bazy danych. Zaktualizowano klasy HomeController znajduje się w ofercie 6.

**Wyświetlanie listy 6 – Controllers\HomeController.vb (metod edycji)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

W ofercie 6 zostały dodane do oba przeciążenia metody Edit() dodatkowej logiki. Pierwsza metoda Edit() zwraca rekord bazy danych filmów, który odnosi się do parametru Id przekazany do metody. Drugie przeciążenie wykonuje aktualizacje nagrywanie filmu w bazie danych.

Należy zauważyć, że należy pobrać oryginalnego filmu, a następnie wywołaj ApplyPropertyChanges(), aby zaktualizować istniejące filmu w bazie danych.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było dać Ci przedsmak środowisko tworzenia aplikacji ASP.NET MVC. Mam nadzieję, że wykryte, że tworzenie aplikacji sieci web platformy ASP.NET MVC jest bardzo podobne do środowiska tworzenia aplikacji stron ASP lub ASP.NET.

W tym samouczku zbadaliśmy tylko podstawowe funkcje struktury ASP.NET MVC. W przyszłości samouczków firma szczegółowe zagadnienia, takie jak kontrolerów, akcji kontrolera, widoków, dane widoku i pomocników HTML.

> [!div class="step-by-step"]
> [Poprzednie](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
