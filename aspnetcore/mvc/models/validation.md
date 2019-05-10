---
title: Weryfikacja modelu w programie ASP.NET Core MVC
author: tdykstra
description: Więcej informacji o weryfikacji modelu w programie ASP.NET Core MVC i stron Razor.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
monikerRange: '>= aspnetcore-2.1'
uid: mvc/models/validation
ms.openlocfilehash: acb0ae989f6e82a5bc80935a8acfc96e51073d2f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898396"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a>Weryfikacja modelu w programie ASP.NET Core MVC i stron Razor

W tym artykule opisano sposób sprawdzania poprawności danych wejściowych użytkownika w aplikacji ASP.NET Core MVC lub stron Razor.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

## <a name="model-state"></a>Stan modelu

Stan modelu reprezentuje błędy, które pochodzą z dwóch podsystemów: wiązania modelu i sprawdzanie poprawności modelu. Błędy, które pochodzą z [wiązanie modelu](model-binding.md) są zwykle błędów konwersji danych (na przykład, "x" jest wprowadzana w polu, które oczekuje, że liczba całkowita). Sprawdzanie poprawności modelu występuje po wiązania modelu i raporty błędów, gdy dane nie są zgodne z regułami biznesowymi (na przykład 0 jest wprowadzana w polu, które oczekuje, że ma klasyfikację od 1 do 5).

Zarówno powiązanie modelu, jak i weryfikacja odbywają się przed wykonaniem tej akcji kontrolera lub metody obsługi stron Razor. Dla aplikacji sieci web odpowiada aplikacji aby sprawdzić `ModelState.IsValid` i odpowiednio reagują. Aplikacje internetowe zazwyczaj ponownie wyświetlić stronę komunikatu o błędzie:

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

Kontrolery interfejsu API sieci Web nie ma pod `ModelState.IsValid` jeśli mają one `[ApiController]` atrybutu. W takim przypadku automatyczne HTTP 400 odpowiedzi zawierające szczegółowe informacje o problemie zwracany jest stan modelu jest nieprawidłowy. Aby uzyskać więcej informacji, zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses).

## <a name="rerun-validation"></a>Uruchom ponownie sprawdzanie poprawności

Sprawdzanie poprawności jest automatyczne, ale może być konieczne powtórzenie go ręcznie. Na przykład możesz obliczyć wartość dla właściwości i aby ponownie uruchomić sprawdzanie poprawności po ustawieniu właściwości na podstawie wartości. Ponowne uruchamianie sprawdzania poprawności, należy wywołać `TryValidateModel` metody, jak pokazano poniżej:

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a>Sprawdzanie poprawności atrybutów

Sprawdzanie poprawności atrybutów umożliwiają określenie reguł sprawdzania poprawności dla właściwości modelu. Poniższy przykład z [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) zawiera klasę modelu, który jest oznaczony za pomocą atrybutów sprawdzania poprawności. `[ClassicMovie]` Atrybut jest atrybutem niestandardowego sprawdzania poprawności, a pozostałe są wbudowane. (Niewyświetlany jest `[ClassicMovie2]`, który zawiera alternatywny sposób implementacji atrybutu niestandardowego.)

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a>Wbudowanych atrybutów

Oto niektóre atrybuty weryfikacji wbudowane:

