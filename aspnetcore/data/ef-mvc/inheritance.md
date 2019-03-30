---
title: 'Samouczek: Implementowanie dziedziczenia — ASP.NET MVC z programem EF Core'
description: Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych przy użyciu platformy Entity Framework Core w aplikacji ASP.NET Core.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/inheritance
ms.openlocfilehash: ba688b259206f21045376b6a38a266ad7528aec1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750765"
---
# <a name="tutorial-implement-inheritance---aspnet-mvc-with-ef-core"></a>Samouczek: Implementowanie dziedziczenia — ASP.NET MVC z programem EF Core

W poprzednim samouczku obsługiwane są wyjątki współbieżności. Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.

W programowanie zorientowane obiektowo, można użyć dziedziczenia ułatwiające ponowne wykorzystanie kodu. W tym samouczku poznasz, jak zmienić `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowej klasy, która zawiera właściwości, takie jak `LastName` , które są wspólne dla instruktorów i studentów. Nie będzie dodać lub zmienić dowolnymi stronami sieci web, ale zmienisz część kodu, a te zmiany są automatycznie odzwierciedlane w bazie danych.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Mapowanie dziedziczenia do bazy danych
> * Utwórz klasę osoby
> * Aktualizacja przez instruktorów i uczniów
> * Dodanie osoby do modelu
> * Tworzenie i aktualizowanie migracji
> * Testowanie wdrożenia

## <a name="prerequisites"></a>Wymagania wstępne

* [Obsługa współbieżności](concurrency.md)

## <a name="map-inheritance-to-database"></a>Mapowanie dziedziczenia do bazy danych

`Instructor` i `Student` klasami w modelu danych School ma kilka właściwości, które są identyczne:

![Klasy dla uczniów i przez instruktorów](inheritance/_static/no-inheritance.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek. Lub aby pisać usługi, które umożliwia formatowanie nazwy bez opiekowanie, czy nazwa pochodzi od pod kierunkiem instruktora lub studenta. Można utworzyć `Person` podstawowa klasa, która zawiera tylko te właściwości udostępnionego, a następnie wprowadź `Instructor` i `Student` klasy dziedziczą z tej klasy bazowej, jak pokazano na poniższej ilustracji:

![Dla uczniów i instruktora klasy wywodzące się z osobą klasy](inheritance/_static/inheritance.png)

Istnieje kilka sposobów, w których ta struktura dziedziczenia, mogą być reprezentowane w bazie danych. Może mieć tabelę osoby, która zawiera informacje na temat uczniów i Instruktorzy w jednej tabeli. Niektóre kolumny mogą dotyczyć tylko Instruktorzy (DataZatrudnienia), niektóre tylko dla uczniów i studentów (EnrollmentDate), niektóre zarówno (LastName, FirstName). Zazwyczaj trzeba kolumna dyskryminatora, aby wskazać typ, który każdy wiersz reprezentuje. Na przykład kolumna dyskryminatora może być "Instruktora" instruktorów i "Student" dla uczniów lub studentów.

![Przykład tabela wg hierarchii](inheritance/_static/tph.png)

Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany Tabela wg hierarchii (TPH) dziedziczenia.

Alternatywą jest, aby wyglądała bardziej jak struktury dziedziczenia bazę danych. Na przykład może zawierać tylko pola nazwy tabeli osób i mają oddzielnych tabelach przez instruktorów i uczniów za pomocą pola daty.

![Tabela wg typu dziedziczenia](inheritance/_static/tpt.png)

Ten wzorzec polegający na wprowadzaniu tabeli bazy danych dla każdej klasy jednostki nosi nazwę tabeli na dziedziczenie typu (TPT).

Innym rozwiązaniem jest jeszcze mapowania wszystkich typów nieabstrakcyjnej poszczególnych tabel. Wszystkie właściwości klasy, w tym właściwości dziedziczonych są mapowane na kolumny odpowiedniej tabeli. Ten wzorzec jest nazywany dziedziczenia klas tabeli na konkretny (TPC). Jeśli zaimplementowano TPC dziedziczenia klas osoby, studentów i przez instruktorów, jak pokazano wcześniej, tabel, studentów i przez instruktorów wygląda nie różni się po zaimplementowaniu dziedziczenia, niż zajmowały przed.

Wzorców dziedziczenia TPC i TPH ogólnie dostarczyć lepszą wydajność niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań.

Ten samouczek demonstruje sposób implementacji TPH dziedziczenia. TPH jest wzorzec tylko dziedziczenie, który obsługuje Entity Framework Core.  Co należy to zrobić, jest utworzenie `Person` klasy, zmienić `Instructor` i `Student` pochodzi od klasy `Person`, Dodaj nową klasę do `DbContext`i Utwórz migracji.

> [!TIP]
> Warto zapisać kopię projektu przed dokonaniem następujących zmian.  Następnie w razie problemów i musisz zacząć od początku, będzie można łatwiej uruchomić z zapisany projekt zamiast cofania kroki wykonywane w ramach tego samouczka lub przejdź do początku całej serii.

## <a name="create-the-person-class"></a>Utwórz klasę osoby

W folderze modele osoba.cs tworzenie i Zastąp kod szablonu poniższym kodem:

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="update-instructor-and-student"></a>Aktualizacja przez instruktorów i uczniów

W *Instructor.cs*dziedziczyć klasy instruktora klasy osoby i usunąć klucza i nazwy pola. Ten kod będzie wyglądać następująco:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Wprowadzenie identycznych zmian w *Student.cs*.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-person-to-the-model"></a>Dodanie osoby do modelu

Dodaj typ jednostki osoby do *SchoolContext.cs*. Nowe wiersze są wyróżnione.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

To wszystko, co wymaga programu Entity Framework, w celu skonfigurowania Tabela wg hierarchii dziedziczenia. Jak można zauważyć, gdy baza danych zostanie zaktualizowany, będzie miał tabeli osób, zamiast tabel dla uczniów i instruktora.

## <a name="create-and-update-migrations"></a>Tworzenie i aktualizowanie migracji

Zapisz zmiany i skompiluj projekt. Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenie:

```console
dotnet ef migrations add Inheritance
```

Nie uruchamiaj `database update` jeszcze polecenia. To polecenie spowoduje utratę danych, ponieważ będzie on porzucić tę tabelę przez instruktorów i Zmień nazwę tabeli uczniów do osoby. Należy podać kod niestandardowy, aby zachować istniejące dane.

Otwórz *migracje /\<sygnatura czasowa > _Inheritance.cs* i Zastąp `Up` metoda następującym kodem:

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Ten kod zajmuje się następujące zadania aktualizacji bazy danych:

* Usuwa ograniczenia klucza obcego i indeksy, które wskazują na Tabela Student.

* Zmienia nazwę tabeli przez instruktorów jako osoba i sprawia, że zmiany wymagane w celu przechowywania danych dla uczniów:

* Dodaje EnrollmentDate dopuszczającego wartość null dla uczniów lub studentów.

* Dodaje kolumnę dyskryminator, aby wskazać, czy wiersz jest dla uczniów lub pod kierunkiem instruktora.

* Sprawia, że DataZatrudnienia dopuszczającego wartość null, ponieważ wiersze dla uczniów nie będzie mieć dat.

* Dodaje tymczasowe pola, które będzie można używać do aktualizowania kluczy obcych, które wskazują dla uczniów i studentów. Podczas kopiowania studentów do tabeli osoba otrzyma nowe wartości klucza podstawowego.

* Kopiuje dane z tabeli uczniów do tabeli osoby. Powoduje to uczniom zostaną przypisane nowe wartości klucza podstawowego.

* Rozwiązuje wartości klucza obcego, które wskazują dla uczniów i studentów.

* Ponownie tworzy ograniczenia klucza obcego i indeksy, teraz wskazaniem tabeli osób.

(Identyfikator GUID gdyby użyto zamiast liczby całkowitej jako typ klucza podstawowego, nie trzeba zmieniać wartości klucza podstawowego dla uczniów i niektóre z tych kroków można została pominięta.)

Uruchom `database update` polecenia:

```console
dotnet ef database update
```

(W systemie produkcyjnym będzie wprowadzić odpowiednie zmiany do `Down` metody w przypadku nigdy nie było go użyć, aby wrócić do poprzedniej wersji bazy danych. W tym samouczku nie będzie używany `Down` metody.)

> [!NOTE]
> Istnieje możliwość uzyskać inne błędy, podczas wprowadzania zmian schematu w bazie danych, która ma istniejące dane. Występują błędy migracji, których nie można rozwiązać, można zmienić nazwy bazy danych w parametrach połączenia lub Usuń bazę danych. Za pomocą nowej bazy danych nie ma żadnych danych do migracji, a polecenie update-database jest bardziej prawdopodobne zakończyć bez błędów. Aby usunąć bazy danych, użyj SSOX, lub uruchom `database drop` interfejsu wiersza polecenia.

## <a name="test-the-implementation"></a>Testowanie wdrożenia

Uruchom aplikację i spróbuj różnych stronach. Wszystko działa to tak jak poprzednio.

W **Eksplorator obiektów SQL Server**, rozwiń węzeł **danych połączeń/SchoolContext** i następnie **tabel**, zobaczysz zastępuje tabel dla uczniów i przez instruktorów Tabela osoby. Otwórz projektanta tabel osoby i możesz zobaczyć, że ma wszystkie kolumny, które kiedyś w tabelach studenta i programistę-instruktora.

![Tabela osoby w SSOX](inheritance/_static/ssox-person-table.png)

Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** się kolumna dyskryminatora.

![Tabela osoby w SSOX - tabeli danych](inheritance/_static/ssox-person-data.png)

## <a name="get-the-code"></a>Pobierz kod

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji dotyczących dziedziczenia w Entity Framework Core, zobacz [dziedziczenia](/ef/core/modeling/inheritance).

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dziedziczenie mapowanego do bazy danych
> * Utworzone klasy osoby
> * Zaktualizowane przez instruktorów i uczniów
> * Dodano osoby do modelu
> * Utworzone i zaktualizuj migracji
> * Przetestowane wdrożenia

Przejdź do następnego samouczka, aby dowiedzieć się, jak obsługiwać różnych stosunkowo zaawansowane scenariusze platformy Entity Framework.

> [!div class="nextstepaction"]
> [Dalej: Tematy zaawansowane](advanced.md)
