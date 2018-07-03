---
title: Otwarty interfejs internetowy dla platformy .NET (OWIN) za pomocą programu ASP.NET Core
author: ardalis
description: Dowiedz się, jak platforma ASP.NET Core obsługuje Open Web Interface for .NET (OWIN), który umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
uid: fundamentals/owin
ms.openlocfilehash: 04042eedc52b4e6f57685e2d9ec1a75cd130fd8d
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144966"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a>Otwarty interfejs internetowy dla platformy .NET (OWIN) za pomocą programu ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Platforma ASP.NET Core obsługuje Open Web Interface for .NET (OWIN). OWIN umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web. Definiuje standardowy mechanizm, oprogramowaniu pośredniczącym, aby używać ich w potoku do obsługi żądań i odpowiedzi skojarzona. Aplikacje platformy ASP.NET Core i oprogramowanie pośredniczące może współdziałać z aplikacji OWIN, serwerów i oprogramowania pośredniczącego.

OWIN zawiera odsprzęgania warstwy, która umożliwia dwie struktury za pomocą modeli obiektów różnych być używane razem. `Microsoft.AspNetCore.Owin` Pakiet zawiera dwie implementacje karty:

* Platforma ASP.NET Core dla OWIN 
* OWIN do platformy ASP.NET Core

Dzięki temu platforma ASP.NET Core będzie hostowana na podstawie OWIN zgodny serwera/host lub inne składniki zgodne OWIN do uruchomienia na podstawie platformy ASP.NET Core.

> [!NOTE]
> Korzystanie z tych kart jest powiązana z negatywnie na wydajność. Nie używaj aplikacji przy użyciu tylko składniki platformy ASP.NET Core `Microsoft.AspNetCore.Owin` pakietu lub kart.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>Działające oprogramowanie pośredniczące OWIN w potoku platformy ASP.NET

Obsługa OWIN platformy ASP.NET Core jest wdrażana jako część `Microsoft.AspNetCore.Owin` pakietu. Po zainstalowaniu tego pakietu, można zaimportować obsługi OWIN do projektu.

Oprogramowanie pośredniczące OWIN, który jest zgodny z [specyfikacji OWIN](http://owin.org/spec/spec/owin-1.0.0.html), co wymaga `Func<IDictionary<string, object>, Task>` można ustawić interfejsu i określonych kluczy (takie jak `owin.ResponseBody`). Poniższy prosty oprogramowania pośredniczącego OWIN Wyświetla "Hello World":

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

Zwraca podpis przykładowe `Task` i akceptuje `IDictionary<string, object>` zgodnie z wymaganiami OWIN.

Poniższy kod przedstawia sposób dodawania `OwinHello` (jak pokazano powyżej) do potoku platformy ASP.NET za pomocą oprogramowania pośredniczącego `UseOwin` — metoda rozszerzenia.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

Można skonfigurować inne akcje do wykonania w ramach potoku OWIN.

> [!NOTE]
> Nagłówki odpowiedzi powinny być modyfikowane tylko przed pierwszej operacji zapisu w strumieniu odpowiedzi.

> [!NOTE]
> Wiele wywołań `UseOwin` jest odradzane ze względu na wydajność. Składniki OWIN będzie działać najlepiej, jeśli zgrupowane.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(async (next) =>
    {
        // do something before
        await OwinHello(new OwinEnvironment(HttpContext));
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Za pomocą hostingu platformy ASP.NET na serwerze OWIN

Na podstawie OWIN serwerów mogło obsługiwać aplikacje platformy ASP.NET. Jeden z serwerów jest [Nowin](https://github.com/Bobris/Nowin), serwer sieci web .NET OWIN. W tym przykładzie w tym artykule I uwzględnione projekt, który odwołuje się Nowin i używa jej do utworzenia `IServer` zdolne do hostingu samodzielnego ASP.NET Core.

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer` to interfejs, który wymaga `Features` właściwości i `Start` metody.

`Start` jest odpowiedzialny za konfigurowanie i uruchamianie na serwerze, który w tym przypadku jest realizowane za pomocą szeregu fluent wywołań interfejsu API, które ustawić adresy przeanalizować za IServerAddressesFeature. Należy pamiętać, że konfiguracja fluent `_builder` zmienna Określa, że żądania będą obsługiwane przez `appFunc` zdefiniowany wcześniej w metodzie. To `Func` nosi nazwę na każde żądanie, aby przetworzyć żądań przychodzących.

Dodamy również `IWebHostBuilder` rozszerzenia, aby ułatwić dodawanie i konfigurowanie serwera Nowin.

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

Dzięki temu w miejscu wywołania rozszerzenie w *Program.cs* do uruchamiania aplikacji ASP.NET Core za pomocą tego niestandardowego serwera:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}
```

Dowiedz się więcej o programie ASP.NET [serwerów](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Uruchamianie platformy ASP.NET Core na serwerze OWIN i użyj jego obsługa funkcji WebSockets

Innym przykładem funkcje jak OWIN na podstawie serwerów mogą zostać wykorzystane przez ASP.NET Core to dostęp do funkcji, takich jak funkcja WebSockets. Serwer sieci web .NET OWIN, które są używane w poprzednim przykładzie zapewnia obsługę wbudowanych, gniazda sieci Web, które mogą zostać wykorzystane przez aplikację ASP.NET Core. W poniższym przykładzie pokazano prostą aplikację internetową, która obsługuje gniazda sieci Web a funkcją ponownie wszystkie wiadomości wysyłane na serwer za pomocą funkcji WebSockets.

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

To [przykładowe](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) jest skonfigurowany, korzystając z tych samych `NowinServer` jak poprzedni — jedyna różnica polega na w jaki sposób aplikacja jest skonfigurowana w jego `Configure` metody. Test za pomocą [klienta prostego protokołu websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) pokazuje aplikacji:

![Klient testowy gniazda sieci Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Środowisko OWIN

Można skonstruować środowisko OWIN za pomocą `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Klucze OWIN

Zależy od OWIN `IDictionary<string,object>` obiektu do przekazywania informacji w całej wymiany żądania/odpowiedzi HTTP. Platforma ASP.NET Core implementuje klucze wymienione poniżej. Zobacz [podstawowej specyfikacji, rozszerzeń](http://owin.org/#spec), i [wytyczne klucz OWIN i klucze wspólne](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Dane żądania (wersja 1.0.0 OWIN)

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin. RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin. RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Dane żądania (OWIN v1.1.0)

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | Optional |

### <a name="response-data-owin-v100"></a>Dane odpowiedzi (OWIN 1.0.0)

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | Optional |
| owin.ResponseReasonPhrase | `String` | Optional |
| owin.ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin. ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Inne dane (wersja 1.0.0 OWIN)

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.CallCancelled | `CancellationToken` |  |
| owin. Wersja  | `String` | |   


### <a name="common-keys"></a>Klucze wspólne

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| serwer. IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Na żądanie |


### <a name="opaque-v030"></a>V0.3.0 nieprzezroczyste

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| nieprzezroczysta. Uaktualnienie | `OpaqueUpgrade` | Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| opaque.CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| protokołu websocket. Wersja | `String` |  |
| websocket.Accept | `WebSocketAccept` | Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | Non-spec |
| websocket.SubProtocol | `String` | Zobacz [RFC6455 sekcja 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) kroku 5.5 |
| websocket.SendAsync | `WebSocketSendAsync` | Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | Optional |
| websocket.ClientCloseDescription | `String` | Optional |

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Serwery](xref:fundamentals/servers/index)
