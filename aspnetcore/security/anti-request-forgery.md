---
title: Ataki zapobiec Cross-Site Request Forgery (XSRF/CSRF) w programie ASP.NET Core
author: steve-smith
description: Dowiedz się, jak zapobiegać atakom aplikacji sieci web, w którym złośliwą witrynę sieci Web mogą mieć wpływ na interakcję między przeglądarką klienta i aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 4498bd88e7188ea6c0063aeec054bc20e13ed5f1
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086444"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Ataki zapobiec Cross-Site Request Forgery (XSRF/CSRF) w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), i [Rick Anderson](https://twitter.com/RickAndMSFT)

Fałszowanie żądań między witrynami (znany także jako Wymowa XSRF lub CSRF, *szkl zobacz*) jest ataku na aplikacje hostowane w sieci web, zgodnie z którą aplikacja sieci web złośliwego mogą mieć wpływ na interakcję między przeglądarką klienta i aplikacji sieci web, która ufa Przeglądarka. Te ataki są możliwe, ponieważ przeglądarki sieci web wysyłać niektóre typy tokenów uwierzytelniania automatycznie za pomocą każdego żądania do witryny sieci Web. Ta forma wykorzystać jest także znana jako *ataku jednym kliknięciem* lub *sesji jazda* ponieważ ataku korzysta z zalet użytkownik wcześniej uwierzytelniona sesji.

Przykład ataku typu CSRF:

1. Użytkownik zaloguje się do `www.good-banking-site.com` za pomocą uwierzytelniania formularzy. Serwer uwierzytelnia użytkownika i wysyła odpowiedź, który zawiera plik cookie uwierzytelniania. Witryna jest narażony na ataki, ponieważ Magazyn uzna każde żądanie, które otrzymuje z prawidłowy plik cookie.
1. Użytkownik odwiedzi złośliwą witrynę, `www.bad-crook-site.com`.

   Złośliwa witryna `www.bad-crook-site.com`, podobny do następującego formularza HTML zawiera:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Należy zauważyć, że formularz `action` wpisy do lokacji narażony, a nie do złośliwych witryn. Jest to część "cross-site" CSRF.

1. Gdy użytkownik wybierze przycisk Prześlij. Przeglądarka sprawia, że żądanie i automatycznie dodaje plik cookie uwierzytelniania dla Żądana domena `www.good-banking-site.com`.
1. Żądanie jest uruchamiany na `www.good-banking-site.com` serwera przy użyciu kontekstu uwierzytelniania użytkownika i mogą wykonywać żadnych akcji, która może wykonywać uwierzytelnionego użytkownika.

Oprócz tego scenariusza, gdy użytkownik wybierze przycisk, który można przesłać formularza złośliwych witryn wykonać następujące akcje:

* Uruchom skrypt, który automatycznie przesyła formularz.
* Wyślij przesyłania formularza jako żądaniem AJAX.
* Ukryj formularza przy użyciu CSS.

Te scenariusze alternatywnych nie wymagają żadnych akcji lub danych wejściowych od użytkownika innego niż początkowo odwiedzający złośliwych witryn.

Przy użyciu protokołu HTTPS nie uniemożliwia ataku CSRF. Złośliwa witryna może wysyłać `https://www.good-banking-site.com/` równie łatwo, jak może wysłać żądanie niezabezpieczone żądania.

Niektóre ataki docelowych punktów końcowych, które odpowiadają na żądania GET, w którym to przypadku tag obrazu może służyć do wykonania akcji. Ta forma ataku jest typowe w witrynach forum, które zezwala na obrazach, ale Blokuj kod JavaScript. Aplikacje, które zmieniają stan dla żądań GET, gdzie zmiennych lub zasoby zostały zmienione, są podatne na złośliwe ataki. **Żądania GET, które zmiany stanu jest niebezpieczne. Najlepszym rozwiązaniem jest nigdy nie ulegną zmianie stanu na żądanie GET.**

Możliwe dla aplikacji sieci web, które używać plików cookie uwierzytelniania, ponieważ są CSRF ataków:

