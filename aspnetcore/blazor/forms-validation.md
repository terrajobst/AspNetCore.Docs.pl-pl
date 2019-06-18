---
title: ASP.NET Core Blazor formularze i Walidacja
author: guardrex
description: Dowiedz się, jak używać formularzy i scenariusze weryfikacji pola w Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/forms-validation
ms.openlocfilehash: a3e47844d5d0611371f2fce24ed8d3ccb07754d7
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152721"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="02c5f-103">ASP.NET Core Blazor formularze i Walidacja</span><span class="sxs-lookup"><span data-stu-id="02c5f-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="02c5f-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="02c5f-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="02c5f-105">Formularze i Walidacja są obsługiwane w Blazor przy użyciu [adnotacje danych](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="02c5f-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="02c5f-106">Formularze i scenariusze weryfikacji prawdopodobnie ulegną zmianie po każdym wydaniu wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="02c5f-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="02c5f-107">Następujące `ExampleModel` typ definiuje logikę weryfikacji przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="02c5f-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="02c5f-108">Formularz jest definiowana za pomocą `<EditForm>` składnika.</span><span class="sxs-lookup"><span data-stu-id="02c5f-108">A form is defined using the `<EditForm>` component.</span></span> <span data-ttu-id="02c5f-109">Następującą postać przedstawia typowe elementy, składników i kod Razor:</span><span class="sxs-lookup"><span data-stu-id="02c5f-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="02c5f-110">Formularz sprawdza poprawność danych wejściowych użytkownika w `name` pola za pomocą weryfikacji zdefiniowane w `ExampleModel` typu.</span><span class="sxs-lookup"><span data-stu-id="02c5f-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="02c5f-111">Model jest tworzony w składniku `@code` zablokować, a także przechowywane w pole prywatne (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="02c5f-111">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="02c5f-112">Pole jest przypisany do `Model` atrybutu `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="02c5f-112">The field is assigned to the `Model` attribute of the `<EditForm>`.</span></span>
* <span data-ttu-id="02c5f-113">Składnik weryfikacji adnotacji danych (`<DataAnnotationsValidator>`) dołącza Obsługa weryfikacji przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="02c5f-113">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations.</span></span>
* <span data-ttu-id="02c5f-114">Składnik podsumowania sprawdzania poprawności (`<ValidationSummary>`) znajduje się podsumowanie komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="02c5f-114">The Validation Summary component (`<ValidationSummary>`) summarizes validation messages.</span></span>
* <span data-ttu-id="02c5f-115">`HandleValidSubmit` jest wyzwalany, gdy formularz pomyślnie przesyła (przebiegów weryfikacji).</span><span class="sxs-lookup"><span data-stu-id="02c5f-115">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="02c5f-116">Zestaw wbudowanych składników danych wejściowych są dostępne do odbierania i weryfikowanie danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="02c5f-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="02c5f-117">Dane wejściowe są prawidłowe, gdy są one zmienione i podczas przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="02c5f-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="02c5f-118">Dostępne składniki wejściowe są pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="02c5f-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="02c5f-119">Składnika danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="02c5f-119">Input component</span></span>   | <span data-ttu-id="02c5f-120">Renderowane jako&hellip;</span><span class="sxs-lookup"><span data-stu-id="02c5f-120">Rendered as&hellip;</span></span>       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

<span data-ttu-id="02c5f-121">Wszystkich składników danych wejściowych, włącznie z `<EditForm>`, obsługuje dowolne atrybutów.</span><span class="sxs-lookup"><span data-stu-id="02c5f-121">All of the input components, including `<EditForm>`, support arbitrary attributes.</span></span> <span data-ttu-id="02c5f-122">Dowolny atrybut, który nie jest zgodny z parametrem jest dodawany do wygenerowany `<form>`, `<input>`, `<select>`, lub `<textarea>` elementu.</span><span class="sxs-lookup"><span data-stu-id="02c5f-122">Any attribute that doesn't match a parameter is added to the generated `<form>`, `<input>`, `<select>`, or `<textarea>` element.</span></span>

<span data-ttu-id="02c5f-123">Składniki danych wejściowych udostępniają domyślne zachowanie sprawdzania poprawności po edycji i zmienianie ich klasy CSS, aby odzwierciedlić stan pola.</span><span class="sxs-lookup"><span data-stu-id="02c5f-123">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="02c5f-124">Niektóre składniki zawierają przydatne podczas analizowania logiki.</span><span class="sxs-lookup"><span data-stu-id="02c5f-124">Some components include useful parsing logic.</span></span> <span data-ttu-id="02c5f-125">Na przykład `<InputDate>` i `<InputNumber>` bezpiecznie obsłużyć niemożliwy do przeanalizowania wartości, rejestrując je jako błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="02c5f-125">For example, `<InputDate>` and `<InputNumber>` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="02c5f-126">Typy, które może akceptować wartości null obsługuje również dopuszczanie wartości null dla pola docelowego (na przykład `int?`).</span><span class="sxs-lookup"><span data-stu-id="02c5f-126">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="02c5f-127">Następujące `Starship` typ definiuje logikę weryfikacji za pomocą większy zbiór właściwości oraz [adnotacje danych](xref:mvc/models/validation) niż wcześniej `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="02c5f-127">The following `Starship` type defines validation logic using a larger set of properties and [data annotations](xref:mvc/models/validation) than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, 
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    // Optional (no data annotations)
    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "Form disallowed for unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="02c5f-128">Następującą postać weryfikuje użytkownika danych wejściowych za pomocą weryfikacji zdefiniowane w `Starship` modelu:</span><span class="sxs-lookup"><span data-stu-id="02c5f-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" @bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="@starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" @bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
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

<span data-ttu-id="02c5f-129">`<EditForm>` Tworzy `EditContext` jako [cascading wartość](xref:blazor/components#cascading-values-and-parameters) , śledzi metadane dotyczące proces edycji, w tym, które pola zostały zmienione oraz bieżące wiadomości sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="02c5f-129">The `<EditForm>` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including what fields have been modified and the current validation messages.</span></span> <span data-ttu-id="02c5f-130">`<EditForm>` Udostępnia również wygodną zdarzenia dla prawidłowe i nieprawidłowe przesyła (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="02c5f-130">The `<EditForm>` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="02c5f-131">Można również użyć `OnSubmit` wyzwolić weryfikacji i sprawdź wartości pola z kodu niestandardowego sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="02c5f-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="02c5f-132">Składnik weryfikacji adnotacji danych (`<DataAnnotationsValidator>`) dołącza Obsługa weryfikacji przy użyciu adnotacji danych w celu kaskadowy `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="02c5f-132">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="02c5f-133">Włączanie obsługi sprawdzania poprawności obecnie przy użyciu adnotacji danych wymaga to jawne gestu, ale rozważamy, dzięki czemu to zachowanie domyślne, które można przesłonić.</span><span class="sxs-lookup"><span data-stu-id="02c5f-133">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="02c5f-134">Aby korzystać z systemu weryfikacji innego niż adnotacje danych, moduł weryfikacji adnotacji danych należy zastąpić implementację niestandardową.</span><span class="sxs-lookup"><span data-stu-id="02c5f-134">To use a different validation system than data annotations, replace the Data Annotations Validator with a custom implementation.</span></span> <span data-ttu-id="02c5f-135">Implementacja platformy ASP.NET Core jest dostępny do kontroli źródła odwołania: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="02c5f-135">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> <span data-ttu-id="02c5f-136">*Implementacja programu ASP.NET Core podlega szybkiej aktualizacji w wersji zapoznawczej.*</span><span class="sxs-lookup"><span data-stu-id="02c5f-136">*The ASP.NET Core implementation is subject to rapid updates during the preview release period.*</span></span>

<span data-ttu-id="02c5f-137">Składnik podsumowania sprawdzania poprawności (`<ValidationSummary>`) znajduje się podsumowanie wszystkich komunikatów o weryfikacji, który jest podobny do [Pomocnik tagu podsumowania sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="02c5f-137">The Validation Summary component (`<ValidationSummary>`) summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="02c5f-138">Składnik komunikat sprawdzania poprawności (`<ValidationMessage>`) wyświetla komunikaty weryfikacji dla określonego pola, które są podobne do [Pomocnik tagu komunikat sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="02c5f-138">The Validation Message component (`<ValidationMessage>`) displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="02c5f-139">Należy określić to pole do weryfikacji za pomocą `For` atrybutu i wyrażenia lambda nazewnictwa właściwości modelu:</span><span class="sxs-lookup"><span data-stu-id="02c5f-139">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="02c5f-140">`<ValidationMessage>` i `<ValidationSummary>` składniki obsługują dowolnymi atrybutami.</span><span class="sxs-lookup"><span data-stu-id="02c5f-140">The `<ValidationMessage>` and `<ValidationSummary>` components support arbitrary attributes.</span></span> <span data-ttu-id="02c5f-141">Dowolny atrybut, który nie jest zgodny z parametrem jest dodawany do wygenerowany `<div>` lub `<ul>` elementu.</span><span class="sxs-lookup"><span data-stu-id="02c5f-141">Any attribute that doesn't match a parameter is added to the generated `<div>` or `<ul>` element.</span></span>
