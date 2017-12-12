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
<a name="aspnet-web-pages-razor-api-quick-reference"></a>Strony sieci Web platformy ASP.NET (Razor) interfejsu API szybkie odwołanie
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ta strona zawiera listę wraz z przykładami krótki najczęściej używanych obiektów, właściwości i metody dla programowania w języku ASP.NET Web Pages o składni Razor.
> 
> Opisy oznaczonej jako "(v2)" zostały wprowadzone w programie ASP.NET Web Pages w wersji 2.
> 
> Dla dokumentacji interfejsu API, zobacz [ASP.NET Web Pages dokumentacji](https://go.microsoft.com/fwlink/?LinkId=208659) w witrynie MSDN.
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
>   
> 
> W tym samouczku współdziała również z ASP.NET Web Pages 2 i stron sieci Web ASP.NET w wersji 1.0 (z wyjątkiem funkcji oznaczone v2).


Ta strona zawiera informacje dotyczące następujących czynności:

- [Klasy](#Classes)
- [Dane](#Data)
- [Pomocnicy](#Helpers)
- [Sprawdzanie poprawności](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Klasy

### `AppState[key], AppState[index],App`

Zawiera dane, które mogą być współużytkowane przez wszystkich stron w aplikacji. Można użyć dynamicznej `App` właściwości dostępu do danych sam, jak w poniższym przykładzie:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Konwertuje wartość ciągu na wartość logiczną (PRAWDA/FAŁSZ). Zwraca wartość false lub określoną wartość, jeśli ciąg nie reprezentuje PRAWDA/FAŁSZ.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Konwertuje wartość ciągu do daty/godziny. Zwraca `DateTime.MinValue` lub określoną wartość, jeśli ciąg nie reprezentuje daty/godziny.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Konwertuje wartość ciągu o wartości dziesiętnej. Zwraca 0,0 lub określoną wartość, jeśli ciąg nie reprezentuje wartości dziesiętnej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Konwertuje wartość ciągu na typ float. Zwraca 0,0 lub określoną wartość, jeśli ciąg nie reprezentuje wartości dziesiętnej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Konwertuje wartość ciągu na liczbę całkowitą. Zwraca wartość 0 lub określoną wartość, jeśli ciąg nie reprezentuje liczbą całkowitą.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Tworzy adres URL przeglądarki zgodnej z ścieżkę do pliku lokalnego, z części opcjonalne dodatkowe ścieżki.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Renderuje *wartość* jako kod znaczników HTML zamiast renderowaniem go jako zakodowany w formacie HTML danych wyjściowych.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Zwraca wartość PRAWDA, jeśli można przekonwertować daną wartość ciągu do określonego typu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Zwraca wartość PRAWDA, jeśli obiektu lub zmienna nie ma wartości.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Zwraca wartość PRAWDA, jeśli żądanie jest żądaniem POST. (Początkowej żądań są zazwyczaj GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Określa ścieżkę strony układu do zastosowania do tej strony.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Zawiera dane w bieżącym żądaniu współużytkowane przez strony, strony układu i strony częściowe. Można użyć dynamicznej `Page` właściwości dostępu do danych sam, jak w poniższym przykładzie:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Układ strony) Renderuje zawartość strony zawartości, której nie ma żadnych nazwanych sekcji.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Renderuje stronę zawartości przy użyciu określonej ścieżki i opcjonalne dodatkowe dane. Można uzyskać wartości dodatkowych parametrów z `PageData` za pomocą pozycji (przykład: 1) lub klucza (przykład 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Układ strony) Renderuje zawartość sekcji o nazwie. Ustaw *wymagane* na wartość false powoduje, że sekcja opcjonalne.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Pobiera lub ustawia wartość pliku cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Pobiera pliki, które zostały przekazane w bieżącym żądaniu.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Pobiera dane, która była umieszczona w formie (jako ciąg). `Request[key]`sprawdza zarówno `Request.Form` i `Request.QueryString` kolekcji.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Pobiera dane, która została określona w zapytaniu URL. `Request[key]`sprawdza zarówno `Request.Form` i `Request.QueryString` kolekcji.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Selektywnie wyłącza żądanie weryfikacji dla elementu form, wartość ciągu kwerendy, pliku cookie lub wartość nagłówka. Weryfikacja żądania jest domyślnie włączona i uniemożliwia użytkownikom przesyłanie znaczników lub inne potencjalnie niebezpieczną zawartość.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Dodaje do odpowiedzi nagłówek serwera HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Przechowuje dane wyjściowe strony w wyznaczonym czasie. Opcjonalnie ustawić *przedłużanie* zresetować limitu czasu na dostęp do każdej strony i *varyByParams* celu buforowania różnych wersji strony dla każdego ciągu innego zapytania w żądaniu strony.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Przekierowuje żądanie do nowej lokalizacji.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Ustawia kod stanu HTTP wysyłany do przeglądarki.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Zapisuje zawartość *danych* do odpowiedzi z typem MIME opcjonalne.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Zapisuje zawartość pliku do odpowiedzi.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Układ strony) Definiuje sekcję zawartości o nazwie.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Dekoduje ciąg, który ma kodowania HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Koduje ciąg w celu renderowania w kod znaczników HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Zwraca ścieżkę fizyczną serwera dla określonej ścieżki wirtualnej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Dekoduje adresu URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Koduje tekst w adresie URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Pobiera lub ustawia wartość, która istnieje do momentu zamknięcia przeglądarki przez użytkownika.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Wyświetla wartość obiektu reprezentację ciągu.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Pobiera dodatkowe dane z adresu URL (na przykład */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Zmienia hasło dla określonego użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Potwierdzenie konta za pomocą tokenu potwierdzenia konta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Tworzy nowe konto użytkownika przy użyciu określonej nazwy użytkownika i hasła. Aby wymagać tokenu potwierdzenia, przekazać wartość true dla *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Pobiera identyfikator całkowitą dla obecnie zalogowanego użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Pobiera nazwę dla obecnie zalogowanego użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Generuje token resetowania hasła, który można wysłać w wiadomości e-mail do użytkownika, aby zresetować hasło użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Zwraca identyfikator użytkownika z nazwą użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Zwraca wartość PRAWDA, jeśli bieżący użytkownik jest zalogowany.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Zwraca wartość PRAWDA, jeśli użytkownik został potwierdzony (na przykład za pomocą wiadomość e-mail z potwierdzeniem).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Zwraca wartość PRAWDA, jeśli nazwa bieżącego użytkownika jest zgodna z określoną nazwą użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Rejestruje użytkownika przez ustawienie token uwierzytelniania w pliku cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Limit loguje użytkownika przez usunięcie tokenu pliku cookie uwierzytelniania.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Jeśli użytkownik nie jest uwierzytelniony, ustawia stan HTTP na wartość 401 (bez autoryzacji).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Jeśli bieżący użytkownik nie jest członkiem jednej z określonych ról, ustawia stan HTTP na wartość 401 (bez autoryzacji).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Jeśli bieżący użytkownik nie jest określony przez użytkownika *username*, ustawia stan HTTP na wartość 401 (bez autoryzacji).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Jeśli token resetowania hasła jest prawidłowy, zmiany hasła nowe hasło.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Dane

### `Database.Execute(SQLstatement [,parameters]`

Wykonuje *SQLstatement* (z parametrami opcjonalnymi), takich jak wstawianie, DELETE lub UPDATE i zwraca liczbę uwzględnionych rekordów.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Zwraca kolumnę tożsamości ostatnio wstawionego wiersza.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Otwiera określony plik bazy danych lub baza danych określona przy użyciu parametrów połączenia nazwanego w *Web.config* pliku.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Otwiera bazę danych przy użyciu parametrów połączenia. (To zachowanie różni się od `Database.Open`, który używa nazwy ciągu połączenia.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Wysyła zapytanie do bazy danych, za pomocą *SQLstatement* (opcjonalnie przekazywanie parametrów) i zwraca wynik jako kolekcja.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Wykonuje *SQLstatement* (z parametrami opcjonalnymi) i zwraca pojedynczy rekord.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Wykonuje *SQLstatement* (z parametrami opcjonalnymi) i zwraca pojedynczą wartość.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Pomocnicy

### `Analytics.GetGoogleHtml(webPropertyId)`

Renderuje Google Analytics JavaScript dla określonego identyfikatora.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Renderuje kod StatCounter Analytics JavaScript dla określonego projektu.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Renderuje kod Yahoo Analytics JavaScript dla określonego konta.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Przekazuje wyszukiwania usługi Bing. Aby określić lokację do wyszukiwania i tytuł pola wyszukiwania, można ustawić `Bing.SiteUrl` i `Bing.SiteTitle` właściwości. Zwykle ustaw te właściwości  *\_AppStart* strony.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inicjuje wykresu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Dodaje legendę do wykresu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Dodaje szereg wartości do wykresu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Zwraca wartość skrótu dla określonych danych. Domyślnym algorytmem jest `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Umożliwia użytkownikom usługi Facebook, utworzyć połączenie do stron.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Renderuje interfejsu użytkownika dla przekazywania plików.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Renderuje określony tag graczy Xbox.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Renderuje obraz Gravatar określony adres e-mail.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Konwertuje obiekt danych na ciąg w formacie JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Konwertuje ciąg wejściowy zakodowane w formacie JSON na obiekt danych, które mogą przejść przez lub wstawienia do bazy danych.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Renderuje społecznościowych łączy sieciowych przy użyciu określonego tytułu i opcjonalnie adresu URL.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Komunikat o błędzie zostanie skojarzony z polem formularza. Użyj `ModelState` pomocy można uzyskać dostępu do tego elementu członkowskiego.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Kojarzy komunikat o błędzie z formularza. Użyj `ModelState` pomocy można uzyskać dostępu do tego elementu członkowskiego.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Zwraca wartość PRAWDA, jeśli nie ma żadnych błędów sprawdzania poprawności. Użyj `ModelState` pomocy można uzyskać dostępu do tego elementu członkowskiego.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Renderuje właściwości i wartości obiektu i wszystkich podrzędnych obiektów.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Renderuje reCAPTCHA testu weryfikacji.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Ustawia klucze publiczne i prywatne dla usługi reCAPTCHA. Zwykle ustaw te właściwości  *\_AppStart* strony.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Zwraca wynik testu reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Wyświetla informacje o stanie składnika ASP.NET Web Pages.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Renderuje strumienia usługi Twitter dla określonego użytkownika.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Renderuje strumienia usługi Twitter dla określony tekst wyszukiwania.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Renderuje Flash player wideo dla określonego pliku z opcjonalne szerokości i wysokości.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Renderuje programu Windows Media player dla określonego pliku z opcjonalne szerokości i wysokości.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Renderuje odtwarzacza Silverlight dla określonego *XAP* pliku wymagane szerokości i wysokości.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Zwraca obiekt określony przez *klucza*, lub wartość null, jeśli nie znaleziono obiektu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Usuwa obiekt określony przez *klucza* z pamięci podręcznej.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Umieszcza *wartość* do pamięci podręcznej o nazwie określonej przez *klucza*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Tworzy nową `WebGrid` przy użyciu danych z zapytania.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Renderuje kod znaczników, aby wyświetlić dane w tabeli HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Renderuje pagera dla `WebGrid` obiektu.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Ładuje obraz z podanej ścieżki.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Dodaje określony obraz znaku wodnego.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Dodaje określony tekst do obrazu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Odwraca obraz w poziomie lub pionie.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Ładuje obraz, gdy obraz jest przesyłana do strony podczas przekazywania pliku.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Zmienia rozmiar obrazu.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Obraca obraz w lewo lub w prawo.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Zapisuje obraz w określonej ścieżce.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Ustawia hasło dla serwera SMTP. Zwykle tę właściwość można ustawić w  *\_AppStart* strony.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Wysyła wiadomość e-mail.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Ustawia nazwę serwera SMTP. Zwykle tę właściwość można ustawić w*\_AppStart* strony.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Ustawia nazwę użytkownika dla serwera SMTP. Zwykle tę właściwość należy ustawić w  *\_AppStart* strony.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Walidacja

### `Html.ValidationMessage(field)`

(v2) Wyświetla komunikat o błędzie weryfikacji dla określonego pola.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Wyświetla listę wszystkich błędów sprawdzania poprawności.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Rejestruje element wejściowy użytkownika dla określonego typu weryfikacji.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Dynamicznie renderuje atrybuty klasy CSS do weryfikacji po stronie klienta, dzięki czemu można sformatować komunikatów o błędach weryfikacji. (Wymaga odwoływania bibliotek odpowiedni skrypt klienta i zdefiniowanie klas CSS).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Umożliwia weryfikację pola wejściowego użytkownika po stronie klienta. (Wymaga odwoływania bibliotek odpowiedni skrypt klienta).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Zwraca wartość PRAWDA, jeśli wszystkich elementów wejściowych użytkownika, które są zarejestrowanych składników do weryfikacji zawierają prawidłowe wartości.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Określa, że użytkownik musi podać wartość dla elementu wejściowego użytkownika.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Określa, że użytkownik musi podać wartości dla każdego z elementów wejściowych użytkownika. Ta metoda nie pozwalają określić niestandardowy komunikat o błędzie.

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

(v2) Określa test weryfikacji, korzystając z `Validation.Add` metody.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
