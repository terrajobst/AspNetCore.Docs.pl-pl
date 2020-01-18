---
title: Walidacja modelu w ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się więcej o walidacji modelu w ASP.NET Core MVC i Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 12/15/2019
uid: mvc/models/validation
ms.openlocfilehash: b697f02183c76b9a96471a748a86c144fde47bb0
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2020
ms.locfileid: "76268744"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="19c7a-103">Walidacja modelu w ASP.NET Core MVC i Razor Pages</span><span class="sxs-lookup"><span data-stu-id="19c7a-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="19c7a-104">Autor [Kirka Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="19c7a-104">By [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="19c7a-105">W tym artykule wyjaśniono, jak sprawdzić poprawność danych wprowadzonych przez użytkownika w aplikacji ASP.NET Core MVC lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="19c7a-105">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="19c7a-106">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="19c7a-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="19c7a-107">Stan modelu</span><span class="sxs-lookup"><span data-stu-id="19c7a-107">Model state</span></span>

<span data-ttu-id="19c7a-108">Stan modelu reprezentuje błędy pochodzące z dwóch podsystemów: powiązanie modelu i walidacja modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-108">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="19c7a-109">Błędy, które pochodzą z [powiązania modelu](model-binding.md) , są zwykle Błędy konwersji danych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-109">Errors that originate from [model binding](model-binding.md) are generally data conversion errors.</span></span> <span data-ttu-id="19c7a-110">Na przykład znak "x" jest wprowadzany w polu liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="19c7a-110">For example, an "x" is entered in an integer field.</span></span> <span data-ttu-id="19c7a-111">Walidacja modelu odbywa się po powiązaniu modelu i zgłasza błędy, gdy dane nie są zgodne z regułami biznesowymi.</span><span class="sxs-lookup"><span data-stu-id="19c7a-111">Model validation occurs after model binding and reports errors where data doesn't conform to business rules.</span></span> <span data-ttu-id="19c7a-112">Na przykład wartość 0 jest wprowadzana w polu, które oczekuje klasyfikacji z przedziału od 1 do 5.</span><span class="sxs-lookup"><span data-stu-id="19c7a-112">For example, a 0 is entered in a field that expects a rating between 1 and 5.</span></span>

<span data-ttu-id="19c7a-113">Powiązanie modelu i walidacja modelu są wykonywane przed wykonaniem akcji kontrolera lub metody obsługi Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="19c7a-113">Both model binding and model validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="19c7a-114">W przypadku aplikacji sieci Web jest odpowiedzialna za to, aby aplikacja była odpowiednio sprawdzana `ModelState.IsValid` i reagować.</span><span class="sxs-lookup"><span data-stu-id="19c7a-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="19c7a-115">Aplikacja internetowa zazwyczaj ponownie wyświetla stronę z komunikatem o błędzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

<span data-ttu-id="19c7a-116">Kontrolery interfejsu API sieci Web nie muszą sprawdzać `ModelState.IsValid`, jeśli mają atrybut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="19c7a-117">W takim przypadku automatyczna odpowiedź HTTP 400 zawierająca szczegóły błędu jest zwracana, gdy stan modelu jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="19c7a-117">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="19c7a-118">Aby uzyskać więcej informacji, zobacz [Automatyczne HTTP 400 odpowiedzi](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="19c7a-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="19c7a-119">Uruchom ponownie weryfikację</span><span class="sxs-lookup"><span data-stu-id="19c7a-119">Rerun validation</span></span>

<span data-ttu-id="19c7a-120">Walidacja jest automatyczna, ale warto powtórzyć ją ręcznie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="19c7a-121">Na przykład można obliczyć wartość właściwości i chcieć ponownie uruchomić weryfikację po ustawieniu właściwości na wartość obliczaną.</span><span class="sxs-lookup"><span data-stu-id="19c7a-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="19c7a-122">Aby ponownie uruchomić weryfikację, wywołaj metodę `TryValidateModel`, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="19c7a-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_TryValidate&highlight=3-6)]

## <a name="validation-attributes"></a><span data-ttu-id="19c7a-123">Atrybuty walidacji</span><span class="sxs-lookup"><span data-stu-id="19c7a-123">Validation attributes</span></span>

<span data-ttu-id="19c7a-124">Atrybuty walidacji umożliwiają określanie reguł walidacji dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="19c7a-125">W poniższym przykładzie z przykładowej aplikacji przedstawiono klasę modelu, która ma adnotację z atrybutami walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-125">The following example from the sample app shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="19c7a-126">Atrybut `[ClassicMovie]` jest niestandardowym atrybutem walidacji, a inne są wbudowane.</span><span class="sxs-lookup"><span data-stu-id="19c7a-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="19c7a-127">Niepokazywany jest `[ClassicMovieWithClientValidator]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-127">Not shown is `[ClassicMovieWithClientValidator]`.</span></span> <span data-ttu-id="19c7a-128">`[ClassicMovieWithClientValidator]` pokazuje alternatywny sposób implementacji atrybutu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="19c7a-128">`[ClassicMovieWithClientValidator]` shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/Movie.cs?name=snippet_Class)]

## <a name="built-in-attributes"></a><span data-ttu-id="19c7a-129">Atrybuty wbudowane</span><span class="sxs-lookup"><span data-stu-id="19c7a-129">Built-in attributes</span></span>

<span data-ttu-id="19c7a-130">Poniżej przedstawiono niektóre wbudowane atrybuty walidacji:</span><span class="sxs-lookup"><span data-stu-id="19c7a-130">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="19c7a-131">`[CreditCard]`: sprawdza, czy właściwość ma format karty kredytowej.</span><span class="sxs-lookup"><span data-stu-id="19c7a-131">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="19c7a-132">`[Compare]`: sprawdza, czy dwie właściwości w modelu pasują do siebie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-132">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="19c7a-133">`[EmailAddress]`: sprawdza, czy właściwość ma format wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="19c7a-133">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="19c7a-134">`[Phone]`: sprawdza, czy właściwość ma format numeru telefonu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-134">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="19c7a-135">`[Range]`: sprawdza, czy wartość właściwości znajduje się w określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-135">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="19c7a-136">`[RegularExpression]`: sprawdza, czy wartość właściwości jest zgodna z określonym wyrażeniem regularnym.</span><span class="sxs-lookup"><span data-stu-id="19c7a-136">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="19c7a-137">`[Required]`: sprawdza, czy pole nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-137">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="19c7a-138">Zobacz [`[Required]` atrybutu](#required-attribute) , aby uzyskać szczegółowe informacje o zachowaniu tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-138">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="19c7a-139">`[StringLength]`: sprawdza, czy wartość właściwości String nie przekracza podanego limitu długości.</span><span class="sxs-lookup"><span data-stu-id="19c7a-139">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="19c7a-140">`[Url]`: sprawdza, czy właściwość ma format adresu URL.</span><span class="sxs-lookup"><span data-stu-id="19c7a-140">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="19c7a-141">`[Remote]`: sprawdza poprawność danych wejściowych na kliencie przez wywołanie metody akcji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="19c7a-141">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="19c7a-142">Zobacz [`[Remote]` atrybutu](#remote-attribute) , aby uzyskać szczegółowe informacje o zachowaniu tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-142">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="19c7a-143">Pełną listę atrybutów sprawdzania poprawności można znaleźć w przestrzeni nazw [System. ComponentModel. DataAnnotations](xref:System.ComponentModel.DataAnnotations) .</span><span class="sxs-lookup"><span data-stu-id="19c7a-143">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="19c7a-144">Komunikaty o błędach</span><span class="sxs-lookup"><span data-stu-id="19c7a-144">Error messages</span></span>

<span data-ttu-id="19c7a-145">Atrybuty walidacji pozwalają określić komunikat o błędzie, który ma być wyświetlany dla nieprawidłowych danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-145">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="19c7a-146">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="19c7a-146">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="19c7a-147">Wewnętrznie atrybuty wywołują `String.Format` z symbolem zastępczym dla nazwy pola i czasami dodatkowych symboli zastępczych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-147">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="19c7a-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="19c7a-148">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="19c7a-149">W przypadku zastosowania do właściwości `Name` komunikat o błędzie utworzony przez poprzedni kod powinien mieć wartość "nazwa musi zawierać się w przedziale od 6 do 8".</span><span class="sxs-lookup"><span data-stu-id="19c7a-149">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="19c7a-150">Aby dowiedzieć się, które parametry są przesyłane do `String.Format` dla komunikatu o błędzie określonego atrybutu, zobacz [kod źródłowy adnotacji](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations)danych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-150">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="19c7a-151">[Required] — atrybut</span><span class="sxs-lookup"><span data-stu-id="19c7a-151">[Required] attribute</span></span>

<span data-ttu-id="19c7a-152">System sprawdzania poprawności w programie .NET Core 3,0 lub nowszy traktuje parametry niedopuszczające wartości null lub właściwości powiązane tak, jakby miały atrybut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-152">The validation system in .NET Core 3.0 and later treats non-nullable parameters or bound properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="19c7a-153">[Typy wartości](/dotnet/csharp/language-reference/keywords/value-types) , takie jak `decimal` i `int`, nie dopuszczają wartości null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-153">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span> <span data-ttu-id="19c7a-154">To zachowanie można wyłączyć przez skonfigurowanie <xref:Microsoft.AspNetCore.Mvc.MvcOptions.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes> w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-154">This behavior can be disabled by configuring <xref:Microsoft.AspNetCore.Mvc.MvcOptions.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes> in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="19c7a-155">usługi CSharp. Addcontrollers (Opcje = > Opcje. SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true); ...</span><span class="sxs-lookup"><span data-stu-id="19c7a-155">\`\`csharp services.AddControllers(options => options.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true); ...</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="19c7a-156">[Wymagane] Walidacja na serwerze</span><span class="sxs-lookup"><span data-stu-id="19c7a-156">[Required] validation on the server</span></span>

<span data-ttu-id="19c7a-157">Na serwerze, wymagana wartość jest uważana za brakującą, jeśli właściwość ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-157">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="19c7a-158">Pole niedopuszczające wartości null jest zawsze prawidłowe, a komunikat o błędzie `[Required]` atrybutu nigdy nie jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="19c7a-158">A non-nullable field is always valid, and the `[Required]` attribute's error message is never displayed.</span></span>

<span data-ttu-id="19c7a-159">Jednak powiązanie modelu dla właściwości niedopuszczających wartości null może zakończyć się niepowodzeniem, co spowoduje, że zostanie wyświetlony komunikat o błędzie, taki jak `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-159">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="19c7a-160">Aby określić niestandardowy komunikat o błędzie dla weryfikacji po stronie serwera dla typów niedopuszczających wartości null, dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="19c7a-160">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="19c7a-161">Ustaw pole jako dopuszczające wartość null (na przykład `decimal?` zamiast `decimal`).</span><span class="sxs-lookup"><span data-stu-id="19c7a-161">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="19c7a-162">[Dopuszczane wartości null\<t >](/dotnet/csharp/programming-guide/nullable-types/) typy wartości są traktowane jak standardowe typy dopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-162">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="19c7a-163">Określ domyślny komunikat o błędzie, który ma być używany przez powiązanie modelu, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-163">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=5-6)]

  <span data-ttu-id="19c7a-164">Aby uzyskać więcej informacji o błędach powiązania modelu, dla których można ustawić domyślne komunikaty dla programu, zobacz <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="19c7a-164">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="19c7a-165">[Wymagane] weryfikacja na kliencie</span><span class="sxs-lookup"><span data-stu-id="19c7a-165">[Required] validation on the client</span></span>

