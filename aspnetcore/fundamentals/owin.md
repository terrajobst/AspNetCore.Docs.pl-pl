---
title: "Otwórz interfejs sieci Web dla platformy .NET (OWIN)"
author: ardalis
description: "Odkryj, jak platformy ASP.NET Core obsługuje interfejsu Open Web dla platformy .NET (OWIN), który umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/owin
ms.openlocfilehash: 1a6a49715840d66dc37465758d3a896af96e2976
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>Wprowadzenie do otworzyć Interfejs sieci Web dla platformy .NET (OWIN)

Przez [Steve Smith](https://ardalis.com/) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Platformy ASP.NET Core obsługuje interfejsu Open Web dla platformy .NET (OWIN). OWIN umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web. Definiuje standardowy sposób dla oprogramowania pośredniczącego do użycia w potoku do obsługi żądań i odpowiedzi skojarzona. Aplikacje platformy ASP.NET Core i oprogramowanie pośredniczące może współdziałać z aplikacji OWIN, serwerów i oprogramowania pośredniczącego.

OWIN zawiera odsprzęgania warstwy, która umożliwia dwóch platform z modeli obiektów różnych być używane razem. `Microsoft.AspNetCore.Owin` Pakiet zawiera dwie karty implementacji:
- Platformy ASP.NET Core dla OWIN 
- OWIN do platformy ASP.NET Core

Dzięki temu platformy ASP.NET Core ma być obsługiwana na górze OWIN zgodne/hosta serwera lub dla innych składników zgodne OWIN do uruchomienia na górze platformy ASP.NET Core.

Uwaga: Korzystanie z tych kart jest dostarczany z na wydajność. Aplikacji przy użyciu tylko składniki platformy ASP.NET Core nie należy używać pakietu Owin lub kart.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>Uruchamianie OWIN oprogramowanie pośredniczące w potoku platformy ASP.NET

Obsługa OWIN platformy ASP.NET Core jest wdrażana jako część `Microsoft.AspNetCore.Owin` pakietu. Obsługa OWIN można zaimportować do projektu, instalując tego pakietu.

Oprogramowanie pośredniczące OWIN odpowiada [specyfikacji OWIN](http://owin.org/spec/spec/owin-1.0.0.html), co wymaga `Func<IDictionary<string, object>, Task>` można ustawić interfejsu i określonych kluczy (takie jak `owin.ResponseBody`). Następujące oprogramowanie pośredniczące OWIN proste Wyświetla "Hello World":

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

Zwraca podpis próbki `Task` i akceptuje `IDictionary<string, object>` zgodnie z żądaniem OWIN.

Poniższy kod przedstawia sposób dodawania `OwinHello` (pokazanym powyżej) do potoku platformy ASP.NET z oprogramowania pośredniczącego `UseOwin` — metoda rozszerzenia.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

Można skonfigurować inne akcje w ramach potoku OWIN.

> [!NOTE]
> Nagłówki odpowiedzi można modyfikować tylko przed pierwszym zapisie w strumieniu odpowiedzi.

> [!NOTE]
> Wiele wywołań `UseOwin` jest niezalecane ze względu na wydajność. Składniki OWIN będzie działać najlepiej, jeśli zgrupowane razem.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Przy użyciu hostingu ASP.NET na serwerze OWIN

Serwery oparte na OWIN mogą obsługiwać aplikacji ASP.NET. Jeden taki serwer jest [Nowin](https://github.com/Bobris/Nowin), serwer sieci web .NET OWIN. W przykładowym tego artykułu, I włączone na projekt, który odwołuje się do Nowin i używa jej do utworzenia `IServer` który może obsługiwać własny platformy ASP.NET Core.

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`to interfejs, który wymaga `Features` właściwości i `Start` metody.

`Start`jest odpowiedzialny za konfigurowanie i uruchamianie serwera, który w tym przypadku odbywa się za pośrednictwem szereg wywołania interfejsu API fluent Ustaw adresy przeanalizować z IServerAddressesFeature. Należy pamiętać, że konfiguracja fluent `_builder` zmienna Określa, że żądania będą obsługiwane przez `appFunc` wcześniej w metodzie. To `Func` jest wywoływane dla każdego żądania przetwarzania przychodzących żądań.

Również dodamy `IWebHostBuilder` rozszerzenia, aby ułatwić dodawanie i konfigurowanie serwera Nowin.

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

Z tym w miejscu ma wszystkie wymagane do uruchamiania aplikacji ASP.NET przy użyciu tego niestandardowego serwera do wywoływania rozszerzenia w *Program.cs*:

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

Dowiedz się więcej o ASP.NET [serwerów](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Platformy ASP.NET Core na serwerze standardem OWIN i użyć jego obsługę protokołu WebSockets

Innym przykładem funkcje jak OWIN serwerów opartych na mogą zostać wykorzystane przez platformy ASP.NET Core jest dostęp do funkcji, takich jak protokół WebSockets. Serwer sieci web .NET OWIN użyty w poprzednim przykładzie obsługuje gniazda sieci Web wbudowane, które mogą zostać wykorzystane przez aplikację ASP.NET Core. W poniższym przykładzie pokazano aplikacji sieci web proste, który obsługuje gniazda sieci Web i zwraca ponownie wszystkie wysłane do serwera przy użyciu protokołu WebSockets.

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

To [próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) skonfigurowano korzystającej z tego samego `NowinServer` jak poprzedni — jedyna różnica polega na w jaki sposób aplikacja jest skonfigurowana w jego `Configure` metody. Test za pomocą [klienta prostego protokołu websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) pokazuje aplikacji:

![Klient testowy gniazda sieci Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Środowisko OWIN

Można utworzyć środowiska OWIN za pomocą `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Klucze OWIN

Zależy od OWIN `IDictionary<string,object>` obiektu do przekazywania informacji w całym wymiany żądania/odpowiedzi HTTP. Platformy ASP.NET Core implementuje klucze wymienione poniżej. Zobacz [specyfikacji podstawowego, rozszerzenia](http://owin.org/#spec), i [wskazówki klucz OWIN i klucze wspólne](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Dane żądania (OWIN v1.0.0)

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin.RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin.RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Dane żądania (OWIN v1.1.0)

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | Optional |

### <a name="response-data-owin-v100"></a>Dane odpowiedzi (OWIN v1.0.0)

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | Optional |
| owin.ResponseReasonPhrase | `String` | Optional |
| owin.ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin.ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Inne dane (OWIN v1.0.0)

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.CallCancelled | `CancellationToken` |  |
| owin.Version  | `String` | |   


### <a name="common-keys"></a>Klucze wspólne

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| server.IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Na żądanie |


### <a name="opaque-v030"></a>V0.3.0 nieprzezroczyste

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| opaque.Upgrade | `OpaqueUpgrade` | Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| opaque.CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| Key               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| Protokół websocket. Wersja | `String` |  |
| websocket.Accept | `WebSocketAccept` | Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | Non-spec |
| websocket.SubProtocol | `String` | Zobacz [RFC6455 sekcja 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) krok 5,5 |
| websocket.SendAsync | `WebSocketSendAsync` | Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | Optional |
| websocket.ClientCloseDescription | `String` | Optional |

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Serwery](xref:fundamentals/servers/index)
