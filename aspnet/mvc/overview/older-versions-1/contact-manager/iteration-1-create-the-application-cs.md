---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Iteracja #1 — Tworzenie aplikacji (C#) | Dokumentacja firmy Microsoft'
author: microsoft
description: 'W pierwszej iteracji utworzymy menedżera kontaktu w najprostszym sposobem możliwe. Możemy dodać obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i D...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 30f626511164363fea2195a05e73aeee5764933b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877388"
---
<a name="iteration-1--create-the-application-c"></a>Iteracja #1 — Tworzenie aplikacji (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> W pierwszej iteracji utworzymy menedżera kontaktu w najprostszym sposobem możliwe. Możemy dodać obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji ASP.NET MVC kontaktu administracyjnego (VB)

W tej serii samouczków budujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacji skontaktuj się z Menedżera umożliwia przechowywanie informacji kontaktowych - nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft kompilowania aplikacji za pośrednictwem przejść przez wiele iteracji. Przy każdej iteracji firma Microsoft stopniowego zwiększenia aplikacji. Celem tego podejścia wiele iteracji jest ułatwia zrozumienie przyczyn, dla każdej zmiany.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy menedżera kontaktu w najprostszym sposobem możliwe. Możemy dodać obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracji #2 — należy Szukaj nieuprzywilejowany aplikacji. W tym iteracji możemy ulepszyć wyglądu aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku programu ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzecim iteracji dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail i numerów telefonów.

- Iteracji #4 — należy luźno powiązane z aplikacji. W tym trzeci iteracji możemy korzystać z kilku wzorce projektowe oprogramowania do ułatwiają obsługiwanie i modyfikowanie aplikacji Menedżera skontaktuj się z pomocą. Na przykład firma Microsoft Refaktoryzuj naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 - tworzenia testów jednostkowych. W piątym iteracji możemy ułatwić naszej aplikacji obsługiwanie i modyfikowanie przez dodanie testów jednostkowych. Firma Microsoft mock naszej klasy modelu danych i tworzenie testów jednostkowych dla naszych kontrolerów i logiki sprawdzania poprawności.

- Iteracja #6 - użyj test-driven development. W tym szóstego iteracji dodania nowych funkcji do naszej aplikacji najpierw pisania testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 - dodawania funkcjonalności interfejsu Ajax. Siódmego iteracji możemy ulepszyć czas reakcji i wydajności aplikacji, dodając obsługę technologii Ajax.

## <a name="this-iteration"></a>Tej iteracji

W tym pierwszym iteracji budujemy aplikacji w warstwie podstawowa. Celem jest tworzenie menedżera kontaktu w sposób możliwie najszybszym i najprostszym. W późniejszym iteracji możemy ulepszyć projekt aplikacji.

Aplikacja menedżera kontaktu jest podstawowej aplikacji opartej na bazie danych. Aplikacja umożliwia tworzenie nowych kontaktów, edytować istniejące kontakty i usuwanie kontaktów.

W tym iteracji możemy wykonaj następujące czynności:

1. Aplikacji ASP.NET MVC
2. Utwórz bazę danych do przechowywania naszych kontaktów
3. Generowanie klasę modelu do naszej bazie danych Microsoft Entity Framework
4. Tworzenie akcji kontrolera i widoku, który pozwala na liście wszystkie kontakty w bazie danych
5. Tworzenie akcji kontrolera i widoku, który pozwala na utworzenie nowego kontaktu w bazie danych
6. Tworzenie akcji kontrolera i widoku, który pozwala na edytowanie istniejącego kontaktu w bazie danych
7. Tworzenie akcji kontrolera i widoku, który pozwala na usuwanie istniejącego kontaktu w bazie danych

## <a name="software-prerequisites"></a>Wstępnie wymagane oprogramowanie