<span data-ttu-id="19c7a-166">Typy niedopuszczające wartości null i ciągi są obsługiwane inaczej na kliencie w porównaniu z serwerem.</span><span class="sxs-lookup"><span data-stu-id="19c7a-166">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="19c7a-167">Na kliencie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-167">On the client:</span></span>

* <span data-ttu-id="19c7a-168">Wartość jest uważana za obecną tylko wtedy, gdy wprowadzono dla niej dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-168">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="19c7a-169">W związku z tym, walidacja po stronie klienta obsługuje niedopuszczające wartości null typy takie same jak typy dopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-169">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="19c7a-170">Biały znak w polu ciągu jest uznawany za prawidłowe dane wejściowe przez [wymaganą](https://jqueryvalidation.org/required-method/) metodę weryfikacji jQuery.</span><span class="sxs-lookup"><span data-stu-id="19c7a-170">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="19c7a-171">Walidacja po stronie serwera traktuje wymagane pole ciągu nieprawidłowe, jeśli wprowadzono tylko odstępy.</span><span class="sxs-lookup"><span data-stu-id="19c7a-171">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="19c7a-172">Jak wspomniano wcześniej, typy niedopuszczające wartości null są traktowane jako chociaż mają atrybut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-172">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="19c7a-173">Oznacza to, że można uzyskać weryfikację po stronie klienta, nawet jeśli nie zastosowano atrybutu `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-173">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="19c7a-174">Ale jeśli nie używasz tego atrybutu, zostanie wyświetlony domyślny komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-174">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="19c7a-175">Aby określić niestandardowy komunikat o błędzie, Użyj atrybutu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-175">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="19c7a-176">[Remote] — atrybut</span><span class="sxs-lookup"><span data-stu-id="19c7a-176">[Remote] attribute</span></span>

<span data-ttu-id="19c7a-177">Atrybut `[Remote]` implementuje walidację po stronie klienta, która wymaga wywołania metody na serwerze w celu określenia, czy dane wejściowe pola są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-177">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="19c7a-178">Na przykład aplikacja może wymagać sprawdzenia, czy nazwa użytkownika jest już używana.</span><span class="sxs-lookup"><span data-stu-id="19c7a-178">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="19c7a-179">Aby zaimplementować zdalne sprawdzanie poprawności:</span><span class="sxs-lookup"><span data-stu-id="19c7a-179">To implement remote validation:</span></span>

1. <span data-ttu-id="19c7a-180">Utwórz metodę akcji dla języka JavaScript do wywołania.</span><span class="sxs-lookup"><span data-stu-id="19c7a-180">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="19c7a-181">Metoda [zdalna](https://jqueryvalidation.org/remote-method/) walidacji jQuery oczekuje odpowiedzi JSON:</span><span class="sxs-lookup"><span data-stu-id="19c7a-181">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="19c7a-182">`true` oznacza, że dane wejściowe są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-182">`true` means the input data is valid.</span></span>
   * <span data-ttu-id="19c7a-183">`false`, `undefined`lub `null` oznacza, że dane wejściowe są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-183">`false`, `undefined`, or `null` means the input is invalid.</span></span> <span data-ttu-id="19c7a-184">Wyświetl domyślny komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-184">Display the default error message.</span></span>
   * <span data-ttu-id="19c7a-185">Każdy inny ciąg oznacza, że dane wejściowe są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-185">Any other string means the input is invalid.</span></span> <span data-ttu-id="19c7a-186">Wyświetl ciąg jako niestandardowy komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-186">Display the string as a custom error message.</span></span>

   <span data-ttu-id="19c7a-187">Oto przykład metody akcji, która zwraca niestandardowy komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-187">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="19c7a-188">W klasie modelu Dodaj adnotację do właściwości z atrybutem `[Remote]`, który wskazuje na metodę akcji walidacji, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-188">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Email)]
 
   <span data-ttu-id="19c7a-189">Atrybut `[Remote]` znajduje się w przestrzeni nazw `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-189">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="19c7a-190">Dodatkowe pola</span><span class="sxs-lookup"><span data-stu-id="19c7a-190">Additional fields</span></span>

<span data-ttu-id="19c7a-191">Właściwość `AdditionalFields` atrybutu `[Remote]` umożliwia Weryfikowanie kombinacji pól względem danych na serwerze.</span><span class="sxs-lookup"><span data-stu-id="19c7a-191">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="19c7a-192">Na przykład jeśli model `User` ma `FirstName` i `LastName` właściwości, warto sprawdzić, czy żaden istniejący użytkownik ma już tę parę nazw.</span><span class="sxs-lookup"><span data-stu-id="19c7a-192">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="19c7a-193">Poniższy przykład pokazuje, jak używać `AdditionalFields`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-193">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Name&highlight=1,5)]

<span data-ttu-id="19c7a-194">`AdditionalFields` można jawnie ustawić dla ciągów "FirstName" i "LastName", ale użycie operatora [nameof](/dotnet/csharp/language-reference/keywords/nameof) upraszcza późniejsze refaktoryzacje.</span><span class="sxs-lookup"><span data-stu-id="19c7a-194">`AdditionalFields` could be set explicitly to the strings "FirstName" and "LastName", but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="19c7a-195">Metoda akcji dla tej walidacji musi akceptować zarówno argumenty `firstName`, jak i `lastName`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-195">The action method for this validation must accept both `firstName` and `lastName` arguments:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="19c7a-196">Gdy użytkownik wprowadzi imię lub nazwisko, kod JavaScript wykonuje zdalne wywołanie, aby sprawdzić, czy ta para nazw została podjęta.</span><span class="sxs-lookup"><span data-stu-id="19c7a-196">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="19c7a-197">Aby sprawdzić poprawność dwóch lub więcej pól, podaj je jako listę rozdzielaną przecinkami.</span><span class="sxs-lookup"><span data-stu-id="19c7a-197">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="19c7a-198">Na przykład aby dodać właściwość `MiddleName` do modelu, należy ustawić atrybut `[Remote]`, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-198">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="19c7a-199">`AdditionalFields`, podobnie jak wszystkie argumenty atrybutu, musi być wyrażeniem stałym.</span><span class="sxs-lookup"><span data-stu-id="19c7a-199">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="19c7a-200">W związku z tym nie należy używać [interpolowanego ciągu](/dotnet/csharp/language-reference/keywords/interpolated-strings) ani <xref:System.String.Join*> wywołań, aby zainicjować `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-200">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="19c7a-201">Alternatywy dla wbudowanych atrybutów</span><span class="sxs-lookup"><span data-stu-id="19c7a-201">Alternatives to built-in attributes</span></span>

<span data-ttu-id="19c7a-202">Jeśli potrzebujesz weryfikacji niedostarczonej przez wbudowane atrybuty, możesz:</span><span class="sxs-lookup"><span data-stu-id="19c7a-202">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="19c7a-203">[Tworzenie atrybutów niestandardowych](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="19c7a-203">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="19c7a-204">[Zaimplementuj IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="19c7a-204">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="19c7a-205">Atrybuty niestandardowe</span><span class="sxs-lookup"><span data-stu-id="19c7a-205">Custom attributes</span></span>

<span data-ttu-id="19c7a-206">W przypadku scenariuszy, w których wbudowane atrybuty walidacji nie obsługują, można utworzyć niestandardowe atrybuty walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-206">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="19c7a-207">Utwórz klasę, która dziedziczy po <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, i Zastąp metodę <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="19c7a-207">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="19c7a-208">Metoda `IsValid` akceptuje obiekt o nazwie *Value*, czyli dane wejściowe do zweryfikowania.</span><span class="sxs-lookup"><span data-stu-id="19c7a-208">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="19c7a-209">Przeciążenie akceptuje również obiekt `ValidationContext`, który zawiera dodatkowe informacje, takie jak wystąpienie modelu utworzone przez powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-209">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="19c7a-210">Poniższy przykład sprawdza, czy Data wydania filmu w *klasycznym* gatunku nie jest późniejsza niż określony rok.</span><span class="sxs-lookup"><span data-stu-id="19c7a-210">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="19c7a-211">`[ClassicMovie]` atrybut:</span><span class="sxs-lookup"><span data-stu-id="19c7a-211">The `[ClassicMovie]` attribute:</span></span>

* <span data-ttu-id="19c7a-212">Jest uruchamiany tylko na serwerze.</span><span class="sxs-lookup"><span data-stu-id="19c7a-212">Is only run on the server.</span></span>
* <span data-ttu-id="19c7a-213">W przypadku klasycznych filmów program sprawdza poprawność daty wydania:</span><span class="sxs-lookup"><span data-stu-id="19c7a-213">For Classic movies, validates the release date:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttribute.cs?name=snippet_Class)]

