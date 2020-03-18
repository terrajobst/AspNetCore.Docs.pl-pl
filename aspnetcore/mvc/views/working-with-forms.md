---
title: Pomocnicy tagów w formularzach w ASP.NET Core
author: rick-anderson
description: Opisuje wbudowane pomocnicy tagów używane z formularzami.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/working-with-forms
ms.openlocfilehash: 5af532db35b858d157f61a6aca30f55d15e9ff1e
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2020
ms.locfileid: "79416241"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a>Pomocnicy tagów w formularzach w ASP.NET Core

Autor [Rick Anderson](https://twitter.com/RickAndMSFT), [N. Taylor Mullen](https://github.com/NTaylorMullen), [Dave Paquette](https://twitter.com/Dave_Paquette)i [Jerrie Pelser](https://github.com/jerriep)

Ten dokument przedstawia pracę z formularzami i elementami HTML często używanymi w formularzu. Element HTML [form](https://www.w3.org/TR/html401/interact/forms.html) zawiera podstawowe mechanizmy aplikacje sieci Web używane do publikowania danych na serwerze. W większości tego dokumentu opisano [pomocników tagów](tag-helpers/intro.md) i sposób, w jaki mogą one pomóc w produktywnym tworzeniu niezawodnych formularzy HTML. Zalecamy zapoznanie [się z wprowadzeniem do pomocników tagów](tag-helpers/intro.md) przed przeczytaniem tego dokumentu.

W wielu przypadkach pomocników HTML udostępniają alternatywne podejście do osoby pomagającej z określonym znacznikiem, ale ważne jest, aby rozpoznać, że pomocnicy tagów nie zastępują pomocników HTML i nie ma pomocy dla tagów dla każdego pomocnika HTML. Gdy istnieje alternatywa dla pomocnika HTML, jest ona wspomniana.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Pomocnik tagów formularza

Pomocnik tagu [formularza](https://www.w3.org/TR/html401/interact/forms.html) :

* Generuje [formularz\<HTML >](https://www.w3.org/TR/html401/interact/forms.html) `action` wartości atrybutu dla akcji kontrolera MVC lub nazwanej trasy

* Generuje [token weryfikacji ukrytego żądania](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) , aby zapobiec fałszerstwu żądania między lokacjami (gdy jest używany z atrybutem `[ValidateAntiForgeryToken]` w metodzie akcji post protokołu HTTP)

* Udostępnia atrybut `asp-route-<Parameter Name>`, w którym `<Parameter Name>` jest dodawany do wartości trasy. `routeValues` parametry `Html.BeginForm` i `Html.BeginRouteForm` zapewniają podobną funkcjonalność.

* Ma `Html.BeginForm` alternatywną dla pomocnika HTML i `Html.BeginRouteForm`

Przykład:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

Pomocnik tagu formularza powyżej generuje następujący kod HTML:

```html
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

Środowisko uruchomieniowe MVC generuje wartość atrybutu `action` na podstawie atrybutów pomocnika tagów formularza `asp-controller` i `asp-action`. Pomocnik tagu formularza generuje również [token weryfikacji ukrytego żądania](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) , aby zapobiec fałszerstwu żądania między lokacjami (gdy jest używany z atrybutem `[ValidateAntiForgeryToken]` w metodzie akcji post protokołu HTTP). Ochrona czystej formy HTML z poziomu fałszerstwa żądań między lokacjami jest trudna, ponieważ pomocnik tagów formularza udostępnia tę usługę.

### <a name="using-a-named-route"></a>Używanie nazwanej trasy

Atrybut pomocnika tagów `asp-route` może również generować znaczniki dla atrybutu `action` HTML. Aplikacja o [trasie](../../fundamentals/routing.md) o nazwie `register` może użyć następującego znacznika na stronie rejestracji:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Wiele widoków w folderze *widoki/konto* (generowane podczas tworzenia nowej aplikacji sieci Web przy użyciu *poszczególnych kont użytkowników*) zawiera atrybut [ASP-Route-ReturnUrl](xref:mvc/views/working-with-forms) :

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Przy użyciu wbudowanych szablonów `returnUrl` jest wypełniany automatycznie tylko podczas próby uzyskania dostępu do autoryzowanego zasobu, ale nie są uwierzytelnione ani autoryzowane. W przypadku próby uzyskania nieautoryzowanego dostępu oprogramowanie pośredniczące zabezpieczeń przekierowuje użytkownika do strony logowania z zestawem `returnUrl`.

## <a name="the-form-action-tag-helper"></a>Pomocnik tagów akcji formularza

Pomocnik tagów akcji formularza generuje atrybut `formaction` dla wygenerowanego `<button ...>` lub tagu `<input type="image" ...>`. Atrybut `formaction` kontroluje, gdzie formularz przesyła dane. Tworzy powiązanie do [\<danych wejściowych >](https://www.w3.org/wiki/HTML/Elements/input) elementów typu `image` i [\<>](https://www.w3.org/wiki/HTML/Elements/button) elementów. Pomocnik tagów akcji formularza umożliwia użycie kilku [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-` atrybutów, aby określić, jakie `formaction` łącza są generowane dla odpowiedniego elementu.

Obsługiwane atrybuty [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) do kontrolowania wartości `formaction`:

|Atrybut|Opis|
|---|---|
|[ASP-Controller](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-controller)|Nazwa kontrolera.|
|[ASP — akcja](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-action)|Nazwa metody akcji.|
|[obszar ASP](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-area)|Nazwa obszaru.|
|[ASP — Strona](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page)|Nazwa strony Razor.|
|[ASP — obsługa stron](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page-handler)|Nazwa programu obsługi stron Razor.|
|[ASP — Route](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route)|Nazwa trasy.|
|[ASP-Route-{Value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route-value)|Wartość trasy pojedynczego adresu URL. Na przykład `asp-route-id="1234"`.|
|[ASP — wszystkie trasy — dane](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-all-route-data)|Wszystkie wartości trasy.|
|[ASP — fragment](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-fragment)|Fragment adresu URL.|

### <a name="submit-to-controller-example"></a>Przykład przesłania do kontrolera

Następujące znaczniki przesłaniają formularz do `Index` akcji `HomeController`, gdy wybrane są dane wejściowe lub przycisk:

```cshtml
<form method="post">
    <button asp-controller="Home" asp-action="Index">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-controller="Home" 
                                asp-action="Index">
</form>
```

Poprzednia Adiustacja generuje następujący kod HTML:

```html
<form method="post">
    <button formaction="/Home">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home">
</form>
```

### <a name="submit-to-page-example"></a>Prześlij do przykładowej strony

Poniższy znacznik przesłania formularz do `About` stronie Razor:

```cshtml
<form method="post">
    <button asp-page="About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-page="About">
</form>
```

Poprzednia Adiustacja generuje następujący kod HTML:

```html
<form method="post">
    <button formaction="/About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/About">
</form>
```

### <a name="submit-to-route-example"></a>Prześlij do przykładu trasy

Rozważ użycie punktu końcowego `/Home/Test`:

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

Poniższy znacznik przesłania formularz do punktu końcowego `/Home/Test`.

```cshtml
<form method="post">
    <button asp-route="Custom">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-route="Custom">
</form>
```

Poprzednia Adiustacja generuje następujący kod HTML:

```html
<form method="post">
    <button formaction="/Home/Test">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home/Test">
</form>
```

## <a name="the-input-tag-helper"></a>Pomocnik tagu wejściowego

Pomocnik tagu wejściowego tworzy powiązanie elementu [\<Input](https://www.w3.org/wiki/HTML/Elements/input) języka HTML > do wyrażenia modelu w widoku Razor.

Składnia:

```cshtml
<input asp-for="<Expression Name>">
```

Pomocnik tagu wejściowego:

* Generuje atrybuty HTML `id` i `name` dla nazwy wyrażenia określonej w atrybucie `asp-for`. `asp-for="Property1.Property2"` jest równoznaczna z `m => m.Property1.Property2`. Nazwa wyrażenia jest używana jako wartość atrybutu `asp-for`. Aby uzyskać dodatkowe informacje, zobacz sekcję [nazwy wyrażeń](#expression-names) .

* Ustawia wartość atrybutu HTML `type` na podstawie atrybutów typu modelu i [adnotacji danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) zastosowanych do właściwości model

* Nie zastępuje wartości atrybutu `type` HTML, gdy jest określony

* Generuje atrybuty walidacji [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) z atrybutów [adnotacji danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) zastosowanych do właściwości modelu

* Ma funkcję pomocnika HTML, która pokrywa się z `Html.TextBoxFor` i `Html.EditorFor`. Szczegóły można znaleźć w sekcji pomocnik **HTML — alternatywy dla tagów wejściowych** .

* Zapewnia silne wpisywanie. Jeśli nazwa właściwości ulegnie zmianie i nie zostanie zaktualizowany pomocnika tagów, zostanie wyświetlony komunikat o błędzie podobny do następującego:

```
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

Pomocnik tagu `Input` ustawia atrybut HTML `type` na podstawie typu .NET. W poniższej tabeli wymieniono niektóre typowe typy .NET i wygenerowany typ HTML (nie każdy typ .NET jest wyświetlany).

|Typ .NET|Typ danych wejściowych|
|---|---|
|Bool|type="checkbox"|
|Ciąg|Type = "text"|
|DateTime|Type =["DateTime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)|
|Byte|type="number"|
|int|type="number"|
|Pojedyncza, Podwójna|type="number"|

W poniższej tabeli przedstawiono niektóre typowe atrybuty [adnotacji danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) , które pomocnik tagów wejściowych będzie mapowany do określonych typów danych wejściowych (nie każdy atrybut walidacji jest wymieniony):

|Atrybut|Typ danych wejściowych|
|---|---|
|[EmailAddress]|Type = "e-mail"|
|[Url]|type="url"|
|[HiddenInput]|type="hidden"|
|Połączenia|type="tel"|
|[DataType(DataType.Password)]|type="password"|
|[DataType(DataType.Date)]|type="date"|
|[DataType(DataType.Time)]|type="time"|

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Kod powyżej generuje następujący HTML:

```html
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

Adnotacje danych zastosowane do `Email` i `Password` właściwości generują metadane w modelu. Pomocnik tagu wejściowego korzysta z metadanych modelu i tworzy atrybuty `data-val-*` [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) (zobacz [Walidacja modelu](../models/validation.md)). Te atrybuty opisują moduły walidacji do dołączenia do pól wejściowych. Zapewnia to niezauważalne sprawdzanie poprawności HTML5 i [jQuery](https://jquery.com/) . Atrybuty niezauważalne mają format `data-val-rule="Error Message"`, gdzie Rule to nazwa reguły walidacji (na przykład `data-val-required`, `data-val-email`, `data-val-maxlength`itp.). Jeśli w atrybucie jest podany komunikat o błędzie, jest on wyświetlany jako wartość atrybutu `data-val-rule`. Istnieją także atrybuty formularza `data-val-ruleName-argumentName="argumentValue"`, które zawierają dodatkowe szczegóły dotyczące reguły, na przykład `data-val-maxlength-max="1024"`.

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Alternatywa pomocnika HTML dla pomocnika tagów wejściowych

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` i `Html.EditorFor` mają nakładające się funkcje pomocnika tagów wejściowych. Pomocnik tagu wejściowego automatycznie ustawi atrybut `type`; nie `Html.TextBox` i `Html.TextBoxFor`. `Html.Editor` i `Html.EditorFor` obsługi kolekcji, złożonych obiektów i szablonów; Pomocnik tagu wejściowego nie. Pomocnik tagu wejściowego, `Html.EditorFor` i `Html.TextBoxFor` są silnie wpisane (używa wyrażeń lambda); `Html.TextBox` i `Html.Editor` nie są (używają nazw wyrażeń).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()` i `@Html.EditorFor()` użyć specjalnej pozycji `ViewDataDictionary` o nazwie `htmlAttributes` podczas wykonywania ich szablonów domyślnych. To zachowanie jest opcjonalnie rozszerzane przy użyciu parametrów `additionalViewData`. W kluczu "htmlAttributes" nie jest rozróżniana wielkość liter. Klucz "htmlAttributes" jest obsługiwany w podobny sposób, jak obiekt `htmlAttributes` przekazywać do pomocników wejściowych, takich jak `@Html.TextBox()`.

```cshtml
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Nazwy wyrażeń

Wartość atrybutu `asp-for` jest `ModelExpression` i prawej strony wyrażenia lambda. W związku z tym `asp-for="Property1"` stać się `m => m.Property1` w wygenerowanym kodzie, co oznacza, że nie musisz prefiksować za pomocą `Model`. Możesz użyć znaku "\@", aby rozpocząć wyrażenie śródwierszowe i przejść przed `m.`:

```cshtml
@{
  var joe = "Joe";
}

<input asp-for="@joe">
```

Generuje następujące elementy:

```html
<input type="text" id="joe" name="joe" value="Joe">
```

W przypadku właściwości kolekcji `asp-for="CollectionProperty[23].Member"` generuje taką samą nazwę co `asp-for="CollectionProperty[i].Member"`, gdy `i` ma `23`wartość.

Gdy ASP.NET Core MVC oblicza wartość `ModelExpression`, sprawdza kilka źródeł, w tym `ModelState`. Rozważ `<input type="text" asp-for="@Name">`. Obliczony atrybut `value` jest pierwszą wartością o wartości innej niż null z:

* `ModelState` wpis z kluczem "name".
* Wynik wyrażenia `Model.Name`.

### <a name="navigating-child-properties"></a>Nawigowanie po właściwościach podrzędnych

Możesz również przejść do właściwości podrzędnych przy użyciu ścieżki właściwości modelu widoku. Rozważmy bardziej złożoną klasę modelu, która zawiera właściwość podrzędną `Address`.

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

W widoku zostanie powiązana `Address.AddressLine1`:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Następujący kod HTML jest generowany dla `Address.AddressLine1`:

```html
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="">
```

### <a name="expression-names-and-collections"></a>Nazwy wyrażeń i kolekcje

Przykładowy model zawierający tablicę `Colors`:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Metoda akcji:

```csharp
public IActionResult Edit(int id, int colorIndex)
{
    ViewData["Index"] = colorIndex;
    return View(GetPerson(id));
}
```

Poniższy Razor pokazuje, jak uzyskać dostęp do określonego elementu `Color`:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

Szablon *widoki/Shared/EditorTemplates/String. cshtml* :

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Przykład przy użyciu `List<T>`:

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Poniższy Razor pokazuje, jak wykonać iterację kolekcji:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

Szablon *widoki/Shared/EditorTemplates/ToDoItem. cshtml* :

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

`foreach` powinna być używana, jeśli jest to możliwe, gdy wartość ma być używana w kontekście `asp-for` lub `Html.DisplayFor` równoważnej. Ogólnie rzecz biorąc, `for` jest lepsza niż `foreach` (Jeśli scenariusz to umożliwia), ponieważ nie musi alokować modułu wyliczającego; jednak ocenianie indeksatora w wyrażeniu LINQ może być kosztowne i powinno być zminimalizowane.

&nbsp;

>[!NOTE]
>Powyższy przykładowy kod z komentarzem pokazuje, jak zamienić wyrażenie lambda z operatorem `@`, aby uzyskać dostęp do każdego `ToDoItem` na liście.

## <a name="the-textarea-tag-helper"></a>Pomocnik tagów TextArea

Pomocnik tagów `Textarea Tag Helper` jest podobny do pomocnika tagu wejściowego.

* Generuje atrybuty `id` i `name` oraz atrybuty walidacji danych z modelu dla [\<elementu textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) .

* Zapewnia silne wpisywanie.

* Alternatywa dla pomocnika HTML: `Html.TextAreaFor`

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Następujący kod HTML jest generowany:

```html
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

## <a name="the-label-tag-helper"></a>Pomocnik tagów etykiety

* Generuje podpis etykiety i atrybut `for` na [\<etykieta >](https://www.w3.org/wiki/HTML/Elements/label) elementu dla nazwy wyrażenia

* Alternatywa dla pomocnika HTML: `Html.LabelFor`.

`Label Tag Helper` zapewnia następujące korzyści w stosunku do czystego elementu etykiety HTML:

* Wartość etykiety opisowej jest automatycznie pobierana z atrybutu `Display`. Zamierzona nazwa wyświetlana może ulec zmianie z upływem czasu, a kombinacja `Display` atrybutu i tagu etykiety będzie stosowała `Display` wszędzie tam, gdzie jest używana.

* Mniej znaczników w kodzie źródłowym

* Silne pisanie przy użyciu właściwości model.

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Następujący kod HTML jest generowany dla elementu `<label>`:

```html
<label for="Email">Email Address</label>
```

Pomocnik tagu etykiety wygenerował `for` wartość atrybutu "E-mail", który jest IDENTYFIKATORem skojarzonym z elementem `<input>`. Pomocnicy tagów generują spójne elementy `id` i `for`, dzięki czemu mogą być prawidłowo skojarzone. Podpis w tym przykładzie pochodzi z atrybutu `Display`. Jeśli model nie zawierał atrybutu `Display`, będzie to nazwa właściwości wyrażenia.

## <a name="the-validation-tag-helpers"></a>Pomocnicy tagów walidacji

Istnieją dwa pomocnicy tagów sprawdzania poprawności. `Validation Message Tag Helper` (który wyświetla komunikat weryfikacyjny dla pojedynczej właściwości w modelu) i `Validation Summary Tag Helper` (w którym wyświetlane jest podsumowanie błędów walidacji). `Input Tag Helper` dodaje atrybuty walidacji po stronie klienta HTML5 do elementów wejściowych na podstawie atrybutów adnotacji danych klas modelu. Sprawdzanie poprawności jest również wykonywane na serwerze. Pomocnik tagów walidacji wyświetla te komunikaty o błędach w przypadku wystąpienia błędu walidacji.

### <a name="the-validation-message-tag-helper"></a>Pomocnik tagu komunikatu weryfikacji

* Dodaje atrybut [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)`data-valmsg-for="property"` do elementu [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) , który dołącza komunikaty o błędach walidacji w polu wejściowym określonej właściwości modelu. Gdy wystąpi błąd walidacji po stronie klienta, program [jQuery](https://jquery.com/) wyświetla komunikat o błędzie w elemencie `<span>`.

* Sprawdzanie poprawności odbywa się również na serwerze. Klienci mogą mieć wyłączone skrypty JavaScript, a niektóre sprawdzanie poprawności można wykonać tylko po stronie serwera.

* Alternatywa dla pomocnika HTML: `Html.ValidationMessageFor`

`Validation Message Tag Helper` jest używany z atrybutem `asp-validation-for` w elemencie [zakresu](https://developer.mozilla.org/docs/Web/HTML/Element/span) html.

```cshtml
<span asp-validation-for="Email"></span>
```

Pomocnik tagu komunikatu weryfikacji wygeneruje następujący kod HTML:

```html
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Zwykle używasz `Validation Message Tag Helper` po Pomocniku tagu `Input` dla tej samej właściwości. Spowoduje to wyświetlenie wszystkich komunikatów o błędach weryfikacji obok danych wejściowych, które spowodowały błąd.

> [!NOTE]
> Musisz mieć widok z prawidłowymi odwołaniami do skryptów JavaScript i [jQuery](https://jquery.com/) na potrzeby weryfikacji po stronie klienta. Aby uzyskać więcej informacji, zobacz [Walidacja modelu](../models/validation.md) .

Gdy wystąpi błąd walidacji po stronie serwera (na przykład w przypadku wyłączenia niestandardowej walidacji po stronie serwera lub weryfikacji po stronie klienta), MVC umieszcza ten komunikat o błędzie jako treść elementu `<span>`.

```html
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Pomocnik tagów podsumowania walidacji

* Elementy docelowe `<div>` z atrybutem `asp-validation-summary`

* Alternatywa dla pomocnika HTML: `@Html.ValidationSummary`

`Validation Summary Tag Helper` służy do wyświetlania podsumowania komunikatów weryfikacyjnych. Wartość atrybutu `asp-validation-summary` może być dowolną z następujących:

|ASP-Walidacja — podsumowanie|Wyświetlane komunikaty weryfikacji|
|--- |--- |
|ValidationSummary.All|Poziom właściwości i modelu|
|ValidationSummary.ModelOnly|Modelowanie|
|Podsumowania walidacji. None|None|

### <a name="sample"></a>Sample

W poniższym przykładzie model danych ma atrybuty `DataAnnotation`, które generują komunikaty o błędach walidacji w elemencie `<input>`.  Gdy wystąpi błąd walidacji, pomocnik tagów walidacji wyświetli komunikat o błędzie:

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Wygenerowany kod HTML (gdy model jest prawidłowy):

```html
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

## <a name="the-select-tag-helper"></a>Pomocnik wybierania tagu

* Generuje elementy [opcji](https://www.w3.org/wiki/HTML/Elements/option) [zaznaczania](https://www.w3.org/wiki/HTML/Elements/select) i skojarzone dla właściwości modelu.

* Ma `Html.DropDownListFor` alternatywną dla pomocnika HTML i `Html.ListBoxFor`

`Select Tag Helper` `asp-for` określa nazwę właściwości modelu dla elementu [SELECT](https://www.w3.org/wiki/HTML/Elements/select) i `asp-items` określa elementy [opcji](https://www.w3.org/wiki/HTML/Elements/option) .  Na przykład:

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

Metoda `Index` inicjuje `CountryViewModel`, ustawia wybrany kraj i przekazuje go do widoku `Index`.

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

Metoda HTTP POST `Index` wyświetla zaznaczenie:

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

Widok `Index`:

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Który generuje następujący kod HTML (z wybranym "CA"):

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
> Nie zaleca się używania `ViewBag` ani `ViewData` z pomocnikiem SELECT tag. Model widoku jest bardziej niezawodny przy udostępnianiu metadanych MVC i ogólnie mniej problematycznych.

Wartość atrybutu `asp-for` jest szczególnym przypadkiem i nie wymaga prefiksu `Model`, inne atrybuty pomocnika tagów to (takie jak `asp-items`).

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Stałe powiązania

Często wygodnie jest używać `<select>` z właściwością `enum` i generować elementy `SelectListItem` z wartości `enum`.

Przykład:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

Metoda `GetEnumSelectList` generuje obiekt `SelectList` dla wyliczenia.

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Można oznaczyć listę modułów wyliczających atrybutem `Display`, aby uzyskać bogatszy interfejs użytkownika:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Następujący kod HTML jest generowany:

```html
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

### <a name="option-group"></a>Grupa opcji

Element [> HTML\<optgroup](https://www.w3.org/wiki/HTML/Elements/optgroup) jest generowany, gdy model widoku zawiera jeden lub więcej obiektów `SelectListGroup`.

`CountryViewModelGroup` grupuje `SelectListItem` elementy do grup "Ameryka Północna" i "Europa":

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

Poniżej przedstawiono dwie grupy:

![przykład grupy opcji](working-with-forms/_static/grp.png)

Wygenerowany kod HTML:

```html
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

Pomocnik Wybierz tag automatycznie generuje atrybut [wielokrotne = "Multiple"](https://w3c.github.io/html-reference/select.html) , jeśli właściwość określona w atrybucie `asp-for` jest `IEnumerable`. Na przykład, uwzględniając następujący model:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Z następującym widokiem:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Generuje następujący kod HTML:

```html
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

Jeśli znajdziesz samodzielnie opcję "nie określono" na wielu stronach, możesz utworzyć szablon, aby wyeliminować powtarzanie kodu HTML:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

Szablon *widoki/Shared/EditorTemplates/CountryViewModel. cshtml* :

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

Dodawanie [opcji\<HTML >](https://www.w3.org/wiki/HTML/Elements/option) elementów nie jest ograniczone do *żadnego przypadku zaznaczania* . Na przykład następująca metoda widok i akcja spowoduje wygenerowanie kodu HTML podobnego do powyższego kodu:

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?name=snippetNone)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Zostanie wybrany poprawny element `<option>` (zawierający atrybut `selected="selected"`) w zależności od bieżącej wartości `Country`.

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

```html
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
* [Token weryfikacji żądania](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* [IAttributeAdapter, interfejs](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [Fragmenty kodu dla tego dokumentu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
