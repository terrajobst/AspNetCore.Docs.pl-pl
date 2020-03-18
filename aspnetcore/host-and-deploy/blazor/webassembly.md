---
title: Hostowanie i wdrażanie ASP.NET Core Blazor webassembly
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor przy użyciu ASP.NET Core, sieci dostarczania zawartości (CDN), serwerów plików i stron usługi GitHub.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: ea2c625f424447209a362cdc58bdb18be061e47f
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511356"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="b79f6-103">Hostowanie i wdrażanie ASP.NET Core Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="b79f6-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="b79f6-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="b79f6-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="b79f6-105">Z [modelem hostinguBlazor webassembly](xref:blazor/hosting-models#blazor-webassembly):</span><span class="sxs-lookup"><span data-stu-id="b79f6-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="b79f6-106">Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b79f6-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="b79f6-107">Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b79f6-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="b79f6-108">Obsługiwane są następujące strategie wdrażania:</span><span class="sxs-lookup"><span data-stu-id="b79f6-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="b79f6-109">Aplikacja Blazor jest obsługiwana przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b79f6-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="b79f6-110">Ta strategia jest objęta [wdrożeniem hostowanym za pomocą ASP.NET Core](#hosted-deployment-with-aspnet-core) sekcji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="b79f6-111">Aplikacja Blazor jest umieszczana na statycznym, hostingowym serwerze sieci Web lub usłudze, w której program .NET nie jest używany do obsługi aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="b79f6-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="b79f6-112">Ta strategia została omówiona w sekcji [wdrażanie autonomiczne](#standalone-deployment) , która zawiera informacje na temat hostowania aplikacji Blazor webassembly jako aplikacji podrzędnej IIS.</span><span class="sxs-lookup"><span data-stu-id="b79f6-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="b79f6-113">Ponownie Napisz adresy URL pod kątem prawidłowego routingu</span><span class="sxs-lookup"><span data-stu-id="b79f6-113">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="b79f6-114">Żądania routingu dla składników strony w Blazor aplikacji webassembly nie są tak proste jak żądania routingu na serwerze Blazor, hostowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-114">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="b79f6-115">Weź pod uwagę Blazor aplikacji webassembly z dwoma składnikami:</span><span class="sxs-lookup"><span data-stu-id="b79f6-115">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="b79f6-116">*Główny. razor* &ndash; ładowany w katalogu głównym aplikacji i zawiera link do składnika `About` (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="b79f6-116">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="b79f6-117">*Informacje o* składniku `About` &ndash; Razor.</span><span class="sxs-lookup"><span data-stu-id="b79f6-117">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="b79f6-118">Gdy zażądano dokumentu domyślnego aplikacji przy użyciu paska adresu przeglądarki (na przykład `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="b79f6-118">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="b79f6-119">Przeglądarka wykonuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="b79f6-119">The browser makes a request.</span></span>
1. <span data-ttu-id="b79f6-120">Zostanie zwrócona strona domyślna, która jest zwykle *index. html*.</span><span class="sxs-lookup"><span data-stu-id="b79f6-120">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="b79f6-121">*index. html* Bootstrap aplikację.</span><span class="sxs-lookup"><span data-stu-id="b79f6-121">*index.html* bootstraps the app.</span></span>
1. Blazor<span data-ttu-id="b79f6-122">ładowania routera, a składnik Razor `Main` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="b79f6-122">'s router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="b79f6-123">Na stronie głównej wybranie linku do składnika `About` działa na kliencie, ponieważ router Blazor uniemożliwia przeglądarce żądanie w Internecie, aby `www.contoso.com` dla `About` i obsłużyć renderowany składnik `About`.</span><span class="sxs-lookup"><span data-stu-id="b79f6-123">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="b79f6-124">Wszystkie żądania dotyczące wewnętrznych punktów końcowych *w aplikacji Blazor webassembly* działają w taki sam sposób: żądania nie wyzwalają żądań przeglądarki do zasobów hostowanych przez serwer w Internecie.</span><span class="sxs-lookup"><span data-stu-id="b79f6-124">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="b79f6-125">Router obsługuje wewnętrznie żądania.</span><span class="sxs-lookup"><span data-stu-id="b79f6-125">The router handles the requests internally.</span></span>

<span data-ttu-id="b79f6-126">Jeśli żądanie zostanie wykonane przy użyciu paska adresu przeglądarki dla `www.contoso.com/About`, żądanie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b79f6-126">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="b79f6-127">Ten zasób nie istnieje na hoście internetowym aplikacji, więc zwracana jest odpowiedź *404 — nie znaleziono* .</span><span class="sxs-lookup"><span data-stu-id="b79f6-127">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="b79f6-128">Ponieważ przeglądarki wysyłają żądania do hostów internetowych dla stron po stronie klienta, serwery sieci Web i usługi hostingu muszą ponownie zapisywać wszystkie żądania dotyczące zasobów, które nie znajdują się fizycznie na serwerze, na stronie *index. html* .</span><span class="sxs-lookup"><span data-stu-id="b79f6-128">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="b79f6-129">Gdy jest zwracany *plik index. html* , router Blazor aplikacji przejmuje i odpowiada przy użyciu poprawnego zasobu.</span><span class="sxs-lookup"><span data-stu-id="b79f6-129">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="b79f6-130">Podczas wdrażania na serwerze IIS można użyć modułu ponownego zapisywania adresu URL z opublikowanym plikiem *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-130">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="b79f6-131">Aby uzyskać więcej informacji, zobacz sekcję [usług IIS](#iis) .</span><span class="sxs-lookup"><span data-stu-id="b79f6-131">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="b79f6-132">Hostowane wdrożenie z ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b79f6-132">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="b79f6-133">*Wdrożenie hostowane* służy do obsługi aplikacji Blazor webassembly w przeglądarkach z poziomu [aplikacji ASP.NET Core](xref:index) działającej na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b79f6-133">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="b79f6-134">Aplikacja webassembly Blazor Client jest publikowana w folderze */bin/Release/{Target Framework}/Publish/wwwroot* aplikacji serwerowej wraz ze wszystkimi innymi statycznymi zasobami sieci Web aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="b79f6-134">The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="b79f6-135">Te dwie aplikacje są wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="b79f6-135">The two apps are deployed together.</span></span> <span data-ttu-id="b79f6-136">Wymagany jest serwer sieci Web, który umożliwia hostowanie aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b79f6-136">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="b79f6-137">W przypadku wdrożenia hostowanego program Visual Studio zawiera szablon projektu **aplikacjiBlazor webassembly** (szablon`blazorwasm` przy użyciu polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ) z wybraną opcją **hostowaną** (`-ho|--hosted` przy użyciu polecenia `dotnet new`).</span><span class="sxs-lookup"><span data-stu-id="b79f6-137">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected (`-ho|--hosted` when using the `dotnet new` command).</span></span>

<span data-ttu-id="b79f6-138">Aby uzyskać więcej informacji na temat ASP.NET Core hostingu i wdrażania aplikacji, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="b79f6-138">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="b79f6-139">Aby uzyskać informacje na temat wdrażania do Azure App Service, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="b79f6-139">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="b79f6-140">Wdrożenie autonomiczne</span><span class="sxs-lookup"><span data-stu-id="b79f6-140">Standalone deployment</span></span>

<span data-ttu-id="b79f6-141">*Wdrożenie autonomiczne* służy aplikacji Blazor webassembly jako zestawu plików statycznych, które są żądane bezpośrednio przez klientów.</span><span class="sxs-lookup"><span data-stu-id="b79f6-141">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="b79f6-142">Każdy statyczny serwer plików jest w stanie obsłużyć Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-142">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="b79f6-143">Zasoby wdrażania autonomicznego są publikowane w folderze */bin/Release/{Target Framework}/Publish/wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="b79f6-143">Standalone deployment assets are published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="b79f6-144">IIS</span><span class="sxs-lookup"><span data-stu-id="b79f6-144">IIS</span></span>

<span data-ttu-id="b79f6-145">Program IIS jest możliwym do obsługi statycznego serwera plików dla Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-145">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="b79f6-146">Aby skonfigurować usługi IIS do hostowania Blazor, zobacz [Tworzenie statycznej witryny sieci Web w usługach IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="b79f6-146">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="b79f6-147">Opublikowane zasoby są tworzone w folderze */bin/Release/{Target Framework}/Publish* .</span><span class="sxs-lookup"><span data-stu-id="b79f6-147">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="b79f6-148">Hostowanie zawartości folderu *publikowania* na serwerze sieci Web lub w usłudze hostingu.</span><span class="sxs-lookup"><span data-stu-id="b79f6-148">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="b79f6-149">web.config</span><span class="sxs-lookup"><span data-stu-id="b79f6-149">web.config</span></span>

<span data-ttu-id="b79f6-150">Po opublikowaniu Blazor projektu zostanie utworzony plik *Web. config* z następującą konfiguracją usług IIS:</span><span class="sxs-lookup"><span data-stu-id="b79f6-150">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="b79f6-151">Typy MIME są ustawiane dla następujących rozszerzeń plików:</span><span class="sxs-lookup"><span data-stu-id="b79f6-151">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="b79f6-152">*dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="b79f6-152">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="b79f6-153">`application/json` &ndash; *JSON*</span><span class="sxs-lookup"><span data-stu-id="b79f6-153">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="b79f6-154">*wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="b79f6-154">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="b79f6-155">*woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="b79f6-155">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="b79f6-156">*woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="b79f6-156">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="b79f6-157">Kompresja HTTP jest włączona dla następujących typów MIME:</span><span class="sxs-lookup"><span data-stu-id="b79f6-157">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="b79f6-158">Reguły modułu ponownego zapisywania adresu URL zostały ustanowione:</span><span class="sxs-lookup"><span data-stu-id="b79f6-158">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="b79f6-159">Obsługuj podkatalog, w którym znajdują się zasoby statyczne aplikacji (*wwwroot/{ŻĄDANA ścieżka}* ).</span><span class="sxs-lookup"><span data-stu-id="b79f6-159">Serve the sub-directory where the app's static assets reside (*wwwroot/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="b79f6-160">Utwórz Routing awaryjny SPA, aby żądania dotyczące zasobów nienależących do pliku zostały przekierowane do domyślnego dokumentu aplikacji w folderze zasobów statycznych (*wwwroot/index.html*).</span><span class="sxs-lookup"><span data-stu-id="b79f6-160">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*wwwroot/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="b79f6-161">Zainstaluj moduł ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="b79f6-161">Install the URL Rewrite Module</span></span>

<span data-ttu-id="b79f6-162">[Moduł ponownego zapisywania adresu URL](https://www.iis.net/downloads/microsoft/url-rewrite) jest wymagany do ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="b79f6-162">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="b79f6-163">Moduł nie jest instalowany domyślnie i nie jest dostępny do zainstalowania jako funkcja usługi roli Serwer sieci Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="b79f6-163">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="b79f6-164">Moduł musi zostać pobrany z witryny sieci Web usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b79f6-164">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="b79f6-165">Zainstaluj moduł przy użyciu Instalatora platformy sieci Web:</span><span class="sxs-lookup"><span data-stu-id="b79f6-165">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="b79f6-166">Lokalnie przejdź do [strony pobierania modułu ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="b79f6-166">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="b79f6-167">W przypadku wersji angielskiej wybierz pozycję **Instalatora WebPI** , aby pobrać Instalatora Instalatora WebPI.</span><span class="sxs-lookup"><span data-stu-id="b79f6-167">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="b79f6-168">W przypadku innych języków wybierz odpowiednią architekturę dla serwera (x86/x64), aby pobrać Instalatora.</span><span class="sxs-lookup"><span data-stu-id="b79f6-168">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="b79f6-169">Skopiuj Instalatora na serwer.</span><span class="sxs-lookup"><span data-stu-id="b79f6-169">Copy the installer to the server.</span></span> <span data-ttu-id="b79f6-170">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="b79f6-170">Run the installer.</span></span> <span data-ttu-id="b79f6-171">Wybierz przycisk **Zainstaluj** i zaakceptuj postanowienia licencyjne.</span><span class="sxs-lookup"><span data-stu-id="b79f6-171">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="b79f6-172">Po zakończeniu instalacji nie jest wymagane ponowne uruchomienie serwera.</span><span class="sxs-lookup"><span data-stu-id="b79f6-172">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="b79f6-173">Skonfiguruj witrynę sieci Web</span><span class="sxs-lookup"><span data-stu-id="b79f6-173">Configure the website</span></span>

<span data-ttu-id="b79f6-174">Ustaw **ścieżkę fizyczną** witryny sieci Web do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-174">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="b79f6-175">Folder zawiera:</span><span class="sxs-lookup"><span data-stu-id="b79f6-175">The folder contains:</span></span>

* <span data-ttu-id="b79f6-176">Plik *Web. config* , za pomocą którego usługi IIS konfigurują witrynę sieci Web, w tym wymagane reguły przekierowań i typy zawartości plików.</span><span class="sxs-lookup"><span data-stu-id="b79f6-176">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="b79f6-177">Folder elementu zawartości statycznej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-177">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="b79f6-178">Host jako podrzędną aplikację usług IIS</span><span class="sxs-lookup"><span data-stu-id="b79f6-178">Host as an IIS sub-app</span></span>

<span data-ttu-id="b79f6-179">Jeśli aplikacja autonomiczna jest hostowana jako podaplikacja usług IIS, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b79f6-179">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="b79f6-180">Wyłącz procedurę obsługi ASP.NET Core dziedziczonego modułu.</span><span class="sxs-lookup"><span data-stu-id="b79f6-180">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="b79f6-181">Usuń program obsługi w opublikowanym pliku *Web. config* aplikacji Blazor, dodając do pliku sekcję `<handlers>`:</span><span class="sxs-lookup"><span data-stu-id="b79f6-181">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="b79f6-182">Wyłącz dziedziczenie sekcji `<system.webServer>` głównej (nadrzędnej) aplikacji przy użyciu elementu `<location>` z `inheritInChildApplications`m ustawionym na `false`:</span><span class="sxs-lookup"><span data-stu-id="b79f6-182">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="b79f6-183">Usuwanie procedury obsługi lub wyłączanie dziedziczenia jest wykonywane poza [konfiguracją ścieżki podstawowej aplikacji](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="b79f6-183">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="b79f6-184">Ustaw ścieżkę bazową aplikacji w pliku *index. html* aplikacji na alias IIS używany podczas konfigurowania aplikacji podrzędnej w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="b79f6-184">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="b79f6-185">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="b79f6-185">Troubleshooting</span></span>

<span data-ttu-id="b79f6-186">W przypadku odebrania *500 — wewnętrzny błąd serwera* , a Menedżer usług IIS zgłasza błędy przy próbie uzyskania dostępu do konfiguracji witryny sieci Web, upewnij się, że zainstalowano moduł ponownego zapisywania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="b79f6-186">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="b79f6-187">Gdy moduł nie jest zainstalowany, nie można przeanalizować pliku *Web. config* przez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="b79f6-187">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="b79f6-188">Zapobiega to załadowaniu przez Menedżera usług IIS konfiguracji witryny sieci Web i witryny sieci Web do obsługi plików statycznych Blazor.</span><span class="sxs-lookup"><span data-stu-id="b79f6-188">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="b79f6-189">Aby uzyskać więcej informacji na temat rozwiązywania problemów z wdrożeniami w usługach IIS, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="b79f6-189">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="b79f6-190">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b79f6-190">Azure Storage</span></span>

<span data-ttu-id="b79f6-191">Hosting pliku statycznego [usługi Azure Storage](/azure/storage/) umożliwia hosting aplikacji bezserwerowych Blazor.</span><span class="sxs-lookup"><span data-stu-id="b79f6-191">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="b79f6-192">Obsługiwane są niestandardowe nazwy domen, usługa Azure Content Delivery Network (CDN) i protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b79f6-192">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="b79f6-193">Gdy usługa BLOB jest włączona dla hostingu statycznej witryny sieci Web na koncie magazynu:</span><span class="sxs-lookup"><span data-stu-id="b79f6-193">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="b79f6-194">Ustaw **nazwę dokumentu indeksu** na `index.html`.</span><span class="sxs-lookup"><span data-stu-id="b79f6-194">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="b79f6-195">Ustaw **ścieżkę dokumentu błędu** na `index.html`.</span><span class="sxs-lookup"><span data-stu-id="b79f6-195">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="b79f6-196">Składniki Razor i inne punkty końcowe inne niż pliki nie znajdują się w ścieżkach fizycznych w zawartości statycznej przechowywanej przez usługę BLOB.</span><span class="sxs-lookup"><span data-stu-id="b79f6-196">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="b79f6-197">Po otrzymaniu żądania dla jednego z tych zasobów, który powinien być obsługiwany przez router Blazor, błąd *404-nie znaleziono* przez usługę BLOB Service kieruje żądanie do **ścieżki dokumentu błędu**.</span><span class="sxs-lookup"><span data-stu-id="b79f6-197">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="b79f6-198">Zwracany jest obiekt BLOB *index. html* , a router Blazor ładuje i przetwarza ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="b79f6-198">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="b79f6-199">Aby uzyskać więcej informacji, zobacz [Obsługa statycznej witryny sieci Web w usłudze Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="b79f6-199">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="b79f6-200">Nginx</span><span class="sxs-lookup"><span data-stu-id="b79f6-200">Nginx</span></span>

<span data-ttu-id="b79f6-201">Następujący plik *Nginx. conf* został uproszczony, aby pokazać, jak skonfigurować Nginx do wysyłania pliku *index. html* za każdym razem, gdy nie można znaleźć odpowiedniego pliku na dysku.</span><span class="sxs-lookup"><span data-stu-id="b79f6-201">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="b79f6-202">Aby uzyskać więcej informacji na temat konfiguracji serwera sieci Web w środowisku produkcyjnym, zobacz [Tworzenie plików konfiguracji Nginx Plus i Nginx](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="b79f6-202">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="b79f6-203">Nginx w Docker</span><span class="sxs-lookup"><span data-stu-id="b79f6-203">Nginx in Docker</span></span>

<span data-ttu-id="b79f6-204">Aby hostować Blazor w programie Docker przy użyciu Nginx, skonfiguruj pliku dockerfile do korzystania z obrazu Nginx opartego na Alpine.</span><span class="sxs-lookup"><span data-stu-id="b79f6-204">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="b79f6-205">Zaktualizuj pliku dockerfile, aby skopiować plik *Nginx. config* do kontenera.</span><span class="sxs-lookup"><span data-stu-id="b79f6-205">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="b79f6-206">Dodaj jeden wiersz do pliku dockerfile, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="b79f6-206">Add one line to the Dockerfile, as shown in the following example:</span></span>

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a><span data-ttu-id="b79f6-207">Apache</span><span class="sxs-lookup"><span data-stu-id="b79f6-207">Apache</span></span>

<span data-ttu-id="b79f6-208">Aby wdrożyć aplikację webassembly w programie Blazor CentOS 7 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="b79f6-208">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="b79f6-209">Utwórz plik konfiguracji Apache.</span><span class="sxs-lookup"><span data-stu-id="b79f6-209">Create the Apache configuration file.</span></span> <span data-ttu-id="b79f6-210">Poniższy przykład to uproszczony plik konfiguracji (*blazorapp. config*):</span><span class="sxs-lookup"><span data-stu-id="b79f6-210">The following example is a simplified configuration file (*blazorapp.config*):</span></span>

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType application/wasm .wasm
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

1. <span data-ttu-id="b79f6-211">Umieść plik konfiguracji Apache w katalogu `/etc/httpd/conf.d/`, który jest domyślnym katalogiem konfiguracji Apache w CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="b79f6-211">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="b79f6-212">Umieść pliki aplikacji w katalogu `/var/www/blazorapp` (lokalizacja określona do `DocumentRoot` w pliku konfiguracyjnym).</span><span class="sxs-lookup"><span data-stu-id="b79f6-212">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="b79f6-213">Uruchom ponownie usługę Apache.</span><span class="sxs-lookup"><span data-stu-id="b79f6-213">Restart the Apache service.</span></span>

<span data-ttu-id="b79f6-214">Aby uzyskać więcej informacji, zobacz [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) i [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span><span class="sxs-lookup"><span data-stu-id="b79f6-214">For more information, see [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="b79f6-215">Strony serwisu GitHub</span><span class="sxs-lookup"><span data-stu-id="b79f6-215">GitHub Pages</span></span>

<span data-ttu-id="b79f6-216">Aby obsłużyć ponowne zapisywanie adresów URL, Dodaj plik *404. html* ze skryptem, który obsługuje przekierowywanie żądania do strony *index. html* .</span><span class="sxs-lookup"><span data-stu-id="b79f6-216">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="b79f6-217">Aby zapoznać się z przykładową implementacją dostarczoną przez społeczność, zobacz [aplikacje jednostronicowe dla stron usługi GitHub](https://spa-github-pages.rafrex.com/) ([rafrex/Spa-GitHub-Pages w witrynie GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="b79f6-217">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="b79f6-218">Przykład użycia podejścia społecznościowego można znaleźć[w witrynie](https://blazor-demo.github.io/) [GitHub (blazor — Demonstracja/blazor-Demonstracja](https://github.com/blazor-demo/blazor-demo.github.io) ).</span><span class="sxs-lookup"><span data-stu-id="b79f6-218">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="b79f6-219">W przypadku korzystania z witryny projektu zamiast witryny organizacji Dodaj lub zaktualizuj tag `<base>` w *pliku index. html*.</span><span class="sxs-lookup"><span data-stu-id="b79f6-219">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="b79f6-220">Ustaw wartość atrybutu `href` na nazwę repozytorium GitHub z końcowym ukośnikiem (na przykład `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="b79f6-220">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="b79f6-221">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="b79f6-221">Host configuration values</span></span>

<span data-ttu-id="b79f6-222">[Blazor aplikacje webassembly](xref:blazor/hosting-models#blazor-webassembly) mogą akceptować następujące wartości konfiguracji hosta jako argumenty wiersza polecenia w czasie wykonywania w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="b79f6-222">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="b79f6-223">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="b79f6-223">Content root</span></span>

<span data-ttu-id="b79f6-224">Argument `--contentroot` ustawia ścieżkę bezwzględną do katalogu, który zawiera pliki zawartości aplikacji ([katalog główny zawartości](xref:fundamentals/index#content-root)).</span><span class="sxs-lookup"><span data-stu-id="b79f6-224">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="b79f6-225">W poniższych przykładach `/content-root-path` jest ścieżką katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-225">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="b79f6-226">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="b79f6-226">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="b79f6-227">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b79f6-227">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="b79f6-228">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="b79f6-228">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="b79f6-229">To ustawienie jest używane, gdy aplikacja jest uruchamiana z debugerem programu Visual Studio i z wiersza polecenia z `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b79f6-229">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="b79f6-230">W programie Visual Studio Określ argument we **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b79f6-230">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="b79f6-231">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b79f6-231">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="b79f6-232">Baza ścieżki</span><span class="sxs-lookup"><span data-stu-id="b79f6-232">Path base</span></span>

<span data-ttu-id="b79f6-233">`--pathbase` argument ustawia ścieżkę bazową aplikacji dla aplikacji uruchamianej lokalnie z niegłówną względną ścieżką URL (tag `<base>` `href` jest ustawiona na ścieżkę inną niż `/` do przemieszczania i produkcji).</span><span class="sxs-lookup"><span data-stu-id="b79f6-233">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="b79f6-234">W poniższych przykładach `/relative-URL-path` jest baza ścieżki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b79f6-234">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="b79f6-235">Aby uzyskać więcej informacji, zobacz [Ścieżka podstawowa aplikacji](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="b79f6-235">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b79f6-236">W przeciwieństwie do ścieżki podanej do `href` tagu `<base>`, nie dodawaj końcowego ukośnika (`/`) podczas przekazywania wartości argumentu `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="b79f6-236">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="b79f6-237">Jeśli ścieżka podstawowa aplikacji znajduje się w tagu `<base>` jako `<base href="/CoolApp/">` (zawiera końcowy ukośnik), należy przekazać wartość argumentu wiersza polecenia jako `--pathbase=/CoolApp` (bez ukośników końcowych).</span><span class="sxs-lookup"><span data-stu-id="b79f6-237">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="b79f6-238">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="b79f6-238">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="b79f6-239">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b79f6-239">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="b79f6-240">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="b79f6-240">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="b79f6-241">To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia z `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b79f6-241">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="b79f6-242">W programie Visual Studio Określ argument we **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b79f6-242">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="b79f6-243">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b79f6-243">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="b79f6-244">Adresy URL</span><span class="sxs-lookup"><span data-stu-id="b79f6-244">URLs</span></span>

<span data-ttu-id="b79f6-245">`--urls` argument ustawia adresy IP lub adresy hosta z portami i protokołami, aby nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="b79f6-245">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="b79f6-246">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="b79f6-246">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="b79f6-247">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b79f6-247">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="b79f6-248">Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="b79f6-248">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="b79f6-249">To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia z `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b79f6-249">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="b79f6-250">W programie Visual Studio Określ argument we **właściwościach** > **Debuguj** > **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b79f6-250">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="b79f6-251">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b79f6-251">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="b79f6-252">Konfigurowanie konsolidatora</span><span class="sxs-lookup"><span data-stu-id="b79f6-252">Configure the Linker</span></span>

Blazor<span data-ttu-id="b79f6-253"> wykonuje konsolidację języka pośredniego (IL) dla każdej kompilacji wydania, aby usunąć niepotrzebny kod IL z zestawów wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="b79f6-253"> performs Intermediate Language (IL) linking on each Release build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="b79f6-254">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="b79f6-254">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>
