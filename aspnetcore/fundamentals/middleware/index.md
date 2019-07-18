---
title: ASP.NET Core oprogramowanie pośredniczące
author: rick-anderson
description: Dowiedz się więcej na temat ASP.NET Core oprogramowania pośredniczącego i potoku żądania.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/09/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: 89cd505810eefeeeb8f708ab82244bbd2e341f38
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308175"
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core oprogramowanie pośredniczące

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)

Oprogramowanie pośredniczące jest oprogramowaniem, które jest połączone z potokiem aplikacji w celu obsługi żądań i odpowiedzi. Każdy składnik:

* Wybiera, czy żądanie ma zostać przekazane do następnego składnika w potoku.
* Może wykonać prace przed i po następnym składniku potoku.

Delegaty żądań są używane do kompilowania potoku żądania. Delegaty żądań obsługują każde żądanie HTTP.

Delegaty żądań są konfigurowane <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>przy <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>użyciu metod <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> rozszerzających, i. Osobnego delegata żądania można określić jako metodę anonimową (nazywaną w wierszu) lub można ją zdefiniować w klasie wielokrotnego użytku. Te klasy wielokrotnego użytku i metody anonimowe w wierszu to *oprogramowanie pośredniczące*, nazywane również *składnikami oprogramowania pośredniczącego*. Każdy składnik pośredniczący w potoku żądania jest odpowiedzialny za wywoływanie następnego składnika w potoku lub krótkiego obwodu potoku. W przypadku krótkiego obwodu oprogramowanie pośredniczące nosi nazwę *oprogramowania pośredniczącego terminalu* , ponieważ zapobiega przetwarzaniu żądania przez dalsze oprogramowanie pośredniczące.

<xref:migration/http-modules>Wyjaśnia różnicę między potokami żądań w ASP.NET Core i ASP.NET 4. x i udostępnia dodatkowe przykłady oprogramowania pośredniczącego.

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder

Potok żądania ASP.NET Core składa się z sekwencji delegatów żądań o nazwie jeden po drugim. Na poniższym diagramie przedstawiono koncepcję. Wątek wykonywania jest zgodny z czarnym strzałką.

![Wzorzec przetwarzania żądań pokazujący żądanie docierające do, przetwarzania przez trzy middlewares i odpowiedzi opuszczających aplikację. Każde oprogramowanie pośredniczące uruchamia swoją logikę i przekazuje żądanie do następnego oprogramowania pośredniczącego przy następnej instrukcji (). Po przetworzeniu żądania przez trzecie oprogramowanie pośredniczące żądanie przechodzi z powrotem przez poprzednie dwie middlewares w odwrotnej kolejności, aby można było je przetwarzać po kolejne instrukcje () przed opuszczeniem aplikacji jako odpowiedzi na klienta.](index/_static/request-delegate-pipeline.png)

Każdy delegat może wykonać operacje przed i po następnym delegacie. Delegaty obsługi wyjątków powinny być wywoływane wczesnie w potoku, dlatego mogą przechwytywać wyjątki, które występują w późniejszych etapach potoku.

Najprostszym możliwym ASP.NET Core aplikacji jest skonfigurowanie pojedynczego delegata żądania, który obsługuje wszystkie żądania. Ten przypadek nie obejmuje rzeczywistego potoku żądania. Zamiast tego funkcja pojedynczej anonimowej jest wywoływana w odpowiedzi na każde żądanie HTTP.

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

Pierwszy <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegat kończy potok.

Łączenie wielu delegatów żądań z <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. `next` Parametr reprezentuje następny delegat w potoku. Potok można skrócić, *nie* wywołując *następnego* parametru. Zazwyczaj można wykonywać akcje zarówno przed, jak i po następnym delegatze, jak pokazano w poniższym przykładzie:

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

