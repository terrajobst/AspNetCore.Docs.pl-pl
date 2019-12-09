---
title: ASP.NET Core Blazor cyklu życia
author: guardrex
description: Dowiedz się, jak używać metod cyklu życia składnika Razor w aplikacjach Blazor ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/lifecycle
ms.openlocfilehash: e600e7c7a6a8c646a655520bd5c127f2cd662753
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944034"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>ASP.NET Core Blazor cyklu życia

Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)

Platforma Blazor obejmuje metody cyklu życia synchronicznego i asynchronicznego. Zastąp metody cyklu życia, aby wykonać dodatkowe operacje na składnikach podczas inicjowania i renderowania składnika.

## <a name="lifecycle-methods"></a>Metody cyklu życia

### <a name="component-initialization-methods"></a>Metody inicjujące składniki

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> wykonywania kodu, który inicjuje składnik. Te metody są wywoływane tylko raz podczas pierwszego wystąpienia składnika.

Aby wykonać operację asynchroniczną, użyj `OnInitializedAsync` i słowa kluczowego `await` w operacji:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> Asynchroniczne działanie podczas inicjowania składnika musi wystąpić w trakcie `OnInitializedAsync`go zdarzenia cyklu życia.

W przypadku operacji synchronicznej Użyj `OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

### <a name="before-parameters-are-set"></a>Przed ustawieniem parametrów

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> ustawia parametry dostarczone przez element nadrzędny składnika w drzewie renderowania:

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView> zawiera cały zbiór wartości parametrów każdorazowo po wywołaniu metody `SetParametersAsync`.

Domyślna implementacja `SetParametersAsync` ustawia wartość każdej właściwości z atrybutem `[Parameter]` lub `[CascadingParameter]`, który ma odpowiednią wartość w `ParameterView`. Parametry, które nie mają odpowiadającej wartości w `ParameterView` są pozostawione bez zmian.

Jeśli `base.SetParametersAync` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób. Na przykład nie jest wymagane przypisanie parametrów przychodzących do właściwości w klasie.

### <a name="after-parameters-are-set"></a>Po ustawieniu parametrów

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> są wywoływane, gdy składnik otrzymał parametry z jego elementu nadrzędnego, a wartości są przypisywane do właściwości. Te metody są wykonywane po zainicjowaniu składnika i każdej chwili, gdy są określone nowe wartości parametrów:

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> Asynchroniczne działanie, gdy stosowane są parametry i wartości właściwości w trakcie `OnParametersSetAsync`go zdarzenia cyklu życia.

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a>Po renderowania składników

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> i <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> są wywoływane po zakończeniu renderowania składnika. Odwołania do elementów i składników są wypełniane w tym momencie. Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.

`firstRender` parametr `OnAfterRenderAsync` i `OnAfterRender`:

* Jest ustawiony na `true` podczas pierwszego renderowania wystąpienia składnika.
* Można go użyć, aby upewnić się, że zadania inicjowania są wykonywane tylko raz.

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> Asynchroniczne działanie natychmiast po wyrenderowaniu musi wystąpić w trakcie zdarzenia cyklu życia `OnAfterRenderAsync`.
>
> Nawet jeśli zwracasz <xref:System.Threading.Tasks.Task> z `OnAfterRenderAsync`, struktura nie zaplanuje dalszych cykli renderowania dla składnika po zakończeniu tego zadania. Ma to na celu uniknięcie nieskończonej pętli renderowania. Różnią się one od innych metod cyklu życia, które Zaplanuj kolejny cykl renderowania po zakończeniu zwracanego zadania.

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

`OnAfterRender` i `OnAfterRenderAsync` *nie są wywoływane podczas renderowania na serwerze.*

### <a name="suppress-ui-refreshing"></a>Pomiń odświeżanie interfejsu użytkownika

Zastąp <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>, aby pominąć odświeżanie interfejsu użytkownika. Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony:

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

`ShouldRender` jest wywoływana za każdym razem, gdy składnik jest renderowany.

Nawet jeśli `ShouldRender` jest zastępowana, składnik jest zawsze początkowo renderowany.

## <a name="state-changes"></a>Zmiany stanu

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> powiadamia składnik o zmianie jego stanu. Jeśli ma to zastosowanie, wywołanie `StateHasChanged` powoduje, że składnik zostanie przerenderowany.

## <a name="handle-incomplete-async-actions-at-render"></a>Obsługuj niekompletne akcje asynchroniczne podczas renderowania

Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogły nie zostać ukończone przed renderowaniem składnika. Obiekty mogą być `null` lub uzupełniane z danymi podczas wykonywania metody cyklu życia. Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane. Renderowanie zastępczych elementów interfejsu użytkownika (na przykład komunikatów ładowania) podczas `null`obiektów.

W `FetchData` składniku szablonów Blazor `OnInitializedAsync` został zastąpiony asynchronicznie odbierania danych prognozy (`forecasts`). Gdy `forecasts` jest `null`, zostanie wyświetlony komunikat ładowania użytkownika. Po `Task` zwrócone przez `OnInitializedAsync` zostanie wykonane, składnik zostanie przerenderowany ze zaktualizowanym stanem.

*Pages/FetchData. Razor* w szablonie serwera Blazor:

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>Usuwanie składnika z interfejsem IDisposable

Jeśli składnik implementuje <xref:System.IDisposable>, [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika. Poniższy składnik używa `@implements IDisposable` i metody `Dispose`:

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

> [!NOTE]
> Wywoływanie <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> w `Dispose` nie jest obsługiwane. `StateHasChanged` mogą być wywoływane w ramach rozrywania modułu renderowania, dlatego żądanie aktualizacji interfejsu użytkownika nie jest obsługiwane.

## <a name="handle-errors"></a>Obsługa błędów

Aby uzyskać informacje na temat obsługi błędów podczas wykonywania metody cyklu życia, zobacz <xref:blazor/handle-errors#lifecycle-methods>.
