---
title: "Zapobieganie atakom sfałszowaniem (XSRF/CSRF) żądania Międzywitrynowego na platformie ASP.NET Core"
author: steve-smith
description: "Zapobieganie atakom sfałszowaniem (XSRF/CSRF) żądania Międzywitrynowego na platformie ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: e076e301004c04b5c516d775353a4b6e50a3f36e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Zapobieganie atakom sfałszowaniem (XSRF/CSRF) żądania Międzywitrynowego na platformie ASP.NET Core

[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), i [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>Jakie ataku uniemożliwia zabezpieczający przed sfałszowaniem?

Fałszowanie żądań między witrynami (znanej także jako XSRF lub CSRF, Wymowa *surf zobacz*) jest atak wykorzystujący aplikacje obsługiwane w sieci web, zgodnie z którymi złośliwa witryna sieci web może mieć wpływ interakcji między przeglądarką klienta i witryny sieci web, które ufają Przeglądarka. Tego rodzaju ataki są możliwe, ponieważ przeglądarki sieci web wysyłać niektóre typy tokenów uwierzytelniania automatycznie z każdego żądania do witryny sieci web. Ten formularz wykorzystać do znanej także jako *ataku jednym kliknięciem* lub jako *sesji jazda*, ponieważ wykorzystuje ataku użytkownika wcześniej uwierzytelniona sesji.

Przykład atak CSRF:

1. Użytkownicy logujący się do `www.example.com`, uwierzytelnianie formularzy przy użyciu.
2. Serwer uwierzytelnia użytkownika i generuje odpowiedzi, który zawiera plik cookie uwierzytelniania.
3. Użytkownik odwiedzi złośliwą witrynę.

   Złośliwa witryna zawiera formularza HTML podobny do następującego:

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

Należy zauważyć, że akcja formularza zapisuje do lokacji narażony, nie niebezpiecznej witryny. Jest to część CSRF "cross-site".

4. Użytkownik klika przycisk Prześlij. Przeglądarka automatycznie uwzględnia pliku cookie uwierzytelniania dla żądanej domeny (narażone lokacji w tym przypadku) z żądaniem.
5. Żądanie działa na serwerze z kontekstem uwierzytelniania użytkownika i mogą wykonywać żadnych czynności, że uwierzytelniony użytkownik może wykonywać.

W tym przykładzie wymaga od użytkownika kliknij przycisk formularza. Strony złośliwych można:

* Uruchom skrypt, który automatycznie wyśle formularz.
* Wysyła żądanie AJAX przesyłania formularza. 
* Formularz ukryty z CSS. 

Przy użyciu protokołu SSL nie uniemożliwia atak CSRF, złośliwa witryna można wysyłać `https://` żądania. 

Niektóre ataki docelowych punktów końcowych witryny, które odpowiadają na `GET` żądania, w których przypadku tag obrazu może służyć do wykonywania akcji (Ta forma ataku jest typowe na forum witryn, które zezwala na obrazy, ale zablokowanie JavaScript). Aplikacje, które spowodują zmianę stanu z `GET` żądania są narażone przed złośliwymi atakami.

Ataki CSRF są możliwe do przed witryn sieci web, które używają plików cookie do uwierzytelniania, ponieważ przeglądarki wysyłać wszystkich odpowiednich plików cookie do docelowej witryny sieci web. Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane również są zagrożone. Po zalogowaniu się użytkownika za pomocą uwierzytelnianie podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia zakończenia sesji.

Uwaga: W tym kontekście *sesji* odwołuje się do sesji po stronie klienta, w którym użytkownik jest uwierzytelniony. Jest niezwiązanych ze sobą do sesji po stronie serwera lub [oprogramowanie pośredniczące sesji](xref:fundamentals/app-state).

Użytkownicy mogą ochronić CSRF luki w zabezpieczeniach poprzez:
* Wylogowując się witryn sieci web, gdy zostało ukończone z nich korzystać.
* Okresowo wyczyszczenie plików cookie w przeglądarce.

Jednak luk w zabezpieczeniach CSRF są zasadniczo problem z aplikacji sieci web, a nie użytkownika końcowego.

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>W jaki sposób program ASP.NET Core MVC uwzględnia CSRF

> [!WARNING]
> Implementuje platformy ASP.NET Core za pomocą przeciwko request forgery [stosu ochrony danych platformy ASP.NET Core](xref:security/data-protection/introduction). Ochrona danych platformy ASP.NET Core musi być skonfigurowana do pracy w farmie serwerów. Zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview) Aby uzyskać więcej informacji.

