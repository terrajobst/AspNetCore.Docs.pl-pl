---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: "Strony sieci Web platformy ASP.NET (Razor) interfejsu API szybkie odwołanie | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ta strona zawiera listę wraz z przykładami krótki najczęściej używanych obiektów, właściwości i metody dla programowania w języku ASP.NET Web Pages o składni Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 35f91f4dbea4881d9dabc4ab7c6b96dbb6a01ea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="19ad5-103">Strony sieci Web platformy ASP.NET (Razor) interfejsu API szybkie odwołanie</span><span class="sxs-lookup"><span data-stu-id="19ad5-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="19ad5-104">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="19ad5-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="19ad5-105">Ta strona zawiera listę wraz z przykładami krótki najczęściej używanych obiektów, właściwości i metody dla programowania w języku ASP.NET Web Pages o składni Razor.</span><span class="sxs-lookup"><span data-stu-id="19ad5-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="19ad5-106">Opisy oznaczonej jako "(v2)" zostały wprowadzone w programie ASP.NET Web Pages w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="19ad5-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="19ad5-107">Dla dokumentacji interfejsu API, zobacz [ASP.NET Web Pages dokumentacji](https://go.microsoft.com/fwlink/?LinkId=208659) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="19ad5-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="19ad5-108">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="19ad5-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="19ad5-109">Strony sieci Web platformy ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="19ad5-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="19ad5-110">W tym samouczku współdziała również z ASP.NET Web Pages 2 i stron sieci Web ASP.NET w wersji 1.0 (z wyjątkiem funkcji oznaczone v2).</span><span class="sxs-lookup"><span data-stu-id="19ad5-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="19ad5-111">Ta strona zawiera informacje dotyczące następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="19ad5-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="19ad5-112">Klasy</span><span class="sxs-lookup"><span data-stu-id="19ad5-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="19ad5-113">Dane</span><span class="sxs-lookup"><span data-stu-id="19ad5-113">Data</span></span>](#Data)
- [<span data-ttu-id="19ad5-114">Pomocnicy</span><span class="sxs-lookup"><span data-stu-id="19ad5-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="19ad5-115">Sprawdzanie poprawności</span><span class="sxs-lookup"><span data-stu-id="19ad5-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="19ad5-116">Klasy</span><span class="sxs-lookup"><span data-stu-id="19ad5-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="19ad5-117">Zawiera dane, które mogą być współużytkowane przez wszystkich stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="19ad5-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="19ad5-118">Można użyć dynamicznej `App` właściwości dostępu do danych sam, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19ad5-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="19ad5-119">Konwertuje wartość ciągu na wartość logiczną (PRAWDA/FAŁSZ).</span><span class="sxs-lookup"><span data-stu-id="19ad5-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="19ad5-120">Zwraca wartość false lub określoną wartość, jeśli ciąg nie reprezentuje PRAWDA/FAŁSZ.</span><span class="sxs-lookup"><span data-stu-id="19ad5-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="19ad5-121">Konwertuje wartość ciągu do daty/godziny.</span><span class="sxs-lookup"><span data-stu-id="19ad5-121">Converts a string value to date/time.</span></span> <span data-ttu-id="19ad5-122">Zwraca `DateTime.MinValue` lub określoną wartość, jeśli ciąg nie reprezentuje daty/godziny.</span><span class="sxs-lookup"><span data-stu-id="19ad5-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="19ad5-123">Konwertuje wartość ciągu o wartości dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="19ad5-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="19ad5-124">Zwraca 0,0 lub określoną wartość, jeśli ciąg nie reprezentuje wartości dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="19ad5-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="19ad5-125">Konwertuje wartość ciągu na typ float.</span><span class="sxs-lookup"><span data-stu-id="19ad5-125">Converts a string value to a float.</span></span> <span data-ttu-id="19ad5-126">Zwraca 0,0 lub określoną wartość, jeśli ciąg nie reprezentuje wartości dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="19ad5-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="19ad5-127">Konwertuje wartość ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="19ad5-127">Converts a string value to an integer.</span></span> <span data-ttu-id="19ad5-128">Zwraca wartość 0 lub określoną wartość, jeśli ciąg nie reprezentuje liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="19ad5-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="19ad5-129">Tworzy adres URL przeglądarki zgodnej z ścieżkę do pliku lokalnego, z części opcjonalne dodatkowe ścieżki.</span><span class="sxs-lookup"><span data-stu-id="19ad5-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="19ad5-130">Renderuje *wartość* jako kod znaczników HTML zamiast renderowaniem go jako zakodowany w formacie HTML danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="19ad5-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="19ad5-131">Zwraca wartość PRAWDA, jeśli można przekonwertować daną wartość ciągu do określonego typu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="19ad5-132">Zwraca wartość PRAWDA, jeśli obiektu lub zmienna nie ma wartości.</span><span class="sxs-lookup"><span data-stu-id="19ad5-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="19ad5-133">Zwraca wartość PRAWDA, jeśli żądanie jest żądaniem POST.</span><span class="sxs-lookup"><span data-stu-id="19ad5-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="19ad5-134">(Początkowej żądań są zazwyczaj GET).</span><span class="sxs-lookup"><span data-stu-id="19ad5-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="19ad5-135">Określa ścieżkę strony układu do zastosowania do tej strony.</span><span class="sxs-lookup"><span data-stu-id="19ad5-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="19ad5-136">Zawiera dane w bieżącym żądaniu współużytkowane przez strony, strony układu i strony częściowe.</span><span class="sxs-lookup"><span data-stu-id="19ad5-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="19ad5-137">Można użyć dynamicznej `Page` właściwości dostępu do danych sam, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="19ad5-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="19ad5-138">(Układ strony) Renderuje zawartość strony zawartości, której nie ma żadnych nazwanych sekcji.</span><span class="sxs-lookup"><span data-stu-id="19ad5-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="19ad5-139">Renderuje stronę zawartości przy użyciu określonej ścieżki i opcjonalne dodatkowe dane.</span><span class="sxs-lookup"><span data-stu-id="19ad5-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="19ad5-140">Można uzyskać wartości dodatkowych parametrów z `PageData` za pomocą pozycji (przykład: 1) lub klucza (przykład 2).</span><span class="sxs-lookup"><span data-stu-id="19ad5-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="19ad5-141">(Układ strony) Renderuje zawartość sekcji o nazwie.</span><span class="sxs-lookup"><span data-stu-id="19ad5-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="19ad5-142">Ustaw *wymagane* na wartość false powoduje, że sekcja opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="19ad5-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="19ad5-143">Pobiera lub ustawia wartość pliku cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="19ad5-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="19ad5-144">Pobiera pliki, które zostały przekazane w bieżącym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="19ad5-145">Pobiera dane, która była umieszczona w formie (jako ciąg).</span><span class="sxs-lookup"><span data-stu-id="19ad5-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="19ad5-146">`Request[key]`sprawdza zarówno `Request.Form` i `Request.QueryString` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="19ad5-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="19ad5-147">Pobiera dane, która została określona w zapytaniu URL.</span><span class="sxs-lookup"><span data-stu-id="19ad5-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="19ad5-148">`Request[key]`sprawdza zarówno `Request.Form` i `Request.QueryString` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="19ad5-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="19ad5-149">Selektywnie wyłącza żądanie weryfikacji dla elementu form, wartość ciągu kwerendy, pliku cookie lub wartość nagłówka.</span><span class="sxs-lookup"><span data-stu-id="19ad5-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="19ad5-150">Weryfikacja żądania jest domyślnie włączona i uniemożliwia użytkownikom przesyłanie znaczników lub inne potencjalnie niebezpieczną zawartość.</span><span class="sxs-lookup"><span data-stu-id="19ad5-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="19ad5-151">Dodaje do odpowiedzi nagłówek serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="19ad5-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="19ad5-152">Przechowuje dane wyjściowe strony w wyznaczonym czasie.</span><span class="sxs-lookup"><span data-stu-id="19ad5-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="19ad5-153">Opcjonalnie ustawić *przedłużanie* zresetować limitu czasu na dostęp do każdej strony i *varyByParams* celu buforowania różnych wersji strony dla każdego ciągu innego zapytania w żądaniu strony.</span><span class="sxs-lookup"><span data-stu-id="19ad5-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="19ad5-154">Przekierowuje żądanie do nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="19ad5-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="19ad5-155">Ustawia kod stanu HTTP wysyłany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="19ad5-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="19ad5-156">Zapisuje zawartość *danych* do odpowiedzi z typem MIME opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="19ad5-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="19ad5-157">Zapisuje zawartość pliku do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="19ad5-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="19ad5-158">(Układ strony) Definiuje sekcję zawartości o nazwie.</span><span class="sxs-lookup"><span data-stu-id="19ad5-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="19ad5-159">Dekoduje ciąg, który ma kodowania HTML.</span><span class="sxs-lookup"><span data-stu-id="19ad5-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="19ad5-160">Koduje ciąg w celu renderowania w kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="19ad5-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="19ad5-161">Zwraca ścieżkę fizyczną serwera dla określonej ścieżki wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="19ad5-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="19ad5-162">Dekoduje adresu URL.</span><span class="sxs-lookup"><span data-stu-id="19ad5-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="19ad5-163">Koduje tekst w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="19ad5-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="19ad5-164">Pobiera lub ustawia wartość, która istnieje do momentu zamknięcia przeglądarki przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="19ad5-165">Wyświetla wartość obiektu reprezentację ciągu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="19ad5-166">Pobiera dodatkowe dane z adresu URL (na przykład */MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="19ad5-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="19ad5-167">Zmienia hasło dla określonego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="19ad5-168">Potwierdzenie konta za pomocą tokenu potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="19ad5-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="19ad5-169">Tworzy nowe konto użytkownika przy użyciu określonej nazwy użytkownika i hasła.</span><span class="sxs-lookup"><span data-stu-id="19ad5-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="19ad5-170">Aby wymagać tokenu potwierdzenia, przekazać wartość true dla *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="19ad5-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="19ad5-171">Pobiera identyfikator całkowitą dla obecnie zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="19ad5-172">Pobiera nazwę dla obecnie zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="19ad5-173">Generuje token resetowania hasła, który można wysłać w wiadomości e-mail do użytkownika, aby zresetować hasło użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="19ad5-174">Zwraca identyfikator użytkownika z nazwą użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="19ad5-175">Zwraca wartość PRAWDA, jeśli bieżący użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="19ad5-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="19ad5-176">Zwraca wartość PRAWDA, jeśli użytkownik został potwierdzony (na przykład za pomocą wiadomość e-mail z potwierdzeniem).</span><span class="sxs-lookup"><span data-stu-id="19ad5-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="19ad5-177">Zwraca wartość PRAWDA, jeśli nazwa bieżącego użytkownika jest zgodna z określoną nazwą użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="19ad5-178">Rejestruje użytkownika przez ustawienie token uwierzytelniania w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="19ad5-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="19ad5-179">Limit loguje użytkownika przez usunięcie tokenu pliku cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="19ad5-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="19ad5-180">Jeśli użytkownik nie jest uwierzytelniony, ustawia stan HTTP na wartość 401 (bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="19ad5-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="19ad5-181">Jeśli bieżący użytkownik nie jest członkiem jednej z określonych ról, ustawia stan HTTP na wartość 401 (bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="19ad5-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="19ad5-182">Jeśli bieżący użytkownik nie jest określony przez użytkownika *username*, ustawia stan HTTP na wartość 401 (bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="19ad5-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="19ad5-183">Jeśli token resetowania hasła jest prawidłowy, zmiany hasła nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="19ad5-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="19ad5-184">Dane</span><span class="sxs-lookup"><span data-stu-id="19ad5-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="19ad5-185">Wykonuje *SQLstatement* (z parametrami opcjonalnymi), takich jak wstawianie, DELETE lub UPDATE i zwraca liczbę uwzględnionych rekordów.</span><span class="sxs-lookup"><span data-stu-id="19ad5-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="19ad5-186">Zwraca kolumnę tożsamości ostatnio wstawionego wiersza.</span><span class="sxs-lookup"><span data-stu-id="19ad5-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="19ad5-187">Otwiera określony plik bazy danych lub baza danych określona przy użyciu parametrów połączenia nazwanego w *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="19ad5-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="19ad5-188">Otwiera bazę danych przy użyciu parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="19ad5-188">Opens a database using the connection string.</span></span> <span data-ttu-id="19ad5-189">(To zachowanie różni się od `Database.Open`, który używa nazwy ciągu połączenia.)</span><span class="sxs-lookup"><span data-stu-id="19ad5-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="19ad5-190">Wysyła zapytanie do bazy danych, za pomocą *SQLstatement* (opcjonalnie przekazywanie parametrów) i zwraca wynik jako kolekcja.</span><span class="sxs-lookup"><span data-stu-id="19ad5-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="19ad5-191">Wykonuje *SQLstatement* (z parametrami opcjonalnymi) i zwraca pojedynczy rekord.</span><span class="sxs-lookup"><span data-stu-id="19ad5-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="19ad5-192">Wykonuje *SQLstatement* (z parametrami opcjonalnymi) i zwraca pojedynczą wartość.</span><span class="sxs-lookup"><span data-stu-id="19ad5-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="19ad5-193">Pomocnicy</span><span class="sxs-lookup"><span data-stu-id="19ad5-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="19ad5-194">Renderuje Google Analytics JavaScript dla określonego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="19ad5-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="19ad5-195">Renderuje kod StatCounter Analytics JavaScript dla określonego projektu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="19ad5-196">Renderuje kod Yahoo Analytics JavaScript dla określonego konta.</span><span class="sxs-lookup"><span data-stu-id="19ad5-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="19ad5-197">Przekazuje wyszukiwania usługi Bing.</span><span class="sxs-lookup"><span data-stu-id="19ad5-197">Passes a search to Bing.</span></span> <span data-ttu-id="19ad5-198">Aby określić lokację do wyszukiwania i tytuł pola wyszukiwania, można ustawić `Bing.SiteUrl` i `Bing.SiteTitle` właściwości.</span><span class="sxs-lookup"><span data-stu-id="19ad5-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="19ad5-199">Zwykle ustaw te właściwości  *\_AppStart* strony.</span><span class="sxs-lookup"><span data-stu-id="19ad5-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="19ad5-200">Inicjuje wykresu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="19ad5-201">Dodaje legendę do wykresu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="19ad5-202">Dodaje szereg wartości do wykresu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="19ad5-203">Zwraca wartość skrótu dla określonych danych.</span><span class="sxs-lookup"><span data-stu-id="19ad5-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="19ad5-204">Domyślnym algorytmem jest `sha256`.</span><span class="sxs-lookup"><span data-stu-id="19ad5-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="19ad5-205">Umożliwia użytkownikom usługi Facebook, utworzyć połączenie do stron.</span><span class="sxs-lookup"><span data-stu-id="19ad5-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="19ad5-206">Renderuje interfejsu użytkownika dla przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="19ad5-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="19ad5-207">Renderuje określony tag graczy Xbox.</span><span class="sxs-lookup"><span data-stu-id="19ad5-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="19ad5-208">Renderuje obraz Gravatar określony adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="19ad5-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="19ad5-209">Konwertuje obiekt danych na ciąg w formacie JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="19ad5-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="19ad5-210">Konwertuje ciąg wejściowy zakodowane w formacie JSON na obiekt danych, które mogą przejść przez lub wstawienia do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="19ad5-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="19ad5-211">Renderuje społecznościowych łączy sieciowych przy użyciu określonego tytułu i opcjonalnie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="19ad5-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="19ad5-212">Komunikat o błędzie zostanie skojarzony z polem formularza.</span><span class="sxs-lookup"><span data-stu-id="19ad5-212">Associates an error message with a form field.</span></span> <span data-ttu-id="19ad5-213">Użyj `ModelState` pomocy można uzyskać dostępu do tego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="19ad5-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="19ad5-214">Kojarzy komunikat o błędzie z formularza.</span><span class="sxs-lookup"><span data-stu-id="19ad5-214">Associates an error message with a form.</span></span> <span data-ttu-id="19ad5-215">Użyj `ModelState` pomocy można uzyskać dostępu do tego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="19ad5-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="19ad5-216">Zwraca wartość PRAWDA, jeśli nie ma żadnych błędów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="19ad5-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="19ad5-217">Użyj `ModelState` pomocy można uzyskać dostępu do tego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="19ad5-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="19ad5-218">Renderuje właściwości i wartości obiektu i wszystkich podrzędnych obiektów.</span><span class="sxs-lookup"><span data-stu-id="19ad5-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="19ad5-219">Renderuje reCAPTCHA testu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="19ad5-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="19ad5-220">Ustawia klucze publiczne i prywatne dla usługi reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="19ad5-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="19ad5-221">Zwykle ustaw te właściwości  *\_AppStart* strony.</span><span class="sxs-lookup"><span data-stu-id="19ad5-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="19ad5-222">Zwraca wynik testu reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="19ad5-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="19ad5-223">Wyświetla informacje o stanie składnika ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="19ad5-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="19ad5-224">Renderuje strumienia usługi Twitter dla określonego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="19ad5-225">Renderuje strumienia usługi Twitter dla określony tekst wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="19ad5-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="19ad5-226">Renderuje Flash player wideo dla określonego pliku z opcjonalne szerokości i wysokości.</span><span class="sxs-lookup"><span data-stu-id="19ad5-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="19ad5-227">Renderuje programu Windows Media player dla określonego pliku z opcjonalne szerokości i wysokości.</span><span class="sxs-lookup"><span data-stu-id="19ad5-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="19ad5-228">Renderuje odtwarzacza Silverlight dla określonego *XAP* pliku wymagane szerokości i wysokości.</span><span class="sxs-lookup"><span data-stu-id="19ad5-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="19ad5-229">Zwraca obiekt określony przez *klucza*, lub wartość null, jeśli nie znaleziono obiektu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="19ad5-230">Usuwa obiekt określony przez *klucza* z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="19ad5-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="19ad5-231">Umieszcza *wartość* do pamięci podręcznej o nazwie określonej przez *klucza*.</span><span class="sxs-lookup"><span data-stu-id="19ad5-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="19ad5-232">Tworzy nową `WebGrid` przy użyciu danych z zapytania.</span><span class="sxs-lookup"><span data-stu-id="19ad5-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="19ad5-233">Renderuje kod znaczników, aby wyświetlić dane w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="19ad5-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="19ad5-234">Renderuje pagera dla `WebGrid` obiektu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="19ad5-235">Ładuje obraz z podanej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="19ad5-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="19ad5-236">Dodaje określony obraz znaku wodnego.</span><span class="sxs-lookup"><span data-stu-id="19ad5-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="19ad5-237">Dodaje określony tekst do obrazu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="19ad5-238">Odwraca obraz w poziomie lub pionie.</span><span class="sxs-lookup"><span data-stu-id="19ad5-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="19ad5-239">Ładuje obraz, gdy obraz jest przesyłana do strony podczas przekazywania pliku.</span><span class="sxs-lookup"><span data-stu-id="19ad5-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="19ad5-240">Zmienia rozmiar obrazu.</span><span class="sxs-lookup"><span data-stu-id="19ad5-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="19ad5-241">Obraca obraz w lewo lub w prawo.</span><span class="sxs-lookup"><span data-stu-id="19ad5-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="19ad5-242">Zapisuje obraz w określonej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="19ad5-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="19ad5-243">Ustawia hasło dla serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="19ad5-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="19ad5-244">Zwykle tę właściwość można ustawić w  *\_AppStart* strony.</span><span class="sxs-lookup"><span data-stu-id="19ad5-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="19ad5-245">Wysyła wiadomość e-mail.</span><span class="sxs-lookup"><span data-stu-id="19ad5-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="19ad5-246">Ustawia nazwę serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="19ad5-246">Sets the SMTP server name.</span></span> <span data-ttu-id="19ad5-247">Zwykle tę właściwość można ustawić w*\_AppStart* strony.</span><span class="sxs-lookup"><span data-stu-id="19ad5-247">Normally you set this property in the*\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="19ad5-248">Ustawia nazwę użytkownika dla serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="19ad5-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="19ad5-249">Zwykle tę właściwość należy ustawić w  *\_AppStart* strony.</span><span class="sxs-lookup"><span data-stu-id="19ad5-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="19ad5-250">Walidacja</span><span class="sxs-lookup"><span data-stu-id="19ad5-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="19ad5-251">(v2) Wyświetla komunikat o błędzie weryfikacji dla określonego pola.</span><span class="sxs-lookup"><span data-stu-id="19ad5-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="19ad5-252">(v2) Wyświetla listę wszystkich błędów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="19ad5-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="19ad5-253">(v2) Rejestruje element wejściowy użytkownika dla określonego typu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="19ad5-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="19ad5-254">(v2) Dynamicznie renderuje atrybuty klasy CSS do weryfikacji po stronie klienta, dzięki czemu można sformatować komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="19ad5-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="19ad5-255">(Wymaga odwoływania bibliotek odpowiedni skrypt klienta i zdefiniowanie klas CSS).</span><span class="sxs-lookup"><span data-stu-id="19ad5-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="19ad5-256">(v2) Umożliwia weryfikację pola wejściowego użytkownika po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="19ad5-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="19ad5-257">(Wymaga odwoływania bibliotek odpowiedni skrypt klienta).</span><span class="sxs-lookup"><span data-stu-id="19ad5-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="19ad5-258">(v2) Zwraca wartość PRAWDA, jeśli wszystkich elementów wejściowych użytkownika, które są zarejestrowanych składników do weryfikacji zawierają prawidłowe wartości.</span><span class="sxs-lookup"><span data-stu-id="19ad5-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="19ad5-259">(v2) Określa, że użytkownik musi podać wartość dla elementu wejściowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="19ad5-260">(v2) Określa, że użytkownik musi podać wartości dla każdego z elementów wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="19ad5-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="19ad5-261">Ta metoda nie pozwalają określić niestandardowy komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="19ad5-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="19ad5-262">(v2) Określa test weryfikacji, korzystając z `Validation.Add` metody.</span><span class="sxs-lookup"><span data-stu-id="19ad5-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
