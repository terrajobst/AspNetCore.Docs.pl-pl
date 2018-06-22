---
title: Stan sesji i aplikacji w ASP.NET Core
author: rick-anderson
description: Wykryj podejścia, aby zachować stan sesji i aplikacji między żądaniami.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 9c63d9313acb055e6c692a7fef3d28e94cb37093
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272886"
---
# <a name="session-and-app-state-in-aspnet-core"></a>Stan sesji i aplikacji w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), i [Luke Latham](https://github.com/guardrex)

HTTP jest protokołem bezstanowe. Bez wykonywania dodatkowych czynności, żądania HTTP są niezależne wiadomości, które nie zachowuje stan aplikacji lub użytkownika. W tym artykule opisano kilka metod, aby zachować stan danych i aplikacji użytkownika między żądaniami.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="state-management"></a>Stan zarządzania

Stan mogą być przechowywane przy użyciu kilku metod. Każde podejście jest opisane w dalszej części tego tematu.

| Podejście magazynu | Mechanizmu magazynowania |
| ---------------- | ----------------- |
| [Pliki cookie](#cookies) | Pliki cookie protokołu HTTP (mogą obejmować dane przechowywane przy użyciu kodu aplikacji po stronie serwera) |
| [Stan sesji](#session-state) | Pliki cookie protokołu HTTP i kodu aplikacji po stronie serwera |
| [TempData](#tempdata) | Pliki cookie protokołu HTTP lub stan sesji |
| [Ciągi zapytań](#query-strings) | Ciągi zapytań HTTP |
| [Ukryte pola](#hidden-fields) | Pola formularza HTTP |
| [HttpContext.Items](#httpcontextitems) | Kod aplikacji po stronie serwera |
| [Cache](#cache) | Kod aplikacji po stronie serwera |
| [Wstrzykiwanie zależności](#dependency-injection) | Kod aplikacji po stronie serwera |

## <a name="cookies"></a>Pliki cookie

Pliki cookie są przechowywane dane żądań. Ponieważ pliki cookie są wysyłane z każdym żądaniem, ich rozmiar powinny być ograniczone do minimum. Najlepiej, jeśli tylko identyfikator powinny być przechowywane w pliku cookie z danych przechowywanych przez aplikację. W większości przeglądarek ograniczyć rozmiar pliku cookie do 4096 bajtów. Ograniczona liczba pliki cookie są dostępne dla każdej domeny.

Ponieważ pliki cookie podlegają naruszeniu, musi zostać zweryfikowany przez aplikację. Pliki cookie mogą zostać usunięte przez użytkowników i wygaśnie w dniu klientów. Jednak pliki cookie są zwykle najbardziej niezawodna formę trwałości danych na kliencie.

Pliki cookie są często używane na potrzeby personalizacji, gdy zawartość jest dostosowany do znanego użytkownika. Użytkownik jest tylko zidentyfikowane i nie jest uwierzytelniony w większości przypadków. Plik cookie można przechowywać nazwy użytkownika, nazwę konta lub unikatowe Identyfikatory (na przykład identyfikator GUID). Plik cookie umożliwia następnie uzyskać dostępu do spersonalizowanych ustawień użytkownika, takich jak ich kolor tła preferowanych witryny sieci Web.

Można w trosce o [interfejsów Unii Europejskiej ogólne dane ochrony wykonawcze (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) podczas wystawiania pliki cookie i dotyczących prywatności dotyczy. Aby uzyskać więcej informacji, zobacz [obsługę rozporządzenia ogólne ochrony danych (GDPR) w ASP.NET Core](xref:security/gdpr).

## <a name="session-state"></a>Stan sesji

Stan sesji jest scenariusz platformy ASP.NET Core dla magazynu danych użytkownika, gdy użytkownik będzie przeglądać aplikacji sieci web. Stan sesji używa magazynu obsługiwane przez aplikację do utrwalenia danych między żądań klienta. Dane sesji jest obsługiwana przez pamięć podręczną i uwzględniony danych tymczasowych&mdash;lokacji będą nadal działać bez danych sesji.

> [!NOTE]
> Sesja nie jest obsługiwany w [SignalR](xref:signalr/index) aplikacji ponieważ [koncentratora SignalR](xref:signalr/hubs) może być wykonywane niezależnie od kontekstu HTTP. Na przykład, to może wystąpić, gdy długi żądanie sondowania jest otwarty przez koncentrator poza okres istnienia kontekstu HTTP żądania.

Platformy ASP.NET Core zachowuje stan sesji, zapewniając pliku cookie do klienta, który zawiera identyfikator sesji, który jest wysyłany do aplikacji z każdym żądaniem. Aplikacja używa Identyfikatora sesji można pobrać danych sesji.

Stan sesji spowoduje następujące zachowania:

* Ponieważ plik cookie sesji jest specyficzna dla przeglądarki, sesji nie są współużytkowane przez przeglądarki.
* Pliki cookie dotyczące sesji są usuwane podczas kończenia sesji przeglądarki.
* Jeśli plik cookie zostanie odebrana dla wygasłych sesji, tworzony jest nowej sesji, który używa tego samego pliku cookie sesji.
* Pusty sesji nie są zachowywane&mdash;sesja musi mieć co najmniej jedną wartość ustawioną w nim utrwalić sesji dla żądań. Podczas sesji nie jest zachowywana, nowy identyfikator sesji jest generowany dla każdego nowego żądania.
* Aplikacja zachowuje sesję przez ograniczony czas, po zgłoszeniu ostatniego żądania. Aplikacja ustawia limit czasu sesji lub domyślną wartość 20 minut. Stan sesji jest idealny dla przechowywania danych użytkownika, która jest specyficzna dla konkretnej sesji, ale których danych nie wymaga magazynie trwałym między sesjami.
* Dane sesji zostaną usunięte albo gdy [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementacji jest wywoływana lub utraty ważności sesji.
* Nie istnieje domyślny mechanizm informują kodu aplikacji, przeglądarka klienta zostało zamknięte lub gdy usunięty lub ważność na kliencie pliku cookie sesji.

> [!WARNING]
> Nie należy przechowywać poufnych danych stanu sesji. Użytkownik nie może być Zamknij przeglądarkę i wyczyść pliku cookie sesji. Niektóre przeglądarki Obsługa plików cookie sesji prawidłowe między okna przeglądarki. Sesja nie może być ograniczony do jednego użytkownika&mdash;następnego użytkownika może w dalszym ciągu Przeglądaj aplikacji przy użyciu tego samego pliku cookie sesji.

Dostawca w pamięci podręcznej przechowuje dane sesji w pamięci serwera, w którym znajduje się aplikacja. W scenariuszu farmy serwera:

* Użyj *trwałe sesje* powiązać każdej sesji do wystąpienia określonej aplikacji na wybranym serwerze. [Usługa aplikacji Azure](https://azure.microsoft.com/services/app-service/) używa [Routing żądań aplikacji (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) wymusić trwałe sesje domyślnie. Trwałe sesje mogą jednak mieć wpływ na skalowalność i skomplikować aktualizacji aplikacji sieci web. Lepszym rozwiązaniem jest użycie pamięci podręcznej Redis lub SQL Server rozproszonej pamięci podręcznej, które nie wymagają trwałe sesje. Aby uzyskać więcej informacji, zobacz [pracować z rozproszonej pamięci podręcznej](xref:performance/caching/distributed).
* Plik cookie sesji jest szyfrowany za pomocą [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector). Ochrona danych muszą być poprawnie skonfigurowane do odczytywania plików cookie sesji na każdym komputerze. Aby uzyskać więcej informacji, zobacz [ochrony danych w ASP.NET Core](xref:security/data-protection/index) i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).

### <a name="configure-session-state"></a>Skonfiguruj stan sesji

::: moniker range=">= aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakietu, który jest dostępny w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), udostępnia oprogramowanie pośredniczące do zarządzania stanem sesji. Aby włączyć sesji oprogramowanie pośredniczące, `Startup` musi zawierać:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) zawiera pakiet oprogramowania pośredniczącego zarządzania stanu sesji. Aby włączyć sesji oprogramowanie pośredniczące, `Startup` musi zawierać:

::: moniker-end

* Żadnego z [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) pamięci podręcznej pamięci. `IDistributedCache` Implementacji jest używany jako magazynu zapasowego dla sesji. Aby uzyskać więcej informacji, zobacz [pracować z rozproszonej pamięci podręcznej](xref:performance/caching/distributed).
* Wywołanie [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) w `ConfigureServices`.
* Wywołanie [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) w `Configure`.

Poniższy kod przedstawia, jak skonfigurować dostawcę sesji w pamięci z domyślną implementację w pamięci `IDistributedCache`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

Ważna jest kolejność oprogramowania pośredniczącego. W powyższym przykładzie `InvalidOperationException` Wystąpił wyjątek podczas `UseSession` jest wywoływana po `UseMvc`. Aby uzyskać więcej informacji, zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware/index#ordering).

[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) jest dostępna, gdy stan sesji jest skonfigurowany.

`HttpContext.Session` Nie można uzyskać dostępu przed `UseSession` została wywołana.

Nie można utworzyć nowej sesji z nowego pliku cookie sesji, po jej rozpoczęciu zapisywania do strumienia odpowiedzi. Wyjątek jest rejestrowane w dzienniku serwera sieci web i nie są wyświetlane w przeglądarce.

### <a name="load-session-state-asynchronously"></a>Asynchronicznie ładowania stanu sesji

Domyślny dostawca sesji w ASP.NET Core ładuje rekordy sesji z podstawową [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) magazynu zapasowego asynchronicznie tylko wtedy, gdy [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) jawnie wywoływana jest metoda przed [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [ustawić](/dotnet/api/microsoft.aspnetcore.http.isession.set), lub [Usuń](/dotnet/api/microsoft.aspnetcore.http.isession.remove) metody. Jeśli `LoadAsync` nie jest wywoływany jako pierwszy, odpowiadającego rekordu sesji jest ładowany synchronicznie, która może spowodować zmniejszenie wydajności na dużą skalę.

Aby wymusić ten wzorzec aplikacji, zawijać [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) i [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementacje wersje zgłosić wyjątek, jeśli `LoadAsync` metoda nie jest wywoływana przed `TryGetValue`, `Set`, lub `Remove`. Zarejestruj opakowana wersje w kontenerze usług.

### <a name="session-options"></a>Opcje sesji

Aby zastąpić wartości domyślne sesji, użyj [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).

::: moniker range=">= aspnetcore-2.0"

| Opcja | Opis |
| ------ | ----------- |
| [Plik cookie](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | Określa ustawienia używane do utworzenia pliku cookie. [Nazwa](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) domyślnie [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). [Ścieżka](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) domyślnie [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) domyślnie [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`). [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) domyślnie `true`. [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) domyślnie `false`. |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` Wskazuje, jak długo sesja może być bezczynne, zanim zostały porzucone jego zawartość. Dostęp do każdej sesji resetuje limit czasu. Uwaga: dotyczy to tylko zawartość sesji, nie pliku cookie. Wartość domyślna to 20 minut. |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | Zasada ilość czasu dozwolone, aby załadować sesji z magazynu lub przekazać go do magazynu. Należy zauważyć, że tylko może to dotyczyć operacji asynchronicznych. Tego limitu czasu można wyłączyć przy użyciu [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan). Wartość domyślna to 1 minuta. |

Sesja używa pliku cookie do śledzenia i zidentyfikować żądań z jednej przeglądarki. Domyślnie ten plik cookie o nazwie `.AspNetCore.Session`, i używa ścieżkę `/`. Ponieważ domyślny plik cookie nie określa domeny, nie jest on dla skryptu po stronie klienta na stronie (ponieważ [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) domyślnie `true`).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Opcja | Opis |
| ------ | ----------- |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | Określa domenę użytą do utworzenia pliku cookie. `CookieDomain` nie jest ustawiona domyślnie. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | Określa, czy przeglądarka powinna zezwalać pliku cookie do uzyskiwał JavaScript po stronie klienta. Wartość domyślna to `true`, co oznacza, że plik cookie zostanie tylko przekazany do żądań HTTP i nie ma zostać udostępnione skryptu na stronie. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | Określa nazwę pliku cookie użytą do utrwalenia identyfikator sesji. Wartość domyślna to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | Określa ścieżkę użytą do utworzenia pliku cookie. Domyślnie [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | Określa, czy plik cookie powinien być przesyłany tylko na żądania HTTPS. Wartość domyślna to [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`). |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` Wskazuje, jak długo sesja może być bezczynne, zanim zostały porzucone jego zawartość. Dostęp do każdej sesji resetuje limit czasu. Uwaga: dotyczy to tylko zawartość sesji, nie pliku cookie. Wartość domyślna to 20 minut. |

Sesja używa pliku cookie do śledzenia i zidentyfikować żądań z jednej przeglądarki. Domyślnie ten plik cookie o nazwie `.AspNet.Session`, i używa ścieżkę `/`.

::: moniker-end

Aby zastąpić wartości pliku cookie sesji domyślnych, użyj `SessionOptions`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

Aplikacja używa [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) właściwości w celu określenia, jak długo sesji może być bezczynne, zanim jego zawartość w pamięci podręcznej serwera zostały porzucone. Ta właściwość jest niezależna od datę ważności pliku cookie. Każde żądanie, który przechodzi przez [oprogramowanie pośredniczące sesji](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resetuje limit czasu.

Stan sesji jest *— blokowanie*. Jeśli dwa żądania jednocześnie próbę zmodyfikowania zawartości sesji, ostatniego żądania przesłania pierwszego. `Session` jest zaimplementowany jako *spójnego sesji*, co oznacza, że cała zawartość są przechowywane razem. Gdy dwa żądania wyszukiwania można zmodyfikować wartości z innej sesji, ostatniego żądania mogą zastąpić sesji zmiany wprowadzone przez pierwszy.

### <a name="set-and-get-session-values"></a>Ustawianie i pobieranie wartości sesji

Stan sesji jest dostępny ze stron Razor [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) klasy lub MVC [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller) klasy z [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session). Ta właściwość jest [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementacji.

::: moniker range=">= aspnetcore-2.0"

`ISession` Implementacji zapewnia kilka metod rozszerzenia zestawu i pobrać całkowitą i wartości ciągu. Metody rozszerzenia znajdują się w [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) przestrzeni nazw (Dodaj `using Microsoft.AspNetCore.Http;` instrukcji w celu uzyskania dostępu do metody rozszerzenia) podczas [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) pakiet jest odwołuje się projekt. Oba pakiety znajdują się w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`ISession` Implementacji zapewnia kilka metod rozszerzenia zestawu i pobrać całkowitą i wartości ciągu. Metody rozszerzenia znajdują się w [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) przestrzeni nazw (Dodaj `using Microsoft.AspNetCore.Http;` instrukcji w celu uzyskania dostępu do metody rozszerzenia) podczas [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) pakiet jest odwołuje się projekt.

::: moniker-end

`ISession` metody rozszerzenia:

* [GET (ISession, ciąg)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString (ISession, ciąg)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [SetInt32 (Int32 ISession, ciąg)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString (ISession, ciąg, ciąg)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

Poniższy przykład pobiera wartość sesji `IndexModel.SessionKeyName` klucza (`_Name` w przykładowej aplikacji) na stronie aparatu Razor strony:

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

Poniższy przykład przedstawia sposób Ustawianie i pobieranie liczba całkowita i ciąg:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

Wszystkie dane sesji musi być serializowany scenariusza rozproszonej pamięci podręcznej, nawet w przypadku korzystania w pamięci podręcznej. Ciąg minimalnego i serializatorów numer są udostępniane (zobacz metody i metod rozszerzenia [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)). Typy złożone musi być serializowany przez użytkownika przy użyciu innego mechanizmu, takiego jak JSON.

Dodaj następujące metody rozszerzenia umożliwiające ustawianie i pobieranie obiekty możliwe do serializacji:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

Poniższy przykład przedstawia sposób Ustawianie i pobieranie obiektu podlegającego serializacji z metody rozszerzenia:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a>TempData

Przedstawia platformy ASP.NET Core [TempData właściwości modelu stron Razor strony](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) lub [TempData kontroler MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata). Ta właściwość przechowuje dane, dopóki nie jest do odczytu. [Zachować](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) i [wgląd](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) metod można użyć do sprawdzenia danych bez usuwania. TempData jest szczególnie przydatne podczas przekierowania, gdy dane są wymagane dla więcej niż jednego żądania. TempData jest implementowany przez dostawców TempData przy użyciu plików cookie lub stanu sesji.

### <a name="tempdata-providers"></a>TempData dostawców

::: moniker range=">= aspnetcore-2.0"

W programie ASP.NET Core 2.0 lub nowszej dostawca TempData na podstawie plików cookie jest używany domyślnie do przechowywania TempData w plikach cookie.

Dane pliku cookie jest zaszyfrowany przy użyciu [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), zakodowaną z [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), następnie fragmentaryczne. Ponieważ plik cookie jest fragmentaryczne, pojedynczy plik cookie rozmiar limit w ASP.NET Core 1.x nie ma zastosowania. Dane pliku cookie nie jest skompresowana, ponieważ kompresowania zaszyfrowanych danych może prowadzić do problemów z bezpieczeństwem takich jak [ataki CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków. Aby uzyskać więcej informacji o dostawcy TempData na podstawie plików cookie, zobacz [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Program ASP.NET Core 1.0 i 1.1 dostawca TempData stanu sesji jest dostawcą domyślnym.

::: moniker-end

### <a name="choose-a-tempdata-provider"></a>Wybierz dostawcę TempData

Wybieranie dostawcy TempData obejmuje kilka kwestii, takich jak:

1. Aplikacja już używa stanu sesji? Jeśli jest to tak, przy użyciu dostawcy TempData stan sesji nie ma żadnych dodatkowych kosztów do aplikacji (jako uzupełnienie rozmiar danych).
2. Aplikacja używa TempData tylko oszczędnie przypadku względnie niewielkich ilości danych (maksymalnie 500 bajtów)? Jeśli tak, dostawca TempData pliku cookie doda małych koszt na każde żądanie, który przenosi TempData. Jeśli nie, dostawca TempData stanu sesji korzystne może okazać się uniknąć dwustronną komunikację dużej ilości danych do wszystkich żądań do momentu TempData jest używany.
3. Czy aplikacja działa w farmie serwerów na wielu serwerach? Jeśli tak, istnieje dodatkowa konfiguracja, nie trzeba używać plików cookie dostawcy TempData poza ochrony danych (zobacz [ochrony danych](xref:security/data-protection/index) i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers)).

> [!NOTE]
> Większość klientów w sieci web (na przykład przeglądarki sieci web) wymuszać ograniczenia dotyczące maksymalny rozmiar każdego pliku cookie i całkowita liczba plików cookie. Podczas korzystania z dostawcy TempData pliku cookie, sprawdź, czy aplikacja nie przekroczy te ograniczenia. Należy wziąć pod uwagę całkowity rozmiar danych. Konto zwiększenie rozmiaru pliku cookie z powodu szyfrowania i podziału.

### <a name="configure-the-tempdata-provider"></a>Konfigurowanie dostawcy TempData

::: moniker range=">= aspnetcore-2.0"

Dostawca TempData na podstawie plików cookie jest domyślnie włączona.

Aby włączyć dostawcy TempData opartymi na sesji, należy użyć [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) — metoda rozszerzenia:

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Następujące `Startup` kod klasy konfiguruje dostawcy TempData opartymi na sesji:

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

Ważna jest kolejność oprogramowania pośredniczącego. W powyższym przykładzie `InvalidOperationException` Wystąpił wyjątek podczas `UseSession` jest wywoływana po `UseMvc`. Aby uzyskać więcej informacji, zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware/index#ordering).

> [!IMPORTANT]
> Jeśli przeznaczonych dla platformy .NET Framework i przy użyciu dostawcy TempData opartymi na sesji, należy dodać [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) pakietu do projektu.

## <a name="query-strings"></a>Ciągi zapytań

Ograniczoną ilość danych mogą być przekazywane między żądaniami do innego przez dodanie go do nowego żądania ciągu zapytania. Jest to przydatne w przypadku przechwytywania stanu w sposób ciągły, umożliwiający łącza osadzonego stanu do udostępnienia za pośrednictwem poczty e-mail lub sieci społecznościowych. Ponieważ ciągi zapytań adres URL są publiczne, nigdy nie używaj ciągów zapytania dla danych poufnych.

Oprócz udostępniania niezamierzone, włącznie z danymi w ciągów zapytania można tworzyć możliwości [sfałszowaniem żądań Cross-Site (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataków, których można wymuszać użytkowników do odwiedzenia złośliwych witryn podczas uwierzytelnienia. Osoby atakujące można kradzieży danych użytkownika z poziomu aplikacji lub podjąć złośliwe akcje w imieniu użytkownika. Dowolnej aplikacji zachowanego lub stanu sesji należy chronić przed atakami CSRF. Aby uzyskać więcej informacji, zobacz [zapobiec Cross-Site żądania Międzywitrynowego (XSRF/CSRF) przed atakami opartymi na](xref:security/anti-request-forgery).

## <a name="hidden-fields"></a>Ukryte pola

Dane można zapisane w ukrytym pól i opublikować ponownie przy kolejnym żądaniu. To jest typowe w formularzach wiele stron. Ponieważ klienta może potencjalnie manipulować danymi, aplikacja musi ponownie zawsze zatwierdzać danych przechowywanych w ukryte pola.

## <a name="httpcontextitems"></a>HttpContext.Items

[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) kolekcja jest używana do przechowywania danych podczas przetwarzania pojedynczego żądania. Zawartość kolekcji zostaną odrzucone po przetworzeniu żądania. `Items` Kolekcji jest często stosowane do umożliwienia składniki lub oprogramowanie pośredniczące do komunikacji, gdy działają w różnych punktach w czasie żądania i nie może bezpośrednio do przekazania parametrów.

W poniższym przykładzie [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) dodaje `isVerified` do `Items` kolekcji.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Później w potoku, innego oprogramowania pośredniczącego mogą uzyskać dostęp do wartości `isVerified`:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

Dla oprogramowania pośredniczącego, która jest używana tylko jednej aplikacji `string` klucze są akceptowane. Oprogramowanie pośredniczące udostępniane między wystąpieniami aplikacji należy używać obiektu unikatowy kluczy, aby uniknąć kolizji kluczy. Poniższy przykład przedstawia sposób użycia klucza unikatowym obiektem zdefiniowana w klasie oprogramowania pośredniczącego:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

Inny kod, mogą uzyskiwać dostęp do wartości przechowywanej w `HttpContext.Items` przy użyciu klucza udostępniane przez oprogramowanie pośredniczące klasy:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

Takie podejście charakteryzuje się również zaletą wyeliminowanie użycia klucza parametrów w kodzie.

## <a name="cache"></a>Pamięć podręczna

Buforowanie jest wydajny sposób przechowywania i pobierania danych. Aplikację można kontrolować okres istnienia pamięci podręcznej elementów.

Dane w pamięci podręcznej nie jest skojarzona z określonego żądania, użytkownika lub sesję. **Uważaj, aby nie pamięci podręcznej dane specyficzne dla użytkownika, które mogą być pobierane przez innych użytkowników żądań.**

Aby uzyskać więcej informacji, zobacz [buforuje odpowiedzi](xref:performance/caching/index) tematu.

## <a name="dependency-injection"></a>Iniekcji zależności

Użyj [iniekcji zależności](xref:fundamentals/dependency-injection) udostępnić dane dla wszystkich użytkowników:

1. Zdefiniuj usługą zawierający dane. Na przykład klasa o nazwie `MyAppData` jest zdefiniowana:

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. Dodawanie klasy usługi do `Startup.ConfigureServices`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. Korzystanie z klasy usługi danych:

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

  Jest to zazwyczaj spowodowane nie powiodło się skonfigurowanie co najmniej jednego `IDistributedCache` implementacji. Aby uzyskać więcej informacji, zobacz [pracować z rozproszonej pamięci podręcznej](xref:performance/caching/distributed) i [pamięci podręcznej w pamięci](xref:performance/caching/memory).

* W tym przypadku sesji, oprogramowanie pośredniczące nie powiedzie się, aby utrwalić sesji (na przykład jeśli magazynu zapasowego nie jest dostępna), oprogramowanie pośredniczące rejestruje wyjątek i kontynuuje działanie żądania. Prowadzi to do nieprzewidywalne zachowanie.

  Na przykład użytkownik zapisuje koszyk w sesji. Użytkownik dodaje element do koszyka, ale zatwierdzenia kończy się niepowodzeniem. Aplikacja nie może ustalić o awarii, raporty do użytkownika czy element został dodany do ich koszyka, który nie jest spełniony.

  Zalecane podejście, aby sprawdzić błędy dotyczące jest wywołanie `await feature.Session.CommitAsync();` z kodu aplikacji, gdy aplikacja działa zapisywania do sesji. `CommitAsync` zgłasza wyjątek, jeśli magazynu zapasowego jest niedostępny. Jeśli `CommitAsync` kończy się niepowodzeniem, aplikacja może przetwarzać wyjątek. `LoadAsync` zgłasza na warunkach, w którym magazyn danych jest niedostępny.
