---
title: Razor Pages EF Core w ASP.NET Core — model danych-5 z 8
author: tdykstra
description: W tym samouczku Dodaj więcej jednostek i relacje i Dostosuj model danych, określając formatowanie, walidację i reguły mapowania.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 8a1c0759453b02f4ce1c45471a8f93da626f8261
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583289"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="43879-103">Razor Pages EF Core w ASP.NET Core — model danych-5 z 8</span><span class="sxs-lookup"><span data-stu-id="43879-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="43879-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="43879-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43879-105">Poprzednie samouczki pracowały z podstawowym modelem danych, który składa się z trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="43879-106">W tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="43879-106">In this tutorial:</span></span>

* <span data-ttu-id="43879-107">Dodano więcej jednostek i relacji.</span><span class="sxs-lookup"><span data-stu-id="43879-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="43879-108">Model danych jest dostosowywany przez określenie formatowania, walidacji i reguł mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43879-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="43879-109">Ukończony model danych przedstawiono na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="43879-109">The completed data model is shown in the following illustration:</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="43879-111">Jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="43879-111">The Student entity</span></span>

![Jednostka ucznia](complex-data-model/_static/student-entity.png)

<span data-ttu-id="43879-113">Zastąp kod w *modelach/student. cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="43879-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="43879-114">Poprzedni kod dodaje `FullName` Właściwość i dodaje następujące atrybuty do istniejących właściwości:</span><span class="sxs-lookup"><span data-stu-id="43879-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="43879-115">Właściwość obliczeniowa FullName</span><span class="sxs-lookup"><span data-stu-id="43879-115">The FullName calculated property</span></span>

<span data-ttu-id="43879-116">`FullName`jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="43879-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="43879-117">`FullName`nie można ustawić, dlatego ma tylko metodę dostępu get.</span><span class="sxs-lookup"><span data-stu-id="43879-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="43879-118">Nie `FullName` utworzono żadnej kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="43879-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="43879-119">Atrybut DataType</span><span class="sxs-lookup"><span data-stu-id="43879-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="43879-120">W przypadku dat rejestracji uczniów wszystkie strony wyświetlają teraz godzinę i datę, chociaż tylko data jest ważna.</span><span class="sxs-lookup"><span data-stu-id="43879-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="43879-121">Używając atrybutów adnotacji danych, można wprowadzić jedną zmianę kodu, która naprawi format wyświetlania na każdej stronie, która wyświetla dane.</span><span class="sxs-lookup"><span data-stu-id="43879-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="43879-122">Atrybut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) określa typ danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43879-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="43879-123">W tym przypadku należy wyświetlić tylko datę, a nie datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="43879-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="43879-124">[Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress itp. Ten `DataType` atrybut może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu.</span><span class="sxs-lookup"><span data-stu-id="43879-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="43879-125">Przykład:</span><span class="sxs-lookup"><span data-stu-id="43879-125">For example:</span></span>

* <span data-ttu-id="43879-126">Łącze jest tworzone automatycznie dla `DataType.EmailAddress`. `mailto:`</span><span class="sxs-lookup"><span data-stu-id="43879-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="43879-127">Selektor daty jest dostępny `DataType.Date` w większości przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="43879-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="43879-128">Ten `DataType` atrybut emituje atrybuty HTML `data-` 5 (wymawiane kreski danych).</span><span class="sxs-lookup"><span data-stu-id="43879-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="43879-129">`DataType` Atrybuty nie zapewniają walidacji.</span><span class="sxs-lookup"><span data-stu-id="43879-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="43879-130">Atrybut DisplayFormat</span><span class="sxs-lookup"><span data-stu-id="43879-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="43879-131">`DataType.Date`nie określa formatu wyświetlanej daty.</span><span class="sxs-lookup"><span data-stu-id="43879-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="43879-132">Domyślnie pole Date jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)serwera.</span><span class="sxs-lookup"><span data-stu-id="43879-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="43879-133">Ten `DisplayFormat` atrybut służy do jawnego określenia formatu daty.</span><span class="sxs-lookup"><span data-stu-id="43879-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="43879-134">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie ma być również stosowane do interfejsu użytkownika edytowania.</span><span class="sxs-lookup"><span data-stu-id="43879-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="43879-135">Niektóre pola nie powinny `ApplyFormatInEditMode`być używane.</span><span class="sxs-lookup"><span data-stu-id="43879-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="43879-136">Na przykład symbol waluty zazwyczaj nie powinien być wyświetlany w polu tekstowym Edycja.</span><span class="sxs-lookup"><span data-stu-id="43879-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="43879-137">Ten `DisplayFormat` atrybut może być używany przez siebie.</span><span class="sxs-lookup"><span data-stu-id="43879-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="43879-138">Zwykle dobrym pomysłem jest użycie `DataType` atrybutu `DisplayFormat` z atrybutem.</span><span class="sxs-lookup"><span data-stu-id="43879-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="43879-139">Ten `DataType` atrybut przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie.</span><span class="sxs-lookup"><span data-stu-id="43879-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="43879-140">Ten `DataType` atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`programie:</span><span class="sxs-lookup"><span data-stu-id="43879-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="43879-141">Przeglądarka może włączyć funkcje HTML5.</span><span class="sxs-lookup"><span data-stu-id="43879-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="43879-142">Na przykład Pokaż kontrolkę kalendarz, odpowiedni dla ustawień regionalnych symbol waluty, linki poczty e-mail i sprawdzanie poprawności danych po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="43879-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="43879-143">Domyślnie przeglądarka renderuje dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.</span><span class="sxs-lookup"><span data-stu-id="43879-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="43879-144">Aby uzyskać więcej informacji, zobacz [ \<dokumentację pomocnika tagów > danych wejściowych](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="43879-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="43879-145">Atrybut StringLength</span><span class="sxs-lookup"><span data-stu-id="43879-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="43879-146">Można określić reguły walidacji danych i komunikaty o błędach walidacji z atrybutami.</span><span class="sxs-lookup"><span data-stu-id="43879-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="43879-147">Atrybut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych.</span><span class="sxs-lookup"><span data-stu-id="43879-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="43879-148">Kod pokazuje limity nazw do nie więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="43879-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="43879-149">Przykład określający, że minimalna długość ciągu jest pokazywana w [dalszej](#the-required-attribute)części.</span><span class="sxs-lookup"><span data-stu-id="43879-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="43879-150">Ten `StringLength` atrybut zapewnia również weryfikację po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="43879-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="43879-151">Wartość minimalna nie ma wpływu na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43879-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="43879-152">Ten `StringLength` atrybut nie uniemożliwia użytkownikowi wprowadzania białych znaków w nazwie.</span><span class="sxs-lookup"><span data-stu-id="43879-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="43879-153">Atrybut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) może służyć do stosowania ograniczeń do danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="43879-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="43879-154">Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:</span><span class="sxs-lookup"><span data-stu-id="43879-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43879-156">W **Eksplorator obiektów SQL Server** (SSOX) Otwórz projektanta tabeli uczniów, klikając dwukrotnie tabelę **uczniów** .</span><span class="sxs-lookup"><span data-stu-id="43879-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela studentów w SSOX przed migracją](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="43879-158">Na powyższym obrazie przedstawiono schemat `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="43879-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="43879-159">Pola nazw mają typ `nvarchar(MAX)`.</span><span class="sxs-lookup"><span data-stu-id="43879-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="43879-160">Gdy migracja zostanie utworzona i zastosowana w dalszej części tego samouczka, nazwy `nvarchar(50)` pól staną się wynikiem atrybutów długości ciągu.</span><span class="sxs-lookup"><span data-stu-id="43879-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="43879-162">W narzędziu SQLite Sprawdź definicje `Student` kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="43879-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="43879-163">Pola nazw mają typ `Text`.</span><span class="sxs-lookup"><span data-stu-id="43879-163">The name fields have type `Text`.</span></span> <span data-ttu-id="43879-164">Zwróć uwagę, że jest wywoływana `FirstMidName`nazwa pola imię.</span><span class="sxs-lookup"><span data-stu-id="43879-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="43879-165">W następnej sekcji zmienisz nazwę tej kolumny na `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="43879-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="43879-166">Atrybut Column</span><span class="sxs-lookup"><span data-stu-id="43879-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="43879-167">Atrybuty mogą kontrolować sposób mapowania klas i właściwości do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43879-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="43879-168">`Student` W modelu `Column` atrybut jest używany do `FirstMidName` mapowania nazwy właściwości na "FirstName" w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="43879-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="43879-169">Po utworzeniu bazy danych nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, `Column` gdy atrybut jest używany).</span><span class="sxs-lookup"><span data-stu-id="43879-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="43879-170">`Student` Model używa`FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową.</span><span class="sxs-lookup"><span data-stu-id="43879-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="43879-171">Atrybut w modelu danych jest mapowany`Student` do kolumnytabeli.`FirstName` `Student.FirstMidName` `[Column]`</span><span class="sxs-lookup"><span data-stu-id="43879-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="43879-172">Dodanie `Column` atrybutu zmienia model z `SchoolContext`kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="43879-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="43879-173">Model, który wykonuje `SchoolContext` kopię zapasową, nie jest już zgodny z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="43879-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="43879-174">Niezgodność zostanie rozwiązany przez dodanie migracji w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="43879-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="43879-175">Wymagany atrybut</span><span class="sxs-lookup"><span data-stu-id="43879-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="43879-176">`Required` Atrybut powoduje, że właściwości nazwy są wymagane.</span><span class="sxs-lookup"><span data-stu-id="43879-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="43879-177">Ten `Required` atrybut nie jest wymagany dla typów niedopuszczających wartości null, takich jak typy wartości `DateTime`( `int`na przykład `double`,, i).</span><span class="sxs-lookup"><span data-stu-id="43879-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="43879-178">Typy, które nie mogą mieć wartości null, są automatycznie traktowane jako pola wymagane.</span><span class="sxs-lookup"><span data-stu-id="43879-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="43879-179">Ten `Required` atrybut może być zastąpiony parametrem o minimalnej długości `StringLength` w atrybucie:</span><span class="sxs-lookup"><span data-stu-id="43879-179">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="43879-180">Atrybut wyświetlania</span><span class="sxs-lookup"><span data-stu-id="43879-180">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="43879-181">Ten `Display` atrybut określa, że podpis pól tekstowych powinien mieć wartość "imię i nazwisko", "nazwisko", "imię i nazwisko" oraz "Data rejestracji".</span><span class="sxs-lookup"><span data-stu-id="43879-181">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="43879-182">Domyślne podpisy nie zawierają spacji dzielących wyrazy, na przykład "LastName".</span><span class="sxs-lookup"><span data-stu-id="43879-182">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="43879-183">Tworzenie migracji</span><span class="sxs-lookup"><span data-stu-id="43879-183">Create a migration</span></span>

