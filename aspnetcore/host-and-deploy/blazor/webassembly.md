---
title: Hostowanie i wdrażanie ASP.NET Core Blazor webassembly
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor przy użyciu ASP.NET Core, sieci dostarczania zawartości (CDN), serwerów plików i stron usługi GitHub.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: 861935ff31652f923399a8aa5ae52baa6b77fa91
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172405"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="321b7-103">Hostowanie i wdrażanie ASP.NET Core Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="321b7-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="321b7-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="321b7-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="321b7-105">Z [modelem hostinguBlazor webassembly](xref:blazor/hosting-models#blazor-webassembly):</span><span class="sxs-lookup"><span data-stu-id="321b7-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="321b7-106">Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="321b7-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="321b7-107">Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="321b7-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="321b7-108">Obsługiwane są następujące strategie wdrażania:</span><span class="sxs-lookup"><span data-stu-id="321b7-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="321b7-109">Aplikacja Blazor jest obsługiwana przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="321b7-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="321b7-110">Ta strategia jest objęta [wdrożeniem hostowanym za pomocą ASP.NET Core](#hosted-deployment-with-aspnet-core) sekcji.</span><span class="sxs-lookup"><span data-stu-id="321b7-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="321b7-111">Aplikacja Blazor jest umieszczana na statycznym, hostingowym serwerze sieci Web lub usłudze, w której program .NET nie jest używany do obsługi aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="321b7-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="321b7-112">Ta strategia została omówiona w sekcji [wdrażanie autonomiczne](#standalone-deployment) , która zawiera informacje na temat hostowania aplikacji Blazor webassembly jako aplikacji podrzędnej IIS.</span><span class="sxs-lookup"><span data-stu-id="321b7-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="321b7-113">Ponownie Napisz adresy URL pod kątem prawidłowego routingu</span><span class="sxs-lookup"><span data-stu-id="321b7-113">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="321b7-114">Żądania routingu dla składników strony w Blazor aplikacji webassembly nie są tak proste jak żądania routingu na serwerze Blazor, hostowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-114">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="321b7-115">Weź pod uwagę Blazor aplikacji webassembly z dwoma składnikami:</span><span class="sxs-lookup"><span data-stu-id="321b7-115">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="321b7-116">*Główny. razor* &ndash; ładowany w katalogu głównym aplikacji i zawiera link do składnika `About` (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="321b7-116">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="321b7-117">*Informacje o* składniku `About` &ndash; Razor.</span><span class="sxs-lookup"><span data-stu-id="321b7-117">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="321b7-118">Gdy zażądano dokumentu domyślnego aplikacji przy użyciu paska adresu przeglądarki (na przykład `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="321b7-118">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="321b7-119">Przeglądarka wykonuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="321b7-119">The browser makes a request.</span></span>
1. <span data-ttu-id="321b7-120">Zostanie zwrócona strona domyślna, która jest zwykle *index. html*.</span><span class="sxs-lookup"><span data-stu-id="321b7-120">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="321b7-121">*index. html* Bootstrap aplikację.</span><span class="sxs-lookup"><span data-stu-id="321b7-121">*index.html* bootstraps the app.</span></span>
1. Blazor<span data-ttu-id="321b7-122">ładowania routera, a składnik Razor `Main` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="321b7-122">'s router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="321b7-123">Na stronie głównej wybranie linku do składnika `About` działa na kliencie, ponieważ router Blazor uniemożliwia przeglądarce żądanie w Internecie, aby `www.contoso.com` dla `About` i obsłużyć renderowany składnik `About`.</span><span class="sxs-lookup"><span data-stu-id="321b7-123">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="321b7-124">Wszystkie żądania dotyczące wewnętrznych punktów końcowych *w aplikacji Blazor webassembly* działają w taki sam sposób: żądania nie wyzwalają żądań przeglądarki do zasobów hostowanych przez serwer w Internecie.</span><span class="sxs-lookup"><span data-stu-id="321b7-124">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="321b7-125">Router obsługuje wewnętrznie żądania.</span><span class="sxs-lookup"><span data-stu-id="321b7-125">The router handles the requests internally.</span></span>

<span data-ttu-id="321b7-126">Jeśli żądanie zostanie wykonane przy użyciu paska adresu przeglądarki dla `www.contoso.com/About`, żądanie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="321b7-126">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="321b7-127">Ten zasób nie istnieje na hoście internetowym aplikacji, więc zwracana jest odpowiedź *404 — nie znaleziono* .</span><span class="sxs-lookup"><span data-stu-id="321b7-127">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="321b7-128">Ponieważ przeglądarki wysyłają żądania do hostów internetowych dla stron po stronie klienta, serwery sieci Web i usługi hostingu muszą ponownie zapisywać wszystkie żądania dotyczące zasobów, które nie znajdują się fizycznie na serwerze, na stronie *index. html* .</span><span class="sxs-lookup"><span data-stu-id="321b7-128">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="321b7-129">Gdy jest zwracany *plik index. html* , router Blazor aplikacji przejmuje i odpowiada przy użyciu poprawnego zasobu.</span><span class="sxs-lookup"><span data-stu-id="321b7-129">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="321b7-130">Podczas wdrażania na serwerze IIS można użyć modułu ponownego zapisywania adresu URL z opublikowanym plikiem *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-130">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="321b7-131">Aby uzyskać więcej informacji, zobacz sekcję [usług IIS](#iis) .</span><span class="sxs-lookup"><span data-stu-id="321b7-131">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="321b7-132">Hostowane wdrożenie z ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="321b7-132">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="321b7-133">*Wdrożenie hostowane* służy do obsługi aplikacji Blazor webassembly w przeglądarkach z poziomu [aplikacji ASP.NET Core](xref:index) działającej na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="321b7-133">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="321b7-134">Aplikacja Blazor jest dołączona do aplikacji ASP.NET Core w opublikowanych danych wyjściowych, dzięki czemu dwie aplikacje są wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="321b7-134">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="321b7-135">Wymagany jest serwer sieci Web, który umożliwia hostowanie aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="321b7-135">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="321b7-136">W przypadku wdrożenia hostowanego program Visual Studio zawiera szablon projektu **aplikacjiBlazor webassembly** (`blazorwasm` szablon w przypadku używania polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ) z wybraną opcją **hostowaną** .</span><span class="sxs-lookup"><span data-stu-id="321b7-136">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected.</span></span>

<span data-ttu-id="321b7-137">Aby uzyskać więcej informacji na temat ASP.NET Core hostingu i wdrażania aplikacji, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="321b7-137">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="321b7-138">Aby uzyskać informacje na temat wdrażania do Azure App Service, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="321b7-138">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="321b7-139">Wdrożenie autonomiczne</span><span class="sxs-lookup"><span data-stu-id="321b7-139">Standalone deployment</span></span>

<span data-ttu-id="321b7-140">*Wdrożenie autonomiczne* służy aplikacji Blazor webassembly jako zestawu plików statycznych, które są żądane bezpośrednio przez klientów.</span><span class="sxs-lookup"><span data-stu-id="321b7-140">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="321b7-141">Każdy statyczny serwer plików jest w stanie obsłużyć Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-141">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="321b7-142">Zasoby wdrażania autonomicznego są publikowane w folderze *bin/Release/{Target Framework}/Publish/{Assembly Name}/dist* .</span><span class="sxs-lookup"><span data-stu-id="321b7-142">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="321b7-143">IIS</span><span class="sxs-lookup"><span data-stu-id="321b7-143">IIS</span></span>

<span data-ttu-id="321b7-144">Program IIS jest możliwym do obsługi statycznego serwera plików dla Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-144">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="321b7-145">Aby skonfigurować usługi IIS do hostowania Blazor, zobacz [Tworzenie statycznej witryny sieci Web w usługach IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="321b7-145">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="321b7-146">Opublikowane zasoby są tworzone w folderze */bin/Release/{Target Framework}/Publish* .</span><span class="sxs-lookup"><span data-stu-id="321b7-146">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="321b7-147">Hostowanie zawartości folderu *publikowania* na serwerze sieci Web lub w usłudze hostingu.</span><span class="sxs-lookup"><span data-stu-id="321b7-147">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="321b7-148">web.config</span><span class="sxs-lookup"><span data-stu-id="321b7-148">web.config</span></span>

<span data-ttu-id="321b7-149">Po opublikowaniu Blazor projektu zostanie utworzony plik *Web. config* z następującą konfiguracją usług IIS:</span><span class="sxs-lookup"><span data-stu-id="321b7-149">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="321b7-150">Typy MIME są ustawiane dla następujących rozszerzeń plików:</span><span class="sxs-lookup"><span data-stu-id="321b7-150">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="321b7-151">*dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="321b7-151">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="321b7-152">`application/json` &ndash; *JSON*</span><span class="sxs-lookup"><span data-stu-id="321b7-152">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="321b7-153">*wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="321b7-153">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="321b7-154">*woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="321b7-154">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="321b7-155">*woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="321b7-155">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="321b7-156">Kompresja HTTP jest włączona dla następujących typów MIME:</span><span class="sxs-lookup"><span data-stu-id="321b7-156">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="321b7-157">Reguły modułu ponownego zapisywania adresu URL zostały ustanowione:</span><span class="sxs-lookup"><span data-stu-id="321b7-157">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="321b7-158">Obsługuj podkatalog, w którym znajdują się zasoby statyczne aplikacji ( *{Assembly Name}/dist/{Path*).</span><span class="sxs-lookup"><span data-stu-id="321b7-158">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="321b7-159">Utwórz Routing awaryjny SPA, aby żądania dotyczące zasobów nienależących do pliku zostały przekierowane do domyślnego dokumentu aplikacji w folderze zasobów statycznych ( *{Assembly Name}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="321b7-159">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="321b7-160">Zainstaluj moduł ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="321b7-160">Install the URL Rewrite Module</span></span>

<span data-ttu-id="321b7-161">[Moduł ponownego zapisywania adresu URL](https://www.iis.net/downloads/microsoft/url-rewrite) jest wymagany do ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="321b7-161">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="321b7-162">Moduł nie jest instalowany domyślnie i nie jest dostępny do zainstalowania jako funkcja usługi roli Serwer sieci Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="321b7-162">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="321b7-163">Moduł musi zostać pobrany z witryny sieci Web usług IIS.</span><span class="sxs-lookup"><span data-stu-id="321b7-163">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="321b7-164">Zainstaluj moduł przy użyciu Instalatora platformy sieci Web:</span><span class="sxs-lookup"><span data-stu-id="321b7-164">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="321b7-165">Lokalnie przejdź do [strony pobierania modułu ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="321b7-165">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="321b7-166">W przypadku wersji angielskiej wybierz pozycję **Instalatora WebPI** , aby pobrać Instalatora Instalatora WebPI.</span><span class="sxs-lookup"><span data-stu-id="321b7-166">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="321b7-167">W przypadku innych języków wybierz odpowiednią architekturę dla serwera (x86/x64), aby pobrać Instalatora.</span><span class="sxs-lookup"><span data-stu-id="321b7-167">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="321b7-168">Skopiuj Instalatora na serwer.</span><span class="sxs-lookup"><span data-stu-id="321b7-168">Copy the installer to the server.</span></span> <span data-ttu-id="321b7-169">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="321b7-169">Run the installer.</span></span> <span data-ttu-id="321b7-170">Wybierz przycisk **Zainstaluj** i zaakceptuj postanowienia licencyjne.</span><span class="sxs-lookup"><span data-stu-id="321b7-170">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="321b7-171">Po zakończeniu instalacji nie jest wymagane ponowne uruchomienie serwera.</span><span class="sxs-lookup"><span data-stu-id="321b7-171">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="321b7-172">Skonfiguruj witrynę sieci Web</span><span class="sxs-lookup"><span data-stu-id="321b7-172">Configure the website</span></span>

<span data-ttu-id="321b7-173">Ustaw **ścieżkę fizyczną** witryny sieci Web do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-173">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="321b7-174">Folder zawiera:</span><span class="sxs-lookup"><span data-stu-id="321b7-174">The folder contains:</span></span>

* <span data-ttu-id="321b7-175">Plik *Web. config* , za pomocą którego usługi IIS konfigurują witrynę sieci Web, w tym wymagane reguły przekierowań i typy zawartości plików.</span><span class="sxs-lookup"><span data-stu-id="321b7-175">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="321b7-176">Folder elementu zawartości statycznej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-176">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="321b7-177">Host jako podrzędną aplikację usług IIS</span><span class="sxs-lookup"><span data-stu-id="321b7-177">Host as an IIS sub-app</span></span>

<span data-ttu-id="321b7-178">Jeśli aplikacja autonomiczna jest hostowana jako podaplikacja usług IIS, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="321b7-178">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="321b7-179">Wyłącz procedurę obsługi ASP.NET Core dziedziczonego modułu.</span><span class="sxs-lookup"><span data-stu-id="321b7-179">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="321b7-180">Usuń program obsługi w opublikowanym pliku *Web. config* aplikacji Blazor, dodając do pliku sekcję `<handlers>`:</span><span class="sxs-lookup"><span data-stu-id="321b7-180">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="321b7-181">Wyłącz dziedziczenie sekcji `<system.webServer>` głównej (nadrzędnej) aplikacji przy użyciu elementu `<location>` z `inheritInChildApplications`m ustawionym na `false`:</span><span class="sxs-lookup"><span data-stu-id="321b7-181">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="321b7-182">Usuwanie procedury obsługi lub wyłączanie dziedziczenia jest wykonywane poza [konfiguracją ścieżki podstawowej aplikacji](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="321b7-182">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="321b7-183">Ustaw ścieżkę bazową aplikacji w pliku *index. html* aplikacji na alias IIS używany podczas konfigurowania aplikacji podrzędnej w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="321b7-183">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="321b7-184">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="321b7-184">Troubleshooting</span></span>

<span data-ttu-id="321b7-185">W przypadku odebrania *500 — wewnętrzny błąd serwera* , a Menedżer usług IIS zgłasza błędy przy próbie uzyskania dostępu do konfiguracji witryny sieci Web, upewnij się, że zainstalowano moduł ponownego zapisywania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="321b7-185">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="321b7-186">Gdy moduł nie jest zainstalowany, nie można przeanalizować pliku *Web. config* przez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="321b7-186">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="321b7-187">Zapobiega to załadowaniu przez Menedżera usług IIS konfiguracji witryny sieci Web i witryny sieci Web do obsługi plików statycznych Blazor.</span><span class="sxs-lookup"><span data-stu-id="321b7-187">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="321b7-188">Aby uzyskać więcej informacji na temat rozwiązywania problemów z wdrożeniami w usługach IIS, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="321b7-188">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="321b7-189">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="321b7-189">Azure Storage</span></span>

<span data-ttu-id="321b7-190">Hosting pliku statycznego [usługi Azure Storage](/azure/storage/) umożliwia hosting aplikacji bezserwerowych Blazor.</span><span class="sxs-lookup"><span data-stu-id="321b7-190">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="321b7-191">Obsługiwane są niestandardowe nazwy domen, usługa Azure Content Delivery Network (CDN) i protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="321b7-191">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="321b7-192">Gdy usługa BLOB jest włączona dla hostingu statycznej witryny sieci Web na koncie magazynu:</span><span class="sxs-lookup"><span data-stu-id="321b7-192">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="321b7-193">Ustaw **nazwę dokumentu indeksu** na `index.html`.</span><span class="sxs-lookup"><span data-stu-id="321b7-193">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="321b7-194">Ustaw **ścieżkę dokumentu błędu** na `index.html`.</span><span class="sxs-lookup"><span data-stu-id="321b7-194">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="321b7-195">Składniki Razor i inne punkty końcowe inne niż pliki nie znajdują się w ścieżkach fizycznych w zawartości statycznej przechowywanej przez usługę BLOB.</span><span class="sxs-lookup"><span data-stu-id="321b7-195">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="321b7-196">Po otrzymaniu żądania dla jednego z tych zasobów, który powinien być obsługiwany przez router Blazor, błąd *404-nie znaleziono* przez usługę BLOB Service kieruje żądanie do **ścieżki dokumentu błędu**.</span><span class="sxs-lookup"><span data-stu-id="321b7-196">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="321b7-197">Zwracany jest obiekt BLOB *index. html* , a router Blazor ładuje i przetwarza ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="321b7-197">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="321b7-198">Aby uzyskać więcej informacji, zobacz [Obsługa statycznej witryny sieci Web w usłudze Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="321b7-198">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="321b7-199">Nginx</span><span class="sxs-lookup"><span data-stu-id="321b7-199">Nginx</span></span>

<span data-ttu-id="321b7-200">Następujący plik *Nginx. conf* został uproszczony, aby pokazać, jak skonfigurować Nginx do wysyłania pliku *index. html* za każdym razem, gdy nie można znaleźć odpowiedniego pliku na dysku.</span><span class="sxs-lookup"><span data-stu-id="321b7-200">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="321b7-201">Aby uzyskać więcej informacji na temat konfiguracji serwera sieci Web w środowisku produkcyjnym, zobacz [Tworzenie plików konfiguracji Nginx Plus i Nginx](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="321b7-201">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="321b7-202">Nginx w Docker</span><span class="sxs-lookup"><span data-stu-id="321b7-202">Nginx in Docker</span></span>

<span data-ttu-id="321b7-203">Aby hostować Blazor w programie Docker przy użyciu Nginx, skonfiguruj pliku dockerfile do korzystania z obrazu Nginx opartego na Alpine.</span><span class="sxs-lookup"><span data-stu-id="321b7-203">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="321b7-204">Zaktualizuj pliku dockerfile, aby skopiować plik *Nginx. config* do kontenera.</span><span class="sxs-lookup"><span data-stu-id="321b7-204">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="321b7-205">Dodaj jeden wiersz do pliku dockerfile, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="321b7-205">Add one line to the Dockerfile, as shown in the following example:</span></span>

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a><span data-ttu-id="321b7-206">Apache</span><span class="sxs-lookup"><span data-stu-id="321b7-206">Apache</span></span>

<span data-ttu-id="321b7-207">Aby wdrożyć aplikację webassembly w programie Blazor CentOS 7 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="321b7-207">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="321b7-208">Utwórz plik konfiguracji Apache.</span><span class="sxs-lookup"><span data-stu-id="321b7-208">Create the Apache configuration file.</span></span> <span data-ttu-id="321b7-209">Poniższy przykład to uproszczony plik konfiguracji (*blazorapp. config*):</span><span class="sxs-lookup"><span data-stu-id="321b7-209">The following example is a simplified configuration file (*blazorapp.config*):</span></span>

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType aplication/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. <span data-ttu-id="321b7-210">Umieść plik konfiguracji Apache w katalogu `/etc/httpd/conf.d/`, który jest domyślnym katalogiem konfiguracji Apache w CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="321b7-210">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="321b7-211">Umieść pliki aplikacji w katalogu `/var/www/blazorapp` (lokalizacja określona do `DocumentRoot` w pliku konfiguracyjnym).</span><span class="sxs-lookup"><span data-stu-id="321b7-211">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="321b7-212">Uruchom ponownie usługę Apache.</span><span class="sxs-lookup"><span data-stu-id="321b7-212">Restart the Apache service.</span></span>

<span data-ttu-id="321b7-213">Aby uzyskać więcej informacji, zobacz [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) i [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span><span class="sxs-lookup"><span data-stu-id="321b7-213">For more information, see [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="321b7-214">Strony serwisu GitHub</span><span class="sxs-lookup"><span data-stu-id="321b7-214">GitHub Pages</span></span>

<span data-ttu-id="321b7-215">Aby obsłużyć ponowne zapisywanie adresów URL, Dodaj plik *404. html* ze skryptem, który obsługuje przekierowywanie żądania do strony *index. html* .</span><span class="sxs-lookup"><span data-stu-id="321b7-215">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="321b7-216">Aby zapoznać się z przykładową implementacją dostarczoną przez społeczność, zobacz [aplikacje jednostronicowe dla stron usługi GitHub](https://spa-github-pages.rafrex.com/) ([rafrex/Spa-GitHub-Pages w witrynie GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="321b7-216">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="321b7-217">Przykład użycia podejścia społecznościowego można znaleźć[w witrynie](https://blazor-demo.github.io/) [GitHub (blazor — Demonstracja/blazor-Demonstracja](https://github.com/blazor-demo/blazor-demo.github.io) ).</span><span class="sxs-lookup"><span data-stu-id="321b7-217">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="321b7-218">W przypadku korzystania z witryny projektu zamiast witryny organizacji Dodaj lub zaktualizuj tag `<base>` w *pliku index. html*.</span><span class="sxs-lookup"><span data-stu-id="321b7-218">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="321b7-219">Ustaw wartość atrybutu `href` na nazwę repozytorium GitHub z końcowym ukośnikiem (na przykład `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="321b7-219">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="321b7-220">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="321b7-220">Host configuration values</span></span>

<span data-ttu-id="321b7-221">[Blazor aplikacje webassembly](xref:blazor/hosting-models#blazor-webassembly) mogą akceptować następujące wartości konfiguracji hosta jako argumenty wiersza polecenia w czasie wykonywania w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="321b7-221">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="321b7-222">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="321b7-222">Content root</span></span>

<span data-ttu-id="321b7-223">Argument `--contentroot` ustawia ścieżkę bezwzględną do katalogu, który zawiera pliki zawartości aplikacji ([katalog główny zawartości](xref:fundamentals/index#content-root)).</span><span class="sxs-lookup"><span data-stu-id="321b7-223">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="321b7-224">W poniższych przykładach `/content-root-path` jest ścieżką katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-224">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="321b7-225">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="321b7-225">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="321b7-226">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="321b7-226">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="321b7-227">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="321b7-227">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="321b7-228">To ustawienie jest używane, gdy aplikacja jest uruchamiana z debugerem programu Visual Studio i z wiersza polecenia z `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="321b7-228">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="321b7-229">W programie Visual Studio Określ argument we **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="321b7-229">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="321b7-230">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="321b7-230">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="321b7-231">Baza ścieżki</span><span class="sxs-lookup"><span data-stu-id="321b7-231">Path base</span></span>

<span data-ttu-id="321b7-232">`--pathbase` argument ustawia ścieżkę bazową aplikacji dla aplikacji uruchamianej lokalnie z niegłówną względną ścieżką URL (tag `<base>` `href` jest ustawiona na ścieżkę inną niż `/` do przemieszczania i produkcji).</span><span class="sxs-lookup"><span data-stu-id="321b7-232">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="321b7-233">W poniższych przykładach `/relative-URL-path` jest baza ścieżki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-233">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="321b7-234">Aby uzyskać więcej informacji, zobacz [Ścieżka podstawowa aplikacji](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="321b7-234">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="321b7-235">W przeciwieństwie do ścieżki podanej do `href` tagu `<base>`, nie dodawaj końcowego ukośnika (`/`) podczas przekazywania wartości argumentu `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="321b7-235">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="321b7-236">Jeśli ścieżka podstawowa aplikacji znajduje się w tagu `<base>` jako `<base href="/CoolApp/">` (zawiera końcowy ukośnik), należy przekazać wartość argumentu wiersza polecenia jako `--pathbase=/CoolApp` (bez ukośników końcowych).</span><span class="sxs-lookup"><span data-stu-id="321b7-236">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="321b7-237">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="321b7-237">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="321b7-238">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="321b7-238">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="321b7-239">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="321b7-239">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="321b7-240">To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia z `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="321b7-240">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="321b7-241">W programie Visual Studio Określ argument we **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="321b7-241">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="321b7-242">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="321b7-242">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="321b7-243">Adresy URL</span><span class="sxs-lookup"><span data-stu-id="321b7-243">URLs</span></span>

<span data-ttu-id="321b7-244">`--urls` argument ustawia adresy IP lub adresy hosta z portami i protokołami, aby nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="321b7-244">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="321b7-245">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="321b7-245">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="321b7-246">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="321b7-246">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="321b7-247">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="321b7-247">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="321b7-248">To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia z `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="321b7-248">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="321b7-249">W programie Visual Studio Określ argument we **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="321b7-249">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="321b7-250">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="321b7-250">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="321b7-251">Konfigurowanie konsolidatora</span><span class="sxs-lookup"><span data-stu-id="321b7-251">Configure the Linker</span></span>

Blazor<span data-ttu-id="321b7-252"> wykonuje konsolidację języka pośredniego (IL) dla każdej kompilacji, aby usunąć niepotrzebny kod IL z zestawów wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="321b7-252"> performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="321b7-253">Łączenie zestawu może być kontrolowane podczas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="321b7-253">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="321b7-254">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="321b7-254">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>
