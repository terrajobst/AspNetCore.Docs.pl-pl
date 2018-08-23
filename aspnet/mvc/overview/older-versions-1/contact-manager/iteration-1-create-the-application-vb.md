---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Iteracja #1 — Tworzenie aplikacji (VB) | Dokumentacja firmy Microsoft'
author: microsoft
description: 'W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 9369f843719d7198716ff83c5bbd5d3995f70973
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754685"
---
<a name="iteration-1--create-the-application-vb"></a>Iteracja #1 — Tworzenie aplikacji (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz program Code](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji zarządzania kontaktami platformy ASP.NET MVC (VB)

W tej serii samouczków wbudowujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft tworzy aplikację za pośrednictwem wiele iteracji. Z każdą iteracją można stopniowo ulepszyć aplikację. Celem tego wielu podejścia iteracji jest, aby umożliwić Ci zrozumienie przyczyn wprowadzenia poszczególnych zmian.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracja 2 # — należy wyglądu nieuprzywilejowany aplikacji. W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie weryfikacji formularza. W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.

- Iteracja 4 # — należy luźne sprzężenie aplikacji. W tym czwarty iteracji możemy korzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.

- Iteracja #6 — korzystanie z projektowania opartego na testach. W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 — dodawanie funkcji Ajax. W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Tej iteracji

W tym pierwszą iteracją firma Microsoft tworzy podstawową aplikację. Celem jest dołączanie Contact Manager to najszybszy i najprostszy sposób. W późniejszej iteracji można ulepszyć projekt aplikacji.

Aplikacja Contact Manager to podstawowa aplikacja opartego na bazie danych. Za pomocą aplikacji do tworzenia nowych kontaktów, edytować istniejące kontakty i usuwanie kontaktów.

W tym iteracji firma Microsoft wykonaj następujące czynności:

1. Aplikacja platformy ASP.NET MVC
2. Utwórz bazę danych do przechowywania naszych kontaktów
3. Generuj klasę modelu dla naszej bazie danych za pomocą programu Microsoft Entity Framework
4. Tworzenie akcji kontrolera i widoku, która umożliwia nam wyświetlić listę wszystkich kontaktów w bazie danych
5. Tworzenie akcji kontrolera i widoku, który pozwala utworzyć nowy kontakt w bazie danych
6. Tworzenie akcji kontrolera i widoku, który pozwala na edytowanie istniejącego kontaktu w bazie danych
7. Tworzenie akcji kontrolera i widoku, który pozwala na usuwanie istniejącego kontaktu w bazie danych

## <a name="software-prerequisites"></a>Wstępnie wymagane oprogramowanie

W aplikacjach ASP.NET MVC musisz mieć program Visual Studio 2008 lub Visual Web Developer 2008 zainstalowane na komputerze (Visual Web Developer jest bezpłatną wersję programu Visual Studio, który nie zawiera wszystkich zaawansowanych funkcji programu Visual Studio). Możesz pobrać wersję próbną programu Visual Studio 2008 albo Visual Web Developer z następującego adresu:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Dla aplikacji ASP.NET MVC za pomocą programu Visual Web Developer konieczne jest posiadanie programu Visual Web Developer dodatku Service Pack 1. Bez dodatku Service Pack 1 nie można utworzyć projektów aplikacji sieci Web.


Platforma ASP.NET MVC. Platforma ASP.NET MVC można pobrać z następującego adresu:

