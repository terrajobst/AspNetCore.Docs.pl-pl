---
title: ASP.NET Core oprogramowanie pośredniczące
author: rick-anderson
description: Dowiedz się więcej na temat ASP.NET Core oprogramowania pośredniczącego i potoku żądania.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2020
uid: fundamentals/middleware/index
ms.openlocfilehash: 6698e269e0a6480cd5a03c59f9a19da31e23bf69
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089152"
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core oprogramowanie pośredniczące

::: moniker range=">= aspnetcore-3.0"

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)

Oprogramowanie pośredniczące jest oprogramowaniem, które jest połączone z potokiem aplikacji w celu obsługi żądań i odpowiedzi. Każdy składnik:

* Wybiera, czy żądanie ma zostać przekazane do następnego składnika w potoku.
* Może wykonać prace przed i po następnym składniku potoku.

Delegaty żądań są używane do kompilowania potoku żądania. Delegaty żądań obsługują każde żądanie HTTP.

Delegatów żądań konfiguruje się za pomocą metod rozszerzenia <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>i <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. Osobnego delegata żądania można określić jako metodę anonimową (nazywaną w wierszu) lub można ją zdefiniować w klasie wielokrotnego użytku. Te klasy wielokrotnego użytku i metody anonimowe w wierszu to *oprogramowanie pośredniczące*, nazywane również *składnikami oprogramowania pośredniczącego*. Każdy składnik pośredniczący w potoku żądania jest odpowiedzialny za wywoływanie następnego składnika w potoku lub krótkiego obwodu potoku. W przypadku krótkiego obwodu oprogramowanie pośredniczące nosi nazwę *oprogramowania pośredniczącego terminalu* , ponieważ zapobiega przetwarzaniu żądania przez dalsze oprogramowanie pośredniczące.

<xref:migration/http-modules> objaśnia różnicę między potokami żądań w ASP.NET Core i ASP.NET 4. x i oferuje dodatkowe przykłady oprogramowania pośredniczącego.

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder

Potok żądania ASP.NET Core składa się z sekwencji delegatów żądań o nazwie jeden po drugim. Na poniższym diagramie przedstawiono koncepcję. Wątek wykonywania jest zgodny z czarnym strzałką.

![Wzorzec przetwarzania żądań pokazujący żądanie docierające do, przetwarzania przez trzy middlewares i odpowiedzi opuszczających aplikację. Każde oprogramowanie pośredniczące uruchamia swoją logikę i przekazuje żądanie do następnego oprogramowania pośredniczącego przy następnej instrukcji (). Po przetworzeniu żądania przez trzecie oprogramowanie pośredniczące żądanie przechodzi z powrotem przez poprzednie dwie middlewares w odwrotnej kolejności, aby można było je przetwarzać po kolejne instrukcje () przed opuszczeniem aplikacji jako odpowiedzi na klienta.](index/_static/request-delegate-pipeline.png)

Każdy delegat może wykonać operacje przed i po następnym delegacie. Delegaty obsługi wyjątków powinny być wywoływane wczesnie w potoku, dlatego mogą przechwytywać wyjątki, które występują w późniejszych etapach potoku.

Najprostszym możliwym ASP.NET Core aplikacji jest skonfigurowanie pojedynczego delegata żądania, który obsługuje wszystkie żądania. Ten przypadek nie obejmuje rzeczywistego potoku żądania. Zamiast tego funkcja pojedynczej anonimowej jest wywoływana w odpowiedzi na każde żądanie HTTP.

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

Łączenie wielu delegatów żądań z <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. `next` parametr reprezentuje następny delegat w potoku. Potok można skrócić, *nie* wywołując *następnego* parametru. Zazwyczaj można wykonywać akcje zarówno przed, jak i po następnym delegatze, jak pokazano w poniższym przykładzie:

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=5-10)]

Gdy delegat nie przekazuje żądania do następnego delegata, jest on nazywany *krótkim obwodem potoku żądania*. Krótki obwód jest często pożądany, ponieważ pozwala uniknąć niepotrzebnej pracy. Na przykład [statyczne oprogramowanie pośredniczące](xref:fundamentals/static-files) może działać jako *oprogramowanie pośredniczące terminalu* przez przetwarzanie żądania dla pliku statycznego i krótkiego obwodu pozostałej części potoku. Oprogramowanie pośredniczące dodane do potoku zanim oprogramowanie pośredniczące, które zakończy dalsze przetwarzanie, nadal przetwarza kod po instrukcji `next.Invoke`. Należy jednak zapoznać się z poniższym ostrzeżeniem dotyczącym próby zapisu do odpowiedzi, która została już wysłana.

