---
title: Stan sesji i aplikacji w ASP.NET Core
author: rick-anderson
description: Odkryj podejścia do zachowywania stanu sesji i aplikacji między żądaniami.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: fundamentals/app-state
ms.openlocfilehash: b80b1e72eb2f25e9c9fe07a0c33c14ecf5ae05aa
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963484"
---
# <a name="session-and-app-state-in-aspnet-core"></a>Stan sesji i aplikacji w ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Dianę LaRose](https://github.com/DianaLaRose)i [Luke Latham](https://github.com/guardrex)

HTTP jest bezstanowy protokół. Bez podejmowania dodatkowych kroków żądania HTTP są niezależnymi komunikatami, które nie zachowują wartości użytkownika lub stanu aplikacji. W tym artykule opisano kilka metod zachowywania danych użytkownika i stanu aplikacji między żądaniami.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="state-management"></a>Zarządzanie stanem

Stan może być przechowywany przy użyciu kilku metod. Każde podejście zostało opisane w dalszej części tego tematu.

| Podejście do magazynu | Mechanizm magazynu |
| ---------------- | ----------------- |
| [Plik cookie](#cookies) | Pliki cookie protokołu HTTP (mogą zawierać dane przechowywane przy użyciu kodu aplikacji po stronie serwera) |
| [Stan sesji](#session-state) | Pliki cookie HTTP i kod aplikacji po stronie serwera |
| [TempData](#tempdata) | Pliki cookie HTTP lub stan sesji |
| [Ciągi zapytań](#query-strings) | Ciągi zapytań HTTP |
| [Ukryte pola](#hidden-fields) | Pola formularza HTTP |
| [HttpContext. Items](#httpcontextitems) | Kod aplikacji po stronie serwera |
| [Pamięć podręczna](#cache) | Kod aplikacji po stronie serwera |
| [Wstrzykiwanie zależności](#dependency-injection) | Kod aplikacji po stronie serwera |

## <a name="cookies"></a>Cookie

Pliki cookie przechowują dane między żądaniami. Ponieważ pliki cookie są wysyłane przy użyciu każdego żądania, ich rozmiar powinien być minimalny. W idealnym przypadku tylko identyfikator powinien być przechowywany w pliku cookie z danymi przechowywanymi w aplikacji. Większość przeglądarek ogranicza rozmiar plików cookie do 4096 bajtów. Dla każdej domeny dostępne są tylko ograniczone liczby plików cookie.

Ponieważ pliki cookie podlegają naruszeniu, muszą być zweryfikowane przez aplikację. Pliki cookie mogą zostać usunięte przez użytkowników i wygasnąć na klientach. Jednak pliki cookie są generalnie najbardziej trwałą formą trwałości danych na kliencie.

Pliki cookie są często używane do personalizacji, gdzie zawartość jest dostosowywana dla znanego użytkownika. Użytkownik jest identyfikowany i nie jest uwierzytelniany w większości przypadków. Plik cookie może przechowywać nazwę użytkownika, nazwę konta lub unikatowy identyfikator użytkownika (na przykład identyfikator GUID). Następnie można użyć pliku cookie, aby uzyskać dostęp do spersonalizowanych ustawień użytkownika, takich jak preferowany kolor tła witryny sieci Web.

Należy mieć na celu zachowanie [ogólnych przepisów dotyczących ochrony danych w Unii Europejskiej (Rodo)](https://ec.europa.eu/info/law/law-topic/data-protection) podczas wystawiania plików cookie i rozwiązywania problemów z ochroną prywatności. Aby uzyskać więcej informacji, zobacz temat [obsługa ogólne rozporządzenie o ochronie danych (Rodo) w programie ASP.NET Core](xref:security/gdpr).

## <a name="session-state"></a>Stan sesji

Stan sesji to ASP.NET Core scenariusz przechowywania danych użytkownika podczas przeglądania aplikacji sieci Web przez użytkownika. Stan sesji używa magazynu obsługiwanego przez aplikację w celu utrwalania danych między żądaniami od klienta. Dane sesji są obsługiwane przez pamięć podręczną i uznawane za dane tymczasowe&mdash;lokacja powinna nadal działać bez danych sesji. Krytyczne dane aplikacji powinny być przechowywane w bazie danych użytkownika i buforowane w sesji tylko jako Optymalizacja wydajności.

> [!NOTE]
> Sesja nie jest obsługiwana w aplikacjach [SignalR](xref:signalr/index) , ponieważ [centrumSignalR](xref:signalr/hubs) może działać niezależnie od kontekstu http. Na przykład może się to zdarzyć, gdy długotrwałe żądanie sondowania jest przechowywane przez centrum poza okresem istnienia kontekstu HTTP żądania.

ASP.NET Core utrzymuje stan sesji, dostarczając plik cookie do klienta zawierającego identyfikator sesji, który jest wysyłany do aplikacji przy użyciu każdego żądania. Aplikacja używa identyfikatora sesji w celu pobrania danych sesji.

Stan sesji wykazuje następujące zachowania:

* Ponieważ plik cookie sesji jest specyficzny dla przeglądarki, sesje nie są współużytkowane przez przeglądarki.
* Pliki cookie sesji są usuwane po zakończeniu sesji przeglądarki.
* W przypadku odebrania pliku cookie dla wygasłej sesji zostanie utworzona nowa sesja, która używa tego samego pliku cookie sesji.
* Puste sesje nie są zachowywane&mdash;sesja musi mieć ustawioną co najmniej jedną wartość, aby zachować sesję między żądaniami. Gdy sesja nie jest zachowywana, dla każdego nowego żądania jest generowany nowy identyfikator sesji.
* Aplikacja zachowuje sesję przez ograniczony czas po ostatnim żądaniu. Aplikacja ustawia limit czasu sesji lub używa wartości domyślnej wynoszącej 20 minut. Stan sesji jest idealnym rozwiązaniem do przechowywania danych użytkownika, które są specyficzne dla określonej sesji, ale w przypadku, gdy dane nie wymagają stałego magazynu między sesjami.
* Dane sesji są usuwane, gdy jest wywoływana implementacja [ISession. Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) lub gdy sesja wygaśnie.
* Nie istnieje domyślny mechanizm powiadamiania o kodzie aplikacji, że przeglądarka klienta została zamknięta lub gdy plik cookie sesji został usunięty lub wygasł na kliencie.
* ASP.NET Core MVC i szablony stron Razor obejmują obsługę Ogólne rozporządzenie o ochronie danych (Rodo). Pliki cookie stanu sesji nie są domyślnie oznaczone jako podstawowe, więc stan sesji nie działa, chyba że śledzenie jest dozwolone przez odwiedzających witrynę. Aby uzyskać więcej informacji, zobacz <xref:security/gdpr#tempdata-provider-and-session-state-cookies-arent-essential>.

> [!WARNING]
> Nie należy przechowywać poufnych danych w stanie sesji. Użytkownik może nie zamknąć przeglądarki i wyczyścić plik cookie sesji. Niektóre przeglądarki przechowują prawidłowe pliki cookie sesji w oknach przeglądarki. Sesja może nie być ograniczona do pojedynczego użytkownika,&mdash;następnym użytkownik może kontynuować przeglądanie aplikacji przy użyciu tego samego pliku cookie sesji.

Dostawca pamięci podręcznej w pamięci przechowuje dane sesji w pamięci serwera, na którym znajduje się aplikacja. W scenariuszu farmy serwerów:

* Użyj *sesji programu Sticky* , aby powiązać każdą sesję z określonym wystąpieniem aplikacji na pojedynczym serwerze. [Azure App Service](https://azure.microsoft.com/services/app-service/) używa [routingu żądań aplikacji (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) do wymuszania sesji usługi Sticky one domyślnie. Jednak sesje programu Sticky Notes mogą mieć wpływ na skalowalność i komplikują aktualizacje aplikacji sieci Web. Lepszym rozwiązaniem jest użycie rozproszonej pamięci podręcznej Redis lub SQL Server, która nie wymaga sesji programu Sticky Notes. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.
* Plik cookie sesji jest szyfrowany za pośrednictwem [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector). Ochrona danych musi być poprawnie skonfigurowana do odczytywania plików cookie sesji na poszczególnych komputerach. Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/introduction> i [dostawców magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).

### <a name="configure-session-state"></a>Konfigurowanie stanu sesji

Pakiet [Microsoft. AspNetCore. Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) , który jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), zapewnia oprogramowanie pośredniczące do zarządzania stanem sesji. Aby włączyć oprogramowanie pośredniczące sesji, `Startup` musi zawierać:

* Dowolna z pamięci podręcznej pamięci [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) . Implementacja `IDistributedCache` jest używana jako magazyn zapasowy dla sesji. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.
* Wywołanie [addsession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) w `ConfigureServices`.
* Wywołanie [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions.usesession#Microsoft_AspNetCore_Builder_SessionMiddlewareExtensions_UseSession_Microsoft_AspNetCore_Builder_IApplicationBuilder_) w `Configure`.

Poniższy kod pokazuje, jak skonfigurować dostawcę sesji w pamięci z domyślną implementacją w pamięci `IDistributedCache`:

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=5-14,34)]

Kolejność oprogramowania pośredniczącego jest ważna. W poprzednim przykładzie wyjątek `InvalidOperationException` występuje, gdy `UseSession` jest wywoływana po `UseMvc`. Aby uzyskać więcej informacji, zobacz [porządkowanie oprogramowania pośredniczącego](xref:fundamentals/middleware/index#order).

Element [HttpContext. Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) jest dostępny po skonfigurowaniu stanu sesji.

nie można uzyskać dostępu do `HttpContext.Session` przed wywołaniem `UseSession`.

Nie można utworzyć nowej sesji z nowym plikiem cookie sesji, gdy aplikacja zaczyna zapisywać w strumieniu odpowiedzi. Wyjątek jest rejestrowany w dzienniku serwera sieci Web i nie jest wyświetlany w przeglądarce.

### <a name="load-session-state-asynchronously"></a>Załaduj stan sesji asynchronicznie

Domyślny dostawca sesji w ASP.NET Core ładuje rekordy sesji z bazowego magazynu zapasowego [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) tylko wtedy, gdy metoda [ISession. LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) jest jawnie wywoływana przed metodami [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set)lub [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) . Jeśli `LoadAsync` nie zostanie najpierw wywołana, źródłowy rekord sesji jest ładowany synchronicznie, co może spowodować spadek wydajności na dużą skalę.

Aby aplikacje wymuszają ten wzorzec, zawiń implementacje [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) i [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) z wersjami, które zgłaszają wyjątek, jeśli metoda `LoadAsync` nie zostanie wywołana przed `TryGetValue`, `Set`lub `Remove`. Zarejestruj opakowane wersje w kontenerze usługi.

### <a name="session-options"></a>Opcje sesji

Aby zastąpić wartości domyślne sesji, należy użyć [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).

| Opcja | Opis |
| ------ | ----------- |
| [Plików](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | Określa ustawienia używane do tworzenia plików cookie. [Nazwa](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) domyślna to [SessionDefaults. cookiename](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). Domyślna [ścieżka](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) do [SessionDefaults. CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) domyślnie [SameSiteMode. swobodny](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`). [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) `true`. [Wartość domyślna](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) to `false`. |
| [Czynności](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` wskazuje, jak długo sesja może być bezczynna, zanim jej zawartość zostanie porzucona. Każdy dostęp do sesji resetuje limit czasu. To ustawienie dotyczy tylko zawartości sesji, a nie pliku cookie. Wartość domyślna to 20 minut. |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | Maksymalna ilość czasu, którą można załadować sesji ze sklepu lub zatwierdzić ją z powrotem do magazynu. To ustawienie może dotyczyć tylko operacji asynchronicznych. Ten limit czasu można wyłączyć za pomocą [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan). Wartość domyślna to 1 minuta. |

Sesja służy do śledzenia i identyfikowania żądań z pojedynczej przeglądarki. Domyślnie ten plik cookie nosi nazwę `.AspNetCore.Session`i używa ścieżki `/`. Ponieważ domyślnie plik cookie nie określa domeny, nie jest on dostępny dla skryptu po stronie klienta na stronie (ponieważ [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) domyślnie to `true`).

Aby zastąpić wartości domyślne sesji plików cookie, użyj `SessionOptions`:

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=14-19)]

Aplikacja używa właściwości [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) , aby określić, jak długo sesja może być bezczynna, zanim jej zawartość w pamięci podręcznej serwera zostanie porzucona. Ta właściwość jest niezależna od wygaśnięcia pliku cookie. Każde żądanie przechodzące przez [oprogramowanie pośredniczące sesji](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resetuje limit czasu.

Stan sesji to *nie jest blokowanie*. Jeśli dwa żądania jednocześnie próbują zmodyfikować zawartość sesji, ostatnie żądanie zastępuje pierwsze. `Session` jest zaimplementowana jako *spójna sesja*, co oznacza, że cała zawartość jest przechowywana razem. Gdy dwa żądania poszukują modyfikacji różnych wartości sesji, ostatnie żądanie może zastąpić zmiany sesji wykonane przez pierwsze.

### <a name="set-and-get-session-values"></a>Ustawianie i pobieranie wartości sesji

Uzyskano dostęp do stanu sesji z klasy [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) lub klasy [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller) MVC z Razor Pages obiektem [HttpContext. Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session). Ta właściwość jest implementacją [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) .

Implementacja `ISession` zawiera kilka metod rozszerzających, które umożliwiają ustawianie i pobieranie wartości całkowitych i ciągów. Metody rozszerzające znajdują się w przestrzeni nazw [Microsoft. AspNetCore. http](/dotnet/api/microsoft.aspnetcore.http) (dodaj instrukcję `using Microsoft.AspNetCore.Http;`, aby uzyskać dostęp do metod rozszerzenia), gdy odwołuje się do niego pakiet [Microsoft. AspNetCore. http. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) . Oba pakiety są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Metody rozszerzenia `ISession`:

* [Get (ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32 (ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString (ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [SetInt32 (ISession, String, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString (ISession, String, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

Poniższy przykład pobiera wartość sesji dla klucza `IndexModel.SessionKeyName` (`_Name` w aplikacji przykładowej) na stronie Razor Pages:

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

Poniższy przykład pokazuje, jak ustawić i pobrać liczbę całkowitą i ciąg:

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

Wszystkie dane sesji muszą być serializowane w celu włączenia scenariusza rozproszonej pamięci podręcznej, nawet w przypadku korzystania z pamięci podręcznej w pamięci. Podano minimalną liczbę serializatorów ciągów i liczb (Zobacz metody i metody rozszerzające [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)). Typy złożone muszą być serializowane przez użytkownika przy użyciu innego mechanizmu, takiego jak JSON.

Dodaj następujące metody rozszerzające, aby ustawić i pobrać możliwe do serializacji obiekty:

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

Poniższy przykład pokazuje, jak ustawić i pobrać obiekt możliwy do serializacji przy użyciu metod rozszerzających:

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="tempdata"></a>TempData

ASP.NET Core uwidacznia Razor Pages [TempData](xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.TempData) lub kontroler <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>. Ta właściwość przechowuje dane, dopóki nie zostanie odczytany w innym żądaniu. Metody [Keep (String)](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Keep*) i [Peek (String)](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Peek*) mogą służyć do badania danych bez usuwania na końcu żądania. Wartość [Keep ()](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Keep*) oznacza wszystkie elementy w słowniku do przechowywania. `TempData` jest szczególnie przydatne w przypadku przekierowywania, gdy dane są wymagane dla więcej niż jednego żądania. `TempData` jest implementowana przez dostawców `TempData` przy użyciu plików cookie lub stanu sesji.

## <a name="tempdata-samples"></a>Przykłady TempData

Rozważmy następującą stronę, która tworzy klienta:

[!code-csharp[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet&highlight=15-16,30)]

Na poniższej stronie przedstawiono `TempData["Message"]`:

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/IndexPeek.cshtml?range=1-14)]

W powyższym znaczniku na końcu żądania `TempData["Message"]` **nie** jest usuwana, ponieważ jest używane `Peek`. Odświeżenie strony powoduje wyświetlenie `TempData["Message"]`.

Poniższe znaczniki przypominają poprzedni kod, ale używają `Keep`, aby zachować dane na końcu żądania:

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/IndexKeep.cshtml?range=1-14)]

Przechodzenie między stronami *IndexPeek* i *IndexKeep* nie spowoduje usunięcia `TempData["Message"]`.

Poniższy kod wyświetla `TempData["Message"]`, ale na końcu żądania `TempData["Message"]` został usunięty:

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/Index.cshtml?range=1-14)]

### <a name="tempdata-providers"></a>Dostawcy TempData

Dostawca TempData oparty na plikach cookie jest domyślnie używany do przechowywania TempData w plikach cookie.

Dane plików cookie są szyfrowane za pomocą [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), kodowane z [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), a następnie fragmenty. Ponieważ plik cookie jest podzielony na fragmenty, limit rozmiaru pojedynczego pliku cookie znaleziony w ASP.NET Core 1. x nie ma zastosowania. Dane pliku cookie nie są kompresowane, ponieważ kompresowanie zaszyfrowanych danych może prowadzić do problemów z bezpieczeństwem, takich jak naruszenia [przestępczości](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszeń](https://wikipedia.org/wiki/BREACH_(security_exploit)) . Aby uzyskać więcej informacji na temat dostawcy TempData opartego na plikach cookie, zobacz [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).

### <a name="choose-a-tempdata-provider"></a>Wybierz dostawcę TempData

Wybór dostawcy TempData obejmuje kilka zagadnień, takich jak:

1. Czy aplikacja już używa stanu sesji? Jeśli tak, użycie dostawcy TempData stanu sesji nie ma dodatkowych kosztów dla aplikacji (poza rozmiarem danych).
2. Czy aplikacja będzie używać TempData tylko dla stosunkowo małych ilości danych (do 500 bajtów)? Jeśli tak, dostawca cookie TempData dodaje niewielki koszt do każdego żądania, które przenosi TempData. W przeciwnym razie dostawca TempData stanu sesji może być korzystne, aby uniknąć jednoczesnego wyzwolenia dużej ilości danych w każdym żądaniu do momentu użycia TempData.
3. Czy aplikacja działa w farmie serwerów na wielu serwerach? W takim przypadku nie jest wymagana dodatkowa konfiguracja do korzystania z dostawcy TempData cookie poza ochroną danych (zobacz <xref:security/data-protection/introduction> i [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers)).

> [!NOTE]
> Większość klientów sieci Web (takich jak przeglądarki sieci Web) wymusza limity maksymalnego rozmiaru każdego pliku cookie, łącznej liczby plików cookie lub obu tych elementów. W przypadku korzystania z dostawcy TempData cookie Sprawdź, czy aplikacja nie przekroczy tych limitów. Należy wziąć pod uwagę łączny rozmiar danych. Konto zwiększające rozmiar pliku cookie z powodu szyfrowania i fragmentacji.

### <a name="configure-the-tempdata-provider"></a>Konfigurowanie dostawcy TempData

Dostawca TempData oparty na plikach cookie jest domyślnie włączony.

Aby włączyć dostawcę TempData opartego na sesji, należy użyć metody rozszerzenia [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) :

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

Kolejność oprogramowania pośredniczącego jest ważna. W poprzednim przykładzie wyjątek `InvalidOperationException` występuje, gdy `UseSession` jest wywoływana po `UseMvc`. Aby uzyskać więcej informacji, zobacz [porządkowanie oprogramowania pośredniczącego](xref:fundamentals/middleware/index#order).

> [!IMPORTANT]
> Jeśli celem jest .NET Framework i użycie dostawcy TempData opartego na sesji, Dodaj pakiet [Microsoft. AspNetCore. Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) do projektu.

## <a name="query-strings"></a>Ciągi zapytań

Ograniczoną ilość danych można przekazywać z jednego żądania do innego przez dodanie go do ciągu zapytania nowego żądania. Jest to przydatne do przechwytywania stanu w sposób trwały, który umożliwia udostępnianie linków ze stanem osadzonym za pośrednictwem poczty e-mail lub sieci społecznościowych. Ponieważ ciągi zapytań URL są publiczne, nigdy nie używaj ciągów zapytań do poufnych danych.

Oprócz niezamierzonego udostępniania, w tym danych w ciągach zapytań, można tworzyć szanse na ataki [między lokacjami (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) , które mogą nakłonić użytkowników do odwiedzania złośliwych witryn podczas uwierzytelniania. Osoby atakujące mogą następnie ukraść dane użytkowników z aplikacji lub podejmować złośliwe działania w imieniu użytkownika. Wszelkie zachowane aplikacje lub Stany sesji muszą chronić przed atakami CSRF. Aby uzyskać więcej informacji, zobacz [Zapobiegaj fałszowaniu żądań między witrynami (XSRF/CSRF)](xref:security/anti-request-forgery).

## <a name="hidden-fields"></a>Ukryte pola

Dane można zapisywać w ukrytych polach formularzy i ogłaszane przy użyciu następnego żądania. Jest to typowe w formularzach wielostronicowych. Ponieważ klient może potencjalnie naruszać dane, aplikacja musi zawsze ponownie sprawdzić poprawność danych przechowywanych w ukrytych polach.

## <a name="httpcontextitems"></a>HttpContext. Items

Kolekcja [HttpContext. Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) służy do przechowywania danych podczas przetwarzania pojedynczego żądania. Zawartość kolekcji jest odrzucana po przetworzeniu żądania. Kolekcja `Items` jest często używana do zezwalania składnikom lub oprogramowaniu pośredniczącemu na komunikowanie się, gdy pracują w różnych punktach w czasie w trakcie żądania i nie mają bezpośredniego sposobu przekazywania parametrów.

W poniższym przykładzie [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) dodaje `isVerified` do kolekcji `Items`.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

W dalszej części potoku inne oprogramowanie pośredniczące może uzyskać dostęp do wartości `isVerified`:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

W przypadku oprogramowania pośredniczącego, które jest używane tylko przez pojedynczą aplikację, `string` klucze są akceptowalne. Oprogramowanie pośredniczące współużytkowane przez wystąpienia aplikacji powinno używać unikatowych kluczy obiektów, aby uniknąć kolizji kluczy. Poniższy przykład pokazuje, jak używać unikatowego klucza obiektu zdefiniowanego w klasie pośredniczącej:

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

Inny kod może uzyskać dostęp do wartości przechowywanej w `HttpContext.Items` przy użyciu klucza uwidacznianego przez klasę pośredniczącego oprogramowania:

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

Takie podejście ma również zalety wyeliminowania używania ciągów kluczy w kodzie.

## <a name="cache"></a>Pamięć podręczna

Buforowanie jest wydajnym sposobem przechowywania i pobierania danych. Aplikacja może kontrolować okres istnienia elementów w pamięci podręcznej.

Dane w pamięci podręcznej nie są skojarzone z określonym żądaniem, użytkownikiem lub sesją. **Należy zachować ostrożność, aby nie buforować danych specyficznych dla użytkownika, które mogą być pobierane przez inne żądania użytkowników.**

Aby uzyskać więcej informacji, zobacz <xref:performance/caching/response>.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Użyj [iniekcji zależności](xref:fundamentals/dependency-injection) , aby udostępnić dane wszystkim użytkownikom:

1. Zdefiniuj usługę zawierającą dane. Na przykład Klasa o nazwie `MyAppData` jest zdefiniowana:

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

3. Korzystaj z klasy usługi danych:

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

## <a name="common-errors"></a>Typowe błędy

* "Nie można rozpoznać usługi dla typu" Microsoft. Extensions. buforowania. Distributed. IDistributedCache "podczas próby aktywowania elementu" Microsoft. AspNetCore. Session. DistributedSessionStore "."

  Jest to zazwyczaj spowodowane niepowodzeniem konfigurowania co najmniej jednej implementacji `IDistributedCache`. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed> i <xref:performance/caching/memory>.

* W przypadku niepowodzenia utrwalania sesji przez oprogramowanie pośredniczące sesji (na przykład jeśli magazyn zapasowy nie jest dostępny), oprogramowanie pośredniczące rejestruje wyjątek, a żądanie nadal odbywa się normalnie. Prowadzi to do nieprzewidywalnego zachowania.

  Na przykład użytkownik przechowuje koszyk w sesji. Użytkownik dodaje element do koszyka, ale zatwierdzanie kończy się niepowodzeniem. Aplikacja nie wie o niepowodzeniu, dlatego zgłasza użytkownikowi informacje o tym, że element został dodany do swojego koszyka, co nie jest prawdziwe.

  Zalecanym podejściem do sprawdzenia pod kątem błędów jest wywołanie `await feature.Session.CommitAsync();` z kodu aplikacji, gdy aplikacja zakończy zapisywanie w sesji. `CommitAsync` zgłasza wyjątek, jeśli magazyn zapasowy jest niedostępny. Jeśli `CommitAsync` zakończy się niepowodzeniem, aplikacja może przetworzyć wyjątek. `LoadAsync` zgłaszany w ramach tych samych warunków, w których magazyn danych jest niedostępny.
  
## <a name="opno-locsignalr-and-session-state"></a>SignalR i stan sesji

aplikacje SignalR nie powinny używać stanu sesji do przechowywania informacji. aplikacje SignalR mogą przechowywać stan dla połączenia w `Context.Items` w centrum. <!-- https://github.com/aspnet/SignalR/issues/2139 -->

## <a name="additional-resources"></a>Dodatkowe zasoby

<xref:host-and-deploy/web-farm>
