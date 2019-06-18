---
title: ASP.NET Core Blazor hostingu modeli
author: guardrex
description: Dowiedz się, Blazor po stronie klienta i po stronie serwera, hostowania modeli.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/hosting-models
ms.openlocfilehash: c794daf6f33138c57500a04116a3d172f0201bd5
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152797"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="a5248-103">ASP.NET Core Blazor hostingu modeli</span><span class="sxs-lookup"><span data-stu-id="a5248-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="a5248-104">Przez [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a5248-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a5248-105">Blazor to struktura sieci web przeznaczony do działania po stronie klienta w przeglądarce na [format WebAssembly](http://webassembly.org/)— na podstawie środowiska uruchomieniowego .NET (*Blazor po stronie klienta*) lub po stronie serwera w programie ASP.NET Core (*Blazor po stronie serwera* ).</span><span class="sxs-lookup"><span data-stu-id="a5248-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="a5248-106">Niezależnie od tego modelu, aplikacji i składników modelach hostingu *są takie same*.</span><span class="sxs-lookup"><span data-stu-id="a5248-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="a5248-107">Aby utworzyć projekt modelach hostingu opisane w tym artykule, zobacz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="a5248-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="a5248-108">Po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="a5248-108">Client-side</span></span>

<span data-ttu-id="a5248-109">Jednostki modelu hostowania Blazor jest uruchomiona po stronie klienta w przeglądarce na format WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="a5248-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="a5248-110">Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a5248-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="a5248-111">Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a5248-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="a5248-112">Aktualizacje interfejsu użytkownika i obsługa zdarzeń odbywa się w obrębie tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="a5248-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="a5248-113">Zasoby aplikacji są wdrażane jako pliki statyczne z serwera sieci web lub usługą umożliwia obsługę zawartości statycznej dla klientów.</span><span class="sxs-lookup"><span data-stu-id="a5248-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

<span data-ttu-id="a5248-115">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie klienta, użyj jednej z następujących szablonów:</span><span class="sxs-lookup"><span data-stu-id="a5248-115">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="a5248-116">**Blazor (po stronie klienta)** ([blazor nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; wdrożony jako zbiór plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="a5248-116">**Blazor (client-side)** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="a5248-117">**Blazor (ASP.NET Core hostowane)** ([blazorhosted nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; obsługiwanej przez serwer programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5248-117">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="a5248-118">Aplikacja platformy ASP.NET Core udostępnia aplikacji Blazor klientom.</span><span class="sxs-lookup"><span data-stu-id="a5248-118">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="a5248-119">Aplikacja Blazor po stronie klienta mogą wchodzić w interakcje z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci web lub [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="a5248-119">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="a5248-120">Szablony zawierają *blazor.webassembly.js* skrypt, który obsługuje:</span><span class="sxs-lookup"><span data-stu-id="a5248-120">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="a5248-121">Pobieranie, środowisko uruchomieniowe platformy .NET, aplikacji i zależności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5248-121">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="a5248-122">Inicjalizacja środowiska uruchomieniowego, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="a5248-122">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="a5248-123">Model hostingu w sieci po stronie klienta oferuje wiele korzyści.</span><span class="sxs-lookup"><span data-stu-id="a5248-123">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="a5248-124">Blazor po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="a5248-124">Client-side Blazor:</span></span>

* <span data-ttu-id="a5248-125">Ma nie zależności po stronie serwera .NET.</span><span class="sxs-lookup"><span data-stu-id="a5248-125">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="a5248-126">W pełni wykorzystuje zasoby klienta i możliwości.</span><span class="sxs-lookup"><span data-stu-id="a5248-126">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="a5248-127">Odciążanie pracować z serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="a5248-127">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="a5248-128">Obsługuje scenariusze w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="a5248-128">Supports offline scenarios.</span></span>

<span data-ttu-id="a5248-129">Brak wad hostingu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a5248-129">There are downsides to client-side hosting.</span></span> <span data-ttu-id="a5248-130">Blazor po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="a5248-130">Client-side Blazor:</span></span>

* <span data-ttu-id="a5248-131">Ogranicza ją do możliwości przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a5248-131">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="a5248-132">Wymaga klienta obsługującego sprzętu i oprogramowania (na przykład w przypadku, format WebAssembly Obsługa).</span><span class="sxs-lookup"><span data-stu-id="a5248-132">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="a5248-133">Ma większy rozmiar pobierania i aplikacji dłuższy czas ładowania.</span><span class="sxs-lookup"><span data-stu-id="a5248-133">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="a5248-134">Ma mniejszą dla dorosłych, środowisko uruchomieniowe platformy .NET oraz narzędzia do obsługi (na przykład ograniczenia w [.NET Standard](/dotnet/standard/net-standard) pomocy technicznej i debugowania).</span><span class="sxs-lookup"><span data-stu-id="a5248-134">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side"></a><span data-ttu-id="a5248-135">Po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="a5248-135">Server-side</span></span>

<span data-ttu-id="a5248-136">Za pomocą modelu hostingu po stronie serwera aplikacji jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5248-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="a5248-137">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="a5248-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Przeglądarka wchodzi w interakcję z aplikacją (obsługiwana wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="a5248-139">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie serwera, należy użyć platformy ASP.NET Core **Blazor (po stronie serwera)** szablonu ([blazorserverside nowe dotnet](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="a5248-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor (server-side)** template ([dotnet new blazorserverside](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="a5248-140">Aplikacja platformy ASP.NET Core obsługuje aplikacji po stronie serwera i ustawia punkt końcowy SignalR, gdy klienci łączą.</span><span class="sxs-lookup"><span data-stu-id="a5248-140">The ASP.NET Core app hosts the server-side app and sets up the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="a5248-141">Aplikacja platformy ASP.NET Core odwołuje się do aplikacji `Startup` klasy do dodania:</span><span class="sxs-lookup"><span data-stu-id="a5248-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="a5248-142">Usługi po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="a5248-142">Server-side services.</span></span>
* <span data-ttu-id="a5248-143">Aplikacja do żądania obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="a5248-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="a5248-144">*Blazor.server.js* skryptu&dagger; nawiązuje połączenie z klientem.</span><span class="sxs-lookup"><span data-stu-id="a5248-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="a5248-145">To aplikacja odpowiada za utrwalanie i przywracanie stanu aplikacji, zgodnie z potrzebami (na przykład w przypadku połączenia sieciowego utracone).</span><span class="sxs-lookup"><span data-stu-id="a5248-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="a5248-146">Model hostingu w sieci po stronie serwera oferuje wiele korzyści:</span><span class="sxs-lookup"><span data-stu-id="a5248-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="a5248-147">Rozmiar aplikacji znacznie mniejszy niż aplikacji po stronie klienta i ładuje znacznie szybciej.</span><span class="sxs-lookup"><span data-stu-id="a5248-147">Has a significantly smaller app size than a client-side app and loads much faster.</span></span>
* <span data-ttu-id="a5248-148">Pełne wykorzystanie możliwości serwera, takie jak przy użyciu zgodnych interfejsów API dowolnej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5248-148">Takes full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="a5248-149">Działa na platformie .NET Core w serwer, więc .NET istniejących narzędzi, takich jak debugowanie, działa zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="a5248-149">Runs on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="a5248-150">Działa z elastycznej klientów.</span><span class="sxs-lookup"><span data-stu-id="a5248-150">Works with thin clients.</span></span> <span data-ttu-id="a5248-151">Na przykład współpracuje z przeglądarek, które nie obsługują format WebAssembly i zasobów ograniczone urządzeń.</span><span class="sxs-lookup"><span data-stu-id="a5248-151">For example, works with browsers that don't support WebAssembly and resource constrained devices.</span></span>
* <span data-ttu-id="a5248-152">.NET /C# bazy kodu, w tym kodu składnika aplikacji, nie jest obsługiwane dla klientów.</span><span class="sxs-lookup"><span data-stu-id="a5248-152">The .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="a5248-153">Istnieją wad po stronie serwera hostingu:</span><span class="sxs-lookup"><span data-stu-id="a5248-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="a5248-154">Czas oczekiwania: Każdy interakcja użytkownika obejmuje przeskok sieci.</span><span class="sxs-lookup"><span data-stu-id="a5248-154">Higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="a5248-155">Brak obsługi w trybie offline: Jeśli połączenie klienta zakończy się niepowodzeniem, aplikacja przestaje działać.</span><span class="sxs-lookup"><span data-stu-id="a5248-155">No offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="a5248-156">Ograniczoną skalowalność: Serwer musi zarządzać wieloma połączeń klientów i obsługi stanu klienta.</span><span class="sxs-lookup"><span data-stu-id="a5248-156">Reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="a5248-157">Serwer programu ASP.NET Core jest wymagana do obsługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5248-157">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="a5248-158">Wdrożenie bez serwera (na przykład z sieci CDN) nie jest możliwe.</span><span class="sxs-lookup"><span data-stu-id="a5248-158">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="a5248-159">&dagger;*Blazor.server.js* skrypt jest obsługiwany z zasobu osadzonego w udostępnionej platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5248-159">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="a5248-160">Ponowne nawiązanie połączenia z tym samym serwerem</span><span class="sxs-lookup"><span data-stu-id="a5248-160">Reconnection to the same server</span></span>

<span data-ttu-id="a5248-161">Aplikacje serwerowe Blazor wymagają aktywnego połączenia SignalR do serwera.</span><span class="sxs-lookup"><span data-stu-id="a5248-161">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="a5248-162">Jeśli połączenie zostanie przerwane, aplikacja próbuje ponownie połączyć się z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a5248-162">If a connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="a5248-163">Tak długo, jak stan klienta jest nadal w pamięci, bez utraty stanu wznawia działanie sesji klienta.</span><span class="sxs-lookup"><span data-stu-id="a5248-163">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="a5248-164">Klient wykryje, że połączenie zostało utracone, domyślny interfejs użytkownika jest wyświetlany użytkownikowi, gdy klient próbuje ponownie połączyć się.</span><span class="sxs-lookup"><span data-stu-id="a5248-164">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="a5248-165">W przypadku niepowodzenia ponowne nawiązanie połączenia użytkownika podano opcję, aby spróbować ponownie.</span><span class="sxs-lookup"><span data-stu-id="a5248-165">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="a5248-166">Aby dostosować interfejsu użytkownika, należy zdefiniować element z `components-reconnect-modal` jako jego `id`.</span><span class="sxs-lookup"><span data-stu-id="a5248-166">To customize the UI, define an element with `components-reconnect-modal` as its `id`.</span></span> <span data-ttu-id="a5248-167">Klient aktualizuje tego elementu z jedną z następujących klas CSS na podstawie stanu połączenia:</span><span class="sxs-lookup"><span data-stu-id="a5248-167">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="a5248-168">`components-reconnect-show` &ndash; Umożliwia wyświetlenie interfejsu użytkownika, aby wskazać, połączenie zostało utracone, a klient próbuje połączyć.</span><span class="sxs-lookup"><span data-stu-id="a5248-168">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="a5248-169">`components-reconnect-hide` &ndash; Klient ma aktywne połączenie, Ukryj interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a5248-169">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="a5248-170">`components-reconnect-failed` &ndash; Ponowne nawiązanie połączenia nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="a5248-170">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="a5248-171">Próba ponownego połączenia ponownie, należy wywołać `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="a5248-171">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="a5248-172">Stanowe ponownego łączenia po prerendering</span><span class="sxs-lookup"><span data-stu-id="a5248-172">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="a5248-173">Blazor po stronie serwera aplikacji są konfigurowane domyślnie prerender interfejsu użytkownika na serwerze, zanim zostanie nawiązane połączenie klienta z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a5248-173">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="a5248-174">Tę opcję można zdefiniować *_Host.cshtml* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="a5248-174">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="a5248-175">Klient ponownie nawiąże połączenie z serwerem za pomocą takiego samego stanu, który został użyty do prerender aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5248-175">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="a5248-176">Jeśli stan aplikacji jest nadal w pamięci, stan składnika nie jest rerendered po nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="a5248-176">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="a5248-177">Renderowanie stanowych interaktywnych składników z widoków i stron Razor</span><span class="sxs-lookup"><span data-stu-id="a5248-177">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="a5248-178">Stanowe interaktywnych składników można dodać do strony Razor lub widoku.</span><span class="sxs-lookup"><span data-stu-id="a5248-178">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="a5248-179">Gdy powoduje wyświetlenie strony lub widoku składnika jest prerendered z nim.</span><span class="sxs-lookup"><span data-stu-id="a5248-179">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="a5248-180">Aplikacja następnie ponownie nawiązuje połączenie stan składnika po ustanowieniu połączenia klienta, tak długo, jak stan jest nadal w pamięci.</span><span class="sxs-lookup"><span data-stu-id="a5248-180">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="a5248-181">Na przykład następująca strona Razor renderuje składnikiem licznika z liczbą początkowej, który jest określony za pomocą formularza:</span><span class="sxs-lookup"><span data-stu-id="a5248-181">For example, the following Razor page renders a Counter component with an initial count that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="a5248-182">Wykryj, kiedy aplikacja jest prerendering</span><span class="sxs-lookup"><span data-stu-id="a5248-182">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="a5248-183">Konfigurowanie klienta SignalR Blazor po stronie serwera aplikacji</span><span class="sxs-lookup"><span data-stu-id="a5248-183">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="a5248-184">Czasami trzeba skonfigurować używane przez aplikacje serwerowe Blazor klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="a5248-184">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="a5248-185">Na przykład można skonfigurować klienta SignalR, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="a5248-185">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="a5248-186">Aby skonfigurować klienta SignalR w *stron /\_Host.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="a5248-186">To configure the SignalR client in the *Pages/\_Host.cshtml* file:</span></span>

* <span data-ttu-id="a5248-187">Dodaj `autostart="false"` atrybutu `<script>` tagu dla *blazor.server.js* skryptu.</span><span class="sxs-lookup"><span data-stu-id="a5248-187">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="a5248-188">Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa konstruktora SignalR.</span><span class="sxs-lookup"><span data-stu-id="a5248-188">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
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

## <a name="additional-resources"></a><span data-ttu-id="a5248-189">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a5248-189">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