W aplikacjach ASP.NET MVC musi mieć programu Visual Studio 2008 lub Visual Web Developer 2008 zainstalowane na komputerze (Visual Web Developer jest bezpłatna wersja programu Visual Studio, która nie zawiera wszystkich zaawansowanych funkcji programu Visual Studio). Można pobrać wersję próbną programu Visual Studio 2008 albo Visual Web Developer z następującego adresu:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Dla aplikacji ASP.NET MVC za pomocą programu Visual Web Developer muszą mieć Visual Web Developer dodatku Service Pack 1. Bez dodatku Service Pack 1 nie można utworzyć projekty aplikacji sieci Web.


Platforma ASP.NET MVC. Platforma ASP.NET MVC można pobrać z następującego adresu:

[https://www.asp.net/mvc](../../../index.md)

W tym samouczku używamy Microsoft Entity Framework dostępu do bazy danych. Entity Framework jest dołączony do programu .NET Framework 3.5 Service Pack 1. Ten dodatek service pack można pobrać z następującej lokalizacji:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Jako alternatywę do wykonywania poszczególnych te pliki do pobrania pojedynczo można korzystać z Instalatora platformy sieci Web (Web PI). Możesz pobrać Instalator Web PI z następującego adresu:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projektu programu ASP.NET MVC

Projekt aplikacji sieci Web platformy ASP.NET MVC. Uruchom program Visual Studio i wybierz opcję menu **plik, nowy projekt**. **Nowy projekt** zostanie wyświetlone okno dialogowe (zobacz rysunek 1). Wybierz **Web** typ projektu i **aplikacji sieci Web platformy ASP.NET MVC** szablonu. Nazwa nowego projektu *ContactManager* i kliknij przycisk OK.


Upewnij się, że program .NET Framework 3.5 wybrany z listy rozwijanej u góry po prawej **nowy projekt** okna dialogowego. W przeciwnym razie są wyświetlane won t szablonu aplikacji sieci Web platformy ASP.NET MVC.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Rysunek 01**: okno dialogowe Nowy projekt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image2.png))


Aplikacji ASP.NET MVC **Utwórz jednostkowy projekt testowy** zostanie wyświetlone okno dialogowe. To okno, aby wskazać, że chcesz utworzyć i dodać do rozwiązania jednostkowy projekt testowy, podczas tworzenia aplikacji ASP.NET MVC. Chociaż firma Microsoft kupione t tworzenia testów jednostkowych w tym iteracji, należy wybrać opcję **tak, Utwórz jednostkowy projekt testowy** ponieważ planujemy dodać testów jednostkowych w późniejszym iteracji. Dodawanie projektu testowego, podczas tworzenia nowego projektu platformy ASP.NET MVC jest znacznie prostsze niż dodawanie projektu testowego, po utworzeniu projektu programu ASP.NET MVC.

> [!NOTE] 
> 
> Ponieważ Visual Web Developer nie wspiera projekty testowe, nie pobrać okna dialogowego Utwórz jednostkowy projekt testowy, korzystając z programu Visual Web Developer.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Rysunek 02**: okna dialogowego Utwórz jednostkowy projekt testowy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image4.png))


Aplikacji ASP.NET MVC jest wyświetlany w oknie programu Visual Studio Solution Explorer (patrz rysunek 3). Jeśli ADAM t Zobacz okno Eksploratora rozwiązań, a następnie wybierając opcję menu można otworzyć tego okna **widok, w Eksploratorze rozwiązań**. Powiadomienie, że rozwiązania zawiera dwa projekty: projekt programu ASP.NET MVC i projektu testowego. Projektu programu ASP.NET MVC ma nazwę ContactManager i nazwie ContactManager.Tests projektu testowego.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Rysunek 03**: okna Eksploratora rozwiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Usuwanie przykładowych plików projektu

Szablon projektu programu ASP.NET MVC zawiera pliki przykładowe dla widoków i kontrolerów. Przed utworzeniem nowej aplikacji ASP.NET MVC, należy usunąć te pliki. Można usunąć pliki i foldery w oknie Eksploratora rozwiązań prawym przyciskiem myszy plik lub folder, a następnie wybierając opcję menu **usunąć**.

Należy usunąć następujące pliki z projektu programu ASP.NET MVC:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

