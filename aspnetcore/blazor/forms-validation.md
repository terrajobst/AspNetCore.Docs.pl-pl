---
title: ASP.NET Core formularzy i walidacji Blazor
author: guardrex
description: Dowiedz się, jak używać scenariuszy formularzy i walidacji pól w Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/forms-validation
ms.openlocfilehash: 0b2e38cdbd974a28960b917fb6b5ce370f8c4659
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030331"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="868ff-103">ASP.NET Core formularzy i walidacji Blazor</span><span class="sxs-lookup"><span data-stu-id="868ff-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="868ff-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="868ff-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="868ff-105">Formularze i walidacje są obsługiwane w programie Blazor przy użyciu [adnotacji danych](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="868ff-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="868ff-106">Scenariusze formularzy i weryfikacji mogą ulec zmianie w przypadku każdej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="868ff-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="868ff-107">Następujący `ExampleModel` typ definiuje logikę walidacji przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="868ff-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="868ff-108">Formularz jest definiowany przy użyciu `EditForm` składnika.</span><span class="sxs-lookup"><span data-stu-id="868ff-108">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="868ff-109">W poniższej formie przedstawiono typowe elementy, składniki i kod Razor:</span><span class="sxs-lookup"><span data-stu-id="868ff-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* <span data-ttu-id="868ff-110">Formularz sprawdza poprawność danych wejściowych użytkownika `name` w polu przy użyciu walidacji zdefiniowanej `ExampleModel` w typie.</span><span class="sxs-lookup"><span data-stu-id="868ff-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="868ff-111">Model jest tworzony w `@code` bloku składnika i przechowywany w prywatnym polu (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="868ff-111">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="868ff-112">Pole jest przypisane do `Model` atrybutu `<EditForm>` elementu.</span><span class="sxs-lookup"><span data-stu-id="868ff-112">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="868ff-113">`DataAnnotationsValidator` Składnik dołącza obsługę walidacji przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="868ff-113">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="868ff-114">`ValidationSummary` Składnik podsumowuje komunikaty weryfikacyjne.</span><span class="sxs-lookup"><span data-stu-id="868ff-114">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="868ff-115">`HandleValidSubmit`jest wyzwalany po pomyślnym przesłaniu formularza (kończy walidację).</span><span class="sxs-lookup"><span data-stu-id="868ff-115">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="868ff-116">Zestaw wbudowanych składników wejściowych jest dostępny do odbierania i weryfikowania danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="868ff-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="868ff-117">Dane wejściowe są weryfikowane po ich zmianie i po przesłaniu formularza.</span><span class="sxs-lookup"><span data-stu-id="868ff-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="868ff-118">W poniższej tabeli przedstawiono dostępne składniki danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="868ff-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="868ff-119">Składnik wejściowy</span><span class="sxs-lookup"><span data-stu-id="868ff-119">Input component</span></span> | <span data-ttu-id="868ff-120">Renderowane jako&hellip;</span><span class="sxs-lookup"><span data-stu-id="868ff-120">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="868ff-121">Wszystkie składniki danych wejściowych, w tym `EditForm`, obsługują dowolne atrybuty.</span><span class="sxs-lookup"><span data-stu-id="868ff-121">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="868ff-122">Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest dodawany do renderowanego elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="868ff-122">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="868ff-123">Składniki wejściowe zapewniają domyślne zachowanie podczas sprawdzania poprawności edycji i zmiany ich klasy CSS, aby odzwierciedlały stan pola.</span><span class="sxs-lookup"><span data-stu-id="868ff-123">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="868ff-124">Niektóre składniki obejmują przydatne logiki analizy.</span><span class="sxs-lookup"><span data-stu-id="868ff-124">Some components include useful parsing logic.</span></span> <span data-ttu-id="868ff-125">Na przykład i `InputDate` `InputNumber` bezproblemowo obsłużyć wartości, które można przeanalizować, rejestrując je jako błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="868ff-125">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="868ff-126">Typy, które mogą akceptować wartości null, obsługują również wartość null pola docelowego (na przykład `int?`).</span><span class="sxs-lookup"><span data-stu-id="868ff-126">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="868ff-127">Następujący `Starship` typ definiuje logikę walidacji przy użyciu większego zestawu właściwości i adnotacji danych niż wcześniej `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="868ff-127">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="868ff-128">W powyższym przykładzie `Description` jest opcjonalne, ponieważ nie są obecne adnotacje danych.</span><span class="sxs-lookup"><span data-stu-id="868ff-128">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="868ff-129">Następujący formularz sprawdza poprawność danych wejściowych użytkownika przy użyciu weryfikacji zdefiniowanej w `Starship` modelu:</span><span class="sxs-lookup"><span data-stu-id="868ff-129">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="868ff-130">Tworzy jako wartość kaskadową, która śledzi metadane dotyczące procesu edycji, w tym pola, które zostały zmodyfikowane i bieżące komunikaty weryfikacyjne. [](xref:blazor/components#cascading-values-and-parameters) `EditForm` `EditContext`</span><span class="sxs-lookup"><span data-stu-id="868ff-130">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="868ff-131">Zapewnia również wygodne zdarzenia dla prawidłowych i nieprawidłowych przesyłania`OnValidSubmit`( `OnInvalidSubmit`,). `EditForm`</span><span class="sxs-lookup"><span data-stu-id="868ff-131">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="868ff-132">Alternatywnie można użyć `OnSubmit` do wyzwolenia walidacji i sprawdzenia wartości pól z niestandardowym kodem walidacji.</span><span class="sxs-lookup"><span data-stu-id="868ff-132">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="868ff-133">Składnik dołącza obsługę walidacji przy użyciu adnotacji danych do `EditContext`kaskadowo. `DataAnnotationsValidator`</span><span class="sxs-lookup"><span data-stu-id="868ff-133">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="868ff-134">Włączenie obsługi walidacji przy użyciu adnotacji danych obecnie wymaga tego jawnego gestu, ale rozważamy, że to zachowanie domyślne można przesłonić.</span><span class="sxs-lookup"><span data-stu-id="868ff-134">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="868ff-135">Aby użyć innego systemu sprawdzania poprawności niż adnotacje danych, Zastąp zmienną `DataAnnotationsValidator` implementacją niestandardową.</span><span class="sxs-lookup"><span data-stu-id="868ff-135">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="868ff-136">Implementacja ASP.NET Core jest dostępna do inspekcji w źródle odwołania: [](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)DataAnnotationsValidator/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="868ff-136">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> <span data-ttu-id="868ff-137">*Implementacja ASP.NET Core podlega szybkim aktualizacjom w okresie wersji zapoznawczej.*</span><span class="sxs-lookup"><span data-stu-id="868ff-137">*The ASP.NET Core implementation is subject to rapid updates during the preview release period.*</span></span>

<span data-ttu-id="868ff-138">Składnik podsumowuje wszystkie komunikaty weryfikacyjne podobne do [pomocnika tagów podsumowania walidacji.](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper) `ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="868ff-138">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="868ff-139">Składnik wyświetla komunikaty sprawdzania poprawności dla określonego pola, które jest podobne do pomocnika [tagów komunikatu weryfikacji.](xref:mvc/views/working-with-forms#the-validation-message-tag-helper) `ValidationMessage`</span><span class="sxs-lookup"><span data-stu-id="868ff-139">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="868ff-140">Określ pole do walidacji z `For` atrybutem i wyrażeniem lambda, które nazywa właściwość modelu:</span><span class="sxs-lookup"><span data-stu-id="868ff-140">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="868ff-141">Składniki `ValidationMessage` i`ValidationSummary` obsługują dowolne atrybuty.</span><span class="sxs-lookup"><span data-stu-id="868ff-141">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="868ff-142">Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest `<div>` dodawany `<ul>` do wygenerowanego elementu or.</span><span class="sxs-lookup"><span data-stu-id="868ff-142">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>