<span data-ttu-id="19c7a-214">Zmienna `movie` w poprzednim przykładzie reprezentuje obiekt `Movie`, który zawiera dane z przesłania formularza.</span><span class="sxs-lookup"><span data-stu-id="19c7a-214">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="19c7a-215">Gdy Walidacja nie powiedzie się, zostanie zwrócony `ValidationResult` z komunikatem o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-215">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="19c7a-216">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="19c7a-216">IValidatableObject</span></span>

<span data-ttu-id="19c7a-217">Poprzedni przykład działa tylko z typami `Movie`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-217">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="19c7a-218">Inną opcją weryfikacji na poziomie klasy jest implementacja `IValidatableObject` w klasie modelu, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-218">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/ValidatableMovie.cs?name=snippet_Class&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="19c7a-219">Sprawdzanie poprawności węzła najwyższego poziomu</span><span class="sxs-lookup"><span data-stu-id="19c7a-219">Top-level node validation</span></span>

<span data-ttu-id="19c7a-220">Węzły najwyższego poziomu obejmują:</span><span class="sxs-lookup"><span data-stu-id="19c7a-220">Top-level nodes include:</span></span>

* <span data-ttu-id="19c7a-221">Parametry akcji</span><span class="sxs-lookup"><span data-stu-id="19c7a-221">Action parameters</span></span>
* <span data-ttu-id="19c7a-222">Właściwości kontrolera</span><span class="sxs-lookup"><span data-stu-id="19c7a-222">Controller properties</span></span>
* <span data-ttu-id="19c7a-223">Parametry procedury obsługi stron</span><span class="sxs-lookup"><span data-stu-id="19c7a-223">Page handler parameters</span></span>
* <span data-ttu-id="19c7a-224">Właściwości modelu strony</span><span class="sxs-lookup"><span data-stu-id="19c7a-224">Page model properties</span></span>

<span data-ttu-id="19c7a-225">Węzły najwyższego poziomu powiązane z modelem są weryfikowane jako uzupełnienie właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-225">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="19c7a-226">W poniższym przykładzie z przykładowej aplikacji Metoda `VerifyPhone` używa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> do walidacji parametru akcji `phone`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-226">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="19c7a-227">Węzły najwyższego poziomu mogą używać <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> z atrybutami walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-227">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="19c7a-228">W poniższym przykładzie z przykładowej aplikacji Metoda `CheckAge` określa, że parametr `age` musi być powiązany z ciągu zapytania, gdy formularz zostanie przesłany:</span><span class="sxs-lookup"><span data-stu-id="19c7a-228">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAgeSignature)]

<span data-ttu-id="19c7a-229">Na stronie sprawdzanie wieku (*Sprawdź. cshtml*) Istnieją dwa formy.</span><span class="sxs-lookup"><span data-stu-id="19c7a-229">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="19c7a-230">Pierwszy formularz przesyła `Age` wartość `99` jako parametr ciągu zapytania: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-230">The first form submits an `Age` value of `99` as a query string parameter: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="19c7a-231">Gdy zostanie przesłane prawidłowo sformatowany parametr `age` z ciągu zapytania, formularz zostanie sprawdzony.</span><span class="sxs-lookup"><span data-stu-id="19c7a-231">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="19c7a-232">Drugi formularz na stronie sprawdzanie wieku przesyła wartość `Age` w treści żądania, a Walidacja nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="19c7a-232">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="19c7a-233">Powiązanie nie powiodło się, ponieważ parametr `age` musi pochodzić z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="19c7a-233">Binding fails because the `age` parameter must come from a query string.</span></span>

## <a name="maximum-errors"></a><span data-ttu-id="19c7a-234">Maksymalna liczba błędów</span><span class="sxs-lookup"><span data-stu-id="19c7a-234">Maximum errors</span></span>

<span data-ttu-id="19c7a-235">Walidacja jest zatrzymywana, gdy zostanie osiągnięta maksymalna liczba błędów (domyślnie 200).</span><span class="sxs-lookup"><span data-stu-id="19c7a-235">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="19c7a-236">Tę liczbę można skonfigurować przy użyciu następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-236">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=4)]

## <a name="maximum-recursion"></a><span data-ttu-id="19c7a-237">Maksymalna rekursja</span><span class="sxs-lookup"><span data-stu-id="19c7a-237">Maximum recursion</span></span>

<span data-ttu-id="19c7a-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> przechodzące przez wykres obiektów z zweryfikowanym modelem.</span><span class="sxs-lookup"><span data-stu-id="19c7a-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="19c7a-239">W przypadku modeli, które są głębokie lub nieskończonie cykliczne, walidacja może spowodować przepełnienie stosu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-239">For models that are deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="19c7a-240">[MvcOptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) umożliwia szybkie zakończenie sprawdzania poprawności, Jeśli rekursja odwiedzających przekracza skonfigurowaną głębokość.</span><span class="sxs-lookup"><span data-stu-id="19c7a-240">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="19c7a-241">Wartość domyślna `MvcOptions.MaxValidationDepth` to 32.</span><span class="sxs-lookup"><span data-stu-id="19c7a-241">The default value of `MvcOptions.MaxValidationDepth` is 32.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="19c7a-242">Automatyczny krótki obwód</span><span class="sxs-lookup"><span data-stu-id="19c7a-242">Automatic short-circuit</span></span>

<span data-ttu-id="19c7a-243">Sprawdzanie poprawności jest automatycznie skracane (pomijane), jeśli wykres modelu nie wymaga weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-243">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="19c7a-244">Obiekty, dla których środowisko uruchomieniowe pomija sprawdzanie poprawności dla dołączania kolekcji elementów pierwotnych (takich jak `byte[]`, `string[]`, `Dictionary<string, string>`) i wykresy złożonego obiektu, które nie mają żadnych modułów walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-244">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="19c7a-245">Wyłącz weryfikację</span><span class="sxs-lookup"><span data-stu-id="19c7a-245">Disable validation</span></span>

<span data-ttu-id="19c7a-246">Aby wyłączyć weryfikację:</span><span class="sxs-lookup"><span data-stu-id="19c7a-246">To disable validation:</span></span>

1. <span data-ttu-id="19c7a-247">Utwórz implementację `IObjectModelValidator`, która nie oznacza żadnych pól jako nieprawidłowych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-247">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/NullObjectModelValidator.cs?name=snippet_Class)]

1. <span data-ttu-id="19c7a-248">Dodaj następujący kod do `Startup.ConfigureServices`, aby zastąpić domyślną implementację `IObjectModelValidator` w kontenerze iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="19c7a-248">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="19c7a-249">Nadal mogą pojawić się błędy stanu modelu pochodzące z powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-249">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="19c7a-250">Weryfikacja po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-250">Client-side validation</span></span>

<span data-ttu-id="19c7a-251">Weryfikacja po stronie klienta uniemożliwia przesyłanie, dopóki formularz nie będzie prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="19c7a-251">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="19c7a-252">Przycisk Prześlij uruchamia kod JavaScript, który przesyła formularz lub wyświetla komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="19c7a-252">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="19c7a-253">Weryfikacja po stronie klienta umożliwia uniknięcie niepotrzebnej komunikacji z serwerem w przypadku błędów danych wejściowych w formularzu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-253">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="19c7a-254">Następujący skrypt odwołuje się do *_Layout. cshtml* i *_ValidationScriptsPartial. cshtml* obsługuje walidację po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="19c7a-254">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_Scripts)]

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_Scripts)]

