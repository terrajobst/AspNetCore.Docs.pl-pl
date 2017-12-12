---
title: "Otwórz interfejs sieci Web dla platformy .NET (OWIN)"
author: ardalis
description: "Odkryj, jak platformy ASP.NET Core obsługuje interfejsu Open Web dla platformy .NET (OWIN), który umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web."
keywords: Platformy ASP.NET Core interfejsu Open Web dla platformy .NET, OWIN
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2ee970a1c9cd05ebee76b92c3e2c7c6c6cc6ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="1f8fa-104">Wprowadzenie do otworzyć Interfejs sieci Web dla platformy .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="1f8fa-105">Przez [Steve Smith](https://ardalis.com/) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f8fa-106">Platformy ASP.NET Core obsługuje interfejsu Open Web dla platformy .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="1f8fa-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="1f8fa-107">OWIN umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="1f8fa-108">Definiuje standardowy sposób dla oprogramowania pośredniczącego do użycia w potoku do obsługi żądań i odpowiedzi skojarzona.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="1f8fa-109">Aplikacje platformy ASP.NET Core i oprogramowanie pośredniczące może współdziałać z aplikacji OWIN, serwerów i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="1f8fa-110">OWIN zawiera odsprzęgania warstwy, która umożliwia dwóch platform z modeli obiektów różnych być używane razem.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="1f8fa-111">`Microsoft.AspNetCore.Owin` Pakiet zawiera dwie karty implementacji:</span><span class="sxs-lookup"><span data-stu-id="1f8fa-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="1f8fa-112">Platformy ASP.NET Core dla OWIN</span><span class="sxs-lookup"><span data-stu-id="1f8fa-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="1f8fa-113">OWIN do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f8fa-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="1f8fa-114">Dzięki temu platformy ASP.NET Core ma być obsługiwana na górze OWIN zgodne/hosta serwera lub dla innych składników zgodne OWIN do uruchomienia na górze platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="1f8fa-115">Uwaga: Korzystanie z tych kart jest dostarczany z na wydajność.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="1f8fa-116">Aplikacje przy użyciu tylko składniki platformy ASP.NET Core nie należy używać pakietu Owin lub kart.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="1f8fa-117">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1f8fa-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="1f8fa-118">Uruchamianie OWIN oprogramowanie pośredniczące w potoku platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1f8fa-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="1f8fa-119">Obsługa OWIN platformy ASP.NET Core jest wdrażana jako część `Microsoft.AspNetCore.Owin` pakietu.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="1f8fa-120">Obsługa OWIN można zaimportować do projektu, instalując tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="1f8fa-121">Oprogramowanie pośredniczące OWIN odpowiada [specyfikacji OWIN](http://owin.org/spec/spec/owin-1.0.0.html), co wymaga `Func<IDictionary<string, object>, Task>` można ustawić interfejsu i określonych kluczy (takie jak `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="1f8fa-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="1f8fa-122">Następujące oprogramowanie pośredniczące OWIN proste Wyświetla "Hello World":</span><span class="sxs-lookup"><span data-stu-id="1f8fa-122">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="1f8fa-123">Zwraca podpis próbki `Task` i akceptuje `IDictionary<string, object>` zgodnie z żądaniem OWIN.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="1f8fa-124">Poniższy kod przedstawia sposób dodawania `OwinHello` (pokazanym powyżej) do potoku platformy ASP.NET z oprogramowania pośredniczącego `UseOwin` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="1f8fa-125">Można skonfigurować inne akcje w ramach potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="1f8fa-126">Nagłówki odpowiedzi można modyfikować tylko przed pierwszym zapisie w strumieniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="1f8fa-127">Wiele wywołań `UseOwin` jest niezalecane ze względu na wydajność.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="1f8fa-128">Składniki OWIN będzie działać najlepiej, jeśli zgrupowane razem.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-128">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="1f8fa-129">Przy użyciu hostingu ASP.NET na serwerze OWIN</span><span class="sxs-lookup"><span data-stu-id="1f8fa-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="1f8fa-130">Serwery oparte na OWIN mogą obsługiwać aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="1f8fa-131">Jeden taki serwer jest [Nowin](https://github.com/Bobris/Nowin), serwer sieci web .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="1f8fa-132">W przykładowym tego artykułu, I włączone na projekt, który odwołuje się do Nowin i używa jej do utworzenia `IServer` który może obsługiwać własny platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="1f8fa-133">`IServer`to interfejs, który wymaga `Features` właściwości i `Start` metody.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-133">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="1f8fa-134">`Start`jest odpowiedzialny za konfigurowanie i uruchamianie serwera, który w tym przypadku odbywa się za pośrednictwem szereg wywołania interfejsu API fluent Ustaw adresy przeanalizować z IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-134">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="1f8fa-135">Należy pamiętać, że konfiguracja fluent `_builder` zmienna Określa, że żądania będą obsługiwane przez `appFunc` wcześniej w metodzie.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-135">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="1f8fa-136">To `Func` jest wywoływane dla każdego żądania przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-136">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="1f8fa-137">Również dodamy `IWebHostBuilder` rozszerzenia, aby ułatwić dodawanie i konfigurowanie serwera Nowin.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-137">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="1f8fa-138">Z tym w miejscu ma wszystkie wymagane do uruchamiania aplikacji ASP.NET przy użyciu tego niestandardowego serwera do wywoływania rozszerzenia w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1f8fa-138">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

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

<span data-ttu-id="1f8fa-139">Dowiedz się więcej o ASP.NET [serwerów](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1f8fa-139">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="1f8fa-140">Platformy ASP.NET Core na serwerze standardem OWIN i użyć jego obsługę protokołu WebSockets</span><span class="sxs-lookup"><span data-stu-id="1f8fa-140">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="1f8fa-141">Innym przykładem funkcje jak OWIN serwerów opartych na mogą zostać wykorzystane przez platformy ASP.NET Core jest dostęp do funkcji, takich jak protokół WebSockets.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-141">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="1f8fa-142">Serwer sieci web .NET OWIN użyty w poprzednim przykładzie obsługuje gniazda sieci Web wbudowane, które mogą zostać wykorzystane przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-142">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="1f8fa-143">W poniższym przykładzie pokazano aplikacji sieci web proste, który obsługuje gniazda sieci Web i zwraca ponownie wszystkie wysłane do serwera przy użyciu protokołu WebSockets.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-143">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="1f8fa-144">To [próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) skonfigurowano korzystającej z tego samego `NowinServer` jak poprzedni — jedyna różnica polega na w jaki sposób aplikacja jest skonfigurowana w jego `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-144">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="1f8fa-145">Test za pomocą [klienta prostego protokołu websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) pokazuje aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1f8fa-145">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Klient testowy gniazda sieci Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="1f8fa-147">Środowisko OWIN</span><span class="sxs-lookup"><span data-stu-id="1f8fa-147">OWIN environment</span></span>

<span data-ttu-id="1f8fa-148">Można utworzyć środowiska OWIN za pomocą `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-148">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="1f8fa-149">Klucze OWIN</span><span class="sxs-lookup"><span data-stu-id="1f8fa-149">OWIN keys</span></span>

<span data-ttu-id="1f8fa-150">Zależy od OWIN `IDictionary<string,object>` obiektu do przekazywania informacji w całym wymiany żądania/odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-150">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="1f8fa-151">Platformy ASP.NET Core implementuje klucze wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="1f8fa-151">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="1f8fa-152">Zobacz [specyfikacji podstawowego, rozszerzenia](http://owin.org/#spec), i [wskazówki klucz OWIN i klucze wspólne](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="1f8fa-152">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="1f8fa-153">Dane żądania (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-153">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="1f8fa-154">Key</span><span class="sxs-lookup"><span data-stu-id="1f8fa-154">Key</span></span>               | <span data-ttu-id="1f8fa-155">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-155">Value (type)</span></span> | <span data-ttu-id="1f8fa-156">Opis</span><span class="sxs-lookup"><span data-stu-id="1f8fa-156">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1f8fa-157">owin. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="1f8fa-157">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="1f8fa-158">owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="1f8fa-158">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="1f8fa-159">owin. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="1f8fa-159">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="1f8fa-160">owin. RequestPath</span><span class="sxs-lookup"><span data-stu-id="1f8fa-160">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="1f8fa-161">owin. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="1f8fa-161">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="1f8fa-162">owin. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="1f8fa-162">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="1f8fa-163">owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="1f8fa-163">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="1f8fa-164">owin. RequestBody</span><span class="sxs-lookup"><span data-stu-id="1f8fa-164">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="1f8fa-165">Dane żądania (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-165">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="1f8fa-166">Key</span><span class="sxs-lookup"><span data-stu-id="1f8fa-166">Key</span></span>               | <span data-ttu-id="1f8fa-167">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-167">Value (type)</span></span> | <span data-ttu-id="1f8fa-168">Opis</span><span class="sxs-lookup"><span data-stu-id="1f8fa-168">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1f8fa-169">owin. Identyfikator żądania</span><span class="sxs-lookup"><span data-stu-id="1f8fa-169">owin.RequestId</span></span> | `String` | <span data-ttu-id="1f8fa-170">Optional</span><span class="sxs-lookup"><span data-stu-id="1f8fa-170">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="1f8fa-171">Dane odpowiedzi (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-171">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="1f8fa-172">Key</span><span class="sxs-lookup"><span data-stu-id="1f8fa-172">Key</span></span>               | <span data-ttu-id="1f8fa-173">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-173">Value (type)</span></span> | <span data-ttu-id="1f8fa-174">Opis</span><span class="sxs-lookup"><span data-stu-id="1f8fa-174">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1f8fa-175">owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="1f8fa-175">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="1f8fa-176">Optional</span><span class="sxs-lookup"><span data-stu-id="1f8fa-176">Optional</span></span> |
| <span data-ttu-id="1f8fa-177">owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="1f8fa-177">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="1f8fa-178">Optional</span><span class="sxs-lookup"><span data-stu-id="1f8fa-178">Optional</span></span> |
| <span data-ttu-id="1f8fa-179">owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="1f8fa-179">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="1f8fa-180">owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="1f8fa-180">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="1f8fa-181">Inne dane (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-181">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="1f8fa-182">Key</span><span class="sxs-lookup"><span data-stu-id="1f8fa-182">Key</span></span>               | <span data-ttu-id="1f8fa-183">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-183">Value (type)</span></span> | <span data-ttu-id="1f8fa-184">Opis</span><span class="sxs-lookup"><span data-stu-id="1f8fa-184">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1f8fa-185">owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="1f8fa-185">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="1f8fa-186">owin. Wersja</span><span class="sxs-lookup"><span data-stu-id="1f8fa-186">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="1f8fa-187">Klucze wspólne</span><span class="sxs-lookup"><span data-stu-id="1f8fa-187">Common Keys</span></span>

| <span data-ttu-id="1f8fa-188">Key</span><span class="sxs-lookup"><span data-stu-id="1f8fa-188">Key</span></span>               | <span data-ttu-id="1f8fa-189">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-189">Value (type)</span></span> | <span data-ttu-id="1f8fa-190">Opis</span><span class="sxs-lookup"><span data-stu-id="1f8fa-190">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1f8fa-191">Protokół SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="1f8fa-191">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="1f8fa-192">Protokół SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="1f8fa-192">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="1f8fa-193">serwer. Zdalny_adres_ip</span><span class="sxs-lookup"><span data-stu-id="1f8fa-193">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="1f8fa-194">serwer. Port zdalny</span><span class="sxs-lookup"><span data-stu-id="1f8fa-194">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="1f8fa-195">serwer. Lokalny_adres_ip</span><span class="sxs-lookup"><span data-stu-id="1f8fa-195">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="1f8fa-196">serwer. Port lokalny</span><span class="sxs-lookup"><span data-stu-id="1f8fa-196">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="1f8fa-197">serwer. IsLocal</span><span class="sxs-lookup"><span data-stu-id="1f8fa-197">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="1f8fa-198">serwer. OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="1f8fa-198">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="1f8fa-199">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="1f8fa-199">SendFiles v0.3.0</span></span>

| <span data-ttu-id="1f8fa-200">Key</span><span class="sxs-lookup"><span data-stu-id="1f8fa-200">Key</span></span>               | <span data-ttu-id="1f8fa-201">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-201">Value (type)</span></span> | <span data-ttu-id="1f8fa-202">Opis</span><span class="sxs-lookup"><span data-stu-id="1f8fa-202">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1f8fa-203">sendfile. SendAsync</span><span class="sxs-lookup"><span data-stu-id="1f8fa-203">sendfile.SendAsync</span></span> | <span data-ttu-id="1f8fa-204">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-204">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="1f8fa-205">Na żądanie</span><span class="sxs-lookup"><span data-stu-id="1f8fa-205">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="1f8fa-206">V0.3.0 nieprzezroczyste</span><span class="sxs-lookup"><span data-stu-id="1f8fa-206">Opaque v0.3.0</span></span>

| <span data-ttu-id="1f8fa-207">Key</span><span class="sxs-lookup"><span data-stu-id="1f8fa-207">Key</span></span>               | <span data-ttu-id="1f8fa-208">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-208">Value (type)</span></span> | <span data-ttu-id="1f8fa-209">Opis</span><span class="sxs-lookup"><span data-stu-id="1f8fa-209">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1f8fa-210">Brak przezroczystości. Wersja</span><span class="sxs-lookup"><span data-stu-id="1f8fa-210">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="1f8fa-211">Brak przezroczystości. Uaktualnienie</span><span class="sxs-lookup"><span data-stu-id="1f8fa-211">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="1f8fa-212">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-212">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="1f8fa-213">Brak przezroczystości. Strumień</span><span class="sxs-lookup"><span data-stu-id="1f8fa-213">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="1f8fa-214">Brak przezroczystości. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="1f8fa-214">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="1f8fa-215">Protokół WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="1f8fa-215">WebSocket v0.3.0</span></span>

| <span data-ttu-id="1f8fa-216">Key</span><span class="sxs-lookup"><span data-stu-id="1f8fa-216">Key</span></span>               | <span data-ttu-id="1f8fa-217">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-217">Value (type)</span></span> | <span data-ttu-id="1f8fa-218">Opis</span><span class="sxs-lookup"><span data-stu-id="1f8fa-218">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1f8fa-219">Protokół websocket. Wersja</span><span class="sxs-lookup"><span data-stu-id="1f8fa-219">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="1f8fa-220">Protokół websocket. Zaakceptuj</span><span class="sxs-lookup"><span data-stu-id="1f8fa-220">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="1f8fa-221">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-221">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="1f8fa-222">Protokół websocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="1f8fa-222">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="1f8fa-223">Non-spec</span><span class="sxs-lookup"><span data-stu-id="1f8fa-223">Non-spec</span></span> |
| <span data-ttu-id="1f8fa-224">Protokół websocket. Podprotokół</span><span class="sxs-lookup"><span data-stu-id="1f8fa-224">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="1f8fa-225">Zobacz [RFC6455 sekcja 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) krok 5,5</span><span class="sxs-lookup"><span data-stu-id="1f8fa-225">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="1f8fa-226">Protokół websocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="1f8fa-226">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="1f8fa-227">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-227">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="1f8fa-228">Protokół websocket. Metody ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="1f8fa-228">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="1f8fa-229">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-229">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="1f8fa-230">Protokół websocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="1f8fa-230">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="1f8fa-231">Zobacz [podpisu delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1f8fa-231">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="1f8fa-232">Protokół websocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="1f8fa-232">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="1f8fa-233">Protokół websocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="1f8fa-233">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="1f8fa-234">Optional</span><span class="sxs-lookup"><span data-stu-id="1f8fa-234">Optional</span></span> |
| <span data-ttu-id="1f8fa-235">Protokół websocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="1f8fa-235">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="1f8fa-236">Optional</span><span class="sxs-lookup"><span data-stu-id="1f8fa-236">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="1f8fa-237">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1f8fa-237">Additional Resources</span></span>

* [<span data-ttu-id="1f8fa-238">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="1f8fa-238">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="1f8fa-239">Serwery</span><span class="sxs-lookup"><span data-stu-id="1f8fa-239">Servers</span></span>](servers/index.md)