Gdy delegat nie przekazuje żądania do następnego delegata, jest on nazywany *krótkim obwodem potoku żądania*. Krótki obwód jest często pożądany, ponieważ pozwala uniknąć niepotrzebnej pracy. Na przykład [statyczne oprogramowanie pośredniczące](xref:fundamentals/static-files) może działać jako *oprogramowanie pośredniczące terminalu* przez przetwarzanie żądania dla pliku statycznego i krótkiego obwodu pozostałej części potoku. Oprogramowanie pośredniczące dodane do potoku przed oprogramowanie pośredniczące, które zakończy dalsze przetwarzanie, ciągle przetwarza `next.Invoke` kod po ich zestawie. Należy jednak zapoznać się z poniższym ostrzeżeniem dotyczącym próby zapisu do odpowiedzi, która została już wysłana.

> [!WARNING]
> Nie wywołuj `next.Invoke` po wysłaniu odpowiedzi do klienta. <xref:Microsoft.AspNetCore.Http.HttpResponse> Po rozpoczęciu zgłaszania wyjątku zmiany wprowadzone po odpowiedzi. Na przykład zmiany, takie jak nagłówki ustawień i kod stanu zgłaszają wyjątek. Zapisywanie w treści odpowiedzi po wywołaniu `next`:
>
> * Może spowodować naruszenie protokołu. Na przykład zapisywanie więcej niż podane `Content-Length`.
> * Może uszkodzić format treści. Na przykład, pisząc stopkę HTML do pliku CSS.
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*>jest przydatną wskazówką wskazującą, czy nagłówki zostały wysłane lub w których zapisano treść.

## <a name="order"></a>Zamówienie

Kolejność, w jakiej składniki pośredniczące są dodawane w `Startup.Configure` metodzie, definiuje kolejność, w jakiej składniki oprogramowania pośredniczącego są wywoływane w żądaniach i odwrotnej kolejności odpowiedzi. Kolejność ma kluczowe znaczenie dla bezpieczeństwa, wydajności i funkcjonalności.

Poniższa `Startup.Configure` Metoda dodaje składniki pośredniczące dla typowych scenariuszy aplikacji:

1. Obsługa wyjątków/błędów
   * Gdy aplikacja jest uruchamiana w środowisku deweloperskim:
     * Oprogramowanie pośredniczące strony wyjątków dla<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>deweloperów () zgłasza błędy środowiska uruchomieniowego aplikacji.
     * Strona błędu bazy danych oprogramowanie pośredniczące (<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) raportuje błędy środowiska uruchomieniowego bazy danych.
   * Gdy aplikacja jest uruchamiana w środowisku produkcyjnym:
     * Program obsługi wyjątków (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) przechwytuje wyjątki zgłoszone w następujących middlewares.
     * Protokół pośredniczący protokołu HTTP Strict Transport Security (HSTS)<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>() `Strict-Transport-Security` dodaje nagłówek.
1. Oprogramowanie pośredniczące przekierowywania<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>https () przekierowuje żądania HTTP do protokołu HTTPS.
1. Oprogramowanie pośredniczące plików statycznych<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>() zwraca pliki statyczne i dalsze przetwarzanie żądań na krótkie obwody.
1. Oprogramowanie pośredniczące zasad plików<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>cookie () powoduje, że aplikacja jest zgodna z przepisami UE ogólne rozporządzenie o ochronie danych (Rodo).
1. Oprogramowanie pośredniczące uwierzytelniania<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>() próbuje uwierzytelnić użytkownika przed zezwoleniem im na dostęp do zabezpieczonych zasobów.
1. Oprogramowanie pośredniczące sesji<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>() ustanawia i utrzymuje stan sesji. Jeśli aplikacja używa stanu sesji, wywołaj oprogramowanie pośredniczące sesji po wyjściu z zasad plików cookie i przed oprogramowania MVC.
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