[https://www.asp.net/mvc](../../../index.md)

W tym samouczku używamy Microsoft Entity Framework dostępu do bazy danych. Entity Framework jest dołączana do programu .NET Framework 3.5 Service Pack 1. Tego dodatku service pack można pobrać z następującej lokalizacji:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Jako alternatywę do wykonywania poszczególnych te pliki do pobrania pojedynczo, możesz korzystać z zalet Instalatora platformy sieci Web (Web PI). Możesz pobrać Instalatora platformy sieci Web z następującego adresu:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projekt składnika ASP.NET MVC

Projekt aplikacji sieci Web platformy ASP.NET MVC. Uruchom program Visual Studio i wybierz opcję menu **plik, nowy projekt**. **Nowy projekt** zostanie wyświetlone okno dialogowe (patrz rysunek 1). Wybierz **Web** typ projektu i **aplikacji sieci Web programu ASP.NET MVC** szablonu. Nazwa nowego projektu *ContactManager* i kliknij przycisk OK.


Upewnij się, że program .NET Framework 3.5 wybrany z listy rozwijanej u góry po prawej stronie **nowy projekt** okna dialogowego. W przeciwnym razie są wyświetlane wygrał t szablonu aplikacji sieci Web programu ASP.NET MVC.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Rysunek 01**: okno dialogowe Nowy projekt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image2.png))


Aplikacja platformy ASP.NET MVC **Tworzenie projektu testu jednostkowego** zostanie wyświetlone okno dialogowe. Można użyć tego okna dialogowego, aby wskazać, że chcesz utworzyć i dodać do rozwiązania projekt testu jednostkowego, podczas tworzenia aplikacji ASP.NET MVC. Chociaż zdobyliśmy t tworzenia testów jednostkowych w tej iteracji, należy wybrać opcję **tak, Utwórz projekt testu jednostkowego** , ponieważ planujemy Dodawanie testów jednostkowych w późniejszej iteracji. Dodawanie projektu testowego, podczas tworzenia nowego projektu ASP.NET MVC jest znacznie prostsze niż dodawanie projektu testowego, po utworzeniu projektu ASP.NET MVC.

> [!NOTE] 
> 
> Ponieważ Visual Web Developer nie obsługuje projektów testowych, nie uzyskasz okna dialogowego Utwórz projekt testów jednostkowych podczas korzystania z programu Visual Web Developer.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Rysunek 02**: projekt testu jednostkowego utworzyć okno dialogowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image4.png))


Aplikacja platformy ASP.NET MVC pojawi się w oknie Eksploratora rozwiązań w usłudze Visual Studio (zobacz rysunek 3). Jeśli don t znajdują się w oknie Eksploratora rozwiązań, a następnie można otworzyć tego okna, wybierając opcję menu **widok, w Eksploratorze rozwiązań**. Zwróć uwagę, że rozwiązanie zawiera dwa projekty: projekt testu i projekt składnika ASP.NET MVC. Projekt składnika ASP.NET MVC ma nazwę ContactManager i projekt testu o nazwie ContactManager.Tests.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Rysunek 03**: okna Eksploratora rozwiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Usuwanie przykładowych plików projektu

Szablon projektu platformy ASP.NET MVC zawiera pliki przykładowe dla widoków i kontrolerów. Przed utworzeniem nowej aplikacji platformy ASP.NET MVC, należy usunąć te pliki. Możesz usunąć pliki i foldery w oknie Eksploratora rozwiązań kliknij prawym przyciskiem myszy plik lub folder, a następnie wybierając opcję menu **Usuń**.

Należy usunąć następujące pliki z projektu platformy ASP.NET MVC:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Ponadto należy usunąć następujący plik z projektu testów:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Tworzenie bazy danych

Aplikacja Contact Manager to aplikacja sieci web opartej na bazie danych. Używamy bazę danych do przechowywania informacji kontaktowych.

Platforma ASP.NET MVC z żadną bazą danych nowoczesnych, łącznie z bazy danych programu Microsoft SQL Server, Oracle, MySQL i IBM DB2. W tym samouczku używamy bazę danych programu Microsoft SQL Server. Po zainstalowaniu programu Visual Studio, zostaną udostępnione zainstalować program Microsoft SQL Server Express, która jest bezpłatną wersję bazy danych programu Microsoft SQL Server.

