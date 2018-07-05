---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Tworzenie klas modelu za pomocą platformy Entity Framework (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku dowiesz się, jak używać platformy ASP.NET MVC z programem Microsoft Entity Framework. Dowiesz się, jak używać Kreator modelu Entity do tworzenia aplikacji rozproszonej programu ADO.NET Entity...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: efb8d7206cba2fd5d8db1817d57a4e2813cb5ace
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377191"
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Tworzenie klas modelu za pomocą platformy Entity Framework (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym samouczku dowiesz się, jak używać platformy ASP.NET MVC z programem Microsoft Entity Framework. Dowiesz się, jak używać Kreator modelu Entity do tworzenia modelu danych jednostki ADO.NET. W trakcie tego samouczka firma Microsoft tworzenie aplikacji sieci web, który ilustruje sposób Wybierz, Wstaw, Aktualizuj i usunąć dane z bazy danych za pomocą programu Entity Framework.


Celem tego samouczka jest wyjaśniono sposób tworzenia klas dostępu do danych za pomocą programu Microsoft Entity Framework, podczas tworzenia aplikacji ASP.NET MVC. Ten samouczek zakłada się nie wcześniejsza wiedza Microsoft Entity Framework. Do końca tego samouczka będziesz zrozumieć, jak używać programu Entity Framework wybierz, wstawianie, aktualizowanie i usuwanie rekordów bazy danych.

Microsoft Entity Framework to narzędzie obiektu relacyjne mapowanie (O/RM), które umożliwia automatyczne generowanie warstwy dostępu do danych z bazy danych. Entity Framework pozwala uniknąć konieczność ręcznego tworzenia swojej klasy dostępu do danych.

> [!NOTE] 
> 
> Nie ma zasadnicze połączenia między platformy ASP.NET MVC i Microsoft Entity Framework. Istnieje kilka rozwiązań alternatywnych, w programie Entity Framework, który za pomocą platformy ASP.NET MVC. Można na przykład, utworzyć przy użyciu innych narzędzi Obiektowo, takich jak Microsoft LINQ to SQL i NHibernate, SubSonic klasach modeli MVC.


Aby zilustrować, jak za pomocą programu Entity Framework Microsoft ASP.NET MVC, utworzymy prostą przykładowej aplikacji. Utworzymy aplikację baza danych filmów, która umożliwia wyświetlanie i edytowanie rekordów bazy danych filmów.

Ten samouczek zakłada, że masz program Visual Studio 2008 lub Visual Web Developer 2008 z dodatkiem Service Pack 1. Z dodatkiem Service Pack 1 jest niezbędna, aby używać programu Entity Framework. Możesz pobrać program Visual Studio 2008 z dodatkiem Service Pack 1 lub Visual Web Developer z dodatkiem Service Pack 1 z następującego adresu:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>Tworzenie przykładowej bazy danych filmów

Baza danych filmów aplikację o nazwie filmy do tabeli bazy danych, która zawiera następujące kolumny:

| Nazwa kolumny | Typ danych | Zezwalaj na wartości null? | Jest klucz podstawowy? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Tytuł | Nvarchar(100) | False | False |
| Dyrektor ds. | Nvarchar(100) | False | False |

W tej tabeli można dodać do projektu programu ASP.NET MVC, wykonując następujące czynności:

1. Kliknij prawym przyciskiem myszy aplikację\_folderu danych w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element.**
2. Z **Dodaj nowy element** okno dialogowe, wybierz opcję **bazy danych SQL Server**, nazwę bazy danych MoviesDB.mdf i kliknij przycisk **Dodaj** przycisku.
3. Kliknij dwukrotnie plik MoviesDB.mdf, aby otworzyć okno Server Explorer/Eksploratorze bazy danych.
4. Rozwiń MoviesDB.mdf połączenia z bazą danych, kliknij prawym przyciskiem myszy folder tabele, a następnie wybierz opcję menu **Dodaj nową tabelę**.
5. W Projektancie tabel Dodaj identyfikator, tytuł i dyrektor ds. kolumny.
6. Kliknij przycisk **Zapisz** przycisku (ma on ikonę dyskietek) aby zapisać nową tabelę z filmy nazwy.

Po utworzeniu tabeli bazy danych filmów, należy dodać przykładowe dane do tabeli. Kliknij prawym przyciskiem myszy tabelę filmów, a następnie wybierz opcję menu **Pokaż dane tabeli**. Można wprowadzić filmu fałszywych danych w siatce, która jest wyświetlana.

## <a name="creating-the-adonet-entity-data-model"></a>Tworzenie modelu danych jednostki ADO.NET

Aby można było używać programu Entity Framework, należy utworzyć model Entity Data Model. Możesz skorzystać z programu Visual Studio *Kreator modelu Entity Data Model* do automatycznego wygenerowania modelu Entity Data Model z bazy danych.

Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy folder modeli, w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. W **Dodaj nowy element** okno dialogowe, wybierz kategorię danych (patrz rysunek 1).
3. Wybierz **ADO.NET Entity Data Model** szablonu, nadaj nazwę MoviesDBModel.edmx modelu Entity Data Model, a następnie kliknij przycisk **Dodaj** przycisku. Klikając **Dodaj** przycisk uruchamia Kreatora modelu danych.
4. W **wybierz zawartość modelu** kroku, wybierz **Generuj z bazy danych** opcji, a następnie kliknij przycisk **dalej** przycisku (patrz rysunek 2).
5. W **wybierz połączenie danych** krok, wybierz połączenie z bazą danych MoviesDB.mdf, wprowadź jednostek ustawienia połączenia nazwę MoviesDBEntities, a następnie kliknij przycisk **dalej** przycisku (zobacz rysunek 3).
6. W **wybierz obiekty bazy danych** , wybierz tabelę bazy danych filmów i kliknij pozycję **Zakończ** przycisku (zobacz rysunek 4).

Po wykonaniu tych kroków, zostanie otwarty projektant modelu danych jednostki ADO.NET (Projektant jednostki).

**Rysunek 1 — Tworzenie nowego modelu Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Rysunek 2 — Wybierz etap zawartość modelu**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Rysunek 3 – Wybierz połączenie danych**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Rysunek 4 — Wybierz obiekty bazy danych**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modyfikowanie modelu danych jednostki ADO.NET

Po utworzeniu modelu Entity Data Model, można modyfikować modelu, wykorzystując Projektancie jednostki (zobacz rysunek 5). Projektant jednostki można otworzyć w dowolnym momencie, klikając dwukrotnie plik MoviesDBModel.edmx znajdujące się w folderze modeli w oknie Eksploratora rozwiązań.

**Rysunek 5 — Projektant modelu danych jednostki ADO.NET**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Na przykład można użyć w Projektancie jednostki można zmienić nazwy klasy, które generuje Kreator modelu Entity Data Model. Kreator utworzył nową klasę dostępu do danych o nazwie filmów. Innymi słowy Kreator zapewniła klasy bardzo takiej samej nazwie jak tabela bazy danych. Ponieważ firma Microsoft użyje tej klasy do reprezentowania konkretnego wystąpienia filmu, możemy zmienić nazwy klasy filmy filmu.

Jeśli chcesz zmienić nazwę klasy jednostki, możesz kliknij dwukrotnie nazwę klasy w Projektancie jednostki i wprowadź nową nazwę (patrz rysunek 6). Alternatywnie możesz zmienić nazwę jednostki w oknie dialogowym właściwości po wybraniu jednostki w Projektancie jednostki.

**Rysunek 6 — zmiana nazwy podmiotu**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Pamiętaj, aby zapisać modelu Entity Data Model po wprowadzeniu zmiany przez kliknięcie przycisku Zapisz (ikona dysku). W tle w Projektancie jednostki generuje zestaw klas w języku Visual Basic .NET. Te klasy można wyświetlić, otwierając plik MoviesDBModel.Designer.vb z okna Eksploratora rozwiązań.


Nie należy modyfikować kodu w pliku Designer.vb, ponieważ zmiany zostaną zastąpione podczas następnego używany jest Projektant jednostki. Jeśli chcesz rozszerzyć funkcjonalność klasy jednostek zdefiniowane w pliku Designer.vb, a następnie można utworzyć *klas częściowych* w oddzielnych plikach.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Wybieranie rekordów bazy danych za pomocą programu Entity Framework

Zacznijmy od tworzenia aplikacji bazy danych filmów, tworząc strony, który wyświetla listę rekordów filmu. Kontrolera głównego w ofercie 1 przedstawia akcję o nazwie indeks(). Akcja indeks() zwraca wszystkie rekordy film z tabeli bazy danych filmów, korzystając z programu Entity Framework.

**Wyświetlanie listy 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Zwróć uwagę, że kontroler w ofercie 1 konstruktora. Konstruktor inicjuje poziomie klasa pole o nazwie \_bazy danych. \_Jednostek bazy danych generowanych przez Microsoft Entity Framework reprezentuje pole bazy danych. \_Pole bazy danych jest wystąpieniem klasy MoviesDBEntities, który został wygenerowany przez Projektanta obiektów.

\_Db pole jest używane w ramach akcji indeks() można pobrać rekordów z tabeli bazy danych filmów. Wyrażenie \_bazy danych. MovieSet reprezentuje wszystkie rekordy z tabeli bazy danych filmów. Metoda ToList() jest używana do przekonwertowania zbiór filmy na ogólnych kolekcji obiektów filmu: Lista (film z).

Rekordy filmu są pobierane za pomocą LINQ to Entities. Akcja indeks() w ofercie 1 używa LINQ *składni metody* można pobrać zestawu rekordów bazy danych. Jeśli wolisz, możesz użyć LINQ *składni zapytania* zamiast tego. Następujące dwie instrukcje bardzo te same czynności wykonasz:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Użyj niezależnie od składni LINQ — składnia metody lub składnia zapytania —, który można znaleźć w najbardziej intuicyjnej. Nie ma żadnej różnicy w wydajności między dwa podejścia — jedyna różnica polega na styl.

Wyświetl w ofercie 2 służy do wyświetlania rekordów filmu.

**Wyświetlanie listy 2 — Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

Widok w ofercie 2 zawiera **dla każdego** pętli, który iteruje przez każdy rekord film i wyświetla wartości właściwości tytuł i dyrektor ds. nagrywanie filmu. Należy zauważyć, że edytowania i usuwania łącze jest wyświetlany obok każdego rekordu. Ponadto łącze Dodaj film pojawi się w dolnej części widoku (zobacz rysunek 7).

**Rysunek 7 — widok indeksu**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

Widok indeksu jest *wpisane widoku*. Widok indeksu ma &lt;% @ % strony&gt; dyrektywę, który zawiera atrybut Inherits. Atrybut Inherits rzutuje właściwość ViewData.Model silnie typizowaną ogólnego listy kolekcji obiektów filmu — listy (film z).

## <a name="inserting-database-records-with-the-entity-framework"></a>Wstawianie rekordów bazy danych za pomocą programu Entity Framework

Aby ułatwić wstawianie nowych rekordów do tabeli bazy danych, można użyć programu Entity Framework. Wyświetlanie listy 3 zawiera dwie nowe akcje dodane do klasy kontrolera głównej, która umożliwia wstawianie nowych rekordów do tabeli bazy danych filmów.

**Wyświetlanie listy 3 — Controllers\HomeController.vb (Dodaj metody)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

Pierwszą akcją Add() po prostu zwraca widok. Widok zawiera formularz służący do dodawania nowej bazy danych filmów rejestrowania (zobacz rysunek 8). Po przesłaniu formularza jest wywoływana drugiej akcji Add().

Należy zauważyć, że drugiej akcji Add() zostanie nadany atrybut AcceptVerbs. Ta akcja może być wywoływany tylko wtedy, gdy wykonanie operacji HTTP POST. Innymi słowy ta akcja może być wywoływany tylko ogłaszając formularza HTML.

Drugiej akcji Add() tworzy nowe wystąpienie klasy Entity Framework filmu przy pomocy metody ASP.NET MVC TryUpdateModel(). Metoda TryUpdateModel() przyjmuje pól w FormCollection przekazywany do metody Add() i przypisuje wartości pól formularza HTML do klasy filmu.


Korzystając z programu Entity Framework, należy podać "białą listę" właściwości, korzystając z metody TryUpdateModel lub UpdateModel można zaktualizować właściwości klasy jednostki.


Następnie akcji Add() przeprowadza weryfikację niektóre prosty formularz. Akcja sprawdza, czy tytuł i dyrektor ds. właściwości mają wartości. Jeśli występuje błąd sprawdzania poprawności, komunikat o błędzie weryfikacji jest dodawany do ModelState.

Jeśli nie ma żadnych błędów sprawdzania poprawności nowego rekordu film zostanie dodany do tabeli bazy danych filmów za pomocą programu Entity Framework. Nowy rekord zostanie dodany do bazy danych za pomocą następujących dwóch wierszy kodu:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

Pierwszy wiersz kodu dodaje nową jednostkę film do zestawu filmy śledzone przez program Entity Framework. Drugi wiersz kodu zapisuje wszelkie zmiany wprowadzono filmy są śledzone w podstawowej bazie danych.

**Rysunek 8 — Dodawanie widoku**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Aktualizowanie rekordów bazy danych za pomocą programu Entity Framework

Możesz wykonać niemal to samo podejście do edytowania rekordu bazy danych za pomocą programu Entity Framework metoda, która była tylko do wstawiania nowego rekordu bazy danych. Wyświetlanie listy 4 zawiera dwie nowe akcje kontroler o nazwie Edit(). Pierwszą akcją Edit() zwraca formularza HTML do edytowania rekordu filmu. Drugiej akcji Edit() próbuje zaktualizować bazę danych.

**Wyświetlanie listy 4 – Controllers\HomeController.vb (metod edycji)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

Drugiej akcji Edit() rozpoczyna się od pobierania rekordu film z bazy danych, który jest zgodny z identyfikatorem filmu edytowany. Następujące LINQ do jednostek instrukcji bierze pierwszy rekord bazy danych, który pasuje do określonego identyfikatora:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Następnie metoda TryUpdateModel() służy do przypisywania wartości do pól formularza HTML do właściwości elementu entity filmu. Należy zauważyć, że białą listę jest dostarczany do określenia dokładnego właściwości do zaktualizowania.

Następnie można sprawdzić, czy tytuł filmu i dyrektor ds. właściwości mają wartości odbywa się prostą walidację. Jeśli właściwość albo brakuje wartości, komunikat o błędzie weryfikacji jest dodawany do ModelState i ModelState.IsValid zwraca wartość false.

Na koniec Jeśli nie ma żadnych błędów sprawdzania poprawności, następnie tabeli podstawowej bazy danych filmów jest aktualizowana wszelkie zmiany przez wywołanie metody SaveChanges().

Podczas edytowania rekordów bazy danych, musisz przekazać identyfikator rekordu edytowanym do akcji kontrolera, który wykonuje aktualizację bazy danych. W przeciwnym razie akcji kontrolera nie będą wiedzieć, który rekord ma zostać zaktualizowany w źródłowej bazie danych. Widok do edycji, zawarte w ofercie 5 zawiera pola formularza, które reprezentuje identyfikator rekordu bazy danych, edytowany.

**Wyświetlanie listy 5 — Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Usuwanie rekordów bazy danych za pomocą programu Entity Framework

Operacja końcowego bazy danych, która musimy czoła w ramach tego samouczka, jest usunięcie rekordów bazy danych. Aby usunąć rekord określonej bazy danych, można użyć akcji kontrolera w ofercie 6.

**Wyświetlanie listy 6 — \Controllers\HomeController.vb (Akcja usuwania)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Akcja Delete() najpierw pobiera filmu jednostki, który jest zgodny z identyfikatorem przekazywany do akcji. Następnie film zostanie usunięty z bazy danych przez wywołanie metody DeleteObject() następuje metoda SaveChanges(). Ponadto użytkownik jest przekierowany z powrotem do widoku indeksu.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było pokazują, jak tworzyć aplikacje sieci web opartej na bazie danych, korzystając z wzorca ASP.NET MVC i Microsoft Entity Framework. Pokazaliśmy ci, jak utworzyć aplikację, która umożliwia wybieranie oraz wstawiania, aktualizowania i usuwania rekordów bazy danych.

Po pierwsze omówiono, jak można użyć Kreator modelu Entity Data Model do generowania modelu danych jednostki z poziomu programu Visual Studio. Następnie dowiesz się, jak pobrać zestaw rekordów bazy danych z tabeli bazy danych za pomocą LINQ to Entities. Na koniec użyliśmy platformy Entity Framework, aby Wstawianie, aktualizowanie i usuwanie rekordów bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](validation-with-the-data-annotation-validators-cs.md)
> [dalej](creating-model-classes-with-linq-to-sql-vb.md)
