---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Wdrażanie dziedziczenia z programu Entity Framework w aplikacji ASP.NET MVC (8, 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d3c3d19a62f601d007b0fac031ade1eef9f35771
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378939"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="f7fb9-103">Wdrażanie dziedziczenia z programu Entity Framework w aplikacji ASP.NET MVC (8, 10)</span><span class="sxs-lookup"><span data-stu-id="f7fb9-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="f7fb9-104">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f7fb9-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f7fb9-105">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="f7fb9-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="f7fb9-106">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="f7fb9-107">Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="f7fb9-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="f7fb9-108">Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="f7fb9-109">Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="f7fb9-110">Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="f7fb9-111">Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="f7fb9-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="f7fb9-112">W poprzednim samouczku obsługiwane są wyjątki współbieżności.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="f7fb9-113">Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="f7fb9-114">W programowanie zorientowane obiektowo, można użyć dziedziczenia, aby wyeliminować nadmiarowy kod.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="f7fb9-115">W tym samouczku poznasz, jak zmienić `Instructor` i `Student` klasy tak, że pochodzą one od `Person` podstawowej klasy, która zawiera właściwości, takie jak `LastName` , które są wspólne dla instruktorów i studentów.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="f7fb9-116">Nie będzie dodać lub zmienić dowolnymi stronami sieci web, ale zmienisz część kodu, a te zmiany są automatycznie odzwierciedlane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="f7fb9-117">Tabela wg hierarchii i tabeli na typ dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="f7fb9-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="f7fb9-118">W programowanie zorientowane obiektowo, można użyć dziedziczenia w celu ułatwienia pracy z powiązanymi klasami.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="f7fb9-119">Na przykład `Instructor` i `Student` klas w `School` modelu danych udostępnianie kilka właściwości, które powoduje nadmiarowy kod:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="f7fb9-121">Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="f7fb9-122">Można utworzyć `Person` klasy, która zawiera tylko te właściwości udostępnionego podstawowej, a następnie wprowadź `Instructor` i `Student` jednostek dziedziczą z tej klasy bazowej, jak pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="f7fb9-124">Istnieje kilka sposobów, w których ta struktura dziedziczenia, mogą być reprezentowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="f7fb9-125">Może mieć `Person` tabelę, która zawiera informacje na temat uczniów i Instruktorzy w jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="f7fb9-126">Niektóre kolumny można zastosować tylko do Instruktorzy (`HireDate`), niektóre tylko dla uczniów i studentów (`EnrollmentDate`), niektóre na wartość oba (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="f7fb9-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="f7fb9-127">Zazwyczaj trzeba *dyskryminatora* kolumny, aby wskazać, jakiego typu każdy wiersz reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="f7fb9-128">Na przykład kolumna dyskryminatora może być "Instruktora" instruktorów i "Student" dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tabela na hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="f7fb9-130">Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany *Tabela wg hierarchii* dziedziczenia (TPH).</span><span class="sxs-lookup"><span data-stu-id="f7fb9-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="f7fb9-131">Alternatywą jest, aby wyglądała bardziej jak struktury dziedziczenia bazę danych.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="f7fb9-132">Na przykład można mieć tylko pola nazwy `Person` tabeli i mieć osobne `Instructor` i `Student` tabele z polami daty.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Tabela na type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="f7fb9-134">Ten wzorzec polegający na wprowadzaniu tabeli bazy danych dla każdej klasie jednostki jest wywoływana *tabeli na typ* dziedziczenia (TPT).</span><span class="sxs-lookup"><span data-stu-id="f7fb9-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="f7fb9-135">Wzorce dziedziczenia TPH ogólnie zapewniają lepszą wydajność platformy Entity Framework niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="f7fb9-136">Ten samouczek demonstruje sposób implementacji TPH dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="f7fb9-137">Należy to zrobić, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="f7fb9-138">Tworzenie `Person` klasy i zmień `Instructor` i `Student` pochodzi od klasy `Person`.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="f7fb9-139">Dodaj mapowanie modelu w bazie danych kod do klasy kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="f7fb9-140">Zmiana `InstructorID` i `StudentID` odwołania w całym projekcie, aby `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="f7fb9-141">Tworzenie klasy osoby</span><span class="sxs-lookup"><span data-stu-id="f7fb9-141">Creating the Person Class</span></span>

 <span data-ttu-id="f7fb9-142">Uwaga: Nie będzie możliwe do skompilowania projektu po utworzeniu klasy poniżej, dopóki nie zaktualizujesz kontrolerów, które korzysta z tych klas.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="f7fb9-143">W *modeli* folderze utwórz *osoba.cs* i Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="f7fb9-144">W *Instructor.cs*, pochodzi `Instructor` klasy z `Person` klasy, a następnie usuń klucza i nazwy pola.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="f7fb9-145">Ten kod będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="f7fb9-146">Wprowadzić zmiany podobne do *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="f7fb9-147">`Student` Klasy będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="f7fb9-148">Dodawanie osoby typu jednostki do modelu</span><span class="sxs-lookup"><span data-stu-id="f7fb9-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="f7fb9-149">W *SchoolContext.cs*, Dodaj `DbSet` właściwość `Person` typ jednostki:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="f7fb9-150">To wszystko, co wymaga programu Entity Framework, w celu skonfigurowania Tabela wg hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="f7fb9-151">Jak można zauważyć, gdy baza danych jest ponownie utworzyć jej `Person` tabeli zamiast `Student` i `Instructor` tabel.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="f7fb9-152">Zmiana InstructorID i StudentID PersonID</span><span class="sxs-lookup"><span data-stu-id="f7fb9-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="f7fb9-153">W *SchoolContext.cs*, w instrukcji mapowania kurs przez instruktorów, zmień `MapRightKey("InstructorID")` do `MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="f7fb9-154">Ta zmiana nie jest wymagane; po prostu zmienia nazwę kolumny InstructorID w tabeli sprzężenia wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="f7fb9-155">Jeśli nazwa jako InstructorID, aplikacja nadal będzie działać poprawnie.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="f7fb9-156">Poniżej przedstawiono ukończoną *SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="f7fb9-157">Następnie należy zmienić `InstructorID` do `PersonID` i `StudentID` do `PersonID` w całym projekcie ***z wyjątkiem*** w plikach z sygnaturami czasowymi migracje *migracje* folder.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="f7fb9-158">Aby to zrobić można będzie znaleźć otwierać tylko pliki, które muszą zostać zmienione, a następnie wykonać globalne zmiany w otwartych plików.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="f7fb9-159">To jedyny plik w *migracje* folder, należy zmienić to *Migrations\Configuration.cs.*</span><span class="sxs-lookup"><span data-stu-id="f7fb9-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="f7fb9-160">Rozpocznij od zamyka wszystkie otwarte pliki w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="f7fb9-161">Kliknij przycisk **Znajdź i Zamień — wszystkie pliki** w **Edytuj** menu, a następnie wyszukaj wszystkie pliki w projekcie, które zawierają `InstructorID`.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="f7fb9-162">Otwórz każdy plik w **Find Results** okna ***z wyjątkiem*** &lt;sygnatura czasowa&gt;*\_.cs* migracji plików w *Migracje* folderu, klikając dwukrotnie jeden wiersz dla każdego pliku.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="f7fb9-163">Otwórz **Zamień w plikach** okno dialogowe i zmień **przeszukania** do **wszystkimi otwartymi dokumentami**.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="f7fb9-164">Użyj **Zamień w plikach** okna dialogowego, aby zmienić wszystkie `InstructorID` do `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="f7fb9-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="f7fb9-165">Znajdź wszystkie pliki w projekcie, który zawiera `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="f7fb9-166">Otwórz każdy plik w **Find Results** okna ***z wyjątkiem*** &lt;sygnatura czasowa&gt;*\_\*.cs* plików migracji w *migracje* folderu, klikając dwukrotnie jeden wiersz dla każdego pliku.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="f7fb9-167">Otwórz **Zamień w plikach** okno dialogowe i zmień **przeszukania** do **wszystkimi otwartymi dokumentami**.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="f7fb9-168">Użyj **Zamień w plikach** okna dialogowego, aby zmienić wszystkie `StudentID` do `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="f7fb9-169">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-169">Build the project.</span></span>

<span data-ttu-id="f7fb9-170">(Należy pamiętać, że w tym przykładzie pokazano *wadą* z `classnameID` wzorzec nazewnictwa kluczy podstawowych.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="f7fb9-171">Jeśli klucze podstawowe identyfikator miał nazwę bez poprzedzania ich nazwą klasy, *nie* zmiana nazwy konieczna może być teraz.)</span><span class="sxs-lookup"><span data-stu-id="f7fb9-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="f7fb9-172">Tworzenie i aktualizowanie plików migracji</span><span class="sxs-lookup"><span data-stu-id="f7fb9-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="f7fb9-173">W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="f7fb9-174">Uruchom `Update-Database` polecenia w konsoli zarządzania Pakietami.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="f7fb9-175">Polecenie będzie działać na tym etapie, ponieważ będziemy mieć istniejące dane migracji nie wie, jak obsługiwać.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="f7fb9-176">Zostanie wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-176">You get the following error:</span></span>

<span data-ttu-id="f7fb9-177">*Instrukcja ALTER TABLE konflikt z ograniczeniem klucza OBCEGO "klucza Obcego\_dbo. Dział\_dbo. Osoby\_PersonID ". Konflikt wystąpił w bazie danych "ContosoUniversity" table "dbo. Osoba", kolumna"PersonID".*</span><span class="sxs-lookup"><span data-stu-id="f7fb9-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="f7fb9-178">Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp `Up` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="f7fb9-179">Uruchom `update-database` ponownie polecenie.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="f7fb9-180">Istnieje możliwość uzyskać inne błędy, podczas migracji danych i wprowadzania zmian schematu.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="f7fb9-181">Jeśli występują błędy migracji nie można rozwiązać, możesz kontynuować samouczek, zmieniając parametry połączenia w *Web.config* pliku lub usunięcie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="f7fb9-182">Najprostszą metodą jest zmiana nazwy bazy danych w *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="f7fb9-183">Na przykład zmienić nazwę bazy danych do pojemności\_testów, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="f7fb9-184">Za pomocą nowej bazy danych, nie ma żadnych danych, aby przeprowadzić migrację oraz `update-database` polecenia jest znacznie bardziej prawdopodobne zakończyć bez błędów.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="f7fb9-185">Aby uzyskać instrukcje dotyczące sposobu usuwania z bazy danych, zobacz [jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="f7fb9-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="f7fb9-186">Jeśli w przypadku zastosowania tego podejścia, aby można było kontynuować z tego samouczka, należy pominąć krok wdrożenia na końcu tego samouczka, ponieważ witrynę wdrożoną otrzymamy ten sam błąd, gdy migracja jest uruchamiany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="f7fb9-187">Jeśli chcesz rozwiązać błąd migracji, zostanie najlepszy zasób jest jednym z forów platformy Entity Framework lub StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="f7fb9-188">Testowanie</span><span class="sxs-lookup"><span data-stu-id="f7fb9-188">Testing</span></span>

<span data-ttu-id="f7fb9-189">Uruchamianie witryny, a następnie spróbuj różnych stronach.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-189">Run the site and try various pages.</span></span> <span data-ttu-id="f7fb9-190">Wszystko działa to tak jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="f7fb9-191">W **Eksploratora serwera** rozwiń **SchoolContext** i następnie **tabel**, i zobaczysz, że **uczniów** i **przez instruktorów**  tabele zostały zastąpione przez **osoby** tabeli.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="f7fb9-192">Rozwiń **osoby** tabeli i zawiera wszystkie kolumny, które wcześniej w **uczniów** i **przez instruktorów** tabel.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="f7fb9-194">Kliknij prawym przyciskiem myszy tabeli osób, a następnie kliknij przycisk **Pokaż dane tabeli** się kolumna dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="f7fb9-195">Na poniższym diagramie przedstawiono strukturę nową bazę danych School:</span><span class="sxs-lookup"><span data-stu-id="f7fb9-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="f7fb9-197">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f7fb9-197">Summary</span></span>

<span data-ttu-id="f7fb9-198">Tabela wg hierarchii dziedziczenia teraz została wdrożona w celu `Person`, `Student`, i `Instructor` klasy.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="f7fb9-199">Aby uzyskać więcej informacji na temat tego i innych struktur dziedziczenia, zobacz [Strategie mapowania dziedziczenia](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) na blogu Morteza Manavi.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="f7fb9-200">W następnym samouczku zobaczysz niektóre sposoby zaimplementowania repozytorium i jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="f7fb9-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="f7fb9-201">Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="f7fb9-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f7fb9-202">[Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="f7fb9-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
