---
title: Co nowego w ASP.NET Core 3,0
author: rick-anderson
description: Dowiedz się więcej o nowych funkcjach w ASP.NET Core 3,0.
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: aspnetcore-3.0
ms.openlocfilehash: a32307f6de01e03e548cbde2c38c9469db74b506
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688044"
---
# <a name="whats-new-in-aspnet-core-30"></a>Co nowego w ASP.NET Core 3,0

W tym artykule przedstawiono najbardziej znaczące zmiany w ASP.NET Core 3,0 z linkami do odpowiedniej dokumentacji.

## <a name="blazor"></a>Blazor

Blazor to nowa struktura w ASP.NET Core tworzenia interakcyjnego interfejsu użytkownika sieci Web po stronie klienta przy użyciu platformy .NET:

* Utwórz rozbudowane interaktywne C# interfejsów użytkownika przy użyciu zamiast języka JavaScript.
* Udostępnianie logiki aplikacji po stronie serwera i klienta zapisaną w środowisku .NET.
* Renderuj interfejs użytkownika jako HTML i CSS, aby obsługiwał szeroką przeglądarkę, w tym przeglądarki dla urządzeń przenośnych.

Scenariusze obsługiwane przez środowisko Blazor Framework:

* Składniki interfejsu użytkownika wielokrotnego użytku (składniki Razor)
* Routing po stronie klienta
* Układy składników
* Obsługa iniekcji zależności
* Formularze i walidacja
* Kompiluj biblioteki składników z bibliotekami klas Razor
* Międzyoperacyjność w języku JavaScript

Aby uzyskać więcej informacji, zobacz <xref:blazor/index>.

### <a name="blazor-server"></a>Serwer Blazor

Blazor oddziela logikę renderowania składników od sposobu stosowania aktualizacji interfejsu użytkownika. Serwer Blazor zapewnia obsługę hostingu składników Razor na serwerze w aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika są obsługiwane przez połączenie sygnalizujące. Serwer Blazor jest obsługiwany w ASP.NET Core 3,0.

### <a name="blazor-webassembly-preview"></a>Webassembly Blazor (wersja zapoznawcza)

Aplikacje Blazor można również uruchamiać bezpośrednio w przeglądarce przy użyciu środowiska uruchomieniowego .NET opartego na zestawie. Zestaw webBlazor jest w wersji zapoznawczej i *nie* jest obsługiwany w ASP.NET Core 3,0. Blazor webassembly będzie obsługiwany w przyszłej wersji ASP.NET Core.

### <a name="razor-components"></a>Składniki Razor

Aplikacje Blazor są kompilowane ze składników. Składniki to samodzielne fragmenty interfejsu użytkownika (UI), takie jak strona, okno dialogowe lub formularz. Składniki to normalne klasy .NET, które definiują logikę renderowania interfejsu użytkownika i obsługę zdarzeń po stronie klienta. Możesz tworzyć rozbudowane interaktywne aplikacje sieci Web bez języka JavaScript.

Składniki w Blazor są zwykle tworzone przy użyciu składnia Razor, naturalnej mieszanki HTML i C#. Składniki Razor są podobne do widoków Razor Pages i MVC, w których oba używają Razor. W przeciwieństwie do stron i widoków, które są oparte na modelu odpowiedzi na żądanie, składniki są używane do obsługi kompozycji interfejsu użytkownika.

## <a name="grpc"></a>gRPC

