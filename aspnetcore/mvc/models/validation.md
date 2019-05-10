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
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="25182-103">Weryfikacja modelu w programie ASP.NET Core MVC i stron Razor</span><span class="sxs-lookup"><span data-stu-id="25182-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

<span data-ttu-id="25182-104">W tym artykule opisano sposób sprawdzania poprawności danych wejściowych użytkownika w aplikacji ASP.NET Core MVC lub stron Razor.</span><span class="sxs-lookup"><span data-stu-id="25182-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="25182-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="25182-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="25182-106">Stan modelu</span><span class="sxs-lookup"><span data-stu-id="25182-106">Model state</span></span>

<span data-ttu-id="25182-107">Stan modelu reprezentuje błędy, które pochodzą z dwóch podsystemów: wiązania modelu i sprawdzanie poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="25182-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="25182-108">Błędy, które pochodzą z [wiązanie modelu](model-binding.md) są zwykle błędów konwersji danych (na przykład, "x" jest wprowadzana w polu, które oczekuje, że liczba całkowita).</span><span class="sxs-lookup"><span data-stu-id="25182-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="25182-109">Sprawdzanie poprawności modelu występuje po wiązania modelu i raporty błędów, gdy dane nie są zgodne z regułami biznesowymi (na przykład 0 jest wprowadzana w polu, które oczekuje, że ma klasyfikację od 1 do 5).</span><span class="sxs-lookup"><span data-stu-id="25182-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="25182-110">Zarówno powiązanie modelu, jak i weryfikacja odbywają się przed wykonaniem tej akcji kontrolera lub metody obsługi stron Razor.</span><span class="sxs-lookup"><span data-stu-id="25182-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="25182-111">Dla aplikacji sieci web odpowiada aplikacji aby sprawdzić `ModelState.IsValid` i odpowiednio reagują.</span><span class="sxs-lookup"><span data-stu-id="25182-111">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="25182-112">Aplikacje internetowe zazwyczaj ponownie wyświetlić stronę komunikatu o błędzie:</span><span class="sxs-lookup"><span data-stu-id="25182-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="25182-113">Kontrolery interfejsu API sieci Web nie ma pod `ModelState.IsValid` jeśli mają one `[ApiController]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="25182-113">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="25182-114">W takim przypadku automatyczne HTTP 400 odpowiedzi zawierające szczegółowe informacje o problemie zwracany jest stan modelu jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="25182-114">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="25182-115">Aby uzyskać więcej informacji, zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="25182-115">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="25182-116">Uruchom ponownie sprawdzanie poprawności</span><span class="sxs-lookup"><span data-stu-id="25182-116">Rerun validation</span></span>

<span data-ttu-id="25182-117">Sprawdzanie poprawności jest automatyczne, ale może być konieczne powtórzenie go ręcznie.</span><span class="sxs-lookup"><span data-stu-id="25182-117">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="25182-118">Na przykład możesz obliczyć wartość dla właściwości i aby ponownie uruchomić sprawdzanie poprawności po ustawieniu właściwości na podstawie wartości.</span><span class="sxs-lookup"><span data-stu-id="25182-118">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="25182-119">Ponowne uruchamianie sprawdzania poprawności, należy wywołać `TryValidateModel` metody, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="25182-119">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="25182-120">Sprawdzanie poprawności atrybutów</span><span class="sxs-lookup"><span data-stu-id="25182-120">Validation attributes</span></span>

<span data-ttu-id="25182-121">Sprawdzanie poprawności atrybutów umożliwiają określenie reguł sprawdzania poprawności dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="25182-121">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="25182-122">Poniższy przykład z [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) zawiera klasę modelu, który jest oznaczony za pomocą atrybutów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="25182-122">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="25182-123">`[ClassicMovie]` Atrybut jest atrybutem niestandardowego sprawdzania poprawności, a pozostałe są wbudowane.</span><span class="sxs-lookup"><span data-stu-id="25182-123">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="25182-124">(Niewyświetlany jest `[ClassicMovie2]`, który zawiera alternatywny sposób implementacji atrybutu niestandardowego.)</span><span class="sxs-lookup"><span data-stu-id="25182-124">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="25182-125">Wbudowanych atrybutów</span><span class="sxs-lookup"><span data-stu-id="25182-125">Built-in attributes</span></span>