Utwórz nową bazę danych, klikając prawym przyciskiem myszy aplikację\_folderu danych w oknie Eksploratora rozwiązań i wybierając opcję menu **Dodaj, nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **danych** kategorii i **bazy danych SQL Server** szablonu (zobacz rysunek 4). Nadaj nazwę nowej bazy danych ContactManagerDB.mdf i kliknij przycisk OK.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Rysunek 04**: Tworzenie nowej bazy danych Microsoft SQL Server Express ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image8.png))


Po utworzeniu nowej bazy danych, baza danych jest wyświetlana w aplikacji\_folderu danych w oknie Eksploratora rozwiązań. Kliknij dwukrotnie plik ContactManager.mdf, aby otworzyć okno Eksploratora serwera i łączenia z bazą danych.

> [!NOTE] 
> 
> Okno Eksploratora serwera nosi nazwę okna Eksplorator bazy danych w przypadku programu Microsoft Visual Web Developer.


Okno Eksploratora serwera do tworzenia nowych obiektów bazy danych, takich jak tabele bazy danych, widoki, wyzwalacze i procedury składowane. Kliknij prawym przyciskiem myszy folder Tabele i wybierz opcję menu **Dodaj nową tabelę**. Pojawi się Projektant tabeli bazy danych (zobacz rysunek 5).


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Rysunek 05**: projektanta tabel bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image10.png))


Należy utworzyć tabelę, która zawiera następujące kolumny:

<a id="0.2_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Id | int | false |
| Imię | nvarchar(50) | false |
| Nazwisko | nvarchar(50) | false |
| Telefon | nvarchar(50) | false |
| Adres e-mail | nvarchar(255) | false |


Pierwsza kolumna, kolumna identyfikatora to specjalne. Należy oznaczyć kolumna identyfikatora kolumny tożsamości oraz kolumnę klucza podstawowego. Wskazuje, że kolumna jest kolumną tożsamości, rozwijając kolumnę właściwości (odszukaj pozycję w dolnej części rysunek 6) i przewijając w dół do właściwości Specyfikacja tożsamości. Ustaw **(tożsamość jest)** właściwości na wartość **tak**.

Możesz oznaczyć kolumny jako kolumny klucza podstawowego, zaznaczając ją i klikając przycisk z ikoną klucza. Po kolumna została oznaczona jako kolumna klucza podstawowego, ikona klucza pojawia się obok kolumny (patrz rysunek 6).

Po zakończeniu tworzenia tabeli, kliknij przycisk Zapisz (przycisk z ikoną dyskietki), aby zapisać nową tabelę. Nadaj nazwę nowej tabeli *kontakty*.

Po Zakończ tworzenie kontaktów tabeli bazy danych należy dodać niektóre rekordy w tabeli. Kliknij prawym przyciskiem myszy tabeli kontaktów w oknie Eksploratora serwera, a następnie wybierz opcję menu **Pokaż dane tabeli**. Wprowadź jeden lub więcej kontaktów w siatce, która jest wyświetlana.

## <a name="creating-the-data-model"></a>Tworzenie modelu danych

Aplikacja platformy ASP.NET MVC składa się z modeli, widoków i kontrolerów. Zaczniemy od utworzenia klasy modelu, który reprezentuje tabeli kontaktów, które zostały utworzone w poprzedniej sekcji.

W tym samouczku używamy Microsoft Entity Framework do automatycznego wygenerowania klasy modelu z bazy danych.

> [!NOTE] 
> 
> Platforma ASP.NET MVC nie jest związany z Microsoft Entity Framework w dowolny sposób. Za pomocą platformy ASP.NET MVC technologii dostępu do alternatywnej bazy danych, w tym NHibernate, LINQ to SQL i ADO.NET.


Wykonaj następujące kroki, aby tworzenie klas modelu danych:

