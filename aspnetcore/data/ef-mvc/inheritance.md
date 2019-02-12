---
title: 'Samouczek: Implementowanie dziedziczenia — ASP.NET MVC z programem EF Core'
description: Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych przy użyciu platformy Entity Framework Core w aplikacji ASP.NET Core.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/05/2019
ms.topic: tutorial
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 0a5eb1aba43bc2adf746202772c7f98eff49b4ff
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56103010"
---
# <a name="tutorial-implement-inheritance---aspnet-mvc-with-ef-core"></a><span data-ttu-id="b2f75-103">Samouczek: Implementowanie dziedziczenia — ASP.NET MVC z programem EF Core</span><span class="sxs-lookup"><span data-stu-id="b2f75-103">Tutorial: Implement inheritance - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="b2f75-104">W poprzednim samouczku obsługiwane są wyjątki współbieżności.</span><span class="sxs-lookup"><span data-stu-id="b2f75-104">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="b2f75-105">Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="b2f75-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="b2f75-106">W programowanie zorientowane obiektowo, można użyć dziedziczenia ułatwiające ponowne wykorzystanie kodu.</span><span class="sxs-lookup"><span data-stu-id="b2f75-106">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="b2f75-107">W tym samouczku poznasz, jak zmienić `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowej klasy, która zawiera właściwości, takie jak `LastName` , które są wspólne dla instruktorów i studentów.</span><span class="sxs-lookup"><span data-stu-id="b2f75-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="b2f75-108">Nie będzie dodać lub zmienić dowolnymi stronami sieci web, ale zmienisz część kodu, a te zmiany są automatycznie odzwierciedlane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b2f75-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="b2f75-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="b2f75-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b2f75-110">Mapowanie dziedziczenia do bazy danych</span><span class="sxs-lookup"><span data-stu-id="b2f75-110">Map inheritance to database</span></span>
> * <span data-ttu-id="b2f75-111">Utwórz klasę osoby</span><span class="sxs-lookup"><span data-stu-id="b2f75-111">Create the Person class</span></span>
> * <span data-ttu-id="b2f75-112">Aktualizacja przez instruktorów i uczniów</span><span class="sxs-lookup"><span data-stu-id="b2f75-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="b2f75-113">Dodanie osoby do modelu</span><span class="sxs-lookup"><span data-stu-id="b2f75-113">Add Person to the model</span></span>
> * <span data-ttu-id="b2f75-114">Tworzenie i aktualizowanie migracji</span><span class="sxs-lookup"><span data-stu-id="b2f75-114">Create and update migrations</span></span>
> * <span data-ttu-id="b2f75-115">Testowanie wdrożenia</span><span class="sxs-lookup"><span data-stu-id="b2f75-115">Test the implementation</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2f75-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b2f75-116">Prerequisites</span></span>

* [<span data-ttu-id="b2f75-117">Obsługa współbieżności przy użyciu programu EF Core w aplikacji internetowej ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b2f75-117">Handle Concurrency with EF Core in an ASP.NET Core MVC web app</span></span>](concurrency.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="b2f75-118">Mapowanie dziedziczenia do bazy danych</span><span class="sxs-lookup"><span data-stu-id="b2f75-118">Map inheritance to database</span></span>

<span data-ttu-id="b2f75-119">`Instructor` i `Student` klasami w modelu danych School ma kilka właściwości, które są identyczne:</span><span class="sxs-lookup"><span data-stu-id="b2f75-119">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Klasy dla uczniów i przez instruktorów](inheritance/_static/no-inheritance.png)

<span data-ttu-id="b2f75-121">Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek.</span><span class="sxs-lookup"><span data-stu-id="b2f75-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="b2f75-122">Lub aby pisać usługi, które umożliwia formatowanie nazwy bez opiekowanie, czy nazwa pochodzi od pod kierunkiem instruktora lub studenta.</span><span class="sxs-lookup"><span data-stu-id="b2f75-122">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="b2f75-123">Można utworzyć `Person` podstawowa klasa, która zawiera tylko te właściwości udostępnionego, a następnie wprowadź `Instructor` i `Student` klasy dziedziczą z tej klasy bazowej, jak pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="b2f75-123">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Dla uczniów i instruktora klasy wywodzące się z osobą klasy](inheritance/_static/inheritance.png)

