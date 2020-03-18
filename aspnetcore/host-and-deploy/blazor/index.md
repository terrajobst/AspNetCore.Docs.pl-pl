---
title: Hostowanie i wdrażanie ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: ddf70da29a82d462422c1bdf74ff45b92bb10b56
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434268"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="04198-103">Hostowanie i wdrażanie ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="04198-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="04198-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="04198-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="04198-105">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="04198-105">Publish the app</span></span>

<span data-ttu-id="04198-106">Aplikacje są publikowane do wdrożenia w konfiguracji wydania.</span><span class="sxs-lookup"><span data-stu-id="04198-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="04198-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04198-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="04198-108">Wybierz pozycję **kompilacja** > **Opublikuj aplikację {Application}** na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="04198-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="04198-109">Wybierz *element docelowy publikowania*.</span><span class="sxs-lookup"><span data-stu-id="04198-109">Select the *publish target*.</span></span> <span data-ttu-id="04198-110">Aby opublikować lokalnie, wybierz pozycję **folder**.</span><span class="sxs-lookup"><span data-stu-id="04198-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="04198-111">Zaakceptuj lokalizację domyślną w polu **Wybierz folder** lub określ inną lokalizację.</span><span class="sxs-lookup"><span data-stu-id="04198-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="04198-112">Wybierz przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="04198-112">Select the **Publish** button.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="04198-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="04198-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="04198-114">Użyj [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia, aby opublikować aplikację z konfiguracją wydania:</span><span class="sxs-lookup"><span data-stu-id="04198-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="04198-115">Opublikowanie aplikacji wyzwala [przywracanie](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompiluje](/dotnet/core/tools/dotnet-build) projekt przed utworzeniem zasobów do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="04198-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="04198-116">W ramach procesu kompilacji nieużywane metody i zestawy są usuwane w celu zmniejszenia rozmiaru pobierania aplikacji i czasów ładowania.</span><span class="sxs-lookup"><span data-stu-id="04198-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="04198-117">Lokalizacje publikowania:</span><span class="sxs-lookup"><span data-stu-id="04198-117">Publish locations:</span></span>

* Blazor<span data-ttu-id="04198-118"> webassembly</span><span class="sxs-lookup"><span data-stu-id="04198-118"> WebAssembly</span></span>
  * <span data-ttu-id="04198-119">Autonomiczna &ndash; aplikacja jest publikowana w folderze */bin/Release/{Target Framework}/Publish/wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="04198-119">Standalone &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span> <span data-ttu-id="04198-120">Aby wdrożyć aplikację jako lokację statyczną, skopiuj zawartość folderu *wwwroot* do hosta lokacji statycznej.</span><span class="sxs-lookup"><span data-stu-id="04198-120">To deploy the app as a static site, copy the contents of the *wwwroot* folder to the static site host.</span></span>
  * <span data-ttu-id="04198-121">Hostowane &ndash; aplikacja webassembly Blazor Client jest publikowana w folderze */bin/Release/{Target Framework}/Publish/wwwroot* aplikacji serwera, wraz ze wszystkimi innymi statycznymi zasobami sieci Web aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="04198-121">Hosted &ndash; The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="04198-122">Wdróż zawartość folderu *publikowania* na hoście.</span><span class="sxs-lookup"><span data-stu-id="04198-122">Deploy the contents of the *publish* folder to the host.</span></span>
* Blazor<span data-ttu-id="04198-123"> Server &ndash; aplikacja zostanie opublikowana w folderze */bin/Release/{Target Framework}/Publish* .</span><span class="sxs-lookup"><span data-stu-id="04198-123"> Server &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="04198-124">Wdróż zawartość folderu *publikowania* na hoście.</span><span class="sxs-lookup"><span data-stu-id="04198-124">Deploy the contents of the *publish* folder to the host.</span></span>

<span data-ttu-id="04198-125">Zasoby w folderze są wdrażane na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="04198-125">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="04198-126">Wdrożenie może być procesem ręcznym lub zautomatyzowanym w zależności od używanych narzędzi programistycznych.</span><span class="sxs-lookup"><span data-stu-id="04198-126">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="04198-127">Ścieżka podstawowa aplikacji</span><span class="sxs-lookup"><span data-stu-id="04198-127">App base path</span></span>

