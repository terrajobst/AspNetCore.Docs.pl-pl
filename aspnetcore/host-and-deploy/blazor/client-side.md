---
title: Hostowanie i wdrażanie Blazor po stronie klienta
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie aplikacji Blazor przy użyciu platformy ASP.NET Core, sieci dostarczania zawartości (CDN), serwery plików i stron w witrynie GitHub.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: ea8ece266809913e32ac212bc55cb3c2499c234f
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874975"
---
# <a name="host-and-deploy-blazor-client-side"></a><span data-ttu-id="e24c8-103">Hostowanie i wdrażanie Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="e24c8-103">Host and deploy Blazor client-side</span></span>

<span data-ttu-id="e24c8-104">Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="e24c8-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="e24c8-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="e24c8-105">Host configuration values</span></span>

<span data-ttu-id="e24c8-106">Blazor aplikacje, które używają [modelu hostingu po stronie klienta](xref:blazor/hosting-models#client-side) może akceptować następujące wartości konfiguracji hosta jako argumenty wiersza polecenia w czasie wykonywania w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="e24c8-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="e24c8-107">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="e24c8-107">Content Root</span></span>

<span data-ttu-id="e24c8-108">`--contentroot` Argument ustawia ścieżkę bezwzględną do katalogu, który zawiera pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="e24c8-109">W poniższych przykładach `/content-root-path` jest ścieżką zawartości katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="e24c8-110">Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="e24c8-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="e24c8-111">W katalogu aplikacji wykonaj polecenie:</span><span class="sxs-lookup"><span data-stu-id="e24c8-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="e24c8-112">Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu.</span><span class="sxs-lookup"><span data-stu-id="e24c8-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="e24c8-113">To ustawienie jest używane, gdy aplikacja jest uruchamiana za pomocą debugera programu Visual Studio i w wierszu polecenia za pomocą `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e24c8-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="e24c8-114">W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="e24c8-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="e24c8-115">Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="e24c8-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="e24c8-116">Podstawa ścieżki</span><span class="sxs-lookup"><span data-stu-id="e24c8-116">Path base</span></span>

<span data-ttu-id="e24c8-117">`--pathbase` Argument określa podstawową ścieżkę aplikacji dla aplikacji, które działają lokalnie z innego niż główny ścieżki wirtualnej ( `<base>` tag `href` ustawiono ścieżki innego niż `/` dla środowisk przejściowych i produkcyjnych).</span><span class="sxs-lookup"><span data-stu-id="e24c8-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="e24c8-118">W poniższych przykładach `/virtual-path` jest podstawowa ścieżka aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="e24c8-119">Aby uzyskać więcej informacji, zobacz [ścieżki podstawowej aplikacji](#app-base-path) sekcji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e24c8-120">W odróżnieniu od podana ścieżka do `href` z `<base>` tag, nie dołączaj końcowy ukośnik (`/`) podczas przekazywania `--pathbase` wartość argumentu.</span><span class="sxs-lookup"><span data-stu-id="e24c8-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="e24c8-121">Jeśli ścieżka podstawowa aplikacja znajduje się w `<base>` otaguj jako `<base href="/CoolApp/">` (w tym znakiem ukośnika), Przekaż wartość argumentu wiersza polecenia jako `--pathbase=/CoolApp` (nie ukośnikiem końcowym).</span><span class="sxs-lookup"><span data-stu-id="e24c8-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="e24c8-122">Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="e24c8-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="e24c8-123">W katalogu aplikacji wykonaj polecenie:</span><span class="sxs-lookup"><span data-stu-id="e24c8-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="e24c8-124">Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu.</span><span class="sxs-lookup"><span data-stu-id="e24c8-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="e24c8-125">To ustawienie jest używane podczas uruchamiania aplikacji przy użyciu debugera programu Visual Studio i w wierszu polecenia za pomocą `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e24c8-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="e24c8-126">W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="e24c8-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="e24c8-127">Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="e24c8-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="e24c8-128">adresy URL</span><span class="sxs-lookup"><span data-stu-id="e24c8-128">URLs</span></span>

<span data-ttu-id="e24c8-129">`--urls` Argument ustawia adresów IP lub adresy hostów, porty i protokoły do nasłuchiwania żądań.</span><span class="sxs-lookup"><span data-stu-id="e24c8-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="e24c8-130">Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="e24c8-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="e24c8-131">W katalogu aplikacji wykonaj polecenie:</span><span class="sxs-lookup"><span data-stu-id="e24c8-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="e24c8-132">Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu.</span><span class="sxs-lookup"><span data-stu-id="e24c8-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="e24c8-133">To ustawienie jest używane podczas uruchamiania aplikacji przy użyciu debugera programu Visual Studio i w wierszu polecenia za pomocą `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e24c8-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="e24c8-134">W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="e24c8-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="e24c8-135">Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="e24c8-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="e24c8-136">wdrażania</span><span class="sxs-lookup"><span data-stu-id="e24c8-136">Deployment</span></span>

<span data-ttu-id="e24c8-137">Za pomocą [modelu hostingu po stronie klienta](xref:blazor/hosting-models#client-side):</span><span class="sxs-lookup"><span data-stu-id="e24c8-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="e24c8-138">Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e24c8-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="e24c8-139">Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e24c8-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="e24c8-140">Obsługiwany jest jedną z następujących strategii:</span><span class="sxs-lookup"><span data-stu-id="e24c8-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="e24c8-141">Aplikacja Blazor jest obsługiwany przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e24c8-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="e24c8-142">Ta strategia jest objęte [hostowanych wdrażanie za pomocą platformy ASP.NET Core](#hosted-deployment-with-aspnet-core) sekcji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="e24c8-143">Aplikacji Blazor jest umieszczany na statyczne hostingu serwera sieci web lub usługi, której platformy .NET nie jest używany do obsługi aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="e24c8-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="e24c8-144">Ta strategia jest objęte [wdrażania autonomicznego](#standalone-deployment) sekcji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="e24c8-145">Konfigurowanie konsolidatora</span><span class="sxs-lookup"><span data-stu-id="e24c8-145">Configure the Linker</span></span>

<span data-ttu-id="e24c8-146">Blazor wykonuje języka pośredniego (IL) łączenia każdego kompilacji, aby usunąć niepotrzebne IL z zestawów danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="e24c8-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="e24c8-147">Łączenie zestawu mogą być kontrolowane na kompilację.</span><span class="sxs-lookup"><span data-stu-id="e24c8-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="e24c8-148">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="e24c8-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="e24c8-149">Ponowne zapisywanie adresów URL poprawne routingu</span><span class="sxs-lookup"><span data-stu-id="e24c8-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="e24c8-150">Routing żądań dla elementów strony w aplikacji po stronie klienta nie jest tak proste, jak kierowania żądań do aplikacji po stronie serwera, hostowaną.</span><span class="sxs-lookup"><span data-stu-id="e24c8-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="e24c8-151">Należy wziąć pod uwagę aplikacji po stronie klienta, zawierający dwie strony:</span><span class="sxs-lookup"><span data-stu-id="e24c8-151">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="e24c8-152">**_Main.razor** &ndash; obciążeń w katalogu głównym aplikacji i zawiera łącza do strony informacje (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="e24c8-152">**_Main.razor** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="e24c8-153">**_About.razor** &ndash; o stronie.</span><span class="sxs-lookup"><span data-stu-id="e24c8-153">**_About.razor** &ndash; About page.</span></span>

<span data-ttu-id="e24c8-154">Gdy dokument domyślny aplikacji przy użyciu paska adresu w przeglądarce (na przykład `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="e24c8-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="e24c8-155">Żąda przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e24c8-155">The browser makes a request.</span></span>
1. <span data-ttu-id="e24c8-156">Domyślna strona ma zostać zwrócona, co jest zazwyczaj *index.html*.</span><span class="sxs-lookup"><span data-stu-id="e24c8-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="e24c8-157">*index.HTML* używa do ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="e24c8-158">Router firmy Blazor obciążenia i strony Razor Main (*Main.razor*) jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="e24c8-158">Blazor's router loads, and the Razor Main page (*Main.razor*) is displayed.</span></span>

<span data-ttu-id="e24c8-159">Na stronie głównej wybierając link do strony informacje ładuje stronę informacje.</span><span class="sxs-lookup"><span data-stu-id="e24c8-159">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="e24c8-160">Wybierając link do strony informacje działa na komputerze klienckim, ponieważ Blazor router zatrzymuje przeglądarki z żądania w Internecie, aby `www.contoso.com` dla `About` i służy sama strona informacje.</span><span class="sxs-lookup"><span data-stu-id="e24c8-160">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="e24c8-161">Wszystkie żądania dla wewnętrznej stron *w aplikacji po stronie klienta* działają tak samo: Żądania nie wyzwalacza opartego na przeglądarce żądania hostowany serwer zasobów w Internecie.</span><span class="sxs-lookup"><span data-stu-id="e24c8-161">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="e24c8-162">Wewnętrznie obsługuje żądania przez router.</span><span class="sxs-lookup"><span data-stu-id="e24c8-162">The router handles the requests internally.</span></span>

<span data-ttu-id="e24c8-163">Jeśli żądanie zostało nawiązane za pomocą paska adresu w przeglądarce dla `www.contoso.com/About`, żądanie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="e24c8-163">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="e24c8-164">Żaden z tych zasobów istnieje na hoście Internet aplikacji, więc *404 — Nie można odnaleźć* zwróceniem odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e24c8-164">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="e24c8-165">Ponieważ przeglądarki wysyłać żądania do internetowego hostów dla stron po stronie klienta, serwery sieci web i usług hostingu należy przepisać wszystkie żądania dotyczące zasobów bez fizycznego na serwerze, aby *index.html* strony.</span><span class="sxs-lookup"><span data-stu-id="e24c8-165">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="e24c8-166">Gdy *index.html* jest zwracany przez aplikację klienta routera przejmuje i odpowiada za pomocą odpowiedniego zasobu.</span><span class="sxs-lookup"><span data-stu-id="e24c8-166">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="e24c8-167">Podstawowa ścieżka aplikacji</span><span class="sxs-lookup"><span data-stu-id="e24c8-167">App base path</span></span>

<span data-ttu-id="e24c8-168">*Ścieżki podstawowej aplikacji* to ścieżka katalogu głównego aplikacji wirtualnej na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e24c8-168">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="e24c8-169">Na przykład aplikację, która znajduje się na serwerze Contoso folder wirtualny o `/CoolApp/` zostanie osiągnięty w `https://www.contoso.com/CoolApp` i ma wirtualnej ścieżki podstawowej z `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="e24c8-169">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="e24c8-170">Ustawiając ścieżki podstawowej aplikacji na ścieżkę wirtualną (`<base href="/CoolApp/">`), aplikacja jest powiadamianym o którym praktycznie znajduje się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e24c8-170">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="e24c8-171">Aplikacja może używać ścieżki podstawowej aplikacji do tworzenia adresów URL, względem katalogu głównego aplikacji ze składnika, który nie znajduje się w katalogu głównym.</span><span class="sxs-lookup"><span data-stu-id="e24c8-171">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="e24c8-172">Dzięki temu składniki, które istnieją na różnych poziomach struktury katalogów, tworzenie łączy do innych zasobów w lokalizacjach w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-172">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="e24c8-173">Ścieżka podstawowa aplikacja jest również używane do przechwycenia hiperłącze kliknie gdzie `href` miejsce docelowe łącza jest w ramach ścieżki podstawowej aplikacji identyfikator URI przestrzeni&mdash;routera Blazor obsługuje wewnętrzny nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-173">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="e24c8-174">W wielu scenariuszach hostingu serwer ścieżka wirtualna do aplikacji jest głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-174">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="e24c8-175">W takich przypadkach ścieżki podstawowej aplikacji jest ukośnikiem (`<base href="/" />`), która jest domyślna konfiguracja dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-175">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="e24c8-176">W innych scenariuszach hostingu, takich jak katalogi wirtualne stronach GitHub oraz usług IIS lub aplikacje podrzędne ścieżki podstawowej aplikacji należy określić ścieżkę wirtualną serwera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-176">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="e24c8-177">Aby ustawić ścieżki podstawowej aplikacji, należy zaktualizować `<base>` tagów w ramach `<head>` tagów elementów *wwwroot/index.html* pliku.</span><span class="sxs-lookup"><span data-stu-id="e24c8-177">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="e24c8-178">Ustaw `href` wartość do atrybutu `/virtual-path/` (wymagane jest podanie końcowy ukośnik), gdzie `/virtual-path/` to ścieżka katalogu głównego aplikacji pełna wirtualna na serwerze aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-178">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="e24c8-179">W powyższym przykładzie, ścieżka wirtualna jest ustawiona na `/CoolApp/`: `<base href="/CoolApp/">`.</span><span class="sxs-lookup"><span data-stu-id="e24c8-179">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="e24c8-180">Dla aplikacji za pomocą innego niż główny ścieżki wirtualnej skonfigurowane (na przykład `<base href="/CoolApp/">`), aplikacja nie może odnaleźć swoich zasobów *uruchomienia lokalnie*.</span><span class="sxs-lookup"><span data-stu-id="e24c8-180">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="e24c8-181">Aby rozwiązać ten problem, podczas lokalnego opracowywania i testowania, możesz podać *podstawa ścieżki* argument, który odpowiada `href` wartość `<base>` tagu w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e24c8-181">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="e24c8-182">Aby przekazać argument podstawowa ścieżka o ścieżce katalogu głównego (`/`) podczas uruchamiania aplikacji lokalnie, należy wykonać `dotnet run` polecenie z katalogu aplikacji z `--pathbase` — opcja:</span><span class="sxs-lookup"><span data-stu-id="e24c8-182">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="e24c8-183">Dla aplikacji za pomocą wirtualnej ścieżki podstawowej z `/CoolApp/` (`<base href="/CoolApp/">`), to polecenie:</span><span class="sxs-lookup"><span data-stu-id="e24c8-183">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="e24c8-184">Aplikacja reaguje lokalnie na `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="e24c8-184">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="e24c8-185">Aby uzyskać więcej informacji, zobacz sekcję na [wartość konfiguracji podstawowej hosta ścieżki](#path-base).</span><span class="sxs-lookup"><span data-stu-id="e24c8-185">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="e24c8-186">Jeśli aplikacja używa [modelu hostingu po stronie klienta](xref:blazor/hosting-models#client-side) (na podstawie **Blazor** szablonu projektu; `blazor` szablon podczas korzystania [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia), a jest obsługiwana jako aplikacja podrzędnych usług IIS w aplikacji ASP.NET Core, ważne jest, aby wyłączyć dziedziczone obsługi modułu ASP.NET Core lub upewnić się, że aplikacja głównego (nadrzędnego) `<handlers>` sekcji *web.config* pliku nie jest dziedziczone przez Sub — aplikacja.</span><span class="sxs-lookup"><span data-stu-id="e24c8-186">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor** project template; the `blazor` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="e24c8-187">Usuń procedurę obsługi w aplikacji — opublikowane *web.config* pliku, dodając `<handlers>` sekcji w pliku:</span><span class="sxs-lookup"><span data-stu-id="e24c8-187">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="e24c8-188">Alternatywnie wyłączyć funkcję dziedziczenia aplikacji głównej (nadrzędnej) `<system.webServer>` sekcji przy użyciu `<location>` element z `inheritInChildApplications` równa `false`:</span><span class="sxs-lookup"><span data-stu-id="e24c8-188">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="e24c8-189">Usuwanie obsługi lub wyłączając dziedziczenie jest wykonywane Oprócz konfigurowania ścieżki podstawowej aplikacji, zgodnie z opisem w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-189">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="e24c8-190">Ustawianie ścieżki podstawowej aplikacji w aplikacji *index.html* pliku do aliasu usług IIS, używane podczas konfigurowania aplikacji podrzędnej w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="e24c8-190">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="e24c8-191">Wdrożenie hostowane za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e24c8-191">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="e24c8-192">A *hostowanych wdrożenia* służy aplikacja Blazor po stronie klienta do przeglądarki z [aplikacji ASP.NET Core](xref:index) , które jest uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e24c8-192">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="e24c8-193">Aplikacja Blazor jest dołączone do aplikacji platformy ASP.NET Core w opublikowanych danych wyjściowych, więc, że dwie aplikacje wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="e24c8-193">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="e24c8-194">Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e24c8-194">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="e24c8-195">W przypadku wdrożenia hostowanego obejmuje program Visual Studio **Blazor (platformy ASP.NET Core, obsługiwane)** szablonu projektu (`blazorhosted` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia).</span><span class="sxs-lookup"><span data-stu-id="e24c8-195">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="e24c8-196">Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="e24c8-196">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="e24c8-197">Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="e24c8-197">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="e24c8-198">Wdrażania autonomicznego</span><span class="sxs-lookup"><span data-stu-id="e24c8-198">Standalone deployment</span></span>

<span data-ttu-id="e24c8-199">A *wdrażania autonomicznego* służy aplikacja Blazor po stronie klienta jako zbiór plików statycznych, które są żądane bezpośrednio przez klientów.</span><span class="sxs-lookup"><span data-stu-id="e24c8-199">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="e24c8-200">Serwer sieci web nie jest używany do obsługi aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="e24c8-200">A web server isn't used to serve the Blazor app.</span></span>

### <a name="iis"></a><span data-ttu-id="e24c8-201">IIS</span><span class="sxs-lookup"><span data-stu-id="e24c8-201">IIS</span></span>

<span data-ttu-id="e24c8-202">Program IIS jest serwerem plików statycznych z możliwością Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-202">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="e24c8-203">Aby skonfigurować usługi IIS do hosta Blazor, zobacz [Tworzenie statycznej witryny sieci Web w usługach IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="e24c8-203">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="e24c8-204">Opublikowane zasoby są tworzone w */bin/wersji / {struktury docelowej} / publish* folderu.</span><span class="sxs-lookup"><span data-stu-id="e24c8-204">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="e24c8-205">Hostowanie zawartości *publikowania* folderu na serwerze sieci web lub innej usługi hostingu.</span><span class="sxs-lookup"><span data-stu-id="e24c8-205">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="e24c8-206">web.config</span><span class="sxs-lookup"><span data-stu-id="e24c8-206">web.config</span></span>

<span data-ttu-id="e24c8-207">Po opublikowaniu projektu Blazor *web.config* plik jest tworzony przy użyciu następującej konfiguracji usług IIS:</span><span class="sxs-lookup"><span data-stu-id="e24c8-207">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="e24c8-208">Typy MIME są ustawiane dla następujących rozszerzeń pliku:</span><span class="sxs-lookup"><span data-stu-id="e24c8-208">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="e24c8-209">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="e24c8-209">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="e24c8-210">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="e24c8-210">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="e24c8-211">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="e24c8-211">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="e24c8-212">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="e24c8-212">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="e24c8-213">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="e24c8-213">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="e24c8-214">Kompresja protokołu HTTP jest włączone dla następujących typów MIME:</span><span class="sxs-lookup"><span data-stu-id="e24c8-214">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="e24c8-215">Ustanawiane są reguły moduł ponowne zapisywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="e24c8-215">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="e24c8-216">Obsługiwać podkatalogu, gdzie znajdują się zasoby statyczne aplikacji (*/dist/ {Nazwa zestawu} {ŻĄDANA ŚCIEŻKA}*).</span><span class="sxs-lookup"><span data-stu-id="e24c8-216">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="e24c8-217">Utwórz SPA rezerwowego routingu, tak aby żądania innego niż plik zasobów są przekierowywane do aplikacji dokumentu domyślnego w folderze statycznych zasobów (*{NAME}/dist/index.html zestawu*).</span><span class="sxs-lookup"><span data-stu-id="e24c8-217">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="e24c8-218">Zainstaluj moduł ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="e24c8-218">Install the URL Rewrite Module</span></span>

<span data-ttu-id="e24c8-219">[Moduł ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite) jest wymagany do ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="e24c8-219">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="e24c8-220">Moduł nie jest instalowany domyślnie i nie jest dostępna do zainstalowania jako funkcja usługi roli Serwer sieci Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="e24c8-220">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="e24c8-221">Moduł musi zostać pobrany z witryny sieci Web usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e24c8-221">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="e24c8-222">Aby zainstalować moduł, należy użyć Instalatora platformy sieci Web:</span><span class="sxs-lookup"><span data-stu-id="e24c8-222">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="e24c8-223">Lokalnie, przejdź do [moduł ponowne zapisywanie adresów URL strony pobierania](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="e24c8-223">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="e24c8-224">W wersji angielskiej, wybierz **WebPI** do pobierania Instalatora WebPI.</span><span class="sxs-lookup"><span data-stu-id="e24c8-224">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="e24c8-225">W przypadku języków wybierz odpowiednią architekturę dla serwera — x86/x64 64 do pobierania Instalatora.</span><span class="sxs-lookup"><span data-stu-id="e24c8-225">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="e24c8-226">Skopiuj Instalatora na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e24c8-226">Copy the installer to the server.</span></span> <span data-ttu-id="e24c8-227">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="e24c8-227">Run the installer.</span></span> <span data-ttu-id="e24c8-228">Wybierz **zainstalować** przycisk i zaakceptuj postanowienia licencyjne.</span><span class="sxs-lookup"><span data-stu-id="e24c8-228">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="e24c8-229">Ponowne uruchomienie serwera nie jest wymagane po ukończeniu instalacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-229">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="e24c8-230">Konfigurowanie witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="e24c8-230">Configure the website</span></span>

<span data-ttu-id="e24c8-231">Ustaw witrynę sieci Web **ścieżkę fizyczną** do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-231">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="e24c8-232">Folder zawiera:</span><span class="sxs-lookup"><span data-stu-id="e24c8-232">The folder contains:</span></span>

* <span data-ttu-id="e24c8-233">*Web.config* pliku wykorzystywanym przez usługi IIS do konfigurowania witryny sieci Web, łącznie z regułami wymagane przekierowania i typy zawartości plików.</span><span class="sxs-lookup"><span data-stu-id="e24c8-233">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="e24c8-234">Folder statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e24c8-234">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="e24c8-235">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="e24c8-235">Troubleshooting</span></span>

<span data-ttu-id="e24c8-236">Jeśli *500 — Wewnętrzny błąd serwera* odebraniu i Menedżera usług IIS zgłasza błędy podczas próby dostępu do konfiguracji witryny sieci Web, upewnij się, że zainstalowano moduł ponowne zapisywanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="e24c8-236">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="e24c8-237">Jeśli moduł nie jest zainstalowany, *web.config* nie można przeanalizować pliku przez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="e24c8-237">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="e24c8-238">Zapobiega to ładowania konfiguracji witryny sieci Web i witryny sieci Web z dostarczania plików statycznych dla Blazor Menedżera usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e24c8-238">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="e24c8-239">Aby uzyskać więcej informacji na temat Rozwiązywanie problemów z wdrożeniami usług IIS, zobacz <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="e24c8-239">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

### <a name="nginx"></a><span data-ttu-id="e24c8-240">Nginx</span><span class="sxs-lookup"><span data-stu-id="e24c8-240">Nginx</span></span>

<span data-ttu-id="e24c8-241">Następujące *nginx.conf* pliku została uproszczona w celu pokazują, jak skonfigurować serwer Nginx, aby wysłać *index.html* plików zawsze wtedy, gdy nie można odnaleźć odpowiedniego pliku na dysku.</span><span class="sxs-lookup"><span data-stu-id="e24c8-241">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="e24c8-242">Aby uzyskać więcej informacji na temat konfiguracji serwera sieci web Nginx produkcji, zobacz [tworzenia serwer NGINX Plus i pliki konfiguracji NGINX](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="e24c8-242">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="e24c8-243">Serwer Nginx na platformie Docker</span><span class="sxs-lookup"><span data-stu-id="e24c8-243">Nginx in Docker</span></span>

<span data-ttu-id="e24c8-244">Aby hostować Blazor na platformie Docker przy użyciu serwera Nginx, Konfiguracja pliku Dockerfile, aby użyć obrazu Alpine na podstawie serwera Nginx.</span><span class="sxs-lookup"><span data-stu-id="e24c8-244">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="e24c8-245">Aktualizacja pliku Dockerfile, aby skopiować *nginx.config* pliku do kontenera.</span><span class="sxs-lookup"><span data-stu-id="e24c8-245">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="e24c8-246">Dodaj jeden wiersz do pliku Dockerfile, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="e24c8-246">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="e24c8-247">Strony w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="e24c8-247">GitHub Pages</span></span>

<span data-ttu-id="e24c8-248">Aby obsłużyć ponownego adresu URL, Dodaj *404. html* pliku ze skryptem, który obsługuje żądanie przekierowania *index.html* strony.</span><span class="sxs-lookup"><span data-stu-id="e24c8-248">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="e24c8-249">Przykładem implementacji dostarczane przez społeczność, można zobaczyć [pojedynczej aplikacji strony dla strony GitHub](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github strony w witrynie GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="e24c8-249">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="e24c8-250">Przykładem dotyczącym używania podejścia społeczności są widoczne w [blazor-demo/blazor-demo.github.io w serwisie GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([aktywnej witryny](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="e24c8-250">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="e24c8-251">Korzystając z witryny projektu zamiast witryny organizacji, Dodaj lub zaktualizuj `<base>` tagów w *index.html*.</span><span class="sxs-lookup"><span data-stu-id="e24c8-251">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="e24c8-252">Ustaw `href` wartość atrybutu Nazwa repozytorium GitHub kończących się ukośnikiem (na przykład `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="e24c8-252">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