1. Kliknij prawym przyciskiem myszy folder modeli, w oknie Eksploratora rozwiązań, a następnie wybierz pozycję **Dodaj, nowy element**. **Dodaj nowy element** zostanie wyświetlone okno dialogowe (patrz rysunek 6).
2. Wybierz **danych** kategorii i **ADO.NET Entity Data Model** szablonu. Nazwa modelu danych *ContactManagerModel.edmx* i kliknij przycisk **Dodaj** przycisku. Pojawi się Kreator modelu Entity Data Model (zobacz rysunek 7).
3. W **wybierz zawartość modelu** kroku, wybierz pozycję **Generuj z bazy danych** (zobacz rysunek 7).
4. W **wybierz połączenie danych** krok, wybierz bazę danych ContactManagerDB.mdf i wprowadź nazwę *ContactManagerDBEntities* dla ustawień połączenia jednostki (zobacz rysunek 8).
5. W **wybierz obiekty bazy danych** krok, zaznacz pole wyboru tabel (patrz rysunek 9). Model danych będzie zawierać wszystkich tabel znajdujących się w bazie danych (istnieje tylko jeden, tabeli kontaktów). Wprowadź przestrzeń nazw *modeli*. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Rysunek 06**: okno dialogowe Dodaj nowy element ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image12.png))


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Rysunek 07**: Wybierz zawartość modelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image14.png))


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Rysunek 08**: Wybierz połączenie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image16.png))


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Rysunek 09**: Wybierz obiekty bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image18.png))


Po zakończeniu działania Kreator modelu Entity Data Model, zostanie wyświetlony Projektant modelu danych jednostki. Projektant Wyświetla klasę, która odnosi się do każdej tabeli są modelowane. Powinien zostać wyświetlony jedną klasę o nazwie kontaktów.

Kreator modelu Entity Data Model generuje nazwy klas na podstawie nazw tabel bazy danych. Prawie zawsze należy zmienić nazwę klasy generowanej przez kreatora. Kliknij prawym przyciskiem myszy klasy kontaktów w projektancie, a następnie wybierz opcję menu **Zmień nazwę**. Zmień nazwę klasy kontaktów (liczba mnoga) do kontaktu (w liczbie pojedynczej). Po zmianie nazwy klasy, klasy powinna pojawić się, jak na rysunku nr 10.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Na rysunku nr 10**: klasa Contact ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image20.png))


W tym momencie utworzyliśmy nasz model bazy danych. Możemy użyć klasy kontakt do reprezentowania określonego rekordu kontaktu w naszej bazie danych.

## <a name="creating-the-home-controller"></a>Tworzenie kontrolera głównego

Następnym krokiem jest do utworzenia naszej kontrolera głównego. Kontroler głównej jest domyślna wywołany w aplikacji ASP.NET MVC.

Utwórz klasę kontrolera głównej prawym przyciskiem myszy folder kontrolerów w oknie Eksploratora rozwiązań i wybierając opcję menu **Dodaj, kontroler** (zobacz rysunek 11). Zwróć uwagę, pola wyboru **dodają metody akcji na potrzeby scenariuszy tworzenia, aktualizowania lub szczegóły**. Upewnij się, że to pole wyboru jest zaznaczone przed kliknięciem przycisku **Dodaj** przycisku.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Rysunek 11**: dodanie kontrolera głównego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image22.png))


Podczas tworzenia kontrolera głównego uzyskujesz klasę w ofercie 1.

**Wyświetlanie listy 1 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Wyświetlanie listy kontaktów

Aby wyświetlić rekordy w tabeli bazy danych kontaktów, musimy utworzyć akcję indeks() i widoku indeksu.

Kontrolera głównego zawiera już akcję indeks(). Należy zmodyfikować tę metodę, tak, aby wygląda jak lista 2.

**Wyświetlanie listy 2 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Zwróć uwagę, że klasa kontrolera głównej w ofercie 2 zawiera prywatny pola o nazwie \_jednostek. \_Pola jednostki reprezentuje jednostek z modelu danych. Używamy \_pola jednostek, do komunikowania się z bazą danych.

