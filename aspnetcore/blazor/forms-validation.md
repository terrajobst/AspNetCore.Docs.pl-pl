---
title: ASP.NET Core formularzy i walidacji Blazor
author: guardrex
description: Dowiedz się, jak używać scenariuszy formularzy i walidacji pól w Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: blazor/forms-validation
ms.openlocfilehash: 09281779e7f0b31e525e0e79c2d6d9ce9ca5b8ce
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659787"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="146d5-103">ASP.NET Core formularzy i walidacji Blazor</span><span class="sxs-lookup"><span data-stu-id="146d5-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="146d5-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="146d5-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="146d5-105">Formularze i walidacje są obsługiwane w programie Blazor przy użyciu [adnotacji danych](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="146d5-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="146d5-106">Następujący typ `ExampleModel` definiuje logikę walidacji przy użyciu adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="146d5-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="146d5-107">Formularz jest definiowany przy użyciu składnika `EditForm`.</span><span class="sxs-lookup"><span data-stu-id="146d5-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="146d5-108">W poniższej formie przedstawiono typowe elementy, składniki i kod Razor:</span><span class="sxs-lookup"><span data-stu-id="146d5-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="146d5-109">Formularz sprawdza poprawność danych wejściowych użytkownika w polu `name` przy użyciu walidacji zdefiniowanej w typie `ExampleModel`.</span><span class="sxs-lookup"><span data-stu-id="146d5-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="146d5-110">Model jest tworzony w bloku `@code` składnika i przechowywany w polu prywatnym (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="146d5-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="146d5-111">Pole jest przypisane do atrybutu `Model` elementu `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="146d5-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="146d5-112">Składnik `DataAnnotationsValidator` dołącza obsługę walidacji przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="146d5-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="146d5-113">Składnik `ValidationSummary` podsumowuje komunikaty weryfikacyjne.</span><span class="sxs-lookup"><span data-stu-id="146d5-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="146d5-114">`HandleValidSubmit` jest wyzwalany po pomyślnym przesłaniu formularza (kończy walidację).</span><span class="sxs-lookup"><span data-stu-id="146d5-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="146d5-115">Zestaw wbudowanych składników wejściowych jest dostępny do odbierania i weryfikowania danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="146d5-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="146d5-116">Dane wejściowe są weryfikowane po ich zmianie i po przesłaniu formularza.</span><span class="sxs-lookup"><span data-stu-id="146d5-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="146d5-117">W poniższej tabeli przedstawiono dostępne składniki danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="146d5-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="146d5-118">Składnik wejściowy</span><span class="sxs-lookup"><span data-stu-id="146d5-118">Input component</span></span> | <span data-ttu-id="146d5-119">Renderowane jako&hellip;</span><span class="sxs-lookup"><span data-stu-id="146d5-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="146d5-120">Wszystkie składniki danych wejściowych, w tym `EditForm`, obsługują dowolne atrybuty.</span><span class="sxs-lookup"><span data-stu-id="146d5-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="146d5-121">Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest dodawany do renderowanego elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="146d5-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="146d5-122">Składniki wejściowe zapewniają domyślne zachowanie podczas sprawdzania poprawności edycji i zmiany ich klasy CSS, aby odzwierciedlały stan pola.</span><span class="sxs-lookup"><span data-stu-id="146d5-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="146d5-123">Niektóre składniki obejmują przydatne logiki analizy.</span><span class="sxs-lookup"><span data-stu-id="146d5-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="146d5-124">Na przykład `InputDate` i `InputNumber` obsłużyć bezproblemowo przeanalizować wartości, rejestrując je jako błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="146d5-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="146d5-125">Typy, które mogą akceptować wartości null, obsługują również wartość null pola docelowego (na przykład `int?`).</span><span class="sxs-lookup"><span data-stu-id="146d5-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="146d5-126">Następujący typ `Starship` definiuje logikę walidacji przy użyciu większego zestawu właściwości i adnotacji danych niż wcześniejsza `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="146d5-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="146d5-127">W poprzednim przykładzie `Description` jest opcjonalne, ponieważ nie są obecne adnotacje danych.</span><span class="sxs-lookup"><span data-stu-id="146d5-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="146d5-128">Następujący formularz sprawdza poprawność danych wejściowych użytkownika przy użyciu weryfikacji zdefiniowanej w modelu `Starship`:</span><span class="sxs-lookup"><span data-stu-id="146d5-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="146d5-129">`EditForm` tworzy `EditContext` jako [wartość kaskadową](xref:blazor/components#cascading-values-and-parameters) , która śledzi metadane dotyczące procesu edycji, w tym pola, które zostały zmodyfikowane, i bieżące komunikaty weryfikacyjne.</span><span class="sxs-lookup"><span data-stu-id="146d5-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="146d5-130">`EditForm` również zapewnia wygodne zdarzenia dla prawidłowych i nieprawidłowych przesyłania (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="146d5-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="146d5-131">Alternatywnie możesz użyć `OnSubmit`, aby wyzwolić walidację i sprawdzanie wartości pól przy użyciu niestandardowego kodu sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="146d5-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="146d5-132">InputText na podstawie zdarzenia wejściowego</span><span class="sxs-lookup"><span data-stu-id="146d5-132">InputText based on the input event</span></span>

<span data-ttu-id="146d5-133">Użyj składnika `InputText`, aby utworzyć niestandardowy składnik, który używa zdarzenia `input` zamiast zdarzenia `change`.</span><span class="sxs-lookup"><span data-stu-id="146d5-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="146d5-134">Utwórz składnik z następującą adiustacją i użyj składnika, tak jak `InputText` jest używany:</span><span class="sxs-lookup"><span data-stu-id="146d5-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="146d5-135">Obsługa walidacji</span><span class="sxs-lookup"><span data-stu-id="146d5-135">Validation support</span></span>

<span data-ttu-id="146d5-136">Składnik `DataAnnotationsValidator` dołącza obsługę walidacji przy użyciu adnotacji danych do `EditContext`kaskadowo.</span><span class="sxs-lookup"><span data-stu-id="146d5-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="146d5-137">Włączenie obsługi walidacji przy użyciu adnotacji danych wymaga tego jawnego gestu.</span><span class="sxs-lookup"><span data-stu-id="146d5-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="146d5-138">Aby użyć innego systemu sprawdzania poprawności niż adnotacje danych, Zastąp `DataAnnotationsValidator` implementacją niestandardową.</span><span class="sxs-lookup"><span data-stu-id="146d5-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="146d5-139">Implementacja ASP.NET Core jest dostępna do inspekcji w źródle referencyjnym: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="146d5-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

<span data-ttu-id="146d5-140">Składnik `ValidationSummary` podsumowuje wszystkie komunikaty weryfikacyjne podobne do [pomocnika tagów podsumowania walidacji](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="146d5-140">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="146d5-141">Składnik `ValidationMessage` wyświetla komunikaty weryfikacyjne dla określonego pola, który jest podobny do [pomocnika tagów komunikatu weryfikacji](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="146d5-141">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="146d5-142">Określ pole do walidacji z atrybutem `For` i wyrażenie lambda, które nazywa właściwość modelu:</span><span class="sxs-lookup"><span data-stu-id="146d5-142">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="146d5-143">Składniki `ValidationMessage` i `ValidationSummary` obsługują dowolne atrybuty.</span><span class="sxs-lookup"><span data-stu-id="146d5-143">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="146d5-144">Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest dodawany do wygenerowanego elementu `<div>` lub `<ul>`.</span><span class="sxs-lookup"><span data-stu-id="146d5-144">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="146d5-145">**Microsoft. AspNetCore. Blazor. DataAnnotations. Validation — pakiet**</span><span class="sxs-lookup"><span data-stu-id="146d5-145">**Microsoft.AspNetCore.Blazor.DataAnnotations.Validation package**</span></span>

<span data-ttu-id="146d5-146">[Microsoft. AspNetCore. Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) to pakiet, który wypełnia luki w środowisku walidacji przy użyciu składnika `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="146d5-146">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="146d5-147">Pakiet jest obecnie *eksperymentalny*i planujemy dodać te scenariusze do ASP.NET Core Framework w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="146d5-147">The package is currently *experimental*, and we plan to add these scenarios into the ASP.NET Core framework in a future release.</span></span>

<span data-ttu-id="146d5-148">Składnik `DataAnnotationsValidator` nie weryfikuje podwłaściwości złożonych właściwości w modelu walidacji.</span><span class="sxs-lookup"><span data-stu-id="146d5-148">The `DataAnnotationsValidator` component doesn't validate subproperties of complex properties on a validating model.</span></span> <span data-ttu-id="146d5-149">Elementy właściwości typu kolekcji nie są weryfikowane.</span><span class="sxs-lookup"><span data-stu-id="146d5-149">Items of collection-type properties aren't validated.</span></span> <span data-ttu-id="146d5-150">Aby sprawdzić poprawność tych typów, pakiet `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` wprowadza atrybut walidacji `ValidateComplexType`, który działa wspólnie ze składnikiem `ObjectGraphDataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="146d5-150">To validate these types, the `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` package introduces the `ValidateComplexType` validation attribute that works in tandem with the `ObjectGraphDataAnnotationsValidator` component.</span></span> <span data-ttu-id="146d5-151">Aby zapoznać się z przykładem tych typów, zobacz [przykład Blazor Validation w repozytorium GitHub/Samples ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span><span class="sxs-lookup"><span data-stu-id="146d5-151">For an example of these types in use, see the [Blazor Validation sample in the aspnet/samples GitHub repository ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

<span data-ttu-id="146d5-152"><xref:System.ComponentModel.DataAnnotations.CompareAttribute> nie działa dobrze ze składnikiem `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="146d5-152">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="146d5-153">Pakiet `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` wprowadza dodatkowy atrybut sprawdzania poprawności, `ComparePropertyAttribute`, który działa wokół tych ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="146d5-153">The `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="146d5-154">W aplikacji Blazor `ComparePropertyAttribute` jest bezpośrednią wymianą dla `CompareAttribute`.</span><span class="sxs-lookup"><span data-stu-id="146d5-154">In a Blazor app, `ComparePropertyAttribute` is a direct replacement for the `CompareAttribute`.</span></span> <span data-ttu-id="146d5-155">Aby uzyskać więcej informacji, zobacz [CompareAttribute został zignorowany z OnValidSubmit EditForm (ASPNET/AspNetCore \#10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).</span><span class="sxs-lookup"><span data-stu-id="146d5-155">For more information, see [CompareAttribute ignored with OnValidSubmit EditForm (aspnet/AspNetCore \#10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="146d5-156">Walidacja właściwości typu złożonego lub kolekcji</span><span class="sxs-lookup"><span data-stu-id="146d5-156">Validation of complex or collection type properties</span></span>

<span data-ttu-id="146d5-157">Atrybuty walidacji zastosowane do właściwości modelu sprawdzają poprawność podczas przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="146d5-157">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="146d5-158">Jednak właściwości kolekcji lub złożonych typów danych modelu nie są sprawdzane w wyniku przesłania formularza przez składnik `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="146d5-158">However, the properties of collections or complex data types of a model aren't validated on form submission by the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="146d5-159">Aby przestrzegać zagnieżdżonych atrybutów walidacji w tym scenariuszu, Użyj niestandardowego składnika walidacji.</span><span class="sxs-lookup"><span data-stu-id="146d5-159">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="146d5-160">Aby zapoznać się z przykładem, zobacz przykład [Blazor Validation w repozytorium GitHub/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span><span class="sxs-lookup"><span data-stu-id="146d5-160">For an example, see the [Blazor Validation sample in the aspnet/samples GitHub repository](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

::: moniker-end
