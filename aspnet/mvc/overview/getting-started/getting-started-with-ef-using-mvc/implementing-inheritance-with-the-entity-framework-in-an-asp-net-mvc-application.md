---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Wdrażanie dziedziczenia z programu Entity Framework 6 w aplikacji ASP.NET MVC 5 (11, 12) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio...
ms.author: aspnetcontent
ms.date: 11/07/2014
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 782ccbbec94cc8ee27995b88b89b2d3bd0bfeb70
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818998"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Wdrażanie dziedziczenia z programu Entity Framework 6 w aplikacji ASP.NET MVC 5 (11, 12)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [Pobierz plik PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i Visual Studio 2013. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednim samouczku obsługiwane są wyjątki współbieżności. Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.

Programowanie zorientowane obiektowo umożliwia [dziedziczenia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) ułatwiające [ponowne użycie kodu](http://en.wikipedia.org/wiki/Code_reuse). W tym samouczku poznasz, jak zmienić `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowej klasy, która zawiera właściwości, takie jak `LastName` , które są wspólne dla instruktorów i studentów. Nie będzie dodać lub zmienić dowolnymi stronami sieci web, ale zmienisz część kodu, a te zmiany są automatycznie odzwierciedlane w bazie danych.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opcje mapowania dziedziczenia w tabelach bazy danych

`Instructor` i `Student` klas w `School` model danych ma kilka właściwości, które są identyczne:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek. Lub aby pisać usługi, które umożliwia formatowanie nazwy bez opiekowanie, czy nazwa pochodzi od pod kierunkiem instruktora lub studenta. Można utworzyć `Person` klasy, która zawiera tylko te właściwości udostępnionego podstawowej, a następnie wprowadź `Instructor` i `Student` jednostek dziedziczą z tej klasy bazowej, jak pokazano na poniższej ilustracji:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Istnieje kilka sposobów, w których ta struktura dziedziczenia, mogą być reprezentowane w bazie danych. Może mieć `Person` tabelę, która zawiera informacje na temat uczniów i Instruktorzy w jednej tabeli. Niektóre kolumny można zastosować tylko do Instruktorzy (`HireDate`), niektóre tylko dla uczniów i studentów (`EnrollmentDate`), niektóre na wartość oba (`LastName`, `FirstName`). Zazwyczaj trzeba *dyskryminatora* kolumny, aby wskazać, jakiego typu każdy wiersz reprezentuje. Na przykład kolumna dyskryminatora może być "Instruktora" instruktorów i "Student" dla uczniów lub studentów.

![Tabela na hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany *Tabela wg hierarchii* dziedziczenia (TPH).

Alternatywą jest, aby wyglądała bardziej jak struktury dziedziczenia bazę danych. Na przykład można mieć tylko pola nazwy `Person` tabeli i mieć osobne `Instructor` i `Student` tabele z polami daty.

![Tabela na type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ten wzorzec polegający na wprowadzaniu tabeli bazy danych dla każdej klasie jednostki jest wywoływana *tabeli na typ* dziedziczenia (TPT).

Innym rozwiązaniem jest jeszcze mapowania wszystkich typów nieabstrakcyjnej poszczególnych tabel. Wszystkie właściwości klasy, w tym właściwości dziedziczonych są mapowane na kolumny odpowiedniej tabeli. Ten wzorzec jest nazywany dziedziczenia klas tabeli na konkretny (TPC). Jeśli zaimplementowano dziedziczenie TPC `Person`, `Student`, i `Instructor` klasy, jak pokazano wcześniej, `Student` i `Instructor` tabel będzie wyglądać nie różni się po zaimplementowaniu dziedziczenia, niż zajmowały przed.

TPC i wzorców dziedziczenia TPH ogólnie dostarczyć, lepszej wydajności platformy Entity Framework niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań.

Ten samouczek demonstruje sposób implementacji TPH dziedziczenia. TPH jest domyślny wzorzec dziedziczenia platformy Entity Framework, więc trzeba wystarczy utworzyć `Person` klasy, zmienić `Instructor` i `Student` pochodzi od klasy `Person`, Dodaj nową klasę do `DbContext`i Utwórz Migracja. (Uzyskać informacji o sposobie wdrażania z innymi wzorami dziedziczenia, zobacz [mapowanie dziedziczenia tabela wg typu (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) i [mapowanie dziedziczenia klasy tabeli na konkretny (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) w witrynie MSDN Dokumentacja programu Entity Framework.)

## <a name="create-the-person-class"></a>Utwórz klasę osoby

W *modeli* folderze utwórz *osoba.cs* i Zastąp kod szablonu poniższym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Utworzyć dla uczniów i instruktora klasy dziedziczyć od osoby

W *Instructor.cs*, pochodzi `Instructor` klasy z `Person` klasy, a następnie usuń klucza i nazwy pola. Ten kod będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Wprowadzić zmiany podobne do *Student.cs*. `Student` Klasy będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Dodaj osoby typu jednostki do modelu

W *SchoolContext.cs*, Dodaj `DbSet` właściwość `Person` typ jednostki:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To wszystko, co wymaga programu Entity Framework, w celu skonfigurowania Tabela wg hierarchii dziedziczenia. Jak można zauważyć, gdy baza danych zostanie zaktualizowany, będzie miał `Person` tabeli zamiast `Student` i `Instructor` tabel.

## <a name="create-and-update-a-migrations-file"></a>Tworzenie i aktualizowanie plików migracji

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:

`Add-Migration Inheritance`

Uruchom `Update-Database` polecenia w konsoli zarządzania Pakietami. Polecenie będzie działać na tym etapie, ponieważ będziemy mieć istniejące dane migracji nie wie, jak obsługiwać. Otrzymasz komunikat o błędzie podobny do następującego:

> *Nie można usunąć obiektu "dbo. Przez instruktorów ", ponieważ jest ona przywoływana przez ograniczenie klucza OBCEGO.*


Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp `Up` metoda następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Ten kod zajmuje się następujące zadania aktualizacji bazy danych:

- Usuwa ograniczenia klucza obcego i indeksy, które wskazują na Tabela Student.
- Zmienia nazwę tabeli przez instruktorów jako osoba i sprawia, że zmiany wymagane w celu przechowywania danych dla uczniów:

    - Dodaje EnrollmentDate dopuszczającego wartość null dla uczniów lub studentów.
    - Dodaje kolumnę dyskryminator, aby wskazać, czy wiersz jest dla uczniów lub pod kierunkiem instruktora.
    - Sprawia, że DataZatrudnienia dopuszczającego wartość null, ponieważ wiersze dla uczniów nie będzie mieć dat.
    - Dodaje tymczasowe pola, które będzie można używać do aktualizowania kluczy obcych, które wskazują dla uczniów i studentów. Podczas kopiowania studentów do tabeli osoby otrzymują nowe wartości klucza podstawowego.
- Kopiuje dane z tabeli uczniów do tabeli osoby. Powoduje to uczniom zostaną przypisane nowe wartości klucza podstawowego.
- Rozwiązuje wartości klucza obcego, które wskazują dla uczniów i studentów.
- Ponownie tworzy ograniczenia klucza obcego i indeksy, teraz wskazaniem tabeli osób.

(Identyfikator GUID gdyby użyto zamiast liczby całkowitej jako typ klucza podstawowego, nie trzeba zmieniać wartości klucza podstawowego dla uczniów i niektóre z tych kroków można została pominięta.)

Uruchom `update-database` ponownie polecenie.

(W systemie produkcyjnym użytkownik może wprowadzić odpowiednie zmiany do metody w dół w przypadku, gdy nigdy nie było go użyć, aby wrócić do poprzedniej wersji bazy danych. Na potrzeby tego samouczka możesz nie będzie używany metody w dół.)

> [!NOTE]
> Istnieje możliwość uzyskać inne błędy, podczas migracji danych i wprowadzania zmian schematu. Jeśli występują błędy migracji nie można rozwiązać, możesz kontynuować samouczek, zmieniając parametry połączenia w *Web.config* pliku lub przez usunięcie bazy danych. Najprostszą metodą jest zmiana nazwy bazy danych w *Web.config* pliku. Na przykład zmienić nazwę bazy danych do ContosoUniversity2, jak pokazano w poniższym przykładzie:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Za pomocą nowej bazy danych, nie ma żadnych danych, aby przeprowadzić migrację oraz `update-database` polecenia jest znacznie bardziej prawdopodobne zakończyć bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania z bazy danych, zobacz [jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). W przypadku zastosowania tego podejścia w celu przejdź do samouczka pominąć krok wdrażania na końcu tego samouczka lub wdrożyć nowej lokacji i bazy danych. Jeśli Wdróż aktualizację w tej samej lokacji, które możesz już został wdrożenie już EF zostanie wyświetlony ten sam błąd, gdy migracja jest uruchamiany automatycznie. Jeśli chcesz rozwiązać błąd migracji, zostanie najlepszy zasób jest jednym z forów platformy Entity Framework lub StackOverflow.com.


## <a name="testing"></a>Testowanie

Uruchamianie witryny, a następnie spróbuj różnych stronach. Wszystko działa to tak jak poprzednio.

W **Eksploratora serwera** rozwiń **Connections\SchoolContext danych** i następnie **tabel**, i zobaczysz, że **uczniów** i **Przez instruktorów** tabele zostały zastąpione przez **osoby** tabeli. Rozwiń **osoby** tabeli i zawiera wszystkie kolumny, które wcześniej w **uczniów** i **przez instruktorów** tabel.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** się kolumna dyskryminatora.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Na poniższym diagramie przedstawiono strukturę nową bazę danych School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tej sekcji, musisz ukończyć opcjonalną **wdrażania aplikacji na platformie Azure** sekcji [część 3, sortowanie, filtrowanie i stronicowanie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) z tej serii samouczka. Jeśli użytkownik ma błędy migracji, które rozwiązano problem, usuwając bazy danych w projekcie lokalnym, Pomiń ten krok; Tworzenie nowej lokacji i bazy danych lub wdrożenia do nowego środowiska.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj** z menu kontekstowego.  
  
    ![Opublikuj w menu kontekstowego projektu](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Kliknij przycisk **publikowania**.  
  
    ![Publikowanie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   Aplikacja sieci Web zostanie otwarta w domyślnej przeglądarce.
3. Testowanie aplikacji w celu zweryfikowania działa.

    Uruchom stronę po raz pierwszy uzyskuje dostęp do bazy danych, platformy Entity Framework umożliwia uruchamianie wszystkich migracjach `Up` metod wymaganych do przełączenia bazy danych na bieżąco z bieżącym modelem danych.

## <a name="summary"></a>Podsumowanie

Tabela wg hierarchii dziedziczenia dla zastało zaimplementowane `Person`, `Student`, i `Instructor` klasy. Aby uzyskać więcej informacji na temat tego i innych struktur dziedziczenia, zobacz [wzorzec dziedziczenia TPT](https://msdn.microsoft.com/data/jj618293) i [wzorzec dziedziczenia TPH](https://msdn.microsoft.com/data/jj618292) w witrynie MSDN. W następnym samouczku pokazano, jak do obsługi różnorodnych stosunkowo zaawansowane scenariusze platformy Entity Framework.

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