Metoda indeks() zwraca widok, który reprezentuje wszystkie kontakty z tabeli bazy danych kontaktów. Wyrażenie \_jednostek. ContactSet.ToList() zwraca listę kontaktów w postaci listy ogólnej.

Obecnie tego wykonujemy ve tworzenia kontrolera indeksu, następnie należy utworzyć widok indeksu. Przed utworzeniem widoku indeksu, skompiluj aplikację, wybierając opcję menu **twórz, Kompiluj rozwiązanie**. Należy zawsze Skompiluj projekt przed dodaniem widoku w kolejności, aby uzyskać listę klas modelu mają być wyświetlane w **Dodaj widok** okna dialogowego.

Tworzenie widoku indeksu, klikając prawym przyciskiem myszy metodę indeks() i wybierając opcję menu **Dodaj widok** (zobacz rysunek 12). Wybranie tej opcji menu otwiera **Dodaj widok** okna dialogowego (zobacz rysunek 13).


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Rysunek 12**: Dodawanie widok indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image24.png))


W **Dodaj widok** okno dialogowe, zaznacz pola wyboru **utworzyć widok silnie typizowane**. Wybierz widok danych klasy ContactManager.Contact i wyświetlanie listy zawartości. Zaznaczenie tych opcji generuje widok, który wyświetla listę rekordów kontaktów.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Rysunek 13**: okno dialogowe Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image26.png))


Po kliknięciu **Dodaj** przycisk, widoku indeksu w ofercie 3 jest generowany. Zwróć uwagę &lt;% @ % strony&gt; dyrektywę, który pojawia się u góry pliku. Widok indeksu dziedziczy ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; klasy. Innymi słowy klasa modelu w widoku reprezentuje listę jednostkami kontaktowymi.

Treść widoku indeksu zawiera pętlę foreach iteruje przez każdy z kontaktów, reprezentowane przez klasę modelu. Wartość każdej właściwości klasy, skontaktuj się z pomocą jest wyświetlany w tabeli HTML.

**Wyświetlanie listy 3 - Views\Home\Index.aspx (bez modyfikacji)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Musimy wprowadzić modyfikacji jednego widoku indeksu. Ponieważ firma Microsoft są nietworzenie widoku szczegółów, możemy usunąć łącze szczegółowe informacje. Znajdź i usuń następujący kod z widoku indeksu:

{.id = element. % Identyfikator})&gt;

Po zmodyfikowaniu widoku indeksu można uruchomić aplikacji Contact Manager. Wybierz opcję menu debugowania, Rozpocznij debugowanie lub po prostu naciśnij klawisz F5. Podczas pierwszego uruchomienia aplikacji, uzyskasz okna dialogowego na rysunku 14. Wybierz opcję **modyfikowanie pliku Web.config, aby włączyć debugowanie** i kliknij przycisk OK.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Rysunek 14**: włączenie debugowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image28.png))


Widok indeksu jest zwracany przez domyślne. Ten widok zawiera listę wszystkich danych z tabeli bazy danych kontaktów (zobacz rysunek 15).


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Rysunek 15**: widoku indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image30.png))


Zwróć uwagę, że widok indeksu łącze oznaczone etykietą Utwórz nową, w dolnej części widoku. W następnej sekcji dowiesz się, jak utworzyć nowe kontakty.

## <a name="creating-new-contacts"></a>Tworzenie nowych kontaktów

Aby umożliwić użytkownikom tworzenie nowych kontaktów, należy dodać dwie akcje Create() kontrolera głównego. Należy utworzyć jedną akcję Create(), która zwraca formularza HTML do tworzenia nowego kontaktu. Należy utworzyć drugą akcję Create(), który wykonuje Wstawianie bazy danych w jego miejsce nowego kontaktu.

Nowe metody Create(), które musimy dodać kontrolera głównego są zawarte w ofercie 4.

**Wyświetlanie listy 4 - Controllers\HomeController.vb (za pomocą metody Create)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