> [!WARNING]
> Nie wywołuj `next.Invoke` po wysłaniu odpowiedzi do klienta. Zmiany w <xref:Microsoft.AspNetCore.Http.HttpResponse> po rozpoczęciu zgłaszania wyjątku przez odpowiedź. Na przykład zmiany, takie jak nagłówki ustawień i kod stanu zgłaszają wyjątek. Zapisywanie w treści odpowiedzi po wywołaniu `next`:
>
> * Może spowodować naruszenie protokołu. Na przykład pisanie więcej niż podane `Content-Length`.
> * Może uszkodzić format treści. Na przykład, pisząc stopkę HTML do pliku CSS.
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> jest przydatną wskazówką wskazującą, czy nagłówki zostały wysłane lub w których zapisano treść.

<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegatów nie otrzymają parametru `next`. Pierwszy delegat `Run` jest zawsze terminalem i przerywa potoku. `Run` jest konwencją. Niektóre składniki pośredniczące mogą uwidaczniać `Run[Middleware]` metody, które są uruchamiane na końcu potoku:

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=12-15)]

W poprzednim przykładzie `Run` delegat zapisuje `"Hello from 2nd delegate."` do odpowiedzi, a następnie kończy potok. Jeśli po delegatze `Run` zostanie dodany inny `Use` lub delegat `Run`, nie jest on wywoływany.

<a name="order"></a>

## <a name="middleware-order"></a>Zamówienie oprogramowania pośredniczącego

Kolejność, w jakiej składniki pośredniczące są dodawane w metodzie `Startup.Configure` definiuje kolejność, w jakiej składniki pośredniczące są wywoływane na żądaniach i odwrotnej kolejności odpowiedzi. Kolejność ma **kluczowe znaczenie** dla bezpieczeństwa, wydajności i funkcjonalności.

Poniższa metoda `Startup.Configure` dodaje składniki pośredniczące powiązane z zabezpieczeniami w zalecanej kolejności:

[!code-csharp[](index/snapshot/StartupAll3.cs?name=snippet)]

W powyższym kodzie:

* Oprogramowanie pośredniczące, które nie jest dodawane podczas tworzenia nowej aplikacji sieci Web z [kontami poszczególnych użytkowników](xref:security/authentication/identity) , jest oznaczone jako komentarz.
* Nie każde oprogramowanie pośredniczące musi przejść do tej dokładnej kolejności, ale wiele do. Na przykład `UseCors`, `UseAuthentication`i `UseAuthorization` muszą przejść w podanej kolejności.

Poniższa metoda `Startup.Configure` dodaje składniki pośredniczące dla typowych scenariuszy aplikacji:

1. Obsługa wyjątków/błędów
   * Gdy aplikacja jest uruchamiana w środowisku deweloperskim:
     * Oprogramowanie pośredniczące strony wyjątków dla deweloperów (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) zgłasza błędy środowiska uruchomieniowego aplikacji.
     * Strona błędu bazy danych raporty pośredniczące — błędy środowiska uruchomieniowego bazy danych.
   * Gdy aplikacja jest uruchamiana w środowisku produkcyjnym:
     * Program obsługi wyjątków — oprogramowanie pośredniczące (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) przechwytuje wyjątki zgłoszone w następujących middlewares.
     * Oprogramowanie pośredniczące protokołu HTTP Strict Transport Security (HSTS) (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) dodaje nagłówek `Strict-Transport-Security`.
1. Oprogramowanie pośredniczące przekierowania HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) przekierowuje żądania HTTP do protokołu HTTPS.
1. Oprogramowanie pośredniczące plików statycznych (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) zwraca pliki statyczne i dalsze przetwarzanie żądań na krótkie obwody.
1. Oprogramowanie pośredniczące zasad plików cookie (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) jest zgodne z przepisami dla Ogólne rozporządzenie o ochronie danych UE (Rodo).
1. Kierowanie oprogramowania pośredniczącego (`UseRouting`) do przesyłania żądań.
1. Oprogramowanie pośredniczące uwierzytelniania (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) próbuje uwierzytelnić użytkownika przed zezwoleniem na dostęp do zabezpieczonych zasobów.
1. Oprogramowanie pośredniczące autoryzacji (`UseAuthorization`) autoryzuje użytkownika do uzyskiwania dostępu do zabezpieczonych zasobów.
1. Oprogramowanie pośredniczące sesji (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) ustanawia i utrzymuje stan sesji. Jeśli aplikacja używa stanu sesji, wywołaj oprogramowanie pośredniczące sesji po wyjściu z zasad plików cookie i przed oprogramowania MVC.
1. Oprogramowanie pośredniczące routingu punktu końcowego (`UseEndpoints` z `MapRazorPages`), aby dodać Razor Pages punkty końcowe do potoku żądania.

<!--

FUTURE UPDATE