* Przeglądarki przechowywania plików cookie, wystawiony przez aplikację sieci web.
* Przechowywane pliki cookie obejmują pliki cookie z sesji uwierzytelnionych użytkowników.
* Przeglądarki Wyślij wszystkie pliki cookie skojarzone z domeną, do aplikacji sieci web każde żądanie, niezależnie od tego, jak żądania do aplikacji został wygenerowany w przeglądarce.

Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane są również narażone. Po zalogowaniu się przy użyciu uwierzytelniania podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia do sesji&dagger; kończy.

&dagger;W tym kontekście *sesji* odwołuje się do sesji po stronie klienta, w którym użytkownik jest uwierzytelniony. Jest niezwiązanych ze sobą do sesji po stronie serwera lub [platformy ASP.NET Core sesji w oprogramowaniu pośredniczącym](xref:fundamentals/app-state).

Użytkownicy mogą zabezpieczyć się przed CSRF luk w zabezpieczeniach, wykonując różne środki ostrożności:

* Zaloguj się na zniżki w stosunku do aplikacji sieci web po zakończeniu korzystania z nich.
* Wyczyść pliki cookie przeglądarki okresowo.

Luki w zabezpieczeniach CSRF są jednak zasadniczo problem z aplikacją sieci web, a nie użytkownika końcowego.

## <a name="authentication-fundamentals"></a>Podstawowe informacje dotyczące uwierzytelniania

Uwierzytelnianie na podstawie plików cookie jest popularnych formy uwierzytelniania. Systemy uwierzytelniania opartego na tokenach rosnące popularność, szczególnie w przypadku aplikacje jednostronicowe (źródła).

### <a name="cookie-based-authentication"></a>Na podstawie plików cookie uwierzytelniania

