---
title: Weryfikacja modelu w programie ASP.NET MVC Core
author: rachelappel
description: Więcej informacji o weryfikacji modelu w programie ASP.NET MVC Core.
manager: wpickett
ms.author: riande
ms.date: 12/18/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/validation
ms.openlocfilehash: befbec393c089ec1f4dfdac5dbbdc3bdc7ddbf69
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="model-validation-in-aspnet-core-mvc"></a>Weryfikacja modelu w programie ASP.NET MVC Core

Przez [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Wprowadzenie do sprawdzania poprawności modelu

Zanim aplikacja przechowuje dane w bazie danych, aplikacja musi sprawdzić poprawności danych. Dane muszą zostać sprawdzone za możliwe zagrożenia bezpieczeństwa, sprawdzić, czy jest prawidłowo sformatowany według typu i rozmiar, a musi być zgodna z regułami. Sprawdzania poprawności jest niezbędne, chociaż może być zbędne i niewygodne wdrożenia. W nazwie wzorca MVC Weryfikacja odbywa się na kliencie i serwerze.

Na szczęście platformy .NET ma pobieranej weryfikacji do atrybutów sprawdzania poprawności. Te atrybuty zawierają kod sprawdzania poprawności, co zmniejsza ilość kodu, który musi być zapisana.

[Wyświetlanie lub pobieranie próbki z usługi GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Sprawdzanie poprawności atrybutów

Atrybuty weryfikacji służą do konfigurowania weryfikacji modelu, jest on podobny koncepcyjnie do sprawdzania poprawności dla pól w tabelach bazy danych. W tym ograniczenia, takie jak przypisywanie typów danych lub wymagane pola. Inne rodzaje weryfikacji obejmują stosowania wzorców do danych, aby wymusić reguł biznesowych, takich jak karty kredytowej lub numer telefonu lub adres e-mail. Atrybutów sprawdzania poprawności należy wymuszania tych wymagań, znacznie prostsze i łatwiejsze do użycia.

Poniżej znajduje się adnotacjami `Movie` modelu z aplikacji, która przechowuje informacje dotyczące filmy i programy telewizyjne. Większość właściwości są wymagane i kilka właściwości ciągu mają wymagania dotyczące długości. Ponadto istnieje ograniczenie zakresu numerycznego w przypadku `Price` właściwości z zakresu od 0 do $999,99, wraz z atrybutu niestandardowego sprawdzania poprawności.

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

Po prostu odczytywania za pośrednictwem modelu ujawnia zasad dotyczących danych dla tej aplikacji, co ułatwia utrzymanie kodu. Poniżej przedstawiono kilka atrybutów popularnych wbudowanych sprawdzania poprawności:

* `[CreditCard]`: Weryfikuje właściwość ma format karty kredytowej.

* `[Compare]`: Sprawdza dwie właściwości w modelu dopasowania.

* `[EmailAddress]`: Weryfikuje właściwość ma format wiadomości e-mail.

* `[Phone]`: Weryfikuje właściwość ma format telefonu.

* `[Range]`: Weryfikuje wypada wartości właściwości w danym zakresie.

* `[RegularExpression]`: Sprawdza, czy dane odpowiada określonemu wyrażeniu regularnemu.

* `[Required]`: Powoduje, że właściwość wymagane.

* `[StringLength]`: Sprawdza właściwości ciągu ma co najwyżej podanej długości maksymalnej.

* `[Url]`: Weryfikuje właściwość ma format adresu URL.

Wszelkie atrybuty, która jest pochodną obsługuje MVC `ValidationAttribute` do celów weryfikacji. Wiele atrybutów sprawdzania poprawności przydatne znajdują się w [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw.

Może być konieczne więcej funkcji niż wbudowane atrybuty wystąpień. W takich sytuacjach można utworzyć niestandardowego sprawdzania poprawności atrybutów wynikających z `ValidationAttribute` lub zmiana modelu do zaimplementowania `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Uwagi dotyczące stosowania wymaganego atrybutu

Niedopuszczająca wartości null [typów wartości](/dotnet/csharp/language-reference/keywords/value-types) (takich jak `decimal`, `int`, `float`, i `DateTime`) są z założenia wymagane i nie wymagają `Required` atrybutu. Aplikacja projekcie nie są sprawdzane weryfikacji po stronie serwera dla typów wartości null, które są oznaczone jako `Required`.

Wiązanie modelu MVC, który nie jest związana z weryfikacji i atrybutów sprawdzania poprawności, odrzuca przesłanie pole formularza zawierających brakujące wartości lub odstępem dla typu wartości null. W przypadku braku `BindRequired` atrybutu na właściwość target wiązania modelu ignoruje brakujące dane dla typów wartości null, w którym nie ma pola formularza z przychodzących danych formularza.

[Atrybutu BindRequired](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (Zobacz też [dostosować zachowanie wiązania modelu z atrybutami](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) przydaje się do zapewnienia zakończeniu danych formularza. Gdy jest stosowany do właściwości, system powiązanie modelu wymaga wartości tej właściwości. Po zastosowaniu do typu systemu powiązanie modelu wymaga wartości dla wszystkich właściwości tego typu.

Jeśli używasz [Nullable\<T > typ](/dotnet/csharp/programming-guide/nullable-types/) (na przykład `decimal?` lub `System.Nullable<decimal>`) i oznacz ją `Required`, sprawdzanie poprawności po stronie serwera odbywa się tak, jakby właściwość był Standardowy typ dopuszczający wartość null (dla przykład `string`).

Sprawdzanie poprawności klienta wymaga wartości dla pola formularza, które odpowiada właściwości modelu, który został oznaczony `Required` i właściwości Typ niedopuszczający wartości null, które nie zostały oznaczone jako `Required`. `Required` może służyć do kontrolowania komunikatów o błędach weryfikacji po stronie klienta.

## <a name="model-state"></a>Stan modelu

Stan modelu reprezentuje błędy sprawdzania poprawności w przesłanych wartości formularza HTML.

MVC będzie sprawdzanie poprawności pól, dopóki nie osiągnie maksymalną liczbę błędów (200 domyślnie). Numer ten można skonfigurować przez wstawienie poniższego kodu do `ConfigureServices` metody w *Startup.cs* pliku:

[!code-csharp[](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Stan modelu obsługi błędów

Weryfikacja modelu występuje przed każdego wywoływana Akcja kontrolera i odpowiada metoda akcji sprawdzić `ModelState.IsValid` i odpowiednio zareagować. W wielu przypadkach odpowiednie reakcji jest zwracany w odpowiedzi na błąd, najlepiej opisujący szczegółowo przyczynę niepowodzenia weryfikacji modelu.

Niektóre aplikacje wybierze wykonać standardowej konwencji zajmujących się błędy sprawdzania poprawności modelu, w których przypadku filtru może być odpowiednie miejsce do wdrożenia tych zasad. Należy przetestować zachowanie akcji z stanów modelu prawidłowe oraz nieprawidłowe.

## <a name="manual-validation"></a>Weryfikowanie ręczne

Po zakończeniu wiązania modelu i sprawdzania poprawności można powtórzyć jej części. Na przykład użytkownik może zostać wprowadzony tekst w polu Oczekiwano liczby całkowitej lub może być konieczne do obliczenia wartości dla właściwości modelu.

Może być konieczne ręczne uruchomienie walidacji. Aby to zrobić, należy wywołać `TryValidateModel` metody, jak pokazano poniżej:

[!code-csharp[](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Walidacji niestandardowej

Atrybuty weryfikacji działają w wielu zastosowaniach sprawdzania poprawności. Jednak niektóre reguły sprawdzania poprawności są specyficzne dla firmy. Reguły może nie być typowe techniki sprawdzania poprawności danych, takich jak zapewnienie pole jest wymagane lub że spełnia on zakresu wartości. W tych sytuacjach niestandardowego sprawdzania poprawności atrybutów są doskonałe rozwiązanie. Tworzenie własnego niestandardowego sprawdzania poprawności atrybutów w MVC jest bardzo proste. Tylko dziedziczyć `ValidationAttribute`i Zastąp `IsValid` metody. `IsValid` Metoda przyjmuje dwa parametry pierwszy jest obiekt o nazwie *wartość* a drugim `ValidationContext` obiektu o nazwie *validationContext*. *Wartość* odwołuje się do wartości rzeczywistej z pola niestandardowego modułu sprawdzania poprawności przeprowadza walidację.

W poniższym przykładzie reguła biznesowa określają, czy użytkownicy mogą nie ustawiono genre *klasycznego* filmu wydaną po 1960. `[ClassicMovie]` Atrybut najpierw sprawdza genre, a jeśli klasyczny, następnie sprawdza Data wydania jest późniejsza niż 1960. Jeśli po uwolnieniu po 1960, uwierzytelnienie nie powiedzie się. Atrybut akceptuje parametr całkowitą reprezentującą rok, który służy do sprawdzania poprawności danych. Wartość parametru w Konstruktorze ten atrybut można przechwycić w sposób pokazany poniżej:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

`movie` Zmiennej powyżej reprezentuje `Movie` obiekt, który zawiera dane z przesyłania formularza do zweryfikowania. W takim przypadku kodu walidacji sprawdza datę i genre w `IsValid` metody `ClassicMovieAttribute` klasy zgodnie z zasadami. Po pomyślnym zweryfikowaniem `IsValid` zwraca `ValidationResult.Success` kodu, i w przypadku niepowodzenia weryfikacji, `ValidationResult` z komunikatem o błędzie. Gdy użytkownik modyfikuje `Genre` pól i przesyła formularz, `IsValid` metody `ClassicMovieAttribute` Sprawdź, czy jest klasyczny. Podobnie jak wszelkie atrybuty, wbudowane, zastosuj `ClassicMovieAttribute` do właściwości, takie jak `ReleaseDate` aby upewnić się, występuje sprawdzania poprawności, jak pokazano w poprzednim przykładzie kodu. Ponieważ przykładzie działa tylko w przypadku `Movie` typów, lepszym rozwiązaniem jest użycie `IValidatableObject` jak pokazano w poniższych akapitu.

Alternatywnie można umieścić ten sam kod w modelu zaimplementowanie `Validate` metoda `IValidatableObject` interfejsu. Podczas pracy niestandardowego sprawdzania poprawności atrybutów do sprawdzania poprawności poszczególnych właściwości, implementacja `IValidatableObject` może służyć do zaimplementowania weryfikacji na poziomie klasy, jak pokazano poniżej.

[!code-csharp[](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Sprawdzanie poprawności po stronie klienta

Sprawdzanie poprawności po stronie klienta jest doskonałym ułatwienia dla użytkowników. Zaoszczędzić czas, które w przeciwnym razie poświęcić oczekiwanie na obiegu do serwera. W terminologii biznesowej nawet kilka części sekundy pomnożone setki razy każdego dnia są dodaje do można dużo czasu, wydatków i frustracji spowodowanej. Proste i bezpośrednie weryfikacji umożliwia użytkownikom wydajniejszą pracę i tworzy lepszą jakość danych wejściowych i wyjściowych.

Musi mieć widoku z właściwego odwołań do skryptów JavaScript w celu weryfikacji po stronie klienta będzie działać zgodnie z widoczną w tym miejscu.

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[JQuery sprawdzania poprawności dyskretnego kodu](https://github.com/aspnet/jquery-validation-unobtrusive) skryptów jest niestandardowe frontonu biblioteki Microsoft, która opiera się na popularnych [jQuery weryfikacji](https://jqueryvalidation.org/) wtyczki. Bez sprawdzania poprawności dyskretnego kodu jQuery, konieczne będzie przeprowadzenie code tej samej logiki sprawdzania poprawności w dwóch miejscach: raz w atrybuty weryfikacji po stronie serwera dla właściwości modelu, a następnie ponownie w skrypty po stronie klienta (przykłady jquery weryfikacji [ `validate()` ](https://jqueryvalidation.org/validate/) metody pokazuje, jak złożoność to może stać się). Zamiast tego MVC [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) można używać atrybutów sprawdzania poprawności i wpisz metadanych właściwości modelu do renderowania kodu HTML 5 [atrybuty danych](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) w elementy formularza wymagające weryfikacji. Generuje MVC `data-` atrybutów dla atrybutów zarówno wbudowane i niestandardowe. jQuery sprawdzania poprawności dyskretnego kodu następnie analizuje te `data-` atrybutów i przekazuje logikę do jQuery weryfikacji skutecznie "kopiowanie" logikę weryfikacji po stronie serwera do klienta. Błędy sprawdzania poprawności można wyświetlać na kliencie przy użyciu pomocników tagów istotne, jak pokazano poniżej:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Powyżej pomocników tagów renderowania elementów HTML poniżej. Zwróć uwagę, że `data-` atrybutów w kodzie HTML output odpowiadają atrybutów weryfikacji `ReleaseDate` właściwości. `data-val-required` Atrybut poniżej zawiera komunikat o błędzie wyświetlany, jeśli użytkownik nie wypełnić pole daty wydania. jQuery sprawdzania poprawności dyskretnego kodu przekazuje tę wartość do weryfikacji jQuery [ `required()` ](https://jqueryvalidation.org/required-method/) metodę, która następnie wyświetla ten komunikat w odpowiednim  **\<span >** elementu.

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
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

Weryfikacji po stronie klienta uniemożliwia przesyłanie, dopóki formularza jest nieprawidłowy. Przycisk Prześlij uruchamia JavaScript, która wyśle formularz lub wyświetlane komunikaty o błędach.

MVC określa na podstawie typu danych .NET właściwości, prawdopodobnie przesłonić przy użyciu wartości atrybutu typu `[DataType]` atrybutów. Podstawowym `[DataType]` atrybut zapewnia Weryfikacja nie rzeczywistym po stronie serwera. Przeglądarki wybrać własne komunikaty o błędach i wyświetlić te błędy, jak życzą, jednak pakiet sprawdzania poprawności dyskretnego kodu jQuery można zastąpić wiadomości i ich konsekwentnie wyświetlić z innymi osobami. Dzieje się tak najczęściej oczywiście, gdy użytkownicy zastosują `[DataType]` podklasy, takich jak `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Dodawanie walidacji do dynamicznego formularzy

Ponieważ jQuery sprawdzania poprawności dyskretnego kodu przekazuje parametry i logikę weryfikacji do weryfikacji jQuery, po pierwszym załadowaniu strony, formularze dynamicznie generowanym automatycznie nie będą działać sprawdzania poprawności. Zamiast tego należy wskazać jQuery dyskretnego kodu sprawdzania poprawności można przeanalizować dynamiczny formularz natychmiast po jej utworzeniu. Na przykład poniższy kod przedstawia, jak można skonfigurować weryfikacji po stronie klienta na formularzu dodane za pośrednictwem interfejsu AJAX.

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

`$.validator.unobtrusive.parse()` Metoda przyjmuje selektora jQuery dla jednego argumentu. Ta metoda określa, że jQuery dyskretnego kodu sprawdzania poprawności można przeanalizować `data-` atrybuty formularzy w selektora. Wartości tych atrybutów są następnie przekazywane do wtyczki weryfikacji jQuery tak, aby formularz wykazuje reguł weryfikacji po stronie klienta żądany.

### <a name="add-validation-to-dynamic-controls"></a>Dodawanie walidacji do formantów dynamicznych

Można także zaktualizować reguły walidacji na formularzu, gdy osoba formanty, takie jak `<input/>`s i `<select/>`s, są generowane dynamicznie. Nie można przekazać selektory dla tych elementów do `parse()` metody bezpośrednio ponieważ otaczające formularz już został przeanalizowany i nie będzie aktualizować. Zamiast tego należy najpierw usunąć istniejące dane sprawdzania poprawności, a następnie ponownej analizy całego formularza, jak pokazano poniżej:

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

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Możesz utworzyć logiki po stronie klienta dla użytkownika niestandardowego atrybutu i [sprawdzania poprawności dyskretnego kodu](http://jqueryvalidation.org/documentation/) będą wykonywane na kliencie zostanie automatycznie w ramach sprawdzania poprawności. Pierwszym krokiem jest kontrolować, jakie atrybuty danych są dodawane zaimplementowanie `IClientModelValidator` interfejsu, jak pokazano poniżej:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Atrybuty, które implementują ten interfejs można dodawać atrybuty HTML do wygenerowanego pola. Badanie danych wyjściowych dla `ReleaseDate` element ujawnia HTML, która jest podobna do poprzedniego przykładu, lecz teraz jest `data-val-classicmovie` atrybut, który został zdefiniowany w `AddValidation` metody `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Sprawdzania poprawności dyskretnego kodu używa danych w `data-` atrybutów, aby wyświetlić komunikaty o błędach. Jednak nie ma informacji dotyczących reguły jQuery lub wiadomości do momentu dodania do tego jQuery `validator` obiektu. Przedstawiono to w poniższym przykładzie, który dodaje metodę o nazwie `classicmovie` niestandardowego klienta kodem weryfikacji jQuery `validator` obiektu.

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

JQuery zawiera teraz informacje do wykonywania niestandardowego sprawdzania poprawności JavaScript, a także komunikat o błędzie wyświetlany w przypadku tego kodu walidacji zwraca wartość false.

## <a name="remote-validation"></a>Walidacja zdalnego

Walidacja zdalnego jest funkcją doskonałe do użycia, gdy trzeba sprawdzić poprawności danych na kliencie w odniesieniu do danych na serwerze. Na przykład aplikacja może być konieczne Zweryfikuj, czy nazwa użytkownika lub adres e-mail jest już używana, i należy zbadać dużej ilości danych, aby to zrobić. Pobieranie dużych zestawów danych sprawdzania poprawności jedną lub kilka pól zużywa zbyt wiele zasobów. Może również spowodować ujawnienie poufnych informacji. Alternatywą jest obustronne żądania, aby sprawdzić poprawność pola.

W procesie dwóch krok można zaimplementować weryfikację zdalną. Najpierw należy dodawać adnotacje modelu za pomocą `[Remote]` atrybutu. `[Remote]` Atrybut akceptuje wielu przeciążeń umożliwia bezpośrednie JavaScript po stronie klienta do odpowiedniego kodu do wywołania. W poniższym przykładzie wskazuje `VerifyEmail` metody akcji `Users` kontrolera.

[!code-csharp[](validation/sample/User.cs?range=7-8)]

Drugim krokiem jest wprowadzenie kodu walidacji w odpowiedniej metody akcji, zgodnie z definicją w `[Remote]` atrybutu. Zgodnie z jQuery weryfikacji [ `remote()` ](https://jqueryvalidation.org/remote-method/) dokumentacji metody:

> Odpowiedź serverside musi być ciągiem formatu JSON, który musi być `"true"` elementów prawidłową i może być `"false"`, `undefined`, lub `null` dla nieprawidłowe elementy przy użyciu domyślnego komunikatu o błędzie. Jeśli odpowiedź serverside jest ciąg znaków, np. `"That name is already taken, try peter123 instead"`, ciąg ten będzie wyświetlany jako niestandardowy komunikat o błędzie zamiast domyślnego.

Definicja `VerifyEmail()` metody obowiązują następujące reguły, jak pokazano poniżej. Zwraca błąd sprawdzania poprawności komunikatu podjęcia wiadomości e-mail lub `true` Jeśli wiadomość e-mail jest bezpłatna i opakowuje wynik w `JsonResult` obiektu. Następnie po stronie klienta można użyć zwracanej wartości, aby kontynuować, lub wyświetlić błąd, jeśli to konieczne.

[!code-csharp[](validation/sample/UsersController.cs?range=19-28)]

Podczas wprowadzania wiadomości e-mail, JavaScript, w widoku umożliwia teraz zdalne wywołanie czy tej wiadomości e-mail zostały podjęte, a jeśli tak, wyświetla komunikat o błędzie. W przeciwnym razie użytkownik można przesłać formularza, w zwykły sposób.

`AdditionalFields` Właściwość `[Remote]` atrybutu przydaje się do sprawdzania poprawności kombinacji pól w odniesieniu do danych na serwerze. Na przykład jeśli `User` modelu z powyższych ma dwie dodatkowe właściwości o nazwie `FirstName` i `LastName`, należy sprawdzić, czy nie istniejący użytkownicy mają już tej pary nazw. Możesz zdefiniować nowe właściwości, jak pokazano w poniższym kodzie:

[!code-csharp[](validation/sample/User.cs?range=10-13)]

`AdditionalFields` można ustawiono jawnie ciągi `"FirstName"` i `"LastName"`, ale przy użyciu [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) upraszcza operator podobny do tego później refaktoryzacji. Metoda akcji do wykonania walidacji następnie zaakceptować dwa argumenty, jeden dla wartości `FirstName` i jeden dla wartości `LastName`.

[!code-csharp[](validation/sample/UsersController.cs?range=30-39)]

Teraz po użytkowników Wprowadź imię i nazwisko, JavaScript:

* Umożliwia zdalne wywołanie, aby zobaczyć, czy podjęto tej pary nazw.
* Jeśli podjęto pary, zostanie wyświetlony komunikat o błędzie. 
* Jeśli nie zostanie podjęta, użytkownik może przesłać formularza.

Jeśli musisz sprawdzić poprawności co najmniej dwa dodatkowe pola z `[Remote]` atrybutu, można podać je jako listę rozdzielaną przecinkami. Na przykład, aby dodać `MiddleName` ustawić właściwości w modelu `[Remote]` atrybutu, jak pokazano w poniższym kodzie:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, takich jak wszystkich argumentów atrybutu musi być wyrażeniem stałym. W związku z tym nie można używać [interpolowane ciąg](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) lub zadzwoń [ `string.Join()` ](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) zainicjować `AdditionalFields`. Dla każdego pola dodatkowe, które można dodać do `[Remote]` atrybutu, należy dodać inny argument do odpowiedniej metody akcji kontrolera.