<span data-ttu-id="25182-126">Oto niektóre atrybuty weryfikacji wbudowane:</span><span class="sxs-lookup"><span data-stu-id="25182-126">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="25182-127">`[CreditCard]`: Weryfikuje, że właściwość ma format karty kredytowej.</span><span class="sxs-lookup"><span data-stu-id="25182-127">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="25182-128">`[Compare]`: Sprawdza poprawność tego dwie właściwości w modelu są zgodne.</span><span class="sxs-lookup"><span data-stu-id="25182-128">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="25182-129">`[EmailAddress]`: Weryfikuje, że właściwość ma format adresu e-mail.</span><span class="sxs-lookup"><span data-stu-id="25182-129">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="25182-130">`[Phone]`: Weryfikuje, że właściwość ma format numeru telefonu.</span><span class="sxs-lookup"><span data-stu-id="25182-130">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="25182-131">`[Range]`: Sprawdza, czy wartość właściwości mieści się w określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="25182-131">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="25182-132">`[RegularExpression]`: Weryfikuje, że wartość właściwości odpowiada określonemu wyrażeniu regularnemu.</span><span class="sxs-lookup"><span data-stu-id="25182-132">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="25182-133">`[Required]`: Sprawdza, czy pole nie jest null.</span><span class="sxs-lookup"><span data-stu-id="25182-133">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="25182-134">Zobacz [atrybutu [wymagane]](#required-attribute) szczegółowe informacje na temat zachowania tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="25182-134">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="25182-135">`[StringLength]`: Weryfikuje, że wartość właściwości ciągu nie przekracza limit określonej długości.</span><span class="sxs-lookup"><span data-stu-id="25182-135">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="25182-136">`[Url]`: Weryfikuje, że właściwość ma format adresu URL.</span><span class="sxs-lookup"><span data-stu-id="25182-136">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="25182-137">`[Remote]`: Sprawdza poprawność danych wejściowych na komputerze klienckim przez wywołanie metody akcji, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="25182-137">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="25182-138">Zobacz [atrybutu [zdalnego]](#remote-attribute) szczegółowe informacje na temat zachowania tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="25182-138">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="25182-139">Pełna lista atrybutów weryfikacji można znaleźć w [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="25182-139">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="25182-140">Komunikaty o błędach</span><span class="sxs-lookup"><span data-stu-id="25182-140">Error messages</span></span>

<span data-ttu-id="25182-141">Sprawdzanie poprawności atrybutów umożliwiają określenie komunikat o błędzie do wyświetlenia w nieprawidłowe dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="25182-141">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="25182-142">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="25182-142">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="25182-143">Wewnętrznie, wywołanie atrybuty `String.Format` z symbolem zastępczym dla nazwy pola i czasami dodatkowe symboli zastępczych.</span><span class="sxs-lookup"><span data-stu-id="25182-143">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="25182-144">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="25182-144">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="25182-145">Po zastosowaniu do `Name` właściwości, komunikat o błędzie, utworzone przez poprzedni kod będzie "długość nazwy musi wynosić od 6 i 8.".</span><span class="sxs-lookup"><span data-stu-id="25182-145">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="25182-146">Aby dowiedzieć się, które parametry są przekazywane do `String.Format` określonego atrybutu komunikatu o błędzie, zobacz [kod źródłowy DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="25182-146">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="25182-147">Atrybut [wymagane]</span><span class="sxs-lookup"><span data-stu-id="25182-147">[Required] attribute</span></span>

<span data-ttu-id="25182-148">Domyślnie system sprawdzania poprawności traktuje parametrów innych niż null lub właściwości, tak, jakby mieli oni `[Required]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="25182-148">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="25182-149">[Typy wartości](/dotnet/csharp/language-reference/keywords/value-types) takich jak `decimal` i `int` nie dopuszczają wartości.</span><span class="sxs-lookup"><span data-stu-id="25182-149">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="25182-150">[Wymagane] sprawdzanie poprawności na serwerze</span><span class="sxs-lookup"><span data-stu-id="25182-150">[Required] validation on the server</span></span>

<span data-ttu-id="25182-151">Na serwerze wymagana wartość jest uważana za brak, jeśli właściwość ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="25182-151">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="25182-152">Dopuszcza pole zawsze jest prawidłowy, a komunikat o błędzie atrybutu [wymagane] nigdy nie jest wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="25182-152">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="25182-153">Jednak wiązanie modelu dla właściwości niedopuszczającej może zakończyć się niepowodzeniem, co w komunikacie o błędzie, takich jak `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="25182-153">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="25182-154">Aby określić niestandardowy komunikat o błędzie weryfikacji po stronie serwera typów innych niż null, dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="25182-154">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="25182-155">Zmień pole na wartość null (na przykład `decimal?` zamiast `decimal`).</span><span class="sxs-lookup"><span data-stu-id="25182-155">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="25182-156">[Dopuszcza wartości null\<T >](/dotnet/csharp/programming-guide/nullable-types/) typy wartości są traktowane jak standardowe typy dopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="25182-156">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="25182-157">Określ domyślny komunikat o błędzie do użycia przez powiązanie modelu, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="25182-157">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="25182-158">Aby uzyskać więcej informacji na temat błędów powiązanie modelu ustawić domyślne komunikaty dla, zobacz <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="25182-158">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="25182-159">Walidacja [wymagane] na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="25182-159">[Required] validation on the client</span></span>

<span data-ttu-id="25182-160">Typy dopuszczające wartości inne niż null i ciągi są obsługiwane inaczej na komputerze klienckim, w porównaniu do serwera.</span><span class="sxs-lookup"><span data-stu-id="25182-160">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="25182-161">Na komputerze klienckim:</span><span class="sxs-lookup"><span data-stu-id="25182-161">On the client:</span></span>

* <span data-ttu-id="25182-162">Wartość jest obecna, tylko wtedy, gdy dane wejściowe podano dla niego.</span><span class="sxs-lookup"><span data-stu-id="25182-162">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="25182-163">W związku z tym weryfikacji po stronie klienta obsługuje typy niedopuszczające takie same jak typy dopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="25182-163">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="25182-164">Białe znaki w polu ciągu jest uznawany za prawidłowych danych wejściowych przez jQuery weryfikacji [wymagane](https://jqueryvalidation.org/required-method/) metody.</span><span class="sxs-lookup"><span data-stu-id="25182-164">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="25182-165">Weryfikacji po stronie serwera uwzględnia pola wymaganego ciągu nieprawidłowy, jeśli tylko białe znaki jest wprowadzana.</span><span class="sxs-lookup"><span data-stu-id="25182-165">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="25182-166">Jak wspomniano wcześniej, typy niedopuszczające są traktowane tak, jakby mieli oni `[Required]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="25182-166">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="25182-167">Oznacza to, że uzyskujesz weryfikacji po stronie klienta, nawet wtedy, gdy nie mają zastosowania `[Required]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="25182-167">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="25182-168">Ale jeśli ten atrybut nie jest używana, otrzymasz domyślnego komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="25182-168">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="25182-169">Aby określić niestandardowy komunikat o błędzie, należy użyć atrybutu.</span><span class="sxs-lookup"><span data-stu-id="25182-169">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="25182-170">Atrybut [zdalnego]</span><span class="sxs-lookup"><span data-stu-id="25182-170">[Remote] attribute</span></span>

<span data-ttu-id="25182-171">`[Remote]` Atrybut implementuje walidacji po stronie klienta, czy wymagane jest wywołanie metody na serwerze, aby ustalić, czy pole danych wejściowych jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="25182-171">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="25182-172">Na przykład aplikacja może być konieczne Sprawdź, czy nazwa użytkownika jest już używana.</span><span class="sxs-lookup"><span data-stu-id="25182-172">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="25182-173">Aby zaimplementować weryfikację zdalną:</span><span class="sxs-lookup"><span data-stu-id="25182-173">To implement remote validation:</span></span>

1. <span data-ttu-id="25182-174">Utwórz metodę akcji dla języka JavaScript do wywołania.</span><span class="sxs-lookup"><span data-stu-id="25182-174">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="25182-175">Sprawdź poprawność jQuery [zdalnego](https://jqueryvalidation.org/remote-method/) metoda oczekuje na odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="25182-175">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="25182-176">`"true"` oznacza, że dane wejściowe są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="25182-176">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="25182-177">`"false"`, `undefined`, lub `null` oznacza, że dane wejściowe są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="25182-177">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="25182-178">Wyświetl domyślny komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="25182-178">Display the default error message.</span></span>
   * <span data-ttu-id="25182-179">Inny ciąg oznacza, że dane wejściowe są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="25182-179">Any other string means the input is invalid.</span></span> <span data-ttu-id="25182-180">Wyświetlenie ciągu jako niestandardowy komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="25182-180">Display the string as a custom error message.</span></span>

   <span data-ttu-id="25182-181">Oto przykład metody akcji, która zwraca niestandardowy komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="25182-181">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="25182-182">W klasie modelu, dodawać adnotacje do właściwości o `[Remote]` atrybut, który wskazuje sprawdzania poprawności metody akcji, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="25182-182">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]

### <a name="additional-fields"></a><span data-ttu-id="25182-183">Dodatkowe pola</span><span class="sxs-lookup"><span data-stu-id="25182-183">Additional fields</span></span>

<span data-ttu-id="25182-184">`AdditionalFields` Właściwość `[Remote]` atrybut umożliwia weryfikowanie kombinacje pól w odniesieniu do danych na serwerze.</span><span class="sxs-lookup"><span data-stu-id="25182-184">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="25182-185">Na przykład jeśli `User` modelu ma `FirstName` i `LastName` właściwości, warto sprawdzić, czy nie istniejący użytkownicy mają już tej pary nazw.</span><span class="sxs-lookup"><span data-stu-id="25182-185">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="25182-186">Poniższy przykład pokazuje, jak używać `AdditionalFields`:</span><span class="sxs-lookup"><span data-stu-id="25182-186">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="25182-187">`AdditionalFields` można jawnie ustawić do ciągów `"FirstName"` i `"LastName"`, ale przy użyciu [ `nameof` ](/dotnet/csharp/language-reference/keywords/nameof) operator upraszcza później refaktoryzacji.</span><span class="sxs-lookup"><span data-stu-id="25182-187">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="25182-188">Metody akcji dla tej weryfikacji musi zaakceptować, imię i ostatnie argumenty nazwy:</span><span class="sxs-lookup"><span data-stu-id="25182-188">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="25182-189">Gdy użytkownik wprowadza imię lub nazwisko, JavaScript sprawia, że zdalnego wywołania, aby zobaczyć, że pary nazw jest już zajęta.</span><span class="sxs-lookup"><span data-stu-id="25182-189">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="25182-190">Aby sprawdzić, co najmniej dwa dodatkowe pola, podaj je jako listę rozdzielonych przecinkami.</span><span class="sxs-lookup"><span data-stu-id="25182-190">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="25182-191">Na przykład, aby dodać `MiddleName` właściwością modelu, `[Remote]` atrybutu, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="25182-191">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="25182-192">`AdditionalFields`, podobnie jak wszystkie argumenty atrybutu, musi być wyrażeniem stałym.</span><span class="sxs-lookup"><span data-stu-id="25182-192">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="25182-193">W związku z tym, nie używaj [ciągiem interpolowanym](/dotnet/csharp/language-reference/keywords/interpolated-strings) lub zadzwoń <xref:System.String.Join*> zainicjować `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="25182-193">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="25182-194">Alternatywy dla wbudowanych atrybutów</span><span class="sxs-lookup"><span data-stu-id="25182-194">Alternatives to built-in attributes</span></span>

<span data-ttu-id="25182-195">Jeśli potrzebujesz nie został dostarczony przez wbudowanych atrybutów sprawdzania poprawności, można:</span><span class="sxs-lookup"><span data-stu-id="25182-195">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="25182-196">[Tworzenie atrybutów niestandardowych, które](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="25182-196">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="25182-197">[Implementowanie IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="25182-197">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="25182-198">Atrybuty niestandardowe</span><span class="sxs-lookup"><span data-stu-id="25182-198">Custom attributes</span></span>

<span data-ttu-id="25182-199">Dla scenariuszy, które nie obsługują atrybuty weryfikacji wbudowanych można utworzyć niestandardowego sprawdzania poprawności atrybutów.</span><span class="sxs-lookup"><span data-stu-id="25182-199">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="25182-200">Utwórz klasę, która dziedziczy z <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>i zastępowania <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> metody.</span><span class="sxs-lookup"><span data-stu-id="25182-200">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="25182-201">`IsValid` Metoda przyjmuje obiekt o nazwie *wartość*, czyli danych wejściowych, które ma zostać zweryfikowana.</span><span class="sxs-lookup"><span data-stu-id="25182-201">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="25182-202">Akceptuje także przeciążenia `ValidationContext` obiektu, który zawiera dodatkowe informacje, takie jak wystąpienie modelu, utworzone przez wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="25182-202">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="25182-203">Poniższy przykład sprawdza, czy data wydania dla filmu w *klasycznego* gatunku nie jest późniejsza niż określonego roku.</span><span class="sxs-lookup"><span data-stu-id="25182-203">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="25182-204">`[ClassicMovie2]` Atrybutu najpierw sprawdza gatunek i kontynuuje tylko, jeśli zawiera on *klasycznego*.</span><span class="sxs-lookup"><span data-stu-id="25182-204">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="25182-205">Dla filmów zidentyfikowane jako klasyka sprawdza daty wydania, aby upewnić się, że nie jest nowszy niż limit przekazany do konstruktora atrybutu.)</span><span class="sxs-lookup"><span data-stu-id="25182-205">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="25182-206">`movie` Reprezentuje zmienną w poprzednim przykładzie `Movie` obiekt, który zawiera dane z przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="25182-206">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="25182-207">`IsValid` Metoda sprawdza, czy data i gatunku.</span><span class="sxs-lookup"><span data-stu-id="25182-207">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="25182-208">Po pomyślnej weryfikacji `IsValid` zwraca `ValidationResult.Success` kodu.</span><span class="sxs-lookup"><span data-stu-id="25182-208">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="25182-209">Podczas sprawdzania poprawności zakończy się niepowodzeniem, `ValidationResult` ze względu na błąd jest zwracany komunikat.</span><span class="sxs-lookup"><span data-stu-id="25182-209">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="25182-210">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="25182-210">IValidatableObject</span></span>

<span data-ttu-id="25182-211">Poprzedni przykład działa tylko w przypadku `Movie` typów.</span><span class="sxs-lookup"><span data-stu-id="25182-211">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="25182-212">Inna opcja weryfikacji na poziomie klasy jest zaimplementowanie `IValidatableObject` w klasie modelu, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="25182-212">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="25182-213">Sprawdzanie poprawności węzeł najwyższego poziomu</span><span class="sxs-lookup"><span data-stu-id="25182-213">Top-level node validation</span></span>

<span data-ttu-id="25182-214">Obejmują węzłów najwyższego poziomu:</span><span class="sxs-lookup"><span data-stu-id="25182-214">Top-level nodes include:</span></span>

* <span data-ttu-id="25182-215">Parametry akcji</span><span class="sxs-lookup"><span data-stu-id="25182-215">Action parameters</span></span>
* <span data-ttu-id="25182-216">Właściwości kontrolera</span><span class="sxs-lookup"><span data-stu-id="25182-216">Controller properties</span></span>
* <span data-ttu-id="25182-217">Parametry obsługi strony</span><span class="sxs-lookup"><span data-stu-id="25182-217">Page handler parameters</span></span>
* <span data-ttu-id="25182-218">Strona właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="25182-218">Page model properties</span></span>

<span data-ttu-id="25182-219">Powiązane z modelu węzłów najwyższego poziomu są weryfikowane oprócz weryfikacji właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="25182-219">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="25182-220">W poniższym przykładzie z przykładowej aplikacji `VerifyPhone` metoda używa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> do sprawdzania poprawności `phone` parametr akcji:</span><span class="sxs-lookup"><span data-stu-id="25182-220">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="25182-221">Można użyć węzłów najwyższego poziomu <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> za pomocą atrybutów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="25182-221">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="25182-222">W poniższym przykładzie z przykładowej aplikacji `CheckAge` Metoda określa, że `age` parametru musi być powiązana z ciągu zapytania po przesłaniu formularza:</span><span class="sxs-lookup"><span data-stu-id="25182-222">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="25182-223">Na stronie Sprawdzanie wiekowa (*CheckAge.cshtml*), istnieją dwie formy.</span><span class="sxs-lookup"><span data-stu-id="25182-223">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="25182-224">Pierwszy formularz przesyła `Age` wartość `99` jako ciąg zapytania: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="25182-224">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="25182-225">Gdy jest to poprawnie sformatowany `age` jest przesyłany przez parametr ciągu zapytania, sprawdza poprawność formularza.</span><span class="sxs-lookup"><span data-stu-id="25182-225">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="25182-226">Drugi formularz na stronie Sprawdzanie wiekowa przesyła `Age` wartością w treści żądania, a weryfikacja zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="25182-226">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="25182-227">Powiązanie kończy się niepowodzeniem, ponieważ `age` parametru muszą pochodzić z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="25182-227">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="25182-228">Podczas korzystania z użyciem `CompatibilityVersion.Version_2_1` lub później, sprawdzanie poprawności węzeł najwyższego poziomu jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="25182-228">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="25182-229">W przeciwnym razie Weryfikacja węzeł najwyższego poziomu jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="25182-229">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="25182-230">Opcja domyślna może zostać zastąpione przez ustawienie <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> właściwości (`Startup.ConfigureServices`), jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="25182-230">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="25182-231">Maksymalna liczba błędów</span><span class="sxs-lookup"><span data-stu-id="25182-231">Maximum errors</span></span>

<span data-ttu-id="25182-232">Sprawdzanie poprawności zostanie zatrzymane, jeśli osiągnięto maksymalną liczbę błędów (200 domyślnie).</span><span class="sxs-lookup"><span data-stu-id="25182-232">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="25182-233">Tę liczbę można skonfigurować w następującym kodem `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25182-233">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="25182-234">Maksymalna rekursji</span><span class="sxs-lookup"><span data-stu-id="25182-234">Maximum recursion</span></span>

<span data-ttu-id="25182-235"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> przechodzi przez wykres obiektu modelu, w trakcie sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="25182-235"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="25182-236">W przypadku modeli, które są bardzo szczegółowe lub nieskończenie cyklicznego sprawdzania poprawności może spowodować przepełnienie stosu.</span><span class="sxs-lookup"><span data-stu-id="25182-236">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="25182-237">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) umożliwia powstrzymywanie weryfikacji wcześnie, jeśli rekursji odwiedzający przekracza skonfigurowaną głębokość.</span><span class="sxs-lookup"><span data-stu-id="25182-237">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="25182-238">Wartość domyślna `MvcOptions.MaxValidationDepth` to 200, podczas korzystania z użyciem `CompatibilityVersion.Version_2_2` lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="25182-238">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="25182-239">W przypadku starszych wersji ma wartość null, co oznacza, że określono ograniczenia głębi.</span><span class="sxs-lookup"><span data-stu-id="25182-239">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="25182-240">Zwarcie automatyczne</span><span class="sxs-lookup"><span data-stu-id="25182-240">Automatic short-circuit</span></span>

<span data-ttu-id="25182-241">Sprawdzanie poprawności jest automatycznie zwartym (pominięto) Jeśli na wykresie modelu nie wymaga weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="25182-241">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="25182-242">Obiekty, których środowisko uruchomieniowe Pomija weryfikację obejmują kolekcje elementów podstawowych (takie jak `byte[]`, `string[]`, `Dictionary<string, string>`) i wykresów złożonych obiektów, które nie mają żadnych modułów weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="25182-242">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="25182-243">Wyłączyć sprawdzanie poprawności</span><span class="sxs-lookup"><span data-stu-id="25182-243">Disable validation</span></span>

<span data-ttu-id="25182-244">Aby wyłączyć sprawdzanie poprawności:</span><span class="sxs-lookup"><span data-stu-id="25182-244">To disable validation:</span></span>

1. <span data-ttu-id="25182-245">Utworzenie implementacji klasy `IObjectModelValidator` , nie Oznacz wszystkie pola jako nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="25182-245">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="25182-246">Dodaj następujący kod do `Startup.ConfigureServices` zastąpić domyślną `IObjectModelValidator` implementacji kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="25182-246">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="25182-247">Nadal mogą pojawiać się błędy stanu modelu, które pochodzą z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="25182-247">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="25182-248">Weryfikacja po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="25182-248">Client-side validation</span></span>

<span data-ttu-id="25182-249">Weryfikacji po stronie klienta zapobiega przesyłanie, aż formularz jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="25182-249">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="25182-250">Przycisk Zatwierdź uruchamia JavaScript, która przesyła formularz lub wyświetla komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="25182-250">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="25182-251">Weryfikacji po stronie klienta pozwala uniknąć niepotrzebnych komunikacji dwustronnej z serwerem, gdy istnieją błędy w danych wejściowych w formularzu.</span><span class="sxs-lookup"><span data-stu-id="25182-251">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="25182-252">Poniższy skrypt, który odwołuje się w *_Layout.cshtml* i *_ValidationScriptsPartial.cshtml* obsługi weryfikacji po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="25182-252">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="25182-253">[JQuery sprawdzania poprawności dyskretnego kodu](https://github.com/aspnet/jquery-validation-unobtrusive) skrypt jest niestandardową biblioteką frontonu firmy Microsoft, która jest oparta na popularnej [jQuery weryfikacji](https://jqueryvalidation.org/) wtyczki.</span><span class="sxs-lookup"><span data-stu-id="25182-253">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="25182-254">Bez sprawdzania poprawności dyskretnego kodu jQuery, trzeba by code tę samą logikę weryfikacji w dwóch miejscach: jeden raz w atrybutach weryfikacji po stronie serwera właściwości model, a następnie ponownie w skryptach po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="25182-254">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="25182-255">Zamiast tego [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) należy użyć atrybutów sprawdzania poprawności i wpisać metadane z właściwości modelu do renderowania kodu HTML 5 `data-` atrybutów dla elementów formularza, które wymagają weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="25182-255">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="25182-256">analizuje sprawdzania poprawności dyskretnego kodu jQuery `data-` atrybutów, a następnie przekazuje logikę do technologii jQuery sprawdzania poprawności, efektywnie "kopiowanie" logiki weryfikacji po stronie serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="25182-256">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="25182-257">Błędy sprawdzania poprawności można wyświetlać na kliencie, używanie pomocników tagów, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="25182-257">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="25182-258">Poprzedni pomocnicy tagów, że poniższy kod HTML.</span><span class="sxs-lookup"><span data-stu-id="25182-258">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="25182-259">Należy zauważyć, że `data-` atrybuty w kodzie HTML dane wyjściowe odnoszą się do atrybutów weryfikacji `ReleaseDate` właściwości.</span><span class="sxs-lookup"><span data-stu-id="25182-259">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="25182-260">`data-val-required` Atrybut zawiera komunikat o błędzie wyświetlany w sytuacji, gdy użytkownik nie wypełnić pole daty wydania.</span><span class="sxs-lookup"><span data-stu-id="25182-260">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="25182-261">jQuery sprawdzania poprawności dyskretnego kodu przekazuje tę wartość do weryfikacji jQuery [ `required()` ](https://jqueryvalidation.org/required-method/) metody, która wyświetla ten komunikat w towarzyszącego  **\<span >** elementu.</span><span class="sxs-lookup"><span data-stu-id="25182-261">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="25182-262">Sprawdzanie poprawności typu danych opiera się na typ architektury .NET własności, chyba że, który jest zastępowany przez `[DataType]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="25182-262">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="25182-263">Przeglądarek ma swoje własne komunikaty o błędach domyślne, ale pakietu sprawdzania poprawności dyskretnego kodu weryfikacji jQuery można zastąpić te komunikaty.</span><span class="sxs-lookup"><span data-stu-id="25182-263">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="25182-264">`[DataType]` atrybuty i podklas, takie jak `[EmailAddress]` umożliwiają określenie komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="25182-264">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="25182-265">Dodawanie walidacji do formularzy dynamicznych</span><span class="sxs-lookup"><span data-stu-id="25182-265">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="25182-266">jQuery sprawdzania poprawności dyskretnego kodu przekazuje logikę weryfikacji i parametry do technologii jQuery sprawdzania poprawności, po pierwszym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="25182-266">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="25182-267">W związku z tym sprawdzanie poprawności nie działa automatycznie na dynamicznie generowanym formularzy.</span><span class="sxs-lookup"><span data-stu-id="25182-267">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="25182-268">Aby włączyć sprawdzanie poprawności, poinformuj jQuery dyskretny kod sprawdzania poprawności, aby przeanalizować dynamiczny formularz natychmiast, po jego utworzeniu.</span><span class="sxs-lookup"><span data-stu-id="25182-268">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="25182-269">Na przykład poniższy kod konfiguruje weryfikacji po stronie klienta w formularzu dodane za pośrednictwem technologii AJAX.</span><span class="sxs-lookup"><span data-stu-id="25182-269">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="25182-270">`$.validator.unobtrusive.parse()` Metoda przyjmuje selektor jQuery, jego jednego argumentu.</span><span class="sxs-lookup"><span data-stu-id="25182-270">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="25182-271">Ta metoda informuje jQuery dyskretny kod sprawdzania poprawności, aby przeanalizować `data-` atrybuty odeśle selektora.</span><span class="sxs-lookup"><span data-stu-id="25182-271">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="25182-272">Wartości te atrybuty są następnie przekazywane do wtyczki weryfikacji jQuery.</span><span class="sxs-lookup"><span data-stu-id="25182-272">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="25182-273">Dodawanie walidacji do kontrolek dynamicznych</span><span class="sxs-lookup"><span data-stu-id="25182-273">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="25182-274">`$.validator.unobtrusive.parse()` Metoda działa na cały formularz, nie w poszczególnych formantów generowanych dynamicznie, takich jak `<input>` i `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="25182-274">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="25182-275">Aby ponownej analizy formularza, Usuń danych weryfikacji, który został dodany podczas formularz był analizowany wcześniej, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="25182-275">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="25182-276">Niestandardowe weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="25182-276">Custom client-side validation</span></span>

<span data-ttu-id="25182-277">Niestandardowe sprawdzanie poprawności klienta jest wykonywane przez generowanie `data-` współpracujące z kartą weryfikacji jQuery niestandardowych atrybutów HTML.</span><span class="sxs-lookup"><span data-stu-id="25182-277">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="25182-278">Następujący przykładowy kod adapter został napisany dla `ClassicMovie` i `ClassicMovie2` atrybuty, które zostały wprowadzone wcześniej w tym artykule:</span><span class="sxs-lookup"><span data-stu-id="25182-278">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="25182-279">Aby dowiedzieć się, jak napisać kart, zobacz [jQuery dokumentacji weryfikacji](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="25182-279">For information about how to write adapters, see the [jQuery Validate documentation](http://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="25182-280">Użycie adaptera dla danego pola jest wyzwalany przez `data-` atrybuty, które:</span><span class="sxs-lookup"><span data-stu-id="25182-280">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="25182-281">Flaga pole jako podlegające weryfikacji (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="25182-281">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="25182-282">Identyfikowanie sprawdzania poprawności reguły nazwy i błąd tekst komunikatu (na przykład `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="25182-282">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="25182-283">Podaj wszelkie dodatkowe parametry modułu sprawdzania poprawności musi (na przykład `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="25182-283">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="25182-284">W poniższym przykładzie przedstawiono `data-` atrybuty dla przykładowej aplikacji `ClassicMovie` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="25182-284">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="25182-285">Jak wspomniano wcześniej, [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) używać informacji z weryfikacji atrybutów w celu renderowania `data-` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="25182-285">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="25182-286">Dostępne są dwie opcje dotyczące pisania kodu, które powoduje utworzenie niestandardowego `data-` atrybuty kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="25182-286">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="25182-287">Utwórz klasę, która pochodzi od klasy `AttributeAdapterBase<TAttribute>` i klasę, która implementuje `IValidationAttributeAdapterProvider`i zarejestrować swoje atrybutu i jego karty w DI.</span><span class="sxs-lookup"><span data-stu-id="25182-287">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="25182-288">Ta metoda jest zgodna [jednostki pojedynczej odpowiedzialności](https://wikipedia.org/wiki/Single_responsibility_principle) , kod sprawdzania poprawności związanych z serwerami i związanych z klientem znajduje się w osobnych klas.</span><span class="sxs-lookup"><span data-stu-id="25182-288">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="25182-289">Karta ma też tę zaletę, że ponieważ jest on zarejestrowany DI, inne usługi w ramach DI są dostępne do niego w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="25182-289">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="25182-290">Implementowanie `IClientModelValidator` w swojej `ValidationAttribute` klasy.</span><span class="sxs-lookup"><span data-stu-id="25182-290">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="25182-291">Ta metoda może być odpowiednie, jeśli atrybut nie wykonuje żadnych weryfikacji po stronie serwera i nie wymaga żadnych usług z DI.</span><span class="sxs-lookup"><span data-stu-id="25182-291">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="25182-292">AttributeAdapter weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="25182-292">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="25182-293">Ta metoda renderowania `data-` atrybutów w formacie HTML jest używany przez `ClassicMovie` atrybutu w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25182-293">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="25182-294">Aby dodać sprawdzanie poprawności klienta za pomocą tej metody:</span><span class="sxs-lookup"><span data-stu-id="25182-294">To add client validation by using this method:</span></span>

1. <span data-ttu-id="25182-295">Utwórz klasę atrybutów adapter dla atrybutu niestandardowego sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="25182-295">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="25182-296">Pochodną klasy z [AttributeAdapterBase\<T >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="25182-296">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="25182-297">Tworzenie `AddValidation` metodę, która dodaje `data-` atrybuty do wyniku renderowania, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="25182-297">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="25182-298">Tworzenie klasy dostawcy karty, która implementuje <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="25182-298">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="25182-299">W `GetAttributeAdapter` metoda przekazać w atrybucie niestandardowym do konstruktora karty, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="25182-299">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="25182-300">Zarejestruj dostawcę karty dla DI w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="25182-300">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="25182-301">IClientModelValidator weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="25182-301">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="25182-302">Ta metoda renderowania `data-` atrybutów w formacie HTML jest używany przez `ClassicMovie2` atrybutu w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25182-302">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="25182-303">Aby dodać sprawdzanie poprawności klienta za pomocą tej metody:</span><span class="sxs-lookup"><span data-stu-id="25182-303">To add client validation by using this method:</span></span>

* <span data-ttu-id="25182-304">W atrybucie niestandardowego sprawdzania poprawności, należy zaimplementować `IClientModelValidator` interfejs i utworzyć `AddValidation` metody.</span><span class="sxs-lookup"><span data-stu-id="25182-304">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="25182-305">W `AddValidation` metody, Dodaj `data-` atrybuty weryfikacji, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="25182-305">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="25182-306">Wyłącz weryfikację po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="25182-306">Disable client-side validation</span></span>

<span data-ttu-id="25182-307">Poniższy kod wyłącza sprawdzanie poprawności klienta w MVC widoki:</span><span class="sxs-lookup"><span data-stu-id="25182-307">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="25182-308">W przypadku stron Razor:</span><span class="sxs-lookup"><span data-stu-id="25182-308">And in Razor Pages:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="25182-309">Inną opcją wyłączenia sprawdzanie poprawności klienta jest komentarz odwołanie do `_ValidationScriptsPartial` w swojej *.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="25182-309">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25182-310">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="25182-310">Additional resources</span></span>

* [<span data-ttu-id="25182-311">Przestrzeń nazw System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="25182-311">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="25182-312">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="25182-312">Model Binding</span></span>](model-binding.md)
