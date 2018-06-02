---
title: Oprogramowanie pośredniczące platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat oprogramowania pośredniczącego platformy ASP.NET Core i żądania potoku.
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 381c1a5cecee945559ea0dabd0aa086c8d52b43a
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729171"
---
# <a name="aspnet-core-middleware"></a>Oprogramowanie pośredniczące platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>Co to jest oprogramowanie pośredniczące?

Oprogramowanie pośredniczące to oprogramowanie, które są umieszczone w potoku aplikacji do obsługi żądań i odpowiedzi. Poszczególne składniki:

* Wybiera opcję Przekaż żądanie do następnego składnika w potoku.
* Może wykonywać pracy przed i po wywołaniu następny składnik w potoku. 

Obiekty delegowane żądania są używane do tworzenia potoku żądania. Obiekty delegowane żądania obsługi każdego żądania HTTP.

Żądanie delegatów są skonfigurowane przy użyciu [Uruchom](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [mapy](/dotnet/api/microsoft.aspnetcore.builder.mapextensions), i [użyj](/dotnet/api/microsoft.aspnetcore.builder.useextensions) metody rozszerzenia. Obiekt delegowany oddzielne żądanie może być określony w wierszu jako metody anonimowej (nazywane oprogramowanie pośredniczące w wierszu) lub może być zdefiniowana w klasie wielokrotnego użytku. Te klasy wielokrotnego użytku i metod anonimowych w wierszu są *oprogramowanie pośredniczące*, lub *składników oprogramowania pośredniczącego*. Każdy składnik oprogramowania pośredniczącego w potoku żądania jest odpowiedzialny za wywoływanie następny składnik w potoku lub zwarcie łańcucha, w razie potrzeby.

[Migrowanie moduły HTTP z oprogramowaniem pośredniczącym](xref:migration/http-modules) objaśniono różnicę między potoków żądania w ASP.NET Core i ASP.NET 4.x i zawiera więcej przykładów oprogramowania pośredniczącego.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder

Potok żądań ASP.NET Core składa się z sekwencji delegatów żądania, jeden po drugim wywołać, ponieważ ten diagram przedstawia (wątek sposób wykonywania strzałki czarny):

![Żądanie przetwarzania wzorca przedstawiający żądania przychodzące, przetwarzania za pośrednictwem trzy middlewares i odpowiedzi, pozostawiając aplikacji. Każdy oprogramowanie pośredniczące uruchamia swojej logiki i przekazuje żądanie do następnego oprogramowania pośredniczącego w instrukcji next(). Po trzeci oprogramowanie pośredniczące przetwarza żądanie, żądanie Przechodzi wstecz poprzednich dwóch middlewares w odwrotnej kolejności dla dodatkowego przetwarzania po deklaracji next() przed opuszczeniem aplikacji jako odpowiedź do klienta.](index/_static/request-delegate-pipeline.png)

Każdy delegata mogą wykonywać operacje przed i po następnym delegata. Delegat można też przekazuje żądanie do następnego delegata, która jest wywoływana zwarcie żądania potoku. Zwarcie często jest pożądane, ponieważ takie rozwiązanie pomaga uniknąć niepotrzebnych pracy. Na przykład oprogramowanie pośredniczące plików statycznych wrócić żądania plików statycznych i zwarcia pozostałego potoku. Delegatów obsługi wyjątków muszą można wywołać w potoku, więc ich catch wyjątków, które występują w późniejszym etapie w potoku.

Najprostsza możliwa aplikacji platformy ASP.NET Core ustawia delegata pojedynczego żądania, który obsługuje wszystkie żądania. Ten przypadek nie zawiera rzeczywistego żądania potoku. Zamiast tego jednej funkcji anonimowy jest wywoływana w odpowiedzi na każde żądanie HTTP.

[!code-csharp[](index/sample/Middleware/Startup.cs)]

Pierwszy [aplikacji. Uruchom](/dotnet/api/microsoft.aspnetcore.builder.runextensions) delegata kończy potoku.

Tworzenia łańcucha wielu delegatów żądania razem z [aplikacji. Użyj](/dotnet/api/microsoft.aspnetcore.builder.useextensions). `next` Parametr reprezentuje dalej delegata w potoku. (Należy pamiętać, że można zwarcia potoku przez *nie* wywoływania *dalej* parametru.) Zazwyczaj można wykonywać akcje przed i po następnej delegata, jak pokazano w tym przykładzie:

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Nie wywołuj `next.Invoke` po wysłaniu odpowiedzi do klienta. Zmienia się na `HttpResponse` po rozpoczęciu odpowiedzi spowoduje zgłoszenie wyjątku. Na przykład zmiany, takie jak ustawianie nagłówków, kod stanu itd., spowoduje zgłoszenie wyjątku. Zapisywanie w treści odpowiedzi po wywołaniu `next`:
> - Może spowodować naruszenie protokołu. Na przykład zapisywania więcej niż podanej `content-length`.
> - Może spowodować uszkodzenie formatu treści. Na przykład zapisywanie stopkę HTML w pliku CSS.
>
> [HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) przydatne podpowiada, aby wskazać, czy wysłaniu nagłówków i/lub treść została zapisana.

## <a name="ordering"></a>Szeregowanie

Dodano składników oprogramowania pośredniczącego w kolejności `Configure` metoda definiuje kolejność, w którym jest wywoływana w odpowiedzi na żądania i odwrotna kolejność dla odpowiedzi. Ta kolejność jest krytyczny dla zabezpieczeń, wydajności i funkcji.

Konfigurowanie metody (pokazana poniżej) dodaje następujące składniki oprogramowania pośredniczącego:

1. Obsługa wyjątków/błędów
2. Serwer plików statycznych
3. Uwierzytelnianie
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

W powyższym kodzie `UseExceptionHandler` jest pierwszy składnik oprogramowania pośredniczącego dodane do potoku — w związku z tym go przechwytuje żadnych wyjątków, które występują w późniejszym wywołania.

Oprogramowanie pośredniczące plików statycznych jest wywoływana wcześnie w potoku, więc może obsługiwać żądania ani zwarcia bez pośrednictwa pozostałe składniki. Udostępnia oprogramowanie pośredniczące plików statycznych **nie** sprawdzeń autoryzacji. Wszystkie pliki obsługiwane przez, włącznie z zawartymi w obszarze *wwwroot*, są dostępne publicznie. Zobacz [pliki statyczne](xref:fundamentals/static-files) podejścia do zabezpieczania plików statycznych.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


Jeśli żądanie nie jest obsługiwane przez oprogramowanie pośredniczące plików statycznych, będzie przekazany z oprogramowaniem pośredniczącym tożsamości (`app.UseAuthentication`), który przeprowadza uwierzytelnianie. Tożsamość nie zwarcia nieuwierzytelnione żądania. Mimo że tożsamości uwierzytelniania żądań autoryzacji (i odrzucenia) występuje tylko po MVC wybierze określonych Razor strony lub kontrolera i akcji.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli żądanie nie jest obsługiwane przez oprogramowanie pośredniczące plików statycznych, będzie przekazany z oprogramowaniem pośredniczącym tożsamości (`app.UseIdentity`), który przeprowadza uwierzytelnianie. Tożsamość nie zwarcia nieuwierzytelnione żądania. Mimo że tożsamości uwierzytelnia żądania, autoryzacji (i odrzucenia) występuje tylko wtedy, gdy wybiera MVC określonego kontrolera i akcji.

-----------

W poniższym przykładzie pokazano oprogramowanie pośredniczące kolejność, w którym żądań dotyczących plików statycznych są obsługiwane przez oprogramowanie pośredniczące plików statycznych przed oprogramowanie pośredniczące kompresji odpowiedzi. Pliki statyczne nie są kompresowane z ta kolejność oprogramowania pośredniczącego. Odpowiedzi MVC z [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) można skompresować.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Użyj, uruchamiania i mapowanie

Można skonfigurować za pomocą potoku HTTP `Use`, `Run`, i `Map`. `Use` Metody może zwarcia potoku (to znaczy, jeśli go nie wywołać `next` delegata żądania). `Run` jest to Konwencja i niektórych składników oprogramowania pośredniczącego może narazić `Run[Middleware]` metod, które są uruchamiane na końcu potoku.

`Map*` rozszerzenia są używane jako Konwencji Rozgałęzienie potoku. [Mapa](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) odgałęzień potoku żądania na podstawie dopasowań z podanej ścieżki żądania. Jeśli ścieżka żądania rozpoczyna się od podanej ścieżce, gałęzi jest wykonywana.

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu:

| Żądanie | Odpowiedź |
| --- | --- |
| localhost:1234 | Powitania od elementu delegate-Map.  |
| localhost:1234/map1 | Mapa Test 1 |
| localhost:1234/map2 | Mapa Test 2 |
| localhost:1234/map3 | Powitania od elementu delegate-Map.  |

Gdy `Map` jest, następującą liczbę segmentów ścieżki dopasowane są usuwane z `HttpRequest.Path` i jest dołączany do `HttpRequest.PathBase` dla każdego żądania.

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) odgałęzień potoku żądania na podstawie wyniku danego predykatu. Predykat dowolnego typu `Func<HttpContext, bool>` służy do mapowania żądania do nowej gałęzi potoku. W poniższym przykładzie predykat jest używana do wykrywania obecności zmiennej ciągu zapytania `branch`:

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

