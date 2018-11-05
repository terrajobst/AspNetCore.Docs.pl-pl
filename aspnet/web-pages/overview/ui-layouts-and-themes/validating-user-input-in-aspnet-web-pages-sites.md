---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Walidacja danych wejściowych użytkownika we wzorcu ASP.NET Web Pages witryny (Razor) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym artykule omówiono sposób sprawdzania poprawności informacji można uzyskać od użytkowników &mdash; oznacza to, się upewnić, że użytkownicy wprowadzają prawidłowe informacje w formacie HTML formularzy w programie sz jako...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 8f049adce33e452896b5e2a444635ff30d18e480
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021524"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Sprawdzanie poprawności danych wejściowych użytkownika w witrynach ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule omówiono sposób sprawdzania poprawności informacji można uzyskać od użytkowników &mdash; czyli się upewnić, że użytkownicy wprowadzają prawidłowe informacje w formacie HTML formularzy w witrynie ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Jak sprawdzić, czy użytkownik dane wejściowe podane przez kryteria weryfikacji zdefiniowanych przez użytkownika.
> - Jak ustalić, czy zostały pomyślnie sprawdzone wszystkie testy sprawdzania poprawności.
> - Jak wyświetlić błędy sprawdzania poprawności (i jak sformatować je).
> - Jak sprawdzić poprawność danych, które nie pochodzi bezpośrednio od użytkowników.
> 
> Poniżej przedstawiono ASP.NET programowania pojęciami opisanymi w artykule:
> 
> - `Validation` Pomocnika.
> - `Html.ValidationSummary` i `Html.ValidationMessage` metody.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


Ten artykuł zawiera następujące sekcje:

- [Omówienie sprawdzania poprawności danych wejściowych użytkownika](#Overview_of_User_Input_Validation)
- [Walidacja danych wejściowych użytkownika](#Validating_User_Input)
- [Dodawanie walidacji po stronie klienta](#Adding_Client-Side_Validation)
- [Błędy sprawdzania poprawności formatowania](#Formatting_Validation_Errors)
- [Sprawdzanie poprawności danych, które nie pochodzi bezpośrednio od użytkowników](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Omówienie sprawdzania poprawności danych wejściowych użytkownika

Jeśli Poproś użytkowników o podanie informacji na stronie — na przykład w formie — jest ważne upewnić się, że wartości, które użytkownik podał są prawidłowe. Na przykład nie chcesz do przetwarzania formularza, brak jest krytycznych informacji.

Podczas wprowadzania wartości do formularza HTML, wartości, które użytkownik podał są ciągami. W wielu przypadkach wartości, potrzebne są niektóre inne typy danych, takich jak liczby całkowite lub daty. W związku z tym również należy upewnić się, że wartości, które użytkownicy wprowadzają może zostać prawidłowo skonwertowany do formatu odpowiedni typ danych.

Można również zainstalować pewne ograniczenia na podstawie wartości. Nawet wtedy, gdy użytkownicy poprawnie wprowadź liczbę całkowitą, na przykład, konieczne może być upewnij się, że wartość mieści się w pewnym zakresie.

![Błędy sprawdzania poprawności, które używają klasy stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Ważne** sprawdzanie poprawności danych wejściowych użytkownika jest również ważne dla bezpieczeństwa. Ograniczenie wartości, które użytkownicy mogą wprowadzać w formularzach, można zmniejszyć prawdopodobieństwo, że ktoś wprowadź wartość, która może naruszyć bezpieczeństwo witryny sieci.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Walidacja danych wejściowych użytkownika

W programie ASP.NET Web Pages 2 można użyć `Validator` element pomocniczy służący do danych wejściowych użytkownika. Podstawowe podejście jest wykonanie następujących czynności:

1. Określ, której dane wejściowe elementów (pola), którą chcesz zweryfikować.

    Zazwyczaj Sprawdź poprawność wartości w `<input>` elementów w formularzu. Jednak jest dobrą praktyką Aby sprawdzić poprawność wszystkich danych wejściowych, nawet w danych wejściowych, która pochodzi z elementu ograniczone, takich jak `<select>` listy. Pomaga to upewnij się, że użytkownicy nie obejścia formantów na stronie i przesyłania formularza.
2. W kodzie strony dodać sprawdzanie poprawności poszczególnych dla każdego elementu wejściowego przy użyciu metody `Validation` pomocnika.

    Aby sprawdzić, czy wymagane pola, użyj `Validation.RequireField(field, [error message])` (dla poszczególnych pól) lub `Validation.RequireFields(field1, field2, ...))` (Aby uzyskać listę pól). W przypadku innych typów weryfikacji, użyj `Validation.Add(field, ValidationType)`. Aby uzyskać `ValidationType`, możesz użyć tych opcji:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Po przesłaniu strony należy sprawdzić, czy Weryfikacja został przekazany, sprawdzając `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Jeśli występują błędy sprawdzania poprawności, możesz pominąć strony normalnego przetwarzania. Na przykład jeśli strona ma na celu aktualizowanie bazy danych, nie zrobisz, dopóki nie zostały naprawione wszystkie błędy weryfikacji.
4. Jeśli występują błędy sprawdzania poprawności, wyświetlić komunikaty o błędach w znaczniku strony przy użyciu `Html.ValidationSummary` lub `Html.ValidationMessage`, lub obu.

Strona, która ilustruje te kroki można znaleźć w poniższym przykładzie.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Aby zobaczyć, jak działa sprawdzania poprawności, uruchom na tej stronie, a następnie celowo popełnione. Na przykład, w tym miejscu wygląda strony Jeśli zapomnisz o wprowadzenie nazwy kurs, po wprowadzeniu, a jeśli wprowadzasz nieprawidłowa data:

![Błędy sprawdzania poprawności na renderowanej stronie](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Dodawanie walidacji po stronie klienta

Domyślnie dane wejściowe użytkownika sprawdzania poprawności po użytkowników przedstawia stronę — oznacza to, sprawdzanie poprawności jest wykonywane w kodzie serwera. Wadą tego podejścia jest to, że użytkownicy nie wiedzą one wprowadzone błąd aż po ich Prześlij na stronie. Jeśli formularz jest długie lub zbyt złożone, raportowanie błędów tylko wtedy, gdy strona jest przekazywana może być niewygodne do użytkownika.

Możesz dodać obsługę przeprowadzania weryfikacji w skrypt po stronie klienta. W takiej sytuacji sprawdzanie poprawności jest wykonywane, gdy użytkownik pracuje w przeglądarce. Na przykład załóżmy, że możesz określić, czy wartość powinna być liczbą całkowitą. Jeśli użytkownik wprowadza wartość nie jest liczbą całkowitą, zgłaszany jest błąd, zaraz po użytkownik opuści pole wprowadzania. Użytkownicy natychmiast otrzymać opinię, która jest wygodne w przypadku ich. Walidacja oparta na klienta również pozwala zmniejszyć liczbę prób użytkownik musi przesłać formularz, aby rozwiązać wiele błędów.

> [!NOTE]
> Nawet w przypadku używania weryfikacji po stronie klienta jest zawsze również przeprowadzana Walidacja w kodzie serwera. Wykonywanie sprawdzania poprawności w kodzie serwera jest miarą zabezpieczeń, w przypadku, gdy użytkownicy pominąć weryfikacji opartą na kliencie.


1. Zarejestruj następujące biblioteki JavaScript na stronie:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Są dwa bibliotek obciążana z sieci dostarczania zawartości (CDN), dzięki czemu masz zawsze mieć je na komputerze lub serwerze. Jednak musi mieć kopię lokalną *jquery.validate.unobtrusive.js*. Jeśli nie już pracujesz za pomocą szablonu programu WebMatrix (takich jak **witryny początkowej** ) zawierającej biblioteki, utworzyć witrynę stron sieci Web, która opiera się na **witryny początkowej**. Następnie skopiuj *js* pliku do bieżącej lokacji.
2. W znaczniku dla każdego elementu, który jest sprawdzanie poprawności, należy dodać wywołanie `Validation.For(field)`. Ta metoda generuje atrybuty, które są używane przez weryfikację po stronie klienta. (Zamiast emitowania rzeczywisty kod JavaScript, metoda emituje atrybutów, takich jak `data-val-...`. Te atrybuty obsługuje sprawdzanie poprawności dyskretnego kodu klienta, która używa technologii jQuery, które wykonają tę pracę).

Następująca strona przedstawiono sposób dodawania funkcji weryfikacji klienta, jak w przykładzie przedstawionym wcześniej.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Sprawdzanie poprawności nie wszystkie uruchomienia na kliencie. W szczególności weryfikacji typu danych (liczba całkowita, daty i tak dalej) nie działają na komputerze klienckim. Następujące testy pracować na kliencie i serwerze:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

W tym przykładzie test na prawidłową datę nie będą działać w kodzie klienta. Jednak test zostanie wykonany w kodzie serwera.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Błędy sprawdzania poprawności formatowania

Można kontrolować sposób wyświetlania błędów sprawdzania poprawności, definiując klas CSS, które mają następujące nazwy zarezerwowane:

- `field-validation-error`. Definiuje dane wyjściowe `Html.ValidationMessage` metody, gdy go jest wyświetlany błąd.
- `field-validation-valid`. Definiuje dane wyjściowe `Html.ValidationMessage` metody, gdy nie ma błędów.
- `input-validation-error`. Definiuje sposób `<input>` elementy są renderowane po wystąpieniu błędu. (Na przykład, można użyć tej klasy, aby ustawić kolor tła &lt;wejściowych&gt; elementu na inny kolor, jeśli jego wartość jest nieprawidłowa.) Ta klasa CSS jest używana tylko podczas weryfikacji klienta (w programie ASP.NET Web Pages 2).
- `input-validation-valid`. Definiuje wygląd elementów `<input>` elementów, gdy nie ma błędów.
- `validation-summary-errors`. Definiuje dane wyjściowe `Html.ValidationSummary` metoda Wyświetla listę błędów.
- `validation-summary-valid`. Definiuje dane wyjściowe `Html.ValidationSummary` metody, gdy nie ma błędów.

Następujące `<style>` bloku przedstawiono zasady dotyczące warunków błędów.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Jeśli dodasz tego bloku stylu w przykładzie strony z we wcześniejszej części tego artykułu, wyświetlania błędów będzie wyglądać podobnie do poniższej ilustracji:

![Błędy sprawdzania poprawności, które używają klasy stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Jeśli nie używasz Sprawdzanie poprawności klienta w programie ASP.NET Web Pages 2, arkusze CSS klasy dla `<input>` elementów (`input-validation-error` i `input-validation-valid` nie ma żadnego efektu.


### <a name="static-and-dynamic-error-display"></a>Wyświetlanie błędów statycznych i dynamicznych

Reguły CSS są dostępne w parach, takich jak `validation-summary-errors` i `validation-summary-valid`. Te pary pozwalają zdefiniować zasady dla obu warunków: warunek błędu i warunek "normal" (bez błędów). Jest ważne dowiedzieć się, że zawsze renderowania kodu znaczników dla wyświetlania błędów, nawet, jeśli nie ma żadnych błędów. Na przykład, jeśli strona ma `Html.ValidationSummary` metody w znaczniku, źródło strony będzie zawierać następujące znaczniki nawet wtedy, gdy strona jest wykonywana po raz pierwszy:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Innymi słowy `Html.ValidationSummary` metody zawsze powoduje wyświetlenie `<div>` elementu i listy, nawet jeśli lista błędów jest pusty. Podobnie `Html.ValidationMessage` metody zawsze powoduje wyświetlenie `<span>` elementu jako symbolu zastępczego dla poszczególnych pól błąd, nawet jeśli nie ma błędów.

W niektórych sytuacjach, wyświetlając komunikat o błędzie może spowodować, że strona ułożenia i może spowodować, że elementy na stronie, aby poruszać się. Reguły CSS, które kończą się na `-valid` pozwalają zdefiniować układ, które mogą pomóc uniknąć tego problemu. Na przykład można zdefiniować `field-validation-error` i `field-validation-valid` zarówno mają takie same stałe rozmiar. W ten sposób obszaru wyświetlania pola jest statyczna i nie ulegnie zmianie przepływem strony, jeśli jest wyświetlany komunikat o błędzie.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Sprawdzanie poprawności danych, które nie pochodzi bezpośrednio od użytkowników

Czasami trzeba sprawdzić informacje, które nie pochodzą bezpośrednio z formularza HTML. Typowym przykładem jest strona, której wartością jest przekazywany w ciągu zapytania, jak w poniższym przykładzie:

`http://server/myapp/EditClassInformation?classid=1022`

W tym przypadku chcesz upewnij się, że wartość, która jest przekazywana do strony (tutaj, 1022 dla wartości `classid`) jest nieprawidłowy. Nie można używać bezpośrednio `Validation` element pomocniczy służący do wykonywania tej weryfikacji. Jednak można użyć innych funkcji systemu sprawdzania poprawności, takich jak możliwość wyświetlania komunikatów o błędach weryfikacji.

> [!NOTE] 
> 
> **Ważne** zawsze sprawdzają poprawność wartości, które można uzyskać z *wszelkie* źródła, takiego jak wartości pola formularza, wartości ciągu zapytania i wartości pliku cookie. To proste, użytkownicy będą mogli zmieniać te wartości (np. do złośliwych celów). Dlatego należy sprawdzić te wartości w celu ochrony aplikacji.


Poniższy przykład pokazuje, jak może zweryfikować wartości, który jest przekazywany w ciągu zapytania. Kod sprawdza, czy wartość nie jest pusty i że jest liczbą całkowitą.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Należy zauważyć, że test jest wykonywane, gdy żądanie jest przesłanie formularza (`if(!IsPost)`). Ten test przejdzie przy pierwszym żądaniu strony, ale nie, gdy żądanie jest przesyłania formularza.

Aby wyświetlić ten błąd, można dodać błąd do listy błędów sprawdzania poprawności, wywołując `Validation.AddFormError("message")`. Jeśli strona zawiera wywołanie `Html.ValidationSummary` metody, błąd jest wyświetlany, podobnie jak błąd sprawdzania poprawności danych wejściowych użytkownika.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[Praca z formularzami HTML w witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202892)
