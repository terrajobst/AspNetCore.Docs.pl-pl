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

* Formularz sprawdza poprawność danych wejściowych użytkownika w polu `name` przy użyciu walidacji zdefiniowanej w typie `ExampleModel`. Model jest tworzony w bloku `@code` składnika i przechowywany w polu prywatnym (`exampleModel`). Pole jest przypisane do atrybutu `Model` elementu `<EditForm>`.
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

`EditForm` tworzy `EditContext` jako [wartość kaskadową](xref:blazor/components#cascading-values-and-parameters) , która śledzi metadane dotyczące procesu edycji, w tym pola, które zostały zmodyfikowane, i bieżące komunikaty weryfikacyjne. `EditForm` również zapewnia wygodne zdarzenia dla prawidłowych i nieprawidłowych przesyłania (`OnValidSubmit`, `OnInvalidSubmit`). Alternatywnie możesz użyć `OnSubmit`, aby wyzwolić walidację i sprawdzanie wartości pól przy użyciu niestandardowego kodu sprawdzania poprawności.

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

::: moniker range=">= aspnetcore-3.1"

### <a name="opno-locblazor-data-annotations-validation-package"></a>Pakiet weryfikacji adnotacji danych Blazor

[Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) to pakiet, który wypełnia luki w działaniu sprawdzania poprawności przy użyciu składnika `DataAnnotationsValidator`. Pakiet jest obecnie *eksperymentalny*.

### <a name="compareproperty-attribute"></a>[CompareProperty] — atrybut

<xref:System.ComponentModel.DataAnnotations.CompareAttribute> nie działa dobrze ze składnikiem `DataAnnotationsValidator`. [Microsoft. AspNetCore.Blazor. .](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) Pakiet *eksperymentalny* walidacji wprowadza dodatkowy atrybut walidacji, `ComparePropertyAttribute`, który działa wokół tych ograniczeń. W aplikacji Blazor `[CompareProperty]` jest bezpośrednią wymianą dla atrybutu `[Compare]`. Aby uzyskać więcej informacji, zobacz [CompareAttribute został zignorowany z OnValidSubmit EditForm (dotnet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).

### <a name="nested-models-collection-types-and-complex-types"></a>Modele zagnieżdżone, typy kolekcji i typy złożone

Blazor zapewnia obsługę sprawdzania poprawności danych wejściowych formularza przy użyciu adnotacji danych z wbudowaną `DataAnnotationsValidator`. Jednak `DataAnnotationsValidator` sprawdza tylko poprawność właściwości najwyższego poziomu modelu powiązanego z formularzem, który nie jest właściwościami typu "Collection" lub "Complex".

Aby sprawdzić poprawność całego grafu obiektów modelu powiązanego, w tym właściwości kolekcji i typu złożonego, użyj `ObjectGraphDataAnnotationsValidator` dostarczonej przez *eksperymentalny* [Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) — pakiet:

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
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

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Walidacja właściwości typu złożonego lub kolekcji

Atrybuty walidacji zastosowane do właściwości modelu sprawdzają poprawność podczas przesyłania formularza. Jednak właściwości kolekcji lub złożonych typów danych modelu nie są sprawdzane w wyniku przesłania formularza przez składnik `DataAnnotationsValidator`. Aby przestrzegać zagnieżdżonych atrybutów walidacji w tym scenariuszu, Użyj niestandardowego składnika walidacji. Aby zapoznać się z przykładem, zobacz [przykładoweBlazor walidacji (ASPNET/Samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

::: moniker-end