<span data-ttu-id="43879-184">Uruchom aplikację i przejdź do strony uczniów.</span><span class="sxs-lookup"><span data-stu-id="43879-184">Run the app and go to the Students page.</span></span> <span data-ttu-id="43879-185">Zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="43879-185">An exception is thrown.</span></span> <span data-ttu-id="43879-186">Ten `[Column]` atrybut powoduje, że Dr powinien znaleźć kolumnę o nazwie `FirstName`, ale nazwa kolumny w bazie danych jest nadal `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="43879-186">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-187">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43879-188">Komunikat o błędzie jest podobny do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="43879-188">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="43879-189">W obszarze PMC wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="43879-189">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="43879-190">Pierwsze z tych poleceń generuje następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="43879-190">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="43879-191">Ostrzeżenie jest generowane, ponieważ pola nazw są teraz ograniczone do 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="43879-191">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="43879-192">Jeśli nazwa w bazie danych ma więcej niż 50 znaków, zostanie utracony od 51 do ostatniego znaku.</span><span class="sxs-lookup"><span data-stu-id="43879-192">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="43879-193">Otwórz tabelę uczniów w SSOX:</span><span class="sxs-lookup"><span data-stu-id="43879-193">Open the Student table in SSOX:</span></span>

  ![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="43879-195">Przed zainstalowaniem migracji kolumny nazw były typu [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="43879-195">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="43879-196">Kolumny nazw są teraz `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="43879-196">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="43879-197">Nazwa kolumny została zmieniona z `FirstMidName` na. `FirstName`</span><span class="sxs-lookup"><span data-stu-id="43879-197">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="43879-199">Komunikat o błędzie jest podobny do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="43879-199">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="43879-200">Otwórz okno polecenia w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="43879-200">Open a command window in the project folder.</span></span> <span data-ttu-id="43879-201">Wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="43879-201">Enter the following commands to create a new migration and update the database:</span></span>

  ```console
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="43879-202">Polecenie aktualizacji bazy danych wyświetla błąd podobny do następującego przykładu:</span><span class="sxs-lookup"><span data-stu-id="43879-202">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="43879-203">W tym samouczku sposób, w jaki można to zrobić, jest usunięcie i ponowne utworzenie początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="43879-203">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="43879-204">Aby uzyskać więcej informacji, zobacz uwagi dotyczące ostrzeżeń oprogramowania SQLite w górnej części [samouczka migracji](xref:data/ef-rp/migrations).</span><span class="sxs-lookup"><span data-stu-id="43879-204">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="43879-205">Usuń folder *migracji* .</span><span class="sxs-lookup"><span data-stu-id="43879-205">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="43879-206">Uruchom następujące polecenia, aby usunąć bazę danych, utworzyć nową migrację początkową i zastosować migrację:</span><span class="sxs-lookup"><span data-stu-id="43879-206">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```console
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="43879-207">Sprawdź tabelę uczniów w narzędziu SQLite.</span><span class="sxs-lookup"><span data-stu-id="43879-207">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="43879-208">Kolumna, która była FirstMidName, jest teraz FirstName.</span><span class="sxs-lookup"><span data-stu-id="43879-208">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="43879-209">Uruchom aplikację i przejdź do strony uczniów.</span><span class="sxs-lookup"><span data-stu-id="43879-209">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="43879-210">Należy zauważyć, że czasy nie są wprowadzane ani wyświetlane wraz z datami.</span><span class="sxs-lookup"><span data-stu-id="43879-210">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="43879-211">Wybierz pozycję **Utwórz nową**, a następnie spróbuj wprowadzić nazwę dłuższą niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="43879-211">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="43879-212">W poniższych sekcjach Kompilowanie aplikacji na niektórych etapach powoduje wygenerowanie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="43879-212">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="43879-213">Instrukcje określają, kiedy należy skompilować aplikację.</span><span class="sxs-lookup"><span data-stu-id="43879-213">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="43879-214">Jednostka instruktora</span><span class="sxs-lookup"><span data-stu-id="43879-214">The Instructor Entity</span></span>

