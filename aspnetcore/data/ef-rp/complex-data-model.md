---
title: Strony razor z programem EF Core w programie ASP.NET Core — Model danych — 5 8
author: rick-anderson
description: W tym samouczku należy dodać większą liczbę jednostek i relacji i Dostosuj model danych, określając formatowania i sprawdzania poprawności i reguł mapowania.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 1dc9f1278e502cd5040e82c18d99e2da6f139568
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410589"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="4ab2f-103">Strony razor z programem EF Core w programie ASP.NET Core — Model danych — 5 8</span><span class="sxs-lookup"><span data-stu-id="4ab2f-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4ab2f-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ab2f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="4ab2f-105">Poprzednich samouczków pracy z modelem danych podstawowych, który został składające się z trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="4ab2f-106">W tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-106">In this tutorial:</span></span>

* <span data-ttu-id="4ab2f-107">Więcej jednostek i relacji są dodawane.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="4ab2f-108">Model danych jest dostosowane, określając formatowania i sprawdzania poprawności i reguł mapowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="4ab2f-109">Na poniższej ilustracji pokazano klas jednostek dla modelu danych zakończone:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="4ab2f-111">Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-111">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="4ab2f-112">Dostosuj model danych za pomocą atrybutów</span><span class="sxs-lookup"><span data-stu-id="4ab2f-112">Customize the data model with attributes</span></span>

<span data-ttu-id="4ab2f-113">W tej sekcji model danych został dostosowany, za pomocą atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="4ab2f-114">Atrybut typu danych</span><span class="sxs-lookup"><span data-stu-id="4ab2f-114">The DataType attribute</span></span>