I należy usunąć następujący plik z projektu testowego:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Tworzenie bazy danych

Aplikacja menedżera kontaktu jest aplikacji sieci web opartej na bazie danych. Używamy bazy danych do przechowywania informacji kontaktowych.

Platforma ASP.NET MVC z żadną bazą danych nowoczesnych, łącznie z bazy danych programu Microsoft SQL Server, Oracle, MySQL i IBM DB2. W tym samouczku używamy bazy danych programu Microsoft SQL Server. Po zainstalowaniu programu Visual Studio są podane z opcją instalacji programu Microsoft SQL Server Express, który jest bezpłatną wersję bazy danych programu Microsoft SQL Server.

Utwórz nową bazę danych, klikając prawym przyciskiem myszy aplikację\_folderu danych w oknie Eksploratora rozwiązań i wybierając opcję menu **Dodaj, nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **danych** kategorii i **bazy danych programu SQL Server** szablonu (patrz rysunek 4). Nazwa nowej bazy danych ContactManagerDB.mdf, a następnie kliknij przycisk OK.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Rysunek 04**: Tworzenie nowej bazy danych programu Microsoft SQL Server Express ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image8.png))


Po utworzeniu nowej bazy danych, bazy danych jest wyświetlany w aplikacji\_folderu danych w oknie Eksploratora rozwiązań. Kliknij dwukrotnie plik ContactManager.mdf, aby otworzyć okno Eksploratora serwera i połączenia z bazą danych.

> [!NOTE] 
> 
> Okno Eksploratora serwera jest nazywany okno Eksploratora bazy danych w przypadku programu Microsoft Visual Web Developer.


Okno Eksploratora serwera do tworzenia nowych obiektów bazy danych, takich jak tabele bazy danych, widoki, wyzwalacze i procedury składowane. Kliknij prawym przyciskiem myszy folder Tabele i wybierz opcję menu **Dodaj nową tabelę**. Pojawi się Projektant tabeli bazy danych (zobacz rysunek 5).


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Rysunek 05**: projektanta tabel bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image10.png))


Należy utworzyć tabelę, która zawiera następujące kolumny:

<a id="0.1_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Dopuszcza wartości null** |
| --- | --- | --- |
| Id | int | false |
| Imię | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Telefon | nvarchar(50) | false |
| Adres e-mail | nvarchar(255) | false |


Pierwsza kolumna, w kolumnie identyfikator to specjalne. Należy oznaczyć identyfikator kolumny jako kolumny tożsamości i kolumną klucza podstawowego. Wskazuje, czy kolumna jest kolumną tożsamości rozwijając kolumny właściwości (wyglądu w dolnej części rysunek 6) i przewijania w dół do właściwości specyfikacji tożsamości. Ustaw **(tożsamość jest)** na wartość **tak**.

Będzie oznaczenie kolumny jako kolumny klucza podstawowego, zaznaczając ją i klikając przycisk z ikoną klucza. Po kolumna jest oznaczona jako kolumna klucza podstawowego, obok kolumnie jest wyświetlana ikona klucza (patrz rysunek 6).

Po zakończeniu tworzenia tabeli, kliknij przycisk Zapisz (przycisk z ikoną dyskietki), aby zapisać nową tabelę. Nadaj nazwę nowej tabeli *kontaktów*.

Po Zakończ tworzenie kontaktów tabeli bazy danych należy dodać niektóre rekordy do tabeli. Kliknij prawym przyciskiem myszy tabelę Kontakty w oknie Eksploratora serwera i wybierz opcję menu **Pokaż dane tabeli**. Wprowadź jeden lub więcej kontaktów w siatce, która jest wyświetlana.

## <a name="creating-the-data-model"></a>Tworzenie modelu danych

Aplikacja ASP.NET MVC składa się z modele, widoki i kontrolery. Firma Microsoft Rozpocznij od utworzenia klasa modelu, która reprezentuje tabeli Kontakty utworzonej w poprzedniej sekcji.

W tym samouczku używamy programu Entity Framework firmy Microsoft można automatycznie wygenerować klasy modelu z bazy danych.

