---
title: Otwórz interfejs sieci Web dla platformy .NET (OWIN) z ASP.NET Core
author: ardalis
description: Dowiedz się, w jaki sposób ASP.NET Core obsługuje otwarty interfejs sieci Web dla platformy .NET (OWIN), dzięki czemu aplikacje sieci Web mają być oddzielone od serwerów sieci Web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/18/2018
uid: fundamentals/owin
ms.openlocfilehash: 14b23ba6d284413e20417bbd4142e19a656350ac
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666685"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a>Otwórz interfejs sieci Web dla platformy .NET (OWIN) z ASP.NET Core

[Steve Kowalski](https://ardalis.com/) i [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core obsługuje otwarty interfejs sieci Web dla platformy .NET (OWIN). OWIN umożliwia oddzielenie aplikacji sieci Web od serwerów sieci Web. Definiuje standardowy sposób użycia oprogramowania pośredniczącego w potoku do obsługi żądań i skojarzonych odpowiedzi. ASP.NET Core aplikacje i oprogramowanie pośredniczące mogą współdziałać z aplikacjami opartymi na OWIN, serwerami i oprogramowania pośredniczącego.

OWIN zapewnia warstwę odsprzęgającą, która umożliwia wspólne używanie dwóch platform z różnymi modelami obiektów. Pakiet `Microsoft.AspNetCore.Owin` udostępnia dwie implementacje kart:

* ASP.NET Core OWIN 
* OWIN do ASP.NET Core

Dzięki temu ASP.NET Core mogą być hostowane na serwerze/hoście zgodnym z OWIN lub dla innych składników zgodnych OWIN, które mają być uruchamiane w oparciu o ASP.NET Core.

> [!NOTE]
> Korzystanie z tych kart wiąże się z kosztem wydajności. Aplikacje używające tylko składników ASP.NET Core nie powinny używać pakietu `Microsoft.AspNetCore.Owin` lub kart.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a>Uruchamianie oprogramowania pośredniczącego OWIN w potoku ASP.NET Core

Obsługa OWIN ASP.NET Core jest wdrażana w ramach pakietu `Microsoft.AspNetCore.Owin`. Możesz zaimportować obsługę OWIN do projektu, instalując ten pakiet.

Oprogramowanie pośredniczące OWIN jest zgodne ze [specyfikacją Owin](https://owin.org/spec/spec/owin-1.0.0.html), która wymaga interfejsu `Func<IDictionary<string, object>, Task>`, i należy ustawić określone klucze (takie jak `owin.ResponseBody`). Następujące proste oprogramowanie pośredniczące OWIN wyświetla "Hello world":

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: https://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

Przykładowa sygnatura zwraca `Task` i akceptuje `IDictionary<string, object>` zgodnie z wymaganiami OWIN.

Poniższy kod przedstawia sposób dodawania `OwinHello` oprogramowania pośredniczącego (pokazanych powyżej) do potoku ASP.NET Core za pomocą metody rozszerzenia `UseOwin`.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

Można skonfigurować inne akcje, które mają być wykonywane w potoku OWIN.

> [!NOTE]
> Nagłówki odpowiedzi należy modyfikować tylko przed pierwszym zapisem w strumieniu odpowiedzi.

> [!NOTE]
> Wiele wywołań `UseOwin` nie jest zalecane ze względu na wydajność. Składniki OWIN działają najlepiej, jeśli są zgrupowane razem.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        return async environment =>
        {
            // Do something before.
            await next(environment);
            // Do something after.
        };
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-core-hosting-on-an-owin-based-server"></a>Korzystanie z ASP.NET Core hosting na serwerze z systemem OWIN

Serwery z systemem OWIN mogą hostować aplikacje ASP.NET Core. Jeden taki serwer to [nowin](https://github.com/Bobris/Nowin), serwer sieci Web programu .NET Owin. W przykładzie w tym artykule dodaliśmy projekt, który odwołuje się do nowin i używa go do tworzenia `IServer`, które umożliwiają samodzielne ASP.NET Core.

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer` to interfejs, który wymaga właściwości `Features` i metody `Start`.

`Start` jest odpowiedzialna za konfigurowanie i uruchamianie serwera, co w tym przypadku jest realizowane za pośrednictwem szeregu wywołań interfejsu API Fluent, które ustawiają adresy przeanalizowane z IServerAddressesFeature. Należy zauważyć, że konfiguracja Fluent zmiennej `_builder` określa, że żądania będą obsługiwane przez `appFunc` zdefiniowane wcześniej w metodzie. Ta `Func` jest wywoływana dla każdego żądania w celu przetworzenia żądań przychodzących.

Dodamy również rozszerzenie `IWebHostBuilder`, aby ułatwić Dodawanie i Konfigurowanie serwera nowin.

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

W tym miejscu Wywołaj rozszerzenie *program.cs* , aby uruchomić aplikację ASP.NET Core przy użyciu tego serwera niestandardowego:

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

Dowiedz się więcej o [serwerach ASP.NET Core](xref:fundamentals/servers/index).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Uruchom ASP.NET Core na serwerze z systemem OWIN i Użyj obsługi jego gniazd WebSockets

Innym przykładem sposobu korzystania z funkcji serwerów opartych na OWIN można użyć ASP.NET Core jest dostęp do funkcji, takich jak WebSockets. Serwer sieci Web programu .NET OWIN używany w poprzednim przykładzie obsługuje wbudowane w sieć Web Sockets, które mogą być używane przez aplikację ASP.NET Core. W poniższym przykładzie przedstawiono prostą aplikację sieci Web, która obsługuje gniazda sieci Web i zwraca wszystkie dane wysyłane do serwera za pomocą elementów WebSockets.

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

Ten [przykład](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) jest konfigurowany przy użyciu tego samego `NowinServer` co poprzedni — jedyną różnicą jest to, w jaki sposób aplikacja jest skonfigurowana w ramach metody `Configure`. Test korzystający z [prostego klienta protokołu WebSocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) pokazuje aplikację:

![Klient testowy gniazda sieci Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Środowisko OWIN.

Środowisko OWIN można utworzyć przy użyciu `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Klucze OWIN

OWIN zależy od obiektu `IDictionary<string,object>`, aby komunikować się z informacjami w trakcie wymiany żądania/odpowiedzi HTTP. ASP.NET Core implementuje klucze wymienione poniżej. Zobacz [podstawowe specyfikacje, rozszerzenia](https://owin.org/#spec)i [wskazówki dotyczące klucza Owin oraz wspólne klucze](https://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Dane żądania (OWIN v 1.0.0)

| Klucz               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin.RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin.RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Dane żądania (OWIN v 1.1.0)

| Klucz               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | Optional (Opcjonalność) |

### <a name="response-data-owin-v100"></a>Dane odpowiedzi (OWIN v 1.0.0)

| Klucz               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | Optional (Opcjonalność) |
| owin.ResponseReasonPhrase | `String` | Optional (Opcjonalność) |
| owin.ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin.ResponseBody | `Stream`  | |

### <a name="other-data-owin-v100"></a>Inne dane (OWIN v 1.0.0)

| Klucz               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| owin.CallCancelled | `CancellationToken` |  |
| Owin. Wersja  | `String` | |   

### <a name="common-keys"></a>Klucze wspólne

| Klucz               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| Server. IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |

### <a name="sendfiles-v030"></a>SendFiles v 0.3.0

| Klucz               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | Zobacz [podpis delegata](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Na żądanie |

### <a name="opaque-v030"></a>Nieprzezroczysty 0.3.0 v

| Klucz               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| zakrywa. Uaktualniony | `OpaqueUpgrade` | Zobacz [podpis delegata](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| opaque.CallCancelled | `CancellationToken` |  |

### <a name="websocket-v030"></a>WebSocket v0.3.0

| Klucz               | Wartość (typ) | Opis |
| ----------------- | ------------ | ----------- |
| akceptowan. Wersja | `String` |  |
| websocket.Accept | `WebSocketAccept` | Zobacz [podpis delegata](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | Nie-spec |
| websocket.SubProtocol | `String` | Zobacz [RFC6455 sekcja 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) krok 5,5 |
| websocket.SendAsync | `WebSocketSendAsync` | Zobacz [podpis delegata](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Zobacz [podpis delegata](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Zobacz [podpis delegata](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | Optional (Opcjonalność) |
| websocket.ClientCloseDescription | `String` | Optional (Opcjonalność) |

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Serwery](xref:fundamentals/servers/index)