<span data-ttu-id="b2f75-125">Istnieje kilka sposobów, w których ta struktura dziedziczenia, mogą być reprezentowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b2f75-125">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="b2f75-126">Może mieć tabelę osoby, która zawiera informacje na temat uczniów i Instruktorzy w jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b2f75-126">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="b2f75-127">Niektóre kolumny mogą dotyczyć tylko Instruktorzy (DataZatrudnienia), niektóre tylko dla uczniów i studentów (EnrollmentDate), niektóre zarówno (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="b2f75-127">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="b2f75-128">Zazwyczaj trzeba kolumna dyskryminatora, aby wskazać typ, który każdy wiersz reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="b2f75-128">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="b2f75-129">Na przykład kolumna dyskryminatora może być "Instruktora" instruktorów i "Student" dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="b2f75-129">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Przykład tabela wg hierarchii](inheritance/_static/tph.png)

<span data-ttu-id="b2f75-131">Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany Tabela wg hierarchii (TPH) dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="b2f75-131">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="b2f75-132">Alternatywą jest, aby wyglądała bardziej jak struktury dziedziczenia bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b2f75-132">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="b2f75-133">Na przykład może zawierać tylko pola nazwy tabeli osób i mają oddzielnych tabelach przez instruktorów i uczniów za pomocą pola daty.</span><span class="sxs-lookup"><span data-stu-id="b2f75-133">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Tabela wg typu dziedziczenia](inheritance/_static/tpt.png)

<span data-ttu-id="b2f75-135">Ten wzorzec polegający na wprowadzaniu tabeli bazy danych dla każdej klasy jednostki nosi nazwę tabeli na dziedziczenie typu (TPT).</span><span class="sxs-lookup"><span data-stu-id="b2f75-135">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="b2f75-136">Innym rozwiązaniem jest jeszcze mapowania wszystkich typów nieabstrakcyjnej poszczególnych tabel.</span><span class="sxs-lookup"><span data-stu-id="b2f75-136">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="b2f75-137">Wszystkie właściwości klasy, w tym właściwości dziedziczonych są mapowane na kolumny odpowiedniej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b2f75-137">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="b2f75-138">Ten wzorzec jest nazywany dziedziczenia klas tabeli na konkretny (TPC).</span><span class="sxs-lookup"><span data-stu-id="b2f75-138">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="b2f75-139">Jeśli zaimplementowano TPC dziedziczenia klas osoby, studentów i przez instruktorów, jak pokazano wcześniej, tabel, studentów i przez instruktorów wygląda nie różni się po zaimplementowaniu dziedziczenia, niż zajmowały przed.</span><span class="sxs-lookup"><span data-stu-id="b2f75-139">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="b2f75-140">Wzorców dziedziczenia TPC i TPH ogólnie dostarczyć lepszą wydajność niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań.</span><span class="sxs-lookup"><span data-stu-id="b2f75-140">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="b2f75-141">Ten samouczek demonstruje sposób implementacji TPH dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="b2f75-141">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="b2f75-142">TPH jest wzorzec tylko dziedziczenie, który obsługuje Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b2f75-142">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="b2f75-143">Co należy to zrobić, jest utworzenie `Person` klasy, zmienić `Instructor` i `Student` pochodzi od klasy `Person`, Dodaj nową klasę do `DbContext`i Utwórz migracji.</span><span class="sxs-lookup"><span data-stu-id="b2f75-143">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP]
> <span data-ttu-id="b2f75-144">Warto zapisać kopię projektu przed dokonaniem następujących zmian.</span><span class="sxs-lookup"><span data-stu-id="b2f75-144">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="b2f75-145">Następnie w razie problemów i musisz zacząć od początku, będzie można łatwiej uruchomić z zapisany projekt zamiast cofania kroki wykonywane w ramach tego samouczka lub przejdź do początku całej serii.</span><span class="sxs-lookup"><span data-stu-id="b2f75-145">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="b2f75-146">Utwórz klasę osoby</span><span class="sxs-lookup"><span data-stu-id="b2f75-146">Create the Person class</span></span>

<span data-ttu-id="b2f75-147">W folderze modele osoba.cs tworzenie i Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="b2f75-147">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="b2f75-148">Aktualizacja przez instruktorów i uczniów</span><span class="sxs-lookup"><span data-stu-id="b2f75-148">Update Instructor and Student</span></span>

<span data-ttu-id="b2f75-149">W *Instructor.cs*dziedziczyć klasy instruktora klasy osoby i usunąć klucza i nazwy pola.</span><span class="sxs-lookup"><span data-stu-id="b2f75-149">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="b2f75-150">Ten kod będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="b2f75-150">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="b2f75-151">Wprowadzenie identycznych zmian w *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2f75-151">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="b2f75-152">Dodanie osoby do modelu</span><span class="sxs-lookup"><span data-stu-id="b2f75-152">Add Person to the model</span></span>

