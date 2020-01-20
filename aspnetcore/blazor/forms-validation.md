---
title: ASP.NET Core Blazor formularzy i walidacji
author: guardrex
description: Dowiedz się, jak używać scenariuszy formularzy i walidacji pól w Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/forms-validation
ms.openlocfilehash: 6f6fdc13dbb754ecfe06025d496017d3c16951fe
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159966"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a>ASP.NET Core Blazor formularzy i walidacji

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

Formularze i walidacje są obsługiwane w Blazor przy użyciu [adnotacji danych](xref:mvc/models/validation).

Następujący typ `ExampleModel` definiuje logikę walidacji przy użyciu adnotacji danych:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Formularz jest definiowany przy użyciu składnika `EditForm`. W poniższej formie przedstawiono typowe elementy, składniki i kod Razor:

```razor
<EditForm Model="@exampleModel" OnValidSubmit="HandleValidSubmit">
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

W poprzednim przykładzie:

* Formularz sprawdza poprawność danych wejściowych użytkownika w polu `name` przy użyciu walidacji zdefiniowanej w typie `ExampleModel`. Model jest tworzony w bloku `@code` składnika i przechowywany w polu prywatnym (`exampleModel`). Pole jest przypisane do atrybutu `Model` elementu `<EditForm>`.
* `@bind-Value` powiązań składnika `InputText`:
  * Właściwość modelu (`exampleModel.Name`) do właściwości `Value` składnika `InputText`.
  * Delegowanie zdarzenia zmiany do właściwości `ValueChanged` składnika `InputText`.
* Składnik `DataAnnotationsValidator` dołącza obsługę walidacji przy użyciu adnotacji danych.
* Składnik `ValidationSummary` podsumowuje komunikaty weryfikacyjne.
* `HandleValidSubmit` jest wyzwalany po pomyślnym przesłaniu formularza (kończy walidację).

Zestaw wbudowanych składników wejściowych jest dostępny do odbierania i weryfikowania danych wejściowych użytkownika. Dane wejściowe są weryfikowane po ich zmianie i po przesłaniu formularza. W poniższej tabeli przedstawiono dostępne składniki danych wejściowych.

| Składnik wejściowy | Renderowane jako&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

Wszystkie składniki danych wejściowych, w tym `EditForm`, obsługują dowolne atrybuty. Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest dodawany do renderowanego elementu HTML.

Składniki wejściowe zapewniają domyślne zachowanie podczas sprawdzania poprawności edycji i zmiany ich klasy CSS, aby odzwierciedlały stan pola. Niektóre składniki obejmują przydatne logiki analizy. Na przykład `InputDate` i `InputNumber` obsłużyć bezproblemowo przeanalizować wartości, rejestrując je jako błędy walidacji. Typy, które mogą akceptować wartości null, obsługują również wartość null pola docelowego (na przykład `int?`).

Następujący typ `Starship` definiuje logikę walidacji przy użyciu większego zestawu właściwości i adnotacji danych niż wcześniejsza `ExampleModel`:

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

W poprzednim przykładzie `Description` jest opcjonalne, ponieważ nie są obecne adnotacje danych.

Następujący formularz sprawdza poprawność danych wejściowych użytkownika przy użyciu weryfikacji zdefiniowanej w modelu `Starship`:

```razor
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label>
            Identifier:
            <InputText @bind-Value="starship.Identifier" />
        </label>
    </p>
    <p>
        <label>
            Description (optional):
            <InputTextArea @bind-Value="starship.Description" />
        </label>
    </p>
    <p>
        <label>
            Primary Classification:
            <InputSelect @bind-Value="starship.Classification">
                <option value="">Select classification ...</option>
                <option value="Exploration">Exploration</option>
                <option value="Diplomacy">Diplomacy</option>
                <option value="Defense">Defense</option>
            </InputSelect>
        </label>
    </p>
    <p>
        <label>
            Maximum Accommodation:
            <InputNumber @bind-Value="starship.MaximumAccommodation" />
        </label>
    </p>
    <p>
        <label>
            Engineering Approval:
            <InputCheckbox @bind-Value="starship.IsValidatedDesign" />
        </label>
    </p>
    <p>
        <label>
            Production Date:
            <InputDate @bind-Value="starship.ProductionDate" />
        </label>
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

