---
title: Platforma ASP.NET Core MVC z programem EF Core — Model danych — 5, 10
author: rick-anderson
description: W tym samouczku należy dodać większą liczbę jednostek i relacji i Dostosuj model danych, określając formatowania i sprawdzania poprawności i reguł mapowania.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 3714cf7ce705a52653394319fef1728a6ddcc3ee
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011773"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a><span data-ttu-id="ed653-103">Platforma ASP.NET Core MVC z programem EF Core — Model danych — 5, 10</span><span class="sxs-lookup"><span data-stu-id="ed653-103">ASP.NET Core MVC with EF Core - Data Model - 5 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ed653-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed653-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed653-105">Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed653-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="ed653-106">Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="ed653-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="ed653-107">W poprzednich samouczkach doświadczenie w pracy z modelu prostego danych, który został składające się z trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed653-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="ed653-108">W tym samouczku dodasz więcej jednostek i relacji i będzie Dostosuj model danych, określając formatowania i sprawdzania poprawności i reguł mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="ed653-109">Gdy skończysz, klas jednostek tworzących model danych ukończone, który jest wyświetlany na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="ed653-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="ed653-111">Dostosuj Model danych przy użyciu atrybutów</span><span class="sxs-lookup"><span data-stu-id="ed653-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="ed653-112">W tej sekcji pokazano, jak dostosować model danych przy użyciu atrybutów, które określają, formatowanie, sprawdzanie poprawności i reguł mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="ed653-113">Następnie w kilku z następujących sekcji, które zostaną utworzone pełnego modelu danych służbowych, dodając atrybutów do klasy została już utworzona oraz tworzenie nowych klas pozostałe typy jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="ed653-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="ed653-114">Atrybut typu danych</span><span class="sxs-lookup"><span data-stu-id="ed653-114">The DataType attribute</span></span>

<span data-ttu-id="ed653-115">Dat rejestracji dla uczniów wszystkie strony sieci web obecnie wyświetlić czas wraz z datą, mimo że wszystkich interesujących Cię dla tego pola jest datą.</span><span class="sxs-lookup"><span data-stu-id="ed653-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="ed653-116">Za pomocą atrybutów adnotacji danych, można go utworzyć kod zmian, który naprawi format wyświetlania w każdym widoku, który zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="ed653-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="ed653-117">Aby zobaczyć przykład sposobu wykonania, należy dodać atrybut `EnrollmentDate` właściwość `Student` klasy.</span><span class="sxs-lookup"><span data-stu-id="ed653-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="ed653-118">W *Models/Student.cs*, Dodaj `using` poufności informacji dotyczące `System.ComponentModel.DataAnnotations` przestrzeni nazw i Dodaj `DataType` i `DisplayFormat` atrybuty do `EnrollmentDate` właściwości, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="ed653-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="ed653-119">`DataType` Atrybut jest używany do określenia typu danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-119">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="ed653-120">W tym przypadku ma być uruchamiany tylko do śledzenia daty, nie daty i godziny.</span><span class="sxs-lookup"><span data-stu-id="ed653-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="ed653-121">`DataType` Wyliczenie udostępnia wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress i więcej.</span><span class="sxs-lookup"><span data-stu-id="ed653-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="ed653-122">`DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed653-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="ed653-123">Na przykład `mailto:` łącza mogą być tworzone dla `DataType.EmailAddress`, i można podać selektora daty `DataType.Date` w przeglądarkach obsługujących HTML5.</span><span class="sxs-lookup"><span data-stu-id="ed653-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="ed653-124">`DataType` Atrybut emituje HTML 5 `data-` atrybutów (Wymowa: dane dash), które może zrozumieć przeglądarki HTML 5.</span><span class="sxs-lookup"><span data-stu-id="ed653-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="ed653-125">`DataType` Atrybuty nie zawierają żadnych sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="ed653-125">The `DataType` attributes don't provide any validation.</span></span>

