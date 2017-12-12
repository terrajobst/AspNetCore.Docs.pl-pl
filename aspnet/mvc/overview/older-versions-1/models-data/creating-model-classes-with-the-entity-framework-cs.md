---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Tworzenie klasy modelu Entity Framework (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: "W tym samouczku Dowiedz się jak używać programu ASP.NET MVC Microsoft Entity Framework. Jak utworzyć Da jednostki ADO.NET za pomocą kreatora jednostki..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 8a897f671de73d9991189e32a5d86b513051ef05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-the-entity-framework-c"></a>Tworzenie klasy modelu Entity Framework (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym samouczku Dowiedz się jak używać programu ASP.NET MVC Microsoft Entity Framework. Jak używać Kreatora jednostki do tworzenia modelu danych jednostki ADO.NET. W trakcie tego samouczka możemy utworzyć aplikacji sieci web, która ilustruje sposób Wybierz, wstawiania, aktualizowania i usuwania danych w bazie danych przy użyciu programu Entity Framework.


Celem tego samouczka jest opisano sposób tworzenia klas dostępu do danych za pomocą programu Microsoft Entity Framework, podczas tworzenia aplikacji platformy ASP.NET MVC. Ten samouczek zakłada nie wcześniejsza wiedza Microsoft Entity Framework. Na koniec tego samouczka będziesz zrozumienie, jak Entity Framework umożliwia wybranie, wstawianie, aktualizowanie i usuwanie rekordów bazy danych.

Microsoft Entity Framework jest narzędziem obiektów relacyjnych mapowania (O/RM) umożliwia automatyczne generowanie Warstwa dostępu do danych z bazy danych. Entity Framework pozwala uniknąć konieczność ręcznego tworzenia klas dostępu do danych.

Aby zilustrować, jak za pomocą programu Entity Framework Microsoft ASP.NET MVC, firma Microsoft będzie Tworzenie prostego przykładowej aplikacji. Utworzymy aplikację filmu bazy danych, która umożliwia wyświetlanie i edytowanie filmu rekordów bazy danych.

Ten samouczek zakłada, że masz program Visual Studio 2008 lub Visual Web Developer 2008 z dodatkiem Service Pack 1. Należy z dodatkiem Service Pack 1, aby można było używać programu Entity Framework. Visual Studio 2008 z dodatkiem Service Pack 1 lub Visual Web Developer z dodatkiem Service Pack 1 można pobrać z następującego adresu:

> [https://www.asp.NET/downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> Brak istotnych połączenia między ASP.NET MVC i Microsoft Entity Framework. Istnieje kilka rozwiązań alternatywnych do narzędzia Entity Framework, korzystających z platformy ASP.NET MVC. Na przykład można tworzyć przy użyciu innych narzędzi O/RM, takich jak Microsoft LINQ to SQL, NHibernate lub SubSonic klasach modeli MVC.


## <a name="creating-the-movie-sample-database"></a>Tworzenie przykładowej bazy danych film

Aplikacja filmu bazy danych korzysta z tabeli bazy danych o nazwie filmów, który zawiera następujące kolumny:

| Nazwa kolumny | Typ danych | Dopuszcza wartości null? | To jest klucz podstawowy? |
| --- | --- | --- | --- |
| Identyfikator | int | False | Wartość true |
| Tytuł | Nvarchar(100) | False | False |
| Dyrektor | Nvarchar(100) | False | False |

W tej tabeli można dodać do projektu programu ASP.NET MVC, wykonując następujące czynności:

1. Kliknij prawym przyciskiem myszy aplikację\_folderu danych w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element.**
2. Z **Dodaj nowy element** okno dialogowe, wybierz opcję **bazy danych programu SQL Server**, nazwę bazy danych MoviesDB.mdf i kliknij przycisk **Dodaj** przycisku.
3. Kliknij dwukrotnie plik MoviesDB.mdf, aby otworzyć okno Eksploratora Explorer/bazy danych serwera.
4. Rozwiń węzeł MoviesDB.mdf połączenie z bazą danych, kliknij prawym przyciskiem myszy folder Tabele i wybierz opcję menu **Dodaj nową tabelę**.
5. Przy użyciu projektanta tabeli Dodaj identyfikator, tytuł i dyrektora kolumny.
6. Kliknij przycisk **zapisać** przycisk (ma ikony dyskietek) aby zapisać nową tabelę z filmów nazwy.

Po utworzeniu filmów tabeli bazy danych, należy dodać przykładowych danych do tabeli. Kliknij prawym przyciskiem myszy tabelę filmy i wybierz opcję menu **Pokaż dane tabeli**. Do siatki, którą można wprowadzić filmu fałszywych danych.

## <a name="creating-the-adonet-entity-data-model"></a>Tworzenie modelu danych jednostki ADO.NET

Aby można było używać programu Entity Framework, musisz utworzyć modelu danych jednostki. Możesz korzystać z programu Visual Studio *kreatora modelu danych jednostki* automatycznie wygenerowany modelu Entity Data Model z bazy danych.

Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy folder modeli w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. W **Dodaj nowy element** okno dialogowe, wybierz kategorię danych (zobacz rysunek 1).
3. Wybierz **modelu danych jednostki ADO.NET** szablonie, nazwę modelu Entity Data Model MoviesDBModel.edmx, a następnie kliknij przycisk **Dodaj** przycisku. Kliknięcie przycisku **Dodaj** przycisk uruchamia Kreatora modelu danych.
4. W **wybierz zawartość modelu** kroku, wybierz **generowania z bazy danych** opcję i kliknij przycisk **dalej** przycisk (patrz rysunek 2).
5. W **wybierz połączenie danych** krok, wybierz połączenie z bazą danych MoviesDB.mdf, wprowadź jednostek ustawienia połączenia nazwę MoviesDBEntities, a następnie kliknij przycisk **dalej** przycisk (patrz rysunek 3).
6. W **wybierz obiekty bazy danych użytkownika** kroku, wybierz film tabeli bazy danych i kliknij przycisk **Zakończ** przycisk (patrz rysunek 4).

Po wykonaniu tych kroków otwiera Projektant modelu danych jednostki ADO.NET (Entity Designer).

**Rysunek 1 — Tworzenie nowego modelu danych jednostki**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Rysunek 2 — Wybierz Model zawartości kroku**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Rysunek 3 – Wybierz połączenie danych**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Rysunek 4 — Wybierz obiekty bazy danych**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Modyfikowanie modelu danych jednostki ADO.NET

Po utworzeniu modelu danych jednostki, można zmodyfikować modelu dzięki wykorzystaniu Entity Designer (patrz rysunek 5). Program Entity Designer można otworzyć w dowolnym momencie, klikając dwukrotnie plik MoviesDBModel.edmx znajdujące się w folderze modeli w oknie Eksploratora rozwiązań.

**Rysunek 5 — Projektant modelu danych jednostki ADO.NET**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Na przykład służy program Entity Designer zmiana nazw klas, generowane przez kreatora Entity Model danych. Kreator utworzyć nową klasę dostępu do danych o nazwie filmów. Innymi słowy Kreator udzielił klasy bardzo samą nazwę jak tabeli bazy danych. Ponieważ klasa będzie używana do reprezentowania konkretnego wystąpienia film, możemy zmienić nazwy klasy filmy film.

Jeśli chcesz zmienić nazwę klasy jednostki można kliknij dwukrotnie nazwę klasy w programie Entity Designer i wprowadź nową nazwę (patrz rysunek 6). Alternatywnie można zmienić nazwę jednostki w oknie właściwości po wybraniu jednostki w programie Entity Designer.

**Rysunek 6 — zmiana nazwy podmiotu**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Pamiętaj, aby zapisać modelu Entity Data Model po wprowadzeniu zmiany przez kliknięcie przycisku Zapisz (ikona dyskietki). W tle przez program Entity Designer generuje zestaw klas C#. Te klasy można wyświetlić, otwierając plik MoviesDBModel.Designer.cs z okna Eksploratora rozwiązań.


Nie należy modyfikować kod w pliku Designer.cs narzędzie, ponieważ zmiany zostaną zastąpione podczas następnego Użyj Projektanta obiektów. Jeśli chcesz rozszerzyć funkcjonalność klas jednostek zdefiniowanych w pliku Designer.cs narzędzie, a następnie można utworzyć *klasy częściowe* w oddzielnych plikach.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Wybieranie rekordów bazy danych z programu Entity Framework

Zacznijmy tworzenia aplikacji filmu bazy danych, tworząc strona, wyświetlająca listę rekordów filmu. Kontroler głównej w 1 lista przedstawia akcji o nazwie indeks(). Akcja indeks() zwraca wszystkie rekordy film z tabeli bazy danych film korzystając z programu Entity Framework.

**Wyświetlanie listy 1 – Controllers\HomeController.cs**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Zwróć uwagę, że kontroler w 1 Lista zawiera konstruktora. Konstruktor inicjuje pola poziomie klasy o nazwie \_bazy danych. \_Reprezentuje pole db jednostki bazy danych wygenerowanych przez program Microsoft Entity Framework. \_Pole bazy danych jest wystąpienie klasy MoviesDBEntities, który został wygenerowany przez program Entity Designer.


Aby można było używać klasy theMoviesDBEntities w kontrolerze główną, należy zaimportować przestrzeni nazw MovieEntityApp.Models (*MVCProjectName*. Modele).


\_Pole bazy danych jest używany w akcji indeks() pobrać rekordy z tabeli bazy danych filmów. Wyrażenie \_bazy danych. MovieSet reprezentuje wszystkie rekordy z tabeli filmy bazy danych. Metoda ToList() służy do konwertowania zbiór filmów w ogólnej kolekcji obiektów Movie (listy&lt;film&gt;).

Rejestruje filmu są pobierane za pomocą LINQ to Entities. Akcja indeks() wyświetlania 1 używa LINQ *składni metody* można pobrać zestaw rekordów bazy danych. Jeśli wolisz, możesz użyć LINQ *Składnia kwerendy* zamiast tego. Poniższe instrukcje dwóch wykonaj bardzo samo:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Użyj niezależnie od składni LINQ — składnia metody lub składnia zapytania —, który można znaleźć najbardziej intuicyjnego. Nie ma różnic w wydajności między dwa podejścia — jedyna różnica polega na styl.

Widok wyświetlania 2 służy do wyświetlania rekordów filmu.

**Wyświetlanie listy 2 — Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

Widok wyświetlania 2 zawiera **foreach** pętli, który iteruje każdego rekordu filmu i wyświetla wartości właściwości Title i dyrektora rekordu filmu. Należy zauważyć, że edytowanie i usuwanie link jest wyświetlany obok każdego rekordu. Ponadto łącze Dodaj film jest wyświetlane w dolnej części widoku (patrz rysunek 7).

**Rysunek 7 — widok indeksu**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

Widok indeksu jest *widoku wpisanych*. Widok indeksu zawiera &lt;% @ strony %&gt; dyrektywy z *Inherits* atrybut, który rzutuje właściwości modelu do silnie typizowaną ogólnej kolekcji listy obiektów Movie (listy&lt;film).

## <a name="inserting-database-records-with-the-entity-framework"></a>Wstawianie rekordów bazy danych z programu Entity Framework

Aby ułatwić wstawianie nowych rekordów do tabeli bazy danych, można użyć programu Entity Framework. Wyświetlanie listy 3 zawiera dwa nowe akcje dodany do klasy kontrolera głównej, która umożliwia wstawianie nowych rekordów do filmu tabeli bazy danych.

**Wyświetlanie listy 3 – Controllers\HomeController.cs (Dodaj metody)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

Pierwszą akcją Add() po prostu zwraca widok. Widok zawiera formularz służący do dodawania nowej bazy danych filmu rekordów (patrz rysunek 8). Po przesłaniu formularza jest wywoływany drugiej akcji Add().

Zwróć uwagę, że drugiej akcji Add() zostanie nadany atrybut AcceptVerbs. Ta akcja może być wywoływany tylko w przypadku wykonywania operacji POST protokołu HTTP. Innymi słowy ta akcja może być wywoływany tylko zaksięgowania formularza HTML.

Drugiej akcji Add() tworzy nowe wystąpienie klasy Entity Framework filmu przy pomocy metody ASP.NET MVC TryUpdateModel(). Metoda TryUpdateModel() pobiera pola FormCollection przekazany do metody Add() i przypisuje wartości tych pól formularza HTML do klasy filmu.


Podczas korzystania z programu Entity Framework, należy podać "białą listę" właściwości podczas przy użyciu metod TryUpdateModel lub UpdateModel, aby zaktualizować właściwości klasy jednostka.


Następnie akcji Add() sprawdza poprawność niektórych prostych. Akcja sprawdza, czy tytuł i dyrektora właściwości mają wartości. W przypadku błędu sprawdzania poprawności, komunikat o błędzie weryfikacji jest dodawany do ModelState.

Jeśli nie ma żadnych błędów sprawdzania poprawności nowego rekordu film jest dodawany do filmy tabeli bazy danych za pomocą programu Entity Framework. Nowy rekord został dodany do bazy danych z następujących dwóch wierszy kodu:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

Pierwszy wiersz kodu dodaje nową jednostkę filmu do zestawu filmy śledzony przez program Entity Framework. Drugi wiersz kodu zapisuje, niezależnie od zmian wprowadzono filmy śledzone do podstawowej bazy danych.

**Rysunek 8 — Dodaj widok**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Aktualizowanie rekordów bazy danych z programu Entity Framework

Można wykonać prawie te same podejście, aby edytować rekord bazy danych z programu Entity Framework jako podejście, która była tylko do wstawienia nowego rekordu bazy danych. Wyświetlanie listy 4 zawiera dwa nowe akcje kontrolera o nazwie Edit(). Pierwszą akcją Edit() zwraca formularza HTML do edytowania rekordu filmu. Drugiej akcji Edit() próbuje zaktualizować bazę danych.

**Wyświetlanie listy 4 — Controllers\HomeController.cs (Edycja metody)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

Uruchamia drugiej akcji Edit() pobierając filmu rekordu z bazy danych, który jest zgodny z identyfikatorem filmu edytowany. Następujące składnika LINQ to Entities instrukcji grabs pierwszy rekord bazy danych, zgodny z określonym identyfikatorem:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

Następnie metoda TryUpdateModel() jest używana do przypisywania wartości pola formularza HTML do właściwości elementu entity filmu. Zwróć uwagę, czy białą listę jest dostarczany do określenia dokładnej właściwości do zaktualizowania.

Następnie niektóre proste weryfikacji można sprawdzić, czy tytuł filmu i dyrektora właściwości mają wartości. Jeśli właściwość albo brakuje wartości, komunikat o błędzie weryfikacji jest dodawany do ModelState i ModelState.IsValid zwraca wartość false.

Ponadto jeśli nie ma żadnych błędów sprawdzania poprawności, następnie tabeli podstawowej bazy danych filmy zostało zaktualizowane o zmianach, wywołując metodę SaveChanges().

Podczas edytowania rekordów bazy danych, musisz przekazać właściwości Id rekordu edytowany do akcji kontrolera, który wykonuje aktualizacji bazy danych. W przeciwnym razie akcji kontrolera nie będzie wiedzieć, który rekordów do aktualizacji w bazie danych. Widok edycji, zawiera listę 5 zawiera ukryte pole formularza reprezentujący identyfikator edytowanego rekordu bazy danych.

**Wyświetlanie listy 5 — Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Usuwanie rekordów bazy danych z programu Entity Framework

Operacja końcowego bazy danych, która potrzebne do rozwiązania problemu w tym samouczku, jest usunięcie rekordów bazy danych. Akcji kontrolera wyświetlania 6 służy do usuwania rekordu określonej bazy danych.

**Wyświetlanie listy 6 — \Controllers\HomeController.cs (Akcja usuwania)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

Akcja Delete() najpierw pobiera filmu jednostki, który jest zgodny z identyfikatorem przekazany do akcji. Następnie filmu został usunięty z bazy danych przez wywołanie metody DeleteObject() następuje metody SaveChanges(). Na koniec zostanie przekierowany użytkownik w powrót do widoku indeksu.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było pokazują, jak można tworzyć aplikacje opartej na bazie danych w sieci web dzięki wykorzystaniu ASP.NET MVC i Microsoft Entity Framework. Przedstawiono sposób tworzenia aplikacji, która umożliwia wybranie, wstawianie, aktualizowanie i usuwanie rekordów bazy danych.

Po pierwsze omówiono, jak można użyć Kreatora modelu danych jednostki do wygenerowania modelu Entity Data Model z poziomu programu Visual Studio. Następnie zostanie przedstawiony sposób Pobierz zestaw rekordów bazy danych z tabeli bazy danych za pomocą LINQ to Entities. Na koniec użyliśmy programu Entity Framework do wstawiania, aktualizacji i usuwania rekordów bazy danych.

>[!div class="step-by-step"]
[Dalej](creating-model-classes-with-linq-to-sql-cs.md)
