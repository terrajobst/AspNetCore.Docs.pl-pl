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
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="63cae-103">Walidacja danych wejściowych użytkownika w sieci Web platformy ASP.NET (Razor) stron witryny</span><span class="sxs-lookup"><span data-stu-id="63cae-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="63cae-104">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="63cae-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="63cae-105">W tym artykule omówiono sposób sprawdzania poprawności informacji Uzyskaj od użytkowników &mdash; oznacza to, się upewnić, że użytkownicy Wprowadź prawidłowe informacje w języku HTML formularzy w witrynie stron sieci Web platformy ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="63cae-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="63cae-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="63cae-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="63cae-107">Jak sprawdzić, czy użytkownik wejściowych kryteria weryfikacji zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="63cae-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="63cae-108">Jak ustalić, czy wszystkie testy sprawdzania poprawności zostały pomyślnie sprawdzone.</span><span class="sxs-lookup"><span data-stu-id="63cae-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="63cae-109">Sposób wyświetlania błędów sprawdzania poprawności (i sposób ich formatowania).</span><span class="sxs-lookup"><span data-stu-id="63cae-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="63cae-110">Jak można sprawdzić poprawności danych, który nie pochodzi bezpośrednio z użytkowników.</span><span class="sxs-lookup"><span data-stu-id="63cae-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="63cae-111">Są to programowania pojęciami opisanymi w artykule programu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="63cae-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="63cae-112">`Validation` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="63cae-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="63cae-113">`Html.ValidationSummary` i `Html.ValidationMessage` metody.</span><span class="sxs-lookup"><span data-stu-id="63cae-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="63cae-114">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="63cae-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="63cae-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="63cae-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="63cae-116">W tym samouczku współdziała również z programu ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="63cae-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="63cae-117">Ten artykuł zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="63cae-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="63cae-118">Omówienie sprawdzania poprawności danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="63cae-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="63cae-119">Walidacja danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="63cae-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="63cae-120">Dodawanie walidacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="63cae-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="63cae-121">Błędy sprawdzania poprawności formatowania</span><span class="sxs-lookup"><span data-stu-id="63cae-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="63cae-122">Sprawdzanie poprawności danych, który nie pochodzi bezpośrednio z użytkowników</span><span class="sxs-lookup"><span data-stu-id="63cae-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="63cae-123">Omówienie sprawdzania poprawności danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="63cae-123">Overview of User Input Validation</span></span>

<span data-ttu-id="63cae-124">Jeśli Poproś użytkowników, aby wprowadzić informacje na stronie — na przykład w formie — ważne jest, aby upewnić się, że wartości, które użytkownik podał są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="63cae-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="63cae-125">Na przykład nie chcesz przetworzyć formularz, który jest Brak ważnych informacji.</span><span class="sxs-lookup"><span data-stu-id="63cae-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="63cae-126">Podczas wprowadzania wartości do formularza HTML, wartości, które użytkownik podał są ciągami.</span><span class="sxs-lookup"><span data-stu-id="63cae-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="63cae-127">W wielu przypadkach wartości, które są potrzebne są pewne inne typy danych, takich jak liczb całkowitych lub daty.</span><span class="sxs-lookup"><span data-stu-id="63cae-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="63cae-128">Dlatego też należy upewnij się, że wartości, które użytkownicy wprowadzają można poprawnie przekonwertować do typów danych.</span><span class="sxs-lookup"><span data-stu-id="63cae-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="63cae-129">Można również zainstalować na wartościach pewne ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="63cae-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="63cae-130">Nawet jeśli użytkownicy prawidłowo wprowadź liczbę całkowitą, na przykład może być konieczne upewnij się, że wartość mieści się w pewnym zakresie.</span><span class="sxs-lookup"><span data-stu-id="63cae-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Błędy sprawdzania poprawności, które używają klasy stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="63cae-132">**Ważne** Walidacja danych wejściowych użytkownika również jest ważna dla bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="63cae-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="63cae-133">Ograniczenie wartości, które użytkownicy mogą wprowadzać w formularzach można zmniejszyć prawdopodobieństwo, że ktoś wprowadź wartość, która może naruszyć bezpieczeństwo witryny.</span><span class="sxs-lookup"><span data-stu-id="63cae-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="63cae-134">Walidacja danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="63cae-134">Validating User Input</span></span>

