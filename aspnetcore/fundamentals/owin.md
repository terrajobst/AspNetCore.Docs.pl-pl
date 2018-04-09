---
title: Otwórz interfejs sieci Web dla platformy .NET (OWIN) z platformy ASP.NET Core
author: ardalis
description: Odkryj, jak platformy ASP.NET Core obsługuje interfejsu Open Web dla platformy .NET (OWIN), który umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/owin
ms.openlocfilehash: 7c379ddbcc1f66ba9f7a4d0a50a4607394fed0af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="e51d2-103">Otwórz interfejs sieci Web dla platformy .NET (OWIN) z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e51d2-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="e51d2-104">Przez [Steve Smith](https://ardalis.com/) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e51d2-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e51d2-105">Platformy ASP.NET Core obsługuje interfejsu Open Web dla platformy .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="e51d2-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="e51d2-106">OWIN umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.</span><span class="sxs-lookup"><span data-stu-id="e51d2-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="e51d2-107">Definiuje standardowy sposób dla oprogramowania pośredniczącego do użycia w potoku do obsługi żądań i odpowiedzi skojarzona.</span><span class="sxs-lookup"><span data-stu-id="e51d2-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="e51d2-108">Aplikacje platformy ASP.NET Core i oprogramowanie pośredniczące może współdziałać z aplikacji OWIN, serwerów i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="e51d2-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="e51d2-109">OWIN zawiera odsprzęgania warstwy, która umożliwia dwóch platform z modeli obiektów różnych być używane razem.</span><span class="sxs-lookup"><span data-stu-id="e51d2-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="e51d2-110">`Microsoft.AspNetCore.Owin` Pakiet zawiera dwie karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="e51d2-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="e51d2-111">Platformy ASP.NET Core dla OWIN</span><span class="sxs-lookup"><span data-stu-id="e51d2-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="e51d2-112">OWIN do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e51d2-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="e51d2-113">Dzięki temu platformy ASP.NET Core ma być obsługiwana na górze OWIN zgodne/hosta serwera lub dla innych składników zgodne OWIN do uruchomienia na górze platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e51d2-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="e51d2-114">Uwaga: Korzystanie z tych kart jest dostarczany z na wydajność.</span><span class="sxs-lookup"><span data-stu-id="e51d2-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="e51d2-115">Aplikacji przy użyciu tylko składniki platformy ASP.NET Core nie należy używać pakietu Owin lub kart.</span><span class="sxs-lookup"><span data-stu-id="e51d2-115">Applications using only ASP.NET Core components shouldn't use the Owin package or adapters.</span></span>

<span data-ttu-id="e51d2-116">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e51d2-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="e51d2-117">Uruchamianie OWIN oprogramowanie pośredniczące w potoku platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e51d2-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="e51d2-118">Obsługa OWIN platformy ASP.NET Core jest wdrażana jako część `Microsoft.AspNetCore.Owin` pakietu.</span><span class="sxs-lookup"><span data-stu-id="e51d2-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="e51d2-119">Obsługa OWIN można zaimportować do projektu, instalując tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="e51d2-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="e51d2-120">Oprogramowanie pośredniczące OWIN odpowiada [specyfikacji OWIN](http://owin.org/spec/spec/owin-1.0.0.html), co wymaga `Func<IDictionary<string, object>, Task>` można ustawić interfejsu i określonych kluczy (takie jak `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="e51d2-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="e51d2-121">Następujące oprogramowanie pośredniczące OWIN proste Wyświetla "Hello World":</span><span class="sxs-lookup"><span data-stu-id="e51d2-121">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="e51d2-122">Zwraca podpis próbki `Task` i akceptuje `IDictionary<string, object>` zgodnie z żądaniem OWIN.</span><span class="sxs-lookup"><span data-stu-id="e51d2-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="e51d2-123">Poniższy kod przedstawia sposób dodawania `OwinHello` (pokazanym powyżej) do potoku platformy ASP.NET z oprogramowania pośredniczącego `UseOwin` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="e51d2-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="e51d2-124">Można skonfigurować inne akcje w ramach potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="e51d2-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="e51d2-125">Nagłówki odpowiedzi można modyfikować tylko przed pierwszym zapisie w strumieniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e51d2-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="e51d2-126">Wiele wywołań `UseOwin` jest niezalecane ze względu na wydajność.</span><span class="sxs-lookup"><span data-stu-id="e51d2-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="e51d2-127">Składniki OWIN będzie działać najlepiej, jeśli zgrupowane razem.</span><span class="sxs-lookup"><span data-stu-id="e51d2-127">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="e51d2-128">Przy użyciu hostingu ASP.NET na serwerze OWIN</span><span class="sxs-lookup"><span data-stu-id="e51d2-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="e51d2-129">Serwery oparte na OWIN mogą obsługiwać aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e51d2-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="e51d2-130">Jeden taki serwer jest [Nowin](https://github.com/Bobris/Nowin), serwer sieci web .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="e51d2-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="e51d2-131">W przykładowym tego artykułu, I włączone na projekt, który odwołuje się do Nowin i używa jej do utworzenia `IServer` który może obsługiwać własny platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e51d2-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="e51d2-132">`IServer` to interfejs, który wymaga `Features` właściwości i `Start` metody.</span><span class="sxs-lookup"><span data-stu-id="e51d2-132">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="e51d2-133">`Start` jest odpowiedzialny za konfigurowanie i uruchamianie serwera, który w tym przypadku odbywa się za pośrednictwem szereg wywołania interfejsu API fluent Ustaw adresy przeanalizować z IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="e51d2-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="e51d2-134">Należy pamiętać, że konfiguracja fluent `_builder` zmienna Określa, że żądania będą obsługiwane przez `appFunc` wcześniej w metodzie.</span><span class="sxs-lookup"><span data-stu-id="e51d2-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="e51d2-135">To `Func` jest wywoływane dla każdego żądania przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="e51d2-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="e51d2-136">Również dodamy `IWebHostBuilder` rozszerzenia, aby ułatwić dodawanie i konfigurowanie serwera Nowin.</span><span class="sxs-lookup"><span data-stu-id="e51d2-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="e51d2-137">Z tym w miejscu ma wszystkie wymagane do uruchamiania aplikacji ASP.NET przy użyciu tego niestandardowego serwera do wywoływania rozszerzenia w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e51d2-137">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

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

<span data-ttu-id="e51d2-138">Dowiedz się więcej o ASP.NET [serwerów](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e51d2-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="e51d2-139">Platformy ASP.NET Core na serwerze standardem OWIN i użyć jego obsługę protokołu WebSockets</span><span class="sxs-lookup"><span data-stu-id="e51d2-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="e51d2-140">Innym przykładem funkcje jak OWIN serwerów opartych na mogą zostać wykorzystane przez platformy ASP.NET Core jest dostęp do funkcji, takich jak protokół WebSockets.</span><span class="sxs-lookup"><span data-stu-id="e51d2-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="e51d2-141">Serwer sieci web .NET OWIN użyty w poprzednim przykładzie obsługuje gniazda sieci Web wbudowane, które mogą zostać wykorzystane przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e51d2-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="e51d2-142">W poniższym przykładzie pokazano aplikacji sieci web proste, który obsługuje gniazda sieci Web i zwraca ponownie wszystkie wysłane do serwera przy użyciu protokołu WebSockets.</span><span class="sxs-lookup"><span data-stu-id="e51d2-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="e51d2-143">To [próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) skonfigurowano korzystającej z tego samego `NowinServer` jak poprzedni — jedyna różnica polega na w jaki sposób aplikacja jest skonfigurowana w jego `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="e51d2-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="e51d2-144">Test za pomocą [klienta prostego protokołu websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) pokazuje aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e51d2-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Klient testowy gniazda sieci Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="e51d2-146">Środowisko OWIN</span><span class="sxs-lookup"><span data-stu-id="e51d2-146">OWIN environment</span></span>

<span data-ttu-id="e51d2-147">Można utworzyć środowiska OWIN za pomocą `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="e51d2-147">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="e51d2-148">Klucze OWIN</span><span class="sxs-lookup"><span data-stu-id="e51d2-148">OWIN keys</span></span>

<span data-ttu-id="e51d2-149">Zależy od OWIN `IDictionary<string,object>` obiektu do przekazywania informacji w całym wymiany żądania/odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e51d2-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="e51d2-150">Platformy ASP.NET Core implementuje klucze wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="e51d2-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="e51d2-151">Zobacz [specyfikacji podstawowego, rozszerzenia](http://owin.org/#spec), i [wskazówki klucz OWIN i klucze wspólne](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="e51d2-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="e51d2-152">Dane żądania (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="e51d2-152">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="e51d2-153">Key</span><span class="sxs-lookup"><span data-stu-id="e51d2-153">Key</span></span>               | <span data-ttu-id="e51d2-154">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="e51d2-154">Value (type)</span></span> | <span data-ttu-id="e51d2-155">Opis</span><span class="sxs-lookup"><span data-stu-id="e51d2-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e51d2-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="e51d2-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="e51d2-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="e51d2-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="e51d2-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="e51d2-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="e51d2-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="e51d2-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="e51d2-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="e51d2-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="e51d2-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="e51d2-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="e51d2-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="e51d2-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="e51d2-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="e51d2-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="e51d2-164">Dane żądania (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="e51d2-164">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="e51d2-165">Key</span><span class="sxs-lookup"><span data-stu-id="e51d2-165">Key</span></span>               | <span data-ttu-id="e51d2-166">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="e51d2-166">Value (type)</span></span> | <span data-ttu-id="e51d2-167">Opis</span><span class="sxs-lookup"><span data-stu-id="e51d2-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e51d2-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="e51d2-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="e51d2-169">Optional</span><span class="sxs-lookup"><span data-stu-id="e51d2-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="e51d2-170">Dane odpowiedzi (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="e51d2-170">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="e51d2-171">Key</span><span class="sxs-lookup"><span data-stu-id="e51d2-171">Key</span></span>               | <span data-ttu-id="e51d2-172">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="e51d2-172">Value (type)</span></span> | <span data-ttu-id="e51d2-173">Opis</span><span class="sxs-lookup"><span data-stu-id="e51d2-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e51d2-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="e51d2-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="e51d2-175">Optional</span><span class="sxs-lookup"><span data-stu-id="e51d2-175">Optional</span></span> |
| <span data-ttu-id="e51d2-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="e51d2-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="e51d2-177">Optional</span><span class="sxs-lookup"><span data-stu-id="e51d2-177">Optional</span></span> |
| <span data-ttu-id="e51d2-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="e51d2-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="e51d2-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="e51d2-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="e51d2-180">Inne dane (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="e51d2-180">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="e51d2-181">Key</span><span class="sxs-lookup"><span data-stu-id="e51d2-181">Key</span></span>               | <span data-ttu-id="e51d2-182">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="e51d2-182">Value (type)</span></span> | <span data-ttu-id="e51d2-183">Opis</span><span class="sxs-lookup"><span data-stu-id="e51d2-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e51d2-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="e51d2-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="e51d2-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="e51d2-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="e51d2-186">Klucze wspólne</span><span class="sxs-lookup"><span data-stu-id="e51d2-186">Common keys</span></span>

| <span data-ttu-id="e51d2-187">Key</span><span class="sxs-lookup"><span data-stu-id="e51d2-187">Key</span></span>               | <span data-ttu-id="e51d2-188">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="e51d2-188">Value (type)</span></span> | <span data-ttu-id="e51d2-189">Opis</span><span class="sxs-lookup"><span data-stu-id="e51d2-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e51d2-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="e51d2-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="e51d2-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="e51d2-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="e51d2-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="e51d2-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="e51d2-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="e51d2-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="e51d2-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="e51d2-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="e51d2-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="e51d2-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="e51d2-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="e51d2-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="e51d2-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="e51d2-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="e51d2-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="e51d2-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="e51d2-199">Key</span><span class="sxs-lookup"><span data-stu-id="e51d2-199">Key</span></span>               | <span data-ttu-id="e51d2-200">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="e51d2-200">Value (type)</span></span> | <span data-ttu-id="e51d2-201">Opis</span><span class="sxs-lookup"><span data-stu-id="e51d2-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e51d2-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="e51d2-202">sendfile.SendAsync</span></span> | <span data-ttu-id="e51d2-203">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e51d2-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="e51d2-204">Na żądanie</span><span class="sxs-lookup"><span data-stu-id="e51d2-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="e51d2-205">V0.3.0 nieprzezroczyste</span><span class="sxs-lookup"><span data-stu-id="e51d2-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="e51d2-206">Key</span><span class="sxs-lookup"><span data-stu-id="e51d2-206">Key</span></span>               | <span data-ttu-id="e51d2-207">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="e51d2-207">Value (type)</span></span> | <span data-ttu-id="e51d2-208">Opis</span><span class="sxs-lookup"><span data-stu-id="e51d2-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e51d2-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="e51d2-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="e51d2-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="e51d2-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="e51d2-211">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e51d2-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="e51d2-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="e51d2-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="e51d2-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="e51d2-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="e51d2-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="e51d2-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="e51d2-215">Key</span><span class="sxs-lookup"><span data-stu-id="e51d2-215">Key</span></span>               | <span data-ttu-id="e51d2-216">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="e51d2-216">Value (type)</span></span> | <span data-ttu-id="e51d2-217">Opis</span><span class="sxs-lookup"><span data-stu-id="e51d2-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e51d2-218">Protokół websocket. Wersja</span><span class="sxs-lookup"><span data-stu-id="e51d2-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="e51d2-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="e51d2-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="e51d2-220">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e51d2-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="e51d2-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="e51d2-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="e51d2-222">Non-spec</span><span class="sxs-lookup"><span data-stu-id="e51d2-222">Non-spec</span></span> |
| <span data-ttu-id="e51d2-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="e51d2-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="e51d2-224">Zobacz [RFC6455 sekcja 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) krok 5,5</span><span class="sxs-lookup"><span data-stu-id="e51d2-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="e51d2-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="e51d2-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="e51d2-226">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e51d2-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="e51d2-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="e51d2-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="e51d2-228">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e51d2-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="e51d2-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="e51d2-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="e51d2-230">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e51d2-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="e51d2-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="e51d2-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="e51d2-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="e51d2-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="e51d2-233">Optional</span><span class="sxs-lookup"><span data-stu-id="e51d2-233">Optional</span></span> |
| <span data-ttu-id="e51d2-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="e51d2-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="e51d2-235">Optional</span><span class="sxs-lookup"><span data-stu-id="e51d2-235">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="e51d2-236">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e51d2-236">Additional resources</span></span>

* [<span data-ttu-id="e51d2-237">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="e51d2-237">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="e51d2-238">Serwery</span><span class="sxs-lookup"><span data-stu-id="e51d2-238">Servers</span></span>](xref:fundamentals/servers/index)
