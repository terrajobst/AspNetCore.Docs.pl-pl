---
title: "Pomocników tagów w formularzy w programie ASP.NET Core"
author: rick-anderson
description: "W tym artykule opisano wbudowane pomocników tagów używane w formularzach."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9fd51755e1dc9a1dfb9ab5cc4558f7da9475ce32
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>Wprowadzenie do korzystania z pomocników tagów w formularzy w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), i [Jerrie Pelser](https://github.com/jerriep)

W tym dokumencie przedstawiono pracy z formularzami i elementy HTML często używane w formularzu. Kod HTML [formularza](https://www.w3.org/TR/html401/interact/forms.html) element udostępnia użycia aplikacji sieci web podstawowego mechanizmu publikowania dane do serwera. Większość ten dokument zawiera opis [pomocników tagów](tag-helpers/intro.md) i jak ich można efektywnie tworzyć niezawodne formularzy HTML. Zalecamy przeczytanie [wprowadzenie do pomocników tagów](tag-helpers/intro.md) przed przeczytaniem tego dokumentu.

W wielu przypadkach pomocników HTML Podaj informacje o innym podejściu do określonych pomocniczego znacznika, ale ważne jest, aby rozpoznać pomocników tagów nie Zastąp pomocników HTML, a nie ma tagu pomocnika dla każdego pomocnika kodu HTML. Jeśli istnieje alternatywna pomocnika kodu HTML, to wymienione.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Pomocnik Tag formularza

[Formularza](https://www.w3.org/TR/html401/interact/forms.html) pomocnika tagów:

* Generuje kod HTML [ \<formularza >](https://www.w3.org/TR/html401/interact/forms.html) `action` wartości atrybutu akcji kontrolera MVC lub nazwanej trasy

* Generuje ukryty [żądania weryfikacji tokenu](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) celu zapobiegania fałszowaniu żądania między witrynami (w przypadku użycia z `[ValidateAntiForgeryToken]` atrybutu w metodzie akcji HTTP Post)

* Udostępnia `asp-route-<Parameter Name>` atrybutu, gdzie `<Parameter Name>` jest dodawany do wartości trasy. `routeValues` Parametry `Html.BeginForm` i `Html.BeginRouteForm` zapewniają podobne funkcje.

* Jest to alternatywa pomocnika kodu HTML `Html.BeginForm` i`Html.BeginRouteForm`

Przykład:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

Pomocnik Tag formularza powyżej generuje poniższy kod HTML:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

Środowisko uruchomieniowe MVC generuje `action` wartość atrybutu z atrybutów pomocnika Tag formularza `asp-controller` i `asp-action`. Pomocnik Tag formularza również generuje ukryty [żądania weryfikacji tokenu](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) celu zapobiegania fałszowaniu żądania między witrynami (w przypadku użycia z `[ValidateAntiForgeryToken]` atrybutu w metodzie akcji Post protokołu HTTP). Ochrona czysty formularza HTML przed sfałszowaniem żądań cross-site jest trudne, Pomocnik Tag formularza zapewnia tej usługi można.

### <a name="using-a-named-route"></a>Za pomocą nazwanej trasy

`asp-route` Atrybut pomocnika tagów można również generować kod znaczników dla kodu HTML `action` atrybutu. Aplikację z [trasy](../../fundamentals/routing.md) o nazwie `register` można użyć następujących znaczników dla strony rejestracji:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Wiele widoków w *widoków/konta* folder (generowane podczas tworzenia nowej aplikacji sieci web z *indywidualnych kont użytkowników*) zawierać [asp trasy returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) atrybutu:

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Wbudowane szablony `returnUrl` tylko jest wypełniane automatycznie, gdy próbują uzyskać dostęp do autoryzowanych zasobów, ale nie ma uwierzytelniony ani autoryzacji. Podczas próby nieautoryzowanego dostępu, oprogramowanie pośredniczące zabezpieczeń przekieruje Cię do strony logowania z `returnUrl` ustawiony.

## <a name="the-input-tag-helper"></a>Pomocnik tagu wejściowego

Wejściowy pomocnika tagów wiąże HTML [ \<wejściowych >](https://www.w3.org/wiki/HTML/Elements/input) element na wyrażenie modelu w widoku razor.

Składnia:

```HTML
<input asp-for="<Expression Name>" />
```

Pomocnik tagu wejściowego:

* Generuje `id` i `name` atrybutów HTML dla podanej nazwy wyrażenia w `asp-for` atrybutu. `asp-for="Property1.Property2"`jest odpowiednikiem `m => m.Property1.Property2`. Nazwa wyrażenia jest, do czego służy `asp-for` wartość atrybutu. Zobacz [nazwy wyrażeń](#expression-names) sekcji, aby uzyskać dodatkowe informacje.

* Ustawia kod HTML `type` atrybutu wartości na podstawie typu modelu i [adnotacji danych elementu](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty zastosowane do właściwości modelu

* Nie zastępować HTML `type` wartość atrybutu, gdy został określony

* Generuje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) sprawdzania poprawności atrybutów z [adnotacji danych elementu](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty stosowane do właściwości modelu

* Funkcję pomocnika kodu HTML, które nakładają się na `Html.TextBoxFor` i `Html.EditorFor`. Zobacz **alternatyw pomocnika kodu HTML do pomocniczego Tag danych wejściowych** sekcji, aby uzyskać szczegółowe informacje.

* Umożliwia wpisanie silne. Jeśli nazwa zmiany właściwości, a nie Aktualizuj pomocnika tagów pojawi błąd podobny do następującego:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input` Pomocnika tagów ustawia HTML `type` atrybut oparty na typie .NET. W poniższej tabeli wymieniono niektóre typowe typów .NET i wygenerowanego typu HTML (nie każdy typ architektury .NET to wymienione).

|Typ architektury .NET|Typ danych wejściowych|
|---|---|
|wartość logiczna|type=”checkbox”|
|String|Typ = "text"|
|DataGodzina|type=”datetime”|
|Byte|Typ = "number"|
|int|Typ = "number"|
|Single, Double|Typ = "number"|


W poniższej tabeli przedstawiono niektóre typowe [adnotacji danych](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty, które pomocnika tagu wejściowego przypisze do określonych typów wejściowych (nie każdy atrybut weryfikacji znajduje się):


|Atrybut|Typ danych wejściowych|
|---|---|
|[EmailAddress]|type=”email”|
|[Url]|type=”url”|
|[HiddenInput]|type=”hidden”|
|[Phone]|type=”tel”|
|[DataType(DataType.Password)]| type=”password”|
|[DataType(DataType.Date)]| Typ "date" =|
|[DataType(DataType.Time)]| Typ = "razem"|


Przykład:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Powyższy kod generuje poniższy kod HTML:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

Adnotacje danych dotyczy `Email` i `Password` właściwości Generowanie metadanych dla modelu. Pomocnik Tag danych wejściowych zużywa metadanych modelu i tworzy [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atrybutów (zobacz [sprawdzania poprawności modelu](../models/validation.md)). Te atrybuty opisują moduły weryfikacji, aby dołączyć do pól wejściowych. Zapewnia to dyskretnego kodu HTML5 i [jQuery](https://jquery.com/) sprawdzania poprawności. Atrybuty dyskretnego kodu mają format `data-val-rule="Error Message"`, gdzie reguła jest nazwa reguły sprawdzania poprawności (takich jak `data-val-required`, `data-val-email`, `data-val-maxlength`itp.) Jeśli komunikat o błędzie jest podana w atrybucie, jest wyświetlany jako wartość `data-val-rule` atrybutu. Istnieją również atrybuty formularza `data-val-ruleName-argumentName="argumentValue"` dodatkowe szczegóły dotyczące reguły, które zapewniają na przykład `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Alternatywy pomocnika kodu HTML dla danych wejściowych pomocnika tagów

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` i `Html.EditorFor` mają pokrywające się funkcji przy użyciu Pomocnika Tag danych wejściowych. Automatycznie ustawi pomocnika Tag danych wejściowych `type` atrybutu; `Html.TextBox` i `Html.TextBoxFor` nie. `Html.Editor`i `Html.EditorFor` obsługiwać kolekcje obiektów złożonych i szablonów; nie pomocnika Tag danych wejściowych. Pomocnik Tag danych wejściowych, `Html.EditorFor` i `Html.TextBoxFor` są silnie typizowane (one użycie wyrażeń lambda); `Html.TextBox` i `Html.Editor` nie są (używają nazwy wyrażeń).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`i `@Html.EditorFor()` użycia specjalnego `ViewDataDictionary` wpis o nazwie `htmlAttributes` podczas wykonywania ich domyślnych szablonów. To zachowanie jest opcjonalnie rozszerzone przy użyciu `additionalViewData` parametrów. Klucz "htmlAttributes" jest rozróżniana wielkość liter. Klucz "htmlAttributes" odbywa się podobnie do `htmlAttributes` obiekt przekazany do danych wejściowych pomocników, takich jak `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Nazwy wyrażeń

`asp-for` Wartość atrybutu jest `ModelExpression` i po prawej stronie wyrażenia lambda. W związku z tym `asp-for="Property1"` staje się `m => m.Property1` w wygenerowanym kodzie, który, dlatego nie trzeba prefiks z `Model`. Można użyć "@" znak do uruchamiania wyrażenia wbudowanego i przenieść przed `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Generuje następujące czynności:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

Z właściwościami kolekcji `asp-for="CollectionProperty[23].Member"` generuje taką samą nazwę jak `asp-for="CollectionProperty[i].Member"` podczas `i` ma wartość `23`.

### <a name="navigating-child-properties"></a>Nawigowanie po właściwości podrzędnej

Można także przejść do właściwości podrzędnej przy użyciu ścieżki właściwości modelu widoku. Należy wziąć pod uwagę bardziej złożonych klasy modelu, który zawiera element podrzędny `Address` właściwości.

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

W widoku, możemy powiązać `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Poniższy kod HTML jest generowane dla `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a>Wyrażenie nazwy i kolekcji

Przykładowy model zawierający tablicę `Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Metody akcji:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

Następujące Razor pokazuje, jak uzyskać dostępu do określonego `Color` elementu:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml* szablonu:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Przykładowe przy użyciu `List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Następujące Razor pokazano, jak Iterowanie przez kolekcję:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/ToDoItem.cshtml* szablonu:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>Zawsze używaj `for` (i *nie* `foreach`) do wykonywania iteracji listy. Ocena indeksatora w składniku LINQ wyrażenie może być kosztowne i powinien być minimalny.

&nbsp;

>[!NOTE]
>Powyższym kodzie przykładowym komentarze pokazuje, jak należy zastąpić wyrażenia lambda za pomocą `@` operatora, aby uzyskać dostęp każdy `ToDoItem` na liście.

## <a name="the-textarea-tag-helper"></a>Pomocnik Textarea Tag

`Textarea Tag Helper` Pomocnika tagów jest podobny do pomocniczego Tag danych wejściowych.

* Generuje `id` i `name` atrybuty oraz atrybuty weryfikacji danych z modelu dla [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elementu.

* Umożliwia wpisanie silne.

* Alternatywa pomocnika kodu HTML:`Html.TextAreaFor`

Przykład:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

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
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a>Etykieta pomocnika tagów

* Generuje podpis etykiety i `for` atrybutu [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) elementu nazwa wyrażenia

* Alternatywne pomocnika kodu HTML: `Html.LabelFor`.

`Label Tag Helper` Zapewnia następujące korzyści przez pure element label kodu HTML:

* Automatycznie pobrana wartość opisową etykietę `Display` atrybutu. Nazwa wyświetlana danego może zmieniać wraz z upływem czasu i kombinacja `Display` będą miały zastosowanie atrybutu i pomocnika tagów etykiety `Display` everywhere jest używany.

* Mniej znacznika w kodzie źródłowym

* Silne wpisywanie razem z właściwością modelu.

Przykład:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Poniższy kod HTML jest generowane dla `<label>` elementu:

```HTML
<label for="Email">Email Address</label>
```

Pomocnika tagów etykiety wygenerowany `for` wartość atrybutu "E-mail", który jest identyfikator skojarzony z `<input>` elementu. Generowanie spójne pomocników tagów `id` i `for` elementy, tak aby były prawidłowo skojarzony. Podpis w tym przykładzie pochodzi z `Display` atrybutu. Jeśli model nie zawiera `Display` atrybut podpisu może być nazwą właściwości wyrażenia.

## <a name="the-validation-tag-helpers"></a>Sprawdzanie poprawności pomocników tagów

Istnieją dwa pomocników tagów sprawdzania poprawności. `Validation Message Tag Helper` (Która wyświetla komunikat dotyczący sprawdzania poprawności dla jednej właściwości w modelu) i `Validation Summary Tag Helper` (która wyświetla podsumowanie błędy sprawdzania poprawności). `Input Tag Helper` Dodaje atrybuty weryfikacji po stronie klienta HTML5 do wprowadzania elementów na podstawie danych atrybuty adnotacji w klasach modeli. Sprawdzanie poprawności również jest wykonywane na serwerze. Pomocnika tagów weryfikacji wyświetla te komunikaty o błędach, gdy wystąpi błąd sprawdzania poprawności.

### <a name="the-validation-message-tag-helper"></a>Pomocnik Tag komunikatu weryfikacji

* Dodaje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` atrybutu [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, który dołącza komunikatów o błędach weryfikacji na pole wejściowe właściwości określonego modelu. Gdy wystąpi błąd weryfikacji po stronie klienta, [jQuery](https://jquery.com/) wyświetla komunikat o błędzie w `<span>` elementu.

* Sprawdzanie poprawności również odbywa się na serwerze. Klienci mogą mieć JavaScript wyłączona i niektóre sprawdzania poprawności jest możliwe tylko po stronie serwera.

* Alternatywa pomocnika kodu HTML:`Html.ValidationMessageFor`

`Validation Message Tag Helper` Jest używany z `asp-validation-for` atrybutu HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elementu.

```HTML
<span asp-validation-for="Email"></span>
```

Poniższy kod HTML generuje pomocnika tagów komunikatu sprawdzania poprawności:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Na ogół służy `Validation Message Tag Helper` po `Input` pomocnika tagów dla tej samej właściwości. Dzięki temu wyświetla komunikaty o błędach weryfikacji w pobliżu danych wejściowych, który spowodował błąd.

> [!NOTE]
> Musi mieć widoku z poprawną JavaScript i [jQuery](https://jquery.com/) skryptu odwołań w celu weryfikacji po stronie klienta. Zobacz [sprawdzania poprawności modelu](../models/validation.md) Aby uzyskać więcej informacji.

W przypadku wystąpienia błędu weryfikacji po stronie serwera (na przykład po masz weryfikacji po stronie serwera niestandardowych lub weryfikacji po stronie klienta jest wyłączony), MVC umieszcza ten komunikat o błędzie jako treść `<span>` elementu.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Pomocnik weryfikacji Summary — Tag

* Obiekty docelowe `<div>` elementy z `asp-validation-summary` atrybutu

* Alternatywa pomocnika kodu HTML:`@Html.ValidationSummary`

`Validation Summary Tag Helper` Służy do wyświetlania podsumowania komunikatów dotyczących sprawdzania poprawności. `asp-validation-summary` Wartość atrybutu może być dowolną z następujących czynności:

|Podsumowanie ASP-sprawdzania poprawności|Wyświetlane komunikatów dotyczących sprawdzania poprawności|
|--- |--- |
|ValidationSummary.All|Właściwości i modelu|
|ValidationSummary.ModelOnly|Model|
|ValidationSummary.None|Brak|

### <a name="sample"></a>Przykład

W poniższym przykładzie zostanie nadany modelu danych `DataAnnotation` atrybuty, które generuje komunikaty o błędach weryfikacji na `<input>` elementu.  Gdy wystąpi błąd sprawdzania poprawności, weryfikacji pomocnika tagów wyświetla komunikat o błędzie:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Wygenerowany kod HTML, (Jeśli model jest nieprawidłowy):

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a>Pomocnik tagu Select

* Generuje [wybierz](https://www.w3.org/wiki/HTML/Elements/select) i skojarzone [opcji](https://www.w3.org/wiki/HTML/Elements/option) elementy dla właściwości modelu.

* Jest to alternatywa pomocnika kodu HTML `Html.DropDownListFor` i`Html.ListBoxFor`

`Select Tag Helper` `asp-for` Określa nazwę właściwości modelu [wybierz](https://www.w3.org/wiki/HTML/Elements/select) elementu i `asp-items` Określa [opcji](https://www.w3.org/wiki/HTML/Elements/option) elementów.  Na przykład:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Przykład:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index` Inicjuje metody `CountryViewModel`, ustawia wybranego kraju i przekazuje je do `Index` widoku.

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

HTTP POST `Index` metoda Wyświetla zaznaczenie:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index` Widoku:

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Generująca poniższy kod HTML (za pomocą "CA" wybrane):

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> Nie zaleca się przy użyciu `ViewBag` lub `ViewData` z Pomocnika tagów wybierz. Model widoku jest bardziej niezawodne zapewnienie metadanych MVC i zazwyczaj mniej powodować problemy.

`asp-for` Wartość atrybutu jest szczególnych przypadkach i nie wymaga `Model` prefiksu, inne są atrybuty pomocnika tagów (takie jak `asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Powiązanie wyliczenia

Często jest to łatwe w użyciu `<select>` z `enum` właściwości i generować `SelectListItem` elementy z `enum` wartości.

Przykład:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList` Metoda generuje `SelectList` obiektu dla wyliczenia.

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Można dekoracji lista modułu wyliczającego `Display` atrybutu, aby uzyskać bardziej zaawansowane funkcje interfejsu użytkownika:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

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
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a>Opcja grupy

Kod HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) element jest generowany, gdy model widoku zawiera jeden lub więcej `SelectListGroup` obiektów.

`CountryViewModelGroup` Grup `SelectListItem` elementy w grupach "Europy" i "Ameryki Północnej":

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

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
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a>Wielokrotny wybór

Automatycznie wygeneruje pomocnika tagów wybierz [wielu = "wielu"](http://w3c.github.io/html-reference/select.html) atrybut, jeśli określona właściwość w `asp-for` atrybutu `IEnumerable`. Na przykład podane następującego modelu:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Z następującego widoku:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

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
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a>Brak zaznaczenia

Okaże się przy użyciu opcji "nie został określony" na wielu stronach, po utworzeniu szablonu celu wyeliminowania powtarzających HTML:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml* szablonu:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

Dodawanie HTML [ \<opcji >](https://www.w3.org/wiki/HTML/Elements/option) elementów nie jest ograniczona do *Brak zaznaczenia* case. Na przykład następujące metody akcji i widoku wygeneruje HTML, podobnie jak w powyższym kodzie:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Poprawny `<option>` będzie można wybrać elementu (zawierać `selected="selected"` atrybut) w zależności od bieżącej `Country` wartość.

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocnicy tagów](tag-helpers/intro.md)

* [Element formularza HTML](https://www.w3.org/TR/html401/interact/forms.html)

* [Token weryfikacji żądania](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [Wiązanie modelu](../models/model-binding.md)

* [Weryfikacja modelu](../models/validation.md)

* [adnotacji danych](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* [Kod fragmenty kodu dla tego dokumentu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).