<span data-ttu-id="63cae-135">W ASP.NET Web Pages 2, można użyć `Validator` pomocnika do przetestowania danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="63cae-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="63cae-136">Podstawowe podejście jest wykonanie następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="63cae-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="63cae-137">Ustal, które wejściowych elementów (pól), aby zweryfikować.</span><span class="sxs-lookup"><span data-stu-id="63cae-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="63cae-138">Zwykle sprawdzania poprawności wartości w `<input>` elementów w formularzu.</span><span class="sxs-lookup"><span data-stu-id="63cae-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="63cae-139">Jednak jest dobrą praktyką jest zweryfikować wszystkie dane wejściowe, nawet danych wejściowych, która pochodzi z elementu ograniczone, takich jak `<select>` listy.</span><span class="sxs-lookup"><span data-stu-id="63cae-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="63cae-140">Pozwala to upewnij się, że użytkownicy nie obejścia formantów na stronie i Prześlij formularz.</span><span class="sxs-lookup"><span data-stu-id="63cae-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="63cae-141">W kodzie strony dodać sprawdzanie poprawności poszczególnych każdym elemencie wejściowym przy użyciu metody `Validation` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="63cae-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="63cae-142">Aby sprawdzić, czy są wymagane pola, użyj `Validation.RequireField(field, [error message])` (dla poszczególnych pól) lub `Validation.RequireFields(field1, field2, ...))` (Aby uzyskać listę pól).</span><span class="sxs-lookup"><span data-stu-id="63cae-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="63cae-143">Dla innych typów sprawdzania poprawności, użyj `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="63cae-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="63cae-144">Aby uzyskać `ValidationType`, korzystając z tych opcji:</span><span class="sxs-lookup"><span data-stu-id="63cae-144">For `ValidationType`, you can use these options:</span></span>

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
3. <span data-ttu-id="63cae-145">Po przesłaniu strony, sprawdź, czy Weryfikacja został przekazany przez sprawdzanie `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="63cae-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="63cae-146">Jeśli wystąpią jakieś błędy sprawdzania poprawności, możesz pominąć strony normalnego przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="63cae-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="63cae-147">Na przykład jeśli strona ma na celu zaktualizować bazę danych, możesz nie zrobić dopóki wszystkie błędy weryfikacji został rozwiązany.</span><span class="sxs-lookup"><span data-stu-id="63cae-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="63cae-148">Jeśli występują błędy sprawdzania poprawności, wyświetlane komunikaty o błędach w znaczniku strony za pomocą `Html.ValidationSummary` lub `Html.ValidationMessage`, lub obie.</span><span class="sxs-lookup"><span data-stu-id="63cae-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="63cae-149">W poniższym przykładzie przedstawiono strony, która ilustruje następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="63cae-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="63cae-150">Aby zobaczyć, jak działa sprawdzania poprawności, uruchom tę stronę i celowo popełnione.</span><span class="sxs-lookup"><span data-stu-id="63cae-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="63cae-151">Na przykład, w tym miejscu jest strony wygląd Jeśli zapomnisz o wprowadzenie nazwy ciągu, po wprowadzeniu, a po wprowadzeniu nieprawidłową datę:</span><span class="sxs-lookup"><span data-stu-id="63cae-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Błędy sprawdzania poprawności na renderowanej stronie](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="63cae-153">Dodawanie walidacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="63cae-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="63cae-154">Domyślnie poprawności danych wejściowych użytkownika po stronie przedstawia użytkowników, oznacza to, że weryfikacja jest przeprowadzana w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="63cae-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="63cae-155">Wadą tego podejścia jest, że użytkownicy nie wiedzą, że zostały one błąd dopiero po ich przesłanie strony.</span><span class="sxs-lookup"><span data-stu-id="63cae-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="63cae-156">Jeśli formularz jest długie lub zbyt złożone, raportowanie błędów dopiero po przesłaniu strony można niewygodne dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="63cae-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="63cae-157">Możesz dodać obsługę przeprowadzania weryfikacji w skrypt po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="63cae-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="63cae-158">W takim przypadku sprawdzanie poprawności jest wykonywane przez użytkowników podczas pracy w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="63cae-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="63cae-159">Na przykład załóżmy, że możesz określić, że wartość powinna być liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="63cae-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="63cae-160">Jeśli użytkownik wprowadzi wartość nie jest liczbą całkowitą, zgłaszany jest błąd, gdy użytkownik opuści pole wprowadzania.</span><span class="sxs-lookup"><span data-stu-id="63cae-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="63cae-161">Użytkownicy otrzymują natychmiast uzyskuje opinie, czyli w dogodnej chwili.</span><span class="sxs-lookup"><span data-stu-id="63cae-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="63cae-162">Weryfikacja opartą na kliencie pomaga również zmniejszyć liczbę razy, które użytkownik ma do przesyłania formularza, aby poprawić wiele błędów.</span><span class="sxs-lookup"><span data-stu-id="63cae-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="63cae-163">Nawet jeśli używasz weryfikacji po stronie klienta, zawsze również weryfikacji w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="63cae-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="63cae-164">Wykonywanie weryfikacji w kodzie serwera jest miarą zabezpieczeń, w przypadku, gdy użytkownicy pominąć weryfikacji klienta.</span><span class="sxs-lookup"><span data-stu-id="63cae-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="63cae-165">Zarejestruj następujące biblioteki JavaScript na stronie:</span><span class="sxs-lookup"><span data-stu-id="63cae-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="63cae-166">Są dwa bibliotek obciążana z sieci dostarczania zawartości (CDN), więc masz zawsze mieć je na komputerze lub serwerze.</span><span class="sxs-lookup"><span data-stu-id="63cae-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="63cae-167">Jednak musi mieć kopię lokalną *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="63cae-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="63cae-168">Jeśli nie już pracujesz z szablonem programu WebMatrix (takich jak **witryny początkowej** ) zawierającej biblioteki należy utworzyć witrynę stron sieci Web na podstawie **witryny początkowej**.</span><span class="sxs-lookup"><span data-stu-id="63cae-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="63cae-169">Następnie skopiuj *js* plik do bieżącej witryny.</span><span class="sxs-lookup"><span data-stu-id="63cae-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="63cae-170">W znaczniku dla każdego elementu, który jest sprawdzanie poprawności, dodaj wywołanie do `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="63cae-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="63cae-171">Ta metoda emituje atrybuty, które są używane przez weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="63cae-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="63cae-172">(Zamiast emitowanie rzeczywisty kod JavaScript, metoda emituje atrybutów, takich jak `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="63cae-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="63cae-173">Te atrybuty obsługuje weryfikacji dyskretnego kodu klienta, który używa jQuery do wykonywania pracy).</span><span class="sxs-lookup"><span data-stu-id="63cae-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="63cae-174">Następująca strona przedstawiono sposób dodawania funkcji sprawdzania poprawności klienta do wcześniej przykładzie.</span><span class="sxs-lookup"><span data-stu-id="63cae-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="63cae-175">Nie wszystkie sprawdzanie poprawności uruchomienia na kliencie.</span><span class="sxs-lookup"><span data-stu-id="63cae-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="63cae-176">W szczególności Sprawdzanie typu danych (liczba całkowita, Data itd.) nie działają na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="63cae-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="63cae-177">Następujące testy działa na kliencie i serwerze:</span><span class="sxs-lookup"><span data-stu-id="63cae-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="63cae-178">W tym przykładzie testu dla prawidłowej daty nie będzie działać w kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="63cae-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="63cae-179">Jednak uruchomienie testu zostanie wykonane w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="63cae-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="63cae-180">Błędy sprawdzania poprawności formatowania</span><span class="sxs-lookup"><span data-stu-id="63cae-180">Formatting Validation Errors</span></span>

