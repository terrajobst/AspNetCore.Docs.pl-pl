---
title: Dodawanie walidacji do strony ASP.NET Core Razor
author: rick-anderson
description: Dowiedz się, jak dodać sprawdzanie poprawności na stronę Razor programu ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: d4cc0ab9de314c0c5a1a9016efd1e566ff1c47d2
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505781"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="070e1-103">Dodawanie walidacji do strony ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="070e1-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="070e1-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="070e1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="070e1-105">W tej sekcji logikę weryfikacji jest dodawany do `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="070e1-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="070e1-106">Reguły sprawdzania poprawności są wymuszane w dowolnym momencie użytkownik tworzy lub edytowania filmu.</span><span class="sxs-lookup"><span data-stu-id="070e1-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="070e1-107">Walidacja</span><span class="sxs-lookup"><span data-stu-id="070e1-107">Validation</span></span>

<span data-ttu-id="070e1-108">Nosi nazwę główną cechą Wytwarzanie oprogramowania [susz](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D** **R**epeat **Y**ourself").</span><span class="sxs-lookup"><span data-stu-id="070e1-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="070e1-109">Strony razor zachęca do którego funkcje określono raz i znajduje odzwierciedlenie w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="070e1-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="070e1-110">PRÓBNEGO może zmniejszyć ilość kodu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="070e1-110">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="070e1-111">PRÓBNEGO sprawia, że kod błędu mniej podatne i łatwiejsze do testowania i obsługi.</span><span class="sxs-lookup"><span data-stu-id="070e1-111">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="070e1-112">Sprawdzanie poprawności wsparcie ze stronami Razor i programem Entity Framework jest dobrym przykładem susz zasady.</span><span class="sxs-lookup"><span data-stu-id="070e1-112">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="070e1-113">Reguły sprawdzania poprawności deklaratywne są określone w jednym miejscu (w klasie modelu), a zasady są wymuszane wszędzie, gdzie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="070e1-113">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="070e1-114">Dodawania reguł sprawdzania poprawności do modelu movie</span><span class="sxs-lookup"><span data-stu-id="070e1-114">Adding validation rules to the movie model</span></span>

<span data-ttu-id="070e1-115">Otwórz *Models/Movie.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="070e1-115">Open the *Models/Movie.cs* file.</span></span> <span data-ttu-id="070e1-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) zawiera zestaw wbudowanych atrybutów sprawdzania poprawności, które są stosowane w sposób deklaratywny do klasa lub właściwość.</span><span class="sxs-lookup"><span data-stu-id="070e1-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="070e1-117">DataAnnotations zawiera też atrybuty formatowania, takich jak `DataType` ułatwić formatowanie i nie zapewniają weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="070e1-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="070e1-118">Aktualizacja `Movie` klasy, aby korzystać z zalet `Required`, `StringLength`, `RegularExpression`, i `Range` atrybutów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="070e1-118">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="070e1-119">Atrybuty weryfikacji określić zachowanie, który jest wymuszany dla właściwości modelu:</span><span class="sxs-lookup"><span data-stu-id="070e1-119">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="070e1-120">`Required` i `MinimumLength` atrybuty wskazują, że właściwość musi mieć wartość.</span><span class="sxs-lookup"><span data-stu-id="070e1-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="070e1-121">Jednakże nic nie uniemożliwia użytkownikowi wprowadzanie odstępów, aby spełniać ograniczenie sprawdzania poprawności dla typu dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="070e1-121">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="070e1-122">Nieprzyjmujące [typy wartości](/dotnet/csharp/language-reference/keywords/value-types) (takie jak `decimal`, `int`, `float`, i `DateTime`) są założenia wymagane i nie ma potrzeby `Required` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="070e1-122">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="070e1-123">`RegularExpression` Atrybut ogranicza znaki, które użytkownik może wprowadzić.</span><span class="sxs-lookup"><span data-stu-id="070e1-123">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="070e1-124">W poprzednim kodzie `Genre` musi zaczynać się wielkie litery i postępuj zgodnie z zero lub więcej liter, pojedynczym lub podwójnym cudzysłowie, białych znaków lub kreski.</span><span class="sxs-lookup"><span data-stu-id="070e1-124">In the preceding code, `Genre` must start with one or more capital letters and follow with zero or more letters, single or double quotes, whitespace characters, or dashes.</span></span> <span data-ttu-id="070e1-125">`Rating` musi rozpoczynać się wielkie litery, a następnie postępuj zgodnie z zero lub więcej liter, cyfr, pojedynczym lub podwójnym cudzysłowie, białych znaków lub łączniki.</span><span class="sxs-lookup"><span data-stu-id="070e1-125">`Rating` must start with one or more capital letters and follow with zero or more letters, numbers, single or double quotes, whitespace characters, or dashes.</span></span>
* <span data-ttu-id="070e1-126">`Range` Atrybut ogranicza wartości do określonego zakresu.</span><span class="sxs-lookup"><span data-stu-id="070e1-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="070e1-127">`StringLength` Atrybut Ustawia maksymalną długość ciągu i opcjonalnie minimalnej długości.</span><span class="sxs-lookup"><span data-stu-id="070e1-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="070e1-128">Posiadanie reguły sprawdzania poprawności, które automatycznie wymuszanych przez platformy ASP.NET Core ułatwia tworzenie aplikacji bardziej niezawodne.</span><span class="sxs-lookup"><span data-stu-id="070e1-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="070e1-129">Automatyczna Walidacja modeli pomaga w ochronie aplikacji, ponieważ nie masz Pamiętaj, aby zastosować je, gdy zostanie dodany nowy kod.</span><span class="sxs-lookup"><span data-stu-id="070e1-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="070e1-130">Błąd sprawdzania poprawności interfejsu użytkownika w stron Razor</span><span class="sxs-lookup"><span data-stu-id="070e1-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="070e1-131">Uruchom aplikację i przejdź do strony/filmów.</span><span class="sxs-lookup"><span data-stu-id="070e1-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="070e1-132">Wybierz **Utwórz nowy** łącza.</span><span class="sxs-lookup"><span data-stu-id="070e1-132">Select the **Create New** link.</span></span> <span data-ttu-id="070e1-133">Wypełnij formularz z niektórych z nieprawidłowymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="070e1-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="070e1-134">Podczas weryfikacji po stronie klienta jQuery wykryje błąd, wyświetla komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="070e1-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Film wyświetlanie formularza za pomocą wielu błędów weryfikacji po stronie klienta jQuery](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="070e1-136">Zwróć uwagę, jak formularz automatycznie renderowany komunikat o błędzie weryfikacji w każdym polu zawierający nieprawidłową wartość.</span><span class="sxs-lookup"><span data-stu-id="070e1-136">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="070e1-137">Błędy są wymuszane, zarówno po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (Jeśli użytkownik ma Obsługa skryptów JavaScript wyłączona).</span><span class="sxs-lookup"><span data-stu-id="070e1-137">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="070e1-138">Jest to znaczące korzyści, że **nie** zmian w kodzie były konieczne na stronach Utwórz lub zmodyfikuj.</span><span class="sxs-lookup"><span data-stu-id="070e1-138">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="070e1-139">Po DataAnnotations zastosowano do modelu, została włączona weryfikacja interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="070e1-139">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="070e1-140">Strony Razor utworzonych w tym samouczku automatycznie pobierane reguł sprawdzania poprawności (przy użyciu atrybutów weryfikacji właściwości `Movie` klasa modelu).</span><span class="sxs-lookup"><span data-stu-id="070e1-140">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="070e1-141">Walidacja testu przy użyciu strony edytowania tego samego sprawdzania poprawności jest stosowana.</span><span class="sxs-lookup"><span data-stu-id="070e1-141">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="070e1-142">Dane formularza nie jest opublikowana na serwerze, aż nie wystąpią żadne błędy weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="070e1-142">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="070e1-143">Sprawdź formularza, którego dane nie są opublikowane przez co najmniej jeden z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="070e1-143">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="070e1-144">Umieść punkt przerwania w `OnPostAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="070e1-144">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="070e1-145">Walidacja (wybierz **Utwórz** lub **Zapisz**).</span><span class="sxs-lookup"><span data-stu-id="070e1-145">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="070e1-146">Nigdy nie zostanie osiągnięty punkt przerwania.</span><span class="sxs-lookup"><span data-stu-id="070e1-146">The break point is never hit.</span></span>
* <span data-ttu-id="070e1-147">Użyj [narzędzie Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="070e1-147">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="070e1-148">Narzędzia dla deweloperów przeglądarki do monitorowania ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="070e1-148">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="070e1-149">Weryfikacja po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="070e1-149">Server-side validation</span></span>

<span data-ttu-id="070e1-150">Po wyłączeniu JavaScript w przeglądarce przesyłania formularza z błędami opublikuje do serwera.</span><span class="sxs-lookup"><span data-stu-id="070e1-150">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="070e1-151">Opcjonalne, Walidacja po stronie serwera testu:</span><span class="sxs-lookup"><span data-stu-id="070e1-151">Optional, test server-side validation:</span></span>

* <span data-ttu-id="070e1-152">Wyłącz JavaScript w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="070e1-152">Disable JavaScript in the browser.</span></span> <span data-ttu-id="070e1-153">Można to zrobić za pomocą narzędzi dla deweloperów w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="070e1-153">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="070e1-154">Jeśli nie można wyłączyć języka JavaScript w przeglądarce, spróbuj innej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="070e1-154">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="070e1-155">Ustaw punkt przerwania w `OnPostAsync` metody tworzenia lub edycji strony.</span><span class="sxs-lookup"><span data-stu-id="070e1-155">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="070e1-156">Prześlij formularz z błędami sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="070e1-156">Submit a form with validation errors.</span></span>
* <span data-ttu-id="070e1-157">Sprawdź, czy stan modelu jest nieprawidłowy:</span><span class="sxs-lookup"><span data-stu-id="070e1-157">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="070e1-158">Poniższy kod ilustruje część *Create.cshtml* strona, która szkieletu we wcześniejszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="070e1-158">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="070e1-159">Jest używany przez tworzenie i edytowanie strony początkowej formularz wyświetlania i wyświetl ponownie formularz w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="070e1-159">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="070e1-160">[Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) używa [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów, a następnie tworzy atrybutów HTML potrzebne dla technologii jQuery weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="070e1-160">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="070e1-161">[Pomocnik tagu weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) wyświetla błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="070e1-161">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="070e1-162">Zobacz [weryfikacji](xref:mvc/models/validation) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="070e1-162">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="070e1-163">Tworzenie i edytowanie strony mają reguł sprawdzania poprawności w nich.</span><span class="sxs-lookup"><span data-stu-id="070e1-163">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="070e1-164">Reguł sprawdzania poprawności i ciągi błędów są określane tylko w `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="070e1-164">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="070e1-165">Te reguły sprawdzania poprawności są automatycznie stosowane do stron Razor, którą edytować `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="070e1-165">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="070e1-166">Gdy logikę weryfikacji wymaga wprowadzenia zmian, odbywa się tylko w modelu.</span><span class="sxs-lookup"><span data-stu-id="070e1-166">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="070e1-167">Sprawdzania poprawności jest stosowane spójnie w całej aplikacji (logika sprawdzania poprawności jest zdefiniowany w jednym miejscu).</span><span class="sxs-lookup"><span data-stu-id="070e1-167">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="070e1-168">Sprawdzanie poprawności w jednym miejscu pomaga zachować czyste kodu i ułatwia utrzymanie i aktualizowanie.</span><span class="sxs-lookup"><span data-stu-id="070e1-168">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="070e1-169">Przy użyciu atrybutów typu danych</span><span class="sxs-lookup"><span data-stu-id="070e1-169">Using DataType Attributes</span></span>

<span data-ttu-id="070e1-170">Sprawdź `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="070e1-170">Examine the `Movie` class.</span></span> <span data-ttu-id="070e1-171">`System.ComponentModel.DataAnnotations` Przestrzeń nazw zawiera atrybuty formatowania, oprócz wbudowanych zestaw atrybutów weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="070e1-171">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="070e1-172">`DataType` Atrybut jest stosowany do `ReleaseDate` i `Price` właściwości.</span><span class="sxs-lookup"><span data-stu-id="070e1-172">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="070e1-173">`DataType` Atrybuty zawierają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i dostarcza atrybutów, takich jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` do obsługi poczty e-mail).</span><span class="sxs-lookup"><span data-stu-id="070e1-173">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="070e1-174">Użyj `RegularExpression` atrybutu, aby sprawdzić poprawność formatu danych.</span><span class="sxs-lookup"><span data-stu-id="070e1-174">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="070e1-175">`DataType` Atrybut jest używany do określenia typu danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="070e1-175">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="070e1-176">`DataType` atrybuty nie są atrybutów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="070e1-176">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="070e1-177">W przykładowej aplikacji tylko data jest wyświetlana bez godziny.</span><span class="sxs-lookup"><span data-stu-id="070e1-177">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="070e1-178">`DataType` Wyliczenie udostępnia wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress i więcej.</span><span class="sxs-lookup"><span data-stu-id="070e1-178">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="070e1-179">`DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="070e1-179">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="070e1-180">Na przykład `mailto:` łącza mogą być tworzone dla `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="070e1-180">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="070e1-181">Selektor daty można określić dla `DataType.Date` w przeglądarkach obsługujących HTML5.</span><span class="sxs-lookup"><span data-stu-id="070e1-181">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="070e1-182">`DataType` Atrybuty emituje HTML 5 `data-` atrybutów (Wymowa: dane dash), korzystających z przeglądarki HTML 5.</span><span class="sxs-lookup"><span data-stu-id="070e1-182">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="070e1-183">`DataType` Atrybuty czy **nie** Podaj wszelkie sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="070e1-183">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="070e1-184">`DataType.Date` nie określa format daty, która jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="070e1-184">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="070e1-185">Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze `CultureInfo`.</span><span class="sxs-lookup"><span data-stu-id="070e1-185">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="070e1-186">`[Column(TypeName = "decimal(18, 2)")]` Wymagana jest adnotacja danych, dzięki czemu można poprawnie mapowane na platformy Entity Framework Core `Price` walutę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="070e1-186">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="070e1-187">Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="070e1-187">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

::: moniker-end

<span data-ttu-id="070e1-188">`DisplayFormat` Atrybut jest używany jawnie określić format daty:</span><span class="sxs-lookup"><span data-stu-id="070e1-188">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="070e1-189">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinna być stosowana, gdy wartość jest wyświetlana do edycji.</span><span class="sxs-lookup"><span data-stu-id="070e1-189">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="070e1-190">Nie można tego zachowania w przypadku niektórych pól.</span><span class="sxs-lookup"><span data-stu-id="070e1-190">You might not want that behavior for some fields.</span></span> <span data-ttu-id="070e1-191">Na przykład w wartości waluty, prawdopodobnie nie chcesz, symbol waluty w trakcie edycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="070e1-191">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="070e1-192">`DisplayFormat` Atrybut może być używany przez siebie, ale zazwyczaj jest to dobry pomysł, aby użyć `DataType` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="070e1-192">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="070e1-193">`DataType` Atrybut umożliwia przekazywanie semantykę dane, a nie jak renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać za pomocą DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="070e1-193">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="070e1-194">Przeglądarka można włączyć funkcje HTML5 (na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy, itp.)</span><span class="sxs-lookup"><span data-stu-id="070e1-194">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="070e1-195">Domyślnie przeglądarka wyświetli dane przy użyciu poprawny format, w oparciu o ustawienia regionalne.</span><span class="sxs-lookup"><span data-stu-id="070e1-195">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="070e1-196">`DataType` Atrybut można włączyć platformę ASP.NET Core, aby wybrać szablon po prawej stronie pola w celu przedstawienia tych danych.</span><span class="sxs-lookup"><span data-stu-id="070e1-196">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="070e1-197">`DisplayFormat` Jeśli używany przez samego korzysta z szablonu ciągu.</span><span class="sxs-lookup"><span data-stu-id="070e1-197">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="070e1-198">Uwaga: dotyczącą weryfikacji jQuery nie działa w przypadku `Range` atrybutu i `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="070e1-198">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="070e1-199">Na przykład poniższy kod zawsze wyświetli błąd sprawdzania poprawności po stronie klienta, nawet wtedy, gdy jest to data mieści się w określonym zakresie:</span><span class="sxs-lookup"><span data-stu-id="070e1-199">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="070e1-200">Ogólnie nie jest dobrą praktyką jest kompilowanie twardych dat w ramach modeli za pomocą `Range` atrybutu i `DateTime` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="070e1-200">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="070e1-201">Poniższy kod pokazuje atrybuty łączenie w jednym wierszu:</span><span class="sxs-lookup"><span data-stu-id="070e1-201">The following code shows combining attributes on one line:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="070e1-202">[Rozpoczynanie pracy ze stronami Razor i programem EF Core](xref:data/ef-rp/intro) pokazuje zaawansowane operacji programu EF Core przy użyciu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="070e1-202">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="070e1-203">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="070e1-203">Publish to Azure</span></span>

<span data-ttu-id="070e1-204">Aby uzyskać informacje na temat wdrażania na platformie Azure, zobacz [samouczek: tworzenie aplikacji ASP.NET na platformie Azure przy użyciu bazy danych SQL](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="070e1-204">For information on deploying to Azure, See [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="070e1-205">Te instrukcje są przeznaczone dla aplikacji platformy ASP.NET, nie aplikacji ASP.NET Core, ale kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="070e1-205">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="070e1-206">Dziękujemy za wypełnienie tego wprowadzenia do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="070e1-206">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="070e1-207">Dziękujemy za opinię.</span><span class="sxs-lookup"><span data-stu-id="070e1-207">We appreciate feedback.</span></span> <span data-ttu-id="070e1-208">[Rozpoczynanie pracy ze stronami Razor i programem EF Core](xref:data/ef-rp/intro) jest doskonałym uzupełnianie w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="070e1-208">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="070e1-209">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="070e1-209">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>

> [!div class="step-by-step"]
> [<span data-ttu-id="070e1-210">Poprzedni: Dodanie nowego pola</span><span class="sxs-lookup"><span data-stu-id="070e1-210">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
