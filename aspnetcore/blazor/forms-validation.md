---
title: ASP.NET Core Blazor formularze i Walidacja
author: guardrex
description: Dowiedz się, jak używać formularzy i scenariusze weryfikacji pola w Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/forms-validation
ms.openlocfilehash: e1b7de6e31adae8102bbefba5d08418c4daac687
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855781"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core Blazor formularze i Walidacja

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

Formularze i Walidacja są obsługiwane w Blazor przy użyciu [adnotacje danych](xref:mvc/models/validation).

> [!NOTE]
> Formularze i scenariusze weryfikacji prawdopodobnie ulegną zmianie po każdym wydaniu wersji zapoznawczej.

Następujące `ExampleModel` typ definiuje logikę weryfikacji przy użyciu adnotacji danych:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Formularz jest definiowana za pomocą `EditForm` składnika. Następującą postać przedstawia typowe elementy, składników i kod Razor:

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

* Formularz sprawdza poprawność danych wejściowych użytkownika w `name` pola za pomocą weryfikacji zdefiniowane w `ExampleModel` typu. Model jest tworzony w składniku `@code` zablokować, a także przechowywane w pole prywatne (`exampleModel`). Pole jest przypisany do `Model` atrybutu `<EditForm>` elementu.
* `DataAnnotationsValidator` Składnika dołącza Obsługa weryfikacji przy użyciu adnotacji danych.
* `ValidationSummary` Składnik znajduje się podsumowanie komunikatów dotyczących sprawdzania poprawności.
* `HandleValidSubmit` jest wyzwalany, gdy formularz pomyślnie przesyła (przebiegów weryfikacji).

Zestaw wbudowanych składników danych wejściowych są dostępne do odbierania i weryfikowanie danych wejściowych użytkownika. Dane wejściowe są prawidłowe, gdy są one zmienione i podczas przesyłania formularza. Dostępne składniki wejściowe są pokazane w poniższej tabeli.

| Składnika danych wejściowych | Renderowane jako&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

Wszystkich składników danych wejściowych, włącznie z `EditForm`, obsługuje dowolne atrybutów. Dowolny atrybut, który nie jest zgodny z parametrem składnika jest dodawany do renderowanego elementu HTML.

Składniki danych wejściowych udostępniają domyślne zachowanie sprawdzania poprawności po edycji i zmienianie ich klasy CSS, aby odzwierciedlić stan pola. Niektóre składniki zawierają przydatne podczas analizowania logiki. Na przykład `InputDate` i `InputNumber` bezpiecznie obsłużyć niemożliwy do przeanalizowania wartości, rejestrując je jako błędy sprawdzania poprawności. Typy, które może akceptować wartości null obsługuje również dopuszczanie wartości null dla pola docelowego (na przykład `int?`).

Następujące `Starship` typ definiuje logikę weryfikacji za pomocą większy zbiór właściwości i danych adnotacje niż wcześniej `ExampleModel`:

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

W powyższym przykładzie `Description` jest opcjonalny, ponieważ brak adnotacji danych.

Następującą postać weryfikuje użytkownika danych wejściowych za pomocą weryfikacji zdefiniowane w `Starship` modelu:

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
        <InputTextArea Id="description" @bind-Value="starship.Description" />
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
        <InputDate Id="productionDate" @bind-Value="starship.ProductionDate" />
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

`EditForm` Tworzy `EditContext` jako [cascading wartość](xref:blazor/components#cascading-values-and-parameters) , śledzi metadane dotyczące procesu edycji, takie jak pola, które zostały zmodyfikowane oraz bieżące wiadomości sprawdzania poprawności. `EditForm` Udostępnia również wygodną zdarzenia dla prawidłowe i nieprawidłowe przesyła (`OnValidSubmit`, `OnInvalidSubmit`). Można również użyć `OnSubmit` wyzwolić weryfikacji i sprawdź wartości pola z kodu niestandardowego sprawdzania poprawności.

`DataAnnotationsValidator` Składnika dołącza Obsługa weryfikacji przy użyciu adnotacji danych w celu kaskadowy `EditContext`. Włączanie obsługi sprawdzania poprawności obecnie przy użyciu adnotacji danych wymaga to jawne gestu, ale rozważamy, dzięki czemu to zachowanie domyślne, które można przesłonić. Aby korzystać z systemu weryfikacji innego niż adnotacje danych, należy zastąpić `DataAnnotationsValidator` przy użyciu niestandardowych implementacji. Implementacja platformy ASP.NET Core jest dostępny do kontroli źródła odwołania: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs). *Implementacja programu ASP.NET Core podlega szybkiej aktualizacji w wersji zapoznawczej.*

`ValidationSummary` Składnika znajduje się podsumowanie wszystkich komunikatów o weryfikacji, który przypomina [Pomocnik tagu podsumowania sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

`ValidationMessage` Składnik wyświetla komunikaty sprawdzania poprawności dla określonego pola, które są podobne do [Pomocnik tagu komunikat sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Należy określić to pole do weryfikacji za pomocą `For` atrybutu i wyrażenia lambda nazewnictwa właściwości modelu:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

`ValidationMessage` i `ValidationSummary` składniki obsługują dowolnymi atrybutami. Dowolny atrybut, który nie jest zgodny z parametrem składnik zostanie dodany do wygenerowany `<div>` lub `<ul>` elementu.
