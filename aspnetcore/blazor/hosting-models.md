---
title: Blazor modelach hostingu
author: guardrex
description: Dowiedz się, Blazor po stronie klienta i po stronie serwera, hostowania modeli.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2019
uid: blazor/hosting-models
ms.openlocfilehash: 7de93e8721b06e545b3125d78d5e9e0e34c04511
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982989"
---
# <a name="blazor-hosting-models"></a><span data-ttu-id="cb02d-103">Blazor modelach hostingu</span><span class="sxs-lookup"><span data-stu-id="cb02d-103">Blazor hosting models</span></span>

<span data-ttu-id="cb02d-104">Przez [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="cb02d-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="cb02d-105">Blazor to struktura sieci web przeznaczony do działania po stronie klienta w przeglądarce na [format WebAssembly](http://webassembly.org/)— na podstawie środowiska uruchomieniowego .NET (*Blazor po stronie klienta*) lub po stronie serwera w programie ASP.NET Core (*Blazor po stronie serwera* ).</span><span class="sxs-lookup"><span data-stu-id="cb02d-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="cb02d-106">Niezależnie od tego modelu, aplikacji i składników modelach hostingu *pozostają takie same*.</span><span class="sxs-lookup"><span data-stu-id="cb02d-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span>

## <a name="client-side"></a><span data-ttu-id="cb02d-107">Po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="cb02d-107">Client-side</span></span>

<span data-ttu-id="cb02d-108">Jednostki modelu hostowania Blazor jest uruchomiona po stronie klienta w przeglądarce na format WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="cb02d-108">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="cb02d-109">Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cb02d-109">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="cb02d-110">Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cb02d-110">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="cb02d-111">Aktualizacje interfejsu użytkownika i obsługa zdarzeń odbywa się w obrębie tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="cb02d-111">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="cb02d-112">Zasoby aplikacji są wdrażane jako pliki statyczne z serwera sieci web lub usługą umożliwia obsługę zawartości statycznej dla klientów.</span><span class="sxs-lookup"><span data-stu-id="cb02d-112">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

<span data-ttu-id="cb02d-114">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie klienta, użyj jednej z następujących szablonów:</span><span class="sxs-lookup"><span data-stu-id="cb02d-114">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="cb02d-115">**Blazor** ([blazor nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; wdrożony jako zbiór plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="cb02d-115">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="cb02d-116">**Blazor (ASP.NET Core hostowane)** ([blazorhosted nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; obsługiwanej przez serwer programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb02d-116">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="cb02d-117">Aplikacja platformy ASP.NET Core udostępnia aplikacji Blazor klientom.</span><span class="sxs-lookup"><span data-stu-id="cb02d-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="cb02d-118">Aplikacja Blazor po stronie klienta mogą wchodzić w interakcje z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci web lub [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="cb02d-118">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="cb02d-119">Szablony zawierają *blazor.webassembly.js* skrypt, który obsługuje:</span><span class="sxs-lookup"><span data-stu-id="cb02d-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="cb02d-120">Pobieranie, środowisko uruchomieniowe platformy .NET, aplikacji i zależności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cb02d-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="cb02d-121">Inicjalizacja środowiska uruchomieniowego, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="cb02d-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="cb02d-122">Model hostingu w sieci po stronie klienta oferuje wiele korzyści.</span><span class="sxs-lookup"><span data-stu-id="cb02d-122">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="cb02d-123">Blazor po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="cb02d-123">Client-side Blazor:</span></span>

* <span data-ttu-id="cb02d-124">Ma nie zależności po stronie serwera .NET.</span><span class="sxs-lookup"><span data-stu-id="cb02d-124">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="cb02d-125">W pełni wykorzystuje zasoby klienta i możliwości.</span><span class="sxs-lookup"><span data-stu-id="cb02d-125">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="cb02d-126">Odciążanie pracować z serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="cb02d-126">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="cb02d-127">Obsługuje scenariusze w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="cb02d-127">Supports offline scenarios.</span></span>

<span data-ttu-id="cb02d-128">Brak wad hostingu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="cb02d-128">There are downsides to client-side hosting.</span></span> <span data-ttu-id="cb02d-129">Blazor po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="cb02d-129">Client-side Blazor:</span></span>

* <span data-ttu-id="cb02d-130">Ogranicza ją do możliwości przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cb02d-130">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="cb02d-131">Wymaga klienta obsługującego sprzętu i oprogramowania (na przykład w przypadku, format WebAssembly Obsługa).</span><span class="sxs-lookup"><span data-stu-id="cb02d-131">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="cb02d-132">Ma większy rozmiar pobierania i aplikacji dłuższy czas ładowania.</span><span class="sxs-lookup"><span data-stu-id="cb02d-132">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="cb02d-133">Ma mniejszą dla dorosłych, środowisko uruchomieniowe platformy .NET oraz narzędzia do obsługi (na przykład ograniczenia w [.NET Standard](/dotnet/standard/net-standard) pomocy technicznej i debugowania).</span><span class="sxs-lookup"><span data-stu-id="cb02d-133">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side"></a><span data-ttu-id="cb02d-134">Po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="cb02d-134">Server-side</span></span>

<span data-ttu-id="cb02d-135">Za pomocą modelu hostingu po stronie serwera aplikacji jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb02d-135">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="cb02d-136">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="cb02d-136">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Przeglądarka wchodzi w interakcję z aplikacją (obsługiwana wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="cb02d-138">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie serwera, należy użyć platformy ASP.NET Core **Blazor (po stronie serwera)** szablonu ([blazorserverside nowe dotnet](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="cb02d-138">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor (server-side)** template ([dotnet new blazorserverside](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="cb02d-139">Aplikacja platformy ASP.NET Core obsługuje aplikacji po stronie serwera i ustawia punkt końcowy SignalR, gdy klienci łączą.</span><span class="sxs-lookup"><span data-stu-id="cb02d-139">The ASP.NET Core app hosts the server-side app and sets up the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="cb02d-140">Aplikacja platformy ASP.NET Core odwołuje się do aplikacji `Startup` klasy do dodania:</span><span class="sxs-lookup"><span data-stu-id="cb02d-140">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="cb02d-141">Usługi po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="cb02d-141">Server-side services.</span></span>
* <span data-ttu-id="cb02d-142">Aplikacja do żądania obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="cb02d-142">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="cb02d-143">*Blazor.server.js* skryptu&dagger; nawiązuje połączenie z klientem.</span><span class="sxs-lookup"><span data-stu-id="cb02d-143">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="cb02d-144">To aplikacja odpowiada za utrwalanie i przywracanie stanu aplikacji, zgodnie z potrzebami (na przykład w przypadku połączenia sieciowego utracone).</span><span class="sxs-lookup"><span data-stu-id="cb02d-144">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="cb02d-145">Model hostingu w sieci po stronie serwera oferuje wiele korzyści:</span><span class="sxs-lookup"><span data-stu-id="cb02d-145">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="cb02d-146">Znacznie aplikacja mniejszy rozmiar niż aplikacji po stronie klienta i ładowane dużo szybciej.</span><span class="sxs-lookup"><span data-stu-id="cb02d-146">Significantly smaller app size than a client-side app and load much faster.</span></span>
* <span data-ttu-id="cb02d-147">W pełni korzystać z możliwości serwera, takie jak przy użyciu zgodnych interfejsów API dowolnej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb02d-147">Take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="cb02d-148">Uruchom na platformie .NET Core na serwerze, dzięki czemu .NET istniejących narzędzi, takich jak debugowanie, działa zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="cb02d-148">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="cb02d-149">Współpracuje z elastycznej klientów (na przykład przeglądarek, które nie obsługują format WebAssembly i zasobach ograniczonego urządzenia).</span><span class="sxs-lookup"><span data-stu-id="cb02d-149">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="cb02d-150">.NET /C# bazy kodu, w tym kodu składnika aplikacji, nie jest obsługiwane dla klientów.</span><span class="sxs-lookup"><span data-stu-id="cb02d-150">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="cb02d-151">Istnieją wad po stronie serwera hostingu:</span><span class="sxs-lookup"><span data-stu-id="cb02d-151">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="cb02d-152">Czas oczekiwania: Każdy interakcja użytkownika obejmuje przeskok sieci.</span><span class="sxs-lookup"><span data-stu-id="cb02d-152">Higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="cb02d-153">Brak obsługi w trybie offline: Jeśli połączenie klienta zakończy się niepowodzeniem, aplikacja przestaje działać.</span><span class="sxs-lookup"><span data-stu-id="cb02d-153">No offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="cb02d-154">Ograniczoną skalowalność: Serwer musi zarządzać wieloma połączeń klientów i obsługi stanu klienta.</span><span class="sxs-lookup"><span data-stu-id="cb02d-154">Reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="cb02d-155">Serwer programu ASP.NET Core jest wymagana do obsługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cb02d-155">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="cb02d-156">Wdrożenie bez serwera (na przykład z sieci CDN) nie jest możliwe.</span><span class="sxs-lookup"><span data-stu-id="cb02d-156">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="cb02d-157">&dagger;*Blazor.server.js* skryptu jest opublikowana w następującej ścieżce: *bin / {debugowanie | Zlecenia} / {struktury docelowej} /publish/ {Nazwa aplikacji}. Aplikacja/dist/_struktura*.</span><span class="sxs-lookup"><span data-stu-id="cb02d-157">&dagger;The *blazor.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="cb02d-158">Ponowne nawiązanie połączenia z tym samym serwerem</span><span class="sxs-lookup"><span data-stu-id="cb02d-158">Reconnection to the same server</span></span>

<span data-ttu-id="cb02d-159">Aplikacje serwerowe Blazor wymagają aktywnego połączenia SignalR do serwera.</span><span class="sxs-lookup"><span data-stu-id="cb02d-159">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="cb02d-160">Jeśli połączenie zostanie przerwane, aplikacja próbuje ponownie połączyć się z serwerem.</span><span class="sxs-lookup"><span data-stu-id="cb02d-160">If a connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="cb02d-161">Tak długo, jak stan klienta jest nadal w pamięci, nie tracąc dowolny stan zostanie wznowione sesji klienta.</span><span class="sxs-lookup"><span data-stu-id="cb02d-161">As long as the client's state is still in memory, the client session resumes without losing any state.</span></span>
 
<span data-ttu-id="cb02d-162">Klient wykryje, że połączenie zostało utracone, domyślny interfejs użytkownika jest wyświetlany użytkownikowi, gdy klient próbuje ponownie połączyć się.</span><span class="sxs-lookup"><span data-stu-id="cb02d-162">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="cb02d-163">W przypadku niepowodzenia ponowne nawiązanie połączenia użytkownika podano opcję, aby spróbować ponownie.</span><span class="sxs-lookup"><span data-stu-id="cb02d-163">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="cb02d-164">Aby dostosować interfejsu użytkownika, należy zdefiniować element z `components-reconnect-modal` jako jego `id`.</span><span class="sxs-lookup"><span data-stu-id="cb02d-164">To customize the UI, define an element with `components-reconnect-modal` as its `id`.</span></span> <span data-ttu-id="cb02d-165">Klient aktualizuje tego elementu z jedną z następujących klas CSS na podstawie stanu połączenia:</span><span class="sxs-lookup"><span data-stu-id="cb02d-165">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="cb02d-166">`components-reconnect-show` &ndash; Umożliwia wyświetlenie interfejsu użytkownika, aby wskazać, połączenie zostało utracone, a klient próbuje połączyć.</span><span class="sxs-lookup"><span data-stu-id="cb02d-166">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="cb02d-167">`components-reconnect-hide` &ndash; Klient ma aktywne połączenie, Ukryj interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cb02d-167">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="cb02d-168">`components-reconnect-failed` &ndash; Ponowne nawiązanie połączenia nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="cb02d-168">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="cb02d-169">Próba ponownego połączenia ponownie, należy wywołać `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="cb02d-169">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="cb02d-170">Stanowe ponownego łączenia po prerendering</span><span class="sxs-lookup"><span data-stu-id="cb02d-170">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="cb02d-171">Blazor po stronie serwera aplikacji są konfigurowane domyślnie prerender interfejsu użytkownika na serwerze, zanim zostanie nawiązane połączenie klienta z serwerem.</span><span class="sxs-lookup"><span data-stu-id="cb02d-171">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="cb02d-172">Tę opcję można zdefiniować *_Host.cshtml* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="cb02d-172">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="cb02d-173">Klient ponownie nawiąże połączenie z serwerem za pomocą takiego samego stanu, który został użyty do prerender aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cb02d-173">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="cb02d-174">Jeśli stan aplikacji jest nadal w pamięci, stan składnika nie trzeba rerendered, po nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="cb02d-174">If the app's state is still in memory, the component state doesn't need to be rerendered once the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="cb02d-175">Renderowanie stanowych interaktywnych składników z widoków i stron Razor</span><span class="sxs-lookup"><span data-stu-id="cb02d-175">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="cb02d-176">Stanowe interaktywnych składników można dodać do strony Razor lub widoku.</span><span class="sxs-lookup"><span data-stu-id="cb02d-176">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="cb02d-177">Gdy powoduje wyświetlenie strony lub widoku składnika jest prerendered z nim.</span><span class="sxs-lookup"><span data-stu-id="cb02d-177">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="cb02d-178">Aplikacja następnie ponownie nawiązuje połączenie stan składnika po ustanowieniu połączenia klienta, tak długo, jak stan jest nadal w pamięci.</span><span class="sxs-lookup"><span data-stu-id="cb02d-178">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="cb02d-179">Na przykład następująca strona Razor renderuje składnikiem licznika z liczbą początkowej, który jest określony za pomocą formularza:</span><span class="sxs-lookup"><span data-stu-id="cb02d-179">For example, the following Razor page renders a Counter component with an initial count that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="cb02d-180">Wykryj, kiedy aplikacja jest prerendering</span><span class="sxs-lookup"><span data-stu-id="cb02d-180">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="cb02d-181">Konfigurowanie klienta SignalR Blazor po stronie serwera aplikacji</span><span class="sxs-lookup"><span data-stu-id="cb02d-181">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="cb02d-182">Czasami trzeba skonfigurować używane przez aplikacje serwerowe Blazor klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="cb02d-182">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="cb02d-183">Na przykład można skonfigurować klienta SignalR, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="cb02d-183">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="cb02d-184">Aby skonfigurować klienta SignalR w *wwwroot/index.htm* pliku:</span><span class="sxs-lookup"><span data-stu-id="cb02d-184">To configure the SignalR client in the *wwwroot/index.htm* file:</span></span>

* <span data-ttu-id="cb02d-185">Dodaj `autostart="false"` atrybutu `<script>` tagu dla *blazor.server.js* skryptu.</span><span class="sxs-lookup"><span data-stu-id="cb02d-185">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="cb02d-186">Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa konstruktora SignalR:</span><span class="sxs-lookup"><span data-stu-id="cb02d-186">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder:</span></span>
 
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

### <a name="improved-signalr-connection-lifetime-handling"></a><span data-ttu-id="cb02d-187">Ulepszona obsługa okres istnienia połączenia w SignalR</span><span class="sxs-lookup"><span data-stu-id="cb02d-187">Improved SignalR connection lifetime handling</span></span>

<span data-ttu-id="cb02d-188">Można włączyć automatyczne ponowne podłączenia przez wywołanie metody `withAutomaticReconnect` metody `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="cb02d-188">Automatic reconnects can be enabled by calling the `withAutomaticReconnect` method on `HubConnectionBuilder`:</span></span>

```csharp
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="cb02d-189">Bez określania parametrów, `withAutomaticReconnect` konfiguruje klienta, aby ponowić próbę połączenia, oczekiwanie na 0, 2, 10 i 30 sekund między kolejnymi próbami.</span><span class="sxs-lookup"><span data-stu-id="cb02d-189">Without specifying parameters, `withAutomaticReconnect` configures the client to try to reconnect, waiting 0, 2, 10, and 30 seconds between each attempt.</span></span>

<span data-ttu-id="cb02d-190">Aby skonfigurować inne niż domyślne liczbę prób ponownego połączenia przed awarią lub zmienić czas ponownego nawiązania połączenia `withAutomaticReconnect` akceptuje tablicy liczb reprezentujący opóźnienie (w milisekundach) oczekiwania przed uruchomieniem każdą próbę ponownego połączenia.</span><span class="sxs-lookup"><span data-stu-id="cb02d-190">To configure a non-default number of reconnect attempts before failure or to change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 2000, 5000]) // defaults to [0, 2000, 10000, 30000]
    .build();
```

### <a name="improved-disconnect-and-reconnect-handling"></a><span data-ttu-id="cb02d-191">Ulepszone Odłącz i ponownie obsługi</span><span class="sxs-lookup"><span data-stu-id="cb02d-191">Improved disconnect and reconnect handling</span></span>

<span data-ttu-id="cb02d-192">Przed rozpoczęciem wszelkich prób ponownego połączenia HubConnection, przechodzi do `Reconnecting` stanu i generowane jego `onreconnecting` wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="cb02d-192">Before starting any reconnect attempts, the HubConnection transitions to the `Reconnecting` state and fires its `onreconnecting` callback.</span></span> <span data-ttu-id="cb02d-193">Zapewnia to możliwość ostrzegać użytkowników, że połączenie zostało utracone, wyłączający elementy interfejsu użytkownika i eliminowanie mylące scenariuszy użytkowników, które mogą wystąpić z powodu stanie odłączonym.</span><span class="sxs-lookup"><span data-stu-id="cb02d-193">This provides an opportunity to warn users that the connection was lost, disable UI elements, and mitigate confusing user scenarios that might occur due to the disconnected state.</span></span>

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="cb02d-194">Jeśli klient pomyślnie połączy się ponownie w ramach jego pierwsze cztery prób `HubConnection`przejść z powrotem do `Connected` stanu i generowane `onreconnected` wywołań zwrotnych.</span><span class="sxs-lookup"><span data-stu-id="cb02d-194">If the client successfully reconnects within its first four attempts, the `HubConnection`transitions back to the `Connected` state and fires `onreconnected` callbacks.</span></span> <span data-ttu-id="cb02d-195">To daje deweloperom możliwość zawiadomić użytkowników o tym, że połączenie zostało nawiązane ponownie.</span><span class="sxs-lookup"><span data-stu-id="cb02d-195">This gives developers an opportunity to inform users that the connection is re-established.</span></span>

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="cb02d-196">Jeśli klient nie pomyślnie ponownie połączyć w ramach jego pierwsze cztery prób `HubConnection` przechodzi do `Disconnected` stanu i generowane jego `onclosed` wywołań zwrotnych.</span><span class="sxs-lookup"><span data-stu-id="cb02d-196">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` transitions to the `Disconnected` state and fires its `onclosed` callbacks.</span></span> <span data-ttu-id="cb02d-197">Jest to doskonała okazja, aby zawiadomić użytkowników o tym, że połączenie jest trwale utracone i zaleca, aby odświeżyć stronę.</span><span class="sxs-lookup"><span data-stu-id="cb02d-197">This is a good opportunity to inform users that the connection is permanently lost and recommend refreshing the page.</span></span>

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

## <a name="additional-resources"></a><span data-ttu-id="cb02d-198">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cb02d-198">Additional resources</span></span>

* <xref:signalr/introduction>
