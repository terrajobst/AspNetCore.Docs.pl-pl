---
title: Blazor modelach hostingu
author: guardrex
description: Dowiedz się, Blazor po stronie klienta i po stronie serwera ASP.NET Razor składniki podstawowe modele obsługi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/hosting-models
ms.openlocfilehash: 0e7598c9f27201a989d1088764e09461a37beae4
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614847"
---
# <a name="blazor-hosting-models"></a><span data-ttu-id="0bbc6-103">Blazor modelach hostingu</span><span class="sxs-lookup"><span data-stu-id="0bbc6-103">Blazor hosting models</span></span>

<span data-ttu-id="0bbc6-104">Przez [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="0bbc6-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="0bbc6-105">Blazor to struktura sieci web przeznaczony do działania po stronie klienta w przeglądarce na [format WebAssembly](http://webassembly.org/)— na podstawie środowiska uruchomieniowego .NET (*Blazor*) lub po stronie serwera w programie ASP.NET Core (*składniki aplikacji ASP.NET Core Razor*).</span><span class="sxs-lookup"><span data-stu-id="0bbc6-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="0bbc6-106">Niezależnie od tego modelu, aplikacji i składników modelach hostingu *pozostają takie same*.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="0bbc6-107">W tym artykule omówiono dostępne modele hostingu:</span><span class="sxs-lookup"><span data-stu-id="0bbc6-107">This article discusses the available hosting models:</span></span>

* [<span data-ttu-id="0bbc6-108">Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="0bbc6-108">Client-side Blazor</span></span>](#client-side-hosting-model)
* [<span data-ttu-id="0bbc6-109">Po stronie serwera platformy ASP.NET Core Razor składników</span><span class="sxs-lookup"><span data-stu-id="0bbc6-109">Server-side ASP.NET Core Razor Components</span></span>](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a><span data-ttu-id="0bbc6-110">Model hostingu po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="0bbc6-110">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="0bbc6-111">Jednostki modelu hostowania Blazor jest uruchomiona po stronie klienta w przeglądarce na format WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-111">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="0bbc6-112">Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-112">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="0bbc6-113">Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-113">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="0bbc6-114">Aktualizacje interfejsu użytkownika i obsługa zdarzeń odbywa się w obrębie tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-114">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="0bbc6-115">Zasoby aplikacji są wdrażane jako pliki statyczne z serwera sieci web lub usługą umożliwia obsługę zawartości statycznej dla klientów.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-115">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

<span data-ttu-id="0bbc6-117">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie klienta, użyj jednej z następujących szablonów:</span><span class="sxs-lookup"><span data-stu-id="0bbc6-117">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="0bbc6-118">**Blazor** ([blazor nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; wdrożony jako zbiór plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-118">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="0bbc6-119">**Blazor (ASP.NET Core hostowane)** ([blazorhosted nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; obsługiwanej przez serwer programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-119">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="0bbc6-120">Aplikacja platformy ASP.NET Core udostępnia aplikacji Blazor klientom.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-120">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="0bbc6-121">Aplikacja Blazor po stronie klienta mogą wchodzić w interakcje z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci web lub [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="0bbc6-121">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="0bbc6-122">Szablony zawierają *components.webassembly.js* skrypt, który obsługuje:</span><span class="sxs-lookup"><span data-stu-id="0bbc6-122">The templates include the *components.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="0bbc6-123">Pobieranie, środowisko uruchomieniowe platformy .NET, aplikacji i zależności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-123">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="0bbc6-124">Inicjalizacja środowiska uruchomieniowego, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-124">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="0bbc6-125">Model hostingu w sieci po stronie klienta oferuje wiele korzyści.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-125">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="0bbc6-126">Blazor po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="0bbc6-126">Client-side Blazor:</span></span>

* <span data-ttu-id="0bbc6-127">Ma nie zależności po stronie serwera .NET.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-127">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="0bbc6-128">W pełni wykorzystuje zasoby klienta i możliwości.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-128">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="0bbc6-129">Odciążanie pracować z serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-129">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="0bbc6-130">Obsługuje scenariusze w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-130">Supports offline scenarios.</span></span>

<span data-ttu-id="0bbc6-131">Brak wad hostingu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-131">There are downsides to client-side hosting.</span></span> <span data-ttu-id="0bbc6-132">Blazor po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="0bbc6-132">Client-side Blazor:</span></span>

* <span data-ttu-id="0bbc6-133">Ogranicza ją do możliwości przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-133">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="0bbc6-134">Wymaga klienta obsługującego sprzętu i oprogramowania (na przykład w przypadku, format WebAssembly Obsługa).</span><span class="sxs-lookup"><span data-stu-id="0bbc6-134">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="0bbc6-135">Ma większy rozmiar pobierania i aplikacji dłuższy czas ładowania.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-135">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="0bbc6-136">Ma mniejszą dla dorosłych, środowisko uruchomieniowe platformy .NET oraz narzędzia do obsługi (na przykład ograniczenia w [.NET Standard](/dotnet/standard/net-standard) pomocy technicznej i debugowania).</span><span class="sxs-lookup"><span data-stu-id="0bbc6-136">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="0bbc6-137">Model hostingu po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="0bbc6-137">Server-side hosting model</span></span>

<span data-ttu-id="0bbc6-138">Za pomocą modelu hostingu po stronie serwera aplikacji jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-138">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="0bbc6-139">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-139">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Przeglądarka wchodzi w interakcję z aplikacją (obsługiwana wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="0bbc6-141">Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie serwera, należy użyć platformy ASP.NET Core **składniki Razor** szablonu ([razorcomponents nowe dotnet](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="0bbc6-141">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Razor Components** template ([dotnet new razorcomponents](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="0bbc6-142">Aplikacja platformy ASP.NET Core hostuje składniki Razor aplikacji po stronie serwera i ustawia punkt końcowy SignalR, gdy klienci łączą.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-142">The ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="0bbc6-143">Aplikacja platformy ASP.NET Core odwołuje się do aplikacji `Startup` klasy do dodania:</span><span class="sxs-lookup"><span data-stu-id="0bbc6-143">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="0bbc6-144">Usługi aparatu Razor składniki po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-144">Server-side Razor Components services.</span></span>
* <span data-ttu-id="0bbc6-145">Aplikacja do żądania obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-145">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="0bbc6-146">*Components.server.js* skryptu&dagger; nawiązuje połączenie z klientem.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-146">The *components.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="0bbc6-147">To aplikacja odpowiada za utrwalanie i przywracanie stanu aplikacji, zgodnie z potrzebami (na przykład w przypadku połączenia sieciowego utracone).</span><span class="sxs-lookup"><span data-stu-id="0bbc6-147">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="0bbc6-148">Model hostingu w sieci po stronie serwera oferuje wiele korzyści.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-148">The server-side hosting model offers several benefits.</span></span> <span data-ttu-id="0bbc6-149">Składniki po stronie serwera Razor:</span><span class="sxs-lookup"><span data-stu-id="0bbc6-149">Server-side Razor Components:</span></span>

* <span data-ttu-id="0bbc6-150">Rozmiar aplikacji znacznie mniejszy niż aplikacja Blazor po stronie klienta i ładowane dużo szybciej.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-150">Have a significantly smaller app size than a client-side Blazor app and load much faster.</span></span>
* <span data-ttu-id="0bbc6-151">Można wykorzystać możliwości serwera, w tym przy użyciu zgodnych interfejsów API dowolnej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-151">Can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="0bbc6-152">Uruchom na platformie .NET Core na serwerze, dzięki czemu .NET istniejących narzędzi, takich jak debugowanie, działa zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-152">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="0bbc6-153">Współpracuje z elastycznej klientów (na przykład przeglądarek, które nie obsługują format WebAssembly i zasobach ograniczonego urządzenia).</span><span class="sxs-lookup"><span data-stu-id="0bbc6-153">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="0bbc6-154">.NET /C# bazy kodu, w tym kodu składnika aplikacji, nie jest obsługiwane dla klientów.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-154">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="0bbc6-155">Brak wad hostingu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-155">There are downsides to server-side hosting.</span></span> <span data-ttu-id="0bbc6-156">Składniki po stronie serwera Razor:</span><span class="sxs-lookup"><span data-stu-id="0bbc6-156">Server-side Razor Components:</span></span>

* <span data-ttu-id="0bbc6-157">Mają wyższe opóźnienie: Każdy interakcja użytkownika obejmuje przeskok sieci.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-157">Have higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="0bbc6-158">Oferty obsługi w trybie offline: Jeśli połączenie klienta zakończy się niepowodzeniem, aplikacja przestaje działać.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-158">Offer no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="0bbc6-159">Ograniczona skalowalność: Serwer musi zarządzać wieloma połączeń klientów i obsługi stanu klienta.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-159">Have reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="0bbc6-160">Wymaga platformy ASP.NET Core serwerze do obsługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-160">Require an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="0bbc6-161">Wdrożenie bez serwera (na przykład z sieci CDN) nie jest możliwe.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="0bbc6-162">&dagger;*Components.server.js* skryptu jest opublikowana w następującej ścieżce: *bin / {debugowanie | Zlecenia} / {struktury docelowej} /publish/ {Nazwa aplikacji}. Aplikacja/dist/_struktura*.</span><span class="sxs-lookup"><span data-stu-id="0bbc6-162">&dagger;The *components.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
