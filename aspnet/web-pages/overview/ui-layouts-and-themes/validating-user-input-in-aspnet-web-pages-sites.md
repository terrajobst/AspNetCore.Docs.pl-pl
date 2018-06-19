---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Walidacja danych wejściowych użytkownika w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule omówiono sposób sprawdzania poprawności informacji Uzyskaj od użytkowników &mdash; oznacza to, że się upewnić, że użytkownicy Wprowadź prawidłowe informacje w języku HTML formularzy w jako...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899181"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Walidacja danych wejściowych użytkownika w sieci Web platformy ASP.NET (Razor) stron witryny
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule omówiono sposób sprawdzania poprawności informacji Uzyskaj od użytkowników &mdash; oznacza to, się upewnić, że użytkownicy Wprowadź prawidłowe informacje w języku HTML formularzy w witrynie stron sieci Web platformy ASP.NET (Razor).
> 
> Zawartość:
> 
> - Jak sprawdzić, czy użytkownik wejściowych kryteria weryfikacji zdefiniowanych przez użytkownika.
> - Jak ustalić, czy wszystkie testy sprawdzania poprawności zostały pomyślnie sprawdzone.
> - Sposób wyświetlania błędów sprawdzania poprawności (i sposób ich formatowania).
> - Jak można sprawdzić poprawności danych, który nie pochodzi bezpośrednio z użytkowników.
> 
> Są to programowania pojęciami opisanymi w artykule programu ASP.NET:
> 
> - `Validation` Pomocnika.
> - `Html.ValidationSummary` i `Html.ValidationMessage` metody.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


Ten artykuł zawiera następujące sekcje:

- [Omówienie sprawdzania poprawności danych wejściowych użytkownika](#Overview_of_User_Input_Validation)
- [Walidacja danych wejściowych użytkownika](#Validating_User_Input)
- [Dodawanie walidacji po stronie klienta](#Adding_Client-Side_Validation)
- [Błędy sprawdzania poprawności formatowania](#Formatting_Validation_Errors)
- [Sprawdzanie poprawności danych, który nie pochodzi bezpośrednio z użytkowników](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Omówienie sprawdzania poprawności danych wejściowych użytkownika

Jeśli Poproś użytkowników, aby wprowadzić informacje na stronie — na przykład w formie — ważne jest, aby upewnić się, że wartości, które użytkownik podał są prawidłowe. Na przykład nie chcesz przetworzyć formularz, który jest Brak ważnych informacji.

Podczas wprowadzania wartości do formularza HTML, wartości, które użytkownik podał są ciągami. W wielu przypadkach wartości, które są potrzebne są pewne inne typy danych, takich jak liczb całkowitych lub daty. Dlatego też należy upewnij się, że wartości, które użytkownicy wprowadzają można poprawnie przekonwertować do typów danych.

Można również zainstalować na wartościach pewne ograniczenia. Nawet jeśli użytkownicy prawidłowo wprowadź liczbę całkowitą, na przykład może być konieczne upewnij się, że wartość mieści się w pewnym zakresie.

![Błędy sprawdzania poprawności, które używają klasy stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Ważne** Walidacja danych wejściowych użytkownika również jest ważna dla bezpieczeństwa. Ograniczenie wartości, które użytkownicy mogą wprowadzać w formularzach można zmniejszyć prawdopodobieństwo, że ktoś wprowadź wartość, która może naruszyć bezpieczeństwo witryny.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Walidacja danych wejściowych użytkownika

W ASP.NET Web Pages 2, można użyć `Validator` pomocnika do przetestowania danych wejściowych użytkownika. Podstawowe podejście jest wykonanie następujących czynności:

1. Ustal, które wejściowych elementów (pól), aby zweryfikować.

    Zwykle sprawdzania poprawności wartości w `<input>` elementów w formularzu. Jednak jest dobrą praktyką jest zweryfikować wszystkie dane wejściowe, nawet danych wejściowych, która pochodzi z elementu ograniczone, takich jak `<select>` listy. Pozwala to upewnij się, że użytkownicy nie obejścia formantów na stronie i Prześlij formularz.
2. W kodzie strony dodać sprawdzanie poprawności poszczególnych każdym elemencie wejściowym przy użyciu metody `Validation` pomocnika.

    Aby sprawdzić, czy są wymagane pola, użyj `Validation.RequireField(field, [error message])` (dla poszczególnych pól) lub `Validation.RequireFields(field1, field2, ...))` (Aby uzyskać listę pól). Dla innych typów sprawdzania poprawności, użyj `Validation.Add(field, ValidationType)`. Aby uzyskać `ValidationType`, korzystając z tych opcji:

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
3. Po przesłaniu strony, sprawdź, czy Weryfikacja został przekazany przez sprawdzanie `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Jeśli wystąpią jakieś błędy sprawdzania poprawności, możesz pominąć strony normalnego przetwarzania. Na przykład jeśli strona ma na celu zaktualizować bazę danych, możesz nie zrobić dopóki wszystkie błędy weryfikacji został rozwiązany.
4. Jeśli występują błędy sprawdzania poprawności, wyświetlane komunikaty o błędach w znaczniku strony za pomocą `Html.ValidationSummary` lub `Html.ValidationMessage`, lub obie.

W poniższym przykładzie przedstawiono strony, która ilustruje następujące kroki.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Aby zobaczyć, jak działa sprawdzania poprawności, uruchom tę stronę i celowo popełnione. Na przykład, w tym miejscu jest strony wygląd Jeśli zapomnisz o wprowadzenie nazwy ciągu, po wprowadzeniu, a po wprowadzeniu nieprawidłową datę:

![Błędy sprawdzania poprawności na renderowanej stronie](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Dodawanie walidacji po stronie klienta

Domyślnie poprawności danych wejściowych użytkownika po stronie przedstawia użytkowników, oznacza to, że weryfikacja jest przeprowadzana w kodzie serwera. Wadą tego podejścia jest, że użytkownicy nie wiedzą, że zostały one błąd dopiero po ich przesłanie strony. Jeśli formularz jest długie lub zbyt złożone, raportowanie błędów dopiero po przesłaniu strony można niewygodne dla użytkownika.

Możesz dodać obsługę przeprowadzania weryfikacji w skrypt po stronie klienta. W takim przypadku sprawdzanie poprawności jest wykonywane przez użytkowników podczas pracy w przeglądarce. Na przykład załóżmy, że możesz określić, że wartość powinna być liczbą całkowitą. Jeśli użytkownik wprowadzi wartość nie jest liczbą całkowitą, zgłaszany jest błąd, gdy użytkownik opuści pole wprowadzania. Użytkownicy otrzymują natychmiast uzyskuje opinie, czyli w dogodnej chwili. Weryfikacja opartą na kliencie pomaga również zmniejszyć liczbę razy, które użytkownik ma do przesyłania formularza, aby poprawić wiele błędów.

> [!NOTE]
> Nawet jeśli używasz weryfikacji po stronie klienta, zawsze również weryfikacji w kodzie serwera. Wykonywanie weryfikacji w kodzie serwera jest miarą zabezpieczeń, w przypadku, gdy użytkownicy pominąć weryfikacji klienta.


1. Zarejestruj następujące biblioteki JavaScript na stronie:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Są dwa bibliotek obciążana z sieci dostarczania zawartości (CDN), więc masz zawsze mieć je na komputerze lub serwerze. Jednak musi mieć kopię lokalną *jquery.validate.unobtrusive.js*. Jeśli nie już pracujesz z szablonem programu WebMatrix (takich jak **witryny początkowej** ) zawierającej biblioteki należy utworzyć witrynę stron sieci Web na podstawie **witryny początkowej**. Następnie skopiuj *js* plik do bieżącej witryny.
2. W znaczniku dla każdego elementu, który jest sprawdzanie poprawności, dodaj wywołanie do `Validation.For(field)`. Ta metoda emituje atrybuty, które są używane przez weryfikacji po stronie klienta. (Zamiast emitowanie rzeczywisty kod JavaScript, metoda emituje atrybutów, takich jak `data-val-...`. Te atrybuty obsługuje weryfikacji dyskretnego kodu klienta, który używa jQuery do wykonywania pracy).

Następująca strona przedstawiono sposób dodawania funkcji sprawdzania poprawności klienta do wcześniej przykładzie.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Nie wszystkie sprawdzanie poprawności uruchomienia na kliencie. W szczególności Sprawdzanie typu danych (liczba całkowita, Data itd.) nie działają na komputerze klienckim. Następujące testy działa na kliencie i serwerze:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

W tym przykładzie testu dla prawidłowej daty nie będzie działać w kodu klienta. Jednak uruchomienie testu zostanie wykonane w kodzie serwera.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Błędy sprawdzania poprawności formatowania

Można kontrolować sposób wyświetlania błędów sprawdzania poprawności, definiując klasy CSS, które mają następujących zarezerwowanych nazw:

- `field-validation-error`. Określa dane wyjściowe `Html.ValidationMessage` metody, gdy są wyświetlane wystąpił błąd.
- `field-validation-valid`. Określa dane wyjściowe `Html.ValidationMessage` metody, gdy nie było błędu.
- `input-validation-error`. Definiuje sposób `<input>` elementy są renderowane po wystąpieniu błędu. (Na przykład ta klasa umożliwia ustawianie koloru tła &lt;wejściowych&gt; elementu na inny kolor, jeśli jego wartość jest nieprawidłowa.) Ta klasa CSS jest używana tylko podczas weryfikacji klienta (w ASP.NET Web Pages 2).
- `input-validation-valid`. Określa wygląd `<input>` elementów, gdy nie było błędu.
- `validation-summary-errors`. Określa dane wyjściowe `Html.ValidationSummary` metody jest wyświetlana lista błędów.
- `validation-summary-valid`. Określa dane wyjściowe `Html.ValidationSummary` metody, gdy nie było błędu.

Następujące `<style>` blok zawiera zasady dla warunków błędu.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Jeśli ten blok stylu w przykładzie strony z we wcześniejszej części tego artykułu, wyświetlania błędów będzie wyglądać jak na poniższej ilustracji:

![Błędy sprawdzania poprawności, które używają klasy stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Jeśli nie używasz weryfikacji klienta ASP.NET Web Pages 2, kod CSS klasy dla `<input>` elementy (`input-validation-error` i `input-validation-valid` nie ma żadnego efektu.


### <a name="static-and-dynamic-error-display"></a>Wyświetlania błędów statycznych i dynamicznych

Reguły CSS występować parami, takich jak `validation-summary-errors` i `validation-summary-valid`. Te pary pozwalają zdefiniować reguły oba warunki: warunek błędu i warunku "normal" (z systemem innym niż błąd). Należy zrozumieć, że zawsze renderowania kodu znaczników dla wyświetlania błędów, nawet jeśli nie ma żadnych błędów. Na przykład, jeśli strona zawiera `Html.ValidationSummary` metody w znaczniku źródło strony będzie zawierać następujący kod znaczników nawet wtedy, gdy strona jest wykonywana po raz pierwszy:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Innymi słowy `Html.ValidationSummary` metoda zawsze renderuje `<div>` element i lista, nawet jeśli na liście jest pusty. Podobnie `Html.ValidationMessage` metoda zawsze renderuje `<span>` element jako element zastępczy dla poszczególnych pól błąd, nawet jeśli nie było błędu.

W niektórych sytuacjach wyświetlanie komunikat o błędzie może spowodować strony ułożenia i może spowodować elementów na stronie poruszanie się. Reguły CSS, które kończą się `-valid` umożliwiają definiowanie układu, które mogą pomóc uniknąć tego problemu. Na przykład można zdefiniować `field-validation-error` i `field-validation-valid` zarówno mają takie same stały rozmiar. W ten sposób obszaru wyświetlania pola jest statyczna i nie zostaną zmienione przepływem strony wyświetlany jest komunikat o błędzie.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Sprawdzanie poprawności danych, który nie pochodzi bezpośrednio z użytkowników

Czasami trzeba sprawdzić poprawności informacji, które nie pochodzi bezpośrednio z formularza HTML. Typowym przykładem jest strona, której wartością jest przekazywany w ciągu zapytania, jak w poniższym przykładzie:

`http://server/myapp/EditClassInformation?classid=1022`

W takim przypadku chcesz upewnij się, że wartość, która została przekazana do strony (tutaj 1022 dla wartości `classid`) jest nieprawidłowy. Nie można bezpośrednio używać `Validation` pomocnika do wykonania tej weryfikacji. Można jednak użyć innych funkcji systemu sprawdzania poprawności, takie jak możliwość wyświetlania komunikatów o błędach.

> [!NOTE] 
> 
> **Ważne** sprawdzanie poprawności wartości, które można uzyskać z *żadnych* źródła, w tym wartości pola formularza, wartości ciągu zapytania i wartości plików cookie. To proste dla osób do zmiany tych wartości (np. dla celów złośliwego). Dlatego należy sprawdzić te wartości w celu ochrony aplikacji.


W poniższym przykładzie pokazano, jak może zweryfikować wartość, która jest przekazywany w ciągu zapytania. Testy kodu wartość nie jest pusty i że jest liczbą całkowitą.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Zwróć uwagę, że uruchomienie testu jest wykonywane podczas żądania nie jest przesłanie formularza (`if(!IsPost)`). Ten test przejdzie strona jest wykonywana po raz pierwszy, ale nie, jeśli żądanie jest przesyłania formularza.

Aby wyświetlić ten błąd, można dodać błąd na liście błędów sprawdzania poprawności, wywołując `Validation.AddFormError("message")`. Jeśli strona zawiera wywołanie `Html.ValidationSummary` metody, błąd jest wyświetlany, podobnie jak błąd sprawdzania poprawności danych wejściowych użytkownika.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[Praca z formularzy HTML w lokacjach stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202892)
