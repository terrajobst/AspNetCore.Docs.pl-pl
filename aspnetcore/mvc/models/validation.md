---
title: Weryfikacja modelu w programie ASP.NET Core MVC
author: tdykstra
description: Więcej informacji o weryfikacji modelu w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 12/18/2016
uid: mvc/models/validation
ms.openlocfilehash: 9c2ba1c1fad3ac077a886b3465142acfd4d639af
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095830"
---
# <a name="model-validation-in-aspnet-core-mvc"></a>Weryfikacja modelu w programie ASP.NET Core MVC

Przez [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Wprowadzenie do sprawdzania poprawności modelu

Zanim aplikacja przechowuje dane w bazie danych, aplikacja musi sprawdzić poprawność danych. Potencjalne zagrożenia bezpieczeństwa w celu weryfikacji, że jest prawidłowo sformatowany według typu i rozmiaru i musi stosować się do reguł, dane muszą zostać sprawdzone. Sprawdzanie poprawności jest niezbędne, chociaż może być nadmiarowy i żmudnym do zaimplementowania. W przypadku platformy MVC weryfikacji odbywa się na kliencie i serwerze.

Na szczęście platformy .NET ma wyodrębnione sprawdzania poprawności do atrybutów sprawdzania poprawności. Te atrybuty zawierają kod sprawdzania poprawności, w tym samym zmniejsza ilość kodu, który trzeba napisać.

[Wyświetlanie lub pobieranie przykładowy z serwisu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Sprawdzanie poprawności atrybutów

Atrybuty weryfikacji służą do konfigurowania weryfikacji modelu, więc podobne pod względem koncepcyjnym do sprawdzania poprawności w polach w tabelach bazy danych. Obejmuje to ograniczenia, takie jak przypisywanie typów danych lub wymagane pola. Inne rodzaje weryfikacji obejmują stosowania wzorców do danych w celu wymuszania reguł biznesowych, takich jak karty kredytowej lub numer telefonu lub adres e-mail. Atrybuty weryfikacji upewnij się, wymuszając te wymagania, znacznie prostsze i łatwiejsze w użyciu.

Poniżej znajduje się adnotacjami `Movie` modelu w aplikacji, która przechowuje informacje dotyczące filmów i programów telewizyjnych. Większość właściwości są wymagane, i kilka właściwości ciągu mają wymagania dotyczące długości. Ponadto ma ograniczenie zakresu liczbowego w miejscu, aby `Price` właściwości z zakresu od 0 do $999,99, wraz z atrybutu niestandardowego sprawdzania poprawności.

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

Po prostu odczytywanie za pomocą modelu, co spowoduje wyświetlenie zasad dotyczących danych dla tej aplikacji, co ułatwia zachowania kodu. Poniżej przedstawiono kilka popularnych wbudowanych sprawdzania poprawności atrybutów:

* `[CreditCard]`: Weryfikuje właściwość ma format karty kredytowej.

* `[Compare]`: Sprawdza poprawność dwie właściwości w modelu są zgodne.

* `[EmailAddress]`: Weryfikuje właściwość ma format adresu e-mail.

* `[Phone]`: Weryfikuje właściwość ma format telefonu.

* `[Range]`: Sprawdzanie poprawności właściwości wartość znajduje się w danym zakresie.

* `[RegularExpression]`: Sprawdza, czy dane odpowiada określonemu wyrażeniu regularnemu.

* `[Required]`: Sprawia, że wymagane właściwości.

* `[StringLength]`: Sprawdza, czy właściwość ciągu ma co najwyżej podanej długości maksymalnej.

* `[Url]`: Weryfikuje właściwość ma format adresu URL.

MVC obsługuje dowolnego atrybutu, która pochodzi od klasy `ValidationAttribute` do celów sprawdzania poprawności. Wiele atrybutów sprawdzania poprawności przydatne znajdują się w [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw.

Mogą wystąpić sytuacje, gdy potrzebujesz więcej funkcji niż wbudowanych atrybutów. W takich sytuacjach można utworzyć niestandardowego sprawdzania poprawności atrybutów pochodząca od `ValidationAttribute` lub zmiana modelu do zaimplementowania `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Uwagi dotyczące użytkowania wymaganego atrybutu

Nieprzyjmujące [typy wartości](/dotnet/csharp/language-reference/keywords/value-types) (takie jak `decimal`, `int`, `float`, i `DateTime`) są założenia wymagane i nie ma potrzeby `Required` atrybutu. Aplikacja wykonuje żadnych testów weryfikacji po stronie serwera dla typów innych niż null, które są oznaczone `Required`.

Wiązanie modelu MVC, która nie jest rozpatrywany wraz z weryfikacji i atrybutów sprawdzania poprawności, odrzuca przesyłania pola formularza, zawierający brakujące wartości lub białe znaki dla typów innych niż null. W przypadku braku `BindRequired` atrybutu na właściwość target wiązania modelu ignoruje Brak danych dla typów innych niż null, gdy formularz nie ma pola ukrytego przychodzące dane formularza.

[Atrybut BindRequired](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (Zobacz też [dostosować zachowanie wiązania modelu z atrybutami](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) jest przydatne do upewnij się formularz danych zostało zakończone. Gdy jest stosowany do właściwości, system powiązanie modelu wymaga podania wartości dla tej właściwości. Gdy jest stosowany do typu, system powiązanie modelu wymaga wartości wszystkich właściwości tego typu.

Kiedy używać [Nullable\<T > typu](/dotnet/csharp/programming-guide/nullable-types/) (na przykład `decimal?` lub `System.Nullable<decimal>`) i oznacz ją `Required`, sprawdzanie poprawności po stronie serwera odbywa się tak, jakby właściwości zostały standardowego typu dopuszczającego wartość null (na przykład `string`).

Weryfikacja po stronie klienta wymaga podania wartości dla pola formularza, który odnosi się do właściwości modelu, który został oznaczony `Required` i dla właściwości typu innego niż dopuszczającego wartość null, która nie została oznaczona jako `Required`. `Required` może służyć do kontrolowania komunikat o błędzie weryfikacji po stronie klienta.

## <a name="model-state"></a>Stan modelu

Stan modelu reprezentuje błędy sprawdzania poprawności w przesłanych wartości z formularza HTML.

MVC będą nadal sprawdzanie poprawności pól, dopóki nie osiągnie maksymalną liczbę błędów (200 domyślnie). Możesz skonfigurować tę liczbę, wstawiając następujący kod do `ConfigureServices` method in Class metoda *Startup.cs* pliku:

[!code-csharp[](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Stan modelu obsługi błędów

Sprawdzanie poprawności modelu występującą przed każdym wywoływana Akcja kontrolera i odpowiada metoda akcji sprawdzanie `ModelState.IsValid` i odpowiednio reagują. W wielu przypadkach odpowiednie reakcji ma zwracać odpowiedź o błędzie, najlepiej szczegółowych informacji na temat przyczyny niepowodzenia weryfikacji modelu.

Niektóre aplikacje wybierze wykonać standardowej Konwencji za zajmowanie się błędy sprawdzania poprawności modelu, w których filtr przypadku odpowiednim miejscu, aby zaimplementować takie zasady. Należy przetestować zachowaniem akcji ze Stanami prawidłowe oraz nieprawidłowe modelu.

## <a name="manual-validation"></a>Weryfikowanie ręczne

Po zakończeniu wiązania modelu i sprawdzanie poprawności można powtórzyć jej części. Na przykład użytkownik mógł zostać wprowadzony tekst w polu, oczekiwano typu integer lub może być konieczne do obliczenia wartości dla właściwości modelu.

Może być konieczne ręczne uruchomienie sprawdzania poprawności. Aby to zrobić, należy wywołać `TryValidateModel` metody, jak pokazano poniżej:

[!code-csharp[](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Niestandardowego sprawdzania poprawności

Atrybutów sprawdzania poprawności działa w wielu zastosowaniach sprawdzania poprawności. Jednak niektóre reguły sprawdzania poprawności są specyficzne dla Twojej firmy. Reguły mogą nie być typowe metody sprawdzania poprawności danych, takich jak zapewnienie pole jest wymagane, lub że spełnia on zakres wartości. Dla tych scenariuszy atrybuty niestandardowe sprawdzanie poprawności są doskonałe rozwiązanie. Tworzenie własnego niestandardowego sprawdzania poprawności atrybutów w MVC jest łatwe. Tylko dziedziczyć `ValidationAttribute`i zastępowania `IsValid` metody. `IsValid` Metoda przyjmuje dwa parametry pierwszy jest obiekt o nazwie *wartość* , a drugim `ValidationContext` obiektu o nazwie *validationContext*. *Wartość* odwołuje się do rzeczywistej wartości z pól, z niestandardowego modułu sprawdzania poprawności jest sprawdzania poprawności.

W następującym przykładzie reguła biznesowa stwierdzający, że użytkownicy mogą nie ustawiono tego gatunku *klasycznego* wydana po roku 1960 filmu. `[ClassicMovie]` Atrybut tego gatunku sprawdza, czy w przypadku klasycznej go następnie sprawdzi daty wydania, aby zobaczyć, że jest późniejsza niż 1960. Jest on zwalniany po roku 1960, sprawdzanie poprawności nie powiedzie się. Ten atrybut akceptuje parametr z liczbą całkowitą reprezentującą rok, który służy do sprawdzania poprawności danych. Wartość parametru w Konstruktorze ten atrybut można przechwycić, jak pokazano poniżej:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

`movie` Zmiennej powyżej reprezentuje `Movie` obiekt, który zawiera dane z przesyłania formularza do sprawdzania poprawności. W tym przypadku kod sprawdzania poprawności sprawdza, czy data i gatunku w `IsValid` metody `ClassicMovieAttribute` klasy zgodnie z zasadami. Po pomyślnej weryfikacji`IsValid` zwraca `ValidationResult.Success` kodu. Podczas sprawdzania poprawności zakończy się niepowodzeniem, `ValidationResult` z powodu błędu zwrócony komunikat:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=55-58)]

Gdy użytkownik modyfikuje `Genre` pola, a następnie przesyła formularz, `IsValid` metody `ClassicMovieAttribute` sprawdzi, czy jest klasycznej. Podobnie jak dowolny atrybut wbudowanych, należy zastosować `ClassicMovieAttribute` do właściwości, takie jak `ReleaseDate` aby zapewnić miejsce sprawdzania poprawności, jak pokazano w poprzednim przykładzie kodu. Ponieważ przykład działa tylko w przypadku `Movie` typów, lepszym rozwiązaniem jest użycie `IValidatableObject` jak pokazano w poniższym akapitu.

Alternatywnie można umieścić ten sam kod w modelu implementując `Validate` metody `IValidatableObject` interfejsu. Gdy niestandardowego sprawdzania poprawności atrybutów działa dobrze w przypadku sprawdzania poprawności poszczególnych właściwości, implementowanie `IValidatableObject` może służyć do implementowania weryfikacji na poziomie klasy, jak pokazano tutaj.

[!code-csharp[](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Weryfikacja po stronie klienta

Weryfikacja po stronie klienta jest doskonałym wygodę dla użytkowników. Zaoszczędzić czas, który one byłby przeznaczany na oczekiwanie na komunikację dwustronną z serwerem. W terminologii biznesowej nawet kilka użycie ułamkowych części sekundy pomnożone setki razy każdego dnia są dodaje do być dużo czasu i pieniędzy oraz Rozczarowanie. Proste i natychmiastowe sprawdzanie poprawności umożliwia użytkownikom bardziej wydajną pracę i wygenerować lepszą jakość danych wejściowych i wyjściowych.

Konieczne jest posiadanie widoku przy użyciu prawidłowego odwołania do skryptu JavaScript w miejscu na potrzeby weryfikacji po stronie klienta do pracy, jak widać w tym miejscu.

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[JQuery sprawdzania poprawności dyskretnego kodu](https://github.com/aspnet/jquery-validation-unobtrusive) skrypt jest niestandardową biblioteką frontonu firmy Microsoft, która jest oparta na popularnej [jQuery weryfikacji](https://jqueryvalidation.org/) wtyczki. Bez sprawdzania poprawności dyskretnego kodu jQuery, trzeba by code tę samą logikę weryfikacji w dwóch miejscach: jeden raz w atrybuty weryfikacji po stronie serwera we właściwościach modelu, a następnie ponownie w skryptach po stronie klienta (przykłady jQuery weryfikacji [ `validate()` ](https://jqueryvalidation.org/validate/) metoda ma pokazać, jak złożone to może stać się). Zamiast tego MVC [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) będą mogli używać atrybutów sprawdzania poprawności, a następnie wpisz metadane z właściwości modelu do renderowania kodu HTML 5 [atrybuty danych](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) w elementy formularza, wymagające weryfikacji. Generuje MVC `data-` atrybutów dla atrybutów niestandardowe i wbudowane. jQuery sprawdzania poprawności dyskretnego kodu następnie analizuje te `data-` atrybutów, a następnie przekazuje logikę do technologii jQuery sprawdzania poprawności, efektywnie "kopiowanie" logiki weryfikacji po stronie serwera do klienta. Błędy sprawdzania poprawności można wyświetlać na kliencie, używanie pomocników tagów istotne, jak pokazano poniżej:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Pomocnicy tagów powyżej renderować HTML poniżej. Należy zauważyć, że `data-` atrybuty w kodzie HTML dane wyjściowe odnoszą się do atrybutów weryfikacji `ReleaseDate` właściwości. `data-val-required` Poniższego atrybutu zawiera komunikat o błędzie wyświetlany w sytuacji, gdy użytkownik nie wypełnić pole daty wydania. jQuery sprawdzania poprawności dyskretnego kodu przekazuje tę wartość do weryfikacji jQuery [ `required()` ](https://jqueryvalidation.org/required-method/) metody, która wyświetla ten komunikat w towarzyszącego  **\<span >** elementu.

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

Weryfikacji po stronie klienta zapobiega przesyłanie, aż formularz jest nieprawidłowy. Przycisk Zatwierdź uruchamia JavaScript, która przesyła formularz lub wyświetla komunikaty o błędach.

MVC określa na podstawie typu danych .NET właściwości, prawdopodobnie przesłonić przy użyciu wartości atrybutu typu `[DataType]` atrybutów. Podstawa `[DataType]` atrybut zapewnia nie nastąpi sprawdzanie poprawności liczby rzeczywistej po stronie serwera. Przeglądarki wybierz ich własnych komunikatów o błędach i wyświetlania tych błędów, jak mają być, jednak pakietu sprawdzania poprawności dyskretnego kodu jQuery można zastąpić komunikaty i wyświetlać je stale innym osobom. Dzieje się to najbardziej oczywiste, gdy użytkownicy zastosują `[DataType]` podklasy, takie jak `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Dodawanie walidacji do formularzy dynamicznych

Ponieważ jQuery sprawdzania poprawności dyskretnego kodu przekazuje logikę weryfikacji i parametry do technologii jQuery sprawdzania poprawności, gdy strona ładuje się najpierw, dynamicznie generowanych formularzy nie będzie automatycznie następującej liczby etapów stwierdzono sprawdzania poprawności. Zamiast tego musisz poinformować jQuery dyskretny kod sprawdzania poprawności, aby przeanalizować dynamiczny formularz bezpośrednio po jej utworzeniu. Na przykład poniższy kod pokazuje, jak można skonfigurować weryfikacji po stronie klienta w formularzu dodane za pośrednictwem technologii AJAX.

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

`$.validator.unobtrusive.parse()` Metoda przyjmuje selektor jQuery, jego jednego argumentu. Ta metoda informuje jQuery dyskretny kod sprawdzania poprawności, aby przeanalizować `data-` atrybuty odeśle selektora. Wartości tych atrybutów są następnie przekazywane do wtyczki weryfikacji jQuery, tak, aby formularz wykazuje reguł weryfikacji po stronie klienta żądaną.

### <a name="add-validation-to-dynamic-controls"></a>Dodawanie walidacji do kontrolek dynamicznych

Możesz także zaktualizować reguły sprawdzania poprawności w formularzu, gdy osoba pod kontrolą, takich jak `<input/>`s i `<select/>`s, są generowane dynamicznie. Nie można przekazać selektory te elementy, aby `parse()` bezpośrednio metody ponieważ otaczającego formularz już został przeanalizowany i nie będzie aktualizować. Zamiast tego należy najpierw usunąć istniejące dane sprawdzania poprawności, a następnie ponownej analizy całego formularza, jak pokazano poniżej:

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

Możesz utworzyć logikę po stronie klienta dla Twojego niestandardowego atrybutu i [sprawdzania poprawności dyskretnego kodu](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) tworzy adapter w celu [dotyczącą weryfikacji jquery](http://jqueryvalidation.org/documentation/) będą wykonywane na kliencie dla Ciebie automatycznie jako część Sprawdzanie poprawności. Pierwszym krokiem jest kontrolować, jakie atrybuty danych są dodawane przez zaimplementowanie `IClientModelValidator` interfejsu, jak pokazano poniżej:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Atrybuty, które implementują ten interfejs, można dodać atrybuty HTML do wygenerowanego pola. Badanie danych wyjściowych dla `ReleaseDate` element, co spowoduje wyświetlenie kodu HTML, który jest podobny do poprzedniego przykładu, ale teraz ma `data-val-classicmovie` atrybut, który został zdefiniowany w `AddValidation` metody `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Sprawdzania poprawności dyskretnego kodu używa tych danych w `data-` atrybutów, które mają być wyświetlane komunikaty o błędach. Jednak jQuery nie wie o regułach lub wiadomości do momentu dodania do technologii jQuery firmy `validator` obiektu. Jest to pokazane w poniższym przykładzie, który dodaje metodę o nazwie `classicmovie` zawierający Klient niestandardowy kod walidacji do jQuery `validator` obiektu. Znajdują się objaśnienia dotyczące metody unobtrusive.adapters.add [tutaj](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

Teraz jQuery ma informacje o do wykonywania niestandardowego sprawdzania poprawności języka JavaScript, a także komunikat o błędzie wyświetlany, jeśli kod sprawdzania poprawności zwróci wartość false.

## <a name="remote-validation"></a>Zdalnej weryfikacji

Zdalnej weryfikacji to doskonałe funkcja do użycia, gdy trzeba wykonać walidację danych na komputerze klienckim względem danych na serwerze. Na przykład aplikacja może być konieczne Sprawdź, czy nazwa użytkownika lub adres e-mail jest już używana, a musi wykonać zapytanie dużej ilości danych, aby to zrobić. Pobieranie dużych zestawów danych sprawdzania poprawności jeden lub kilka pól zużywa zbyt dużo zasobów. Może również spowodować ujawnienie poufnych informacji. Alternatywą jest żądanie Rundy, aby sprawdzić poprawność pola.

W procesie dwóch kroków można zaimplementować weryfikację zdalną. Najpierw należy dodać adnotacje modelu za pomocą `[Remote]` atrybutu. `[Remote]` Atrybut akceptuje wiele przeciążeń, które można użyć do kierowania JavaScript po stronie klienta do odpowiedniego kodu do wywołania. W poniższym przykładzie wskazuje `VerifyEmail` metody akcji `Users` kontrolera.

[!code-csharp[](validation/sample/User.cs?range=7-8)]

Drugim krokiem jest umieszczenie kodu sprawdzania poprawności w odpowiedniej metody akcji, zgodnie z definicją w `[Remote]` atrybutu. Zgodnie z jQuery weryfikacji [ `remote()` ](https://jqueryvalidation.org/remote-method/) dokumentacji metody:

> Odpowiedź serverside musi być ciąg JSON, który musi być `"true"` prawidłowe elementy i może być `"false"`, `undefined`, lub `null` dla nieprawidłowe elementy przy użyciu domyślnego komunikatu o błędzie. Jeśli odpowiedź serverside jest ciągiem, np. `"That name is already taken, try peter123 instead"`, te parametry, które będą wyświetlane jako niestandardowy komunikat o błędzie zamiast domyślnego.

Definicja `VerifyEmail()` metoda obowiązują następujące reguły, jak pokazano poniżej. Zwraca błąd sprawdzania poprawności komunikat, jeśli jest pobierana w wiadomości e-mail lub `true` Jeśli wiadomość e-mail jest bezpłatny i otacza wynik w `JsonResult` obiektu. Następnie po stronie klienta można użyć zwracanej wartości, aby kontynuować, lub wyświetla błąd, jeśli to konieczne.

[!code-csharp[](validation/sample/UsersController.cs?range=19-28)]

Teraz gdy użytkownicy wprowadzają wiadomość e-mail, JavaScript, w widoku wywołuje zdalnego czy tego adresu e-mail jest już zajęta, a jeśli tak, wyświetla komunikat o błędzie. W przeciwnym razie użytkownik może przesłać formularza, w zwykły sposób.

`AdditionalFields` Właściwość `[Remote]` atrybut jest przydatna do zweryfikowania kombinacje pól w odniesieniu do danych na serwerze. Na przykład jeśli `User` modelu z powyższych ma dwa dodatkowe właściwości o nazwie `FirstName` i `LastName`, warto sprawdzić, czy nie istniejący użytkownicy mają już tej pary nazw. Możesz zdefiniować nowe właściwości, jak pokazano w poniższym kodzie:

[!code-csharp[](validation/sample/User.cs?range=10-13)]

`AdditionalFields` może ustawiono jawnie ciągi `"FirstName"` i `"LastName"`, ale przy użyciu [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operator następująco upraszcza później refaktoryzacji. Metody akcji, aby wykonać sprawdzanie poprawności następnie musi zaakceptować dwa argumenty, jeden dla wartości `FirstName` i jeden dla wartości `LastName`.

[!code-csharp[](validation/sample/UsersController.cs?range=30-39)]

Teraz gdy użytkownicy wprowadzają imię i nazwisko JavaScript:

* Umożliwia zdalne wywołanie, aby zobaczyć, że pary nazw jest już zajęta.
* Jeśli zostanie podjęta pary, jest wyświetlany komunikat o błędzie. 
* Jeśli nie zostanie podjęte, użytkownik może przesłać formularza.

Jeśli musisz zweryfikować co najmniej dwa dodatkowe pola z `[Remote]` atrybutu one podawane jako listę rozdzielonych przecinkami. Na przykład, aby dodać `MiddleName` właściwością modelu, `[Remote]` atrybutu, jak pokazano w poniższym kodzie:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, podobnie jak wszystkie argumenty atrybutu, musi być wyrażeniem stałym. W związku z tym, nie można używać [ciągiem interpolowanym](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) lub zadzwoń [ `string.Join()` ](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) zainicjować `AdditionalFields`. Dla każdego dodatkowego pola, które możesz dodać do `[Remote]` atrybutu, należy dodać kolejny argument do odpowiedniej metody akcji kontrolera.
