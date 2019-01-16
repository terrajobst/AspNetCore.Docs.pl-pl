---
title: Stan sesji i aplikacji w programie ASP.NET Core
author: rick-anderson
description: Poznaj metody, aby zachować stan sesji i aplikacji między żądaniami.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 23bbb3063d696f588abecb1fe05ea3381ab2346d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341696"
---
# <a name="session-and-app-state-in-aspnet-core"></a>Stan sesji i aplikacji w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), i [Luke Latham](https://github.com/guardrex)

Protokół HTTP jest protokół bezstanowe. Bez wykonywania dodatkowych kroków, żądania HTTP są niezależnych komunikatów, które nie zawierają wartości użytkownika lub stan aplikacji. W tym artykule opisano kilka metod, aby zachować stan danych i aplikacji między żądaniami użytkownika.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="state-management"></a>Zarządzanie stanem

Stan, mogą być przechowywane przy użyciu kilku metod. Każde podejście jest opisane w dalszej części tego tematu.

| Podejście do magazynu | Mechanizm magazynu |
| ---------------- | ----------------- |
| [Plik cookie](#cookies) | Pliki cookie protokołu HTTP (mogą obejmować dane przechowywane przy użyciu kodu po stronie serwera aplikacji) |
| [Stan sesji](#session-state) | Pliki cookie protokołu HTTP i kodu aplikacji po stronie serwera |
| [TempData](#tempdata) | Pliki cookie protokołu HTTP lub stan sesji |
| [Ciągi zapytań](#query-strings) | Ciągi kwerendy HTTP |
| [Ukryte pola](#hidden-fields) | Pola formularza HTTP |
| [HttpContext.Items](#httpcontextitems) | Kod aplikacji po stronie serwera |
| [Pamięć podręczna](#cache) | Kod aplikacji po stronie serwera |
| [Wstrzykiwanie zależności](#dependency-injection) | Kod aplikacji po stronie serwera |

## <a name="cookies"></a>Pliki cookie

Pliki cookie przechowywane dane żądań. Ponieważ pliki cookie są wysyłane z każdym żądaniem, ich wielkości powinna być ograniczona do minimum. W idealnym przypadku tylko identyfikator powinny być przechowywane w pliku cookie z danymi przechowywanymi przez aplikację. W większości przeglądarek ograniczenie rozmiaru pliku cookie do 4096 bajtów. Tylko ograniczoną liczbę pliki cookie są dostępne dla każdej domeny.

Ponieważ pliki cookie podlegają naruszeniem, należy sprawdzić poprawności przez aplikację. Pliki cookie mogą zostać usunięte przez użytkowników i wygaśnięcia na komputerach klienckich. Jednak pliki cookie są zwykle najbardziej niezawodne formularza funkcji trwałości danych na komputerze klienckim.

Pliki cookie są często używane na potrzeby personalizacji, gdy zawartość jest dostosowany do znanego użytkownika. Użytkownik jest identyfikowane wyłącznie i nie jest uwierzytelniony w większości przypadków. Plik cookie można przechowywać nazwy użytkownika, nazwa konta lub Unikatowy identyfikator użytkownika (np. identyfikator GUID). Następnie można pliku cookie dostępu spersonalizowane ustawienia użytkownika, takich jak kolor tła ich preferowany witryny sieci Web.

Można w trosce o [Unii Europejskiej ogólnego ochronie danych wykonawczych (RODO)](https://ec.europa.eu/info/law/law-topic/data-protection) podczas wystawiania pliki cookie i rozwiązywania problemów związanych z prywatnością dotyczy. Aby uzyskać więcej informacji, zobacz [obsługi ogólne rozporządzenie o ochronie danych (RODO) w programie ASP.NET Core](xref:security/gdpr).

## <a name="session-state"></a>Stan sesji

Stan sesji jest scenariusz platformy ASP.NET Core, do przechowywania danych użytkownika, gdy użytkownik przegląda aplikacji sieci web. Stan sesji używa magazynu utrzymywane przez aplikację, aby utrwalać dane między żądań klienta. Dane sesji są obsługiwane przez pamięć podręczną i uznane za danych tymczasowych&mdash;witryny powinny nadal działać bez żadnych danych sesji.

> [!NOTE]
> Sesja nie jest obsługiwane w [SignalR](xref:signalr/index) aplikacje ponieważ [Centrum SignalR](xref:signalr/hubs) może zostać wykonany niezależnie od kontekstu HTTP. Na przykład, to może wystąpić, gdy długo żądanie sondowania jest otwarte przez koncentrator poza okres istnienia kontekstu HTTP żądania.

Platforma ASP.NET Core zachowuje stan sesji, zapewniając pliku cookie do klienta, który zawiera identyfikator sesji, który jest wysyłany do aplikacji z każdym żądaniem. Aplikacja używa Identyfikatora sesji można pobrać danych sesji.

Stan sesji wykazuje następujące zachowania:

* Ponieważ plik cookie sesji jest określone w przeglądarce, sesje nie są współużytkowane w różnych przeglądarkach.
* Pliki cookie dotyczące sesji są usuwane po zakończeniu sesji przeglądarki.
* Odebranie pliku cookie sesji wygasła, zostanie utworzona nowa sesja, która używa tego samego pliku cookie sesji.
* Sesje puste nie są zachowywane&mdash;sesja musi mieć co najmniej jedną wartość do niej sesji ulegają zmianie podczas żądania. Gdy sesja nie zostaną zachowane, nowy identyfikator sesji jest generowany dla każdego nowego żądania.
* Aplikacja zachowuje sesję przez ograniczony czas, po wykonaniu ostatniego żądania. Aplikacja ustawia limit czasu sesji lub domyślną wartość 20 minut. Stan sesji jest idealnym rozwiązaniem do przechowywania danych użytkownika, który jest specyficzny dla określonej sesji, ale których danych nie wymaga trwałego magazynu między sesjami.
* Dane sesji zostaną usunięte albo gdy [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementacja jest nazywany lub utraty ważności sesji.
* Nie ma domyślnego mechanizmu do informowania kod aplikacji przeglądarki klienta został zamknięty lub gdy plik cookie sesji został usunięty lub wygasła w dniu klienta.
Szablony stron ASP.NET Core MVC i Razor obejmują obsługę [ogólne rozporządzenie o ochronie danych (RODO), obsługa](xref:security/gdpr). [Pliki cookie z sesji stanu nie są istotne](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential), stan sesji nie jest funkcjonalności, gdy śledzenie jest wyłączone.

> [!WARNING]
> Nie należy przechowywać poufne dane stanu sesji. Użytkownik może nie Zamknij przeglądarkę i wyczyść plik cookie sesji. Niektóre przeglądarki pliki cookie z sesji prawidłowe zachowanie okna przeglądarki. Sesja nie może być ograniczone do pojedynczego użytkownika&mdash;następnego użytkownika mogą w dalszym ciągu Przeglądaj aplikację przy użyciu tego samego pliku cookie sesji.

Dostawcy pamięci podręcznej przechowuje dane sesji w pamięci serwera, w którym znajduje się aplikacja. W przypadku scenariusza farmy serwera:

* Użyj *trwałych sesji* powiązać każdej sesji do wystąpienia specyficzne dla aplikacji na wybranym serwerze. [Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) używa [Routing żądań aplikacji (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) do wymuszania trwałych sesji domyślnie. Jednak trwałych sesji może mieć wpływ na skalowalność i skomplikować aktualizacje aplikacji sieci web. Lepszym rozwiązaniem jest użycie pamięci podręcznej Redis lub SQL Server rozproszonej pamięci podręcznej, która nie wymaga trwałych sesji. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.
* Plik cookie sesji jest szyfrowany za pomocą [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector). Ochrona danych muszą zostać prawidłowo skonfigurowane, można odczytać plików cookie sesji na każdym komputerze. Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/introduction> i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).

### <a name="configure-session-state"></a>Skonfiguruj stan sesji

::: moniker range=">= aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), udostępnia oprogramowanie pośredniczące do zarządzania stanem sesji. Aby włączyć oprogramowanie pośredniczące sesji, `Startup` musi zawierać:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakiet zawiera oprogramowanie pośredniczące do zarządzania stanem sesji. Aby włączyć oprogramowanie pośredniczące sesji, `Startup` musi zawierać:

::: moniker-end

* Jedną z [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) pamięci podręcznej pamięci. `IDistributedCache` Implementacja jest używana jako magazyn zapasowy dla sesji. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.
* Wywołanie [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) w `ConfigureServices`.
* Wywołanie [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) w `Configure`.

Poniższy kod przedstawia sposób konfigurowania dostawcy sesji w pamięci z domyślną implementację w pamięci `IDistributedCache`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

Ważna jest kolejność oprogramowania pośredniczącego. W powyższym przykładzie `InvalidOperationException` wyjątek występuje wtedy, gdy `UseSession` jest wywoływana po `UseMvc`. Aby uzyskać więcej informacji, zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware/index#order).

[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) jest dostępna po skonfigurowaniu stanu sesji.

`HttpContext.Session` Nie można uzyskać dostępu przed `UseSession` została wywołana.

Nie można utworzyć nowej sesji z nowego pliku cookie sesji, po jej rozpoczęciu pisania do strumienia odpowiedzi. Wyjątek jest rejestrowane w dzienniku serwera sieci web i nie są wyświetlane w przeglądarce.

### <a name="load-session-state-asynchronously"></a>Asynchroniczne ładowanie stanu sesji

Domyślny dostawca sesji w programie ASP.NET Core ładuje rekordy sesji z bazowego [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) magazyn zapasowy asynchronicznie tylko wtedy, gdy [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) jawnie wywoływana jest metoda przed [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [ustaw](/dotnet/api/microsoft.aspnetcore.http.isession.set), lub [Usuń](/dotnet/api/microsoft.aspnetcore.http.isession.remove) metody. Jeśli `LoadAsync` nie jest wywoływany jako pierwszy, podstawowe rekordu sesji jest ładowany synchronicznie, która może spowodować zmniejszenie wydajności na dużą skalę.

Aby aplikacje, wymuszać tego wzorca, opakowywanie [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) i [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementacje z wersjami, które zgłosić wyjątek, jeśli `LoadAsync` metoda nie jest wywoływana przed `TryGetValue`, `Set`, lub `Remove`. Zarejestruj opakowana wersje w kontenerze usługi.

### <a name="session-options"></a>Opcje sesji

Aby zastąpić domyślne ustawienia sesji, użyj [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).

::: moniker range=">= aspnetcore-2.0"

| Opcja | Opis |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | Określa ustawienia używane do utworzenia pliku cookie. [Nazwa](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) wartość domyślna to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). [Ścieżka](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) wartość domyślna to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) wartość domyślna to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`). [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) wartość domyślna to `true`. [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) wartość domyślna to `false`. |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` Wskazuje, ile sesji może być bezczynne, zanim ich porzuceniu jego zawartość. Dostęp do każdej sesji resetuje limit czasu. To ustawienie dotyczy tylko do zawartości danej sesji, a nie pliku cookie. Wartość domyślna to 20 minut. |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | Maksymalnej wersji ilość czasu może załadować sesji z magazynu lub do przekazania go z powrotem do magazynu. To ustawienie może dotyczyć tylko operacji asynchronicznych. Limit czasu można wyłączyć za pomocą [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan). Wartość domyślna to 1 minuta. |

Sesja używa pliku cookie do śledzenia i identyfikowania żądań z jednej przeglądarki. Domyślnie ten plik cookie o nazwie `.AspNetCore.Session`, i używa ścieżki z `/`. Ponieważ domyślny plik cookie nie określono domeny, nie jest on dla skryptu po stronie klienta na stronie (ponieważ [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) wartość domyślna to `true`).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Opcja | Opis |
| ------ | ----------- |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | Określa domenę użytą do utworzenia pliku cookie. `CookieDomain` nie jest domyślnie ustawiona. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | Określa, jeśli przeglądarka powinna zezwalać plików cookie były dostępne dla JavaScript po stronie klienta. Wartość domyślna to `true`, co oznacza, że plik cookie zostanie tylko przekazany do żądań HTTP i nie jest udostępniany skryptu na stronie. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | Określa nazwę pliku cookie użytą do utrwalenia identyfikatora sesji. Wartość domyślna to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | Określa ścieżkę użytą do utworzenia pliku cookie. Wartość domyślna to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | Określa, jeśli plik cookie powinien być przesyłany tylko na żądania HTTPS. Wartość domyślna to [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`). |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` Wskazuje, ile sesji może być bezczynne, zanim ich porzuceniu jego zawartość. Dostęp do każdej sesji resetuje limit czasu. Należy pamiętać, że dotyczy to tylko zawartość sesji, a nie pliku cookie. Wartość domyślna to 20 minut. |

Sesja używa pliku cookie do śledzenia i identyfikowania żądań z jednej przeglądarki. Domyślnie ten plik cookie o nazwie `.AspNet.Session`, i używa ścieżki z `/`.

::: moniker-end

Aby zastąpić plik cookie sesji z ustawień domyślnych, użyj `SessionOptions`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

Ta aplikacja używa [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) właściwości w celu określenia, ile sesji może być bezczynne, zanim jego zawartość w pamięci podręcznej serwera są porzucone. Ta właściwość jest niezależna od datę ważności pliku cookie. Każde żądanie, które przechodzą przez [oprogramowania pośredniczącego sesji](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resetuje limit czasu.

Stan sesji jest *bez blokady*. Jeśli dwa żądania jednocześnie podejmie próbę zmodyfikowania zawartości sesji, ostatniego żądania przesłania pierwszego. `Session` jest implementowany jako *spójnego sesji*, co oznacza, że cała zawartość są przechowywane razem. Gdy dwa żądania dążyć do zmodyfikowania wartości w innej sesji, ostatniego żądania może spowodować zastąpienie zmian sesji przez pierwszy.

### <a name="set-and-get-session-values"></a>Ustawianie i pobieranie wartości sesji

Stan sesji jest dostępny ze stronami Razor [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) klasy lub MVC [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller) klasy [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session). Ta właściwość jest [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementacji.

::: moniker range=">= aspnetcore-2.0"

`ISession` Implementacja udostępnia kilka metod rozszerzenia do ustawiania i pobierania wartości liczby całkowitej, ciągu. Metody rozszerzające są w [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) przestrzeni nazw (Dodaj `using Microsoft.AspNetCore.Http;` instrukcję, aby uzyskać dostęp do metod rozszerzenia) podczas [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) pakiet jest przywoływany przez projekt. Oba pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`ISession` Implementacja udostępnia kilka metod rozszerzenia do zestawu i pobrać wartości liczby całkowitej, ciągu. Metody rozszerzające są w [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) przestrzeni nazw (Dodaj `using Microsoft.AspNetCore.Http;` instrukcję, aby uzyskać dostęp do metod rozszerzenia) podczas [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) pakiet jest przywoływany przez projekt.

::: moniker-end

`ISession` metody rozszerzenia:

* [GET (ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString — (ISession, ciąg)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [SetInt32 (ISession, String, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [Setstring — (ISession, String, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

Poniższy przykład pobiera wartość sesji `IndexModel.SessionKeyName` klucza (`_Name` w przykładowej aplikacji) na stronie stron Razor:

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

Poniższy przykład pokazuje, jak ustawiać i pobierać całkowitą i ciąg:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

Wszystkie dane sesji muszą być serializowane umożliwiające scenariusza rozproszonej pamięci podręcznej, nawet w przypadku korzystania z pamięci podręcznej. Znajdują się minimalne ciąg i liczba serializatorów (zobacz metody i metody rozszerzenia [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)). Złożone typy muszą być serializowane przez użytkownika przy użyciu innego mechanizmu, takiego jak JSON.

Dodaj następujące metody rozszerzenia ustawiać i pobierać obiekty możliwe do serializacji:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

Poniższy przykład pokazuje, jak ustawiać i pobierać obiektu podlegającego serializacji przy użyciu metody rozszerzenia:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a>TempData

Udostępnia platformy ASP.NET Core [TempData właściwości modelu strony stron Razor](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) lub [TempData kontroler MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata). Ta właściwość przechowuje dane, dopóki nie jest do odczytu. [Zachować](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) i [wgląd](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) metody może służyć do sprawdzenia danych bez usuwania. TempData jest szczególnie przydatne w przypadku przekierowania w przypadku, gdy dane są wymagane dla więcej niż jedno żądanie. TempData jest implementowany przez dostawców TempData za pomocą plików cookie lub stanu sesji.

### <a name="tempdata-providers"></a>TempData dostawców

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core w wersji 2.0 lub nowszej dostawca TempData na podstawie plików cookie jest używany domyślnie do przechowywania TempData w plikach cookie.

Dane pliku cookie jest zaszyfrowany przy użyciu [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), zakodowany za pomocą [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), następnie fragmentaryczne. Ponieważ plik cookie jest fragmentaryczne, pojedynczy plik cookie rozmiar limitem odczytanym z platformą ASP.NET Core 1.x nie ma zastosowania. Dane pliku cookie nie jest skompresowany, ponieważ kompresja zaszyfrowanych danych może prowadzić do problemów zabezpieczeń takich jak [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków. Aby uzyskać więcej informacji na temat dostawcy TempData na podstawie plików cookie, zobacz [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

W programie ASP.NET Core 1.0 i 1.1 TempData dostawcy stanu sesji jest dostawcą domyślnym.

::: moniker-end

### <a name="choose-a-tempdata-provider"></a>Wybierz dostawcę TempData

Wybieranie dostawcy TempData obejmuje kilka zagadnień, takich jak:

1. Aplikacja już używa stanu sesji? Jeśli tak, przy użyciu dostawcy TempData stanu sesji ma bez dodatkowych kosztów do aplikacji (jako uzupełnienie rozmiar danych).
2. Aplikacja używa TempData tylko rzadko stosunkowo małe ilości danych (maks. 500 w bajtach)? Jeśli tak, plik cookie dostawcy TempData dodaje niewielkim kosztem do każdego żądania, który przenosi TempData. W przeciwnym razie TempData dostawcy stanu sesji może być korzystne uniknąć Pełna zgodnooć wersji dużej ilości danych do wszystkich żądań do momentu TempData jest używany.
3. Czy aplikacja działa w farmie serwerów na wielu serwerach? Jeśli tak, istnieje dodatkowa konfiguracja, nie trzeba używać dostawcy TempData plik cookie poza ochrony danych (zobacz <xref:security/data-protection/introduction> i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers)).

> [!NOTE]
> Większość klientów sieci web (na przykład przeglądarki sieci web) wymuszać limity maksymalny rozmiar poszczególnych plików cookie i/lub całkowita liczba plików cookie. Korzystając z dostawcy TempData plików cookie, sprawdź, czy aplikacja nie będzie przekroczenia limitów. Należy wziąć pod uwagę całkowity rozmiar danych. Konto na potrzeby zwiększenie rozmiaru pliku cookie z powodu szyfrowania i segmentu.

### <a name="configure-the-tempdata-provider"></a>Konfigurowanie dostawcy TempData

::: moniker range=">= aspnetcore-2.0"

Dostawca TempData na podstawie plików cookie jest domyślnie włączona.

Aby włączyć dostawcy TempData oparte na sesji, należy użyć [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) — metoda rozszerzenia:

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Następujące `Startup` kod klasy konfiguruje dostawcę TempData oparte na sesji:

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

Ważna jest kolejność oprogramowania pośredniczącego. W powyższym przykładzie `InvalidOperationException` wyjątek występuje wtedy, gdy `UseSession` jest wywoływana po `UseMvc`. Aby uzyskać więcej informacji, zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware/index#order).

> [!IMPORTANT]
> Jeśli dodasz przeznaczonych dla platformy .NET Framework i przy użyciu dostawcy TempData oparte na sesji [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakietu do projektu.

## <a name="query-strings"></a>Ciągi zapytań

Ograniczona ilość danych, mogą być przekazywane między żądaniami do innego, dodając ją do ciągu zapytania nowe żądanie. Jest to przydatne w przypadku przechwytywania stanu w sposób ciągły, który umożliwia na łącza osadzonego stan ma być udostępniony za pośrednictwem poczty e-mail lub sieci społecznościowych. Ponieważ ciągi zapytania URL są publiczne, nigdy nie używaj ciągi zapytań dla danych poufnych.

Oprócz udostępniania niezamierzone, włącznie z danymi w ciągach zapytań można tworzyć możliwości [fałszerstwo żądania Międzywitrynowego Międzywitrynowych](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataków, które mogą sprawić użytkowników do odwiedzenia złośliwych witryn uwierzytelnieniu. Osoby atakujące mogą następnie kradzieży danych użytkownika z aplikacji lub podjąć działania złośliwego w imieniu użytkownika. Dowolną aplikację zachowanych lub stanu sesji należy chronić przed atakami CSRF. Aby uzyskać więcej informacji, zobacz [zapobiec Cross-Site Request Forgery (XSRF/CSRF) ataki](xref:security/anti-request-forgery).

## <a name="hidden-fields"></a>Ukryte pola

Dane można zapisać w postaci ukrytego pola i opublikowany ponownie przy kolejnym żądaniu. To jest typowe w formularzach wiele stron. Ponieważ klient może potencjalnie manipulować danymi, aplikacja musi zawsze przechowywać dane przechowywane w ukrytych polach.

## <a name="httpcontextitems"></a>HttpContext.Items

[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) kolekcji jest używany do przechowywania danych podczas przetwarzania pojedynczego żądania. Zawartość kolekcji zostaną odrzucone po przetworzeniu żądania. `Items` Kolekcji jest często używany do Zezwalaj na składniki lub oprogramowaniu pośredniczącym, aby komunikować się, gdy działają w różnych punktach w czasie dla żądania i bezpośredni sposób przekazania parametrów.

W poniższym przykładzie [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) dodaje `isVerified` do `Items` kolekcji.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Później w potoku innego oprogramowania pośredniczącego może uzyskać dostęp do wartości `isVerified`:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

Dla oprogramowania pośredniczącego, która jest używana tylko przez pojedynczą aplikacją `string` klucze są akceptowane. Oprogramowanie pośredniczące udostępniane między wystąpieniami aplikacji należy używać kluczy unikatowy obiekt, aby uniknąć najważniejsze kolizje. Poniższy przykład pokazuje, jak używać klucza unikatowy obiekt zdefiniowanej w klasie oprogramowanie pośredniczące:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

Inne kod może uzyskać dostęp, wartość przechowywana we `HttpContext.Items` przy użyciu klucza udostępniane przez oprogramowanie pośredniczące klasy:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

Takie podejście również ma tę zaletę wyeliminowanie korzystanie z kluczowych ciągów w kodzie.

## <a name="cache"></a>Pamięć podręczna

Buforowanie jest skuteczny sposób przechowywania i pobierania danych. Aplikacja może kontrolować okres istnienia pamięci podręcznej elementów.

Dane w pamięci podręcznej nie jest skojarzona z konkretnego żądania, użytkownika lub sesję. **Uważaj, aby nie pamięci podręcznej dane specyficzne dla użytkownika, które mogą być pobierane przez żądania do innych użytkowników.**

Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response>.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Użyj [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) udostępniania danych dla wszystkich użytkowników:

1. W celu zdefiniowania usługi zawierającej dane. Na przykład, klasę o nazwie `MyAppData` zdefiniowano:

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. Dodaj klasę usługi do `Startup.ConfigureServices`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. Używanie klasy usługi danych:

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a>Typowe błędy

* "Nie można rozpoznać usługi dla typu"Microsoft.Extensions.Caching.Distributed.IDistributedCache"podczas próby aktywowania"Microsoft.AspNetCore.Session.DistributedSessionStore"."

  Jest to zazwyczaj spowodowane nie można skonfigurować co najmniej jedną `IDistributedCache` implementacji. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed> i <xref:performance/caching/memory>.

* W tym przypadku sesję, którą oprogramowanie pośredniczące nie powiedzie się, aby utrwalić sesji (na przykład, jeśli magazyn zapasowy nie jest dostępna), oprogramowanie pośredniczące rejestruje wyjątek i kontynuuje działanie żądania. Prowadzi to do nieprzewidywalne zachowanie.

  Na przykład użytkownik zapisuje koszyka zakupów w sesji. Użytkownik dodaje element do koszyka, ale zatwierdzenie zakończy się niepowodzeniem. Aplikacja nie wie o awarii, dzięki użytkownikowi raportuje, czy element został dodany do ich zawartość koszyka nie dotyczy.

  Zalecane podejście do sprawdzania błędów jest wywołanie `await feature.Session.CommitAsync();` od kodu aplikacji po jej zakończeniu pisania do sesji. `CommitAsync` zgłasza wyjątek, jeśli magazyn pomocniczy jest niedostępny. Jeśli `CommitAsync` zakończy się niepowodzeniem, aplikacja może przetworzyć wyjątku. `LoadAsync` zgłasza takich samych warunkach, w którym magazyn danych jest niedostępny.

## <a name="additional-resources"></a>Dodatkowe zasoby

<xref:host-and-deploy/web-farm>
