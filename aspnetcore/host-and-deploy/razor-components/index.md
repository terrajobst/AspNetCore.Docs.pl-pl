---
title: Hostowanie i wdrażanie składników Razor
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie składników Razor i Blazor aplikacje przy użyciu platformy ASP.NET Core, sieci dostarczania zawartości (CDN), serwery plików i stron w witrynie GitHub.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/index
ms.openlocfilehash: 9debd75128ceecb805fc673a8182a785fc9f7942
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668189"
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="c2139-103">Hostowanie i wdrażanie składników Razor</span><span class="sxs-lookup"><span data-stu-id="c2139-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="c2139-104">Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c2139-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="c2139-105">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c2139-105">Publish the app</span></span>

<span data-ttu-id="c2139-106">Aplikacje są publikowane do wdrożenia w konfiguracji wydania przy użyciu [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2139-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="c2139-107">Środowisko IDE może obsługiwać wykonywanie `dotnet publish` polecenia automatycznie przy użyciu jego wbudowanych funkcji publikowania, dzięki czemu może nie być konieczne ręczne wykonaj polecenie w wierszu polecenia, w zależności od poziomu narzędzi deweloperskich w użyciu.</span><span class="sxs-lookup"><span data-stu-id="c2139-107">An IDE may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="c2139-108">`dotnet publish` Wyzwalacze [przywrócić](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompilacje](/dotnet/core/tools/dotnet-build) projekcie przed utworzeniem zasobów dla wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="c2139-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="c2139-109">Jako część procesu kompilacji Aby zmniejszyć rozmiar pobierania aplikacji i czasów ładowania są usuwane nieużywane metod i zestawów.</span><span class="sxs-lookup"><span data-stu-id="c2139-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="c2139-110">Wdrożenie jest tworzony w */bin/wersji/&lt;platformę docelową&gt;/ publish* folderu.</span><span class="sxs-lookup"><span data-stu-id="c2139-110">The deployment is created in the */bin/Release/&lt;target-framework&gt;/publish* folder.</span></span>

<span data-ttu-id="c2139-111">Zasoby w *publikowania* folderu wdrożonych na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="c2139-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="c2139-112">Wdrożenie może być ręcznego lub zautomatyzowanego procesu w zależności od poziomu narzędzi deweloperskich w użyciu.</span><span class="sxs-lookup"><span data-stu-id="c2139-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="c2139-113">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="c2139-113">Host configuration values</span></span>

<span data-ttu-id="c2139-114">Razor składniki aplikacje, które używają [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model) może akceptować [wartości konfiguracji hosta sieci Web](xref:fundamentals/host/web-host#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="c2139-114">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Web Host configuration values](xref:fundamentals/host/web-host#host-configuration-values).</span></span>

<span data-ttu-id="c2139-115">Blazor aplikacje, które używają [modelu hostingu po stronie klienta](xref:razor-components/hosting-models#client-side-hosting-model) może akceptować następujące wartości konfiguracji hosta jako argumenty wiersza polecenia w czasie wykonywania w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="c2139-115">Blazor apps that use the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="c2139-116">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="c2139-116">Content Root</span></span>

<span data-ttu-id="c2139-117">`--contentroot` Argument ustawia ścieżkę bezwzględną do katalogu, który zawiera pliki zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-117">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span>

* <span data-ttu-id="c2139-118">Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2139-118">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="c2139-119">W katalogu aplikacji wykonaj polecenie:</span><span class="sxs-lookup"><span data-stu-id="c2139-119">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* <span data-ttu-id="c2139-120">Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu.</span><span class="sxs-lookup"><span data-stu-id="c2139-120">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="c2139-121">To ustawienie jest pobierane podczas uruchamiania aplikacji przy użyciu debugera programu Visual Studio, a podczas uruchamiania aplikacji z poziomu wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c2139-121">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* <span data-ttu-id="c2139-122">W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="c2139-122">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="c2139-123">Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="c2139-123">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a><span data-ttu-id="c2139-124">Podstawa ścieżki</span><span class="sxs-lookup"><span data-stu-id="c2139-124">Path base</span></span>

<span data-ttu-id="c2139-125">`--pathbase` Argument określa podstawową ścieżkę aplikacji dla aplikacji, które działają lokalnie z innego niż główny ścieżki wirtualnej ( `<base>` tag `href` ustawiono ścieżki innego niż `/` dla środowisk przejściowych i produkcyjnych).</span><span class="sxs-lookup"><span data-stu-id="c2139-125">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="c2139-126">Aby uzyskać więcej informacji, zobacz [ścieżki podstawowej aplikacji](#app-base-path) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2139-126">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2139-127">W odróżnieniu od podana ścieżka do `href` z `<base>` tag, nie dołączaj końcowy ukośnik (`/`) podczas przekazywania `--pathbase` wartość argumentu.</span><span class="sxs-lookup"><span data-stu-id="c2139-127">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="c2139-128">Jeśli ścieżka podstawowa aplikacja znajduje się w `<base>` otaguj jako `<base href="/CoolApp/" />` (w tym znakiem ukośnika), Przekaż wartość argumentu wiersza polecenia jako `--pathbase=/CoolApp` (nie ukośnikiem końcowym).</span><span class="sxs-lookup"><span data-stu-id="c2139-128">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/" />` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="c2139-129">Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2139-129">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="c2139-130">W katalogu aplikacji wykonaj polecenie:</span><span class="sxs-lookup"><span data-stu-id="c2139-130">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* <span data-ttu-id="c2139-131">Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu.</span><span class="sxs-lookup"><span data-stu-id="c2139-131">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="c2139-132">To ustawienie jest pobierane podczas uruchamiania aplikacji przy użyciu debugera programu Visual Studio, a podczas uruchamiania aplikacji z poziomu wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c2139-132">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* <span data-ttu-id="c2139-133">W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="c2139-133">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="c2139-134">Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="c2139-134">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a><span data-ttu-id="c2139-135">adresy URL</span><span class="sxs-lookup"><span data-stu-id="c2139-135">URLs</span></span>

<span data-ttu-id="c2139-136">`--urls` Argument wskazuje adresy IP lub adresy hostów, porty i protokoły do nasłuchiwania żądań.</span><span class="sxs-lookup"><span data-stu-id="c2139-136">The `--urls` argument indicates the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="c2139-137">Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="c2139-137">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="c2139-138">W katalogu aplikacji wykonaj polecenie:</span><span class="sxs-lookup"><span data-stu-id="c2139-138">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="c2139-139">Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu.</span><span class="sxs-lookup"><span data-stu-id="c2139-139">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="c2139-140">To ustawienie jest pobierane podczas uruchamiania aplikacji przy użyciu debugera programu Visual Studio, a podczas uruchamiania aplikacji z poziomu wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c2139-140">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="c2139-141">W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="c2139-141">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="c2139-142">Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="c2139-142">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a><span data-ttu-id="c2139-143">Wdrażanie aplikacji Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="c2139-143">Deploy a client-side Blazor app</span></span>

<span data-ttu-id="c2139-144">Za pomocą [modelu hostingu po stronie klienta](xref:razor-components/hosting-models#client-side-hosting-model):</span><span class="sxs-lookup"><span data-stu-id="c2139-144">With the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="c2139-145">Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c2139-145">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="c2139-146">Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2139-146">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="c2139-147">Obsługiwany jest jedną z następujących strategii:</span><span class="sxs-lookup"><span data-stu-id="c2139-147">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="c2139-148">Aplikacja Blazor jest obsługiwany przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2139-148">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="c2139-149">Objęte [Blazor po stronie klienta hostowane wdrażanie za pomocą platformy ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2139-149">Covered in the [Client-side Blazor hosted deployment with ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="c2139-150">Aplikacji Blazor jest umieszczany na statyczne hostingu serwera sieci web lub usługi, której platformy .NET nie jest używany do obsługi aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="c2139-150">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="c2139-151">Objęte [wdrażania autonomicznego Blazor po stronie klienta](#client-side-blazor-standalone-deployment) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2139-151">Covered in the [Client-side Blazor standalone deployment](#client-side-blazor-standalone-deployment) section.</span></span>
  
### <a name="configure-the-linker"></a><span data-ttu-id="c2139-152">Konfigurowanie konsolidatora</span><span class="sxs-lookup"><span data-stu-id="c2139-152">Configure the Linker</span></span>

<span data-ttu-id="c2139-153">Blazor wykonuje języka pośredniego (IL) łączenia każdego kompilacji, aby usunąć niepotrzebne IL z zestawów danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="c2139-153">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="c2139-154">Można kontrolować łączenie podczas kompilowania zestawu.</span><span class="sxs-lookup"><span data-stu-id="c2139-154">You can control assembly linking on build.</span></span> <span data-ttu-id="c2139-155">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/razor-components/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="c2139-155">For more information, see <xref:host-and-deploy/razor-components/configure-linker>.</span></span>

### <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="c2139-156">Ponowne zapisywanie adresów URL poprawne routingu</span><span class="sxs-lookup"><span data-stu-id="c2139-156">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="c2139-157">Routing żądań dla elementów strony w aplikacji po stronie klienta nie jest tak proste, jak kierowania żądań do aplikacji po stronie serwera, hostowaną.</span><span class="sxs-lookup"><span data-stu-id="c2139-157">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="c2139-158">Należy wziąć pod uwagę aplikacji po stronie klienta, zawierający dwie strony:</span><span class="sxs-lookup"><span data-stu-id="c2139-158">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="c2139-159">**_Main.cshtml_**  &ndash; obciążeń w katalogu głównym aplikacji i zawiera łącza do strony informacje (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="c2139-159">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="c2139-160">**_About.cshtml_**  &ndash; o stronie.</span><span class="sxs-lookup"><span data-stu-id="c2139-160">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="c2139-161">Gdy dokument domyślny aplikacji przy użyciu paska adresu w przeglądarce (na przykład `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="c2139-161">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="c2139-162">Żąda przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c2139-162">The browser makes a request.</span></span>
1. <span data-ttu-id="c2139-163">Domyślna strona ma zostać zwrócona, co jest zazwyczaj *index.html*.</span><span class="sxs-lookup"><span data-stu-id="c2139-163">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="c2139-164">*index.HTML* używa do ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-164">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="c2139-165">Router firmy Blazor obciążeń i strony Razor Main (*Main.cshtml*) jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="c2139-165">Blazor's router loads and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="c2139-166">Na stronie głównej wybierając link do strony informacje ładuje stronę informacje.</span><span class="sxs-lookup"><span data-stu-id="c2139-166">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="c2139-167">Wybierając link do strony informacje działa na komputerze klienckim, ponieważ Blazor router zatrzymuje przeglądarki z żądania w Internecie, aby `www.contoso.com` dla `About` i służy sama strona informacje.</span><span class="sxs-lookup"><span data-stu-id="c2139-167">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="c2139-168">Wszystkie żądania dla wewnętrznej stron *w aplikacji po stronie klienta* działają tak samo: Żądania nie wyzwalacza opartego na przeglądarce żądania hostowany serwer zasobów w Internecie.</span><span class="sxs-lookup"><span data-stu-id="c2139-168">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="c2139-169">Wewnętrznie obsługuje żądania przez router.</span><span class="sxs-lookup"><span data-stu-id="c2139-169">The router handles the requests internally.</span></span>

<span data-ttu-id="c2139-170">Jeśli żądanie zostało nawiązane za pomocą paska adresu w przeglądarce dla `www.contoso.com/About`, żądanie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="c2139-170">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="c2139-171">Żaden z tych zasobów istnieje na hoście Internet aplikacji, więc *404 Nie znaleziono* zwróceniem odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="c2139-171">No such resource exists on the app's Internet host, so a *404 Not found* response is returned.</span></span>

<span data-ttu-id="c2139-172">Ponieważ przeglądarki wysyłać żądania do internetowego hostów dla stron po stronie klienta, serwery sieci web i usług hostingu należy przepisać wszystkie żądania dotyczące zasobów bez fizycznego na serwerze, aby *index.html* strony.</span><span class="sxs-lookup"><span data-stu-id="c2139-172">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="c2139-173">Gdy *index.html* jest zwracany przez aplikację klienta routera przejmuje i odpowiada za pomocą odpowiedniego zasobu.</span><span class="sxs-lookup"><span data-stu-id="c2139-173">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

### <a name="app-base-path"></a><span data-ttu-id="c2139-174">Podstawowa ścieżka aplikacji</span><span class="sxs-lookup"><span data-stu-id="c2139-174">App base path</span></span>

<span data-ttu-id="c2139-175">Ścieżka podstawowa aplikacja jest ścieżka katalogu głównego aplikacji wirtualnej na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c2139-175">The app base path is the virtual app root path on the server.</span></span> <span data-ttu-id="c2139-176">Na przykład aplikację, która znajduje się na serwerze Contoso folder wirtualny o `/CoolApp/` zostanie osiągnięty w `https://www.contoso.com/CoolApp` i ma wirtualnej ścieżki podstawowej z `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="c2139-176">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="c2139-177">Ustawiając ścieżki podstawowej aplikacji `CoolApp/`, aplikacja jest powiadamianym o którym praktycznie znajduje się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c2139-177">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="c2139-178">Aplikacja może używać ścieżki podstawowej aplikacji do tworzenia adresów URL, względem katalogu głównego aplikacji ze składnika, który nie znajduje się w katalogu głównym.</span><span class="sxs-lookup"><span data-stu-id="c2139-178">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="c2139-179">Dzięki temu składniki, które istnieją na różnych poziomach struktury katalogów, tworzenie łączy do innych zasobów w lokalizacjach w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-179">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="c2139-180">Ścieżka podstawowa aplikacja jest również używane do przechwycenia hiperłącze kliknie gdzie `href` miejsce docelowe łącza jest w ramach ścieżki podstawowej aplikacji identyfikator URI przestrzeni&mdash;routera Blazor obsługuje wewnętrzny nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-180">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="c2139-181">W wielu scenariuszach hostingu serwer ścieżka wirtualna do aplikacji jest głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-181">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="c2139-182">W takich przypadkach ścieżki podstawowej aplikacji jest ukośnikiem (`<base href="/" />`), która jest domyślna konfiguracja dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-182">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="c2139-183">W innych scenariuszach hostingu, takich jak katalogi wirtualne stronach GitHub oraz usług IIS lub aplikacje podrzędne ścieżki podstawowej aplikacji należy określić ścieżkę wirtualną serwera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-183">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="c2139-184">Aby ustawić ścieżki podstawowej aplikacji, dodać lub zaktualizować `<base>` tagów w *index.html* można znaleźć w `<head>` tagów elementów.</span><span class="sxs-lookup"><span data-stu-id="c2139-184">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="c2139-185">Ustaw `href` wartość do atrybutu `<virtual-path>/` (wymagane jest podanie końcowy ukośnik), gdzie `<virtual-path>/` to ścieżka katalogu głównego aplikacji pełna wirtualna na serwerze aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-185">Set the `href` attribute value to `<virtual-path>/` (the trailing slash is required), where `<virtual-path>/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="c2139-186">W powyższym przykładzie, ścieżka wirtualna jest ustawiona na `CoolApp/`: `<base href="CoolApp/" />`.</span><span class="sxs-lookup"><span data-stu-id="c2139-186">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/" />`.</span></span>

<span data-ttu-id="c2139-187">Dla aplikacji za pomocą innego niż główny ścieżki wirtualnej skonfigurowane (na przykład `<base href="CoolApp/" />`), aplikacja nie może odnaleźć swoich zasobów *uruchomienia lokalnie*.</span><span class="sxs-lookup"><span data-stu-id="c2139-187">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/" />`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="c2139-188">Aby rozwiązać ten problem, podczas lokalnego opracowywania i testowania, możesz podać *podstawa ścieżki* argument, który odpowiada `href` wartość `<base>` tagu w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c2139-188">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="c2139-189">Aby przekazać argument podstawowa ścieżka o ścieżce katalogu głównego (`/`) podczas uruchamiania aplikacji lokalnie, uruchom następujące polecenie z katalogu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c2139-189">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="c2139-190">Aplikacja reaguje lokalnie na `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="c2139-190">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="c2139-191">Aby uzyskać więcej informacji, zobacz [wartość konfiguracji podstawowej hosta ścieżki](#path-base) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2139-191">For more information, see the [path base host configuration value](#path-base) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2139-192">Jeśli aplikacja używa [modelu hostingu po stronie klienta](xref:razor-components/hosting-models#client-side-hosting-model) (na podstawie **Blazor** szablon projektu) i jest hostowany jako aplikacja podrzędnych usług IIS w aplikacji ASP.NET Core, ważne jest, aby wyłączyć dziedziczone platformy ASP.NET Core Obsługi modułu lub upewnić się, że aplikacja głównego (nadrzędnego) `<handlers>` sekcji *web.config* pliku nie jest dziedziczona przez jej podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="c2139-192">If an app uses the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>
>
> <span data-ttu-id="c2139-193">Usuń procedurę obsługi w aplikacji — opublikowane *web.config* pliku, dodając `<handlers>` sekcji w pliku:</span><span class="sxs-lookup"><span data-stu-id="c2139-193">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> <span data-ttu-id="c2139-194">Alternatywnie wyłączyć funkcję dziedziczenia aplikacji głównej (nadrzędnej) `<system.webServer>` sekcji przy użyciu `<location>` element z `inheritInChildApplications` równa `false`:</span><span class="sxs-lookup"><span data-stu-id="c2139-194">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> <span data-ttu-id="c2139-195">Usuwanie obsługi lub wyłączając dziedziczenie jest wykonywane Oprócz konfigurowania ścieżki podstawowej aplikacji, zgodnie z opisem w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2139-195">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="c2139-196">Ustawianie ścieżki podstawowej aplikacji w aplikacji *index.html* pliku do aliasu usług IIS, używane podczas konfigurowania aplikacji podrzędnej w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="c2139-196">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a><span data-ttu-id="c2139-197">Wdrażanie Blazor hostowanej po stronie klienta za pomocą platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2139-197">Client-side Blazor hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="c2139-198">A *hostowanych wdrożenia* służy aplikacja Blazor po stronie klienta do przeglądarki z [aplikacji ASP.NET Core](xref:index) , które jest uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c2139-198">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="c2139-199">Aplikacja Blazor jest dołączone do aplikacji platformy ASP.NET Core w opublikowanych danych wyjściowych, więc, że dwie aplikacje wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="c2139-199">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="c2139-200">Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2139-200">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="c2139-201">W przypadku wdrożenia hostowanego obejmuje program Visual Studio **Blazor (platformy ASP.NET Core, obsługiwane)** szablonu projektu (`blazorhosted` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia).</span><span class="sxs-lookup"><span data-stu-id="c2139-201">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="c2139-202">Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="c2139-202">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="c2139-203">Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="c2139-203">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="c2139-204">Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2139-204">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

### <a name="client-side-blazor-standalone-deployment"></a><span data-ttu-id="c2139-205">Wdrażania autonomicznego Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="c2139-205">Client-side Blazor standalone deployment</span></span>

<span data-ttu-id="c2139-206">A *wdrażania autonomicznego* służy aplikacja Blazor po stronie klienta jako zbiór plików statycznych, które są żądane bezpośrednio przez klientów.</span><span class="sxs-lookup"><span data-stu-id="c2139-206">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="c2139-207">Serwer sieci web nie jest używany do obsługi aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="c2139-207">A web server isn't used to serve the Blazor app.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a><span data-ttu-id="c2139-208">Po stronie klienta Blazor autonomiczne hostowanie za pomocą programu IIS</span><span class="sxs-lookup"><span data-stu-id="c2139-208">Client-side Blazor standalone hosting with IIS</span></span>

<span data-ttu-id="c2139-209">Program IIS jest serwerem plików statycznych z możliwością Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-209">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="c2139-210">Aby skonfigurować usługi IIS do hosta Blazor, zobacz [Tworzenie statycznej witryny sieci Web w usługach IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="c2139-210">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="c2139-211">Opublikowane zasoby są tworzone w  *\\bin\\wersji\\&lt;platformę docelową&gt;\\publikowania* folderu.</span><span class="sxs-lookup"><span data-stu-id="c2139-211">Published assets are created in the *\\bin\\Release\\&lt;target-framework&gt;\\publish* folder.</span></span> <span data-ttu-id="c2139-212">Hostowanie zawartości *publikowania* folderu na serwerze sieci web lub innej usługi hostingu.</span><span class="sxs-lookup"><span data-stu-id="c2139-212">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

<span data-ttu-id="c2139-213">**web.config**</span><span class="sxs-lookup"><span data-stu-id="c2139-213">**web.config**</span></span>

<span data-ttu-id="c2139-214">Po opublikowaniu projektu Blazor *web.config* plik jest tworzony przy użyciu następującej konfiguracji usług IIS:</span><span class="sxs-lookup"><span data-stu-id="c2139-214">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="c2139-215">Typy MIME są ustawiane dla następujących rozszerzeń pliku:</span><span class="sxs-lookup"><span data-stu-id="c2139-215">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="c2139-216">*\*.dll*: `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="c2139-216">*\*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="c2139-217">*\*.json*: `application/json`</span><span class="sxs-lookup"><span data-stu-id="c2139-217">*\*.json*: `application/json`</span></span>
  * <span data-ttu-id="c2139-218">*\*.wasm*: `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="c2139-218">*\*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="c2139-219">*\*.woff*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="c2139-219">*\*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="c2139-220">*\*.woff2*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="c2139-220">*\*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="c2139-221">Kompresja protokołu HTTP jest włączone dla następujących typów MIME:</span><span class="sxs-lookup"><span data-stu-id="c2139-221">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="c2139-222">Ustanawiane są reguły moduł ponowne zapisywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="c2139-222">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="c2139-223">Obsługiwać podkatalogu, gdzie znajdują się zasoby statyczne aplikacji (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span><span class="sxs-lookup"><span data-stu-id="c2139-223">Serve the sub-directory where the app's static assets reside (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span></span>
  * <span data-ttu-id="c2139-224">Utwórz SPA rezerwowego routingu, tak aby żądania innego niż plik zasobów są przekierowywane do aplikacji dokumentu domyślnego w folderze statycznych zasobów (*&lt;assembly_name&gt;\\dist\\index.html*).</span><span class="sxs-lookup"><span data-stu-id="c2139-224">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*&lt;assembly_name&gt;\\dist\\index.html*).</span></span>

<span data-ttu-id="c2139-225">**Zainstaluj moduł ponowne zapisywanie adresów URL**</span><span class="sxs-lookup"><span data-stu-id="c2139-225">**Install the URL Rewrite Module**</span></span>

<span data-ttu-id="c2139-226">[Moduł ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite) jest wymagany do ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="c2139-226">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="c2139-227">Moduł nie jest instalowany domyślnie i nie jest dostępna do zainstalowania jako funkcja usługi roli Serwer sieci Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="c2139-227">The module isn't installed by default, and it isn't available for install as an Web Server (IIS) role service feature.</span></span> <span data-ttu-id="c2139-228">Moduł musi zostać pobrany z witryny sieci Web usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c2139-228">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="c2139-229">Aby zainstalować moduł, należy użyć Instalatora platformy sieci Web:</span><span class="sxs-lookup"><span data-stu-id="c2139-229">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="c2139-230">Lokalnie, przejdź do [moduł ponowne zapisywanie adresów URL strony pobierania](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="c2139-230">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="c2139-231">W wersji angielskiej, wybierz **WebPI** do pobierania Instalatora WebPI.</span><span class="sxs-lookup"><span data-stu-id="c2139-231">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="c2139-232">W przypadku języków wybierz odpowiednią architekturę dla serwera — x86/x64 64 do pobierania Instalatora.</span><span class="sxs-lookup"><span data-stu-id="c2139-232">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="c2139-233">Skopiuj Instalatora na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c2139-233">Copy the installer to the server.</span></span> <span data-ttu-id="c2139-234">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="c2139-234">Run the installer.</span></span> <span data-ttu-id="c2139-235">Wybierz **zainstalować** przycisk i zaakceptuj postanowienia licencyjne.</span><span class="sxs-lookup"><span data-stu-id="c2139-235">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="c2139-236">Ponowne uruchomienie serwera nie jest wymagane po ukończeniu instalacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-236">A server restart isn't required after the install completes.</span></span>

<span data-ttu-id="c2139-237">**Konfigurowanie witryny sieci Web**</span><span class="sxs-lookup"><span data-stu-id="c2139-237">**Configure the website**</span></span>

<span data-ttu-id="c2139-238">Ustaw witrynę sieci Web **ścieżkę fizyczną** do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-238">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="c2139-239">Folder zawiera:</span><span class="sxs-lookup"><span data-stu-id="c2139-239">The folder contains:</span></span>

* <span data-ttu-id="c2139-240">*Web.config* pliku wykorzystywanym przez usługi IIS do konfigurowania witryny sieci Web, łącznie z regułami wymagane przekierowania i typy zawartości plików.</span><span class="sxs-lookup"><span data-stu-id="c2139-240">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="c2139-241">Folder statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2139-241">The app's static asset folder.</span></span>

<span data-ttu-id="c2139-242">**Rozwiązywanie problemów**</span><span class="sxs-lookup"><span data-stu-id="c2139-242">**Troubleshooting**</span></span>

<span data-ttu-id="c2139-243">Jeśli *500 Wewnętrzny błąd serwera* odebraniu i Menedżera usług IIS zgłasza błędy podczas próby dostępu do konfiguracji witryny sieci Web, upewnij się, że zainstalowano moduł ponowne zapisywanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="c2139-243">If a *500 Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="c2139-244">Jeśli moduł nie jest zainstalowany, *web.config* nie można przeanalizować pliku przez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="c2139-244">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="c2139-245">Zapobiega to ładowania konfiguracji witryny sieci Web i witryny sieci Web z dostarczania plików statycznych dla Blazor Menedżera usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c2139-245">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="c2139-246">Aby uzyskać więcej informacji na temat Rozwiązywanie problemów z wdrożeniami usług IIS, zobacz <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="c2139-246">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a><span data-ttu-id="c2139-247">Po stronie klienta Blazor autonomiczne hostowanie przy użyciu serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="c2139-247">Client-side Blazor standalone hosting with Nginx</span></span>

<span data-ttu-id="c2139-248">Następujące *nginx.conf* pliku została uproszczona w celu pokazują, jak skonfigurować serwer Nginx, aby wysłać *Index.html* plików zawsze wtedy, gdy nie można odnaleźć odpowiedniego pliku na dysku.</span><span class="sxs-lookup"><span data-stu-id="c2139-248">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *Index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

<span data-ttu-id="c2139-249">Aby uzyskać więcej informacji na temat konfiguracji serwera sieci web Nginx produkcji, zobacz [tworzenia serwer NGINX Plus i pliki konfiguracji NGINX](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="c2139-249">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a><span data-ttu-id="c2139-250">Po stronie klienta Blazor autonomiczne hostowanie przy użyciu serwera Nginx na platformie Docker</span><span class="sxs-lookup"><span data-stu-id="c2139-250">Client-side Blazor standalone hosting with Nginx in Docker</span></span>

<span data-ttu-id="c2139-251">Aby hostować Blazor na platformie Docker przy użyciu serwera Nginx, Konfiguracja pliku Dockerfile, aby użyć obrazu Alpine na podstawie serwera Nginx.</span><span class="sxs-lookup"><span data-stu-id="c2139-251">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="c2139-252">Aktualizacja pliku Dockerfile, aby skopiować *nginx.config* pliku do kontenera.</span><span class="sxs-lookup"><span data-stu-id="c2139-252">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="c2139-253">Dodaj jeden wiersz do pliku Dockerfile, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c2139-253">Add one line to the Dockerfile, as shown in the following example:</span></span>

```
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a><span data-ttu-id="c2139-254">Po stronie klienta Blazor autonomiczne hostowanie strony GitHub</span><span class="sxs-lookup"><span data-stu-id="c2139-254">Client-side Blazor standalone hosting with GitHub Pages</span></span>

<span data-ttu-id="c2139-255">Aby obsłużyć ponownego adresu URL, Dodaj *404. html* pliku ze skryptem, który obsługuje żądanie przekierowania *index.html* strony.</span><span class="sxs-lookup"><span data-stu-id="c2139-255">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="c2139-256">Przykładem implementacji dostarczane przez społeczność, można zobaczyć [pojedynczej aplikacji strony dla strony GitHub](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github strony w witrynie GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="c2139-256">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="c2139-257">Przykładem dotyczącym używania podejścia społeczności są widoczne w [blazor-demo/blazor-demo.github.io w serwisie GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([aktywnej witryny](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="c2139-257">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="c2139-258">Korzystając z witryny projektu zamiast witryny organizacji, Dodaj lub zaktualizuj `<base>` tagów w *index.html*.</span><span class="sxs-lookup"><span data-stu-id="c2139-258">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="c2139-259">Ustaw `href` wartość do atrybutu `<repository-name>/`, gdzie `<repository-name>/` jest nazwa repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="c2139-259">Set the `href` attribute value to `<repository-name>/`, where `<repository-name>/` is the GitHub repository name.</span></span>

## <a name="deploy-a-server-side-razor-components-app"></a><span data-ttu-id="c2139-260">Wdrażanie aplikacji Razor składniki po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="c2139-260">Deploy a server-side Razor Components app</span></span>

<span data-ttu-id="c2139-261">Za pomocą [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model), składniki Razor jest wykonywane na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2139-261">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="c2139-262">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="c2139-262">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

<span data-ttu-id="c2139-263">Aplikacja jest dołączone do aplikacji platformy ASP.NET Core w opublikowanych danych wyjściowych, więc, że dwie aplikacje wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="c2139-263">The app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="c2139-264">Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2139-264">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="c2139-265">Wdrożenia po stronie serwera, program Visual Studio obejmuje **Blazor (po stronie serwera w programie ASP.NET Core)** szablonu projektu (`blazorserver` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenie).</span><span class="sxs-lookup"><span data-stu-id="c2139-265">For a server-side deployment, Visual Studio includes the **Blazor (Server-side in ASP.NET Core)** project template (`blazorserver` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="c2139-266">Po opublikowaniu aplikacji ASP.NET Core Razor składniki aplikacji znajduje w opublikowanych danych wyjściowych, dzięki czemu aplikacja platformy ASP.NET Core i składniki Razor aplikacji można wdrożyć ze sobą.</span><span class="sxs-lookup"><span data-stu-id="c2139-266">When the ASP.NET Core app is published, the Razor Components app is included in the published output so that the ASP.NET Core app and the Razor Components app can be deployed together.</span></span> <span data-ttu-id="c2139-267">Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="c2139-267">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="c2139-268">Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="c2139-268">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="c2139-269">Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2139-269">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>