Gdy użytkownik jest uwierzytelniany przy użyciu nazwy użytkownika i hasła, one wystawiony token token zawierający bilet uwierzytelniania, który może służyć do uwierzytelniania i autoryzacji. Token jest przechowywany jako sprawia, że plik cookie, który towarzyszy każde żądanie klienta. Generowanie i weryfikowania tego pliku cookie odbywa się przez oprogramowanie pośredniczące uwierzytelniania plików Cookie. [Oprogramowania pośredniczącego](xref:fundamentals/middleware/index) serializuje głównej nazwy użytkownika w zaszyfrowanym pliku cookie. Kolejne żądania oprogramowania pośredniczącego sprawdza poprawność pliku cookie, ponownie tworzy nazwę główną i przypisuje podmiotowi zabezpieczeń na [użytkownika](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) właściwość [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Uwierzytelnianie oparte na tokenie

Gdy użytkownik jest uwierzytelniany, one wystawiony token token (nie antiforgery token). Token zawiera informacje o użytkowniku w formie [oświadczeń](/dotnet/framework/security/claims-based-identity-model) lub tokenu odwołania, który wskazuje aplikację do stanu użytkownika w aplikacji. Gdy użytkownik próbuje uzyskać dostęp do zasobu wymaga uwierzytelnienia, token jest wysyłany do aplikacji przy użyciu nagłówka autoryzacji dodatkowe w postaci tokenu elementu nośnego. To sprawia, że aplikacja bezstanowe. W kolejnych żądań token jest przekazywany w żądaniu weryfikacji po stronie serwera. Ten token nie jest *zaszyfrowanych*; ma ona *zakodowane*. Na serwerze token jest dekodowana uzyskiwać dostęp do swoich informacji. Aby wysłać token dla kolejnych żądań, należy przechowywać token w magazynie lokalnym w przeglądarce. Nie można zajmującym się ochroną CSRF luk w zabezpieczeniach, jeśli token jest przechowywany w magazynie lokalnym w przeglądarce. CSRF jest istotny, gdy token jest przechowywany w pliku cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Wiele aplikacji hostowanej w jednej domenie

Udostępnione środowiska hostingu są podatne na przejęcie kontroli sesji logowania CSRF i inne ataki.

Mimo że `example1.contoso.net` i `example2.contoso.net` różnych hostów istnieje relacja nawiązywanie niejawnych relacji zaufania między hostami w ramach `*.contoso.net` domeny. Ta relacja niejawne zaufanie umożliwia potencjalnie niezaufane hosty wpływać na siebie nawzajem pliki cookie (zasady tego samego źródła, które określają sposób wysyłanie żądań AJAX nie zawsze dotyczą plików cookie protokołu HTTP).

Można zapobiec ataków wykorzystujących zaufanych plików cookie między aplikacjami hostowanych na tej samej domenie, nie udostępniając domen. Gdy każda aplikacja jest hostowana na własnej domeny, istnieje relacja zaufania niejawnych plików cookie w celu wykorzystania.

## <a name="aspnet-core-antiforgery-configuration"></a>Konfiguracja antiforgery platformy ASP.NET Core

> [!WARNING]
> Platforma ASP.NET Core implementuje antiforgery przy użyciu [ochrony danych programu ASP.NET Core](xref:security/data-protection/introduction). Stos ochrony danych musi być skonfigurowany do pracy w farmie serwerów. Zobacz [Konfigurowanie ochrony danych](xref:security/data-protection/configuration/overview) Aby uzyskać więcej informacji.

W programie ASP.NET Core 2.0 lub nowszej [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) wprowadza antiforgery tokenów do elementów formularza HTML. Następujący kod w pliku Razor automatycznie generuje antiforgery tokenów:

```cshtml
<form method="post">
    ...
</form>
```

Podobnie [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generuje tokeny antiforgery domyślnie, jeśli metoda formularza nie GET.

Automatyczne generowanie tokenów antiforgery dla elementów formularza HTML się dzieje po `<form>` tag zawiera `method="post"` atrybut i jednej z następujących mają wartość true:

* Atrybut akcji jest pusty (`action=""`).
* Nie został dostarczony atrybut akcji (`<form method="post">`).

Można wyłączyć automatyczne generowanie tokenów antiforgery dla elementów formularza HTML:

* Jawnie wyłączyć antiforgery tokenów z informacjami o `asp-antiforgery` atrybutu:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Element formularza jest wyłączony w poziomie dla pomocników tagów przy użyciu Pomocnika tagów [! symbol rezygnacji](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Usuń `FormTagHelper` z widoku. `FormTagHelper` Może zostać usunięty z widokiem, dodając następujące dyrektywy do widoku Razor:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Strony razor](xref:razor-pages/index) są automatycznie chronione przed XSRF/CSRF. Aby uzyskać więcej informacji, zobacz [XSRF/CSRF i stron Razor](xref:razor-pages/index#xsrf).

Najbardziej typowe podejście do obrony przed atakami CSRF jest użycie *wzorzec tokenu Synchronizator* (STP). STP jest używany, gdy użytkownik zażąda strony z danymi formularza:

1. Serwer wysyła token skojarzone z tożsamością bieżącego użytkownika do klienta.
1. Klient wysyła z powrotem token do serwera w celu weryfikacji.
1. Jeśli serwer odbiera token, który nie jest zgodna tożsamość uwierzytelnionego użytkownika, żądanie zostanie odrzucone.

Token jest unikatowy i nieprzewidywalne. Token, również może służyć do zapewnienia prawidłowego sekwencjonowania serii żądań (na przykład, zapewniając sekwencji żądanie: stronie 1 &ndash; stronie 2 &ndash; strony 3). Wszystkie formularze w szablonach ASP.NET Core MVC i stron Razor generowania antiforgery tokenów. Następujące pary Wyświetl przykłady generowania antiforgery tokenów:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Jawnie dodać token antiforgery na `<form>` elementu bez używanie pomocników tagów przy użyciu Pomocnika kodu HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

We wszystkich powyższych przypadkach ASP.NET Core dodaje ukryte pole formularza podobny do następującego:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

Platforma ASP.NET Core obejmuje trzy [filtry](xref:mvc/controllers/filters) do pracy z tokenami antiforgery:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Opcje antiforgery

Dostosowywanie [antiforgery opcje](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) w `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

&dagger;Ustaw antiforgery `Cookie` właściwości za pomocą właściwości [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) klasy.

| Opcja | Opis |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Określa ustawienia używane do tworzenia antiforgery plików cookie. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Nazwa pola formularza, używany przez antiforgery system do renderowania antiforgery tokenów w widokach. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Nazwa nagłówka używany przez antiforgery systemu. Jeśli `null`, system uwzględnia tylko dane formularza. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka. Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN". Wartość domyślna to `false`. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Domena pliku cookie. Wartość domyślna to `null`. Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji. Zalecaną alternatywą jest Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Nazwa pliku cookie. Jeśli nie jest ustawiona, system generuje unikatowa nazwa zaczyna się od [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji. Zalecaną alternatywą jest Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Ścieżka zestawu w pliku cookie. Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji. Zalecaną alternatywą jest Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Nazwa pola formularza, używany przez antiforgery system do renderowania antiforgery tokenów w widokach. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Nazwa nagłówka używany przez antiforgery systemu. Jeśli `null`, system uwzględnia tylko dane formularza. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Określa, czy protokół HTTPS jest wymagany przez antiforgery system. Jeśli `true`, Niepowodzenie żądania innego niż HTTPS. Wartość domyślna to `false`. Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji. Zalecaną alternatywą jest Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka. Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN". Wartość domyślna to `false`. |

::: moniker-end

Aby uzyskać więcej informacji, zobacz [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Konfigurowanie funkcji antiforgery z IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) udostępnia interfejs API w celu skonfigurowania funkcji antiforgery. `IAntiforgery` można zażądać w `Configure` metody `Startup` klasy. W poniższym przykładzie użyto oprogramowania pośredniczącego ze strony głównej aplikacji do wygenerowania tokenu antiforgery i wysłać w odpowiedzi jako plik cookie (przy użyciu domyślnego Angular konwencji nazewnictwa opisane w dalszej części tego tematu):

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

### <a name="require-antiforgery-validation"></a>Wymagaj weryfikacji antiforgery

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) jest filtr akcji, które mogą być stosowane do poszczególnych akcji, kontrolera, lub globalnie. Żądania kierowane do akcji, dla których zastosowano ten filtr są blokowane, chyba że żądanie zawiera prawidłowy token antiforgery.

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

`ValidateAntiForgeryToken` Atrybut wymaga tokenu dla żądań kierowanych do metody akcji, rozszerza, łącznie z żądania HTTP GET. Jeśli `ValidateAntiForgeryToken` atrybut jest stosowany w kontrolerach aplikacji, może zostać zastąpiona przez `IgnoreAntiforgeryToken` atrybutu.

> [!NOTE]
> Platforma ASP.NET Core nie obsługuje automatyczne dodawanie tokenów antiforgery żądania GET.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Automatycznie weryfikować tokeny antiforgery niebezpieczne metod HTTP

Aplikacje platformy ASP.NET Core nie Generuj antiforgery tokenów dla bezpiecznych metod HTTP (GET, HEAD, opcji i śledzenia). Zamiast stosowania szeroko `ValidateAntiForgeryToken` atrybutu, a następnie przesłanianie go za pomocą `IgnoreAntiforgeryToken` atrybutów, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atrybut może być używany. Ten atrybut działa identycznie do `ValidateAntiForgeryToken` atrybutów, z tą różnicą, że nie wymaga tokeny żądań zostało nawiązane przy użyciu następujących metod HTTP:

* GET
* GŁÓWNY
* OPCJE
* TRACE

Firma Microsoft zaleca użycie `AutoValidateAntiforgeryToken` szeroko w scenariuszach bez interfejsu API. Gwarantuje to, że akcja POST są chronione domyślnie. Alternatywą jest Ignoruj antiforgery tokenów domyślnie, chyba że `ValidateAntiForgeryToken` jest stosowany do poszczególnych metod akcji. Je bardziej prawdopodobne w tym scenariuszu dla metody akcji POST pozostać niechronionej przez pomyłkę, wychodzenia z aplikacji jest narażony na ataki CSRF. Wszystkie wpisy, należy wysłać antiforgery tokenu.

Interfejsy API nie ma mechanizmu automatycznego wysyłania-cookie część tokenu. Implementacja prawdopodobnie zależy od implementacji kodu klienta. Poniżej przedstawiono kilka przykładów:

Przykład na poziomie klasy:

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

### <a name="override-global-or-controller-antiforgery-attributes"></a>Zastąpienie globalny lub kontroler antiforgery atrybutów

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtr jest używany, aby wyeliminować konieczność stosowania antiforgery token dla danej akcji (lub kontroler). Po zastosowaniu tego filtru zastępuje `ValidateAntiForgeryToken` i `AutoValidateAntiforgeryToken` filtry określone na wyższym poziomie (globalnie lub na innym kontrolerze).

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

Tokeny powinny być odświeżane po użytkownik jest uwierzytelniany przez przekierowanie użytkownika do widoku lub strony stron Razor.

## <a name="javascript-ajax-and-spas"></a>JavaScript, wywołań AJAX i aplikacji jednostronicowych

W tradycyjnych aplikacji opartych na języku HTML antiforgery tokeny są przekazywane do serwera przy użyciu pól formularza. Nowoczesne aplikacje oparte na języku JavaScript i aplikacji jednostronicowych wielu żądań programowo. Te żądania AJAX może używać innych technik (takie jak nagłówki żądania i pliki cookie) w celu wysyłania tokenu.

Pliki cookie są używane do przechowywania tokenów uwierzytelniania i do uwierzytelniania żądań interfejsu API na serwerze, CSRF czy potencjalny problem. Jeśli Magazyn lokalny jest używany do przechowywania tokenu, CSRF luk w zabezpieczeniach może zminimalizować, ponieważ wartości z magazynu lokalnego nie są automatycznie przesyłane do serwera z każdym żądaniem. W związku z tym przy użyciu lokalnego magazynu do przechowywania antiforgery token na kliencie oraz wysyłania tokenu, ponieważ nagłówek żądania jest zalecanym podejściem.

### <a name="javascript"></a>JavaScript

Przy użyciu języka JavaScript za pomocą widoków, token mogą być tworzone przy użyciu usługi z w widoku. Wstrzykiwanie [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) usługi do wyświetlać i wywoływać [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

To podejście eliminuje konieczność łączenia się bezpośrednio z ustawienia plików cookie z serwera lub ich odczytywania od klienta.

Poprzedni przykład wykorzystuje JavaScript ma zostać odczytana wartość ukrytego pola nagłówka AJAX żądań POST.

JavaScript można również dostęp do tokenów w plikach cookie i użyj zawartość pliku cookie, aby utworzyć nagłówek, wartością tokenu.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Zakładając, że skrypt żądań wysyłania tokenu w zawiera nagłówek o nazwie `X-CSRF-TOKEN`, skonfigurować usługę antiforgery do wyszukania `X-CSRF-TOKEN` nagłówka:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

W poniższym przykładzie użyto języka JavaScript, aby utworzyć żądanie AJAX przy użyciu odpowiedniego nagłówka:

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

Moduł AngularJS używa konwencji adres CSRF. Jeśli serwer wysyła plik cookie o nazwie `XSRF-TOKEN`, AngularJS `$http` usługa dodaje wartość pliku cookie do nagłówka, gdy wysyła żądanie do serwera. Ten proces odbywa się automatycznie. Nagłówek nie musi jawnie ustawiona w kliencie. Nazwa nagłówka jest `X-XSRF-TOKEN`. Serwer powinna wykryć ten nagłówek i zweryfikować jego zawartość.

Dla interfejsu API platformy ASP.NET Core pracować z niniejszej Konwencji, w przypadku uruchamiania aplikacji:

* Skonfiguruj aplikację, aby dostarczyć token w pliku cookie o nazwie `XSRF-TOKEN`.
* Konfigurowanie usługi antiforgery do wyszukania nagłówka o nazwie `X-XSRF-TOKEN`.

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

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Rozszerzanie antiforgery

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typu umożliwia deweloperom rozszerzania działania systemu anti-CSRF przez obustronne konwertowanie w każdy token dodatkowe dane. [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) metoda jest wywoływana za każdym razem generowania tokenu pola i wartość zwracana jest osadzony w generowanych token. Implementujący może zwrócić sygnaturę czasową, identyfikatora jednorazowego lub dowolna inna wartość, a następnie wywołać [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) do sprawdzania poprawności danych podczas weryfikowania tokenu. Nazwa użytkownika klienta już jest osadzony w generowanych tokenach, więc nie trzeba wpisywać te informacje. Jeśli token zawiera dane dodatkowe, ale nie `IAntiForgeryAdditionalDataProvider` jest skonfigurowany, dane dodatkowe nie jest zweryfikowany.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page) (OWASP).
* <xref:host-and-deploy/web-farm>