<span data-ttu-id="4ab2f-115">Na stronach dla uczniów obecnie Wyświetla czas Data rejestracji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="4ab2f-116">Zazwyczaj Data polach wskaźnika myszy wyświetlane tylko datę, a nie czasu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="4ab2f-117">Aktualizacja *Models/Student.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="4ab2f-118">[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atrybut określa typ danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="4ab2f-119">W tym przypadku tylko data powinna być wyświetlana, nie daty i godziny.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="4ab2f-120">[Wyliczenie typu danych](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) udostępnia wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress itp. `DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="4ab2f-121">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-121">For example:</span></span>

* <span data-ttu-id="4ab2f-122">`mailto:` Łącze jest tworzona automatycznie dla `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="4ab2f-123">Selektor daty towarzyszy `DataType.Date` w większości przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="4ab2f-124">`DataType` Atrybut emituje HTML 5 `data-` atrybutów (Wymowa: dane dash), korzystających z przeglądarki HTML 5.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="4ab2f-125">`DataType` Atrybutów nie zapewniają weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="4ab2f-126">`DataType.Date` nie określa format daty, która jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="4ab2f-127">Domyślnie pole daty są wyświetlane domyślne formaty oparte na tym serwerze [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="4ab2f-128">`DisplayFormat` Atrybut jest używany jawnie określić format daty:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="4ab2f-129">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinien również będą stosowane do edycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="4ab2f-130">Niektóre pola nie używaj `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="4ab2f-131">Na przykład symbol waluty ogólnie nie powinien być wyświetlany w polu edycji tekstu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="4ab2f-132">`DisplayFormat` Atrybut może być używany przez siebie.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="4ab2f-133">Zazwyczaj jest to dobry pomysł, aby użyć `DataType` atrybutem `DisplayFormat` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="4ab2f-134">`DataType` Atrybutu powoduje semantykę dane, a nie jak renderować ją na ekranie.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="4ab2f-135">`DataType` Atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="4ab2f-136">Przeglądarka można włączyć funkcje HTML5.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="4ab2f-137">Na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy, sprawdzania poprawności danych wejściowych po stronie klienta, itp.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="4ab2f-138">Domyślnie przeglądarka Renderowanie danych przy użyciu poprawny format, w oparciu o ustawienia regionalne.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="4ab2f-139">Aby uzyskać więcej informacji, zobacz [ \<wejściowych > dokumentacja Pomocnik tagu](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="4ab2f-140">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-140">Run the app.</span></span> <span data-ttu-id="4ab2f-141">Przejdź do strony indeksu studentów.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="4ab2f-142">Nie są wyświetlane godziny.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-142">Times are no longer displayed.</span></span> <span data-ttu-id="4ab2f-143">Każdy widok, który używa `Student` modelu umożliwia wyświetlenie daty bez godziny.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Wyświetlanie daty bez godziny strony indeksu uczniów](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="4ab2f-145">Atrybut StringLength</span><span class="sxs-lookup"><span data-stu-id="4ab2f-145">The StringLength attribute</span></span>

<span data-ttu-id="4ab2f-146">Reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji można określić za pomocą atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="4ab2f-147">[StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atrybut określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="4ab2f-148">`StringLength` Atrybut udostępnia również weryfikacji po stronie klienta i po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="4ab2f-149">Wartość minimalna nie ma wpływu na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="4ab2f-150">Aktualizacja `Student` modelu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="4ab2f-151">Powyższy kod ogranicza nazwy do nie więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="4ab2f-152">`StringLength` Atrybutu nie uniemożliwia użytkownikowi wprowadzanie białe miejsca dla nazwy.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="4ab2f-153">[Wyrażenia regularnego](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atrybut jest używany, aby zastosować ograniczenia danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="4ab2f-154">Na przykład poniższy kod wymaga pierwszy znak na wielkie litery, a pozostałe znaki, aby być znakiem alfabetycznym:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="4ab2f-155">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-155">Run the app:</span></span>

* <span data-ttu-id="4ab2f-156">Przejdź do strony studentów.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="4ab2f-157">Wybierz **Utwórz nowy**, a następnie wprowadź nazwę więcej niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="4ab2f-158">Wybierz **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-158">Select **Create**, client-side validation shows an error message.</span></span>

![Studenci indeksu strona wyświetlająca błędy długość ciągu](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="4ab2f-160">W **Eksplorator obiektów SQL Server** (SSOX), otwórz projektanta tabel dla uczniów, klikając dwukrotnie **uczniów** tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela studentów w SSOX przed migracji](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="4ab2f-162">Na powyższej ilustracji pokazano schematu dla `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="4ab2f-163">Nazwa pola, które mają typ `nvarchar(MAX)` ponieważ migracji nie został uruchomiony dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="4ab2f-164">Podczas migracji są uruchamiane w dalszej części tego samouczka, nazwy pola stają się `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="4ab2f-165">Atrybut kolumny</span><span class="sxs-lookup"><span data-stu-id="4ab2f-165">The Column attribute</span></span>

<span data-ttu-id="4ab2f-166">Atrybuty można kontrolować, jak klasy i właściwości są mapowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="4ab2f-167">W tej sekcji `Column` atrybut jest używany do mapowania nazwy `FirstMidName` właściwość na "FirstName" w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="4ab2f-168">Po utworzeniu bazy danych, nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem kiedy `Column` atrybut jest używany).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="4ab2f-169">`Student` Model używa `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać imienia.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="4ab2f-170">Aktualizacja *Student.cs* pliku następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="4ab2f-171">Z powyższej zmiany `Student.FirstMidName` w aplikacji mapuje `FirstName` kolumny `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="4ab2f-172">Dodanie `Column` atrybut zmieni się zapasowy modelu `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="4ab2f-173">Zapasowy modelu `SchoolContext` nie jest już zgodny z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="4ab2f-174">Jeśli aplikacja jest uruchamiana przed zastosowaniem migracje, generowany jest następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="4ab2f-175">Aby zaktualizować bazy danych:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-175">To update the DB:</span></span>

* <span data-ttu-id="4ab2f-176">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-176">Build the project.</span></span>
* <span data-ttu-id="4ab2f-177">Otwórz okno polecenia w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-177">Open a command window in the project folder.</span></span> <span data-ttu-id="4ab2f-178">Wprowadź następujące polecenia, aby utworzyć nową migrację i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-178">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ab2f-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ab2f-179">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ab2f-180">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ab2f-180">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

<span data-ttu-id="4ab2f-181">`migrations add ColumnFirstName` Polecenie generuje następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-181">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="4ab2f-182">Ostrzeżenia jest generowany, ponieważ nazwa pola, które są obecnie ograniczone do 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-182">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="4ab2f-183">Jeśli nazwa bazy danych — w więcej niż 50 znaków, 51 do ostatniego znaku zostałyby utracone.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-183">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="4ab2f-184">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-184">Test the app.</span></span>

<span data-ttu-id="4ab2f-185">Otwórz Tabela Student SSOX:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-185">Open the Student table in SSOX:</span></span>

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="4ab2f-187">Przed zastosowaniem migracji, nazwa kolumny były typu [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-187">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="4ab2f-188">Nazwa kolumny są teraz `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-188">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="4ab2f-189">Nazwa kolumny został zmieniony z `FirstMidName` do `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-189">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="4ab2f-190">W poniższej sekcji Kompilowanie aplikacji na niektórych etapach generuje błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-190">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="4ab2f-191">Instrukcje Określ, kiedy do skompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-191">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="4ab2f-192">Aktualizacja jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="4ab2f-192">Student entity update</span></span>

![Jednostki dla uczniów](complex-data-model/_static/student-entity.png)

<span data-ttu-id="4ab2f-194">Aktualizacja *Models/Student.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-194">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="4ab2f-195">Wymagany atrybut</span><span class="sxs-lookup"><span data-stu-id="4ab2f-195">The Required attribute</span></span>

<span data-ttu-id="4ab2f-196">`Required` Atrybutu sprawia, że nazwa właściwości wymagane pola.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-196">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="4ab2f-197">`Required` Atrybut nie jest wymagane dla typów innych niż null, takich jak typy wartości (`DateTime`, `int`, `double`itp.).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-197">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="4ab2f-198">Typy, które nie może mieć wartości null są automatycznie traktowane jako wymagane pola.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-198">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="4ab2f-199">`Required` Atrybut może zostać zastąpione minimalną długość parametru w `StringLength` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-199">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="4ab2f-200">Atrybut wyświetlania</span><span class="sxs-lookup"><span data-stu-id="4ab2f-200">The Display attribute</span></span>

<span data-ttu-id="4ab2f-201">`Display` Atrybut określa, że podpis dla pól tekstowych powinien być "Imię", "Last Name", "Pełna nazwa" i "Data rejestracji".</span><span class="sxs-lookup"><span data-stu-id="4ab2f-201">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="4ab2f-202">Podpisy domyślna ma już miejsca dzielenia wyrazów, na przykład "Lastname".</span><span class="sxs-lookup"><span data-stu-id="4ab2f-202">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="4ab2f-203">Właściwości obliczane imię i nazwisko</span><span class="sxs-lookup"><span data-stu-id="4ab2f-203">The FullName calculated property</span></span>

<span data-ttu-id="4ab2f-204">`FullName` jest właściwością obliczeniową, która zwraca wartość, która jest tworzona przez dołączenie dwóch innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-204">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="4ab2f-205">`FullName` Nie można ustawić, ma tylko akcesor pobierania.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-205">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="4ab2f-206">Nie `FullName` kolumna jest tworzona w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-206">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="4ab2f-207">Tworzenie jednostki przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="4ab2f-207">Create the Instructor Entity</span></span>

![Jednostki przez instruktorów](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="4ab2f-209">Tworzenie *Models/Instructor.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-209">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="4ab2f-210">Wiele atrybutów może być w jednym wierszu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="4ab2f-211">`HireDate` Atrybuty, które mogłyby być zapisywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="4ab2f-212">Właściwości nawigacji CourseAssignments i OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="4ab2f-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="4ab2f-213">`CourseAssignments` i `OfficeAssignment` właściwości są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="4ab2f-214">Pod kierunkiem instruktora nauczyć dowolnej liczby kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="4ab2f-215">Jeśli właściwość nawigacji posiada wiele jednostek:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="4ab2f-216">Musi być typu listy, gdzie wpisy mogą być dodawane, usuwane lub zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="4ab2f-217">Typy właściwości nawigacji:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="4ab2f-218">Jeśli `ICollection<T>` jest określona, tworzy programu EF Core `HashSet<T>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="4ab2f-219">`CourseAssignment` Jednostki zostało wyjaśnione w sekcji w relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="4ab2f-220">Firmy Contoso University reguł stanu, że pod kierunkiem instruktora może mieć co najwyżej jednego pakietu office.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="4ab2f-221">`OfficeAssignment` Właściwość przechowuje pojedynczej `OfficeAssignment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="4ab2f-222">`OfficeAssignment` ma wartość null, jeśli nie przypisano żadnych pakietu office.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="4ab2f-223">Tworzenie jednostki OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="4ab2f-223">Create the OfficeAssignment entity</span></span>

![OfficeAssignment jednostki](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="4ab2f-225">Tworzenie *Models/OfficeAssignment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="4ab2f-226">Atrybut klucza</span><span class="sxs-lookup"><span data-stu-id="4ab2f-226">The Key attribute</span></span>

<span data-ttu-id="4ab2f-227">`[Key]` Atrybutu służy do identyfikowania właściwość jako kluczowi podstawowemu (PK) gdy nazwa właściwości jest coś innego niż classnameID lub identyfikatora organizacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="4ab2f-228">Brak relacji jeden do zero lub jeden między `Instructor` i `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="4ab2f-229">Biuro istnieje tylko w odniesieniu do przez instruktorów, do którego jest przypisany.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="4ab2f-230">`OfficeAssignment` Klucz podstawowy jest również jej klucz obcy (klucz OBCY) `Instructor` jednostki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="4ab2f-231">EF Core automatycznie nie może rozpoznać `InstructorID` jako klucz podstawowy z `OfficeAssignment` ponieważ:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="4ab2f-232">`InstructorID` nie należy wykonać identyfikator lub classnameID konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="4ab2f-233">W związku z tym `Key` atrybut jest używany do identyfikowania `InstructorID` jako klucz podstawowy:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="4ab2f-234">Domyślnie EF Core traktuje klucza jako inne niż wygenerowane bazy danych, ponieważ kolumna jest przeznaczona dla relacji identyfikującej.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="4ab2f-235">Właściwość nawigacji przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="4ab2f-235">The Instructor navigation property</span></span>

<span data-ttu-id="4ab2f-236">`OfficeAssignment` Właściwości nawigacji dla `Instructor` jednostki ma wartość null ponieważ:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="4ab2f-237">Typów referencyjnych (takie jak klasy dopuszczają wartości null).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="4ab2f-238">Pod kierunkiem instruktora, może nie mieć biuro.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="4ab2f-239">`OfficeAssignment` Jednostka ma wartość null `Instructor` właściwość nawigacji ponieważ:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="4ab2f-240">`InstructorID` nie dopuszcza wartości.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="4ab2f-241">Przypisanie pakietu office nie może istnieć bez pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="4ab2f-242">Gdy `Instructor` jednostka ma powiązane `OfficeAssignment` jednostki, każdy obiekt odwołuje się do innych niż w jego właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="4ab2f-243">`[Required]` Można zastosować atrybutu do `Instructor` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="4ab2f-244">Powyższy kod określa, że musi być powiązany instruktora.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="4ab2f-245">Powyższy kod nie jest konieczne ponieważ `InstructorID` nie dopuszczają wartości klucza obcego (jest to również PK).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="4ab2f-246">Modyfikowanie jednostek kursu</span><span class="sxs-lookup"><span data-stu-id="4ab2f-246">Modify the Course Entity</span></span>

![Kurs jednostki](complex-data-model/_static/course-entity.png)

<span data-ttu-id="4ab2f-248">Aktualizacja *Models/Course.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="4ab2f-249">`Course` Jednostka ma właściwość klucza obcego (klucz OBCY) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="4ab2f-250">`DepartmentID` Wskazuje powiązane `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="4ab2f-251">`Course` Jednostka ma `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="4ab2f-252">EF Core nie wymaga właściwości klucza Obcego dla modelu danych, gdy model nie ma właściwości nawigacji dla powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="4ab2f-253">EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie będą one potrzebne.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="4ab2f-254">Tworzy programu EF Core [w tle właściwości](/ef/core/modeling/shadow-properties) dla FKs utworzone automatycznie.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-254">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="4ab2f-255">Posiadanie klucza Obcego w modelu danych, że aktualizacje są prostszy i efektywniejszy.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="4ab2f-256">Na przykład, należy wziąć pod uwagę modelu gdzie właściwości klucza Obcego `DepartmentID` jest *nie* uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="4ab2f-257">Gdy jednostka kurs jest pobierana do edycji:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="4ab2f-258">`Department` Jednostki ma wartość null, jeśli nie zostały jawnie jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="4ab2f-259">Do zaktualizowania jednostki kurs `Department` należy najpierw pobrać jednostki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="4ab2f-260">Gdy właściwość klucza Obcego `DepartmentID` znajduje się w modelu danych nie ma potrzeby można pobrać `Department` jednostki przed aktualizacją.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="4ab2f-261">Atrybut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="4ab2f-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="4ab2f-262">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Atrybut określa, że klucz podstawowy jest udostępniany przez aplikację, a nie generowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="4ab2f-263">Domyślnie EF Core przyjęto założenie, że wartości klucza produktu są generowane przez bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="4ab2f-264">DB wygenerowany klucz podstawowy wartościami ogólnie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="4ab2f-265">Aby uzyskać `Course` jednostek, użytkownik określa PK.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="4ab2f-266">Na przykład liczba kurs takich jak seria 1000 dla działu matematycznych, serię 2000 dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="4ab2f-267">`DatabaseGenerated` Atrybut może służyć także do generowania wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="4ab2f-268">Na przykład baza danych może automatycznie generować pole daty, aby zarejestrować Data wiersz został utworzony lub zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="4ab2f-269">Aby uzyskać więcej informacji, zobacz [wygenerowanych właściwości](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-269">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4ab2f-270">Obcy właściwości klucza i nawigacja</span><span class="sxs-lookup"><span data-stu-id="4ab2f-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="4ab2f-271">Właściwości klucza obcego (klucz OBCY) i właściwości nawigacji w `Course` jednostki odzwierciedlają się następująco:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="4ab2f-272">Kurs jest przypisany do jednego działu, więc ma `DepartmentID` klucza Obcego i `Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="4ab2f-273">Kurs może mieć dowolną liczbę uczniów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="4ab2f-274">Kurs może być prowadzone przez instruktorów wielu, więc `CourseAssignments` właściwość nawigacji jest kolekcją:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="4ab2f-275">`CourseAssignment` objaśniono [później](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="4ab2f-276">Tworzenie jednostki działu</span><span class="sxs-lookup"><span data-stu-id="4ab2f-276">Create the Department entity</span></span>

![Dział jednostki](complex-data-model/_static/department-entity.png)

<span data-ttu-id="4ab2f-278">Tworzenie *Models/Department.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="4ab2f-279">Atrybut kolumny</span><span class="sxs-lookup"><span data-stu-id="4ab2f-279">The Column attribute</span></span>

<span data-ttu-id="4ab2f-280">Wcześniej `Column` użyto atrybutu można zmienić mapowania nazwy kolumny.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="4ab2f-281">W kodzie dla `Department` jednostki `Column` atrybut jest używany, aby zmienić mapowanie typu danych SQL.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="4ab2f-282">`Budget` Kolumna jest definiowana za pomocą typu pieniądze programu SQL Server w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="4ab2f-283">Mapowanie kolumn zazwyczaj nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-283">Column mapping is generally not required.</span></span> <span data-ttu-id="4ab2f-284">EF Core zazwyczaj wybiera się odpowiedni typ danych programu SQL Server, na podstawie typu CLR dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="4ab2f-285">Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="4ab2f-286">`Budget` dotyczy waluty, a typ danych pieniądze są bardziej odpowiednie dla waluty.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4ab2f-287">Obcy właściwości klucza i nawigacja</span><span class="sxs-lookup"><span data-stu-id="4ab2f-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="4ab2f-288">Właściwości klucza Obcego i nawigacyjny odzwierciedla następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="4ab2f-289">Dział mogą być lub może nie mieć uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="4ab2f-290">Administrator jest zawsze pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-290">An administrator is always an instructor.</span></span> <span data-ttu-id="4ab2f-291">W związku z tym `InstructorID` właściwość jest uwzględniona jako klucza Obcego do `Instructor` jednostki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="4ab2f-292">Właściwość nawigacji jest o nazwie `Administrator` , ale przechowuje `Instructor` jednostki:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="4ab2f-293">Znak zapytania (?) w poprzednim kodzie Określa, że właściwość ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="4ab2f-294">Dział może mieć wielu kursów, więc ma właściwości nawigacji kursy:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="4ab2f-295">Uwaga: Zgodnie z Konwencją programu EF Core umożliwia usuwanie kaskadowe dopuszcza FKs oraz relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="4ab2f-296">Usuwanie kaskadowe może spowodować cykliczne cascade delete reguły.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="4ab2f-297">Cykliczne usuwanie kaskadowe reguły powoduje, że wyjątek po dodaniu migracji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="4ab2f-298">Na przykład jeśli `Department.InstructorID` właściwości nie został zdefiniowany jako dopuszczający wartość null:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="4ab2f-299">EF Core konfiguruje regułę usuwanie kaskadowe można usunąć instruktora, po usunięciu działu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="4ab2f-300">Usuwanie instruktora, po usunięciu działu nie jest to oczekiwane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="4ab2f-301">W razie potrzeby reguły biznesowe `InstructorID` właściwości nie dopuszcza, użyj następującej instrukcji fluent API:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="4ab2f-302">Powyższy kod wyłącza usuwanie kaskadowe relacji przez instruktorów działu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="4ab2f-303">Aktualizacja jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="4ab2f-303">Update the Enrollment entity</span></span>

<span data-ttu-id="4ab2f-304">Rekord rejestracji dotyczy jednego kursu podjęte przez jeden dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-304">An enrollment record is for one course taken by one student.</span></span>

![Jednostki rejestracji](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="4ab2f-306">Aktualizacja *Models/Enrollment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4ab2f-307">Obcy właściwości klucza i nawigacja</span><span class="sxs-lookup"><span data-stu-id="4ab2f-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="4ab2f-308">Właściwości klucza Obcego i właściwości nawigacji odzwierciedla się następująco:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="4ab2f-309">Rekord rejestracji dotyczy jednego kursu, więc ma `CourseID` właściwości klucza Obcego i `Course` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="4ab2f-310">Rekord rejestracji jest dla jednego uczniów lub studentów, więc ma `StudentID` właściwości klucza Obcego i `Student` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="4ab2f-311">Relacje wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="4ab2f-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="4ab2f-312">Istnieje relacja wiele do wielu między `Student` i `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="4ab2f-313">`Enrollment` Jednostki działa jako tabelę sprzężenia wiele do wielu *z ładunkiem* w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="4ab2f-314">"Z ładunkiem" oznacza, że `Enrollment` tabela zawiera dane dodatkowe, oprócz FKs dla połączonych tabel (w tym przypadku PK i `Grade`).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="4ab2f-315">Poniższa ilustracja przedstawia, jak wyglądają te relacje w diagramie jednostki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="4ab2f-316">(Ten diagram został wygenerowany za pomocą [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) na platformie EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-316">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="4ab2f-317">Tworzenie diagramów nie jest część samouczka).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Kurs dla uczniów wiele do wielu relacji](complex-data-model/_static/student-course.png)

<span data-ttu-id="4ab2f-319">Każdy wiersz relacji ma 1 na jednym końcu i znak gwiazdki (\*) na drugim, wskazując relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="4ab2f-320">Jeśli `Enrollment` tabeli nie włączono informacji klasy korporacyjnej, czy tylko musi on zawierać dwa FKs (`CourseID` i `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="4ab2f-321">Tabelę sprzężenia wiele do wielu, bez ładunku jest czasami nazywane tabelę sprzężenia czystego (PJT).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="4ab2f-322">`Instructor` i `Course` jednostki mają relacji wiele do wielu, przy użyciu czystego sprzężenie tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="4ab2f-323">Uwaga: Niejawne sprzężenie tabel dla relacji wiele do wielu, ale programu EF Core nie obsługuje 6.x EF.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="4ab2f-324">Aby uzyskać więcej informacji, zobacz [wiele do wielu relacji w programie EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="4ab2f-325">Jednostka CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="4ab2f-325">The CourseAssignment entity</span></span>

![CourseAssignment jednostki](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="4ab2f-327">Tworzenie *Models/CourseAssignment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="4ab2f-328">Przez instruktorów do kursów</span><span class="sxs-lookup"><span data-stu-id="4ab2f-328">Instructor-to-Courses</span></span>

![M:M przez instruktorów do kursów](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="4ab2f-330">Relacja wiele do wielu przez instruktorów na kursy:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="4ab2f-331">Wymaga tabelę sprzężenia, która musi być reprezentowana przez zestaw jednostek.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="4ab2f-332">Jest czysty sprzężenie tabeli (tabeli bez ładunku).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="4ab2f-333">Często jest nazwa jednostki sprzężenia `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="4ab2f-334">Na przykład tabela sprzężenia instruktora do kursów, za pomocą tego wzorca jest `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="4ab2f-335">Firma Microsoft zaleca jednak przy użyciu nazwy, która opisuje relację.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="4ab2f-336">Modele danych zaczynają prosty i rozwój.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-336">Data models start out simple and grow.</span></span> <span data-ttu-id="4ab2f-337">Ładunek nie sprzężenia (PJTs) często ewolucji obejmujący ładunku.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="4ab2f-338">Począwszy od jednostki opisową nazwę, nazwę nie trzeba zmienić w przypadku zmiany tabeli sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="4ab2f-339">W idealnym przypadku jednostki sprzężenia będzie mieć własną fizyczną nazwę (prawdopodobnie jednowyrazowego) w domenie firmy.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="4ab2f-340">Na przykład książek i klientów można połączyć z jednostką sprzężenia o nazwie klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="4ab2f-341">Dla relacji wiele do wielu instruktora do kursów `CourseAssignment` jest preferowana nad `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="4ab2f-342">Klucz złożony</span><span class="sxs-lookup"><span data-stu-id="4ab2f-342">Composite key</span></span>

<span data-ttu-id="4ab2f-343">Nie dopuszczają FKs.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-343">FKs are not nullable.</span></span> <span data-ttu-id="4ab2f-344">Dwa FKs w `CourseAssignment` (`InstructorID` i `CourseID`) razem jednoznacznie identyfikują poszczególne wiersze z `CourseAssignment` tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="4ab2f-345">`CourseAssignment` nie wymaga dedykowanego PK.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="4ab2f-346">`InstructorID` i `CourseID` właściwości pełnią PK. złożone</span><span class="sxs-lookup"><span data-stu-id="4ab2f-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="4ab2f-347">Jedynym sposobem, aby określić złożonego PKs do programu EF Core jest *interfejsu API fluent*.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="4ab2f-348">W następnej sekcji pokazano, jak skonfigurować PK. złożonego</span><span class="sxs-lookup"><span data-stu-id="4ab2f-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="4ab2f-349">Klucz złożony zapewnia:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-349">The composite key ensures:</span></span>

* <span data-ttu-id="4ab2f-350">Wiele wierszy są dozwolone dla jednego kursu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="4ab2f-351">Wiele wierszy są dozwolone dla jednego instruktora.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="4ab2f-352">Wiele wierszy dla tego samego przez instruktorów i kurs jest niedozwolona.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="4ab2f-353">`Enrollment` Jednostki sprzężenia definiuje swój własny klucz podstawowy, więc możliwe są duplikatami tego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="4ab2f-354">Aby uniknąć takich duplikaty:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="4ab2f-355">Dodaj unikatowy indeks pól klucza Obcego lub</span><span class="sxs-lookup"><span data-stu-id="4ab2f-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="4ab2f-356">Konfigurowanie `Enrollment` przy użyciu podstawowego klucza złożonego podobne do `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="4ab2f-357">Aby uzyskać więcej informacji, zobacz [indeksów](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-357">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="4ab2f-358">Aktualizuj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="4ab2f-358">Update the DB context</span></span>

<span data-ttu-id="4ab2f-359">Dodaj następujący wyróżniony kod do *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="4ab2f-360">Poprzedzający kod dodaje nowe jednostki i konfiguruje `CourseAssignment` PK. złożonego jednostki</span><span class="sxs-lookup"><span data-stu-id="4ab2f-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="4ab2f-361">Zamiast interfejsu API Fluent atrybutów</span><span class="sxs-lookup"><span data-stu-id="4ab2f-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="4ab2f-362">`OnModelCreating` Metody w poprzednim kodzie używa *interfejsu API fluent* do konfigurowania zachowania programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="4ab2f-363">Interfejs API jest nazywany "fluent", ponieważ jest ona często używana przez centrali szereg wywołań metod w pojedynczej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="4ab2f-364">[Następujący kod](/ef/core/modeling/#methods-of-configuration) jest przykładem wygodnego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-364">The [following code](/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="4ab2f-365">W tym samouczku interfejs fluent API jest używany tylko w przypadku mapowania bazy danych, który nie może odbywać się za pomocą atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="4ab2f-366">Jednak interfejs fluent API można określić większość formatowania, sprawdzanie poprawności i reguł mapowania, które mogą realizować za pomocą atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="4ab2f-367">Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować za pomocą interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="4ab2f-368">`MinimumLength` nie powoduje zmiany schematu, ma zastosowanie tylko minimalną długość reguły weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="4ab2f-369">Niektórzy deweloperzy wolą używać interfejsu API fluent, wyłącznie tak, aby ich zachowania ich klasami jednostki "Wyczyść".</span><span class="sxs-lookup"><span data-stu-id="4ab2f-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="4ab2f-370">Atrybuty i wygodnego interfejsu API mogą być mieszane.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="4ab2f-371">Istnieją pewne konfiguracje, które mogą być przeprowadzone wyłącznie z interfejsu API fluent (określenia złożony klucz podstawowy).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="4ab2f-372">Istnieją pewne konfiguracje, które może być przeprowadzone wyłącznie z atrybutami (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="4ab2f-373">Zalecana praktyka używania fluent API lub atrybutów:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="4ab2f-374">Wybierz jedną z tych dwóch metod.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="4ab2f-375">Użyć wybranej metody spójnie możliwie.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="4ab2f-376">Niektóre atrybuty używane w tym samouczku są używane do:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="4ab2f-377">Sprawdzanie poprawności tylko (na przykład `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="4ab2f-378">EF Core tylko konfiguracji (na przykład `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="4ab2f-379">Sprawdzanie poprawności i programem EF Core konfiguracji (na przykład `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="4ab2f-380">Aby uzyskać więcej informacji na temat atrybutów, a interfejs fluent API zobacz [metod konfiguracji](/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-380">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="4ab2f-381">Jednostki Diagram przedstawiający relacje</span><span class="sxs-lookup"><span data-stu-id="4ab2f-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="4ab2f-382">Poniższa ilustracja przedstawia diagram, który EF Power Tools Tworzenie modelu ukończone szkoły.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagram jednostek](complex-data-model/_static/diagram.png)

<span data-ttu-id="4ab2f-384">Na powyższym diagramie przedstawiono:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="4ab2f-385">Kilka wierszy relacji jeden do wielu (1, aby \*).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="4ab2f-386">Wiersz relacji jeden do zero lub jeden (1 się od 0 do 1) między `Instructor` i `OfficeAssignment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="4ab2f-387">Wiersz relacji zero lub jeden do wielu (od 0 do 1 do \*) między `Instructor` i `Department` jednostek.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="4ab2f-388">Inicjator bazy danych przy użyciu danych testowych</span><span class="sxs-lookup"><span data-stu-id="4ab2f-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="4ab2f-389">Aktualizowanie kodu w *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="4ab2f-390">Powyższy kod udostępnia dane inicjatora dla nowych jednostek.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="4ab2f-391">Większość ten kod tworzy nowe obiekty jednostki i ładowania przykładowych danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="4ab2f-392">Dane przykładowe są używane do testowania.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-392">The sample data is used for testing.</span></span> <span data-ttu-id="4ab2f-393">Zobacz `Enrollments` i `CourseAssignments` dla przykładów jak wiele do wielu Dołączanie tabel może zostać rozpoczęta.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-393">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="4ab2f-394">Dodaj migrację</span><span class="sxs-lookup"><span data-stu-id="4ab2f-394">Add a migration</span></span>

<span data-ttu-id="4ab2f-395">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-395">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ab2f-396">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ab2f-396">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ab2f-397">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ab2f-397">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

<span data-ttu-id="4ab2f-398">Poprzednie polecenie wyświetli ostrzeżenie o możliwej utracie danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="4ab2f-399">Jeśli `database update` polecenie jest wykonywane, generowany jest następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="4ab2f-400">Zastosuj migracji</span><span class="sxs-lookup"><span data-stu-id="4ab2f-400">Apply the migration</span></span>

<span data-ttu-id="4ab2f-401">Teraz, gdy masz istniejącą bazę danych, należy wziąć pod uwagę sposób stosowania przyszłe zmiany do niego.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-401">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="4ab2f-402">W tym samouczku przedstawiono dwie metody:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-402">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="4ab2f-403">Porzuć i ponownie utworzyć bazę danych</span><span class="sxs-lookup"><span data-stu-id="4ab2f-403">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="4ab2f-404">[Dotyczą migracji z istniejącej bazy danych](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="4ab2f-404">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="4ab2f-405">Ta metoda jest bardziej złożony i czasochłonny proces, jest preferowanym podejściem w środowiskach produkcyjnych w rzeczywistych warunkach.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-405">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="4ab2f-406">**Uwaga**: Jest to opcjonalne części samouczka.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-406">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="4ab2f-407">Można zrobić listy i ponownie utwórz kroki i pominąć tę sekcję.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-407">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="4ab2f-408">Jeśli chcesz wykonać kroki zawarte w tej sekcji, nie są listy i ponownie utwórz kroki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-408">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="4ab2f-409">Porzuć i ponownie utworzyć bazę danych</span><span class="sxs-lookup"><span data-stu-id="4ab2f-409">Drop and re-create the database</span></span>

<span data-ttu-id="4ab2f-410">Kod w zaktualizowanej `DbInitializer` dodaje dane nowe jednostki.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-410">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="4ab2f-411">Aby wymusić programu EF Core do utworzenia nowej bazy danych, Porzuć i aktualizowanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-411">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ab2f-412">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ab2f-412">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4ab2f-413">W **Konsola Menedżera pakietów** (PMC), uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-413">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="4ab2f-414">Uruchom `Get-Help about_EntityFrameworkCore` z konsoli zarządzania Pakietami, aby uzyskać informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-414">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ab2f-415">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ab2f-415">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4ab2f-416">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-416">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="4ab2f-417">Folder projektu zawiera *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-417">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="4ab2f-418">W oknie wiersza polecenia, należy wprowadzić następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-418">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

<span data-ttu-id="4ab2f-419">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-419">Run the app.</span></span> <span data-ttu-id="4ab2f-420">Uruchamianie uruchomienia aplikacji `DbInitializer.Initialize` metody.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-420">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="4ab2f-421">`DbInitializer.Initialize` Wypełnia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-421">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="4ab2f-422">Otwórz SSOX bazy danych:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-422">Open the DB in SSOX:</span></span>

* <span data-ttu-id="4ab2f-423">Jeśli SSOX był wcześniej otwarty, kliknij przycisk **Odśwież** przycisku.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-423">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="4ab2f-424">Rozwiń **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-424">Expand the **Tables** node.</span></span> <span data-ttu-id="4ab2f-425">Utworzone tabele są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-425">The created tables are displayed.</span></span>

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="4ab2f-427">Sprawdź **CourseAssignment** tabeli:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-427">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="4ab2f-428">Kliknij prawym przyciskiem myszy **CourseAssignment** tabeli, a następnie wybierz pozycję **dane widoku**.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-428">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="4ab2f-429">Sprawdź **CourseAssignment** tabela zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-429">Verify the **CourseAssignment** table contains data.</span></span>

![Dane CourseAssignment SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="4ab2f-431">Dotyczą migracji z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="4ab2f-431">Apply the migration to the existing database</span></span>

<span data-ttu-id="4ab2f-432">Ta sekcja jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-432">This section is optional.</span></span> <span data-ttu-id="4ab2f-433">Następujące kroki działają tylko wtedy, gdy pominięto poprzednie [Usuń i ponownie utworzyć bazę danych](#drop) sekcji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-433">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="4ab2f-434">Podczas migracji są uruchamiane z istniejącymi danymi, mogą istnieć ograniczenia klucza Obcego, które nie są spełnione z istniejącymi danymi.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="4ab2f-435">Z danymi produkcyjnymi należy przedsięwziąć do migrowania istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="4ab2f-436">Ta sekcja zawiera przykład poprawiania naruszenia ograniczeń klucza Obcego.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="4ab2f-437">Nie wprowadzaj tych zmian kodu, bez kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="4ab2f-438">Nie należy wprowadzić te zmiany kodu, jeśli wykonano poprzedniej sekcji, a aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-438">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="4ab2f-439">*{Timestamp}_ComplexDataModel.cs* plik zawiera następujący kod:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="4ab2f-440">Poprzedzający kod dodaje dopuszcza `DepartmentID` klucza Obcego do `Course` tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="4ab2f-441">Bazy danych z poprzedniego samouczka zawiera wiersze w `Course`, więc ta tabela nie może zaktualizować migracji.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-441">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="4ab2f-442">Zapewnienie `ComplexDataModel` pracy migracji z istniejącymi danymi:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="4ab2f-443">Zmień kod, aby nadać nową kolumnę (`DepartmentID`) wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="4ab2f-444">Tworzyć fałszywej dział o nazwie "Temp" jako domyślnego działu.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="4ab2f-445">Rozwiązywanie ograniczeń klucza obcego</span><span class="sxs-lookup"><span data-stu-id="4ab2f-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="4ab2f-446">Aktualizacja `ComplexDataModel` klasy `Up` metody:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-446">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="4ab2f-447">Otwórz *{timestamp}_ComplexDataModel.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="4ab2f-448">Komentarz wiersza kodu, który dodaje `DepartmentID` kolumny `Course` tabeli.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="4ab2f-449">Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-449">Add the following highlighted code.</span></span> <span data-ttu-id="4ab2f-450">Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="4ab2f-451">Za pomocą zmian poprzednim, istniejące `Course` wiersze zostaną powiązane do działu "Temperatura", po `ComplexDataModel` `Up` metoda przebiegów.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-451">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="4ab2f-452">Aplikacji produkcyjnej może:</span><span class="sxs-lookup"><span data-stu-id="4ab2f-452">A production app would:</span></span>

* <span data-ttu-id="4ab2f-453">Uwzględnić w kodzie albo skryptach, aby dodać `Department` wiersze i powiązane `Course` wierszy do nowego `Department` wierszy.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-453">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="4ab2f-454">Używaj działu "Temp" lub wartość domyślną dla `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-454">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="4ab2f-455">Następny samouczek obejmuje powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="4ab2f-455">The next tutorial covers related data.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="4ab2f-456">[Poprzednie](xref:data/ef-rp/migrations)
> [dalej](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="4ab2f-456">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
