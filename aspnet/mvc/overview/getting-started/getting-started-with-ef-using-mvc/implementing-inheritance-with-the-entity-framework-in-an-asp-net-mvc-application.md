---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Wdrażanie dziedziczenia z programu Entity Framework 6 w aplikacji platformy ASP.NET MVC 5 (11, 12) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1826659626106993d4796641492c62fcbd22a1b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Wdrażanie dziedziczenia z programu Entity Framework 6 w aplikacji platformy ASP.NET MVC 5 (11, 12)
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Samouczek poprzedniej służy do obsługi wyjątków współbieżności. Ten samouczek przedstawia sposób wykonania dziedziczenia w modelu danych.

Programowanie zorientowane obiektowo można [dziedziczenia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) ułatwiające [kodu ponownemu](http://en.wikipedia.org/wiki/Code_reuse). W tym samouczku zostanie zmieniona `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowa klasa, która zawiera właściwości, takie jak `LastName` które są wspólne dla uczniów lub studentów i instruktorów. Nie dodać lub zmienić żadnych stron sieci web, ale zostanie zmieniona części kodu i te zmiany są automatycznie odzwierciedlane w bazie danych.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opcje mapowania dziedziczenia w tabelach bazy danych

`Instructor` i `Student` klas w `School` model danych ma kilka właściwości, które są identyczne:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są udostępniane przez `Instructor` i `Student` jednostek. Lub chcesz zapisać to usługa, która może sformatować nazwy bez opiekowanie, czy nazwa pochodzi od instruktora lub studenta. Można utworzyć `Person` podstawowa klasa, która zawiera tylko te właściwości udostępnionego, a następnie wprowadź `Instructor` i `Student` jednostek dziedziczą z tej klasy podstawowej, jak pokazano na poniższej ilustracji:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Istnieje kilka metod, które ta struktura dziedziczenia może być reprezentowane w bazie danych. Może mieć `Person` tabeli, która zawiera informacje o studentów i instruktorów w jednej tabeli. Niektóre kolumny można zastosować tylko do instruktorów (`HireDate`), niektórych tylko dla uczniów lub studentów (`EnrollmentDate`), zarówno niektóre do (`LastName`, `FirstName`). Zazwyczaj konieczne będzie *rozróżniacza* reprezentuje kolumny, która wskazuje typ każdego wiersza. Na przykład kolumny rozróżniacza mogą mieć "Instruktora" dla szkolenia i "Uczniów" dla uczniów lub studentów.

![Tabela na hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ten wzorzec generowania strukturę jednostek dziedziczenia z tabeli pojedynczej bazy danych jest nazywany *tabeli na hierarchii* dziedziczenia (TPH).

Alternatywą jest wyglądu struktury dziedziczenia bazę danych. Na przykład można mieć tylko pola nazwy `Person` tabeli i mieć osobne `Instructor` i `Student` tabele z polami daty.

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ten wzorzec dokonywania tabeli bazy danych w każdej klasie jednostki jest nazywana *tabel na typ* dziedziczenia (TPT).

Jeszcze inną możliwością jest wykonanie mapowania wszystkich typów nieabstrakcyjnej poszczególnych tabel. Wszystkie właściwości klasy, w tym właściwości dziedziczone są mapowane na kolumny tej tabeli. Ten wzorzec jest nazywany dziedziczenia tabeli na konkretnych klas (TPC). Jeśli zaimplementowano dziedziczenie TPC `Person`, `Student`, i `Instructor` klasy, jak pokazano wcześniej, `Student` i `Instructor` tabel będzie wyglądać nie różni się od wdrażanie dziedziczenia, niż zajmowały przed.

TPC i wzorce dziedziczenia TPH zazwyczaj dostarczania lepszą wydajność programu Entity Framework niż w przypadku wzorców dziedziczenia TPT wzorce TPT może powodować sprzężenia złożonych zapytań.

Ten samouczek pokazuje, jak zaimplementować dziedziczenia TPH. TPH jest domyślny wzorzec dziedziczenia w ramach jednostki, tak aby wszystkie wystarczy utworzyć `Person` klasy, zmień `Instructor` i `Student` pochodzi od klasy `Person`, Dodaj nową klasę do `DbContext`i Utwórz Migracja. (Uzyskać informacji o sposobie wykonania innych wzorcach dziedziczenia, zobacz [mapowania dziedziczenia dla tabeli (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) i [mapowania dziedziczenia klasy tabeli na konkretnych (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) w witrynie MSDN Dokumentacja Framework jednostki.)

## <a name="create-the-person-class"></a>Utwórz klasę osoby

W *modele* folderu, Utwórz *Person.cs* i Zastąp kod szablonu z następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Utworzyć klasy dla użytkowników domowych i instruktora dziedziczyć osoby

W *Instructor.cs*, pochodzi `Instructor` klasę z `Person` klasy i usunąć pola klucza i nazwę. Ten kod będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Podobne zmiany do *Student.cs*. `Student` Klasa będzie wyglądać następująco:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Dodanie osoby typ jednostki do modelu

W *SchoolContext.cs*, Dodaj `DbSet` właściwość `Person` typ jednostki:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To wszystko, który programu Entity Framework wymaga, aby skonfigurować tabelę dla hierarchii dziedziczenia. Jak można zauważyć, gdy baza danych jest aktualizowana, będzie mieć `Person` tabeli zamiast `Student` i `Instructor` tabel.

## <a name="create-and-update-a-migrations-file"></a>Tworzenie i aktualizowanie plików migracji

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:

`Add-Migration Inheritance`

Uruchom `Update-Database` w kryterium. Polecenie będzie działać na tym etapie, ponieważ będziemy mieć istniejące dane migracji nie może ustalić sposób obsługi. Zostanie wyświetlony komunikat o błędzie podobny do następującego:

> *Nie można usunąć obiektu "dbo. Instruktora ", ponieważ odwołuje się do niego ograniczenie FOREIGN KEY.*


Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp `Up` metodę z następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Ten kod zajmuje się następujące zadania aktualizacji bazy danych:

- Usuwa ograniczeń klucza obcego i indeksów, które wskazują tabeli uczniów.
- Zmienia nazwę tabeli instruktora osoby i wprowadza zmiany w przypadku przechowywania danych dla użytkowników domowych:

    - Dodaje EnrollmentDate wartości null dla uczniów lub studentów.
    - Dodaje kolumnę dyskryminator wskazująca, czy wiersz jest dla uczniów lub instruktora.
    - Sprawia, że DataZatrudnienia nullable ponieważ wierszy uczniów nie będziesz mieć dat.
    - Dodaje tymczasowego pola, które będą służyć do aktualizowania kluczy obcych, które wskazują studentów. Podczas kopiowania studentów w tabeli osób otrzymują nowe wartości klucza podstawowego.
- Kopiuje dane z tabeli uczniów do tabeli osoby. Powoduje to, że można zostaną przypisane nowe wartości klucza podstawowego.
- Rozwiązuje wartości kluczy obcych, które wskazują studentów.
- Ponownie tworzy ograniczeń klucza obcego i indeksów, teraz wskazaniem tabeli osób.

(W przypadku identyfikatora GUID zamiast liczby całkowitej jako typu klucza podstawowego, nie trzeba zmieniać wartości klucza podstawowego dla użytkowników domowych i niektóre z tych kroków można została pominięta.)

Uruchom `update-database` polecenie ponownie.

(W systemie produkcji można będzie wprowadzić odpowiednie zmiany do metody w dół na wypadek, gdyby kiedykolwiek musiał używać, aby wrócić do poprzedniej wersji bazy danych. W tym samouczku użytkownik nie będzie używany metody w dół.)

> [!NOTE]
> Istnieje możliwość pobrania inne błędy podczas migracji danych i wprowadzania zmian schematu. Jeśli występują błędy migracji nie można rozwiązać, zmieniając w ciągu połączenia można kontynuować samouczka *Web.config* przez usunięcie bazy danych lub plik. Najprostsza metoda jest można zmienić nazwy bazy danych w *Web.config* pliku. Na przykład zmienić nazwę bazy danych na ContosoUniversity2, jak pokazano w poniższym przykładzie:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Z nową bazę danych, nie ma żadnych danych, aby przeprowadzić migrację oraz `update-database` polecenie jest znacznie bardziej prawdopodobne zakończyć bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania bazy danych, zobacz [jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). W przypadku zastosowania tego podejścia aby kontynuować samouczek pominąć krok wdrożenia na końcu tego samouczka lub wdrażania nowej lokacji i bazy danych. Po zainstalowaniu aktualizacji do tej samej lokacji, które już zostały wdrażana do już EF otrzyma ten sam błąd podczas automatycznego uruchamiania migracji. Jeśli chcesz rozwiązać problem migracje najlepsze zasobu jest jednym z fora programu Entity Framework lub StackOverflow.com.


## <a name="testing"></a>Testowanie

Uruchom witrynę i spróbuj różnych stron. Wszystko, co działa tak samo, tak jak poprzednio.

W **Eksploratora serwera** rozwiń **Connections\SchoolContext danych** , a następnie **tabel**, i zobaczysz, że **uczniowie** i **Instruktora** tabele zostały zastąpione przez **osoby** tabeli. Rozwiń węzeł **osoby** tabeli i zawiera wszystkie kolumny, które wcześniej w **uczniowie** i **instruktora** tabel.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** wyświetlić kolumny rozróżniacza.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Na poniższym diagramie przedstawiono struktury nowej bazy danych służbowych:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tej sekcji wymaga zakończył opcjonalny **wdrażania aplikacji na platformie Azure** sekcji [część 3, sortowanie, filtrowanie i stronicowania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) tego samouczka serii. Jeśli były migracje błędów, które można rozwiązać przez usunięcie bazy danych w projekcie lokalnego, Pomiń ten krok; lub Utwórz nową witrynę i bazy danych, a następnie wdrożyć do nowego środowiska.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **publikowania** z menu kontekstowego.  
  
    ![Opublikuj w menu kontekstowego projektu](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Kliknij przycisk **publikowania**.  
  
    ![Publikowanie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   Aplikacja sieci Web zostanie otwarty w domyślnej przeglądarce.
3. Testowanie aplikacji w celu weryfikacji działa.

    Przy pierwszym uruchomieniu strony uzyskuje dostęp do bazy danych, Entity Framework uruchamia wszystkie migracja `Up` metody wymagane do bazy danych na bieżąco z bieżącym modelem danych.

## <a name="summary"></a>Podsumowanie

Zostały zaimplementowane tabeli na hierarchii dziedziczenia dla `Person`, `Student`, i `Instructor` klasy. Aby uzyskać więcej informacji dotyczących tego i innych konstrukcji dziedziczenia, zobacz [wzorzec dziedziczenia TPT](https://msdn.microsoft.com/data/jj618293) i [wzorzec dziedziczenia TPH](https://msdn.microsoft.com/data/jj618292) w witrynie MSDN. W następnym samouczku zobaczysz sposób obsługi różnych stosunkowo zaawansowanych scenariuszy programu Entity Framework.

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
