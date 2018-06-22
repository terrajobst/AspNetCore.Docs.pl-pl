---
title: Platformy ASP.NET Core MVC podstawowych EF — Model danych — 5 10
author: rick-anderson
description: W tym samouczku Dodaj więcej jednostki i relacje i dostosować modelu danych, określając formatowania, sprawdzanie poprawności i mapowanie reguły.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: d89ca44917fac57febc2f8b0d632ae004ca7216c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277390"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a><span data-ttu-id="a7497-103">Platformy ASP.NET Core MVC podstawowych EF — Model danych — 5 10</span><span class="sxs-lookup"><span data-stu-id="a7497-103">ASP.NET Core MVC with EF Core - Data Model - 5 of 10</span></span>

<span data-ttu-id="a7497-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a7497-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a7497-105">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a7497-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="a7497-106">Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="a7497-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="a7497-107">W poprzednich samouczki pracy z modelem danych proste składającą się z trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="a7497-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="a7497-108">W tym samouczku dodasz więcej jednostki i relacje i model danych będzie dostosować, określając formatowania, sprawdzanie poprawności i reguły mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="a7497-109">Po zakończeniu klas jednostek spowoduje model pełne dane, które przedstawiono na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="a7497-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="a7497-111">Dostosowywanie modelu danych za pomocą atrybutów</span><span class="sxs-lookup"><span data-stu-id="a7497-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="a7497-112">W tej sekcji pojawi się, jak dostosować za pomocą atrybutów określających formatowania, sprawdzanie poprawności i reguły mapowania bazy danych modelu danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="a7497-113">Następnie w kilka z następujących sekcji, które należy utworzyć pełną modelu danych służbowych, dodając atrybuty dla klasy została już utworzona i tworzenia nowych klas dla pozostałych typów jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="a7497-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="a7497-114">Atrybut typu danych</span><span class="sxs-lookup"><span data-stu-id="a7497-114">The DataType attribute</span></span>

<span data-ttu-id="a7497-115">Dat rejestracji dla użytkowników domowych wszystkie strony sieci web obecnie Wyświetl czas wraz z datą, mimo że wszystkie potrzebne informacje dla tego pola to data.</span><span class="sxs-lookup"><span data-stu-id="a7497-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="a7497-116">Za pomocą atrybutów adnotacji danych, można go utworzyć kod zmian, który naprawi format wyświetlania w każdym widoku, który zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="a7497-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="a7497-117">Aby wyświetlić przykład sposobu wykonywania, czy zostanie dodana do atrybutu `EnrollmentDate` właściwości w `Student` klasy.</span><span class="sxs-lookup"><span data-stu-id="a7497-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="a7497-118">W *Models/Student.cs*, Dodaj `using` instrukcji dla `System.ComponentModel.DataAnnotations` przestrzeni nazw i Dodaj `DataType` i `DisplayFormat` atrybuty do `EnrollmentDate` właściwości, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a7497-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="a7497-119">`DataType` Atrybut służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-119">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="a7497-120">W takim przypadku tylko chcemy śledzić data nie Data i godzina.</span><span class="sxs-lookup"><span data-stu-id="a7497-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="a7497-121">`DataType` Wyliczenie zawiera wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress i więcej.</span><span class="sxs-lookup"><span data-stu-id="a7497-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="a7497-122">`DataType` Atrybut można również włączyć aplikacji w celu umożliwienia automatycznie funkcji specyficznych dla typu.</span><span class="sxs-lookup"><span data-stu-id="a7497-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="a7497-123">Na przykład `mailto:` można tworzyć łącza `DataType.EmailAddress`, i może zostać dostarczony selektora daty `DataType.Date` w przeglądarkach obsługujących HTML5.</span><span class="sxs-lookup"><span data-stu-id="a7497-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="a7497-124">`DataType` HTML 5 emituje atrybut `data-` atrybutów (wyraźnym danych dash), które byłyby zrozumiałe dla przeglądarki HTML 5.</span><span class="sxs-lookup"><span data-stu-id="a7497-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="a7497-125">`DataType` Atrybutów nie oferują żadnych sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="a7497-125">The `DataType` attributes don't provide any validation.</span></span>

