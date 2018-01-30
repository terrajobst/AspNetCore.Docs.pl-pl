---
title: Stan sesji i aplikacji w ASP.NET Core
author: rick-anderson
description: "Podejścia do zachowania aplikacji i stanu użytkowników (sesja) między żądaniami."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 7aa200d3612f766ab633ccab807421b9c5393975
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Wprowadzenie do stanu sesji oraz aplikacji platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), i [Diana LaRose](https://github.com/DianaLaRose)

HTTP jest protokołem bezstanowe. Serwer sieci web traktuje każde żądanie HTTP jako żądanie niezależne i nie zachowują wartości użytkownika z poprzedniego żądania. W tym artykule opisano różne sposoby, aby zachować aplikacji i stanu sesji między żądaniami. 

## <a name="session-state"></a>Stan sesji

Stan sesji jest funkcją platformy ASP.NET Core, który służy do zapisywania i przechowywania danych użytkownika, gdy użytkownik będzie przeglądać aplikacji sieci web. Składające się tabeli słownika lub skrótu na serwerze, stanu sesji utrzymuje dane żądań w przeglądarce. Dane sesji jest przechowywana w pamięci podręcznej.

Platformy ASP.NET Core zachowuje swój stan sesji, zapewniając klienta pliku cookie, który zawiera identyfikator sesji, który jest wysyłany na serwer z każdym żądaniem. Identyfikator sesji są używane do pobierania danych sesji. Ponieważ plik cookie sesji jest specyficzna dla przeglądarki, nie mogą współużytkować sesji w różnych przeglądarkach. Pliki cookie dotyczące sesji są usuwane tylko wtedy, gdy kończy się sesji przeglądarki. Jeśli plik cookie zostanie odebrana dla wygasłych sesji, utworzeniu nowej sesji, która używa tego samego pliku cookie sesji. 

Serwer zachowuje sesję przez ograniczony czas, po zgłoszeniu ostatniego żądania. Ustaw limit czasu sesji lub użyj wartości domyślnej 20 minut. Stan sesji jest idealny dla przechowywania danych użytkownika, która jest specyficzna dla konkretnej sesji, ale nie musi zostać utrwalony trwale. Dane są usuwane z magazynu zapasowego albo podczas wywoływania metody `Session.Clear` lub utraty ważności sesji w magazynie danych. Serwer nie może ustalić zamknięcia przeglądarki lub usunięcia pliku cookie sesji.

> [!WARNING]
> Nie należy przechowywać poufnych danych w sesji. Klient nie może być Zamknij przeglądarkę i wyczyść pliku cookie sesji (i w niektórych przeglądarkach podtrzymywania plików cookie sesji w systemie windows). Ponadto sesji nie może być ograniczony do jednego użytkownika; Następny użytkownik może kontynuować tej samej sesji.

Dostawca sesji w pamięci są przechowywane dane sesji na serwerze lokalnym. Jeśli planujesz uruchamianie aplikacji sieci web w farmie serwerów, należy użyć trwałe sesje powiązać każdej sesji do określonego serwera. Platforma Windows Azure Web Sites domyślnie trwałe sesje (Routing żądań aplikacji lub ARR). Trwałe sesje mogą jednak mieć wpływ na skalowalność i skomplikować aktualizacji aplikacji sieci web. Lepszym rozwiązaniem jest użycie pamięci podręcznej Redis, lub buforować rozproszonych programu SQL Server, które nie wymagają trwałe sesje. Aby uzyskać więcej informacji, zobacz [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed). Aby uzyskać więcej informacji na temat konfigurowania dostawcy usług, zobacz [Konfigurowanie sesji](#configuring-session) dalszej części tego artykułu.

<a name="temp"></a>
## <a name="tempdata"></a>TempData

Przedstawia platformy ASP.NET Core MVC [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Ta właściwość przechowuje dane, dopóki nie jest do odczytu. `Keep` i `Peek` metod można użyć do sprawdzenia danych bez usuwania. `TempData`jest szczególnie przydatne podczas przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania. `TempData`jest implementowany przez dostawców TempData, na przykład za pomocą plików cookie lub stanu sesji.

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>TempData dostawców

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

W programie ASP.NET Core 2.0 lub nowszego oraz dostawcy TempData na podstawie plików cookie jest używany domyślnie do przechowywania TempData w plikach cookie.

Dane pliku cookie jest zakodowane za pomocą [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0). Ponieważ plik cookie jest zaszyfrowany i fragmentaryczne, pojedynczy plik cookie rozmiar limit w ASP.NET Core 1.x nie ma zastosowania. Ponieważ kompresja zaszyfrowanych danych może prowadzić do problemów z bezpieczeństwem takich jak dane pliku cookie nie jest skompresowany [ataki CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków. Aby uzyskać więcej informacji o dostawcy TempData na podstawie plików cookie, zobacz [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Program ASP.NET Core 1.0 i 1.1 dostawca TempData stanu sesji jest ustawieniem domyślnym.

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>Wybieranie dostawcy TempData

Wybieranie dostawcy TempData obejmuje kilka kwestii, takich jak:

1. Aplikacja już używa stanu sesji dla innych celów? Jeśli jest to tak, przy użyciu dostawcy TempData stan sesji nie ma żadnych dodatkowych kosztów do aplikacji (jako uzupełnienie rozmiar danych).
2. Aplikacja używa TempData tylko oszczędnie przypadku względnie niewielkich ilości danych (maksymalnie 500 bajtów)? Jeśli tak, dostawcy TempData pliku cookie na każde żądanie, który przenosi TempData doda małych kosztów. Jeśli nie, dostawca TempData stanu sesji korzystne może okazać się uniknąć dwustronną komunikację dużej ilości danych do wszystkich żądań do momentu TempData jest używany.
3. Czy aplikacja działa w kolektywie serwerów sieci web (wiele serwerów)? Jeśli tak, nie istnieje żadne dodatkowe czynności konfiguracyjne niezbędne do używania dostawcy TempData pliku cookie.

> [!NOTE]
> Większość klientów w sieci web (na przykład przeglądarki sieci web) wymuszać ograniczenia dotyczące maksymalny rozmiar każdego pliku cookie i całkowita liczba plików cookie. W związku z tym podczas korzystania z dostawcy TempData pliku cookie, sprawdź, czy aplikacja nie przekroczy te ograniczenia. Należy wziąć pod uwagę całkowity rozmiar danych, ewidencjonowanie aktywności traktowane jako zapasy szyfrowania i podziału.

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>Konfigurowanie dostawcy TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Dostawca TempData na podstawie plików cookie jest domyślnie włączona. Następujące `Startup` kod klasy konfiguruje dostawcy TempData opartymi na sesji:

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Następujące `Startup` kod klasy konfiguruje dostawcy TempData opartymi na sesji:

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

Kolejność jest kluczowa dla składników oprogramowania pośredniczącego. W powyższym przykładzie wyjątek typu `InvalidOperationException` występuje, gdy `UseSession` jest wywoływana po `UseMvcWithDefaultRoute`. Zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware#ordering) uzyskać więcej szczegółowych informacji.

> [!IMPORTANT]
> Jeśli przeznaczonych dla platformy .NET Framework i przy użyciu dostawcy opartymi na sesji, Dodaj [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) pakiet NuGet do projektu.

## <a name="query-strings"></a>Ciągi zapytań

Można przekazać ograniczoną ilość danych z jednego żądania do innego przez dodanie go do nowego żądania ciągu zapytania. Jest to przydatne w przypadku przechwytywania stanu w sposób ciągły, umożliwiający łącza osadzonego stanu do udostępnienia za pośrednictwem poczty e-mail lub sieci społecznościowych. Jednak z tego powodu należy nigdy używać ciągów zapytania dla danych poufnych. Oprócz łatwo udostępnianiem, włącznie z danymi w ciągów zapytania można tworzyć możliwości [sfałszowaniem żądań Cross-Site (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataków, których można wymuszać użytkowników do odwiedzenia złośliwych witryn podczas uwierzytelnienia. Osoby atakujące można następnie kradzieży danych użytkownika z aplikacji lub podjąć złośliwe akcje w imieniu użytkownika. Każdy zachowanego stan application lub session należy chronić przed atakami CSRF. Aby uzyskać więcej informacji dotyczących ataków CSRF, zobacz [zapobieganie Cross-Site żądania (XSRF/CSRF) Fałszerstwie w ASP.NET Core](../security/anti-request-forgery.md).

## <a name="post-data-and-hidden-fields"></a>Dane POST i ukryte pola

Dane można zapisane w ukrytym pól i opublikować ponownie przy kolejnym żądaniu. To jest typowe w formularzach wiele stron. Jednak ponieważ klienta może potencjalnie manipulować danymi, serwer musi zawsze ponownie sprawdź poprawność go. 

## <a name="cookies"></a>Pliki cookie

Pliki cookie umożliwiają przechowywanie danych użytkownika w aplikacji sieci web. Ponieważ pliki cookie są wysyłane z każdym żądaniem, ich rozmiar powinny być ograniczone do minimum. Najlepiej, jeśli tylko identyfikator powinny być przechowywane w pliku cookie z rzeczywistymi danymi przechowywanymi na serwerze. W większości przeglądarek ograniczyć plików cookie do 4096 bajtów. Ponadto ograniczonej liczby plików cookie, są dostępne dla każdej domeny.  

Ponieważ pliki cookie podlegają naruszeniu, musi zostać zweryfikowany na serwerze. Trwałość pliku cookie na komputerze klienckim podlega interwencji użytkownika i wygaśnięcia, ale zazwyczaj są one najbardziej niezawodna formę trwałości danych na kliencie.

Pliki cookie są często używane na potrzeby personalizacji, gdy zawartość jest dostosowany do znanego użytkownika. Ponieważ użytkownik tylko zidentyfikowane i nie jest uwierzytelniony w większości przypadków, zwykle można zabezpieczyć plik cookie przez zapisanie nazwę użytkownika, nazwę konta lub unikatowe Identyfikatory (na przykład identyfikator GUID) w pliku cookie. Plik cookie można następnie użyć uzyskiwać dostęp do infrastruktury personalizacji użytkownika witryny.

## <a name="httpcontextitems"></a>HttpContext.Items

`Items` Kolekcja jest dobrym miejscem do przechowywania danych, które ma potrzebne tylko podczas przetwarzania jednego określonego żądania. Zawartość kolekcji zostaną odrzucone po każdym żądaniu. `Items` Kolekcja najlepiej jest używana jako sposób składniki lub oprogramowanie pośredniczące do komunikacji, gdy działają w różnych punktach w czasie żądania i nie może bezpośrednio do przekazania parametrów. Aby uzyskać więcej informacji, zobacz [Praca z HttpContext.Items](#working-with-httpcontextitems)w dalszej części tego artykułu.

## <a name="cache"></a>Pamięć podręczna

Buforowanie jest wydajny sposób przechowywania i pobierania danych. Można kontrolować okres istnienia pamięci podręcznej elementów na podstawie czasu i innych kwestii. Dowiedz się więcej o [buforowanie](../performance/caching/index.md).

<a name="session"></a>
## <a name="working-with-session-state"></a>Praca z stanu sesji

### <a name="configuring-session"></a>Konfigurowanie sesji

`Microsoft.AspNetCore.Session` Pakietu udostępnia oprogramowanie pośredniczące do zarządzania stanem sesji. Aby włączyć sesji oprogramowanie pośredniczące, `Startup` musi zawierać:

- Żadnego z [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) pamięci podręcznej pamięci. `IDistributedCache` Implementacji jest używany jako magazynu zapasowego dla sesji.
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) wywołać, co wymaga pakietu NuGet "Microsoft.AspNetCore.Session".
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) wywołania.

Poniższy kod przedstawia, jak skonfigurować dostawcę sesji w pamięci.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

Możesz odwoływać się do sesji z `HttpContext` po zostanie on zainstalowany i skonfigurowany.

Jeśli użytkownik próbuje uzyskać dostęp `Session` przed `UseSession` została wywołana, wyjątek `InvalidOperationException: Session has not been configured for this application or request` jest generowany.

Jeśli próbujesz utworzyć nowy `Session` (to znaczy, że pliki cookie sesji nie została utworzona) po już Rozpoczęto zapisywanie `Response` strumienia wyjątek `InvalidOperationException: The session cannot be established after the response has started` jest generowany. Wyjątek można znaleźć w dzienniku serwera sieci web; nie będzie wyświetlana w przeglądarce.

### <a name="loading-session-asynchronously"></a>Ładowany asynchronicznie sesji 

Domyślny dostawca sesji w ASP.NET Core ładuje rekordu sesji z podstawową [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) magazynu asynchronicznie tylko wtedy, gdy [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) metoda jawnie jest wywoływana przed  `TryGetValue`, `Set`, lub `Remove` metody. Jeśli `LoadAsync` nie jest wywoływany jako pierwszy, odpowiadającego rekordu sesji jest ładowany synchronicznie, które mogą potencjalnie wpłynąć na możliwość skalowania aplikacji.

Aby wymusić ten wzorzec aplikacji, zawijać [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) i [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementacje wersje zgłosić wyjątek, jeśli `LoadAsync` — metoda nie jest wywoływana przed `TryGetValue`, `Set`, lub `Remove`. Zarejestruj opakowana wersje w kontenerze usług.

### <a name="implementation-details"></a>Szczegóły implementacji

Sesja używa pliku cookie do śledzenia i zidentyfikować żądań z jednej przeglądarki. Domyślnie ten plik cookie o nazwie ". AspNet.Session"i używa ścieżką"/". Ponieważ domyślny plik cookie nie określa domeny, nie zostało to zrobione dla skryptu po stronie klienta na stronie (ponieważ `CookieHttpOnly` domyślnie `true`).

Aby zastąpić wartości domyślne sesji, użyj `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Serwer używa `IdleTimeout` właściwości w celu określenia, jak długo sesji może być bezczynne, zanim zostały porzucone jego zawartość. Ta właściwość jest niezależna od datę ważności pliku cookie. Każde żądanie, który przechodzi przez oprogramowanie pośredniczące sesji (odczytywane lub zapisywane) resetuje limit czasu.

Ponieważ `Session` jest *— blokowanie*, jeśli dwa żądania zarówno próbę zmodyfikowania zawartości sesji, ostatnią przesłania pierwszego. `Session`jest zaimplementowany jako *spójnego sesji*, co oznacza, że cała zawartość są przechowywane razem. Dwa żądania, które modyfikowania różnych części sesji (różne klucze) nadal może mieć wpływ na siebie.

### <a name="setting-and-getting-session-values"></a>Ustawianie i pobieranie wartości sesji

Sesja jest dostępny za pośrednictwem `Session` właściwość `HttpContext`. Ta właściwość jest [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementacji.

W poniższym przykładzie pokazano, ustawiania i pobierania int i ciąg:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Jeśli dodasz następujące metody rozszerzenia, należy Ustawianie i pobieranie obiekty możliwe do serializacji do sesji:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

Poniższy przykład przedstawia sposób Ustawianie i pobieranie obiektu podlegającego serializacji:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Praca z HttpContext.Items

`HttpContext` Abstrakcji zapewnia obsługę kolekcji słownika typu `IDictionary<object, object>`o nazwie `Items`. Ta kolekcja jest dostępny od początku *HttpRequest* i zostaną odrzucone pod koniec każdego żądania. Można do niego dostęp, przypisując wartość wpisu kluczem lub przez zażądanie wartość dla określonego klucza.

W przykładzie poniżej [oprogramowanie pośredniczące](middleware.md) dodaje `isVerified` do `Items` kolekcji.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Później w potoku innego oprogramowania pośredniczącego można do niego dostęp:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Dla oprogramowania pośredniczącego, która będzie używana tylko w jednej aplikacji `string` klucze są akceptowane. Jednak aby uniknąć wysłanej kolizji kluczy obiekt unikatowy kluczy należy używać oprogramowania pośredniczącego, które będą udostępniane między aplikacjami. Jeśli tworzysz oprogramowania pośredniczącego, które muszą działać dla wielu aplikacji, należy użyć klucza unikatowym obiektem zdefiniowany w klasie oprogramowanie pośredniczące, jak pokazano poniżej:

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Inny kod, mogą uzyskiwać dostęp do wartości przechowywanej w `HttpContext.Items` przy użyciu klucza udostępniane przez oprogramowanie pośredniczące klasy:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Takie podejście charakteryzuje się również zaletą wyeliminowanie powtórzenia "magic ciągi" w kilku miejscach w kodzie.

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>Dane o stanie aplikacji

Użyj [iniekcji zależności](xref:fundamentals/dependency-injection) udostępnić dane dla wszystkich użytkowników:

1. Zdefiniuj usługą zawierającego dane (na przykład klasa o nazwie `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Dodawanie klasy usługi do `ConfigureServices` (na przykład `services.AddSingleton<MyAppData>();`).
3. Korzystanie z klasy usługi danych w każdym kontrolerze:

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a>Typowe błędy podczas pracy z sesji

* "Nie można rozpoznać usługi dla typu"Microsoft.Extensions.Caching.Distributed.IDistributedCache"podczas próby aktywowania"Microsoft.AspNetCore.Session.DistributedSessionStore"."

  Jest to zazwyczaj spowodowane nie powiodło się skonfigurowanie co najmniej jednego `IDistributedCache` implementacji. Aby uzyskać więcej informacji, zobacz [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed) i [w ramach buforowania pamięci](xref:performance/caching/memory).

* W przypadku który sesji oprogramowanie pośredniczące nie powiedzie się, aby utrwalić sesji (na przykład: Jeśli bazy danych nie jest dostępna), rejestruje wyjątek i swallows go. Żądanie będzie się nadal normalnie, która prowadzi do bardzo nieprzewidywalne zachowanie.

Typowym przykładem:

Ktoś przechowuje koszyka zakupów w sesji. Użytkownik dodaje elementu, ale zatwierdzenia kończy się niepowodzeniem. Aplikacja nie może ustalić o awarii, zgłasza komunikat "element został dodany", który nie jest spełniony.

Zalecanym sposobem Sprawdź, czy błędy takie jest wywołać `await feature.Session.CommitAsync();` z kodu aplikacji, gdy wszystko będzie gotowe zapisywania do sesji. Następnie możesz zrobić, takich jak z powodu błędu. Działa tak samo, podczas wywoływania metody `LoadAsync`.

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Platformy ASP.NET Core 1.x: Przykładowy kod używany w tym dokumencie](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [Platformy ASP.NET Core 2.x: Przykładowy kod używany w tym dokumencie](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
