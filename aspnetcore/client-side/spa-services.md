---
title: "Przy użyciu JavaScriptServices do tworzenia aplikacji jednej strony"
author: scottaddie
description: "Poznaj korzyści wynikające ze stosowania JavaScriptServices do utworzenia jednej strony aplikacji JEDNOSTRONICOWEJ obsługiwana przez platformy ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: c617f1a563b0eeccea0ab313bba8b90a4c947e28
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a><span data-ttu-id="38cdc-103">Do tworzenia aplikacji jednej strony z platformy ASP.NET Core za pomocą JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="38cdc-103">Using JavaScriptServices for Creating Single Page Applications with ASP.NET Core</span></span>

<span data-ttu-id="38cdc-104">Przez [Scott Addie](https://github.com/scottaddie) i [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="38cdc-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="38cdc-105">Jednej strony aplikacji (SPA) jest typem popularnych aplikacji sieci web z powodu jego związanego z używaniem zaawansowanego środowiska użytkownika.</span><span class="sxs-lookup"><span data-stu-id="38cdc-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="38cdc-106">Integrowanie struktur SPA po stronie klienta lub biblioteki, takich jak [kątową](https://angular.io/) lub [React](https://facebook.github.io/react/), z platform po stronie serwera, takich jak ASP.NET Core mogą być trudne.</span><span class="sxs-lookup"><span data-stu-id="38cdc-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="38cdc-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) został opracowany, aby zmniejszyć tarcia w procesie integracji.</span><span class="sxs-lookup"><span data-stu-id="38cdc-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="38cdc-108">Umożliwia bezproblemowe operacji między innego klienta i serwera technologii stosów.</span><span class="sxs-lookup"><span data-stu-id="38cdc-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="38cdc-109">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="38cdc-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="38cdc-110">Co to jest JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="38cdc-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="38cdc-111">JavaScriptServices jest kolekcja technologii po stronie klienta dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="38cdc-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="38cdc-112">Jego celem jest pozwala platformy ASP.NET Core jako deweloperów preferowaną platformę po stronie serwera do tworzenia źródła.</span><span class="sxs-lookup"><span data-stu-id="38cdc-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="38cdc-113">JavaScriptServices składa się z trzech różnych pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="38cdc-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="38cdc-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="38cdc-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="38cdc-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="38cdc-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="38cdc-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="38cdc-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="38cdc-117">Te pakiety są przydatne w przypadku możesz:</span><span class="sxs-lookup"><span data-stu-id="38cdc-117">These packages are useful if you:</span></span>
* <span data-ttu-id="38cdc-118">Uruchom JavaScript na serwerze</span><span class="sxs-lookup"><span data-stu-id="38cdc-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="38cdc-119">Użyj SPA framework lub biblioteki</span><span class="sxs-lookup"><span data-stu-id="38cdc-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="38cdc-120">Tworzenie zasobów po stronie klienta z Webpack</span><span class="sxs-lookup"><span data-stu-id="38cdc-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="38cdc-121">Większość fokus w tym artykule jest umieszczona na przy użyciu pakietu SpaServices.</span><span class="sxs-lookup"><span data-stu-id="38cdc-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="38cdc-122">Co to jest SpaServices?</span><span class="sxs-lookup"><span data-stu-id="38cdc-122">What is SpaServices?</span></span>

<span data-ttu-id="38cdc-123">SpaServices utworzono pozwala platformy ASP.NET Core jako deweloperów preferowaną platformę po stronie serwera do tworzenia źródła.</span><span class="sxs-lookup"><span data-stu-id="38cdc-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="38cdc-124">SpaServices nie jest wymagane, aby opracować źródła z platformy ASP.NET Core, a nie blokuje w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="38cdc-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="38cdc-125">SpaServices zapewnia przydatne infrastruktury, takich jak:</span><span class="sxs-lookup"><span data-stu-id="38cdc-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="38cdc-126">Prerendering po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="38cdc-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="38cdc-127">Oprogramowanie pośredniczące deweloperów Webpack</span><span class="sxs-lookup"><span data-stu-id="38cdc-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="38cdc-128">Moduł dynamicznej wymiany</span><span class="sxs-lookup"><span data-stu-id="38cdc-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="38cdc-129">Pomocnicy routingu</span><span class="sxs-lookup"><span data-stu-id="38cdc-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="38cdc-130">Zbiorczo te składniki infrastruktury zwiększenia zarówno przepływu pracy programowania i obsługi środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="38cdc-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="38cdc-131">Składniki może przyjąć pojedynczo.</span><span class="sxs-lookup"><span data-stu-id="38cdc-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="38cdc-132">Wymagania wstępne dotyczące korzystania z SpaServices</span><span class="sxs-lookup"><span data-stu-id="38cdc-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="38cdc-133">Aby pracować z SpaServices, należy zainstalować następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="38cdc-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="38cdc-134">[Node.js](https://nodejs.org/) (w wersji 6 lub nowszej) z pakietu npm</span><span class="sxs-lookup"><span data-stu-id="38cdc-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="38cdc-135">Aby sprawdzić te składniki są zainstalowane i można go znaleźć, należy uruchomić następujące polecenie z wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="38cdc-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="38cdc-136">Uwaga: Jeśli wdrażasz do witryny sieci web platformy Azure, nie musisz tutaj wykonywać żadnych czynności &mdash; Node.js jest zainstalowany i dostępny w środowisku serwerów.</span><span class="sxs-lookup"><span data-stu-id="38cdc-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="38cdc-137">[Zestaw SDK programu .NET core](https://www.microsoft.com/net/download/core) 1.0 (lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="38cdc-137">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="38cdc-138">Jeśli w systemie Windows, to można zainstalować, wybierając w Visual Studio 2017 **aplikacji dla wielu platform .NET Core** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="38cdc-138">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="38cdc-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span><span class="sxs-lookup"><span data-stu-id="38cdc-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="38cdc-140">Prerendering po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="38cdc-140">Server-side prerendering</span></span>

<span data-ttu-id="38cdc-141">Uniwersalnej aplikacji (nazywane również isomorphic) to aplikacja JavaScript może działać zarówno na serwerze i kliencie.</span><span class="sxs-lookup"><span data-stu-id="38cdc-141">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="38cdc-142">Kątową platformy React i innych popularnych struktur podanie tego stylu rozwoju aplikacji platformy uniwersalnej.</span><span class="sxs-lookup"><span data-stu-id="38cdc-142">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="38cdc-143">Koncepcja jest najpierw przetworzyć składników framework na serwerze za pośrednictwem środowiska Node.js, a następnie poprzez delegowanie przekazać dalej wykonywania do klienta.</span><span class="sxs-lookup"><span data-stu-id="38cdc-143">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="38cdc-144">Platformy ASP.NET Core [pomocników tagów](xref:mvc/views/tag-helpers/intro) dostarczonych przez SpaServices upraszczająca implementację elementów prerendering po stronie serwera za pomocą funkcji JavaScript na serwerze.</span><span class="sxs-lookup"><span data-stu-id="38cdc-144">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="38cdc-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="38cdc-145">Prerequisites</span></span>

<span data-ttu-id="38cdc-146">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="38cdc-146">Install the following:</span></span>
* <span data-ttu-id="38cdc-147">[ASPNET prerendering](https://www.npmjs.com/package/aspnet-prerendering) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="38cdc-147">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="38cdc-148">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="38cdc-148">Configuration</span></span>

<span data-ttu-id="38cdc-149">Pomocników tagów zostają wykrywalny przez rejestracji przestrzeni nazw w projekcie *_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="38cdc-149">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="38cdc-150">Te pomocników tagów optymalizacji abstrakcyjnej mogli dokładnie zapoznać się z komunikują się bezpośrednio z interfejsów API niskiego poziomu przy użyciu składni notacji języka HTML w widoku Razor:</span><span class="sxs-lookup"><span data-stu-id="38cdc-150">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="38cdc-151">`asp-prerender-module` Pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="38cdc-151">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="38cdc-152">`asp-prerender-module` Wykonuje pomocnika tagów, używany w poprzednim przykładzie kodu *ClientApp/dist/main-server.js* na serwerze za pośrednictwem środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="38cdc-152">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="38cdc-153">Dla jasności *main server.js* plik jest artefaktu zadania transpilation TypeScript i JavaScript w [Webpack](http://webpack.github.io/) proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="38cdc-153">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="38cdc-154">Webpack definiuje alias punktu wejścia `main-server`; i przechodzenie na wykresie zależności dla tego aliasu zaczyna się od *ClientApp/rozruchu server.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="38cdc-154">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="38cdc-155">W poniższym przykładzie kątowego *ClientApp/rozruchu server.ts* korzysta z pliku `createServerRenderer` funkcji i `RenderResult` typu `aspnet-prerendering` pakiet npm, aby skonfigurować renderowania serwera za pomocą środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="38cdc-155">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="38cdc-156">Kod znaczników HTML, przeznaczonych do renderowania po stronie serwera jest przekazywana do wywołanie funkcji rozpoznawania jest ujęte w języku JavaScript jednoznacznie `Promise` obiektu.</span><span class="sxs-lookup"><span data-stu-id="38cdc-156">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="38cdc-157">`Promise` Znaczenie obiektu to, że asynchronicznie dostarcza kod znaczników HTML strony iniekcji w modelu DOM — symbol zastępczy elementu.</span><span class="sxs-lookup"><span data-stu-id="38cdc-157">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="38cdc-158">`asp-prerender-data` Pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="38cdc-158">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="38cdc-159">W połączeniu z `asp-prerender-module` pomocnika tagów `asp-prerender-data` pomocnika tagów może służyć do przekazywania informacji kontekstowych z widoku Razor do JavaScript po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="38cdc-159">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="38cdc-160">Na przykład następujący kod znaczników przekazuje dane użytkownika do `main-server` modułu:</span><span class="sxs-lookup"><span data-stu-id="38cdc-160">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="38cdc-161">Odebranej `UserName` argument jest zserializowanym przy użyciu wbudowanych serializator JSON i są przechowywane w `params.data` obiektu.</span><span class="sxs-lookup"><span data-stu-id="38cdc-161">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="38cdc-162">W poniższym przykładzie kątowego danych jest używany do utworzenia spersonalizowanego pozdrowienia w `h1` elementu:</span><span class="sxs-lookup"><span data-stu-id="38cdc-162">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="38cdc-163">Uwaga: Nazwy właściwości przekazano pomocników tagów są reprezentowane z **PascalCase** notacji.</span><span class="sxs-lookup"><span data-stu-id="38cdc-163">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="38cdc-164">Natomiast który do JavaScript, w których są reprezentowane pod tą samą nazwą właściwości z **(camelcase)**.</span><span class="sxs-lookup"><span data-stu-id="38cdc-164">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="38cdc-165">Domyślna konfiguracja serializacji JSON jest odpowiedzialny za tej różnicy.</span><span class="sxs-lookup"><span data-stu-id="38cdc-165">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="38cdc-166">Aby rozszerzyć na poprzednim przykładzie kodu, dane mogą być przekazywane z serwera do widoku przez nawilżania `globals` przekazane do właściwości `resolve` funkcji:</span><span class="sxs-lookup"><span data-stu-id="38cdc-166">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="38cdc-167">`postList` Zdefiniowany w tablicy `globals` obiekt jest dołączony do przeglądarki globalne `window` obiektu.</span><span class="sxs-lookup"><span data-stu-id="38cdc-167">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="38cdc-168">Tej zmiennej podnoszenia do globalnego zakresu eliminuje dublowania działań, w szczególności, w odniesieniu do ładowania danych tego samego raz na serwerze i ponownie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="38cdc-168">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![postList — zmienna globalna dołączony do obiektu okna](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="38cdc-170">Oprogramowanie pośredniczące deweloperów Webpack</span><span class="sxs-lookup"><span data-stu-id="38cdc-170">Webpack Dev Middleware</span></span>

<span data-ttu-id="38cdc-171">[Oprogramowanie pośredniczące deweloperów Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) wprowadza usprawnić programowanie przepływu pracy zgodnie z którymi Webpack kompilacje zasobów na żądanie.</span><span class="sxs-lookup"><span data-stu-id="38cdc-171">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="38cdc-172">Oprogramowanie pośredniczące automatycznie kompiluje i służy zasobów po stronie klienta, gdy strona zostanie ponownie załadowana w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="38cdc-172">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="38cdc-173">Alternatywne podejście jest ręczne wywoływanie Webpack za pomocą skryptu kompilacji npm projektu po zmianie zależności innych firm lub niestandardowy kod.</span><span class="sxs-lookup"><span data-stu-id="38cdc-173">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="38cdc-174">Npm kompilacji skryptu *package.json* pliku przedstawiono w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="38cdc-174">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="38cdc-175">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="38cdc-175">Prerequisites</span></span>

<span data-ttu-id="38cdc-176">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="38cdc-176">Install the following:</span></span>
* <span data-ttu-id="38cdc-177">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="38cdc-177">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="38cdc-178">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="38cdc-178">Configuration</span></span>

<span data-ttu-id="38cdc-179">Oprogramowanie pośredniczące deweloperów Webpack został zarejestrowany z potokiem żądań HTTP za pośrednictwem następujący kod w *Startup.cs* pliku `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="38cdc-179">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="38cdc-180">`UseWebpackDevMiddleware` — Metoda rozszerzenia musi zostać wywołana przed [rejestrowanie obsługi plików statycznych](xref:fundamentals/static-files) za pośrednictwem `UseStaticFiles` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="38cdc-180">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="38cdc-181">Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="38cdc-181">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="38cdc-182">*Webpack.config.js* pliku `output.publicPath` właściwości informuje oprogramowanie pośredniczące, które należy obserwować `dist` folderu zmiany:</span><span class="sxs-lookup"><span data-stu-id="38cdc-182">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="38cdc-183">Moduł dynamicznej wymiany</span><span class="sxs-lookup"><span data-stu-id="38cdc-183">Hot Module Replacement</span></span>

<span data-ttu-id="38cdc-184">Pomyśl o jego Webpack [dynamicznej wymiany modułu](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) funkcji (HMR) jako rozwoju [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="38cdc-184">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="38cdc-185">HMR wprowadzono takich samych korzyści, ale dodatkowo upraszcza przepływu pracy programowania przez automatyczne aktualizowanie zawartości strony po kompilowanie zmiany.</span><span class="sxs-lookup"><span data-stu-id="38cdc-185">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="38cdc-186">Nie pomyl go z odświeżenia przeglądarki, która będzie zakłócać bieżący stan w pamięci i sesji debugowania aplikacja JEDNOSTRONICOWA.</span><span class="sxs-lookup"><span data-stu-id="38cdc-186">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="38cdc-187">Istnieje aktywne łącze między usługą oprogramowanie pośredniczące deweloperów Webpack i przeglądarki, co oznacza, że zmiany są przenoszone do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="38cdc-187">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="38cdc-188">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="38cdc-188">Prerequisites</span></span>

<span data-ttu-id="38cdc-189">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="38cdc-189">Install the following:</span></span>
* <span data-ttu-id="38cdc-190">[webpack-hot — oprogramowanie pośredniczące](https://www.npmjs.com/package/webpack-hot-middleware) pakietu npm:</span><span class="sxs-lookup"><span data-stu-id="38cdc-190">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="38cdc-191">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="38cdc-191">Configuration</span></span>

<span data-ttu-id="38cdc-192">Składnik HMR muszą być rejestrowane w MVC Potok żądań HTTP w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="38cdc-192">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="38cdc-193">Tak jak została wartość true z [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` — metoda rozszerzenia musi zostać wywołana przed `UseStaticFiles` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="38cdc-193">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="38cdc-194">Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="38cdc-194">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="38cdc-195">*Webpack.config.js* muszą być zdefiniowane w pliku `plugins` tablicy nawet wtedy, gdy zostanie pozostawiony pusty:</span><span class="sxs-lookup"><span data-stu-id="38cdc-195">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="38cdc-196">Po załadowaniu aplikacji w przeglądarce, karta konsoli narzędzi deweloperskich zawiera potwierdzenie HMR aktywacji:</span><span class="sxs-lookup"><span data-stu-id="38cdc-196">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Gorących komunikat połączonych zastąpienie modułu](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="38cdc-198">Pomocnicy routingu</span><span class="sxs-lookup"><span data-stu-id="38cdc-198">Routing helpers</span></span>

<span data-ttu-id="38cdc-199">W większości na podstawie platformy ASP.NET Core źródła należy po stronie klienta routingu oprócz routingu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="38cdc-199">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="38cdc-200">Systemy routingu SPA i MVC może pracować wielel osób bez zakłóceń.</span><span class="sxs-lookup"><span data-stu-id="38cdc-200">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="38cdc-201">Brak, jednak jeden krawędzi wielkość stwarza żąda: Identyfikowanie odpowiedzi HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="38cdc-201">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="38cdc-202">Rozważmy scenariusz, w którym bez rozszerzenia trasa `/some/page` jest używany.</span><span class="sxs-lookup"><span data-stu-id="38cdc-202">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="38cdc-203">Załóżmy, żądanie nie-dopasowania wzorca trasy po stronie serwera, ale jego wzorzec pasuje do trasy po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="38cdc-203">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="38cdc-204">Teraz Rozważmy przychodzącego żądania dla `/images/user-512.png`, która oczekuje zwykle można znaleźć pliku obrazu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="38cdc-204">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="38cdc-205">Jeśli trasie po stronie serwera lub pliku statycznego że ścieżka do żądanego zasobu nie jest zgodny, jest mało prawdopodobne, czy aplikacja kliencka będzie jego obsługa — zwykle chcesz zwracać kod stanu HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="38cdc-205">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="38cdc-206">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="38cdc-206">Prerequisites</span></span>

<span data-ttu-id="38cdc-207">Zainstaluj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="38cdc-207">Install the following:</span></span>
* <span data-ttu-id="38cdc-208">Po stronie klienta routingu pakiet npm.</span><span class="sxs-lookup"><span data-stu-id="38cdc-208">The client-side routing npm package.</span></span> <span data-ttu-id="38cdc-209">Na przykład za pomocą kątową:</span><span class="sxs-lookup"><span data-stu-id="38cdc-209">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="38cdc-210">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="38cdc-210">Configuration</span></span>

<span data-ttu-id="38cdc-211">Metody rozszerzenia o nazwie `MapSpaFallbackRoute` jest używany w `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="38cdc-211">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="38cdc-212">Porada: Trasy są oceniane w kolejności, w którym jest skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="38cdc-212">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="38cdc-213">W rezultacie `default` trasy w poprzednim przykładzie kodu jest używana jako pierwsza dopasowywanie do wzorców.</span><span class="sxs-lookup"><span data-stu-id="38cdc-213">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="38cdc-214">Tworzenie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="38cdc-214">Creating a new project</span></span>

<span data-ttu-id="38cdc-215">JavaScriptServices zawiera szablony wstępnie skonfigurowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="38cdc-215">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="38cdc-216">SpaServices jest używany w tych szablonów w połączeniu z różnych struktury i biblioteki, takich jak kątową, Aurelia odcinania, platformy React i Vue.</span><span class="sxs-lookup"><span data-stu-id="38cdc-216">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="38cdc-217">Szablony te można zainstalować za pomocą interfejsu wiersza polecenia platformy .NET Core, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="38cdc-217">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="38cdc-218">Zostanie wyświetlona lista dostępnych szablonów SPA:</span><span class="sxs-lookup"><span data-stu-id="38cdc-218">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="38cdc-219">Szablony</span><span class="sxs-lookup"><span data-stu-id="38cdc-219">Templates</span></span>                                 | <span data-ttu-id="38cdc-220">Krótka nazwa</span><span class="sxs-lookup"><span data-stu-id="38cdc-220">Short Name</span></span> | <span data-ttu-id="38cdc-221">Język</span><span class="sxs-lookup"><span data-stu-id="38cdc-221">Language</span></span> | <span data-ttu-id="38cdc-222">Znaczniki</span><span class="sxs-lookup"><span data-stu-id="38cdc-222">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="38cdc-223">MVC ASP.NET Core z dyrektywy Angular</span><span class="sxs-lookup"><span data-stu-id="38cdc-223">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="38cdc-224">dyrektywy angular</span><span class="sxs-lookup"><span data-stu-id="38cdc-224">angular</span></span>    | <span data-ttu-id="38cdc-225">[C#]</span><span class="sxs-lookup"><span data-stu-id="38cdc-225">[C#]</span></span>     | <span data-ttu-id="38cdc-226">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="38cdc-226">Web/MVC/SPA</span></span> |
| <span data-ttu-id="38cdc-227">MVC ASP.NET Core z Aurelia</span><span class="sxs-lookup"><span data-stu-id="38cdc-227">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="38cdc-228">aurelia</span><span class="sxs-lookup"><span data-stu-id="38cdc-228">aurelia</span></span>    | <span data-ttu-id="38cdc-229">[C#]</span><span class="sxs-lookup"><span data-stu-id="38cdc-229">[C#]</span></span>     | <span data-ttu-id="38cdc-230">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="38cdc-230">Web/MVC/SPA</span></span> |
| <span data-ttu-id="38cdc-231">MVC ASP.NET Core z Knockout.js</span><span class="sxs-lookup"><span data-stu-id="38cdc-231">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="38cdc-232">Odcinania</span><span class="sxs-lookup"><span data-stu-id="38cdc-232">knockout</span></span>   | <span data-ttu-id="38cdc-233">[C#]</span><span class="sxs-lookup"><span data-stu-id="38cdc-233">[C#]</span></span>     | <span data-ttu-id="38cdc-234">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="38cdc-234">Web/MVC/SPA</span></span> |
| <span data-ttu-id="38cdc-235">MVC ASP.NET Core z React.js</span><span class="sxs-lookup"><span data-stu-id="38cdc-235">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="38cdc-236">Reakcji</span><span class="sxs-lookup"><span data-stu-id="38cdc-236">react</span></span>      | <span data-ttu-id="38cdc-237">[C#]</span><span class="sxs-lookup"><span data-stu-id="38cdc-237">[C#]</span></span>     | <span data-ttu-id="38cdc-238">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="38cdc-238">Web/MVC/SPA</span></span> |
| <span data-ttu-id="38cdc-239">MVC ASP.NET Core z React.js i Redux</span><span class="sxs-lookup"><span data-stu-id="38cdc-239">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="38cdc-240">reactredux</span><span class="sxs-lookup"><span data-stu-id="38cdc-240">reactredux</span></span> | <span data-ttu-id="38cdc-241">[C#]</span><span class="sxs-lookup"><span data-stu-id="38cdc-241">[C#]</span></span>     | <span data-ttu-id="38cdc-242">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="38cdc-242">Web/MVC/SPA</span></span> |
| <span data-ttu-id="38cdc-243">MVC ASP.NET Core z Vue.js</span><span class="sxs-lookup"><span data-stu-id="38cdc-243">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="38cdc-244">VUE</span><span class="sxs-lookup"><span data-stu-id="38cdc-244">vue</span></span>        | <span data-ttu-id="38cdc-245">[C#]</span><span class="sxs-lookup"><span data-stu-id="38cdc-245">[C#]</span></span>     | <span data-ttu-id="38cdc-246">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="38cdc-246">Web/MVC/SPA</span></span> | 

<span data-ttu-id="38cdc-247">Aby utworzyć nowy projekt za pomocą jednego z szablonów SPA, dołącz **krótką nazwę** szablonu w [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia.</span><span class="sxs-lookup"><span data-stu-id="38cdc-247">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="38cdc-248">Poniższe polecenie tworzy aplikację kątowego z platformą ASP.NET MVC Core skonfigurowany po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="38cdc-248">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="38cdc-249">Ustaw tryb konfiguracji środowiska wykonawczego</span><span class="sxs-lookup"><span data-stu-id="38cdc-249">Set the runtime configuration mode</span></span>

<span data-ttu-id="38cdc-250">Istnieją dwa tryby konfiguracji podstawowego środowiska wykonawczego:</span><span class="sxs-lookup"><span data-stu-id="38cdc-250">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="38cdc-251">**Programowanie**:</span><span class="sxs-lookup"><span data-stu-id="38cdc-251">**Development**:</span></span>
    * <span data-ttu-id="38cdc-252">Obejmuje map źródła do jej obsługi ułatwiają debugowania.</span><span class="sxs-lookup"><span data-stu-id="38cdc-252">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="38cdc-253">Nie Optymalizuj kod wydajności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="38cdc-253">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="38cdc-254">**Produkcji**:</span><span class="sxs-lookup"><span data-stu-id="38cdc-254">**Production**:</span></span>
    * <span data-ttu-id="38cdc-255">Wyklucza mapy źródła.</span><span class="sxs-lookup"><span data-stu-id="38cdc-255">Excludes source maps.</span></span>
    * <span data-ttu-id="38cdc-256">Optymalizuje kod po stronie klienta za pośrednictwem tworzenie pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="38cdc-256">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="38cdc-257">Zmienna środowiskowa o nazwie korzysta z platformy ASP.NET Core `ASPNETCORE_ENVIRONMENT` do przechowywania trybu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="38cdc-257">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="38cdc-258">Zobacz  **[konfiguracji środowiska](xref:fundamentals/environments#setting-the-environment)**  Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="38cdc-258">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="38cdc-259">Uruchomione z platformą .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="38cdc-259">Running with .NET Core CLI</span></span>

<span data-ttu-id="38cdc-260">Przywróć wymagane NuGet i pakietów npm, uruchamiając następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="38cdc-260">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="38cdc-261">Tworzenie i uruchamianie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="38cdc-261">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="38cdc-262">Na hoście lokalnym zgodnie z uruchomieniem aplikacji [trybu konfiguracji środowiska uruchomieniowego](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="38cdc-262">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="38cdc-263">Nawigowanie do `http://localhost:5000` w przeglądarce zostanie wyświetlona strona docelowa.</span><span class="sxs-lookup"><span data-stu-id="38cdc-263">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="38cdc-264">Uruchomiony z programu Visual Studio 2017 r.</span><span class="sxs-lookup"><span data-stu-id="38cdc-264">Running with Visual Studio 2017</span></span>

<span data-ttu-id="38cdc-265">Otwórz *.csproj* plik utworzony przez [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia.</span><span class="sxs-lookup"><span data-stu-id="38cdc-265">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="38cdc-266">Wymagane pakiety NuGet i npm zostaną przywrócone automatycznie po otwarciu projektu.</span><span class="sxs-lookup"><span data-stu-id="38cdc-266">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="38cdc-267">Ten proces przywracania może potrwać kilka minut, a aplikacja jest gotowa do uruchomienia po jego ukończeniu.</span><span class="sxs-lookup"><span data-stu-id="38cdc-267">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="38cdc-268">Kliknij zielony przycisk uruchamiania lub naciśnij klawisz `Ctrl + F5`, i przeglądarka otworzy się Strona początkowa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="38cdc-268">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="38cdc-269">Aplikacja zostanie uruchomiona na hoście lokalnym zgodnie z [trybu konfiguracji środowiska uruchomieniowego](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="38cdc-269">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="38cdc-270">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="38cdc-270">Testing the app</span></span>

<span data-ttu-id="38cdc-271">Jest wstępnie skonfigurowana do uruchamiania testów po stronie klienta przy użyciu szablonów SpaServices [Karma](https://karma-runner.github.io/1.0/index.html) i [jaśmin](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="38cdc-271">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="38cdc-272">Jaśmin jest jednostka popularnych testowania framework dla języka JavaScript, Karma jest test runner do tych testów.</span><span class="sxs-lookup"><span data-stu-id="38cdc-272">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="38cdc-273">Karma jest skonfigurowana do pracy z [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware) tak, aby projektanta nie jest wymagane, aby zatrzymać i uruchomić test za każdym razem, gdy zostaną wprowadzone zmiany.</span><span class="sxs-lookup"><span data-stu-id="38cdc-273">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="38cdc-274">Czy jest ono kodu uruchamianego przypadek testowy lub samego przypadek testowy, test jest uruchamiany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="38cdc-274">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="38cdc-275">Korzystanie z aplikacji kątowego jako przykład dwóch przypadków testowych jaśmin są już udostępniane dla `CounterComponent` w *counter.component.spec.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="38cdc-275">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="38cdc-276">Otwórz wiersz polecenia w katalogu głównym projektu i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="38cdc-276">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="38cdc-277">Skrypt uruchamia uruchamiający Karma, który brzmi ustawień zdefiniowanych w *karma.conf.js* pliku.</span><span class="sxs-lookup"><span data-stu-id="38cdc-277">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="38cdc-278">Wśród innych ustawień *karma.conf.js* identyfikuje pliki testowe, aby być wykonywane przy użyciu jego `files` tablicy:</span><span class="sxs-lookup"><span data-stu-id="38cdc-278">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="38cdc-279">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="38cdc-279">Publishing the application</span></span>

<span data-ttu-id="38cdc-280">Łączenie wygenerowanego zasoby po stronie klienta i opublikowanych artefakty platformy ASP.NET Core w gotowe do wdrożenia pakietu może być skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="38cdc-280">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="38cdc-281">Thankfully, SpaServices organizuje procesu całej publikacji o niestandardowych docelowy programu MSBuild o nazwie `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="38cdc-281">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="38cdc-282">Element docelowy programu MSBuild ma następujące obowiązki:</span><span class="sxs-lookup"><span data-stu-id="38cdc-282">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="38cdc-283">Przywracanie pakietów npm</span><span class="sxs-lookup"><span data-stu-id="38cdc-283">Restore the npm packages</span></span>
1. <span data-ttu-id="38cdc-284">Utwórz kompilację produkcji klasy zasobów innych firm, po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="38cdc-284">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="38cdc-285">Utwórz kompilację klasy produkcji niestandardowych zasobów po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="38cdc-285">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="38cdc-286">Skopiuj wygenerowany Webpack zasoby do folderu publikowania</span><span class="sxs-lookup"><span data-stu-id="38cdc-286">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="38cdc-287">Element docelowy programu MSBuild jest wywoływane, gdy uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="38cdc-287">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="38cdc-288">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="38cdc-288">Additional resources</span></span>

* [<span data-ttu-id="38cdc-289">Dokumentacja dyrektywy angular</span><span class="sxs-lookup"><span data-stu-id="38cdc-289">Angular Docs</span></span>](https://angular.io/docs)