> [!NOTE] 
> 
> Platforma ASP.NET MVC nie jest powiązany Microsoft Entity Framework w dowolny sposób. ASP.NET MVC można użyć z tym NHibernate, składnika LINQ to SQL lub ADO.NET technologii dostępu do alternatywnej bazy danych.


Wykonaj następujące kroki, aby utworzyć klasy modelu danych:

1. Kliknij prawym przyciskiem myszy folder modeli w oknie Eksploratora rozwiązań i wybierz **Dodaj, nowy element**. **Dodaj nowy element** zostanie wyświetlone okno dialogowe (patrz rysunek 6).
2. Wybierz **danych** kategorii i **modelu danych jednostki ADO.NET** szablonu. Nazwa modelu danych *ContactManagerModel.edmx* i kliknij przycisk **Dodaj** przycisku. Pojawi się Kreator modelu Entity Data Model (patrz rysunek 7).
3. W **wybierz zawartość modelu** krok, wybierz opcję **generowania z bazy danych** (patrz rysunek 7).
4. W **wybierz połączenie danych** kroku, wybierz bazę danych ContactManagerDB.mdf i wprowadź nazwę *ContactManagerDBEntities* dla ustawień połączenia jednostki (patrz rysunek 8).
5. W **wybierz obiekty bazy danych użytkownika** krok, zaznacz pole wyboru tabel (zobacz rysunek 9). Model danych będzie zawierać wszystkie tabele zawarte w bazie danych (Brak co najmniej jeden, tabela kontaktów). Wprowadź przestrzeń nazw *modele*. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Rysunek 06**: okno dialogowe Dodawanie nowego elementu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image12.png))


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Rysunek 07**: Wybierz zawartość modelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image14.png))


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Rysunek 08**: Wybierz połączenie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image16.png))


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Rysunek 09**: Wybierz obiekty bazy danych użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image18.png))


Po zakończeniu pracy Kreatora modelu danych jednostki, pojawi się Projektant modelu danych jednostki. Projektant wyświetla klasy, która odpowiada do każdej tabeli modelowanego. Powinny pojawić się jedną klasę o nazwie kontaktów.

Kreator modelu Entity Data Model generuje nazwy klas na podstawie nazw tabeli bazy danych. Prawie zawsze należy zmienić nazwę klasy generowane przez kreatora. Kliknij prawym przyciskiem myszy klasę kontakty w Projektancie i wybierz opcję menu **zmienić**. Zmień nazwę klasy kontaktów (w liczbie mnogiej) do kontaktu (w liczbie pojedynczej). Po zmianie nazwy klasy, klasy powinny być wyświetlane, takich jak rysunek 10.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Na rysunku nr 10**: Skontaktuj się z klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image20.png))


W tym momencie utworzono nasz model bazy danych. Możemy użyć klasy skontaktuj się z pomocą do reprezentowania określonego rekordu kontaktu w naszej bazie danych.

## <a name="creating-the-home-controller"></a>Tworzenie kontrolera macierzystego

Następnym krokiem jest tworzenie kontrolera głównej. Kontroler głównej jest domyślne wywoływana w aplikacji platformy ASP.NET MVC.

Tworzenie klasy kontrolera głównej prawym przyciskiem myszy folder kontrolery, w oknie Eksploratora rozwiązań i wybierając opcję menu **Dodaj, kontrolera** (patrz rysunek 11). Zwróć uwagę, pole wyboru **Dodaj metody akcji dla scenariuszy tworzenia, aktualizacji i szczegółów**. Upewnij się, że to pole wyboru jest zaznaczone, przed kliknięciem przycisku **Dodaj** przycisku.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Rysunek 11**: Dodawanie kontrolera głównej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image22.png))


Podczas tworzenia kontrolera głównej otrzymasz klasy wyświetlania 1.

**Wyświetlanie listy 1 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Wyświetlanie listy kontaktów

Aby wyświetlić rekordy w tabeli bazy danych kontaktów, należy utworzyć akcję indeks() i widoku indeksu.

