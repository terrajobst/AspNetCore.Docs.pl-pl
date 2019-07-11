---
title: Pomocnicy tagów w formularzach w programie ASP.NET Core
author: rick-anderson
description: W tym artykule opisano wbudowane pomocników tagów używane w formularzach.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/working-with-forms
ms.openlocfilehash: a07bb4f539c8bd38b08402c598924e14c748921d
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815234"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a>Pomocnicy tagów w formularzach w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [MULLENA N. Taylora](https://github.com/NTaylorMullen), [Dave Paquette](https://twitter.com/Dave_Paquette), i [Jerrie Pelser](https://github.com/jerriep)

W tym dokumencie przedstawiono pracy z formularzami i często używane w formularzu elementów HTML. Kod HTML [formularza](https://www.w3.org/TR/html401/interact/forms.html) element udostępnia użycia aplikacji sieci web podstawowego mechanizmu publikować dane do serwera. Większość w tym dokumencie opisano [pomocników tagów](tag-helpers/intro.md) i jak mogą one pomóc produktywną tworzyć niezawodne formularzy HTML. Zalecamy przeczytanie [wprowadzenie do pomocników tagów](tag-helpers/intro.md) przed przeczytaniem tego dokumentu.

W wielu przypadkach pomocników HTML zapewnić alternatywne podejście do określonych Pomocnik tagu, ale ważne jest, aby rozpoznać, czy pomocników tagów nie zastąpić pomocników HTML i nie jest pomocnika tagów dla każdego pomocnika kodu HTML. Jeśli istnieje alternatywna pomocnika kodu HTML, jest wymienione.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Pomocnik tagu formularza

[Formularza](https://www.w3.org/TR/html401/interact/forms.html) pomocnika tagów:

* Generuje kod HTML [ \<formularza >](https://www.w3.org/TR/html401/interact/forms.html) `action` wartość atrybutu dla akcji kontrolera MVC lub nazwanej trasy

* Generuje ukryty [tokenu weryfikacji żądania](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) celu zapobiegania fałszowaniu żądania między witrynami (gdy jest używane z `[ValidateAntiForgeryToken]` atrybutu w metodzie akcji Post protokołu HTTP)

* Udostępnia `asp-route-<Parameter Name>` atrybutu, gdzie `<Parameter Name>` jest dodawany do wartości trasy. `routeValues` Parametry `Html.BeginForm` i `Html.BeginRouteForm` zapewniają podobne funkcje.

* Ma alternatywa pomocnika kodu HTML `Html.BeginForm` i `Html.BeginRouteForm`

Przykład:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

Pomocnik tagu formularza powyżej generuje poniższy kod HTML:

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

Środowisko wykonawcze MVC zgłasza `action` wartość atrybutu z atrybutów Pomocnik tagu formularza `asp-controller` i `asp-action`. Pomocnik tagu formularza również generuje ukryty [tokenu weryfikacji żądania](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) celu zapobiegania fałszowaniu żądania między witrynami (gdy jest używane z `[ValidateAntiForgeryToken]` atrybutu w metodzie akcji Post protokołu HTTP). Ochrona przed fałszerstwem żądań między witrynami czysty formularz HTML jest trudne, Pomocnik tagu formularza udostępnia tę usługę dla Ciebie.

### <a name="using-a-named-route"></a>Korzystanie z nazwanej trasy

`asp-route` Atrybut pomocnika tagów można również wygenerować kod znaczników dla kodu HTML `action` atrybutu. Aplikację przy użyciu [trasy](../../fundamentals/routing.md) o nazwie `register` można użyć następujących znaczników dla strony rejestracji:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Wiele widoków w *widoków/konto* folder (generowane podczas tworzenia nowej aplikacji sieci web za pomocą *indywidualne konta użytkowników*) zawierają [asp trasy returnurl](xref:mvc/views/working-with-forms) atrybutu:

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Za pomocą wbudowanych szablonów `returnUrl` tylko jest wypełniane automatycznie podczas spróbuj uzyskać dostęp do autoryzowanych zasobów, ale nie uwierzytelniony i autoryzowany. Podczas próby nieautoryzowanego dostępu do oprogramowania pośredniczącego zabezpieczeń przekieruje Cię do strony logowania za pomocą `returnUrl` zestawu.

## <a name="the-form-action-tag-helper"></a>Pomocnik tagu akcji formularza

Pomocnik tagu akcji formularza generuje `formaction` atrybutu w wygenerowanym `<button ...>` lub `<input type="image" ...>` tagu. `formaction` Atrybut kontroluje, gdzie formularza przesłaniu jego danych. Powiąże [ \<wejściowych >](https://www.w3.org/wiki/HTML/Elements/input) elementów typu `image` i [ \<przycisk >](https://www.w3.org/wiki/HTML/Elements/button) elementów. Pomocnik tagu akcji formularza umożliwia użycie kilku [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-` atrybuty można kontrolować, co `formaction` łącze jest generowany dla odpowiedniego elementu.

Obsługiwane [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) atrybuty kontrolować wartość `formaction`:

|Atrybut|Opis|
|---|---|
|[asp-controller](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-controller)|Nazwa kontrolera.|
|[asp-action](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-action)|Nazwa metody akcji.|
|[asp-area](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-area)|Nazwa obszaru.|
|[asp-page](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page)|Nazwa strony Razor.|
|[asp-page-handler](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page-handler)|Nazwa procedury obsługi stron Razor.|
|[asp-route](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route)|Nazwa trasy.|
|[asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route-value)|Adres URL trasy wartość typu single. Na przykład `asp-route-id="1234"`.|
|[asp-all-route-data](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-all-route-data)|Wszystkie wartości trasy.|
|[asp-fragment](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-fragment)|Fragmentu adresu URL.|

### <a name="submit-to-controller-example"></a>Przedstawia przykład kontrolera

Następujące znaczniki przesyła formularz, aby `Index` akcji `HomeController` po wybraniu danych wejściowych lub przycisk:

```cshtml
<form method="post">
    <button asp-controller="Home" asp-action="Index">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-controller="Home" 
                                asp-action="Index">
</form>
```

Poprzednie znaczników generuje następujące HTML:

```html
<form method="post">
    <button formaction="/Home">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home">
</form>
```

### <a name="submit-to-page-example"></a>Przedstawia przykład strony

Następujące znaczniki przesyła formularz, aby `About` strona Razor:

```cshtml
<form method="post">
    <button asp-page="About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-page="About">
</form>
```

Poprzednie znaczników generuje następujące HTML:

```html
<form method="post">
    <button formaction="/About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/About">
</form>
```

### <a name="submit-to-route-example"></a>Przedstawia przykład tras

Należy wziąć pod uwagę `/Home/Test` punktu końcowego:

```csharp
public class HomeController : Controller
{
    [Route("/Home/Test", Name = "Custom")]
    public string Test()
    {
        return "This is the test page";
    }
}
```

Następujące znaczniki przesyła formularz, aby `/Home/Test` punktu końcowego.

```cshtml
<form method="post">
    <button asp-route="Custom">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-route="Custom">
</form>
```

Poprzednie znaczników generuje następujące HTML:

```html
<form method="post">
    <button formaction="/Home/Test">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home/Test">
</form>
```

## <a name="the-input-tag-helper"></a>Pomocnik tagu wejściowego

Wejściowy Pomocnik tagu wiąże HTML [ \<wejściowych >](https://www.w3.org/wiki/HTML/Elements/input) element wyrażenia modelu, w tym widoku razor.

Składnia:

```HTML
<input asp-for="<Expression Name>">
```

Pomocnik tagu wejściowego:

* Generuje `id` i `name` atrybuty kodu HTML nazwa wyrażenia określone w `asp-for` atrybutu. `asp-for="Property1.Property2"` jest odpowiednikiem `m => m.Property1.Property2`. Nazwa wyrażenia jest, do czego służy `asp-for` wartość atrybutu. Zobacz [nazwy wyrażeń](#expression-names) sekcji, aby uzyskać dodatkowe informacje.

* Ustawia kod HTML `type` atrybutu wartości na podstawie typu modelu i [adnotacji danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty stosowane do właściwości modelu

* Nie zastępować HTML `type` wartość atrybutu, gdy został określony

* Generuje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) sprawdzania poprawności atrybutów z [adnotacji danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty stosowane do właściwości modelu

* Ma funkcję pomocnika kodu HTML, nakładają się na `Html.TextBoxFor` i `Html.EditorFor`. Zobacz **alternatywy pomocnika kodu HTML dla danych wejściowych Pomocnik tagu** sekcji, aby uzyskać szczegółowe informacje.

* Zapewnia silne wpisywanie. Jeśli nazwa zmiany właściwości, a nie Aktualizuj Pomocnik tagu otrzymasz błąd podobny do następującego:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input` Pomocnik tagu ustawia HTML `type` atrybut oparty na typie .NET. W poniższej tabeli wymieniono niektóre typowe typy .NET i wygenerowany typ HTML (wymienione nie każdy typ .NET).

|Typ architektury .NET|Typ danych wejściowych|
|---|---|
|Bool|type="checkbox"|
|String|type="text"|
|DataGodzina|type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)|
|Byte|type="number"|
|int|type="number"|
|Pojedynczy Double|type="number"|

W poniższej tabeli przedstawiono niektóre typowe [adnotacje danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybutów, które Pomocnik tagu wejściowego będzie zmapowana do określonych typów wejściowych (nie każdy atrybut weryfikacji znajduje się):

|Atrybut|Typ danych wejściowych|
|---|---|
|[EmailAddress]|type="email"|
|[Url]|type="url"|
|[HiddenInput]|type="hidden"|
|[Phone]|type="tel"|
|[DataType(DataType.Password)]|type="password"|
|[DataType(DataType.Date)]|type="date"|
|[DataType(DataType.Time)]|type="time"|

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Powyższy kod generuje poniższy kod HTML:

```HTML
  <form method="post" action="/Demo/RegisterInput">
      Email:
      <input type="email" data-val="true"
             data-val-email="The Email Address field is not a valid email address."
             data-val-required="The Email Address field is required."
             id="Email" name="Email" value=""><br>
      Password:
      <input type="password" data-val="true"
             data-val-required="The Password field is required."
             id="Password" name="Password"><br>
      <button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
   </form>
```

Adnotacje danych dotyczą `Email` i `Password` właściwości Generowanie metadanych na podstawie modelu. Pomocnik tagu dane wejściowe zużywa metadanych modelu i tworzy [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atrybutów (zobacz [sprawdzania poprawności modelu](../models/validation.md)). Te atrybuty opisać moduły weryfikacji, aby dołączyć do pól wejściowych. Dzięki temu dyskretnego kodu HTML5 i [jQuery](https://jquery.com/) sprawdzania poprawności. Atrybuty dyskretnego kodu mają format `data-val-rule="Error Message"`, gdzie reguła jest nazwa reguły sprawdzania poprawności (takie jak `data-val-required`, `data-val-email`, `data-val-maxlength`itp.) Jeśli komunikat o błędzie zostanie podany w atrybucie, jest on wyświetlany jako wartość pozycji `data-val-rule` atrybutu. Dostępne są także atrybutów w formie `data-val-ruleName-argumentName="argumentValue"` zapewniające dodatkowe szczegóły dotyczące reguły, na przykład `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Pomocnik tagu dane wejściowe alternatywy dla pomocnika kodu HTML

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` i `Html.EditorFor` nakładających się funkcji pomocnika Tag danych wejściowych. Pomocnik tagu dane wejściowe automatycznie ustawi `type` atrybutu; `Html.TextBox` i `Html.TextBoxFor` nie będzie. `Html.Editor` i `Html.EditorFor` obsługi kolekcji, złożonych obiektów i szablony nie Pomocnik tagu danych wejściowych. Pomocnik tagu danych wejściowych, `Html.EditorFor` i `Html.TextBoxFor` są silnie typizowane (ich użycie wyrażeń lambda); `Html.TextBox` i `Html.Editor` nie (używają nazwy wyrażeń).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()` i `@Html.EditorFor()` użyć specjalnej `ViewDataDictionary` wpis o nazwie `htmlAttributes` podczas wykonywania ich domyślnych szablonów. To zachowanie jest opcjonalnie rozszerzone przy użyciu `additionalViewData` parametrów. Klucz "htmlAttributes" jest rozróżniana wielkość liter. Klucz "htmlAttributes" odbywa się podobnie jak `htmlAttributes` przekazano obiekt jako danych wejściowych pomocników, takich jak `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Wyrażenie nazwy

`asp-for` Wartość atrybutu jest `ModelExpression` i po prawej stronie wyrażenia lambda. W związku z tym `asp-for="Property1"` staje się `m => m.Property1` w wygenerowanym kodzie, co jest Dlaczego nie trzeba prefiks z `Model`. Można użyć "\@" znaku, aby uruchomić wyrażenia wbudowane i przenieść przed `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe">
```

Generuje następujące czynności:

```HTML
<input type="text" id="joe" name="joe" value="Joe">
```

Z właściwościami kolekcji `asp-for="CollectionProperty[23].Member"` generuje taką samą nazwę jak `asp-for="CollectionProperty[i].Member"` podczas `i` ma wartość `23`.

Gdy program ASP.NET Core MVC oblicza wartość `ModelExpression`, sprawdza ono kilka źródeł, w tym `ModelState`. Należy wziąć pod uwagę `<input type="text" asp-for="@Name">`. Obliczony `value` atrybut jest pierwsza wartość inną niż null z:

* `ModelState` wpis z kluczem "Name".
* Wynik wyrażenia `Model.Name`.

### <a name="navigating-child-properties"></a>Nawigacja właściwości podrzędnej

Możesz także przejść do właściwości podrzędnej przy użyciu ścieżki właściwości modelu widoku. Należy wziąć pod uwagę bardziej złożonych klasy modelu, który zawiera element podrzędny `Address` właściwości.

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

W widoku, możemy powiązać `Address.AddressLine1`:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Poniższy kod HTML jest generowany dla `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="">
```

### <a name="expression-names-and-collections"></a>Wyrażenie nazwy i kolekcji

Przykładowy model zawierający tablicę `Colors`:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Metody akcji:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

Następujące Razor pokazano, jak uzyskiwać dostęp do określonego `Color` elementu:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml* szablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Przykładowy przy użyciu `List<T>`:

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Następujące Razor pokazuje, jak iteracji po kolekcji:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/ToDoItem.cshtml* szablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

`foreach` powinny być używane, jeśli jest to możliwe, gdy wartość ma być używany w `asp-for` lub `Html.DisplayFor` równoważne kontekstu. Ogólnie rzecz biorąc `for` jest lepsze niż `foreach` (jeśli scenariusz umożliwia), ponieważ nie trzeba ją przydzielić moduł wyliczający; jednak ocena indeksatora w wyrażeniu LINQ może być kosztowne i należy zminimalizować.

&nbsp;

>[!NOTE]
>Powyższy kod przykładowy komentarzem pokazuje, jak należy zamienić wyrażenia lambda z `@` operator, aby uzyskać dostęp każdy `ToDoItem` na liście.

## <a name="the-textarea-tag-helper"></a>Pomocnik tagu Textarea

`Textarea Tag Helper` Pomocnik tagu jest podobny do Pomocnik tagu danych wejściowych.

* Generuje `id` i `name` atrybuty i atrybutów sprawdzania poprawności danych z modelu dla [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elementu.

* Zapewnia silne wpisywanie.

* Alternatywa pomocnika kodu HTML: `Html.TextAreaFor`

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Poniższy kod HTML jest generowany:

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

## <a name="the-label-tag-helper"></a>Pomocnik tagu etykiet

* Generuje podpis etykiety i `for` atrybutu na [ \<Etykieta >](https://www.w3.org/wiki/HTML/Elements/label) element to nazwa wyrażenia

* Alternatywa pomocnika kodu HTML: `Html.LabelFor`.

`Label Tag Helper` Zapewnia następujące korzyści za pośrednictwem czystego element label kodu HTML:

* Automatycznie pobrana wartość opisową etykietę `Display` atrybutu. Nazwę wyświetlaną zamierzony mogą ulec zmianie czasu i kombinacja `Display` zastosuje atrybut i Pomocnik tagu etykiet `Display` wszędzie, gdzie jest używany.

* Mniej znacznika w kodzie źródłowym

* Silne wpisywanie z właściwością modelu.

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Poniższy kod HTML jest generowany dla `<label>` elementu:

```HTML
<label for="Email">Email Address</label>
```

Pomocnik tagu etykiet, które są generowane `for` wartość atrybutu "Email", czyli identyfikator skojarzony z `<input>` elementu. Pomocnicy tagów Generowanie spójne `id` i `for` elementów, tak aby były prawidłowo skojarzone. Podpis w tym przykładzie pochodzi z `Display` atrybutu. Jeśli model nie zawiera `Display` atrybutu, podpis będzie nazwa właściwości wyrażenia.

## <a name="the-validation-tag-helpers"></a>Sprawdzanie poprawności pomocnicy tagów

Istnieją dwa pomocników tagów sprawdzania poprawności. `Validation Message Tag Helper` (Która wyświetla komunikat sprawdzania poprawności dla jednej właściwości modelu), a `Validation Summary Tag Helper` (które jest wyświetlane podsumowanie błędów sprawdzania poprawności). `Input Tag Helper` Dodaje atrybuty weryfikacji po stronie klienta HTML5 do wprowadzania elementów na podstawie danych atrybuty adnotacji w klasach modeli. Sprawdzanie poprawności, również odbywa się na serwerze. Pomocnik tagu weryfikacji wyświetla te komunikaty o błędach, gdy wystąpi błąd sprawdzania poprawności.

### <a name="the-validation-message-tag-helper"></a>Pomocnik tagu komunikat sprawdzania poprawności

* Dodaje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` atrybutu [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, który dołącza komunikatów o błędach weryfikacji na pole wejściowe właściwości określonego modelu. Gdy wystąpi błąd weryfikacji po stronie klienta, [jQuery](https://jquery.com/) wyświetla komunikat o błędzie w `<span>` elementu.

* Sprawdzanie poprawności również odbywa się na serwerze. Klienci mogą mieć Obsługa skryptów JavaScript wyłączona i niektóre sprawdzania poprawności jest możliwe tylko po stronie serwera.

* Alternatywa pomocnika kodu HTML: `Html.ValidationMessageFor`

`Validation Message Tag Helper` Jest używana z `asp-validation-for` atrybutu HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elementu.

```HTML
<span asp-validation-for="Email"></span>
```

Pomocnik tagu komunikat sprawdzania poprawności generuje poniższy kod HTML:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Na ogół służy `Validation Message Tag Helper` po `Input` Pomocnik tagu dla tej samej właściwości. Ten sposób wyświetla komunikaty o błędach weryfikacji w danych wejściowych, które spowodowały błąd.

> [!NOTE]
> Konieczne jest posiadanie widoku z poprawną JavaScript i [jQuery](https://jquery.com/) skryptu odwołania w miejscu na potrzeby weryfikacji po stronie klienta. Zobacz [sprawdzania poprawności modelu](../models/validation.md) Aby uzyskać więcej informacji.

Gdy pojawia się błąd weryfikacji po stronie serwera, (na przykład gdy masz weryfikacji po stronie niestandardowego serwera lub Weryfikacja po stronie klienta jest wyłączona), MVC umieszcza komunikat o błędzie jako treść metody `<span>` elementu.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Pomocnik tagu podsumowania sprawdzania poprawności

* Obiekty docelowe `<div>` elementów za pomocą `asp-validation-summary` atrybutu

* Alternatywa pomocnika kodu HTML: `@Html.ValidationSummary`

`Validation Summary Tag Helper` Służy do wyświetlania podsumowania komunikatów dotyczących sprawdzania poprawności. `asp-validation-summary` Wartość atrybutu może być dowolną z następujących czynności:

|asp-validation-summary|Wyświetlane komunikatów dotyczących sprawdzania poprawności|
|--- |--- |
|ValidationSummary.All|Poziom właściwości i model|
|ValidationSummary.ModelOnly|Model|
|ValidationSummary.None|Brak|

### <a name="sample"></a>Przykład

W poniższym przykładzie zostanie nadany modelu danych `DataAnnotation` atrybuty, które generuje komunikaty o błędach weryfikacji na `<input>` elementu.  Gdy wystąpi błąd sprawdzania poprawności, Pomocnik tagu weryfikacji wyświetla komunikat o błędzie:

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Wygenerowany kod HTML, (gdy model jest prawidłowy):

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

## <a name="the-select-tag-helper"></a>Pomocnik tagu Select

* Generuje [wybierz](https://www.w3.org/wiki/HTML/Elements/select) i skojarzone [opcji](https://www.w3.org/wiki/HTML/Elements/option) elementy dla właściwości modelu.

* Ma alternatywa pomocnika kodu HTML `Html.DropDownListFor` i `Html.ListBoxFor`

`Select Tag Helper` `asp-for` Określa nazwę właściwości modelu [wybierz](https://www.w3.org/wiki/HTML/Elements/select) elementu i `asp-items` Określa [opcji](https://www.w3.org/wiki/HTML/Elements/option) elementów.  Na przykład:

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index` Inicjuje metodę `CountryViewModel`, ustawia wybranym kraju i przekazuje go do `Index` widoku.

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

HTTP POST `Index` metoda Wyświetla zaznaczenie:

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index` Widoku:

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Która generuje poniższy kod HTML (się od liter "CA" wybrane):

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
   </form>
```

> [!NOTE]
> Nie zalecamy używania `ViewBag` lub `ViewData` przy użyciu Pomocnika tagów wybierz. Model widoku jest bardziej niezawodna w zapewnieniu metadanych platformy MVC i zazwyczaj mniej problematyczne.

`asp-for` Wartość atrybutu jest przypadkiem szczególnym i nie wymaga `Model` prefiksu, inne są atrybuty Pomocnik tagu (takie jak `asp-items`)

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Powiązanie typu wyliczeniowego

Często jest to łatwe w użyciu `<select>` z `enum` właściwości i wygenerować `SelectListItem` elementy z `enum` wartości.

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList` Metoda generuje `SelectList` obiektu dla typu wyliczeniowego.

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Można dekoracji moduł wyliczający listę o `Display` atrybutu, aby uzyskać bardziej rozbudowane interfejs użytkownika:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Poniższy kod HTML jest generowany:

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
    </form>
```

### <a name="option-group"></a>Opcja grupy

Kod HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) element jest generowany, gdy model widoku zawiera jeden lub więcej `SelectListGroup` obiektów.

`CountryViewModelGroup` Grup `SelectListItem` elementów do grupy "Ameryka Północna" i "Europa":

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

Poniżej przedstawiono dwie grupy:

![przykład grupy opcji](working-with-forms/_static/grp.png)

Wygenerowany kod HTML:

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
 </form>
```

### <a name="multiple-select"></a>Wybór wielokrotny

Pomocnik tagu wybierz automatycznie wygeneruje [wielu = "wielu"](https://w3c.github.io/html-reference/select.html) atrybutu, jeśli właściwość określona w `asp-for` atrybut jest `IEnumerable`. Na przykład biorąc pod uwagę następujący wzór:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Za pomocą następującego widoku:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Generuje poniższy kod HTML:

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

### <a name="no-selection"></a>Brak zaznaczenia

Jeśli okaże się, przy użyciu opcji "nie został określony" na wielu stronach, można utworzyć szablon, aby wyeliminować powtarzające się kodu HTML:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml* szablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

Dodawanie HTML [ \<opcja >](https://www.w3.org/wiki/HTML/Elements/option) elementów nie jest ograniczona do *Brak zaznaczenia* przypadek. Na przykład następującej metody akcji i widoku spowoduje wygenerowanie podobny do powyższego kodu HTML:

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Poprawny `<option>` element zostanie wybrana (zawierają `selected="selected"` atrybutów) w zależności od bieżącego `Country` wartość.

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
 </form>
 ```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/tag-helpers/intro>
* [Element formularza HTML](https://www.w3.org/TR/html401/interact/forms.html)
* [Żądanie weryfikacji tokenu](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* [Interfejs IAttributeAdapter](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [Fragmenty kodu dla tego dokumentu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