Pierwsza metoda Create() może być wywoływany za pomocą żądania HTTP GET, podczas gdy druga metoda Create() może być wywoływany tylko przez metodę POST protokołu HTTP. Innymi słowy druga metoda Create() może być wywoływany tylko wtedy, gdy opublikowanie formularza HTML. Pierwsza metoda Create() po prostu zwraca widok, który zawiera formularz HTML do tworzenia nowego kontaktu. Druga metoda Create() jest znacznie bardziej interesujące: dodaje nowy kontakt w bazie danych.

Należy zauważyć, że druga metoda Create() została zmodyfikowana, aby zaakceptować wystąpienia klasy skontaktuj się z pomocą. Wartości formularza opublikowane z formularza HTML są powiązane z tej klasy kontakt przez platformę ASP.NET MVC automatycznie. Każdego pola formularza z formularza HTML, tworzenie jest przypisany do właściwości parametru kontaktu.

Należy zauważyć, że parametr skontaktuj się z pomocą zostanie nadany atrybut [powiązania]. Atrybut [powiązania] jest używany do wykluczenia właściwości identyfikator kontaktu z powiązania. Ponieważ właściwość Id reprezentuje właściwość tożsamości, firma Microsoft don t chcesz ustawić właściwość Id.

W treści metody Create() Entity Framework służy do wstawiania nowego kontaktu do bazy danych. Nowy kontakt jest dodawany do istniejącego zestawu kontakty i SaveChanges() metoda jest wywoływana w celu wypchnięcia tych zmian z powrotem do podstawowej bazy danych.

Możesz wygenerować formularza HTML do tworzenia nowych kontaktów w jednej z dwóch metod Create() prawym przyciskiem myszy i wybierając opcję menu **Dodaj widok** (zobacz rysunek 16).


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Rysunek 16**: Dodawanie widoku Create ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image32.png))


W **Dodaj widok** okno dialogowe, wybierz opcję **ContactManager.Contact** klasy i **Utwórz** opcję Wyświetl zawartość (zobacz rysunek 17). Po kliknięciu **Dodaj** przycisk Utwórz, Wyświetl jest generowana automatycznie.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Rysunek 17**: wyświetlanie strony explode ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image34.png))


Utwórz widok zawiera pól formularza, dla każdej właściwości klasy skontaktuj się z. Kod dla widoku Utwórz znajduje się w ofercie 5.

**Wyświetlanie listy 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Po zmodyfikowaniu metody Create() i Dodaj widok Utwórz, można uruchomić aplikację Menedżer skontaktuj się z pomocą i tworzenie nowych kontaktów. Kliknij przycisk **Utwórz nowy** łącze, które pojawia się w widoku indeksu, aby przejść do widoku Create. Powinien zostać wyświetlony widok na rysunku 18.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Rysunek 18**: Create View ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image36.png))


## <a name="editing-contacts"></a>Edytowanie kontaktów

Dodawanie funkcji do edytowania rekordu kontaktu jest bardzo podobne do dodawania funkcjonalności do tworzenia nowych rekordów kontaktów. Najpierw należy dodać dwie nowe metody edycji do klasy kontrolera głównej. Te nowe metody Edit() są zawarte w ofercie 6.

**Wyświetlanie listy 6 - Controllers\HomeController.vb (przy użyciu metod edycji)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

Pierwsza metoda Edit() jest wywoływany przez operację GET protokołu HTTP. Parametr Id jest przekazywany do tej metody, która reprezentuje identyfikator rekordu kontaktu edytowany. Entity Framework służy do pobierania kontaktu, który pasuje do identyfikatora. Widok, który zawiera formularz HTML na potrzeby edytowania rekordu jest zwracana.

Druga metoda Edit() wykonuje rzeczywistej aktualizacji do bazy danych. Ta metoda przyjmuje wystąpienie klasy skontaktuj się z pomocą jako parametr. Platforma ASP.NET MVC wiąże pola formularza z formularza edycji tej klasy automatycznie. Zwróć uwagę, że komputer t include — atrybut [powiązania] podczas edycji kontaktu (potrzebujemy wartość właściwości identyfikator).