Kontroler główna zawiera już indeks() akcji. Potrzebujemy zmodyfikować tę metodę, tak aby wygląda jak lista 2.

**Wyświetlanie listy 2 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Powiadomienie, że klasy kontrolera głównej wyświetlania 2 zawiera prywatne pole o nazwie \_jednostek. \_Jednostek reprezentuje jednostek z modelu danych. Używamy \_jednostek pola do komunikowania się z bazą danych.

Metoda indeks() zwraca widoku, który reprezentuje wszystkie kontakty z tabeli bazy danych kontaktów. Wyrażenie \_jednostek. ContactSet.ToList() zwraca listy kontaktów w postaci listy ogólnej.

Teraz tego możemy kolejnych tworzenia indeksu kontrolera, następnie należy utworzyć widok indeksu. Przed utworzeniem widoku indeksu, kompilacja aplikacji przez wybranie opcji menu **kompilacji Kompiluj rozwiązanie**. Należy zawsze Skompiluj projekt przed dodaniem aby lista klas modelu mają być wyświetlane w widoku **Dodaj widok** okna dialogowego.

W przypadku utworzenia widoku indeksu prawym przyciskiem myszy metodę indeks() i wybierając opcję menu **Dodaj widok** (patrz rysunek 12). Wybranie tej opcji menu otwiera **Dodaj widok** okna dialogowego (patrz rysunek 13).


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Rysunek 12**: Dodawanie widok indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image24.png))


W **Dodaj widok** okno dialogowe, zaznacz pole wyboru z etykietą **utworzyć widok jednoznacznie**. Wybierz klasy danych widoku ContactManager.Models.Contact oraz Wyświetl listę zawartości. Wybranie opcji generuje widok, w którym zostanie wyświetlona lista rekordów kontaktów.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Rysunek 13**: okno dialogowe dodawania widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image26.png))


Po kliknięciu **Dodaj** przycisk, widok indeksu w 3 wyświetlania jest generowany. Powiadomienie &lt;% @ strony %&gt; dyrektywy wyświetloną w górnej części pliku. Widok indeksu dziedziczy ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; klasy. Innymi słowy klasa modelu w widoku reprezentuje listę, skontaktuj się z jednostek.

Treść widoku indeksu zawiera pętli foreach, który iteruje po każdego z kontaktów reprezentowany przez klasę modelu. Wartość każdej właściwości, skontaktuj się z klasy jest wyświetlany w tabeli HTML.

**Wyświetlanie listy 3 - Views\Home\Index.aspx (bez modyfikacji)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Musimy upewnić jedna modyfikacja widoku indeksu. Ponieważ firma Microsoft nie tworzenia widoku szczegółów, możemy Usuń łącze Szczegóły. Znajdź i usuń poniższy kod z widoku indeksu:

{Identyfikator elementu =. % Identyfikator})&gt;

Po zmodyfikowaniu widoku indeksu można uruchomić Menedżera skontaktuj się z aplikacji. Wybierz opcję menu debugowania, Rozpocznij debugowanie lub po prostu naciśnij klawisz F5. Podczas pierwszego uruchomienia aplikacji, Pobierz okna dialogowego na rysunku 14. Wybierz opcję **zmodyfikować plik Web.config w celu włączenia debugowania** i kliknij przycisk OK.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Rysunek 14**: Włączanie debugowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image28.png))


Widok indeksu jest zwracany przez domyślne. Ten widok zawiera listę wszystkich danych z tabeli bazy danych kontaktów (patrz rysunek 15).


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Rysunek 15**: widok Index ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image30.png))


Zwróć uwagę, że widok indeksu zawiera łącze oznaczone etykietą Utwórz nowy, u dołu widoku. W następnej sekcji omówiono tworzenie nowych kontaktów.

## <a name="creating-new-contacts"></a>Tworzenie nowych kontaktów

