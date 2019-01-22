---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Szablon: Implementowanie dziedziczenia z programów EF w aplikacjach ASP.NET MVC 5'
description: Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: df8715e4416ce3ccdf1d9e380addcded553d85f8
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444288"
---
# <a name="template-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a>Szablon: Implementowanie dziedziczenia z programów EF w aplikacji ASP.NET MVC 5

W poprzednim samouczku obsługiwane są wyjątki współbieżności. Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.

Programowanie zorientowane obiektowo umożliwia [dziedziczenia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) ułatwiające [ponowne użycie kodu](http://en.wikipedia.org/wiki/Code_reuse). W tym samouczku poznasz, jak zmienić `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowej klasy, która zawiera właściwości, takie jak `LastName` , które są wspólne dla instruktorów i studentów. Nie będzie dodać lub zmienić dowolnymi stronami sieci web, ale zmienisz część kodu, a te zmiany są automatycznie odzwierciedlane w bazie danych.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dowiedz się, jak mapować dziedziczenia do bazy danych
> * Utwórz klasę osoby
> * Aktualizacja przez instruktorów i uczniów
> * Dodanie osoby do modelu
> * Tworzenie i aktualizowanie migracji
> * Testowanie wdrożenia
> * Wdrażanie na platformie Azure

## <a name="prerequisites"></a>Wymagania wstępne

* [Implementowanie dziedziczenia](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a>Mapowanie dziedziczenia do bazy danych

`Instructor` i `Student` klas w `School` model danych ma kilka właściwości, które są identyczne:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek. Lub aby pisać usługi, które umożliwia formatowanie nazwy bez opiekowanie, czy nazwa pochodzi od pod kierunkiem instruktora lub studenta. Można utworzyć `Person` klasy, która zawiera tylko te właściwości udostępnionego podstawowej, a następnie wprowadź `Instructor` i `Student` jednostek dziedziczą z tej klasy bazowej, jak pokazano na poniższej ilustracji:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Istnieje kilka sposobów, w których ta struktura dziedziczenia, mogą być reprezentowane w bazie danych. Może mieć `Person` tabelę, która zawiera informacje na temat uczniów i Instruktorzy w jednej tabeli. Niektóre kolumny można zastosować tylko do Instruktorzy (`HireDate`), niektóre tylko dla uczniów i studentów (`EnrollmentDate`), niektóre na wartość oba (`LastName`, `FirstName`). Zazwyczaj trzeba *dyskryminatora* kolumny, aby wskazać, jakiego typu każdy wiersz reprezentuje. Na przykład kolumna dyskryminatora może być "Instruktora" instruktorów i "Student" dla uczniów lub studentów.

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany *Tabela wg hierarchii* dziedziczenia (TPH).

Alternatywą jest, aby wyglądała bardziej jak struktury dziedziczenia bazę danych. Na przykład można mieć tylko pola nazwy `Person` tabeli i mieć osobne `Instructor` i `Student` tabele z polami daty.

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ten wzorzec polegający na wprowadzaniu tabeli bazy danych dla każdej klasie jednostki jest wywoływana *tabeli na typ* dziedziczenia (TPT).

Innym rozwiązaniem jest jeszcze mapowania wszystkich typów nieabstrakcyjnej poszczególnych tabel. Wszystkie właściwości klasy, w tym właściwości dziedziczonych są mapowane na kolumny odpowiedniej tabeli. Ten wzorzec jest nazywany dziedziczenia klas tabeli na konkretny (TPC). Jeśli zaimplementowano dziedziczenie TPC `Person`, `Student`, i `Instructor` klasy, jak pokazano wcześniej, `Student` i `Instructor` tabel będzie wyglądać nie różni się po zaimplementowaniu dziedziczenia, niż zajmowały przed.

TPC i wzorców dziedziczenia TPH ogólnie dostarczyć, lepszej wydajności platformy Entity Framework niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań.

Ten samouczek demonstruje sposób implementacji TPH dziedziczenia. TPH jest domyślny wzorzec dziedziczenia platformy Entity Framework, więc trzeba wystarczy utworzyć `Person` klasy, zmienić `Instructor` i `Student` pochodzi od klasy `Person`, Dodaj nową klasę do `DbContext`i Utwórz Migracja. (Uzyskać informacji o sposobie wdrażania z innymi wzorami dziedziczenia, zobacz [mapowanie dziedziczenia tabela wg typu (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) i [mapowanie dziedziczenia klasy tabeli na konkretny (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) w witrynie MSDN Dokumentacja programu Entity Framework.)

## <a name="create-the-person-class"></a>Utwórz klasę osoby

W *modeli* folderze utwórz *osoba.cs* i Zastąp kod szablonu poniższym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a>Aktualizacja przez instruktorów i uczniów

Teraz zaktualizować *Instructor.cs* i *Sudent.cs* dziedziczy wartości z *Person.sc*.

W *Instructor.cs*, pochodzi `Instructor` klasy z `Person` klasy, a następnie usuń klucza i nazwy pola. Ten kod będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Wprowadzić zmiany podobne do *Student.cs*. `Student` Klasy będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a>Dodanie osoby do modelu

W *SchoolContext.cs*, Dodaj `DbSet` właściwość `Person` typ jednostki:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To wszystko, co wymaga programu Entity Framework, w celu skonfigurowania Tabela wg hierarchii dziedziczenia. Jak można zauważyć, gdy baza danych zostanie zaktualizowany, będzie miał `Person` tabeli zamiast `Student` i `Instructor` tabel.

## <a name="create-and-update-migrations"></a>Tworzenie i aktualizowanie migracji

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

## <a name="test-the-implementation"></a>Testowanie wdrożenia

Uruchamianie witryny, a następnie spróbuj różnych stronach. Wszystko działa to tak jak poprzednio.

W **Eksploratora serwera** rozwiń **Connections\SchoolContext danych** i następnie **tabel**, i zobaczysz, że **uczniów** i **Przez instruktorów** tabele zostały zastąpione przez **osoby** tabeli. Rozwiń **osoby** tabeli i zawiera wszystkie kolumny, które wcześniej w **uczniów** i **przez instruktorów** tabel.

Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** się kolumna dyskryminatora.

Na poniższym diagramie przedstawiono strukturę nową bazę danych School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tej sekcji, musisz ukończyć opcjonalną **wdrażania aplikacji na platformie Azure** sekcji [część 3, sortowanie, filtrowanie i stronicowanie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) z tej serii samouczka. Jeśli użytkownik ma błędy migracji, które rozwiązano problem, usuwając bazy danych w projekcie lokalnym, Pomiń ten krok; Tworzenie nowej lokacji i bazy danych lub wdrożenia do nowego środowiska.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj** z menu kontekstowego.

2. Kliknij przycisk **publikowania**.

    Aplikacja sieci Web zostanie otwarta w domyślnej przeglądarce.

3. Testowanie aplikacji w celu zweryfikowania działa.

    Uruchom stronę po raz pierwszy uzyskuje dostęp do bazy danych, platformy Entity Framework umożliwia uruchamianie wszystkich migracjach `Up` metod wymaganych do przełączenia bazy danych na bieżąco z bieżącym modelem danych.

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

Aby uzyskać więcej informacji na temat tego i innych struktur dziedziczenia, zobacz [wzorzec dziedziczenia TPT](https://msdn.microsoft.com/data/jj618293) i [wzorzec dziedziczenia TPH](https://msdn.microsoft.com/data/jj618292) w witrynie MSDN. W następnym samouczku pokazano, jak do obsługi różnorodnych stosunkowo zaawansowane scenariusze platformy Entity Framework.

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Przedstawiono do mapowania dziedziczenia bazy danych
> * Utworzone klasy osoby
> * Zaktualizowane przez instruktorów i uczniów
> * Dodano osoby do modelu
> * Utworzone i zaktualizuj migracji
> * Przetestowane wdrożenia
> * Wdrażane na platformie Azure

Przejdź do następnego artykułu, aby dowiedzieć się więcej na temat tematów, które są przydatne pod uwagę podczas czegoś podstawy tworzenia aplikacji sieci web ASP.NET, które używają programu Entity Framework Code First.
> [!div class="nextstepaction"]
> [Zaawansowane scenariusze platformy Entity Framework](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)