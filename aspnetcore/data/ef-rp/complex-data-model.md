---
title: Stron razor podstawowych EF w platformy ASP.NET Core - Model danych — 5 8
author: rick-anderson
description: W tym samouczku Dodaj więcej jednostki i relacje i dostosować modelu danych, określając formatowania, sprawdzanie poprawności i mapowanie reguły.
manager: wpickett
ms.author: riande
ms.date: 10/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 2cec45afbf08e5dd379a54e780e4218bfc86d13f
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741280"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="395cb-103">Stron razor podstawowych EF w platformy ASP.NET Core - Model danych — 5 8</span><span class="sxs-lookup"><span data-stu-id="395cb-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="395cb-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="395cb-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="395cb-105">Samouczki poprzedniej pracy z modelem danych podstawowych składającą się z trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="395cb-106">W tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="395cb-106">In this tutorial:</span></span>

* <span data-ttu-id="395cb-107">Więcej jednostki i relacje są dodawane.</span><span class="sxs-lookup"><span data-stu-id="395cb-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="395cb-108">Model danych jest dostosowane przez określenie formatowania, sprawdzanie poprawności i reguły mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="395cb-109">Na poniższej ilustracji pokazano klas jednostek w modelu danych zakończone:</span><span class="sxs-lookup"><span data-stu-id="395cb-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="395cb-111">Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="395cb-111">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="395cb-112">Dostosowywanie modelu danych z atrybutami</span><span class="sxs-lookup"><span data-stu-id="395cb-112">Customize the data model with attributes</span></span>

<span data-ttu-id="395cb-113">W tej sekcji modelu danych jest dostosowane przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="395cb-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="395cb-114">Atrybut typu danych</span><span class="sxs-lookup"><span data-stu-id="395cb-114">The DataType attribute</span></span>