<span data-ttu-id="19c7a-255">Skrypt [niezauważalnego sprawdzania poprawności jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) jest niestandardową biblioteką frontonu firmy Microsoft, która kompiluje się w popularnej wersji interfejsu [jQuery](https://jqueryvalidation.org/) .</span><span class="sxs-lookup"><span data-stu-id="19c7a-255">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="19c7a-256">Bez dyskretnej weryfikacji jQuery należy wykonać kod tej samej logiki walidacji w dwóch miejscach: raz w atrybuty walidacji po stronie serwera we właściwościach modelu, a następnie ponownie w skryptach po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="19c7a-256">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="19c7a-257">Zamiast tego, [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) używają atrybutów walidacji oraz metadanych typu z właściwości modelu, aby renderować Tagi HTML 5 `data-` atrybuty dla elementów formularza, które wymagają walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-257">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="19c7a-258">niezauważalne sprawdzenie poprawności przez program jQuery umożliwia przeanalizowanie atrybutów `data-` i przekazanie logiki do programu jQuery Validate, efektywne "Kopiowanie" logiki walidacji po stronie serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="19c7a-258">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="19c7a-259">Błędy sprawdzania poprawności można wyświetlić na kliencie przy użyciu pomocników tagów, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="19c7a-259">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=3-4)]

<span data-ttu-id="19c7a-260">Poprzednie pomocnicy tagów renderują następujący kod HTML:</span><span class="sxs-lookup"><span data-stu-id="19c7a-260">The preceding tag helpers render the following HTML:</span></span>

```html
<div class="form-group">
    <label class="control-label" for="Movie_ReleaseDate">Release Date</label>
    <input class="form-control" type="date" data-val="true"
        data-val-required="The Release Date field is required."
        id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
    <span class="text-danger field-validation-valid"
        data-valmsg-for="Movie.ReleaseDate" data-valmsg-replace="true"></span>
</div>
```

<span data-ttu-id="19c7a-261">Zwróć uwagę, że atrybuty `data-` w danych wyjściowych HTML odpowiadają atrybutom walidacji właściwości `Movie.ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-261">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `Movie.ReleaseDate` property.</span></span> <span data-ttu-id="19c7a-262">Atrybut `data-val-required` zawiera komunikat o błędzie, który zostanie wyświetlony, jeśli użytkownik nie wypełni pola Data wydania.</span><span class="sxs-lookup"><span data-stu-id="19c7a-262">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="19c7a-263">niezauważalne sprawdzenie poprawności przez funkcję jQuery powoduje, że ta wartość jest przekazywana do walidacji elementu jQuery [()](https://jqueryvalidation.org/required-method/) , a następnie wyświetla ten komunikat w towarzyszącym **\<span >** .</span><span class="sxs-lookup"><span data-stu-id="19c7a-263">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="19c7a-264">Walidacja typu danych jest oparta na typie .NET właściwości, chyba że zostanie zastąpiona przez atrybut `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-264">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="19c7a-265">Przeglądarki mają własne domyślne komunikaty o błędach, ale pakietem weryfikacji jQuery nie dyskretnego sprawdzania poprawności może przesłonić te komunikaty.</span><span class="sxs-lookup"><span data-stu-id="19c7a-265">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="19c7a-266">`[DataType]` atrybuty i podklasy, takie jak `[EmailAddress]` pozwalają określić komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-266">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

## <a name="unobtrusive-validation"></a><span data-ttu-id="19c7a-267">Niezauważalna weryfikacja</span><span class="sxs-lookup"><span data-stu-id="19c7a-267">Unobtrusive validation</span></span>

<span data-ttu-id="19c7a-268">Aby uzyskać informacje o niezauważalnej weryfikacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/1111)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="19c7a-268">For information on unobtrusive validation, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/1111).</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="19c7a-269">Dodawanie walidacji do formularzy dynamicznych</span><span class="sxs-lookup"><span data-stu-id="19c7a-269">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="19c7a-270">w przypadku niedyskretnego sprawdzania poprawności jest sprawdzana logika walidacji i parametry do jQuery podczas pierwszego ładowania strony.</span><span class="sxs-lookup"><span data-stu-id="19c7a-270">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="19c7a-271">W związku z tym sprawdzanie poprawności nie działa automatycznie na formularzach generowanych dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-271">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="19c7a-272">Aby włączyć weryfikację, poinformuj jQuery o niezauważalnej weryfikacji, aby przeanalizować formularz dynamiczny bezpośrednio po jego utworzeniu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-272">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="19c7a-273">Na przykład poniższy kod konfiguruje walidację po stronie klienta w formularzu dodanym przez AJAX.</span><span class="sxs-lookup"><span data-stu-id="19c7a-273">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="19c7a-274">Metoda `$.validator.unobtrusive.parse()` akceptuje selektor jQuery dla jednego argumentu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-274">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="19c7a-275">Ta metoda informuje niedyskretną weryfikację jQuery, aby przeanalizować `data-` atrybuty formularzy w ramach tego selektora.</span><span class="sxs-lookup"><span data-stu-id="19c7a-275">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="19c7a-276">Wartości tych atrybutów są następnie przesyłane do wtyczki do walidacji jQuery.</span><span class="sxs-lookup"><span data-stu-id="19c7a-276">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="19c7a-277">Dodawanie walidacji do formantów dynamicznych</span><span class="sxs-lookup"><span data-stu-id="19c7a-277">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="19c7a-278">Metoda `$.validator.unobtrusive.parse()` działa na całym formularzu, a nie na poszczególnych dynamicznie generowanych kontrolkach, takich jak `<input>` i `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-278">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="19c7a-279">Aby przeanalizować formularz, Usuń dane sprawdzania poprawności, które zostały dodane, gdy formularz został wcześniej przeanalizowany, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-279">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="19c7a-280">Niestandardowe sprawdzanie poprawności po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-280">Custom client-side validation</span></span>

<span data-ttu-id="19c7a-281">Niestandardowe sprawdzanie poprawności po stronie klienta jest wykonywane przez wygenerowanie `data-` atrybutów HTML, które działają z niestandardowym identyfikatorem platformy jQuery.</span><span class="sxs-lookup"><span data-stu-id="19c7a-281">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="19c7a-282">Następujący przykładowy kod adaptera został zapisany dla atrybutów `[ClassicMovie]` i `[ClassicMovieWithClientValidator]`, które zostały wprowadzone we wcześniejszej części tego artykułu:</span><span class="sxs-lookup"><span data-stu-id="19c7a-282">The following sample adapter code was written for the `[ClassicMovie]` and `[ClassicMovieWithClientValidator]` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/3.x/ValidationSample/wwwroot/js/classicMovieValidator.js)]

