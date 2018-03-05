---
title: Platformy ASP.NET Core MVC podstawowych EF - dziedziczenia - 9, 10
author: tdykstra
description: "Ten samouczek przedstawia sposób implementacji dziedziczenia w modelu danych, przy użyciu programu Entity Framework Core w aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 55221846422def25452bc148b68573a02299bbfe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>Dziedziczenie - Core EF z samouczek platformy ASP.NET Core MVC (9, 10)

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

Samouczek poprzedniej służy do obsługi wyjątków współbieżności. Ten samouczek przedstawia sposób wykonania dziedziczenia w modelu danych.

W programowanie zorientowane obiektowo można użyć dziedziczenia, aby ułatwić ponowne użycie kodu. W tym samouczku zostanie zmieniona `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowa klasa, która zawiera właściwości, takie jak `LastName` które są wspólne dla uczniów lub studentów i instruktorów. Nie dodać lub zmienić żadnych stron sieci web, ale zostanie zmieniona części kodu i te zmiany są automatycznie odzwierciedlane w bazie danych.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opcje mapowania dziedziczenia w tabelach bazy danych

`Instructor` i `Student` klasy w szkole modelu danych mają kilka właściwości, które są identyczne:

![Dla użytkowników domowych i instruktora klas](inheritance/_static/no-inheritance.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są udostępniane przez `Instructor` i `Student` jednostek. Lub chcesz zapisać to usługa, która może sformatować nazwy bez opiekowanie, czy nazwa pochodzi od instruktora lub studenta. Można utworzyć `Person` podstawowa klasa, która zawiera tylko te właściwości udostępnionego, a następnie wprowadź `Instructor` i `Student` klasy dziedziczą z tej klasy podstawowej, jak pokazano na poniższej ilustracji:

![Dla użytkowników domowych i instruktora klasy wywodzące się z osobą klasy](inheritance/_static/inheritance.png)

Istnieje kilka metod, które ta struktura dziedziczenia może być reprezentowane w bazie danych. Może mieć tabeli osoby, która zawiera informacje o studentów i instruktorów w jednej tabeli. Niektóre kolumny można zastosować tylko do instruktorów (DataZatrudnienia), niektórych tylko dla uczniów lub studentów (EnrollmentDate), niektórych zarówno (LastName, FirstName). Zazwyczaj trzeba kolumna dyskryminatora, aby wskazać, który typ reprezentuje każdego wiersza. Na przykład kolumny rozróżniacza mogą mieć "Instruktora" dla szkolenia i "Uczniów" dla uczniów lub studentów.

![Przykład tabeli na hierarchii](inheritance/_static/tph.png)

Ten wzorzec generowania strukturę jednostek dziedziczenia z tabeli pojedynczej bazy danych jest nazywana tabeli na hierarchii dziedziczenia (TPH).

Alternatywą jest wyglądu struktury dziedziczenia bazę danych. Na przykład może zawierać tylko pola Nazwa tabeli osób i mieć osobne instruktora i uczniów tabele z polami daty.

![Dziedziczenie tabeli według typu](inheritance/_static/tpt.png)

Ten wzorzec dokonywania tabeli bazy danych dla każdej klasy jednostki nosi nazwę tabeli na dziedziczenia typu (TPT).

Jeszcze inną możliwością jest wykonanie mapowania wszystkich typów nieabstrakcyjnej poszczególnych tabel. Wszystkie właściwości klasy, w tym właściwości dziedziczone są mapowane na kolumny tej tabeli. Ten wzorzec jest nazywany dziedziczenia tabeli na konkretnych klas (TPC). Jeśli zaimplementowano TPC dziedziczenia klas osoby, dla użytkowników domowych i instruktora jak pokazano wcześniej, tabele dla użytkowników domowych i instruktora będzie wyglądać nie różni się po wdrożeniu dziedziczenia, niż zajmowały przed.

Wzorce dziedziczenia TPC i TPH zazwyczaj zapewniają lepszą wydajność niż w przypadku wzorców dziedziczenia TPT, wzorce TPT może powodować sprzężenia złożonych zapytań.

Ten samouczek pokazuje, jak zaimplementować dziedziczenia TPH. TPH to wzorzec tylko dziedziczenia, który obsługuje programu Entity Framework Core.  Co należy zrobić to utworzyć `Person` klasy, zmień `Instructor` i `Student` pochodzi od klasy `Person`, Dodaj nową klasę do `DbContext`i Utwórz migracji.

> [!TIP] 
> Warto zapisać kopię projektu przed wprowadzeniem następujące zmiany.  Następnie w razie problemów i należy zacząć od początku, będzie łatwiejsze do uruchamiania z zapisanym projektu zamiast cofania czynności wykonywane w ramach tego samouczka lub będzie powrót do początku całej serii.

## <a name="create-the-person-class"></a>Utwórz klasę osoby

W folderze modeli Utwórz Person.cs i Zastąp kod szablonu z następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Utworzyć klasy dla użytkowników domowych i instruktora dziedziczyć osoby

W *Instructor.cs*, pochodzi z klasy instruktora od klasy osoby i usuń pola klucza i nazwę. Ten kod będzie wyglądać następująco:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Wprowadzenie identycznych zmian w *Student.cs*.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Dodaj typ jednostki osoby do modelu danych

Dodaj typ jednostki osoby do *SchoolContext.cs*. Nowe wiersze są wyróżnione.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

To wszystko, który programu Entity Framework wymaga, aby skonfigurować tabelę dla hierarchii dziedziczenia. Jak można zauważyć, gdy baza danych jest aktualizowana, ma zamiast tabel dla użytkowników domowych i instruktora tabeli osoby.

## <a name="create-and-customize-migration-code"></a>Tworzyć i dostosowywać kod migracji

Zapisz zmiany i skompilować projekt. Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenie:

```console
dotnet ef migrations add Inheritance
```

Nie uruchamiaj `database update` jeszcze polecenia. Tego polecenia zostanie spowodować utratę danych, ponieważ będzie porzucić tę tabelę instruktora i nazwy tabeli uczniów do osoby. Musisz podać kodu niestandardowego, aby zachować istniejące dane.

Otwórz *migracje /\<sygnatury czasowej > _Inheritance.cs* i Zastąp `Up` metodę z następującym kodem:

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Ten kod zajmuje się następujące zadania aktualizacji bazy danych:

* Usuwa ograniczeń klucza obcego i indeksów, które wskazują tabeli uczniów.

* Zmienia nazwę tabeli instruktora osoby i wprowadza zmiany w przypadku przechowywania danych dla użytkowników domowych:

* Dodaje EnrollmentDate wartości null dla uczniów lub studentów.

* Dodaje kolumnę dyskryminator wskazująca, czy wiersz jest dla uczniów lub instruktora.

* Sprawia, że DataZatrudnienia nullable ponieważ wierszy uczniów nie będziesz mieć dat.

* Dodaje tymczasowego pola, które będą służyć do aktualizowania kluczy obcych, które wskazują studentów. Podczas kopiowania studentów w tabeli osób otrzyma nowe wartości klucza podstawowego.

* Kopiuje dane z tabeli uczniów do tabeli osoby. Powoduje to, że można zostaną przypisane nowe wartości klucza podstawowego.

* Rozwiązuje wartości kluczy obcych, które wskazują studentów.

* Ponownie tworzy ograniczeń klucza obcego i indeksów, teraz wskazaniem tabeli osób.

(W przypadku identyfikatora GUID zamiast liczby całkowitej jako typu klucza podstawowego, nie trzeba zmieniać wartości klucza podstawowego dla użytkowników domowych i niektóre z tych kroków można została pominięta.)

Uruchom `database update` polecenia:

```console
dotnet ef database update
```

(W systemie produkcji może wprowadzić odpowiednie zmiany do `Down` metody w przypadku kiedykolwiek musiał używać, aby wrócić do poprzedniej wersji bazy danych. W tym samouczku nie będzie używany `Down` metody.)

> [!NOTE] 
> Istnieje możliwość pobrania inne błędy podczas wprowadzania zmian schematu w bazie danych istniejących danych. Jeśli występują błędy migracji, których nie można rozwiązać, można zmienić nazwę bazy danych w parametrach połączenia lub Usuń bazę danych. Z nową bazę danych nie ma żadnych danych do migracji, a polecenia update-database jest bardziej prawdopodobne zakończyć bez błędów. Aby usunąć bazę danych, użyj SSOX lub uruchom `database drop` polecenia interfejsu wiersza polecenia.

## <a name="test-with-inheritance-implemented"></a>Test z dziedziczenia zaimplementowany

Uruchom aplikację i spróbuj różnych stron. Wszystko, co działa tak samo, tak jak poprzednio.

W **Eksplorator obiektów SQL Server**, rozwiń węzeł **danych połączenia/SchoolContext** , a następnie **tabel**, i zastępuje tabel dla użytkowników domowych i instruktora Tabela osoby. Otwórz projektanta tabel osoby i zobacz, czy zawiera wszystkie kolumny, które są używane w tabelach dla użytkowników domowych i instruktora.

![Osoba tabeli w SSOX](inheritance/_static/ssox-person-table.png)

Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** wyświetlić kolumny rozróżniacza.

![Tabela osoby w SSOX - tabeli danych](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Podsumowanie

Zostały zaimplementowane tabeli na hierarchii dziedziczenia dla `Person`, `Student`, i `Instructor` klasy. Aby uzyskać więcej informacji na temat dziedziczenia w Entity Framework Core, zobacz [dziedziczenia](https://docs.microsoft.com/ef/core/modeling/inheritance). W następnym samouczku zobaczysz sposób obsługi różnych stosunkowo zaawansowanych scenariuszy programu Entity Framework.

>[!div class="step-by-step"]
[Poprzednie](concurrency.md)
[dalej](advanced.md)  