Aby umożliwić użytkownikom tworzenie nowych kontaktów, należy dodać dwie akcje Create() do kontrolera głównej. Należy utworzyć jedną akcję Create(), która zwraca formularza HTML służący do tworzenia nowego kontaktu. Należy utworzyć drugą akcję Create(), który wykonuje wstawienie rzeczywistej bazy danych w jego miejsce nowego kontaktu.

Nowych metod Create(), które należy dodać do kontrolera głównej znajdują się w listę 4.

**Wyświetlanie listy 4 - Controllers\HomeController.cs (za pomocą metody Create)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

Pierwsza metoda Create() może być wywoływana ze HTTP GET, podczas gdy druga metoda Create() może być wywoływana tylko przez akcję POST protokołu HTTP. Innymi słowy druga metoda Create() można wywołać tylko wtedy, gdy przesyłanie formularza HTML. Pierwsza metoda Create() po prostu zwraca widok zawierający formularz HTML do tworzenia nowego kontaktu. Druga metoda Create() jest bardziej interesujące: dodaje nowy kontakt z bazą danych.

Zwróć uwagę, że druga metoda Create() została zmodyfikowana do akceptowania wystąpienia klasy skontaktuj się z pomocą. Wartości formularza zaksięgowany z formularza HTML są powiązane z tej klasy kontaktu przez platformę ASP.NET MVC automatycznie. Każde pole formularza z formularza HTML utworzyć jest przypisany do właściwości parametru kontaktu.

Należy zauważyć, że parametr skontaktuj się z zostanie nadany atrybut [Bind]. Atrybut [Bind] służy do wykluczenia właściwość identyfikatora kontaktu z powiązania. Ponieważ właściwość identyfikatora reprezentuje właściwość tożsamości, możemy ADAM t chcesz ustawić właściwość identyfikator.

W treści metody Create() programu Entity Framework służy do wstawiania jego miejsce nowego kontaktu w bazie danych. Nowy kontakt jest dodawane do istniejącego zestawu kontaktów i wywoływana jest metoda SaveChanges() usunięcia tych zmian do podstawowej bazy danych.

Możesz wygenerować formularza HTML do tworzenia nowych kontaktów w jednej z dwóch metod Create() prawym przyciskiem myszy i wybierając opcję menu **Dodaj widok** (patrz rysunek 16).


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Rysunek 16**: Dodawanie widoku Create ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image32.png))


W **Dodaj widok** okno dialogowe, wybierz opcję **ContactManager.Models.Contact** klasy i **Utwórz** opcję Wyświetl zawartość (patrz rysunek 17). Po kliknięciu **Dodaj** przycisk Utwórz, Wyświetl jest generowany automatycznie.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Rysunek 17**: strona, rozwiń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image34.png))


Utwórz widok zawiera pola formularza, dla każdej właściwości klasy, skontaktuj się z pomocą. Kod dla widoku Utwórz znajduje się w 5 wyświetlania.

**Wyświetlanie listy 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Po zmodyfikować metody Create() i Dodaj Utwórz widok, można uruchomić Menedżera skontaktuj się z aplikacji i tworzenie nowych kontaktów. Kliknij przycisk **Utwórz nowy** łącze wyświetlane w widoku indeksu można przejść do widoku Utwórz. Widok na rysunku 18 powinna zostać wyświetlona.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Rysunek 18**: Tworzenie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>Edytowanie kontaktów

Dodawanie funkcji do edycji rekordu kontaktu jest bardzo podobne do dodawania funkcjonalności do tworzenia nowych rekordów kontaktu. Najpierw należy dodać dwie nowe metody edycji do klasy kontrolera głównej. Te nowe metody Edit() znajdują się w 6 wyświetlania.

**Wyświetlanie listy 6 - Controllers\HomeController.cs (za pomocą metod Edycja)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

Pierwsza metoda Edit() jest wywoływana przez operację GET protokołu HTTP. Parametr Id jest przekazywany do tej metody, która reprezentuje identyfikator rekordu kontaktu edytowany. Entity Framework jest używana do pobrania kontaktu, który odpowiada identyfikator. Zwracany jest widok zawierający formularza HTML do edytowania rekordu.

