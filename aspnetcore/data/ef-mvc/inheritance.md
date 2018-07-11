---
title: Platforma ASP.NET Core MVC z programem EF Core — dziedziczenie - 9, 10
author: rick-anderson
description: Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych przy użyciu platformy Entity Framework Core w aplikacji ASP.NET Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a71954297f44f936893a7f1e9d3b0685f81378b9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126707"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a><span data-ttu-id="b2b33-103">Platforma ASP.NET Core MVC z programem EF Core — dziedziczenie - 9, 10</span><span class="sxs-lookup"><span data-stu-id="b2b33-103">ASP.NET Core MVC with EF Core - Inheritance - 9 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b2b33-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b2b33-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b2b33-105">Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b2b33-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="b2b33-106">Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="b2b33-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="b2b33-107">W poprzednim samouczku obsługiwane są wyjątki współbieżności.</span><span class="sxs-lookup"><span data-stu-id="b2b33-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="b2b33-108">Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="b2b33-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="b2b33-109">W programowanie zorientowane obiektowo, można użyć dziedziczenia ułatwiające ponowne wykorzystanie kodu.</span><span class="sxs-lookup"><span data-stu-id="b2b33-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="b2b33-110">W tym samouczku poznasz, jak zmienić `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowej klasy, która zawiera właściwości, takie jak `LastName` , które są wspólne dla instruktorów i studentów.</span><span class="sxs-lookup"><span data-stu-id="b2b33-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="b2b33-111">Nie będzie dodać lub zmienić dowolnymi stronami sieci web, ale zmienisz część kodu, a te zmiany są automatycznie odzwierciedlane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b2b33-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="b2b33-112">Opcje mapowania dziedziczenia w tabelach bazy danych</span><span class="sxs-lookup"><span data-stu-id="b2b33-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="b2b33-113">`Instructor` i `Student` klasami w modelu danych School ma kilka właściwości, które są identyczne:</span><span class="sxs-lookup"><span data-stu-id="b2b33-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Klasy dla uczniów i przez instruktorów](inheritance/_static/no-inheritance.png)

<span data-ttu-id="b2b33-115">Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek.</span><span class="sxs-lookup"><span data-stu-id="b2b33-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="b2b33-116">Lub aby pisać usługi, które umożliwia formatowanie nazwy bez opiekowanie, czy nazwa pochodzi od pod kierunkiem instruktora lub studenta.</span><span class="sxs-lookup"><span data-stu-id="b2b33-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="b2b33-117">Można utworzyć `Person` podstawowa klasa, która zawiera tylko te właściwości udostępnionego, a następnie wprowadź `Instructor` i `Student` klasy dziedziczą z tej klasy bazowej, jak pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="b2b33-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Dla uczniów i instruktora klasy wywodzące się z osobą klasy](inheritance/_static/inheritance.png)

<span data-ttu-id="b2b33-119">Istnieje kilka sposobów, w których ta struktura dziedziczenia, mogą być reprezentowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b2b33-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="b2b33-120">Może mieć tabelę osoby, która zawiera informacje na temat uczniów i Instruktorzy w jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b2b33-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="b2b33-121">Niektóre kolumny mogą dotyczyć tylko Instruktorzy (DataZatrudnienia), niektóre tylko dla uczniów i studentów (EnrollmentDate), niektóre zarówno (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="b2b33-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="b2b33-122">Zazwyczaj trzeba kolumna dyskryminatora, aby wskazać typ, który każdy wiersz reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="b2b33-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="b2b33-123">Na przykład kolumna dyskryminatora może być "Instruktora" instruktorów i "Student" dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="b2b33-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Przykład tabela wg hierarchii](inheritance/_static/tph.png)

<span data-ttu-id="b2b33-125">Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany Tabela wg hierarchii (TPH) dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="b2b33-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="b2b33-126">Alternatywą jest, aby wyglądała bardziej jak struktury dziedziczenia bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b2b33-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="b2b33-127">Na przykład może zawierać tylko pola nazwy tabeli osób i mają oddzielnych tabelach przez instruktorów i uczniów za pomocą pola daty.</span><span class="sxs-lookup"><span data-stu-id="b2b33-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Tabela wg typu dziedziczenia](inheritance/_static/tpt.png)

<span data-ttu-id="b2b33-129">Ten wzorzec polegający na wprowadzaniu tabeli bazy danych dla każdej klasy jednostki nosi nazwę tabeli na dziedziczenie typu (TPT).</span><span class="sxs-lookup"><span data-stu-id="b2b33-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="b2b33-130">Innym rozwiązaniem jest jeszcze mapowania wszystkich typów nieabstrakcyjnej poszczególnych tabel.</span><span class="sxs-lookup"><span data-stu-id="b2b33-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="b2b33-131">Wszystkie właściwości klasy, w tym właściwości dziedziczonych są mapowane na kolumny odpowiedniej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b2b33-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="b2b33-132">Ten wzorzec jest nazywany dziedziczenia klas tabeli na konkretny (TPC).</span><span class="sxs-lookup"><span data-stu-id="b2b33-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="b2b33-133">Jeśli zaimplementowano TPC dziedziczenia klas osoby, studentów i przez instruktorów, jak pokazano wcześniej, tabel, studentów i przez instruktorów wygląda nie różni się po zaimplementowaniu dziedziczenia, niż zajmowały przed.</span><span class="sxs-lookup"><span data-stu-id="b2b33-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="b2b33-134">Wzorców dziedziczenia TPC i TPH ogólnie dostarczyć lepszą wydajność niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań.</span><span class="sxs-lookup"><span data-stu-id="b2b33-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="b2b33-135">Ten samouczek demonstruje sposób implementacji TPH dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="b2b33-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="b2b33-136">TPH jest wzorzec tylko dziedziczenie, który obsługuje Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b2b33-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="b2b33-137">Co należy to zrobić, jest utworzenie `Person` klasy, zmienić `Instructor` i `Student` pochodzi od klasy `Person`, Dodaj nową klasę do `DbContext`i Utwórz migracji.</span><span class="sxs-lookup"><span data-stu-id="b2b33-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP]
> <span data-ttu-id="b2b33-138">Warto zapisać kopię projektu przed dokonaniem następujących zmian.</span><span class="sxs-lookup"><span data-stu-id="b2b33-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="b2b33-139">Następnie w razie problemów i musisz zacząć od początku, będzie można łatwiej uruchomić z zapisany projekt zamiast cofania kroki wykonywane w ramach tego samouczka lub przejdź do początku całej serii.</span><span class="sxs-lookup"><span data-stu-id="b2b33-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="b2b33-140">Utwórz klasę osoby</span><span class="sxs-lookup"><span data-stu-id="b2b33-140">Create the Person class</span></span>

<span data-ttu-id="b2b33-141">W folderze modele osoba.cs tworzenie i Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="b2b33-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="b2b33-142">Utworzyć dla uczniów i instruktora klasy dziedziczyć od osoby</span><span class="sxs-lookup"><span data-stu-id="b2b33-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="b2b33-143">W *Instructor.cs*dziedziczyć klasy instruktora klasy osoby i usunąć klucza i nazwy pola.</span><span class="sxs-lookup"><span data-stu-id="b2b33-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="b2b33-144">Ten kod będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="b2b33-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="b2b33-145">Wprowadzenie identycznych zmian w *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2b33-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="b2b33-146">Dodaj osoby typu jednostki do modelu danych</span><span class="sxs-lookup"><span data-stu-id="b2b33-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="b2b33-147">Dodaj typ jednostki osoby do *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2b33-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="b2b33-148">Nowe wiersze są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="b2b33-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="b2b33-149">To wszystko, co wymaga programu Entity Framework, w celu skonfigurowania Tabela wg hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="b2b33-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="b2b33-150">Jak można zauważyć, gdy baza danych zostanie zaktualizowany, będzie miał tabeli osób, zamiast tabel dla uczniów i instruktora.</span><span class="sxs-lookup"><span data-stu-id="b2b33-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="b2b33-151">Tworzenie i dostosowywanie kodu migracji</span><span class="sxs-lookup"><span data-stu-id="b2b33-151">Create and customize migration code</span></span>

<span data-ttu-id="b2b33-152">Zapisz zmiany i skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="b2b33-152">Save your changes and build the project.</span></span> <span data-ttu-id="b2b33-153">Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b2b33-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="b2b33-154">Nie uruchamiaj `database update` jeszcze polecenia.</span><span class="sxs-lookup"><span data-stu-id="b2b33-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="b2b33-155">To polecenie spowoduje utratę danych, ponieważ będzie on porzucić tę tabelę przez instruktorów i Zmień nazwę tabeli uczniów do osoby.</span><span class="sxs-lookup"><span data-stu-id="b2b33-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="b2b33-156">Należy podać kod niestandardowy, aby zachować istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="b2b33-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="b2b33-157">Otwórz *migracje /\<sygnatura czasowa > _Inheritance.cs* i Zastąp `Up` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b2b33-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="b2b33-158">Ten kod zajmuje się następujące zadania aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="b2b33-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="b2b33-159">Usuwa ograniczenia klucza obcego i indeksy, które wskazują na Tabela Student.</span><span class="sxs-lookup"><span data-stu-id="b2b33-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="b2b33-160">Zmienia nazwę tabeli przez instruktorów jako osoba i sprawia, że zmiany wymagane w celu przechowywania danych dla uczniów:</span><span class="sxs-lookup"><span data-stu-id="b2b33-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="b2b33-161">Dodaje EnrollmentDate dopuszczającego wartość null dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="b2b33-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="b2b33-162">Dodaje kolumnę dyskryminator, aby wskazać, czy wiersz jest dla uczniów lub pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="b2b33-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="b2b33-163">Sprawia, że DataZatrudnienia dopuszczającego wartość null, ponieważ wiersze dla uczniów nie będzie mieć dat.</span><span class="sxs-lookup"><span data-stu-id="b2b33-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="b2b33-164">Dodaje tymczasowe pola, które będzie można używać do aktualizowania kluczy obcych, które wskazują dla uczniów i studentów.</span><span class="sxs-lookup"><span data-stu-id="b2b33-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="b2b33-165">Podczas kopiowania studentów do tabeli osoba otrzyma nowe wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="b2b33-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="b2b33-166">Kopiuje dane z tabeli uczniów do tabeli osoby.</span><span class="sxs-lookup"><span data-stu-id="b2b33-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="b2b33-167">Powoduje to uczniom zostaną przypisane nowe wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="b2b33-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="b2b33-168">Rozwiązuje wartości klucza obcego, które wskazują dla uczniów i studentów.</span><span class="sxs-lookup"><span data-stu-id="b2b33-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="b2b33-169">Ponownie tworzy ograniczenia klucza obcego i indeksy, teraz wskazaniem tabeli osób.</span><span class="sxs-lookup"><span data-stu-id="b2b33-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="b2b33-170">(Identyfikator GUID gdyby użyto zamiast liczby całkowitej jako typ klucza podstawowego, nie trzeba zmieniać wartości klucza podstawowego dla uczniów i niektóre z tych kroków można została pominięta.)</span><span class="sxs-lookup"><span data-stu-id="b2b33-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="b2b33-171">Uruchom `database update` polecenia:</span><span class="sxs-lookup"><span data-stu-id="b2b33-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="b2b33-172">(W systemie produkcyjnym będzie wprowadzić odpowiednie zmiany do `Down` metody w przypadku nigdy nie było go użyć, aby wrócić do poprzedniej wersji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2b33-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="b2b33-173">W tym samouczku nie będzie używany `Down` metody.)</span><span class="sxs-lookup"><span data-stu-id="b2b33-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE]
> <span data-ttu-id="b2b33-174">Istnieje możliwość uzyskać inne błędy, podczas wprowadzania zmian schematu w bazie danych, która ma istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="b2b33-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="b2b33-175">Występują błędy migracji, których nie można rozwiązać, można zmienić nazwy bazy danych w parametrach połączenia lub Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b2b33-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="b2b33-176">Za pomocą nowej bazy danych nie ma żadnych danych do migracji, a polecenie update-database jest bardziej prawdopodobne zakończyć bez błędów.</span><span class="sxs-lookup"><span data-stu-id="b2b33-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="b2b33-177">Aby usunąć bazy danych, użyj SSOX, lub uruchom `database drop` interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b2b33-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="b2b33-178">Testowanie za pomocą dziedziczenia zaimplementowany</span><span class="sxs-lookup"><span data-stu-id="b2b33-178">Test with inheritance implemented</span></span>

<span data-ttu-id="b2b33-179">Uruchom aplikację i spróbuj różnych stronach.</span><span class="sxs-lookup"><span data-stu-id="b2b33-179">Run the app and try various pages.</span></span> <span data-ttu-id="b2b33-180">Wszystko działa to tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="b2b33-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="b2b33-181">W **Eksplorator obiektów SQL Server**, rozwiń węzeł **danych połączeń/SchoolContext** i następnie **tabel**, zobaczysz zastępuje tabel dla uczniów i przez instruktorów Tabela osoby.</span><span class="sxs-lookup"><span data-stu-id="b2b33-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="b2b33-182">Otwórz projektanta tabel osoby i możesz zobaczyć, że ma wszystkie kolumny, które kiedyś w tabelach studenta i programistę-instruktora.</span><span class="sxs-lookup"><span data-stu-id="b2b33-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabela osoby w SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="b2b33-184">Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** się kolumna dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="b2b33-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabela osoby w SSOX - tabeli danych](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="b2b33-186">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b2b33-186">Summary</span></span>

<span data-ttu-id="b2b33-187">Tabela wg hierarchii dziedziczenia dla zastało zaimplementowane `Person`, `Student`, i `Instructor` klasy.</span><span class="sxs-lookup"><span data-stu-id="b2b33-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="b2b33-188">Aby uzyskać więcej informacji dotyczących dziedziczenia w Entity Framework Core, zobacz [dziedziczenia](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="b2b33-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="b2b33-189">W następnym samouczku pokazano, jak do obsługi różnorodnych stosunkowo zaawansowane scenariusze platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b2b33-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="b2b33-190">[Poprzednie](concurrency.md)
> [dalej](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="b2b33-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>