W poniższej tabeli przedstawiono żądania i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu:

| Żądanie | Odpowiedź |
| --- | --- |
| localhost:1234 | Powitania od elementu delegate-Map.  |
| localhost:1234/?branch=master | Gałąź używane = wzorca|

`Map` obsługuje zagnieżdżania, na przykład:

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map` można również odpowiada wielu segmentach jednocześnie, na przykład:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Wbudowane oprogramowania pośredniczącego

Platformy ASP.NET Core jest dostarczany z następujących składników oprogramowania pośredniczącego, a także opis kolejności, w którym powinny zostać dodane:

| Oprogramowanie pośredniczące | Opis | Kolejność |
| ---------- | ----------- | ----- |
| [Uwierzytelnianie](xref:security/authentication/identity) | Zapewnia obsługę uwierzytelniania. | Przed `HttpContext.User` jest wymagana. Terminalu wywołania zwrotne OAuth. |
| [CORS](xref:security/cors) | Konfiguruje współużytkowanie zasobów między źródłami. | Przed składników, które korzystają z CORS. |
| [Diagnostyka](xref:fundamentals/error-handling) | Konfiguruje diagnostyki. | Przed składniki, które generują błędy. |
| [Zastąpienia przekazywane nagłówki/HTTP](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Przekazuje proxy nagłówki na bieżącego żądania. | Przed składniki używające zaktualizowanych pól (przykłady: schemat, hosta, ClientIP, metoda). |
| [Przekierowania protokołu HTTPS](xref:security/enforcing-ssl#require-https) | Przekieruj żądania HTTP, HTTPS (platformy ASP.NET Core 2.1 lub nowszej). | Przed składniki używające adresu URL. |
| [Zabezpieczenia transportu Strict HTTP (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Pośredniczącym ulepszenie zabezpieczeń dodaje nagłówek odpowiedzi specjalne (platformy ASP.NET Core 2.1 lub nowszej). | Przed wysłaniem odpowiedzi oraz po wykonaniu składników, które modyfikują żądań (na przykład przekazywane nagłówków, ponowne zapisywanie adresów URL). |
| [Buforowanie odpowiedzi](xref:performance/caching/middleware) | Zapewnia obsługę buforowania odpowiedzi. | Przed składniki, które wymagają buforowania. |
| [Kompresja odpowiedzi](xref:performance/response-compression) | Zapewnia obsługę kompresowania odpowiedzi. | Przed składniki, które wymagają kompresji. |
| [Lokalizacja żądania](xref:fundamentals/localization) | Zapewnia obsługę lokalizacji. | Przed składniki poufnych lokalizacji. |
| [Routing](xref:fundamentals/routing) | Definiuje i ogranicza trasy żądania. | Terminalu zgodnych tras. |
| [Sesja](xref:fundamentals/app-state) | Zapewnia obsługę zarządzania sesjami użytkownika. | Przed składniki, które wymagają sesji. |
| [Pliki statyczne](xref:fundamentals/static-files) | Zapewnia obsługę obsługujących pliki statyczne oraz przeglądanie katalogów. | Terminal, jeśli żądanie pasuje do plików. |
| [Ponowne zapisywanie adresów URL](xref:fundamentals/url-rewriting) | Umożliwia ponowne zapisywanie adresów URL i przekierowywanie żądań. | Przed składniki używające adresu URL. |
| [Obiekty WebSocket](xref:fundamentals/websockets) | Włącza protokołu WebSockets. | Przed składników, które są wymagane, aby akceptował żądania protokołu WebSocket. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Oprogramowanie pośredniczące zapisu

Oprogramowanie pośredniczące jest zazwyczaj hermetyzowany w klasie i udostępniany z metodą rozszerzenia. Należy wziąć pod uwagę następujące oprogramowanie pośredniczące, które ustawia kulturę bieżącego żądania z ciągu zapytania:

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

Uwaga: W powyższym przykładowym kodzie służy do zaprezentowania tworzenia składników oprogramowania pośredniczącego. Zobacz [ lokalizacja i globalizacja](xref:fundamentals/localization) obsługę wbudowanych lokalizacja platformy ASP.NET Core.

Możesz przetestować oprogramowanie pośredniczące, przekazując kultury, na przykład `http://localhost:7997/?culture=no`.