`EditForm` tworzy `EditContext` jako [wartość kaskadową](xref:blazor/components#cascading-values-and-parameters) , która śledzi metadane dotyczące procesu edycji, w tym pola, które zostały zmodyfikowane, i bieżące komunikaty weryfikacyjne. `EditForm` również zapewnia wygodne zdarzenia dla prawidłowych i nieprawidłowych przesyłania (`OnValidSubmit`, `OnInvalidSubmit`). Alternatywnie możesz użyć `OnSubmit`, aby wyzwolić walidację i sprawdzanie wartości pól przy użyciu niestandardowego kodu sprawdzania poprawności.

W poniższym przykładzie:

* Metoda `HandleSubmit` jest uruchamiana po wybraniu przycisku **Prześlij** .
* Formularz zostanie sprawdzony przy użyciu `EditContext`formularza.
* Formularz jest weryfikowany przez przekazanie `EditContext` do metody `ServerValidate`, która wywołuje punkt końcowy interfejsu API sieci Web na serwerze (*niepokazywany*).
* Dodatkowy kod jest uruchamiany w zależności od wyniku weryfikacji po stronie klienta i serwera, sprawdzając `isValid`.

```razor
<EditForm EditContext="@editContext" OnSubmit="@HandleSubmit">

    ...

    <button type="submit">Submit</button>
</EditForm>

@code {
    private Starship starship = new Starship();
    private EditContext editContext;

    protected override void OnInitialized()
    {
        editContext = new EditContext(starship);
    }

    private async Task HandleSubmit()
    {
        var isValid = editContext.Validate() && 
            await ServerValidate(editContext);

        if (isValid)
        {
            ...
        }
        else
        {
            ...
        }
    }

    private async Task<bool> ServerValidate(EditContext editContext)
    {
        var serverChecksValid = ...

        return serverChecksValid;
    }
}
```

## <a name="inputtext-based-on-the-input-event"></a>InputText na podstawie zdarzenia wejściowego

Użyj składnika `InputText`, aby utworzyć niestandardowy składnik, który używa zdarzenia `input` zamiast zdarzenia `change`.

Utwórz składnik z następującą adiustacją i użyj składnika, tak jak `InputText` jest używany:

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a>Pracuj z przyciskami radiowymi

Podczas pracy z przyciskami radiowymi w formularzu powiązanie danych jest obsługiwane inaczej niż inne elementy, ponieważ przyciski radiowe są oceniane jako Grupa. Wartość każdego przycisku radiowego jest stała, ale wartość grupy przycisków radiowych jest wartością wybranego przycisku radiowego. W poniższym przykładzie przedstawiono sposób:

* Obsługa powiązania danych dla grupy przycisków radiowych.
* Obsługa walidacji przy użyciu niestandardowego składnika `InputRadio`.

```razor
@using System.Globalization
@typeparam TValue
@inherits InputBase<TValue>

<input @attributes="AdditionalAttributes" type="radio" value="@SelectedValue" 
       checked="@(SelectedValue.Equals(Value))" @onchange="OnChange" />

@code {
    [Parameter]
    public TValue SelectedValue { get; set; }

    private void OnChange(ChangeEventArgs args)
    {
        CurrentValueAsString = args.Value.ToString();
    }

    protected override bool TryParseValueFromString(string value, 
        out TValue result, out string errorMessage)
    {
        var success = BindConverter.TryConvertTo<TValue>(
            value, CultureInfo.CurrentCulture, out var parsedValue);
        if (success)
        {
            result = parsedValue;
            errorMessage = null;

            return true;
        }
        else
        {
            result = default;
            errorMessage = $"{FieldIdentifier.FieldName} field isn't valid.";

            return false;
        }
    }
}
```

Poniższy `EditForm` używa poprzedniego składnika `InputRadio` w celu uzyskania i zweryfikowania klasyfikacji od użytkownika:

```razor
@page "/RadioButtonExample"
@using System.ComponentModel.DataAnnotations

<h1>Radio Button Group Test</h1>

<EditForm Model="model" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    @for (int i = 1; i <= 5; i++)
    {
        <label>
            <InputRadio name="rate" SelectedValue="i" @bind-Value="model.Rating" />
            @i
        </label>
    }

    <button type="submit">Submit</button>
</EditForm>

<p>You chose: @model.Rating</p>

@code {
    private Model model = new Model();

    private void HandleValidSubmit()
    {
        Console.WriteLine("valid");
    }

    public class Model
    {
        [Range(1, 5)]
        public int Rating { get; set; }
    }
}
```

## <a name="validation-support"></a>Obsługa walidacji

Składnik `DataAnnotationsValidator` dołącza obsługę walidacji przy użyciu adnotacji danych do `EditContext`kaskadowo. Włączenie obsługi walidacji przy użyciu adnotacji danych wymaga tego jawnego gestu. Aby użyć innego systemu sprawdzania poprawności niż adnotacje danych, Zastąp `DataAnnotationsValidator` implementacją niestandardową. Implementacja ASP.NET Core jest dostępna do inspekcji w źródle referencyjnym: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Blazor wykonuje dwa typy walidacji:

* *Sprawdzanie poprawności pola* jest wykonywane, gdy użytkownik wyprowadzi karty z pola. Podczas sprawdzania poprawności pola składnik `DataAnnotationsValidator` kojarzy wszystkie zgłoszone wyniki walidacji z polem.
* *Sprawdzanie poprawności modelu* jest wykonywane, gdy użytkownik prześle formularz. Podczas walidacji modelu składnik `DataAnnotationsValidator` próbuje określić pole na podstawie nazwy elementu członkowskiego, który raportuje wynik sprawdzania poprawności. Wyniki walidacji, które nie są skojarzone z poszczególnymi elementami członkowskimi, są skojarzone z modelem, a nie polem.

### <a name="validation-summary-and-validation-message-components"></a>Składniki podsumowania walidacji i komunikatów weryfikacji

Składnik `ValidationSummary` podsumowuje wszystkie komunikaty weryfikacyjne podobne do [pomocnika tagów podsumowania walidacji](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):

```razor
<ValidationSummary />
```

Wyprowadź komunikaty weryfikacji dla określonego modelu z parametrem `Model`:
  
```razor
<ValidationSummary Model="@starship" />
```

Składnik `ValidationMessage` wyświetla komunikaty weryfikacyjne dla określonego pola, który jest podobny do [pomocnika tagów komunikatu weryfikacji](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Określ pole do walidacji z atrybutem `For` i wyrażenie lambda, które nazywa właściwość modelu:

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

Składniki `ValidationMessage` i `ValidationSummary` obsługują dowolne atrybuty. Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest dodawany do wygenerowanego elementu `<div>` lub `<ul>`.

### <a name="custom-validation-attributes"></a>Niestandardowe atrybuty walidacji

Aby upewnić się, że wynik walidacji jest prawidłowo skojarzony z polem przy użyciu [niestandardowego atrybutu walidacji](xref:mvc/models/validation#custom-attributes), należy przekazać <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> kontekstu walidacji podczas tworzenia <xref:System.ComponentModel.DataAnnotations.ValidationResult>:

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>Pakiet weryfikacji adnotacji danych Blazor

[Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) to pakiet, który wypełnia luki w działaniu sprawdzania poprawności przy użyciu składnika `DataAnnotationsValidator`. Pakiet jest obecnie *eksperymentalny*.

### <a name="compareproperty-attribute"></a>[CompareProperty] — atrybut

<xref:System.ComponentModel.DataAnnotations.CompareAttribute> nie działa dobrze ze składnikiem `DataAnnotationsValidator`, ponieważ nie kojarzy wyniku walidacji z określonym elementem członkowskim. Może to spowodować niespójne zachowanie między walidacją na poziomie pola i po sprawdzeniu poprawności całego modelu podczas przesyłania. [Microsoft. AspNetCore.Blazor. .](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) Pakiet *eksperymentalny* walidacji wprowadza dodatkowy atrybut walidacji, `ComparePropertyAttribute`, który działa wokół tych ograniczeń. W aplikacji Blazor `[CompareProperty]` jest bezpośrednią wymianą dla atrybutu `[Compare]`.

### <a name="nested-models-collection-types-and-complex-types"></a>Modele zagnieżdżone, typy kolekcji i typy złożone

Blazor zapewnia obsługę sprawdzania poprawności danych wejściowych formularza przy użyciu adnotacji danych z wbudowaną `DataAnnotationsValidator`. Jednak `DataAnnotationsValidator` sprawdza tylko poprawność właściwości najwyższego poziomu modelu powiązanego z formularzem, który nie jest właściwościami typu "Collection" lub "Complex".

Aby sprawdzić poprawność całego grafu obiektów modelu powiązanego, w tym właściwości kolekcji i typu złożonego, użyj `ObjectGraphDataAnnotationsValidator` dostarczonej przez *eksperymentalny* [Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) — pakiet:

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

Dodawanie adnotacji do właściwości modelu za pomocą `[ValidateComplexType]`. W poniższych klasach modelu Klasa `ShipDescription` zawiera dodatkowe adnotacje danych do zweryfikowania, gdy model jest powiązany z formularzem:

*Starship.cs*:

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

*ShipDescription.cs*:

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

### <a name="enable-the-submit-button-based-on-form-validation"></a>Włączanie przycisku przesyłania na podstawie walidacji formularza

Aby włączyć i wyłączyć przycisk Prześlij na podstawie walidacji formularza:

* Użyj `EditContext` formularza, aby przypisać model, gdy składnik zostanie zainicjowany.
* Sprawdź poprawność formularza w `OnFieldChanged` wywołanie zwrotne kontekstu, aby włączyć i wyłączyć przycisk Prześlij.

```razor
<EditForm EditContext="@editContext">
    <DataAnnotationsValidator />
    <ValidationSummary />

    ...

    <button type="submit" disabled="@formInvalid">Submit</button>
</EditForm>

@code {
    private Starship starship = new Starship();
    private bool formInvalid = true;
    private EditContext editContext;

    protected override void OnInitialized()
    {
        editContext = new EditContext(starship);

        editContext.OnFieldChanged += (_, __) =>
        {
            formInvalid = !editContext.Validate();
            StateHasChanged();
        };
    }
}
```

W poprzednim przykładzie Ustaw `formInvalid` na `false`, jeśli:

* Formularz jest wstępnie załadowany z prawidłowymi wartościami domyślnymi.
* Przycisk Prześlij ma być włączony podczas ładowania formularza.

Efektem ubocznym poprzedniego podejścia jest to, że składnik `ValidationSummary` jest wypełniany przy użyciu nieprawidłowych pól, gdy użytkownik przeprowadzi interakcję z jednym polem. Ten scenariusz można rozwiązać w jeden z następujących sposobów:

* Nie używaj składnika `ValidationSummary` w formularzu.
* Ustaw składnik `ValidationSummary` widoczny po wybraniu przycisku Prześlij (na przykład w metodzie `HandleValidSubmit`).

```razor
<EditForm EditContext="@editContext" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary style="@displaySummary" />

    ...

    <button type="submit" disabled="@formInvalid">Submit</button>
</EditForm>

@code {
    private string displaySummary = "display:none";

    ...

    private void HandleValidSubmit()
    {
        displaySummary = "display:block";
    }
}
```
