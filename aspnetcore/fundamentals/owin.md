---
title: Otwarty interfejs internetowy dla platformy .NET (OWIN) za pomocą programu ASP.NET Core
author: ardalis
description: Dowiedz się, jak platforma ASP.NET Core obsługuje Open Web Interface for .NET (OWIN), który umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/18/2018
uid: fundamentals/owin
ms.openlocfilehash: 9d6ce79c15fe768c260c6361ac3babecab5f3f9b
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087302"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="f526a-103">Otwarty interfejs internetowy dla platformy .NET (OWIN) za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f526a-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="f526a-104">Przez [Steve Smith](https://ardalis.com/) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f526a-104">By [Steve Smith](https://ardalis.com/) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f526a-105">Platforma ASP.NET Core obsługuje Open Web Interface for .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="f526a-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="f526a-106">OWIN umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.</span><span class="sxs-lookup"><span data-stu-id="f526a-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="f526a-107">Definiuje standardowy mechanizm, oprogramowaniu pośredniczącym, aby używać ich w potoku do obsługi żądań i odpowiedzi skojarzona.</span><span class="sxs-lookup"><span data-stu-id="f526a-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="f526a-108">Aplikacje platformy ASP.NET Core i oprogramowanie pośredniczące może współdziałać z aplikacji OWIN, serwerów i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="f526a-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="f526a-109">OWIN zawiera odsprzęgania warstwy, która umożliwia dwie struktury za pomocą modeli obiektów różnych być używane razem.</span><span class="sxs-lookup"><span data-stu-id="f526a-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="f526a-110">`Microsoft.AspNetCore.Owin` Pakiet zawiera dwie implementacje karty:</span><span class="sxs-lookup"><span data-stu-id="f526a-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>

* <span data-ttu-id="f526a-111">Platforma ASP.NET Core dla OWIN</span><span class="sxs-lookup"><span data-stu-id="f526a-111">ASP.NET Core to OWIN</span></span> 
* <span data-ttu-id="f526a-112">OWIN do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f526a-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="f526a-113">Dzięki temu platforma ASP.NET Core będzie hostowana na podstawie OWIN zgodny serwera/host lub inne składniki zgodne OWIN do uruchomienia na podstawie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f526a-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="f526a-114">Korzystanie z tych kart jest powiązana z negatywnie na wydajność.</span><span class="sxs-lookup"><span data-stu-id="f526a-114">Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="f526a-115">Nie używaj aplikacji przy użyciu tylko składniki platformy ASP.NET Core `Microsoft.AspNetCore.Owin` pakietu lub kart.</span><span class="sxs-lookup"><span data-stu-id="f526a-115">Apps using only ASP.NET Core components shouldn't use the `Microsoft.AspNetCore.Owin` package or adapters.</span></span>

<span data-ttu-id="f526a-116">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f526a-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a><span data-ttu-id="f526a-117">Uruchamianie oprogramowania pośredniczącego OWIN w potoku platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f526a-117">Running OWIN middleware in the ASP.NET Core pipeline</span></span>

<span data-ttu-id="f526a-118">Obsługa OWIN platformy ASP.NET Core jest wdrażana jako część `Microsoft.AspNetCore.Owin` pakietu.</span><span class="sxs-lookup"><span data-stu-id="f526a-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="f526a-119">Po zainstalowaniu tego pakietu, można zaimportować obsługi OWIN do projektu.</span><span class="sxs-lookup"><span data-stu-id="f526a-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="f526a-120">Oprogramowanie pośredniczące OWIN, który jest zgodny z [specyfikacji OWIN](http://owin.org/spec/spec/owin-1.0.0.html), co wymaga `Func<IDictionary<string, object>, Task>` można ustawić interfejsu i określonych kluczy (takie jak `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="f526a-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="f526a-121">Poniższy prosty oprogramowania pośredniczącego OWIN Wyświetla "Hello World":</span><span class="sxs-lookup"><span data-stu-id="f526a-121">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="f526a-122">Zwraca podpis przykładowe `Task` i akceptuje `IDictionary<string, object>` zgodnie z wymaganiami OWIN.</span><span class="sxs-lookup"><span data-stu-id="f526a-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="f526a-123">Poniższy kod przedstawia sposób dodawania `OwinHello` (jak pokazano powyżej) do potoku platformy ASP.NET Core za pomocą oprogramowania pośredniczącego `UseOwin` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f526a-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET Core pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="f526a-124">Można skonfigurować inne akcje do wykonania w ramach potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="f526a-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="f526a-125">Nagłówki odpowiedzi powinny być modyfikowane tylko przed pierwszej operacji zapisu w strumieniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f526a-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="f526a-126">Wiele wywołań `UseOwin` jest odradzane ze względu na wydajność.</span><span class="sxs-lookup"><span data-stu-id="f526a-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="f526a-127">Składniki OWIN będzie działać najlepiej, jeśli zgrupowane.</span><span class="sxs-lookup"><span data-stu-id="f526a-127">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-core-hosting-on-an-owin-based-server"></a><span data-ttu-id="f526a-128">Za pomocą platformy ASP.NET Core hostingu na serwerze OWIN</span><span class="sxs-lookup"><span data-stu-id="f526a-128">Using ASP.NET Core Hosting on an OWIN-based server</span></span>

<span data-ttu-id="f526a-129">Na podstawie OWIN serwerów mogło obsługiwać aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f526a-129">OWIN-based servers can host ASP.NET Core apps.</span></span> <span data-ttu-id="f526a-130">Jeden z serwerów jest [Nowin](https://github.com/Bobris/Nowin), serwer sieci web .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="f526a-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="f526a-131">W tym przykładzie w tym artykule I uwzględnione projekt, który odwołuje się Nowin i używa jej do utworzenia `IServer` zdolne do hostingu samodzielnego ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f526a-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="f526a-132">`IServer` to interfejs, który wymaga `Features` właściwości i `Start` metody.</span><span class="sxs-lookup"><span data-stu-id="f526a-132">`IServer` is an interface that requires a `Features` property and a `Start` method.</span></span>

<span data-ttu-id="f526a-133">`Start` jest odpowiedzialny za konfigurowanie i uruchamianie na serwerze, który w tym przypadku jest realizowane za pomocą szeregu fluent wywołań interfejsu API, które ustawić adresy przeanalizować za IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="f526a-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="f526a-134">Należy pamiętać, że konfiguracja fluent `_builder` zmienna Określa, że żądania będą obsługiwane przez `appFunc` zdefiniowany wcześniej w metodzie.</span><span class="sxs-lookup"><span data-stu-id="f526a-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="f526a-135">To `Func` nosi nazwę na każde żądanie, aby przetworzyć żądań przychodzących.</span><span class="sxs-lookup"><span data-stu-id="f526a-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="f526a-136">Dodamy również `IWebHostBuilder` rozszerzenia, aby ułatwić dodawanie i konfigurowanie serwera Nowin.</span><span class="sxs-lookup"><span data-stu-id="f526a-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="f526a-137">Dzięki temu w miejscu wywołania rozszerzenie w *Program.cs* do uruchamiania aplikacji ASP.NET Core za pomocą tego niestandardowego serwera:</span><span class="sxs-lookup"><span data-stu-id="f526a-137">With this in place, invoke the extension in *Program.cs* to run an ASP.NET Core app using this custom server:</span></span>

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

<span data-ttu-id="f526a-138">Dowiedz się więcej o [platformy ASP.NET Core serwerów](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="f526a-138">Learn more about [ASP.NET Core Servers](xref:fundamentals/servers/index).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="f526a-139">Uruchamianie platformy ASP.NET Core na serwerze OWIN i użyj jego obsługa funkcji WebSockets</span><span class="sxs-lookup"><span data-stu-id="f526a-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="f526a-140">Innym przykładem funkcje jak OWIN na podstawie serwerów mogą zostać wykorzystane przez ASP.NET Core to dostęp do funkcji, takich jak funkcja WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f526a-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="f526a-141">Serwer sieci web .NET OWIN, które są używane w poprzednim przykładzie zapewnia obsługę wbudowanych, gniazda sieci Web, które mogą zostać wykorzystane przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f526a-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="f526a-142">W poniższym przykładzie pokazano prostą aplikację internetową, która obsługuje gniazda sieci Web a funkcją ponownie wszystkie wiadomości wysyłane na serwer za pomocą funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f526a-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="f526a-143">To [przykładowe](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) jest skonfigurowany, korzystając z tych samych `NowinServer` jak poprzedni — jedyna różnica polega na w jaki sposób aplikacja jest skonfigurowana w jego `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="f526a-143">This [sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="f526a-144">Test za pomocą [klienta prostego protokołu websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) pokazuje aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f526a-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Klient testowy gniazda sieci Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="f526a-146">Środowisko OWIN</span><span class="sxs-lookup"><span data-stu-id="f526a-146">OWIN environment</span></span>

<span data-ttu-id="f526a-147">Można skonstruować środowisko OWIN za pomocą `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="f526a-147">You can construct an OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="f526a-148">Klucze OWIN</span><span class="sxs-lookup"><span data-stu-id="f526a-148">OWIN keys</span></span>

<span data-ttu-id="f526a-149">Zależy od OWIN `IDictionary<string,object>` obiektu do przekazywania informacji w całej wymiany żądania/odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="f526a-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="f526a-150">Platforma ASP.NET Core implementuje klucze wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="f526a-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="f526a-151">Zobacz [podstawowej specyfikacji, rozszerzeń](http://owin.org/#spec), i [wytyczne klucz OWIN i klucze wspólne](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="f526a-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="f526a-152">Dane żądania (wersja 1.0.0 OWIN)</span><span class="sxs-lookup"><span data-stu-id="f526a-152">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="f526a-153">Key</span><span class="sxs-lookup"><span data-stu-id="f526a-153">Key</span></span>               | <span data-ttu-id="f526a-154">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="f526a-154">Value (type)</span></span> | <span data-ttu-id="f526a-155">Opis</span><span class="sxs-lookup"><span data-stu-id="f526a-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="f526a-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="f526a-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="f526a-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="f526a-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="f526a-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="f526a-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="f526a-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="f526a-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="f526a-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="f526a-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="f526a-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="f526a-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="f526a-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="f526a-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="f526a-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="f526a-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="f526a-164">Dane żądania (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="f526a-164">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="f526a-165">Key</span><span class="sxs-lookup"><span data-stu-id="f526a-165">Key</span></span>               | <span data-ttu-id="f526a-166">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="f526a-166">Value (type)</span></span> | <span data-ttu-id="f526a-167">Opis</span><span class="sxs-lookup"><span data-stu-id="f526a-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="f526a-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="f526a-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="f526a-169">Optional</span><span class="sxs-lookup"><span data-stu-id="f526a-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="f526a-170">Dane odpowiedzi (OWIN 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="f526a-170">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="f526a-171">Key</span><span class="sxs-lookup"><span data-stu-id="f526a-171">Key</span></span>               | <span data-ttu-id="f526a-172">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="f526a-172">Value (type)</span></span> | <span data-ttu-id="f526a-173">Opis</span><span class="sxs-lookup"><span data-stu-id="f526a-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="f526a-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="f526a-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="f526a-175">Optional</span><span class="sxs-lookup"><span data-stu-id="f526a-175">Optional</span></span> |
| <span data-ttu-id="f526a-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="f526a-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="f526a-177">Optional</span><span class="sxs-lookup"><span data-stu-id="f526a-177">Optional</span></span> |
| <span data-ttu-id="f526a-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="f526a-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="f526a-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="f526a-179">owin.ResponseBody</span></span> | `Stream`  | |

### <a name="other-data-owin-v100"></a><span data-ttu-id="f526a-180">Inne dane (wersja 1.0.0 OWIN)</span><span class="sxs-lookup"><span data-stu-id="f526a-180">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="f526a-181">Key</span><span class="sxs-lookup"><span data-stu-id="f526a-181">Key</span></span>               | <span data-ttu-id="f526a-182">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="f526a-182">Value (type)</span></span> | <span data-ttu-id="f526a-183">Opis</span><span class="sxs-lookup"><span data-stu-id="f526a-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="f526a-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="f526a-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="f526a-185">owin. Wersja</span><span class="sxs-lookup"><span data-stu-id="f526a-185">owin.Version</span></span>  | `String` | |   

### <a name="common-keys"></a><span data-ttu-id="f526a-186">Klucze wspólne</span><span class="sxs-lookup"><span data-stu-id="f526a-186">Common keys</span></span>

| <span data-ttu-id="f526a-187">Key</span><span class="sxs-lookup"><span data-stu-id="f526a-187">Key</span></span>               | <span data-ttu-id="f526a-188">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="f526a-188">Value (type)</span></span> | <span data-ttu-id="f526a-189">Opis</span><span class="sxs-lookup"><span data-stu-id="f526a-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="f526a-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="f526a-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="f526a-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="f526a-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="f526a-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="f526a-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="f526a-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="f526a-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="f526a-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="f526a-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="f526a-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="f526a-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="f526a-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="f526a-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="f526a-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="f526a-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |

### <a name="sendfiles-v030"></a><span data-ttu-id="f526a-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="f526a-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="f526a-199">Key</span><span class="sxs-lookup"><span data-stu-id="f526a-199">Key</span></span>               | <span data-ttu-id="f526a-200">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="f526a-200">Value (type)</span></span> | <span data-ttu-id="f526a-201">Opis</span><span class="sxs-lookup"><span data-stu-id="f526a-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="f526a-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="f526a-202">sendfile.SendAsync</span></span> | <span data-ttu-id="f526a-203">Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="f526a-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="f526a-204">Na żądanie</span><span class="sxs-lookup"><span data-stu-id="f526a-204">Per Request</span></span> |

### <a name="opaque-v030"></a><span data-ttu-id="f526a-205">V0.3.0 nieprzezroczyste</span><span class="sxs-lookup"><span data-stu-id="f526a-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="f526a-206">Key</span><span class="sxs-lookup"><span data-stu-id="f526a-206">Key</span></span>               | <span data-ttu-id="f526a-207">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="f526a-207">Value (type)</span></span> | <span data-ttu-id="f526a-208">Opis</span><span class="sxs-lookup"><span data-stu-id="f526a-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="f526a-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="f526a-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="f526a-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="f526a-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="f526a-211">Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="f526a-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="f526a-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="f526a-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="f526a-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="f526a-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |

### <a name="websocket-v030"></a><span data-ttu-id="f526a-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="f526a-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="f526a-215">Key</span><span class="sxs-lookup"><span data-stu-id="f526a-215">Key</span></span>               | <span data-ttu-id="f526a-216">Wartość (typ)</span><span class="sxs-lookup"><span data-stu-id="f526a-216">Value (type)</span></span> | <span data-ttu-id="f526a-217">Opis</span><span class="sxs-lookup"><span data-stu-id="f526a-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="f526a-218">protokołu websocket. Wersja</span><span class="sxs-lookup"><span data-stu-id="f526a-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="f526a-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="f526a-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="f526a-220">Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="f526a-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="f526a-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="f526a-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="f526a-222">Non-spec</span><span class="sxs-lookup"><span data-stu-id="f526a-222">Non-spec</span></span> |
| <span data-ttu-id="f526a-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="f526a-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="f526a-224">Zobacz [RFC6455 sekcja 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) kroku 5.5</span><span class="sxs-lookup"><span data-stu-id="f526a-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="f526a-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="f526a-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="f526a-226">Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="f526a-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="f526a-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="f526a-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="f526a-228">Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="f526a-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="f526a-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="f526a-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="f526a-230">Zobacz [podpis delegata](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="f526a-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="f526a-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="f526a-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="f526a-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="f526a-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="f526a-233">Optional</span><span class="sxs-lookup"><span data-stu-id="f526a-233">Optional</span></span> |
| <span data-ttu-id="f526a-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="f526a-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="f526a-235">Optional</span><span class="sxs-lookup"><span data-stu-id="f526a-235">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="f526a-236">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f526a-236">Additional resources</span></span>

* [<span data-ttu-id="f526a-237">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="f526a-237">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="f526a-238">Serwery</span><span class="sxs-lookup"><span data-stu-id="f526a-238">Servers</span></span>](xref:fundamentals/servers/index)