On the next topic overhaul/release update, add API crosslink to "Database Error Page Middleware" in Item 1 of the list ...

Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*

... when available via the API docs.

-->

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseRouting();
    app.UseAuthentication();
    app.UseAuthorization();
    app.UseSession();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

W powyższym przykładowym kodzie każda Metoda rozszerzenia oprogramowania pośredniczącego jest uwidaczniana na <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> za pomocą przestrzeni nazw <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> jest pierwszym składnikiem pośredniczącym dodanym do potoku. W związku z tym, oprogramowanie pośredniczące programu obsługi wyjątków przechwytuje wszystkie wyjątki występujące w późniejszych wywołaniach.

Oprogramowanie pośredniczące plików statycznych jest wczesnie wywoływane w potoku, dzięki czemu może obsługiwać żądania i krótkie obwód bez przechodzenia przez pozostałe składniki. Oprogramowanie pośredniczące plików statycznych **nie zapewnia żadnych** kontroli autoryzacji. Wszystkie pliki obsługiwane przez oprogramowanie pośredniczące plików statycznych, w tym w katalogu *wwwroot*, są publicznie dostępne. Aby zapoznać się z podejściem do zabezpieczania plików statycznych, zobacz <xref:fundamentals/static-files>.

Jeśli żądanie nie jest obsługiwane przez oprogramowanie pośredniczące pliku statycznego, jest ono przesyłane do oprogramowania pośredniczącego uwierzytelniania (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), które wykonuje uwierzytelnianie. Uwierzytelnianie nie ma krótkoterminowych żądań nieuwierzytelnionych. Chociaż uwierzytelnianie pośredniczące uwierzytelnia żądania, autoryzacja (i odrzucanie) występuje tylko po zaznaczeniu określonej strony Razor lub kontrolera MVC i akcji.

Poniższy przykład ilustruje kolejność oprogramowania pośredniczącego, w którym żądania plików statycznych są obsługiwane przez oprogramowanie pośredniczące plików statycznych przed użyciem oprogramowania pośredniczącego kompresji. Pliki statyczne nie są kompresowane z tą kolejnością oprogramowania. Razor Pages odpowiedzi można skompresować.

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

W przypadku aplikacji jednostronicowych oprogramowanie pośredniczące SPA <xref:Microsoft.Extensions.DependencyInjection.SpaStaticFilesExtensions.UseSpaStaticFiles*> zwykle jest ostatnim węzłem potoku oprogramowania pośredniczącego. Oprogramowanie pośredniczące SPA jest ostatnie:

* Aby zezwolić wszystkim innym middlewares na odpowiadanie na pasujące żądania.
* Aby umożliwić uruchamianie aplikacji jednostronicowych z routingiem po stronie klienta dla wszystkich tras, które nie są rozpoznawane przez aplikację serwera.

Aby uzyskać więcej informacji na temat aplikacji jednostronicowych, zapoznaj się z przewodnikami dla szablonów projektów [reagowanie](xref:spa/react) i [kątowych](xref:spa/angular) .

## <a name="branch-the-middleware-pipeline"></a>Rozgałęzianie potoku oprogramowania pośredniczącego

rozszerzenia <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> są używane jako konwencja rozgałęziania potoku. `Map` rozgałęziać potok żądania na podstawie dopasowań podanej ścieżki żądania. Jeśli ścieżka żądania rozpoczyna się od podaną ścieżką, rozgałęzienie jest wykonywane.

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu.

| Żądanie             | Odpowiedź                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Witaj od delegata innego niż mapowanie. |
| localhost:1234/map1 | Test mapy 1                   |
| localhost:1234/map2 | Test mapy 2                   |
| localhost:1234/map3 | Witaj od delegata innego niż mapowanie. |

Gdy jest używana `Map`, dopasowane segmenty ścieżki są usuwane z `HttpRequest.Path` i dołączane do `HttpRequest.PathBase` dla każdego żądania.