<span data-ttu-id="a7497-126">`DataType.Date` nie określono format daty, która jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="a7497-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="a7497-127">Domyślnie są wyświetlane w polu danych domyślny format oparte na obiekt CultureInfo serwera.</span><span class="sxs-lookup"><span data-stu-id="a7497-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="a7497-128">`DisplayFormat` Atrybut służy do jawnie określić format daty:</span><span class="sxs-lookup"><span data-stu-id="a7497-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="a7497-129">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinny również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji.</span><span class="sxs-lookup"><span data-stu-id="a7497-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="a7497-130">(Nie można że w przypadku niektórych pól — na przykład w przypadku wartości walutowych nie można symbol waluty w polu tekstowym do edycji.)</span><span class="sxs-lookup"><span data-stu-id="a7497-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="a7497-131">Można użyć `DisplayFormat` atrybutu przez sam, ale jest zwykle warto użyć `DataType` również atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a7497-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="a7497-132">`DataType` Atrybut przekazuje semantykę danych zamiast sposób renderowania jej na ekranie i zapewnia następujące korzyści, które nie można uzyskać z `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="a7497-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="a7497-133">Przeglądarki, można włączyć funkcje HTML5 (na przykład do wyświetlenia w formancie kalendarza, symbol waluty odpowiednie ustawienia regionalne, przesyłanie pocztą e-mail łączy, niektóre klienta wejściowe weryfikacji itp.).</span><span class="sxs-lookup"><span data-stu-id="a7497-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="a7497-134">Domyślnie przeglądarka wyświetli danych przy użyciu właściwego formatu oparte na ustawienia regionalne.</span><span class="sxs-lookup"><span data-stu-id="a7497-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="a7497-135">Aby uzyskać więcej informacji, zobacz [ \<wejściowych > tagów dokumentacji Pomocnika](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="a7497-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="a7497-136">Uruchom aplikację, przejdź do strony indeksu studentów i zwróć uwagę, nie są wyświetlane godziny dla daty rejestracji.</span><span class="sxs-lookup"><span data-stu-id="a7497-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="a7497-137">Będzie taka sama wartość true dla dowolnego widoku, który używa modelu uczniów.</span><span class="sxs-lookup"><span data-stu-id="a7497-137">The same will be true for any view that uses the Student model.</span></span>

![Wyświetlanie dat bez godzin strony indeksu uczniów lub studentów](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="a7497-139">Atrybut StringLength</span><span class="sxs-lookup"><span data-stu-id="a7497-139">The StringLength attribute</span></span>

<span data-ttu-id="a7497-140">Można również określić reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a7497-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="a7497-141">`StringLength` Atrybut Ustawia maksymalną długość w bazie danych i zapewnia po stronie klienta i po stronie serwera weryfikacji dla platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a7497-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="a7497-142">Minimalna długość ciągu można również określić, w tym atrybucie, ale wartość minimalna nie ma wpływu na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="a7497-143">Załóżmy, że chcesz upewnić się, że użytkownicy nie wprowadzić więcej niż 50 znaków dla nazwy.</span><span class="sxs-lookup"><span data-stu-id="a7497-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="a7497-144">Aby dodać to ograniczenie, Dodaj `StringLength` atrybuty do `LastName` i `FirstMidName` właściwości, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a7497-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="a7497-145">`StringLength` Atrybutu nie uniemożliwić wprowadzanie biały znak dla nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a7497-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="a7497-146">Można użyć `RegularExpression` atrybutu, aby zastosować ograniczenia do danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="a7497-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="a7497-147">Na przykład następujący kod wymaga pierwszego znaku się wielkie litery i pozostałych znaków jako alfabetycznej:</span><span class="sxs-lookup"><span data-stu-id="a7497-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="a7497-148">`MaxLength` Atrybutu zapewnia funkcje podobne do `StringLength` atrybutu, ale nie zapewnia po stronie klienta sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="a7497-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="a7497-149">Model bazy danych zostanie zmieniona w taki sposób, który wymaga zmiany w schemacie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="a7497-150">Aby zaktualizować schemat bez utraty danych, które zostały dodane do bazy danych przy użyciu interfejsu użytkownika aplikacji użyjesz migracji.</span><span class="sxs-lookup"><span data-stu-id="a7497-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="a7497-151">Zapisz zmiany i skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="a7497-151">Save your changes and build the project.</span></span> <span data-ttu-id="a7497-152">Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a7497-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="a7497-153">`migrations add` Polecenie ostrzega, że może dojść do utraty danych, ponieważ zmiana powoduje, że maksymalna długość dla dwóch kolumn.</span><span class="sxs-lookup"><span data-stu-id="a7497-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="a7497-154">Migracje tworzy plik o nazwie  *\<sygnatury czasowej > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7497-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="a7497-155">Ten plik zawiera kod w `Up` metodę, która spowoduje zaktualizowanie bazy danych zgodnie z bieżącym modelem danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="a7497-156">`database update` Polecenie zadziałało kodu.</span><span class="sxs-lookup"><span data-stu-id="a7497-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="a7497-157">Sygnatura czasowa prefiksem nazwy pliku migracji jest używany przez programu Entity Framework porządkowania migracji.</span><span class="sxs-lookup"><span data-stu-id="a7497-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="a7497-158">Przed uruchomieniem polecenia update-database można utworzyć wiele migracji, a następnie wszystkie migracja są stosowane w kolejności, w której zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="a7497-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="a7497-159">Uruchom aplikację, wybierz **studentów** , kliknij pozycję **Utwórz nowy**i wprowadź nazwę, albo więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="a7497-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="a7497-160">Po kliknięciu **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="a7497-160">When you click **Create**, client side validation shows an error message.</span></span>

![Strona wyświetlająca ciąg błędy długość indeksu uczniów lub studentów](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="a7497-162">Atrybut kolumny</span><span class="sxs-lookup"><span data-stu-id="a7497-162">The Column attribute</span></span>

<span data-ttu-id="a7497-163">Atrybuty umożliwia także kontrolować sposób z klas i właściwości są mapowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="a7497-164">Załóżmy, że używasz nazwy `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać drugie imię.</span><span class="sxs-lookup"><span data-stu-id="a7497-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="a7497-165">Ale ma być nazwane kolumna bazy danych `FirstName`, ponieważ użytkownicy, którzy będą Pisanie zapytań ad hoc w bazie danych są zapoznanie tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="a7497-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="a7497-166">Aby to mapowanie, można użyć `Column` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a7497-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="a7497-167">`Column` Atrybut określa, że po utworzeniu bazy danych, w kolumnie `Student` tabeli, który jest mapowany na `FirstMidName` właściwość o nazwie `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="a7497-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="a7497-168">Innymi słowy, gdy kod odwołuje się do `Student.FirstMidName`, dane będą pobierane z lub zaktualizowane w `FirstName` kolumny `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="a7497-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="a7497-169">Jeśli nie określisz nazwy kolumn, ich otrzymuje taką samą nazwę jak nazwa właściwości.</span><span class="sxs-lookup"><span data-stu-id="a7497-169">If you don't specify column names, they're given the same name as the property name.</span></span>

<span data-ttu-id="a7497-170">W *Student.cs* plików, dodawanie `using` instrukcji dla `System.ComponentModel.DataAnnotations.Schema` i dodać atrybut nazwy kolumny do `FirstMidName` właściwości, jak pokazano w poniższym kodzie wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="a7497-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="a7497-171">Dodanie `Column` atrybutu zmieni model obsługujący `SchoolContext`, więc nie będzie zgodny z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="a7497-172">Zapisz zmiany i skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="a7497-172">Save your changes and build the project.</span></span> <span data-ttu-id="a7497-173">Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenia, aby utworzyć inną migracji:</span><span class="sxs-lookup"><span data-stu-id="a7497-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="a7497-174">W **Eksplorator obiektów SQL Server**, otwórz projektanta tabel dla użytkowników domowych, klikając dwukrotnie **uczniowie** tabeli.</span><span class="sxs-lookup"><span data-stu-id="a7497-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="a7497-176">Przed zastosowaniem dwóch pierwszych migracji, nazwa kolumny były typu nvarchar(MAX).</span><span class="sxs-lookup"><span data-stu-id="a7497-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="a7497-177">Są one obecnie nvarchar(50) i nazwę kolumny zmieniła się z FirstMidName na imię.</span><span class="sxs-lookup"><span data-stu-id="a7497-177">They're now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="a7497-178">Jeśli spróbujesz skompilować przed zakończeniem Tworzenie wszystkich klas jednostek w poniższych sekcjach można otrzymać błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="a7497-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="a7497-179">Końcowe zmiany do jednostki dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="a7497-179">Final changes to the Student entity</span></span>

![Jednostki dla użytkowników domowych](complex-data-model/_static/student-entity.png)

<span data-ttu-id="a7497-181">W *Models/Student.cs*, Zastąp kod dodane wcześniej następujący kod.</span><span class="sxs-lookup"><span data-stu-id="a7497-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="a7497-182">Zmiany zostały wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="a7497-182">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="a7497-183">Wymagany atrybut</span><span class="sxs-lookup"><span data-stu-id="a7497-183">The Required attribute</span></span>

<span data-ttu-id="a7497-184">`Required` Atrybut powoduje, że nazwa właściwości wymaganych pól.</span><span class="sxs-lookup"><span data-stu-id="a7497-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="a7497-185">`Required` Atrybut nie jest wymagane dla typów wartości null, takich jak typy wartości (DateTime, int, kliknij dwukrotnie, float, itp.).</span><span class="sxs-lookup"><span data-stu-id="a7497-185">The `Required` attribute isn't needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="a7497-186">Typy, które nie może mieć wartości null są automatycznie traktowane jako wymagane pola.</span><span class="sxs-lookup"><span data-stu-id="a7497-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="a7497-187">Można usunąć `Required` atrybutu i zamień ją na minimalną długość parametru `StringLength` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="a7497-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="a7497-188">Atrybut wyświetlania</span><span class="sxs-lookup"><span data-stu-id="a7497-188">The Display attribute</span></span>

<span data-ttu-id="a7497-189">`Display` Atrybut określa, że podpis pola tekstowe powinny być "Imię", "Nazwisko", "Pełna nazwa" i "Data rejestracji" zamiast nazwy właściwości w każdym wystąpieniu (który nie ma miejsca dzielenia wyrazów).</span><span class="sxs-lookup"><span data-stu-id="a7497-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="a7497-190">Właściwość obliczona imię i nazwisko</span><span class="sxs-lookup"><span data-stu-id="a7497-190">The FullName calculated property</span></span>

<span data-ttu-id="a7497-191">`FullName` jest obliczonej właściwości, która zwraca wartość, która jest tworzona przez łączenie dwóch innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="a7497-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="a7497-192">W związku z tym ma metodę dostępu get i nie `FullName` kolumny zostanie wygenerowany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="a7497-193">Utwórz jednostkę instruktora</span><span class="sxs-lookup"><span data-stu-id="a7497-193">Create the Instructor Entity</span></span>

![Jednostka Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="a7497-195">Utwórz *Models/Instructor.cs*, zastępując kod szablonu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a7497-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="a7497-196">Zwróć uwagę, że kilka właściwości są takie same, w jednostkach dla użytkowników domowych i instruktora.</span><span class="sxs-lookup"><span data-stu-id="a7497-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="a7497-197">W [wdrażanie dziedziczenia](inheritance.md) później w tym samouczku będziesz Refaktoryzuj ten kod, aby wyeliminować nadmiarowości.</span><span class="sxs-lookup"><span data-stu-id="a7497-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="a7497-198">Możesz też zaznaczyć wiele atrybutów w jednym wierszu, można także zapisać `HireDate` atrybutów w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a7497-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="a7497-199">Właściwości nawigacji CourseAssignments i OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="a7497-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="a7497-200">`CourseAssignments` i `OfficeAssignment` właściwości są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a7497-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="a7497-201">Instruktora nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="a7497-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="a7497-202">Właściwość nawigacji może zawierać wiele jednostek, w którym wpisów można być dodane, usunięte i zaktualizowane listy musi być typu.</span><span class="sxs-lookup"><span data-stu-id="a7497-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="a7497-203">Można określić `ICollection<T>` lub typu, takich jak `List<T>` lub `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="a7497-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="a7497-204">Jeśli określisz `ICollection<T>`, tworzy EF `HashSet<T>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a7497-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="a7497-205">Przyczyny, dlaczego są `CourseAssignment` jednostek opisanej poniżej w sekcji o relacje wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="a7497-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="a7497-206">Reguły biznesowe contoso University stanu, że instruktora tylko może mieć co najwyżej jeden pakiet office, więc `OfficeAssignment` właściwość przechowuje pojedynczą jednostką OfficeAssignment (co może mieć wartości zerowej, jeśli nie przypisano żadnych pakietu office).</span><span class="sxs-lookup"><span data-stu-id="a7497-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="a7497-207">Utwórz jednostkę OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="a7497-207">Create the OfficeAssignment entity</span></span>

![OfficeAssignment jednostki](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="a7497-209">Utwórz *Models/OfficeAssignment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a7497-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="a7497-210">Atrybut klucza</span><span class="sxs-lookup"><span data-stu-id="a7497-210">The Key attribute</span></span>

<span data-ttu-id="a7497-211">Brak relacji jeden do zero lub jeden między instruktora i jednostkami, OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="a7497-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="a7497-212">Przypisania office występuje tylko w odniesieniu do instruktora, który jest przypisany do, a więc jego klucz podstawowy również jest jego klucza obcego do jednostki instruktora.</span><span class="sxs-lookup"><span data-stu-id="a7497-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="a7497-213">Ale programu Entity Framework automatycznie nie może rozpoznać InstructorID jako klucz podstawowy tej jednostki, ponieważ jego nazwa nie będzie zgodna z konwencją nazewnictwa Identyfikatora lub classnameID.</span><span class="sxs-lookup"><span data-stu-id="a7497-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="a7497-214">W związku z tym `Key` atrybut służy do identyfikacji jako klucz:</span><span class="sxs-lookup"><span data-stu-id="a7497-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="a7497-215">Można również użyć `Key` atrybut, jeśli jednostka ma własny klucz podstawowy, ale ma nazwę właściwości, coś innego niż classnameID lub identyfikator.</span><span class="sxs-lookup"><span data-stu-id="a7497-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="a7497-216">Domyślnie EF traktuje klucz jako z systemem innym niż baza danych wygenerowała, ponieważ kolumna jest identyfikujące relacji.</span><span class="sxs-lookup"><span data-stu-id="a7497-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="a7497-217">Właściwość nawigacji instruktora</span><span class="sxs-lookup"><span data-stu-id="a7497-217">The Instructor navigation property</span></span>

<span data-ttu-id="a7497-218">Jednostka Instructor ma wartość null `OfficeAssignment` właściwości nawigacji (ponieważ instruktora nie może być przypisanie pakietu office), i jednostki OfficeAssignment ma niedopuszczającą `Instructor` właściwości nawigacji (ponieważ przypisania office nie istnieje bez instruktora — `InstructorID` nie dopuszcza wartości null).</span><span class="sxs-lookup"><span data-stu-id="a7497-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="a7497-219">Jednostka Instructor ma powiązanej jednostki OfficeAssignment, każdej jednostki ma odwołanie do jeden z nich w jej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a7497-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="a7497-220">Można umieścić `[Required]` atrybutu we właściwości nawigacji instruktora, aby określić, że muszą być powiązane instruktora, ale nie trzeba to zrobić, ponieważ `InstructorID` wartości null jest klucz obcy (który jest także klucz do tej tabeli).</span><span class="sxs-lookup"><span data-stu-id="a7497-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="a7497-221">Modyfikowanie jednostek kursu</span><span class="sxs-lookup"><span data-stu-id="a7497-221">Modify the Course Entity</span></span>

![Jednostki ciągu](complex-data-model/_static/course-entity.png)

<span data-ttu-id="a7497-223">W *Models/Course.cs*, Zastąp kod dodane wcześniej następujący kod.</span><span class="sxs-lookup"><span data-stu-id="a7497-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="a7497-224">Zmiany zostały wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="a7497-224">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="a7497-225">Jednostka kursu ma właściwości klucza obcego `DepartmentID` wskazujących powiązanej jednostki działu i ma `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="a7497-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="a7497-226">Nie wymaga programu Entity Framework, można dodać właściwości klucza obcego do modelu danych w przypadku właściwości nawigacji dla obiekt pokrewny.</span><span class="sxs-lookup"><span data-stu-id="a7497-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="a7497-227">EF automatycznie tworzy klucze obce w bazie danych, wszędzie tam, gdzie są potrzebne i tworzy [w tle właściwości](https://docs.microsoft.com/ef/core/modeling/shadow-properties) dla nich.</span><span class="sxs-lookup"><span data-stu-id="a7497-227">EF automatically creates foreign keys in the database wherever they're needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="a7497-228">Jednak po klucz obcy w modelu danych może uaktualniać prostszy i efektywniejszy.</span><span class="sxs-lookup"><span data-stu-id="a7497-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="a7497-229">Na przykład podczas pobierania jednostki kursu, aby edytować, jednostki działu ma wartość null, jeśli nie zostanie załadowany, więc po zaktualizowaniu jednostki kursu należy najpierw pobrać jednostki działu.</span><span class="sxs-lookup"><span data-stu-id="a7497-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="a7497-230">Gdy właściwość klucza obcego `DepartmentID` znajduje się w modelu danych, nie trzeba było pobrać jednostki działu, przed uaktualnieniem.</span><span class="sxs-lookup"><span data-stu-id="a7497-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="a7497-231">Atrybut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="a7497-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="a7497-232">`DatabaseGenerated` Atrybutem `None` parametru `CourseID` właściwość określa, że wartości klucza podstawowego są dostarczane przez użytkownika zamiast wygenerowanych przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="a7497-233">Domyślnie program Entity Framework przyjęto założenie, że wartości klucza podstawowego są generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="a7497-234">To już ma w większości przypadków.</span><span class="sxs-lookup"><span data-stu-id="a7497-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="a7497-235">Jednak dla porach jednostek, będzie używać numer kursu określone przez użytkownika, takie jak seria 1000 działu jednej serii 2000 do innego działu i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="a7497-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="a7497-236">`DatabaseGenerated` Atrybutu mogą służyć do generowania wartości domyślne, jak w przypadku kolumn bazy danych używane do rejestrowania daty wiersz został utworzony lub zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="a7497-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="a7497-237">Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="a7497-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a7497-238">Właściwości obcego klucza i nawigacji</span><span class="sxs-lookup"><span data-stu-id="a7497-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="a7497-239">Właściwości klucza obcego i właściwości nawigacji w jednostce kursu odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="a7497-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="a7497-240">Kursu jest przypisany do jednego działu, więc ma `DepartmentID` klucza obcego i `Department` właściwość nawigacji z powodów wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="a7497-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="a7497-241">Kursu może mieć dowolną liczbę studentów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="a7497-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="a7497-242">Kursu może organizowane jednocześnie przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją (typ `CourseAssignment` objaśniono [później](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="a7497-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="a7497-243">Utwórz jednostkę działu</span><span class="sxs-lookup"><span data-stu-id="a7497-243">Create the Department entity</span></span>

![Dział jednostki](complex-data-model/_static/department-entity.png)


<span data-ttu-id="a7497-245">Utwórz *Models/Department.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a7497-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="a7497-246">Atrybut kolumny</span><span class="sxs-lookup"><span data-stu-id="a7497-246">The Column attribute</span></span>

<span data-ttu-id="a7497-247">Wcześniej używane `Column` atrybutu można zmienić mapowania nazw kolumn.</span><span class="sxs-lookup"><span data-stu-id="a7497-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="a7497-248">W kodzie dla obiekt działu `Column` atrybutów jest używane do zmienić mapowanie typu danych SQL, tak aby kolumna zostanie zdefiniowana przy użyciu typu money serwera SQL w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="a7497-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="a7497-249">Mapowanie kolumny zwykle nie jest wymagane, ponieważ odpowiedni typ danych programu SQL Server, na podstawie typu CLR dla właściwości wybierze programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a7497-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="a7497-250">Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu.</span><span class="sxs-lookup"><span data-stu-id="a7497-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="a7497-251">Ale w takim przypadku wiesz kolumna zostanie zawierający kwot, czy typ danych money jest bardziej odpowiednie dla danego.</span><span class="sxs-lookup"><span data-stu-id="a7497-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a7497-252">Właściwości obcego klucza i nawigacji</span><span class="sxs-lookup"><span data-stu-id="a7497-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="a7497-253">Właściwości klucza i nawigacja obcego odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="a7497-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="a7497-254">Dział może lub nie ma uprawnienia administratora, a administrator jest zawsze instruktora.</span><span class="sxs-lookup"><span data-stu-id="a7497-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="a7497-255">W związku z tym `InstructorID` właściwość jest uwzględniona jako klucza obcego do jednostki instruktora, a znak zapytania są dodawane po `int` określenie można oznaczyć jako wartości null właściwość typu.</span><span class="sxs-lookup"><span data-stu-id="a7497-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="a7497-256">Właściwość nawigacji jest o nazwie `Administrator` , ale zawiera jednostki instruktora:</span><span class="sxs-lookup"><span data-stu-id="a7497-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="a7497-257">Dział może mieć wiele kursów, więc ma właściwości nawigacji kursy:</span><span class="sxs-lookup"><span data-stu-id="a7497-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="a7497-258">Według Konwencji programu Entity Framework umożliwia usuwanie kaskadowe dla klucza obcego nie dopuszcza wartości null i relacje wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="a7497-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="a7497-259">Może to spowodować cykliczne cascade delete reguł, które spowoduje, że wystąpił wyjątek podczas próby dodania migracji.</span><span class="sxs-lookup"><span data-stu-id="a7497-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="a7497-260">Na przykład jeśli właściwość Department.InstructorID nie zostały zdefiniowane jako wartości null, EF skonfigurować reguły usuwania kaskadowego można usunąć instruktora po usunięciu działu, które nie mają mieć wystąpić.</span><span class="sxs-lookup"><span data-stu-id="a7497-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which isn't what you want to have happen.</span></span> <span data-ttu-id="a7497-261">W razie potrzeby reguł biznesowych `InstructorID` właściwości dopuszczać wartości null, użyj następującej instrukcji interfejsu API fluent, aby wyłączyć usuwanie kaskadowe w relacji musi:</span><span class="sxs-lookup"><span data-stu-id="a7497-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="a7497-262">Modyfikowanie jednostek rejestracji</span><span class="sxs-lookup"><span data-stu-id="a7497-262">Modify the Enrollment entity</span></span>

![Jednostki rejestracji](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="a7497-264">W *Models/Enrollment.cs*, Zastąp kod dodane wcześniej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="a7497-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a7497-265">Właściwości obcego klucza i nawigacji</span><span class="sxs-lookup"><span data-stu-id="a7497-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="a7497-266">Właściwości klucza obcego i właściwości nawigacji odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="a7497-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="a7497-267">Rekord rejestracji dotyczy jednego ciągu, więc ma `CourseID` właściwości kluczy obcych i `Course` właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="a7497-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="a7497-268">Rekord rejestracji jest dla pojedynczego studentów, więc ma `StudentID` właściwości kluczy obcych i `Student` właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="a7497-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="a7497-269">Relacje wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="a7497-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="a7497-270">Brak relacji wiele do wielu między jednostkami dla użytkowników domowych i kursu i jednostki rejestracji działa jako tabelę sprzężenia wiele do wielu *z ładunku* w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="a7497-271">"Za pomocą ładunku" oznacza, że w tabeli rejestracji zawiera dodatkowe dane poza kluczy obcych, dołączonego do tabel (w tym przypadku klucza podstawowego i właściwości klasy).</span><span class="sxs-lookup"><span data-stu-id="a7497-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="a7497-272">Na poniższej ilustracji przedstawiono, jak wyglądają te relacje w diagramie jednostki.</span><span class="sxs-lookup"><span data-stu-id="a7497-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="a7497-273">(Ten diagram został wygenerowany za pomocą narzędzia Entity Framework zasilania dla EF 6.x; Tworzenie diagramu nie stanowi części samouczka, po prostu jest on używany jako ilustrację.)</span><span class="sxs-lookup"><span data-stu-id="a7497-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Uczniów kursu wiele do wielu](complex-data-model/_static/student-course.png)

<span data-ttu-id="a7497-275">Każdy wiersz relacji ma 1 w jeden element end i znak gwiazdki (\*) w innych, wskazując relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="a7497-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="a7497-276">Jeśli w tabeli rejestracji nie włączono informacji o kategorii, tylko będzie konieczne zawiera dwa klucze obce CourseID i StudentID.</span><span class="sxs-lookup"><span data-stu-id="a7497-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="a7497-277">W takim przypadku byłoby wiele do wielu sprzężenia tabeli bez ładunku (lub tabeli czysty sprzężenia) w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="a7497-278">Jednostki instruktora i kursu mieć typu relacji wiele do wielu, a następnym krokiem jest utworzenie klasę jednostki do działania jako tabelę sprzężenia bez ładunku.</span><span class="sxs-lookup"><span data-stu-id="a7497-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="a7497-279">(Niejawne sprzężenie tabel dla relacji wiele do wielu, ale podstawowe EF nie obsługuje 6.x EF.</span><span class="sxs-lookup"><span data-stu-id="a7497-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="a7497-280">Aby uzyskać więcej informacji, zobacz [dyskusji w repozytorium EF Core GitHub](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="a7497-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="a7497-281">Jednostka CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="a7497-281">The CourseAssignment entity</span></span>

![CourseAssignment jednostki](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="a7497-283">Utwórz *Models/CourseAssignment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a7497-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="a7497-284">Dołącz do nazwy podmiotu</span><span class="sxs-lookup"><span data-stu-id="a7497-284">Join entity names</span></span>

<span data-ttu-id="a7497-285">Tabela sprzężenia jest wymagana w bazie danych dla relacji wiele do wielu instruktora do szkolenia, a musi być reprezentowane przez zestaw jednostek.</span><span class="sxs-lookup"><span data-stu-id="a7497-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="a7497-286">Często jest nazwa jednostki sprzężenia `EntityName1EntityName2`, w tym przypadku byłoby `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="a7497-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="a7497-287">Jednak zaleca się, że wybierz nazwę, która opisuje relację.</span><span class="sxs-lookup"><span data-stu-id="a7497-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="a7497-288">Modeli danych uruchamiane prosty i wzrostu za pomocą sprzężeń ładunku nie często pobierania ładunków później.</span><span class="sxs-lookup"><span data-stu-id="a7497-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="a7497-289">Rozpoczyna nazwę opisową jednostki, nie musisz zmienić nazwę.</span><span class="sxs-lookup"><span data-stu-id="a7497-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="a7497-290">W idealnym przypadku jednostki sprzężenia musi własną nazwę fizycznych (prawdopodobnie pojedynczego wyrazu) w domenie biznesowych.</span><span class="sxs-lookup"><span data-stu-id="a7497-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="a7497-291">Na przykład książki i klientów można połączyć za pośrednictwem klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="a7497-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="a7497-292">Dla tej relacji `CourseAssignment` jest lepszym rozwiązaniem niż `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="a7497-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="a7497-293">Klucz złożony</span><span class="sxs-lookup"><span data-stu-id="a7497-293">Composite key</span></span>

<span data-ttu-id="a7497-294">Ponieważ klucze obce nie dopuszcza wartości null i razem jednoznacznie zidentyfikować każdego wiersza tabeli, nie istnieje potrzeba oddzielne klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="a7497-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there's no need for a separate primary key.</span></span> <span data-ttu-id="a7497-295">*InstructorID* i *CourseID* właściwości powinny działać jako klucz podstawowy złożonego.</span><span class="sxs-lookup"><span data-stu-id="a7497-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="a7497-296">Jedynym sposobem, aby zidentyfikować złożonych kluczy podstawowych, aby EF polega na użyciu *interfejsu API fluent* (go nie można wykonać przy użyciu atrybutów).</span><span class="sxs-lookup"><span data-stu-id="a7497-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="a7497-297">Jak skonfigurować złożonego klucza podstawowego w następnej sekcji zostanie wyświetlone.</span><span class="sxs-lookup"><span data-stu-id="a7497-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="a7497-298">Klucz złożony gwarantuje, że natomiast może mieć wiele wierszy dla przebiegu jednego i wielu wierszy dla jednego instruktora, nie może mieć wiele wierszy dla tego samego instruktora oraz przebiegu.</span><span class="sxs-lookup"><span data-stu-id="a7497-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="a7497-299">`Enrollment` Jednostki sprzężenia definiuje własny klucz podstawowy, więc możliwe są duplikatami tego sortowania.</span><span class="sxs-lookup"><span data-stu-id="a7497-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="a7497-300">Aby zapobiec takiej duplikatów, możesz można dodać unikatowego indeksu na pola klucza obcego, lub skonfigurować `Enrollment` z głównej klucz złożony, podobnie jak `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a7497-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="a7497-301">Aby uzyskać więcej informacji, zobacz [indeksów](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="a7497-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="a7497-302">Aktualizacja kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="a7497-302">Update the database context</span></span>

<span data-ttu-id="a7497-303">Dodaj następujący wyróżniony kod, aby *Data/SchoolContext.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="a7497-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="a7497-304">Ten kod dodaje nowe jednostki i konfiguruje jednostki CourseAssignment złożonego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="a7497-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="a7497-305">Zamiast interfejsu API Fluent atrybutów</span><span class="sxs-lookup"><span data-stu-id="a7497-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="a7497-306">Kod w `OnModelCreating` metody `DbContext` klasy używa *interfejsu API fluent* można skonfigurować zachowanie EF.</span><span class="sxs-lookup"><span data-stu-id="a7497-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="a7497-307">Interfejs API jest nazywany "fluent", ponieważ jest często używany przez instalowanie szereg wywołania metody razem w jednej instrukcji, jak w poniższym przykładzie z [EF podstawowej dokumentacji](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="a7497-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="a7497-308">W tym samouczku używasz interfejsu API fluent tylko w przypadku mapowanie bazy danych, które nie są z atrybutami.</span><span class="sxs-lookup"><span data-stu-id="a7497-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="a7497-309">Jednak umożliwia także interfejsu API fluent określić większość formatowania, sprawdzanie poprawności i reguły mapowania, które możesz wykonać za pomocą atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a7497-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="a7497-310">Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować z interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="a7497-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="a7497-311">Jak wspomniano wcześniej, `MinimumLength` nie powoduje zmiany schematu, ma zastosowanie tylko reguły sprawdzania poprawności po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="a7497-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="a7497-312">Niektórzy deweloperzy wolą Użyj interfejsu API fluent wyłącznie tak, aby ich zachowanie ich klasami jednostki "Wyczyść".</span><span class="sxs-lookup"><span data-stu-id="a7497-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="a7497-313">Jeśli chcesz, i istnieje kilka dostosowania, które jest możliwe tylko przy użyciu interfejsu API fluent można mieszać atrybutów i wygodnego interfejsu API, ale na ogół zalecaną praktyką jest wybierz jedną z tych dwóch metod i użyj to spójnie możliwie.</span><span class="sxs-lookup"><span data-stu-id="a7497-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="a7497-314">Jeśli używasz zarówno, należy pamiętać, że zawsze, gdy wystąpi konflikt, interfejsu API Fluent zastępuje atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a7497-314">If you do use both, note that wherever there's a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="a7497-315">Aby uzyskać więcej informacji dotyczących atrybutów w porównaniu z interfejsu API fluent, zobacz [metody konfiguracji](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="a7497-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="a7497-316">Jednostki Diagram przedstawiający relacje</span><span class="sxs-lookup"><span data-stu-id="a7497-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="a7497-317">Na poniższej ilustracji przedstawiono diagram do narzędzia Entity Framework zasilania utworzyć dla ukończonych modelu służbowe.</span><span class="sxs-lookup"><span data-stu-id="a7497-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="a7497-319">Oprócz linii relacji jeden do wielu (od 1 do \*), można widoczną w tym miejscu wiersza relacji jeden do zero lub jeden (1 do od 0 do 1) między jednostkami instruktora i OfficeAssignment i linię relacji zero lub jeden do wielu (od 0 do 1 do *) między Jednostki instruktora i działu.</span><span class="sxs-lookup"><span data-stu-id="a7497-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="a7497-320">Inicjatora bazy danych z danych testowych</span><span class="sxs-lookup"><span data-stu-id="a7497-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="a7497-321">Zastąp kod w *Data/DbInitializer.cs* pliku następującym kodem w celu zapewnienia danych inicjatora nowe jednostki po utworzeniu.</span><span class="sxs-lookup"><span data-stu-id="a7497-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="a7497-322">Jak przedstawiono w pierwszym samouczku większość ten kod po prostu tworzy nowe obiekty jednostki i ładuje przykładowych danych do właściwości wymaganych do testowania.</span><span class="sxs-lookup"><span data-stu-id="a7497-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="a7497-323">Zwróć uwagę, jak relacje wiele do wielu są obsługiwane: kod tworzy relacje przez tworzenie jednostek w `Enrollments` i `CourseAssignment` join zestawów jednostek.</span><span class="sxs-lookup"><span data-stu-id="a7497-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="a7497-324">Dodaj migracji</span><span class="sxs-lookup"><span data-stu-id="a7497-324">Add a migration</span></span>

<span data-ttu-id="a7497-325">Zapisz zmiany i skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="a7497-325">Save your changes and build the project.</span></span> <span data-ttu-id="a7497-326">Następnie otwórz okno polecenia w folderze projektu i wprowadź `migrations add` polecenie (nie wykonuj polecenia update-database jeszcze):</span><span class="sxs-lookup"><span data-stu-id="a7497-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="a7497-327">Zostanie wyświetlone ostrzeżenie o możliwości utraty danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="a7497-328">Jeśli próbujesz uruchomić `database update` polecenia w tym momencie (nie należy go jeszcze) jak następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="a7497-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="a7497-329">Instrukcja ALTER TABLE jest w konflikcie z ograniczenie FOREIGN KEY "FK_dbo. Course_dbo. Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="a7497-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="a7497-330">Konflikt wystąpił w bazie danych "ContosoUniversity" tabeli "dbo. Dział", kolumna"DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="a7497-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="a7497-331">Czasami podczas wykonywania migracji z istniejącymi danymi, należy wstawić dane do bazy danych do zaspokojenia ograniczeń klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="a7497-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="a7497-332">Wygenerowany kod w `Up` metody dodaje klucz obcy DepartmentID wartości null do tabeli kursu.</span><span class="sxs-lookup"><span data-stu-id="a7497-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="a7497-333">Jeśli istnieje już wierszy w tabeli kursu po uruchomieniu kodu `AddColumn` operacja nie powiedzie się, ponieważ program SQL Server nie może ustalić, jaka wartość w kolumnie, która nie może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="a7497-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="a7497-334">W tym samouczku uruchomisz migracji na nową bazę danych, ale w aplikacji produkcyjnej trzeba będzie dokonanie migracji obsługuje istniejących danych, dlatego poniższe instrukcje przedstawiają przykłady, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="a7497-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="a7497-335">Aby migracja pracy z istniejącymi danymi, trzeba zmienić kod, aby podać nową kolumnę wartości domyślnej, a następnie utwórz działu stub o nazwie "Temp" jako domyślnego działu.</span><span class="sxs-lookup"><span data-stu-id="a7497-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="a7497-336">W związku z tym istniejących wierszy kursu zostaną wszystkie powiązane do działu "Temp" po `Up` uruchamia metody.</span><span class="sxs-lookup"><span data-stu-id="a7497-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="a7497-337">Otwórz *{timestamp}_ComplexDataModel.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="a7497-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="a7497-338">Komentarz wiersz kodu, który dodaje kolumnę DepartmentID tabeli kursu.</span><span class="sxs-lookup"><span data-stu-id="a7497-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="a7497-339">Dodaj następujący wyróżniony kod po kod, który tworzy tabelę działu:</span><span class="sxs-lookup"><span data-stu-id="a7497-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="a7497-340">W aplikacji produkcyjnej możesz zapisać kodu lub skryptów służących do dodawania wierszy działu i dotyczą wierszy kursu nowych wierszy działu.</span><span class="sxs-lookup"><span data-stu-id="a7497-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="a7497-341">Następnie już nie musisz dział "Temp" lub wartość domyślną w kolumnie Course.DepartmentID.</span><span class="sxs-lookup"><span data-stu-id="a7497-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="a7497-342">Zapisz zmiany i skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="a7497-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="a7497-343">Zmień parametry połączenia i zaktualizować bazę danych</span><span class="sxs-lookup"><span data-stu-id="a7497-343">Change the connection string and update the database</span></span>

<span data-ttu-id="a7497-344">Masz teraz nowy kod `DbInitializer` klasy, która dodaje nowe jednostki dane do pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="a7497-345">Aby EF, Utwórz nową, pustą bazę danych, Zmień nazwę bazy danych w parametrach połączenia w *appsettings.json* ContosoUniversity3 lub inna nazwa, który nie był używany na komputerze korzystasz.</span><span class="sxs-lookup"><span data-stu-id="a7497-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="a7497-346">Zapisz zmiany w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a7497-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="a7497-347">Alternatywą wobec zmiana nazwy bazy danych można usunąć bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="a7497-348">Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="a7497-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="a7497-349">Po zmianie nazwy bazy danych lub usunąć bazy danych uruchom `database update` polecenie w oknie wiersza polecenia do wykonania migracji.</span><span class="sxs-lookup"><span data-stu-id="a7497-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="a7497-350">Uruchom aplikację, aby spowodować `DbInitializer.Initialize` metodę, aby uruchomić i wypełnić nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="a7497-351">Otwórz bazę danych w SSOX tak samo jak wcześniej, a następnie rozwiń **tabel** węzeł, aby zobaczyć, czy wszystkie tabele zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="a7497-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="a7497-352">(Jeśli nadal masz SSOX otworzyć z wcześniejszego stanu, kliknij przycisk **Odśwież** przycisku.)</span><span class="sxs-lookup"><span data-stu-id="a7497-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="a7497-354">Uruchom aplikację do wyzwolenia kodu inicjatora określającej wartość początkową bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="a7497-355">Kliknij prawym przyciskiem myszy **CourseAssignment** tabeli i wybierz **danych widoku** Aby sprawdzić, czy zawiera dane w nim.</span><span class="sxs-lookup"><span data-stu-id="a7497-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![Dane CourseAssignment w SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="a7497-357">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a7497-357">Summary</span></span>

<span data-ttu-id="a7497-358">Masz teraz bardziej złożonych modelu danych i odpowiednią bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a7497-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="a7497-359">Następujące samouczka dowiesz się więcej na temat dostępu do danych powiązanych.</span><span class="sxs-lookup"><span data-stu-id="a7497-359">In the following tutorial, you'll learn more about how to access related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a7497-360">[Poprzednie](migrations.md)
> [dalej](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="a7497-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