<span data-ttu-id="63cae-181">Można kontrolować sposób wyświetlania błędów sprawdzania poprawności, definiując klasy CSS, które mają następujących zarezerwowanych nazw:</span><span class="sxs-lookup"><span data-stu-id="63cae-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="63cae-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="63cae-182">`field-validation-error`.</span></span> <span data-ttu-id="63cae-183">Określa dane wyjściowe `Html.ValidationMessage` metody, gdy są wyświetlane wystąpił błąd.</span><span class="sxs-lookup"><span data-stu-id="63cae-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="63cae-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="63cae-184">`field-validation-valid`.</span></span> <span data-ttu-id="63cae-185">Określa dane wyjściowe `Html.ValidationMessage` metody, gdy nie było błędu.</span><span class="sxs-lookup"><span data-stu-id="63cae-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="63cae-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="63cae-186">`input-validation-error`.</span></span> <span data-ttu-id="63cae-187">Definiuje sposób `<input>` elementy są renderowane po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="63cae-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="63cae-188">(Na przykład ta klasa umożliwia ustawianie koloru tła &lt;wejściowych&gt; elementu na inny kolor, jeśli jego wartość jest nieprawidłowa.) Ta klasa CSS jest używana tylko podczas weryfikacji klienta (w ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="63cae-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="63cae-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="63cae-189">`input-validation-valid`.</span></span> <span data-ttu-id="63cae-190">Określa wygląd `<input>` elementów, gdy nie było błędu.</span><span class="sxs-lookup"><span data-stu-id="63cae-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="63cae-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="63cae-191">`validation-summary-errors`.</span></span> <span data-ttu-id="63cae-192">Określa dane wyjściowe `Html.ValidationSummary` metody jest wyświetlana lista błędów.</span><span class="sxs-lookup"><span data-stu-id="63cae-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="63cae-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="63cae-193">`validation-summary-valid`.</span></span> <span data-ttu-id="63cae-194">Określa dane wyjściowe `Html.ValidationSummary` metody, gdy nie było błędu.</span><span class="sxs-lookup"><span data-stu-id="63cae-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="63cae-195">Następujące `<style>` blok zawiera zasady dla warunków błędu.</span><span class="sxs-lookup"><span data-stu-id="63cae-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="63cae-196">Jeśli ten blok stylu w przykładzie strony z we wcześniejszej części tego artykułu, wyświetlania błędów będzie wyglądać jak na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="63cae-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Błędy sprawdzania poprawności, które używają klasy stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="63cae-198">Jeśli nie używasz weryfikacji klienta ASP.NET Web Pages 2, kod CSS klasy dla `<input>` elementy (`input-validation-error` i `input-validation-valid` nie ma żadnego efektu.</span><span class="sxs-lookup"><span data-stu-id="63cae-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="63cae-199">Wyświetlania błędów statycznych i dynamicznych</span><span class="sxs-lookup"><span data-stu-id="63cae-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="63cae-200">Reguły CSS występować parami, takich jak `validation-summary-errors` i `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="63cae-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="63cae-201">Te pary pozwalają zdefiniować reguły oba warunki: warunek błędu i warunku "normal" (z systemem innym niż błąd).</span><span class="sxs-lookup"><span data-stu-id="63cae-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="63cae-202">Należy zrozumieć, że zawsze renderowania kodu znaczników dla wyświetlania błędów, nawet jeśli nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="63cae-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="63cae-203">Na przykład, jeśli strona zawiera `Html.ValidationSummary` metody w znaczniku źródło strony będzie zawierać następujący kod znaczników nawet wtedy, gdy strona jest wykonywana po raz pierwszy:</span><span class="sxs-lookup"><span data-stu-id="63cae-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="63cae-204">Innymi słowy `Html.ValidationSummary` metoda zawsze renderuje `<div>` element i lista, nawet jeśli na liście jest pusty.</span><span class="sxs-lookup"><span data-stu-id="63cae-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="63cae-205">Podobnie `Html.ValidationMessage` metoda zawsze renderuje `<span>` element jako element zastępczy dla poszczególnych pól błąd, nawet jeśli nie było błędu.</span><span class="sxs-lookup"><span data-stu-id="63cae-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="63cae-206">W niektórych sytuacjach wyświetlanie komunikat o błędzie może spowodować strony ułożenia i może spowodować elementów na stronie poruszanie się.</span><span class="sxs-lookup"><span data-stu-id="63cae-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="63cae-207">Reguły CSS, które kończą się `-valid` umożliwiają definiowanie układu, które mogą pomóc uniknąć tego problemu.</span><span class="sxs-lookup"><span data-stu-id="63cae-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="63cae-208">Na przykład można zdefiniować `field-validation-error` i `field-validation-valid` zarówno mają takie same stały rozmiar.</span><span class="sxs-lookup"><span data-stu-id="63cae-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="63cae-209">W ten sposób obszaru wyświetlania pola jest statyczna i nie zostaną zmienione przepływem strony wyświetlany jest komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="63cae-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="63cae-210">Sprawdzanie poprawności danych, który nie pochodzi bezpośrednio z użytkowników</span><span class="sxs-lookup"><span data-stu-id="63cae-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="63cae-211">Czasami trzeba sprawdzić poprawności informacji, które nie pochodzi bezpośrednio z formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="63cae-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="63cae-212">Typowym przykładem jest strona, której wartością jest przekazywany w ciągu zapytania, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="63cae-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="63cae-213">W takim przypadku chcesz upewnij się, że wartość, która została przekazana do strony (tutaj 1022 dla wartości `classid`) jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="63cae-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="63cae-214">Nie można bezpośrednio używać `Validation` pomocnika do wykonania tej weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="63cae-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="63cae-215">Można jednak użyć innych funkcji systemu sprawdzania poprawności, takie jak możliwość wyświetlania komunikatów o błędach.</span><span class="sxs-lookup"><span data-stu-id="63cae-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="63cae-216">**Ważne** sprawdzanie poprawności wartości, które można uzyskać z *żadnych* źródła, w tym wartości pola formularza, wartości ciągu zapytania i wartości plików cookie.</span><span class="sxs-lookup"><span data-stu-id="63cae-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="63cae-217">To proste dla osób do zmiany tych wartości (np. dla celów złośliwego).</span><span class="sxs-lookup"><span data-stu-id="63cae-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="63cae-218">Dlatego należy sprawdzić te wartości w celu ochrony aplikacji.</span><span class="sxs-lookup"><span data-stu-id="63cae-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="63cae-219">W poniższym przykładzie pokazano, jak może zweryfikować wartość, która jest przekazywany w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="63cae-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="63cae-220">Testy kodu wartość nie jest pusty i że jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="63cae-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="63cae-221">Zwróć uwagę, że uruchomienie testu jest wykonywane podczas żądania nie jest przesłanie formularza (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="63cae-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="63cae-222">Ten test przejdzie strona jest wykonywana po raz pierwszy, ale nie, jeśli żądanie jest przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="63cae-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="63cae-223">Aby wyświetlić ten błąd, można dodać błąd na liście błędów sprawdzania poprawności, wywołując `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="63cae-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="63cae-224">Jeśli strona zawiera wywołanie `Html.ValidationSummary` metody, błąd jest wyświetlany, podobnie jak błąd sprawdzania poprawności danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="63cae-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="63cae-225">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="63cae-225">Additional Resources</span></span>

[<span data-ttu-id="63cae-226">Praca z formularzy HTML w lokacjach stron sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="63cae-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