Konfiguracja ochrony danych przeciwko request forgery domyślnego platformy ASP.NET Core 

W programie ASP.NET 2.0 MVC Core [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML. Na przykład następujący kod w pliku Razor automatycznie wygeneruje tokenów zabezpieczających przed sfałszowaniem:

```html
<form method="post">
  <!-- form markup -->
</form>
```

Automatyczne generowanie tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML się stanie, gdy:

* `form` Tag zawiera `method="post"` atrybutu i

  * Atrybut akcji jest pusty. ( `action=""`) OR
  * Nie jest podany w atrybucie akcji. (`<form method="post">`)

Możesz wyłączyć automatyczne generowanie tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML przez:

* Jawnie wyłącza `asp-antiforgery`. Na przykład

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* OPT element form poza pomocników tagów przy użyciu Pomocnika tagów [! symbol Wypisz](xref:mvc/views/tag-helpers/intro#opt-out).

 ```html
  <!form method="post">
  </!form>
  ```

* Usuń `FormTagHelper` z widoku. Możesz usunąć `FormTagHelper` z widoku przez dodanie do widoku Razor następujące dyrektywy:

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Stron razor](xref:mvc/razor-pages/index) są automatycznie chronione przed XSRF/CSRF. Nie trzeba zapisać jakiegokolwiek dodatkowego kodu. Zobacz [XSRF/CSRF i stron Razor](xref:mvc/razor-pages/index#xsrf) Aby uzyskać więcej informacji.

Najbardziej typowym podejściem do obrony przed atakami CSRF jest wzorzec tokenu Synchronizatora (STP). STP to technika używany, gdy użytkownik zażąda strony z danych formularza. Serwer wysyła token skojarzone z tożsamością bieżącego użytkownika do klienta. Klient wysyła ponownie token do serwera w celu weryfikacji. Jeśli serwer odbiera token, który nie odpowiada tożsamość uwierzytelnionego użytkownika, żądanie zostanie odrzucone. Token jest unikatowy i nieprzewidywalny. Token mogą służyć do zapewnienia prawidłowego sekwencjonowania szereg żądań (strona zapewnienie 1 poprzedza strona 2 poprzedza strony 3). Wszystkich formularzy w szablonach platformy ASP.NET Core MVC generowania antiforgery tokenów. Dwa poniższe przykłady logiki widoku generowania tokenów antiforgery:

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

Należy jawnie dodać antiforgery token do ``<form>`` elementu bez użycia pomocników tagów z Pomocnika kodu HTML ``@Html.AntiForgeryToken``:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

W każdym z powyższych przypadków platformy ASP.NET Core doda ukryte pole formularza podobny do następującego:
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

Platformy ASP.NET Core obejmuje trzy [filtry](xref:mvc/controllers/filters) do pracy z tokenami antiforgery: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, i ``IgnoreAntiforgeryToken``.

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

``ValidateAntiForgeryToken`` Jest filtr akcji, który można zastosować do poszczególnych akcji kontrolera, lub globalnie. Żądania kierowane do akcji, które mają ten filtr zostanie zablokowane, jeśli żądanie zawiera prawidłowy token antiforgery.

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

``ValidateAntiForgeryToken`` Atrybut wymaga tokenu dla żądań kierowanych do metod akcji go decorates tym `HTTP GET` żądań. W przypadku zastosowania go szeroko, można zastąpić go przy użyciu ``IgnoreAntiforgeryToken`` atrybutu.

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

Ogólnie rzecz biorąc aplikacji platformy ASP.NET Core nie generować antiforgery tokeny dla bezpieczne metody HTTP (GET, HEAD, opcje i śledzenia). Zamiast stosowania szeroko ``ValidateAntiForgeryToken`` atrybutu, a następnie przesłanianie go z ``IgnoreAntiforgeryToken`` atrybutów, można użyć ``AutoValidateAntiforgeryToken`` atrybutu. Ten atrybut działa identycznie do ``ValidateAntiForgeryToken`` atrybutów, z wyjątkiem tego, czy nie wymaga tokeny żądań zostało nawiązane przy użyciu następujących metod HTTP:

* GET
* HEAD
* OPCJE
* TRACE

Zalecane jest użycie ``AutoValidateAntiforgeryToken`` szeroko dla scenariuszy-API. Dzięki temu akcji POST są chronione przez domyślny. Alternatywą jest Ignoruj antiforgery tokeny domyślnie, chyba że ``ValidateAntiForgeryToken`` jest stosowany do metody akcji indywidualnych. Bardziej prawdopodobne, w tym scenariuszu dla metody akcji POST się po lewej niechronionej, pozostawiając narażony na ataki CSRF aplikacji. Nawet anonimowe WPISÓW, należy wysłać antiforgery tokenu.

Uwaga: Interfejsów API nie mają mechanizm automatycznego wysyłania-cookie część tokenu; prawdopodobnie implementacji zależy od implementacji kodu klienta. Poniżej przedstawiono kilka przykładów.


Przykład (klasa poziom):

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Przykład (globalne):

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

``IgnoreAntiforgeryToken`` Użyć filtru, aby wyeliminować potrzebę antiforgery token obecności dla danej akcji (lub kontrolera). Po zastosowaniu filtru spowoduje zastąpienie ``ValidateAntiForgeryToken`` i/lub ``AutoValidateAntiforgeryToken`` filtry określone na wyższym poziomie (globalnie lub w kontrolerze).

```c#
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

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX i źródła

W aplikacjach opartych na języku HTML tradycyjnych antiforgery tokenów są przekazywane do serwera przy użyciu pól ukrytym. Nowoczesne aplikacje oparte na języku JavaScript i aplikacje jednej strony (źródła) składają się programowo wiele żądań. Te żądania AJAX mogą używać innych technik (np. nagłówki żądania lub pliki cookie) do wysyłania tokenu. Jeśli pliki cookie są używane do przechowywania tokeny uwierzytelniania i do uwierzytelniania żądań interfejsu API na serwerze, CSRF będzie potencjalny problem. Jednak jeśli magazyn lokalny jest używany do przechowywania token, CSRF luki w zabezpieczeniach mogą można zminimalizować, ponieważ wartości z magazynu lokalnego nie są automatycznie wysyłane do serwera z każdego nowego żądania. W związku z tym korzystanie z magazynu lokalnego do przechowywania antiforgery token na kliencie i wysyłania tokenu, ponieważ nagłówek żądania jest zalecane podejście.

### <a name="angularjs"></a>AngularJS

AngularJS używa konwencji adres CSRF. Jeśli serwer wysyła plik cookie o nazwie ``XSRF-TOKEN``, kątową ``$http`` usługi doda wartość z tego pliku cookie do nagłówka podczas wysyłania żądania do tego serwera. Ten proces odbywa się automatycznie; nie musisz jawnie ustawić nagłówka. Nazwa nagłówka jest ``X-XSRF-TOKEN``. Serwer powinna wykryć ten nagłówek i zweryfikować jego zawartość.

Dla interfejsu API platformy ASP.NET Core współpracują z tę Konwencję:

* Skonfiguruj aplikację, aby dostarczyć token w pliku cookie o nazwie``XSRF-TOKEN``
* Skonfiguruj usługę antiforgery do wyszukania nagłówek o nazwie``X-XSRF-TOKEN``

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Przykładowy widok](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).

### <a name="javascript"></a>JavaScript

Przy użyciu języka JavaScript z widokami, można utworzyć tokenu przy użyciu usługi z w danym widoku. Aby to zrobić, Wstaw `Microsoft.AspNetCore.Antiforgery.IAntiforgery` usługi do widoku i wywołanie `GetAndStoreTokens`, jak pokazano:

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

Takie podejście eliminuje potrzebę bezpośrednio przez ustawienia plików cookie z serwera lub odczytywania ich z klienta.

JavaScript można także uzyskać dostęp do tokenów podane w plikach cookie, a następnie użyj zawartość pliku cookie do tworzenia nagłówka o wartości tokenu, jak pokazano poniżej.

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Następnie, przy założeniu utworzenia skryptu żądania do wysyłania tokenu w nagłówku o nazwie ``X-CSRF-TOKEN``, skonfiguruj usługę antiforgery do wyszukania ``X-CSRF-TOKEN`` nagłówka:

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

W poniższym przykładzie użyto jQuery żądania AJAX z odpowiedni nagłówek:

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>Konfigurowanie Antiforgery

`IAntiforgery`udostępnia interfejs API do skonfigurowania antiforgery systemu. Może być wymagane w `Configure` metody `Startup` klasy. W poniższym przykładzie użyto oprogramowanie pośredniczące ze strony głównej aplikacji do wygenerowania tokenu antiforgery i wysłać go w odpowiedzi jako plik cookie (przy użyciu domyślnego kątowego konwencji nazewnictwa opisane powyżej):


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>Opcje

Można dostosować [antiforgery opcje](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) w `ConfigureServices`:

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|Opcja        | Opis |
|------------- | ----------- |
|CookieDomain  | Domena pliku cookie. Domyślnie `null`. |
|CookieName    | Nazwa pliku cookie. Jeśli nie jest ustawiona, system wygeneruje unikatowa nazwa zaczyna się od `DefaultCookiePrefix` (". AspNetCore.Antiforgery."). |
|CookiePath    | Ścieżka ustawiona w pliku cookie. |
|FormFieldName | Nazwa ukryte pole formularza używany przez antiforgery system do renderowania antiforgery tokenów w widokach. |
|HeaderName    | Nazwa nagłówka używany przez antiforgery system. Jeśli `null`, system będzie uwzględniać tylko dane formularza. |
|RequireSsl    | Określa, czy protokół SSL jest wymagany przez antiforgery system. Domyślnie `false`. Jeśli `true`, żądania bez użycia protokołu SSL zakończy się niepowodzeniem. |
|SuppressXFrameOptionsHeader  | Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka. Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN". Domyślnie `false`. |

Zobacz https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Aby uzyskać więcej informacji.

### <a name="extending-antiforgery"></a>Rozszerzanie Antiforgery

[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typu umożliwia deweloperom rozszerzyć zachowanie systemu anti-XSRF przez dwustronną komunikację w każdym token dodatkowe dane. [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) metoda jest wywoływana za każdym razem zostanie wygenerowany token pola i wartość zwracana jest osadzony w wygenerowany token. Realizator może zwracać sygnaturę czasową, identyfikatora jednorazowego lub wszelkie inne wartości, a następnie wywołać [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) do sprawdzania poprawności danych podczas weryfikowania tokenu. Nazwa użytkownika klienta już jest osadzony w generowanych tokenach, więc nie trzeba wpisywać te informacje. Jeśli token zawiera dane dodatkowe, ale nie `IAntiForgeryAdditionalDataProvider` został skonfigurowany, dane dodatkowe nie jest zweryfikowany.

## <a name="fundamentals"></a>Podstawy

CSRF ataki polegają na domyślne zachowanie przeglądarki przesyłać pliki cookie skojarzone z domeną z wszystkie żądania skierowane do tej domeny. Te pliki cookie są przechowywane w przeglądarce. Często zawierają one plików cookie sesji dla uwierzytelnionych użytkowników. Plik cookie uwierzytelniania jest popularnych formy uwierzytelniania. Systemami uwierzytelniania opartego na tokenie ma zostały rośnie w popularne, szczególnie w przypadku źródła i innych scenariuszy "inteligentne klienta".

### <a name="cookie-based-authentication"></a>Uwierzytelnianie na podstawie plików cookie

Gdy użytkownik został uwierzytelniony przy użyciu nazwy użytkownika i hasła, ich jest wystawiony token, który może służyć do ich identyfikacji i weryfikacji, czy zostały uwierzytelnione. Token jest przechowywany jako sprawia, że plik cookie dołączona każde żądanie klienta. Generowanie i sprawdzanie poprawności ten plik cookie odbywa się przez oprogramowanie pośredniczące uwierzytelniania plików cookie. Platformy ASP.NET Core zawiera plik cookie [oprogramowanie pośredniczące](../fundamentals/middleware.md) co serializuje głównej nazwy użytkownika do zaszyfrowanego pliku cookie i kolejne żądania sprawdza poprawność pliku cookie, odtwarza podmiot zabezpieczeń i przypisuje go do `User` właściwości `HttpContext`.

Gdy używany jest plik cookie, plik cookie uwierzytelniania jest tylko kontenerem dla biletu uwierzytelniania formularzy. Bilet jest przekazywany jako wartość pliku cookie uwierzytelniania formularzy z każdym żądaniem i jest używany przez uwierzytelnianie formularzy, na serwerze, aby zidentyfikować uwierzytelnionego użytkownika.

Gdy użytkownik jest zalogowany do systemu, sesja użytkownika jest tworzony po stronie serwera i są przechowywane w bazie danych lub innych magazynu trwałego. System generuje klucz sesji wskazujące rzeczywiste sesji w magazynie danych i są wysyłane jako plik cookie po stronie klienta. Serwer sieci web będzie sprawdzać ten klucz sesji żadnych żądaniem użytkownika z zasobem, który wymaga autoryzacji. System sprawdza, czy sesja skojarzonego użytkownika ma uprawnienia dostępu do żądanego zasobu. Jeśli tak, nadal żądania. W przeciwnym razie żądania zwraca w postaci nie autoryzowanych. W takie podejście pliki cookie służą do tworzenia aplikacji jest stanowa, ponieważ jest w stanie "Pamiętaj" który użytkownik wcześniej uwierzytelnił się z serwerem.

### <a name="user-tokens"></a>Tokeny użytkownika

Uwierzytelnianie na podstawie tokenu nie przechowuje sesji na serwerze. Gdy użytkownik jest zalogowany, są one wystawiony token (nie antiforgery token). Token ten zawiera dane, które są wymagane do weryfikacji tokenu. Zawiera także informacje o użytkowniku w formie [oświadczeń](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model). Gdy użytkownik chce, aby uzyskać dostęp do zasobów serwera wymaga uwierzytelnienia, token jest wysyłany na serwer z nagłówkiem dodatkowe autoryzacji w formie {tokenu} elementu nośnego. Dzięki temu aplikacji bezstanowych, ponieważ w kolejnych żądań token jest przekazywany w żądania do weryfikacji po stronie serwera. Ten token nie jest *zaszyfrowanych*; jest raczej *zakodowane*. Po stronie serwera token może zostać odczytany na dostęp do nieprzetworzonej informacji w tokenie. Aby wysłać token w kolejnych żądań, albo zapisz go w magazynie lokalnym w przeglądarce lub w pliku cookie. Nie martw się o XSRF luki w zabezpieczeniach, jeśli token jest przechowywany w magazynie lokalnym, ale jest to problem, jeśli token jest przechowywany w pliku cookie.

### <a name="multiple-applications-are-hosted-in-one-domain"></a>Wiele aplikacji znajdują się w jednej domenie

Mimo że `example1.cloudapp.net` i `example2.cloudapp.net` są różnych hostach, ma zależności nawiązywanie niejawnych relacji zaufania między hostami w obszarze `*.cloudapp.net` domeny. Ta relacja zaufania niejawne umożliwia potencjalnie niezaufane hosty wpływa na siebie nawzajem pliki cookie (zasad tego samego źródła, które będą zarządzały sposobem żądania AJAX nie zawsze dotyczą plików cookie protokołu HTTP). Środowisko uruchomieniowe platformy ASP.NET Core zawiera pewne środki zaradcze, nazwa użytkownika jest osadzony w tokenie pola. Nawet jeśli poddomeny złośliwego może zastąpić tokenu sesji, nie może generować prawidłowe pole token dla użytkownika. W przypadku hostowania w takim środowisku, procedury wbudowanych anti-XSRF nadal nie obrony przed przejęcie kontroli sesji lub logowania CSRF ataków. Udostępnionych środowiskach macierzystych są vunerable przejęcie kontroli sesji logowania CSRF i inne ataki.

### <a name="additional-resources"></a>Dodatkowe zasoby

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page) (OWASP).