W poprzednim przykładowym kodzie każda Metoda rozszerzenia oprogramowania pośredniczącego jest udostępniana <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> w <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> przestrzeni nazw.

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>jest pierwszym składnikiem pośredniczącym dodanym do potoku. W związku z tym, oprogramowanie pośredniczące programu obsługi wyjątków przechwytuje wszystkie wyjątki występujące w późniejszych wywołaniach.

Oprogramowanie pośredniczące plików statycznych jest wczesnie wywoływane w potoku, dzięki czemu może obsługiwać żądania i krótkie obwód bez przechodzenia przez pozostałe składniki. Oprogramowanie pośredniczące plików statycznych **nie zapewnia żadnych** kontroli autoryzacji. Wszystkie pliki obsługiwane przez ten program, w tym w katalogu *wwwroot*, są publicznie dostępne. Aby zapoznać się z podejściem do zabezpieczania <xref:fundamentals/static-files>plików statycznych, zobacz.

Jeśli żądanie nie jest obsługiwane przez oprogramowanie pośredniczące pliku statycznego, jest ono przesyłane do oprogramowania pośredniczącego uwierzytelniania (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), które wykonuje uwierzytelnianie. Uwierzytelnianie nie ma krótkoterminowych żądań nieuwierzytelnionych. Chociaż uwierzytelnianie pośredniczące uwierzytelnia żądania, autoryzacja (i odrzucanie) występuje tylko po zaznaczeniu określonej strony Razor lub kontrolera MVC i akcji.

Poniższy przykład ilustruje kolejność oprogramowania pośredniczącego, w którym żądania plików statycznych są obsługiwane przez oprogramowanie pośredniczące plików statycznych przed użyciem oprogramowania pośredniczącego kompresji. Pliki statyczne nie są kompresowane z tą kolejnością oprogramowania. Odpowiedzi <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> MVC można skompresować.

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

## <a name="use-run-and-map"></a>Używanie, uruchamianie i mapowanie

Skonfiguruj potok http przy użyciu <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>i <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>. Metoda może skrócić obwód rurociągu (oznacza to, że jeśli nie `next` wywoła delegata żądania). `Use` `Run`jest konwencją, a niektóre składniki pośredniczące mogą uwidaczniać `Run[Middleware]` metody, które są uruchamiane na końcu potoku.

<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>rozszerzenia są używane jako konwencja rozgałęziania potoku. `Map`rozgałęzienia potoku żądania na podstawie dopasowań podanej ścieżki żądania. Jeśli ścieżka żądania rozpoczyna się od podaną ścieżką, rozgałęzienie jest wykonywane.

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` używania poprzedniego kodu.

| Request             | Odpowiedź                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Witaj od delegata innego niż mapowanie. |
| localhost:1234/map1 | Test mapy 1                   |
| localhost:1234/map2 | Test mapy 2                   |
| localhost:1234/map3 | Witaj od delegata innego niż mapowanie. |

Gdy `Map` jest używany, dopasowane segmenty ścieżki są usuwane z `HttpRequest.Path` i dodawane do `HttpRequest.PathBase` każdego żądania.

<xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*>rozgałęzienia potoku żądania na podstawie wyniku danego predykatu. Dowolny predykat typu `Func<HttpContext, bool>` może służyć do mapowania żądań do nowej gałęzi potoku. W poniższym przykładzie predykat służy do wykrywania obecności zmiennej `branch`ciągu zapytania:

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` używania poprzedniego kodu.

| Request                       | Odpowiedź                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Witaj od delegata innego niż mapowanie. |
| localhost:1234/?branch=master | Używane gałęzie = Master         |

`Map`obsługuje zagnieżdżanie, na przykład:

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

`Map`można również dopasować wiele segmentów jednocześnie:

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a>Wbudowane oprogramowanie pośredniczące

