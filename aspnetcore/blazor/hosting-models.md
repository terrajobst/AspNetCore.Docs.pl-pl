---
title: ASP.NET Core Blazor hostingu modeli
author: guardrex
description: Dowiedz się, Blazor po stronie klienta i modele obsługi na stronie serwera.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 80f5e3260ce991ef67fa2a0dd36f8be1f70b6271
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813377"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="fe6a9-103">ASP.NET Core Blazor hostingu modeli</span><span class="sxs-lookup"><span data-stu-id="fe6a9-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="fe6a9-104">Przez [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="fe6a9-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="fe6a9-105">Blazor to struktura sieci web przeznaczony do działania po stronie klienta w przeglądarce na [format WebAssembly](https://webassembly.org/)— na podstawie środowiska uruchomieniowego .NET (*Blazor po stronie klienta*) lub po stronie serwera w programie ASP.NET Core (*Blazor po stronie serwera* ).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="fe6a9-106">Niezależnie od tego modelu, aplikacji i składników modelach hostingu *są takie same*.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="fe6a9-107">Aby utworzyć projekt modelach hostingu opisane w tym artykule, zobacz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="fe6a9-108">Po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="fe6a9-108">Client-side</span></span>

<span data-ttu-id="fe6a9-109">Jednostki modelu hostowania Blazor jest uruchomiona po stronie klienta w przeglądarce na format WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="fe6a9-110">Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="fe6a9-111">Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="fe6a9-112">Aktualizacje interfejsu użytkownika i obsługa zdarzeń odbywa się w obrębie tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="fe6a9-113">Zasoby aplikacji są wdrażane jako pliki statyczne z serwera sieci web lub usługą umożliwia obsługę zawartości statycznej dla klientów.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

<span data-ttu-id="fe6a9-115">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie klienta, użyj jednej z następujących szablonów:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-115">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="fe6a9-116">**Blazor (po stronie klienta)** ([blazor nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; wdrożony jako zbiór plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-116">**Blazor (client-side)** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="fe6a9-117">**Blazor (ASP.NET Core hostowane)** ([blazorhosted nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; obsługiwanej przez serwer programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-117">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="fe6a9-118">Aplikacja platformy ASP.NET Core udostępnia aplikacji Blazor klientom.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-118">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="fe6a9-119">Blazor aplikacji po stronie klienta mogą wchodzić w interakcje z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci web lub [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-119">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="fe6a9-120">Szablony zawierają *blazor.webassembly.js* skrypt, który obsługuje:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-120">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="fe6a9-121">Pobieranie, środowisko uruchomieniowe platformy .NET, aplikacji i zależności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-121">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="fe6a9-122">Inicjalizacja środowiska uruchomieniowego, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-122">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="fe6a9-123">Model hostingu w sieci po stronie klienta zapewnia kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-123">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="fe6a9-124">Nie ma żadnych zależności po stronie serwera .NET.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-124">There's no .NET server-side dependency.</span></span> <span data-ttu-id="fe6a9-125">Aplikacja działa w pełni pobrane do klienta.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-125">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="fe6a9-126">Zasoby klienta i możliwości w pełni wykorzystywane.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-126">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="fe6a9-127">Praca jest odciążany z serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-127">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="fe6a9-128">Serwer sieci web platformy ASP.NET Core nie jest wymagane do obsługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-128">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="fe6a9-129">Scenariusze wdrażania bez użycia serwera są możliwe (na przykład, udostępnia aplikacji za pośrednictwem sieci CDN).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-129">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="fe6a9-130">Istnieją wad hostingu po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-130">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="fe6a9-131">Aplikacja jest ograniczony do możliwości przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-131">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="fe6a9-132">Klienta obsługującego sprzętu i oprogramowania (na przykład w przypadku, format WebAssembly Obsługa) jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-132">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="fe6a9-133">Rozmiar pliku do pobrania jest większy, a aplikacje załadowanie trwać dłużej.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-133">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="fe6a9-134">Środowisko uruchomieniowe platformy .NET oraz narzędzia do obsługi jest mniej dojrzałe.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-134">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="fe6a9-135">Na przykład istnieją ograniczenia w [.NET Standard](/dotnet/standard/net-standard) pomocy technicznej i debugowania.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-135">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="fe6a9-136">Po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="fe6a9-136">Server-side</span></span>

<span data-ttu-id="fe6a9-137">Za pomocą modelu hostingu po stronie serwera aplikacji jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-137">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="fe6a9-138">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-138">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Przeglądarka wchodzi w interakcję z aplikacją (obsługiwana wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="fe6a9-140">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie serwera, należy użyć platformy ASP.NET Core **Blazor (po stronie serwera)** szablonu ([blazorserverside nowe dotnet](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-140">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor (server-side)** template ([dotnet new blazorserverside](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="fe6a9-141">Aplikacja platformy ASP.NET Core obsługuje aplikacji po stronie serwera i tworzy końcowy SignalR, w którym klienci łączą.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-141">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="fe6a9-142">Aplikacja platformy ASP.NET Core odwołuje się do aplikacji `Startup` klasy do dodania:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-142">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="fe6a9-143">Usługi po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-143">Server-side services.</span></span>
* <span data-ttu-id="fe6a9-144">Aplikacja do żądania obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-144">The app to the request handling pipeline.</span></span>

<span data-ttu-id="fe6a9-145">*Blazor.server.js* skryptu&dagger; nawiązuje połączenie z klientem.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-145">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="fe6a9-146">To aplikacja odpowiada za utrwalanie i przywracanie stanu aplikacji, zgodnie z potrzebami (na przykład w przypadku połączenia sieciowego utracone).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-146">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="fe6a9-147">Model hostingu w sieci po stronie serwera oferuje wiele korzyści:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-147">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="fe6a9-148">Rozmiar pliku do pobrania jest znacznie mniejszy niż aplikacji po stronie klienta, a aplikacja ładuje się szybciej.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-148">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="fe6a9-149">Aplikacja wykorzystuje pełne możliwości serwera, w tym użyj zgodnych interfejsów API dowolnej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-149">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="fe6a9-150">.NET core na serwerze jest używana do uruchamiania aplikacji, więc .NET istniejących narzędzi, takich jak debugowanie, działa zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-150">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="fe6a9-151">Elastycznej klientów są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-151">Thin clients are supported.</span></span> <span data-ttu-id="fe6a9-152">Na przykład aplikacji po stronie serwera działają z przeglądarek, które nie obsługują format WebAssembly i na ograniczonych zasobach urządzeń.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-152">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="fe6a9-153">.NET dla aplikacji /C# bazy kodu, w tym kodu składnika aplikacji, nie jest obsługiwane dla klientów.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-153">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="fe6a9-154">Istnieją wad po stronie serwera hostingu:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-154">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="fe6a9-155">Czas oczekiwania zwykle istnieje.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-155">Higher latency usually exists.</span></span> <span data-ttu-id="fe6a9-156">Każdy interakcja użytkownika obejmuje przeskok sieci.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-156">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="fe6a9-157">Brak obsługi w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-157">There's no offline support.</span></span> <span data-ttu-id="fe6a9-158">Jeśli połączenie klienta zakończy się niepowodzeniem, aplikacja przestaje działać.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-158">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="fe6a9-159">Skalowalność to wyzwanie dla aplikacji z wieloma użytkownikami.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-159">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="fe6a9-160">Serwer musi zarządzać wieloma połączeń klientów i obsługi stanu klienta.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-160">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="fe6a9-161">Serwer programu ASP.NET Core jest wymagana do obsługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-161">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="fe6a9-162">Scenariusze wdrażania bez użycia serwera nie jest możliwe (na przykład, udostępnia aplikacji za pośrednictwem sieci CDN).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-162">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="fe6a9-163">&dagger;*Blazor.server.js* skrypt jest obsługiwany z zasobu osadzonego w udostępnionej platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-163">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="fe6a9-164">Ponowne nawiązanie połączenia z tym samym serwerem</span><span class="sxs-lookup"><span data-stu-id="fe6a9-164">Reconnection to the same server</span></span>

<span data-ttu-id="fe6a9-165">Aplikacje serwerowe Blazor wymagają aktywnego połączenia SignalR do serwera.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-165">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="fe6a9-166">Jeśli połączenie zostanie przerwane, aplikacja próbuje ponownie połączyć się z serwerem.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-166">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="fe6a9-167">Tak długo, jak stan klienta jest nadal w pamięci, bez utraty stanu wznawia działanie sesji klienta.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-167">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="fe6a9-168">Klient wykryje, że połączenie zostało utracone, domyślny interfejs użytkownika jest wyświetlany użytkownikowi, gdy klient próbuje ponownie połączyć się.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-168">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="fe6a9-169">W przypadku niepowodzenia ponowne nawiązanie połączenia użytkownika podano opcję, aby spróbować ponownie.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-169">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="fe6a9-170">Aby dostosować interfejsu użytkownika, należy zdefiniować element z `components-reconnect-modal` jako jego `id`.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-170">To customize the UI, define an element with `components-reconnect-modal` as its `id`.</span></span> <span data-ttu-id="fe6a9-171">Klient aktualizuje tego elementu z jedną z następujących klas CSS na podstawie stanu połączenia:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-171">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="fe6a9-172">`components-reconnect-show` &ndash; Umożliwia wyświetlenie interfejsu użytkownika, aby wskazać, połączenie zostało utracone, a klient próbuje połączyć.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-172">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="fe6a9-173">`components-reconnect-hide` &ndash; Klient ma aktywne połączenie, Ukryj interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-173">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="fe6a9-174">`components-reconnect-failed` &ndash; Ponowne nawiązanie połączenia nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-174">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="fe6a9-175">Próba ponownego połączenia ponownie, należy wywołać `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-175">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="fe6a9-176">Stanowe ponownego łączenia po prerendering</span><span class="sxs-lookup"><span data-stu-id="fe6a9-176">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="fe6a9-177">Blazor po stronie serwera aplikacji są konfigurowane domyślnie prerender interfejsu użytkownika na serwerze, zanim zostanie nawiązane połączenie klienta z serwerem.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-177">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="fe6a9-178">Tę opcję można zdefiniować *_Host.cshtml* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-178">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="fe6a9-179">Klient ponownie nawiąże połączenie z serwerem za pomocą takiego samego stanu, który został użyty do prerender aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-179">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="fe6a9-180">Jeśli stan aplikacji jest nadal w pamięci, stan składnika nie jest rerendered po nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-180">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="fe6a9-181">Renderowanie stanowych interaktywnych składników z widoków i stron Razor</span><span class="sxs-lookup"><span data-stu-id="fe6a9-181">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="fe6a9-182">Stanowe interaktywnych składników można dodać do strony Razor lub widoku.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-182">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="fe6a9-183">Gdy powoduje wyświetlenie strony lub widoku składnika jest prerendered z nim.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-183">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="fe6a9-184">Aplikacja następnie ponownie nawiązuje połączenie stan składnika po ustanowieniu połączenia klienta, tak długo, jak stan jest nadal w pamięci.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-184">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="fe6a9-185">Na przykład następująca strona Razor renderuje `Counter` składnika z liczbą początkowej, który jest określony za pomocą formularza:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-185">For example, the following Razor page renders a `Counter` component with an initial count that's specified using a form:</span></span>
 
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

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="fe6a9-186">Wykryj, kiedy aplikacja jest prerendering</span><span class="sxs-lookup"><span data-stu-id="fe6a9-186">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="fe6a9-187">Konfigurowanie klienta SignalR Blazor po stronie serwera aplikacji</span><span class="sxs-lookup"><span data-stu-id="fe6a9-187">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="fe6a9-188">Czasami trzeba skonfigurować używane przez aplikacje serwerowe Blazor klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-188">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="fe6a9-189">Na przykład można skonfigurować klienta SignalR, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-189">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="fe6a9-190">Aby skonfigurować klienta SignalR w *Pages/_Host.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="fe6a9-190">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="fe6a9-191">Dodaj `autostart="false"` atrybutu `<script>` tagu dla *blazor.server.js* skryptu.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-191">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="fe6a9-192">Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa konstruktora SignalR.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-192">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
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

## <a name="additional-resources"></a><span data-ttu-id="fe6a9-193">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fe6a9-193">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