`Map` obsługuje zagnieżdżanie, na przykład:

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
```

`Map` można również dopasować wiele segmentów jednocześnie:

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

<xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> rozgałęziać potok żądania na podstawie wyniku danego predykatu. Dowolny predykat typu `Func<HttpContext, bool>` może służyć do mapowania żądań do nowej gałęzi potoku. W poniższym przykładzie predykat służy do wykrywania obecności zmiennej ciągu zapytania `branch`:

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?highlight=14-15)]

W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu:

| Żądanie                       | Odpowiedź                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Witaj od delegata innego niż mapowanie. |
| localhost:1234/?branch=master | Używane gałęzie = Master         |

<xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*> również rozgałęziać potok żądania na podstawie wyniku danego predykatu. W przeciwieństwie do `MapWhen`, rozgałęzienie jest ponownie przyłączone do głównego potoku, jeśli ma on krótki obwód lub zawiera oprogramowanie pośredniczące terminalu:

[!code-csharp[](index/snapshot/Chain/StartupUseWhen.cs?highlight=23-24)]

W poprzednim przykładzie odpowiedź "Hello z głównego potoku". jest zapisywana dla wszystkich żądań. Jeśli żądanie zawiera zmienną ciągu zapytania `branch`, jej wartość jest rejestrowana przed ponownym dołączeniem głównego potoku.

## <a name="built-in-middleware"></a>Wbudowane oprogramowanie pośredniczące

ASP.NET Core dostarcza z następującymi składnikami oprogramowania pośredniczącego. Kolumna *Order* zawiera uwagi dotyczące umieszczania oprogramowania pośredniczącego w potoku przetwarzania żądań i w ramach jakich warunków oprogramowanie pośredniczące może przerwać przetwarzanie żądań. W przypadku krótkiego obwody przez oprogramowanie pośredniczące proces przetwarzania żądań i zapobiega przetwarzaniu żądania przez dalsze *podrzędne oprogramowanie pośredniczące.* Aby uzyskać więcej informacji na temat skracania obwodów, zobacz sekcję [Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) .

| Oprogramowanie pośredniczące | Opis | Zamówienie |
| ---------- | ----------- | ----- |
| [Uwierzytelnianie](xref:security/authentication/identity) | Zapewnia obsługę uwierzytelniania. | Przed `HttpContext.User` jest wymagana. Terminal dla wywołań zwrotnych uwierzytelniania OAuth. |
| [Autoryzacja](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*) | Zapewnia obsługę autoryzacji. | Natychmiast po oprogramowaniu pośredniczącym uwierzytelniania. |
| [Zasady dotyczące plików cookie](xref:security/gdpr) | Śledzi zgodę użytkowników na przechowywanie informacji osobistych i wymusza minimalne standardy dotyczące pól plików cookie, takich jak `secure` i `SameSite`. | Przed wystawianiem plików cookie przez oprogramowanie pośredniczące. Przykłady: uwierzytelnianie, sesja, MVC (TempData). |
| [CORS](xref:security/cors) | Konfiguruje udostępnianie zasobów między źródłami. | Przed składnikami korzystającymi z mechanizmu CORS. |
| [Diagnostyka](xref:fundamentals/error-handling) | Kilka oddzielnych middlewares, które udostępniają stronę wyjątku dewelopera, obsługę wyjątków, strony kodu stanu i domyślną stronę sieci Web dla nowych aplikacji. | Przed składnikami, które generują błędy. Terminal dla wyjątków lub obsługa domyślnej strony sieci Web dla nowych aplikacji. |
| [Nagłówki przesłane dalej](xref:host-and-deploy/proxy-load-balancer) | Przekazuje nagłówki proxy do bieżącego żądania. | Przed składnikami, które zużywają zaktualizowane pola. Przykłady: schemat, host, adres IP klienta, metoda. |
| [Sprawdzenie kondycji](xref:host-and-deploy/health-checks) | Sprawdza kondycję aplikacji ASP.NET Core i jej zależności, na przykład sprawdzanie dostępności bazy danych. | Terminal, jeśli żądanie pasuje do punktu końcowego sprawdzania kondycji. |
| [Propagowanie nagłówka](xref:fundamentals/http-requests#header-propagation-middleware) | Propaguje nagłówki HTTP z przychodzącego żądania do wychodzących żądań klienta HTTP. |
| [Zastąpienie metody HTTP](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | Zezwala na przychodzące żądanie POST przesłaniające metodę. | Przed składnikami, które zużywają zaktualizowaną metodę. |
| [Przekierowanie HTTPS](xref:security/enforcing-ssl#require-https) | Przekierowuj wszystkie żądania HTTP do protokołu HTTPS. | Przed składnikami, które używają adresu URL. |
| [Zabezpieczenia protokołu HTTP Strict Transport (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Ulepszanie zabezpieczeń oprogramowanie pośredniczące, które dodaje specjalny nagłówek odpowiedzi. | Przed wysłaniem odpowiedzi i po składnikach, które modyfikują żądania. Przykłady: nagłówki przesłane dalej, ponowne zapisywanie adresów URL. |
| [MVC](xref:mvc/overview) | Przetwarza żądania przy użyciu MVC/Razor Pages. | Terminal, jeśli żądanie pasuje do trasy. |
| [OWIN](xref:fundamentals/owin) | Współdziałanie z aplikacjami opartymi na OWIN, serwerami i oprogramowanie pośredniczące. | Terminal, jeśli oprogramowanie OWIN w pełni przetwarza żądanie. |
| [Buforowanie odpowiedzi](xref:performance/caching/middleware) | Zapewnia obsługę buforowania odpowiedzi. | Przed składnikami, które wymagają buforowania. |
| [Kompresja odpowiedzi](xref:performance/response-compression) | Zapewnia obsługę kompresowania odpowiedzi. | Przed składnikami wymagającymi kompresji. |
| [Lokalizacja żądania](xref:fundamentals/localization) | Zapewnia obsługę lokalizacji. | Przed uwzględnieniem poufnych składników lokalizacji. |
| [Routing punktów końcowych](xref:fundamentals/routing) | Definiuje trasy żądań i ogranicza je. | Terminal dla pasujących tras. |
| [HASŁA](xref:Microsoft.AspNetCore.Builder.SpaApplicationBuilderExtensions.UseSpa*) | Obsługuje wszystkie żądania z tego punktu w łańcuchu oprogramowania pośredniczącego, zwracając domyślną stronę aplikacji jednostronicowej (SPA) | Na koniec łańcucha, dzięki czemu inne oprogramowanie pośredniczące do obsługi plików statycznych, działania MVC itd., ma pierwszeństwo.|
| [Sesja](xref:fundamentals/app-state) | Zapewnia obsługę zarządzania sesjami użytkowników. | Przed składnikami, które wymagają sesji. | 
| [Pliki statyczne](xref:fundamentals/static-files) | Zapewnia obsługę plików statycznych i przeglądania katalogów. | Terminal, jeśli żądanie pasuje do pliku. |
| [Ponowne zapisywanie adresów URL](xref:fundamentals/url-rewriting) | Zapewnia obsługę ponownego zapisywania adresów URL i Przekierowywanie żądań. | Przed składnikami, które używają adresu URL. |
| [Obiekty WebSocket](xref:fundamentals/websockets) | Włącza protokół WebSockets. | Przed składnikami, które są wymagane do akceptowania żądań WebSocket. |

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)

Oprogramowanie pośredniczące jest oprogramowaniem, które jest połączone z potokiem aplikacji w celu obsługi żądań i odpowiedzi. Każdy składnik:

* Wybiera, czy żądanie ma zostać przekazane do następnego składnika w potoku.
* Może wykonać prace przed i po następnym składniku potoku.

Delegaty żądań są używane do kompilowania potoku żądania. Delegaty żądań obsługują każde żądanie HTTP.

Delegatów żądań konfiguruje się za pomocą metod rozszerzenia <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>i <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. Osobnego delegata żądania można określić jako metodę anonimową (nazywaną w wierszu) lub można ją zdefiniować w klasie wielokrotnego użytku. Te klasy wielokrotnego użytku i metody anonimowe w wierszu to *oprogramowanie pośredniczące*, nazywane również *składnikami oprogramowania pośredniczącego*. Każdy składnik pośredniczący w potoku żądania jest odpowiedzialny za wywoływanie następnego składnika w potoku lub krótkiego obwodu potoku. W przypadku krótkiego obwodu oprogramowanie pośredniczące nosi nazwę *oprogramowania pośredniczącego terminalu* , ponieważ zapobiega przetwarzaniu żądania przez dalsze oprogramowanie pośredniczące.

<xref:migration/http-modules> objaśnia różnicę między potokami żądań w ASP.NET Core i ASP.NET 4. x i oferuje dodatkowe przykłady oprogramowania pośredniczącego.

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder

Potok żądania ASP.NET Core składa się z sekwencji delegatów żądań o nazwie jeden po drugim. Na poniższym diagramie przedstawiono koncepcję. Wątek wykonywania jest zgodny z czarnym strzałką.

![Wzorzec przetwarzania żądań pokazujący żądanie docierające do, przetwarzania przez trzy middlewares i odpowiedzi opuszczających aplikację. Każde oprogramowanie pośredniczące uruchamia swoją logikę i przekazuje żądanie do następnego oprogramowania pośredniczącego przy następnej instrukcji (). Po przetworzeniu żądania przez trzecie oprogramowanie pośredniczące żądanie przechodzi z powrotem przez poprzednie dwie middlewares w odwrotnej kolejności, aby można było je przetwarzać po kolejne instrukcje () przed opuszczeniem aplikacji jako odpowiedzi na klienta.](index/_static/request-delegate-pipeline.png)

Każdy delegat może wykonać operacje przed i po następnym delegacie. Delegaty obsługi wyjątków powinny być wywoływane wczesnie w potoku, dlatego mogą przechwytywać wyjątki, które występują w późniejszych etapach potoku.

Najprostszym możliwym ASP.NET Core aplikacji jest skonfigurowanie pojedynczego delegata żądania, który obsługuje wszystkie żądania. Ten przypadek nie obejmuje rzeczywistego potoku żądania. Zamiast tego funkcja pojedynczej anonimowej jest wywoływana w odpowiedzi na każde żądanie HTTP.

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

Pierwszy <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegat kończy potok.

Łączenie wielu delegatów żądań z <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. `next` parametr reprezentuje następny delegat w potoku. Potok można skrócić, *nie* wywołując *następnego* parametru. Zazwyczaj można wykonywać akcje zarówno przed, jak i po następnym delegatze, jak pokazano w poniższym przykładzie:

[!code-csharp[](index/snapshot/Chain/Startup.cs)]

Gdy delegat nie przekazuje żądania do następnego delegata, jest on nazywany *krótkim obwodem potoku żądania*. Krótki obwód jest często pożądany, ponieważ pozwala uniknąć niepotrzebnej pracy. Na przykład [statyczne oprogramowanie pośredniczące](xref:fundamentals/static-files) może działać jako *oprogramowanie pośredniczące terminalu* przez przetwarzanie żądania dla pliku statycznego i krótkiego obwodu pozostałej części potoku. Oprogramowanie pośredniczące dodane do potoku zanim oprogramowanie pośredniczące, które zakończy dalsze przetwarzanie, nadal przetwarza kod po instrukcji `next.Invoke`. Należy jednak zapoznać się z poniższym ostrzeżeniem dotyczącym próby zapisu do odpowiedzi, która została już wysłana.

> [!WARNING]
> Nie wywołuj `next.Invoke` po wysłaniu odpowiedzi do klienta. Zmiany w <xref:Microsoft.AspNetCore.Http.HttpResponse> po rozpoczęciu zgłaszania wyjątku przez odpowiedź. Na przykład zmiany, takie jak nagłówki ustawień i kod stanu zgłaszają wyjątek. Zapisywanie w treści odpowiedzi po wywołaniu `next`:
>
> * Może spowodować naruszenie protokołu. Na przykład pisanie więcej niż podane `Content-Length`.
> * Może uszkodzić format treści. Na przykład, pisząc stopkę HTML do pliku CSS.
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> jest przydatną wskazówką wskazującą, czy nagłówki zostały wysłane lub w których zapisano treść.

<a name="order"></a>

## <a name="middleware-order"></a>Zamówienie oprogramowania pośredniczącego

Kolejność, w jakiej składniki pośredniczące są dodawane w metodzie `Startup.Configure` definiuje kolejność, w jakiej składniki pośredniczące są wywoływane na żądaniach i odwrotnej kolejności odpowiedzi. Kolejność ma **kluczowe znaczenie** dla bezpieczeństwa, wydajności i funkcjonalności.

Poniższa metoda `Startup.Configure` dodaje składniki pośredniczące powiązane z zabezpieczeniami w zalecanej kolejności:

[!code-csharp[](index/snapshot/Startup22.cs?name=snippet)]

W powyższym kodzie:

* Oprogramowanie pośredniczące, które nie jest dodawane podczas tworzenia nowej aplikacji sieci Web z [kontami poszczególnych użytkowników](xref:security/authentication/identity) , jest oznaczone jako komentarz.
* Nie każde oprogramowanie pośredniczące musi przejść do tej dokładnej kolejności, ale wiele do. Na przykład `UseCors` i `UseAuthentication` muszą przejść w podanej kolejności.

Poniższa metoda `Startup.Configure` dodaje składniki pośredniczące dla typowych scenariuszy aplikacji:

1. Obsługa wyjątków/błędów
   * Gdy aplikacja jest uruchamiana w środowisku deweloperskim:
     * Oprogramowanie pośredniczące strony wyjątków dla deweloperów (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) zgłasza błędy środowiska uruchomieniowego aplikacji.
     * Strona błędu bazy danych — oprogramowanie pośredniczące (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) raportuje błędy środowiska uruchomieniowego bazy danych.
   * Gdy aplikacja jest uruchamiana w środowisku produkcyjnym:
     * Program obsługi wyjątków — oprogramowanie pośredniczące (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) przechwytuje wyjątki zgłoszone w następujących middlewares.
     * Oprogramowanie pośredniczące protokołu HTTP Strict Transport Security (HSTS) (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) dodaje nagłówek `Strict-Transport-Security`.
1. Oprogramowanie pośredniczące przekierowania HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) przekierowuje żądania HTTP do protokołu HTTPS.
1. Oprogramowanie pośredniczące plików statycznych (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) zwraca pliki statyczne i dalsze przetwarzanie żądań na krótkie obwody.
1. Oprogramowanie pośredniczące zasad plików cookie (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) jest zgodne z przepisami dla Ogólne rozporządzenie o ochronie danych UE (Rodo).
1. Oprogramowanie pośredniczące uwierzytelniania (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) próbuje uwierzytelnić użytkownika przed zezwoleniem na dostęp do zabezpieczonych zasobów.
1. Oprogramowanie pośredniczące sesji (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) ustanawia i utrzymuje stan sesji. Jeśli aplikacja używa stanu sesji, wywołaj oprogramowanie pośredniczące sesji po wyjściu z zasad plików cookie i przed oprogramowania MVC.
1. MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>), aby dodać MVC do potoku żądania.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();
    app.UseMvc();
}
```

