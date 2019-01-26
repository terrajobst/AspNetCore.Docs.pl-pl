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
ms.openlocfilehash: 79513edce7ac3044f6f547149400cba7d307edfa
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889902"
---
# <a name="template-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="ef725-103">Szablon: Implementowanie dziedziczenia z programów EF w aplikacji ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="ef725-103">Template: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="ef725-104">W poprzednim samouczku obsługiwane są wyjątki współbieżności.</span><span class="sxs-lookup"><span data-stu-id="ef725-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="ef725-105">Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="ef725-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="ef725-106">Programowanie zorientowane obiektowo umożliwia [dziedziczenia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) ułatwiające [ponowne użycie kodu](http://en.wikipedia.org/wiki/Code_reuse).</span><span class="sxs-lookup"><span data-stu-id="ef725-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="ef725-107">W tym samouczku poznasz, jak zmienić `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowej klasy, która zawiera właściwości, takie jak `LastName` , które są wspólne dla instruktorów i studentów.</span><span class="sxs-lookup"><span data-stu-id="ef725-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="ef725-108">Nie będzie dodać lub zmienić dowolnymi stronami sieci web, ale zmienisz część kodu, a te zmiany są automatycznie odzwierciedlane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ef725-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="ef725-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="ef725-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ef725-110">Dowiedz się, jak mapować dziedziczenia do bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef725-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="ef725-111">Utwórz klasę osoby</span><span class="sxs-lookup"><span data-stu-id="ef725-111">Create the Person class</span></span>
> * <span data-ttu-id="ef725-112">Aktualizacja przez instruktorów i uczniów</span><span class="sxs-lookup"><span data-stu-id="ef725-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="ef725-113">Dodanie osoby do modelu</span><span class="sxs-lookup"><span data-stu-id="ef725-113">Add Person to the Model</span></span>
> * <span data-ttu-id="ef725-114">Tworzenie i aktualizowanie migracji</span><span class="sxs-lookup"><span data-stu-id="ef725-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="ef725-115">Testowanie wdrożenia</span><span class="sxs-lookup"><span data-stu-id="ef725-115">Test the implementation</span></span>
> * <span data-ttu-id="ef725-116">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="ef725-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef725-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ef725-117">Prerequisites</span></span>