Entity Framework jest używany do zapisania zmodyfikowanych skontaktuj się z bazą danych. Kontakcie musi najpierw pobrany z bazy danych. Następnie wywoływana jest metoda Entity Framework ApplyPropertyChanges(), aby zarejestrować zmiany do kontaktu. Na koniec wywoływana jest metoda Entity Framework SaveChanges(), aby utrwalić zmiany do podstawowej bazy danych.

Można wygenerować widok, który zawiera formularz edycji, kliknij prawym przyciskiem myszy metodę Edit() i wybierając opcję menu Dodaj widok. W oknie dialogowym Dodawanie widoku wybierz **ContactManager.Models.Contact** klasy i **Edytuj** wyświetlanie zawartości (zobacz rysunek 19).


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Rysunek 19**: Dodawanie widoku edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image38.png))


Po kliknięciu przycisku Dodaj nowy widok edycji jest generowany automatycznie. Formularza HTML, który jest generowany zawiera pola, które odpowiadają każdej z właściwości klasy kontaktu (patrz lista 7).

**Wyświetlanie listy 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Usuwanie kontaktów

Jeśli chcesz usuwać kontakty, a następnie należy dodać dwie akcje Delete() do klasy kontrolera głównej. Pierwszą akcją Delete() Wyświetla formularz potwierdzenie usunięcia. Drugiej akcji Delete() przeprowadza rzeczywiste usunięcie.

> [!NOTE] 
> 
> Później w iteracji #7 zmodyfikujemy Contact Manager tak, aby go obsługuje jeden krok, Usuń Ajax.


Dwie nowe metody Delete() są zawarte w ofercie 8.

**Wyświetlanie listy 8 - Controllers\HomeController.vb (metody Delete)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

Pierwsza metoda Delete() zwraca formularza Potwierdzenie usuwania rekordu kontaktu z bazy danych (zobacz Figure20). Druga metoda Delete() wykonuje operację usuwania rzeczywiste w bazie danych. Po kontakcie ma zostały pobrane z bazy danych, metod programu Entity Framework DeleteObject() i SaveChanges() są wywoływane w celu wykonania usunięcia bazy danych.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Rysunek 20**: Wyświetl potwierdzenie usunięcia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image40.png))


Potrzebujemy do modyfikowania widoku indeksu, tak aby zawierała link do usuwania rekordów kontaktów (patrz rysunek 21). Należy dodać następujący kod do tego samego komórkę tabeli, która zawiera link edycji:

{.id = element. % Identyfikator})&gt;


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Rysunek 21**: indeks widok z link edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image42.png))


Następnie należy utworzyć widok potwierdzenie usunięcia. Kliknij prawym przyciskiem myszy Metoda Delete() klasy kontrolera głównej, a następnie wybierz opcję menu Dodaj widok. Zostanie wyświetlone okno dialogowe dodawania widoku, (zobacz rysunek 22).

W odróżnieniu od przypadku listy, tworzenia i edytowania widoków, okno dialogowe dodawania widoku nie zawiera opcję, aby utworzyć widok Delete. Zamiast tego należy wybrać **ContactManager.Models.Contact** klasy danych i **pusty** wyświetlanie zawartości. Wybieranie pusty widok opcji zawartość będzie wymagać NAS utworzyć widok, określić główną przyczynę.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Rysunek 22**: Dodawanie widoku potwierdzenia usunięcia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image44.png))


Zawartość widoku Delete znajduje się w ofercie 9. Ten widok zawiera formularz, który stanowi potwierdzenie, czy nie powinien być określony kontakt usunięte (patrz rysunek 21).

**Wyświetlanie listy 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Zmienianie nazwy domyślnego kontrolera

Go może być odblokowane, nazwa klasy Nasze kontrolera do pracy z kontaktów nosi nazwę klasy HomeController. T nie powinien kontrolera nosić nazwę ContactController?

