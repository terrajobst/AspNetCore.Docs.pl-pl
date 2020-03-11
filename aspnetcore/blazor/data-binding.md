---
title: ASP.NET Core Blazor powiązania danych
author: guardrex
description: Dowiedz się więcej na temat scenariuszy powiązań danych dla składników i elementów DOM w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: 92377730b9d353a507ffd384710fb979affe7265
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661288"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a>ASP.NET Core Blazor powiązania danych

Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)

Powiązanie danych zarówno ze składnikami, jak i elementami modelu DOM jest realizowane przy użyciu atrybutu [`@bind`](xref:mvc/views/razor#bind) . Poniższy przykład wiąże Właściwość `CurrentValue` z wartością pola tekstowego:

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

Gdy pole tekstowe utraci fokus, wartość właściwości jest aktualizowana.

Pole tekstowe jest aktualizowane w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, a nie w odpowiedzi na zmianę wartości właściwości. Ponieważ składniki renderują się po wykonaniu kodu procedury obsługi zdarzeń, aktualizacje właściwości są *zwykle* odzwierciedlane w interfejsie użytkownika natychmiast po wyzwoleniu programu obsługi zdarzeń.

Używanie `@bind` z właściwością `CurrentValue` (`<input @bind="CurrentValue" />`) jest zasadniczo równoważne z następującymi:

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

Gdy składnik jest renderowany, `value` elementu wejściowego pochodzi z właściwości `CurrentValue`. Gdy użytkownik wpisze w polu tekstowym i zmieni fokus elementu, zdarzenie `onchange` jest wyzwalane, a właściwość `CurrentValue` jest ustawiona na wartość zmieniona. W rzeczywistości generowanie kodu jest bardziej skomplikowane, ponieważ `@bind` obsługuje przypadki, w których są wykonywane konwersje typów. W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia z atrybutem `value` i obsługuje zmiany przy użyciu zarejestrowanej procedury obsługi.

Powiąż właściwość lub pole w innych zdarzeniach, dołączając również atrybut `@bind:event` z parametrem `event`. Poniższy przykład wiąże Właściwość `CurrentValue` w zdarzeniu `oninput`:

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

W przeciwieństwie do `onchange`, które jest wyzwalane, gdy element utraci fokus, `oninput` uruchamiany, gdy wartość pola tekstowego ulegnie zmianie.

Użyj `@bind-{ATTRIBUTE}` ze składnią `@bind-{ATTRIBUTE}:event`, aby powiązać atrybuty elementu inne niż `value`. W poniższym przykładzie styl akapitu zostanie zaktualizowany po zmianie wartości `_paragraphStyle`:

```razor
@page "/binding-example"

<p>
    <input type="text" @bind="_paragraphStyle" />
</p>

<p @bind-style="_paragraphStyle" @bind-style:event="onchange">
    Blazorify the app!
</p>

@code {
    private string _paragraphStyle = "color:red";
}
```

## <a name="unparsable-values"></a>Wartości niemożliwy do przeanalizowania

Gdy użytkownik dostarczy wartość niemożliwy do przeanalizowania do elementu powiązanego z danymi, wartość niemożliwy do przeanalizowania jest automatycznie przywracana do poprzedniej wartości po wyzwoleniu zdarzenia bind.

Poniżej przedstawiono przykładowy scenariusz:

* Element `<input>` jest powiązany z typem `int` z początkową wartością `123`:

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* Użytkownik aktualizuje wartość elementu do `123.45` na stronie i zmienia fokus elementu.

W poprzednim scenariuszu wartość elementu jest przywracana do `123`. Gdy wartość `123.45` zostanie odrzucona na korzyść oryginalnej wartości `123`, użytkownik rozumie, że ich wartość nie została zaakceptowana.

Domyślnie powiązanie dotyczy zdarzenia `onchange` elementu (`@bind="{PROPERTY OR FIELD}"`). Użyj `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}`, aby wyzwolić powiązanie na innym zdarzeniu. W przypadku zdarzenia `oninput` (`@bind:event="oninput"`) następuje rewersja po naciśnięciu klawisza, które wprowadza niemożliwy do przeanalizowania wartość. Podczas określania wartości docelowej zdarzenia `oninput` przy użyciu typu powiązanego z `int`użytkownik nie będzie wpisywać znaku `.`. Znak `.` zostanie natychmiast usunięty, więc użytkownik otrzymuje natychmiastową opinię, że dozwolone są tylko liczby całkowite. Istnieją scenariusze, w których przywrócenie wartości w zdarzeniu `oninput` nie jest idealne, na przykład wtedy, gdy użytkownik powinien mieć możliwość wyczyszczenia wartości `<input>` niemożliwej do przeanalizowania. Alternatywy obejmują:

* Nie używaj zdarzenia `oninput`. Użyj domyślnego zdarzenia `onchange` (Określ tylko `@bind="{PROPERTY OR FIELD}"`), gdzie nie zostanie przywrócona nieprawidłowa wartość, dopóki element nie utraci fokusu.
* Powiąż z typem dopuszczającym wartość null, takim jak `int?` lub `string`, i podaj logikę niestandardową do obsługi nieprawidłowych wpisów.
* Użyj [składnika walidacji formularza](xref:blazor/forms-validation), takiego jak `InputNumber` lub `InputDate`. Składniki walidacji formularza mają wbudowaną obsługę zarządzania nieprawidłowymi danymi wejściowymi. Składniki walidacji formularza:
  * Zezwalaj użytkownikowi na dostarczenie nieprawidłowych danych wejściowych i otrzymywanie błędów walidacji w skojarzonym `EditContext`.
  * Wyświetlaj błędy walidacji w interfejsie użytkownika bez zakłócania wprowadzania dodatkowych danych przez użytkownika.

## <a name="format-strings"></a>Ciągi formatujące

Powiązanie danych działa z ciągami formatu <xref:System.DateTime> przy użyciu [`@bind:format`](xref:mvc/views/razor#bind). W tej chwili nie są dostępne inne wyrażenia formatu, takie jak formaty walutowe lub liczbowe.

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

W powyższym kodzie, typ pola `<input>` elementu (`type`) domyślnie `text`. `@bind:format` jest obsługiwana w celu powiązania następujących typów .NET:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

Atrybut `@bind:format` określa format daty, który ma zostać zastosowany do `value` elementu `<input>`. Format jest również używany do analizowania wartości, gdy wystąpi zdarzenie `onchange`.

Określanie formatu dla typu pola `date` nie jest zalecane, ponieważ Blazor ma wbudowaną obsługę formatowania dat. Pomimo zalecenia, należy używać tylko formatu daty `yyyy-MM-dd`, aby powiązania działały prawidłowo, jeśli format jest dostarczany z typem pola `date`:

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a>Powiązanie element nadrzędny-to-Child z parametrami składnika

Powiązanie rozpoznaje parametry składnika, gdzie `@bind-{PROPERTY}` może powiązać wartość właściwości z składnika nadrzędnego w dół ze składnikiem podrzędnym. Powiązanie z elementu podrzędnego z elementem nadrzędnym jest omówione w [powiązaniu podrzędnie-to-Parent z częściowym powiązaniem powiązania](#child-to-parent-binding-with-chained-bind) .

Poniższy składnik podrzędny (`ChildComponent`) ma parametr składnika `Year` i wywołanie zwrotne `YearChanged`:

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

`EventCallback<T>` wyjaśniono w <xref:blazor/event-handling#eventcallback>.

Poniższy składnik nadrzędny używa:

* `ChildComponent` i wiąże parametr `ParentYear` z elementu nadrzędnego z parametrem `Year` w składniku podrzędnym.
* Zdarzenie `onclick` służy do wyzwalania metody `ChangeTheYear`. Aby uzyskać więcej informacji, zobacz <xref:blazor/event-handling>.

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Załadowanie `ParentComponent` powoduje utworzenie następującej adjustacji:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Jeśli wartość właściwości `ParentYear` zostanie zmieniona przez wybranie przycisku w `ParentComponent`, zostanie zaktualizowana właściwość `Year` `ChildComponent`. Nowa wartość `Year` jest renderowana w interfejsie użytkownika podczas renderowania `ParentComponent`:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Parametr `Year` jest możliwy do powiązania, ponieważ zawiera zdarzenie pomocnika `YearChanged` pasujące do typu parametru `Year`.

Zgodnie z Konwencją `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo równoważne zapisowi:

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

Ogólnie rzecz biorąc, właściwość może być powiązana z odpowiadającą jej obsługą zdarzeń, dołączając atrybut `@bind-{PROPRETY}:event`. Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` przy użyciu następujących dwóch atrybutów:

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a>Powiązanie elementu podrzędnego z elementem nadrzędnym z powiązaniem łańcuchowym

Typowy scenariusz polega na łańcuchu parametru powiązanego z danymi do elementu strony w danych wyjściowych składnika. Ten scenariusz jest nazywany *powiązaniem łańcuchowym* , ponieważ wiele poziomów powiązań występuje jednocześnie.

Nie można zaimplementować powiązania łańcuchowego z składnią `@bind` w elemencie strony. Program obsługi zdarzeń i wartość muszą być określone osobno. Składnik nadrzędny, jednak może używać składni `@bind`ej z parametrem składnika.

Następujący składnik `PasswordField` (*PasswordField. Razor*):

* Ustawia wartość elementu `<input>` na Właściwość `Password`.
* Uwidacznia zmiany właściwości `Password` w składniku nadrzędnym z [EventCallback](xref:blazor/event-handling#eventcallback).
* Używa zdarzenia `onclick` służy do wyzwalania metody `ToggleShowPassword`. Aby uzyskać więcej informacji, zobacz <xref:blazor/event-handling>.

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

Składnik `PasswordField` jest używany w innym składniku:

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

Aby przeprowadzić sprawdzenia lub błędy pułapki dla hasła w poprzednim przykładzie:

* Utwórz pole zapasowe dla `Password` (`_password` w poniższym przykładowym kodzie).
* Wykonaj testy lub błędy pułapki w metodzie ustawiającej `Password`.

Poniższy przykład przedstawia natychmiastową opinię dla użytkownika, jeśli w wartości hasła jest używana spacja:

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

## <a name="radio-buttons"></a>Przyciski radiowe

Aby uzyskać informacje na temat tworzenia powiązań z przyciskami radiowymi w formularzu, zobacz <xref:blazor/forms-validation#work-with-radio-buttons>.