![Jednostka instruktora](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="43879-216">Utwórz *modele/instruktor. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-216">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="43879-217">Wiele atrybutów może znajdować się w jednym wierszu.</span><span class="sxs-lookup"><span data-stu-id="43879-217">Multiple attributes can be on one line.</span></span> <span data-ttu-id="43879-218">`HireDate` Atrybuty mogą być zapisywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="43879-218">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="43879-219">Właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="43879-219">Navigation properties</span></span>

<span data-ttu-id="43879-220">Właściwości `CourseAssignments` i`OfficeAssignment` są właściwościami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="43879-220">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="43879-221">Instruktor może nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="43879-221">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="43879-222">Instruktor może mieć co najwyżej jednego biura, więc `OfficeAssignment` Właściwość zawiera jedną `OfficeAssignment` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="43879-222">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="43879-223">`OfficeAssignment`ma wartość null, jeśli nie przypisano pakietu Office.</span><span class="sxs-lookup"><span data-stu-id="43879-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="43879-224">Jednostka OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="43879-224">The OfficeAssignment entity</span></span>

![Jednostka OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="43879-226">Utwórz *modele/OfficeAssignment. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="43879-227">Atrybut klucza</span><span class="sxs-lookup"><span data-stu-id="43879-227">The Key attribute</span></span>

<span data-ttu-id="43879-228">Ten `[Key]` atrybut służy do identyfikowania właściwości jako klucza podstawowego (PK), gdy nazwa właściwości ma wartość inną niż classnameID lub ID.</span><span class="sxs-lookup"><span data-stu-id="43879-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="43879-229">Istnieje relacja jeden do zera między `Instructor` jednostkami i. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="43879-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="43879-230">Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane.</span><span class="sxs-lookup"><span data-stu-id="43879-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="43879-231">Klucz podstawowy jest również jego kluczem obcym (obcy) `Instructor` do jednostki. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="43879-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="43879-232">EF Core nie może automatycznie `InstructorID` rozpoznać jako `OfficeAssignment` klucza podstawowego, `InstructorID` ponieważ nie jest zgodna z konwencją nazewnictwa ID lub classnameID.</span><span class="sxs-lookup"><span data-stu-id="43879-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="43879-233">W związku z tym `InstructorID` atrybutjestużywanydoidentyfikacjijakokluczpodstawowy:`Key`</span><span class="sxs-lookup"><span data-stu-id="43879-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="43879-234">Domyślnie EF Core traktuje klucz jako wygenerowane poza bazą danych, ponieważ kolumna służy do identyfikowania relacji.</span><span class="sxs-lookup"><span data-stu-id="43879-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="43879-235">Właściwość nawigacji instruktora</span><span class="sxs-lookup"><span data-stu-id="43879-235">The Instructor navigation property</span></span>

<span data-ttu-id="43879-236">Właściwość `Instructor.OfficeAssignment` nawigacji może mieć wartość null, ponieważ nie może `OfficeAssignment` istnieć wiersz danego instruktora.</span><span class="sxs-lookup"><span data-stu-id="43879-236">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="43879-237">Instruktor może nie mieć przypisania pakietu Office.</span><span class="sxs-lookup"><span data-stu-id="43879-237">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="43879-238">Właściwość nawigacji zawsze będzie mieć jednostkę instruktora, ponieważ typ klucza `InstructorID` obcego to `int`, typ wartości niedopuszczający wartości null. `OfficeAssignment.Instructor`</span><span class="sxs-lookup"><span data-stu-id="43879-238">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="43879-239">Przypisanie pakietu Office nie może istnieć bez instruktora.</span><span class="sxs-lookup"><span data-stu-id="43879-239">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="43879-240">Gdy jednostka ma powiązaną `OfficeAssignment` jednostkę, każda jednostka ma odwołanie do drugiej z nich we właściwości nawigacji. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="43879-240">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="43879-241">Jednostka kursu</span><span class="sxs-lookup"><span data-stu-id="43879-241">The Course Entity</span></span>

![Jednostka kursu](complex-data-model/_static/course-entity.png)

<span data-ttu-id="43879-243">Aktualizuj *modele/kurs. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-243">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="43879-244">Jednostka ma właściwość `DepartmentID`klucza obcego (obcy). `Course`</span><span class="sxs-lookup"><span data-stu-id="43879-244">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="43879-245">`DepartmentID`wskazuje powiązaną `Department` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="43879-245">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="43879-246">`Course` Jednostka ma właściwość nawigacji. `Department`</span><span class="sxs-lookup"><span data-stu-id="43879-246">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="43879-247">EF Core nie wymaga właściwości klucza obcego dla modelu danych, gdy model ma właściwość nawigacji dla powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="43879-247">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="43879-248">EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie są one zbędne.</span><span class="sxs-lookup"><span data-stu-id="43879-248">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="43879-249">EF Core tworzy [Właściwości cienia](/ef/core/modeling/shadow-properties) dla automatycznie utworzonych FKs.</span><span class="sxs-lookup"><span data-stu-id="43879-249">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="43879-250">Jednak jawne dołączenie klucza obcego w modelu danych może spowodować uproszczenie i wydajniejsze aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="43879-250">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="43879-251">Rozważmy na przykład model, w którym Właściwość `DepartmentID` FK *nie* jest uwzględniona.</span><span class="sxs-lookup"><span data-stu-id="43879-251">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="43879-252">Gdy jednostka kursu jest pobierana do edycji:</span><span class="sxs-lookup"><span data-stu-id="43879-252">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="43879-253">Właściwość `Department` ma wartość null, jeśli nie jest jawnie załadowana.</span><span class="sxs-lookup"><span data-stu-id="43879-253">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="43879-254">Aby zaktualizować jednostkę kursu, `Department` należy najpierw pobrać jednostkę.</span><span class="sxs-lookup"><span data-stu-id="43879-254">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="43879-255">Gdy właściwość `DepartmentID` FK jest uwzględniona w modelu danych, nie trzeba `Department` pobierać jednostki przed aktualizacją.</span><span class="sxs-lookup"><span data-stu-id="43879-255">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="43879-256">Atrybut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="43879-256">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="43879-257">Ten `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atrybut określa, że klucz podstawowy jest dostarczany przez aplikację, a nie generowany przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43879-257">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="43879-258">Domyślnie EF Core zakłada, że wartości klucza PK są generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43879-258">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="43879-259">Wygenerowana baza danych jest ogólnie najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="43879-259">Database-generated is generally the best approach.</span></span> <span data-ttu-id="43879-260">W `Course` przypadku jednostek użytkownik określa klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="43879-260">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="43879-261">Na przykład numer kursu, taki jak seria 1000 dla działu Math, serii 2000 dla działu angielskiego.</span><span class="sxs-lookup"><span data-stu-id="43879-261">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="43879-262">Ten `DatabaseGenerated` atrybut może być również używany do generowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="43879-262">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="43879-263">Na przykład baza danych może automatycznie wygenerować pole daty, aby zarejestrować datę utworzenia lub zaktualizowania wiersza.</span><span class="sxs-lookup"><span data-stu-id="43879-263">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="43879-264">Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="43879-264">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="43879-265">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="43879-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="43879-266">Właściwości klucza obcego (FK) i właściwości nawigacji w `Course` jednostce odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="43879-266">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="43879-267">Kurs jest przypisywany do jednego działu, więc istnieje `DepartmentID` obcy `Department` i właściwość nawigacji.</span><span class="sxs-lookup"><span data-stu-id="43879-267">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="43879-268">Kurs może zawierać dowolną liczbę studentów, więc `Enrollments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="43879-268">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="43879-269">Kurs może być nauczany przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="43879-269">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="43879-270">`CourseAssignment`wyjaśniono [później](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="43879-270">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="43879-271">Jednostka działu</span><span class="sxs-lookup"><span data-stu-id="43879-271">The Department entity</span></span>

![Jednostka działu](complex-data-model/_static/department-entity.png)

<span data-ttu-id="43879-273">Utwórz *modele/dział. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-273">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="43879-274">Atrybut Column</span><span class="sxs-lookup"><span data-stu-id="43879-274">The Column attribute</span></span>

<span data-ttu-id="43879-275">`Column` Wcześniej atrybut został użyty do zmiany mapowania nazw kolumn.</span><span class="sxs-lookup"><span data-stu-id="43879-275">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="43879-276">W kodzie dla `Department` jednostki `Column` atrybut jest używany do zmiany mapowania typu danych SQL.</span><span class="sxs-lookup"><span data-stu-id="43879-276">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="43879-277">`Budget` Kolumna jest definiowana przy użyciu SQL Server typie pieniędzy w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="43879-277">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="43879-278">Mapowanie kolumn zazwyczaj nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="43879-278">Column mapping is generally not required.</span></span> <span data-ttu-id="43879-279">EF Core wybiera odpowiedni SQL Server typ danych na podstawie typu CLR dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="43879-279">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="43879-280">Typ CLR `decimal` jest mapowany na typ SQL Server `decimal` .</span><span class="sxs-lookup"><span data-stu-id="43879-280">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="43879-281">`Budget`jest dla waluty, a typ danych walutowych jest bardziej odpowiedni dla waluty.</span><span class="sxs-lookup"><span data-stu-id="43879-281">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="43879-282">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="43879-282">Foreign key and navigation properties</span></span>

<span data-ttu-id="43879-283">Właściwości FK i nawigacji odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="43879-283">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="43879-284">Dział może lub nie ma uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="43879-284">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="43879-285">Administrator jest zawsze instruktorem.</span><span class="sxs-lookup"><span data-stu-id="43879-285">An administrator is always an instructor.</span></span> <span data-ttu-id="43879-286">W związku z tym `Instructor` Właściwośćjestdołączanajakoobcydojednostki.`InstructorID`</span><span class="sxs-lookup"><span data-stu-id="43879-286">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="43879-287">Właściwość nawigacji ma nazwę `Administrator` , ale `Instructor` zawiera jednostkę:</span><span class="sxs-lookup"><span data-stu-id="43879-287">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="43879-288">Znak zapytania (?) w powyższym kodzie określa właściwość dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="43879-288">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="43879-289">Dział może mieć wiele kursów, więc istnieje właściwość nawigacji kursów:</span><span class="sxs-lookup"><span data-stu-id="43879-289">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="43879-290">Zgodnie z Konwencją EF Core włącza kaskadowe usuwanie dla FKs niedopuszczających wartości null i dla relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="43879-290">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="43879-291">To domyślne zachowanie może spowodować cykliczne reguły usuwania kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="43879-291">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="43879-292">Cykliczne reguły usuwania kaskadowego powodują wyjątek podczas dodawania migracji.</span><span class="sxs-lookup"><span data-stu-id="43879-292">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="43879-293">Na przykład jeśli `Department.InstructorID` właściwość została zdefiniowana jako niedopuszczający wartości null, EF Core skonfigurować regułę usuwania kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="43879-293">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="43879-294">W takim przypadku dział zostanie usunięty po usunięciu instruktora przypisanego jako administrator.</span><span class="sxs-lookup"><span data-stu-id="43879-294">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="43879-295">W tym scenariuszu reguła ograniczania byłaby bardziej zrozumiała.</span><span class="sxs-lookup"><span data-stu-id="43879-295">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="43879-296">Poniższy interfejs API Fluent ustawi regułę ograniczenia i wyłącza usuwanie kaskadowe.</span><span class="sxs-lookup"><span data-stu-id="43879-296">The following fluent API would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="43879-297">Jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="43879-297">The Enrollment entity</span></span>

<span data-ttu-id="43879-298">Rekord rejestracji jest wykonywany przez jednego ucznia.</span><span class="sxs-lookup"><span data-stu-id="43879-298">An enrollment record is for one course taken by one student.</span></span>

![Jednostka rejestracji](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="43879-300">Aktualizuj *modele/rejestrację. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-300">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="43879-301">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="43879-301">Foreign key and navigation properties</span></span>

<span data-ttu-id="43879-302">Właściwości klucza obcego i właściwości nawigacji odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="43879-302">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="43879-303">Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje `CourseID` Właściwość FK `Course` i właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="43879-303">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="43879-304">Rekord rejestracji jest przeznaczony dla jednego ucznia, dlatego istnieje `StudentID` właściwość klucza obcego `Student` i właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="43879-304">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="43879-305">Relacje wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="43879-305">Many-to-Many Relationships</span></span>

<span data-ttu-id="43879-306">Istnieje relacja wiele-do-wielu między `Student` obiektami i. `Course`</span><span class="sxs-lookup"><span data-stu-id="43879-306">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="43879-307">Jednostka działa jako tabela sprzężenia wiele-do-wielu *z ładunkiem* w bazie danych. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="43879-307">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="43879-308">"Z ładunkiem" oznacza, że `Enrollment` tabela zawiera dodatkowe dane oprócz FKs dla sprzężonych tabel (w tym przypadku klucz podstawowy i `Grade`).</span><span class="sxs-lookup"><span data-stu-id="43879-308">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="43879-309">Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-309">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="43879-310">(Ten diagram został wygenerowany przy użyciu [narzędzi EF PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) dla programu EF 6. x.</span><span class="sxs-lookup"><span data-stu-id="43879-310">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="43879-311">Tworzenie diagramu nie jest częścią samouczka.</span><span class="sxs-lookup"><span data-stu-id="43879-311">Creating the diagram isn't part of the tutorial.)</span></span>

![Student-kurs wiele do wielu relacji](complex-data-model/_static/student-course.png)

<span data-ttu-id="43879-313">Każda linia relacji ma 1 na jednym końcu i gwiazdkę (\*) na drugim, wskazując relację jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="43879-313">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="43879-314">Jeśli tabela nie zawiera informacji o klasie, musi zawierać tylko dwa FKs (`CourseID` i `StudentID`). `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="43879-314">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="43879-315">Tabela sprzężenia wiele-do-wielu bez ładunku jest czasami nazywana tabelą sprzężenia czystego (PJT).</span><span class="sxs-lookup"><span data-stu-id="43879-315">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="43879-316">Jednostki `Instructor` i`Course` mają relację wiele-do-wielu przy użyciu czystej tabeli sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="43879-316">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="43879-317">Uwaga: Dr 6. x obsługuje niejawne tabele sprzężeń dla relacji wiele-do-wielu, ale EF Core nie.</span><span class="sxs-lookup"><span data-stu-id="43879-317">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="43879-318">Aby uzyskać więcej informacji, zobacz [relacje wiele-do-wielu w EF Core 2,0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="43879-318">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="43879-319">Jednostka CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="43879-319">The CourseAssignment entity</span></span>

![Jednostka CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="43879-321">Utwórz *modele/CourseAssignment. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-321">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="43879-322">Relacja "instruktora" do wielu kursów wymaga tabeli sprzężenia, a jednostka dla tej tabeli sprzężenia jest CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="43879-322">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="43879-324">Nazwa jednostki `EntityName1EntityName2`sprzężenia jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="43879-324">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="43879-325">Na przykład tabela sprzężenia "instruktora do kursu" używa tego wzorca `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="43879-325">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="43879-326">Zalecamy jednak użycie nazwy opisującej relację.</span><span class="sxs-lookup"><span data-stu-id="43879-326">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="43879-327">Modele danych rozpoczynają się od siebie i rosną.</span><span class="sxs-lookup"><span data-stu-id="43879-327">Data models start out simple and grow.</span></span> <span data-ttu-id="43879-328">Tabele sprzężenia bez ładunku (PJTs) często są rozwijane w celu uwzględnienia ładunku.</span><span class="sxs-lookup"><span data-stu-id="43879-328">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="43879-329">Rozpoczynając od nazwy obiektu opisowego, nie trzeba zmieniać nazwy po zmianie tabeli sprzężeń.</span><span class="sxs-lookup"><span data-stu-id="43879-329">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="43879-330">Najlepiej, aby jednostka sprzężenia miała własną nazwę naturalną (prawdopodobnie jednowyrazową) w domenie biznesowej.</span><span class="sxs-lookup"><span data-stu-id="43879-330">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="43879-331">Na przykład książki i klienci mogą być połączone z jednostką sprzężenia o nazwie ratings.</span><span class="sxs-lookup"><span data-stu-id="43879-331">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="43879-332">Dla instruktora "wiele do wielu", `CourseAssignment` `CourseInstructor`preferowana jest wartość.</span><span class="sxs-lookup"><span data-stu-id="43879-332">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="43879-333">Klucz złożony</span><span class="sxs-lookup"><span data-stu-id="43879-333">Composite key</span></span>

<span data-ttu-id="43879-334">Dwa FKs w `CourseAssignment` (`InstructorID` `CourseID` i`CourseAssignment` ) jednoznacznie identyfikują każdy wiersz tabeli.</span><span class="sxs-lookup"><span data-stu-id="43879-334">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="43879-335">`CourseAssignment`nie wymaga dedykowanego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="43879-335">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="43879-336">Właściwości `InstructorID` i`CourseID` działają jako złożony klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="43879-336">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="43879-337">Jedynym sposobem określenia złożonego PKs do EF Core jest *interfejs API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="43879-337">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="43879-338">W następnej sekcji przedstawiono sposób konfigurowania złożonego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="43879-338">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="43879-339">Klucz złożony gwarantuje, że:</span><span class="sxs-lookup"><span data-stu-id="43879-339">The composite key ensures that:</span></span>

* <span data-ttu-id="43879-340">W jednym kursie są dozwolone wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="43879-340">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="43879-341">Dla jednego instruktora są dozwolone wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="43879-341">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="43879-342">Wiele wierszy nie jest dozwolonych dla tego samego instruktora i kursu.</span><span class="sxs-lookup"><span data-stu-id="43879-342">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="43879-343">Jednostka `Enrollment` Join definiuje własny klucz podstawowy, więc możliwe jest duplikowanie tego sortowania.</span><span class="sxs-lookup"><span data-stu-id="43879-343">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="43879-344">Aby uniknąć takich duplikatów:</span><span class="sxs-lookup"><span data-stu-id="43879-344">To prevent such duplicates:</span></span>

* <span data-ttu-id="43879-345">Dodaj unikatowy indeks do pól klucza obcego lub</span><span class="sxs-lookup"><span data-stu-id="43879-345">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="43879-346">Skonfiguruj `Enrollment` przy użyciu podstawowego klucza złożonego podobnego `CourseAssignment`do.</span><span class="sxs-lookup"><span data-stu-id="43879-346">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="43879-347">Aby uzyskać więcej informacji, zobacz [indeksy](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="43879-347">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="43879-348">Aktualizowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="43879-348">Update the database context</span></span>

<span data-ttu-id="43879-349">Zaktualizuj *dane/SchoolContext. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-349">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="43879-350">Poprzedni kod dodaje nowe jednostki i konfiguruje `CourseAssignment` złożonego klucza podstawowego jednostki.</span><span class="sxs-lookup"><span data-stu-id="43879-350">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="43879-351">Alternatywny interfejs API Fluent dla atrybutów</span><span class="sxs-lookup"><span data-stu-id="43879-351">Fluent API alternative to attributes</span></span>

<span data-ttu-id="43879-352">Metoda w poprzednim kodzie używa *interfejsu API Fluent* , aby skonfigurować zachowanie EF Core. `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="43879-352">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="43879-353">Interfejs API jest nazywany "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod w jednej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="43879-353">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="43879-354">[Poniższy kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) jest przykładem interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="43879-354">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="43879-355">W tym samouczku interfejs API Fluent jest używany tylko na potrzeby mapowania bazy danych, której nie można wykonać przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="43879-355">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="43879-356">Jednak interfejs API Fluent może określić większość reguł formatowania, walidacji i mapowania, które mogą być wykonywane przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="43879-356">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="43879-357">Niektórych atrybutów, takich `MinimumLength` jak nie można zastosować w interfejsie API Fluent.</span><span class="sxs-lookup"><span data-stu-id="43879-357">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="43879-358">`MinimumLength`nie zmienia schematu, stosuje tylko regułę walidacji o minimalnej długości.</span><span class="sxs-lookup"><span data-stu-id="43879-358">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="43879-359">Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-359">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="43879-360">Atrybuty i interfejs API Fluent mogą być mieszane.</span><span class="sxs-lookup"><span data-stu-id="43879-360">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="43879-361">Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu interfejsu API Fluent (określenie złożonego klucza podstawowego).</span><span class="sxs-lookup"><span data-stu-id="43879-361">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="43879-362">Istnieją pewne konfiguracje, które można wykonać tylko z atrybutami (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="43879-362">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="43879-363">Zalecane rozwiązanie dotyczące korzystania z interfejsu API Fluent lub atrybutów:</span><span class="sxs-lookup"><span data-stu-id="43879-363">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="43879-364">Wybierz jedną z tych dwóch metod.</span><span class="sxs-lookup"><span data-stu-id="43879-364">Choose one of these two approaches.</span></span>
* <span data-ttu-id="43879-365">W miarę możliwości należy stosować wybrane podejście.</span><span class="sxs-lookup"><span data-stu-id="43879-365">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="43879-366">Niektóre atrybuty użyte w tym samouczku są używane w programie:</span><span class="sxs-lookup"><span data-stu-id="43879-366">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="43879-367">Tylko Walidacja (na przykład `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="43879-367">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="43879-368">Tylko konfiguracja EF Core (na przykład `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="43879-368">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="43879-369">Sprawdzanie poprawności i konfiguracja EF Core (na przykład `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="43879-369">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="43879-370">Aby uzyskać więcej informacji na temat atrybutów a interfejs API Fluent, zobacz [metody konfiguracji](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="43879-370">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="43879-371">Diagram jednostek</span><span class="sxs-lookup"><span data-stu-id="43879-371">Entity diagram</span></span>

<span data-ttu-id="43879-372">Na poniższej ilustracji przedstawiono diagram, który jest tworzony przez narzędzia Dr PowerShell dla gotowego modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="43879-372">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="43879-374">Na powyższym diagramie przedstawiono:</span><span class="sxs-lookup"><span data-stu-id="43879-374">The preceding diagram shows:</span></span>

* <span data-ttu-id="43879-375">Kilka linii relacji jeden-do-wielu (od 1 \*do).</span><span class="sxs-lookup"><span data-stu-id="43879-375">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="43879-376">Linia relacji "jeden do zera" (od 1 do 0.. 1) między `Instructor` jednostkami i. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="43879-376">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="43879-377">Linia relacji zero-lub-jeden-do-wielu (0.. 1 do \*) między `Instructor` obiektami i. `Department`</span><span class="sxs-lookup"><span data-stu-id="43879-377">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="43879-378">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="43879-378">Seed the database</span></span>

<span data-ttu-id="43879-379">Zaktualizuj kod w *danych/Dbinitializeer. cs*:</span><span class="sxs-lookup"><span data-stu-id="43879-379">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="43879-380">Poprzedni kod zawiera dane inicjatora dla nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-380">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="43879-381">Większość tego kodu tworzy nowe obiekty Entity i ładuje przykładowe dane.</span><span class="sxs-lookup"><span data-stu-id="43879-381">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="43879-382">Przykładowe dane służą do testowania.</span><span class="sxs-lookup"><span data-stu-id="43879-382">The sample data is used for testing.</span></span> <span data-ttu-id="43879-383">Zobacz `Enrollments` i `CourseAssignments` , aby zapoznać się z przykładami tabel sprzężenia wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="43879-383">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="43879-384">Dodawanie migracji</span><span class="sxs-lookup"><span data-stu-id="43879-384">Add a migration</span></span>

<span data-ttu-id="43879-385">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="43879-385">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-386">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-386">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43879-387">W obszarze PMC Uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="43879-387">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="43879-388">Poprzednie polecenie wyświetla ostrzeżenie o możliwej utracie danych.</span><span class="sxs-lookup"><span data-stu-id="43879-388">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="43879-389">`database update` Jeśli polecenie zostanie uruchomione, zostanie utworzony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="43879-389">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="43879-390">W następnej sekcji opisano, co należy zrobić, aby uzyskać informacje o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="43879-390">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-391">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-391">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="43879-392">W przypadku dodania migracji i uruchomienia `database update` polecenia zostanie utworzony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="43879-392">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="43879-393">W następnej sekcji zobaczysz, jak uniknąć tego błędu.</span><span class="sxs-lookup"><span data-stu-id="43879-393">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="43879-394">Zastosuj migrację lub Porzuć i Utwórz ponownie</span><span class="sxs-lookup"><span data-stu-id="43879-394">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="43879-395">Teraz, gdy masz już istniejącą bazę danych, musisz się zastanowić, jak zastosować do niej zmiany.</span><span class="sxs-lookup"><span data-stu-id="43879-395">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="43879-396">Ten samouczek przedstawia dwie alternatywy:</span><span class="sxs-lookup"><span data-stu-id="43879-396">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="43879-397">[Porzuć i ponownie utwórz bazę danych](#drop).</span><span class="sxs-lookup"><span data-stu-id="43879-397">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="43879-398">Wybierz tę sekcję, jeśli używasz oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="43879-398">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="43879-399">[Zastosuj migrację do istniejącej bazy danych](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="43879-399">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="43879-400">Instrukcje w tej sekcji działają tylko w przypadku SQL Server, a **nie dla oprogramowania SQLite**.</span><span class="sxs-lookup"><span data-stu-id="43879-400">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="43879-401">Dowolny wybór działa dla SQL Server.</span><span class="sxs-lookup"><span data-stu-id="43879-401">Either choice works for SQL Server.</span></span> <span data-ttu-id="43879-402">Chociaż metoda Apply-Migration jest bardziej złożona i czasochłonna, jest to preferowane podejście do rzeczywistych środowisk produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="43879-402">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="43879-403">Porzuć i ponownie utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="43879-403">Drop and re-create the database</span></span>

<span data-ttu-id="43879-404">[Pomiń tę sekcję](#apply-the-migration) , jeśli używasz SQL Server i chcesz wykonać podejście Apply-Migration w poniższej sekcji.</span><span class="sxs-lookup"><span data-stu-id="43879-404">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="43879-405">Aby wymusić EF Core tworzenia nowej bazy danych, Porzuć i zaktualizuj bazę danych:</span><span class="sxs-lookup"><span data-stu-id="43879-405">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="43879-407">W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43879-407">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="43879-408">Usuń folder *migrations* , a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43879-408">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-409">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-409">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="43879-410">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="43879-410">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="43879-411">Folder projektu zawiera plik *ContosoUniversity. csproj* .</span><span class="sxs-lookup"><span data-stu-id="43879-411">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="43879-412">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43879-412">Run the following command:</span></span>

  ```console
  dotnet ef database drop --force
  ```

* <span data-ttu-id="43879-413">Usuń folder *migrations* , a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43879-413">Delete the *Migrations* folder, then run the following command:</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="43879-414">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="43879-414">Run the app.</span></span> <span data-ttu-id="43879-415">Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę.</span><span class="sxs-lookup"><span data-stu-id="43879-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="43879-416">`DbInitializer.Initialize` Wypełnia nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43879-416">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-417">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-417">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43879-418">Otwórz bazę danych w programie SSOX:</span><span class="sxs-lookup"><span data-stu-id="43879-418">Open the database in SSOX:</span></span>

* <span data-ttu-id="43879-419">Jeśli SSOX został wcześniej otwarty, kliknij przycisk **Odśwież** .</span><span class="sxs-lookup"><span data-stu-id="43879-419">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="43879-420">Rozwiń **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="43879-420">Expand the **Tables** node.</span></span> <span data-ttu-id="43879-421">Zostaną wyświetlone utworzone tabele.</span><span class="sxs-lookup"><span data-stu-id="43879-421">The created tables are displayed.</span></span>

  ![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="43879-423">Zapoznaj się z tabelą **CourseAssignment** :</span><span class="sxs-lookup"><span data-stu-id="43879-423">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="43879-424">Kliknij prawym przyciskiem myszy tabelę **CourseAssignment** , a następnie wybierz polecenie **Wyświetl dane**.</span><span class="sxs-lookup"><span data-stu-id="43879-424">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="43879-425">Sprawdź, czy tabela **CourseAssignment** zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="43879-425">Verify the **CourseAssignment** table contains data.</span></span>

  ![CourseAssignment dane w SSOX](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-427">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-427">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="43879-428">Sprawdź bazę danych za pomocą narzędzia SQLite:</span><span class="sxs-lookup"><span data-stu-id="43879-428">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="43879-429">Nowe tabele i kolumny.</span><span class="sxs-lookup"><span data-stu-id="43879-429">New tables and columns.</span></span>
* <span data-ttu-id="43879-430">Dane są umieszczane w tabelach, na przykład w tabeli **CourseAssignment** .</span><span class="sxs-lookup"><span data-stu-id="43879-430">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="43879-431">Zastosuj migrację</span><span class="sxs-lookup"><span data-stu-id="43879-431">Apply the migration</span></span>

<span data-ttu-id="43879-432">Ta sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="43879-432">This section is optional.</span></span> <span data-ttu-id="43879-433">Te kroki działają tylko dla SQL Server LocalDB i tylko wtedy, gdy pominięto poprzednią [i ponownie utworzysz sekcję Database](#drop) .</span><span class="sxs-lookup"><span data-stu-id="43879-433">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="43879-434">Po uruchomieniu migracji z istniejącymi danymi mogą istnieć ograniczenia klucza obcego, które nie są spełnione w przypadku istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="43879-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="43879-435">W przypadku danych produkcyjnych należy wykonać kroki, aby przeprowadzić migrację istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="43879-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="43879-436">Ta sekcja zawiera przykład rozwiązywania naruszeń ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="43879-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="43879-437">Nie należy wprowadzać tych zmian w kodzie bez tworzenia kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="43879-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="43879-438">Nie wprowadzaj tych zmian w kodzie, jeśli wykonano poprzednie [upuszczenie i ponownie utworzysz sekcję bazy danych](#drop) .</span><span class="sxs-lookup"><span data-stu-id="43879-438">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="43879-439">Plik *{timestamp} _ComplexDataModel. cs* zawiera następujący kod:</span><span class="sxs-lookup"><span data-stu-id="43879-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="43879-440">Poprzedzający kod dodaje `DepartmentID` `Course` do tabeli obcy niedopuszczający wartości null.</span><span class="sxs-lookup"><span data-stu-id="43879-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="43879-441">Baza danych z poprzedniego samouczka zawiera wiersze w `Course`, dlatego nie można zaktualizować tabeli za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="43879-441">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="43879-442">Aby `ComplexDataModel` migracja była współdziałać z istniejącymi danymi:</span><span class="sxs-lookup"><span data-stu-id="43879-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="43879-443">Zmień kod, aby nadać nowej kolumnie (`DepartmentID`) wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="43879-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="43879-444">Utwórz fałszywy Departament o nazwie "Temp", który ma pełnić rolę działu domyślnego.</span><span class="sxs-lookup"><span data-stu-id="43879-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="43879-445">Popraw ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="43879-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="43879-446">W klasie `Up` migracji należy zaktualizować metodę: `ComplexDataModel`</span><span class="sxs-lookup"><span data-stu-id="43879-446">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="43879-447">Otwórz plik *{timestamp} _ComplexDataModel. cs* .</span><span class="sxs-lookup"><span data-stu-id="43879-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="43879-448">Dodaj komentarz `DepartmentID` `Course` do wiersza kodu, który dodaje kolumnę do tabeli.</span><span class="sxs-lookup"><span data-stu-id="43879-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="43879-449">Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="43879-449">Add the following highlighted code.</span></span> <span data-ttu-id="43879-450">Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku:</span><span class="sxs-lookup"><span data-stu-id="43879-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

<span data-ttu-id="43879-451">[! code-CSharp [] (wprowadzenie/przykłady/cu30snapshots/5 — złożone/migracje/ComplexDataModel. cs? Name = snippet_CreateDefaultValue & Podświetl = 23-31)]</span><span class="sxs-lookup"><span data-stu-id="43879-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span></span>

<span data-ttu-id="43879-452">Po wykonaniu powyższych zmian istniejące `Course` wiersze będą powiązane z działem "Temp" po uruchomieniu metody.`ComplexDataModel.Up`</span><span class="sxs-lookup"><span data-stu-id="43879-452">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="43879-453">Sposób obsługi pokazanej tutaj sytuacji jest uproszczony dla tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="43879-453">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="43879-454">Aplikacja produkcyjna:</span><span class="sxs-lookup"><span data-stu-id="43879-454">A production app would:</span></span>

* <span data-ttu-id="43879-455">Dołącz kod lub skrypty, aby `Department` dodać wiersze i `Course` powiązane wiersze do nowych `Department` wierszy.</span><span class="sxs-lookup"><span data-stu-id="43879-455">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="43879-456">Nie używaj działu "Temp" ani wartości domyślnej dla `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="43879-456">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-457">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-457">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="43879-458">W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43879-458">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="43879-459">`DbInitializer.Initialize` Ponieważ metoda została zaprojektowana tak, aby działała tylko z pustą bazą danych, użyj SSOX, aby usunąć wszystkie wiersze z tabel uczniów i kursów.</span><span class="sxs-lookup"><span data-stu-id="43879-459">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="43879-460">(Usuwanie kaskadowe zajmie tabelę rejestracji).</span><span class="sxs-lookup"><span data-stu-id="43879-460">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-461">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-461">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="43879-462">Jeśli używasz SQL Server LocalDB z Visual Studio Code, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43879-462">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```console
  dotnet ef database update
  ```

---

<span data-ttu-id="43879-463">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="43879-463">Run the app.</span></span> <span data-ttu-id="43879-464">Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę.</span><span class="sxs-lookup"><span data-stu-id="43879-464">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="43879-465">`DbInitializer.Initialize` Wypełnia nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43879-465">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43879-466">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="43879-466">Next steps</span></span>

<span data-ttu-id="43879-467">W następnych dwóch samouczkach pokazano, jak odczytywać i aktualizować powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="43879-467">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="43879-468">[Poprzedni](xref:data/ef-rp/migrations)
> samouczek w[następnym](xref:data/ef-rp/read-related-data) samouczku</span><span class="sxs-lookup"><span data-stu-id="43879-468">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="43879-469">Poprzednie samouczki pracowały z podstawowym modelem danych, który składa się z trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-469">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="43879-470">W tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="43879-470">In this tutorial:</span></span>

* <span data-ttu-id="43879-471">Dodano więcej jednostek i relacji.</span><span class="sxs-lookup"><span data-stu-id="43879-471">More entities and relationships are added.</span></span>
* <span data-ttu-id="43879-472">Model danych jest dostosowywany przez określenie formatowania, walidacji i reguł mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43879-472">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="43879-473">Klasy jednostek dla kompletnego modelu danych przedstawiono na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="43879-473">The entity classes for the completed data model are shown in the following illustration:</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="43879-475">Jeśli wystąpią problemy, których nie można rozwiązać, Pobierz [ukończoną](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)aplikację.</span><span class="sxs-lookup"><span data-stu-id="43879-475">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="43879-476">Dostosowywanie modelu danych przy użyciu atrybutów</span><span class="sxs-lookup"><span data-stu-id="43879-476">Customize the data model with attributes</span></span>

<span data-ttu-id="43879-477">W tej sekcji model danych jest dostosowany przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="43879-477">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="43879-478">Atrybut DataType</span><span class="sxs-lookup"><span data-stu-id="43879-478">The DataType attribute</span></span>

<span data-ttu-id="43879-479">Na stronach ucznia jest obecnie wyświetlana godzina daty rejestracji.</span><span class="sxs-lookup"><span data-stu-id="43879-479">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="43879-480">Zwykle pola Date zawierają tylko datę, a nie czas.</span><span class="sxs-lookup"><span data-stu-id="43879-480">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="43879-481">Aktualizuj *modele/uczniów. cs* przy użyciu następującego wyróżnionego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-481">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="43879-482">Atrybut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) określa typ danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43879-482">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="43879-483">W tym przypadku należy wyświetlić tylko datę, a nie datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="43879-483">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="43879-484">[Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress itp. Ten `DataType` atrybut może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu.</span><span class="sxs-lookup"><span data-stu-id="43879-484">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="43879-485">Przykład:</span><span class="sxs-lookup"><span data-stu-id="43879-485">For example:</span></span>

* <span data-ttu-id="43879-486">Łącze jest tworzone automatycznie dla `DataType.EmailAddress`. `mailto:`</span><span class="sxs-lookup"><span data-stu-id="43879-486">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="43879-487">Selektor daty jest dostępny `DataType.Date` w większości przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="43879-487">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="43879-488">Ten `DataType` atrybut emituje kod HTML `data-` 5 (wymawiane kreski danych), które wykorzystują przeglądarki HTML 5.</span><span class="sxs-lookup"><span data-stu-id="43879-488">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="43879-489">`DataType` Atrybuty nie zapewniają walidacji.</span><span class="sxs-lookup"><span data-stu-id="43879-489">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="43879-490">`DataType.Date`nie określa formatu wyświetlanej daty.</span><span class="sxs-lookup"><span data-stu-id="43879-490">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="43879-491">Domyślnie pole Date jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)serwera.</span><span class="sxs-lookup"><span data-stu-id="43879-491">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="43879-492">Ten `DisplayFormat` atrybut służy do jawnego określenia formatu daty:</span><span class="sxs-lookup"><span data-stu-id="43879-492">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="43879-493">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie ma być również stosowane do interfejsu użytkownika edytowania.</span><span class="sxs-lookup"><span data-stu-id="43879-493">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="43879-494">Niektóre pola nie powinny `ApplyFormatInEditMode`być używane.</span><span class="sxs-lookup"><span data-stu-id="43879-494">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="43879-495">Na przykład symbol waluty zazwyczaj nie powinien być wyświetlany w polu tekstowym Edycja.</span><span class="sxs-lookup"><span data-stu-id="43879-495">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="43879-496">Ten `DisplayFormat` atrybut może być używany przez siebie.</span><span class="sxs-lookup"><span data-stu-id="43879-496">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="43879-497">Zwykle dobrym pomysłem jest użycie `DataType` atrybutu `DisplayFormat` z atrybutem.</span><span class="sxs-lookup"><span data-stu-id="43879-497">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="43879-498">Ten `DataType` atrybut przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie.</span><span class="sxs-lookup"><span data-stu-id="43879-498">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="43879-499">Ten `DataType` atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`programie:</span><span class="sxs-lookup"><span data-stu-id="43879-499">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="43879-500">Przeglądarka może włączyć funkcje HTML5.</span><span class="sxs-lookup"><span data-stu-id="43879-500">The browser can enable HTML5 features.</span></span> <span data-ttu-id="43879-501">Na przykład Pokaż kontrolkę kalendarz, odpowiedni dla ustawień regionalnych symbol waluty, linki poczty e-mail, sprawdzanie poprawności danych po stronie klienta itd.</span><span class="sxs-lookup"><span data-stu-id="43879-501">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="43879-502">Domyślnie przeglądarka renderuje dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.</span><span class="sxs-lookup"><span data-stu-id="43879-502">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="43879-503">Aby uzyskać więcej informacji, zobacz [ \<dokumentację pomocnika tagów > danych wejściowych](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="43879-503">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="43879-504">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="43879-504">Run the app.</span></span> <span data-ttu-id="43879-505">Przejdź do strony indeksu uczniów.</span><span class="sxs-lookup"><span data-stu-id="43879-505">Navigate to the Students Index page.</span></span> <span data-ttu-id="43879-506">Czasy nie są już wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="43879-506">Times are no longer displayed.</span></span> <span data-ttu-id="43879-507">Każdy widok korzystający z `Student` modelu wyświetla datę bez czasu.</span><span class="sxs-lookup"><span data-stu-id="43879-507">Every view that uses the `Student` model displays the date without time.</span></span>

![Strona indeksu uczniów pokazująca daty bez czasu](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="43879-509">Atrybut StringLength</span><span class="sxs-lookup"><span data-stu-id="43879-509">The StringLength attribute</span></span>

<span data-ttu-id="43879-510">Można określić reguły walidacji danych i komunikaty o błędach walidacji z atrybutami.</span><span class="sxs-lookup"><span data-stu-id="43879-510">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="43879-511">Atrybut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych.</span><span class="sxs-lookup"><span data-stu-id="43879-511">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="43879-512">Ten `StringLength` atrybut zapewnia również weryfikację po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="43879-512">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="43879-513">Wartość minimalna nie ma wpływu na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43879-513">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="43879-514">`Student` Zaktualizuj model przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-514">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="43879-515">Poprzedni kod ogranicza nazwy do nie więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="43879-515">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="43879-516">Ten `StringLength` atrybut nie uniemożliwia użytkownikowi wprowadzania białych znaków w nazwie.</span><span class="sxs-lookup"><span data-stu-id="43879-516">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="43879-517">Atrybut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) służy do stosowania ograniczeń do danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="43879-517">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="43879-518">Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:</span><span class="sxs-lookup"><span data-stu-id="43879-518">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="43879-519">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="43879-519">Run the app:</span></span>

* <span data-ttu-id="43879-520">Przejdź do strony uczniów.</span><span class="sxs-lookup"><span data-stu-id="43879-520">Navigate to the Students page.</span></span>
* <span data-ttu-id="43879-521">Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę o długości większej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="43879-521">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="43879-522">Wybierz pozycję **Utwórz**, a po stronie klienta zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="43879-522">Select **Create**, client-side validation shows an error message.</span></span>

![Strona indeksu studentów przedstawiająca błędy długości ciągu](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="43879-524">W **Eksplorator obiektów SQL Server** (SSOX) Otwórz projektanta tabeli uczniów, klikając dwukrotnie tabelę **uczniów** .</span><span class="sxs-lookup"><span data-stu-id="43879-524">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela studentów w SSOX przed migracją](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="43879-526">Na powyższym obrazie przedstawiono schemat `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="43879-526">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="43879-527">Pola nazw mają typ `nvarchar(MAX)` , ponieważ migracja nie została uruchomiona w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="43879-527">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="43879-528">Gdy migracja zostanie uruchomiona w dalszej części tego samouczka, pola nazwy `nvarchar(50)`stają się.</span><span class="sxs-lookup"><span data-stu-id="43879-528">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="43879-529">Atrybut Column</span><span class="sxs-lookup"><span data-stu-id="43879-529">The Column attribute</span></span>

<span data-ttu-id="43879-530">Atrybuty mogą kontrolować sposób mapowania klas i właściwości do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43879-530">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="43879-531">W tej sekcji `Column` atrybut jest używany do mapowania nazwy `FirstMidName` właściwości na "FirstName" w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="43879-531">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="43879-532">Po utworzeniu bazy danych nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, `Column` gdy atrybut jest używany).</span><span class="sxs-lookup"><span data-stu-id="43879-532">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="43879-533">`Student` Model używa`FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową.</span><span class="sxs-lookup"><span data-stu-id="43879-533">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="43879-534">Zaktualizuj plik *student.cs* za pomocą następującego wyróżnionego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-534">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="43879-535">Z poprzednią zmianą `Student.FirstMidName` w aplikacji jest mapowany `FirstName` do kolumny `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="43879-535">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="43879-536">Dodanie `Column` atrybutu zmienia model z `SchoolContext`kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="43879-536">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="43879-537">Model, który wykonuje `SchoolContext` kopię zapasową, nie jest już zgodny z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="43879-537">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="43879-538">Jeśli aplikacja jest uruchamiana przed zastosowaniem migracji, generowany jest następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="43879-538">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="43879-539">Aby zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="43879-539">To update the DB:</span></span>

* <span data-ttu-id="43879-540">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="43879-540">Build the project.</span></span>
* <span data-ttu-id="43879-541">Otwórz okno polecenia w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="43879-541">Open a command window in the project folder.</span></span> <span data-ttu-id="43879-542">Wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="43879-542">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-543">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-544">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-544">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="43879-545">`migrations add ColumnFirstName` Polecenie generuje następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="43879-545">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="43879-546">Ostrzeżenie jest generowane, ponieważ pola nazw są teraz ograniczone do 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="43879-546">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="43879-547">Jeśli nazwa w bazie danych ma więcej niż 50 znaków, zostanie utracony od 51 do ostatniego znaku.</span><span class="sxs-lookup"><span data-stu-id="43879-547">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="43879-548">Przetestuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="43879-548">Test the app.</span></span>

<span data-ttu-id="43879-549">Otwórz tabelę uczniów w SSOX:</span><span class="sxs-lookup"><span data-stu-id="43879-549">Open the Student table in SSOX:</span></span>

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="43879-551">Przed zainstalowaniem migracji kolumny nazw były typu [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="43879-551">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="43879-552">Kolumny nazw są teraz `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="43879-552">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="43879-553">Nazwa kolumny została zmieniona z `FirstMidName` na. `FirstName`</span><span class="sxs-lookup"><span data-stu-id="43879-553">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="43879-554">W poniższej sekcji Kompilowanie aplikacji na niektórych etapach powoduje wygenerowanie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="43879-554">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="43879-555">Instrukcje określają, kiedy należy skompilować aplikację.</span><span class="sxs-lookup"><span data-stu-id="43879-555">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="43879-556">Aktualizacja jednostki ucznia</span><span class="sxs-lookup"><span data-stu-id="43879-556">Student entity update</span></span>

![Jednostka ucznia](complex-data-model/_static/student-entity.png)

<span data-ttu-id="43879-558">Aktualizuj *modele/uczniów. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-558">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="43879-559">Wymagany atrybut</span><span class="sxs-lookup"><span data-stu-id="43879-559">The Required attribute</span></span>

<span data-ttu-id="43879-560">`Required` Atrybut powoduje, że właściwości nazwy są wymagane.</span><span class="sxs-lookup"><span data-stu-id="43879-560">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="43879-561">Ten `Required` atrybut nie jest wymagany dla typów niedopuszczających wartości null, takich jak `int`typy `double`wartości (`DateTime`,, itp.).</span><span class="sxs-lookup"><span data-stu-id="43879-561">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="43879-562">Typy, które nie mogą mieć wartości null, są automatycznie traktowane jako pola wymagane.</span><span class="sxs-lookup"><span data-stu-id="43879-562">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="43879-563">Ten `Required` atrybut może być zastąpiony parametrem o minimalnej długości `StringLength` w atrybucie:</span><span class="sxs-lookup"><span data-stu-id="43879-563">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="43879-564">Atrybut wyświetlania</span><span class="sxs-lookup"><span data-stu-id="43879-564">The Display attribute</span></span>

<span data-ttu-id="43879-565">Ten `Display` atrybut określa, że podpis pól tekstowych powinien mieć wartość "imię i nazwisko", "nazwisko", "imię i nazwisko" oraz "Data rejestracji".</span><span class="sxs-lookup"><span data-stu-id="43879-565">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="43879-566">Domyślne podpisy nie zawierają spacji dzielących wyrazy, na przykład "LastName".</span><span class="sxs-lookup"><span data-stu-id="43879-566">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="43879-567">Właściwość obliczeniowa FullName</span><span class="sxs-lookup"><span data-stu-id="43879-567">The FullName calculated property</span></span>

<span data-ttu-id="43879-568">`FullName`jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="43879-568">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="43879-569">`FullName`nie można ustawić, ma tylko metodę dostępu get.</span><span class="sxs-lookup"><span data-stu-id="43879-569">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="43879-570">Nie `FullName` utworzono żadnej kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="43879-570">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="43879-571">Tworzenie jednostki instruktora</span><span class="sxs-lookup"><span data-stu-id="43879-571">Create the Instructor Entity</span></span>

![Jednostka instruktora](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="43879-573">Utwórz *modele/instruktor. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-573">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="43879-574">Wiele atrybutów może znajdować się w jednym wierszu.</span><span class="sxs-lookup"><span data-stu-id="43879-574">Multiple attributes can be on one line.</span></span> <span data-ttu-id="43879-575">`HireDate` Atrybuty mogą być zapisywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="43879-575">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="43879-576">Właściwości nawigacji CourseAssignments i OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="43879-576">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="43879-577">Właściwości `CourseAssignments` i`OfficeAssignment` są właściwościami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="43879-577">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="43879-578">Instruktor może nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="43879-578">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="43879-579">Jeśli właściwość nawigacji zawiera wiele jednostek:</span><span class="sxs-lookup"><span data-stu-id="43879-579">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="43879-580">Musi to być typ listy, w którym można dodawać, usuwać i aktualizować wpisy.</span><span class="sxs-lookup"><span data-stu-id="43879-580">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="43879-581">Typy właściwości nawigacji obejmują:</span><span class="sxs-lookup"><span data-stu-id="43879-581">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="43879-582">Jeśli `ICollection<T>` jest określony, EF Core domyślnie `HashSet<T>` tworzy kolekcję.</span><span class="sxs-lookup"><span data-stu-id="43879-582">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="43879-583">`CourseAssignment` Jednostka została omówiona w sekcji dotyczącej relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="43879-583">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="43879-584">Firma Contoso University Rules States, że instruktor może mieć co najwyżej jednego biura.</span><span class="sxs-lookup"><span data-stu-id="43879-584">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="43879-585">Właściwość zawiera jedną `OfficeAssignment`jednostkę. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="43879-585">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="43879-586">`OfficeAssignment`ma wartość null, jeśli nie przypisano pakietu Office.</span><span class="sxs-lookup"><span data-stu-id="43879-586">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="43879-587">Tworzenie jednostki OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="43879-587">Create the OfficeAssignment entity</span></span>

![Jednostka OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="43879-589">Utwórz *modele/OfficeAssignment. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-589">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="43879-590">Atrybut klucza</span><span class="sxs-lookup"><span data-stu-id="43879-590">The Key attribute</span></span>

<span data-ttu-id="43879-591">Ten `[Key]` atrybut służy do identyfikowania właściwości jako klucza podstawowego (PK), gdy nazwa właściwości ma wartość inną niż classnameID lub ID.</span><span class="sxs-lookup"><span data-stu-id="43879-591">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="43879-592">Istnieje relacja jeden do zera między `Instructor` jednostkami i. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="43879-592">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="43879-593">Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane.</span><span class="sxs-lookup"><span data-stu-id="43879-593">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="43879-594">Klucz podstawowy jest również jego kluczem obcym (obcy) `Instructor` do jednostki. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="43879-594">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="43879-595">EF Core nie może automatycznie `InstructorID` rozpoznać jako klucz podstawowy `OfficeAssignment` dla:</span><span class="sxs-lookup"><span data-stu-id="43879-595">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="43879-596">`InstructorID`nie jest zgodna z konwencją nazewnictwa ID lub classnameID.</span><span class="sxs-lookup"><span data-stu-id="43879-596">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="43879-597">W związku z tym `InstructorID` atrybutjestużywanydoidentyfikacjijakokluczpodstawowy:`Key`</span><span class="sxs-lookup"><span data-stu-id="43879-597">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="43879-598">Domyślnie EF Core traktuje klucz jako wygenerowane poza bazą danych, ponieważ kolumna służy do identyfikowania relacji.</span><span class="sxs-lookup"><span data-stu-id="43879-598">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="43879-599">Właściwość nawigacji instruktora</span><span class="sxs-lookup"><span data-stu-id="43879-599">The Instructor navigation property</span></span>

<span data-ttu-id="43879-600">Właściwość `OfficeAssignment` nawigacji `Instructor` dla jednostki nie dopuszcza wartości null, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="43879-600">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="43879-601">Typy odwołań (takie jak klasy są dopuszczające wartość null).</span><span class="sxs-lookup"><span data-stu-id="43879-601">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="43879-602">Instruktor może nie mieć przypisania pakietu Office.</span><span class="sxs-lookup"><span data-stu-id="43879-602">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="43879-603">Jednostka ma właściwość nawigacji, która nie `Instructor` dopuszcza wartości null, ponieważ: `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="43879-603">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="43879-604">`InstructorID`nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="43879-604">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="43879-605">Przypisanie pakietu Office nie może istnieć bez instruktora.</span><span class="sxs-lookup"><span data-stu-id="43879-605">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="43879-606">Gdy jednostka ma powiązaną `OfficeAssignment` jednostkę, każda jednostka ma odwołanie do drugiej z nich we właściwości nawigacji. `Instructor`</span><span class="sxs-lookup"><span data-stu-id="43879-606">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="43879-607">Ten `[Required]` atrybut może zostać zastosowany `Instructor` do właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="43879-607">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="43879-608">Poprzedni kod określa, że musi być pokrewnym instruktorem.</span><span class="sxs-lookup"><span data-stu-id="43879-608">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="43879-609">Poprzedni kod jest niezbędny, `InstructorID` ponieważ klucz obcy (który jest również kluczem PK) nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="43879-609">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="43879-610">Modyfikowanie jednostki kursu</span><span class="sxs-lookup"><span data-stu-id="43879-610">Modify the Course Entity</span></span>

![Jednostka kursu](complex-data-model/_static/course-entity.png)

<span data-ttu-id="43879-612">Aktualizuj *modele/kurs. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-612">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="43879-613">Jednostka ma właściwość `DepartmentID`klucza obcego (obcy). `Course`</span><span class="sxs-lookup"><span data-stu-id="43879-613">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="43879-614">`DepartmentID`wskazuje powiązaną `Department` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="43879-614">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="43879-615">`Course` Jednostka ma właściwość nawigacji. `Department`</span><span class="sxs-lookup"><span data-stu-id="43879-615">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="43879-616">EF Core nie wymaga właściwości FK dla modelu danych, gdy model ma właściwość nawigacji dla powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="43879-616">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="43879-617">EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie są one zbędne.</span><span class="sxs-lookup"><span data-stu-id="43879-617">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="43879-618">EF Core tworzy [Właściwości cienia](/ef/core/modeling/shadow-properties) dla automatycznie utworzonych FKs.</span><span class="sxs-lookup"><span data-stu-id="43879-618">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="43879-619">Posiadanie klucza obcego w modelu danych może sprawiać, że aktualizacje są prostsze i wydajniejsze.</span><span class="sxs-lookup"><span data-stu-id="43879-619">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="43879-620">Rozważmy na przykład model, w którym Właściwość `DepartmentID` FK *nie* jest uwzględniona.</span><span class="sxs-lookup"><span data-stu-id="43879-620">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="43879-621">Gdy jednostka kursu jest pobierana do edycji:</span><span class="sxs-lookup"><span data-stu-id="43879-621">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="43879-622">Jednostka `Department` ma wartość null, jeśli nie jest jawnie załadowana.</span><span class="sxs-lookup"><span data-stu-id="43879-622">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="43879-623">Aby zaktualizować jednostkę kursu, `Department` należy najpierw pobrać jednostkę.</span><span class="sxs-lookup"><span data-stu-id="43879-623">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="43879-624">Gdy właściwość `DepartmentID` FK jest uwzględniona w modelu danych, nie trzeba `Department` pobierać jednostki przed aktualizacją.</span><span class="sxs-lookup"><span data-stu-id="43879-624">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="43879-625">Atrybut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="43879-625">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="43879-626">Ten `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atrybut określa, że klucz podstawowy jest dostarczany przez aplikację, a nie generowany przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43879-626">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="43879-627">Domyślnie EF Core zakłada, że wartości klucza PK są generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43879-627">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="43879-628">Wartość klucza podstawowego wygenerowanego przez bazę danych jest ogólnie najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="43879-628">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="43879-629">W `Course` przypadku jednostek użytkownik określa klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="43879-629">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="43879-630">Na przykład numer kursu, taki jak seria 1000 dla działu Math, serii 2000 dla działu angielskiego.</span><span class="sxs-lookup"><span data-stu-id="43879-630">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="43879-631">Ten `DatabaseGenerated` atrybut może być również używany do generowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="43879-631">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="43879-632">Na przykład baza danych może automatycznie wygenerować pole daty, aby zarejestrować datę utworzenia lub zaktualizowania wiersza.</span><span class="sxs-lookup"><span data-stu-id="43879-632">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="43879-633">Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="43879-633">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="43879-634">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="43879-634">Foreign key and navigation properties</span></span>

<span data-ttu-id="43879-635">Właściwości klucza obcego (FK) i właściwości nawigacji w `Course` jednostce odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="43879-635">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="43879-636">Kurs jest przypisywany do jednego działu, więc istnieje `DepartmentID` obcy `Department` i właściwość nawigacji.</span><span class="sxs-lookup"><span data-stu-id="43879-636">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="43879-637">Kurs może zawierać dowolną liczbę studentów, więc `Enrollments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="43879-637">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="43879-638">Kurs może być nauczany przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="43879-638">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="43879-639">`CourseAssignment`wyjaśniono [później](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="43879-639">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="43879-640">Tworzenie jednostki działu</span><span class="sxs-lookup"><span data-stu-id="43879-640">Create the Department entity</span></span>

![Jednostka działu](complex-data-model/_static/department-entity.png)

<span data-ttu-id="43879-642">Utwórz *modele/dział. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-642">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="43879-643">Atrybut Column</span><span class="sxs-lookup"><span data-stu-id="43879-643">The Column attribute</span></span>

<span data-ttu-id="43879-644">`Column` Wcześniej atrybut został użyty do zmiany mapowania nazw kolumn.</span><span class="sxs-lookup"><span data-stu-id="43879-644">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="43879-645">W kodzie dla `Department` jednostki `Column` atrybut jest używany do zmiany mapowania typu danych SQL.</span><span class="sxs-lookup"><span data-stu-id="43879-645">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="43879-646">`Budget` Kolumna jest definiowana przy użyciu SQL Server typie pieniędzy w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="43879-646">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="43879-647">Mapowanie kolumn zazwyczaj nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="43879-647">Column mapping is generally not required.</span></span> <span data-ttu-id="43879-648">EF Core zwykle wybiera odpowiedni SQL Server typ danych oparty na typie CLR właściwości.</span><span class="sxs-lookup"><span data-stu-id="43879-648">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="43879-649">Typ CLR `decimal` jest mapowany na typ SQL Server `decimal` .</span><span class="sxs-lookup"><span data-stu-id="43879-649">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="43879-650">`Budget`jest dla waluty, a typ danych walutowych jest bardziej odpowiedni dla waluty.</span><span class="sxs-lookup"><span data-stu-id="43879-650">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="43879-651">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="43879-651">Foreign key and navigation properties</span></span>

<span data-ttu-id="43879-652">Właściwości FK i nawigacji odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="43879-652">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="43879-653">Dział może lub nie ma uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="43879-653">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="43879-654">Administrator jest zawsze instruktorem.</span><span class="sxs-lookup"><span data-stu-id="43879-654">An administrator is always an instructor.</span></span> <span data-ttu-id="43879-655">W związku z tym `Instructor` Właściwośćjestdołączanajakoobcydojednostki.`InstructorID`</span><span class="sxs-lookup"><span data-stu-id="43879-655">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="43879-656">Właściwość nawigacji ma nazwę `Administrator` , ale `Instructor` zawiera jednostkę:</span><span class="sxs-lookup"><span data-stu-id="43879-656">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="43879-657">Znak zapytania (?) w powyższym kodzie określa właściwość dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="43879-657">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="43879-658">Dział może mieć wiele kursów, więc istnieje właściwość nawigacji kursów:</span><span class="sxs-lookup"><span data-stu-id="43879-658">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="43879-659">Uwaga: Zgodnie z Konwencją EF Core włącza kaskadowe usuwanie dla FKs niedopuszczających wartości null i dla relacji wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="43879-659">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="43879-660">Kaskadowe usuwanie może spowodować cykliczne reguły usuwania kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="43879-660">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="43879-661">Cykliczne reguły usuwania kaskadowego powodują wyjątek podczas dodawania migracji.</span><span class="sxs-lookup"><span data-stu-id="43879-661">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="43879-662">Na przykład, jeśli `Department.InstructorID` właściwość została zdefiniowana jako niedopuszczający wartości null:</span><span class="sxs-lookup"><span data-stu-id="43879-662">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="43879-663">EF Core konfiguruje regułę usuwania kaskadowego w celu usunięcia działu po usunięciu instruktora.</span><span class="sxs-lookup"><span data-stu-id="43879-663">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="43879-664">Usunięcie działu po usunięciu instruktora nie jest zamierzonym zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="43879-664">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="43879-665">Poniższy interfejs API Fluent ustawi regułę ograniczenia zamiast kaskadowo.</span><span class="sxs-lookup"><span data-stu-id="43879-665">The following fluent API would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="43879-666">Poprzedni kod wyłącza usuwanie kaskadowe dla relacji instruktora działu.</span><span class="sxs-lookup"><span data-stu-id="43879-666">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="43879-667">Aktualizowanie jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="43879-667">Update the Enrollment entity</span></span>

<span data-ttu-id="43879-668">Rekord rejestracji jest wykonywany przez jednego ucznia.</span><span class="sxs-lookup"><span data-stu-id="43879-668">An enrollment record is for one course taken by one student.</span></span>

![Jednostka rejestracji](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="43879-670">Aktualizuj *modele/rejestrację. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-670">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="43879-671">Właściwości klucza obcego i nawigacji</span><span class="sxs-lookup"><span data-stu-id="43879-671">Foreign key and navigation properties</span></span>

<span data-ttu-id="43879-672">Właściwości klucza obcego i właściwości nawigacji odzwierciedlają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="43879-672">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="43879-673">Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje `CourseID` Właściwość FK `Course` i właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="43879-673">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="43879-674">Rekord rejestracji jest przeznaczony dla jednego ucznia, dlatego istnieje `StudentID` właściwość klucza obcego `Student` i właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="43879-674">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="43879-675">Relacje wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="43879-675">Many-to-Many Relationships</span></span>

<span data-ttu-id="43879-676">Istnieje relacja wiele-do-wielu między `Student` obiektami i. `Course`</span><span class="sxs-lookup"><span data-stu-id="43879-676">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="43879-677">Jednostka działa jako tabela sprzężenia wiele-do-wielu *z ładunkiem* w bazie danych. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="43879-677">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="43879-678">"Z ładunkiem" oznacza, że `Enrollment` tabela zawiera dodatkowe dane oprócz FKs dla sprzężonych tabel (w tym przypadku klucz podstawowy i `Grade`).</span><span class="sxs-lookup"><span data-stu-id="43879-678">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="43879-679">Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-679">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="43879-680">(Ten diagram został wygenerowany przy użyciu [narzędzi EF PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) dla programu EF 6. x.</span><span class="sxs-lookup"><span data-stu-id="43879-680">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="43879-681">Tworzenie diagramu nie jest częścią samouczka.</span><span class="sxs-lookup"><span data-stu-id="43879-681">Creating the diagram isn't part of the tutorial.)</span></span>

![Student-kurs wiele do wielu relacji](complex-data-model/_static/student-course.png)

<span data-ttu-id="43879-683">Każda linia relacji ma 1 na jednym końcu i gwiazdkę (\*) na drugim, wskazując relację jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="43879-683">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="43879-684">Jeśli tabela nie zawiera informacji o klasie, musi zawierać tylko dwa FKs (`CourseID` i `StudentID`). `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="43879-684">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="43879-685">Tabela sprzężenia wiele-do-wielu bez ładunku jest czasami nazywana tabelą sprzężenia czystego (PJT).</span><span class="sxs-lookup"><span data-stu-id="43879-685">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="43879-686">Jednostki `Instructor` i`Course` mają relację wiele-do-wielu przy użyciu czystej tabeli sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="43879-686">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="43879-687">Uwaga: Dr 6. x obsługuje niejawne tabele sprzężeń dla relacji wiele-do-wielu, ale EF Core nie.</span><span class="sxs-lookup"><span data-stu-id="43879-687">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="43879-688">Aby uzyskać więcej informacji, zobacz [relacje wiele-do-wielu w EF Core 2,0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="43879-688">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="43879-689">Jednostka CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="43879-689">The CourseAssignment entity</span></span>

![Jednostka CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="43879-691">Utwórz *modele/CourseAssignment. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="43879-691">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="43879-692">Instruktorzy do kursów</span><span class="sxs-lookup"><span data-stu-id="43879-692">Instructor-to-Courses</span></span>

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="43879-694">"Instruktora" — relacja wiele do wielu:</span><span class="sxs-lookup"><span data-stu-id="43879-694">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="43879-695">Wymaga tabeli sprzężenia, która musi być reprezentowana przez zestaw jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-695">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="43879-696">Jest czystym tabelą sprzężenia (tabela bez ładunku).</span><span class="sxs-lookup"><span data-stu-id="43879-696">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="43879-697">Nazwa jednostki `EntityName1EntityName2`sprzężenia jest wspólna.</span><span class="sxs-lookup"><span data-stu-id="43879-697">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="43879-698">Na przykład tabela sprzężenia "instruktor-kurs" używa tego wzorca jest `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="43879-698">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="43879-699">Zalecamy jednak użycie nazwy opisującej relację.</span><span class="sxs-lookup"><span data-stu-id="43879-699">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="43879-700">Modele danych rozpoczynają się od siebie i rosną.</span><span class="sxs-lookup"><span data-stu-id="43879-700">Data models start out simple and grow.</span></span> <span data-ttu-id="43879-701">Sprzężenia bez ładunku (PJTs) często są rozwijane w celu uwzględnienia ładunku.</span><span class="sxs-lookup"><span data-stu-id="43879-701">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="43879-702">Rozpoczynając od nazwy obiektu opisowego, nie trzeba zmieniać nazwy po zmianie tabeli sprzężeń.</span><span class="sxs-lookup"><span data-stu-id="43879-702">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="43879-703">Najlepiej, aby jednostka sprzężenia miała własną nazwę naturalną (prawdopodobnie jednowyrazową) w domenie biznesowej.</span><span class="sxs-lookup"><span data-stu-id="43879-703">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="43879-704">Na przykład książki i klienci mogą być połączone z jednostką sprzężenia o nazwie ratings.</span><span class="sxs-lookup"><span data-stu-id="43879-704">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="43879-705">Dla instruktora "wiele do wielu", `CourseAssignment` `CourseInstructor`preferowana jest wartość.</span><span class="sxs-lookup"><span data-stu-id="43879-705">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="43879-706">Klucz złożony</span><span class="sxs-lookup"><span data-stu-id="43879-706">Composite key</span></span>

<span data-ttu-id="43879-707">FKs nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="43879-707">FKs are not nullable.</span></span> <span data-ttu-id="43879-708">Dwa FKs w `CourseAssignment` (`InstructorID` `CourseID` i`CourseAssignment` ) jednoznacznie identyfikują każdy wiersz tabeli.</span><span class="sxs-lookup"><span data-stu-id="43879-708">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="43879-709">`CourseAssignment`nie wymaga dedykowanego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="43879-709">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="43879-710">Właściwości `InstructorID` i`CourseID` działają jako złożony klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="43879-710">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="43879-711">Jedynym sposobem określenia złożonego PKs do EF Core jest *interfejs API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="43879-711">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="43879-712">W następnej sekcji przedstawiono sposób konfigurowania złożonego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="43879-712">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="43879-713">Klucz złożony gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="43879-713">The composite key ensures:</span></span>

* <span data-ttu-id="43879-714">W jednym kursie są dozwolone wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="43879-714">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="43879-715">Dla jednego instruktora są dozwolone wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="43879-715">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="43879-716">Wiele wierszy dla tego samego instruktora i kursu jest niedozwolonych.</span><span class="sxs-lookup"><span data-stu-id="43879-716">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="43879-717">Jednostka `Enrollment` Join definiuje własny klucz podstawowy, więc możliwe jest duplikowanie tego sortowania.</span><span class="sxs-lookup"><span data-stu-id="43879-717">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="43879-718">Aby uniknąć takich duplikatów:</span><span class="sxs-lookup"><span data-stu-id="43879-718">To prevent such duplicates:</span></span>

* <span data-ttu-id="43879-719">Dodaj unikatowy indeks do pól klucza obcego lub</span><span class="sxs-lookup"><span data-stu-id="43879-719">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="43879-720">Skonfiguruj `Enrollment` przy użyciu podstawowego klucza złożonego podobnego `CourseAssignment`do.</span><span class="sxs-lookup"><span data-stu-id="43879-720">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="43879-721">Aby uzyskać więcej informacji, zobacz [indeksy](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="43879-721">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="43879-722">Aktualizowanie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="43879-722">Update the DB context</span></span>

<span data-ttu-id="43879-723">Dodaj następujący wyróżniony kod do *danych/SchoolContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="43879-723">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="43879-724">Poprzedni kod dodaje nowe jednostki i konfiguruje `CourseAssignment` złożonego klucza podstawowego jednostki.</span><span class="sxs-lookup"><span data-stu-id="43879-724">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="43879-725">Alternatywny interfejs API Fluent dla atrybutów</span><span class="sxs-lookup"><span data-stu-id="43879-725">Fluent API alternative to attributes</span></span>

<span data-ttu-id="43879-726">Metoda w poprzednim kodzie używa *interfejsu API Fluent* , aby skonfigurować zachowanie EF Core. `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="43879-726">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="43879-727">Interfejs API jest nazywany "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod w jednej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="43879-727">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="43879-728">[Poniższy kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) jest przykładem interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="43879-728">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="43879-729">W tym samouczku interfejs API Fluent jest używany tylko w przypadku mapowania bazy danych, której nie można wykonać przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="43879-729">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="43879-730">Jednak interfejs API Fluent może określić większość reguł formatowania, walidacji i mapowania, które mogą być wykonywane przy użyciu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="43879-730">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="43879-731">Niektórych atrybutów, takich `MinimumLength` jak nie można zastosować w interfejsie API Fluent.</span><span class="sxs-lookup"><span data-stu-id="43879-731">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="43879-732">`MinimumLength`nie zmienia schematu, stosuje tylko regułę walidacji o minimalnej długości.</span><span class="sxs-lookup"><span data-stu-id="43879-732">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="43879-733">Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-733">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="43879-734">Atrybuty i interfejs API Fluent mogą być mieszane.</span><span class="sxs-lookup"><span data-stu-id="43879-734">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="43879-735">Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu interfejsu API Fluent (określenie złożonego klucza podstawowego).</span><span class="sxs-lookup"><span data-stu-id="43879-735">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="43879-736">Istnieją pewne konfiguracje, które można wykonać tylko z atrybutami (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="43879-736">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="43879-737">Zalecane rozwiązanie dotyczące korzystania z interfejsu API Fluent lub atrybutów:</span><span class="sxs-lookup"><span data-stu-id="43879-737">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="43879-738">Wybierz jedną z tych dwóch metod.</span><span class="sxs-lookup"><span data-stu-id="43879-738">Choose one of these two approaches.</span></span>
* <span data-ttu-id="43879-739">W miarę możliwości należy stosować wybrane podejście.</span><span class="sxs-lookup"><span data-stu-id="43879-739">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="43879-740">Niektóre z atrybutów używanych w tym samouczku są używane w programie:</span><span class="sxs-lookup"><span data-stu-id="43879-740">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="43879-741">Tylko Walidacja (na przykład `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="43879-741">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="43879-742">Tylko konfiguracja EF Core (na przykład `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="43879-742">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="43879-743">Sprawdzanie poprawności i konfiguracja EF Core (na przykład `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="43879-743">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="43879-744">Aby uzyskać więcej informacji na temat atrybutów a interfejs API Fluent, zobacz [metody konfiguracji](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="43879-744">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="43879-745">Diagram jednostek przedstawiający relacje</span><span class="sxs-lookup"><span data-stu-id="43879-745">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="43879-746">Na poniższej ilustracji przedstawiono diagram, który jest tworzony przez narzędzia Dr PowerShell dla gotowego modelu szkoły.</span><span class="sxs-lookup"><span data-stu-id="43879-746">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="43879-748">Na powyższym diagramie przedstawiono:</span><span class="sxs-lookup"><span data-stu-id="43879-748">The preceding diagram shows:</span></span>

* <span data-ttu-id="43879-749">Kilka linii relacji jeden-do-wielu (od 1 \*do).</span><span class="sxs-lookup"><span data-stu-id="43879-749">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="43879-750">Linia relacji "jeden do zera" (od 1 do 0.. 1) między `Instructor` jednostkami i. `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="43879-750">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="43879-751">Linia relacji zero-lub-jeden-do-wielu (0.. 1 do \*) między `Instructor` obiektami i. `Department`</span><span class="sxs-lookup"><span data-stu-id="43879-751">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="43879-752">Wypełnianie bazy danych danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="43879-752">Seed the DB with Test Data</span></span>

<span data-ttu-id="43879-753">Zaktualizuj kod w *danych/Dbinitializeer. cs*:</span><span class="sxs-lookup"><span data-stu-id="43879-753">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="43879-754">Poprzedni kod zawiera dane inicjatora dla nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-754">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="43879-755">Większość tego kodu tworzy nowe obiekty Entity i ładuje przykładowe dane.</span><span class="sxs-lookup"><span data-stu-id="43879-755">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="43879-756">Przykładowe dane służą do testowania.</span><span class="sxs-lookup"><span data-stu-id="43879-756">The sample data is used for testing.</span></span> <span data-ttu-id="43879-757">Zobacz `Enrollments` i `CourseAssignments` , aby zapoznać się z przykładami tabel sprzężenia wiele-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="43879-757">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="43879-758">Dodawanie migracji</span><span class="sxs-lookup"><span data-stu-id="43879-758">Add a migration</span></span>

<span data-ttu-id="43879-759">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="43879-759">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-760">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-760">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-761">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-761">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="43879-762">Poprzednie polecenie wyświetla ostrzeżenie o możliwej utracie danych.</span><span class="sxs-lookup"><span data-stu-id="43879-762">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="43879-763">`database update` Jeśli polecenie zostanie uruchomione, zostanie utworzony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="43879-763">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="43879-764">Zastosuj migrację</span><span class="sxs-lookup"><span data-stu-id="43879-764">Apply the migration</span></span>

<span data-ttu-id="43879-765">Teraz, gdy masz już istniejącą bazę danych, musisz się zastanowić, jak zastosować w niej przyszłe zmiany.</span><span class="sxs-lookup"><span data-stu-id="43879-765">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="43879-766">Ten samouczek przedstawia dwa podejścia:</span><span class="sxs-lookup"><span data-stu-id="43879-766">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="43879-767">Porzuć i ponownie utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="43879-767">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="43879-768">[Zastosuj migrację do istniejącej bazy danych](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="43879-768">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="43879-769">Chociaż ta metoda jest bardziej złożona i czasochłonna, jest preferowanym podejściem dla rzeczywistych środowisk produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="43879-769">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="43879-770">**Uwaga**: Jest to opcjonalna sekcja samouczka.</span><span class="sxs-lookup"><span data-stu-id="43879-770">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="43879-771">Możesz wykonać kroki porzucenia i utworzyć ponownie i pominąć tę sekcję.</span><span class="sxs-lookup"><span data-stu-id="43879-771">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="43879-772">Jeśli chcesz wykonać kroki opisane w tej sekcji, nie wykonuj kroków usuwania i ponownego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="43879-772">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="43879-773">Porzuć i ponownie utwórz bazę danych</span><span class="sxs-lookup"><span data-stu-id="43879-773">Drop and re-create the database</span></span>

<span data-ttu-id="43879-774">Kod w zaktualizowanych `DbInitializer` dodaje dane inicjatora dla nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="43879-774">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="43879-775">Aby wymusić EF Core tworzenia nowej bazy danych, Porzuć i zaktualizuj bazę danych:</span><span class="sxs-lookup"><span data-stu-id="43879-775">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43879-776">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43879-776">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43879-777">W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43879-777">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="43879-778">Uruchom `Get-Help about_EntityFrameworkCore` z PMC, aby uzyskać informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="43879-778">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43879-779">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43879-779">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="43879-780">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="43879-780">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="43879-781">Folder projektu zawiera plik *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="43879-781">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="43879-782">Wprowadź następujące polecenie w oknie polecenia:</span><span class="sxs-lookup"><span data-stu-id="43879-782">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

<span data-ttu-id="43879-783">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="43879-783">Run the app.</span></span> <span data-ttu-id="43879-784">Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę.</span><span class="sxs-lookup"><span data-stu-id="43879-784">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="43879-785">`DbInitializer.Initialize` Wypełnia nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43879-785">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="43879-786">Otwórz bazę danych w programie SSOX:</span><span class="sxs-lookup"><span data-stu-id="43879-786">Open the DB in SSOX:</span></span>

* <span data-ttu-id="43879-787">Jeśli SSOX został wcześniej otwarty, kliknij przycisk **Odśwież** .</span><span class="sxs-lookup"><span data-stu-id="43879-787">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="43879-788">Rozwiń **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="43879-788">Expand the **Tables** node.</span></span> <span data-ttu-id="43879-789">Zostaną wyświetlone utworzone tabele.</span><span class="sxs-lookup"><span data-stu-id="43879-789">The created tables are displayed.</span></span>

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="43879-791">Zapoznaj się z tabelą **CourseAssignment** :</span><span class="sxs-lookup"><span data-stu-id="43879-791">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="43879-792">Kliknij prawym przyciskiem myszy tabelę **CourseAssignment** , a następnie wybierz polecenie **Wyświetl dane**.</span><span class="sxs-lookup"><span data-stu-id="43879-792">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="43879-793">Sprawdź, czy tabela **CourseAssignment** zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="43879-793">Verify the **CourseAssignment** table contains data.</span></span>

![CourseAssignment dane w SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="43879-795">Zastosowanie migracji do istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="43879-795">Apply the migration to the existing database</span></span>

<span data-ttu-id="43879-796">Ta sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="43879-796">This section is optional.</span></span> <span data-ttu-id="43879-797">Kroki te działają tylko wtedy, gdy pominięto poprzednią [listę i ponownie utworzysz sekcję bazy danych](#drop) .</span><span class="sxs-lookup"><span data-stu-id="43879-797">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="43879-798">Po uruchomieniu migracji z istniejącymi danymi mogą istnieć ograniczenia klucza obcego, które nie są spełnione w przypadku istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="43879-798">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="43879-799">W przypadku danych produkcyjnych należy wykonać kroki, aby przeprowadzić migrację istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="43879-799">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="43879-800">Ta sekcja zawiera przykład rozwiązywania naruszeń ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="43879-800">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="43879-801">Nie należy wprowadzać tych zmian w kodzie bez tworzenia kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="43879-801">Don't make these code changes without a backup.</span></span> <span data-ttu-id="43879-802">Nie wprowadzaj tych zmian w kodzie, jeśli wykonano poprzednią sekcję i Zaktualizowano bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43879-802">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="43879-803">Plik *{timestamp} _ComplexDataModel. cs* zawiera następujący kod:</span><span class="sxs-lookup"><span data-stu-id="43879-803">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="43879-804">Poprzedzający kod dodaje `DepartmentID` `Course` do tabeli obcy niedopuszczający wartości null.</span><span class="sxs-lookup"><span data-stu-id="43879-804">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="43879-805">Baza danych z poprzedniego samouczka zawiera wiersze w `Course`, dlatego nie można zaktualizować tabeli za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="43879-805">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="43879-806">Aby `ComplexDataModel` migracja była współdziałać z istniejącymi danymi:</span><span class="sxs-lookup"><span data-stu-id="43879-806">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="43879-807">Zmień kod, aby nadać nowej kolumnie (`DepartmentID`) wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="43879-807">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="43879-808">Utwórz fałszywy Departament o nazwie "Temp", który ma pełnić rolę działu domyślnego.</span><span class="sxs-lookup"><span data-stu-id="43879-808">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="43879-809">Popraw ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="43879-809">Fix the foreign key constraints</span></span>

<span data-ttu-id="43879-810">Zaktualizuj metodę `Up`klasy: `ComplexDataModel`</span><span class="sxs-lookup"><span data-stu-id="43879-810">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="43879-811">Otwórz plik *{timestamp} _ComplexDataModel. cs* .</span><span class="sxs-lookup"><span data-stu-id="43879-811">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="43879-812">Dodaj komentarz `DepartmentID` `Course` do wiersza kodu, który dodaje kolumnę do tabeli.</span><span class="sxs-lookup"><span data-stu-id="43879-812">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="43879-813">Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="43879-813">Add the following highlighted code.</span></span> <span data-ttu-id="43879-814">Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku:</span><span class="sxs-lookup"><span data-stu-id="43879-814">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="43879-815">Po wykonaniu powyższych zmian istniejące `Course` wiersze będą powiązane z działem "Temp" po uruchomieniumetody.`Up` `ComplexDataModel`</span><span class="sxs-lookup"><span data-stu-id="43879-815">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="43879-816">Aplikacja produkcyjna:</span><span class="sxs-lookup"><span data-stu-id="43879-816">A production app would:</span></span>

* <span data-ttu-id="43879-817">Dołącz kod lub skrypty, aby `Department` dodać wiersze i `Course` powiązane wiersze do nowych `Department` wierszy.</span><span class="sxs-lookup"><span data-stu-id="43879-817">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="43879-818">Nie używaj działu "Temp" ani wartości domyślnej dla `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="43879-818">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="43879-819">Następny samouczek obejmuje powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="43879-819">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43879-820">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="43879-820">Additional resources</span></span>

* [<span data-ttu-id="43879-821">Wersja usługi YouTube w tym samouczku (część 1)</span><span class="sxs-lookup"><span data-stu-id="43879-821">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="43879-822">Wersja usługi YouTube w tym samouczku (część 2)</span><span class="sxs-lookup"><span data-stu-id="43879-822">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)



> [!div class="step-by-step"]
> <span data-ttu-id="43879-823">[Poprzedni](xref:data/ef-rp/migrations)Następny
> [](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="43879-823">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end