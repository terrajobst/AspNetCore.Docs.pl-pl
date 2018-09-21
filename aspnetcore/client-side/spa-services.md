---
title: Korzystanie z technologii JavaScriptServices do tworzenia aplikacji jednostronicowej w programie ASP.NET Core
author: scottaddie
description: Dowiedz się więcej o zaletach korzystania z technologii JavaScriptServices w celu utworzenia jednej strony aplikacji (SPA) wspartą platformy ASP.NET Core.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6d6a92427d5d4b853248e60a12625573c4375515
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523301"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="43ff0-103">Korzystanie z technologii JavaScriptServices do tworzenia aplikacji jednostronicowej w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43ff0-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="43ff0-104">Przez [Scott Addie](https://github.com/scottaddie) i [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="43ff0-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="43ff0-105">Jednej strony aplikacji (SPA) jest typem popularnych aplikacji sieci web ze względu na jej nieodłączne zaawansowanego środowiska użytkownika.</span><span class="sxs-lookup"><span data-stu-id="43ff0-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="43ff0-106">Integrowanie SPA struktur po stronie klienta lub bibliotek, takich jak [Angular](https://angular.io/) lub [React](https://facebook.github.io/react/), przy użyciu struktur po stronie serwera, takich jak ASP.NET Core może być trudne.</span><span class="sxs-lookup"><span data-stu-id="43ff0-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="43ff0-107">[Technologii JavaScriptServices](https://github.com/aspnet/JavaScriptServices) został opracowany, aby zmniejszyć liczbę problemów w procesie integracji.</span><span class="sxs-lookup"><span data-stu-id="43ff0-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="43ff0-108">Dzięki temu bezproblemowe wartościach innego klienta i serwera stosów technologicznych.</span><span class="sxs-lookup"><span data-stu-id="43ff0-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="43ff0-109">Co to jest technologii JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="43ff0-109">What is JavaScriptServices</span></span>

<span data-ttu-id="43ff0-110">Technologii JavaScriptServices jest kolekcja technologii po stronie klienta dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43ff0-110">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="43ff0-111">Jego celem jest pozwala platformy ASP.NET Core jako preferowaną platformę po stronie serwera deweloperów do tworzenia aplikacji jednostronicowych.</span><span class="sxs-lookup"><span data-stu-id="43ff0-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="43ff0-112">Technologii JavaScriptServices składa się z trzech odrębnych pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="43ff0-112">JavaScriptServices consists of three distinct NuGet packages:</span></span>

* <span data-ttu-id="43ff0-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="43ff0-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="43ff0-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="43ff0-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="43ff0-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="43ff0-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="43ff0-116">Te pakiety są przydatne w przypadku możesz:</span><span class="sxs-lookup"><span data-stu-id="43ff0-116">These packages are useful if you:</span></span>

* <span data-ttu-id="43ff0-117">Uruchom JavaScript na serwerze</span><span class="sxs-lookup"><span data-stu-id="43ff0-117">Run JavaScript on the server</span></span>
* <span data-ttu-id="43ff0-118">SPA framework lub biblioteki</span><span class="sxs-lookup"><span data-stu-id="43ff0-118">Use a SPA framework or library</span></span>
* <span data-ttu-id="43ff0-119">Tworzenie zasobów po stronie klienta za pomocą Webpack</span><span class="sxs-lookup"><span data-stu-id="43ff0-119">Build client-side assets with Webpack</span></span>

<span data-ttu-id="43ff0-120">Wiele wysiłków w tym artykule jest umieszczany na temat korzystania z pakietu SpaServices.</span><span class="sxs-lookup"><span data-stu-id="43ff0-120">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="43ff0-121">Co to jest SpaServices</span><span class="sxs-lookup"><span data-stu-id="43ff0-121">What is SpaServices</span></span>

<span data-ttu-id="43ff0-122">SpaServices utworzono pozwala platformy ASP.NET Core jako preferowaną platformę po stronie serwera deweloperów do tworzenia aplikacji jednostronicowych.</span><span class="sxs-lookup"><span data-stu-id="43ff0-122">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="43ff0-123">SpaServices nie jest wymagane do rozwoju aplikacji jednostronicowych za pomocą programu ASP.NET Core, a nie blokuje w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="43ff0-123">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="43ff0-124">SpaServices zawiera przydatne infrastruktury, takich jak:</span><span class="sxs-lookup"><span data-stu-id="43ff0-124">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="43ff0-125">Prerendering po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="43ff0-125">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="43ff0-126">Oprogramowanie pośredniczące Dev Webpack</span><span class="sxs-lookup"><span data-stu-id="43ff0-126">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="43ff0-127">Zastąpienie gorąca modułu</span><span class="sxs-lookup"><span data-stu-id="43ff0-127">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="43ff0-128">Pomocnicy routingu</span><span class="sxs-lookup"><span data-stu-id="43ff0-128">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="43ff0-129">Zbiorczo te składniki infrastruktury pozwala zwiększyć przepływ pracy tworzenia oprogramowania i środowiska czasu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="43ff0-129">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="43ff0-130">Składniki mogą przyjąć indywidualnie.</span><span class="sxs-lookup"><span data-stu-id="43ff0-130">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="43ff0-131">Wymagania wstępne dotyczące korzystania z SpaServices</span><span class="sxs-lookup"><span data-stu-id="43ff0-131">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="43ff0-132">Aby pracować z SpaServices, należy zainstalować następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="43ff0-132">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="43ff0-133">[Node.js](https://nodejs.org/) (w wersji 6 lub nowszej) za pomocą pakietu npm</span><span class="sxs-lookup"><span data-stu-id="43ff0-133">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="43ff0-134">Aby sprawdzić te składniki są zainstalowane i można go znaleźć, uruchom następujące polecenie w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="43ff0-134">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="43ff0-135">Uwaga: Jeśli wdrażasz do witryny sieci web systemu Azure, nie trzeba wykonywać żadnych czynności w tym miejscu &mdash; Node.js jest zainstalowany i dostępny w środowisku serwerów.</span><span class="sxs-lookup"><span data-stu-id="43ff0-135">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="43ff0-136">Jeśli Windows przy użyciu programu Visual Studio 2017, zestaw SDK jest zainstalowany, wybierając **programowanie dla wielu platform .NET Core** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="43ff0-136">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="43ff0-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span><span class="sxs-lookup"><span data-stu-id="43ff0-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="43ff0-138">Prerendering po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="43ff0-138">Server-side prerendering</span></span>

<span data-ttu-id="43ff0-139">Uniwersalnej aplikacji (nazywane również isomorphic) jest stanie działać zarówno serwer i klienta aplikacji w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="43ff0-139">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="43ff0-140">Platformy Angular, React i innych popularnych platform podanie tego stylu programowania aplikacji platformy uniwersalnej.</span><span class="sxs-lookup"><span data-stu-id="43ff0-140">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="43ff0-141">Chodzi o to, aby najpierw przetworzyć składników framework na serwerze za pomocą środowiska Node.js, a następnie poprzez delegowanie przekazać dalsze wykonywanie do klienta.</span><span class="sxs-lookup"><span data-stu-id="43ff0-141">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="43ff0-142">Platforma ASP.NET Core [pomocników tagów](xref:mvc/views/tag-helpers/intro) dostarczone przez SpaServices upraszczająca implementację elementów prerendering po stronie serwera za pomocą wywołania funkcji JavaScript na serwerze.</span><span class="sxs-lookup"><span data-stu-id="43ff0-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="43ff0-143">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="43ff0-143">Prerequisites</span></span>

<span data-ttu-id="43ff0-144">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="43ff0-144">Install the following:</span></span>

* <span data-ttu-id="43ff0-145">[ASPNET prerendering](https://www.npmjs.com/package/aspnet-prerendering) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="43ff0-145">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="43ff0-146">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="43ff0-146">Configuration</span></span>

<span data-ttu-id="43ff0-147">Pomocnicy tagów są wykrywalne za pośrednictwem rejestracji przestrzeni nazw w projekcie *_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="43ff0-147">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="43ff0-148">Te pomocników tagów natychmiast abstrakcji niewymagającego komunikować się bezpośrednio z interfejsy API niskiego poziomu, wykorzystując składnia przypominająca HTML w widoku Razor:</span><span class="sxs-lookup"><span data-stu-id="43ff0-148">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="43ff0-149">`asp-prerender-module` Pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="43ff0-149">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="43ff0-150">`asp-prerender-module` Wykonuje pomocnika tagów, używany w poprzednim przykładzie kodu *ClientApp/dist/main-server.js* na serwerze za pomocą środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="43ff0-150">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="43ff0-151">Dla jasności *main server.js* pliku jest pozostałością zadania transpilation TypeScript i JavaScript w [Webpack](http://webpack.github.io/) procesu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="43ff0-151">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="43ff0-152">Webpack definiuje alias punktu wejścia `main-server`; i przechodzenie wykres zależności dla tego aliasu, który rozpoczyna się od *ClientApp/rozruchu server.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="43ff0-152">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="43ff0-153">W poniższym przykładzie Angular *ClientApp/rozruchu server.ts* korzysta z pliku `createServerRenderer` funkcji i `RenderResult` typu `aspnet-prerendering` pakietu npm, aby skonfigurować renderowanie na serwerze za pomocą środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="43ff0-153">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="43ff0-154">Kod znaczników HTML przeznaczonej do renderowania po stronie serwera jest przekazywany do wywołanie funkcji rozwiązania jest opakowywany w języku JavaScript silnie typizowane `Promise` obiektu.</span><span class="sxs-lookup"><span data-stu-id="43ff0-154">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="43ff0-155">`Promise` Znaczenie obiektu to, że asynchronicznie dostarcza kod znaczników HTML strony do iniekcji w elemencie symbolu zastępczego DOM w LICZBIE.</span><span class="sxs-lookup"><span data-stu-id="43ff0-155">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="43ff0-156">`asp-prerender-data` Pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="43ff0-156">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="43ff0-157">W połączeniu z `asp-prerender-module` Pomocnik tagu `asp-prerender-data` Pomocnik tagu może służyć do przekazywania informacji kontekstowych z widoku Razor w kodzie JavaScript po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="43ff0-157">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="43ff0-158">Na przykład, następujący kod przekazuje dane użytkownika do `main-server` modułu:</span><span class="sxs-lookup"><span data-stu-id="43ff0-158">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="43ff0-159">Odebrano `UserName` argument jest serializowana, za pomocą wbudowanych serializator JSON i jest przechowywany w `params.data` obiektu.</span><span class="sxs-lookup"><span data-stu-id="43ff0-159">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="43ff0-160">W poniższym przykładzie Angular danych służy do tworzenia spersonalizowanych powitania w ramach `h1` elementu:</span><span class="sxs-lookup"><span data-stu-id="43ff0-160">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="43ff0-161">Uwaga: Nazwy właściwości przekazanej pomocników tagów są reprezentowane przez **PascalCase** notacji.</span><span class="sxs-lookup"><span data-stu-id="43ff0-161">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="43ff0-162">Natomiast, w kodzie JavaScript, w których takie same nazwy właściwości są reprezentowane za pomocą **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="43ff0-162">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="43ff0-163">Domyślna konfiguracja serializacji JSON jest odpowiedzialny za tę różnicę.</span><span class="sxs-lookup"><span data-stu-id="43ff0-163">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="43ff0-164">Aby rozszerzyć na poprzednim przykładzie kodu, dane mogą być przekazywane z serwera do widoku przez nawilżania `globals` właściwości udostępniane `resolve` funkcji:</span><span class="sxs-lookup"><span data-stu-id="43ff0-164">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="43ff0-165">`postList` Zdefiniowane wewnątrz tablicy `globals` obiekt jest dołączony do przeglądarki globalnego `window` obiektu.</span><span class="sxs-lookup"><span data-stu-id="43ff0-165">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="43ff0-166">Tej zmiennej podnoszenia do zakresu globalnego eliminuje dublowania działań, szczególnie w przypadku, ponieważ odnoszą się do ładowania tych samych danych, gdy na serwerze i ponownie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="43ff0-166">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![Zmienna globalna postList dołączony do obiektu okna](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="43ff0-168">Oprogramowanie pośredniczące Dev Webpack</span><span class="sxs-lookup"><span data-stu-id="43ff0-168">Webpack Dev Middleware</span></span>

<span data-ttu-id="43ff0-169">[Oprogramowanie pośredniczące Dev Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) wprowadza usprawnione Tworzenie przepływu pracy, według której Webpack kompilacji zasobów na żądanie.</span><span class="sxs-lookup"><span data-stu-id="43ff0-169">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="43ff0-170">Oprogramowanie pośredniczące kompiluje i automatycznie obsługuje zasoby po stronie klienta po załadowaniu strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="43ff0-170">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="43ff0-171">Alternatywne podejście jest ręcznie wywołać Webpack za pośrednictwem skryptu kompilacji npm projektu podczas zmiany zależności innych firm lub kod niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="43ff0-171">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="43ff0-172">Npm kompilacja skrypt w *package.json* plik jest wyświetlany w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="43ff0-172">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a><span data-ttu-id="43ff0-173">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="43ff0-173">Prerequisites</span></span>

<span data-ttu-id="43ff0-174">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="43ff0-174">Install the following:</span></span>

* <span data-ttu-id="43ff0-175">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="43ff0-175">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="43ff0-176">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="43ff0-176">Configuration</span></span>

<span data-ttu-id="43ff0-177">Oprogramowanie pośredniczące Dev Webpack zostanie zarejestrowana w Potok żądań HTTP za pośrednictwem następujący kod w *Startup.cs* pliku `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="43ff0-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="43ff0-178">`UseWebpackDevMiddleware` — Metoda rozszerzenia musi zostać wywołana przed [rejestrowanie hostowania plików statycznych](xref:fundamentals/static-files) za pośrednictwem `UseStaticFiles` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="43ff0-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="43ff0-179">Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące, tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="43ff0-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="43ff0-180">*Webpack.config.js* pliku `output.publicPath` właściwość informuje oprogramowanie pośredniczące, aby obejrzeć `dist` folder zmiany:</span><span class="sxs-lookup"><span data-stu-id="43ff0-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="43ff0-181">Zastąpienie gorąca modułu</span><span class="sxs-lookup"><span data-stu-id="43ff0-181">Hot Module Replacement</span></span>

<span data-ttu-id="43ff0-182">Traktować firmy Webpack [gorąca zastąpienie modułu](https://webpack.js.org/concepts/hot-module-replacement/) funkcji (HMR) jako unowocześnienia funkcji [Webpack Dev w oprogramowaniu pośredniczącym](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="43ff0-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="43ff0-183">HMR wprowadza te same korzyści, ale dodatkowo upraszcza przepływ pracy tworzenia oprogramowania przez automatyczną aktualizację zawartość strony po kompilacji zmian.</span><span class="sxs-lookup"><span data-stu-id="43ff0-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="43ff0-184">Nie pomyl go z odświeżaniem przeglądarki i mogące zakłócać bieżący stan w pamięci i sesji debugowania SPA.</span><span class="sxs-lookup"><span data-stu-id="43ff0-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="43ff0-185">Istnieje aktywne łącze między usługą Webpack Dev w oprogramowaniu pośredniczącym i przeglądarki, co oznacza, że zmiany są przekazywane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="43ff0-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="43ff0-186">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="43ff0-186">Prerequisites</span></span>

<span data-ttu-id="43ff0-187">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="43ff0-187">Install the following:</span></span>

* <span data-ttu-id="43ff0-188">[webpack-hot — oprogramowanie pośredniczące](https://www.npmjs.com/package/webpack-hot-middleware) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="43ff0-188">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="43ff0-189">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="43ff0-189">Configuration</span></span>

<span data-ttu-id="43ff0-190">Składnik HMR muszą być rejestrowane w Potok żądań HTTP MVC w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="43ff0-190">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="43ff0-191">Jak był prawdziwy z [oprogramowania pośredniczącego Dev Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` — metoda rozszerzenia musi zostać wywołana przed `UseStaticFiles` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="43ff0-191">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="43ff0-192">Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące, tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="43ff0-192">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="43ff0-193">*Webpack.config.js* musi definiować plik `plugins` tablicy, nawet wtedy, gdy zostanie pozostawiony pusty:</span><span class="sxs-lookup"><span data-stu-id="43ff0-193">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="43ff0-194">Po załadowaniu aplikacji w przeglądarce, karty konsoli narzędzi dla deweloperów zawiera potwierdzenie HMR aktywacji:</span><span class="sxs-lookup"><span data-stu-id="43ff0-194">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Gorąca komunikat połączonych zastąpienie modułu](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="43ff0-196">Pomocnicy routingu</span><span class="sxs-lookup"><span data-stu-id="43ff0-196">Routing helpers</span></span>

<span data-ttu-id="43ff0-197">W większości aplikacji jednostronicowych oparte na programie ASP.NET Core można po stronie klienta routingu oprócz routingu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="43ff0-197">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="43ff0-198">Systemy routingu SPA i MVC może działać niezależnie bez zakłóceń.</span><span class="sxs-lookup"><span data-stu-id="43ff0-198">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="43ff0-199">Brak, jednak jednej krawędzi przypadków stanowiące wyzwań: Identyfikowanie odpowiedzi HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="43ff0-199">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="43ff0-200">Rozważmy scenariusz, w którym będą trasę `/some/page` jest używany.</span><span class="sxs-lookup"><span data-stu-id="43ff0-200">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="43ff0-201">Przyjęto założenie, żądanie nie-dopasowania do wzorca trasy po stronie serwera, ale jego dopasowanie do wzorca, trasę po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="43ff0-201">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="43ff0-202">Teraz należy wziąć pod uwagę żądanie przychodzące dla `/images/user-512.png`, którego oczekuje ogólnie można znaleźć pliku obrazu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="43ff0-202">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="43ff0-203">Jeśli tej ścieżki żądany zasób nie jest zgodny, trasie po stronie serwera lub pliku statycznego, jest mało prawdopodobne, że aplikacja kliencka będzie go obsłużyć — zwykle chcesz zwrócić kod stanu HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="43ff0-203">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="43ff0-204">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="43ff0-204">Prerequisites</span></span>

<span data-ttu-id="43ff0-205">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="43ff0-205">Install the following:</span></span>

* <span data-ttu-id="43ff0-206">Po stronie klienta routingu pakietu npm.</span><span class="sxs-lookup"><span data-stu-id="43ff0-206">The client-side routing npm package.</span></span> <span data-ttu-id="43ff0-207">Przy użyciu usługi Angular, na przykład:</span><span class="sxs-lookup"><span data-stu-id="43ff0-207">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="43ff0-208">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="43ff0-208">Configuration</span></span>

<span data-ttu-id="43ff0-209">Metody rozszerzenia o nazwie `MapSpaFallbackRoute` jest używany w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="43ff0-209">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="43ff0-210">Porada: Trasy są obliczane w kolejności, w którym są one konfigurowane.</span><span class="sxs-lookup"><span data-stu-id="43ff0-210">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="43ff0-211">W związku z tym `default` trasy w poprzednim przykładzie kodu jest używana jako pierwsza dopasowywanie do wzorców.</span><span class="sxs-lookup"><span data-stu-id="43ff0-211">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="43ff0-212">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="43ff0-212">Creating a new project</span></span>

<span data-ttu-id="43ff0-213">Technologii JavaScriptServices udostępnia szablony wstępnie skonfigurowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43ff0-213">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="43ff0-214">SpaServices jest używany w tych szablonów, w połączeniu z różnych platform i bibliotek, takich jak Angular, React i kontenera Redux.</span><span class="sxs-lookup"><span data-stu-id="43ff0-214">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="43ff0-215">Szablony te można zainstalować za pomocą interfejsu wiersza polecenia platformy .NET Core, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43ff0-215">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="43ff0-216">Zostanie wyświetlona lista dostępnych szablonów SPA:</span><span class="sxs-lookup"><span data-stu-id="43ff0-216">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="43ff0-217">Szablony</span><span class="sxs-lookup"><span data-stu-id="43ff0-217">Templates</span></span>                                 | <span data-ttu-id="43ff0-218">Krótka nazwa</span><span class="sxs-lookup"><span data-stu-id="43ff0-218">Short Name</span></span> | <span data-ttu-id="43ff0-219">Język</span><span class="sxs-lookup"><span data-stu-id="43ff0-219">Language</span></span> | <span data-ttu-id="43ff0-220">Znaczniki</span><span class="sxs-lookup"><span data-stu-id="43ff0-220">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="43ff0-221">MVC platformy ASP.NET Core przy użyciu usługi Angular</span><span class="sxs-lookup"><span data-stu-id="43ff0-221">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="43ff0-222">Platformy angular</span><span class="sxs-lookup"><span data-stu-id="43ff0-222">angular</span></span>    | <span data-ttu-id="43ff0-223">[C#]</span><span class="sxs-lookup"><span data-stu-id="43ff0-223">[C#]</span></span>     | <span data-ttu-id="43ff0-224">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="43ff0-224">Web/MVC/SPA</span></span> |
| <span data-ttu-id="43ff0-225">MVC platformy ASP.NET Core z użyciem biblioteki React.js</span><span class="sxs-lookup"><span data-stu-id="43ff0-225">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="43ff0-226">react</span><span class="sxs-lookup"><span data-stu-id="43ff0-226">react</span></span>      | <span data-ttu-id="43ff0-227">[C#]</span><span class="sxs-lookup"><span data-stu-id="43ff0-227">[C#]</span></span>     | <span data-ttu-id="43ff0-228">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="43ff0-228">Web/MVC/SPA</span></span> |
| <span data-ttu-id="43ff0-229">MVC platformy ASP.NET Core z użyciem biblioteki React.js i Redux</span><span class="sxs-lookup"><span data-stu-id="43ff0-229">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="43ff0-230">reactredux dla platformy</span><span class="sxs-lookup"><span data-stu-id="43ff0-230">reactredux</span></span> | <span data-ttu-id="43ff0-231">[C#]</span><span class="sxs-lookup"><span data-stu-id="43ff0-231">[C#]</span></span>     | <span data-ttu-id="43ff0-232">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="43ff0-232">Web/MVC/SPA</span></span> |

<span data-ttu-id="43ff0-233">Aby utworzyć nowy projekt za pomocą jednego z szablonów SPA, należy dołączyć **krótką nazwę** szablonu w [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia.</span><span class="sxs-lookup"><span data-stu-id="43ff0-233">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="43ff0-234">Następujące polecenie tworzy aplikację platformy Angular z platformą ASP.NET Core MVC skonfigurowany po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="43ff0-234">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="43ff0-235">Tryb konfiguracji środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="43ff0-235">Set the runtime configuration mode</span></span>

<span data-ttu-id="43ff0-236">Istnieją dwa tryby konfiguracji podstawowego środowiska uruchomieniowego:</span><span class="sxs-lookup"><span data-stu-id="43ff0-236">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="43ff0-237">**Programowanie**:</span><span class="sxs-lookup"><span data-stu-id="43ff0-237">**Development**:</span></span>
  * <span data-ttu-id="43ff0-238">Obejmuje map źródeł, aby ułatwić debugowanie.</span><span class="sxs-lookup"><span data-stu-id="43ff0-238">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="43ff0-239">Nie Optymalizuj kod po stronie klienta dla wydajności.</span><span class="sxs-lookup"><span data-stu-id="43ff0-239">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="43ff0-240">**Produkcji**:</span><span class="sxs-lookup"><span data-stu-id="43ff0-240">**Production**:</span></span>
  * <span data-ttu-id="43ff0-241">Wyklucza mapy źródła.</span><span class="sxs-lookup"><span data-stu-id="43ff0-241">Excludes source maps.</span></span>
  * <span data-ttu-id="43ff0-242">Optymalizuje kod po stronie klienta za pomocą tworzenia pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="43ff0-242">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="43ff0-243">Platforma ASP.NET Core używa zmiennej środowiskowej o nazwie `ASPNETCORE_ENVIRONMENT` do przechowywania tryb konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="43ff0-243">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="43ff0-244">Zobacz **[należy ustawić środowisko](xref:fundamentals/environments#set-the-environment)** Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="43ff0-244">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="43ff0-245">Uruchamianie przy użyciu platformy .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="43ff0-245">Running with .NET Core CLI</span></span>

<span data-ttu-id="43ff0-246">Przywróć pakiety npm i wymagany NuGet, uruchamiając następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="43ff0-246">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="43ff0-247">Kompilowanie i uruchamianie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="43ff0-247">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="43ff0-248">Aplikacja uruchamia się na hoście lokalnym, zgodnie z opisem w [tryb konfiguracji środowiska uruchomieniowego](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="43ff0-248">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="43ff0-249">Przejdź do `http://localhost:5000` w przeglądarce zostanie wyświetlona strona docelowa.</span><span class="sxs-lookup"><span data-stu-id="43ff0-249">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="43ff0-250">Uruchamianie programu Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="43ff0-250">Running with Visual Studio 2017</span></span>

<span data-ttu-id="43ff0-251">Otwórz *.csproj* pliku wygenerowanego przez [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia.</span><span class="sxs-lookup"><span data-stu-id="43ff0-251">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="43ff0-252">Wymagane pakiety npm i NuGet zostaną przywrócone automatycznie po otwarty projekt.</span><span class="sxs-lookup"><span data-stu-id="43ff0-252">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="43ff0-253">Ten proces przywracania może potrwać kilka minut, a aplikacja jest gotowa do uruchomienia po jego ukończeniu.</span><span class="sxs-lookup"><span data-stu-id="43ff0-253">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="43ff0-254">Kliknij zielony przycisk uruchamiania lub naciśnij klawisz `Ctrl + F5`, i przeglądarce otworzy się Strona docelowa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43ff0-254">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="43ff0-255">Aplikacja zostanie uruchomiona na hoście lokalnym, zgodnie z opisem w [tryb konfiguracji środowiska uruchomieniowego](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="43ff0-255">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span>

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="43ff0-256">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="43ff0-256">Testing the app</span></span>

<span data-ttu-id="43ff0-257">Jest wstępnie skonfigurowana do uruchamiania testów po stronie klienta za pomocą szablonów SpaServices [Karma](https://karma-runner.github.io/1.0/index.html) i [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="43ff0-257">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="43ff0-258">Jasmine to popularne testowania jednostkowego dla języka JavaScript, Karma jest moduł uruchamiający testy dla tych testów.</span><span class="sxs-lookup"><span data-stu-id="43ff0-258">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="43ff0-259">Karma jest skonfigurowana do pracy z [oprogramowania pośredniczącego Dev Webpack](#webpack-dev-middleware) taki sposób, że deweloper nie jest wymagane do zatrzymywania i uruchamiania testu za każdym razem, gdy zostaną wprowadzone zmiany.</span><span class="sxs-lookup"><span data-stu-id="43ff0-259">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="43ff0-260">Czy jest ono kod działających w odniesieniu do przypadku testowego lub sam przypadek testowy, test jest uruchamiany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="43ff0-260">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="43ff0-261">Korzystanie z aplikacji Angular jako przykład dwóch Jasmine przypadki testowe są już udostępniane dla `CounterComponent` w *counter.component.spec.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="43ff0-261">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="43ff0-262">Otwórz wiersz polecenia w *ClientApp* katalogu.</span><span class="sxs-lookup"><span data-stu-id="43ff0-262">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="43ff0-263">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43ff0-263">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="43ff0-264">Skrypt uruchamia Karma narzędzie test runner, który odczytuje ustawienia zdefiniowane w *karma.conf.js* pliku.</span><span class="sxs-lookup"><span data-stu-id="43ff0-264">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="43ff0-265">Wśród innych ustawień *karma.conf.js* identyfikuje pliki testowe, które mają być wykonane za pomocą jego `files` tablicy:</span><span class="sxs-lookup"><span data-stu-id="43ff0-265">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="43ff0-266">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="43ff0-266">Publishing the application</span></span>

<span data-ttu-id="43ff0-267">Łączenie wygenerowanych elementów zawartości po stronie klienta i opublikowane artefaktów ASP.NET Core w gotowe do wdrożenia pakietu może być kłopotliwe.</span><span class="sxs-lookup"><span data-stu-id="43ff0-267">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="43ff0-268">Szczęście SpaServices organizuje procesu całej publikacji z niestandardowy cel programu MSBuild, o nazwie `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="43ff0-268">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="43ff0-269">Element docelowy programu MSBuild ma następujące obowiązki:</span><span class="sxs-lookup"><span data-stu-id="43ff0-269">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="43ff0-270">Przywróć pakiety npm</span><span class="sxs-lookup"><span data-stu-id="43ff0-270">Restore the npm packages</span></span>
1. <span data-ttu-id="43ff0-271">Tworzenie klasy produkcyjnej kompilacji zasobów innych firm, po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="43ff0-271">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="43ff0-272">Tworzenie klasy produkcyjnej kompilacji niestandardowych zasobów po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="43ff0-272">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="43ff0-273">Kopiuj zasoby generowane Webpack do folderu publikowania</span><span class="sxs-lookup"><span data-stu-id="43ff0-273">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="43ff0-274">Element docelowy programu MSBuild jest wywoływane, gdy uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="43ff0-274">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="43ff0-275">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="43ff0-275">Additional resources</span></span>

* [<span data-ttu-id="43ff0-276">Dokumentacja usługi angular</span><span class="sxs-lookup"><span data-stu-id="43ff0-276">Angular Docs</span></span>](https://angular.io/docs)
