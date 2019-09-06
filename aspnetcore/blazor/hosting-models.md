---
title: ASP.NET Core modele hostingowe Blazor
author: guardrex
description: Poznaj modele hostingu po stronie klienta i serwera Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: bf2bce4f89e8bfe6e5aeeb4860c85a60c5eb4b7c
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310391"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="4eb36-103">ASP.NET Core modele hostingowe Blazor</span><span class="sxs-lookup"><span data-stu-id="4eb36-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="4eb36-104">Autor [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="4eb36-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="4eb36-105">Blazor to platforma internetowa przeznaczona do uruchamiania po stronie klienta w przeglądarce w środowisku uruchomieniowym .NET runtime (*Blazor Client*) lub po stronie serwera w ASP.NET Core (*Blazor po stronie serwera*). [](https://webassembly.org/)</span><span class="sxs-lookup"><span data-stu-id="4eb36-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="4eb36-106">Niezależnie od modelu hostingu modele aplikacji i składników *są takie same*.</span><span class="sxs-lookup"><span data-stu-id="4eb36-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="4eb36-107">Aby utworzyć projekt dla modeli hostingu opisanych w tym artykule, zobacz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="4eb36-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="4eb36-108">Po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="4eb36-108">Client-side</span></span>

<span data-ttu-id="4eb36-109">Główny model hostingu dla Blazor jest uruchomiony po stronie klienta w przeglądarce w programie webassembly.</span><span class="sxs-lookup"><span data-stu-id="4eb36-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="4eb36-110">Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4eb36-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="4eb36-111">Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4eb36-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="4eb36-112">Aktualizacje interfejsu użytkownika i obsługa zdarzeń są wykonywane w ramach tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="4eb36-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="4eb36-113">Zasoby aplikacji są wdrażane jako pliki statyczne na serwerze sieci Web lub usłudze obsługującej zawartość statyczną dla klientów.</span><span class="sxs-lookup"><span data-stu-id="4eb36-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

<span data-ttu-id="4eb36-115">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu po stronie klienta, użyj szablonu **aplikacji Blazor webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="4eb36-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="4eb36-116">Po wybraniu szablonu **aplikacji Blazor webassembly** można skonfigurować aplikację do korzystania z zaplecza ASP.NET Core, zaznaczając pole wyboru **hostowane ASP.NET Core** (polecenie[dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="4eb36-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="4eb36-117">Aplikacja ASP.NET Core udostępnia klientom aplikację Blazor.</span><span class="sxs-lookup"><span data-stu-id="4eb36-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="4eb36-118">Aplikacja po stronie klienta Blazor może współdziałać z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci Web lub [sygnalizującego](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="4eb36-118">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="4eb36-119">Szablony obejmują skrypt *blazor. webassembly. js* , który obsługuje:</span><span class="sxs-lookup"><span data-stu-id="4eb36-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="4eb36-120">Pobieranie środowiska uruchomieniowego platformy .NET, aplikacji i zależności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4eb36-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="4eb36-121">Inicjowanie środowiska uruchomieniowego w celu uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4eb36-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="4eb36-122">Model hostingu po stronie klienta oferuje kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="4eb36-122">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="4eb36-123">Nie istnieje zależność po stronie serwera .NET.</span><span class="sxs-lookup"><span data-stu-id="4eb36-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="4eb36-124">Aplikacja jest w pełni funkcjonalna po pobraniu do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="4eb36-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="4eb36-125">Zasoby i możliwości klienta są w pełni wykorzystywane.</span><span class="sxs-lookup"><span data-stu-id="4eb36-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="4eb36-126">Zadania są Odciążone z serwera do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="4eb36-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="4eb36-127">Serwer sieci Web ASP.NET Core nie jest wymagany do hostowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4eb36-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="4eb36-128">Możliwe są scenariusze wdrażania bezserwerowego (na przykład obsługujące aplikację z sieci CDN).</span><span class="sxs-lookup"><span data-stu-id="4eb36-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="4eb36-129">Downsides do hostingu po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="4eb36-129">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="4eb36-130">Aplikacja jest ograniczona do możliwości przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4eb36-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="4eb36-131">Wymagany jest sprzęt i oprogramowanie klienta (na przykład obsługa zestawu webassembly).</span><span class="sxs-lookup"><span data-stu-id="4eb36-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="4eb36-132">Rozmiar pobieranych plików jest większy i ładowanie aplikacji trwa dłużej.</span><span class="sxs-lookup"><span data-stu-id="4eb36-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="4eb36-133">Środowisko uruchomieniowe platformy .NET i obsługa narzędzi są mniej dojrzałe.</span><span class="sxs-lookup"><span data-stu-id="4eb36-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="4eb36-134">Na przykład istnieją ograniczenia dotyczące obsługi [.NET Standard](/dotnet/standard/net-standard) i debugowania.</span><span class="sxs-lookup"><span data-stu-id="4eb36-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="4eb36-135">Po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="4eb36-135">Server-side</span></span>

<span data-ttu-id="4eb36-136">W modelu hostingu po stronie serwera aplikacja jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4eb36-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="4eb36-137">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [sygnalizujące](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="4eb36-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Przeglądarka współdziała z aplikacją (hostowaną wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia sygnalizującego.](hosting-models/_static/server-side.png)

<span data-ttu-id="4eb36-139">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu po stronie serwera, użyj szablonu aplikacji ASP.NET Core **Blazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="4eb36-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="4eb36-140">Aplikacja ASP.NET Core obsługuje aplikację po stronie serwera i tworzy punkt końcowy sygnalizujący, do którego klienci nawiązują połączenie.</span><span class="sxs-lookup"><span data-stu-id="4eb36-140">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="4eb36-141">Aplikacja ASP.NET Core odwołuje się do `Startup` klasy aplikacji do dodania:</span><span class="sxs-lookup"><span data-stu-id="4eb36-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="4eb36-142">Usługi po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4eb36-142">Server-side services.</span></span>
* <span data-ttu-id="4eb36-143">Aplikacja do potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="4eb36-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="4eb36-144">Skrypt&dagger; *blazor. Server. js* nawiązuje połączenie z klientem.</span><span class="sxs-lookup"><span data-stu-id="4eb36-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="4eb36-145">Jest on odpowiedzialny za utrzymanie i przywrócenie stanu aplikacji zgodnie z wymaganiami (na przykład w przypadku utraconego połączenia sieciowego).</span><span class="sxs-lookup"><span data-stu-id="4eb36-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="4eb36-146">Model hostingu po stronie serwera oferuje kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="4eb36-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="4eb36-147">Rozmiar pobieranych plików jest znacznie mniejszy niż aplikacja po stronie klienta, a aplikacja jest znacznie szybsza.</span><span class="sxs-lookup"><span data-stu-id="4eb36-147">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="4eb36-148">Aplikacja w pełni wykorzystuje możliwości serwera, w tym używanie dowolnego zgodnego z platformą .NET Core interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="4eb36-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="4eb36-149">Program .NET Core na serwerze jest używany do uruchamiania aplikacji, więc istniejące narzędzia platformy .NET, takie jak debugowanie, działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="4eb36-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="4eb36-150">Klienci zubożoni są obsługiwani.</span><span class="sxs-lookup"><span data-stu-id="4eb36-150">Thin clients are supported.</span></span> <span data-ttu-id="4eb36-151">Na przykład aplikacje po stronie serwera współpracują z przeglądarkami, które nie obsługują zestawu webassembly i na urządzeniach z ograniczeniami zasobów.</span><span class="sxs-lookup"><span data-stu-id="4eb36-151">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="4eb36-152">Baza danych platformy .NET/C# kodu aplikacji, w tym kod składnika aplikacji, nie jest obsługiwana dla klientów.</span><span class="sxs-lookup"><span data-stu-id="4eb36-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="4eb36-153">Downsides do hostingu po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="4eb36-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="4eb36-154">Wyższe opóźnienia zwykle istnieją.</span><span class="sxs-lookup"><span data-stu-id="4eb36-154">Higher latency usually exists.</span></span> <span data-ttu-id="4eb36-155">Każda interakcja użytkownika obejmuje przeskok sieci.</span><span class="sxs-lookup"><span data-stu-id="4eb36-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="4eb36-156">Brak obsługi offline.</span><span class="sxs-lookup"><span data-stu-id="4eb36-156">There's no offline support.</span></span> <span data-ttu-id="4eb36-157">Jeśli połączenie z klientem zakończy się niepowodzeniem, aplikacja przestanie działać.</span><span class="sxs-lookup"><span data-stu-id="4eb36-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="4eb36-158">Skalowalność jest wyzwaniem dla aplikacji z wieloma użytkownikami.</span><span class="sxs-lookup"><span data-stu-id="4eb36-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="4eb36-159">Serwer musi zarządzać wieloma połączeniami klientów i obsługiwać stan klienta.</span><span class="sxs-lookup"><span data-stu-id="4eb36-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="4eb36-160">Do obsłużynia aplikacji wymagany jest serwer ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4eb36-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="4eb36-161">Scenariusze wdrażania bez użycia serwera nie są możliwe (na przykład w celu obsługi aplikacji z sieci CDN).</span><span class="sxs-lookup"><span data-stu-id="4eb36-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="4eb36-162">&dagger;Skrypt *blazor. Server. js* jest obsługiwany z zasobów osadzonych w ASP.NET Core udostępnionej platformie.</span><span class="sxs-lookup"><span data-stu-id="4eb36-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="4eb36-163">Ponowne nawiązywanie połączenia z tym samym serwerem</span><span class="sxs-lookup"><span data-stu-id="4eb36-163">Reconnection to the same server</span></span>

<span data-ttu-id="4eb36-164">Blazor aplikacje po stronie serwera wymagają aktywnego połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="4eb36-164">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="4eb36-165">Jeśli połączenie zostanie utracone, aplikacja spróbuje ponownie nawiązać połączenie z serwerem.</span><span class="sxs-lookup"><span data-stu-id="4eb36-165">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="4eb36-166">O ile stan klienta nadal znajduje się w pamięci, sesja klienta zostaje wznowiona bez utraty stanu.</span><span class="sxs-lookup"><span data-stu-id="4eb36-166">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="4eb36-167">Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="4eb36-167">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="4eb36-168">Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby.</span><span class="sxs-lookup"><span data-stu-id="4eb36-168">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="4eb36-169">Aby dostosować interfejs użytkownika, zdefiniuj element `components-reconnect-modal` jako jego `id` na stronie Razor *_Host. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="4eb36-169">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="4eb36-170">Klient aktualizuje ten element za pomocą jednej z następujących klas CSS w oparciu o stan połączenia:</span><span class="sxs-lookup"><span data-stu-id="4eb36-170">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="4eb36-171">`components-reconnect-show`&ndash; Pokaż interfejs użytkownika wskazujący, że połączenie zostało utracone, a klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="4eb36-171">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="4eb36-172">`components-reconnect-hide`&ndash; Klient ma aktywne połączenie, Ukryj interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4eb36-172">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="4eb36-173">`components-reconnect-failed`&ndash; Ponowne nawiązywanie połączenia nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="4eb36-173">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="4eb36-174">Aby spróbować ponownie nawiązać połączenie, `window.Blazor.reconnect()`Wywołaj polecenie.</span><span class="sxs-lookup"><span data-stu-id="4eb36-174">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="4eb36-175">Stanowe Ponowne nawiązywanie połączenia po przeprowadzeniu prerenderowania</span><span class="sxs-lookup"><span data-stu-id="4eb36-175">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="4eb36-176">Aplikacje po stronie serwera Blazor są domyślnie skonfigurowane w taki sposób, aby były oczyszczane przez interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="4eb36-176">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="4eb36-177">Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="4eb36-177">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="4eb36-178">`RenderMode`Określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="4eb36-178">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="4eb36-179">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="4eb36-179">Is prerendered into the page.</span></span>
* <span data-ttu-id="4eb36-180">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4eb36-180">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="4eb36-181">Opis</span><span class="sxs-lookup"><span data-stu-id="4eb36-181">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="4eb36-182">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji po stronie serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4eb36-182">Renders the component into static HTML and includes a marker for a Blazor server-side app.</span></span> <span data-ttu-id="4eb36-183">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="4eb36-183">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="4eb36-184">Parametry nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4eb36-184">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="4eb36-185">Renderuje znacznik dla aplikacji po stronie serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4eb36-185">Renders a marker for a Blazor server-side app.</span></span> <span data-ttu-id="4eb36-186">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="4eb36-186">Output from the component isn't included.</span></span> <span data-ttu-id="4eb36-187">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="4eb36-187">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="4eb36-188">Parametry nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4eb36-188">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="4eb36-189">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="4eb36-189">Renders the component into static HTML.</span></span> <span data-ttu-id="4eb36-190">Parametry są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4eb36-190">Parameters are supported.</span></span> |

<span data-ttu-id="4eb36-191">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4eb36-191">Rendering server components from a static HTML page isn't supported.</span></span>
 
<span data-ttu-id="4eb36-192">Klient ponownie nawiązuje połączenie z serwerem z tym samym stanem, który został użyty do wygenerowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4eb36-192">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="4eb36-193">Jeśli stan aplikacji nadal znajduje się w pamięci, stan składnika nie jest ponownie renderowany po nawiązaniu połączenia z sygnałem.</span><span class="sxs-lookup"><span data-stu-id="4eb36-193">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="4eb36-194">Renderuj stanowe składniki interaktywne ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="4eb36-194">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="4eb36-195">Można dodać składniki interaktywne ze stanem do strony lub widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="4eb36-195">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="4eb36-196">Gdy renderuje stronę lub widok:</span><span class="sxs-lookup"><span data-stu-id="4eb36-196">When the page or view renders:</span></span>

* <span data-ttu-id="4eb36-197">Składnik jest wstępnie renderowany przy użyciu strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="4eb36-197">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="4eb36-198">Początkowy stan składnika używany na potrzeby renderowania wstępnego został utracony.</span><span class="sxs-lookup"><span data-stu-id="4eb36-198">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="4eb36-199">Nowy stan składnika jest tworzony po nawiązaniu połączenia z sygnałem.</span><span class="sxs-lookup"><span data-stu-id="4eb36-199">New component state is created when the SignalR connection is established.</span></span>
 
<span data-ttu-id="4eb36-200">Następująca strona Razor renderuje `Counter` składnik:</span><span class="sxs-lookup"><span data-stu-id="4eb36-200">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>
 
@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="4eb36-201">Renderuj nieinteraktywne składniki ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="4eb36-201">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="4eb36-202">Na poniższej stronie `MyComponent` Razor składnik jest statycznie renderowany z wartością początkową określoną przy użyciu formularza:</span><span class="sxs-lookup"><span data-stu-id="4eb36-202">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>
 
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="4eb36-203">Ponieważ `MyComponent` jest renderowany statycznie, składnik nie może być interaktywny.</span><span class="sxs-lookup"><span data-stu-id="4eb36-203">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="4eb36-204">Wykryj, kiedy aplikacja jest przedrenderowana</span><span class="sxs-lookup"><span data-stu-id="4eb36-204">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="4eb36-205">Konfigurowanie klienta sygnalizującego dla aplikacji po stronie serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="4eb36-205">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="4eb36-206">Czasami trzeba skonfigurować klienta sygnalizującego używany przez aplikacje po stronie serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="4eb36-206">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="4eb36-207">Na przykład może być konieczne skonfigurowanie rejestrowania na kliencie sygnalizującego, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="4eb36-207">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="4eb36-208">Aby skonfigurować klienta sygnalizującego w pliku *Pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="4eb36-208">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="4eb36-209">Dodaj atrybut do znacznika dla skryptu *blazor. Server. js.* `<script>` `autostart="false"`</span><span class="sxs-lookup"><span data-stu-id="4eb36-209">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="4eb36-210">Wywoływanie `Blazor.start` i przekazywanie obiektu konfiguracji, który określa konstruktora sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="4eb36-210">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="4eb36-211">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4eb36-211">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
