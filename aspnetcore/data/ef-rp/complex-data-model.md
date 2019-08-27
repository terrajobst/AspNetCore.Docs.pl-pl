---
title: Razor Pages EF Core w ASP.NET Core — model danych-5 z 8
author: tdykstra
description: W tym samouczku Dodaj więcej jednostek i relacje i Dostosuj model danych, określając formatowanie, walidację i reguły mapowania.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 5b810073abc8152773631235b5c21611d1a498b5
ms.sourcegitcommit: 776598f71da0d1e4c9e923b3b395d3c3b5825796
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/26/2019
ms.locfileid: "70024764"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="2d7b6-103">Razor Pages EF Core w ASP.NET Core — model danych-5 z 8</span><span class="sxs-lookup"><span data-stu-id="2d7b6-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="2d7b6-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2d7b6-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2d7b6-105">Poprzednie samouczki pracowały z podstawowym modelem danych, który składa się z trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="2d7b6-106">W tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-106">In this tutorial:</span></span>

* <span data-ttu-id="2d7b6-107">Dodano więcej jednostek i relacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="2d7b6-108">Model danych jest dostosowywany przez określenie formatowania, walidacji i reguł mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="2d7b6-109">Ukończony model danych przedstawiono na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-109">The completed data model is shown in the following illustration:</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="2d7b6-111">Jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="2d7b6-111">The Student entity</span></span>

![Jednostka ucznia](complex-data-model/_static/student-entity.png)

<span data-ttu-id="2d7b6-113">Zastąp kod w *modelach/student. cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="2d7b6-114">Poprzedni kod dodaje `FullName` Właściwość i dodaje następujące atrybuty do istniejących właściwości:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="2d7b6-115">Właściwość obliczeniowa FullName</span><span class="sxs-lookup"><span data-stu-id="2d7b6-115">The FullName calculated property</span></span>

<span data-ttu-id="2d7b6-116">`FullName`jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="2d7b6-117">`FullName`nie można ustawić, dlatego ma tylko metodę dostępu get.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="2d7b6-118">Nie `FullName` utworzono żadnej kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="2d7b6-119">Atrybut DataType</span><span class="sxs-lookup"><span data-stu-id="2d7b6-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="2d7b6-120">W przypadku dat rejestracji uczniów wszystkie strony wyświetlają teraz godzinę i datę, chociaż tylko data jest ważna.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="2d7b6-121">Używając atrybutów adnotacji danych, można wprowadzić jedną zmianę kodu, która naprawi format wyświetlania na każdej stronie, która wyświetla dane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="2d7b6-122">Atrybut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) określa typ danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="2d7b6-123">W tym przypadku należy wyświetlić tylko datę, a nie datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="2d7b6-124">[Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress itp. Ten `DataType` atrybut może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="2d7b6-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-125">For example:</span></span>

