---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 sieci Web Forms — część 8 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: f5ad2c1caf6036a0d8ee2ebbd07de60009090f1b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374055"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="ef6a8-104">Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 Web Forms — część 8</span><span class="sxs-lookup"><span data-stu-id="ef6a8-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>
====================
<span data-ttu-id="ef6a8-105">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="ef6a8-106">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="ef6a8-107">Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="ef6a8-108">Za pomocą funkcji dynamicznego danych do formatu i sprawdzanie poprawności danych</span><span class="sxs-lookup"><span data-stu-id="ef6a8-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="ef6a8-109">W poprzednim samouczku wdrożono procedury składowane.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="ef6a8-110">W tym samouczku opisano, jak dane dynamiczne, funkcje może zapewnić następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="ef6a8-111">Pola są automatycznie formatowane do wyświetlania na podstawie ich typu danych.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="ef6a8-112">Pola są automatycznie zweryfikowana, na podstawie ich typu danych.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="ef6a8-113">Metadane można dodawać do modelu danych, aby dostosować zachowanie formatowania i sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="ef6a8-114">Gdy to zrobisz, możesz dodać reguły formatowania i sprawdzania poprawności w jednym miejscu i są automatycznie stosowane wszędzie, gdzie możesz uzyskać dostęp pola za pomocą formantów danych dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="ef6a8-115">Aby zobaczyć, jak to działa, możesz zmienić formantów, użyj do wyświetlania i edytowania pól w istniejącym *Students.aspx* strony i dodasz metadanych formatowania i sprawdzania poprawności do pola Nazwa i data `Student` typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

<span data-ttu-id="ef6a8-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span></span>

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="ef6a8-117">Przy użyciu DynamicField i DynamicControl formantów</span><span class="sxs-lookup"><span data-stu-id="ef6a8-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="ef6a8-118">Otwórz *Students.aspx* strony i `StudentsGridView` Zastąp formant **nazwa** i **Data rejestracji** `TemplateField` elementów za pomocą następujących znaczników:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="ef6a8-119">Ten kod znaczników używa `DynamicControl` kontroluje zamiast `TextBox` i `Label` kontrolek w student nazwy szablonu, przy czym `DynamicField` kontrola Data rejestracji.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="ef6a8-120">Ciągi formatów, nie są określone.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-120">No format strings are specified.</span></span>

<span data-ttu-id="ef6a8-121">Dodaj `ValidationSummary` kontrolowanie po `StudentsGridView` kontroli.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="ef6a8-122">W `SearchGridView` kontrolka zastąpić kod znaczników dla **nazwa** i **Data rejestracji** kolumn, co Ty, jak w `StudentsGridView` kontrolować, z wyjątkiem pominąć `EditItemTemplate` elementu.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="ef6a8-123">`Columns` Elementu `SearchGridView` kontrolka zawiera teraz następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="ef6a8-124">Otwórz *Students.aspx.cs* i dodaj następującą `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="ef6a8-125">Dodawanie obsługi dla strony `Init` zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="ef6a8-126">Ten kod określa, że dane dynamiczne zapewni formatowania i sprawdzania poprawności w tych formantów powiązanych z danymi dla pól `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="ef6a8-127">Jeśli otrzymasz komunikat o błędzie, jak w poniższym przykładzie, po uruchomieniu strony, zwykle oznacza to, pamiętasz wywołania `EnableDynamicData` method in Class metoda `Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="ef6a8-128">Uruchom stronę.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-128">Run the page.</span></span>

<span data-ttu-id="ef6a8-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span></span>

<span data-ttu-id="ef6a8-130">W **Data rejestracji** kolumnie czasu jest wyświetlany wraz z datą, ponieważ typ właściwości to `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="ef6a8-131">Można to naprawić później.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-131">You'll fix that later.</span></span>

<span data-ttu-id="ef6a8-132">Na razie Zwróć uwagę, dane dynamiczne zapewnia automatycznie Walidacja danych podstawowych.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="ef6a8-133">Na przykład kliknij pozycję **Edytuj**, wyczyść pole daty, kliknij przycisk **aktualizacji**, i zobaczysz, że dane dynamiczne automatycznie sprawia, że to pole wymagane ponieważ wartości nie dopuszcza wartości null w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="ef6a8-134">Zostanie wyświetlona strona gwiazdkę po pola i komunikat o błędzie w `ValidationSummary` sterowania:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

<span data-ttu-id="ef6a8-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span></span>

<span data-ttu-id="ef6a8-136">Można pominąć `ValidationSummary` kontrolować, ponieważ można również przytrzymasz wskaźnik myszy nad gwiazdki, aby wyświetlić komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

<span data-ttu-id="ef6a8-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span></span>

<span data-ttu-id="ef6a8-138">Dane dynamiczne również zostanie przeprowadzona Weryfikacja dane wprowadzone w **Data rejestracji** pole jest prawidłową datą:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

<span data-ttu-id="ef6a8-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span></span>