* `[CreditCard]`: Weryfikuje, że właściwość ma format karty kredytowej.
* `[Compare]`: Sprawdza poprawność tego dwie właściwości w modelu są zgodne.
* `[EmailAddress]`: Weryfikuje, że właściwość ma format adresu e-mail.
* `[Phone]`: Weryfikuje, że właściwość ma format numeru telefonu.
* `[Range]`: Sprawdza, czy wartość właściwości mieści się w określonym zakresie.
* `[RegularExpression]`: Weryfikuje, że wartość właściwości odpowiada określonemu wyrażeniu regularnemu.
* `[Required]`: Sprawdza, czy pole nie jest null. Zobacz [atrybutu [wymagane]](#required-attribute) szczegółowe informacje na temat zachowania tego atrybutu.
* `[StringLength]`: Weryfikuje, że wartość właściwości ciągu nie przekracza limit określonej długości.
* `[Url]`: Weryfikuje, że właściwość ma format adresu URL.
* `[Remote]`: Sprawdza poprawność danych wejściowych na komputerze klienckim przez wywołanie metody akcji, na serwerze. Zobacz [atrybutu [zdalnego]](#remote-attribute) szczegółowe informacje na temat zachowania tego atrybutu.

Pełna lista atrybutów weryfikacji można znaleźć w [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) przestrzeni nazw.

### <a name="error-messages"></a>Komunikaty o błędach

Sprawdzanie poprawności atrybutów umożliwiają określenie komunikat o błędzie do wyświetlenia w nieprawidłowe dane wejściowe. Na przykład:

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

Wewnętrznie, wywołanie atrybuty `String.Format` z symbolem zastępczym dla nazwy pola i czasami dodatkowe symboli zastępczych. Na przykład:

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

Po zastosowaniu do `Name` właściwości, komunikat o błędzie, utworzone przez poprzedni kod będzie "długość nazwy musi wynosić od 6 i 8.".

Aby dowiedzieć się, które parametry są przekazywane do `String.Format` określonego atrybutu komunikatu o błędzie, zobacz [kod źródłowy DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).

## <a name="required-attribute"></a>Atrybut [wymagane]

Domyślnie system sprawdzania poprawności traktuje parametrów innych niż null lub właściwości, tak, jakby mieli oni `[Required]` atrybutu. [Typy wartości](/dotnet/csharp/language-reference/keywords/value-types) takich jak `decimal` i `int` nie dopuszczają wartości.

### <a name="required-validation-on-the-server"></a>[Wymagane] sprawdzanie poprawności na serwerze

Na serwerze wymagana wartość jest uważana za brak, jeśli właściwość ma wartość null. Dopuszcza pole zawsze jest prawidłowy, a komunikat o błędzie atrybutu [wymagane] nigdy nie jest wyświetlane.

Jednak wiązanie modelu dla właściwości niedopuszczającej może zakończyć się niepowodzeniem, co w komunikacie o błędzie, takich jak `The value '' is invalid`. Aby określić niestandardowy komunikat o błędzie weryfikacji po stronie serwera typów innych niż null, dostępne są następujące opcje:

* Zmień pole na wartość null (na przykład `decimal?` zamiast `decimal`). [Dopuszcza wartości null\<T >](/dotnet/csharp/programming-guide/nullable-types/) typy wartości są traktowane jak standardowe typy dopuszczające wartości null.
* Określ domyślny komunikat o błędzie do użycia przez powiązanie modelu, jak pokazano w poniższym przykładzie:

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  Aby uzyskać więcej informacji na temat błędów powiązanie modelu ustawić domyślne komunikaty dla, zobacz <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.

### <a name="required-validation-on-the-client"></a>Walidacja [wymagane] na komputerze klienckim

Typy dopuszczające wartości inne niż null i ciągi są obsługiwane inaczej na komputerze klienckim, w porównaniu do serwera. Na komputerze klienckim:

* Wartość jest obecna, tylko wtedy, gdy dane wejściowe podano dla niego. W związku z tym weryfikacji po stronie klienta obsługuje typy niedopuszczające takie same jak typy dopuszczające wartości null.
* Białe znaki w polu ciągu jest uznawany za prawidłowych danych wejściowych przez jQuery weryfikacji [wymagane](https://jqueryvalidation.org/required-method/) metody. Weryfikacji po stronie serwera uwzględnia pola wymaganego ciągu nieprawidłowy, jeśli tylko białe znaki jest wprowadzana.

Jak wspomniano wcześniej, typy niedopuszczające są traktowane tak, jakby mieli oni `[Required]` atrybutu. Oznacza to, że uzyskujesz weryfikacji po stronie klienta, nawet wtedy, gdy nie mają zastosowania `[Required]` atrybutu. Ale jeśli ten atrybut nie jest używana, otrzymasz domyślnego komunikatu o błędzie. Aby określić niestandardowy komunikat o błędzie, należy użyć atrybutu.

## <a name="remote-attribute"></a>Atrybut [zdalnego]

`[Remote]` Atrybut implementuje walidacji po stronie klienta, czy wymagane jest wywołanie metody na serwerze, aby ustalić, czy pole danych wejściowych jest nieprawidłowy. Na przykład aplikacja może być konieczne Sprawdź, czy nazwa użytkownika jest już używana.

Aby zaimplementować weryfikację zdalną:

1. Utwórz metodę akcji dla języka JavaScript do wywołania.  Sprawdź poprawność jQuery [zdalnego](https://jqueryvalidation.org/remote-method/) metoda oczekuje na odpowiedź JSON:

   * `"true"` oznacza, że dane wejściowe są prawidłowe.
   * `"false"`, `undefined`, lub `null` oznacza, że dane wejściowe są nieprawidłowe.  Wyświetl domyślny komunikat o błędzie.
   * Inny ciąg oznacza, że dane wejściowe są nieprawidłowe. Wyświetlenie ciągu jako niestandardowy komunikat o błędzie.

   Oto przykład metody akcji, która zwraca niestandardowy komunikat o błędzie:

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. W klasie modelu, dodawać adnotacje do właściwości o `[Remote]` atrybut, który wskazuje sprawdzania poprawności metody akcji, jak pokazano w poniższym przykładzie:

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]

### <a name="additional-fields"></a>Dodatkowe pola

`AdditionalFields` Właściwość `[Remote]` atrybut umożliwia weryfikowanie kombinacje pól w odniesieniu do danych na serwerze. Na przykład jeśli `User` modelu ma `FirstName` i `LastName` właściwości, warto sprawdzić, czy nie istniejący użytkownicy mają już tej pary nazw. Poniższy przykład pokazuje, jak używać `AdditionalFields`:

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

`AdditionalFields` można jawnie ustawić do ciągów `"FirstName"` i `"LastName"`, ale przy użyciu [ `nameof` ](/dotnet/csharp/language-reference/keywords/nameof) operator upraszcza później refaktoryzacji. Metody akcji dla tej weryfikacji musi zaakceptować, imię i ostatnie argumenty nazwy:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

Gdy użytkownik wprowadza imię lub nazwisko, JavaScript sprawia, że zdalnego wywołania, aby zobaczyć, że pary nazw jest już zajęta.

Aby sprawdzić, co najmniej dwa dodatkowe pola, podaj je jako listę rozdzielonych przecinkami. Na przykład, aby dodać `MiddleName` właściwością modelu, `[Remote]` atrybutu, jak pokazano w poniższym przykładzie:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, podobnie jak wszystkie argumenty atrybutu, musi być wyrażeniem stałym. W związku z tym, nie używaj [ciągiem interpolowanym](/dotnet/csharp/language-reference/keywords/interpolated-strings) lub zadzwoń <xref:System.String.Join*> zainicjować `AdditionalFields`.

## <a name="alternatives-to-built-in-attributes"></a>Alternatywy dla wbudowanych atrybutów

Jeśli potrzebujesz nie został dostarczony przez wbudowanych atrybutów sprawdzania poprawności, można:

* [Tworzenie atrybutów niestandardowych, które](#custom-attributes).
* [Implementowanie IValidatableObject](#ivalidatableobject).

## <a name="custom-attributes"></a>Atrybuty niestandardowe

Dla scenariuszy, które nie obsługują atrybuty weryfikacji wbudowanych można utworzyć niestandardowego sprawdzania poprawności atrybutów. Utwórz klasę, która dziedziczy z <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>i zastępowania <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> metody.

`IsValid` Metoda przyjmuje obiekt o nazwie *wartość*, czyli danych wejściowych, które ma zostać zweryfikowana. Akceptuje także przeciążenia `ValidationContext` obiektu, który zawiera dodatkowe informacje, takie jak wystąpienie modelu, utworzone przez wiązania modelu.

Poniższy przykład sprawdza, czy data wydania dla filmu w *klasycznego* gatunku nie jest późniejsza niż określonego roku. `[ClassicMovie2]` Atrybutu najpierw sprawdza gatunek i kontynuuje tylko, jeśli zawiera on *klasycznego*. Dla filmów zidentyfikowane jako klasyka sprawdza daty wydania, aby upewnić się, że nie jest nowszy niż limit przekazany do konstruktora atrybutu.)

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

`movie` Reprezentuje zmienną w poprzednim przykładzie `Movie` obiekt, który zawiera dane z przesyłania formularza. `IsValid` Metoda sprawdza, czy data i gatunku. Po pomyślnej weryfikacji `IsValid` zwraca `ValidationResult.Success` kodu. Podczas sprawdzania poprawności zakończy się niepowodzeniem, `ValidationResult` ze względu na błąd jest zwracany komunikat.

## <a name="ivalidatableobject"></a>IValidatableObject

Poprzedni przykład działa tylko w przypadku `Movie` typów. Inna opcja weryfikacji na poziomie klasy jest zaimplementowanie `IValidatableObject` w klasie modelu, jak pokazano w poniższym przykładzie:

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a>Sprawdzanie poprawności węzeł najwyższego poziomu

Obejmują węzłów najwyższego poziomu:

* Parametry akcji
* Właściwości kontrolera
* Parametry obsługi strony
* Strona właściwości modelu

Powiązane z modelu węzłów najwyższego poziomu są weryfikowane oprócz weryfikacji właściwości modelu. W poniższym przykładzie z przykładowej aplikacji `VerifyPhone` metoda używa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> do sprawdzania poprawności `phone` parametr akcji:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

Można użyć węzłów najwyższego poziomu <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> za pomocą atrybutów sprawdzania poprawności. W poniższym przykładzie z przykładowej aplikacji `CheckAge` Metoda określa, że `age` parametru musi być powiązana z ciągu zapytania po przesłaniu formularza:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

Na stronie Sprawdzanie wiekowa (*CheckAge.cshtml*), istnieją dwie formy. Pierwszy formularz przesyła `Age` wartość `99` jako ciąg zapytania: `https://localhost:5001/Users/CheckAge?Age=99`.

Gdy jest to poprawnie sformatowany `age` jest przesyłany przez parametr ciągu zapytania, sprawdza poprawność formularza.

Drugi formularz na stronie Sprawdzanie wiekowa przesyła `Age` wartością w treści żądania, a weryfikacja zakończy się niepowodzeniem. Powiązanie kończy się niepowodzeniem, ponieważ `age` parametru muszą pochodzić z ciągu zapytania.

Podczas korzystania z użyciem `CompatibilityVersion.Version_2_1` lub później, sprawdzanie poprawności węzeł najwyższego poziomu jest domyślnie włączona. W przeciwnym razie Weryfikacja węzeł najwyższego poziomu jest wyłączona. Opcja domyślna może zostać zastąpione przez ustawienie <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> właściwości (`Startup.ConfigureServices`), jak pokazano poniżej:

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a>Maksymalna liczba błędów

Sprawdzanie poprawności zostanie zatrzymane, jeśli osiągnięto maksymalną liczbę błędów (200 domyślnie). Tę liczbę można skonfigurować w następującym kodem `Startup.ConfigureServices`:

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a>Maksymalna rekursji

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> przechodzi przez wykres obiektu modelu, w trakcie sprawdzania poprawności. W przypadku modeli, które są bardzo szczegółowe lub nieskończenie cyklicznego sprawdzania poprawności może spowodować przepełnienie stosu. [MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) umożliwia powstrzymywanie weryfikacji wcześnie, jeśli rekursji odwiedzający przekracza skonfigurowaną głębokość. Wartość domyślna `MvcOptions.MaxValidationDepth` to 200, podczas korzystania z użyciem `CompatibilityVersion.Version_2_2` lub nowszej. W przypadku starszych wersji ma wartość null, co oznacza, że określono ograniczenia głębi.

## <a name="automatic-short-circuit"></a>Zwarcie automatyczne

Sprawdzanie poprawności jest automatycznie zwartym (pominięto) Jeśli na wykresie modelu nie wymaga weryfikacji. Obiekty, których środowisko uruchomieniowe Pomija weryfikację obejmują kolekcje elementów podstawowych (takie jak `byte[]`, `string[]`, `Dictionary<string, string>`) i wykresów złożonych obiektów, które nie mają żadnych modułów weryfikacji.

## <a name="disable-validation"></a>Wyłączyć sprawdzanie poprawności

Aby wyłączyć sprawdzanie poprawności:

1. Utworzenie implementacji klasy `IObjectModelValidator` , nie Oznacz wszystkie pola jako nieprawidłowy.

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. Dodaj następujący kod do `Startup.ConfigureServices` zastąpić domyślną `IObjectModelValidator` implementacji kontenera iniekcji zależności.

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

Nadal mogą pojawiać się błędy stanu modelu, które pochodzą z wiązania modelu.

## <a name="client-side-validation"></a>Weryfikacja po stronie klienta

Weryfikacji po stronie klienta zapobiega przesyłanie, aż formularz jest nieprawidłowy. Przycisk Zatwierdź uruchamia JavaScript, która przesyła formularz lub wyświetla komunikaty o błędach.

Weryfikacji po stronie klienta pozwala uniknąć niepotrzebnych komunikacji dwustronnej z serwerem, gdy istnieją błędy w danych wejściowych w formularzu. Poniższy skrypt, który odwołuje się w *_Layout.cshtml* i *_ValidationScriptsPartial.cshtml* obsługi weryfikacji po stronie klienta:

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

[JQuery sprawdzania poprawności dyskretnego kodu](https://github.com/aspnet/jquery-validation-unobtrusive) skrypt jest niestandardową biblioteką frontonu firmy Microsoft, która jest oparta na popularnej [jQuery weryfikacji](https://jqueryvalidation.org/) wtyczki. Bez sprawdzania poprawności dyskretnego kodu jQuery, trzeba by code tę samą logikę weryfikacji w dwóch miejscach: jeden raz w atrybutach weryfikacji po stronie serwera właściwości model, a następnie ponownie w skryptach po stronie klienta. Zamiast tego [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) należy użyć atrybutów sprawdzania poprawności i wpisać metadane z właściwości modelu do renderowania kodu HTML 5 `data-` atrybutów dla elementów formularza, które wymagają weryfikacji. analizuje sprawdzania poprawności dyskretnego kodu jQuery `data-` atrybutów, a następnie przekazuje logikę do technologii jQuery sprawdzania poprawności, efektywnie "kopiowanie" logiki weryfikacji po stronie serwera do klienta. Błędy sprawdzania poprawności można wyświetlać na kliencie, używanie pomocników tagów, jak pokazano poniżej:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

Poprzedni pomocnicy tagów, że poniższy kod HTML.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

Należy zauważyć, że `data-` atrybuty w kodzie HTML dane wyjściowe odnoszą się do atrybutów weryfikacji `ReleaseDate` właściwości. `data-val-required` Atrybut zawiera komunikat o błędzie wyświetlany w sytuacji, gdy użytkownik nie wypełnić pole daty wydania. jQuery sprawdzania poprawności dyskretnego kodu przekazuje tę wartość do weryfikacji jQuery [ `required()` ](https://jqueryvalidation.org/required-method/) metody, która wyświetla ten komunikat w towarzyszącego  **\<span >** elementu.

Sprawdzanie poprawności typu danych opiera się na typ architektury .NET własności, chyba że, który jest zastępowany przez `[DataType]` atrybutu. Przeglądarek ma swoje własne komunikaty o błędach domyślne, ale pakietu sprawdzania poprawności dyskretnego kodu weryfikacji jQuery można zastąpić te komunikaty. `[DataType]` atrybuty i podklas, takie jak `[EmailAddress]` umożliwiają określenie komunikat o błędzie.

### <a name="add-validation-to-dynamic-forms"></a>Dodawanie walidacji do formularzy dynamicznych

jQuery sprawdzania poprawności dyskretnego kodu przekazuje logikę weryfikacji i parametry do technologii jQuery sprawdzania poprawności, po pierwszym załadowaniu strony. W związku z tym sprawdzanie poprawności nie działa automatycznie na dynamicznie generowanym formularzy. Aby włączyć sprawdzanie poprawności, poinformuj jQuery dyskretny kod sprawdzania poprawności, aby przeanalizować dynamiczny formularz natychmiast, po jego utworzeniu. Na przykład poniższy kod konfiguruje weryfikacji po stronie klienta w formularzu dodane za pośrednictwem technologii AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

`$.validator.unobtrusive.parse()` Metoda przyjmuje selektor jQuery, jego jednego argumentu. Ta metoda informuje jQuery dyskretny kod sprawdzania poprawności, aby przeanalizować `data-` atrybuty odeśle selektora. Wartości te atrybuty są następnie przekazywane do wtyczki weryfikacji jQuery.

### <a name="add-validation-to-dynamic-controls"></a>Dodawanie walidacji do kontrolek dynamicznych

`$.validator.unobtrusive.parse()` Metoda działa na cały formularz, nie w poszczególnych formantów generowanych dynamicznie, takich jak `<input>` i `<select/>`. Aby ponownej analizy formularza, Usuń danych weryfikacji, który został dodany podczas formularz był analizowany wcześniej, jak pokazano w poniższym przykładzie:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a>Niestandardowe weryfikacji po stronie klienta

Niestandardowe sprawdzanie poprawności klienta jest wykonywane przez generowanie `data-` współpracujące z kartą weryfikacji jQuery niestandardowych atrybutów HTML. Następujący przykładowy kod adapter został napisany dla `ClassicMovie` i `ClassicMovie2` atrybuty, które zostały wprowadzone wcześniej w tym artykule:

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

Aby dowiedzieć się, jak napisać kart, zobacz [jQuery dokumentacji weryfikacji](http://jqueryvalidation.org/documentation/).

Użycie adaptera dla danego pola jest wyzwalany przez `data-` atrybuty, które:

* Flaga pole jako podlegające weryfikacji (`data-val="true"`).
* Identyfikowanie sprawdzania poprawności reguły nazwy i błąd tekst komunikatu (na przykład `data-val-rulename="Error message."`).
* Podaj wszelkie dodatkowe parametry modułu sprawdzania poprawności musi (na przykład `data-val-rulename-parm1="value"`).

W poniższym przykładzie przedstawiono `data-` atrybuty dla przykładowej aplikacji `ClassicMovie` atrybutu:

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

Jak wspomniano wcześniej, [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) używać informacji z weryfikacji atrybutów w celu renderowania `data-` atrybutów. Dostępne są dwie opcje dotyczące pisania kodu, które powoduje utworzenie niestandardowego `data-` atrybuty kodu HTML:

* Utwórz klasę, która pochodzi od klasy `AttributeAdapterBase<TAttribute>` i klasę, która implementuje `IValidationAttributeAdapterProvider`i zarejestrować swoje atrybutu i jego karty w DI. Ta metoda jest zgodna [jednostki pojedynczej odpowiedzialności](https://wikipedia.org/wiki/Single_responsibility_principle) , kod sprawdzania poprawności związanych z serwerami i związanych z klientem znajduje się w osobnych klas. Karta ma też tę zaletę, że ponieważ jest on zarejestrowany DI, inne usługi w ramach DI są dostępne do niego w razie potrzeby.
* Implementowanie `IClientModelValidator` w swojej `ValidationAttribute` klasy. Ta metoda może być odpowiednie, jeśli atrybut nie wykonuje żadnych weryfikacji po stronie serwera i nie wymaga żadnych usług z DI.

### <a name="attributeadapter-for-client-side-validation"></a>AttributeAdapter weryfikacji po stronie klienta

Ta metoda renderowania `data-` atrybutów w formacie HTML jest używany przez `ClassicMovie` atrybutu w przykładowej aplikacji. Aby dodać sprawdzanie poprawności klienta za pomocą tej metody:

1. Utwórz klasę atrybutów adapter dla atrybutu niestandardowego sprawdzania poprawności. Pochodną klasy z [AttributeAdapterBase\<T >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2). Tworzenie `AddValidation` metodę, która dodaje `data-` atrybuty do wyniku renderowania, jak pokazano w poniższym przykładzie:

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. Tworzenie klasy dostawcy karty, która implementuje <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>. W `GetAttributeAdapter` metoda przekazać w atrybucie niestandardowym do konstruktora karty, jak pokazano w poniższym przykładzie:

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. Zarejestruj dostawcę karty dla DI w `Startup.ConfigureServices`:

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a>IClientModelValidator weryfikacji po stronie klienta

Ta metoda renderowania `data-` atrybutów w formacie HTML jest używany przez `ClassicMovie2` atrybutu w przykładowej aplikacji. Aby dodać sprawdzanie poprawności klienta za pomocą tej metody:

* W atrybucie niestandardowego sprawdzania poprawności, należy zaimplementować `IClientModelValidator` interfejs i utworzyć `AddValidation` metody. W `AddValidation` metody, Dodaj `data-` atrybuty weryfikacji, jak pokazano w poniższym przykładzie:

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a>Wyłącz weryfikację po stronie klienta

Poniższy kod wyłącza sprawdzanie poprawności klienta w MVC widoki:

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

W przypadku stron Razor:

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

Inną opcją wyłączenia sprawdzanie poprawności klienta jest komentarz odwołanie do `_ValidationScriptsPartial` w swojej *.cshtml* pliku.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Przestrzeń nazw System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations)
* [Wiązanie modelu](model-binding.md)