Druga metoda Edit() wykonuje rzeczywistej aktualizacji do bazy danych. Ta metoda przyjmuje wystąpienia klasy skontaktuj się z jako parametr. Platforma ASP.NET MVC wiąże pola formularza z formularza edycji tej klasy automatycznie. Powiadomienie, że zrobisz t include — atrybut [Bind] podczas edytowania kontaktu (potrzebujemy wartość właściwości Id).

Entity Framework jest używany do zapisać zmodyfikowanych kontaktu w bazie danych. Kontakcie musi najpierw pobrana z bazy danych. Następnie Entity Framework ApplyPropertyChanges() wywoływana jest metoda rejestrowania zmian do kontaktu. Ostatecznie wywoływana jest metoda Entity Framework SaveChanges(), aby utrwalić zmiany do podstawowej bazy danych.

Możesz wygenerować widok zawierający formularz edycji prawym przyciskiem myszy metodę Edit() i wybierając opcję menu Dodaj widok. W oknie dialogowym Dodaj widok, wybierz **ContactManager.Models.Contact** klasy i **Edytuj** wyświetlania zawartości (zobacz rysunek 19).


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Rysunek 19**: Dodawanie, edytowanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image38.png))


Po kliknięciu przycisku Dodaj nowy widok edycji jest generowany automatycznie. Formularza HTML, który jest generowany zawiera pola, które odpowiadają każdej z właściwości klasy kontaktu (patrz lista 7).

**Wyświetlanie listy 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Usuwanie kontaktów

Jeśli chcesz usunąć kontaktów, a następnie należy dodać do klasy kontrolera głównej dwie akcje Delete(). Pierwszą akcją Delete() Wyświetla formularz potwierdzenia usunięcia. Drugiej akcji Delete() wykonuje rzeczywiste delete.

> [!NOTE] 
> 
> Później w iteracji #7, możemy zmodyfikuj Contact Manager tak, aby obsługuje jeden krok, Usuń Ajax.


Dwóch nowych metod Delete() znajdują się w wyświetlania 8.

**Wyświetlanie listy 8 - Controllers\HomeController.cs (metody Delete)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

Pierwsza metoda Delete() zwraca formularza Potwierdzenie usuwania rekordu kontaktu z bazy danych (zobacz Figure20). Druga metoda Delete() wykonuje operację usuwania rzeczywiste w bazie danych. Po kontakcie ma zostały pobrane z bazy danych, Entity Framework DeleteObject() i SaveChanges() metody są wywoływane w celu wykonania usuwania bazy danych.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Rysunek 20**: Wyświetl potwierdzenie usuwania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image40.png))


Należy zmodyfikować widok indeksu, tak aby zawiera łącze do usuwania rekordów kontaktów (patrz rysunek 21). Należy dodać następujący kod do tej samej tabeli zawierającej łącze edycji:

Html.ActionLink( { id=item.Id }) %&gt;


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Rysunek 21**: Indeksuj zawartości widoku z łącza edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image42.png))


Następnie należy utworzyć widok potwierdzenia usunięcia. Kliknij prawym przyciskiem myszy w klasie kontrolera głównej metody Delete() i wybierz opcję menu Dodaj widok. Wyświetli się okno dialogowe dodawania widoku (patrz rysunek 22).

W odróżnieniu od w przypadku widoków listy, tworzenie i edytowanie, okno dialogowe dodawania widoku nie zawiera opcję, aby utworzyć widok Delete. Zamiast tego należy wybrać **ContactManager.Models.Contact** klasy danych i **pusty** wyświetlanie zawartości. Wybieranie pusty widok, który wymaga firmie Microsoft w celu utworzenia widoku nad opcji zawartość.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Rysunek 22**: Dodawanie widoku Potwierdzenie usuwania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image44.png))


Zawartość widoku Delete znajduje się w wyświetlania 9. Ten widok zawiera formularz, który potwierdza, czy nie powinien być określonego kontaktu usunięte (patrz rysunek 21).

**Wyświetlanie listy 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Zmiana nazwy domyślnego kontrolera