<span data-ttu-id="395cb-115">Na stronach uczniów obecnie Wyświetla czas Data rejestracji.</span><span class="sxs-lookup"><span data-stu-id="395cb-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="395cb-116">Zazwyczaj Data zawierają tylko data i czas nie.</span><span class="sxs-lookup"><span data-stu-id="395cb-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="395cb-117">Aktualizacja *Models/Student.cs* z następującymi wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="395cb-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="395cb-118">[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atrybut określa typ danych, który jest bardziej szczegółowy niż typ wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="395cb-119">W tym przypadku powinien zostać wyświetlony tylko data nie daty i godziny.</span><span class="sxs-lookup"><span data-stu-id="395cb-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="395cb-120">[Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takie jak data, czas, numer telefonu, waluty, EmailAddress itp. `DataType` Atrybut można również włączyć automatycznie udostępnić funkcji specyficznych dla typu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="395cb-121">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="395cb-121">For example:</span></span>

* <span data-ttu-id="395cb-122">`mailto:` Link jest tworzony automatycznie dla `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="395cb-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="395cb-123">Selektor Data jest dostępne w celu `DataType.Date` w większości przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="395cb-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="395cb-124">`DataType` HTML 5 emituje atrybut `data-` atrybutów (dash wyraźnym danych), które korzystać z przeglądarki HTML 5.</span><span class="sxs-lookup"><span data-stu-id="395cb-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="395cb-125">`DataType` Atrybutów nie mają funkcje sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="395cb-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="395cb-126">`DataType.Date` nie określono format daty, która jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="395cb-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="395cb-127">Domyślnie pole daty są wyświetlane domyślne formaty oparte na tym serwerze [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="395cb-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="395cb-128">`DisplayFormat` Atrybut służy do jawnie określić format daty:</span><span class="sxs-lookup"><span data-stu-id="395cb-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="395cb-129">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinny również będą stosowane do edycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="395cb-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="395cb-130">Niektóre pola nie powinny używać `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="395cb-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="395cb-131">Na przykład symbol waluty zazwyczaj nie powinny być wyświetlane w polu edycji.</span><span class="sxs-lookup"><span data-stu-id="395cb-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="395cb-132">`DisplayFormat` Atrybut może być używany przez samego siebie.</span><span class="sxs-lookup"><span data-stu-id="395cb-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="395cb-133">Zazwyczaj jest warto użyć `DataType` atrybutem `DisplayFormat` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="395cb-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="395cb-134">`DataType` Atrybut przekazuje semantykę danych zamiast sposób renderowania jej na ekranie.</span><span class="sxs-lookup"><span data-stu-id="395cb-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="395cb-135">`DataType` Atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="395cb-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="395cb-136">Przeglądarki, można włączyć funkcje HTML5.</span><span class="sxs-lookup"><span data-stu-id="395cb-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="395cb-137">Na przykład wyświetlić formant kalendarza, symbol waluty odpowiednie ustawienia regionalne, przesyłanie pocztą e-mail łączy, sprawdzania poprawności danych wejściowych po stronie klienta,... itd.</span><span class="sxs-lookup"><span data-stu-id="395cb-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="395cb-138">Domyślnie przeglądarki Renderowanie danych przy użyciu właściwego formatu oparte na ustawienia regionalne.</span><span class="sxs-lookup"><span data-stu-id="395cb-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="395cb-139">Aby uzyskać więcej informacji, zobacz [ \<wejściowych > pomocnika tagów dokumentacji](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="395cb-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="395cb-140">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="395cb-140">Run the app.</span></span> <span data-ttu-id="395cb-141">Przejdź do strony indeksu studenta.</span><span class="sxs-lookup"><span data-stu-id="395cb-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="395cb-142">Nie są wyświetlane godziny.</span><span class="sxs-lookup"><span data-stu-id="395cb-142">Times are no longer displayed.</span></span> <span data-ttu-id="395cb-143">Każdy widok, który używa `Student` modelu Wyświetla datę bez czasu.</span><span class="sxs-lookup"><span data-stu-id="395cb-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Wyświetlanie dat bez godzin strony indeksu uczniów lub studentów](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="395cb-145">Atrybut StringLength</span><span class="sxs-lookup"><span data-stu-id="395cb-145">The StringLength attribute</span></span>

<span data-ttu-id="395cb-146">Atrybuty można określić reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="395cb-147">[StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atrybut określa minimalną i maksymalną długość znaki, które są dozwolone w polu danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="395cb-148">`StringLength` Atrybut udostępnia również weryfikację po stronie klienta i po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="395cb-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="395cb-149">Wartość minimalna nie ma wpływu na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="395cb-150">Aktualizacja `Student` modelu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="395cb-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="395cb-151">Poprzedni kod ogranicza nazwy do nie więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="395cb-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="395cb-152">`StringLength` Atrybutu nie uniemożliwić wprowadzanie biały znak dla nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="395cb-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="395cb-153">[Wyrażenia regularnego](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atrybut jest używany, aby zastosować ograniczenia do danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="395cb-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="395cb-154">Na przykład następujący kod wymaga pierwszego znaku się wielkie litery i pozostałych znaków jako alfabetycznej:</span><span class="sxs-lookup"><span data-stu-id="395cb-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="395cb-155">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="395cb-155">Run the app:</span></span>

* <span data-ttu-id="395cb-156">Przejdź do strony studenta.</span><span class="sxs-lookup"><span data-stu-id="395cb-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="395cb-157">Wybierz **Utwórz nowy**, a następnie wprowadź nazwa jest dłuższa niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="395cb-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="395cb-158">Wybierz **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="395cb-158">Select **Create**, client-side validation shows an error message.</span></span>

![Strona wyświetlająca ciąg błędy długość indeksu uczniów lub studentów](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="395cb-160">W **Eksplorator obiektów SQL Server** (SSOX), otwórz projektanta tabel dla użytkowników domowych, klikając dwukrotnie **uczniowie** tabeli.</span><span class="sxs-lookup"><span data-stu-id="395cb-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela studentów w SSOX przed migracji](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="395cb-162">Na powyższej ilustracji przedstawiono schematu `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="395cb-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="395cb-163">Nazwa pola ma typ `nvarchar(MAX)` ponieważ migracji nie zostało uruchomione w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="395cb-164">Podczas migracji są uruchamiane w dalszej części tego samouczka, staje się nazwa pola `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="395cb-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="395cb-165">Atrybut kolumny</span><span class="sxs-lookup"><span data-stu-id="395cb-165">The Column attribute</span></span>

<span data-ttu-id="395cb-166">Atrybuty można kontrolować, jak klas i właściwości są mapowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="395cb-167">W tej sekcji `Column` atrybut służy do mapowania nazwy `FirstMidName` dla właściwości "Imię" w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="395cb-168">Po utworzeniu bazy danych, nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, gdy `Column` użyć atrybutu).</span><span class="sxs-lookup"><span data-stu-id="395cb-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="395cb-169">`Student` Model używa `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać drugie imię.</span><span class="sxs-lookup"><span data-stu-id="395cb-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="395cb-170">Aktualizacja *Student.cs* pliku następującym kodem wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="395cb-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="395cb-171">Z tej zmiany `Student.FirstMidName` w aplikacji mapowana `FirstName` kolumny `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="395cb-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="395cb-172">Dodanie `Column` atrybutu zmieni model obsługujący `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="395cb-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="395cb-173">Model obsługujący `SchoolContext` nie jest już zgodny z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="395cb-174">Jeśli aplikacja jest uruchamiana przed zastosowaniem migracji, generowany jest następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="395cb-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="395cb-175">Aby zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="395cb-175">To update the DB:</span></span>

* <span data-ttu-id="395cb-176">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="395cb-176">Build the project.</span></span>
* <span data-ttu-id="395cb-177">Otwórz okno polecenia w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="395cb-177">Open a command window in the project folder.</span></span> <span data-ttu-id="395cb-178">Wprowadź następujące polecenia do tworzenia nowych migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="395cb-178">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="395cb-179">`dotnet ef migrations add ColumnFirstName` Polecenie generuje następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="395cb-179">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="395cb-180">To ostrzeżenie jest generowany, ponieważ nazwa pola są teraz ograniczone do 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="395cb-180">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="395cb-181">Jeśli nazwy w bazie danych ma więcej niż 50 znaków, od 51 do ostatniego znaku może spowodować utratę.</span><span class="sxs-lookup"><span data-stu-id="395cb-181">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="395cb-182">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-182">Test the app.</span></span>

<span data-ttu-id="395cb-183">Otwórz tabelę uczniów w SSOX:</span><span class="sxs-lookup"><span data-stu-id="395cb-183">Open the Student table in SSOX:</span></span>

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="395cb-185">Przed zastosowaniem migracji, nazwa kolumny zostały typu [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="395cb-185">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="395cb-186">Nazwa kolumny są teraz `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="395cb-186">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="395cb-187">Zmieniono nazwę kolumny z `FirstMidName` do `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="395cb-187">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="395cb-188">W poniższej sekcji Tworzenie aplikacji na niektórych etapach generuje błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="395cb-188">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="395cb-189">Instrukcje Określ, kiedy do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-189">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="395cb-190">Aktualizacja encji uczniów</span><span class="sxs-lookup"><span data-stu-id="395cb-190">Student entity update</span></span>

![Jednostki dla użytkowników domowych](complex-data-model/_static/student-entity.png)

<span data-ttu-id="395cb-192">Aktualizacja *Models/Student.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="395cb-192">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="395cb-193">Wymagany atrybut</span><span class="sxs-lookup"><span data-stu-id="395cb-193">The Required attribute</span></span>

<span data-ttu-id="395cb-194">`Required` Atrybut powoduje, że nazwa właściwości wymaganych pól.</span><span class="sxs-lookup"><span data-stu-id="395cb-194">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="395cb-195">`Required` Atrybut nie jest wymagane dla typów wartości null, takich jak typy wartości (`DateTime`, `int`, `double`itp.).</span><span class="sxs-lookup"><span data-stu-id="395cb-195">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="395cb-196">Typy, które nie może mieć wartości null są automatycznie traktowane jako wymagane pola.</span><span class="sxs-lookup"><span data-stu-id="395cb-196">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="395cb-197">`Required` Atrybutu mogły zostać zastąpione z minimalną długość parametru w `StringLength` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="395cb-197">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="395cb-198">Atrybut wyświetlania</span><span class="sxs-lookup"><span data-stu-id="395cb-198">The Display attribute</span></span>

<span data-ttu-id="395cb-199">`Display` Atrybut określa, że podpis pola tekstowe powinny "Imię", "Nazwisko", "Pełna nazwa" i "Data rejestracji".</span><span class="sxs-lookup"><span data-stu-id="395cb-199">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="395cb-200">Podpisy domyślne spacji, nie dzielenia wyrazów, na przykład "Lastname." było</span><span class="sxs-lookup"><span data-stu-id="395cb-200">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="395cb-201">Właściwość obliczona imię i nazwisko</span><span class="sxs-lookup"><span data-stu-id="395cb-201">The FullName calculated property</span></span>

<span data-ttu-id="395cb-202">`FullName` jest obliczonej właściwości, która zwraca wartość, która jest tworzona przez łączenie dwóch innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="395cb-202">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="395cb-203">`FullName` Nie można ustawiać, ma akcesora get.</span><span class="sxs-lookup"><span data-stu-id="395cb-203">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="395cb-204">Nie `FullName` kolumny jest tworzony w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-204">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="395cb-205">Utwórz jednostkę instruktora</span><span class="sxs-lookup"><span data-stu-id="395cb-205">Create the Instructor Entity</span></span>

![Jednostka Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="395cb-207">Utwórz *Models/Instructor.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="395cb-207">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="395cb-208">Zwróć uwagę, że kilka właściwości są takie same, w `Student` i `Instructor` jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-208">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="395cb-209">W samouczku wdrażanie dziedziczenia w dalszej części tej serii ten kod został zrefaktoryzowany wyeliminować nadmiarowości.</span><span class="sxs-lookup"><span data-stu-id="395cb-209">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="395cb-210">Wiele atrybutów może być w jednym wierszu.</span><span class="sxs-lookup"><span data-stu-id="395cb-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="395cb-211">`HireDate` Atrybuty można zapisać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="395cb-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="395cb-212">Właściwości nawigacji CourseAssignments i OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="395cb-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="395cb-213">`CourseAssignments` i `OfficeAssignment` właściwości są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="395cb-214">Instruktora nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="395cb-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="395cb-215">Jeśli właściwość nawigacji posiada wiele jednostek:</span><span class="sxs-lookup"><span data-stu-id="395cb-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="395cb-216">Musi być typu listy, w którym wpisy mogą być dodawane, usunąć lub zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="395cb-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="395cb-217">Typy właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="395cb-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="395cb-218">Jeśli `ICollection<T>` określono tworzy EF Core `HashSet<T>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="395cb-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="395cb-219">`CourseAssignment` Jednostki znajduje się w sekcji w relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="395cb-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="395cb-220">Firm Contoso University zasady stanu, że instruktora może mieć co najwyżej jednego pakietu office.</span><span class="sxs-lookup"><span data-stu-id="395cb-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="395cb-221">`OfficeAssignment` Właściwość przechowuje pojedynczy `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="395cb-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="395cb-222">`OfficeAssignment` ma wartość null, jeśli nie przypisano żadnych pakietu office.</span><span class="sxs-lookup"><span data-stu-id="395cb-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="395cb-223">Utwórz jednostkę OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="395cb-223">Create the OfficeAssignment entity</span></span>

![OfficeAssignment jednostki](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="395cb-225">Utwórz *Models/OfficeAssignment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="395cb-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="395cb-226">Atrybut klucza</span><span class="sxs-lookup"><span data-stu-id="395cb-226">The Key attribute</span></span>

<span data-ttu-id="395cb-227">`[Key]` Atrybut służy do identyfikowania właściwości jako klucz podstawowy (PK) gdy nazwa właściwości coś innego niż classnameID lub identyfikator.</span><span class="sxs-lookup"><span data-stu-id="395cb-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="395cb-228">Brak relacji jeden do zero lub jeden między `Instructor` i `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="395cb-229">Przypisania office występuje tylko w odniesieniu do instruktora, który jest przypisany do.</span><span class="sxs-lookup"><span data-stu-id="395cb-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="395cb-230">`OfficeAssignment` Klucz prywatny jest również jego klucz obcy (klucz OBCY) `Instructor` jednostki.</span><span class="sxs-lookup"><span data-stu-id="395cb-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="395cb-231">Podstawowy EF nie może automatycznie rozpoznaje `InstructorID` jako klucz podstawowy z `OfficeAssignment` ponieważ:</span><span class="sxs-lookup"><span data-stu-id="395cb-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="395cb-232">`InstructorID` nie będzie zgodna z konwencją nazewnictwa Identyfikatora lub classnameID.</span><span class="sxs-lookup"><span data-stu-id="395cb-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="395cb-233">W związku z tym `Key` atrybut służy do identyfikowania `InstructorID` jako klucz podstawowy:</span><span class="sxs-lookup"><span data-stu-id="395cb-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="395cb-234">Domyślnie EF Core traktuje klucz jako z systemem innym niż baza danych wygenerowała, ponieważ kolumna jest identyfikujące relacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="395cb-235">Właściwość nawigacji instruktora</span><span class="sxs-lookup"><span data-stu-id="395cb-235">The Instructor navigation property</span></span>

<span data-ttu-id="395cb-236">`OfficeAssignment` Właściwości nawigacji dla `Instructor` jednostka ma wartość null ponieważ:</span><span class="sxs-lookup"><span data-stu-id="395cb-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="395cb-237">Odwołanie typy (takich jak klasy dopuszczają wartości null).</span><span class="sxs-lookup"><span data-stu-id="395cb-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="395cb-238">Instruktor może nie mieć przypisania pakietu office.</span><span class="sxs-lookup"><span data-stu-id="395cb-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="395cb-239">`OfficeAssignment` Jednostka ma niedopuszczającą `Instructor` właściwość nawigacji ponieważ:</span><span class="sxs-lookup"><span data-stu-id="395cb-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="395cb-240">`InstructorID` jest wartości null.</span><span class="sxs-lookup"><span data-stu-id="395cb-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="395cb-241">Przypisania pakietu office nie może istnieć bez instruktora.</span><span class="sxs-lookup"><span data-stu-id="395cb-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="395cb-242">Gdy `Instructor` jednostka ma powiązanego `OfficeAssignment` jednostki, każdy obiekt ma odwołanie do jeden z nich w jej właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="395cb-243">`[Required]` Można zastosować atrybutu do `Instructor` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="395cb-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="395cb-244">Poprzedni kod określa, że muszą być powiązane instruktora.</span><span class="sxs-lookup"><span data-stu-id="395cb-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="395cb-245">Poprzedni kod nie jest konieczne ponieważ `InstructorID` klucz obcy (która jest również PK) jest wartości null.</span><span class="sxs-lookup"><span data-stu-id="395cb-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="395cb-246">Modyfikowanie jednostek kursu</span><span class="sxs-lookup"><span data-stu-id="395cb-246">Modify the Course Entity</span></span>

![Jednostki ciągu](complex-data-model/_static/course-entity.png)

<span data-ttu-id="395cb-248">Aktualizacja *Models/Course.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="395cb-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="395cb-249">`Course` Jednostka ma właściwość klucza obcego (klucz OBCY) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="395cb-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="395cb-250">`DepartmentID` Wskazuje pokrewny `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="395cb-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="395cb-251">`Course` Jednostka ma `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="395cb-252">Podstawowe EF nie wymaga właściwości klucza Obcego dla modelu danych, jeśli model ma właściwości nawigacji dla obiekt pokrewny.</span><span class="sxs-lookup"><span data-stu-id="395cb-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="395cb-253">Wszędzie tam, gdzie są potrzebne, EF Core automatycznie tworzy FKs w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="395cb-254">Tworzy EF Core [w tle właściwości](https://docs.microsoft.com/ef/core/modeling/shadow-properties) dla FKs automatycznie utworzone.</span><span class="sxs-lookup"><span data-stu-id="395cb-254">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="395cb-255">Po klucz OBCY w modelu danych można uaktualniać prostszy i efektywniejszy.</span><span class="sxs-lookup"><span data-stu-id="395cb-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="395cb-256">Rozważmy na przykład model którym właściwości klucza Obcego `DepartmentID` jest *nie* uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="395cb-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="395cb-257">Gdy jednostki ciągu jest pobierana do edycji:</span><span class="sxs-lookup"><span data-stu-id="395cb-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="395cb-258">`Department` Jednostki ma wartość null, jeśli nie są jawnie został załadowany.</span><span class="sxs-lookup"><span data-stu-id="395cb-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="395cb-259">Do zaktualizowania jednostki kursu `Department` jednostki najpierw musi zostać pobrana.</span><span class="sxs-lookup"><span data-stu-id="395cb-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="395cb-260">Gdy właściwość klucza Obcego `DepartmentID` znajduje się w modelu danych nie jest konieczne do pobierania `Department` jednostki przed aktualizacją.</span><span class="sxs-lookup"><span data-stu-id="395cb-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="395cb-261">Atrybut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="395cb-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="395cb-262">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Atrybut określa, że klucz prywatny jest udostępniany przez aplikację, a nie wygenerowanych przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="395cb-263">Domyślnie EF Core zakłada, że wartości klucza podstawowego są generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="395cb-264">DB wygenerowany klucz podstawowy wartości jest zwykle najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="395cb-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="395cb-265">Aby uzyskać `Course` PK. określa jednostki, na użytkownika</span><span class="sxs-lookup"><span data-stu-id="395cb-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="395cb-266">Na przykład liczba kursu takich jak seria 1000 dla działu matematyczne, serii 2000 działu angielskiej wersji językowej.</span><span class="sxs-lookup"><span data-stu-id="395cb-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="395cb-267">`DatabaseGenerated` Atrybutu mogą służyć do generowania wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="395cb-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="395cb-268">Na przykład bazy danych może automatycznie generować pole daty, aby zarejestrować Data wiersz został utworzony lub zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="395cb-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="395cb-269">Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="395cb-269">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="395cb-270">Właściwości obcego klucza i nawigacji</span><span class="sxs-lookup"><span data-stu-id="395cb-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="395cb-271">Właściwości klucza obcego (klucz OBCY) i właściwości nawigacji w `Course` jednostkę odzwierciedlić się następująco:</span><span class="sxs-lookup"><span data-stu-id="395cb-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="395cb-272">Kursu jest przypisany do jednego działu, więc ma `DepartmentID` klucza Obcego i `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="395cb-273">Kursu może mieć dowolną liczbę studentów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="395cb-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="395cb-274">Kursu może organizowane jednocześnie przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="395cb-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="395cb-275">`CourseAssignment` objaśniono [później](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="395cb-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="395cb-276">Utwórz jednostkę działu</span><span class="sxs-lookup"><span data-stu-id="395cb-276">Create the Department entity</span></span>

![Dział jednostki](complex-data-model/_static/department-entity.png)

<span data-ttu-id="395cb-278">Utwórz *Models/Department.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="395cb-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="395cb-279">Atrybut kolumny</span><span class="sxs-lookup"><span data-stu-id="395cb-279">The Column attribute</span></span>

<span data-ttu-id="395cb-280">Wcześniej `Column` użyto atrybutu można zmienić mapowania nazw kolumn.</span><span class="sxs-lookup"><span data-stu-id="395cb-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="395cb-281">W kodzie `Department` jednostki, `Column` atrybut służy do zmiany mapowanie typu danych SQL.</span><span class="sxs-lookup"><span data-stu-id="395cb-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="395cb-282">`Budget` Zdefiniowana kolumna w bazie danych przy użyciu typu money SQL Server:</span><span class="sxs-lookup"><span data-stu-id="395cb-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="395cb-283">Mapowanie kolumny zwykle nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="395cb-283">Column mapping is generally not required.</span></span> <span data-ttu-id="395cb-284">Podstawowe EF zazwyczaj wybiera odpowiedni typ danych programu SQL Server, na podstawie typu CLR dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="395cb-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="395cb-285">Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu.</span><span class="sxs-lookup"><span data-stu-id="395cb-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="395cb-286">`Budget` jest waluty, a typ danych money jest bardziej odpowiednie dla waluty.</span><span class="sxs-lookup"><span data-stu-id="395cb-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="395cb-287">Właściwości obcego klucza i nawigacji</span><span class="sxs-lookup"><span data-stu-id="395cb-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="395cb-288">Właściwości klucza Obcego i nawigacja odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="395cb-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="395cb-289">Dział może lub nie ma uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="395cb-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="395cb-290">Administrator jest zawsze instruktora.</span><span class="sxs-lookup"><span data-stu-id="395cb-290">An administrator is always an instructor.</span></span> <span data-ttu-id="395cb-291">W związku z tym `InstructorID` właściwość jest uwzględniona jako klucz OBCY do `Instructor` jednostki.</span><span class="sxs-lookup"><span data-stu-id="395cb-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="395cb-292">Właściwość nawigacji jest o nazwie `Administrator` , ale zawiera `Instructor` jednostki:</span><span class="sxs-lookup"><span data-stu-id="395cb-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="395cb-293">Znak zapytania (?) w powyższym kodzie Określa, czy właściwość dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="395cb-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="395cb-294">Dział może mieć wiele kursów, więc ma właściwości nawigacji kursy:</span><span class="sxs-lookup"><span data-stu-id="395cb-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="395cb-295">Uwaga: W Konwencji, EF Core umożliwia usuwanie kaskadowe FKs wartości null i relacje wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="395cb-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="395cb-296">Usuwanie kaskadowe może spowodować cykliczne cascade delete reguły.</span><span class="sxs-lookup"><span data-stu-id="395cb-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="395cb-297">Cykliczne Kaskadowo usuń reguły powoduje, że wystąpił wyjątek podczas migracji jest dodawany.</span><span class="sxs-lookup"><span data-stu-id="395cb-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="395cb-298">Na przykład jeśli `Department.InstructorID` właściwości nie został zdefiniowany jako dopuszczający wartość null:</span><span class="sxs-lookup"><span data-stu-id="395cb-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="395cb-299">Podstawowe EF konfiguruje reguły usuwania kaskadowego można usunąć instruktora po usunięciu działu.</span><span class="sxs-lookup"><span data-stu-id="395cb-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="395cb-300">Usuwanie instruktora po usunięciu działu nie jest to oczekiwane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="395cb-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="395cb-301">W razie potrzeby reguły biznesowe `InstructorID` właściwości dopuszczać wartości null, użyj następujących instrukcji interfejsu API fluent:</span><span class="sxs-lookup"><span data-stu-id="395cb-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="395cb-302">Poprzedni kod wyłącza usuwanie kaskadowe relacji instruktora działu.</span><span class="sxs-lookup"><span data-stu-id="395cb-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="395cb-303">Aktualizacja encji rejestracji</span><span class="sxs-lookup"><span data-stu-id="395cb-303">Update the Enrollment entity</span></span>

<span data-ttu-id="395cb-304">Rekord rejestracji jest jeden kursu wykonywaną przez jeden uczniów.</span><span class="sxs-lookup"><span data-stu-id="395cb-304">An enrollment record is for a one course taken by one student.</span></span>

![Jednostki rejestracji](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="395cb-306">Aktualizacja *Models/Enrollment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="395cb-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="395cb-307">Właściwości obcego klucza i nawigacji</span><span class="sxs-lookup"><span data-stu-id="395cb-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="395cb-308">Właściwości klucza Obcego i właściwości nawigacji odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="395cb-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="395cb-309">Rekord rejestracji dotyczy jednego ciągu, więc ma `CourseID` właściwości klucza Obcego i `Course` właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="395cb-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="395cb-310">Rekord rejestracji jest dla jednego studentów, więc ma `StudentID` właściwości klucza Obcego i `Student` właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="395cb-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="395cb-311">Relacje wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="395cb-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="395cb-312">Relację wiele do wielu między `Student` i `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="395cb-313">`Enrollment` Jednostki działa jako sprzężenia wiele do wielu tabela *z ładunku* w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="395cb-314">"Za pomocą ładunku" oznacza, że `Enrollment` tabela zawiera dodatkowe dane poza FKs dla połączonych tabel (w tym przypadku na PK i `Grade`).</span><span class="sxs-lookup"><span data-stu-id="395cb-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="395cb-315">Na poniższej ilustracji przedstawiono, jak wyglądają te relacje w diagramie jednostki.</span><span class="sxs-lookup"><span data-stu-id="395cb-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="395cb-316">(Ten diagram został wygenerowany za pomocą narzędzia Power EF dla EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="395cb-316">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="395cb-317">Tworzenie diagramu nie jest częścią tego samouczka).</span><span class="sxs-lookup"><span data-stu-id="395cb-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Uczniów kursu wiele do wielu](complex-data-model/_static/student-course.png)

<span data-ttu-id="395cb-319">Każdy wiersz relacji ma 1 w jeden element end i znak gwiazdki (\*) w innych, wskazując relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="395cb-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="395cb-320">Jeśli `Enrollment` tabeli nie włączono informacji o kategorii, go tylko musi zawierać dwa FKs (`CourseID` i `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="395cb-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="395cb-321">Wiele do wielu sprzężenia tabeli bez ładunku jest niekiedy nazywany tabeli czysty sprzężenia (PJT).</span><span class="sxs-lookup"><span data-stu-id="395cb-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="395cb-322">`Instructor` i `Course` jednostek ma relację wiele do wielu, przy użyciu tabeli czysty sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="395cb-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="395cb-323">Uwaga: Niejawna sprzężenia tabel dla relacji wiele do wielu, ale podstawowe EF nie obsługuje 6.x EF.</span><span class="sxs-lookup"><span data-stu-id="395cb-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="395cb-324">Aby uzyskać więcej informacji, zobacz [wiele do wielu relacji w programie EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="395cb-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="395cb-325">Jednostka CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="395cb-325">The CourseAssignment entity</span></span>

![CourseAssignment jednostki](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="395cb-327">Utwórz *Models/CourseAssignment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="395cb-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="395cb-328">Instruktora do szkolenia</span><span class="sxs-lookup"><span data-stu-id="395cb-328">Instructor-to-Courses</span></span>

![M:M instruktora do szkolenia](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="395cb-330">Relacja wiele do wielu instruktora do kursów:</span><span class="sxs-lookup"><span data-stu-id="395cb-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="395cb-331">Wymaga tabeli sprzężenia, który musi być reprezentowany przez zestaw jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="395cb-332">Znajduje się tabela czysty sprzężenia (tabeli bez ładunku).</span><span class="sxs-lookup"><span data-stu-id="395cb-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="395cb-333">Często jest nazwa jednostki sprzężenia `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="395cb-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="395cb-334">Na przykład tabela sprzężenia instruktora-kursy użycia tego wzorca jest `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="395cb-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="395cb-335">Jednak zaleca się przy użyciu nazwy, który opisuje relację.</span><span class="sxs-lookup"><span data-stu-id="395cb-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="395cb-336">Modele danych uruchamiane prosty i powiększania.</span><span class="sxs-lookup"><span data-stu-id="395cb-336">Data models start out simple and grow.</span></span> <span data-ttu-id="395cb-337">Sprzężenia nie ładunku (PJTs) często rozwijać, aby uwzględnić ładunku.</span><span class="sxs-lookup"><span data-stu-id="395cb-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="395cb-338">Począwszy od nazwę opisową jednostki, nazwa nie trzeba zmienić po zmianie tabeli sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="395cb-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="395cb-339">W idealnym przypadku jednostki sprzężenia musi własną nazwę fizycznych (prawdopodobnie pojedynczego wyrazu) w domenie biznesowych.</span><span class="sxs-lookup"><span data-stu-id="395cb-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="395cb-340">Na przykład książki i klientów można połączyć z jednostką sprzężenia o nazwie klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="395cb-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="395cb-341">Dla tej relacji wiele do wielu instruktora do kursów `CourseAssignment` jest preferowana przez `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="395cb-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="395cb-342">Klucz złożony</span><span class="sxs-lookup"><span data-stu-id="395cb-342">Composite key</span></span>

<span data-ttu-id="395cb-343">FKs nie są wartości null.</span><span class="sxs-lookup"><span data-stu-id="395cb-343">FKs are not nullable.</span></span> <span data-ttu-id="395cb-344">Dwa FKs w `CourseAssignment` (`InstructorID` i `CourseID`) ze sobą jednoznacznie zidentyfikować każdy wiersz `CourseAssignment` tabeli.</span><span class="sxs-lookup"><span data-stu-id="395cb-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="395cb-345">`CourseAssignment` nie wymaga dedykowanego PK.</span><span class="sxs-lookup"><span data-stu-id="395cb-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="395cb-346">`InstructorID` i `CourseID` właściwości działać jako PK. złożone</span><span class="sxs-lookup"><span data-stu-id="395cb-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="395cb-347">Jedynym sposobem Określ złożonego PKs na rdzeń EF jest z *interfejsu API fluent*.</span><span class="sxs-lookup"><span data-stu-id="395cb-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="395cb-348">W następnej części pokazano sposób konfigurowania PK. złożone</span><span class="sxs-lookup"><span data-stu-id="395cb-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="395cb-349">Klucz złożony zapewnia:</span><span class="sxs-lookup"><span data-stu-id="395cb-349">The composite key ensures:</span></span>

* <span data-ttu-id="395cb-350">Wiele wierszy są dozwolone dla porach jeden.</span><span class="sxs-lookup"><span data-stu-id="395cb-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="395cb-351">Wiele wierszy są dozwolone dla jednego instruktora.</span><span class="sxs-lookup"><span data-stu-id="395cb-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="395cb-352">Wiele wierszy dla tego samego instruktora i kursu nie jest dozwolona.</span><span class="sxs-lookup"><span data-stu-id="395cb-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="395cb-353">`Enrollment` Jednostki sprzężenia definiuje własny klucz podstawowy, więc możliwe są duplikatami tego sortowania.</span><span class="sxs-lookup"><span data-stu-id="395cb-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="395cb-354">Aby uniknąć tych duplikaty:</span><span class="sxs-lookup"><span data-stu-id="395cb-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="395cb-355">Dodaj unikatowy indeks dla pól klucza Obcego lub</span><span class="sxs-lookup"><span data-stu-id="395cb-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="395cb-356">Skonfiguruj `Enrollment` z głównej klucz złożony, podobnie jak `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="395cb-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="395cb-357">Aby uzyskać więcej informacji, zobacz [indeksów](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="395cb-357">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="395cb-358">Zaktualizuj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="395cb-358">Update the DB context</span></span>

<span data-ttu-id="395cb-359">Dodaj następujący wyróżniony kod, aby *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="395cb-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="395cb-360">Poprzedni kod dodaje nowe jednostki i konfiguruje `CourseAssignment` PK. złożonego jednostki</span><span class="sxs-lookup"><span data-stu-id="395cb-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="395cb-361">Zamiast interfejsu API Fluent atrybutów</span><span class="sxs-lookup"><span data-stu-id="395cb-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="395cb-362">`OnModelCreating` Metody w poprzednim kod używa *interfejsu API fluent* na konfigurowanie zachowania EF Core.</span><span class="sxs-lookup"><span data-stu-id="395cb-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="395cb-363">Interfejs API jest nazywany "fluent", ponieważ jest często używany przez instalowanie szereg wywołania metody w jednej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="395cb-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="395cb-364">[Następującego kodu](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) przykładem interfejsu API fluent:</span><span class="sxs-lookup"><span data-stu-id="395cb-364">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="395cb-365">W tym samouczku interfejsu API fluent jest używany tylko w przypadku mapowania bazy danych, które nie może zostać wykonane z atrybutami.</span><span class="sxs-lookup"><span data-stu-id="395cb-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="395cb-366">Jednak interfejsu API fluent można określić większość formatowania, sprawdzanie poprawności i reguły mapowania, które można wykonać za pomocą atrybutów.</span><span class="sxs-lookup"><span data-stu-id="395cb-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="395cb-367">Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować z interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="395cb-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="395cb-368">`MinimumLength` nie powoduje zmiany schematu, ma zastosowanie tylko reguły weryfikacji minimalnej długości.</span><span class="sxs-lookup"><span data-stu-id="395cb-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="395cb-369">Niektórzy deweloperzy wolą Użyj interfejsu API fluent wyłącznie tak, aby ich zachowanie ich klasami jednostki "Wyczyść".</span><span class="sxs-lookup"><span data-stu-id="395cb-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="395cb-370">Atrybuty i wygodnego interfejsu API można łączyć.</span><span class="sxs-lookup"><span data-stu-id="395cb-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="395cb-371">Istnieją pewne konfiguracje, które jest możliwe tylko z interfejsu API fluent (Określanie złożonego klucza podstawowego).</span><span class="sxs-lookup"><span data-stu-id="395cb-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="395cb-372">Istnieją pewne konfiguracje, które można wykonać tylko z atrybutami (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="395cb-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="395cb-373">Zalecana praktyka dla przy użyciu fluent API lub atrybuty:</span><span class="sxs-lookup"><span data-stu-id="395cb-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="395cb-374">Wybierz jedną z tych dwóch metod.</span><span class="sxs-lookup"><span data-stu-id="395cb-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="395cb-375">Użyć wybranej metody spójnie jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="395cb-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="395cb-376">Niektóre atrybuty używane w tym samouczku są używane do:</span><span class="sxs-lookup"><span data-stu-id="395cb-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="395cb-377">Sprawdzanie poprawności tylko (na przykład `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="395cb-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="395cb-378">Tylko konfiguracja Core EF (na przykład `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="395cb-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="395cb-379">Sprawdzanie poprawności i podstawowe EF konfiguracji (na przykład `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="395cb-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="395cb-380">Aby uzyskać więcej informacji dotyczących atrybutów w porównaniu z interfejsu API fluent, zobacz [metody konfiguracji](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="395cb-380">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="395cb-381">Jednostki Diagram przedstawiający relacje</span><span class="sxs-lookup"><span data-stu-id="395cb-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="395cb-382">Na poniższej ilustracji przedstawiono diagram, który EF zasilania narzędzia tworzenia dla modelu ukończone szkoły.</span><span class="sxs-lookup"><span data-stu-id="395cb-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="395cb-384">Na powyższym diagramie przedstawiono:</span><span class="sxs-lookup"><span data-stu-id="395cb-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="395cb-385">Kilka relacji jeden do wielu wierszy (od 1 do \*).</span><span class="sxs-lookup"><span data-stu-id="395cb-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="395cb-386">Linię relacji jeden do zero lub jeden (1 do od 0 do 1) między `Instructor` i `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="395cb-387">Linię relacji zero lub jeden do wielu (od 0 do 1 do \*) między `Instructor` i `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="395cb-388">Inicjatora bazy danych z danych testowych</span><span class="sxs-lookup"><span data-stu-id="395cb-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="395cb-389">Zaktualizuj kod w *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="395cb-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="395cb-390">Poprzedni kod zawiera dane dla nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="395cb-391">Większość ten kod tworzy nowe obiekty jednostki i ładuje przykładowych danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="395cb-392">Dane przykładowe są używane do testowania.</span><span class="sxs-lookup"><span data-stu-id="395cb-392">The sample data is used for testing.</span></span> <span data-ttu-id="395cb-393">Poprzedni kod tworzy następujące relacje wiele do wielu:</span><span class="sxs-lookup"><span data-stu-id="395cb-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="395cb-394">Uwaga: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) będzie obsługiwać [wstępne wypełnianie danych](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="395cb-394">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="395cb-395">Dodaj migracji</span><span class="sxs-lookup"><span data-stu-id="395cb-395">Add a migration</span></span>

<span data-ttu-id="395cb-396">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="395cb-396">Build the project.</span></span> <span data-ttu-id="395cb-397">Otwórz okno polecenia w folderze projektu i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="395cb-397">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="395cb-398">Poprzednie polecenie wyświetla ostrzeżenie o możliwości utraty danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="395cb-399">Jeśli `database update` polecenie jest uruchamiane, jest generowany następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="395cb-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="395cb-400">Podczas migracji są uruchamiane z istniejącymi danymi, może to być ograniczenia klucza Obcego, które nie są spełnione przy użyciu istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-400">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="395cb-401">W tym samouczku tworzona jest nowej bazy danych, więc nie żadne naruszenia ograniczenia klucza Obcego.</span><span class="sxs-lookup"><span data-stu-id="395cb-401">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="395cb-402">Zobacz [rozwiązywania ograniczeń klucza obcego z danymi starszych](#fk) instrukcje dotyczące sposobu rozwiązania naruszenia klucza Obcego dla bieżącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-402">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="395cb-403">Zmień parametry połączenia i zaktualizować bazę danych</span><span class="sxs-lookup"><span data-stu-id="395cb-403">Change the connection string and update the DB</span></span>

<span data-ttu-id="395cb-404">Kod w zaktualizowanego `DbInitializer` dodaje dane nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="395cb-404">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="395cb-405">Aby wymusić EF Core, aby utworzyć nowy pusty bazy danych:</span><span class="sxs-lookup"><span data-stu-id="395cb-405">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="395cb-406">Zmień nazwę ciągu połączenia bazy danych w *appsettings.json* do ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="395cb-406">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="395cb-407">Nowa nazwa musi być nazwą, który nie został użyty na komputerze.</span><span class="sxs-lookup"><span data-stu-id="395cb-407">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="395cb-408">Można również usunąć przy użyciu bazy danych:</span><span class="sxs-lookup"><span data-stu-id="395cb-408">Alternatively, delete the DB using:</span></span>

  * <span data-ttu-id="395cb-409">**Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="395cb-409">**SQL Server Object Explorer** (SSOX).</span></span>
  * <span data-ttu-id="395cb-410">`database drop` Polecenia interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="395cb-410">The `database drop` CLI command:</span></span>

    ```console
    dotnet ef database drop
    ```

<span data-ttu-id="395cb-411">Uruchom `database update` w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="395cb-411">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="395cb-412">Poprzednie polecenie uruchamia wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="395cb-412">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="395cb-413">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="395cb-413">Run the app.</span></span> <span data-ttu-id="395cb-414">Uruchomiona aplikacja działa `DbInitializer.Initialize` metody.</span><span class="sxs-lookup"><span data-stu-id="395cb-414">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="395cb-415">`DbInitializer.Initialize` Wypełnia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-415">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="395cb-416">Otwórz SSOX bazy danych:</span><span class="sxs-lookup"><span data-stu-id="395cb-416">Open the DB in SSOX:</span></span>

* <span data-ttu-id="395cb-417">Rozwiń węzeł **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="395cb-417">Expand the **Tables** node.</span></span> <span data-ttu-id="395cb-418">Utworzony tabele są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="395cb-418">The created tables are displayed.</span></span>
* <span data-ttu-id="395cb-419">Jeśli SSOX został poprzednio otwarty, kliknij przycisk **Odśwież** przycisku.</span><span class="sxs-lookup"><span data-stu-id="395cb-419">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="395cb-421">Sprawdź **CourseAssignment** tabeli:</span><span class="sxs-lookup"><span data-stu-id="395cb-421">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="395cb-422">Kliknij prawym przyciskiem myszy **CourseAssignment** tabeli i wybierz **danych widoku**.</span><span class="sxs-lookup"><span data-stu-id="395cb-422">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="395cb-423">Sprawdź **CourseAssignment** tabela zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="395cb-423">Verify the **CourseAssignment** table contains data.</span></span>

![Dane CourseAssignment w SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="395cb-425">Ustalenie ograniczeń klucza obcego starszych danych</span><span class="sxs-lookup"><span data-stu-id="395cb-425">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="395cb-426">Ta sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="395cb-426">This section is optional.</span></span>

<span data-ttu-id="395cb-427">Podczas migracji są uruchamiane z istniejącymi danymi, może to być ograniczenia klucza Obcego, które nie są spełnione przy użyciu istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-427">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="395cb-428">Z danymi produkcyjnymi należy przedsięwziąć do migrowania istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-428">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="395cb-429">Ta sekcja zawiera przykład ustalania narusza ograniczenie klucza Obcego.</span><span class="sxs-lookup"><span data-stu-id="395cb-429">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="395cb-430">Nie wprowadzić te zmiany kodu bez kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="395cb-430">Don't make these code changes without a backup.</span></span> <span data-ttu-id="395cb-431">Nie należy wprowadzić te zmiany kodu, gdy wypełnione poprzedniej sekcji i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="395cb-431">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="395cb-432">*{Timestamp}_ComplexDataModel.cs* plik zawiera następujący kod:</span><span class="sxs-lookup"><span data-stu-id="395cb-432">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="395cb-433">Poprzedni kod dodaje niedopuszczającą `DepartmentID` klucza Obcego do `Course` tabeli.</span><span class="sxs-lookup"><span data-stu-id="395cb-433">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="395cb-434">Bazy danych z poprzedniej samouczek zawiera wiersze w `Course`, więc nie można zaktualizować tabeli przez migracje.</span><span class="sxs-lookup"><span data-stu-id="395cb-434">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="395cb-435">Aby `ComplexDataModel` pracy migracji z istniejącymi danymi:</span><span class="sxs-lookup"><span data-stu-id="395cb-435">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="395cb-436">Zmień kod, aby nadać nowej kolumny (`DepartmentID`) wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="395cb-436">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="395cb-437">Utwórz fałszywych dział o nazwie "Temp" jako domyślnego działu.</span><span class="sxs-lookup"><span data-stu-id="395cb-437">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="395cb-438">Usuń ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="395cb-438">Fix the foreign key constraints</span></span>

<span data-ttu-id="395cb-439">Aktualizacja `ComplexDataModel` klasy `Up` metody:</span><span class="sxs-lookup"><span data-stu-id="395cb-439">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="395cb-440">Otwórz *{timestamp}_ComplexDataModel.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="395cb-440">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="395cb-441">Komentarz wiersz kodu, który dodaje `DepartmentID` kolumnę `Course` tabeli.</span><span class="sxs-lookup"><span data-stu-id="395cb-441">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="395cb-442">Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="395cb-442">Add the following highlighted code.</span></span> <span data-ttu-id="395cb-443">Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="395cb-443">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="395cb-444">Z poprzednim zmiany, istniejące `Course` wiersze zostaną powiązane do działu "Temp" po `ComplexDataModel` `Up` uruchamia metody.</span><span class="sxs-lookup"><span data-stu-id="395cb-444">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="395cb-445">Czy aplikacji produkcyjnej:</span><span class="sxs-lookup"><span data-stu-id="395cb-445">A production app would:</span></span>

* <span data-ttu-id="395cb-446">Zawiera kod lub skryptów służących do dodawania `Department` wierszy i powiązanych `Course` wierszy do nowego `Department` wierszy.</span><span class="sxs-lookup"><span data-stu-id="395cb-446">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="395cb-447">Używaj dział "Temp" lub wartość domyślną dla `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="395cb-447">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="395cb-448">Następny samouczek obejmuje dane dotyczące.</span><span class="sxs-lookup"><span data-stu-id="395cb-448">The next tutorial covers related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="395cb-449">[Poprzednie](xref:data/ef-rp/migrations)
> [dalej](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="395cb-449">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