W powyższym przykładowym kodzie każda Metoda rozszerzenia oprogramowania pośredniczącego jest uwidaczniana na <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> za pomocą przestrzeni nazw <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> jest pierwszym składnikiem pośredniczącym dodanym do potoku. W związku z tym, oprogramowanie pośredniczące programu obsługi wyjątków przechwytuje wszystkie wyjątki występujące w późniejszych wywołaniach.

Oprogramowanie pośredniczące plików statycznych jest wczesnie wywoływane w potoku, dzięki czemu może obsługiwać żądania i krótkie obwód bez przechodzenia przez pozostałe składniki. Oprogramowanie pośredniczące plików statycznych **nie zapewnia żadnych** kontroli autoryzacji. Wszystkie pliki obsługiwane przez oprogramowanie pośredniczące plików statycznych, w tym w katalogu *wwwroot*, są publicznie dostępne. Aby zapoznać się z podejściem do zabezpieczania plików statycznych, zobacz <xref:fundamentals/static-files>.

Jeśli żądanie nie jest obsługiwane przez oprogramowanie pośredniczące pliku statycznego, jest ono przesyłane do oprogramowania pośredniczącego uwierzytelniania (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), które wykonuje uwierzytelnianie. Uwierzytelnianie nie ma krótkoterminowych żądań nieuwierzytelnionych. Chociaż uwierzytelnianie pośredniczące uwierzytelnia żądania, autoryzacja (i odrzucanie) występuje tylko po zaznaczeniu określonej strony Razor lub kontrolera MVC i akcji.