<span data-ttu-id="04198-128">*Ścieżka podstawowa aplikacji* jest ścieżką głównego adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04198-128">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="04198-129">Weź pod uwagę następujące ASP.NET Core aplikacji i Blazor aplikacji podrzędnej:</span><span class="sxs-lookup"><span data-stu-id="04198-129">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="04198-130">Aplikacja ASP.NET Core ma nazwę `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="04198-130">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="04198-131">Aplikacja fizycznie znajduje się na *d:/MojaApl*.</span><span class="sxs-lookup"><span data-stu-id="04198-131">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="04198-132">Żądania są odbierane w `https://www.contoso.com/{MYAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="04198-132">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="04198-133">Aplikacja Blazor o nazwie `CoolApp` jest podaplikacją `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="04198-133">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="04198-134">Aplikacja podrzędna fizycznie znajduje się na *d:/MojaApl/CoolApp*.</span><span class="sxs-lookup"><span data-stu-id="04198-134">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="04198-135">Żądania są odbierane w `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="04198-135">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="04198-136">Bez określania dodatkowej konfiguracji `CoolApp`aplikacja podrzędna w tym scenariuszu nie ma informacji o tym, gdzie znajduje się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="04198-136">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="04198-137">Na przykład aplikacja nie może utworzyć prawidłowych względnych adresów URL do swoich zasobów, nie wiedząc, że znajduje się w względnej ścieżce adresu URL `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="04198-137">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="04198-138">Aby zapewnić konfigurację Blazor ścieżce podstawowej `https://www.contoso.com/CoolApp/`, atrybut `href` tagu `<base>` jest ustawiany na względną ścieżkę katalogu głównego w pliku *Pages/_Host. cshtml* (Blazor Server) lub pliku *wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="04198-138">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

<span data-ttu-id="04198-139">aplikacje serwera Blazor dodatkowo ustawiać ścieżkę bazową po stronie serwera, wywołując <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> w potoku żądania aplikacji `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="04198-139">Blazor Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="04198-140">Dostarczając względną ścieżkę adresu URL, składnik, który nie znajduje się w katalogu głównym, może konstruować adresy URL względem ścieżki katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04198-140">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="04198-141">Składniki na różnych poziomach struktury katalogów mogą tworzyć linki do innych zasobów w lokalizacji w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04198-141">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="04198-142">Ścieżka podstawowa aplikacji służy również do przechwytywania wybranych hiperłączy, w których `href` miejsce docelowe łącza znajduje się w obszarze URI ścieżki podstawowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04198-142">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="04198-143">Router Blazor obsługuje nawigację wewnętrzną.</span><span class="sxs-lookup"><span data-stu-id="04198-143">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="04198-144">W wielu scenariuszach hostingu względna ścieżka URL do aplikacji jest katalogiem głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04198-144">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="04198-145">W takich przypadkach ścieżką bazową względnego adresu URL aplikacji jest ukośnik (`<base href="/" />`), który jest domyślną konfiguracją aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="04198-145">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="04198-146">W innych scenariuszach hostingu, takich jak strony GitHub i aplikacje podrzędne IIS, ścieżka podstawowa aplikacji musi być ustawiona na względną ścieżkę URL serwera aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04198-146">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="04198-147">Aby ustawić ścieżkę bazową aplikacji, zaktualizuj tag `<base>` wewnątrz elementów tagów `<head>` pliku *Pages/_Host. cshtml* (serwerBlazor) lub plik *wwwroot/index.html* (Blazor webassembly).</span><span class="sxs-lookup"><span data-stu-id="04198-147">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="04198-148">Ustaw wartość atrybutu `href` na `/{RELATIVE URL PATH}/` (wymagany jest końcowy ukośnik), gdzie `{RELATIVE URL PATH}` jest pełną względną ścieżką URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04198-148">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="04198-149">W przypadku aplikacji Blazor webassembly z ścieżką względnego adresu URL niegłówną (na przykład `<base href="/CoolApp/">`) aplikacja nie będzie mogła znaleźć zasobów, *gdy są uruchamiane lokalnie*.</span><span class="sxs-lookup"><span data-stu-id="04198-149">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="04198-150">Aby rozwiązać ten problem podczas lokalnego tworzenia i testowania, można podać podstawowy argument *ścieżki* , który odpowiada wartości `href` tagu `<base>` w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="04198-150">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="04198-151">Nie dołączaj końcowego ukośnika.</span><span class="sxs-lookup"><span data-stu-id="04198-151">Don't include a trailing slash.</span></span> <span data-ttu-id="04198-152">Aby przekazać argument podstawowy ścieżki podczas lokalnego uruchamiania aplikacji, uruchom `dotnet run` polecenie w katalogu aplikacji z opcją `--pathbase`:</span><span class="sxs-lookup"><span data-stu-id="04198-152">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="04198-153">W przypadku aplikacji Blazor webassembly ze względną ścieżką URL `/CoolApp/` (`<base href="/CoolApp/">`) polecenie to:</span><span class="sxs-lookup"><span data-stu-id="04198-153">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="04198-154">Aplikacja webassembly Blazor reaguje lokalnie na `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="04198-154">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="04198-155">wdrażania</span><span class="sxs-lookup"><span data-stu-id="04198-155">Deployment</span></span>

<span data-ttu-id="04198-156">Aby uzyskać wskazówki dotyczące wdrażania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="04198-156">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
