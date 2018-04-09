---
title: Zapobiegaj Cross-Site żądania (XSRF/CSRF) fałszerstwie w ASP.NET Core
author: steve-smith
description: Wykryj jak nie dopuścić do ataków na aplikacje sieci web, w którym złośliwą witrynę sieci Web może mieć wpływ interakcji między przeglądarką klienta i aplikacji.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Zapobiegaj Cross-Site żądania (XSRF/CSRF) fałszerstwie w ASP.NET Core

Przez [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), i [Rick Anderson](https://twitter.com/RickAndMSFT)

Fałszowanie żądań między witrynami (znanej także jako XSRF lub CSRF, Wymowa *Małgiew zobacz*) jest atak wykorzystujący aplikacji hostowanych w sieci web, zgodnie z którymi aplikacja sieci web złośliwego mogą mieć wpływ interakcji między przeglądarką klienta i aplikacji sieci web, które ufają, który Przeglądarka. Tego rodzaju ataki są możliwe, ponieważ przeglądarki sieci web wysyłać niektóre typy tokenów uwierzytelniania automatycznie z każdego żądania do witryny sieci Web. Ten formularz wykorzystać jest także znana jako *ataku jednym kliknięciem* lub *sesji jazda* ponieważ ataku korzysta z użytkownik wcześniej uwierzytelniona sesji.

Przykład atak CSRF:

1. Użytkownik zaloguje się do `www.good-banking-site.com` uwierzytelnianie za pomocą formularzy. Serwer uwierzytelnia użytkownika i generuje odpowiedzi, który zawiera plik cookie uwierzytelniania. Witryna jest narażony na ataki, ponieważ subskrypcja ufa każde żądanie otrzymanych z prawidłowy plik cookie.
1. Użytkownik odwiedzi złośliwą witrynę, `www.bad-crook-site.com`.

   Złośliwa witryna `www.bad-crook-site.com`, zawiera formularza HTML podobny do następującego:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Zwróć uwagę, że formularza `action` wpisów do lokacji narażony, a nie do niebezpiecznej witryny. Jest to część CSRF "cross-site".

1. Gdy użytkownik wybierze przycisk przesyłania. Przeglądarka wysyła żądanie i automatycznie uwzględnia pliku cookie uwierzytelniania dla żądanej domeny `www.good-banking-site.com`.
1. Żądanie jest uruchamiany na `www.good-banking-site.com` serwera z kontekstem uwierzytelniania użytkownika i mogą wykonywać żadnych czynności, że uwierzytelniony użytkownik może wykonywać.

Gdy użytkownik wybierze przycisk Prześlij formularz, złośliwa witryna można:

* Uruchom skrypt, który automatycznie wyśle formularz.
* Wysyła żądanie AJAX przesyłania formularza. 
* Formularz ukryty z CSS. 

Przy użyciu protokołu HTTPS nie uniemożliwia atak CSRF. Złośliwa witryna można wysyłać `https://www.good-banking-site.com/` równie proste, jak może wysłać żądanie niezabezpieczonego żądania.

Niektóre ataki docelowych punktów końcowych, które odpowiadają na żądania GET w takim przypadku tag obrazu mogą posłużyć do wykonania akcji. Ta forma ataku jest typowe w lokacjach forum, zezwolenie na obrazy, które blokowania języka JavaScript. Aplikacje, które spowodują zmianę stanu dla żądań GET wysyłanych, gdy modyfikacji zmiennych lub zasobów, są podatne na złośliwe ataki. **Żądania GET, które spowodują zmianę stanu jest niebezpieczne. Najlepszym rozwiązaniem jest nigdy nie ulegną zmianie stanu na żądanie GET.**

Ataki CSRF są możliwe w dla aplikacji sieci web, które używają plików cookie do uwierzytelniania, ponieważ:

* Przeglądarki przechowywania plików cookie wystawiony przez aplikację sieci web.
* Przechowywane pliki cookie obejmują pliki cookie sesji dla uwierzytelnionych użytkowników.
* Przeglądarki Wyślij wszystkie pliki cookie skojarzone z domeną w aplikacji sieci web każde żądanie niezależnie od tego, jak żądania do aplikacji został wygenerowany w przeglądarce.

Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane również są zagrożone. Po zalogowaniu się za pomocą uwierzytelnianie podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia do sesji&dagger; kończy się.

&dagger;W tym kontekście *sesji* odwołuje się do sesji po stronie klienta, w którym użytkownik jest uwierzytelniony. Jest niezwiązanych ze sobą do sesji po stronie serwera lub [platformy ASP.NET Core sesji w oprogramowaniu pośredniczącym](xref:fundamentals/app-state).

Użytkownicy mogą chronią przed luk w zabezpieczeniach CSRF wykonując środki ostrożności:

* Zaloguj się wylogowuje aplikacje sieci web po zakończeniu korzystania z nich.
* Wyczyść pliki cookie przeglądarki okresowo.

Jednak luk w zabezpieczeniach CSRF są zasadniczo problem z aplikacji sieci web, a nie użytkownika końcowego.

## <a name="authentication-fundamentals"></a>Podstawowe informacje dotyczące uwierzytelniania

Plik cookie uwierzytelniania jest popularnych formy uwierzytelniania. Systemy uwierzytelniania opartego na tokenie rośnie w popularne, szczególnie w przypadku aplikacje jednostronicowe (źródła).

### <a name="cookie-based-authentication"></a>Uwierzytelnianie na podstawie plików cookie

Podczas uwierzytelniania przy użyciu nazwy użytkownika i hasła użytkownika, są one wystawiony token, zawierający bilet uwierzytelniania, który może służyć do uwierzytelniania i autoryzacji. Token jest przechowywany jako sprawia, że plik cookie dołączona każde żądanie klienta. Generowanie i sprawdzanie poprawności ten plik cookie jest wykonywane przez oprogramowanie pośredniczące uwierzytelniania plików Cookie. [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) serializuje głównej nazwy użytkownika do zaszyfrowanego pliku cookie. Kolejne żądania oprogramowania pośredniczącego sprawdza poprawność pliku cookie, odtwarza podmiot zabezpieczeń i przypisuje do podmiotu zabezpieczeń [użytkownika](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) właściwość [element HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Uwierzytelnianie na podstawie tokenu

Podczas uwierzytelniania użytkownika, są one wystawiony token (nie antiforgery token). Token zawiera informacje o użytkowniku w formie [oświadczeń](/dotnet/framework/security/claims-based-identity-model) lub tokenu odwołania, który wskazuje aplikacji stanu użytkownika, obsługiwane w aplikacji. Gdy użytkownik próbuje uzyskać dostęp do zasobów wymagających uwierzytelniania, token jest wysyłany do aplikacji z nagłówkiem dodatkowe autoryzacji w formie tokenów elementu nośnego. Dzięki temu aplikacji bezstanowych. W kolejnych żądań token jest przekazywany w żądaniu weryfikacji po stronie serwera. Ten token nie jest *zaszyfrowanych*; ma *zakodowane*. Na serwerze token jest dekodowany uzyskiwać dostęp do swoich informacji. Aby wysłać token dla kolejnych żądań, należy przechowywać token w magazynie lokalnym w przeglądarce. Nie można zajmującym się luka w zabezpieczeniach CSRF, jeśli token jest przechowywany w magazynie lokalnym w przeglądarce. CSRF jest istotny, gdy token jest przechowywana w pliku cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Wiele aplikacji hostowanych w jednej domenie

Udostępnionych środowiskach macierzystych są podatne na przejęcie kontroli sesji logowania CSRF i inne ataki.

Mimo że `example1.contoso.net` i `example2.contoso.net` są różnych hostach, ma zależności nawiązywanie niejawnych relacji zaufania między hostami w obszarze `*.contoso.net` domeny. Ta relacja zaufania niejawne umożliwia potencjalnie niezaufane hosty wpływa na siebie nawzajem pliki cookie (zasad tego samego źródła, które będą zarządzały sposobem żądania AJAX nie zawsze dotyczą plików cookie protokołu HTTP).

Ataki wykorzystujące zaufanych plików cookie między aplikacji hostowanych w tej samej domenie można zapobiec przez nie udostępnianie domen. Każda aplikacja znajduje się na jego własnej domeny, nie ma żadnych relacji zaufania niejawnych plików cookie do wykorzystania.

## <a name="aspnet-core-antiforgery-configuration"></a>Konfiguracja antiforgery platformy ASP.NET Core

> [!WARNING]
> Implementuje platformy ASP.NET Core za pomocą antiforgery [ochrony danych platformy ASP.NET Core](xref:security/data-protection/introduction). Stos ochrony danych musi być skonfigurowana do pracy w farmie serwerów. Zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview) Aby uzyskać więcej informacji.

W programie ASP.NET Core 2.0 lub nowszej [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokenów do elementów formularza HTML. Następujący kod w pliku Razor automatycznie generuje tokeny antiforgery:

```cshtml
<form method="post">
    ...
</form>
```

Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generuje tokeny antiforgery domyślnie, jeśli nie będzie formularza metody GET.

Automatycznego generowania tokenów antiforgery elementów formularza HTML się stanie po `<form>` tag zawiera `method="post"` atrybut i jednej z następujących są spełnione:

  * Atrybut akcji jest pusty (`action=""`).
  * Nie jest podana w atrybucie akcji (`<form method="post">`).

Można wyłączyć automatyczne generowanie tokenów antiforgery elementów formularza HTML:

* Jawnie Wyłącz antiforgery tokeny z `asp-antiforgery` atrybutu:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Form element jest wybrana opcja poza pomocników tagów przy użyciu Pomocnika tagów [! symbol Wypisz](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Usuń `FormTagHelper` z widoku. `FormTagHelper` Można usunąć, dodając następujące dyrektywy do widoku Razor z widoku:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Stron razor](xref:mvc/razor-pages/index) są automatycznie chronione przed XSRF/CSRF. Aby uzyskać więcej informacji, zobacz [XSRF/CSRF i stron Razor](xref:mvc/razor-pages/index#xsrf).

Najbardziej typowym podejściem do obrony przed atakami CSRF jest użycie *wzorzec tokenu Synchronizatora* (STP). STP jest używany, gdy użytkownik zażąda strony z danymi formularza:

1. Serwer wysyła token skojarzone z tożsamością bieżącego użytkownika do klienta.
1. Klient wysyła ponownie token do serwera w celu weryfikacji.
1. Jeśli serwer odbiera token, który nie odpowiada tożsamość uwierzytelnionego użytkownika, żądanie zostanie odrzucone.

Token jest unikatowy i nieprzewidywalny. Token mogą służyć do zapewnienia prawidłowego sekwencjonowania szereg żądań (na przykład, zapewniając sekwencji żądanie: strona 1 &ndash; strony 2 &ndash; strony 3). Wszystkie formularze w szablonach platformy ASP.NET Core MVC i stron Razor generowania antiforgery tokenów. Para następujące przykłady widoku generowania tokenów antiforgery:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Jawnie dodać antiforgery token do `<form>` elementu bez użycia pomocników tagów z Pomocnika kodu HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

W każdym z powyższych przypadków platformy ASP.NET Core dodaje ukryte pole formularza podobny do następującego:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

Platformy ASP.NET Core obejmuje trzy [filtry](xref:mvc/controllers/filters) do pracy z tokenami antiforgery:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Opcje antiforgery

Dostosowywanie [antiforgery opcje](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) w `Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| Opcja | Opis |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Określa ustawienia używane do tworzenia antiforgery plików cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Domena pliku cookie. Domyślnie `null`. Ta właściwość jest przestarzała i zostanie usunięta w przyszłych wersjach. Zalecaną alternatywą jest Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Nazwa pliku cookie. Jeśli nie jest ustawiona, system generuje unikatowa nazwa zaczyna się od [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Ta właściwość jest przestarzała i zostanie usunięta w przyszłych wersjach. Zalecaną alternatywą jest Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Ścieżka ustawiona w pliku cookie. Ta właściwość jest przestarzała i zostanie usunięta w przyszłych wersjach. Zalecaną alternatywą jest Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Nazwa ukryte pole formularza używany przez antiforgery system do renderowania antiforgery tokenów w widokach. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Nazwa nagłówka używany przez antiforgery system. Jeśli `null`, system uważa tylko dane formularza. |
| [parametru requireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Określa, czy protokół SSL jest wymagany przez antiforgery system. Jeśli `true`, Niepowodzenie żądania bez użycia protokołu SSL. Domyślnie `false`. Ta właściwość jest przestarzała i zostanie usunięta w przyszłych wersjach. Zalecaną alternatywą jest Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka. Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN". Domyślnie `false`. |

Aby uzyskać więcej informacji, zobacz [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Konfigurowanie funkcji antiforgery z IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) udostępnia interfejs API umożliwia konfigurowanie funkcji antiforgery. `IAntiforgery` może być wymagane w `Configure` metody `Startup` klasy. W poniższym przykładzie użyto oprogramowanie pośredniczące ze strony głównej aplikacji do wygenerowania tokenu antiforgery i wysłać go w odpowiedzi jako plik cookie (przy użyciu domyślnego kątowego konwencji nazewnictwa opisane w dalszej części tego tematu):

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Wymagaj antiforgery weryfikacji

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) jest filtr akcji, który można zastosować do poszczególnych akcji kontrolera, lub globalnie. Żądania skierowane do akcji, które mają ten filtr są zablokowane, chyba że żądanie zawiera prawidłowy token antiforgery.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken` Atrybut wymaga tokenu dla żądań kierowanych do metody akcji decorates, łącznie z żądania HTTP GET. Jeśli `ValidateAntiForgeryToken` atrybut jest stosowany przez kontrolery aplikacji, może zostać zastąpiona przez `IgnoreAntiforgeryToken` atrybutu.

> [!NOTE]
> Platformy ASP.NET Core nie obsługuje automatyczne dodawanie tokenów antiforgery żądania GET.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Automatycznie sprawdzania poprawności tokenów antiforgery niebezpieczny metod HTTP

Aplikacje platformy ASP.NET Core nie generować antiforgery tokeny dla bezpieczne metody HTTP (GET, HEAD, opcje i śledzenia). Zamiast stosowania szeroko `ValidateAntiForgeryToken` atrybutu, a następnie przesłanianie go z `IgnoreAntiforgeryToken` atrybutów, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atrybut może być używany. Ten atrybut działa identycznie do `ValidateAntiForgeryToken` atrybutów, z wyjątkiem tego, czy nie wymaga tokeny żądań zostało nawiązane przy użyciu następujących metod HTTP:

* GET
* HEAD
* OPCJE
* TRACE

Firma Microsoft zaleca użycie `AutoValidateAntiforgeryToken` szeroko dla scenariuszy-API. Dzięki temu akcje po są chronione przez domyślny. Alternatywą jest Ignoruj antiforgery tokeny domyślnie, chyba że `ValidateAntiForgeryToken` jest stosowany do poszczególnych metod akcji. Go najprawdopodobniej w tym scenariuszu dla metody akcji POST pozostać niechronionej przez pomyłkę, pozostawiając narażony na ataki CSRF aplikacji. Wszystkie wpisy należy wysłać antiforgery tokenu.

Interfejsy API nie mają mechanizm automatycznego wysyłania-cookie część tokenu. Implementacja prawdopodobnie zależy od implementacji kodu klienta. Poniżej przedstawiono kilka przykładów:

Przykład poziomie klasy:

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Przykład globalne:

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>Zastąpienie globalnych lub atrybutów antiforgery kontrolera

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) użyć filtru, aby wyeliminować potrzebę antiforgery token dla danej akcji (lub kontrolera). Po zastosowaniu filtru zastępuje `ValidateAntiForgeryToken` i `AutoValidateAntiforgeryToken` filtry określone na wyższym poziomie (globalnie lub w kontrolerze).

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a>Tokenów odświeżania po uwierzytelnieniu

Tokeny należy odświeżyć, po uwierzytelnieniu użytkownika przez przekierowanie użytkownika do widoku lub strony stron Razor.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX i źródła

W tradycyjne aplikacje oparte na języku HTML antiforgery tokenów są przekazywane do serwera przy użyciu pól ukrytym. Nowoczesne aplikacje oparte na języku JavaScript i źródła wielu wniosków programowo. Te żądania AJAX mogą używać innych technik (np. nagłówki żądania lub pliki cookie) do wysyłania tokenu.

Jeśli pliki cookie są używane do przechowywania tokeny uwierzytelniania i do uwierzytelniania żądań interfejsu API na serwerze, CSRF jest potencjalnym problemie. Jeśli Magazyn lokalny jest używany do przechowywania token, CSRF luki w zabezpieczeniach mogą skorygowane, ponieważ wartości z magazynu lokalnego nie są automatycznie wysyłane do serwera z każdym żądaniem. W związku z tym korzystanie z magazynu lokalnego do przechowywania antiforgery token na kliencie i wysyłania tokenu, ponieważ nagłówek żądania jest zalecane podejście.

### <a name="javascript"></a>JavaScript

Przy użyciu języka JavaScript z widokami, token mogą być tworzone przy użyciu usługi z widoku. Wstaw [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) usługi do widoku i wywołanie [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Takie podejście eliminuje potrzebę bezpośrednio przez ustawienia plików cookie z serwera lub odczytywania ich z klienta.

Powyższy przykład używa JavaScript można odczytać wartości pola ukrytego nagłówka AJAX POST.

JavaScript można uzyskać dostępu do tokenów w plikach cookie i użyj zawartość pliku cookie, aby utworzyć nagłówek z wartością tokenu.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Zakładając, że skrypt żądania do wysyłania tokenu w nagłówku o nazwie `X-CSRF-TOKEN`, skonfiguruj usługę antiforgery do wyszukania `X-CSRF-TOKEN` nagłówka:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

W poniższym przykładzie użyto JavaScript żądania AJAX z odpowiedni nagłówek:

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS używa konwencji adres CSRF. Jeśli serwer wysyła plik cookie o nazwie `XSRF-TOKEN`, AngularJS `$http` usługi dodaje wartość pliku cookie do nagłówka podczas wysyłania żądania do serwera. Ten proces odbywa się automatycznie. Nagłówek nie musi być ustawiony w sposób jawny. Nazwa nagłówka jest `X-XSRF-TOKEN`. Serwer powinna wykryć ten nagłówek i zweryfikować jego zawartość.

Dla interfejsu API platformy ASP.NET Core współpracują z tę Konwencję:

* Skonfiguruj aplikację, aby dostarczyć token w pliku cookie o nazwie `XSRF-TOKEN`.
* Skonfiguruj usługę antiforgery do wyszukania nagłówek o nazwie `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Rozszerzanie antiforgery

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typu umożliwia deweloperom rozszerzyć zachowanie systemu anti-CSRF przez dwustronną komunikację w każdym token dodatkowe dane. [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) metoda jest wywoływana za każdym razem zostanie wygenerowany token pola i wartość zwracana jest osadzony w wygenerowany token. Realizator może zwracać sygnaturę czasową, identyfikatora jednorazowego lub wszelkie inne wartości, a następnie wywołać [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) do sprawdzania poprawności danych podczas weryfikowania tokenu. Nazwa użytkownika klienta już jest osadzony w generowanych tokenach, więc nie trzeba wpisywać te informacje. Jeśli token zawiera dane dodatkowe, ale nie `IAntiForgeryAdditionalDataProvider` jest skonfigurowany, dane dodatkowe nie jest zweryfikowany.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page) (OWASP).