Poniższy przykład ilustruje kolejność oprogramowania pośredniczącego, w którym żądania plików statycznych są obsługiwane przez oprogramowanie pośredniczące plików statycznych przed użyciem oprogramowania pośredniczącego kompresji. Pliki statyczne nie są kompresowane z tą kolejnością oprogramowania. Odpowiedzi MVC z <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> mogą być kompresowane.

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseMvcWithDefaultRoute();
}
```

## <a name="use-run-and-map"></a>Używanie, uruchamianie i mapowanie

Skonfiguruj potok HTTP przy użyciu <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>i <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>. Metoda `Use` może skrócić obwód rurociągu (to oznacza, jeśli nie wywoła delegata żądania `next`). `Run` jest konwencją, a niektóre składniki pośredniczące mogą uwidaczniać metody `Run[Middleware]`, które są uruchamiane na końcu potoku.

rozszerzenia <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> są używane jako konwencja rozgałęziania potoku. `Map` rozgałęziać potok żądania na podstawie dopasowań podanej ścieżki żądania. Jeśli ścieżka żądania rozpoczyna się od podaną ścieżką, rozgałęzienie jest wykonywane.

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu.

| Żądanie             | Odpowiedź                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Witaj od delegata innego niż mapowanie. |
| localhost:1234/map1 | Test mapy 1                   |
| localhost:1234/map2 | Test mapy 2                   |
| localhost:1234/map3 | Witaj od delegata innego niż mapowanie. |

Gdy jest używana `Map`, dopasowane segmenty ścieżki są usuwane z `HttpRequest.Path` i dołączane do `HttpRequest.PathBase` dla każdego żądania.

<xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> rozgałęziać potok żądania na podstawie wyniku danego predykatu. Dowolny predykat typu `Func<HttpContext, bool>` może służyć do mapowania żądań do nowej gałęzi potoku. W poniższym przykładzie predykat służy do wykrywania obecności zmiennej ciągu zapytania `branch`:

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs)]

W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu.

| Żądanie                       | Odpowiedź                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Witaj od delegata innego niż mapowanie. |
| localhost:1234/?branch=master | Używane gałęzie = Master         |

`Map` obsługuje zagnieżdżanie, na przykład:

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
```