Poniższy kod powoduje delegata oprogramowanie pośredniczące do klasy:

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> W przypadku platformy ASP.NET Core 1.x, oprogramowanie pośredniczące `Task` Nazwa metody musi być `Invoke`. W programie ASP.NET Core 2.0 lub nowszej, może to być albo `Invoke` lub `InvokeAsync`.

Oprogramowanie pośredniczące za pośrednictwem udostępnia następujące metody rozszerzenia [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

Poniższy kod wywołuje oprogramowanie pośredniczące z `Configure`:

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

Oprogramowanie pośredniczące powinno wykonać [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/) przez udostępnianie jego zależności w jego konstruktora. Oprogramowanie pośredniczące jest tworzony raz na *istnienia aplikacji*. Zobacz *zależności żądania* poniżej Jeśli potrzebne do udostępnienia usług z oprogramowania pośredniczącego w ramach żądania.

Składniki oprogramowania pośredniczącego można rozwiązać zależności z iniekcji zależności za pomocą parametrów konstruktora. [`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) Ponadto może zaakceptować dodatkowe parametry bezpośrednio.

### <a name="per-request-dependencies"></a>Zależności żądania

Ponieważ oprogramowanie pośredniczące jest tworzony podczas uruchamiania aplikacji, nie dla poszczególnych żądań, *zakres* okres istnienia usługi używane przez oprogramowanie pośredniczące konstruktory nie są współużytkowane z innych typów zależności wprowadzonym podczas każdego żądania. Jeśli trzeba udostępnić *zakres* usługi między oprogramowania pośredniczącego i innych typów, należy dodać tych usług `Invoke` podpis metody. `Invoke` Metody może zaakceptować dodatkowe parametry, które są wypełniane przy iniekcji zależności. Na przykład:

```csharp
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Migrowanie moduły HTTP do oprogramowania pośredniczącego](xref:migration/http-modules)
* [Uruchamianie aplikacji](xref:fundamentals/startup)
* [Funkcje na żądanie](xref:fundamentals/request-features)
* [Aktywacji opartej na fabryki oprogramowania pośredniczącego](xref:fundamentals/middleware/extensibility)
* [Oprogramowanie pośredniczące aktywacji z kontenerem innych firm](xref:fundamentals/middleware/extensibility-third-party-container)