<span data-ttu-id="ef6a8-140">Jak widać, to ogólny komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="ef6a8-141">W następnej sekcji pokazano, jak dostosować komunikaty, a także sprawdzania poprawności i reguły formatowania.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="ef6a8-142">Dodawanie metadanych do modelu danych</span><span class="sxs-lookup"><span data-stu-id="ef6a8-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="ef6a8-143">Zazwyczaj który chcesz dostosować funkcje udostępniane przez dane dynamiczne.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="ef6a8-144">Na przykład możesz zmienić sposób wyświetlania danych i zawartość komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="ef6a8-145">Użytkownik zwykle również dostosować reguły sprawdzania poprawności danych, aby zapewnić więcej funkcji niż zawiera dane dynamiczne automatycznie oparte na typach danych.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="ef6a8-146">Aby to zrobić, należy utworzyć klas częściowych, które odnoszą się do typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="ef6a8-147">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu, wybierz opcję **Dodaj odwołanie**i Dodaj odwołanie do `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

<span data-ttu-id="ef6a8-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span></span>

<span data-ttu-id="ef6a8-149">W *DAL* folderu, Utwórz nowy plik klasy, nadaj jej nazwę *Student.cs*i Zastąp kod szablonu w nim poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="ef6a8-150">Ten kod tworzy klasę częściową dla `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="ef6a8-151">`MetadataType` Zastosowany do klasy częściowej identyfikuje klasę, która za pomocą określić metadane.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="ef6a8-152">Klasa metadanych może mieć dowolną nazwę, ale przy użyciu nazwy podmiotu oraz "Metadane" jest powszechną praktyką.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="ef6a8-153">Atrybuty stosowane do właściwości w klasie metadanych określ, formatowania i sprawdzania poprawności, zasady i komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="ef6a8-154">Atrybuty pokazane tutaj będzie miała następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-154">The attributes shown here will have the following results:</span></span>

- <span data-ttu-id="ef6a8-155">`EnrollmentDate` zostanie wyświetlona jako datę (bez czasu).</span><span class="sxs-lookup"><span data-stu-id="ef6a8-155">`EnrollmentDate` will display as a date (without a time).</span></span>
- <span data-ttu-id="ef6a8-156">Zarówno nazwa pola musi być 25 znaków lub mniej długości i niestandardowy komunikat o błędzie jest dostarczany.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="ef6a8-157">Zarówno nazwa pola, które są wymagane, a podano niestandardowy komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="ef6a8-158">Uruchom *Students.aspx* strony ponownie, a zobaczysz, że dane są teraz wyświetlane bez godzin:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

<span data-ttu-id="ef6a8-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span></span>

<span data-ttu-id="ef6a8-160">Edytuj wiersz i spróbuj wyczyścić wartości w polach Nazwa.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="ef6a8-161">Gwiazdek wskazującą błędy pola są wyświetlane tak szybko, jak pozostawić pole, zanim klikniesz pozycję **aktualizacji**.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="ef6a8-162">Po kliknięciu **aktualizacji**, zostanie wyświetlona strona tekst komunikatu o błędzie określony.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-162">When you click **Update**, the page displays the error message text you specified.</span></span>

<span data-ttu-id="ef6a8-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span></span>

<span data-ttu-id="ef6a8-164">Wprowadź nazwy, które są dłuższe niż 25 znaków, kliknij przycisk **aktualizacji**, i zostanie wyświetlona strona tekst komunikatu o błędzie określony.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

<span data-ttu-id="ef6a8-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="ef6a8-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span></span>

<span data-ttu-id="ef6a8-166">Teraz, gdy skonfigurowano te reguły formatowania i sprawdzania poprawności metadanych modelu danych, zasady zostaną automatycznie zastosowane na każdej stronie, który wyświetla lub zezwala na zmiany w tych polach, tak długo, jak długo używasz `DynamicControl` lub `DynamicField` kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="ef6a8-167">Zmniejsza to ilość nadmiarowy kod, który trzeba napisać, co sprawia, że programowanie i testowanie, i zapewnia, że formatowanie danych i sprawdzania poprawności są spójne w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="ef6a8-168">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="ef6a8-168">More Information</span></span>

<span data-ttu-id="ef6a8-169">To już koniec tej serii samouczków na temat rozpoczynania pracy z platformą Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ef6a8-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="ef6a8-170">Aby uzyskać więcej zasobów, aby pomóc nauczyć się, jak używać programu Entity Framework, kontynuuj [pierwszym samouczku dalej serii samouczków Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) lub można znaleźć w następujących witrynach:</span><span class="sxs-lookup"><span data-stu-id="ef6a8-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="ef6a8-171">Entity Framework — często zadawane pytania</span><span class="sxs-lookup"><span data-stu-id="ef6a8-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="ef6a8-172">Blog zespołu programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ef6a8-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="ef6a8-173">Entity Framework w bibliotece MSDN</span><span class="sxs-lookup"><span data-stu-id="ef6a8-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="ef6a8-174">Entity Framework w Centrum deweloperów MSDN danych</span><span class="sxs-lookup"><span data-stu-id="ef6a8-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="ef6a8-175">Omówienie kontrolki serwera sieci Web EntityDataSource w bibliotece MSDN</span><span class="sxs-lookup"><span data-stu-id="ef6a8-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="ef6a8-176">Kontrolka EntityDataSource dokumentacja interfejsu API w bibliotece MSDN</span><span class="sxs-lookup"><span data-stu-id="ef6a8-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="ef6a8-177">Entity Framework fora w witrynie MSDN</span><span class="sxs-lookup"><span data-stu-id="ef6a8-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="ef6a8-178">Blog Julie Lerman</span><span class="sxs-lookup"><span data-stu-id="ef6a8-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [<span data-ttu-id="ef6a8-179">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="ef6a8-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
