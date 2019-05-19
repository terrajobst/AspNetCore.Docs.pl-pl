---
title: Dodawanie walidacji do strony ASP.NET Core Razor
author: rick-anderson
description: Dowiedz się, jak dodać sprawdzanie poprawności na stronę Razor programu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 38e1fff9c7a212af992951dbf57e124cae69d36f
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874992"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="76f17-103">Dodawanie walidacji do strony ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="76f17-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="76f17-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="76f17-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="76f17-105">W tej sekcji logikę weryfikacji jest dodawany do `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="76f17-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="76f17-106">Reguły sprawdzania poprawności są wymuszane w dowolnym momencie użytkownik tworzy lub edytowania filmu.</span><span class="sxs-lookup"><span data-stu-id="76f17-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="76f17-107">Walidacja</span><span class="sxs-lookup"><span data-stu-id="76f17-107">Validation</span></span>

<span data-ttu-id="76f17-108">Nosi nazwę główną cechą Wytwarzanie oprogramowania [susz](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D** **R**epeat **Y**ourself").</span><span class="sxs-lookup"><span data-stu-id="76f17-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="76f17-109">Strony razor zachęca do którego funkcje określono raz i znajduje odzwierciedlenie w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76f17-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="76f17-110">PRÓBNEGO może ułatwić:</span><span class="sxs-lookup"><span data-stu-id="76f17-110">DRY can help:</span></span>

* <span data-ttu-id="76f17-111">Zmniejsz ilość kodu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76f17-111">Reduce the amount of code in an app.</span></span>
* <span data-ttu-id="76f17-112">Powoduje, że kod błędu mniej podatne i łatwiejsze do testowania i obsługi.</span><span class="sxs-lookup"><span data-stu-id="76f17-112">Make the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="76f17-113">Sprawdzanie poprawności wsparcie ze stronami Razor i programem Entity Framework jest dobrym przykładem susz zasady.</span><span class="sxs-lookup"><span data-stu-id="76f17-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="76f17-114">Reguły sprawdzania poprawności deklaratywne są określone w jednym miejscu (w klasie modelu), a zasady są wymuszane wszędzie, gdzie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76f17-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

[!INCLUDE[](~/includes/RP-MVC/validation.md)]

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="76f17-115">Błąd sprawdzania poprawności interfejsu użytkownika w stron Razor</span><span class="sxs-lookup"><span data-stu-id="76f17-115">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="76f17-116">Uruchom aplikację i przejdź do strony/filmów.</span><span class="sxs-lookup"><span data-stu-id="76f17-116">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="76f17-117">Wybierz **Utwórz nowy** łącza.</span><span class="sxs-lookup"><span data-stu-id="76f17-117">Select the **Create New** link.</span></span> <span data-ttu-id="76f17-118">Wypełnij formularz z niektórych z nieprawidłowymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="76f17-118">Complete the form with some invalid values.</span></span> <span data-ttu-id="76f17-119">Podczas weryfikacji po stronie klienta jQuery wykryje błąd, wyświetla komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="76f17-119">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Film wyświetlanie formularza za pomocą wielu błędów weryfikacji po stronie klienta jQuery](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="76f17-121">Zwróć uwagę, jak formularz automatycznie renderowany komunikat o błędzie weryfikacji w każdym polu zawierający nieprawidłową wartość.</span><span class="sxs-lookup"><span data-stu-id="76f17-121">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="76f17-122">Błędy są wymuszane, zarówno po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (Jeśli użytkownik ma Obsługa skryptów JavaScript wyłączona).</span><span class="sxs-lookup"><span data-stu-id="76f17-122">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="76f17-123">Jest to znaczące korzyści, że **nie** zmian w kodzie były konieczne na stronach Utwórz lub zmodyfikuj.</span><span class="sxs-lookup"><span data-stu-id="76f17-123">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="76f17-124">Po DataAnnotations zastosowano do modelu, została włączona weryfikacja interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="76f17-124">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="76f17-125">Strony Razor utworzonych w tym samouczku automatycznie pobierane reguł sprawdzania poprawności (przy użyciu atrybutów weryfikacji właściwości `Movie` klasa modelu).</span><span class="sxs-lookup"><span data-stu-id="76f17-125">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="76f17-126">Walidacja testu przy użyciu strony edytowania tego samego sprawdzania poprawności jest stosowana.</span><span class="sxs-lookup"><span data-stu-id="76f17-126">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="76f17-127">Dane formularza nie jest opublikowana na serwerze, aż nie wystąpią żadne błędy weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="76f17-127">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="76f17-128">Sprawdź formularza, którego dane nie są opublikowane przez co najmniej jeden z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="76f17-128">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="76f17-129">Umieść punkt przerwania w `OnPostAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="76f17-129">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="76f17-130">Walidacja (wybierz **Utwórz** lub **Zapisz**).</span><span class="sxs-lookup"><span data-stu-id="76f17-130">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="76f17-131">Nigdy nie zostanie osiągnięty punkt przerwania.</span><span class="sxs-lookup"><span data-stu-id="76f17-131">The break point is never hit.</span></span>
* <span data-ttu-id="76f17-132">Użyj [narzędzie Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="76f17-132">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="76f17-133">Narzędzia dla deweloperów przeglądarki do monitorowania ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="76f17-133">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="76f17-134">Weryfikacja po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="76f17-134">Server-side validation</span></span>

<span data-ttu-id="76f17-135">Po wyłączeniu JavaScript w przeglądarce przesyłania formularza z błędami opublikuje do serwera.</span><span class="sxs-lookup"><span data-stu-id="76f17-135">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="76f17-136">Opcjonalne, Walidacja po stronie serwera testu:</span><span class="sxs-lookup"><span data-stu-id="76f17-136">Optional, test server-side validation:</span></span>

* <span data-ttu-id="76f17-137">Wyłącz JavaScript w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="76f17-137">Disable JavaScript in the browser.</span></span> <span data-ttu-id="76f17-138">Można to zrobić za pomocą narzędzi dla deweloperów w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="76f17-138">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="76f17-139">Jeśli nie można wyłączyć języka JavaScript w przeglądarce, spróbuj innej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="76f17-139">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="76f17-140">Ustaw punkt przerwania w `OnPostAsync` metody tworzenia lub edycji strony.</span><span class="sxs-lookup"><span data-stu-id="76f17-140">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="76f17-141">Prześlij formularz z błędami sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="76f17-141">Submit a form with validation errors.</span></span>
* <span data-ttu-id="76f17-142">Sprawdź, czy stan modelu jest nieprawidłowy:</span><span class="sxs-lookup"><span data-stu-id="76f17-142">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="76f17-143">Poniższy kod ilustruje część *Create.cshtml* strona, która szkieletu we wcześniejszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="76f17-143">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="76f17-144">Jest używany przez tworzenie i edytowanie strony początkowej formularz wyświetlania i wyświetl ponownie formularz w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="76f17-144">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="76f17-145">[Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) używa [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów, a następnie tworzy atrybutów HTML potrzebne dla technologii jQuery weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="76f17-145">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="76f17-146">[Pomocnik tagu weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) wyświetla błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="76f17-146">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="76f17-147">Zobacz [weryfikacji](xref:mvc/models/validation) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="76f17-147">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="76f17-148">Tworzenie i edytowanie strony mają reguł sprawdzania poprawności w nich.</span><span class="sxs-lookup"><span data-stu-id="76f17-148">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="76f17-149">Reguł sprawdzania poprawności i ciągi błędów są określane tylko w `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="76f17-149">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="76f17-150">Te reguły sprawdzania poprawności są automatycznie stosowane do stron Razor, którą edytować `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="76f17-150">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="76f17-151">Gdy logikę weryfikacji wymaga wprowadzenia zmian, odbywa się tylko w modelu.</span><span class="sxs-lookup"><span data-stu-id="76f17-151">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="76f17-152">Sprawdzania poprawności jest stosowane spójnie w całej aplikacji (logika sprawdzania poprawności jest zdefiniowany w jednym miejscu).</span><span class="sxs-lookup"><span data-stu-id="76f17-152">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="76f17-153">Sprawdzanie poprawności w jednym miejscu pomaga zachować czyste kodu i ułatwia utrzymanie i aktualizowanie.</span><span class="sxs-lookup"><span data-stu-id="76f17-153">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="76f17-154">Przy użyciu atrybutów typu danych</span><span class="sxs-lookup"><span data-stu-id="76f17-154">Using DataType Attributes</span></span>

<span data-ttu-id="76f17-155">Sprawdź `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="76f17-155">Examine the `Movie` class.</span></span> <span data-ttu-id="76f17-156">`System.ComponentModel.DataAnnotations` Przestrzeń nazw zawiera atrybuty formatowania, oprócz wbudowanych zestaw atrybutów weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="76f17-156">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="76f17-157">`DataType` Atrybut jest stosowany do `ReleaseDate` i `Price` właściwości.</span><span class="sxs-lookup"><span data-stu-id="76f17-157">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="76f17-158">`DataType` Atrybuty zawierają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i dostarcza atrybutów, takich jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` do obsługi poczty e-mail).</span><span class="sxs-lookup"><span data-stu-id="76f17-158">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="76f17-159">Użyj `RegularExpression` atrybutu, aby sprawdzić poprawność formatu danych.</span><span class="sxs-lookup"><span data-stu-id="76f17-159">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="76f17-160">`DataType` Atrybut jest używany do określenia typu danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="76f17-160">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="76f17-161">`DataType` atrybuty nie są atrybutów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="76f17-161">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="76f17-162">W przykładowej aplikacji tylko data jest wyświetlana bez godziny.</span><span class="sxs-lookup"><span data-stu-id="76f17-162">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="76f17-163">`DataType` Wyliczenie udostępnia wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress i więcej.</span><span class="sxs-lookup"><span data-stu-id="76f17-163">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="76f17-164">`DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76f17-164">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="76f17-165">Na przykład `mailto:` łącza mogą być tworzone dla `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="76f17-165">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="76f17-166">Selektor daty można określić dla `DataType.Date` w przeglądarkach obsługujących HTML5.</span><span class="sxs-lookup"><span data-stu-id="76f17-166">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="76f17-167">`DataType` Atrybuty emituje HTML 5 `data-` atrybutów (Wymowa: dane dash), korzystających z przeglądarki HTML 5.</span><span class="sxs-lookup"><span data-stu-id="76f17-167">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="76f17-168">`DataType` Atrybuty czy **nie** Podaj wszelkie sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="76f17-168">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="76f17-169">`DataType.Date` nie określa format daty, która jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="76f17-169">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="76f17-170">Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze `CultureInfo`.</span><span class="sxs-lookup"><span data-stu-id="76f17-170">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="76f17-171">`[Column(TypeName = "decimal(18, 2)")]` Wymagana jest adnotacja danych, dzięki czemu można poprawnie mapowane na platformy Entity Framework Core `Price` walutę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="76f17-171">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="76f17-172">Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="76f17-172">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="76f17-173">`DisplayFormat` Atrybut jest używany jawnie określić format daty:</span><span class="sxs-lookup"><span data-stu-id="76f17-173">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="76f17-174">`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinna być stosowana, gdy wartość jest wyświetlana do edycji.</span><span class="sxs-lookup"><span data-stu-id="76f17-174">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="76f17-175">Nie można tego zachowania w przypadku niektórych pól.</span><span class="sxs-lookup"><span data-stu-id="76f17-175">You might not want that behavior for some fields.</span></span> <span data-ttu-id="76f17-176">Na przykład w wartości waluty, prawdopodobnie nie chcesz, symbol waluty w trakcie edycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="76f17-176">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="76f17-177">`DisplayFormat` Atrybut może być używany przez siebie, ale zazwyczaj jest to dobry pomysł, aby użyć `DataType` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="76f17-177">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="76f17-178">`DataType` Atrybut umożliwia przekazywanie semantykę dane, a nie jak renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać za pomocą DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="76f17-178">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="76f17-179">Przeglądarka można włączyć funkcje HTML5 (na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy, itp.)</span><span class="sxs-lookup"><span data-stu-id="76f17-179">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="76f17-180">Domyślnie przeglądarka wyświetli dane przy użyciu poprawny format, w oparciu o ustawienia regionalne.</span><span class="sxs-lookup"><span data-stu-id="76f17-180">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="76f17-181">`DataType` Atrybut można włączyć platformę ASP.NET Core, aby wybrać szablon po prawej stronie pola w celu przedstawienia tych danych.</span><span class="sxs-lookup"><span data-stu-id="76f17-181">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="76f17-182">`DisplayFormat` Jeśli używany przez samego korzysta z szablonu ciągu.</span><span class="sxs-lookup"><span data-stu-id="76f17-182">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="76f17-183">Uwaga: dotyczącą weryfikacji jQuery nie działa w przypadku `Range` atrybutu i `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="76f17-183">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="76f17-184">Na przykład poniższy kod zawsze wyświetli błąd sprawdzania poprawności po stronie klienta, nawet wtedy, gdy jest to data mieści się w określonym zakresie:</span><span class="sxs-lookup"><span data-stu-id="76f17-184">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="76f17-185">Ogólnie nie jest dobrą praktyką jest kompilowanie twardych dat w ramach modeli za pomocą `Range` atrybutu i `DateTime` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="76f17-185">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="76f17-186">Poniższy kod pokazuje atrybuty łączenie w jednym wierszu:</span><span class="sxs-lookup"><span data-stu-id="76f17-186">The following code shows combining attributes on one line:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="76f17-187">[Rozpoczynanie pracy ze stronami Razor i programem EF Core](xref:data/ef-rp/intro) pokazuje zaawansowane operacji programu EF Core przy użyciu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="76f17-187">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="76f17-188">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="76f17-188">Publish to Azure</span></span>

<span data-ttu-id="76f17-189">Aby uzyskać informacje na temat wdrażania na platformie Azure, zobacz [samouczka: Tworzenie aplikacji ASP.NET na platformie Azure przy użyciu bazy danych SQL](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="76f17-189">For information on deploying to Azure, see [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="76f17-190">Te instrukcje są przeznaczone dla aplikacji platformy ASP.NET, nie aplikacji ASP.NET Core, ale kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="76f17-190">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="76f17-191">Dziękujemy za wypełnienie tego wprowadzenia do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="76f17-191">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="76f17-192">[Rozpoczynanie pracy ze stronami Razor i programem EF Core](xref:data/ef-rp/intro) jest doskonałym uzupełnianie w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="76f17-192">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76f17-193">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="76f17-193">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [<span data-ttu-id="76f17-194">Wersja usługi YouTube w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="76f17-194">YouTube version of this tutorial</span></span>](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [<span data-ttu-id="76f17-195">Poprzednie: Dodawanie nowego pola</span><span class="sxs-lookup"><span data-stu-id="76f17-195">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