Ten problem jest łatwe rozwiązać problem. Najpierw musimy Refaktoryzuj nazwę kontrolera głównego. Otwórz klasę HomeController w edytorze kodu programu Visual Studio, kliknij prawym przyciskiem myszy nazwę klasy i wybierz opcję menu **Zmień nazwę**. Wybranie tej opcji menu zostanie otwarte okno dialogowe zmieniania nazwy.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Ilustracja 23**: Refaktoryzacja nazwę kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image46.png))


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Rysunek 24**: za pomocą okna dialogowego zmiany nazwy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image48.png))


Jeśli zmienisz nazwę klasy kontrolera, Visual Studio spowoduje zaktualizowanie nazwę folderu, w tym folderze widoków. Program Visual Studio spowoduje zmianę nazwy folderu \Views\Home do folderu \Views\Contact.

Po wprowadzeniu tej zmiany, aplikacja nie będzie miało kontrolera głównego. Po uruchomieniu aplikacji, uzyskasz strony błędu w ilustracja 25.


[![Okno dialogowe Nowy projekt](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Rysunek 25**: nie domyślnego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-1-create-the-application-vb/_static/image50.png))


Musimy zaktualizować trasy domyślnej w pliku Global.asax do użycia kontrolera skontaktuj się z pomocą zamiast kontrolera głównego. Otwórz plik Global.asax i zmodyfikować kontroler domyślne używane przez trasa domyślna (zobacz listę 10).

**Lista 10 - Global.asax.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Po wprowadzeniu tych zmian, Contact Manager będzie działać poprawnie. Teraz skontaktuj się z klasy controller zostanie użyty jako domyślny kontroler.

## <a name="summary"></a>Podsumowanie

W tym pierwszą iteracją utworzyliśmy Podstawowa aplikacja Contact Manager w najszybszy sposób możliwe. Firma Microsoft skorzystała z programu Visual Studio, aby automatycznie wygenerować kod początkowy dla naszych widoków i kontrolerów. Możemy również skorzystała z platformy Entity Framework, aby automatycznie wygenerować naszych zajęć model bazy danych.

Obecnie firma Microsoft listy, tworzenie, edytowanie i usuwanie rekordów kontaktów z aplikacji Contact Manager. Innymi słowy można wykonać wszystkie operacje podstawowej bazy danych wymagane przez aplikację sieci web opartej na bazie danych.

Niestety nasz aplikacja ma pewne problemy. Pierwszy i wahaj się zezwolić na to, aplikacja Contact Manager nie jest najbardziej atrakcyjnych aplikacji. Musi ona pewne prace projektowe. W następnej iteracji omówimy sposób możemy zmienić domyślnej strony wzorcowej widoku i kaskadowy arkusz stylów, aby poprawić wygląd aplikacji.

Po drugie nie zaimplementowano rozwiązania tlp wszelkie weryfikacji formularza. Na przykład nie ma nic do uniemożliwić przesyłanie formularz kontaktowy Utwórz bez podawania wartości dla pól formularza. Ponadto możesz wprowadzić nieprawidłowy numer telefonu liczb i ich adresy e-mail. Na początek się problemem weryfikacji formularza w iteracji #3.

Na końcu, a co najważniejsze bieżącą iterację aplikacji Contact Manager nie może być łatwo zmodyfikowany lub utrzymywane. Na przykład logiką dostępu do bazy danych jest wbudowanymi prawo do akcji kontrolera. Oznacza to, że nasz kod dostępu do danych nie można zmodyfikować bez modyfikowania naszych kontrolerów. W późniejszej iteracji firma Microsoft zapoznaj się z wzorców projektowania oprogramowania, które można zaimplementować się Contact Manager bardziej odporne na zmiany.

> [!div class="step-by-step"]
> [Poprzednie](iteration-7-add-ajax-functionality-cs.md)
> [dalej](iteration-2-make-the-application-look-nice-vb.md)