<span data-ttu-id="19c7a-283">Aby uzyskać informacje o sposobach pisania kart sieciowych, zobacz [dokumentację dotyczącą platformy jQuery Validate](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="19c7a-283">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="19c7a-284">Użycie karty dla danego pola jest wyzwalane przez `data-` atrybuty, które:</span><span class="sxs-lookup"><span data-stu-id="19c7a-284">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="19c7a-285">Oznacz pole jako podlegające weryfikacji (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="19c7a-285">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="19c7a-286">Zidentyfikuj nazwę reguły walidacji i tekst komunikatu o błędzie (na przykład `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="19c7a-286">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="19c7a-287">Podaj wszelkie dodatkowe parametry wymagane przez moduł walidacji (na przykład `data-val-rulename-param1="value"`).</span><span class="sxs-lookup"><span data-stu-id="19c7a-287">Provide any additional parameters the validator needs (for example, `data-val-rulename-param1="value"`).</span></span>

<span data-ttu-id="19c7a-288">W poniższym przykładzie pokazano `data-` atrybuty `ClassicMovie` atrybutu przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="19c7a-288">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="date"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year no later than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The Release Date field is required."
    id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
```

<span data-ttu-id="19c7a-289">Jak wspomniano wcześniej, [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) używają informacji z atrybutów walidacji do renderowania atrybutów `data-`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-289">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="19c7a-290">Dostępne są dwie opcje pisania kodu, które powoduje utworzenie niestandardowych atrybutów HTML `data-`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-290">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="19c7a-291">Utwórz klasę, która pochodzi od `AttributeAdapterBase<TAttribute>` i klasy implementującej `IValidationAttributeAdapterProvider`, i Zarejestruj swój atrybut i jego kartę w programie DI.</span><span class="sxs-lookup"><span data-stu-id="19c7a-291">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="19c7a-292">Ta metoda jest zgodna z [pojedynczym podmiotem odpowiedzialnym](https://wikipedia.org/wiki/Single_responsibility_principle) w odniesieniu do kodu weryfikacyjnego związanego z serwerem i klienta jest w osobnych klasach.</span><span class="sxs-lookup"><span data-stu-id="19c7a-292">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="19c7a-293">Adapter ma również zalety, że ponieważ jest on zarejestrowany w programie DI, w razie potrzeby są dostępne inne usługi w programie DI.</span><span class="sxs-lookup"><span data-stu-id="19c7a-293">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="19c7a-294">Zaimplementuj `IClientModelValidator` w klasie `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-294">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="19c7a-295">Ta metoda może być odpowiednia, jeśli atrybut nie wykonuje walidacji po stronie serwera i nie wymaga żadnych usług z programu DI.</span><span class="sxs-lookup"><span data-stu-id="19c7a-295">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="19c7a-296">AttributeAdapter dla weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-296">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="19c7a-297">Ta metoda renderowania atrybutów `data-` w kodzie HTML jest używana przez atrybut `ClassicMovie` w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-297">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="19c7a-298">Aby dodać weryfikację klienta przy użyciu tej metody:</span><span class="sxs-lookup"><span data-stu-id="19c7a-298">To add client validation by using this method:</span></span>

1. <span data-ttu-id="19c7a-299">Utwórz klasę adaptera atrybutów dla niestandardowego atrybutu walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-299">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="19c7a-300">Utwórz klasę z [AttributeAdapterBase\<t >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="19c7a-300">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="19c7a-301">Utwórz metodę `AddValidation`, która dodaje atrybuty `data-` do renderowanych danych wyjściowych, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-301">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttributeAdapter.cs?name=snippet_Class)]

1. <span data-ttu-id="19c7a-302">Utwórz klasę dostawcy kart implementującą <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="19c7a-302">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="19c7a-303">W metodzie `GetAttributeAdapter` Przekaż atrybut niestandardowy do konstruktora adaptera, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-303">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/CustomValidationAttributeAdapterProvider.cs?name=snippet_Class)]

1. <span data-ttu-id="19c7a-304">Zarejestruj dostawcę adaptera dla `Startup.ConfigureServices`DI in:</span><span class="sxs-lookup"><span data-stu-id="19c7a-304">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=9-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="19c7a-305">IClientModelValidator dla weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-305">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="19c7a-306">Ta metoda renderowania atrybutów `data-` w kodzie HTML jest używana przez atrybut `ClassicMovieWithClientValidator` w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-306">This method of rendering `data-` attributes in HTML is used by the `ClassicMovieWithClientValidator` attribute in the sample app.</span></span> <span data-ttu-id="19c7a-307">Aby dodać weryfikację klienta przy użyciu tej metody:</span><span class="sxs-lookup"><span data-stu-id="19c7a-307">To add client validation by using this method:</span></span>

* <span data-ttu-id="19c7a-308">W atrybucie niestandardowego sprawdzania poprawności Zaimplementuj interfejs `IClientModelValidator` i Utwórz metodę `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-308">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="19c7a-309">W metodzie `AddValidation` Dodaj atrybuty `data-` do walidacji, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-309">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieWithClientValidatorAttribute.cs?name=snippet_Class)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="19c7a-310">Wyłącz weryfikację po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-310">Disable client-side validation</span></span>

<span data-ttu-id="19c7a-311">Poniższy kod wyłącza weryfikację klienta w Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="19c7a-311">The following code disables client validation in Razor Pages:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableClientValidation&highlight=2-5)]

<span data-ttu-id="19c7a-312">Inne opcje wyłączenia weryfikacji po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="19c7a-312">Other options to disable client-side validation:</span></span>

* <span data-ttu-id="19c7a-313">Dodaj komentarz do odwołania do `_ValidationScriptsPartial` we wszystkich plikach *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="19c7a-313">Comment out the reference to `_ValidationScriptsPartial` in all the *.cshtml* files.</span></span>
* <span data-ttu-id="19c7a-314">Usuń zawartość pliku *Pages\Shared\_ValidationScriptsPartial. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="19c7a-314">Remove the contents of the *Pages\Shared\_ValidationScriptsPartial.cshtml* file.</span></span>

<span data-ttu-id="19c7a-315">Poprzednie podejście nie zapobiega weryfikacji po stronie klienta w bibliotece klas Razor ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="19c7a-315">The preceding approach won't prevent client side validation of ASP.NET Core Identity Razor Class Library.</span></span> <span data-ttu-id="19c7a-316">Aby uzyskać więcej informacji, zobacz temat <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="19c7a-316">For more information, see <xref:security/authentication/scaffold-identity>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="19c7a-317">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="19c7a-317">Additional resources</span></span>

* [<span data-ttu-id="19c7a-318">Przestrzeń nazw System. ComponentModel. DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="19c7a-318">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="19c7a-319">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="19c7a-319">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="19c7a-320">W tym artykule wyjaśniono, jak sprawdzić poprawność danych wprowadzonych przez użytkownika w aplikacji ASP.NET Core MVC lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="19c7a-320">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="19c7a-321">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="19c7a-321">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="19c7a-322">Stan modelu</span><span class="sxs-lookup"><span data-stu-id="19c7a-322">Model state</span></span>

<span data-ttu-id="19c7a-323">Stan modelu reprezentuje błędy pochodzące z dwóch podsystemów: powiązanie modelu i walidacja modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-323">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="19c7a-324">Błędy, które pochodzą z [powiązania modelu](model-binding.md) , to zwykle Błędy konwersji danych (na przykład "x" jest wprowadzany w polu, w którym jest oczekiwana liczba całkowita).</span><span class="sxs-lookup"><span data-stu-id="19c7a-324">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="19c7a-325">Walidacja modelu odbywa się po powiązaniu modelu i zgłasza błędy, gdy dane nie są zgodne z regułami biznesowymi (na przykład wartość 0 jest wprowadzana w polu, w którym oczekiwana jest Ocena z zakresu od 1 do 5).</span><span class="sxs-lookup"><span data-stu-id="19c7a-325">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="19c7a-326">Zarówno powiązanie modelu, jak i walidacja są wykonywane przed wykonaniem akcji kontrolera lub metody obsługi Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="19c7a-326">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="19c7a-327">W przypadku aplikacji sieci Web jest odpowiedzialna za to, aby aplikacja była odpowiednio sprawdzana `ModelState.IsValid` i reagować.</span><span class="sxs-lookup"><span data-stu-id="19c7a-327">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="19c7a-328">Aplikacja internetowa zazwyczaj ponownie wyświetla stronę z komunikatem o błędzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-328">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="19c7a-329">Kontrolery interfejsu API sieci Web nie muszą sprawdzać `ModelState.IsValid`, jeśli mają atrybut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-329">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="19c7a-330">W takim przypadku automatyczna odpowiedź HTTP 400 zawierająca szczegóły błędu jest zwracana, gdy stan modelu jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="19c7a-330">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="19c7a-331">Aby uzyskać więcej informacji, zobacz [Automatyczne HTTP 400 odpowiedzi](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="19c7a-331">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="19c7a-332">Uruchom ponownie weryfikację</span><span class="sxs-lookup"><span data-stu-id="19c7a-332">Rerun validation</span></span>

<span data-ttu-id="19c7a-333">Walidacja jest automatyczna, ale warto powtórzyć ją ręcznie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-333">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="19c7a-334">Na przykład można obliczyć wartość właściwości i chcieć ponownie uruchomić weryfikację po ustawieniu właściwości na wartość obliczaną.</span><span class="sxs-lookup"><span data-stu-id="19c7a-334">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="19c7a-335">Aby ponownie uruchomić weryfikację, wywołaj metodę `TryValidateModel`, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="19c7a-335">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="19c7a-336">Atrybuty walidacji</span><span class="sxs-lookup"><span data-stu-id="19c7a-336">Validation attributes</span></span>

<span data-ttu-id="19c7a-337">Atrybuty walidacji umożliwiają określanie reguł walidacji dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-337">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="19c7a-338">W poniższym przykładzie z [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) przedstawiono klasę modelu, która ma adnotację z atrybutami walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-338">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="19c7a-339">Atrybut `[ClassicMovie]` jest niestandardowym atrybutem walidacji, a inne są wbudowane.</span><span class="sxs-lookup"><span data-stu-id="19c7a-339">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="19c7a-340">Niepokazywany jest `[ClassicMovie2]`, który pokazuje alternatywny sposób implementacji atrybutu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="19c7a-340">Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="19c7a-341">Atrybuty wbudowane</span><span class="sxs-lookup"><span data-stu-id="19c7a-341">Built-in attributes</span></span>

<span data-ttu-id="19c7a-342">Wbudowane atrybuty walidacji obejmują:</span><span class="sxs-lookup"><span data-stu-id="19c7a-342">Built-in validation attributes include:</span></span>

* <span data-ttu-id="19c7a-343">`[CreditCard]`: sprawdza, czy właściwość ma format karty kredytowej.</span><span class="sxs-lookup"><span data-stu-id="19c7a-343">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="19c7a-344">`[Compare]`: sprawdza, czy dwie właściwości w modelu pasują do siebie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-344">`[Compare]`: Validates that two properties in a model match.</span></span> <span data-ttu-id="19c7a-345">Na przykład plik *register.cshtml.cs* używa `[Compare]` do sprawdzania poprawności dwóch wprowadzonych haseł.</span><span class="sxs-lookup"><span data-stu-id="19c7a-345">For example, the *Register.cshtml.cs* file uses `[Compare]` to validate the two entered passwords match.</span></span> <span data-ttu-id="19c7a-346">[Tożsamość szkieletu](xref:security/authentication/scaffold-identity) do wyświetlania kodu rejestru.</span><span class="sxs-lookup"><span data-stu-id="19c7a-346">[Scaffold Identity](xref:security/authentication/scaffold-identity) to see the Register code.</span></span>
* <span data-ttu-id="19c7a-347">`[EmailAddress]`: sprawdza, czy właściwość ma format wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="19c7a-347">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="19c7a-348">`[Phone]`: sprawdza, czy właściwość ma format numeru telefonu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-348">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="19c7a-349">`[Range]`: sprawdza, czy wartość właściwości znajduje się w określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-349">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="19c7a-350">`[RegularExpression]`: sprawdza, czy wartość właściwości jest zgodna z określonym wyrażeniem regularnym.</span><span class="sxs-lookup"><span data-stu-id="19c7a-350">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="19c7a-351">`[Required]`: sprawdza, czy pole nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-351">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="19c7a-352">Zobacz [`[Required]` atrybutu](#required-attribute) , aby uzyskać szczegółowe informacje o zachowaniu tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-352">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="19c7a-353">`[StringLength]`: sprawdza, czy wartość właściwości String nie przekracza podanego limitu długości.</span><span class="sxs-lookup"><span data-stu-id="19c7a-353">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="19c7a-354">`[Url]`: sprawdza, czy właściwość ma format adresu URL.</span><span class="sxs-lookup"><span data-stu-id="19c7a-354">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="19c7a-355">`[Remote]`: sprawdza poprawność danych wejściowych na kliencie przez wywołanie metody akcji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="19c7a-355">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="19c7a-356">Zobacz [`[Remote]` atrybutu](#remote-attribute) , aby uzyskać szczegółowe informacje o zachowaniu tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-356">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="19c7a-357">W przypadku korzystania z atrybutu `[RegularExpression]` z walidacją po stronie klienta wyrażenie regularne jest wykonywane w języku JavaScript na kliencie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-357">When using the `[RegularExpression]` attribute with client-side validation, the regex is executed in JavaScript on the client.</span></span> <span data-ttu-id="19c7a-358">Oznacza to, że będzie używane zachowanie zgodne ze standardem [ECMAScript](/dotnet/standard/base-types/regular-expression-options#ecmascript-matching-behavior) .</span><span class="sxs-lookup"><span data-stu-id="19c7a-358">This means [ECMAScript](/dotnet/standard/base-types/regular-expression-options#ecmascript-matching-behavior) matching behavior will be used.</span></span> <span data-ttu-id="19c7a-359">Aby uzyskać więcej informacji, zobacz [problem w usłudze GitHub](https://github.com/dotnet/corefx/issues/42487).</span><span class="sxs-lookup"><span data-stu-id="19c7a-359">For more information, see [this GitHub issue](https://github.com/dotnet/corefx/issues/42487).</span></span>

<span data-ttu-id="19c7a-360">Pełną listę atrybutów sprawdzania poprawności można znaleźć w przestrzeni nazw [System. ComponentModel. DataAnnotations](xref:System.ComponentModel.DataAnnotations) .</span><span class="sxs-lookup"><span data-stu-id="19c7a-360">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="19c7a-361">Komunikaty o błędach</span><span class="sxs-lookup"><span data-stu-id="19c7a-361">Error messages</span></span>

<span data-ttu-id="19c7a-362">Atrybuty walidacji pozwalają określić komunikat o błędzie, który ma być wyświetlany dla nieprawidłowych danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-362">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="19c7a-363">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="19c7a-363">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="19c7a-364">Wewnętrznie atrybuty wywołują `String.Format` z symbolem zastępczym dla nazwy pola i czasami dodatkowych symboli zastępczych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-364">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="19c7a-365">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="19c7a-365">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="19c7a-366">W przypadku zastosowania do właściwości `Name` komunikat o błędzie utworzony przez poprzedni kod powinien mieć wartość "nazwa musi zawierać się w przedziale od 6 do 8".</span><span class="sxs-lookup"><span data-stu-id="19c7a-366">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="19c7a-367">Aby dowiedzieć się, które parametry są przesyłane do `String.Format` dla komunikatu o błędzie określonego atrybutu, zobacz [kod źródłowy adnotacji](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations)danych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-367">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="19c7a-368">[Required] — atrybut</span><span class="sxs-lookup"><span data-stu-id="19c7a-368">[Required] attribute</span></span>

<span data-ttu-id="19c7a-369">Domyślnie system walidacji traktuje niedopuszczające wartości null parametry lub właściwości tak, jakby miały atrybut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-369">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="19c7a-370">[Typy wartości](/dotnet/csharp/language-reference/keywords/value-types) , takie jak `decimal` i `int`, nie dopuszczają wartości null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-370">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="19c7a-371">[Wymagane] Walidacja na serwerze</span><span class="sxs-lookup"><span data-stu-id="19c7a-371">[Required] validation on the server</span></span>

<span data-ttu-id="19c7a-372">Na serwerze, wymagana wartość jest uważana za brakującą, jeśli właściwość ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-372">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="19c7a-373">Pole niedopuszczające wartości null jest zawsze prawidłowe, a komunikat o błędzie [Required] nie jest nigdy wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="19c7a-373">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="19c7a-374">Jednak powiązanie modelu dla właściwości niedopuszczających wartości null może zakończyć się niepowodzeniem, co spowoduje, że zostanie wyświetlony komunikat o błędzie, taki jak `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-374">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="19c7a-375">Aby określić niestandardowy komunikat o błędzie dla weryfikacji po stronie serwera dla typów niedopuszczających wartości null, dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="19c7a-375">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="19c7a-376">Ustaw pole jako dopuszczające wartość null (na przykład `decimal?` zamiast `decimal`).</span><span class="sxs-lookup"><span data-stu-id="19c7a-376">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="19c7a-377">[Dopuszczane wartości null\<t >](/dotnet/csharp/programming-guide/nullable-types/) typy wartości są traktowane jak standardowe typy dopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-377">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="19c7a-378">Określ domyślny komunikat o błędzie, który ma być używany przez powiązanie modelu, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-378">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="19c7a-379">Aby uzyskać więcej informacji o błędach powiązania modelu, dla których można ustawić domyślne komunikaty dla programu, zobacz <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="19c7a-379">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="19c7a-380">[Wymagane] weryfikacja na kliencie</span><span class="sxs-lookup"><span data-stu-id="19c7a-380">[Required] validation on the client</span></span>

<span data-ttu-id="19c7a-381">Typy niedopuszczające wartości null i ciągi są obsługiwane inaczej na kliencie w porównaniu z serwerem.</span><span class="sxs-lookup"><span data-stu-id="19c7a-381">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="19c7a-382">Na kliencie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-382">On the client:</span></span>

* <span data-ttu-id="19c7a-383">Wartość jest uważana za obecną tylko wtedy, gdy wprowadzono dla niej dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-383">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="19c7a-384">W związku z tym, walidacja po stronie klienta obsługuje niedopuszczające wartości null typy takie same jak typy dopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="19c7a-384">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="19c7a-385">Biały znak w polu ciągu jest uznawany za prawidłowe dane wejściowe przez [wymaganą](https://jqueryvalidation.org/required-method/) metodę weryfikacji jQuery.</span><span class="sxs-lookup"><span data-stu-id="19c7a-385">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="19c7a-386">Walidacja po stronie serwera traktuje wymagane pole ciągu nieprawidłowe, jeśli wprowadzono tylko odstępy.</span><span class="sxs-lookup"><span data-stu-id="19c7a-386">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="19c7a-387">Jak wspomniano wcześniej, typy niedopuszczające wartości null są traktowane jako chociaż mają atrybut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-387">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="19c7a-388">Oznacza to, że można uzyskać weryfikację po stronie klienta, nawet jeśli nie zastosowano atrybutu `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-388">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="19c7a-389">Ale jeśli nie używasz tego atrybutu, zostanie wyświetlony domyślny komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-389">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="19c7a-390">Aby określić niestandardowy komunikat o błędzie, Użyj atrybutu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-390">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="19c7a-391">[Remote] — atrybut</span><span class="sxs-lookup"><span data-stu-id="19c7a-391">[Remote] attribute</span></span>

<span data-ttu-id="19c7a-392">Atrybut `[Remote]` implementuje walidację po stronie klienta, która wymaga wywołania metody na serwerze w celu określenia, czy dane wejściowe pola są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-392">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="19c7a-393">Na przykład aplikacja może wymagać sprawdzenia, czy nazwa użytkownika jest już używana.</span><span class="sxs-lookup"><span data-stu-id="19c7a-393">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="19c7a-394">Aby zaimplementować zdalne sprawdzanie poprawności:</span><span class="sxs-lookup"><span data-stu-id="19c7a-394">To implement remote validation:</span></span>

1. <span data-ttu-id="19c7a-395">Utwórz metodę akcji dla języka JavaScript do wywołania.</span><span class="sxs-lookup"><span data-stu-id="19c7a-395">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="19c7a-396">Metoda [zdalna](https://jqueryvalidation.org/remote-method/) walidacji jQuery oczekuje odpowiedzi JSON:</span><span class="sxs-lookup"><span data-stu-id="19c7a-396">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="19c7a-397">`"true"` oznacza, że dane wejściowe są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-397">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="19c7a-398">`"false"`, `undefined`lub `null` oznacza, że dane wejściowe są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-398">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="19c7a-399">Wyświetl domyślny komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-399">Display the default error message.</span></span>
   * <span data-ttu-id="19c7a-400">Każdy inny ciąg oznacza, że dane wejściowe są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="19c7a-400">Any other string means the input is invalid.</span></span> <span data-ttu-id="19c7a-401">Wyświetl ciąg jako niestandardowy komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-401">Display the string as a custom error message.</span></span>

   <span data-ttu-id="19c7a-402">Oto przykład metody akcji, która zwraca niestandardowy komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-402">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="19c7a-403">W klasie modelu Dodaj adnotację do właściwości z atrybutem `[Remote]`, który wskazuje na metodę akcji walidacji, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-403">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="19c7a-404">Atrybut `[Remote]` znajduje się w przestrzeni nazw `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-404">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="19c7a-405">Zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) , jeśli nie używasz pakietu `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-405">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="19c7a-406">Dodatkowe pola</span><span class="sxs-lookup"><span data-stu-id="19c7a-406">Additional fields</span></span>

<span data-ttu-id="19c7a-407">Właściwość `AdditionalFields` atrybutu `[Remote]` umożliwia Weryfikowanie kombinacji pól względem danych na serwerze.</span><span class="sxs-lookup"><span data-stu-id="19c7a-407">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="19c7a-408">Na przykład jeśli model `User` ma `FirstName` i `LastName` właściwości, warto sprawdzić, czy żaden istniejący użytkownik ma już tę parę nazw.</span><span class="sxs-lookup"><span data-stu-id="19c7a-408">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="19c7a-409">Poniższy przykład pokazuje, jak używać `AdditionalFields`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-409">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="19c7a-410">`AdditionalFields` można jawnie ustawić dla ciągów `"FirstName"` i `"LastName"`, ale użycie operatora [nameof](/dotnet/csharp/language-reference/keywords/nameof) upraszcza późniejsze refaktoryzacje.</span><span class="sxs-lookup"><span data-stu-id="19c7a-410">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="19c7a-411">Metoda akcji dla tej weryfikacji musi akceptować zarówno imiona, jak i nazwiska:</span><span class="sxs-lookup"><span data-stu-id="19c7a-411">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="19c7a-412">Gdy użytkownik wprowadzi imię lub nazwisko, kod JavaScript wykonuje zdalne wywołanie, aby sprawdzić, czy ta para nazw została podjęta.</span><span class="sxs-lookup"><span data-stu-id="19c7a-412">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="19c7a-413">Aby sprawdzić poprawność dwóch lub więcej pól, podaj je jako listę rozdzielaną przecinkami.</span><span class="sxs-lookup"><span data-stu-id="19c7a-413">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="19c7a-414">Na przykład aby dodać właściwość `MiddleName` do modelu, należy ustawić atrybut `[Remote]`, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-414">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="19c7a-415">`AdditionalFields`, podobnie jak wszystkie argumenty atrybutu, musi być wyrażeniem stałym.</span><span class="sxs-lookup"><span data-stu-id="19c7a-415">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="19c7a-416">W związku z tym nie należy używać [interpolowanego ciągu](/dotnet/csharp/language-reference/keywords/interpolated-strings) ani <xref:System.String.Join*> wywołań, aby zainicjować `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-416">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="19c7a-417">Alternatywy dla wbudowanych atrybutów</span><span class="sxs-lookup"><span data-stu-id="19c7a-417">Alternatives to built-in attributes</span></span>

<span data-ttu-id="19c7a-418">Jeśli potrzebujesz weryfikacji niedostarczonej przez wbudowane atrybuty, możesz:</span><span class="sxs-lookup"><span data-stu-id="19c7a-418">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="19c7a-419">[Tworzenie atrybutów niestandardowych](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="19c7a-419">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="19c7a-420">[Zaimplementuj IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="19c7a-420">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="19c7a-421">Atrybuty niestandardowe</span><span class="sxs-lookup"><span data-stu-id="19c7a-421">Custom attributes</span></span>

<span data-ttu-id="19c7a-422">W przypadku scenariuszy, w których wbudowane atrybuty walidacji nie obsługują, można utworzyć niestandardowe atrybuty walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-422">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="19c7a-423">Utwórz klasę, która dziedziczy po <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, i Zastąp metodę <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="19c7a-423">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="19c7a-424">Metoda `IsValid` akceptuje obiekt o nazwie *Value*, czyli dane wejściowe do zweryfikowania.</span><span class="sxs-lookup"><span data-stu-id="19c7a-424">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="19c7a-425">Przeciążenie akceptuje również obiekt `ValidationContext`, który zawiera dodatkowe informacje, takie jak wystąpienie modelu utworzone przez powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-425">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="19c7a-426">Poniższy przykład sprawdza, czy Data wydania filmu w *klasycznym* gatunku nie jest późniejsza niż określony rok.</span><span class="sxs-lookup"><span data-stu-id="19c7a-426">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="19c7a-427">Atrybut `[ClassicMovie2]` sprawdza najpierw gatunek i kontynuuje działanie tylko wtedy, gdy jest on *klasyczny*.</span><span class="sxs-lookup"><span data-stu-id="19c7a-427">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="19c7a-428">W przypadku filmów identyfikowanych jako Classics sprawdza datę wydania, aby upewnić się, że nie jest ona późniejsza niż limit przesłany do konstruktora atrybutu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-428">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="19c7a-429">Zmienna `movie` w poprzednim przykładzie reprezentuje obiekt `Movie`, który zawiera dane z przesłania formularza.</span><span class="sxs-lookup"><span data-stu-id="19c7a-429">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="19c7a-430">Metoda `IsValid` sprawdza datę i gatunek.</span><span class="sxs-lookup"><span data-stu-id="19c7a-430">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="19c7a-431">Po pomyślnej weryfikacji `IsValid` zwraca kod `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-431">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="19c7a-432">Gdy Walidacja nie powiedzie się, zostanie zwrócony `ValidationResult` z komunikatem o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-432">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="19c7a-433">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="19c7a-433">IValidatableObject</span></span>

<span data-ttu-id="19c7a-434">Poprzedni przykład działa tylko z typami `Movie`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-434">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="19c7a-435">Inną opcją weryfikacji na poziomie klasy jest implementacja `IValidatableObject` w klasie modelu, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-435">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="19c7a-436">Sprawdzanie poprawności węzła najwyższego poziomu</span><span class="sxs-lookup"><span data-stu-id="19c7a-436">Top-level node validation</span></span>

<span data-ttu-id="19c7a-437">Węzły najwyższego poziomu obejmują:</span><span class="sxs-lookup"><span data-stu-id="19c7a-437">Top-level nodes include:</span></span>

* <span data-ttu-id="19c7a-438">Parametry akcji</span><span class="sxs-lookup"><span data-stu-id="19c7a-438">Action parameters</span></span>
* <span data-ttu-id="19c7a-439">Właściwości kontrolera</span><span class="sxs-lookup"><span data-stu-id="19c7a-439">Controller properties</span></span>
* <span data-ttu-id="19c7a-440">Parametry procedury obsługi stron</span><span class="sxs-lookup"><span data-stu-id="19c7a-440">Page handler parameters</span></span>
* <span data-ttu-id="19c7a-441">Właściwości modelu strony</span><span class="sxs-lookup"><span data-stu-id="19c7a-441">Page model properties</span></span>

<span data-ttu-id="19c7a-442">Węzły najwyższego poziomu powiązane z modelem są weryfikowane jako uzupełnienie właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-442">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="19c7a-443">W poniższym przykładzie z przykładowej aplikacji Metoda `VerifyPhone` używa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> do walidacji parametru akcji `phone`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-443">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="19c7a-444">Węzły najwyższego poziomu mogą używać <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> z atrybutami walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-444">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="19c7a-445">W poniższym przykładzie z przykładowej aplikacji Metoda `CheckAge` określa, że parametr `age` musi być powiązany z ciągu zapytania, gdy formularz zostanie przesłany:</span><span class="sxs-lookup"><span data-stu-id="19c7a-445">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="19c7a-446">Na stronie sprawdzanie wieku (*Sprawdź. cshtml*) Istnieją dwa formy.</span><span class="sxs-lookup"><span data-stu-id="19c7a-446">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="19c7a-447">Pierwszy formularz przesyła `Age` wartość `99` jako ciąg zapytania: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-447">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="19c7a-448">Gdy zostanie przesłane prawidłowo sformatowany parametr `age` z ciągu zapytania, formularz zostanie sprawdzony.</span><span class="sxs-lookup"><span data-stu-id="19c7a-448">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="19c7a-449">Drugi formularz na stronie sprawdzanie wieku przesyła wartość `Age` w treści żądania, a Walidacja nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="19c7a-449">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="19c7a-450">Powiązanie nie powiodło się, ponieważ parametr `age` musi pochodzić z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="19c7a-450">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="19c7a-451">W przypadku korzystania z programu z `CompatibilityVersion.Version_2_1` lub nowszym Walidacja węzła najwyższego poziomu jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="19c7a-451">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="19c7a-452">W przeciwnym razie Walidacja węzła najwyższego poziomu jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="19c7a-452">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="19c7a-453">Opcję domyślną można przesłonić, ustawiając właściwość <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> w (`Startup.ConfigureServices`), jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="19c7a-453">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="19c7a-454">Maksymalna liczba błędów</span><span class="sxs-lookup"><span data-stu-id="19c7a-454">Maximum errors</span></span>

<span data-ttu-id="19c7a-455">Walidacja jest zatrzymywana, gdy zostanie osiągnięta maksymalna liczba błędów (domyślnie 200).</span><span class="sxs-lookup"><span data-stu-id="19c7a-455">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="19c7a-456">Tę liczbę można skonfigurować przy użyciu następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-456">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="19c7a-457">Maksymalna rekursja</span><span class="sxs-lookup"><span data-stu-id="19c7a-457">Maximum recursion</span></span>

<span data-ttu-id="19c7a-458"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> przechodzące przez wykres obiektów z zweryfikowanym modelem.</span><span class="sxs-lookup"><span data-stu-id="19c7a-458"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="19c7a-459">W przypadku modeli, które są bardzo głębokie lub nieskończonie cykliczne, walidacja może spowodować przepełnienie stosu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-459">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="19c7a-460">[MvcOptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) umożliwia szybkie zakończenie sprawdzania poprawności, Jeśli rekursja odwiedzających przekracza skonfigurowaną głębokość.</span><span class="sxs-lookup"><span data-stu-id="19c7a-460">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="19c7a-461">Domyślna wartość `MvcOptions.MaxValidationDepth` to 32 w przypadku uruchamiania z `CompatibilityVersion.Version_2_2` lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="19c7a-461">The default value of `MvcOptions.MaxValidationDepth` is 32 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="19c7a-462">W przypadku wcześniejszych wersji wartość jest równa null, co oznacza brak ograniczenia głębokości.</span><span class="sxs-lookup"><span data-stu-id="19c7a-462">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="19c7a-463">Automatyczny krótki obwód</span><span class="sxs-lookup"><span data-stu-id="19c7a-463">Automatic short-circuit</span></span>

<span data-ttu-id="19c7a-464">Sprawdzanie poprawności jest automatycznie skracane (pomijane), jeśli wykres modelu nie wymaga weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-464">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="19c7a-465">Obiekty, dla których środowisko uruchomieniowe pomija sprawdzanie poprawności dla dołączania kolekcji elementów pierwotnych (takich jak `byte[]`, `string[]`, `Dictionary<string, string>`) i wykresy złożonego obiektu, które nie mają żadnych modułów walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-465">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="19c7a-466">Wyłącz weryfikację</span><span class="sxs-lookup"><span data-stu-id="19c7a-466">Disable validation</span></span>

<span data-ttu-id="19c7a-467">Aby wyłączyć weryfikację:</span><span class="sxs-lookup"><span data-stu-id="19c7a-467">To disable validation:</span></span>

1. <span data-ttu-id="19c7a-468">Utwórz implementację `IObjectModelValidator`, która nie oznacza żadnych pól jako nieprawidłowych.</span><span class="sxs-lookup"><span data-stu-id="19c7a-468">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="19c7a-469">Dodaj następujący kod do `Startup.ConfigureServices`, aby zastąpić domyślną implementację `IObjectModelValidator` w kontenerze iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="19c7a-469">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="19c7a-470">Nadal mogą pojawić się błędy stanu modelu pochodzące z powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-470">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="19c7a-471">Weryfikacja po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-471">Client-side validation</span></span>

<span data-ttu-id="19c7a-472">Weryfikacja po stronie klienta uniemożliwia przesyłanie, dopóki formularz nie będzie prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="19c7a-472">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="19c7a-473">Przycisk Prześlij uruchamia kod JavaScript, który przesyła formularz lub wyświetla komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="19c7a-473">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="19c7a-474">Weryfikacja po stronie klienta umożliwia uniknięcie niepotrzebnej komunikacji z serwerem w przypadku błędów danych wejściowych w formularzu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-474">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="19c7a-475">Następujący skrypt odwołuje się do *_Layout. cshtml* i *_ValidationScriptsPartial. cshtml* obsługuje walidację po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="19c7a-475">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="19c7a-476">Skrypt [niezauważalnego sprawdzania poprawności jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) jest niestandardową biblioteką frontonu firmy Microsoft, która kompiluje się w popularnej wersji interfejsu [jQuery](https://jqueryvalidation.org/) .</span><span class="sxs-lookup"><span data-stu-id="19c7a-476">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="19c7a-477">Bez dyskretnej weryfikacji jQuery należy wykonać kod tej samej logiki walidacji w dwóch miejscach: raz w atrybuty walidacji po stronie serwera we właściwościach modelu, a następnie ponownie w skryptach po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="19c7a-477">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="19c7a-478">Zamiast tego, [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) używają atrybutów walidacji oraz metadanych typu z właściwości modelu, aby renderować Tagi HTML 5 `data-` atrybuty dla elementów formularza, które wymagają walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-478">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="19c7a-479">niezauważalne sprawdzenie poprawności przez program jQuery umożliwia przeanalizowanie atrybutów `data-` i przekazanie logiki do programu jQuery Validate, efektywne "Kopiowanie" logiki walidacji po stronie serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="19c7a-479">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="19c7a-480">Błędy sprawdzania poprawności można wyświetlić na kliencie przy użyciu pomocników tagów, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="19c7a-480">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="19c7a-481">Poprzednie pomocnicy tagów renderują następujący kod HTML.</span><span class="sxs-lookup"><span data-stu-id="19c7a-481">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="19c7a-482">Zwróć uwagę, że atrybuty `data-` w danych wyjściowych HTML odpowiadają atrybutom walidacji właściwości `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-482">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="19c7a-483">Atrybut `data-val-required` zawiera komunikat o błędzie, który zostanie wyświetlony, jeśli użytkownik nie wypełni pola Data wydania.</span><span class="sxs-lookup"><span data-stu-id="19c7a-483">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="19c7a-484">niezauważalne sprawdzenie poprawności przez funkcję jQuery powoduje, że ta wartość jest przekazywana do walidacji elementu jQuery [()](https://jqueryvalidation.org/required-method/) , a następnie wyświetla ten komunikat w towarzyszącym **\<span >** .</span><span class="sxs-lookup"><span data-stu-id="19c7a-484">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="19c7a-485">Walidacja typu danych jest oparta na typie .NET właściwości, chyba że zostanie zastąpiona przez atrybut `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-485">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="19c7a-486">Przeglądarki mają własne domyślne komunikaty o błędach, ale pakietem weryfikacji jQuery nie dyskretnego sprawdzania poprawności może przesłonić te komunikaty.</span><span class="sxs-lookup"><span data-stu-id="19c7a-486">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="19c7a-487">`[DataType]` atrybuty i podklasy, takie jak `[EmailAddress]` pozwalają określić komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-487">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="19c7a-488">Dodawanie walidacji do formularzy dynamicznych</span><span class="sxs-lookup"><span data-stu-id="19c7a-488">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="19c7a-489">w przypadku niedyskretnego sprawdzania poprawności jest sprawdzana logika walidacji i parametry do jQuery podczas pierwszego ładowania strony.</span><span class="sxs-lookup"><span data-stu-id="19c7a-489">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="19c7a-490">W związku z tym sprawdzanie poprawności nie działa automatycznie na formularzach generowanych dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="19c7a-490">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="19c7a-491">Aby włączyć weryfikację, poinformuj jQuery o niezauważalnej weryfikacji, aby przeanalizować formularz dynamiczny bezpośrednio po jego utworzeniu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-491">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="19c7a-492">Na przykład poniższy kod konfiguruje walidację po stronie klienta w formularzu dodanym przez AJAX.</span><span class="sxs-lookup"><span data-stu-id="19c7a-492">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="19c7a-493">Metoda `$.validator.unobtrusive.parse()` akceptuje selektor jQuery dla jednego argumentu.</span><span class="sxs-lookup"><span data-stu-id="19c7a-493">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="19c7a-494">Ta metoda informuje niedyskretną weryfikację jQuery, aby przeanalizować `data-` atrybuty formularzy w ramach tego selektora.</span><span class="sxs-lookup"><span data-stu-id="19c7a-494">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="19c7a-495">Wartości tych atrybutów są następnie przesyłane do wtyczki do walidacji jQuery.</span><span class="sxs-lookup"><span data-stu-id="19c7a-495">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="19c7a-496">Dodawanie walidacji do formantów dynamicznych</span><span class="sxs-lookup"><span data-stu-id="19c7a-496">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="19c7a-497">Metoda `$.validator.unobtrusive.parse()` działa na całym formularzu, a nie na poszczególnych dynamicznie generowanych kontrolkach, takich jak `<input>` i `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-497">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="19c7a-498">Aby przeanalizować formularz, Usuń dane sprawdzania poprawności, które zostały dodane, gdy formularz został wcześniej przeanalizowany, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-498">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="19c7a-499">Niestandardowe sprawdzanie poprawności po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-499">Custom client-side validation</span></span>

<span data-ttu-id="19c7a-500">Niestandardowe sprawdzanie poprawności po stronie klienta jest wykonywane przez wygenerowanie `data-` atrybutów HTML, które działają z niestandardowym identyfikatorem platformy jQuery.</span><span class="sxs-lookup"><span data-stu-id="19c7a-500">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="19c7a-501">Następujący przykładowy kod adaptera został zapisany dla atrybutów `ClassicMovie` i `ClassicMovie2`, które zostały wprowadzone we wcześniejszej części tego artykułu:</span><span class="sxs-lookup"><span data-stu-id="19c7a-501">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/2.x/ValidationSample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="19c7a-502">Aby uzyskać informacje o sposobach pisania kart sieciowych, zobacz [dokumentację dotyczącą platformy jQuery Validate](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="19c7a-502">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="19c7a-503">Użycie karty dla danego pola jest wyzwalane przez `data-` atrybuty, które:</span><span class="sxs-lookup"><span data-stu-id="19c7a-503">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="19c7a-504">Oznacz pole jako podlegające weryfikacji (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="19c7a-504">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="19c7a-505">Zidentyfikuj nazwę reguły walidacji i tekst komunikatu o błędzie (na przykład `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="19c7a-505">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="19c7a-506">Podaj wszelkie dodatkowe parametry wymagane przez moduł walidacji (na przykład `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="19c7a-506">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="19c7a-507">W poniższym przykładzie pokazano `data-` atrybuty `ClassicMovie` atrybutu przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="19c7a-507">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="19c7a-508">Jak wspomniano wcześniej, [pomocników tagów](xref:mvc/views/tag-helpers/intro) i [pomocników HTML](xref:mvc/views/overview) używają informacji z atrybutów walidacji do renderowania atrybutów `data-`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-508">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="19c7a-509">Dostępne są dwie opcje pisania kodu, które powoduje utworzenie niestandardowych atrybutów HTML `data-`:</span><span class="sxs-lookup"><span data-stu-id="19c7a-509">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="19c7a-510">Utwórz klasę, która pochodzi od `AttributeAdapterBase<TAttribute>` i klasy implementującej `IValidationAttributeAdapterProvider`, i Zarejestruj swój atrybut i jego kartę w programie DI.</span><span class="sxs-lookup"><span data-stu-id="19c7a-510">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="19c7a-511">Ta metoda jest zgodna z [pojedynczym podmiotem odpowiedzialnym](https://wikipedia.org/wiki/Single_responsibility_principle) w odniesieniu do kodu weryfikacyjnego związanego z serwerem i klienta jest w osobnych klasach.</span><span class="sxs-lookup"><span data-stu-id="19c7a-511">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="19c7a-512">Adapter ma również zalety, że ponieważ jest on zarejestrowany w programie DI, w razie potrzeby są dostępne inne usługi w programie DI.</span><span class="sxs-lookup"><span data-stu-id="19c7a-512">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="19c7a-513">Zaimplementuj `IClientModelValidator` w klasie `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-513">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="19c7a-514">Ta metoda może być odpowiednia, jeśli atrybut nie wykonuje walidacji po stronie serwera i nie wymaga żadnych usług z programu DI.</span><span class="sxs-lookup"><span data-stu-id="19c7a-514">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="19c7a-515">AttributeAdapter dla weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-515">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="19c7a-516">Ta metoda renderowania atrybutów `data-` w kodzie HTML jest używana przez atrybut `ClassicMovie` w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-516">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="19c7a-517">Aby dodać weryfikację klienta przy użyciu tej metody:</span><span class="sxs-lookup"><span data-stu-id="19c7a-517">To add client validation by using this method:</span></span>

1. <span data-ttu-id="19c7a-518">Utwórz klasę adaptera atrybutów dla niestandardowego atrybutu walidacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-518">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="19c7a-519">Utwórz klasę z [AttributeAdapterBase\<t >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="19c7a-519">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="19c7a-520">Utwórz metodę `AddValidation`, która dodaje atrybuty `data-` do renderowanych danych wyjściowych, jak pokazano w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-520">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="19c7a-521">Utwórz klasę dostawcy kart implementującą <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="19c7a-521">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="19c7a-522">W metodzie `GetAttributeAdapter` Przekaż atrybut niestandardowy do konstruktora adaptera, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-522">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="19c7a-523">Zarejestruj dostawcę adaptera dla `Startup.ConfigureServices`DI in:</span><span class="sxs-lookup"><span data-stu-id="19c7a-523">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="19c7a-524">IClientModelValidator dla weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-524">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="19c7a-525">Ta metoda renderowania atrybutów `data-` w kodzie HTML jest używana przez atrybut `ClassicMovie2` w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="19c7a-525">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="19c7a-526">Aby dodać weryfikację klienta przy użyciu tej metody:</span><span class="sxs-lookup"><span data-stu-id="19c7a-526">To add client validation by using this method:</span></span>

* <span data-ttu-id="19c7a-527">W atrybucie niestandardowego sprawdzania poprawności Zaimplementuj interfejs `IClientModelValidator` i Utwórz metodę `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="19c7a-527">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="19c7a-528">W metodzie `AddValidation` Dodaj atrybuty `data-` do walidacji, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19c7a-528">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="19c7a-529">Wyłącz weryfikację po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="19c7a-529">Disable client-side validation</span></span>

<span data-ttu-id="19c7a-530">Poniższy kod wyłącza weryfikację klienta w widokach MVC:</span><span class="sxs-lookup"><span data-stu-id="19c7a-530">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="19c7a-531">I w Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="19c7a-531">And in Razor Pages:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="19c7a-532">Kolejną opcją wyłączenia sprawdzania poprawności klienta jest komentarz do odwołania do `_ValidationScriptsPartial` w pliku *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="19c7a-532">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="19c7a-533">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="19c7a-533">Additional resources</span></span>

* [<span data-ttu-id="19c7a-534">Przestrzeń nazw System. ComponentModel. DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="19c7a-534">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="19c7a-535">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="19c7a-535">Model Binding</span></span>](model-binding.md)

::: moniker-end