* <span data-ttu-id="2d7b6-126">Łącze jest tworzone automatycznie dla `DataType.EmailAddress`. `mailto:`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="2d7b6-127">Selektor daty jest dostępny `DataType.Date` w większości przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="2d7b6-128">Ten `DataType` atrybut emituje atrybuty HTML `data-` 5 (wymawiane kreski danych).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="2d7b6-129">`DataType` Atrybuty nie zapewniają walidacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="2d7b6-130">Atrybut DisplayFormat</span><span class="sxs-lookup"><span data-stu-id="2d7b6-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="2d7b6-131">`DataType.Date`nie określa formatu wyświetlanej daty.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="2d7b6-132">Domyślnie pole Date jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)serwera.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="2d7b6-133">Ten `DisplayFormat` atrybut służy do jawnego określenia formatu daty.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="2d7b6-134">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie ma być również stosowane do interfejsu użytkownika edytowania.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="2d7b6-135">Niektóre pola nie powinny `ApplyFormatInEditMode`być używane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="2d7b6-136">Na przykład symbol waluty zazwyczaj nie powinien być wyświetlany w polu tekstowym Edycja.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="2d7b6-137">Ten `DisplayFormat` atrybut może być używany przez siebie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="2d7b6-138">Zwykle dobrym pomysłem jest użycie `DataType` atrybutu `DisplayFormat` z atrybutem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="2d7b6-139">Ten `DataType` atrybut przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="2d7b6-140">Ten `DataType` atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`programie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="2d7b6-141">Przeglądarka może włączyć funkcje HTML5.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="2d7b6-142">Na przykład Pokaż kontrolkę kalendarz, odpowiedni dla ustawień regionalnych symbol waluty, linki poczty e-mail i sprawdzanie poprawności danych po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="2d7b6-143">Domyślnie przeglądarka renderuje dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="2d7b6-144">Aby uzyskać więcej informacji, zobacz [ \<dokumentację pomocnika tagów > danych wejściowych](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="2d7b6-145">Atrybut StringLength</span><span class="sxs-lookup"><span data-stu-id="2d7b6-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="2d7b6-146">Można określić reguły walidacji danych i komunikaty o błędach walidacji z atrybutami.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="2d7b6-147">Atrybut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="2d7b6-148">Kod pokazuje limity nazw do nie więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="2d7b6-149">Przykład określający, że minimalna długość ciągu jest pokazywana w [dalszej](#the-required-attribute)części.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="2d7b6-150">Ten `StringLength` atrybut zapewnia również weryfikację po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="2d7b6-151">Wartość minimalna nie ma wpływu na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="2d7b6-152">Ten `StringLength` atrybut nie uniemożliwia użytkownikowi wprowadzania białych znaków w nazwie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="2d7b6-153">Atrybut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) może służyć do stosowania ograniczeń do danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="2d7b6-154">Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d7b6-156">W **Eksplorator obiektów SQL Server** (SSOX) Otwórz projektanta tabeli uczniów, klikając dwukrotnie tabelę **uczniów** .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela studentów w SSOX przed migracją](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="2d7b6-158">Na powyższym obrazie przedstawiono schemat `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="2d7b6-159">Pola nazw mają typ `nvarchar(MAX)`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="2d7b6-160">Gdy migracja zostanie utworzona i zastosowana w dalszej części tego samouczka, nazwy `nvarchar(50)` pól staną się wynikiem atrybutów długości ciągu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2d7b6-162">W narzędziu SQLite Sprawdź definicje `Student` kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="2d7b6-163">Pola nazw mają typ `Text`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-163">The name fields have type `Text`.</span></span> <span data-ttu-id="2d7b6-164">Zwróć uwagę, że jest wywoływana `FirstMidName`nazwa pola imię.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="2d7b6-165">W następnej sekcji zmienisz nazwę tej kolumny na `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="2d7b6-166">Atrybut Column</span><span class="sxs-lookup"><span data-stu-id="2d7b6-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="2d7b6-167">Atrybuty mogą kontrolować sposób mapowania klas i właściwości do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="2d7b6-168">`Student` W modelu `Column` atrybut jest używany do `FirstMidName` mapowania nazwy właściwości na "FirstName" w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="2d7b6-169">Po utworzeniu bazy danych nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, `Column` gdy atrybut jest używany).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="2d7b6-170">`Student` Model używa`FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="2d7b6-171">Atrybut w modelu danych jest mapowany`Student` do kolumnytabeli.`FirstName` `Student.FirstMidName` `[Column]`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="2d7b6-172">Dodanie `Column` atrybutu zmienia model z `SchoolContext`kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="2d7b6-173">Model, który wykonuje `SchoolContext` kopię zapasową, nie jest już zgodny z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="2d7b6-174">Niezgodność zostanie rozwiązany przez dodanie migracji w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="2d7b6-175">Wymagany atrybut</span><span class="sxs-lookup"><span data-stu-id="2d7b6-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="2d7b6-176">`Required` Atrybut powoduje, że właściwości nazwy są wymagane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="2d7b6-177">Ten `Required` atrybut nie jest wymagany dla typów niedopuszczających wartości null, takich jak typy wartości `DateTime`( `int`na przykład `double`,, i).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="2d7b6-178">Typy, które nie mogą mieć wartości null, są automatycznie traktowane jako pola wymagane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="2d7b6-179">Atrybut musi być używany z `MinimumLength` , `MinimumLength` aby można było wymusić. `Required`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-179">The `Required` attribute must be used with `MinimumLength` for the `MinimumLength` to be enforced.</span></span>

```csharp
[Display(Name = "Last Name")]
[Required]
[StringLength(50, MinimumLength=2)]
public string LastName { get; set; }
```

<span data-ttu-id="2d7b6-180">`MinimumLength`i `Required` Zezwalaj na odstępy, aby spełnić kryteria weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-180">`MinimumLength` and `Required` allow whitespace to satisfy the validation.</span></span> <span data-ttu-id="2d7b6-181">`RegularExpression` Użyj atrybutu w celu zapewnienia pełnej kontroli nad ciągiem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-181">Use the `RegularExpression` attribute for full control over the string.</span></span>

### <a name="the-display-attribute"></a><span data-ttu-id="2d7b6-182">Atrybut wyświetlania</span><span class="sxs-lookup"><span data-stu-id="2d7b6-182">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="2d7b6-183">Ten `Display` atrybut określa, że podpis pól tekstowych powinien mieć wartość "imię i nazwisko", "nazwisko", "imię i nazwisko" oraz "Data rejestracji".</span><span class="sxs-lookup"><span data-stu-id="2d7b6-183">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="2d7b6-184">Domyślne podpisy nie zawierają spacji dzielących wyrazy, na przykład "LastName".</span><span class="sxs-lookup"><span data-stu-id="2d7b6-184">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="2d7b6-185">Tworzenie migracji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-185">Create a migration</span></span>

<span data-ttu-id="2d7b6-186">Uruchom aplikację i przejdź do strony uczniów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-186">Run the app and go to the Students page.</span></span> <span data-ttu-id="2d7b6-187">Zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-187">An exception is thrown.</span></span> <span data-ttu-id="2d7b6-188">Ten `[Column]` atrybut powoduje, że Dr powinien znaleźć kolumnę o nazwie `FirstName`, ale nazwa kolumny w bazie danych jest nadal `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-188">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d7b6-190">Komunikat o błędzie jest podobny do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-190">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="2d7b6-191">W obszarze PMC wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-191">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="2d7b6-192">Pierwsze z tych poleceń generuje następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-192">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="2d7b6-193">Ostrzeżenie jest generowane, ponieważ pola nazw są teraz ograniczone do 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-193">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="2d7b6-194">Jeśli nazwa w bazie danych ma więcej niż 50 znaków, zostanie utracony od 51 do ostatniego znaku.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-194">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="2d7b6-195">Otwórz tabelę uczniów w SSOX:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-195">Open the Student table in SSOX:</span></span>

  ![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="2d7b6-197">Przed zainstalowaniem migracji kolumny nazw były typu [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-197">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="2d7b6-198">Kolumny nazw są teraz `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-198">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="2d7b6-199">Nazwa kolumny została zmieniona z `FirstMidName` na. `FirstName`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-199">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2d7b6-201">Komunikat o błędzie jest podobny do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-201">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="2d7b6-202">Otwórz okno polecenia w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-202">Open a command window in the project folder.</span></span> <span data-ttu-id="2d7b6-203">Wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-203">Enter the following commands to create a new migration and update the database:</span></span>

  ```console
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="2d7b6-204">Polecenie aktualizacji bazy danych wyświetla błąd podobny do następującego przykładu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-204">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="2d7b6-205">W tym samouczku sposób, w jaki można to zrobić, jest usunięcie i ponowne utworzenie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-205">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="2d7b6-206">Aby uzyskać więcej informacji, zobacz uwagi dotyczące ostrzeżeń oprogramowania SQLite w górnej części [samouczka migracji](xref:data/ef-rp/migrations).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-206">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="2d7b6-207">Usuń folder *migracji* .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-207">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="2d7b6-208">Uruchom następujące polecenia, aby usunąć bazę danych, utworzyć nową migrację początkową i zastosować migrację:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-208">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```console
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="2d7b6-209">Sprawdź tabelę uczniów w narzędziu SQLite.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-209">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="2d7b6-210">Kolumna, która była FirstMidName, jest teraz FirstName.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-210">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="2d7b6-211">Uruchom aplikację i przejdź do strony uczniów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-211">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="2d7b6-212">Należy zauważyć, że czasy nie są wprowadzane ani wyświetlane wraz z datami.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-212">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="2d7b6-213">Wybierz pozycję **Utwórz nową**, a następnie spróbuj wprowadzić nazwę dłuższą niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-213">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="2d7b6-214">W poniższych sekcjach Kompilowanie aplikacji na niektórych etapach powoduje wygenerowanie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-214">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="2d7b6-215">Instrukcje określają, kiedy należy skompilować aplikację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-215">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="2d7b6-216">Jednostka instruktora</span><span class="sxs-lookup"><span data-stu-id="2d7b6-216">The Instructor Entity</span></span>

![Jednostka instruktora](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="2d7b6-218">Utwórz *modele/instruktor. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-218">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="2d7b6-219">Wiele atrybutów może znajdować się w jednym wierszu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-219">Multiple attributes can be on one line.</span></span> <span data-ttu-id="2d7b6-220">`HireDate` Atrybuty mogą być zapisywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-220">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="2d7b6-221">Właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-221">Navigation properties</span></span>

<span data-ttu-id="2d7b6-222">Właściwości `CourseAssignments` i`OfficeAssignment` są właściwościami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-222">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="2d7b6-223">Instruktor może nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-223">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="2d7b6-224">Instruktor może mieć co najwyżej jednego biura, więc `OfficeAssignment` Właściwość zawiera jedną `OfficeAssignment` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-224">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="2d7b6-225">`OfficeAssignment`ma wartość null, jeśli nie przypisano pakietu Office.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-225">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="2d7b6-226">Jednostka OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="2d7b6-226">The OfficeAssignment entity</span></span>

![Jednostka OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="2d7b6-228">Utwórz *modele/OfficeAssignment. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-228">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="2d7b6-229">Atrybut klucza</span><span class="sxs-lookup"><span data-stu-id="2d7b6-229">The Key attribute</span></span>

<span data-ttu-id="2d7b6-230">Ten `[Key]` atrybut służy do identyfikowania właściwości jako klucza podstawowego (PK), gdy nazwa właściwości ma wartość inną niż classnameID lub ID.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-230">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="2d7b6-231">Istnieje relacja jeden do zera między `Instructor` jednostkami i. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-231">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="2d7b6-232">Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-232">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="2d7b6-233">Klucz podstawowy jest również jego kluczem obcym (obcy) `Instructor` do jednostki. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-233">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="2d7b6-234">EF Core nie może automatycznie `InstructorID` rozpoznać jako `OfficeAssignment` klucza podstawowego, `InstructorID` ponieważ nie jest zgodna z konwencją nazewnictwa ID lub classnameID.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-234">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="2d7b6-235">W związku z tym `InstructorID` atrybutjestużywanydoidentyfikacjijakokluczpodstawowy:`Key`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-235">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="2d7b6-236">Domyślnie EF Core traktuje klucz jako wygenerowane poza bazą danych, ponieważ kolumna służy do identyfikowania relacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-236">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="2d7b6-237">Właściwość nawigacji instruktora</span><span class="sxs-lookup"><span data-stu-id="2d7b6-237">The Instructor navigation property</span></span>

<span data-ttu-id="2d7b6-238">Właściwość `Instructor.OfficeAssignment` nawigacji może mieć wartość null, ponieważ nie może `OfficeAssignment` istnieć wiersz danego instruktora.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-238">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="2d7b6-239">Instruktor może nie mieć przypisania pakietu Office.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-239">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="2d7b6-240">Właściwość nawigacji zawsze będzie mieć jednostkę instruktora, ponieważ typ klucza `InstructorID` obcego to `int`, typ wartości niedopuszczający wartości null. `OfficeAssignment.Instructor`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-240">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="2d7b6-241">Przypisanie pakietu Office nie może istnieć bez instruktora.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="2d7b6-242">Gdy jednostka ma powiązaną `OfficeAssignment` jednostkę, każda jednostka ma odwołanie do drugiej z nich we właściwości nawigacji. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="2d7b6-243">Jednostka kursu</span><span class="sxs-lookup"><span data-stu-id="2d7b6-243">The Course Entity</span></span>

![Jednostka kursu](complex-data-model/_static/course-entity.png)

<span data-ttu-id="2d7b6-245">Aktualizuj *modele/kurs. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-245">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="2d7b6-246">Jednostka ma właściwość `DepartmentID`klucza obcego (obcy). `Course`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-246">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="2d7b6-247">`DepartmentID`wskazuje powiązaną `Department` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-247">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="2d7b6-248">`Course` Jednostka ma właściwość nawigacji. `Department`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-248">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="2d7b6-249">EF Core nie wymaga właściwości klucza obcego dla modelu danych, gdy model ma właściwość nawigacji dla powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-249">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="2d7b6-250">EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie są one zbędne.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-250">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="2d7b6-251">EF Core tworzy [Właściwości cienia](/ef/core/modeling/shadow-properties) dla automatycznie utworzonych FKs.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-251">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="2d7b6-252">Jednak jawne dołączenie klucza obcego w modelu danych może spowodować uproszczenie i wydajniejsze aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-252">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="2d7b6-253">Rozważmy na przykład model, w którym Właściwość `DepartmentID` FK *nie* jest uwzględniona.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-253">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="2d7b6-254">Gdy jednostka kursu jest pobierana do edycji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-254">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="2d7b6-255">Właściwość `Department` ma wartość null, jeśli nie jest jawnie załadowana.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-255">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="2d7b6-256">Aby zaktualizować jednostkę kursu, `Department` należy najpierw pobrać jednostkę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-256">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="2d7b6-257">Gdy właściwość `DepartmentID` FK jest uwzględniona w modelu danych, nie trzeba `Department` pobierać jednostki przed aktualizacją.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-257">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="2d7b6-258">Atrybut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="2d7b6-258">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="2d7b6-259">Ten `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atrybut określa, że klucz podstawowy jest dostarczany przez aplikację, a nie generowany przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-259">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="2d7b6-260">Domyślnie EF Core zakłada, że wartości klucza PK są generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-260">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="2d7b6-261">Wygenerowana baza danych jest ogólnie najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-261">Database-generated is generally the best approach.</span></span> <span data-ttu-id="2d7b6-262">W `Course` przypadku jednostek użytkownik określa klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-262">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="2d7b6-263">Na przykład numer kursu, taki jak seria 1000 dla działu Math, serii 2000 dla działu angielskiego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-263">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="2d7b6-264">Ten `DatabaseGenerated` atrybut może być również używany do generowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-264">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="2d7b6-265">Na przykład baza danych może automatycznie wygenerować pole daty, aby zarejestrować datę utworzenia lub zaktualizowania wiersza.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-265">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="2d7b6-266">Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-266">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d7b6-267">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-267">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d7b6-268">Właściwości klucza obcego (FK) i właściwości nawigacji w `Course` jednostce odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-268">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="2d7b6-269">Kurs jest przypisywany do jednego działu, więc istnieje `DepartmentID` obcy `Department` i właściwość nawigacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-269">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="2d7b6-270">Kurs może zawierać dowolną liczbę studentów, więc `Enrollments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-270">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="2d7b6-271">Kurs może być nauczany przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-271">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="2d7b6-272">`CourseAssignment`wyjaśniono [później](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-272">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="2d7b6-273">Jednostka działu</span><span class="sxs-lookup"><span data-stu-id="2d7b6-273">The Department entity</span></span>

![Jednostka działu](complex-data-model/_static/department-entity.png)

<span data-ttu-id="2d7b6-275">Utwórz *modele/dział. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-275">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="2d7b6-276">Atrybut Column</span><span class="sxs-lookup"><span data-stu-id="2d7b6-276">The Column attribute</span></span>

<span data-ttu-id="2d7b6-277">`Column` Wcześniej atrybut został użyty do zmiany mapowania nazw kolumn.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-277">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="2d7b6-278">W kodzie dla `Department` jednostki `Column` atrybut jest używany do zmiany mapowania typu danych SQL.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-278">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="2d7b6-279">`Budget` Kolumna jest definiowana przy użyciu SQL Server typie pieniędzy w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-279">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="2d7b6-280">Mapowanie kolumn zazwyczaj nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-280">Column mapping is generally not required.</span></span> <span data-ttu-id="2d7b6-281">EF Core wybiera odpowiedni SQL Server typ danych na podstawie typu CLR dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-281">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="2d7b6-282">Typ CLR `decimal` jest mapowany na typ SQL Server `decimal` .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-282">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="2d7b6-283">`Budget`jest dla waluty, a typ danych walutowych jest bardziej odpowiedni dla waluty.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-283">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d7b6-284">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-284">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d7b6-285">Właściwości FK i nawigacji odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-285">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="2d7b6-286">Dział może lub nie ma uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-286">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="2d7b6-287">Administrator jest zawsze instruktorem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-287">An administrator is always an instructor.</span></span> <span data-ttu-id="2d7b6-288">W związku z tym `Instructor` Właściwośćjestdołączanajakoobcydojednostki.`InstructorID`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-288">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="2d7b6-289">Właściwość nawigacji ma nazwę `Administrator` , ale `Instructor` zawiera jednostkę:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-289">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="2d7b6-290">Znak zapytania (?) w powyższym kodzie określa właściwość dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-290">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="2d7b6-291">Dział może mieć wiele kursów, więc istnieje właściwość nawigacji kursów:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-291">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="2d7b6-292">Zgodnie z Konwencją EF Core włącza kaskadowe usuwanie dla FKs niedopuszczających wartości null i dla relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-292">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="2d7b6-293">To domyślne zachowanie może spowodować cykliczne reguły usuwania kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-293">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="2d7b6-294">Cykliczne reguły usuwania kaskadowego powodują wyjątek podczas dodawania migracji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-294">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="2d7b6-295">Na przykład jeśli `Department.InstructorID` właściwość została zdefiniowana jako niedopuszczający wartości null, EF Core skonfigurować regułę usuwania kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-295">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="2d7b6-296">W takim przypadku dział zostanie usunięty po usunięciu instruktora przypisanego jako administrator.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-296">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="2d7b6-297">W tym scenariuszu reguła ograniczania byłaby bardziej zrozumiała.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-297">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="2d7b6-298">Poniższy interfejs API Fluent ustawi regułę ograniczenia i wyłącza usuwanie kaskadowe.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-298">The following fluent API would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="2d7b6-299">Jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-299">The Enrollment entity</span></span>

<span data-ttu-id="2d7b6-300">Rekord rejestracji jest wykonywany przez jednego ucznia.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-300">An enrollment record is for one course taken by one student.</span></span>

![Jednostka rejestracji](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="2d7b6-302">Aktualizuj *modele/rejestrację. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-302">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d7b6-303">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-303">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d7b6-304">Właściwości klucza obcego i właściwości nawigacji odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-304">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="2d7b6-305">Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje `CourseID` Właściwość FK `Course` i właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-305">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="2d7b6-306">Rekord rejestracji jest przeznaczony dla jednego ucznia, dlatego istnieje `StudentID` właściwość klucza obcego `Student` i właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-306">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="2d7b6-307">Relacje wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="2d7b6-307">Many-to-Many Relationships</span></span>

<span data-ttu-id="2d7b6-308">Istnieje relacja wiele-do-wielu między `Student` obiektami i. `Course`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-308">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="2d7b6-309">Jednostka działa jako tabela sprzężenia wiele-do-wielu *z ładunkiem* w bazie danych. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-309">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="2d7b6-310">"Z ładunkiem" oznacza, że `Enrollment` tabela zawiera dodatkowe dane oprócz FKs dla sprzężonych tabel (w tym przypadku klucz podstawowy i `Grade`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-310">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="2d7b6-311">Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-311">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="2d7b6-312">(Ten diagram został wygenerowany przy użyciu [narzędzi EF PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) dla programu EF 6. x.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-312">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="2d7b6-313">Tworzenie diagramu nie jest częścią samouczka.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-313">Creating the diagram isn't part of the tutorial.)</span></span>

![Student-kurs wiele do wielu relacji](complex-data-model/_static/student-course.png)

<span data-ttu-id="2d7b6-315">Każda linia relacji ma 1 na jednym końcu i gwiazdkę (\*) na drugim, wskazując relację jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-315">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="2d7b6-316">Jeśli tabela nie zawiera informacji o klasie, musi zawierać tylko dwa FKs (`CourseID` i `StudentID`). `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-316">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="2d7b6-317">Tabela sprzężenia wiele-do-wielu bez ładunku jest czasami nazywana tabelą sprzężenia czystego (PJT).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-317">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="2d7b6-318">Jednostki `Instructor` i`Course` mają relację wiele-do-wielu przy użyciu czystej tabeli sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-318">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="2d7b6-319">Uwaga: Dr 6. x obsługuje niejawne tabele sprzężeń dla relacji wiele-do-wielu, ale EF Core nie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-319">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="2d7b6-320">Aby uzyskać więcej informacji, zobacz [relacje wiele-do-wielu w EF Core 2,0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-320">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="2d7b6-321">Jednostka CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="2d7b6-321">The CourseAssignment entity</span></span>

![Jednostka CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="2d7b6-323">Utwórz *modele/CourseAssignment. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-323">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="2d7b6-324">Relacja "instruktora" do wielu kursów wymaga tabeli sprzężenia, a jednostka dla tej tabeli sprzężenia jest CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-324">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="2d7b6-326">Nazwa jednostki `EntityName1EntityName2`sprzężenia jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-326">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="2d7b6-327">Na przykład tabela sprzężenia "instruktora do kursu" używa tego wzorca `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-327">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="2d7b6-328">Zalecamy jednak użycie nazwy opisującej relację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-328">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="2d7b6-329">Modele danych rozpoczynają się od siebie i rosną.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-329">Data models start out simple and grow.</span></span> <span data-ttu-id="2d7b6-330">Tabele sprzężenia bez ładunku (PJTs) często są rozwijane w celu uwzględnienia ładunku.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-330">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="2d7b6-331">Rozpoczynając od nazwy obiektu opisowego, nie trzeba zmieniać nazwy po zmianie tabeli sprzężeń.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-331">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="2d7b6-332">Najlepiej, aby jednostka sprzężenia miała własną nazwę naturalną (prawdopodobnie jednowyrazową) w domenie biznesowej.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-332">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="2d7b6-333">Na przykład książki i klienci mogą być połączone z jednostką sprzężenia o nazwie ratings.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-333">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="2d7b6-334">Dla instruktora "wiele do wielu", `CourseAssignment` `CourseInstructor`preferowana jest wartość.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-334">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="2d7b6-335">Klucz złożony</span><span class="sxs-lookup"><span data-stu-id="2d7b6-335">Composite key</span></span>

<span data-ttu-id="2d7b6-336">Dwa FKs w `CourseAssignment` (`InstructorID` `CourseID` i`CourseAssignment` ) jednoznacznie identyfikują każdy wiersz tabeli.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-336">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="2d7b6-337">`CourseAssignment`nie wymaga dedykowanego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-337">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="2d7b6-338">Właściwości `InstructorID` i`CourseID` działają jako złożony klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-338">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="2d7b6-339">Jedynym sposobem określenia złożonego PKs do EF Core jest *interfejs API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-339">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="2d7b6-340">W następnej sekcji przedstawiono sposób konfigurowania złożonego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-340">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="2d7b6-341">Klucz złożony gwarantuje, że:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-341">The composite key ensures that:</span></span>

* <span data-ttu-id="2d7b6-342">W jednym kursie są dozwolone wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-342">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="2d7b6-343">Dla jednego instruktora są dozwolone wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-343">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="2d7b6-344">Wiele wierszy nie jest dozwolonych dla tego samego instruktora i kursu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-344">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="2d7b6-345">Jednostka `Enrollment` Join definiuje własny klucz podstawowy, więc możliwe jest duplikowanie tego sortowania.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-345">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="2d7b6-346">Aby uniknąć takich duplikatów:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-346">To prevent such duplicates:</span></span>

* <span data-ttu-id="2d7b6-347">Dodaj unikatowy indeks do pól klucza obcego lub</span><span class="sxs-lookup"><span data-stu-id="2d7b6-347">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="2d7b6-348">Skonfiguruj `Enrollment` przy użyciu podstawowego klucza złożonego podobnego `CourseAssignment`do.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-348">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="2d7b6-349">Aby uzyskać więcej informacji, zobacz [indeksy](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-349">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="2d7b6-350">Aktualizowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d7b6-350">Update the database context</span></span>

<span data-ttu-id="2d7b6-351">Zaktualizuj *dane/SchoolContext. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-351">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="2d7b6-352">Poprzedni kod dodaje nowe jednostki i konfiguruje `CourseAssignment` złożonego klucza podstawowego jednostki.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-352">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="2d7b6-353">Alternatywny interfejs API Fluent dla atrybutów</span><span class="sxs-lookup"><span data-stu-id="2d7b6-353">Fluent API alternative to attributes</span></span>

<span data-ttu-id="2d7b6-354">Metoda w poprzednim kodzie używa *interfejsu API Fluent* , aby skonfigurować zachowanie EF Core. `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-354">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="2d7b6-355">Interfejs API jest nazywany "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod w jednej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-355">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="2d7b6-356">[Poniższy kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) jest przykładem interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-356">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="2d7b6-357">W tym samouczku interfejs API Fluent jest używany tylko na potrzeby mapowania bazy danych, której nie można wykonać przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-357">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="2d7b6-358">Jednak interfejs API Fluent może określić większość reguł formatowania, walidacji i mapowania, które mogą być wykonywane przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-358">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="2d7b6-359">Niektórych atrybutów, takich `MinimumLength` jak nie można zastosować w interfejsie API Fluent.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-359">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="2d7b6-360">`MinimumLength`nie zmienia schematu, stosuje tylko regułę walidacji o minimalnej długości.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-360">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="2d7b6-361">Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-361">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="2d7b6-362">Atrybuty i interfejs API Fluent mogą być mieszane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-362">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="2d7b6-363">Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu interfejsu API Fluent (określenie złożonego klucza podstawowego).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-363">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="2d7b6-364">Istnieją pewne konfiguracje, które można wykonać tylko z atrybutami (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-364">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="2d7b6-365">Zalecane rozwiązanie dotyczące korzystania z interfejsu API Fluent lub atrybutów:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-365">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="2d7b6-366">Wybierz jedną z tych dwóch metod.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-366">Choose one of these two approaches.</span></span>
* <span data-ttu-id="2d7b6-367">W miarę możliwości należy stosować wybrane podejście.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-367">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="2d7b6-368">Niektóre atrybuty użyte w tym samouczku są używane w programie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-368">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="2d7b6-369">Tylko Walidacja (na przykład `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-369">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="2d7b6-370">Tylko konfiguracja EF Core (na przykład `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-370">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="2d7b6-371">Sprawdzanie poprawności i konfiguracja EF Core (na przykład `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-371">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="2d7b6-372">Aby uzyskać więcej informacji na temat atrybutów a interfejs API Fluent, zobacz [metody konfiguracji](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-372">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="2d7b6-373">Diagram jednostek</span><span class="sxs-lookup"><span data-stu-id="2d7b6-373">Entity diagram</span></span>

<span data-ttu-id="2d7b6-374">Na poniższej ilustracji przedstawiono diagram, który jest tworzony przez narzędzia Dr PowerShell dla gotowego modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-374">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="2d7b6-376">Na powyższym diagramie przedstawiono:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-376">The preceding diagram shows:</span></span>

* <span data-ttu-id="2d7b6-377">Kilka linii relacji jeden-do-wielu (od 1 \*do).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-377">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="2d7b6-378">Linia relacji "jeden do zera" (od 1 do 0.. 1) między `Instructor` jednostkami i. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-378">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="2d7b6-379">Linia relacji zero-lub-jeden-do-wielu (0.. 1 do \*) między `Instructor` obiektami i. `Department`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-379">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="2d7b6-380">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d7b6-380">Seed the database</span></span>

<span data-ttu-id="2d7b6-381">Zaktualizuj kod w *danych/Dbinitializeer. cs*:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-381">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="2d7b6-382">Poprzedni kod zawiera dane inicjatora dla nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-382">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="2d7b6-383">Większość tego kodu tworzy nowe obiekty Entity i ładuje przykładowe dane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-383">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="2d7b6-384">Przykładowe dane służą do testowania.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-384">The sample data is used for testing.</span></span> <span data-ttu-id="2d7b6-385">Zobacz `Enrollments` i `CourseAssignments` , aby zapoznać się z przykładami tabel sprzężenia wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-385">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="2d7b6-386">Dodawanie migracji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-386">Add a migration</span></span>

<span data-ttu-id="2d7b6-387">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-387">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-388">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-388">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d7b6-389">W obszarze PMC Uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-389">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="2d7b6-390">Poprzednie polecenie wyświetla ostrzeżenie o możliwej utracie danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-390">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="2d7b6-391">`database update` Jeśli polecenie zostanie uruchomione, zostanie utworzony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-391">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="2d7b6-392">W następnej sekcji opisano, co należy zrobić, aby uzyskać informacje o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-392">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-393">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-393">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2d7b6-394">W przypadku dodania migracji i uruchomienia `database update` polecenia zostanie utworzony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-394">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="2d7b6-395">W następnej sekcji zobaczysz, jak uniknąć tego błędu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-395">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="2d7b6-396">Zastosuj migrację lub Porzuć i Utwórz ponownie</span><span class="sxs-lookup"><span data-stu-id="2d7b6-396">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="2d7b6-397">Teraz, gdy masz już istniejącą bazę danych, musisz się zastanowić, jak zastosować do niej zmiany.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-397">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="2d7b6-398">Ten samouczek przedstawia dwie alternatywy:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-398">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="2d7b6-399">[Porzuć i ponownie utwórz bazę danych](#drop).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-399">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="2d7b6-400">Wybierz tę sekcję, jeśli używasz oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-400">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="2d7b6-401">[Zastosuj migrację do istniejącej bazy danych](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-401">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="2d7b6-402">Instrukcje w tej sekcji działają tylko w przypadku SQL Server, a **nie dla oprogramowania SQLite**.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-402">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="2d7b6-403">Dowolny wybór działa dla SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-403">Either choice works for SQL Server.</span></span> <span data-ttu-id="2d7b6-404">Chociaż metoda Apply-Migration jest bardziej złożona i czasochłonna, jest to preferowane podejście do rzeczywistych środowisk produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-404">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="2d7b6-405">Porzuć i ponownie utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="2d7b6-405">Drop and re-create the database</span></span>

<span data-ttu-id="2d7b6-406">[Pomiń tę sekcję](#apply-the-migration) , jeśli używasz SQL Server i chcesz wykonać podejście Apply-Migration w poniższej sekcji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-406">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="2d7b6-407">Aby wymusić EF Core tworzenia nowej bazy danych, Porzuć i zaktualizuj bazę danych:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-407">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-408">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-408">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2d7b6-409">W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-409">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="2d7b6-410">Usuń folder *migrations* , a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-410">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-411">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-411">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2d7b6-412">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-412">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="2d7b6-413">Folder projektu zawiera plik *ContosoUniversity. csproj* .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-413">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="2d7b6-414">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-414">Run the following command:</span></span>

  ```console
  dotnet ef database drop --force
  ```

* <span data-ttu-id="2d7b6-415">Usuń folder *migrations* , a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-415">Delete the *Migrations* folder, then run the following command:</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="2d7b6-416">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-416">Run the app.</span></span> <span data-ttu-id="2d7b6-417">Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-417">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="2d7b6-418">`DbInitializer.Initialize` Wypełnia nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-418">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-419">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-419">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d7b6-420">Otwórz bazę danych w programie SSOX:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-420">Open the database in SSOX:</span></span>

* <span data-ttu-id="2d7b6-421">Jeśli SSOX został wcześniej otwarty, kliknij przycisk **Odśwież** .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-421">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="2d7b6-422">Rozwiń **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-422">Expand the **Tables** node.</span></span> <span data-ttu-id="2d7b6-423">Zostaną wyświetlone utworzone tabele.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-423">The created tables are displayed.</span></span>

  ![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="2d7b6-425">Zapoznaj się z tabelą **CourseAssignment** :</span><span class="sxs-lookup"><span data-stu-id="2d7b6-425">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="2d7b6-426">Kliknij prawym przyciskiem myszy tabelę **CourseAssignment** , a następnie wybierz polecenie **Wyświetl dane**.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-426">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="2d7b6-427">Sprawdź, czy tabela **CourseAssignment** zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-427">Verify the **CourseAssignment** table contains data.</span></span>

  ![CourseAssignment dane w SSOX](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-429">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-429">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2d7b6-430">Sprawdź bazę danych za pomocą narzędzia SQLite:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-430">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="2d7b6-431">Nowe tabele i kolumny.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-431">New tables and columns.</span></span>
* <span data-ttu-id="2d7b6-432">Dane są umieszczane w tabelach, na przykład w tabeli **CourseAssignment** .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-432">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="2d7b6-433">Zastosuj migrację</span><span class="sxs-lookup"><span data-stu-id="2d7b6-433">Apply the migration</span></span>

<span data-ttu-id="2d7b6-434">Ta sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-434">This section is optional.</span></span> <span data-ttu-id="2d7b6-435">Te kroki działają tylko dla SQL Server LocalDB i tylko wtedy, gdy pominięto poprzednią [i ponownie utworzysz sekcję Database](#drop) .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-435">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="2d7b6-436">Po uruchomieniu migracji z istniejącymi danymi mogą istnieć ograniczenia klucza obcego, które nie są spełnione w przypadku istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-436">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="2d7b6-437">W przypadku danych produkcyjnych należy wykonać kroki, aby przeprowadzić migrację istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-437">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="2d7b6-438">Ta sekcja zawiera przykład rozwiązywania naruszeń ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-438">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="2d7b6-439">Nie należy wprowadzać tych zmian w kodzie bez tworzenia kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-439">Don't make these code changes without a backup.</span></span> <span data-ttu-id="2d7b6-440">Nie wprowadzaj tych zmian w kodzie, jeśli wykonano poprzednie [upuszczenie i ponownie utworzysz sekcję bazy danych](#drop) .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-440">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="2d7b6-441">Plik *{timestamp} _ComplexDataModel. cs* zawiera następujący kod:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-441">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="2d7b6-442">Poprzedzający kod dodaje `DepartmentID` `Course` do tabeli obcy niedopuszczający wartości null.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-442">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="2d7b6-443">Baza danych z poprzedniego samouczka zawiera wiersze w `Course`, dlatego nie można zaktualizować tabeli za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-443">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="2d7b6-444">Aby `ComplexDataModel` migracja była współdziałać z istniejącymi danymi:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-444">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="2d7b6-445">Zmień kod, aby nadać nowej kolumnie (`DepartmentID`) wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-445">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="2d7b6-446">Utwórz fałszywy Departament o nazwie "Temp", który ma pełnić rolę działu domyślnego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-446">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="2d7b6-447">Popraw ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="2d7b6-447">Fix the foreign key constraints</span></span>

<span data-ttu-id="2d7b6-448">W klasie `Up` migracji należy zaktualizować metodę: `ComplexDataModel`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-448">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="2d7b6-449">Otwórz plik *{timestamp} _ComplexDataModel. cs* .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-449">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="2d7b6-450">Dodaj komentarz `DepartmentID` `Course` do wiersza kodu, który dodaje kolumnę do tabeli.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-450">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="2d7b6-451">Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-451">Add the following highlighted code.</span></span> <span data-ttu-id="2d7b6-452">Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-452">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

<span data-ttu-id="2d7b6-453">[! code-CSharp [] (wprowadzenie/przykłady/cu30snapshots/5 — złożone/migracje/ComplexDataModel. cs? Name = snippet_CreateDefaultValue & Podświetl = 23-31)]</span><span class="sxs-lookup"><span data-stu-id="2d7b6-453">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span></span>

<span data-ttu-id="2d7b6-454">Po wykonaniu powyższych zmian istniejące `Course` wiersze będą powiązane z działem "Temp" po uruchomieniu metody.`ComplexDataModel.Up`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-454">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="2d7b6-455">Sposób obsługi pokazanej tutaj sytuacji jest uproszczony dla tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-455">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="2d7b6-456">Aplikacja produkcyjna:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-456">A production app would:</span></span>

* <span data-ttu-id="2d7b6-457">Dołącz kod lub skrypty, aby `Department` dodać wiersze i `Course` powiązane wiersze do nowych `Department` wierszy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-457">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="2d7b6-458">Nie używaj działu "Temp" ani wartości domyślnej dla `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-458">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-459">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-459">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2d7b6-460">W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-460">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="2d7b6-461">`DbInitializer.Initialize` Ponieważ metoda została zaprojektowana tak, aby działała tylko z pustą bazą danych, użyj SSOX, aby usunąć wszystkie wiersze z tabel uczniów i kursów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-461">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="2d7b6-462">(Usuwanie kaskadowe zajmie tabelę rejestracji).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-462">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-463">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-463">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2d7b6-464">Jeśli używasz SQL Server LocalDB z Visual Studio Code, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-464">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```console
  dotnet ef database update
  ```

---

<span data-ttu-id="2d7b6-465">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-465">Run the app.</span></span> <span data-ttu-id="2d7b6-466">Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-466">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="2d7b6-467">`DbInitializer.Initialize` Wypełnia nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-467">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d7b6-468">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2d7b6-468">Next steps</span></span>

<span data-ttu-id="2d7b6-469">W następnych dwóch samouczkach pokazano, jak odczytywać i aktualizować powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-469">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2d7b6-470">[Poprzedni](xref:data/ef-rp/migrations)
> samouczek w[następnym](xref:data/ef-rp/read-related-data) samouczku</span><span class="sxs-lookup"><span data-stu-id="2d7b6-470">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2d7b6-471">Poprzednie samouczki pracowały z podstawowym modelem danych, który składa się z trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-471">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="2d7b6-472">W tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-472">In this tutorial:</span></span>

* <span data-ttu-id="2d7b6-473">Dodano więcej jednostek i relacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-473">More entities and relationships are added.</span></span>
* <span data-ttu-id="2d7b6-474">Model danych jest dostosowywany przez określenie formatowania, walidacji i reguł mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-474">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="2d7b6-475">Klasy jednostek dla kompletnego modelu danych przedstawiono na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-475">The entity classes for the completed data model are shown in the following illustration:</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="2d7b6-477">Jeśli wystąpią problemy, których nie można rozwiązać, Pobierz [ukończoną](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)aplikację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-477">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="2d7b6-478">Dostosowywanie modelu danych przy użyciu atrybutów</span><span class="sxs-lookup"><span data-stu-id="2d7b6-478">Customize the data model with attributes</span></span>

<span data-ttu-id="2d7b6-479">W tej sekcji model danych jest dostosowany przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-479">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="2d7b6-480">Atrybut DataType</span><span class="sxs-lookup"><span data-stu-id="2d7b6-480">The DataType attribute</span></span>

<span data-ttu-id="2d7b6-481">Na stronach ucznia jest obecnie wyświetlana godzina daty rejestracji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-481">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="2d7b6-482">Zwykle pola Date zawierają tylko datę, a nie czas.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-482">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="2d7b6-483">Aktualizuj *modele/uczniów. cs* przy użyciu następującego wyróżnionego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-483">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="2d7b6-484">Atrybut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) określa typ danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-484">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="2d7b6-485">W tym przypadku należy wyświetlić tylko datę, a nie datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-485">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="2d7b6-486">[Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress itp. Ten `DataType` atrybut może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-486">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="2d7b6-487">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-487">For example:</span></span>

* <span data-ttu-id="2d7b6-488">Łącze jest tworzone automatycznie dla `DataType.EmailAddress`. `mailto:`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-488">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="2d7b6-489">Selektor daty jest dostępny `DataType.Date` w większości przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-489">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="2d7b6-490">Ten `DataType` atrybut emituje kod HTML `data-` 5 (wymawiane kreski danych), które wykorzystują przeglądarki HTML 5.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-490">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="2d7b6-491">`DataType` Atrybuty nie zapewniają walidacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-491">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="2d7b6-492">`DataType.Date`nie określa formatu wyświetlanej daty.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-492">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="2d7b6-493">Domyślnie pole Date jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)serwera.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-493">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="2d7b6-494">Ten `DisplayFormat` atrybut służy do jawnego określenia formatu daty:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-494">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="2d7b6-495">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie ma być również stosowane do interfejsu użytkownika edytowania.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-495">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="2d7b6-496">Niektóre pola nie powinny `ApplyFormatInEditMode`być używane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-496">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="2d7b6-497">Na przykład symbol waluty zazwyczaj nie powinien być wyświetlany w polu tekstowym Edycja.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-497">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="2d7b6-498">Ten `DisplayFormat` atrybut może być używany przez siebie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-498">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="2d7b6-499">Zwykle dobrym pomysłem jest użycie `DataType` atrybutu `DisplayFormat` z atrybutem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-499">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="2d7b6-500">Ten `DataType` atrybut przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-500">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="2d7b6-501">Ten `DataType` atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`programie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-501">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="2d7b6-502">Przeglądarka może włączyć funkcje HTML5.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-502">The browser can enable HTML5 features.</span></span> <span data-ttu-id="2d7b6-503">Na przykład Pokaż kontrolkę kalendarz, odpowiedni dla ustawień regionalnych symbol waluty, linki poczty e-mail, sprawdzanie poprawności danych po stronie klienta itd.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-503">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="2d7b6-504">Domyślnie przeglądarka renderuje dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-504">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="2d7b6-505">Aby uzyskać więcej informacji, zobacz [ \<dokumentację pomocnika tagów > danych wejściowych](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-505">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="2d7b6-506">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-506">Run the app.</span></span> <span data-ttu-id="2d7b6-507">Przejdź do strony indeksu uczniów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-507">Navigate to the Students Index page.</span></span> <span data-ttu-id="2d7b6-508">Czasy nie są już wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-508">Times are no longer displayed.</span></span> <span data-ttu-id="2d7b6-509">Każdy widok korzystający z `Student` modelu wyświetla datę bez czasu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-509">Every view that uses the `Student` model displays the date without time.</span></span>

![Strona indeksu uczniów pokazująca daty bez czasu](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="2d7b6-511">Atrybut StringLength</span><span class="sxs-lookup"><span data-stu-id="2d7b6-511">The StringLength attribute</span></span>

<span data-ttu-id="2d7b6-512">Można określić reguły walidacji danych i komunikaty o błędach walidacji z atrybutami.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-512">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="2d7b6-513">Atrybut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-513">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="2d7b6-514">Ten `StringLength` atrybut zapewnia również weryfikację po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-514">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="2d7b6-515">Wartość minimalna nie ma wpływu na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-515">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="2d7b6-516">`Student` Zaktualizuj model przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-516">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="2d7b6-517">Poprzedni kod ogranicza nazwy do nie więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-517">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="2d7b6-518">Ten `StringLength` atrybut nie uniemożliwia użytkownikowi wprowadzania białych znaków w nazwie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-518">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="2d7b6-519">Atrybut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) służy do stosowania ograniczeń do danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-519">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="2d7b6-520">Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-520">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="2d7b6-521">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-521">Run the app:</span></span>

* <span data-ttu-id="2d7b6-522">Przejdź do strony uczniów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-522">Navigate to the Students page.</span></span>
* <span data-ttu-id="2d7b6-523">Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę o długości większej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-523">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="2d7b6-524">Wybierz pozycję **Utwórz**, a po stronie klienta zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-524">Select **Create**, client-side validation shows an error message.</span></span>

![Strona indeksu studentów przedstawiająca błędy długości ciągu](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="2d7b6-526">W **Eksplorator obiektów SQL Server** (SSOX) Otwórz projektanta tabeli uczniów, klikając dwukrotnie tabelę **uczniów** .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-526">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela studentów w SSOX przed migracją](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="2d7b6-528">Na powyższym obrazie przedstawiono schemat `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-528">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="2d7b6-529">Pola nazw mają typ `nvarchar(MAX)` , ponieważ migracja nie została uruchomiona w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-529">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="2d7b6-530">Gdy migracja zostanie uruchomiona w dalszej części tego samouczka, pola nazwy `nvarchar(50)`stają się.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-530">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="2d7b6-531">Atrybut Column</span><span class="sxs-lookup"><span data-stu-id="2d7b6-531">The Column attribute</span></span>

<span data-ttu-id="2d7b6-532">Atrybuty mogą kontrolować sposób mapowania klas i właściwości do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-532">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="2d7b6-533">W tej sekcji `Column` atrybut jest używany do mapowania nazwy `FirstMidName` właściwości na "FirstName" w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-533">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="2d7b6-534">Po utworzeniu bazy danych nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, `Column` gdy atrybut jest używany).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-534">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="2d7b6-535">`Student` Model używa`FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-535">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="2d7b6-536">Zaktualizuj plik *student.cs* za pomocą następującego wyróżnionego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-536">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="2d7b6-537">Z poprzednią zmianą `Student.FirstMidName` w aplikacji jest mapowany `FirstName` do kolumny `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-537">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="2d7b6-538">Dodanie `Column` atrybutu zmienia model z `SchoolContext`kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-538">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="2d7b6-539">Model, który wykonuje `SchoolContext` kopię zapasową, nie jest już zgodny z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-539">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="2d7b6-540">Jeśli aplikacja jest uruchamiana przed zastosowaniem migracji, generowany jest następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-540">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="2d7b6-541">Aby zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-541">To update the DB:</span></span>

* <span data-ttu-id="2d7b6-542">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-542">Build the project.</span></span>
* <span data-ttu-id="2d7b6-543">Otwórz okno polecenia w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-543">Open a command window in the project folder.</span></span> <span data-ttu-id="2d7b6-544">Wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-544">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-545">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-545">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-546">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-546">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="2d7b6-547">`migrations add ColumnFirstName` Polecenie generuje następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-547">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="2d7b6-548">Ostrzeżenie jest generowane, ponieważ pola nazw są teraz ograniczone do 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-548">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="2d7b6-549">Jeśli nazwa w bazie danych ma więcej niż 50 znaków, zostanie utracony od 51 do ostatniego znaku.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-549">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="2d7b6-550">Przetestuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-550">Test the app.</span></span>

<span data-ttu-id="2d7b6-551">Otwórz tabelę uczniów w SSOX:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-551">Open the Student table in SSOX:</span></span>

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="2d7b6-553">Przed zainstalowaniem migracji kolumny nazw były typu [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-553">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="2d7b6-554">Kolumny nazw są teraz `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-554">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="2d7b6-555">Nazwa kolumny została zmieniona z `FirstMidName` na. `FirstName`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-555">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="2d7b6-556">W poniższej sekcji Kompilowanie aplikacji na niektórych etapach powoduje wygenerowanie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-556">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="2d7b6-557">Instrukcje określają, kiedy należy skompilować aplikację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-557">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="2d7b6-558">Aktualizacja jednostki ucznia</span><span class="sxs-lookup"><span data-stu-id="2d7b6-558">Student entity update</span></span>

![Jednostka ucznia](complex-data-model/_static/student-entity.png)

<span data-ttu-id="2d7b6-560">Aktualizuj *modele/uczniów. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-560">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="2d7b6-561">Wymagany atrybut</span><span class="sxs-lookup"><span data-stu-id="2d7b6-561">The Required attribute</span></span>

<span data-ttu-id="2d7b6-562">`Required` Atrybut powoduje, że właściwości nazwy są wymagane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-562">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="2d7b6-563">Ten `Required` atrybut nie jest wymagany dla typów niedopuszczających wartości null, takich jak `int`typy `double`wartości (`DateTime`,, itp.).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-563">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="2d7b6-564">Typy, które nie mogą mieć wartości null, są automatycznie traktowane jako pola wymagane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-564">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="2d7b6-565">Ten `Required` atrybut może być zastąpiony parametrem o minimalnej długości `StringLength` w atrybucie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-565">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="2d7b6-566">Atrybut wyświetlania</span><span class="sxs-lookup"><span data-stu-id="2d7b6-566">The Display attribute</span></span>

<span data-ttu-id="2d7b6-567">Ten `Display` atrybut określa, że podpis pól tekstowych powinien mieć wartość "imię i nazwisko", "nazwisko", "imię i nazwisko" oraz "Data rejestracji".</span><span class="sxs-lookup"><span data-stu-id="2d7b6-567">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="2d7b6-568">Domyślne podpisy nie zawierają spacji dzielących wyrazy, na przykład "LastName".</span><span class="sxs-lookup"><span data-stu-id="2d7b6-568">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="2d7b6-569">Właściwość obliczeniowa FullName</span><span class="sxs-lookup"><span data-stu-id="2d7b6-569">The FullName calculated property</span></span>

<span data-ttu-id="2d7b6-570">`FullName`jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-570">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="2d7b6-571">`FullName`nie można ustawić, ma tylko metodę dostępu get.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-571">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="2d7b6-572">Nie `FullName` utworzono żadnej kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-572">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="2d7b6-573">Tworzenie jednostki instruktora</span><span class="sxs-lookup"><span data-stu-id="2d7b6-573">Create the Instructor Entity</span></span>

![Jednostka instruktora](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="2d7b6-575">Utwórz *modele/instruktor. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-575">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="2d7b6-576">Wiele atrybutów może znajdować się w jednym wierszu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-576">Multiple attributes can be on one line.</span></span> <span data-ttu-id="2d7b6-577">`HireDate` Atrybuty mogą być zapisywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-577">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="2d7b6-578">Właściwości nawigacji CourseAssignments i OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="2d7b6-578">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="2d7b6-579">Właściwości `CourseAssignments` i`OfficeAssignment` są właściwościami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-579">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="2d7b6-580">Instruktor może nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-580">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="2d7b6-581">Jeśli właściwość nawigacji zawiera wiele jednostek:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-581">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="2d7b6-582">Musi to być typ listy, w którym można dodawać, usuwać i aktualizować wpisy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-582">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="2d7b6-583">Typy właściwości nawigacji obejmują:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-583">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="2d7b6-584">Jeśli `ICollection<T>` jest określony, EF Core domyślnie `HashSet<T>` tworzy kolekcję.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-584">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="2d7b6-585">`CourseAssignment` Jednostka została omówiona w sekcji dotyczącej relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-585">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="2d7b6-586">Firma Contoso University Rules States, że instruktor może mieć co najwyżej jednego biura.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-586">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="2d7b6-587">Właściwość zawiera jedną `OfficeAssignment`jednostkę. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-587">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="2d7b6-588">`OfficeAssignment`ma wartość null, jeśli nie przypisano pakietu Office.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-588">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="2d7b6-589">Tworzenie jednostki OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="2d7b6-589">Create the OfficeAssignment entity</span></span>

![Jednostka OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="2d7b6-591">Utwórz *modele/OfficeAssignment. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-591">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="2d7b6-592">Atrybut klucza</span><span class="sxs-lookup"><span data-stu-id="2d7b6-592">The Key attribute</span></span>

<span data-ttu-id="2d7b6-593">Ten `[Key]` atrybut służy do identyfikowania właściwości jako klucza podstawowego (PK), gdy nazwa właściwości ma wartość inną niż classnameID lub ID.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-593">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="2d7b6-594">Istnieje relacja jeden do zera między `Instructor` jednostkami i. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-594">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="2d7b6-595">Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-595">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="2d7b6-596">Klucz podstawowy jest również jego kluczem obcym (obcy) `Instructor` do jednostki. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-596">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="2d7b6-597">EF Core nie może automatycznie `InstructorID` rozpoznać jako klucz podstawowy `OfficeAssignment` dla:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-597">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="2d7b6-598">`InstructorID`nie jest zgodna z konwencją nazewnictwa ID lub classnameID.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-598">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="2d7b6-599">W związku z tym `InstructorID` atrybutjestużywanydoidentyfikacjijakokluczpodstawowy:`Key`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-599">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="2d7b6-600">Domyślnie EF Core traktuje klucz jako wygenerowane poza bazą danych, ponieważ kolumna służy do identyfikowania relacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-600">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="2d7b6-601">Właściwość nawigacji instruktora</span><span class="sxs-lookup"><span data-stu-id="2d7b6-601">The Instructor navigation property</span></span>

<span data-ttu-id="2d7b6-602">Właściwość `OfficeAssignment` nawigacji `Instructor` dla jednostki nie dopuszcza wartości null, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-602">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="2d7b6-603">Typy odwołań (takie jak klasy są dopuszczające wartość null).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-603">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="2d7b6-604">Instruktor może nie mieć przypisania pakietu Office.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-604">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="2d7b6-605">Jednostka ma właściwość nawigacji, która nie `Instructor` dopuszcza wartości null, ponieważ: `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-605">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="2d7b6-606">`InstructorID`nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-606">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="2d7b6-607">Przypisanie pakietu Office nie może istnieć bez instruktora.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-607">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="2d7b6-608">Gdy jednostka ma powiązaną `OfficeAssignment` jednostkę, każda jednostka ma odwołanie do drugiej z nich we właściwości nawigacji. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-608">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="2d7b6-609">Ten `[Required]` atrybut może zostać zastosowany `Instructor` do właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-609">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="2d7b6-610">Poprzedni kod określa, że musi być pokrewnym instruktorem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-610">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="2d7b6-611">Poprzedni kod jest niezbędny, `InstructorID` ponieważ klucz obcy (który jest również kluczem PK) nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-611">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="2d7b6-612">Modyfikowanie jednostki kursu</span><span class="sxs-lookup"><span data-stu-id="2d7b6-612">Modify the Course Entity</span></span>

![Jednostka kursu](complex-data-model/_static/course-entity.png)

<span data-ttu-id="2d7b6-614">Aktualizuj *modele/kurs. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-614">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="2d7b6-615">Jednostka ma właściwość `DepartmentID`klucza obcego (obcy). `Course`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-615">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="2d7b6-616">`DepartmentID`wskazuje powiązaną `Department` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-616">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="2d7b6-617">`Course` Jednostka ma właściwość nawigacji. `Department`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-617">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="2d7b6-618">EF Core nie wymaga właściwości FK dla modelu danych, gdy model ma właściwość nawigacji dla powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-618">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="2d7b6-619">EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie są one zbędne.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-619">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="2d7b6-620">EF Core tworzy [Właściwości cienia](/ef/core/modeling/shadow-properties) dla automatycznie utworzonych FKs.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-620">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="2d7b6-621">Posiadanie klucza obcego w modelu danych może sprawiać, że aktualizacje są prostsze i wydajniejsze.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-621">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="2d7b6-622">Rozważmy na przykład model, w którym Właściwość `DepartmentID` FK *nie* jest uwzględniona.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-622">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="2d7b6-623">Gdy jednostka kursu jest pobierana do edycji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-623">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="2d7b6-624">Jednostka `Department` ma wartość null, jeśli nie jest jawnie załadowana.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-624">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="2d7b6-625">Aby zaktualizować jednostkę kursu, `Department` należy najpierw pobrać jednostkę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-625">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="2d7b6-626">Gdy właściwość `DepartmentID` FK jest uwzględniona w modelu danych, nie trzeba `Department` pobierać jednostki przed aktualizacją.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-626">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="2d7b6-627">Atrybut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="2d7b6-627">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="2d7b6-628">Ten `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atrybut określa, że klucz podstawowy jest dostarczany przez aplikację, a nie generowany przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-628">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="2d7b6-629">Domyślnie EF Core zakłada, że wartości klucza PK są generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-629">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="2d7b6-630">Wartość klucza podstawowego wygenerowanego przez bazę danych jest ogólnie najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-630">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="2d7b6-631">W `Course` przypadku jednostek użytkownik określa klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-631">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="2d7b6-632">Na przykład numer kursu, taki jak seria 1000 dla działu Math, serii 2000 dla działu angielskiego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-632">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="2d7b6-633">Ten `DatabaseGenerated` atrybut może być również używany do generowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-633">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="2d7b6-634">Na przykład baza danych może automatycznie wygenerować pole daty, aby zarejestrować datę utworzenia lub zaktualizowania wiersza.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-634">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="2d7b6-635">Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-635">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d7b6-636">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-636">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d7b6-637">Właściwości klucza obcego (FK) i właściwości nawigacji w `Course` jednostce odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-637">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="2d7b6-638">Kurs jest przypisywany do jednego działu, więc istnieje `DepartmentID` obcy `Department` i właściwość nawigacji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-638">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="2d7b6-639">Kurs może zawierać dowolną liczbę studentów, więc `Enrollments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-639">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="2d7b6-640">Kurs może być nauczany przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-640">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="2d7b6-641">`CourseAssignment`wyjaśniono [później](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-641">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="2d7b6-642">Tworzenie jednostki działu</span><span class="sxs-lookup"><span data-stu-id="2d7b6-642">Create the Department entity</span></span>

![Jednostka działu](complex-data-model/_static/department-entity.png)

<span data-ttu-id="2d7b6-644">Utwórz *modele/dział. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-644">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="2d7b6-645">Atrybut Column</span><span class="sxs-lookup"><span data-stu-id="2d7b6-645">The Column attribute</span></span>

<span data-ttu-id="2d7b6-646">`Column` Wcześniej atrybut został użyty do zmiany mapowania nazw kolumn.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-646">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="2d7b6-647">W kodzie dla `Department` jednostki `Column` atrybut jest używany do zmiany mapowania typu danych SQL.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-647">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="2d7b6-648">`Budget` Kolumna jest definiowana przy użyciu SQL Server typie pieniędzy w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-648">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="2d7b6-649">Mapowanie kolumn zazwyczaj nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-649">Column mapping is generally not required.</span></span> <span data-ttu-id="2d7b6-650">EF Core zwykle wybiera odpowiedni SQL Server typ danych oparty na typie CLR właściwości.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-650">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="2d7b6-651">Typ CLR `decimal` jest mapowany na typ SQL Server `decimal` .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-651">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="2d7b6-652">`Budget`jest dla waluty, a typ danych walutowych jest bardziej odpowiedni dla waluty.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-652">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d7b6-653">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-653">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d7b6-654">Właściwości FK i nawigacji odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-654">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="2d7b6-655">Dział może lub nie ma uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-655">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="2d7b6-656">Administrator jest zawsze instruktorem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-656">An administrator is always an instructor.</span></span> <span data-ttu-id="2d7b6-657">W związku z tym `Instructor` Właściwośćjestdołączanajakoobcydojednostki.`InstructorID`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-657">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="2d7b6-658">Właściwość nawigacji ma nazwę `Administrator` , ale `Instructor` zawiera jednostkę:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-658">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="2d7b6-659">Znak zapytania (?) w powyższym kodzie określa właściwość dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-659">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="2d7b6-660">Dział może mieć wiele kursów, więc istnieje właściwość nawigacji kursów:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-660">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="2d7b6-661">Uwaga: Zgodnie z Konwencją EF Core włącza kaskadowe usuwanie dla FKs niedopuszczających wartości null i dla relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-661">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="2d7b6-662">Kaskadowe usuwanie może spowodować cykliczne reguły usuwania kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-662">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="2d7b6-663">Cykliczne reguły usuwania kaskadowego powodują wyjątek podczas dodawania migracji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-663">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="2d7b6-664">Na przykład, jeśli `Department.InstructorID` właściwość została zdefiniowana jako niedopuszczający wartości null:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-664">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="2d7b6-665">EF Core konfiguruje regułę usuwania kaskadowego w celu usunięcia działu po usunięciu instruktora.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-665">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="2d7b6-666">Usunięcie działu po usunięciu instruktora nie jest zamierzonym zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-666">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="2d7b6-667">Poniższy interfejs API Fluent ustawi regułę ograniczenia zamiast kaskadowo.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-667">The following fluent API would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="2d7b6-668">Poprzedni kod wyłącza usuwanie kaskadowe dla relacji instruktora działu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-668">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="2d7b6-669">Aktualizowanie jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-669">Update the Enrollment entity</span></span>

<span data-ttu-id="2d7b6-670">Rekord rejestracji jest wykonywany przez jednego ucznia.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-670">An enrollment record is for one course taken by one student.</span></span>

![Jednostka rejestracji](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="2d7b6-672">Aktualizuj *modele/rejestrację. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-672">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d7b6-673">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-673">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d7b6-674">Właściwości klucza obcego i właściwości nawigacji odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-674">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="2d7b6-675">Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje `CourseID` Właściwość FK `Course` i właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-675">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="2d7b6-676">Rekord rejestracji jest przeznaczony dla jednego ucznia, dlatego istnieje `StudentID` właściwość klucza obcego `Student` i właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-676">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="2d7b6-677">Relacje wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="2d7b6-677">Many-to-Many Relationships</span></span>

<span data-ttu-id="2d7b6-678">Istnieje relacja wiele-do-wielu między `Student` obiektami i. `Course`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-678">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="2d7b6-679">Jednostka działa jako tabela sprzężenia wiele-do-wielu *z ładunkiem* w bazie danych. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-679">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="2d7b6-680">"Z ładunkiem" oznacza, że `Enrollment` tabela zawiera dodatkowe dane oprócz FKs dla sprzężonych tabel (w tym przypadku klucz podstawowy i `Grade`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-680">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="2d7b6-681">Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-681">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="2d7b6-682">(Ten diagram został wygenerowany przy użyciu [narzędzi EF PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) dla programu EF 6. x.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-682">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="2d7b6-683">Tworzenie diagramu nie jest częścią samouczka.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-683">Creating the diagram isn't part of the tutorial.)</span></span>

![Student-kurs wiele do wielu relacji](complex-data-model/_static/student-course.png)

<span data-ttu-id="2d7b6-685">Każda linia relacji ma 1 na jednym końcu i gwiazdkę (\*) na drugim, wskazując relację jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-685">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="2d7b6-686">Jeśli tabela nie zawiera informacji o klasie, musi zawierać tylko dwa FKs (`CourseID` i `StudentID`). `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-686">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="2d7b6-687">Tabela sprzężenia wiele-do-wielu bez ładunku jest czasami nazywana tabelą sprzężenia czystego (PJT).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-687">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="2d7b6-688">Jednostki `Instructor` i`Course` mają relację wiele-do-wielu przy użyciu czystej tabeli sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-688">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="2d7b6-689">Uwaga: Dr 6. x obsługuje niejawne tabele sprzężeń dla relacji wiele-do-wielu, ale EF Core nie.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-689">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="2d7b6-690">Aby uzyskać więcej informacji, zobacz [relacje wiele-do-wielu w EF Core 2,0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-690">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="2d7b6-691">Jednostka CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="2d7b6-691">The CourseAssignment entity</span></span>

![Jednostka CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="2d7b6-693">Utwórz *modele/CourseAssignment. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-693">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="2d7b6-694">Instruktorzy do kursów</span><span class="sxs-lookup"><span data-stu-id="2d7b6-694">Instructor-to-Courses</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="2d7b6-696">"Instruktora" — relacja wiele do wielu:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-696">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="2d7b6-697">Wymaga tabeli sprzężenia, która musi być reprezentowana przez zestaw jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-697">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="2d7b6-698">Jest czystym tabelą sprzężenia (tabela bez ładunku).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-698">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="2d7b6-699">Nazwa jednostki `EntityName1EntityName2`sprzężenia jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-699">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="2d7b6-700">Na przykład tabela sprzężenia "instruktor-kurs" używa tego wzorca jest `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-700">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="2d7b6-701">Zalecamy jednak użycie nazwy opisującej relację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-701">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="2d7b6-702">Modele danych rozpoczynają się od siebie i rosną.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-702">Data models start out simple and grow.</span></span> <span data-ttu-id="2d7b6-703">Sprzężenia bez ładunku (PJTs) często są rozwijane w celu uwzględnienia ładunku.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-703">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="2d7b6-704">Rozpoczynając od nazwy obiektu opisowego, nie trzeba zmieniać nazwy po zmianie tabeli sprzężeń.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-704">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="2d7b6-705">Najlepiej, aby jednostka sprzężenia miała własną nazwę naturalną (prawdopodobnie jednowyrazową) w domenie biznesowej.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-705">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="2d7b6-706">Na przykład książki i klienci mogą być połączone z jednostką sprzężenia o nazwie ratings.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-706">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="2d7b6-707">Dla instruktora "wiele do wielu", `CourseAssignment` `CourseInstructor`preferowana jest wartość.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-707">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="2d7b6-708">Klucz złożony</span><span class="sxs-lookup"><span data-stu-id="2d7b6-708">Composite key</span></span>

<span data-ttu-id="2d7b6-709">FKs nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-709">FKs are not nullable.</span></span> <span data-ttu-id="2d7b6-710">Dwa FKs w `CourseAssignment` (`InstructorID` `CourseID` i`CourseAssignment` ) jednoznacznie identyfikują każdy wiersz tabeli.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-710">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="2d7b6-711">`CourseAssignment`nie wymaga dedykowanego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-711">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="2d7b6-712">Właściwości `InstructorID` i`CourseID` działają jako złożony klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-712">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="2d7b6-713">Jedynym sposobem określenia złożonego PKs do EF Core jest *interfejs API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-713">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="2d7b6-714">W następnej sekcji przedstawiono sposób konfigurowania złożonego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-714">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="2d7b6-715">Klucz złożony gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-715">The composite key ensures:</span></span>

* <span data-ttu-id="2d7b6-716">W jednym kursie są dozwolone wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-716">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="2d7b6-717">Dla jednego instruktora są dozwolone wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-717">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="2d7b6-718">Wiele wierszy dla tego samego instruktora i kursu jest niedozwolonych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-718">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="2d7b6-719">Jednostka `Enrollment` Join definiuje własny klucz podstawowy, więc możliwe jest duplikowanie tego sortowania.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-719">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="2d7b6-720">Aby uniknąć takich duplikatów:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-720">To prevent such duplicates:</span></span>

* <span data-ttu-id="2d7b6-721">Dodaj unikatowy indeks do pól klucza obcego lub</span><span class="sxs-lookup"><span data-stu-id="2d7b6-721">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="2d7b6-722">Skonfiguruj `Enrollment` przy użyciu podstawowego klucza złożonego podobnego `CourseAssignment`do.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-722">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="2d7b6-723">Aby uzyskać więcej informacji, zobacz [indeksy](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-723">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="2d7b6-724">Aktualizowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d7b6-724">Update the DB context</span></span>

<span data-ttu-id="2d7b6-725">Dodaj następujący wyróżniony kod do *danych/SchoolContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-725">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="2d7b6-726">Poprzedni kod dodaje nowe jednostki i konfiguruje `CourseAssignment` złożonego klucza podstawowego jednostki.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-726">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="2d7b6-727">Alternatywny interfejs API Fluent dla atrybutów</span><span class="sxs-lookup"><span data-stu-id="2d7b6-727">Fluent API alternative to attributes</span></span>

<span data-ttu-id="2d7b6-728">Metoda w poprzednim kodzie używa *interfejsu API Fluent* , aby skonfigurować zachowanie EF Core. `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-728">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="2d7b6-729">Interfejs API jest nazywany "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod w jednej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-729">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="2d7b6-730">[Poniższy kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) jest przykładem interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-730">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="2d7b6-731">W tym samouczku interfejs API Fluent jest używany tylko w przypadku mapowania bazy danych, której nie można wykonać przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-731">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="2d7b6-732">Jednak interfejs API Fluent może określić większość reguł formatowania, walidacji i mapowania, które mogą być wykonywane przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-732">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="2d7b6-733">Niektórych atrybutów, takich `MinimumLength` jak nie można zastosować w interfejsie API Fluent.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-733">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="2d7b6-734">`MinimumLength`nie zmienia schematu, stosuje tylko regułę walidacji o minimalnej długości.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-734">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="2d7b6-735">Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-735">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="2d7b6-736">Atrybuty i interfejs API Fluent mogą być mieszane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-736">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="2d7b6-737">Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu interfejsu API Fluent (określenie złożonego klucza podstawowego).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-737">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="2d7b6-738">Istnieją pewne konfiguracje, które można wykonać tylko z atrybutami (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-738">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="2d7b6-739">Zalecane rozwiązanie dotyczące korzystania z interfejsu API Fluent lub atrybutów:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-739">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="2d7b6-740">Wybierz jedną z tych dwóch metod.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-740">Choose one of these two approaches.</span></span>
* <span data-ttu-id="2d7b6-741">W miarę możliwości należy stosować wybrane podejście.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-741">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="2d7b6-742">Niektóre z atrybutów używanych w tym samouczku są używane w programie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-742">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="2d7b6-743">Tylko Walidacja (na przykład `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-743">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="2d7b6-744">Tylko konfiguracja EF Core (na przykład `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-744">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="2d7b6-745">Sprawdzanie poprawności i konfiguracja EF Core (na przykład `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-745">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="2d7b6-746">Aby uzyskać więcej informacji na temat atrybutów a interfejs API Fluent, zobacz [metody konfiguracji](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-746">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="2d7b6-747">Diagram jednostek przedstawiający relacje</span><span class="sxs-lookup"><span data-stu-id="2d7b6-747">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="2d7b6-748">Na poniższej ilustracji przedstawiono diagram, który jest tworzony przez narzędzia Dr PowerShell dla gotowego modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-748">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="2d7b6-750">Na powyższym diagramie przedstawiono:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-750">The preceding diagram shows:</span></span>

* <span data-ttu-id="2d7b6-751">Kilka linii relacji jeden-do-wielu (od 1 \*do).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-751">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="2d7b6-752">Linia relacji "jeden do zera" (od 1 do 0.. 1) między `Instructor` jednostkami i. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-752">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="2d7b6-753">Linia relacji zero-lub-jeden-do-wielu (0.. 1 do \*) między `Instructor` obiektami i. `Department`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-753">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="2d7b6-754">Wypełnianie bazy danych danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="2d7b6-754">Seed the DB with Test Data</span></span>

<span data-ttu-id="2d7b6-755">Zaktualizuj kod w *danych/Dbinitializeer. cs*:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-755">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="2d7b6-756">Poprzedni kod zawiera dane inicjatora dla nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-756">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="2d7b6-757">Większość tego kodu tworzy nowe obiekty Entity i ładuje przykładowe dane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-757">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="2d7b6-758">Przykładowe dane służą do testowania.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-758">The sample data is used for testing.</span></span> <span data-ttu-id="2d7b6-759">Zobacz `Enrollments` i `CourseAssignments` , aby zapoznać się z przykładami tabel sprzężenia wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-759">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="2d7b6-760">Dodawanie migracji</span><span class="sxs-lookup"><span data-stu-id="2d7b6-760">Add a migration</span></span>

<span data-ttu-id="2d7b6-761">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-761">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-762">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-762">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-763">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-763">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="2d7b6-764">Poprzednie polecenie wyświetla ostrzeżenie o możliwej utracie danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-764">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="2d7b6-765">`database update` Jeśli polecenie zostanie uruchomione, zostanie utworzony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-765">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="2d7b6-766">Zastosuj migrację</span><span class="sxs-lookup"><span data-stu-id="2d7b6-766">Apply the migration</span></span>

<span data-ttu-id="2d7b6-767">Teraz, gdy masz już istniejącą bazę danych, musisz się zastanowić, jak zastosować w niej przyszłe zmiany.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-767">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="2d7b6-768">Ten samouczek przedstawia dwa podejścia:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-768">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="2d7b6-769">Porzuć i ponownie utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="2d7b6-769">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="2d7b6-770">[Zastosuj migrację do istniejącej bazy danych](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="2d7b6-770">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="2d7b6-771">Chociaż ta metoda jest bardziej złożona i czasochłonna, jest preferowanym podejściem dla rzeczywistych środowisk produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-771">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="2d7b6-772">**Uwaga**: Jest to opcjonalna sekcja samouczka.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-772">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="2d7b6-773">Możesz wykonać kroki porzucenia i utworzyć ponownie i pominąć tę sekcję.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-773">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="2d7b6-774">Jeśli chcesz wykonać kroki opisane w tej sekcji, nie wykonuj kroków usuwania i ponownego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-774">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="2d7b6-775">Porzuć i ponownie utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="2d7b6-775">Drop and re-create the database</span></span>

<span data-ttu-id="2d7b6-776">Kod w zaktualizowanych `DbInitializer` dodaje dane inicjatora dla nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-776">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="2d7b6-777">Aby wymusić EF Core tworzenia nowej bazy danych, Porzuć i zaktualizuj bazę danych:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-777">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d7b6-778">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d7b6-778">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d7b6-779">W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-779">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="2d7b6-780">Uruchom `Get-Help about_EntityFrameworkCore` z PMC, aby uzyskać informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-780">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d7b6-781">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d7b6-781">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2d7b6-782">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-782">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="2d7b6-783">Folder projektu zawiera plik *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-783">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="2d7b6-784">Wprowadź następujące polecenie w oknie polecenia:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-784">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

<span data-ttu-id="2d7b6-785">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-785">Run the app.</span></span> <span data-ttu-id="2d7b6-786">Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-786">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="2d7b6-787">`DbInitializer.Initialize` Wypełnia nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-787">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="2d7b6-788">Otwórz bazę danych w programie SSOX:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-788">Open the DB in SSOX:</span></span>

* <span data-ttu-id="2d7b6-789">Jeśli SSOX został wcześniej otwarty, kliknij przycisk **Odśwież** .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-789">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="2d7b6-790">Rozwiń **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-790">Expand the **Tables** node.</span></span> <span data-ttu-id="2d7b6-791">Zostaną wyświetlone utworzone tabele.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-791">The created tables are displayed.</span></span>

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="2d7b6-793">Zapoznaj się z tabelą **CourseAssignment** :</span><span class="sxs-lookup"><span data-stu-id="2d7b6-793">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="2d7b6-794">Kliknij prawym przyciskiem myszy tabelę **CourseAssignment** , a następnie wybierz polecenie **Wyświetl dane**.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-794">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="2d7b6-795">Sprawdź, czy tabela **CourseAssignment** zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-795">Verify the **CourseAssignment** table contains data.</span></span>

![CourseAssignment dane w SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="2d7b6-797">Zastosowanie migracji do istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d7b6-797">Apply the migration to the existing database</span></span>

<span data-ttu-id="2d7b6-798">Ta sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-798">This section is optional.</span></span> <span data-ttu-id="2d7b6-799">Kroki te działają tylko wtedy, gdy pominięto poprzednią [listę i ponownie utworzysz sekcję bazy danych](#drop) .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-799">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="2d7b6-800">Po uruchomieniu migracji z istniejącymi danymi mogą istnieć ograniczenia klucza obcego, które nie są spełnione w przypadku istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-800">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="2d7b6-801">W przypadku danych produkcyjnych należy wykonać kroki, aby przeprowadzić migrację istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-801">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="2d7b6-802">Ta sekcja zawiera przykład rozwiązywania naruszeń ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-802">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="2d7b6-803">Nie należy wprowadzać tych zmian w kodzie bez tworzenia kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-803">Don't make these code changes without a backup.</span></span> <span data-ttu-id="2d7b6-804">Nie wprowadzaj tych zmian w kodzie, jeśli wykonano poprzednią sekcję i Zaktualizowano bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-804">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="2d7b6-805">Plik *{timestamp} _ComplexDataModel. cs* zawiera następujący kod:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-805">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="2d7b6-806">Poprzedzający kod dodaje `DepartmentID` `Course` do tabeli obcy niedopuszczający wartości null.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-806">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="2d7b6-807">Baza danych z poprzedniego samouczka zawiera wiersze w `Course`, dlatego nie można zaktualizować tabeli za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-807">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="2d7b6-808">Aby `ComplexDataModel` migracja była współdziałać z istniejącymi danymi:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-808">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="2d7b6-809">Zmień kod, aby nadać nowej kolumnie (`DepartmentID`) wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-809">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="2d7b6-810">Utwórz fałszywy Departament o nazwie "Temp", który ma pełnić rolę działu domyślnego.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-810">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="2d7b6-811">Popraw ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="2d7b6-811">Fix the foreign key constraints</span></span>

<span data-ttu-id="2d7b6-812">Zaktualizuj metodę `Up`klasy: `ComplexDataModel`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-812">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="2d7b6-813">Otwórz plik *{timestamp} _ComplexDataModel. cs* .</span><span class="sxs-lookup"><span data-stu-id="2d7b6-813">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="2d7b6-814">Dodaj komentarz `DepartmentID` `Course` do wiersza kodu, który dodaje kolumnę do tabeli.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-814">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="2d7b6-815">Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-815">Add the following highlighted code.</span></span> <span data-ttu-id="2d7b6-816">Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-816">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="2d7b6-817">Po wykonaniu powyższych zmian istniejące `Course` wiersze będą powiązane z działem "Temp" po uruchomieniumetody.`Up` `ComplexDataModel`</span><span class="sxs-lookup"><span data-stu-id="2d7b6-817">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="2d7b6-818">Aplikacja produkcyjna:</span><span class="sxs-lookup"><span data-stu-id="2d7b6-818">A production app would:</span></span>

* <span data-ttu-id="2d7b6-819">Dołącz kod lub skrypty, aby `Department` dodać wiersze i `Course` powiązane wiersze do nowych `Department` wierszy.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-819">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="2d7b6-820">Nie używaj działu "Temp" ani wartości domyślnej dla `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-820">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="2d7b6-821">Następny samouczek obejmuje powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="2d7b6-821">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2d7b6-822">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2d7b6-822">Additional resources</span></span>

* [<span data-ttu-id="2d7b6-823">Wersja usługi YouTube w tym samouczku (część 1)</span><span class="sxs-lookup"><span data-stu-id="2d7b6-823">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="2d7b6-824">Wersja usługi YouTube w tym samouczku (część 2)</span><span class="sxs-lookup"><span data-stu-id="2d7b6-824">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)



> [!div class="step-by-step"]
> <span data-ttu-id="2d7b6-825">[Poprzedni](xref:data/ef-rp/migrations)Następny
> [](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="2d7b6-825">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end
