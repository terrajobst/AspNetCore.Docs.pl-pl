---
title: ASP.NET Core Blazor formularzy i walidacji
author: guardrex
description: Dowiedz się, jak używać scenariuszy formularzy i walidacji pól w Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/forms-validation
ms.openlocfilehash: a94a433f26e451bbadc73615e502e46d273f05c2
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828142"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a><span data-ttu-id="ae4a6-103">ASP.NET Core Blazor formularzy i walidacji</span><span class="sxs-lookup"><span data-stu-id="ae4a6-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="ae4a6-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ae4a6-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ae4a6-105">Formularze i walidacje są obsługiwane w Blazor przy użyciu [adnotacji danych](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="ae4a6-106">Następujący typ `ExampleModel` definiuje logikę walidacji przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="ae4a6-107">Formularz jest definiowany przy użyciu składnika `EditForm`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="ae4a6-108">W poniższej formie przedstawiono typowe elementy, składniki i kod Razor:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="exampleModel.Name" />

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

* <span data-ttu-id="ae4a6-109">Formularz sprawdza poprawność danych wejściowych użytkownika w polu `name` przy użyciu walidacji zdefiniowanej w typie `ExampleModel`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="ae4a6-110">Model jest tworzony w bloku `@code` składnika i przechowywany w polu prywatnym (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="ae4a6-111">Pole jest przypisane do atrybutu `Model` elementu `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="ae4a6-112">Składnik `DataAnnotationsValidator` dołącza obsługę walidacji przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="ae4a6-113">Składnik `ValidationSummary` podsumowuje komunikaty weryfikacyjne.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="ae4a6-114">`HandleValidSubmit` jest wyzwalany po pomyślnym przesłaniu formularza (kończy walidację).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="ae4a6-115">Zestaw wbudowanych składników wejściowych jest dostępny do odbierania i weryfikowania danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="ae4a6-116">Dane wejściowe są weryfikowane po ich zmianie i po przesłaniu formularza.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="ae4a6-117">W poniższej tabeli przedstawiono dostępne składniki danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="ae4a6-118">Składnik wejściowy</span><span class="sxs-lookup"><span data-stu-id="ae4a6-118">Input component</span></span> | <span data-ttu-id="ae4a6-119">Renderowane jako&hellip;</span><span class="sxs-lookup"><span data-stu-id="ae4a6-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="ae4a6-120">Wszystkie składniki danych wejściowych, w tym `EditForm`, obsługują dowolne atrybuty.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="ae4a6-121">Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest dodawany do renderowanego elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="ae4a6-122">Składniki wejściowe zapewniają domyślne zachowanie podczas sprawdzania poprawności edycji i zmiany ich klasy CSS, aby odzwierciedlały stan pola.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="ae4a6-123">Niektóre składniki obejmują przydatne logiki analizy.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="ae4a6-124">Na przykład `InputDate` i `InputNumber` obsłużyć bezproblemowo przeanalizować wartości, rejestrując je jako błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="ae4a6-125">Typy, które mogą akceptować wartości null, obsługują również wartość null pola docelowego (na przykład `int?`).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="ae4a6-126">Następujący typ `Starship` definiuje logikę walidacji przy użyciu większego zestawu właściwości i adnotacji danych niż wcześniejsza `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="ae4a6-127">W poprzednim przykładzie `Description` jest opcjonalne, ponieważ nie są obecne adnotacje danych.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="ae4a6-128">Następujący formularz sprawdza poprawność danych wejściowych użytkownika przy użyciu weryfikacji zdefiniowanej w modelu `Starship`:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```razor
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

<span data-ttu-id="ae4a6-129">`EditForm` tworzy `EditContext` jako [wartość kaskadową](xref:blazor/components#cascading-values-and-parameters) , która śledzi metadane dotyczące procesu edycji, w tym pola, które zostały zmodyfikowane, i bieżące komunikaty weryfikacyjne.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="ae4a6-130">`EditForm` również zapewnia wygodne zdarzenia dla prawidłowych i nieprawidłowych przesyłania (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="ae4a6-131">Alternatywnie możesz użyć `OnSubmit`, aby wyzwolić walidację i sprawdzanie wartości pól przy użyciu niestandardowego kodu sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="ae4a6-132">InputText na podstawie zdarzenia wejściowego</span><span class="sxs-lookup"><span data-stu-id="ae4a6-132">InputText based on the input event</span></span>

<span data-ttu-id="ae4a6-133">Użyj składnika `InputText`, aby utworzyć niestandardowy składnik, który używa zdarzenia `input` zamiast zdarzenia `change`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="ae4a6-134">Utwórz składnik z następującą adiustacją i użyj składnika, tak jak `InputText` jest używany:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="ae4a6-135">Obsługa walidacji</span><span class="sxs-lookup"><span data-stu-id="ae4a6-135">Validation support</span></span>

<span data-ttu-id="ae4a6-136">Składnik `DataAnnotationsValidator` dołącza obsługę walidacji przy użyciu adnotacji danych do `EditContext`kaskadowo.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="ae4a6-137">Włączenie obsługi walidacji przy użyciu adnotacji danych wymaga tego jawnego gestu.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="ae4a6-138">Aby użyć innego systemu sprawdzania poprawności niż adnotacje danych, Zastąp `DataAnnotationsValidator` implementacją niestandardową.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="ae4a6-139">Implementacja ASP.NET Core jest dostępna do inspekcji w źródle referencyjnym: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="ae4a6-140"> wykonuje dwa typy walidacji:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-140"> performs two types of validation:</span></span>

* <span data-ttu-id="ae4a6-141">*Sprawdzanie poprawności pola* jest wykonywane, gdy użytkownik wyprowadzi karty z pola.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-141">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="ae4a6-142">Podczas sprawdzania poprawności pola składnik `DataAnnotationsValidator` kojarzy wszystkie zgłoszone wyniki walidacji z polem.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-142">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="ae4a6-143">*Sprawdzanie poprawności modelu* jest wykonywane, gdy użytkownik prześle formularz.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-143">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="ae4a6-144">Podczas walidacji modelu składnik `DataAnnotationsValidator` próbuje określić pole na podstawie nazwy elementu członkowskiego, który raportuje wynik sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-144">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="ae4a6-145">Wyniki walidacji, które nie są skojarzone z poszczególnymi elementami członkowskimi, są skojarzone z modelem, a nie polem.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-145">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="ae4a6-146">Składniki podsumowania walidacji i komunikatów weryfikacji</span><span class="sxs-lookup"><span data-stu-id="ae4a6-146">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="ae4a6-147">Składnik `ValidationSummary` podsumowuje wszystkie komunikaty weryfikacyjne podobne do [pomocnika tagów podsumowania walidacji](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="ae4a6-147">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="ae4a6-148">Wyprowadź komunikaty weryfikacji dla określonego modelu z parametrem `Model`:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-148">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@starship" />
```

<span data-ttu-id="ae4a6-149">Składnik `ValidationMessage` wyświetla komunikaty weryfikacyjne dla określonego pola, który jest podobny do [pomocnika tagów komunikatu weryfikacji](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-149">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="ae4a6-150">Określ pole do walidacji z atrybutem `For` i wyrażenie lambda, które nazywa właściwość modelu:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-150">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="ae4a6-151">Składniki `ValidationMessage` i `ValidationSummary` obsługują dowolne atrybuty.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-151">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="ae4a6-152">Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest dodawany do wygenerowanego elementu `<div>` lub `<ul>`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-152">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="ae4a6-153">Niestandardowe atrybuty walidacji</span><span class="sxs-lookup"><span data-stu-id="ae4a6-153">Custom validation attributes</span></span>

<span data-ttu-id="ae4a6-154">Aby upewnić się, że wynik walidacji jest prawidłowo skojarzony z polem przy użyciu [niestandardowego atrybutu walidacji](xref:mvc/models/validation#custom-attributes), należy przekazać <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> kontekstu walidacji podczas tworzenia <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-154">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

private class MyCustomValidator : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, 
        ValidationContext validationContext)
    {
        ...

        return new ValidationResult("Validation message to user.",
            new[] { validationContext.MemberName });
    }
}
```

::: moniker range=">= aspnetcore-3.1"

### <a name="opno-locblazor-data-annotations-validation-package"></a><span data-ttu-id="ae4a6-155">Pakiet weryfikacji adnotacji danych Blazor</span><span class="sxs-lookup"><span data-stu-id="ae4a6-155">Blazor data annotations validation package</span></span>

<span data-ttu-id="ae4a6-156">[Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) to pakiet, który wypełnia luki w działaniu sprawdzania poprawności przy użyciu składnika `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-156">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="ae4a6-157">Pakiet jest obecnie *eksperymentalny*.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-157">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="ae4a6-158">[CompareProperty] — atrybut</span><span class="sxs-lookup"><span data-stu-id="ae4a6-158">[CompareProperty] attribute</span></span>

<span data-ttu-id="ae4a6-159"><xref:System.ComponentModel.DataAnnotations.CompareAttribute> nie działa dobrze ze składnikiem `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-159">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="ae4a6-160">[Microsoft. AspNetCore.Blazor. .](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) Pakiet *eksperymentalny* walidacji wprowadza dodatkowy atrybut walidacji, `ComparePropertyAttribute`, który działa wokół tych ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-160">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="ae4a6-161">W aplikacji Blazor `[CompareProperty]` jest bezpośrednią wymianą dla atrybutu `[Compare]`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-161">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span> <span data-ttu-id="ae4a6-162">Aby uzyskać więcej informacji, zobacz [CompareAttribute został zignorowany z OnValidSubmit EditForm (dotnet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-162">For more information, see [CompareAttribute ignored with OnValidSubmit EditForm (dotnet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="ae4a6-163">Modele zagnieżdżone, typy kolekcji i typy złożone</span><span class="sxs-lookup"><span data-stu-id="ae4a6-163">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="ae4a6-164"> zapewnia obsługę sprawdzania poprawności danych wejściowych formularza przy użyciu adnotacji danych z wbudowaną `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-164"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="ae4a6-165">Jednak `DataAnnotationsValidator` sprawdza tylko poprawność właściwości najwyższego poziomu modelu powiązanego z formularzem, który nie jest właściwościami typu "Collection" lub "Complex".</span><span class="sxs-lookup"><span data-stu-id="ae4a6-165">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="ae4a6-166">Aby sprawdzić poprawność całego grafu obiektów modelu powiązanego, w tym właściwości kolekcji i typu złożonego, użyj `ObjectGraphDataAnnotationsValidator` dostarczonej przez *eksperymentalny* [Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) — pakiet:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-166">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="ae4a6-167">Dodawanie adnotacji do właściwości modelu za pomocą `[ValidateComplexType]`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-167">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="ae4a6-168">W poniższych klasach modelu Klasa `ShipDescription` zawiera dodatkowe adnotacje danych do zweryfikowania, gdy model jest powiązany z formularzem:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-168">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="ae4a6-169">*Starship.cs*:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-169">*Starship.cs*:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    ...

    [ValidateComplexType]
    public ShipDescription ShipDescription { get; set; }

    ...
}
```

<span data-ttu-id="ae4a6-170">*ShipDescription.cs*:</span><span class="sxs-lookup"><span data-stu-id="ae4a6-170">*ShipDescription.cs*:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class ShipDescription
{
    [Required]
    [StringLength(40, ErrorMessage = "Description too long (40 char).")]
    public string ShortDescription { get; set; }
    
    [Required]
    [StringLength(240, ErrorMessage = "Description too long (240 char).")]
    public string LongDescription { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="ae4a6-171">Walidacja właściwości typu złożonego lub kolekcji</span><span class="sxs-lookup"><span data-stu-id="ae4a6-171">Validation of complex or collection type properties</span></span>

<span data-ttu-id="ae4a6-172">Atrybuty walidacji zastosowane do właściwości modelu sprawdzają poprawność podczas przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-172">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="ae4a6-173">Jednak właściwości kolekcji lub złożonych typów danych modelu nie są sprawdzane w wyniku przesłania formularza przez składnik `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-173">However, the properties of collections or complex data types of a model aren't validated on form submission by the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="ae4a6-174">Aby przestrzegać zagnieżdżonych atrybutów walidacji w tym scenariuszu, Użyj niestandardowego składnika walidacji.</span><span class="sxs-lookup"><span data-stu-id="ae4a6-174">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="ae4a6-175">Aby zapoznać się z przykładem, zobacz [przykładoweBlazor walidacji (ASPNET/Samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span><span class="sxs-lookup"><span data-stu-id="ae4a6-175">For an example, see the [Blazor Validation sample (aspnet/samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

::: moniker-end
