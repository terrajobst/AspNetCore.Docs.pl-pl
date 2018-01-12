---
title: "Przy użyciu JavaScriptServices do tworzenia aplikacji jednej strony"
author: scottaddie
description: "Poznaj korzyści wynikające ze stosowania JavaScriptServices do utworzenia jednej strony aplikacji JEDNOSTRONICOWEJ obsługiwana przez platformy ASP.NET Core."
keywords: "Platformy ASP.NET Core kątowego, SPA, JavaScriptServices, SpaServices"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/spa-services
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d47910beef9195295c8da6ac81b83b3ffe20124
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/21/2017
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a><span data-ttu-id="122c3-104">Do tworzenia aplikacji jednej strony z platformy ASP.NET Core za pomocą JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="122c3-104">Using JavaScriptServices for Creating Single Page Applications with ASP.NET Core</span></span>

<span data-ttu-id="122c3-105">Przez [Scott Addie](https://github.com/scottaddie) i [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="122c3-105">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="122c3-106">Jednej strony aplikacji (SPA) jest typem popularnych aplikacji sieci web z powodu jego związanego z używaniem zaawansowanego środowiska użytkownika.</span><span class="sxs-lookup"><span data-stu-id="122c3-106">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="122c3-107">Integrowanie struktur SPA po stronie klienta lub biblioteki, takich jak [kątową](https://angular.io/) lub [React](https://facebook.github.io/react/), z platform po stronie serwera, takich jak ASP.NET Core mogą być trudne.</span><span class="sxs-lookup"><span data-stu-id="122c3-107">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="122c3-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) został opracowany, aby zmniejszyć tarcia w procesie integracji.</span><span class="sxs-lookup"><span data-stu-id="122c3-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="122c3-109">Umożliwia bezproblemowe operacji między innego klienta i serwera technologii stosów.</span><span class="sxs-lookup"><span data-stu-id="122c3-109">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="122c3-110">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="122c3-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="122c3-111">Co to jest JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="122c3-111">What is JavaScriptServices?</span></span>

<span data-ttu-id="122c3-112">JavaScriptServices jest kolekcja technologii po stronie klienta dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="122c3-112">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="122c3-113">Jego celem jest pozwala platformy ASP.NET Core jako deweloperów preferowaną platformę po stronie serwera do tworzenia źródła.</span><span class="sxs-lookup"><span data-stu-id="122c3-113">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="122c3-114">JavaScriptServices składa się z trzech różnych pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="122c3-114">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="122c3-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="122c3-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="122c3-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="122c3-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="122c3-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="122c3-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="122c3-118">Te pakiety są przydatne w przypadku możesz:</span><span class="sxs-lookup"><span data-stu-id="122c3-118">These packages are useful if you:</span></span>
* <span data-ttu-id="122c3-119">Uruchom JavaScript na serwerze</span><span class="sxs-lookup"><span data-stu-id="122c3-119">Run JavaScript on the server</span></span>
* <span data-ttu-id="122c3-120">Użyj SPA framework lub biblioteki</span><span class="sxs-lookup"><span data-stu-id="122c3-120">Use a SPA framework or library</span></span>
* <span data-ttu-id="122c3-121">Tworzenie zasobów po stronie klienta z Webpack</span><span class="sxs-lookup"><span data-stu-id="122c3-121">Build client-side assets with Webpack</span></span>

<span data-ttu-id="122c3-122">Większość fokus w tym artykule jest umieszczona na przy użyciu pakietu SpaServices.</span><span class="sxs-lookup"><span data-stu-id="122c3-122">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="122c3-123">Co to jest SpaServices?</span><span class="sxs-lookup"><span data-stu-id="122c3-123">What is SpaServices?</span></span>

<span data-ttu-id="122c3-124">SpaServices utworzono pozwala platformy ASP.NET Core jako deweloperów preferowaną platformę po stronie serwera do tworzenia źródła.</span><span class="sxs-lookup"><span data-stu-id="122c3-124">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="122c3-125">SpaServices nie jest wymagane do opracowywania źródła z platformy ASP.NET Core, a nie blokuje w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="122c3-125">SpaServices is not required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="122c3-126">SpaServices zapewnia przydatne infrastruktury, takich jak:</span><span class="sxs-lookup"><span data-stu-id="122c3-126">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="122c3-127">Prerendering po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="122c3-127">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="122c3-128">Oprogramowanie pośredniczące deweloperów Webpack</span><span class="sxs-lookup"><span data-stu-id="122c3-128">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="122c3-129">Moduł dynamicznej wymiany</span><span class="sxs-lookup"><span data-stu-id="122c3-129">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="122c3-130">Pomocnicy routingu</span><span class="sxs-lookup"><span data-stu-id="122c3-130">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="122c3-131">Zbiorczo te składniki infrastruktury zwiększenia zarówno przepływu pracy programowania i obsługi środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="122c3-131">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="122c3-132">Składniki może przyjąć pojedynczo.</span><span class="sxs-lookup"><span data-stu-id="122c3-132">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="122c3-133">Wymagania wstępne dotyczące korzystania z SpaServices</span><span class="sxs-lookup"><span data-stu-id="122c3-133">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="122c3-134">Aby pracować z SpaServices, należy zainstalować następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="122c3-134">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="122c3-135">[Node.js](https://nodejs.org/) (w wersji 6 lub nowszej) z pakietu npm</span><span class="sxs-lookup"><span data-stu-id="122c3-135">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="122c3-136">Aby sprawdzić te składniki są zainstalowane i można go znaleźć, należy uruchomić następujące polecenie z wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="122c3-136">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="122c3-137">Uwaga: Jeśli wdrażasz do witryny sieci web platformy Azure, nie musisz tutaj wykonywać żadnych czynności &mdash; Node.js jest zainstalowany i dostępny w środowisku serwerów.</span><span class="sxs-lookup"><span data-stu-id="122c3-137">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="122c3-138">[Zestaw SDK programu .NET core](https://www.microsoft.com/net/download/core) 1.0 (lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="122c3-138">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="122c3-139">Jeśli w systemie Windows, to można zainstalować, wybierając w Visual Studio 2017 **aplikacji dla wielu platform .NET Core** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="122c3-139">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="122c3-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) pakietu NuGet</span><span class="sxs-lookup"><span data-stu-id="122c3-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="122c3-141">Prerendering po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="122c3-141">Server-side prerendering</span></span>

<span data-ttu-id="122c3-142">Uniwersalnej aplikacji (nazywane również isomorphic) to aplikacja JavaScript może działać zarówno na serwerze i kliencie.</span><span class="sxs-lookup"><span data-stu-id="122c3-142">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="122c3-143">Kątową platformy React i innych popularnych struktur podanie tego stylu rozwoju aplikacji platformy uniwersalnej.</span><span class="sxs-lookup"><span data-stu-id="122c3-143">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="122c3-144">Koncepcja jest najpierw przetworzyć składników framework na serwerze za pośrednictwem środowiska Node.js, a następnie poprzez delegowanie przekazać dalej wykonywania do klienta.</span><span class="sxs-lookup"><span data-stu-id="122c3-144">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="122c3-145">Platformy ASP.NET Core [pomocników tagów](xref:mvc/views/tag-helpers/intro) dostarczonych przez SpaServices upraszczająca implementację elementów prerendering po stronie serwera za pomocą funkcji JavaScript na serwerze.</span><span class="sxs-lookup"><span data-stu-id="122c3-145">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="122c3-146">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="122c3-146">Prerequisites</span></span>

<span data-ttu-id="122c3-147">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="122c3-147">Install the following:</span></span>
* <span data-ttu-id="122c3-148">[ASPNET prerendering](https://www.npmjs.com/package/aspnet-prerendering) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="122c3-148">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="122c3-149">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="122c3-149">Configuration</span></span>

<span data-ttu-id="122c3-150">Pomocników tagów zostają wykrywalny przez rejestracji przestrzeni nazw w projekcie *_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="122c3-150">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="122c3-151">Te pomocników tagów optymalizacji abstrakcyjnej mogli dokładnie zapoznać się z komunikują się bezpośrednio z interfejsów API niskiego poziomu przy użyciu składni notacji języka HTML w widoku Razor:</span><span class="sxs-lookup"><span data-stu-id="122c3-151">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="122c3-152">`asp-prerender-module` Pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="122c3-152">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="122c3-153">`asp-prerender-module` Wykonuje pomocnika tagów, używany w poprzednim przykładzie kodu *ClientApp/dist/main-server.js* na serwerze za pośrednictwem środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="122c3-153">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="122c3-154">Dla jasności *main server.js* plik jest artefaktu zadania transpilation TypeScript i JavaScript w [Webpack](http://webpack.github.io/) proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="122c3-154">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="122c3-155">Webpack definiuje alias punktu wejścia `main-server`; i przechodzenie na wykresie zależności dla tego aliasu zaczyna się od *ClientApp/rozruchu server.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="122c3-155">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="122c3-156">W poniższym przykładzie kątowego *ClientApp/rozruchu server.ts* korzysta z pliku `createServerRenderer` funkcji i `RenderResult` typu `aspnet-prerendering` pakiet npm, aby skonfigurować renderowania serwera za pomocą środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="122c3-156">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="122c3-157">Kod znaczników HTML, przeznaczonych do renderowania po stronie serwera jest przekazywana do wywołanie funkcji rozpoznawania jest ujęte w języku JavaScript jednoznacznie `Promise` obiektu.</span><span class="sxs-lookup"><span data-stu-id="122c3-157">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="122c3-158">`Promise` Znaczenie obiektu to, że asynchronicznie dostarcza kod znaczników HTML strony iniekcji w modelu DOM — symbol zastępczy elementu.</span><span class="sxs-lookup"><span data-stu-id="122c3-158">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="122c3-159">`asp-prerender-data` Pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="122c3-159">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="122c3-160">W połączeniu z `asp-prerender-module` pomocnika tagów `asp-prerender-data` pomocnika tagów może służyć do przekazywania informacji kontekstowych z widoku Razor do JavaScript po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="122c3-160">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="122c3-161">Na przykład następujący kod znaczników przekazuje dane użytkownika do `main-server` modułu:</span><span class="sxs-lookup"><span data-stu-id="122c3-161">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="122c3-162">Odebranej `UserName` argument jest zserializowanym przy użyciu wbudowanych serializator JSON i są przechowywane w `params.data` obiektu.</span><span class="sxs-lookup"><span data-stu-id="122c3-162">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="122c3-163">W poniższym przykładzie kątowego danych jest używany do utworzenia spersonalizowanego pozdrowienia w `h1` elementu:</span><span class="sxs-lookup"><span data-stu-id="122c3-163">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="122c3-164">Uwaga: Nazwy właściwości przekazano pomocników tagów są reprezentowane z **PascalCase** notacji.</span><span class="sxs-lookup"><span data-stu-id="122c3-164">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="122c3-165">Natomiast który do JavaScript, w których są reprezentowane pod tą samą nazwą właściwości z **(camelcase)**.</span><span class="sxs-lookup"><span data-stu-id="122c3-165">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="122c3-166">Domyślna konfiguracja serializacji JSON jest odpowiedzialny za tej różnicy.</span><span class="sxs-lookup"><span data-stu-id="122c3-166">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="122c3-167">Aby rozszerzyć na poprzednim przykładzie kodu, dane mogą być przekazywane z serwera do widoku przez nawilżania `globals` przekazane do właściwości `resolve` funkcji:</span><span class="sxs-lookup"><span data-stu-id="122c3-167">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="122c3-168">`postList` Zdefiniowany w tablicy `globals` obiekt jest dołączony do przeglądarki globalne `window` obiektu.</span><span class="sxs-lookup"><span data-stu-id="122c3-168">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="122c3-169">Tej zmiennej podnoszenia do globalnego zakresu eliminuje dublowania działań, w szczególności, w odniesieniu do ładowania danych tego samego raz na serwerze i ponownie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="122c3-169">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![postList — zmienna globalna dołączony do obiektu okna](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="122c3-171">Oprogramowanie pośredniczące deweloperów Webpack</span><span class="sxs-lookup"><span data-stu-id="122c3-171">Webpack Dev Middleware</span></span>

<span data-ttu-id="122c3-172">[Oprogramowanie pośredniczące deweloperów Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) wprowadza usprawnić programowanie przepływu pracy zgodnie z którymi Webpack kompilacje zasobów na żądanie.</span><span class="sxs-lookup"><span data-stu-id="122c3-172">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="122c3-173">Oprogramowanie pośredniczące automatycznie kompiluje i służy zasobów po stronie klienta, gdy strona zostanie ponownie załadowana w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="122c3-173">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="122c3-174">Alternatywne podejście jest ręczne wywoływanie Webpack za pomocą skryptu kompilacji npm projektu po zmianie zależności innych firm lub niestandardowy kod.</span><span class="sxs-lookup"><span data-stu-id="122c3-174">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="122c3-175">Npm kompilacji skryptu *package.json* pliku przedstawiono w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="122c3-175">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="122c3-176">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="122c3-176">Prerequisites</span></span>

<span data-ttu-id="122c3-177">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="122c3-177">Install the following:</span></span>
* <span data-ttu-id="122c3-178">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="122c3-178">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="122c3-179">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="122c3-179">Configuration</span></span>

<span data-ttu-id="122c3-180">Oprogramowanie pośredniczące deweloperów Webpack został zarejestrowany z potokiem żądań HTTP za pośrednictwem następujący kod w *Startup.cs* pliku `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="122c3-180">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="122c3-181">`UseWebpackDevMiddleware` — Metoda rozszerzenia musi zostać wywołana przed [rejestrowanie obsługi plików statycznych](xref:fundamentals/static-files) za pośrednictwem `UseStaticFiles` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="122c3-181">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="122c3-182">Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="122c3-182">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="122c3-183">*Webpack.config.js* pliku `output.publicPath` właściwości informuje oprogramowanie pośredniczące, które należy obserwować `dist` folderu zmiany:</span><span class="sxs-lookup"><span data-stu-id="122c3-183">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="122c3-184">Moduł dynamicznej wymiany</span><span class="sxs-lookup"><span data-stu-id="122c3-184">Hot Module Replacement</span></span>

<span data-ttu-id="122c3-185">Pomyśl o jego Webpack [dynamicznej wymiany modułu](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) funkcji (HMR) jako rozwoju [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="122c3-185">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="122c3-186">HMR wprowadzono takich samych korzyści, ale dodatkowo upraszcza przepływu pracy programowania przez automatyczne aktualizowanie zawartości strony po kompilowanie zmiany.</span><span class="sxs-lookup"><span data-stu-id="122c3-186">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="122c3-187">Nie pomyl go z odświeżenia przeglądarki, która będzie zakłócać bieżący stan w pamięci i sesji debugowania aplikacja JEDNOSTRONICOWA.</span><span class="sxs-lookup"><span data-stu-id="122c3-187">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="122c3-188">Istnieje aktywne łącze między usługą oprogramowanie pośredniczące deweloperów Webpack i przeglądarki, co oznacza, że zmiany są przenoszone do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="122c3-188">There is a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="122c3-189">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="122c3-189">Prerequisites</span></span>

<span data-ttu-id="122c3-190">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="122c3-190">Install the following:</span></span>
* <span data-ttu-id="122c3-191">[webpack-hot — oprogramowanie pośredniczące](https://www.npmjs.com/package/webpack-hot-middleware) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="122c3-191">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="122c3-192">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="122c3-192">Configuration</span></span>

<span data-ttu-id="122c3-193">Składnik HMR muszą być rejestrowane w MVC Potok żądań HTTP w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="122c3-193">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="122c3-194">Tak jak została wartość true z [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` — metoda rozszerzenia musi zostać wywołana przed `UseStaticFiles` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="122c3-194">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="122c3-195">Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="122c3-195">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="122c3-196">*Webpack.config.js* muszą być zdefiniowane w pliku `plugins` tablicy nawet wtedy, gdy zostanie pozostawiony pusty:</span><span class="sxs-lookup"><span data-stu-id="122c3-196">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="122c3-197">Po załadowaniu aplikacji w przeglądarce, karta konsoli narzędzi deweloperskich zawiera potwierdzenie HMR aktywacji:</span><span class="sxs-lookup"><span data-stu-id="122c3-197">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Gorących komunikat połączonych zastąpienie modułu](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="122c3-199">Pomocnicy routingu</span><span class="sxs-lookup"><span data-stu-id="122c3-199">Routing helpers</span></span>

<span data-ttu-id="122c3-200">W większości na podstawie platformy ASP.NET Core źródła należy po stronie klienta routingu oprócz routingu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="122c3-200">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="122c3-201">Systemy routingu SPA i MVC może pracować wielel osób bez zakłóceń.</span><span class="sxs-lookup"><span data-stu-id="122c3-201">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="122c3-202">Jest jednak jeden przypadek krawędzi stwarza problemy: Identyfikowanie odpowiedzi HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="122c3-202">There is, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="122c3-203">Rozważmy scenariusz, w którym bez rozszerzenia trasa `/some/page` jest używany.</span><span class="sxs-lookup"><span data-stu-id="122c3-203">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="122c3-204">Załóżmy, żądanie nie-dopasowania wzorca trasy po stronie serwera, ale jego wzorzec pasuje do trasy po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="122c3-204">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="122c3-205">Teraz Rozważmy przychodzącego żądania dla `/images/user-512.png`, która oczekuje zwykle można znaleźć pliku obrazu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="122c3-205">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="122c3-206">Jeśli trasie po stronie serwera lub pliku statycznego że ścieżka do żądanego zasobu nie jest zgodny, jest mało prawdopodobne, czy aplikacja kliencka będzie jego obsługa — zwykle chcesz zwracać kod stanu HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="122c3-206">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="122c3-207">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="122c3-207">Prerequisites</span></span>

<span data-ttu-id="122c3-208">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="122c3-208">Install the following:</span></span>
* <span data-ttu-id="122c3-209">Po stronie klienta routingu pakiet npm.</span><span class="sxs-lookup"><span data-stu-id="122c3-209">The client-side routing npm package.</span></span> <span data-ttu-id="122c3-210">Na przykład za pomocą kątową:</span><span class="sxs-lookup"><span data-stu-id="122c3-210">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="122c3-211">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="122c3-211">Configuration</span></span>

<span data-ttu-id="122c3-212">Metody rozszerzenia o nazwie `MapSpaFallbackRoute` jest używany w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="122c3-212">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="122c3-213">Porada: Trasy są oceniane w kolejności, w którym jest skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="122c3-213">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="122c3-214">W rezultacie `default` trasy w poprzednim przykładzie kodu jest używana jako pierwsza dopasowywanie do wzorców.</span><span class="sxs-lookup"><span data-stu-id="122c3-214">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="122c3-215">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="122c3-215">Creating a new project</span></span>

<span data-ttu-id="122c3-216">JavaScriptServices zawiera szablony wstępnie skonfigurowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="122c3-216">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="122c3-217">SpaServices jest używany w tych szablonów w połączeniu z różnych struktury i biblioteki, takich jak kątową, Aurelia odcinania, platformy React i Vue.</span><span class="sxs-lookup"><span data-stu-id="122c3-217">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="122c3-218">Szablony te można zainstalować za pomocą interfejsu wiersza polecenia platformy .NET Core, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="122c3-218">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="122c3-219">Zostanie wyświetlona lista dostępnych szablonów SPA:</span><span class="sxs-lookup"><span data-stu-id="122c3-219">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="122c3-220">Szablony</span><span class="sxs-lookup"><span data-stu-id="122c3-220">Templates</span></span>                                 | <span data-ttu-id="122c3-221">Krótka nazwa</span><span class="sxs-lookup"><span data-stu-id="122c3-221">Short Name</span></span> | <span data-ttu-id="122c3-222">Język</span><span class="sxs-lookup"><span data-stu-id="122c3-222">Language</span></span> | <span data-ttu-id="122c3-223">Znaczniki</span><span class="sxs-lookup"><span data-stu-id="122c3-223">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="122c3-224">MVC ASP.NET Core z dyrektywy Angular</span><span class="sxs-lookup"><span data-stu-id="122c3-224">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="122c3-225">dyrektywy angular</span><span class="sxs-lookup"><span data-stu-id="122c3-225">angular</span></span>    | <span data-ttu-id="122c3-226">[C#]</span><span class="sxs-lookup"><span data-stu-id="122c3-226">[C#]</span></span>     | <span data-ttu-id="122c3-227">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="122c3-227">Web/MVC/SPA</span></span> |
| <span data-ttu-id="122c3-228">MVC ASP.NET Core z Aurelia</span><span class="sxs-lookup"><span data-stu-id="122c3-228">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="122c3-229">aurelia</span><span class="sxs-lookup"><span data-stu-id="122c3-229">aurelia</span></span>    | <span data-ttu-id="122c3-230">[C#]</span><span class="sxs-lookup"><span data-stu-id="122c3-230">[C#]</span></span>     | <span data-ttu-id="122c3-231">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="122c3-231">Web/MVC/SPA</span></span> |
| <span data-ttu-id="122c3-232">MVC ASP.NET Core z Knockout.js</span><span class="sxs-lookup"><span data-stu-id="122c3-232">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="122c3-233">Odcinania</span><span class="sxs-lookup"><span data-stu-id="122c3-233">knockout</span></span>   | <span data-ttu-id="122c3-234">[C#]</span><span class="sxs-lookup"><span data-stu-id="122c3-234">[C#]</span></span>     | <span data-ttu-id="122c3-235">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="122c3-235">Web/MVC/SPA</span></span> |
| <span data-ttu-id="122c3-236">MVC ASP.NET Core z React.js</span><span class="sxs-lookup"><span data-stu-id="122c3-236">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="122c3-237">Reakcji</span><span class="sxs-lookup"><span data-stu-id="122c3-237">react</span></span>      | <span data-ttu-id="122c3-238">[C#]</span><span class="sxs-lookup"><span data-stu-id="122c3-238">[C#]</span></span>     | <span data-ttu-id="122c3-239">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="122c3-239">Web/MVC/SPA</span></span> |
| <span data-ttu-id="122c3-240">MVC ASP.NET Core z React.js i Redux</span><span class="sxs-lookup"><span data-stu-id="122c3-240">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="122c3-241">reactredux</span><span class="sxs-lookup"><span data-stu-id="122c3-241">reactredux</span></span> | <span data-ttu-id="122c3-242">[C#]</span><span class="sxs-lookup"><span data-stu-id="122c3-242">[C#]</span></span>     | <span data-ttu-id="122c3-243">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="122c3-243">Web/MVC/SPA</span></span> |
| <span data-ttu-id="122c3-244">MVC ASP.NET Core z Vue.js</span><span class="sxs-lookup"><span data-stu-id="122c3-244">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="122c3-245">VUE</span><span class="sxs-lookup"><span data-stu-id="122c3-245">vue</span></span>        | <span data-ttu-id="122c3-246">[C#]</span><span class="sxs-lookup"><span data-stu-id="122c3-246">[C#]</span></span>     | <span data-ttu-id="122c3-247">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="122c3-247">Web/MVC/SPA</span></span> | 

<span data-ttu-id="122c3-248">Aby utworzyć nowy projekt za pomocą jednego z szablonów SPA, dołącz **krótką nazwę** szablonu w `dotnet new` polecenia.</span><span class="sxs-lookup"><span data-stu-id="122c3-248">To create a new project using one of the SPA templates, include the **Short Name** of the template in the `dotnet new` command.</span></span> <span data-ttu-id="122c3-249">Poniższe polecenie tworzy aplikację kątowego z platformą ASP.NET MVC Core skonfigurowany po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="122c3-249">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="122c3-250">Ustaw tryb konfiguracji środowiska wykonawczego</span><span class="sxs-lookup"><span data-stu-id="122c3-250">Set the runtime configuration mode</span></span>

<span data-ttu-id="122c3-251">Istnieją dwa tryby konfiguracji podstawowego środowiska wykonawczego:</span><span class="sxs-lookup"><span data-stu-id="122c3-251">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="122c3-252">**Programowanie**:</span><span class="sxs-lookup"><span data-stu-id="122c3-252">**Development**:</span></span>
    * <span data-ttu-id="122c3-253">Obejmuje map źródła do jej obsługi ułatwiają debugowania.</span><span class="sxs-lookup"><span data-stu-id="122c3-253">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="122c3-254">Nie Optymalizuj kod wydajności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="122c3-254">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="122c3-255">**Produkcji**:</span><span class="sxs-lookup"><span data-stu-id="122c3-255">**Production**:</span></span>
    * <span data-ttu-id="122c3-256">Wyklucza mapy źródła.</span><span class="sxs-lookup"><span data-stu-id="122c3-256">Excludes source maps.</span></span>
    * <span data-ttu-id="122c3-257">Optymalizuje kod po stronie klienta za pośrednictwem tworzenie pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="122c3-257">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="122c3-258">Zmienna środowiskowa o nazwie korzysta z platformy ASP.NET Core `ASPNETCORE_ENVIRONMENT` do przechowywania trybu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="122c3-258">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="122c3-259">Zobacz  **[konfiguracji środowiska](xref:fundamentals/environments#setting-the-environment)**  Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="122c3-259">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="122c3-260">Uruchomione z platformą .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="122c3-260">Running with .NET Core CLI</span></span>

<span data-ttu-id="122c3-261">Przywróć wymagane NuGet i pakietów npm, uruchamiając następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="122c3-261">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="122c3-262">Tworzenie i uruchamianie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="122c3-262">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="122c3-263">Na hoście lokalnym zgodnie z uruchomieniem aplikacji [trybu konfiguracji środowiska uruchomieniowego](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="122c3-263">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="122c3-264">Nawigowanie do `http://localhost:5000` w przeglądarce zostanie wyświetlona strona docelowa.</span><span class="sxs-lookup"><span data-stu-id="122c3-264">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="122c3-265">Uruchomiony z programu Visual Studio 2017 r.</span><span class="sxs-lookup"><span data-stu-id="122c3-265">Running with Visual Studio 2017</span></span>

<span data-ttu-id="122c3-266">Otwórz *.csproj* plik utworzony przez `dotnet new` polecenia.</span><span class="sxs-lookup"><span data-stu-id="122c3-266">Open the *.csproj* file generated by the `dotnet new` command.</span></span> <span data-ttu-id="122c3-267">Wymagane pakiety NuGet i npm zostaną przywrócone automatycznie po otwarciu projektu.</span><span class="sxs-lookup"><span data-stu-id="122c3-267">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="122c3-268">Ten proces przywracania może potrwać kilka minut, a aplikacja jest gotowa do uruchomienia po jego ukończeniu.</span><span class="sxs-lookup"><span data-stu-id="122c3-268">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="122c3-269">Kliknij zielony przycisk uruchamiania lub naciśnij klawisz `Ctrl + F5`, i przeglądarka otworzy się Strona początkowa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="122c3-269">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="122c3-270">Aplikacja zostanie uruchomiona na hoście lokalnym zgodnie z [trybu konfiguracji środowiska uruchomieniowego](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="122c3-270">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="122c3-271">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="122c3-271">Testing the app</span></span>

<span data-ttu-id="122c3-272">Jest wstępnie skonfigurowana do uruchamiania testów po stronie klienta przy użyciu szablonów SpaServices [Karma](https://karma-runner.github.io/1.0/index.html) i [jaśmin](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="122c3-272">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="122c3-273">Jaśmin jest jednostka popularnych testowania framework dla języka JavaScript, Karma jest test runner do tych testów.</span><span class="sxs-lookup"><span data-stu-id="122c3-273">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="122c3-274">Karma jest skonfigurowana do pracy z [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware) tak, aby nie trzeba zatrzymać i uruchomić test za każdym razem, gdy zostaną wprowadzone zmiany.</span><span class="sxs-lookup"><span data-stu-id="122c3-274">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that you don’t have to stop and run the test every time changes are made.</span></span> <span data-ttu-id="122c3-275">Czy jest ono kodu uruchamianego przypadek testowy lub samego przypadek testowy, test jest uruchamiany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="122c3-275">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="122c3-276">Korzystanie z aplikacji kątowego jako przykład dwóch przypadków testowych jaśmin są już udostępniane dla `CounterComponent` w *counter.component.spec.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="122c3-276">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="122c3-277">Otwórz wiersz polecenia w katalogu głównym projektu i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="122c3-277">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="122c3-278">Skrypt uruchamia uruchamiający Karma, który brzmi ustawień zdefiniowanych w *karma.conf.js* pliku.</span><span class="sxs-lookup"><span data-stu-id="122c3-278">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="122c3-279">Wśród innych ustawień *karma.conf.js* identyfikuje pliki testowe, aby być wykonywane przy użyciu jego `files` tablicy:</span><span class="sxs-lookup"><span data-stu-id="122c3-279">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="122c3-280">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="122c3-280">Publishing the application</span></span>

<span data-ttu-id="122c3-281">Łączenie wygenerowanego zasoby po stronie klienta i opublikowanych artefakty platformy ASP.NET Core w gotowe do wdrożenia pakietu może być skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="122c3-281">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="122c3-282">Thankfully, SpaServices organizuje procesu całej publikacji o niestandardowych docelowy programu MSBuild o nazwie `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="122c3-282">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="122c3-283">Element docelowy programu MSBuild ma następujące obowiązki:</span><span class="sxs-lookup"><span data-stu-id="122c3-283">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="122c3-284">Przywracanie pakietów npm</span><span class="sxs-lookup"><span data-stu-id="122c3-284">Restore the npm packages</span></span>
1. <span data-ttu-id="122c3-285">Utwórz kompilację produkcji klasy zasobów innych firm, po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="122c3-285">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="122c3-286">Utwórz kompilację klasy produkcji niestandardowych zasobów po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="122c3-286">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="122c3-287">Skopiuj wygenerowany Webpack zasoby do folderu publikowania</span><span class="sxs-lookup"><span data-stu-id="122c3-287">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="122c3-288">Element docelowy programu MSBuild jest wywoływane, gdy uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="122c3-288">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="122c3-289">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="122c3-289">Additional resources</span></span>

* [<span data-ttu-id="122c3-290">Dokumentacja dyrektywy angular</span><span class="sxs-lookup"><span data-stu-id="122c3-290">Angular Docs</span></span>](https://angular.io/docs)