* [<span data-ttu-id="ef725-118">Implementowanie dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="ef725-118">Implementing Inheritance</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="ef725-119">Mapowanie dziedziczenia do bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef725-119">Map inheritance to database</span></span>

<span data-ttu-id="ef725-120">`Instructor` i `Student` klas w `School` model danych ma kilka właściwości, które są identyczne:</span><span class="sxs-lookup"><span data-stu-id="ef725-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="ef725-122">Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek.</span><span class="sxs-lookup"><span data-stu-id="ef725-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="ef725-123">Lub aby pisać usługi, które umożliwia formatowanie nazwy bez opiekowanie, czy nazwa pochodzi od pod kierunkiem instruktora lub studenta.</span><span class="sxs-lookup"><span data-stu-id="ef725-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="ef725-124">Można utworzyć `Person` klasy, która zawiera tylko te właściwości udostępnionego podstawowej, a następnie wprowadź `Instructor` i `Student` jednostek dziedziczą z tej klasy bazowej, jak pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="ef725-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="ef725-126">Istnieje kilka sposobów, w których ta struktura dziedziczenia, mogą być reprezentowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ef725-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="ef725-127">Może mieć `Person` tabelę, która zawiera informacje na temat uczniów i Instruktorzy w jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ef725-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="ef725-128">Niektóre kolumny można zastosować tylko do Instruktorzy (`HireDate`), niektóre tylko dla uczniów i studentów (`EnrollmentDate`), niektóre na wartość oba (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="ef725-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="ef725-129">Zazwyczaj trzeba *dyskryminatora* kolumny, aby wskazać, jakiego typu każdy wiersz reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="ef725-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="ef725-130">Na przykład kolumna dyskryminatora może być "Instruktora" instruktorów i "Student" dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="ef725-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="ef725-132">Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany *Tabela wg hierarchii* dziedziczenia (TPH).</span><span class="sxs-lookup"><span data-stu-id="ef725-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="ef725-133">Alternatywą jest, aby wyglądała bardziej jak struktury dziedziczenia bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ef725-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="ef725-134">Na przykład można mieć tylko pola nazwy `Person` tabeli i mieć osobne `Instructor` i `Student` tabele z polami daty.</span><span class="sxs-lookup"><span data-stu-id="ef725-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="ef725-136">Ten wzorzec polegający na wprowadzaniu tabeli bazy danych dla każdej klasie jednostki jest wywoływana *tabeli na typ* dziedziczenia (TPT).</span><span class="sxs-lookup"><span data-stu-id="ef725-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="ef725-137">Innym rozwiązaniem jest jeszcze mapowania wszystkich typów nieabstrakcyjnej poszczególnych tabel.</span><span class="sxs-lookup"><span data-stu-id="ef725-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="ef725-138">Wszystkie właściwości klasy, w tym właściwości dziedziczonych są mapowane na kolumny odpowiedniej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ef725-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="ef725-139">Ten wzorzec jest nazywany dziedziczenia klas tabeli na konkretny (TPC).</span><span class="sxs-lookup"><span data-stu-id="ef725-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="ef725-140">Jeśli zaimplementowano dziedziczenie TPC `Person`, `Student`, i `Instructor` klasy, jak pokazano wcześniej, `Student` i `Instructor` tabel będzie wyglądać nie różni się po zaimplementowaniu dziedziczenia, niż zajmowały przed.</span><span class="sxs-lookup"><span data-stu-id="ef725-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="ef725-141">TPC i wzorców dziedziczenia TPH ogólnie dostarczyć, lepszej wydajności platformy Entity Framework niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań.</span><span class="sxs-lookup"><span data-stu-id="ef725-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="ef725-142">Ten samouczek demonstruje sposób implementacji TPH dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="ef725-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="ef725-143">TPH jest domyślny wzorzec dziedziczenia platformy Entity Framework, więc trzeba wystarczy utworzyć `Person` klasy, zmienić `Instructor` i `Student` pochodzi od klasy `Person`, Dodaj nową klasę do `DbContext`i Utwórz Migracja.</span><span class="sxs-lookup"><span data-stu-id="ef725-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="ef725-144">(Uzyskać informacji o sposobie wdrażania z innymi wzorami dziedziczenia, zobacz [mapowanie dziedziczenia tabela wg typu (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) i [mapowanie dziedziczenia klasy tabeli na konkretny (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) w witrynie MSDN Dokumentacja programu Entity Framework.)</span><span class="sxs-lookup"><span data-stu-id="ef725-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="ef725-145">Utwórz klasę osoby</span><span class="sxs-lookup"><span data-stu-id="ef725-145">Create the Person class</span></span>

<span data-ttu-id="ef725-146">W *modeli* folderze utwórz *osoba.cs* i Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="ef725-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="ef725-147">Aktualizacja przez instruktorów i uczniów</span><span class="sxs-lookup"><span data-stu-id="ef725-147">Update Instructor and Student</span></span>

<span data-ttu-id="ef725-148">Teraz zaktualizować *Instructor.cs* i *Sudent.cs* dziedziczy wartości z *Person.sc*.</span><span class="sxs-lookup"><span data-stu-id="ef725-148">Now update the *Instructor.cs* and *Sudent.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="ef725-149">W *Instructor.cs*, pochodzi `Instructor` klasy z `Person` klasy, a następnie usuń klucza i nazwy pola.</span><span class="sxs-lookup"><span data-stu-id="ef725-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="ef725-150">Ten kod będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="ef725-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="ef725-151">Wprowadzić zmiany podobne do *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="ef725-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="ef725-152">`Student` Klasy będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="ef725-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="ef725-153">Dodanie osoby do modelu</span><span class="sxs-lookup"><span data-stu-id="ef725-153">Add Person to the Model</span></span>

<span data-ttu-id="ef725-154">W *SchoolContext.cs*, Dodaj `DbSet` właściwość `Person` typ jednostki:</span><span class="sxs-lookup"><span data-stu-id="ef725-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="ef725-155">To wszystko, co wymaga programu Entity Framework, w celu skonfigurowania Tabela wg hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="ef725-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="ef725-156">Jak można zauważyć, gdy baza danych zostanie zaktualizowany, będzie miał `Person` tabeli zamiast `Student` i `Instructor` tabel.</span><span class="sxs-lookup"><span data-stu-id="ef725-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="ef725-157">Tworzenie i aktualizowanie migracji</span><span class="sxs-lookup"><span data-stu-id="ef725-157">Create and Update Migrations</span></span>

<span data-ttu-id="ef725-158">W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ef725-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="ef725-159">Uruchom `Update-Database` polecenia w konsoli zarządzania Pakietami.</span><span class="sxs-lookup"><span data-stu-id="ef725-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="ef725-160">Polecenie będzie działać na tym etapie, ponieważ będziemy mieć istniejące dane migracji nie wie, jak obsługiwać.</span><span class="sxs-lookup"><span data-stu-id="ef725-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="ef725-161">Otrzymasz komunikat o błędzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="ef725-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="ef725-162">*Nie można usunąć obiektu "dbo. Przez instruktorów ", ponieważ jest ona przywoływana przez ograniczenie klucza OBCEGO.*</span><span class="sxs-lookup"><span data-stu-id="ef725-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="ef725-163">Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp `Up` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ef725-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="ef725-164">Ten kod zajmuje się następujące zadania aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ef725-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="ef725-165">Usuwa ograniczenia klucza obcego i indeksy, które wskazują na Tabela Student.</span><span class="sxs-lookup"><span data-stu-id="ef725-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="ef725-166">Zmienia nazwę tabeli przez instruktorów jako osoba i sprawia, że zmiany wymagane w celu przechowywania danych dla uczniów:</span><span class="sxs-lookup"><span data-stu-id="ef725-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="ef725-167">Dodaje EnrollmentDate dopuszczającego wartość null dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="ef725-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="ef725-168">Dodaje kolumnę dyskryminator, aby wskazać, czy wiersz jest dla uczniów lub pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="ef725-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="ef725-169">Sprawia, że DataZatrudnienia dopuszczającego wartość null, ponieważ wiersze dla uczniów nie będzie mieć dat.</span><span class="sxs-lookup"><span data-stu-id="ef725-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="ef725-170">Dodaje tymczasowe pola, które będzie można używać do aktualizowania kluczy obcych, które wskazują dla uczniów i studentów.</span><span class="sxs-lookup"><span data-stu-id="ef725-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="ef725-171">Podczas kopiowania studentów do tabeli osoby otrzymują nowe wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ef725-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="ef725-172">Kopiuje dane z tabeli uczniów do tabeli osoby.</span><span class="sxs-lookup"><span data-stu-id="ef725-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="ef725-173">Powoduje to uczniom zostaną przypisane nowe wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ef725-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="ef725-174">Rozwiązuje wartości klucza obcego, które wskazują dla uczniów i studentów.</span><span class="sxs-lookup"><span data-stu-id="ef725-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="ef725-175">Ponownie tworzy ograniczenia klucza obcego i indeksy, teraz wskazaniem tabeli osób.</span><span class="sxs-lookup"><span data-stu-id="ef725-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="ef725-176">(Identyfikator GUID gdyby użyto zamiast liczby całkowitej jako typ klucza podstawowego, nie trzeba zmieniać wartości klucza podstawowego dla uczniów i niektóre z tych kroków można została pominięta.)</span><span class="sxs-lookup"><span data-stu-id="ef725-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="ef725-177">Uruchom `update-database` ponownie polecenie.</span><span class="sxs-lookup"><span data-stu-id="ef725-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="ef725-178">(W systemie produkcyjnym użytkownik może wprowadzić odpowiednie zmiany do metody w dół w przypadku, gdy nigdy nie było go użyć, aby wrócić do poprzedniej wersji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef725-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="ef725-179">Na potrzeby tego samouczka możesz nie będzie używany metody w dół.)</span><span class="sxs-lookup"><span data-stu-id="ef725-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="ef725-180">Istnieje możliwość uzyskać inne błędy, podczas migracji danych i wprowadzania zmian schematu.</span><span class="sxs-lookup"><span data-stu-id="ef725-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="ef725-181">Jeśli występują błędy migracji nie można rozwiązać, możesz kontynuować samouczek, zmieniając parametry połączenia w *Web.config* pliku lub przez usunięcie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef725-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="ef725-182">Najprostszą metodą jest zmiana nazwy bazy danych w *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="ef725-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="ef725-183">Na przykład zmienić nazwę bazy danych do ContosoUniversity2, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="ef725-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="ef725-184">Za pomocą nowej bazy danych, nie ma żadnych danych, aby przeprowadzić migrację oraz `update-database` polecenia jest znacznie bardziej prawdopodobne zakończyć bez błędów.</span><span class="sxs-lookup"><span data-stu-id="ef725-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="ef725-185">Aby uzyskać instrukcje dotyczące sposobu usuwania z bazy danych, zobacz [jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="ef725-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="ef725-186">W przypadku zastosowania tego podejścia w celu przejdź do samouczka pominąć krok wdrażania na końcu tego samouczka lub wdrożyć nowej lokacji i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef725-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="ef725-187">Jeśli Wdróż aktualizację w tej samej lokacji, które możesz już został wdrożenie już EF zostanie wyświetlony ten sam błąd, gdy migracja jest uruchamiany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="ef725-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="ef725-188">Jeśli chcesz rozwiązać błąd migracji, zostanie najlepszy zasób jest jednym z forów platformy Entity Framework lub StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="ef725-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="ef725-189">Testowanie wdrożenia</span><span class="sxs-lookup"><span data-stu-id="ef725-189">Test the implementation</span></span>

<span data-ttu-id="ef725-190">Uruchamianie witryny, a następnie spróbuj różnych stronach.</span><span class="sxs-lookup"><span data-stu-id="ef725-190">Run the site and try various pages.</span></span> <span data-ttu-id="ef725-191">Wszystko działa to tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="ef725-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="ef725-192">W **Eksploratora serwera** rozwiń **Connections\SchoolContext danych** i następnie **tabel**, i zobaczysz, że **uczniów** i **Przez instruktorów** tabele zostały zastąpione przez **osoby** tabeli.</span><span class="sxs-lookup"><span data-stu-id="ef725-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="ef725-193">Rozwiń **osoby** tabeli i zawiera wszystkie kolumny, które wcześniej w **uczniów** i **przez instruktorów** tabel.</span><span class="sxs-lookup"><span data-stu-id="ef725-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="ef725-194">Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** się kolumna dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="ef725-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="ef725-195">Na poniższym diagramie przedstawiono strukturę nową bazę danych School:</span><span class="sxs-lookup"><span data-stu-id="ef725-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="ef725-197">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="ef725-197">Deploy to Azure</span></span>

<span data-ttu-id="ef725-198">W tej sekcji, musisz ukończyć opcjonalną **wdrażania aplikacji na platformie Azure** sekcji [część 3, sortowanie, filtrowanie i stronicowanie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) z tej serii samouczka.</span><span class="sxs-lookup"><span data-stu-id="ef725-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="ef725-199">Jeśli użytkownik ma błędy migracji, które rozwiązano problem, usuwając bazy danych w projekcie lokalnym, Pomiń ten krok; Tworzenie nowej lokacji i bazy danych lub wdrożenia do nowego środowiska.</span><span class="sxs-lookup"><span data-stu-id="ef725-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="ef725-200">W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="ef725-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="ef725-201">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="ef725-201">Click **Publish**.</span></span>

    <span data-ttu-id="ef725-202">Aplikacja sieci Web zostanie otwarta w domyślnej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="ef725-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="ef725-203">Testowanie aplikacji w celu zweryfikowania działa.</span><span class="sxs-lookup"><span data-stu-id="ef725-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="ef725-204">Uruchom stronę po raz pierwszy uzyskuje dostęp do bazy danych, platformy Entity Framework umożliwia uruchamianie wszystkich migracjach `Up` metod wymaganych do przełączenia bazy danych na bieżąco z bieżącym modelem danych.</span><span class="sxs-lookup"><span data-stu-id="ef725-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="ef725-205">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="ef725-205">Get the code</span></span>

[<span data-ttu-id="ef725-206">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="ef725-206">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="ef725-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ef725-207">Additional resources</span></span>

<span data-ttu-id="ef725-208">Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="ef725-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="ef725-209">Aby uzyskać więcej informacji na temat tego i innych struktur dziedziczenia, zobacz [wzorzec dziedziczenia TPT](https://msdn.microsoft.com/data/jj618293) i [wzorzec dziedziczenia TPH](https://msdn.microsoft.com/data/jj618292) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="ef725-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="ef725-210">W następnym samouczku pokazano, jak do obsługi różnorodnych stosunkowo zaawansowane scenariusze platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ef725-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef725-211">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ef725-211">Next steps</span></span>

<span data-ttu-id="ef725-212">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="ef725-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ef725-213">Przedstawiono do mapowania dziedziczenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef725-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="ef725-214">Utworzone klasy osoby</span><span class="sxs-lookup"><span data-stu-id="ef725-214">Created the Person class</span></span>
> * <span data-ttu-id="ef725-215">Zaktualizowane przez instruktorów i uczniów</span><span class="sxs-lookup"><span data-stu-id="ef725-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="ef725-216">Dodano osoby do modelu</span><span class="sxs-lookup"><span data-stu-id="ef725-216">Added Person to the Model</span></span>
> * <span data-ttu-id="ef725-217">Utworzone i zaktualizuj migracji</span><span class="sxs-lookup"><span data-stu-id="ef725-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="ef725-218">Przetestowane wdrożenia</span><span class="sxs-lookup"><span data-stu-id="ef725-218">Tested the implementation</span></span>
> * <span data-ttu-id="ef725-219">Wdrażane na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="ef725-219">Deployed to Azure</span></span>

<span data-ttu-id="ef725-220">Przejdź do następnego artykułu, aby dowiedzieć się więcej na temat tematów, które są przydatne pod uwagę podczas czegoś podstawy tworzenia aplikacji sieci web ASP.NET, które używają programu Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="ef725-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ef725-221">Zaawansowane scenariusze platformy Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ef725-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)