Może być odblokowane możesz, że nazwa klasy Nasze kontrolera do pracy z kontaktów nosi nazwę klasy HomeController. T nie powinien kontrolera nosić ContactController?

Ten problem jest dość proste rozwiązać problem. Najpierw musimy Refaktoryzuj nazwę kontrolera głównej. Otwórz klasy HomeController w edytorze kodu programu Visual Studio, kliknij prawym przyciskiem myszy nazwę klasy i wybierz opcję menu **Refaktoryzuj, Zmień nazwę**. Wybranie tej opcji menu otwiera okno dialogowe zmiany nazwy.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Rysunek 23**: refaktoryzacji nazwę kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image46.png))


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Rysunek 24**: za pomocą okna dialogowego zmiany nazwy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image48.png))


W przypadku zmiany nazwy klasy kontrolera, Visual Studio spowoduje zaktualizowanie nazwę folderu, w tym folderze widoków. Visual Studio spowoduje zmianę nazwy folderu \Views\Home do folderu \Views\Contact.

Po wprowadzeniu tej zmiany aplikacji przestaną być kontrolera głównej. Po uruchomieniu aplikacji zostanie wyświetlony strony błędu w 25 rysunku.


[![Okno dialogowe nowego projektu](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Rysunek 25**: nie domyślnego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-cs/_static/image50.png))


Należy zaktualizować trasy domyślnej w pliku Global.asax do użycia w kontrolerze głównej skontaktuj się z kontrolerem. Otwórz plik Global.asax i zmodyfikować na kontrolerze domyślne używane przez trasa domyślna (zobacz listę 10).

**Wyświetlanie listy 10 — Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Po wprowadzeniu tych zmian, skontaktuj się z Menedżera będzie działać poprawnie. Teraz skontaktuj się z klasy controller zostanie użyty jako domyślny kontroler.

## <a name="summary"></a>Podsumowanie

W tym pierwszym iteracji utworzone podstawowej aplikacji kontaktów Menedżerze najszybszy sposób możliwe. Wybraliśmy korzystać z programu Visual Studio automatycznie wygenerowany kod początkowej naszych kontrolery i widoki. Wybraliśmy również korzystać z programu Entity Framework, można automatycznie wygenerować naszej klasy modelu bazy danych.

Obecnie możemy listy, tworzenia, edytowania i usuwania rekordów kontaktów z aplikacji Menedżera skontaktuj się z. Innymi słowy możemy wykonywać wszystkie operacje bazy danych podstawowa wymagane przez aplikację sieci web opartej na bazie danych.

Niestety naszej aplikacji ma pewne problemy. Pierwszy oraz się zezwolić na to, Menedżera skontaktuj się z aplikacji nie jest najbardziej atrakcyjnych aplikacji. Wymaga dodatkowych czynności projektu. W następnej iteracji wyjaśniono, jak firma Microsoft i można zmieniać domyślnej strony wzorcowej widoku kaskadowy arkusz stylów, aby poprawić wygląd aplikacji.

Po drugie nie wdrożyliśmy wszystkie walidacji formularza. Na przykład nie ma nic aby uniemożliwić przesyłanie Utwórz formularz kontaktów bez podawania wartości dla każdego pola formularza. Ponadto można wprowadzić nieprawidłowy numer telefonu liczb i adresami poczty e-mail. Rozpoczniemy się problemem walidacji formularza w iteracji #3.

Koniec i najważniejsze bieżącą iterację aplikacji Menedżera skontaktuj się z nie może być łatwo zmodyfikowane lub obsługiwane. Na przykład logika dostępu do bazy danych jest rozszerzania prawo do akcji kontrolera. Oznacza to, że firma Microsoft nie można zmodyfikować naszego kodu dostępu do danych, bez konieczności modyfikacji naszych kontrolerów. W późniejszym iteracji możemy Poznaj wzorce projektowe oprogramowania, które możemy wdrożyć dokonanie Menedżera skontaktuj się z bardziej odporne na zmiany.

> [!div class="step-by-step"]
> [Next](iteration-2-make-the-application-look-nice-cs.md)
