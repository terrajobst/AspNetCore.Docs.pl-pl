---
title: ASP.NET Core formularzy i walidacji Blazor
author: guardrex
description: Dowiedz się, jak używać scenariuszy formularzy i walidacji pól w Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: blazor/forms-validation
ms.openlocfilehash: 6dcc36c5133367493b476655dbdf73b75db9d168
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905733"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core formularzy i walidacji Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

Formularze i walidacje są obsługiwane w programie Blazor przy użyciu [adnotacji danych](xref:mvc/models/validation).

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

`EditForm` tworzy `EditContext` jako [wartość kaskadową](xref:blazor/components#cascading-values-and-parameters) , która śledzi metadane dotyczące procesu edycji, w tym pola, które zostały zmodyfikowane, i bieżące komunikaty weryfikacyjne. `EditForm` również zapewnia wygodne zdarzenia dla prawidłowych i nieprawidłowych przesyłania (`OnValidSubmit`, `OnInvalidSubmit`). Alternatywnie możesz użyć `OnSubmit`, aby wyzwolić walidację i sprawdzanie wartości pól przy użyciu niestandardowego kodu sprawdzania poprawności.

## <a name="inputtext-based-on-the-input-event"></a>InputText na podstawie zdarzenia wejściowego

Użyj składnika `InputText`, aby utworzyć niestandardowy składnik, który używa zdarzenia `input` zamiast zdarzenia `change`.

Utwórz składnik z następującą adiustacją i użyj składnika, tak jak `InputText` jest używany:

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Obsługa walidacji

Składnik `DataAnnotationsValidator` dołącza obsługę walidacji przy użyciu adnotacji danych do `EditContext`kaskadowo. Włączenie obsługi walidacji przy użyciu adnotacji danych wymaga tego jawnego gestu. Aby użyć innego systemu sprawdzania poprawności niż adnotacje danych, Zastąp `DataAnnotationsValidator` implementacją niestandardową. Implementacja ASP.NET Core jest dostępna do inspekcji w źródle referencyjnym: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Składnik `ValidationSummary` podsumowuje wszystkie komunikaty weryfikacyjne podobne do [pomocnika tagów podsumowania walidacji](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

Składnik `ValidationMessage` wyświetla komunikaty weryfikacyjne dla określonego pola, który jest podobny do [pomocnika tagów komunikatu weryfikacji](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Określ pole do walidacji z atrybutem `For` i wyrażenie lambda, które nazywa właściwość modelu:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

Składniki `ValidationMessage` i `ValidationSummary` obsługują dowolne atrybuty. Dowolny atrybut, który nie jest zgodny z parametrem składnika, jest dodawany do wygenerowanego elementu `<div>` lub `<ul>`.

::: moniker range=">= aspnetcore-3.1"

**Microsoft. AspNetCore. Blazor. DataAnnotations. Validation — pakiet**

[Microsoft. AspNetCore. Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) to pakiet, który wypełnia luki w środowisku walidacji przy użyciu składnika `DataAnnotationsValidator`. Pakiet jest obecnie *eksperymentalny*i planujemy dodać te scenariusze do ASP.NET Core Framework w przyszłej wersji.

Składnik `DataAnnotationsValidator` nie weryfikuje podwłaściwości złożonych właściwości w modelu walidacji. Elementy właściwości typu kolekcji nie są weryfikowane. Aby sprawdzić poprawność tych typów, pakiet `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` wprowadza atrybut walidacji `ValidateComplexType`, który działa wspólnie ze składnikiem `ObjectGraphDataAnnotationsValidator`. Aby zapoznać się z przykładem tych typów, zobacz [przykład Blazor Validation w repozytorium GitHub/Samples ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

<xref:System.ComponentModel.DataAnnotations.CompareAttribute> nie działa dobrze ze składnikiem `DataAnnotationsValidator`. Pakiet `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` wprowadza dodatkowy atrybut sprawdzania poprawności, `ComparePropertyAttribute`, który działa wokół tych ograniczeń. W aplikacji Blazor `ComparePropertyAttribute` jest bezpośrednią wymianą dla `CompareAttribute`. Aby uzyskać więcej informacji, zobacz [CompareAttribute został zignorowany z OnValidSubmit EditForm (ASPNET/AspNetCore \#10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Walidacja właściwości typu złożonego lub kolekcji

Atrybuty walidacji zastosowane do właściwości modelu sprawdzają poprawność podczas przesyłania formularza. Jednak właściwości kolekcji lub złożonych typów danych modelu nie są sprawdzane w wyniku przesłania formularza przez składnik `DataAnnotationsValidator`. Aby przestrzegać zagnieżdżonych atrybutów walidacji w tym scenariuszu, Użyj niestandardowego składnika walidacji. Aby zapoznać się z przykładem, zobacz przykład [Blazor Validation w repozytorium GitHub/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

::: moniker-end