<span data-ttu-id="ed653-126">`DataType.Date` nie określa format daty, która jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="ed653-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="ed653-127">Domyślnie pole danych są wyświetlane domyślne formaty oparte na serwerze CultureInfo.</span><span class="sxs-lookup"><span data-stu-id="ed653-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="ed653-128">`DisplayFormat` Atrybut jest używany jawnie określić format daty:</span><span class="sxs-lookup"><span data-stu-id="ed653-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="ed653-129">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinien również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji.</span><span class="sxs-lookup"><span data-stu-id="ed653-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="ed653-130">(Nie można w przypadku niektórych pól — na przykład dla wartości waluty, może nie przewidziane symbol waluty w polu tekstowym do edycji.)</span><span class="sxs-lookup"><span data-stu-id="ed653-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="ed653-131">Możesz użyć `DisplayFormat` atrybutu przez sam, ale zazwyczaj jest dobry pomysł, aby użyć `DataType` również atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ed653-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="ed653-132">`DataType` Atrybut umożliwia przekazywanie semantykę dane, a nie jak renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać za pomocą `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="ed653-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="ed653-133">Przeglądarka można włączyć funkcje HTML5 (na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy kilka po stronie klienta. dane wejściowe sprawdzania poprawności, itp.).</span><span class="sxs-lookup"><span data-stu-id="ed653-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="ed653-134">Domyślnie przeglądarka wyświetli dane przy użyciu poprawny format, w oparciu o ustawienia regionalne.</span><span class="sxs-lookup"><span data-stu-id="ed653-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="ed653-135">Aby uzyskać więcej informacji, zobacz [ \<wejściowych > dokumentacja pomocnika tagów](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ed653-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="ed653-136">Uruchom aplikację, przejdź do strony indeksu studentów i zwróć uwagę, nie są wyświetlane godziny dla daty rejestracji.</span><span class="sxs-lookup"><span data-stu-id="ed653-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="ed653-137">Będzie taka sama wartość true dla dowolnego widoku, który używa modelu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="ed653-137">The same will be true for any view that uses the Student model.</span></span>

![Wyświetlanie daty bez godziny strony indeksu uczniów](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="ed653-139">Atrybut StringLength</span><span class="sxs-lookup"><span data-stu-id="ed653-139">The StringLength attribute</span></span>

<span data-ttu-id="ed653-140">Można również określić reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="ed653-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="ed653-141">`StringLength` Atrybut Ustawia maksymalną długość w bazie danych i zapewnia po stronie klienta i po stronie serwera sprawdzania poprawności dla platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ed653-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET Core MVC.</span></span> <span data-ttu-id="ed653-142">Minimalna długość ciągu można również określić, w tym atrybucie, ale wartość minimalna nie ma wpływu na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="ed653-143">Załóżmy, że chcesz upewnić się, że użytkownicy nie wprowadzić więcej niż 50 znaków dla nazwy.</span><span class="sxs-lookup"><span data-stu-id="ed653-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="ed653-144">Aby dodać to ograniczenie `StringLength` atrybuty do `LastName` i `FirstMidName` właściwości, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="ed653-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="ed653-145">`StringLength` Atrybutu nie uniemożliwić wprowadzanie białe miejsca dla nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ed653-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="ed653-146">Możesz użyć `RegularExpression` atrybutu, aby zastosować ograniczenia do danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="ed653-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="ed653-147">Na przykład poniższy kod wymaga pierwszy znak na wielkie litery, a pozostałe znaki, aby być znakiem alfabetycznym:</span><span class="sxs-lookup"><span data-stu-id="ed653-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="ed653-148">`MaxLength` Atrybut oferuje funkcje podobne do `StringLength` atrybutu, ale nie zapewnia po stronie klienta sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="ed653-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="ed653-149">Model bazy danych zostanie zmieniona w taki sposób, że wymaga zmiany w schemacie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="ed653-150">Użyjesz migracji do zaktualizowania schematu bez utraty danych, która może być została dodana do bazy danych przy użyciu interfejsu użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed653-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="ed653-151">Zapisz zmiany i skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="ed653-151">Save your changes and build the project.</span></span> <span data-ttu-id="ed653-152">Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ed653-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="ed653-153">`migrations add` Polecenia wyświetli ostrzeżenie, że może dojść do utraty danych, ponieważ ta zmiana powoduje maksymalną długość krótszy dwie kolumny.</span><span class="sxs-lookup"><span data-stu-id="ed653-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="ed653-154">Migracje tworzy plik o nazwie  *\<sygnatura czasowa > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="ed653-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="ed653-155">Ten plik zawiera kod w `Up` metodę, która spowoduje zaktualizowanie bazy danych, aby dopasować bieżącym modelem danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="ed653-156">`database update` Polecenie zadziałało kod.</span><span class="sxs-lookup"><span data-stu-id="ed653-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="ed653-157">Sygnatura czasowa prefiks do nazwy pliku migracji umożliwia przez program Entity Framework kolejność migracji.</span><span class="sxs-lookup"><span data-stu-id="ed653-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="ed653-158">Możesz utworzyć wiele migracji przed uruchomieniem polecenia update-database, a następnie wszystkie migracje są stosowane w kolejności, w którym zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="ed653-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="ed653-159">Uruchom aplikację, wybierz **studentów** kliknij pozycję **Utwórz nowy**, a następnie wprowadź nazwę, albo więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="ed653-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="ed653-160">Po kliknięciu **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="ed653-160">When you click **Create**, client side validation shows an error message.</span></span>

![Studenci indeksu strona wyświetlająca błędy długość ciągu](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="ed653-162">Atrybut kolumny</span><span class="sxs-lookup"><span data-stu-id="ed653-162">The Column attribute</span></span>

<span data-ttu-id="ed653-163">Umożliwia także atrybuty kontrolować, jak klas i właściwości są mapowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="ed653-164">Załóżmy, że używasz nazwy `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać imienia.</span><span class="sxs-lookup"><span data-stu-id="ed653-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="ed653-165">Jednak kolumna bazy danych, można go nazwać `FirstName`, ponieważ użytkownicy, którzy będą Pisanie zapytań ad hoc w bazie danych są przyzwyczajeni do tej nazwy.</span><span class="sxs-lookup"><span data-stu-id="ed653-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="ed653-166">Aby to mapowanie, można użyć `Column` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ed653-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="ed653-167">`Column` Atrybut określa, że po utworzeniu bazy danych, kolumna `Student` tabelę, która mapuje do `FirstMidName` właściwość o nazwie `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="ed653-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="ed653-168">Innymi słowy, gdy kod odwołuje się do `Student.FirstMidName`, dane będą pochodzić z lub aktualizowane w `FirstName` kolumny `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="ed653-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="ed653-169">Jeśli nie określisz nazwy kolumn, mają one taką samą nazwę jak nazwa właściwości.</span><span class="sxs-lookup"><span data-stu-id="ed653-169">If you don't specify column names, they're given the same name as the property name.</span></span>

<span data-ttu-id="ed653-170">W *Student.cs* Dodaj `using` poufności informacji dotyczące `System.ComponentModel.DataAnnotations.Schema` i Dodaj atrybut nazwy kolumny, aby `FirstMidName` właściwości, jak pokazano na następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="ed653-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="ed653-171">Dodanie `Column` atrybut zmieni się zapasowy modelu `SchoolContext`, dzięki czemu nie będzie ona zgodna z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="ed653-172">Zapisz zmiany i skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="ed653-172">Save your changes and build the project.</span></span> <span data-ttu-id="ed653-173">Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenia, aby utworzyć inną migracji:</span><span class="sxs-lookup"><span data-stu-id="ed653-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="ed653-174">W **Eksplorator obiektów SQL Server**, otwórz projektanta tabel dla uczniów, klikając dwukrotnie **uczniów** tabeli.</span><span class="sxs-lookup"><span data-stu-id="ed653-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="ed653-176">Przed zastosowaniem pierwsze dwie migracje nazwa kolumny były typu nvarchar(MAX).</span><span class="sxs-lookup"><span data-stu-id="ed653-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="ed653-177">Są one teraz nvarchar(50) i nazwę kolumny został zmieniony z FirstMidName na imię.</span><span class="sxs-lookup"><span data-stu-id="ed653-177">They're now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="ed653-178">Jeśli spróbujesz skompilować przed zakończeniem, tworzenie wszystkich klas jednostek w poniższych sekcjach, możesz otrzymać błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="ed653-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="ed653-179">Ostateczne zmiany do jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="ed653-179">Final changes to the Student entity</span></span>

![Jednostki dla uczniów](complex-data-model/_static/student-entity.png)

<span data-ttu-id="ed653-181">W *Models/Student.cs*, Zastąp kod został dodany wcześniej następujący kod.</span><span class="sxs-lookup"><span data-stu-id="ed653-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="ed653-182">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="ed653-182">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="ed653-183">Wymagany atrybut</span><span class="sxs-lookup"><span data-stu-id="ed653-183">The Required attribute</span></span>

<span data-ttu-id="ed653-184">`Required` Atrybutu sprawia, że nazwa właściwości wymagane pola.</span><span class="sxs-lookup"><span data-stu-id="ed653-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="ed653-185">`Required` Atrybut nie jest wymagane dla typów innych niż null, takich jak typy wartości (daty/godziny, int, dwukrotnie, float, itp.).</span><span class="sxs-lookup"><span data-stu-id="ed653-185">The `Required` attribute isn't needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="ed653-186">Typy, które nie może mieć wartości null są automatycznie traktowane jako wymagane pola.</span><span class="sxs-lookup"><span data-stu-id="ed653-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="ed653-187">Można usunąć `Required` atrybutu i zastąp go minimalną długość parametru `StringLength` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="ed653-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="ed653-188">Atrybut wyświetlania</span><span class="sxs-lookup"><span data-stu-id="ed653-188">The Display attribute</span></span>

<span data-ttu-id="ed653-189">`Display` Atrybut określa, czy podpis dla pól tekstowych powinny być "Imię", "Last Name", "Pełna nazwa" i "Data rejestracji" zamiast nazwy właściwości, w każdym wystąpieniu (który nie ma miejsca dzielenia wyrazów).</span><span class="sxs-lookup"><span data-stu-id="ed653-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="ed653-190">Właściwości obliczane imię i nazwisko</span><span class="sxs-lookup"><span data-stu-id="ed653-190">The FullName calculated property</span></span>

<span data-ttu-id="ed653-191">`FullName` jest właściwością obliczeniową, która zwraca wartość, która jest tworzona przez dołączenie dwóch innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="ed653-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="ed653-192">Dlatego ma tylko akcesor pobierania, a nie `FullName` kolumny zostanie wygenerowany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="ed653-193">Tworzenie jednostki przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="ed653-193">Create the Instructor Entity</span></span>

![Jednostki przez instruktorów](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="ed653-195">Tworzenie *Models/Instructor.cs*, zastępując kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="ed653-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="ed653-196">Należy zauważyć, że kilka właściwości są takie same, w jednostkach studenta i programistę-instruktora.</span><span class="sxs-lookup"><span data-stu-id="ed653-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="ed653-197">W [Implementowanie dziedziczenia](inheritance.md) samouczek w dalszej części tej serii, będzie Refaktoryzuj ten kod w celu wyeliminowania nadmiarowości.</span><span class="sxs-lookup"><span data-stu-id="ed653-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="ed653-198">Wiele atrybutów można umieścić w jednym wierszu, dzięki czemu można również napisać `HireDate` atrybutów w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ed653-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="ed653-199">Właściwości nawigacji CourseAssignments i OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="ed653-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="ed653-200">`CourseAssignments` i `OfficeAssignment` właściwości są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ed653-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="ed653-201">Pod kierunkiem instruktora nauczyć dowolnej liczby kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="ed653-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="ed653-202">Właściwość nawigacji może zawierać wiele jednostek, listy, w którym wpisy mogą być dodawane, usuwane lub zaktualizować musi być typu.</span><span class="sxs-lookup"><span data-stu-id="ed653-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="ed653-203">Można określić `ICollection<T>` lub typu, takie jak `List<T>` lub `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="ed653-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="ed653-204">Jeśli określisz `ICollection<T>`, tworzy EF `HashSet<T>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ed653-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="ed653-205">Powód, dlaczego są `CourseAssignment` jednostek zostało wyjaśnione poniżej w sekcji o relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="ed653-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="ed653-206">Reguły biznesowe firmy Contoso University stanu, że pod kierunkiem instruktora może mieć tylko co najwyżej jednego pakietu office, więc `OfficeAssignment` właściwość przechowuje pojedynczej jednostki OfficeAssignment, (które mogą być wartość null, jeśli nie przypisano żadnych pakietu office).</span><span class="sxs-lookup"><span data-stu-id="ed653-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="ed653-207">Tworzenie jednostki OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="ed653-207">Create the OfficeAssignment entity</span></span>

![OfficeAssignment jednostki](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="ed653-209">Tworzenie *Models/OfficeAssignment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ed653-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="ed653-210">Atrybut klucza</span><span class="sxs-lookup"><span data-stu-id="ed653-210">The Key attribute</span></span>

<span data-ttu-id="ed653-211">Brak relacji jeden do zero lub jeden między instruktora oraz OfficeAssignment jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed653-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="ed653-212">Biuro istnieje tylko w odniesieniu do przez instruktorów, który jest przypisany do, a zatem jej klucz podstawowy również jest jego klucza obcego z jednostką instruktora.</span><span class="sxs-lookup"><span data-stu-id="ed653-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="ed653-213">Ale Entity Framework automatycznie nie może rozpoznać InstructorID jako klucz podstawowy dla tej jednostki, ponieważ jego nazwa nie przestrzegać konwencji nazewnictwa identyfikator lub classnameID.</span><span class="sxs-lookup"><span data-stu-id="ed653-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="ed653-214">W związku z tym `Key` atrybut jest używany do identyfikowania jej jako klucza:</span><span class="sxs-lookup"><span data-stu-id="ed653-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="ed653-215">Można również użyć `Key` atrybutu, jeśli jednostka ma swój własny klucz podstawowy, ale chcesz nazwać właściwość, coś innego niż classnameID lub identyfikatora organizacji.</span><span class="sxs-lookup"><span data-stu-id="ed653-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="ed653-216">Domyślnie program EF traktuje klucza jako inne niż wygenerowane bazy danych, ponieważ kolumna jest przeznaczona dla relacji identyfikującej.</span><span class="sxs-lookup"><span data-stu-id="ed653-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="ed653-217">Właściwość nawigacji przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="ed653-217">The Instructor navigation property</span></span>

<span data-ttu-id="ed653-218">Jednostka przez instruktorów ma wartość null `OfficeAssignment` właściwości nawigacji (ponieważ pod kierunkiem instruktora, może nie mieć biuro), i jednostka OfficeAssignment ma wartość null `Instructor` właściwości nawigacji (ponieważ nie przypisania pakietu office istnieje bez pod kierunkiem instruktora — `InstructorID` nie dopuszcza wartości null).</span><span class="sxs-lookup"><span data-stu-id="ed653-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="ed653-219">Jednostka przez instruktorów ma powiązanej jednostki OfficeAssignment, każda jednostka ma odwołania do innych niż w jego właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ed653-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="ed653-220">Możesz umieścić `[Required]` atrybutu właściwość nawigacji przez instruktorów, aby określić, że muszą być powiązane przez instruktorów, ale nie trzeba tego robić, ponieważ `InstructorID` nie dopuszczają wartości klucza obcego, (który również jest kluczem do tej tabeli).</span><span class="sxs-lookup"><span data-stu-id="ed653-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="ed653-221">Modyfikowanie jednostek kursu</span><span class="sxs-lookup"><span data-stu-id="ed653-221">Modify the Course Entity</span></span>

![Kurs jednostki](complex-data-model/_static/course-entity.png)

<span data-ttu-id="ed653-223">W *Models/Course.cs*, Zastąp kod został dodany wcześniej następujący kod.</span><span class="sxs-lookup"><span data-stu-id="ed653-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="ed653-224">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="ed653-224">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="ed653-225">Jednostka kurs ma właściwość klucza obcego `DepartmentID` który wskazuje na powiązanej jednostce działu i ma `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ed653-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="ed653-226">Entity Framework nie wymaga dodanie właściwości klucza obcego w modelu danych mają właściwości nawigacji dla powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="ed653-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="ed653-227">EF automatycznie tworzy klucze obce w bazie danych wszędzie tam, gdzie będą one potrzebne i tworzy [w tle właściwości](https://docs.microsoft.com/ef/core/modeling/shadow-properties) dla nich.</span><span class="sxs-lookup"><span data-stu-id="ed653-227">EF automatically creates foreign keys in the database wherever they're needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="ed653-228">Ale o klucza obcego w modelu danych, że aktualizacje są prostszy i efektywniejszy.</span><span class="sxs-lookup"><span data-stu-id="ed653-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="ed653-229">Na przykład, gdy fetch jednostkę kurs, aby edytować jednostki działu ma wartość null Jeśli nie zostanie załadowany, więc po zaktualizowaniu jednostki kurs, trzeba najpierw pobrać jednostki działu.</span><span class="sxs-lookup"><span data-stu-id="ed653-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="ed653-230">Gdy właściwość klucza obcego `DepartmentID` znajduje się w modelu danych nie trzeba pobrać jednostki działu, przed uaktualnieniem.</span><span class="sxs-lookup"><span data-stu-id="ed653-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="ed653-231">Atrybut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="ed653-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="ed653-232">`DatabaseGenerated` Atrybutem `None` parametru `CourseID` właściwość określa, że wartości klucza podstawowego są dostarczone przez użytkownika, a nie generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="ed653-233">Domyślnie program Entity Framework przyjęto założenie, że wartości klucza podstawowego są generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="ed653-234">To, co ma w większości scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="ed653-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="ed653-235">Jednak w przypadku jednostek kurs będzie numer kurs określonych przez użytkownika, takich jak serie 1000 służy do jednego działu, serię 2000 do innego działu i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="ed653-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="ed653-236">`DatabaseGenerated` Atrybut może służyć także do generowania wartości domyślne, jak w przypadku kolumny bazy danych używane do rejestrowania Data wiersz został utworzony lub zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="ed653-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="ed653-237">Aby uzyskać więcej informacji, zobacz [wygenerowanych właściwości](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="ed653-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ed653-238">Obcy właściwości klucza i nawigacja</span><span class="sxs-lookup"><span data-stu-id="ed653-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="ed653-239">Właściwości klucza obcego i właściwości nawigacji w jednostce kurs odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="ed653-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="ed653-240">Kurs jest przypisany do jednego działu, więc ma `DepartmentID` klucza obcego i `Department` właściwość nawigacji z powodów wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="ed653-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="ed653-241">Kurs może mieć dowolną liczbę uczniów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="ed653-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="ed653-242">Kurs może być prowadzone przez instruktorów wielu, więc `CourseAssignments` właściwość nawigacji jest kolekcją (typ `CourseAssignment` zostało wyjaśnione [później](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="ed653-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="ed653-243">Tworzenie jednostki działu</span><span class="sxs-lookup"><span data-stu-id="ed653-243">Create the Department entity</span></span>

![Dział jednostki](complex-data-model/_static/department-entity.png)


<span data-ttu-id="ed653-245">Tworzenie *Models/Department.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ed653-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="ed653-246">Atrybut kolumny</span><span class="sxs-lookup"><span data-stu-id="ed653-246">The Column attribute</span></span>

<span data-ttu-id="ed653-247">Wcześniej użyto `Column` atrybutu, aby zmienić mapowanie nazwy kolumny.</span><span class="sxs-lookup"><span data-stu-id="ed653-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="ed653-248">W kodzie dla jednostki działu `Column` atrybut jest używany do zmienić mapowanie typu danych SQL, tak aby kolumna zostanie zdefiniowana przy użyciu typu pieniądze programu SQL Server w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="ed653-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="ed653-249">Mapowanie kolumn zazwyczaj nie jest wymagana, ponieważ Entity Framework wybiera odpowiedni typ danych programu SQL Server, na podstawie typu CLR, który zdefiniujesz dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="ed653-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="ed653-250">Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu.</span><span class="sxs-lookup"><span data-stu-id="ed653-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="ed653-251">Ale w takim przypadku wiesz, że kolumna będzie zawierający kwot i pieniędzy typ danych jest bardziej odpowiednia dla tego.</span><span class="sxs-lookup"><span data-stu-id="ed653-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ed653-252">Obcy właściwości klucza i nawigacja</span><span class="sxs-lookup"><span data-stu-id="ed653-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="ed653-253">Obcy właściwości klucza i nawigacji odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="ed653-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="ed653-254">Dział może lub nie ma uprawnienia administratora, a administrator zawsze ma pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="ed653-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="ed653-255">W związku z tym `InstructorID` właściwości jest dołączana jako kluczem obcym jednostki przez instruktorów, a znak zapytania są dodawane po `int` wpisz oznaczenia, aby oznaczyć właściwości jako dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="ed653-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="ed653-256">Właściwość nawigacji jest o nazwie `Administrator` , ale zawiera jednostki przez instruktorów:</span><span class="sxs-lookup"><span data-stu-id="ed653-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="ed653-257">Dział może mieć wielu kursów, więc ma właściwości nawigacji kursy:</span><span class="sxs-lookup"><span data-stu-id="ed653-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="ed653-258">Zgodnie z Konwencją platformy Entity Framework umożliwia usuwanie kaskadowe nieprzyjmujące wartości kluczy obcych i w przypadku relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="ed653-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="ed653-259">Może to spowodować cykliczne cascade delete reguł, które spowoduje wyjątek podczas próby dodania do migracji.</span><span class="sxs-lookup"><span data-stu-id="ed653-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="ed653-260">Na przykład jeśli właściwość Department.InstructorID nie zostały zdefiniowane jako dopuszczającego wartość null, EF konfigurowania cascade, Usuń regułę można usunąć instruktora, gdy usuniesz działu, która nie ma być możliwe.</span><span class="sxs-lookup"><span data-stu-id="ed653-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which isn't what you want to have happen.</span></span> <span data-ttu-id="ed653-261">W razie potrzeby własne reguły biznesowe `InstructorID` właściwość jako wartość null, trzeba wyłączyć przy użyciu następującej instrukcji interfejsu API fluent usuwanie kaskadowe relacji:</span><span class="sxs-lookup"><span data-stu-id="ed653-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="ed653-262">Modyfikowanie jednostek rejestracji</span><span class="sxs-lookup"><span data-stu-id="ed653-262">Modify the Enrollment entity</span></span>

![Jednostki rejestracji](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="ed653-264">W *Models/Enrollment.cs*, Zastąp kod został dodany wcześniej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="ed653-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ed653-265">Obcy właściwości klucza i nawigacja</span><span class="sxs-lookup"><span data-stu-id="ed653-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="ed653-266">Właściwości klucza obcego i właściwości nawigacji odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="ed653-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="ed653-267">Rekord rejestracji jest jednego kursu, więc ma `CourseID` właściwość klucza obcego i `Course` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="ed653-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="ed653-268">Rekord rejestracji jest dla uczniów lub studentów pojedynczego, więc ma `StudentID` właściwość klucza obcego i `Student` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="ed653-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="ed653-269">Relacje wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="ed653-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="ed653-270">Istnieje relacja wiele do wielu między jednostkami studenta i programistę-kurs i jednostki rejestrowanie funkcji jako tabelę sprzężenia wiele do wielu *z ładunkiem* w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="ed653-271">"Z ładunkiem" oznacza, że tabela rejestracji zawiera dane dodatkowe, oprócz klucze obce w dołączonym do tabel (w tym przypadku klucz podstawowy i właściwości klasy korporacyjnej).</span><span class="sxs-lookup"><span data-stu-id="ed653-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="ed653-272">Poniższa ilustracja przedstawia, jak wyglądają te relacje w diagramie jednostki.</span><span class="sxs-lookup"><span data-stu-id="ed653-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="ed653-273">(Ten diagram został wygenerowany za pomocą narzędzi Entity Framework Power Tools dla platformy EF 6.x; tworzenia diagramu nie należały do tego samouczka, po prostu jest on używany tutaj jako ilustrację.)</span><span class="sxs-lookup"><span data-stu-id="ed653-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Kurs dla uczniów wiele do wielu relacji](complex-data-model/_static/student-course.png)

<span data-ttu-id="ed653-275">Każdy wiersz relacji ma 1 na jednym końcu i znak gwiazdki (\*) na drugim, wskazując relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="ed653-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="ed653-276">Jeśli tabela rejestracji nie obejmują informacji klasy korporacyjnej, czy tylko musi on zawierać dwa klucze obce CourseID i StudentID.</span><span class="sxs-lookup"><span data-stu-id="ed653-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="ed653-277">W takim przypadku byłoby tabelę sprzężenia wiele do wielu, bez ładunku (lub tabelę sprzężenia czystego) w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="ed653-278">Jednostki instruktora oraz przypisane kurs mają tego rodzaju relacji wiele do wielu i następnym krokiem jest utworzenie klasę jednostki działa jak sprzężenie tabeli bez ładunku.</span><span class="sxs-lookup"><span data-stu-id="ed653-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="ed653-279">(Tabele niejawne sprzężenia dla relacji wiele do wielu, ale programu EF Core nie obsługuje 6.x EF.</span><span class="sxs-lookup"><span data-stu-id="ed653-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="ed653-280">Aby uzyskać więcej informacji, zobacz [dyskusji w repozytorium programu EF Core w usłudze GitHub](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="ed653-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="ed653-281">Jednostka CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="ed653-281">The CourseAssignment entity</span></span>

![CourseAssignment jednostki](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="ed653-283">Tworzenie *Models/CourseAssignment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ed653-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="ed653-284">Dołącz do nazwy podmiotu</span><span class="sxs-lookup"><span data-stu-id="ed653-284">Join entity names</span></span>

<span data-ttu-id="ed653-285">Tabelę sprzężenia jest wymagana w bazie danych dla relacji wiele do wielu instruktora do kursów, a musi być reprezentowany przez zestaw jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed653-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="ed653-286">Często jest nazwa jednostki sprzężenia `EntityName1EntityName2`, który w tym przypadku wyniesie `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="ed653-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="ed653-287">Jednak zaleca się, że wybierz nazwę, która opisuje relację.</span><span class="sxs-lookup"><span data-stu-id="ed653-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="ed653-288">Modele danych zaczynają prosty i rozwój za pomocą sprzężeń ładunku nie często uzyskiwanie ładunków później.</span><span class="sxs-lookup"><span data-stu-id="ed653-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="ed653-289">W przypadku uruchomienia pod nazwą opisową jednostki, nie musisz później zmienić nazwę.</span><span class="sxs-lookup"><span data-stu-id="ed653-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="ed653-290">W idealnym przypadku jednostki sprzężenia będzie mieć własną fizyczną nazwę (prawdopodobnie jednowyrazowego) w domenie firmy.</span><span class="sxs-lookup"><span data-stu-id="ed653-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="ed653-291">Na przykład książek i klientów można połączyć za pomocą klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="ed653-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="ed653-292">Dla tej relacji `CourseAssignment` jest lepszym rozwiązaniem niż `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="ed653-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="ed653-293">Klucz złożony</span><span class="sxs-lookup"><span data-stu-id="ed653-293">Composite key</span></span>

<span data-ttu-id="ed653-294">Ponieważ klucze obce nie dopuszcza wartości null i razem jednoznacznie identyfikują poszczególne wiersze tabeli, nie ma potrzeby oddzielnych klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ed653-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there's no need for a separate primary key.</span></span> <span data-ttu-id="ed653-295">*InstructorID* i *CourseID* właściwości powinny działać jako złożony klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="ed653-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="ed653-296">Jedynym sposobem, aby zidentyfikować złożonego kluczy podstawowych do programów EF polega na użyciu *interfejsu API fluent* (go nie może odbywać się przy użyciu atrybutów).</span><span class="sxs-lookup"><span data-stu-id="ed653-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="ed653-297">Pokazano, jak skonfigurować złożony klucz podstawowy w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="ed653-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="ed653-298">Klucz złożony gwarantuje, że kiedy masz wiele wierszy do jednego kursu i wiele wierszy dla jednego przez instruktorów, nie może mieć wiele wierszy dla tego samego przez instruktorów i kurs.</span><span class="sxs-lookup"><span data-stu-id="ed653-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="ed653-299">`Enrollment` Sprzężenia jednostka definiuje swój własny klucz podstawowy, więc możliwe są duplikatami tego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="ed653-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="ed653-300">Aby uniknąć takich duplikaty, możesz można dodać unikatowego indeksu na pola kluczy obcych, lub skonfigurować `Enrollment` przy użyciu podstawowego klucza złożonego podobne do `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="ed653-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="ed653-301">Aby uzyskać więcej informacji, zobacz [indeksów](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="ed653-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="ed653-302">Aktualizowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="ed653-302">Update the database context</span></span>

<span data-ttu-id="ed653-303">Dodaj następujący wyróżniony kod do *Data/SchoolContext.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="ed653-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="ed653-304">Ten kod dodaje nowe jednostki i konfiguruje złożony klucz podstawowy jednostki CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="ed653-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="ed653-305">Zamiast interfejsu API Fluent atrybutów</span><span class="sxs-lookup"><span data-stu-id="ed653-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="ed653-306">Kod w `OnModelCreating` metody `DbContext` klasy używa *interfejsu API fluent* do konfigurowania zachowania EF.</span><span class="sxs-lookup"><span data-stu-id="ed653-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="ed653-307">Interfejs API jest nazywany "fluent", ponieważ jest ona często używana przez centrali szereg wywołań metod, które razem w pojedynczej instrukcji, jak w poniższym przykładzie z [dokumentacji programu EF Core](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="ed653-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="ed653-308">W tym samouczku używasz interfejsu API fluent tylko dla mapowania bazy danych, nie można wykonać za pomocą atrybutów.</span><span class="sxs-lookup"><span data-stu-id="ed653-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="ed653-309">Jednak umożliwia także wygodnego interfejsu API do określenia większość formatowania, sprawdzanie poprawności i reguł mapowania, które można wykonać przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="ed653-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="ed653-310">Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować za pomocą interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="ed653-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="ed653-311">Jak wspomniano wcześniej, `MinimumLength` nie zmienił się schemat, ma zastosowanie tylko reguły walidacji po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="ed653-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="ed653-312">Niektórzy deweloperzy wolą używać interfejsu API fluent, wyłącznie tak, aby ich zachowania ich klasami jednostki "Wyczyść".</span><span class="sxs-lookup"><span data-stu-id="ed653-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="ed653-313">Jeśli chcesz, a kilka dostosowania, które jest możliwe tylko przy użyciu interfejsu API fluent można łączyć atrybutów i wygodnego interfejsu API, ale ogólnie rzecz biorąc zalecaną praktyką jest, wybierz jedną z tych dwóch metod i używać oznacza spójnie możliwie.</span><span class="sxs-lookup"><span data-stu-id="ed653-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="ed653-314">Jeśli używasz obu, należy pamiętać, że wszędzie tam, gdzie występuje konflikt, interfejsu API Fluent zastępuje atrybuty.</span><span class="sxs-lookup"><span data-stu-id="ed653-314">If you do use both, note that wherever there's a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="ed653-315">Aby uzyskać więcej informacji na temat atrybutów, a interfejs fluent API zobacz [metod konfiguracji](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="ed653-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="ed653-316">Jednostki Diagram przedstawiający relacje</span><span class="sxs-lookup"><span data-stu-id="ed653-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="ed653-317">Poniższa ilustracja przedstawia diagram, który narzędzi Entity Framework Power Tools Tworzenie modelu ukończone szkoły.</span><span class="sxs-lookup"><span data-stu-id="ed653-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="ed653-319">Oprócz liniach relacji jeden do wielu (1, aby \*), możesz teraz zobaczyć wiersz relacji jeden do zero lub jeden (1 się od 0 do 1) między jednostkami instruktora oraz przypisane OfficeAssignment i wiersz relacji zero lub jeden do wielu (od 0 do 1 do \*) między Jednostki przez instruktorów i działu.</span><span class="sxs-lookup"><span data-stu-id="ed653-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to \*) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="ed653-320">Inicjowanie bazy danych przy użyciu danych testowych</span><span class="sxs-lookup"><span data-stu-id="ed653-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="ed653-321">Zastąp kod w *Data/DbInitializer.cs* pliku następującym kodem w celu dostarczenia danych inicjatora dla nowych jednostek, które zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="ed653-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="ed653-322">Jak przedstawiono w pierwszym samouczku większość ten kod po prostu tworzy nowe obiekty jednostki i ładuje przykładowych danych do właściwości zgodnie z potrzebami testowania.</span><span class="sxs-lookup"><span data-stu-id="ed653-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="ed653-323">Zwróć uwagę, jak są obsługiwane w relacji wiele do wielu: kod tworzy relacje, tworząc jednostek w `Enrollments` i `CourseAssignment` Dołącz zestawy jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed653-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="ed653-324">Dodaj migrację</span><span class="sxs-lookup"><span data-stu-id="ed653-324">Add a migration</span></span>

<span data-ttu-id="ed653-325">Zapisz zmiany i skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="ed653-325">Save your changes and build the project.</span></span> <span data-ttu-id="ed653-326">Następnie otwórz okno polecenia w folderze projektu i wprowadź `migrations add` polecenie (nie jeszcze nie robi polecenia update-database):</span><span class="sxs-lookup"><span data-stu-id="ed653-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="ed653-327">Zostanie wyświetlone ostrzeżenie o możliwej utracie danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="ed653-328">Jeśli próbowano uruchomić `database update` polecenia w tym momencie (nie rób tego jeszcze), czy występuje następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="ed653-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="ed653-329">Instrukcja ALTER TABLE konflikt z ograniczeniem klucza OBCEGO "FK_dbo. Course_dbo. Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="ed653-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="ed653-330">Konflikt wystąpił w bazie danych "ContosoUniversity" table "dbo. Dział", kolumna"DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="ed653-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="ed653-331">Czasami, podczas wykonywania migracji z istniejącymi danymi, należy wstawić dane do bazy danych, aby spełnić ograniczeń klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="ed653-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="ed653-332">Kod wygenerowany w `Up` metoda dodaje klucz obcy dopuszcza DepartmentID do tabeli kursu.</span><span class="sxs-lookup"><span data-stu-id="ed653-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="ed653-333">Jeśli istnieją już wierszy w tabeli kursu po uruchomieniu kodu `AddColumn` operacja zakończy się niepowodzeniem, ponieważ program SQL Server nie może ustalić, jakie wartości do umieszczenia w kolumnie, która nie może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="ed653-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="ed653-334">W tym samouczku będą uruchamiane migracji w nowej bazy danych, ale w aplikacji produkcyjnej trzeba migracji, obsługę istniejących danych, dlatego poniższe instrukcje przedstawiają przykłady jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="ed653-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="ed653-335">Zapewnienie tej migracji działa z istniejącymi danymi, które trzeba zmienić kod, aby nadać nową kolumnę wartości domyślnej, a następnie utwórz działu szkieletu o nazwie "Temp" jako domyślnego działu.</span><span class="sxs-lookup"><span data-stu-id="ed653-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="ed653-336">W rezultacie istniejące wiersze kurs zostaną wszystkie powiązane do działu "Temperatura", po `Up` metoda przebiegów.</span><span class="sxs-lookup"><span data-stu-id="ed653-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="ed653-337">Otwórz *{timestamp}_ComplexDataModel.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="ed653-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>

* <span data-ttu-id="ed653-338">Komentarz wiersza kodu, która dodaje kolumnę DepartmentID do tabeli kursu.</span><span class="sxs-lookup"><span data-stu-id="ed653-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="ed653-339">Dodaj następujący wyróżniony kod po kodzie, który tworzy tabelę działu:</span><span class="sxs-lookup"><span data-stu-id="ed653-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="ed653-340">W przypadku aplikacji produkcyjnych należy napisać kod lub skrypty do dodawania wierszy do działu i dotyczą wierszy kurs nowych wierszy działu.</span><span class="sxs-lookup"><span data-stu-id="ed653-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="ed653-341">Nie jest już należałoby wtedy działu "Temp" lub wartość domyślna w kolumnie Course.DepartmentID.</span><span class="sxs-lookup"><span data-stu-id="ed653-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="ed653-342">Zapisz zmiany i skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="ed653-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="ed653-343">Zmień parametry połączenia i zaktualizować bazę danych</span><span class="sxs-lookup"><span data-stu-id="ed653-343">Change the connection string and update the database</span></span>

<span data-ttu-id="ed653-344">Masz teraz nowy kod w `DbInitializer` klasę, która dodaje dane nowe jednostki do pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="ed653-345">Aby EF, Utwórz nową, pustą bazę danych, Zmień nazwę bazy danych w parametrach połączenia w *appsettings.json* ContosoUniversity3 lub innej nazwy, które nie były używane na komputerze używasz.</span><span class="sxs-lookup"><span data-stu-id="ed653-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="ed653-346">Zapisać zmiany do *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ed653-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="ed653-347">Alternatywnie można zmienić nazwy bazy danych można usunąć bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="ed653-348">Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="ed653-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="ed653-349">Po zmianie nazwy bazy danych lub usunięte z bazy danych uruchom `database update` polecenia w oknie wiersza polecenia do wykonania migracji.</span><span class="sxs-lookup"><span data-stu-id="ed653-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="ed653-350">Uruchom aplikację, aby spowodować, że `DbInitializer.Initialize` metodę, aby uruchomić i wypełnić nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="ed653-351">Otwórz bazę danych w SSOX, tak jak wcześniej, a następnie rozwiń **tabel** węzeł, aby zobaczyć, czy wszystkie tabele zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="ed653-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="ed653-352">(Jeśli nadal masz SSOX otworzyć z wcześniejszego stanu, kliknij przycisk **Odśwież** przycisku.)</span><span class="sxs-lookup"><span data-stu-id="ed653-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="ed653-354">Uruchom aplikację, aby wyzwolić kod inicjatora, który inicjowania inicjuje bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="ed653-355">Kliknij prawym przyciskiem myszy **CourseAssignment** tabeli, a następnie wybierz pozycję **dane widoku** Aby sprawdzić, czy zawiera dane w nim.</span><span class="sxs-lookup"><span data-stu-id="ed653-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![Dane CourseAssignment SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="ed653-357">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="ed653-357">Summary</span></span>

<span data-ttu-id="ed653-358">Masz teraz bardziej złożonego modelu danych i odpowiednią bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="ed653-359">W następującego samouczka dowiesz się więcej o tym, jak uzyskać dostęp do powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="ed653-359">In the following tutorial, you'll learn more about how to access related data.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="ed653-360">[Poprzednie](migrations.md)
> [dalej](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="ed653-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>