<span data-ttu-id="b2f75-153">Dodaj typ jednostki osoby do *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2f75-153">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="b2f75-154">Nowe wiersze są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="b2f75-154">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="b2f75-155">To wszystko, co wymaga programu Entity Framework, w celu skonfigurowania Tabela wg hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="b2f75-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="b2f75-156">Jak można zauważyć, gdy baza danych zostanie zaktualizowany, będzie miał tabeli osób, zamiast tabel dla uczniów i instruktora.</span><span class="sxs-lookup"><span data-stu-id="b2f75-156">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="b2f75-157">Tworzenie i aktualizowanie migracji</span><span class="sxs-lookup"><span data-stu-id="b2f75-157">Create and update migrations</span></span>

<span data-ttu-id="b2f75-158">Zapisz zmiany i skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="b2f75-158">Save your changes and build the project.</span></span> <span data-ttu-id="b2f75-159">Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b2f75-159">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="b2f75-160">Nie uruchamiaj `database update` jeszcze polecenia.</span><span class="sxs-lookup"><span data-stu-id="b2f75-160">Don't run the `database update` command yet.</span></span> <span data-ttu-id="b2f75-161">To polecenie spowoduje utratę danych, ponieważ będzie on porzucić tę tabelę przez instruktorów i Zmień nazwę tabeli uczniów do osoby.</span><span class="sxs-lookup"><span data-stu-id="b2f75-161">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="b2f75-162">Należy podać kod niestandardowy, aby zachować istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="b2f75-162">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="b2f75-163">Otwórz *migracje /\<sygnatura czasowa > _Inheritance.cs* i Zastąp `Up` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b2f75-163">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="b2f75-164">Ten kod zajmuje się następujące zadania aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="b2f75-164">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="b2f75-165">Usuwa ograniczenia klucza obcego i indeksy, które wskazują na Tabela Student.</span><span class="sxs-lookup"><span data-stu-id="b2f75-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="b2f75-166">Zmienia nazwę tabeli przez instruktorów jako osoba i sprawia, że zmiany wymagane w celu przechowywania danych dla uczniów:</span><span class="sxs-lookup"><span data-stu-id="b2f75-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="b2f75-167">Dodaje EnrollmentDate dopuszczającego wartość null dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="b2f75-167">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="b2f75-168">Dodaje kolumnę dyskryminator, aby wskazać, czy wiersz jest dla uczniów lub pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="b2f75-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="b2f75-169">Sprawia, że DataZatrudnienia dopuszczającego wartość null, ponieważ wiersze dla uczniów nie będzie mieć dat.</span><span class="sxs-lookup"><span data-stu-id="b2f75-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="b2f75-170">Dodaje tymczasowe pola, które będzie można używać do aktualizowania kluczy obcych, które wskazują dla uczniów i studentów.</span><span class="sxs-lookup"><span data-stu-id="b2f75-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="b2f75-171">Podczas kopiowania studentów do tabeli osoba otrzyma nowe wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="b2f75-171">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="b2f75-172">Kopiuje dane z tabeli uczniów do tabeli osoby.</span><span class="sxs-lookup"><span data-stu-id="b2f75-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="b2f75-173">Powoduje to uczniom zostaną przypisane nowe wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="b2f75-173">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="b2f75-174">Rozwiązuje wartości klucza obcego, które wskazują dla uczniów i studentów.</span><span class="sxs-lookup"><span data-stu-id="b2f75-174">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="b2f75-175">Ponownie tworzy ograniczenia klucza obcego i indeksy, teraz wskazaniem tabeli osób.</span><span class="sxs-lookup"><span data-stu-id="b2f75-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="b2f75-176">(Identyfikator GUID gdyby użyto zamiast liczby całkowitej jako typ klucza podstawowego, nie trzeba zmieniać wartości klucza podstawowego dla uczniów i niektóre z tych kroków można została pominięta.)</span><span class="sxs-lookup"><span data-stu-id="b2f75-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="b2f75-177">Uruchom `database update` polecenia:</span><span class="sxs-lookup"><span data-stu-id="b2f75-177">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="b2f75-178">(W systemie produkcyjnym będzie wprowadzić odpowiednie zmiany do `Down` metody w przypadku nigdy nie było go użyć, aby wrócić do poprzedniej wersji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2f75-178">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="b2f75-179">W tym samouczku nie będzie używany `Down` metody.)</span><span class="sxs-lookup"><span data-stu-id="b2f75-179">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE]
> <span data-ttu-id="b2f75-180">Istnieje możliwość uzyskać inne błędy, podczas wprowadzania zmian schematu w bazie danych, która ma istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="b2f75-180">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="b2f75-181">Występują błędy migracji, których nie można rozwiązać, można zmienić nazwy bazy danych w parametrach połączenia lub Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b2f75-181">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="b2f75-182">Za pomocą nowej bazy danych nie ma żadnych danych do migracji, a polecenie update-database jest bardziej prawdopodobne zakończyć bez błędów.</span><span class="sxs-lookup"><span data-stu-id="b2f75-182">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="b2f75-183">Aby usunąć bazy danych, użyj SSOX, lub uruchom `database drop` interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b2f75-183">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="b2f75-184">Testowanie wdrożenia</span><span class="sxs-lookup"><span data-stu-id="b2f75-184">Test the implementation</span></span>

<span data-ttu-id="b2f75-185">Uruchom aplikację i spróbuj różnych stronach.</span><span class="sxs-lookup"><span data-stu-id="b2f75-185">Run the app and try various pages.</span></span> <span data-ttu-id="b2f75-186">Wszystko działa to tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="b2f75-186">Everything works the same as it did before.</span></span>

<span data-ttu-id="b2f75-187">W **Eksplorator obiektów SQL Server**, rozwiń węzeł **danych połączeń/SchoolContext** i następnie **tabel**, zobaczysz zastępuje tabel dla uczniów i przez instruktorów Tabela osoby.</span><span class="sxs-lookup"><span data-stu-id="b2f75-187">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="b2f75-188">Otwórz projektanta tabel osoby i możesz zobaczyć, że ma wszystkie kolumny, które kiedyś w tabelach studenta i programistę-instruktora.</span><span class="sxs-lookup"><span data-stu-id="b2f75-188">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabela osoby w SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="b2f75-190">Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** się kolumna dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="b2f75-190">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabela osoby w SSOX - tabeli danych](inheritance/_static/ssox-person-data.png)