`Map` można również dopasować wiele segmentów jednocześnie:

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

## <a name="built-in-middleware"></a>Wbudowane oprogramowanie pośredniczące

ASP.NET Core dostarcza z następującymi składnikami oprogramowania pośredniczącego. Kolumna *Order* zawiera uwagi dotyczące umieszczania oprogramowania pośredniczącego w potoku przetwarzania żądań i w ramach jakich warunków oprogramowanie pośredniczące może przerwać przetwarzanie żądań. W przypadku krótkiego obwody przez oprogramowanie pośredniczące proces przetwarzania żądań i zapobiega przetwarzaniu żądania przez dalsze *podrzędne oprogramowanie pośredniczące.* Aby uzyskać więcej informacji na temat skracania obwodów, zobacz sekcję [Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) .

| Oprogramowanie pośredniczące | Opis | Zamówienie |
| ---------- | ----------- | ----- |
| [Uwierzytelnianie](xref:security/authentication/identity) | Zapewnia obsługę uwierzytelniania. | Przed `HttpContext.User` jest wymagana. Terminal dla wywołań zwrotnych uwierzytelniania OAuth. |
| [Zasady dotyczące plików cookie](xref:security/gdpr) | Śledzi zgodę użytkowników na przechowywanie informacji osobistych i wymusza minimalne standardy dotyczące pól plików cookie, takich jak `secure` i `SameSite`. | Przed wystawianiem plików cookie przez oprogramowanie pośredniczące. Przykłady: uwierzytelnianie, sesja, MVC (TempData). |
| [CORS](xref:security/cors) | Konfiguruje udostępnianie zasobów między źródłami. | Przed składnikami korzystającymi z mechanizmu CORS. |
| [Diagnostyka](xref:fundamentals/error-handling) | Kilka oddzielnych middlewares, które udostępniają stronę wyjątku dewelopera, obsługę wyjątków, strony kodu stanu i domyślną stronę sieci Web dla nowych aplikacji. | Przed składnikami, które generują błędy. Terminal dla wyjątków lub obsługa domyślnej strony sieci Web dla nowych aplikacji. |
| [Nagłówki przesłane dalej](xref:host-and-deploy/proxy-load-balancer) | Przekazuje nagłówki proxy do bieżącego żądania. | Przed składnikami, które zużywają zaktualizowane pola. Przykłady: schemat, host, adres IP klienta, metoda. |
| [Sprawdzenie kondycji](xref:host-and-deploy/health-checks) | Sprawdza kondycję aplikacji ASP.NET Core i jej zależności, na przykład sprawdzanie dostępności bazy danych. | Terminal, jeśli żądanie pasuje do punktu końcowego sprawdzania kondycji. |
| [Zastąpienie metody HTTP](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | Zezwala na przychodzące żądanie POST przesłaniające metodę. | Przed składnikami, które zużywają zaktualizowaną metodę. |
| [Przekierowanie HTTPS](xref:security/enforcing-ssl#require-https) | Przekierowuj wszystkie żądania HTTP do protokołu HTTPS. | Przed składnikami, które używają adresu URL. |
| [Zabezpieczenia protokołu HTTP Strict Transport (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Ulepszanie zabezpieczeń oprogramowanie pośredniczące, które dodaje specjalny nagłówek odpowiedzi. | Przed wysłaniem odpowiedzi i po składnikach, które modyfikują żądania. Przykłady: nagłówki przesłane dalej, ponowne zapisywanie adresów URL. |
| [MVC](xref:mvc/overview) | Przetwarza żądania przy użyciu MVC/Razor Pages. | Terminal, jeśli żądanie pasuje do trasy. |
| [OWIN](xref:fundamentals/owin) | Współdziałanie z aplikacjami opartymi na OWIN, serwerami i oprogramowanie pośredniczące. | Terminal, jeśli oprogramowanie OWIN w pełni przetwarza żądanie. |
| [Buforowanie odpowiedzi](xref:performance/caching/middleware) | Zapewnia obsługę buforowania odpowiedzi. | Przed składnikami, które wymagają buforowania. |
| [Kompresja odpowiedzi](xref:performance/response-compression) | Zapewnia obsługę kompresowania odpowiedzi. | Przed składnikami wymagającymi kompresji. |
| [Lokalizacja żądania](xref:fundamentals/localization) | Zapewnia obsługę lokalizacji. | Przed uwzględnieniem poufnych składników lokalizacji. |
| [Routing punktów końcowych](xref:fundamentals/routing) | Definiuje trasy żądań i ogranicza je. | Terminal dla pasujących tras. |
| [Sesja](xref:fundamentals/app-state) | Zapewnia obsługę zarządzania sesjami użytkowników. | Przed składnikami, które wymagają sesji. |
| [Pliki statyczne](xref:fundamentals/static-files) | Zapewnia obsługę plików statycznych i przeglądania katalogów. | Terminal, jeśli żądanie pasuje do pliku. |
| [Ponowne zapisywanie adresów URL](xref:fundamentals/url-rewriting) | Zapewnia obsługę ponownego zapisywania adresów URL i Przekierowywanie żądań. | Przed składnikami, które używają adresu URL. |
| [Obiekty WebSocket](xref:fundamentals/websockets) | Włącza protokół WebSockets. | Przed składnikami, które są wymagane do akceptowania żądań WebSocket. |

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end
