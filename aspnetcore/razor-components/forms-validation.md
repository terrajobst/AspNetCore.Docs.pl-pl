---
title: Razor składniki formularze i Walidacja
author: guardrex
description: Dowiedz się, jak używać formularzy i scenariusze weryfikacji pola w składnikach Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/forms-validation
ms.openlocfilehash: a4ed75efc6b5a733ce4ff4e82f354a8e2fd48500
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468519"
---
# <a name="razor-components-forms-and-validation"></a>Razor składniki formularze i Walidacja

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

Formularze i Walidacja są obsługiwane w składnikach Razor przy użyciu [adnotacje danych](xref:mvc/models/validation).

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

Formularz jest definiowana za pomocą `<EditForm>` składnika. Następującą postać przedstawia typowe elementy, składników i kod Razor:

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@functions {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* Formularz sprawdza poprawność danych wejściowych użytkownika w `name` pola za pomocą weryfikacji zdefiniowane w `ExampleModel` typu. Model jest tworzony w składniku `@functions` zablokować, a także przechowywane w pole prywatne (`exampleModel`). Pole jest przypisany do `Model` atrybutu `<EditForm>`.
* Składnik weryfikacji adnotacji danych (`<DataAnnotationsValidator>`) dołącza Obsługa weryfikacji przy użyciu adnotacji danych.
* Składnik podsumowania sprawdzania poprawności (`<ValidationSummary>`) znajduje się podsumowanie komunikatów dotyczących sprawdzania poprawności.
* `HandleValidSubmit` jest wyzwalany, gdy formularz pomyślnie przesyła (przebiegów weryfikacji).

Zestaw wbudowanych składników danych wejściowych są dostępne do odbierania i weryfikowanie danych wejściowych użytkownika. Dane wejściowe są prawidłowe, gdy są one zmienione i podczas przesyłania formularza. Dostępne składniki wejściowe są pokazane w poniższej tabeli.

| Składnika danych wejściowych   | Renderowane jako&hellip;       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

Składniki danych wejściowych udostępniają domyślne zachowanie sprawdzania poprawności po edycji i zmienianie ich klasy CSS, aby odzwierciedlić stan pola. Niektóre składniki zawierają przydatne podczas analizowania logiki. Na przykład `<InputDate>` i `<InputNumber>` bezpiecznie obsłużyć niemożliwy do przeanalizowania wartości, rejestrując je jako błędy sprawdzania poprawności. Typy, które może akceptować wartości null obsługuje również dopuszczanie wartości null dla pola docelowego (na przykład `int?`).

Następujące `Starship` typ definiuje logikę weryfikacji za pomocą większy zbiór właściwości oraz [adnotacje danych](xref:mvc/models/validation) niż wcześniej `ExampleModel`:

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
        <InputText id="identifier" bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" bind-Value="@starship.Classification">
            <option value"">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@functions {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

`<EditForm>` Tworzy `EditContext` jako [cascading wartość](xref:razor-components/components#cascading-values-and-parameters) , śledzi metadane dotyczące proces edycji, w tym, które pola zostały zmienione oraz bieżące wiadomości sprawdzania poprawności. `<EditForm>` Udostępnia również wygodną zdarzenia dla prawidłowe i nieprawidłowe przesyła (`OnValidSubmit`, `OnInvalidSubmit`). Można również użyć `OnSubmit` wyzwolić weryfikacji i sprawdź wartości pola z kodu niestandardowego sprawdzania poprawności.

Składnik weryfikacji adnotacji danych (`<DataAnnotationsValidator>`) dołącza Obsługa weryfikacji przy użyciu adnotacji danych w celu kaskadowy `EditContext`. Włączanie obsługi sprawdzania poprawności obecnie przy użyciu adnotacji danych wymaga to jawne gestu, ale rozważamy, dzięki czemu to zachowanie domyślne, które można przesłonić. Aby korzystać z systemu weryfikacji innego niż adnotacje danych, moduł weryfikacji adnotacji danych należy zastąpić implementację niestandardową. Implementacja platformy ASP.NET Core jest dostępny do kontroli źródła odwołania: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs). *Implementacja programu ASP.NET Core podlega szybkiej aktualizacji w wersji zapoznawczej.*

Składnik podsumowania sprawdzania poprawności (`<ValidationSummary>`) znajduje się podsumowanie wszystkich komunikatów o weryfikacji, który jest podobny do [Pomocnik tagu podsumowania sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

Składnik komunikat sprawdzania poprawności (`<ValidationMessage>`) wyświetla komunikaty weryfikacji dla określonego pola, które są podobne do [Pomocnik tagu komunikat sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Należy określić to pole do weryfikacji za pomocą `For` atrybutu i wyrażenia lambda nazewnictwa właściwości modelu:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> Wbudowanych składników danych wejściowych ma ograniczenia, które oczekuje się, aby rozwiązać w przyszłych wersjach. Na przykład nie można określić dowolne atrybutów w wygenerowanym `<input>` tagów. Twórz własne podklasy składnik do obsługi scenariuszy niedostępny.