[gRPC](https://grpc.io/):

* To popularne środowisko RPC o wysokiej wydajności (zdalne wywołanie procedury).
* Oferuje ceniona kontrakt do programowania interfejsu API.
* Używa nowoczesnych technologii, takich jak:

  * HTTP/2 do transportu.
  * Bufory protokołu jako język opisu interfejsu.
  * Binarny format serializacji.
* Udostępnia funkcje takie jak:

  * Authentication
  * Dwukierunkowe przesyłanie strumieniowe i sterowanie przepływem.
  * Anulowanie i przekroczenie limitu czasu.

funkcje gRPC w ASP.NET Core 3,0 obejmują:

* [GRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; platformę ASP.NET Core do hostowania usług GRPC. gRPC na ASP.NET Core integruje się ze standardowymi funkcjami ASP.NET Core, takimi jak rejestrowanie, iniekcja zależności (DI), uwierzytelnianie i autoryzacja.
* [GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; klient programu GRPC dla platformy .NET Core, który kompiluje `HttpClient`się na znanym czasie.
* [GRPC .NET. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; GRPC klienta integracji z `HttpClientFactory`programem.

Aby uzyskać więcej informacji, zobacz <xref:grpc/index>.

## <a name="signalr"></a>SignalR

Instrukcje dotyczące migracji można znaleźć w temacie [Aktualizowanie kodu sygnalizującego](xref:migration/22-to-30#signalr) . Program sygnalizujący jest `System.Text.Json` teraz wykorzystywany do serializacji/deserializacji komunikatów JSON. Aby uzyskać instrukcje dotyczące przywracania serializatora opartego na pliku `Newtonsoft.Json`, zobacz [Switch to Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson) .

W klientach JavaScript i .NET dla sygnalizującego dodano obsługę automatycznego ponownego połączenia. Domyślnie klient próbuje ponownie nawiązać połączenie, a następnie ponowić próbę po 2, 10 i 30 sekundach, jeśli jest to konieczne. Jeśli klient pomyślnie nawiąże połączenie, otrzymuje nowy identyfikator połączenia. Automatyczne ponowne łączenie jest zgodą:

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Interwały ponownego połączenia można określić, przekazując tablicę czasów trwania opartych na milisekundach:

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

Implementację niestandardową można przekazywać w celu zapewnienia pełnej kontroli nad interwałami ponownego połączenia.

Jeśli ponowne połączenie nie powiedzie się po ostatnim interwale ponownego połączenia:

* Klient uważa, że połączenie jest w trybie offline.
* Klient przestanie próbować ponownie nawiązać połączenie.

Podczas próby ponownego połączenia zaktualizuj interfejs użytkownika aplikacji, aby powiadomić użytkownika o tym, że trwa próba ponownego połączenia.

Aby zapewnić informacje zwrotne interfejsu użytkownika w przypadku przerwania połączenia, interfejs API klienta sygnalizującego został rozszerzony w celu uwzględnienia następujących programów obsługi zdarzeń:

* `onreconnecting`:  Umożliwia deweloperom wyłączenie interfejsu użytkownika lub umożliwienie użytkownikom znajomości aplikacji w trybie offline.
* `onreconnected`: Oferuje deweloperom możliwość aktualizowania interfejsu użytkownika po jego nawiązaniu.

Następujący kod używa `onreconnecting` do aktualizowania interfejsu użytkownika podczas próby nawiązania połączenia:

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

Następujący kod używa `onreconnected` do aktualizowania interfejsu użytkownika w połączeniu:

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

Sygnał 3,0 i nowsze udostępnia zasób niestandardowy do programów obsługi autoryzacji, gdy metoda centrum wymaga autoryzacji. Zasób jest wystąpieniem `HubInvocationContext`. `HubInvocationContext` Obejmuje:

* `HubCallerContext`
* Nazwa wywoływanej metody centrum.
* Argumenty metody centrum.

Rozważmy następujący przykład aplikacji pokoju rozmów, która umożliwia logowanie w wielu organizacjach za pośrednictwem Azure Active Directory. Każda osoba mająca konto Microsoft może zalogować się do programu chat, ale tylko członkowie organizacji będącej właścicielem mogą zakazać użytkowników lub wyświetlać historie rozmów użytkowników. Aplikacja może ograniczyć niektóre funkcje do określonych użytkowników.

```csharp
public class DomainRestrictedRequirement :
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>,
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement,
        HubInvocationContext resource)
    {
        if (context.User?.Identity?.Name == null)
        {
            return Task.CompletedTask;
        }

        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name))
        {
            context.Succeed(requirement);
        }

        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName, string currentUsername)
    {
        if (hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase))
        {
            return currentUsername.Equals("bob42@jabbr.net", StringComparison.OrdinalIgnoreCase);
        }

        return currentUsername.EndsWith("@jabbr.net", StringComparison.OrdinalIgnoreCase));
    }
}
```

W poprzednim kodzie program `DomainRestrictedRequirement` służy jako niestandardowy. `IAuthorizationRequirement` Ponieważ parametr `HubInvocationContext` zasobu jest przesyłany, wewnętrzna logika może:

* Sprawdź kontekst, w którym jest wywoływany centrum.
* Podejmowanie decyzji na umożliwienie użytkownikowi wykonywania poszczególnych metod centrów.

Poszczególne metody centrów mogą być dekoracyjne z nazwą zasad, które kod sprawdza w czasie wykonywania. Gdy klienci próbują wywołać poszczególne metody centrum, `DomainRestrictedRequirement` program obsługi i kontroluje dostęp do tych metod. W oparciu o sposób dostępu `DomainRestrictedRequirement` do formantów:

* Wszyscy zalogowani użytkownicy mogą wywoływać `SendMessage` metodę.
* Historie użytkowników mogą wyświetlać tylko użytkownicy, którzy `@jabbr.net` zalogowali się przy użyciu adresu e-mail.
* Mogą `bob42@jabbr.net` oni jedynie blokowanie użytkowników z pokoju rozmowy.

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}
```

`DomainRestricted` Tworzenie zasad może wymagać:

* W *Startup.cs*Dodaj nowe zasady.
* Podaj niestandardowe `DomainRestrictedRequirement` wymagania jako parametr.
* Rejestrowanie `DomainRestricted` w oprogramowaniu pośredniczącym autoryzacji.

```csharp
services
    .AddAuthorization(options =>
    {
        options.AddPolicy("DomainRestricted", policy =>
        {
            policy.Requirements.Add(new DomainRestrictedRequirement());
        });
    });
```

Centra sygnałów używają [routingu punktu końcowego](xref:fundamentals/routing). Połączenie centrum sygnałów zostało wcześniej wykonane jawnie:

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

W poprzedniej wersji deweloperzy musieli połączyć kontrolery, strony Razor i centra w różnych różnych miejscach. Jawne połączenie daje w wyniku serię niemal identycznych segmentów routingu:

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});

app.UseRouting(routes =>
{
    routes.MapRazorPages();
});
```

Centra sygnalizujące 3,0 mogą być kierowane za pośrednictwem routingu punktu końcowego. Za pomocą routingu punktów końcowych, zazwyczaj wszystkie Routing można `UseRouting`skonfigurować w:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

Dodano ASP.NET Core sygnalizujący 3,0:

Przesyłanie strumieniowe klient-serwer. W przypadku przesyłania strumieniowego klient-serwer metody po stronie serwera mogą przyjmować wystąpienia `IAsyncEnumerable<T>` lub. `ChannelReader<T>` W poniższym C# przykładzie `UploadStream` Metoda w centrum będzie odbierać strumienie ciągów od klienta:

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

Aplikacje klienckie `IAsyncEnumerable<T>` platformy .NET mogą przekazać `ChannelReader<T>` albo wystąpienie `UploadStream` jako `stream` argument powyższej metody centrum.

Po zakończeniu `for` pętli, gdy funkcja lokalna zostanie zakończona, zostanie wysłany strumień:

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
}

await connection.SendAsync("UploadStream", clientStreamData());
```

Aplikacje `Subject` klienckie JavaScript używają sygnalizującego (lub [tematu RxJS](https://rxjs.dev/api/index/class/Subject)) dla `stream` argumentu `UploadStream` powyższej metody centrum.

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

Kod JavaScript może używać `subject.next` metody do obsługi ciągów, które są przechwytywane i gotowe do wysłania do serwera.

```javascript
subject.next("example");
subject.complete();
```

Korzystając z kodu, takiego jak dwa poprzednie fragmenty, można utworzyć środowiska przesyłania strumieniowego w czasie rzeczywistym.

### <a name="new-json-serialization"></a>Nowa Serializacja JSON

ASP.NET Core 3,0 teraz domyślnie <xref:System.Text.Json> używa dla serializacji JSON:

* Odczytuje i zapisuje dane JSON asynchronicznie.
* Jest zoptymalizowany pod kątem tekstu w formacie UTF-8.
* Zwykle wyższa wydajność niż `Newtonsoft.Json`.

Aby dodać Json.NET do ASP.NET Core 3,0, zobacz [Dodawanie obsługi formatu JSON opartego na Newtonsoft. JSON](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).

## <a name="new-razor-directives"></a>Nowe dyrektywy Razor

Poniższa lista zawiera nowe dyrektywy Razor:

* [@attribute](xref:mvc/views/razor#attribute)&ndash; Dyrektywastosujedanyatrybutdoklasywygenerowanej`@attribute` strony lub widoku. Na przykład `@attribute [Authorize]`.
* [@implements](xref:mvc/views/razor#implements)&ndash; Dyrektywaimplementujeinterfejsdla`@implements` wygenerowanej klasy. Na przykład `@implements IDisposable`.

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a>Usługi identityserver4 obsługuje uwierzytelnianie i autoryzację dla interfejsów API sieci Web i aplikacji jednostronicowych

[Usługi identityserver4](https://identityserver.io) to struktura OpenID Connect Connect i OAuth 2,0 dla ASP.NET Core 3,0. Usługi identityserver4 włącza następujące funkcje zabezpieczeń:

* Uwierzytelnianie jako usługa (AaaS)
* Logowanie jednokrotne (SSO) dla wielu typów aplikacji
* Kontrola dostępu do interfejsów API
* Brama federacyjna

Aby uzyskać więcej informacji, zobacz [Welcome to usługi identityserver4](http://docs.identityserver.io/en/latest/index.html).

## <a name="certificate-and-kerberos-authentication"></a>Certyfikat i uwierzytelnianie Kerberos

Wymagane jest uwierzytelnianie certyfikatu:

* Konfigurowanie serwera do akceptowania certyfikatów.
* Dodawanie oprogramowania pośredniczącego uwierzytelniania w `Startup.Configure`programie.
* Dodawanie usługi uwierzytelniania certyfikatów w programie `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

Opcje uwierzytelniania certyfikatów obejmują:

* Zaakceptuj certyfikaty z podpisem własnym.
* Sprawdź, czy certyfikaty są odwoływane.
* Sprawdź, czy certyfikat proffered ma odpowiednie flagi użycia.

Domyślny podmiot zabezpieczeń użytkownika jest konstruowany ze wszystkich właściwości certyfikatu. Nazwa główna użytkownika zawiera zdarzenie, które umożliwia uzupełnianie lub zastępowanie podmiotu zabezpieczeń. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/certauth>.

[Uwierzytelnianie systemu Windows](/windows-server/security/windows-authentication/windows-authentication-overview) zostało rozszerzone na system Linux i macOS. W poprzednich wersjach uwierzytelnianie systemu Windows było ograniczone do [usług IIS](xref:host-and-deploy/iis/index) i [HttpSys](xref:fundamentals/servers/httpsys). W ASP.NET Core 3,0 [Kestrel](xref:fundamentals/servers/kestrel) ma możliwość używania [protokołów](/windows-server/security/kerberos/kerberos-authentication-overview)Negotiate, Kerberos i [NTLM w systemach Windows](/windows-server/security/kerberos/ntlm-overview), Linux i macOS dla hostów przyłączonych do domeny systemu Windows. Kestrel obsługa tych schematów uwierzytelniania jest udostępniana przez pakiet [NuGet Microsoft. AspNetCore. Authentication. Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) . Podobnie jak w przypadku innych usług uwierzytelniania, skonfiguruj aplikację uwierzytelniania Wide, a następnie skonfiguruj usługę:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
        .AddNegotiate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

Wymagania dotyczące hosta:

* Hosty z systemem Windows muszą mieć dodane [główne nazwy usług](/windows/win32/ad/service-principal-names) (SPN) do konta użytkownika obsługującego aplikację.
* Maszyny z systemami Linux i macOS muszą być przyłączone do domeny.
  * Dla procesu sieci Web należy utworzyć nazwy SPN.
  * Na komputerze hosta muszą być generowane i skonfigurowane [pliki plik KEYTAB](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) .

Aby uzyskać więcej informacji, zobacz <xref:security/authentication/windowsauth>.

## <a name="template-changes"></a>Zmiany szablonu

Szablony interfejsu użytkownika sieci Web (Razor Pages, MVC z kontrolerem i widokami) mają następujące usunięte:

* Interfejs użytkownika zgody na plik cookie nie jest już uwzględniony. Aby włączyć funkcję wyrażania zgody plików cookie w aplikacji ASP.NET Core 3,0 szablonu wygenerowanej, zobacz <xref:security/gdpr>.
* Skrypty i powiązane zasoby statyczne są teraz przywoływane jako pliki lokalne zamiast używać sieci CDN. Aby uzyskać więcej informacji, zobacz [skrypty i powiązane zasoby statyczne są teraz odwołujące się do plików lokalnych zamiast używania sieci CDN w oparciu o bieżące środowisko (ASPNET/AspNetCore. Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).

Szablon kątowy został zaktualizowany do użycia kątowy 8.

Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor. Nowa opcja szablonu w programie Visual Studio zapewnia obsługę szablonów dla stron i widoków. Podczas tworzenia RCL z szablonu w powłoce poleceń, należy przekazać `-support-pages-and-views` opcję (`dotnet new razorclasslib -support-pages-and-views`).

## <a name="generic-host"></a>Host ogólny

Szablony ASP.NET Core 3,0 używają <xref:fundamentals/host/generic-host>. Użyto <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>poprzednich wersji. Korzystanie z hosta ogólnego platformy .NET Core<xref:Microsoft.Extensions.Hosting.HostBuilder>() zapewnia lepszą integrację aplikacji ASP.NET Core z innymi scenariuszami serwera, które nie są specyficzne dla sieci Web. Aby uzyskać więcej informacji, zobacz [HostBuilder zastępuje WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).

### <a name="host-configuration"></a>Konfiguracja hosta

Przed wydaniem ASP.NET Core 3,0, zmienne środowiskowe poprzedzone prefiksem `ASPNETCORE_` zostały załadowane na potrzeby konfiguracji hosta hosta sieci Web. W 3,0 `AddEnvironmentVariables` służy do ładowania zmiennych środowiskowych, które są `DOTNET_` poprzedzone prefiksem konfiguracji `CreateDefaultBuilder`hosta w programie.

### <a name="changes-to-startup-contructor-injection"></a>Zmiany wprowadzane do uruchomienia konstruktora

Host generyczny obsługuje tylko następujące typy dla `Startup` iniekcji konstruktora:

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

Wszystkie usługi można nadal dodawać bezpośrednio jako argumenty `Startup.Configure` metody. Aby uzyskać więcej informacji, zobacz [host ogólny ogranicza iniekcje konstruktorów uruchamiania (ASPNET/anonse #353)](https://github.com/aspnet/Announcements/issues/353).

## <a name="kestrel"></a>Kestrel

* Konfiguracja Kestrel została zaktualizowana na potrzeby migracji do hosta ogólnego. W 3,0 Kestrel jest skonfigurowany w konstruktorze hosta sieci Web dostarczonym `ConfigureWebHostDefaults`przez program.
* Adaptery połączeń zostały usunięte z usługi Kestrel i zastąpione przez oprogramowanie pośredniczące połączenia, które jest podobne do oprogramowania pośredniczącego HTTP w potoku ASP.NET Core ale dla połączeń niższego poziomu.
* Warstwa transportu Kestrel została udostępniona jako interfejs publiczny w programie `Connections.Abstractions`.
* Niejednoznaczność między nagłówkami a przyczepami została rozwiązana przez przeniesienie końcowych nagłówków do nowej kolekcji.
* Synchroniczne interfejsy API we/wy `HttpRequest.Body.Read`, takie jak, są typowym źródłem zablokowania wątków prowadzącego do awarii aplikacji. W 3,0, `AllowSynchronousIO` jest domyślnie wyłączona.

Aby uzyskać więcej informacji, zobacz <xref:migration/22-to-30#kestrel>.

## <a name="http2-enabled-by-default"></a>Protokół HTTP/2 włączony domyślnie

Protokół HTTP/2 jest domyślnie włączony w Kestrel dla punktów końcowych HTTPS. Obsługa protokołu HTTP/2 dla usług IIS lub HTTP. sys jest włączona, gdy jest ona obsługiwana przez system operacyjny.

## <a name="eventcounters-on-request"></a>EventCounters na żądanie

Hosting EventSource `Microsoft.AspNetCore.Hosting`,, emituje następujące nowe <xref:System.Diagnostics.Tracing.EventCounter> typy związane z żądaniami przychodzącymi:

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a>Routing punktów końcowych

Routing punktów końcowych, który umożliwia platformom (na przykład MVC) współdziałanie z oprogramowanie pośredniczące, jest ulepszony:

* Kolejność programów pośredniczących i punktów końcowych można skonfigurować w potoku `Startup.Configure`przetwarzania żądań.
* Punkty końcowe i oprogramowanie pośredniczące dobrze tworzą inne technologie ASP.NET Core, na przykład kontrole kondycji.
* Punkty końcowe mogą implementować zasady, takie jak CORS lub Authorization, zarówno w oprogramowaniu pośredniczącym, jak i MVC.
* Filtry i atrybuty mogą być umieszczane na metodach w kontrolerach.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing#routing-basics>.

## <a name="health-checks"></a>Kontrole kondycji

Kontrole kondycji korzystają z routingu punktu końcowego z hostem ogólnym. W `Startup.Configure`programie Wywołaj `MapHealthChecks` program Endpoint Builder z adresem URL punktu końcowego lub ścieżką względną:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

Punkty końcowe sprawdzania kondycji mogą:

* Określ jeden lub więcej dozwolonych hostów/portów.
* Wymagaj autoryzacji.
* Wymagaj mechanizmu CORS.

Aby uzyskać więcej informacji zobacz następujące artykuły:

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a>Potoki w obiekcie HttpContext

Teraz można odczytać treść żądania i napisać treść odpowiedzi przy użyciu <xref:System.IO.Pipelines> interfejsu API. Program <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> `HttpRequest.BodyReader`Właściwość zawiera wartość <xref:System.IO.Pipelines.PipeReader> , która może być używana do odczytywania treści żądania. Program <!-- <xref:Microsoft.AspNetCore.Http.> --> `HttpResponse.BodyWriter`Właściwość zawiera wartość <xref:System.IO.Pipelines.PipeWriter> , która może być używana do pisania treści odpowiedzi. `HttpRequest.BodyReader`jest analogiczny `HttpRequest.Body` do strumienia. `HttpResponse.BodyWriter`jest analogiczny `HttpResponse.Body` do strumienia.

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a>Udoskonalone raportowanie błędów w usługach IIS

Błędy uruchamiania podczas hostowania aplikacji ASP.NET Core w usługach IIS teraz generują bogatsze dane diagnostyczne. Te błędy są zgłaszane w dzienniku zdarzeń systemu Windows przy użyciu śladów stosu wszędzie tam, gdzie ma to zastosowanie. Ponadto wszystkie ostrzeżenia, błędy i Nieobsłużone wyjątki są rejestrowane w dzienniku zdarzeń systemu Windows.

## <a name="worker-service-and-worker-sdk"></a>Zestaw SDK usługi i procesu roboczego

W programie .NET Core 3,0 wprowadzono nowy szablon aplikacji usługi Worker. Ten szablon stanowi punkt początkowy do pisania długotrwałych usług w programie .NET Core.

Aby uzyskać więcej informacji, zobacz:

* [Pracownicy .NET Core jako usługi systemu Windows](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a>Podniesienie — ulepszenia oprogramowania pośredniczącego

W poprzednich wersjach ASP.NET Core wywoływanie <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> i <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> problemy występujące podczas wdrażania w systemie Linux systemu Azure lub poza serwerem proxy innych niż usługi IIS. Poprawka dla poprzednich wersji została udokumentowana w [tym samym schemacie dla systemu Linux i zwrotnych serwerów proxy innych niż IIS](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).

Ten scenariusz jest ustalony w ASP.NET Core 3,0. Host włącza program [pośredniczący z przekierowanymi nagłówkami](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) , `ASPNETCORE_FORWARDEDHEADERS_ENABLED` gdy zmienna środowiskowa jest ustawiona na `true`. `ASPNETCORE_FORWARDEDHEADERS_ENABLED`jest ustawiona na `true` w naszym obrazach kontenera.

## <a name="performance-improvements"></a>Usprawnienia wydajności

ASP.NET Core 3,0 zawiera wiele ulepszeń, które zmniejszają wykorzystanie pamięci i zwiększają przepływność:

* Zmniejszenie użycia pamięci w przypadku korzystania z wbudowanego kontenera iniekcji zależności dla usług objętych zakresem.
* Zmniejszenie alokacji w całym środowisku, w tym scenariusze i Routing oprogramowania pośredniczącego.
* Zmniejszenie użycia pamięci dla połączeń protokołu WebSocket.
* Zmniejszenie ilości pamięci i ulepszenia przepływności dla połączeń HTTPS.
* Nowy, zoptymalizowany i w pełni asynchroniczny serializator JSON.
* Zmniejszenie użycia pamięci i ulepszeń przepływności podczas analizowania formularzy.

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a>ASP.NET Core 3,0 działa tylko na platformie .NET Core 3,0

Począwszy od ASP.NET Core 3,0, .NET Framework nie jest już obsługiwaną platformą docelową. Projekty ukierunkowane na .NET Framework mogą być nadal w pełni obsługiwane przy użyciu programu [.NET Core 2,1 LTS Release](https://www.microsoft.com/net/download/dotnet-core/2.1). Większość pakietów pokrewnych ASP.NET Core 2.1. x będzie obsługiwana przez czas dłuższy niż 3-letni okres LTS dla platformy .NET Core 2,1.

Informacje o migracji znajdują się w temacie [Porting Code from .NET Framework to .NET Core](/dotnet/core/porting/).

## <a name="use-the-aspnet-core-shared-framework"></a>Użyj ASP.NET Core udostępnionej platformy

Współdzielona struktura ASP.NET Core 3,0, która znajduje się w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), nie wymaga już jawnego `<PackageReference />` elementu w pliku projektu. W przypadku korzystania z `Microsoft.NET.Sdk.Web` zestawu SDK w pliku projektu automatycznie występuje odwołanie do udostępnionej struktury:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a>Zestawy usunięte z ASP.NET Core udostępnionej platformy

Najbardziej istotnymi zestawami usuniętymi ze współdzielonej platformy ASP.NET Core 3,0 są:

* [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/) (JSON.NET). Aby dodać Json.NET do ASP.NET Core 3,0, zobacz [Dodawanie obsługi formatu JSON opartego na Newtonsoft. JSON](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support). ASP.NET Core 3,0 wprowadza `System.Text.Json` do odczytu i zapisu JSON. Aby uzyskać więcej informacji, zobacz [Nowa Serializacja kodu JSON](#new-json-serialization) w tym dokumencie.
* [Entity Framework Core](/ef/core/)

Aby uzyskać pełną listę zestawów usuniętych z udostępnionej platformy, zobacz [zestawy usuwane z Microsoft. AspNetCore. App 3,0](https://github.com/aspnet/AspNetCore/issues/3755). Aby uzyskać więcej informacji na temat motywacji dotyczącej tej zmiany, zobacz artykuł [dotyczący zmiany w firmie Microsoft. AspNetCore. App w 3,0](https://github.com/aspnet/Announcements/issues/325) i [pierwsze spojrzenie na zmiany wprowadzone w ASP.NET Core 3,0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->