ASP.NET Core dostarcza z następującymi składnikami oprogramowania pośredniczącego. Kolumna *Order* zawiera uwagi dotyczące umieszczania oprogramowania pośredniczącego w potoku przetwarzania żądań i w ramach jakich warunków oprogramowanie pośredniczące może przerwać przetwarzanie żądań. W przypadku krótkiego obwody przez oprogramowanie pośredniczące proces przetwarzania żądań i zapobiega przetwarzaniu żądania przez dalsze podrzędne oprogramowanie pośredniczące *.* Aby uzyskać więcej informacji na temat skracania obwodów, zobacz sekcję [Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) .

| Oprogramowanie pośredniczące | Opis | Zamówienie |
| ---------- | ----------- | ----- |
| [Uwierzytelnianie](xref:security/authentication/identity) | Zapewnia obsługę uwierzytelniania. | Przed `HttpContext.User` zainstalowaniem. Terminal dla wywołań zwrotnych uwierzytelniania OAuth. |
| [Zasady dotyczące plików cookie](xref:security/gdpr) | Śledzi zgodę użytkowników na przechowywanie informacji osobistych i wymusza minimalne standardy dotyczące pól plików cookie, takich jak `secure` i `SameSite`. | Przed wystawianiem plików cookie przez oprogramowanie pośredniczące. Przykłady: Uwierzytelnianie, sesja, MVC (TempData). |
| [CORS](xref:security/cors) | Konfiguruje udostępnianie zasobów między źródłami. | Przed składnikami korzystającymi z mechanizmu CORS. |
| [Obsługa wyjątków](xref:fundamentals/error-handling) | Obsługuje wyjątki. | Przed składnikami, które generują błędy. |
| [Nagłówki przesłane dalej](xref:host-and-deploy/proxy-load-balancer) | Przekazuje nagłówki proxy do bieżącego żądania. | Przed składnikami, które zużywają zaktualizowane pola. Przykłady: schemat, host, adres IP klienta, metoda. |
| [Sprawdzenie kondycji](xref:host-and-deploy/health-checks) | Sprawdza kondycję aplikacji ASP.NET Core i jej zależności, na przykład sprawdzanie dostępności bazy danych. | Terminal, jeśli żądanie pasuje do punktu końcowego sprawdzania kondycji. |
| [Zastąpienie metody HTTP](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | Zezwala na przychodzące żądanie POST przesłaniające metodę. | Przed składnikami, które zużywają zaktualizowaną metodę. |
| [Przekierowanie HTTPS](xref:security/enforcing-ssl#require-https) | Przekierowuj wszystkie żądania HTTP do protokołu HTTPS. | Przed składnikami, które używają adresu URL. |
| [Zabezpieczenia protokołu HTTP Strict Transport (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Ulepszanie zabezpieczeń oprogramowanie pośredniczące, które dodaje specjalny nagłówek odpowiedzi. | Przed wysłaniem odpowiedzi i po składnikach, które modyfikują żądania. Przykłady: Nagłówki przesłane dalej, ponowne zapisywanie adresów URL. |
| [MVC](xref:mvc/overview) | Przetwarza żądania przy użyciu MVC/Razor Pages. | Terminal, jeśli żądanie pasuje do trasy. |
| [OWIN](xref:fundamentals/owin) | Współdziałanie z aplikacjami opartymi na OWIN, serwerami i oprogramowanie pośredniczące. | Terminal, jeśli oprogramowanie OWIN w pełni przetwarza żądanie. |
| [Buforowanie odpowiedzi](xref:performance/caching/middleware) | Zapewnia obsługę buforowania odpowiedzi. | Przed składnikami, które wymagają buforowania. |
| [Kompresja odpowiedzi](xref:performance/response-compression) | Zapewnia obsługę kompresowania odpowiedzi. | Przed składnikami wymagającymi kompresji. |
| [Lokalizacja żądania](xref:fundamentals/localization) | Zapewnia obsługę lokalizacji. | Przed uwzględnieniem poufnych składników lokalizacji. |
| [Routing](xref:fundamentals/routing) | Definiuje trasy żądań i ogranicza je. | Terminal dla pasujących tras. |
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