## <a name="get-the-code"></a><span data-ttu-id="b2f75-192">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="b2f75-192">Get the code</span></span>

[<span data-ttu-id="b2f75-193">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2f75-193">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a><span data-ttu-id="b2f75-194">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b2f75-194">Additional resources</span></span>

<span data-ttu-id="b2f75-195">Aby uzyskać więcej informacji dotyczących dziedziczenia w Entity Framework Core, zobacz [dziedziczenia](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="b2f75-195">For more information about inheritance in Entity Framework Core, see [Inheritance](/ef/core/modeling/inheritance).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2f75-196">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b2f75-196">Next steps</span></span>

<span data-ttu-id="b2f75-197">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="b2f75-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b2f75-198">Dziedziczenie mapowanego do bazy danych</span><span class="sxs-lookup"><span data-stu-id="b2f75-198">Mapped inheritance to database</span></span>
> * <span data-ttu-id="b2f75-199">Utworzone klasy osoby</span><span class="sxs-lookup"><span data-stu-id="b2f75-199">Created the Person class</span></span>
> * <span data-ttu-id="b2f75-200">Zaktualizowane przez instruktorów i uczniów</span><span class="sxs-lookup"><span data-stu-id="b2f75-200">Updated Instructor and Student</span></span>
> * <span data-ttu-id="b2f75-201">Dodano osoby do modelu</span><span class="sxs-lookup"><span data-stu-id="b2f75-201">Added Person to the model</span></span>
> * <span data-ttu-id="b2f75-202">Utworzone i zaktualizuj migracji</span><span class="sxs-lookup"><span data-stu-id="b2f75-202">Created and update migrations</span></span>
> * <span data-ttu-id="b2f75-203">Przetestowane wdrożenia</span><span class="sxs-lookup"><span data-stu-id="b2f75-203">Tested the implementation</span></span>

<span data-ttu-id="b2f75-204">Przejdź do następnego artykułu, aby dowiedzieć się, jak obsługiwać różnych stosunkowo zaawansowane scenariusze platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b2f75-204">Advance to the next article to learn how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b2f75-205">Tematy zaawansowane</span><span class="sxs-lookup"><span data-stu-id="b2f75-205">Advanced topics</span></span>](advanced.md)
