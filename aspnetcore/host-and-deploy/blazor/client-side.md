---
title: Hostowanie i wdrażanie ASP.NET Core Blazor po stronie klienta
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor przy użyciu ASP.NET Core, usługi Content Delivery Networks (CDN), serwerów plików i stron usługi GitHub.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: be6b6c245440cb085a1a6b115f4f087306f7cc83
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308087"
---
# <a name="host-and-deploy-aspnet-core-blazor-client-side"></a><span data-ttu-id="27be4-103">Hostowanie i wdrażanie ASP.NET Core Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="27be4-103">Host and deploy ASP.NET Core Blazor client-side</span></span>

<span data-ttu-id="27be4-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="27be4-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="27be4-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="27be4-105">Host configuration values</span></span>

<span data-ttu-id="27be4-106">Aplikacje Blazor korzystające z [modelu hostingu po stronie klienta](xref:blazor/hosting-models#client-side) mogą akceptować następujące wartości konfiguracji hosta jako argumenty wiersza polecenia w środowisku uruchomieniowym.</span><span class="sxs-lookup"><span data-stu-id="27be4-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="27be4-107">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="27be4-107">Content Root</span></span>

<span data-ttu-id="27be4-108">`--contentroot` Argument ustawia ścieżkę bezwzględną do katalogu, który zawiera pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="27be4-109">W poniższych przykładach `/content-root-path` jest ścieżką katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="27be4-110">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="27be4-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="27be4-111">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27be4-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="27be4-112">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="27be4-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="27be4-113">To ustawienie jest używane, gdy aplikacja jest uruchamiana z debugerem programu Visual Studio i z wiersza polecenia `dotnet run`z.</span><span class="sxs-lookup"><span data-stu-id="27be4-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="27be4-114">W programie Visual Studio Określ argument w **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="27be4-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="27be4-115">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="27be4-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="27be4-116">Baza ścieżki</span><span class="sxs-lookup"><span data-stu-id="27be4-116">Path base</span></span>

<span data-ttu-id="27be4-117">Argument ustawia ścieżkę bazową aplikacji dla aplikacji uruchamianej lokalnie z niegłówną ścieżką wirtualną `<base>` (tag `href` jest ustawiony na ścieżkę inną niż `/` w przypadku przemieszczania i produkcji). `--pathbase`</span><span class="sxs-lookup"><span data-stu-id="27be4-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="27be4-118">W poniższych przykładach `/virtual-path` jest podstawą ścieżki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="27be4-119">Aby uzyskać więcej informacji, zobacz sekcję [Ścieżka podstawowa aplikacji](#app-base-path) .</span><span class="sxs-lookup"><span data-stu-id="27be4-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="27be4-120">W przeciwieństwie `href` do ścieżki przekazanej do `<base>` tagu, nie dodawaj końcowego ukośnika `--pathbase` (`/`) podczas przekazywania wartości argumentu.</span><span class="sxs-lookup"><span data-stu-id="27be4-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="27be4-121">Jeśli ścieżka podstawowa aplikacji jest podana w `<base>` tagu jako `<base href="/CoolApp/">` (zawiera końcowy ukośnik), należy przekazać wartość argumentu wiersza polecenia jako `--pathbase=/CoolApp` (bez ukośnika na końcu).</span><span class="sxs-lookup"><span data-stu-id="27be4-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="27be4-122">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="27be4-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="27be4-123">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27be4-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="27be4-124">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="27be4-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="27be4-125">To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia w programie `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="27be4-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="27be4-126">W programie Visual Studio Określ argument w **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="27be4-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="27be4-127">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="27be4-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="27be4-128">adresy URL</span><span class="sxs-lookup"><span data-stu-id="27be4-128">URLs</span></span>

<span data-ttu-id="27be4-129">`--urls` Argument ustawia adresy IP lub adresy hosta z portami i protokołami, aby nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="27be4-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="27be4-130">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="27be4-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="27be4-131">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="27be4-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="27be4-132">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="27be4-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="27be4-133">To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia w programie `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="27be4-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="27be4-134">W programie Visual Studio Określ argument w **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="27be4-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="27be4-135">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="27be4-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="27be4-136">wdrażania</span><span class="sxs-lookup"><span data-stu-id="27be4-136">Deployment</span></span>

<span data-ttu-id="27be4-137">Z [modelem hostingu po stronie klienta](xref:blazor/hosting-models#client-side):</span><span class="sxs-lookup"><span data-stu-id="27be4-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="27be4-138">Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="27be4-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="27be4-139">Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="27be4-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="27be4-140">Obsługiwane są następujące strategie:</span><span class="sxs-lookup"><span data-stu-id="27be4-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="27be4-141">Aplikacja Blazor jest obsługiwana przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="27be4-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="27be4-142">Ta strategia jest objęta [wdrożeniem hostowanym za pomocą ASP.NET Core](#hosted-deployment-with-aspnet-core) sekcji.</span><span class="sxs-lookup"><span data-stu-id="27be4-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="27be4-143">Aplikacja Blazor jest umieszczana na statycznym, hostingowym serwerze sieci Web lub usłudze, w której program .NET nie jest używany do obsługi aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="27be4-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="27be4-144">Ta strategia została omówiona w sekcji [wdrażanie autonomiczne](#standalone-deployment) .</span><span class="sxs-lookup"><span data-stu-id="27be4-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="27be4-145">Konfigurowanie konsolidatora</span><span class="sxs-lookup"><span data-stu-id="27be4-145">Configure the Linker</span></span>

<span data-ttu-id="27be4-146">Blazor wykonuje konsolidację języka pośredniego (IL) dla każdej kompilacji, aby usunąć niepotrzebny kod IL z zestawów wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="27be4-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="27be4-147">Łączenie zestawu może być kontrolowane podczas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="27be4-148">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="27be4-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="27be4-149">Ponownie Napisz adresy URL pod kątem prawidłowego routingu</span><span class="sxs-lookup"><span data-stu-id="27be4-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="27be4-150">Żądania routingu dla składników strony w aplikacji po stronie klienta nie są tak proste jak żądania routingu do aplikacji hostowanej po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="27be4-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="27be4-151">Rozważ użycie aplikacji po stronie klienta z dwoma składnikami:</span><span class="sxs-lookup"><span data-stu-id="27be4-151">Consider a client-side app with two components:</span></span>

* <span data-ttu-id="27be4-152">*Główny. Razor* &ndash; ładuje się w katalogu głównym aplikacji i zawiera link do `About` składnika (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="27be4-152">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="27be4-153">*Informacje o* &ndash; składniku Razor `About` .</span><span class="sxs-lookup"><span data-stu-id="27be4-153">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="27be4-154">Gdy zażądano dokumentu domyślnego aplikacji przy użyciu paska adresu przeglądarki (na przykład `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="27be4-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="27be4-155">Przeglądarka wykonuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="27be4-155">The browser makes a request.</span></span>
1. <span data-ttu-id="27be4-156">Zostanie zwrócona strona domyślna, która jest zwykle *index. html*.</span><span class="sxs-lookup"><span data-stu-id="27be4-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="27be4-157">*index. html* Bootstrap aplikację.</span><span class="sxs-lookup"><span data-stu-id="27be4-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="27be4-158">Ładowanie routera Blazor, a składnik Razor `Main` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="27be4-158">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="27be4-159">Na stronie głównej wybranie `About` linku do składnika działa na kliencie, ponieważ router Blazor zatrzyma w przeglądarce żądanie połączenia z `www.contoso.com` Internetem `About` i obsługuje wyrenderowany `About` składnik.</span><span class="sxs-lookup"><span data-stu-id="27be4-159">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="27be4-160">Wszystkie żądania dotyczące wewnętrznych punktów końcowych *w aplikacji po stronie klienta* działają w taki sam sposób: Żądania nie wyzwalają żądań przeglądarki do zasobów hostowanych przez serwer w Internecie.</span><span class="sxs-lookup"><span data-stu-id="27be4-160">All of the requests for internal endpoints *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="27be4-161">Router obsługuje wewnętrznie żądania.</span><span class="sxs-lookup"><span data-stu-id="27be4-161">The router handles the requests internally.</span></span>

<span data-ttu-id="27be4-162">Żądanie kończy się niepowodzeniem `www.contoso.com/About`, jeśli żądanie zostanie wykonane przy użyciu paska adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="27be4-162">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="27be4-163">Ten zasób nie istnieje na hoście internetowym aplikacji, więc zwracana jest odpowiedź *404 — nie znaleziono* .</span><span class="sxs-lookup"><span data-stu-id="27be4-163">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="27be4-164">Ponieważ przeglądarki wysyłają żądania do hostów internetowych dla stron po stronie klienta, serwery sieci Web i usługi hostingu muszą ponownie zapisywać wszystkie żądania dotyczące zasobów, które nie znajdują się fizycznie na serwerze, na stronie *index. html* .</span><span class="sxs-lookup"><span data-stu-id="27be4-164">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="27be4-165">Gdy jest zwracany *plik index. html* , router po stronie klienta aplikacji przejmuje i reaguje na prawidłowy zasób.</span><span class="sxs-lookup"><span data-stu-id="27be4-165">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="27be4-166">Ścieżka podstawowa aplikacji</span><span class="sxs-lookup"><span data-stu-id="27be4-166">App base path</span></span>

<span data-ttu-id="27be4-167">*Ścieżka podstawowa aplikacji* jest ścieżką katalogu głównego aplikacji wirtualnej na serwerze.</span><span class="sxs-lookup"><span data-stu-id="27be4-167">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="27be4-168">Na przykład aplikacja, która znajduje się na serwerze firmy Contoso w folderze `/CoolApp/` wirtualnym, jest dostępna pod adresem `https://www.contoso.com/CoolApp` i ma wirtualną ścieżkę `/CoolApp/`bazową.</span><span class="sxs-lookup"><span data-stu-id="27be4-168">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="27be4-169">Ustawiając ścieżkę bazową aplikacji na ścieżkę wirtualną (`<base href="/CoolApp/">`), aplikacja zostanie powiadomiona o tym, gdzie praktycznie znajduje się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="27be4-169">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="27be4-170">Aplikacja może używać ścieżki podstawowej aplikacji do konstruowania adresów URL względem katalogu głównego aplikacji ze składnika, który nie znajduje się w katalogu głównym.</span><span class="sxs-lookup"><span data-stu-id="27be4-170">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="27be4-171">Dzięki temu składniki, które znajdują się na różnych poziomach struktury katalogów, umożliwiają tworzenie linków do innych zasobów w lokalizacji w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-171">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="27be4-172">Ścieżka podstawowa aplikacji jest również używana do przechwytywania hiperłącze, gdzie `href` element docelowy linku znajduje się w przestrzeni&mdash;URI ścieżki bazowej aplikacji. router Blazor obsługuje nawigację wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="27be4-172">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="27be4-173">W wielu scenariuszach hostingu ścieżka wirtualna serwera do aplikacji jest katalogiem głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-173">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="27be4-174">W takich przypadkach Ścieżka podstawowa aplikacji jest ukośnikiem (`<base href="/" />`), który jest domyślną konfiguracją dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-174">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="27be4-175">W innych scenariuszach hostingu, takich jak strony GitHub i katalogi wirtualne lub aplikacje podrzędne usług IIS, ścieżka podstawowa aplikacji musi być ustawiona na ścieżkę wirtualną serwera do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-175">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="27be4-176">Aby ustawić ścieżkę bazową aplikacji, zaktualizuj `<base>` tag `<head>` wewnątrz elementów tagów pliku *wwwroot/index.html* .</span><span class="sxs-lookup"><span data-stu-id="27be4-176">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="27be4-177">Ustaw wartość `/virtual-path/`atrybutuna (wymagany jest końcowy ukośnik), gdzie `/virtual-path/` to pełna ścieżka katalogu głównego aplikacji wirtualnej na serwerze dla aplikacji. `href`</span><span class="sxs-lookup"><span data-stu-id="27be4-177">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="27be4-178">W poprzednim przykładzie ścieżka wirtualna jest ustawiona na `/CoolApp/`:. `<base href="/CoolApp/">`</span><span class="sxs-lookup"><span data-stu-id="27be4-178">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="27be4-179">W przypadku aplikacji z skonfigurowaną niegłówną ścieżką wirtualną (na przykład `<base href="/CoolApp/">`) aplikacja nie będzie mogła znaleźć zasobów w *przypadku uruchamiania lokalnego*.</span><span class="sxs-lookup"><span data-stu-id="27be4-179">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="27be4-180">Aby rozwiązać ten problem podczas lokalnego tworzenia i testowania, można podać podstawowy argument *ścieżki* , który jest zgodny `href` z wartością `<base>` tagu w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="27be4-180">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="27be4-181">Aby przekazać argument podstawowy ścieżki z ścieżką główną (`/`) podczas lokalnego uruchamiania aplikacji, `dotnet run` wykonaj polecenie z katalogu `--pathbase` aplikacji z opcją:</span><span class="sxs-lookup"><span data-stu-id="27be4-181">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="27be4-182">W przypadku aplikacji z wirtualną ścieżką `/CoolApp/` bazową (`<base href="/CoolApp/">`) polecenie to:</span><span class="sxs-lookup"><span data-stu-id="27be4-182">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="27be4-183">Aplikacja reaguje lokalnie o `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="27be4-183">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="27be4-184">Aby uzyskać więcej informacji, zapoznaj się z sekcją w sekcji [Konfiguracja podstawowego hosta ścieżki](#path-base).</span><span class="sxs-lookup"><span data-stu-id="27be4-184">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="27be4-185">Jeśli aplikacja korzysta z [modelu hostingu po stronie klienta](xref:blazor/hosting-models#client-side) (na podstawie `blazor` szablonu projektu **Blazor (po stronie klienta)** , szablonu w przypadku użycia polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ) i jest hostowana jako podaplikacja IIS w aplikacji ASP.NET Core, ważne jest, aby Wyłącz procedurę obsługi modułu dziedziczonego ASP.NET Core lub upewnij się, że `<handlers>` sekcja główna (nadrzędna) aplikacji w pliku *Web. config* nie jest dziedziczona przez aplikację podrzędną.</span><span class="sxs-lookup"><span data-stu-id="27be4-185">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor (client-side)** project template, the `blazor` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-app in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="27be4-186">Usuń program obsługi w opublikowanym pliku *Web. config* aplikacji, dodając `<handlers>` sekcję do pliku:</span><span class="sxs-lookup"><span data-stu-id="27be4-186">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="27be4-187">Alternatywnie można wyłączyć dziedziczenie `<system.webServer>` sekcji głównej (nadrzędnej) aplikacji `<location>` przy użyciu elementu z `inheritInChildApplications` ustawionym na `false`:</span><span class="sxs-lookup"><span data-stu-id="27be4-187">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" ... />
      </handlers>
      <aspNetCore ... />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="27be4-188">Usuwanie procedury obsługi lub wyłączanie dziedziczenia jest wykonywane poza konfiguracją ścieżki podstawowej aplikacji zgodnie z opisem w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="27be4-188">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="27be4-189">Ustaw ścieżkę bazową aplikacji w pliku *index. html* aplikacji na alias IIS używany podczas konfigurowania aplikacji podrzędnej w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="27be4-189">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="27be4-190">Hostowane wdrożenie z ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27be4-190">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="27be4-191">*Wdrożenie hostowane* umożliwia Blazor aplikacji po stronie klienta w przeglądarkach z [aplikacji ASP.NET Core](xref:index) działającej na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="27be4-191">A *hosted deployment* serves the Blazor client-side app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="27be4-192">Aplikacja Blazor jest dołączana do aplikacji ASP.NET Core w publikowanym danych wyjściowych, dzięki czemu dwie aplikacje są wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="27be4-192">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="27be4-193">Wymagany jest serwer sieci Web, który umożliwia hostowanie aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="27be4-193">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="27be4-194">W przypadku wdrożenia hostowanego program Visual Studio zawiera szablon projektu **Blazor (ASP.NET Core Hosted)** (`blazorhosted` szablon w przypadku używania polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ).</span><span class="sxs-lookup"><span data-stu-id="27be4-194">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="27be4-195">Aby uzyskać więcej informacji na temat ASP.NET Core hostingu i wdrażania aplikacji <xref:host-and-deploy/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="27be4-195">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="27be4-196">Aby uzyskać informacje na temat wdrażania do Azure App Service <xref:tutorials/publish-to-azure-webapp-using-vs>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="27be4-196">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="27be4-197">Wdrożenie autonomiczne</span><span class="sxs-lookup"><span data-stu-id="27be4-197">Standalone deployment</span></span>

<span data-ttu-id="27be4-198">*Wdrożenie autonomiczne* służy aplikacji Blazor po stronie klienta jako zestawu plików statycznych, które są żądane bezpośrednio przez klientów programu.</span><span class="sxs-lookup"><span data-stu-id="27be4-198">A *standalone deployment* serves the Blazor client-side app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="27be4-199">Każdy statyczny serwer plików jest w stanie obsłużyć aplikację Blazor.</span><span class="sxs-lookup"><span data-stu-id="27be4-199">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="27be4-200">Zasoby wdrażania autonomicznego są publikowane w folderze *bin/Release/{Target Framework}/Publish/{Assembly Name}/dist* .</span><span class="sxs-lookup"><span data-stu-id="27be4-200">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="27be4-201">IIS</span><span class="sxs-lookup"><span data-stu-id="27be4-201">IIS</span></span>

<span data-ttu-id="27be4-202">Usługi IIS to obsługujący statyczny serwer plików dla aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="27be4-202">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="27be4-203">Aby skonfigurować usługi IIS do hostowania Blazor, zobacz [Tworzenie statycznej witryny sieci Web w usługach IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="27be4-203">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="27be4-204">Opublikowane zasoby są tworzone w folderze */bin/Release/{Target Framework}/Publish* .</span><span class="sxs-lookup"><span data-stu-id="27be4-204">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="27be4-205">Hostowanie zawartości folderu *publikowania* na serwerze sieci Web lub w usłudze hostingu.</span><span class="sxs-lookup"><span data-stu-id="27be4-205">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="27be4-206">web.config</span><span class="sxs-lookup"><span data-stu-id="27be4-206">web.config</span></span>

<span data-ttu-id="27be4-207">Po opublikowaniu projektu Blazor zostanie utworzony plik *Web. config* z następującą konfiguracją usług IIS:</span><span class="sxs-lookup"><span data-stu-id="27be4-207">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="27be4-208">Typy MIME są ustawiane dla następujących rozszerzeń plików:</span><span class="sxs-lookup"><span data-stu-id="27be4-208">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="27be4-209">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="27be4-209">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="27be4-210">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="27be4-210">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="27be4-211">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="27be4-211">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="27be4-212">*. WOFF* &ndash;`application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="27be4-212">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="27be4-213">*. woff2* &ndash;`application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="27be4-213">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="27be4-214">Kompresja HTTP jest włączona dla następujących typów MIME:</span><span class="sxs-lookup"><span data-stu-id="27be4-214">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="27be4-215">Reguły modułu ponownego zapisywania adresu URL zostały ustanowione:</span><span class="sxs-lookup"><span data-stu-id="27be4-215">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="27be4-216">Obsługuj podkatalog, w którym znajdują się zasoby statyczne aplikacji ( *{Assembly Name}/dist/{Path*).</span><span class="sxs-lookup"><span data-stu-id="27be4-216">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="27be4-217">Utwórz Routing awaryjny SPA, aby żądania dotyczące zasobów nienależących do pliku zostały przekierowane do domyślnego dokumentu aplikacji w folderze zasobów statycznych ( *{Assembly Name}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="27be4-217">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="27be4-218">Zainstaluj moduł ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="27be4-218">Install the URL Rewrite Module</span></span>

<span data-ttu-id="27be4-219">[Moduł ponownego zapisywania adresu URL](https://www.iis.net/downloads/microsoft/url-rewrite) jest wymagany do ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="27be4-219">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="27be4-220">Moduł nie jest instalowany domyślnie i nie jest dostępny do zainstalowania jako funkcja usługi roli Serwer sieci Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="27be4-220">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="27be4-221">Moduł musi zostać pobrany z witryny sieci Web usług IIS.</span><span class="sxs-lookup"><span data-stu-id="27be4-221">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="27be4-222">Zainstaluj moduł przy użyciu Instalatora platformy sieci Web:</span><span class="sxs-lookup"><span data-stu-id="27be4-222">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="27be4-223">Lokalnie przejdź do [strony pobierania modułu ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="27be4-223">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="27be4-224">W przypadku wersji angielskiej wybierz pozycję **Instalatora WebPI** , aby pobrać Instalatora Instalatora WebPI.</span><span class="sxs-lookup"><span data-stu-id="27be4-224">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="27be4-225">W przypadku innych języków wybierz odpowiednią architekturę dla serwera (x86/x64), aby pobrać Instalatora.</span><span class="sxs-lookup"><span data-stu-id="27be4-225">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="27be4-226">Skopiuj Instalatora na serwer.</span><span class="sxs-lookup"><span data-stu-id="27be4-226">Copy the installer to the server.</span></span> <span data-ttu-id="27be4-227">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="27be4-227">Run the installer.</span></span> <span data-ttu-id="27be4-228">Wybierz przycisk **Zainstaluj** i zaakceptuj postanowienia licencyjne.</span><span class="sxs-lookup"><span data-stu-id="27be4-228">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="27be4-229">Po zakończeniu instalacji nie jest wymagane ponowne uruchomienie serwera.</span><span class="sxs-lookup"><span data-stu-id="27be4-229">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="27be4-230">Skonfiguruj witrynę sieci Web</span><span class="sxs-lookup"><span data-stu-id="27be4-230">Configure the website</span></span>

<span data-ttu-id="27be4-231">Ustaw **ścieżkę fizyczną** witryny sieci Web do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-231">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="27be4-232">Folder zawiera:</span><span class="sxs-lookup"><span data-stu-id="27be4-232">The folder contains:</span></span>

* <span data-ttu-id="27be4-233">Plik *Web. config* , za pomocą którego usługi IIS konfigurują witrynę sieci Web, w tym wymagane reguły przekierowań i typy zawartości plików.</span><span class="sxs-lookup"><span data-stu-id="27be4-233">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="27be4-234">Folder elementu zawartości statycznej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27be4-234">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="27be4-235">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="27be4-235">Troubleshooting</span></span>

<span data-ttu-id="27be4-236">W przypadku odebrania *500 — wewnętrzny błąd serwera* , a Menedżer usług IIS zgłasza błędy przy próbie uzyskania dostępu do konfiguracji witryny sieci Web, upewnij się, że zainstalowano moduł ponownego zapisywania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="27be4-236">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="27be4-237">Gdy moduł nie jest zainstalowany, nie można przeanalizować pliku *Web. config* przez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="27be4-237">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="27be4-238">Zapobiega to załadowaniu przez Menedżera usług IIS konfiguracji witryny sieci Web i witryny sieci Web do obsługi plików statycznych Blazor.</span><span class="sxs-lookup"><span data-stu-id="27be4-238">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="27be4-239">Aby uzyskać więcej informacji na temat rozwiązywania problemów z <xref:test/troubleshoot-azure-iis>WDROŻENIAMI w usługach IIS, zobacz.</span><span class="sxs-lookup"><span data-stu-id="27be4-239">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="27be4-240">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="27be4-240">Azure Storage</span></span>

<span data-ttu-id="27be4-241">Hosting pliku statycznego usługi Azure Storage umożliwia hosting aplikacji bezserwerowych Blazor.</span><span class="sxs-lookup"><span data-stu-id="27be4-241">Azure Storage static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="27be4-242">Obsługiwane są niestandardowe nazwy domen, usługa Azure Content Delivery Network (CDN) i protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="27be4-242">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="27be4-243">Gdy usługa BLOB jest włączona dla hostingu statycznej witryny sieci Web na koncie magazynu:</span><span class="sxs-lookup"><span data-stu-id="27be4-243">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="27be4-244">Ustaw **nazwę dokumentu indeksu** na `index.html`.</span><span class="sxs-lookup"><span data-stu-id="27be4-244">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="27be4-245">Ustaw `index.html` **ścieżkę do dokumentu błędu** .</span><span class="sxs-lookup"><span data-stu-id="27be4-245">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="27be4-246">Składniki Razor i inne punkty końcowe inne niż pliki nie znajdują się w ścieżkach fizycznych w zawartości statycznej przechowywanej przez usługę BLOB.</span><span class="sxs-lookup"><span data-stu-id="27be4-246">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="27be4-247">Po otrzymaniu żądania dla jednego z tych zasobów, który powinien zostać obsłużony przez router Blazor, błąd *404-nie znaleziono* przez usługę BLOB Service kieruje żądanie do **ścieżki dokumentu błędu**.</span><span class="sxs-lookup"><span data-stu-id="27be4-247">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="27be4-248">Zwracany jest obiekt BLOB *index. html* , a router Blazor ładuje i przetwarza ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="27be4-248">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="27be4-249">Aby uzyskać więcej informacji, zobacz [Obsługa statycznej witryny sieci Web w usłudze Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="27be4-249">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="27be4-250">Nginx</span><span class="sxs-lookup"><span data-stu-id="27be4-250">Nginx</span></span>

<span data-ttu-id="27be4-251">Następujący plik *Nginx. conf* został uproszczony, aby pokazać, jak skonfigurować Nginx do wysyłania pliku *index. html* za każdym razem, gdy nie można znaleźć odpowiedniego pliku na dysku.</span><span class="sxs-lookup"><span data-stu-id="27be4-251">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

<span data-ttu-id="27be4-252">Aby uzyskać więcej informacji na temat konfiguracji serwera sieci Web w środowisku produkcyjnym, zobacz [Tworzenie plików konfiguracji Nginx Plus i Nginx](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="27be4-252">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="27be4-253">Nginx w Docker</span><span class="sxs-lookup"><span data-stu-id="27be4-253">Nginx in Docker</span></span>

<span data-ttu-id="27be4-254">Aby hostować Blazor w platformie Docker przy użyciu Nginx, skonfiguruj pliku dockerfile do korzystania z obrazu Nginx opartego na Alpine.</span><span class="sxs-lookup"><span data-stu-id="27be4-254">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="27be4-255">Zaktualizuj pliku dockerfile, aby skopiować plik *Nginx. config* do kontenera.</span><span class="sxs-lookup"><span data-stu-id="27be4-255">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="27be4-256">Dodaj jeden wiersz do pliku dockerfile, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="27be4-256">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="27be4-257">Strony serwisu GitHub</span><span class="sxs-lookup"><span data-stu-id="27be4-257">GitHub Pages</span></span>

<span data-ttu-id="27be4-258">Aby obsłużyć ponowne zapisywanie adresów URL, Dodaj plik *404. html* ze skryptem, który obsługuje przekierowywanie żądania do strony *index. html* .</span><span class="sxs-lookup"><span data-stu-id="27be4-258">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="27be4-259">Aby zapoznać się z przykładową implementacją dostarczoną przez społeczność, zobacz [aplikacje jednostronicowe dla stron usługi GitHub](https://spa-github-pages.rafrex.com/) ([rafrex/Spa-GitHub-Pages w witrynie GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="27be4-259">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="27be4-260">Przykład użycia podejścia społecznościowego można znaleźć w witrynie GitHub[(](https://blazor-demo.github.io/) [blazor — Demonstracja/blazor-Demonstracja](https://github.com/blazor-demo/blazor-demo.github.io) ).</span><span class="sxs-lookup"><span data-stu-id="27be4-260">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="27be4-261">W przypadku korzystania z witryny projektu zamiast witryny organizacji Dodaj lub zaktualizuj `<base>` tag w *pliku index. html*.</span><span class="sxs-lookup"><span data-stu-id="27be4-261">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="27be4-262">Ustaw wartość `my-repository/`atrybutu na nazwę repozytorium GitHub z końcowym ukośnikiem (na przykład. `href`</span><span class="sxs-lookup"><span data-stu-id="27be4-262">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
