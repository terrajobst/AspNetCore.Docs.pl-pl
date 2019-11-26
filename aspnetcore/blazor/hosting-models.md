---
title: ASP.NET Core Blazor modele hostingowe
author: guardrex
description: Informacje na temat Blazor modeli hostingu i Blazor Server.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: a017737eacd93ac776afe7ee8024eed602d7edcc
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317228"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="05135-103">ASP.NET Core Blazor modele hostingowe</span><span class="sxs-lookup"><span data-stu-id="05135-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="05135-104">Autor [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="05135-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="05135-105"> to platforma internetowa przeznaczona do uruchamiania po stronie klienta w przeglądarce w środowisku uruchomieniowym .NET runtime ( *Blazor webassembly*) lub po stronie serwera w ASP.NET Core ( [](https://webassembly.org/) *Blazor Server*).</span><span class="sxs-lookup"><span data-stu-id="05135-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="05135-106">Niezależnie od modelu hostingu modele aplikacji i składników *są takie same*.</span><span class="sxs-lookup"><span data-stu-id="05135-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="05135-107">Aby utworzyć projekt dla modeli hostingu opisanych w tym artykule, zobacz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="05135-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="05135-108"> webassembly</span><span class="sxs-lookup"><span data-stu-id="05135-108"> WebAssembly</span></span>

<span data-ttu-id="05135-109">Główny model hostingu dla Blazor jest uruchamiany po stronie klienta w przeglądarce w zestawie webassembly.</span><span class="sxs-lookup"><span data-stu-id="05135-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="05135-110">Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="05135-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="05135-111">Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="05135-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="05135-112">Aktualizacje interfejsu użytkownika i obsługa zdarzeń są wykonywane w ramach tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="05135-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="05135-113">Zasoby aplikacji są wdrażane jako pliki statyczne na serwerze sieci Web lub usłudze obsługującej zawartość statyczną dla klientów.</span><span class="sxs-lookup"><span data-stu-id="05135-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! OP. NO-LOC (Blazor)]<span data-ttu-id="05135-114"> webassembly: [! OP. Aplikacja NO-LOC (Blazor)] jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="05135-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="05135-115">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu po stronie klienta, użyj szablonu **aplikacjiBlazor webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="05135-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="05135-116">Po wybraniu szablonu **aplikacjiBlazor webassembly** można skonfigurować aplikację do korzystania z zaplecza ASP.NET Core, zaznaczając pole wyboru **hostowane w ASP.NET Core** ([Nowy blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="05135-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="05135-117">Aplikacja ASP.NET Core obsługuje klientów Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05135-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="05135-118">Aplikacja webassembly Blazor może współdziałać z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci Web lub [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="05135-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="05135-119">Szablony obejmują skrypt *blazor. webassembly. js* , który obsługuje:</span><span class="sxs-lookup"><span data-stu-id="05135-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="05135-120">Pobieranie środowiska uruchomieniowego platformy .NET, aplikacji i zależności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05135-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="05135-121">Inicjowanie środowiska uruchomieniowego w celu uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05135-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="05135-122">Model hostingu Blazor webassembly oferuje kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="05135-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="05135-123">Nie istnieje zależność po stronie serwera .NET.</span><span class="sxs-lookup"><span data-stu-id="05135-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="05135-124">Aplikacja jest w pełni funkcjonalna po pobraniu do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="05135-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="05135-125">Zasoby i możliwości klienta są w pełni wykorzystywane.</span><span class="sxs-lookup"><span data-stu-id="05135-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="05135-126">Zadania są Odciążone z serwera do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="05135-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="05135-127">Serwer sieci Web ASP.NET Core nie jest wymagany do hostowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05135-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="05135-128">Możliwe są scenariusze wdrażania bezserwerowego (na przykład obsługujące aplikację z sieci CDN).</span><span class="sxs-lookup"><span data-stu-id="05135-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="05135-129">Istnieją downsides Blazor hostingu zestawu webassembly:</span><span class="sxs-lookup"><span data-stu-id="05135-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="05135-130">Aplikacja jest ograniczona do możliwości przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="05135-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="05135-131">Wymagany jest sprzęt i oprogramowanie klienta (na przykład obsługa zestawu webassembly).</span><span class="sxs-lookup"><span data-stu-id="05135-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="05135-132">Rozmiar pobieranych plików jest większy i ładowanie aplikacji trwa dłużej.</span><span class="sxs-lookup"><span data-stu-id="05135-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="05135-133">Środowisko uruchomieniowe platformy .NET i obsługa narzędzi są mniej dojrzałe.</span><span class="sxs-lookup"><span data-stu-id="05135-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="05135-134">Na przykład istnieją ograniczenia dotyczące obsługi [.NET Standard](/dotnet/standard/net-standard) i debugowania.</span><span class="sxs-lookup"><span data-stu-id="05135-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a><span data-ttu-id="05135-135">Serwer Blazor</span><span class="sxs-lookup"><span data-stu-id="05135-135">Blazor Server</span></span>

<span data-ttu-id="05135-136">W modelu hostingu serwera Blazor aplikacja jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05135-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="05135-137">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="05135-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Przeglądarka współdziała z aplikacją (hostowaną wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem [! OP. Nie-LOC (sygnalizujący)] połączenie.](hosting-models/_static/blazor-server.png)

<span data-ttu-id="05135-139">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu Blazor Server, użyj szablonu aplikacji ASP.NET Core **Blazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="05135-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="05135-140">Aplikacja ASP.NET Core obsługuje aplikację serwera Blazor i tworzy punkt końcowy SignalR, w którym klienci nawiązują połączenie.</span><span class="sxs-lookup"><span data-stu-id="05135-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="05135-141">Aplikacja ASP.NET Core odwołuje się do klasy `Startup` aplikacji do dodania:</span><span class="sxs-lookup"><span data-stu-id="05135-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="05135-142">Usługi po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="05135-142">Server-side services.</span></span>
* <span data-ttu-id="05135-143">Aplikacja do potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="05135-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="05135-144">Skrypt *blazor. Server. js*&dagger; nawiązuje połączenie z klientem.</span><span class="sxs-lookup"><span data-stu-id="05135-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="05135-145">Jest on odpowiedzialny za utrzymanie i przywrócenie stanu aplikacji zgodnie z wymaganiami (na przykład w przypadku utraconego połączenia sieciowego).</span><span class="sxs-lookup"><span data-stu-id="05135-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="05135-146">Model hostingu serwera Blazor oferuje kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="05135-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="05135-147">Rozmiar pobieranych plików jest znacznie mniejszy niż Blazor aplikacji sieci webassembly, a aplikacja jest znacznie szybsza.</span><span class="sxs-lookup"><span data-stu-id="05135-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="05135-148">Aplikacja w pełni wykorzystuje możliwości serwera, w tym używanie dowolnego zgodnego z platformą .NET Core interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="05135-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="05135-149">Program .NET Core na serwerze jest używany do uruchamiania aplikacji, więc istniejące narzędzia platformy .NET, takie jak debugowanie, działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="05135-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="05135-150">Klienci zubożoni są obsługiwani.</span><span class="sxs-lookup"><span data-stu-id="05135-150">Thin clients are supported.</span></span> <span data-ttu-id="05135-151">Na przykład Blazor aplikacje serwera współpracują z przeglądarkami, które nie obsługują elementu webassembly i urządzeń z ograniczeniami zasobów.</span><span class="sxs-lookup"><span data-stu-id="05135-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="05135-152">Baza danych platformy .NET/C# kodu aplikacji, w tym kod składnika aplikacji, nie jest obsługiwana dla klientów.</span><span class="sxs-lookup"><span data-stu-id="05135-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="05135-153">Istnieją downsides Blazor hostingu serwera:</span><span class="sxs-lookup"><span data-stu-id="05135-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="05135-154">Wyższe opóźnienia zwykle istnieją.</span><span class="sxs-lookup"><span data-stu-id="05135-154">Higher latency usually exists.</span></span> <span data-ttu-id="05135-155">Każda interakcja użytkownika obejmuje przeskok sieci.</span><span class="sxs-lookup"><span data-stu-id="05135-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="05135-156">Brak obsługi offline.</span><span class="sxs-lookup"><span data-stu-id="05135-156">There's no offline support.</span></span> <span data-ttu-id="05135-157">Jeśli połączenie z klientem zakończy się niepowodzeniem, aplikacja przestanie działać.</span><span class="sxs-lookup"><span data-stu-id="05135-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="05135-158">Skalowalność jest wyzwaniem dla aplikacji z wieloma użytkownikami.</span><span class="sxs-lookup"><span data-stu-id="05135-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="05135-159">Serwer musi zarządzać wieloma połączeniami klientów i obsługiwać stan klienta.</span><span class="sxs-lookup"><span data-stu-id="05135-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="05135-160">Do obsłużynia aplikacji wymagany jest serwer ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05135-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="05135-161">Scenariusze wdrażania bez użycia serwera nie są możliwe (na przykład w celu obsługi aplikacji z sieci CDN).</span><span class="sxs-lookup"><span data-stu-id="05135-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="05135-162">&dagger;skrypt *blazor. Server. js* jest obsługiwany z zasobów osadzonych w ASP.NET Core współdzielonej platformie.</span><span class="sxs-lookup"><span data-stu-id="05135-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="05135-163">Porównanie z renderowanym przez serwer interfejsem użytkownika</span><span class="sxs-lookup"><span data-stu-id="05135-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="05135-164">Jednym ze sposobów zrozumienia Blazor aplikacji serwerowych jest zrozumienie, jak różni się od tradycyjnych modeli służących do renderowania interfejsu użytkownika w aplikacjach ASP.NET Core przy użyciu widoków Razor lub Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05135-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="05135-165">Oba modele używają języka Razor do opisywania zawartości HTML, ale znacząco różnią się sposobem renderowania znaczników.</span><span class="sxs-lookup"><span data-stu-id="05135-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="05135-166">Gdy strona Razor lub widok jest renderowany, każdy wiersz kodu Razor emituje kod HTML w postaci tekstowej.</span><span class="sxs-lookup"><span data-stu-id="05135-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="05135-167">Po wyrenderowaniu serwer usuwa wystąpienie strony lub widoku, w tym dowolny utworzony stan.</span><span class="sxs-lookup"><span data-stu-id="05135-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="05135-168">Gdy występuje inne żądanie dotyczące strony, na przykład w przypadku niepowodzenia walidacji serwera i wyświetlenia podsumowania walidacji:</span><span class="sxs-lookup"><span data-stu-id="05135-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="05135-169">Cała strona zostanie ponownie przerenderowana na tekst HTML.</span><span class="sxs-lookup"><span data-stu-id="05135-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="05135-170">Strona jest wysyłana do klienta.</span><span class="sxs-lookup"><span data-stu-id="05135-170">The page is sent to the client.</span></span>

<span data-ttu-id="05135-171">Aplikacja Blazor składa się z elementów wielokrotnego użytku interfejsu użytkownika o nazwie *Components*.</span><span class="sxs-lookup"><span data-stu-id="05135-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="05135-172">Składnik zawiera C# kod, znacznik i inne składniki.</span><span class="sxs-lookup"><span data-stu-id="05135-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="05135-173">Gdy składnik jest renderowany, Blazor generuje wykres dołączonych składników podobne do Document Object Model HTML lub XML (DOM).</span><span class="sxs-lookup"><span data-stu-id="05135-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="05135-174">Ten wykres zawiera stan składnika przechowywany w właściwościach i polach.</span><span class="sxs-lookup"><span data-stu-id="05135-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="05135-175"> oblicza wykres składnika, aby utworzyć binarną reprezentację znaczników.</span><span class="sxs-lookup"><span data-stu-id="05135-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="05135-176">Format binarny może:</span><span class="sxs-lookup"><span data-stu-id="05135-176">The binary format can be:</span></span>

* <span data-ttu-id="05135-177">Włączono tekst HTML (podczas renderowania prerenderingu).</span><span class="sxs-lookup"><span data-stu-id="05135-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="05135-178">Służy do wydajnej aktualizacji znaczników podczas normalnego renderowania.</span><span class="sxs-lookup"><span data-stu-id="05135-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="05135-179">Aktualizacja interfejsu użytkownika w Blazor jest wyzwalana przez:</span><span class="sxs-lookup"><span data-stu-id="05135-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="05135-180">Interakcja z użytkownikiem, na przykład wybranie przycisku.</span><span class="sxs-lookup"><span data-stu-id="05135-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="05135-181">Wyzwalacze aplikacji, takie jak czasomierz.</span><span class="sxs-lookup"><span data-stu-id="05135-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="05135-182">Wykres jest ponownie renderowany i obliczana *jest różnica między interfejsami* użytkownika.</span><span class="sxs-lookup"><span data-stu-id="05135-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="05135-183">Różnica ta jest najmniejszym zestawem zmian modelu DOM wymaganym do zaktualizowania interfejsu użytkownika na kliencie.</span><span class="sxs-lookup"><span data-stu-id="05135-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="05135-184">Różnica jest wysyłana do klienta w formacie binarnym i stosowana przez przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="05135-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="05135-185">Składnik jest usuwany po przejściu przez użytkownika na klienta.</span><span class="sxs-lookup"><span data-stu-id="05135-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="05135-186">Gdy użytkownik korzysta ze składnika, stan składnika (usługi, zasoby) musi być przechowywany w pamięci serwera.</span><span class="sxs-lookup"><span data-stu-id="05135-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="05135-187">Ponieważ stan wielu składników może być obsługiwany przez serwer współbieżnie, wyczerpanie pamięci jest problemem, który należy rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="05135-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="05135-188">Aby uzyskać wskazówki dotyczące sposobu tworzenia aplikacji serwera Blazor w celu zapewnienia optymalnego wykorzystania pamięci serwera, zobacz <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="05135-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="circuits"></a><span data-ttu-id="05135-189">Elektrycznych</span><span class="sxs-lookup"><span data-stu-id="05135-189">Circuits</span></span>

<span data-ttu-id="05135-190">Aplikacja serwera Blazor jest oparta na [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="05135-190">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="05135-191">Każdy klient komunikuje się z serwerem za pośrednictwem co najmniej jednego SignalR połączenia o nazwie *obwodu*.</span><span class="sxs-lookup"><span data-stu-id="05135-191">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="05135-192">Obwód Blazorabstrakcję za pośrednictwem SignalR połączeń, które mogą tolerować tymczasowe przerwy w sieci.</span><span class="sxs-lookup"><span data-stu-id="05135-192">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="05135-193">Gdy klient Blazor zobaczy, że połączenie SignalR zostało rozłączone, próbuje ponownie nawiązać połączenie z serwerem przy użyciu nowego połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="05135-193">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="05135-194">Każdy ekran przeglądarki (karta przeglądarki lub iframe), który jest połączony z aplikacją serwera Blazor używa połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="05135-194">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="05135-195">Jest to jeszcze inna ważna różnica w porównaniu z typowymi aplikacjami renderowanymi przez serwer.</span><span class="sxs-lookup"><span data-stu-id="05135-195">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="05135-196">W aplikacji renderowanej na serwerze otwieranie tej samej aplikacji na wielu ekranach przeglądarki zazwyczaj nie jest przeważnie uwzględniane w dodatkowych wymaganiach dotyczących zasobów na serwerze.</span><span class="sxs-lookup"><span data-stu-id="05135-196">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="05135-197">W aplikacji serwera Blazor, każdy ekran przeglądarki wymaga oddzielnego obwodu i oddzielnych wystąpień stanu składnika, które mają być zarządzane przez serwer programu.</span><span class="sxs-lookup"><span data-stu-id="05135-197">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="05135-198"> uważa *, że zamyka* kartę przeglądarki lub przechodzenie do zewnętrznego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="05135-198"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="05135-199">W przypadku bezpiecznego zakończenia obwód i skojarzone zasoby są natychmiast uwalniane.</span><span class="sxs-lookup"><span data-stu-id="05135-199">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="05135-200">Klient może również odłączyć się niebezpiecznie, na przykład z powodu przerwy w działaniu sieci.</span><span class="sxs-lookup"><span data-stu-id="05135-200">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> <span data-ttu-id="05135-201">Serwer Blazor przechowuje rozłączone obwody przez konfigurowalny interwał, aby umożliwić klientowi Ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="05135-201">Blazor Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="05135-202">Aby uzyskać więcej informacji, zobacz sekcję [ponowne łączenie z tym samym serwerem](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="05135-202">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="05135-203">Opóźnienie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="05135-203">UI Latency</span></span>

<span data-ttu-id="05135-204">Opóźnienie interfejsu użytkownika to czas od zainicjowanej akcji do momentu zaktualizowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="05135-204">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="05135-205">Mniejsze wartości opóźnienia interfejsu użytkownika są bezwzględnie konieczne, aby aplikacja mogła reagować na użytkownika.</span><span class="sxs-lookup"><span data-stu-id="05135-205">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="05135-206">W aplikacji serwera Blazor każda akcja jest wysyłana do serwera, przetwarzane i różnic interfejsu użytkownika jest wysyłana z powrotem.</span><span class="sxs-lookup"><span data-stu-id="05135-206">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="05135-207">W związku z tym opóźnienia interfejsu użytkownika to suma opóźnień sieci i opóźnienia serwera w przetwarzaniu akcji.</span><span class="sxs-lookup"><span data-stu-id="05135-207">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="05135-208">W przypadku aplikacji biznesowych, która jest ograniczona do prywatnej sieci firmowej, wpływ na postrzeganie opóźnień przez użytkownika z powodu opóźnienia sieci jest zwykle niezauważalny.</span><span class="sxs-lookup"><span data-stu-id="05135-208">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="05135-209">W przypadku aplikacji wdrożonej za pośrednictwem Internetu opóźnienie może być zauważalne dla użytkowników, szczególnie w przypadku, gdy użytkownicy są szeroko rozproszona geograficznie.</span><span class="sxs-lookup"><span data-stu-id="05135-209">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="05135-210">Użycie pamięci może również przyczynić się do opóźnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05135-210">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="05135-211">Zwiększone użycie pamięci powoduje częste zbieranie elementów bezużytecznych lub stronicowanie pamięci na dysku, co zmniejsza wydajność aplikacji i w związku z tym zwiększa opóźnienia interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="05135-211">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="05135-212">Aby uzyskać więcej informacji, zobacz temat <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="05135-212">For more information, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="05135-213">aplikacje serwera Blazor powinny być zoptymalizowane w celu zminimalizowania opóźnień interfejsu użytkownika przez zmniejszenie opóźnienia sieci i użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="05135-213">Blazor Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="05135-214">Aby uzyskać podejście do mierzenia opóźnień sieci, zobacz <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="05135-214">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="05135-215">Aby uzyskać więcej informacji na temat SignalR i Blazor, zobacz:</span><span class="sxs-lookup"><span data-stu-id="05135-215">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="05135-216">Połączenie z serwerem</span><span class="sxs-lookup"><span data-stu-id="05135-216">Connection to the server</span></span>

<span data-ttu-id="05135-217">aplikacje serwera Blazor wymagają aktywnego połączenia SignalR z serwerem.</span><span class="sxs-lookup"><span data-stu-id="05135-217">Blazor Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="05135-218">Jeśli połączenie zostanie utracone, aplikacja spróbuje ponownie nawiązać połączenie z serwerem.</span><span class="sxs-lookup"><span data-stu-id="05135-218">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="05135-219">O ile stan klienta nadal znajduje się w pamięci, sesja klienta zostaje wznowiona bez utraty stanu.</span><span class="sxs-lookup"><span data-stu-id="05135-219">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="05135-220">Ponowne nawiązywanie połączenia z tym samym serwerem</span><span class="sxs-lookup"><span data-stu-id="05135-220">Reconnection to the same server</span></span>

<span data-ttu-id="05135-221">Aplikacja serwera Blazor jest przedstawiona w odpowiedzi na pierwsze żądanie klienta, która konfiguruje stan interfejsu użytkownika na serwerze.</span><span class="sxs-lookup"><span data-stu-id="05135-221">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="05135-222">Gdy klient próbuje utworzyć połączenie SignalR, klient musi ponownie nawiązać połączenie z tym samym serwerem.</span><span class="sxs-lookup"><span data-stu-id="05135-222">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> <span data-ttu-id="05135-223">aplikacje serwera Blazor, które używają więcej niż jednego serwera wewnętrznej bazy danych, powinny implementować *sesje programu sticky* SignalR dla połączeń.</span><span class="sxs-lookup"><span data-stu-id="05135-223">Blazor Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="05135-224">Zalecamy korzystanie z [usługi Azure SignalR](/azure/azure-signalr) dla aplikacji Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="05135-224">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="05135-225">Usługa umożliwia skalowanie aplikacji serwera Blazor do dużej liczby jednoczesnych połączeń SignalR.</span><span class="sxs-lookup"><span data-stu-id="05135-225">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="05135-226">Sesje programu Sticky Notes są włączone dla usługi Azure SignalR przez ustawienie opcji `ServerStickyMode` lub wartości konfiguracji usługi na `Required`.</span><span class="sxs-lookup"><span data-stu-id="05135-226">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="05135-227">Aby uzyskać więcej informacji, zobacz temat <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="05135-227">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="05135-228">W przypadku korzystania z usług IIS sesje programu Sticky są włączane przy użyciu routingu żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05135-228">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="05135-229">Aby uzyskać więcej informacji, zobacz [równoważenie obciążenia HTTP przy użyciu routingu żądań aplikacji](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="05135-229">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="05135-230">Odzwierciedlanie stanu połączenia w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="05135-230">Reflect the connection state in the UI</span></span>

<span data-ttu-id="05135-231">Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="05135-231">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="05135-232">Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby.</span><span class="sxs-lookup"><span data-stu-id="05135-232">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="05135-233">Aby dostosować interfejs użytkownika, zdefiniuj element z `id` `components-reconnect-modal` w `<body>` na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="05135-233">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```html
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="05135-234">W poniższej tabeli opisano klasy CSS stosowane do elementu `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="05135-234">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="05135-235">Klasa CSS</span><span class="sxs-lookup"><span data-stu-id="05135-235">CSS class</span></span>                       | <span data-ttu-id="05135-236">Wskazuje&hellip;</span><span class="sxs-lookup"><span data-stu-id="05135-236">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="05135-237">Utracono połączenie.</span><span class="sxs-lookup"><span data-stu-id="05135-237">A lost connection.</span></span> <span data-ttu-id="05135-238">Klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="05135-238">The client is attempting to reconnect.</span></span> <span data-ttu-id="05135-239">Pokaż modalne.</span><span class="sxs-lookup"><span data-stu-id="05135-239">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="05135-240">Aktywne połączenie zostanie ponownie nawiązane z serwerem.</span><span class="sxs-lookup"><span data-stu-id="05135-240">An active connection is re-established to the server.</span></span> <span data-ttu-id="05135-241">Ukryj modalne.</span><span class="sxs-lookup"><span data-stu-id="05135-241">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="05135-242">Ponowne połączenie nie powiodło się, prawdopodobnie z powodu błędu sieci.</span><span class="sxs-lookup"><span data-stu-id="05135-242">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="05135-243">Aby spróbować ponownie nawiązać połączenie, wywołaj `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="05135-243">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="05135-244">Odrzucono ponowne połączenie.</span><span class="sxs-lookup"><span data-stu-id="05135-244">Reconnection rejected.</span></span> <span data-ttu-id="05135-245">Serwer został osiągnięty, ale odmówił połączenia, a stan użytkownika na serwerze został utracony.</span><span class="sxs-lookup"><span data-stu-id="05135-245">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="05135-246">Aby ponownie załadować aplikację, wywołaj `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="05135-246">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="05135-247">Ten stan połączenia może skutkować tym, że:</span><span class="sxs-lookup"><span data-stu-id="05135-247">This connection state may result when:</span></span><ul><li><span data-ttu-id="05135-248">Wystąpił awaria w obwodzie po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="05135-248">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="05135-249">Klient jest odłączony wystarczająco długo, aby serwer mógł porzucić stan użytkownika.</span><span class="sxs-lookup"><span data-stu-id="05135-249">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="05135-250">Wystąpienia składników, z którymi łączy się użytkownik, są usuwane.</span><span class="sxs-lookup"><span data-stu-id="05135-250">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="05135-251">Serwer zostanie uruchomiony ponownie lub proces roboczy aplikacji zostanie odtworzony.</span><span class="sxs-lookup"><span data-stu-id="05135-251">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="05135-252">Stanowe Ponowne nawiązywanie połączenia po przeprowadzeniu prerenderowania</span><span class="sxs-lookup"><span data-stu-id="05135-252">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="05135-253">aplikacje serwera Blazor są domyślnie skonfigurowane, aby wyrównać interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="05135-253">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="05135-254">Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="05135-254">This is set up in the *_Host.cshtml* Razor page:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

<span data-ttu-id="05135-255">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="05135-255">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="05135-256">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="05135-256">Is prerendered into the page.</span></span>
* <span data-ttu-id="05135-257">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia Blazor aplikacji z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="05135-257">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="05135-258">Opis</span><span class="sxs-lookup"><span data-stu-id="05135-258">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="05135-259">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="05135-259">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="05135-260">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="05135-260">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="05135-261">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="05135-261">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="05135-262">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="05135-262">Output from the component isn't included.</span></span> <span data-ttu-id="05135-263">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="05135-263">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="05135-264">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="05135-264">Renders the component into static HTML.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="05135-265">Opis</span><span class="sxs-lookup"><span data-stu-id="05135-265">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="05135-266">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="05135-266">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="05135-267">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="05135-267">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="05135-268">Parametry nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="05135-268">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="05135-269">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="05135-269">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="05135-270">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="05135-270">Output from the component isn't included.</span></span> <span data-ttu-id="05135-271">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="05135-271">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="05135-272">Parametry nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="05135-272">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="05135-273">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="05135-273">Renders the component into static HTML.</span></span> <span data-ttu-id="05135-274">Parametry są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="05135-274">Parameters are supported.</span></span> |

::: moniker-end

<span data-ttu-id="05135-275">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="05135-275">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="05135-276">Gdy `RenderMode` jest `ServerPrerendered`, składnik jest początkowo renderowany statycznie jako część strony.</span><span class="sxs-lookup"><span data-stu-id="05135-276">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="05135-277">Gdy przeglądarka nawiąże połączenie z serwerem, składnik jest renderowany *ponownie*, a składnik jest teraz interaktywny.</span><span class="sxs-lookup"><span data-stu-id="05135-277">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="05135-278">Jeśli istnieje [Metoda cyklu życia](xref:blazor/components#lifecycle-methods) służąca do inicjowania składnika (`OnInitialized{Async}`), metoda jest wykonywana *dwukrotnie*:</span><span class="sxs-lookup"><span data-stu-id="05135-278">If a [lifecycle method](xref:blazor/components#lifecycle-methods) for initializing the component (`OnInitialized{Async}`) is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="05135-279">Gdy składnik jest wstępnie renderowany statycznie.</span><span class="sxs-lookup"><span data-stu-id="05135-279">When the component is prerendered statically.</span></span>
* <span data-ttu-id="05135-280">Po nawiązaniu połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="05135-280">After the server connection has been established.</span></span>

<span data-ttu-id="05135-281">Może to spowodować zauważalną zmianę danych wyświetlanych w interfejsie użytkownika, gdy składnik jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="05135-281">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="05135-282">Aby uniknąć podwójnego renderowania w aplikacji serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="05135-282">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="05135-283">Przekaż identyfikator, który może służyć do buforowania stanu podczas wykonywania prerenderowania i pobierania stanu po ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="05135-283">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="05135-284">Użyj identyfikatora podczas renderowania, aby zapisać stan składnika.</span><span class="sxs-lookup"><span data-stu-id="05135-284">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="05135-285">Użyj identyfikatora po włączeniu, aby pobrać buforowany stan.</span><span class="sxs-lookup"><span data-stu-id="05135-285">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="05135-286">Poniższy kod ilustruje zaktualizowany `WeatherForecastService` w aplikacji serwerowej Blazor opartej na szablonach, która pozwala uniknąć podwójnego renderowania:</span><span class="sxs-lookup"><span data-stu-id="05135-286">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
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
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="05135-287">Renderuj stanowe składniki interaktywne ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="05135-287">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="05135-288">Można dodać składniki interaktywne ze stanem do strony lub widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="05135-288">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="05135-289">Gdy renderuje stronę lub widok:</span><span class="sxs-lookup"><span data-stu-id="05135-289">When the page or view renders:</span></span>

* <span data-ttu-id="05135-290">Składnik jest wstępnie renderowany przy użyciu strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="05135-290">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="05135-291">Początkowy stan składnika używany na potrzeby renderowania wstępnego został utracony.</span><span class="sxs-lookup"><span data-stu-id="05135-291">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="05135-292">Nowy stan składnika jest tworzony po nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="05135-292">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="05135-293">Następująca strona Razor renderuje składnik `Counter`:</span><span class="sxs-lookup"><span data-stu-id="05135-293">The following Razor page renders a `Counter` component:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="05135-294">Renderuj nieinteraktywne składniki ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="05135-294">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="05135-295">Na poniższej stronie Razor składnik `MyComponent` jest renderowany statycznie z wartością początkową określoną przy użyciu formularza:</span><span class="sxs-lookup"><span data-stu-id="05135-295">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>

::: moniker range=">= aspnetcore-3.1"

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

::: moniker-end

::: moniker range="< aspnetcore-3.1"

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

::: moniker-end

<span data-ttu-id="05135-296">Ponieważ `MyComponent` jest renderowany statycznie, składnik nie może być interaktywny.</span><span class="sxs-lookup"><span data-stu-id="05135-296">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="05135-297">Wykryj, kiedy aplikacja jest przedrenderowana</span><span class="sxs-lookup"><span data-stu-id="05135-297">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="05135-298">Konfigurowanie klienta SignalR dla aplikacji Blazor Server</span><span class="sxs-lookup"><span data-stu-id="05135-298">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="05135-299">Czasami trzeba skonfigurować klienta SignalR używanego przez aplikacje Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="05135-299">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="05135-300">Na przykład możesz chcieć skonfigurować rejestrowanie na kliencie SignalR, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="05135-300">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="05135-301">Aby skonfigurować klienta SignalR w pliku *Pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="05135-301">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="05135-302">Dodaj atrybut `autostart="false"` do tagu `<script>` dla skryptu *blazor. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="05135-302">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="05135-303">Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa SignalR konstruktora.</span><span class="sxs-lookup"><span data-stu-id="05135-303">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="05135-304">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="05135-304">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
