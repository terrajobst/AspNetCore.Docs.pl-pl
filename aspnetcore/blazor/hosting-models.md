---
title: ASP.NET Core Blazor modele hostingowe
author: guardrex
description: Informacje na temat Blazor modeli hostingu i Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 145f385fd6c5d04510a4ac15a41b879591ab5caa
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885526"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="6bb15-103">ASP.NET Core modele hostingowe Blazor</span><span class="sxs-lookup"><span data-stu-id="6bb15-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="6bb15-104">Autor [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="6bb15-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="6bb15-105">Blazor to platforma internetowa, która umożliwia uruchamianie po stronie klienta w przeglądarce w środowisku [](https://webassembly.org/)uruchomieniowym .NET runtime (*Blazor webassembly*) lub po stronie serwera w ASP.NET Core (*Blazor Server*).</span><span class="sxs-lookup"><span data-stu-id="6bb15-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="6bb15-106">Niezależnie od modelu hostingu modele aplikacji i składników *są takie same*.</span><span class="sxs-lookup"><span data-stu-id="6bb15-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="6bb15-107">Aby utworzyć projekt dla modeli hostingu opisanych w tym artykule, zobacz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="6bb15-108">Zestaw WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="6bb15-108">Blazor WebAssembly</span></span>

<span data-ttu-id="6bb15-109">Główny model hostingu dla Blazor jest uruchomiony po stronie klienta w przeglądarce w programie webassembly.</span><span class="sxs-lookup"><span data-stu-id="6bb15-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="6bb15-110">Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6bb15-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="6bb15-111">Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6bb15-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="6bb15-112">Aktualizacje interfejsu użytkownika i obsługa zdarzeń są wykonywane w ramach tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="6bb15-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="6bb15-113">Zasoby aplikacji są wdrażane jako pliki statyczne na serwerze sieci Web lub usłudze obsługującej zawartość statyczną dla klientów.</span><span class="sxs-lookup"><span data-stu-id="6bb15-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor webassembly: aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="6bb15-115">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu po stronie klienta, użyj szablonu **aplikacji Blazor webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="6bb15-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="6bb15-116">Po wybraniu szablonu **aplikacji Blazor webassembly** można skonfigurować aplikację do korzystania z zaplecza ASP.NET Core, zaznaczając pole wyboru **hostowane ASP.NET Core** (polecenie[dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="6bb15-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="6bb15-117">Aplikacja ASP.NET Core udostępnia klientom aplikację Blazor.</span><span class="sxs-lookup"><span data-stu-id="6bb15-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="6bb15-118">Aplikacja webassembly Blazor może współdziałać z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci Web lub [sygnalizującego](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="6bb15-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="6bb15-119">Szablony obejmują skrypt `blazor.webassembly.js`, który obsługuje:</span><span class="sxs-lookup"><span data-stu-id="6bb15-119">The templates include the `blazor.webassembly.js` script that handles:</span></span>

* <span data-ttu-id="6bb15-120">Pobieranie środowiska uruchomieniowego platformy .NET, aplikacji i zależności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="6bb15-121">Inicjowanie środowiska uruchomieniowego w celu uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="6bb15-122">Model hostingu zestawu webBlazor oferuje kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="6bb15-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="6bb15-123">Nie istnieje zależność po stronie serwera .NET.</span><span class="sxs-lookup"><span data-stu-id="6bb15-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="6bb15-124">Aplikacja jest w pełni funkcjonalna po pobraniu do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="6bb15-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="6bb15-125">Zasoby i możliwości klienta są w pełni wykorzystywane.</span><span class="sxs-lookup"><span data-stu-id="6bb15-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="6bb15-126">Zadania są Odciążone z serwera do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="6bb15-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="6bb15-127">Serwer sieci Web ASP.NET Core nie jest wymagany do hostowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="6bb15-128">Możliwe są scenariusze wdrażania bezserwerowego (na przykład obsługujące aplikację z sieci CDN).</span><span class="sxs-lookup"><span data-stu-id="6bb15-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="6bb15-129">Istnieją Downsides do hostingu Blazor webassembly:</span><span class="sxs-lookup"><span data-stu-id="6bb15-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="6bb15-130">Aplikacja jest ograniczona do możliwości przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6bb15-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="6bb15-131">Wymagany jest sprzęt i oprogramowanie klienta (na przykład obsługa zestawu webassembly).</span><span class="sxs-lookup"><span data-stu-id="6bb15-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="6bb15-132">Rozmiar pobieranych plików jest większy i ładowanie aplikacji trwa dłużej.</span><span class="sxs-lookup"><span data-stu-id="6bb15-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="6bb15-133">Środowisko uruchomieniowe platformy .NET i obsługa narzędzi są mniej dojrzałe.</span><span class="sxs-lookup"><span data-stu-id="6bb15-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="6bb15-134">Na przykład istnieją ograniczenia dotyczące obsługi [.NET Standard](/dotnet/standard/net-standard) i debugowania.</span><span class="sxs-lookup"><span data-stu-id="6bb15-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="6bb15-135">Serwer Blazor</span><span class="sxs-lookup"><span data-stu-id="6bb15-135">Blazor Server</span></span>

<span data-ttu-id="6bb15-136">W modelu hostingu serwera Blazor aplikacja jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6bb15-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="6bb15-137">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [sygnalizujące](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="6bb15-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Przeglądarka współdziała z aplikacją (hostowaną wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia sygnalizującego.](hosting-models/_static/blazor-server.png)

<span data-ttu-id="6bb15-139">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu serwera Blazor, użyj szablonu aplikacji ASP.NET Core **Blazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="6bb15-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="6bb15-140">Aplikacja ASP.NET Core hostuje aplikację serwera Blazor i tworzy punkt końcowy sygnalizujący, do którego klienci nawiązują połączenie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="6bb15-141">Aplikacja ASP.NET Core odwołuje się do klasy `Startup` aplikacji do dodania:</span><span class="sxs-lookup"><span data-stu-id="6bb15-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="6bb15-142">Usługi po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="6bb15-142">Server-side services.</span></span>
* <span data-ttu-id="6bb15-143">Aplikacja do potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="6bb15-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="6bb15-144">Skrypt `blazor.server.js`&dagger; nawiązuje połączenie z klientem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-144">The `blazor.server.js` script&dagger; establishes the client connection.</span></span> <span data-ttu-id="6bb15-145">Jest on odpowiedzialny za utrzymanie i przywrócenie stanu aplikacji zgodnie z wymaganiami (na przykład w przypadku utraconego połączenia sieciowego).</span><span class="sxs-lookup"><span data-stu-id="6bb15-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="6bb15-146">Model hostingu serwera Blazor oferuje kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="6bb15-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="6bb15-147">Rozmiar pobieranych plików jest znacznie mniejszy niż aplikacja Blazor webassembly, a aplikacja jest znacznie szybsza.</span><span class="sxs-lookup"><span data-stu-id="6bb15-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="6bb15-148">Aplikacja w pełni wykorzystuje możliwości serwera, w tym używanie dowolnego zgodnego z platformą .NET Core interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="6bb15-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="6bb15-149">Program .NET Core na serwerze jest używany do uruchamiania aplikacji, więc istniejące narzędzia platformy .NET, takie jak debugowanie, działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="6bb15-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="6bb15-150">Klienci zubożoni są obsługiwani.</span><span class="sxs-lookup"><span data-stu-id="6bb15-150">Thin clients are supported.</span></span> <span data-ttu-id="6bb15-151">Na przykład aplikacje serwera Blazor działają z przeglądarkami, które nie obsługują zestawu webassembly ani urządzeń z ograniczeniami zasobów.</span><span class="sxs-lookup"><span data-stu-id="6bb15-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="6bb15-152">Baza danych platformy .NET/C# kodu aplikacji, w tym kod składnika aplikacji, nie jest obsługiwana dla klientów.</span><span class="sxs-lookup"><span data-stu-id="6bb15-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="6bb15-153">Istnieją Downsides do hostingu serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="6bb15-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="6bb15-154">Wyższe opóźnienia zwykle istnieją.</span><span class="sxs-lookup"><span data-stu-id="6bb15-154">Higher latency usually exists.</span></span> <span data-ttu-id="6bb15-155">Każda interakcja użytkownika obejmuje przeskok sieci.</span><span class="sxs-lookup"><span data-stu-id="6bb15-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="6bb15-156">Brak obsługi offline.</span><span class="sxs-lookup"><span data-stu-id="6bb15-156">There's no offline support.</span></span> <span data-ttu-id="6bb15-157">Jeśli połączenie z klientem zakończy się niepowodzeniem, aplikacja przestanie działać.</span><span class="sxs-lookup"><span data-stu-id="6bb15-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="6bb15-158">Skalowalność jest wyzwaniem dla aplikacji z wieloma użytkownikami.</span><span class="sxs-lookup"><span data-stu-id="6bb15-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="6bb15-159">Serwer musi zarządzać wieloma połączeniami klientów i obsługiwać stan klienta.</span><span class="sxs-lookup"><span data-stu-id="6bb15-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="6bb15-160">Do obsłużynia aplikacji wymagany jest serwer ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6bb15-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="6bb15-161">Scenariusze wdrażania bez użycia serwera nie są możliwe (na przykład w celu obsługi aplikacji z sieci CDN).</span><span class="sxs-lookup"><span data-stu-id="6bb15-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="6bb15-162">&dagger;skrypt `blazor.server.js` jest obsługiwany z zasobów osadzonych w ASP.NET Core udostępnionej platformie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-162">&dagger;The `blazor.server.js` script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="6bb15-163">Porównanie z renderowanym przez serwer interfejsem użytkownika</span><span class="sxs-lookup"><span data-stu-id="6bb15-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="6bb15-164">Jednym ze sposobów zrozumienia aplikacji Blazor Server jest zrozumienie, jak różni się od tradycyjnych modeli na potrzeby renderowania interfejsu użytkownika w aplikacjach ASP.NET Core przy użyciu widoków Razor lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6bb15-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="6bb15-165">Oba modele używają języka Razor do opisywania zawartości HTML, ale znacząco różnią się sposobem renderowania znaczników.</span><span class="sxs-lookup"><span data-stu-id="6bb15-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="6bb15-166">Gdy strona Razor lub widok jest renderowany, każdy wiersz kodu Razor emituje kod HTML w postaci tekstowej.</span><span class="sxs-lookup"><span data-stu-id="6bb15-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="6bb15-167">Po wyrenderowaniu serwer usuwa wystąpienie strony lub widoku, w tym dowolny utworzony stan.</span><span class="sxs-lookup"><span data-stu-id="6bb15-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="6bb15-168">Gdy występuje inne żądanie dotyczące strony, na przykład w przypadku niepowodzenia walidacji serwera i wyświetlenia podsumowania walidacji:</span><span class="sxs-lookup"><span data-stu-id="6bb15-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="6bb15-169">Cała strona zostanie ponownie przerenderowana na tekst HTML.</span><span class="sxs-lookup"><span data-stu-id="6bb15-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="6bb15-170">Strona jest wysyłana do klienta.</span><span class="sxs-lookup"><span data-stu-id="6bb15-170">The page is sent to the client.</span></span>

<span data-ttu-id="6bb15-171">Aplikacja Blazor składa się z elementów wielokrotnego użytku interfejsu użytkownika o nazwie *Components*.</span><span class="sxs-lookup"><span data-stu-id="6bb15-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="6bb15-172">Składnik zawiera C# kod, znacznik i inne składniki.</span><span class="sxs-lookup"><span data-stu-id="6bb15-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="6bb15-173">Gdy składnik jest renderowany, Blazor tworzy wykres dołączonych składników podobny do Document Object Model HTML lub XML (DOM).</span><span class="sxs-lookup"><span data-stu-id="6bb15-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="6bb15-174">Ten wykres zawiera stan składnika przechowywany w właściwościach i polach.</span><span class="sxs-lookup"><span data-stu-id="6bb15-174">This graph includes component state held in properties and fields.</span></span> <span data-ttu-id="6bb15-175">Blazor oblicza wykres składnika, aby utworzyć binarną reprezentację znaczników.</span><span class="sxs-lookup"><span data-stu-id="6bb15-175">Blazor evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="6bb15-176">Format binarny może:</span><span class="sxs-lookup"><span data-stu-id="6bb15-176">The binary format can be:</span></span>

* <span data-ttu-id="6bb15-177">Włączono tekst HTML (podczas renderowania prerenderingu).</span><span class="sxs-lookup"><span data-stu-id="6bb15-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="6bb15-178">Służy do wydajnej aktualizacji znaczników podczas normalnego renderowania.</span><span class="sxs-lookup"><span data-stu-id="6bb15-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="6bb15-179">Aktualizacja interfejsu użytkownika w Blazor jest wyzwalana przez:</span><span class="sxs-lookup"><span data-stu-id="6bb15-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="6bb15-180">Interakcja z użytkownikiem, na przykład wybranie przycisku.</span><span class="sxs-lookup"><span data-stu-id="6bb15-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="6bb15-181">Wyzwalacze aplikacji, takie jak czasomierz.</span><span class="sxs-lookup"><span data-stu-id="6bb15-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="6bb15-182">Wykres jest ponownie renderowany i obliczana *jest różnica między interfejsami* użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6bb15-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="6bb15-183">Różnica ta jest najmniejszym zestawem zmian modelu DOM wymaganym do zaktualizowania interfejsu użytkownika na kliencie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="6bb15-184">Różnica jest wysyłana do klienta w formacie binarnym i stosowana przez przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="6bb15-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="6bb15-185">Składnik jest usuwany po przejściu przez użytkownika na klienta.</span><span class="sxs-lookup"><span data-stu-id="6bb15-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="6bb15-186">Gdy użytkownik korzysta ze składnika, stan składnika (usługi, zasoby) musi być przechowywany w pamięci serwera.</span><span class="sxs-lookup"><span data-stu-id="6bb15-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="6bb15-187">Ponieważ stan wielu składników może być obsługiwany przez serwer współbieżnie, wyczerpanie pamięci jest problemem, który należy rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="6bb15-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="6bb15-188">Aby uzyskać wskazówki dotyczące sposobu tworzenia aplikacji serwera Blazor w celu zapewnienia najlepszego wykorzystania pamięci serwera, zobacz <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="6bb15-189">Integrowanie składników Razor z aplikacjami Razor Pages i MVC</span><span class="sxs-lookup"><span data-stu-id="6bb15-189">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="6bb15-190">Używanie składników na stronach i widokach</span><span class="sxs-lookup"><span data-stu-id="6bb15-190">Use components in pages and views</span></span>

<span data-ttu-id="6bb15-191">Istniejąca aplikacja Razor Pages lub MVC może zintegrować składniki Razor ze stronami i widokami:</span><span class="sxs-lookup"><span data-stu-id="6bb15-191">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="6bb15-192">W pliku układu aplikacji ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="6bb15-192">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="6bb15-193">Dodaj następujący tag `<base>` do elementu `<head>`:</span><span class="sxs-lookup"><span data-stu-id="6bb15-193">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="6bb15-194">Wartość `href` ( *Ścieżka podstawowa aplikacji*) w poprzednim przykładzie założono, że aplikacja znajduje się w ścieżce adresu URL katalogu głównego (`/`).</span><span class="sxs-lookup"><span data-stu-id="6bb15-194">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="6bb15-195">Jeśli aplikacja jest aplikacją podrzędną, postępuj zgodnie ze wskazówkami w sekcji *Ścieżka podstawowa aplikacji* w artykule <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-195">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="6bb15-196">Plik *_Layout. cshtml* znajduje się w folderze *Pages/shared* w aplikacji Razor Pages lub *widokach/folderze udostępnionym* w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="6bb15-196">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="6bb15-197">Dodaj tag `<script>` dla skryptu *blazor. Server. js* wewnątrz tagu zamykającego `</body>`:</span><span class="sxs-lookup"><span data-stu-id="6bb15-197">Add a `<script>` tag for the *blazor.server.js* script inside of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="6bb15-198">Struktura dodaje skrypt *blazor. Server. js* do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-198">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="6bb15-199">Nie trzeba ręcznie dodawać skryptu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-199">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="6bb15-200">Dodaj plik *_Imports. Razor* do folderu głównego projektu o następującej zawartości (Zmień ostatnią przestrzeń nazw, `MyAppNamespace`, na przestrzeń nazw aplikacji):</span><span class="sxs-lookup"><span data-stu-id="6bb15-200">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="6bb15-201">W `Startup.ConfigureServices`Dodaj usługę serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="6bb15-201">In `Startup.ConfigureServices`, add the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="6bb15-202">W `Startup.Configure`Dodaj punkt końcowy centrum Blazor do `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="6bb15-202">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="6bb15-203">Integruj składniki na dowolną stronę lub widok.</span><span class="sxs-lookup"><span data-stu-id="6bb15-203">Integrate components into any page or view.</span></span> <span data-ttu-id="6bb15-204">Aby uzyskać więcej informacji, zobacz sekcję *integrowanie składników w Razor Pages i aplikacje MVC* w artykule <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-204">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="6bb15-205">Używanie składników rutowanych w aplikacji Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6bb15-205">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="6bb15-206">Aby obsługiwać Routing składników Razor w aplikacjach Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="6bb15-206">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="6bb15-207">Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Używanie składników w stronach i widokach](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="6bb15-207">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="6bb15-208">Dodaj plik *App. Razor* do katalogu głównego projektu z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="6bb15-208">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="6bb15-209">Dodaj plik *_Host. cshtml* do folderu *stron* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="6bb15-209">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="6bb15-210">Składniki używają udostępnionego pliku *_Layout. cshtml* dla ich układu.</span><span class="sxs-lookup"><span data-stu-id="6bb15-210">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="6bb15-211">Dodaj trasę o niskim priorytecie dla strony *_Host. cshtml* do konfiguracji punktu końcowego w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6bb15-211">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="6bb15-212">Dodaj składniki routingu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-212">Add routable components to the app.</span></span> <span data-ttu-id="6bb15-213">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6bb15-213">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="6bb15-214">W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do pliku *Pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="6bb15-214">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="6bb15-215">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-215">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="6bb15-216">Używanie składników rutowanych w aplikacji MVC</span><span class="sxs-lookup"><span data-stu-id="6bb15-216">Use routable components in an MVC app</span></span>

<span data-ttu-id="6bb15-217">Aby zapewnić obsługę routingu składników Razor w aplikacjach MVC:</span><span class="sxs-lookup"><span data-stu-id="6bb15-217">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="6bb15-218">Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Używanie składników w stronach i widokach](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="6bb15-218">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="6bb15-219">Dodaj plik *App. Razor* do katalogu głównego projektu z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="6bb15-219">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="6bb15-220">Dodaj plik *_Host. cshtml* do folderu *widoki/główne* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="6bb15-220">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="6bb15-221">Składniki używają udostępnionego pliku *_Layout. cshtml* dla ich układu.</span><span class="sxs-lookup"><span data-stu-id="6bb15-221">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="6bb15-222">Dodaj akcję do kontrolera macierzystego:</span><span class="sxs-lookup"><span data-stu-id="6bb15-222">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="6bb15-223">Dodaj trasę o niskim priorytecie dla akcji kontrolera, która zwraca widok *_Host. cshtml* do konfiguracji punktu końcowego w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="6bb15-223">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="6bb15-224">Utwórz folder *strony* i Dodaj składniki do obsługi routingu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-224">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="6bb15-225">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6bb15-225">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="6bb15-226">W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do pliku *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="6bb15-226">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="6bb15-227">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-227">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="circuits"></a><span data-ttu-id="6bb15-228">Elektrycznych</span><span class="sxs-lookup"><span data-stu-id="6bb15-228">Circuits</span></span>

<span data-ttu-id="6bb15-229">Aplikacja serwera Blazor jest tworzona na podstawie [sygnału ASP.NET Core](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="6bb15-229">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="6bb15-230">Każdy klient komunikuje się z serwerem za pośrednictwem co najmniej jednego połączenia sygnalizującego zwanego *obwodem*.</span><span class="sxs-lookup"><span data-stu-id="6bb15-230">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="6bb15-231">Obwód jest abstrakcją Blazor za pośrednictwem połączeń sygnalizujących, które mogą tolerować tymczasowe przerwy w działaniu sieci.</span><span class="sxs-lookup"><span data-stu-id="6bb15-231">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="6bb15-232">Gdy klient Blazor widzi, że połączenie sygnalizujące jest odłączone, próbuje ponownie nawiązać połączenie z serwerem przy użyciu nowego połączenia sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="6bb15-232">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="6bb15-233">Każdy ekran przeglądarki (karta przeglądarki lub iframe) połączony z aplikacją serwera Blazor korzysta z połączenia sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="6bb15-233">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="6bb15-234">Jest to jeszcze inna ważna różnica w porównaniu z typowymi aplikacjami renderowanymi przez serwer.</span><span class="sxs-lookup"><span data-stu-id="6bb15-234">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="6bb15-235">W aplikacji renderowanej na serwerze otwieranie tej samej aplikacji na wielu ekranach przeglądarki zazwyczaj nie jest przeważnie uwzględniane w dodatkowych wymaganiach dotyczących zasobów na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6bb15-235">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="6bb15-236">W aplikacji serwera Blazor każdy ekran przeglądarki wymaga oddzielnego obwodu i oddzielnych wystąpień stanu składnika, które mają być zarządzane przez serwer programu.</span><span class="sxs-lookup"><span data-stu-id="6bb15-236">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

<span data-ttu-id="6bb15-237">Blazor uważa *, że zamyka* kartę przeglądarki lub przechodzenie do zewnętrznego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6bb15-237">Blazor considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="6bb15-238">W przypadku bezpiecznego zakończenia obwód i skojarzone zasoby są natychmiast uwalniane.</span><span class="sxs-lookup"><span data-stu-id="6bb15-238">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="6bb15-239">Klient może również odłączyć się niebezpiecznie, na przykład z powodu przerwy w działaniu sieci.</span><span class="sxs-lookup"><span data-stu-id="6bb15-239">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> <span data-ttu-id="6bb15-240">Serwer Blazor przechowuje rozłączone obwody przez konfigurowalny interwał, aby umożliwić klientowi Ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="6bb15-240">Blazor Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="6bb15-241">Aby uzyskać więcej informacji, zobacz sekcję [ponowne łączenie z tym samym serwerem](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="6bb15-241">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="6bb15-242">Opóźnienie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="6bb15-242">UI Latency</span></span>

<span data-ttu-id="6bb15-243">Opóźnienie interfejsu użytkownika to czas od zainicjowanej akcji do momentu zaktualizowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6bb15-243">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="6bb15-244">Mniejsze wartości opóźnienia interfejsu użytkownika są bezwzględnie konieczne, aby aplikacja mogła reagować na użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6bb15-244">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="6bb15-245">W aplikacji serwera Blazor każda akcja jest wysyłana do serwera, przetwarzane i różnic interfejsu użytkownika jest wysyłana z powrotem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-245">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="6bb15-246">W związku z tym opóźnienia interfejsu użytkownika to suma opóźnień sieci i opóźnienia serwera w przetwarzaniu akcji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-246">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="6bb15-247">W przypadku aplikacji biznesowych, która jest ograniczona do prywatnej sieci firmowej, wpływ na postrzeganie opóźnień przez użytkownika z powodu opóźnienia sieci jest zwykle niezauważalny.</span><span class="sxs-lookup"><span data-stu-id="6bb15-247">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="6bb15-248">W przypadku aplikacji wdrożonej za pośrednictwem Internetu opóźnienie może być zauważalne dla użytkowników, szczególnie w przypadku, gdy użytkownicy są szeroko rozproszona geograficznie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-248">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="6bb15-249">Użycie pamięci może również przyczynić się do opóźnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-249">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="6bb15-250">Zwiększone użycie pamięci powoduje częste zbieranie elementów bezużytecznych lub stronicowanie pamięci na dysku, co zmniejsza wydajność aplikacji i w związku z tym zwiększa opóźnienia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6bb15-250">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="6bb15-251">Aby uzyskać więcej informacji, zobacz temat <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-251">For more information, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="6bb15-252">Aplikacje serwera Blazor powinny być zoptymalizowane w celu zminimalizowania opóźnień interfejsu użytkownika przez zmniejszenie opóźnienia sieci i użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="6bb15-252">Blazor Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="6bb15-253">Aby uzyskać podejście do mierzenia opóźnień sieci, zobacz <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-253">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="6bb15-254">Aby uzyskać więcej informacji na temat sygnałów i Blazor, zobacz:</span><span class="sxs-lookup"><span data-stu-id="6bb15-254">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="6bb15-255">Połączenie z serwerem</span><span class="sxs-lookup"><span data-stu-id="6bb15-255">Connection to the server</span></span>

<span data-ttu-id="6bb15-256">Aplikacje serwera Blazor wymagają aktywnego połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-256">Blazor Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="6bb15-257">Jeśli połączenie zostanie utracone, aplikacja spróbuje ponownie nawiązać połączenie z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-257">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="6bb15-258">O ile stan klienta nadal znajduje się w pamięci, sesja klienta zostaje wznowiona bez utraty stanu.</span><span class="sxs-lookup"><span data-stu-id="6bb15-258">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="6bb15-259">Ponowne nawiązywanie połączenia z tym samym serwerem</span><span class="sxs-lookup"><span data-stu-id="6bb15-259">Reconnection to the same server</span></span>

<span data-ttu-id="6bb15-260">Aplikacja serwera Blazor jest przedstawiona w odpowiedzi na pierwsze żądanie klienta, która konfiguruje stan interfejsu użytkownika na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6bb15-260">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="6bb15-261">Gdy klient próbuje utworzyć połączenie sygnalizujące, klient musi ponownie nawiązać połączenie z tym samym serwerem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-261">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> <span data-ttu-id="6bb15-262">Aplikacje serwera Blazor korzystające z więcej niż jednego serwera wewnętrznej bazy danych powinny implementować *sesje usługi Sticky Notes* dla połączeń sygnałów.</span><span class="sxs-lookup"><span data-stu-id="6bb15-262">Blazor Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="6bb15-263">Zalecamy korzystanie z [usługi Azure Signal Service](/azure/azure-signalr) dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="6bb15-263">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="6bb15-264">Usługa umożliwia skalowanie aplikacji serwera Blazor na dużą liczbę współbieżnych połączeń sygnałów.</span><span class="sxs-lookup"><span data-stu-id="6bb15-264">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="6bb15-265">Sesje programu Sticky Notes są włączone dla usługi Azure Signal, ustawiając opcję `ServerStickyMode` usługi lub wartość konfiguracji na `Required`.</span><span class="sxs-lookup"><span data-stu-id="6bb15-265">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="6bb15-266">Aby uzyskać więcej informacji, zobacz temat <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="6bb15-266">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="6bb15-267">W przypadku korzystania z usług IIS sesje programu Sticky są włączane przy użyciu routingu żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-267">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="6bb15-268">Aby uzyskać więcej informacji, zobacz [równoważenie obciążenia HTTP przy użyciu routingu żądań aplikacji](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="6bb15-268">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="6bb15-269">Odzwierciedlanie stanu połączenia w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="6bb15-269">Reflect the connection state in the UI</span></span>

<span data-ttu-id="6bb15-270">Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-270">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="6bb15-271">Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby.</span><span class="sxs-lookup"><span data-stu-id="6bb15-271">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="6bb15-272">Aby dostosować interfejs użytkownika, zdefiniuj element z `id` `components-reconnect-modal` w `<body>` na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="6bb15-272">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="6bb15-273">W poniższej tabeli opisano klasy CSS stosowane do elementu `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="6bb15-273">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="6bb15-274">Klasa CSS</span><span class="sxs-lookup"><span data-stu-id="6bb15-274">CSS class</span></span>                       | <span data-ttu-id="6bb15-275">Wskazuje&hellip;</span><span class="sxs-lookup"><span data-stu-id="6bb15-275">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="6bb15-276">Utracono połączenie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-276">A lost connection.</span></span> <span data-ttu-id="6bb15-277">Klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-277">The client is attempting to reconnect.</span></span> <span data-ttu-id="6bb15-278">Pokaż modalne.</span><span class="sxs-lookup"><span data-stu-id="6bb15-278">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="6bb15-279">Aktywne połączenie zostanie ponownie nawiązane z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-279">An active connection is re-established to the server.</span></span> <span data-ttu-id="6bb15-280">Ukryj modalne.</span><span class="sxs-lookup"><span data-stu-id="6bb15-280">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="6bb15-281">Ponowne połączenie nie powiodło się, prawdopodobnie z powodu błędu sieci.</span><span class="sxs-lookup"><span data-stu-id="6bb15-281">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="6bb15-282">Aby spróbować ponownie nawiązać połączenie, wywołaj `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="6bb15-282">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="6bb15-283">Odrzucono ponowne połączenie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-283">Reconnection rejected.</span></span> <span data-ttu-id="6bb15-284">Serwer został osiągnięty, ale odmówił połączenia, a stan użytkownika na serwerze został utracony.</span><span class="sxs-lookup"><span data-stu-id="6bb15-284">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="6bb15-285">Aby ponownie załadować aplikację, wywołaj `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="6bb15-285">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="6bb15-286">Ten stan połączenia może skutkować tym, że:</span><span class="sxs-lookup"><span data-stu-id="6bb15-286">This connection state may result when:</span></span><ul><li><span data-ttu-id="6bb15-287">Wystąpił awaria w obwodzie po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="6bb15-287">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="6bb15-288">Klient jest odłączony wystarczająco długo, aby serwer mógł porzucić stan użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6bb15-288">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="6bb15-289">Wystąpienia składników, z którymi łączy się użytkownik, są usuwane.</span><span class="sxs-lookup"><span data-stu-id="6bb15-289">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="6bb15-290">Serwer zostanie uruchomiony ponownie lub proces roboczy aplikacji zostanie odtworzony.</span><span class="sxs-lookup"><span data-stu-id="6bb15-290">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="6bb15-291">Stanowe Ponowne nawiązywanie połączenia po przeprowadzeniu prerenderowania</span><span class="sxs-lookup"><span data-stu-id="6bb15-291">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="6bb15-292">Aplikacje serwera Blazor są domyślnie skonfigurowane, aby skonfigurować interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-292">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="6bb15-293">Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="6bb15-293">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="6bb15-294">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="6bb15-294">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="6bb15-295">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-295">Is prerendered into the page.</span></span>
* <span data-ttu-id="6bb15-296">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6bb15-296">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="6bb15-297">Opis</span><span class="sxs-lookup"><span data-stu-id="6bb15-297">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="6bb15-298">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="6bb15-298">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="6bb15-299">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="6bb15-299">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="6bb15-300">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="6bb15-300">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="6bb15-301">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="6bb15-301">Output from the component isn't included.</span></span> <span data-ttu-id="6bb15-302">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="6bb15-302">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="6bb15-303">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="6bb15-303">Renders the component into static HTML.</span></span> |

<span data-ttu-id="6bb15-304">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6bb15-304">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="6bb15-305">Gdy `RenderMode` jest `ServerPrerendered`, składnik jest początkowo renderowany statycznie jako część strony.</span><span class="sxs-lookup"><span data-stu-id="6bb15-305">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="6bb15-306">Gdy przeglądarka nawiąże połączenie z serwerem, składnik jest renderowany *ponownie*, a składnik jest teraz interaktywny.</span><span class="sxs-lookup"><span data-stu-id="6bb15-306">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="6bb15-307">Jeśli istnieje metoda cyklu życia " [OnInitialized {Async}](xref:blazor/lifecycle#component-initialization-methods) " dla inicjowania składnika, metoda jest wykonywana *dwukrotnie*:</span><span class="sxs-lookup"><span data-stu-id="6bb15-307">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="6bb15-308">Gdy składnik jest wstępnie renderowany statycznie.</span><span class="sxs-lookup"><span data-stu-id="6bb15-308">When the component is prerendered statically.</span></span>
* <span data-ttu-id="6bb15-309">Po nawiązaniu połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-309">After the server connection has been established.</span></span>

<span data-ttu-id="6bb15-310">Może to spowodować zauważalną zmianę danych wyświetlanych w interfejsie użytkownika, gdy składnik jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="6bb15-310">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="6bb15-311">Aby uniknąć podwójnego renderowania w aplikacji serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="6bb15-311">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="6bb15-312">Przekaż identyfikator, który może służyć do buforowania stanu podczas wykonywania prerenderowania i pobierania stanu po ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6bb15-312">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="6bb15-313">Użyj identyfikatora podczas renderowania, aby zapisać stan składnika.</span><span class="sxs-lookup"><span data-stu-id="6bb15-313">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="6bb15-314">Użyj identyfikatora po włączeniu, aby pobrać buforowany stan.</span><span class="sxs-lookup"><span data-stu-id="6bb15-314">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="6bb15-315">Poniższy kod ilustruje zaktualizowany `WeatherForecastService` w aplikacji serwerowej Blazor opartej na szablonach, która pozwala uniknąć podwójnego renderowania:</span><span class="sxs-lookup"><span data-stu-id="6bb15-315">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="6bb15-316">Renderuj stanowe składniki interaktywne ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="6bb15-316">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="6bb15-317">Można dodać składniki interaktywne ze stanem do strony lub widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="6bb15-317">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="6bb15-318">Gdy renderuje stronę lub widok:</span><span class="sxs-lookup"><span data-stu-id="6bb15-318">When the page or view renders:</span></span>

* <span data-ttu-id="6bb15-319">Składnik jest wstępnie renderowany przy użyciu strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="6bb15-319">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="6bb15-320">Początkowy stan składnika używany na potrzeby renderowania wstępnego został utracony.</span><span class="sxs-lookup"><span data-stu-id="6bb15-320">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="6bb15-321">Nowy stan składnika jest tworzony po nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="6bb15-321">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="6bb15-322">Następująca strona Razor renderuje składnik `Counter`:</span><span class="sxs-lookup"><span data-stu-id="6bb15-322">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="6bb15-323">Renderuj nieinteraktywne składniki ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="6bb15-323">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="6bb15-324">Na poniższej stronie Razor składnik `Counter` jest renderowany statycznie z wartością początkową określoną przy użyciu formularza:</span><span class="sxs-lookup"><span data-stu-id="6bb15-324">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="6bb15-325">Ponieważ `MyComponent` jest renderowany statycznie, składnik nie może być interaktywny.</span><span class="sxs-lookup"><span data-stu-id="6bb15-325">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="6bb15-326">Wykryj, kiedy aplikacja jest przedrenderowana</span><span class="sxs-lookup"><span data-stu-id="6bb15-326">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="6bb15-327">Konfigurowanie klienta SignalR dla aplikacji Blazor Server</span><span class="sxs-lookup"><span data-stu-id="6bb15-327">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="6bb15-328">Czasami trzeba skonfigurować klienta SignalR używanego przez aplikacje Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="6bb15-328">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="6bb15-329">Na przykład możesz chcieć skonfigurować rejestrowanie na kliencie SignalR, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="6bb15-329">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="6bb15-330">Aby skonfigurować klienta SignalR w pliku *Pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6bb15-330">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="6bb15-331">Dodaj atrybut `autostart="false"` do tagu `<script>` dla skryptu `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="6bb15-331">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="6bb15-332">Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa SignalR konstruktora.</span><span class="sxs-lookup"><span data-stu-id="6bb15-332">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="6bb15-333">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6bb15-333">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
