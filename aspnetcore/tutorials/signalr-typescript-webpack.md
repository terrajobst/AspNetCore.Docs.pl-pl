---
title: Korzystanie z ASP.NET Core SignalR z użyciem języka TypeScript i pakietu WebPack
author: ssougnez
description: W tym samouczku skonfigurujesz pakiet WebPack do tworzenia pakietów i kompilowania ASP.NET Core SignalR aplikacji sieci Web, której klient został zapisany w języku TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7c99c9e79647995886aec5b3a91584fd2f24451
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317486"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a><span data-ttu-id="8b91a-103">Korzystanie z ASP.NET Core SignalR z użyciem języka TypeScript i pakietu WebPack</span><span class="sxs-lookup"><span data-stu-id="8b91a-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="8b91a-104">Autorzy [Sébastien Sougnez](https://twitter.com/ssougnez) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8b91a-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8b91a-105">[Pakiet WebPack](https://webpack.js.org/) umożliwia deweloperom tworzenie i kompilowanie zasobów po stronie klienta aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8b91a-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="8b91a-106">W tym samouczku pokazano, jak używać pakietu WebPack w ASP.NET Core SignalR aplikacji sieci Web, której klient został zapisany w języku [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="8b91a-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="8b91a-107">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="8b91a-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8b91a-108">Tworzenie szkieletu ASP.NET Core aplikacji SignalR Starter</span><span class="sxs-lookup"><span data-stu-id="8b91a-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="8b91a-109">Konfigurowanie SignalR klienta TypeScript</span><span class="sxs-lookup"><span data-stu-id="8b91a-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="8b91a-110">Konfigurowanie potoku kompilacji przy użyciu pakietu WebPack</span><span class="sxs-lookup"><span data-stu-id="8b91a-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="8b91a-111">Skonfiguruj serwer SignalR</span><span class="sxs-lookup"><span data-stu-id="8b91a-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="8b91a-112">Włącz komunikację między klientem a serwerem</span><span class="sxs-lookup"><span data-stu-id="8b91a-112">Enable communication between client and server</span></span>

<span data-ttu-id="8b91a-113">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8b91a-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="8b91a-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="8b91a-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b91a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b91a-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b91a-116">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="8b91a-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8b91a-117">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="8b91a-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8b91a-118">[Node. js](https://nodejs.org/) z [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="8b91a-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b91a-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b91a-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="8b91a-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b91a-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8b91a-121">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="8b91a-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8b91a-122">C#dla Visual Studio Code w wersji 1.17.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="8b91a-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="8b91a-123">[Node. js](https://nodejs.org/) z [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="8b91a-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="8b91a-124">Tworzenie aplikacji sieci Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b91a-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b91a-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b91a-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8b91a-126">Skonfiguruj program Visual Studio, aby szukać npm w zmiennej środowiskowej *Path* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="8b91a-127">Domyślnie program Visual Studio używa wersji npm znajdującej się w katalogu instalacyjnym.</span><span class="sxs-lookup"><span data-stu-id="8b91a-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="8b91a-128">Wykonaj te instrukcje w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8b91a-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="8b91a-129">Przejdź do **opcji narzędzia** > **Opcje** > **projekty i rozwiązania** > **sieci Web zarządzanie pakietami** > **zewnętrznych narzędzi sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="8b91a-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="8b91a-130">Wybierz z listy wpis *$ (Path)* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="8b91a-131">Kliknij strzałkę w górę, aby przenieść wpis do drugiej pozycji na liście.</span><span class="sxs-lookup"><span data-stu-id="8b91a-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Konfiguracja programu Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="8b91a-133">Konfiguracja programu Visual Studio została ukończona.</span><span class="sxs-lookup"><span data-stu-id="8b91a-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="8b91a-134">Czas na utworzenie projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-134">It's time to create the project.</span></span>

1. <span data-ttu-id="8b91a-135">Użyj opcji **plik** > **Nowy** > **projekt** , a następnie wybierz szablon **aplikacja sieci Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="8b91a-136">Nazwij projekt *SignalRWebPack*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="8b91a-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="8b91a-137">Wybierz pozycję *.NET Core* z listy rozwijanej platforma docelowa, a następnie wybierz pozycję *ASP.NET Core 3,0* z listy rozwijanej selektora struktury.</span><span class="sxs-lookup"><span data-stu-id="8b91a-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="8b91a-138">Wybierz **pusty** szablon i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="8b91a-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b91a-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b91a-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8b91a-140">Uruchom następujące polecenie w **zintegrowanym terminalu**:</span><span class="sxs-lookup"><span data-stu-id="8b91a-140">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="8b91a-141">Pusta aplikacja sieci Web ASP.NET Core, ukierunkowana na .NET Core, jest tworzona w katalogu *SignalRWebPack* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="8b91a-142">Konfigurowanie pakietu WebPack i języka TypeScript</span><span class="sxs-lookup"><span data-stu-id="8b91a-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="8b91a-143">Poniższe kroki umożliwiają skonfigurowanie konwersji języka TypeScript na JavaScript i zgrupowanie zasobów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8b91a-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="8b91a-144">Wykonaj następujące polecenie w katalogu głównym projektu, aby utworzyć plik *Package. JSON* :</span><span class="sxs-lookup"><span data-stu-id="8b91a-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="8b91a-145">Dodaj wyróżnioną właściwość do pliku *Package. JSON* :</span><span class="sxs-lookup"><span data-stu-id="8b91a-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="8b91a-146">Ustawienie właściwości `private` na `true` uniemożliwia ostrzeżenia instalacji pakietu w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="8b91a-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="8b91a-147">Zainstaluj wymagane pakiety npm.</span><span class="sxs-lookup"><span data-stu-id="8b91a-147">Install the required npm packages.</span></span> <span data-ttu-id="8b91a-148">Wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="8b91a-149">Niektóre szczegóły polecenia do uwagi:</span><span class="sxs-lookup"><span data-stu-id="8b91a-149">Some command details to note:</span></span>

    * <span data-ttu-id="8b91a-150">Numer wersji jest zgodny ze znakiem `@` dla każdej nazwy pakietu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="8b91a-151">npm instaluje te określone wersje pakietu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="8b91a-152">Opcja `-E` wyłącza domyślne zachowanie npm podczas pisania operatorów zakresu [wersji semantycznej](https://semver.org/) w pliku *Package. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8b91a-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="8b91a-153">Na przykład `"webpack": "4.29.3"` jest używany zamiast `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="8b91a-154">Ta opcja Zapobiega niezamierzonym uaktualnianiu do nowszych wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="8b91a-155">Aby uzyskać więcej informacji, zobacz oficjalne dokumenty z [npm-Install](https://docs.npmjs.com/cli/install) .</span><span class="sxs-lookup"><span data-stu-id="8b91a-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="8b91a-156">Zastąp Właściwość `scripts` pliku *Package. JSON* następującym fragmentem kodu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="8b91a-157">Niektóre objaśnienia dotyczące skryptów:</span><span class="sxs-lookup"><span data-stu-id="8b91a-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="8b91a-158">`build`: pakietuje zasoby po stronie klienta w trybie tworzenia i czujki pod kątem zmian w pliku.</span><span class="sxs-lookup"><span data-stu-id="8b91a-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="8b91a-159">Obserwator plików powoduje, że pakiet jest generowany ponownie za każdym razem, gdy plik projektu jest zmieniany.</span><span class="sxs-lookup"><span data-stu-id="8b91a-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="8b91a-160">Opcja `mode` wyłącza optymalizacje produkcyjne, takie jak wstrząsanie i minifikacjaowanie drzewa.</span><span class="sxs-lookup"><span data-stu-id="8b91a-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="8b91a-161">`build` w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="8b91a-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="8b91a-162">`release`: pakietuje zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="8b91a-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="8b91a-163">`publish`: uruchamia skrypt `release`, aby powiązać zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="8b91a-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="8b91a-164">Wywołuje polecenie [publikowania](/dotnet/core/tools/dotnet-publish) interfejs wiersza polecenia platformy .NET Core, aby opublikować aplikację.</span><span class="sxs-lookup"><span data-stu-id="8b91a-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="8b91a-165">Utwórz plik o nazwie *WebPack. config. js*w katalogu głównym projektu o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="8b91a-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="8b91a-166">Poprzedni plik konfiguruje kompilację pakietu WebPack.</span><span class="sxs-lookup"><span data-stu-id="8b91a-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="8b91a-167">Niektóre szczegóły konfiguracji do uwagi:</span><span class="sxs-lookup"><span data-stu-id="8b91a-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="8b91a-168">Właściwość `output` przesłania domyślną wartość *rozkłu*.</span><span class="sxs-lookup"><span data-stu-id="8b91a-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="8b91a-169">Pakiet jest emitowany w katalogu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="8b91a-170">Tablica `resolve.extensions` zawiera *js* do zaimportowania klienta SignalR JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8b91a-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="8b91a-171">Utwórz nowy katalog *src* w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="8b91a-172">Celem jest przechowywanie zasobów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8b91a-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="8b91a-173">Utwórz *src/index.html* z następującą zawartością.</span><span class="sxs-lookup"><span data-stu-id="8b91a-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="8b91a-174">Powyższy kod HTML definiuje standardowe znaczniki strony głównej.</span><span class="sxs-lookup"><span data-stu-id="8b91a-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="8b91a-175">Utwórz nowy katalog *src/CSS* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="8b91a-176">Celem jest przechowywanie plików *CSS* projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="8b91a-177">Utwórz *src/CSS/Main. css* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="8b91a-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="8b91a-178">Poprzedni plik *Main. css* jest stylem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b91a-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="8b91a-179">Utwórz plik *src/tsconfig. JSON* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="8b91a-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="8b91a-180">Poprzedni kod konfiguruje kompilator języka TypeScript w celu utworzenia kodu JavaScript zgodnego z [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="8b91a-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="8b91a-181">Utwórz *src/index. TS* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="8b91a-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="8b91a-182">Poprzedni kod TypeScript pobiera odwołania do elementów DOM i dołącza dwa programy obsługi zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="8b91a-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="8b91a-183">`keyup`: to zdarzenie jest wyzwalane, gdy użytkownik wpisze coś w polu tekstowym określonym jako `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="8b91a-184">Funkcja `send` jest wywoływana, gdy użytkownik naciśnie klawisz **Enter** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="8b91a-185">`click`: to zdarzenie jest wyzwalane, gdy użytkownik kliknie przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="8b91a-186">Funkcja `send` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8b91a-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="8b91a-187">Konfigurowanie aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b91a-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="8b91a-188">W metodzie `Startup.Configure` Dodaj wywołania do [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="8b91a-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="8b91a-189">Poprzedni kod pozwala serwerowi zlokalizować i obsłużyć plik *index. html* , niezależnie od tego, czy użytkownik wprowadza pełny adres URL, czy główny adres URL aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8b91a-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="8b91a-190">Na końcu metody `Startup.Configure` Mapuj trasę */Hub* na centrum `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="8b91a-191">Zastąp kod, który wyświetla *Hello World!*</span><span class="sxs-lookup"><span data-stu-id="8b91a-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="8b91a-192">z następującym wierszem:</span><span class="sxs-lookup"><span data-stu-id="8b91a-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="8b91a-193">W metodzie `Startup.ConfigureServices` Wywołaj metodę [addsignal](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span><span class="sxs-lookup"><span data-stu-id="8b91a-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="8b91a-194">Dodaje do projektu usługi SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b91a-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="8b91a-195">Utwórz nowy katalog o nazwie *Hubs*w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="8b91a-196">Celem jest przechowywanie centrum SignalR, które jest tworzone w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="8b91a-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="8b91a-197">Utwórz centra centrów */ChatHub. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="8b91a-198">Dodaj następujący kod w górnej części pliku *Startup.cs* , aby rozwiązać `ChatHub` odwołanie:</span><span class="sxs-lookup"><span data-stu-id="8b91a-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="8b91a-199">Włączanie komunikacji klienta i serwera</span><span class="sxs-lookup"><span data-stu-id="8b91a-199">Enable client and server communication</span></span>

<span data-ttu-id="8b91a-200">W aplikacji jest obecnie wyświetlany prosty formularz służący do wysyłania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="8b91a-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="8b91a-201">Nic się nie dzieje, gdy użytkownik spróbuje to zrobić.</span><span class="sxs-lookup"><span data-stu-id="8b91a-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="8b91a-202">Serwer nasłuchuje określonej trasy, ale nic nie robi z wysłanymi komunikatami.</span><span class="sxs-lookup"><span data-stu-id="8b91a-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="8b91a-203">Wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="8b91a-204">Poprzednie polecenie instaluje [SignalR klienta języka TypeScript](https://www.npmjs.com/package/@microsoft/signalr), który umożliwia klientowi wysyłanie komunikatów do serwera programu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="8b91a-205">Dodaj wyróżniony kod do pliku *src/index. TS* :</span><span class="sxs-lookup"><span data-stu-id="8b91a-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="8b91a-206">Poprzedni kod obsługuje otrzymywanie komunikatów z serwera.</span><span class="sxs-lookup"><span data-stu-id="8b91a-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="8b91a-207">Klasa `HubConnectionBuilder` tworzy nowy Konstruktor służący do konfigurowania połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="8b91a-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="8b91a-208">Funkcja `withUrl` konfiguruje adres URL centrum.</span><span class="sxs-lookup"><span data-stu-id="8b91a-208">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="8b91a-209"> umożliwia wymianę komunikatów między klientem a serwerem.</span><span class="sxs-lookup"><span data-stu-id="8b91a-209"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="8b91a-210">Każdy komunikat ma określoną nazwę.</span><span class="sxs-lookup"><span data-stu-id="8b91a-210">Each message has a specific name.</span></span> <span data-ttu-id="8b91a-211">Można na przykład mieć komunikaty o nazwie `messageReceived`, które wykonują logikę odpowiedzialną za wyświetlanie nowej wiadomości w strefie messages.</span><span class="sxs-lookup"><span data-stu-id="8b91a-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="8b91a-212">Nasłuchiwanie określonego komunikatu można wykonać za pomocą funkcji `on`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="8b91a-213">Można nasłuchiwać dowolnej liczby nazw komunikatów.</span><span class="sxs-lookup"><span data-stu-id="8b91a-213">You can listen to any number of message names.</span></span> <span data-ttu-id="8b91a-214">Możliwe jest również przekazywanie parametrów do wiadomości, takich jak nazwa autora i zawartość otrzymanej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="8b91a-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="8b91a-215">Po odebraniu komunikatu przez klienta zostanie utworzony nowy element `div` z nazwą autora i treścią komunikatu w jego atrybucie `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="8b91a-216">Jest on dodawany do głównego elementu `div` wyświetlającego komunikaty.</span><span class="sxs-lookup"><span data-stu-id="8b91a-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="8b91a-217">Teraz, gdy klient może odebrać komunikat, skonfiguruj go do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="8b91a-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="8b91a-218">Dodaj wyróżniony kod do pliku *src/index. TS* :</span><span class="sxs-lookup"><span data-stu-id="8b91a-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="8b91a-219">Wysyłanie komunikatu przez połączenie z usługą WebSockets wymaga wywołania metody `send`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="8b91a-220">Pierwszym parametrem metody jest nazwa komunikatu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="8b91a-221">Dane komunikatu są nieodpowiednie dla innych parametrów.</span><span class="sxs-lookup"><span data-stu-id="8b91a-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="8b91a-222">W tym przykładzie komunikat identyfikowany jako `newMessage` jest wysyłany do serwera.</span><span class="sxs-lookup"><span data-stu-id="8b91a-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="8b91a-223">Komunikat składa się z nazwy użytkownika i danych wejściowych użytkownika z pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="8b91a-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="8b91a-224">Jeśli wysyłanie działa, wartość pola tekstowego jest wyczyszczona.</span><span class="sxs-lookup"><span data-stu-id="8b91a-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="8b91a-225">Dodaj podświetloną metodę do klasy `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="8b91a-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="8b91a-226">Poprzedni kod emituje odebrane komunikaty wszystkim połączonym użytkownikom po ich odebraniu przez serwer.</span><span class="sxs-lookup"><span data-stu-id="8b91a-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="8b91a-227">Nie jest konieczne posiadanie generycznej metody `on`, aby odbierać wszystkie komunikaty.</span><span class="sxs-lookup"><span data-stu-id="8b91a-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="8b91a-228">Metoda o nazwie po wystarczającej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8b91a-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="8b91a-229">W tym przykładzie klient TypeScript wysyła komunikat zidentyfikowany jako `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="8b91a-230">Metoda C# `NewMessage` oczekuje danych wysyłanych przez klienta.</span><span class="sxs-lookup"><span data-stu-id="8b91a-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="8b91a-231">Wykonano wywołanie metody [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) na [klientach. All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="8b91a-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="8b91a-232">Odebrane komunikaty są wysyłane do wszystkich klientów podłączonych do centrum.</span><span class="sxs-lookup"><span data-stu-id="8b91a-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="8b91a-233">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8b91a-233">Test the app</span></span>

<span data-ttu-id="8b91a-234">Upewnij się, że aplikacja działa z następującymi krokami.</span><span class="sxs-lookup"><span data-stu-id="8b91a-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b91a-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b91a-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8b91a-236">Uruchom pakiet WebPack w trybie *wydania* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="8b91a-237">Za pomocą okna **konsoli Menedżera pakietów** wykonaj następujące polecenie w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="8b91a-238">Jeśli nie jesteś w katalogu głównym projektu, wprowadź `cd SignalRWebPack` przed wprowadzeniem polecenia.</span><span class="sxs-lookup"><span data-stu-id="8b91a-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="8b91a-239">Wybierz pozycję **debuguj** > **Rozpocznij bez debugowania** , aby uruchomić aplikację w przeglądarce bez dołączania debugera.</span><span class="sxs-lookup"><span data-stu-id="8b91a-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="8b91a-240">Plik *wwwroot/index.html* jest obsługiwany w `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="8b91a-241">Jeśli zostaną wyświetlone błędy kompilacji, spróbuj zamknąć i ponownie otworzyć rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="8b91a-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="8b91a-242">Otwórz inne wystąpienie przeglądarki (dowolna przeglądarka).</span><span class="sxs-lookup"><span data-stu-id="8b91a-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="8b91a-243">Wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8b91a-244">Wybierz opcję przeglądarka, wpisz coś w polu tekstowym **komunikat** , a następnie kliknij przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="8b91a-245">Unikatowa nazwa użytkownika i komunikat są wyświetlane na obu stronach natychmiastowo.</span><span class="sxs-lookup"><span data-stu-id="8b91a-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b91a-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b91a-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="8b91a-247">Uruchom pakiet WebPack w trybie *wydania* , wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="8b91a-248">Skompiluj i uruchom aplikację, wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-248">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="8b91a-249">Serwer sieci Web uruchamia aplikację i udostępnia ją na hoście lokalnym.</span><span class="sxs-lookup"><span data-stu-id="8b91a-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="8b91a-250">Otwórz przeglądarkę, aby `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="8b91a-251">Plik *wwwroot/index.html* jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="8b91a-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="8b91a-252">Skopiuj adres URL z paska adresu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="8b91a-253">Otwórz inne wystąpienie przeglądarki (dowolna przeglądarka).</span><span class="sxs-lookup"><span data-stu-id="8b91a-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="8b91a-254">Wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8b91a-255">Wybierz opcję przeglądarka, wpisz coś w polu tekstowym **komunikat** , a następnie kliknij przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="8b91a-256">Unikatowa nazwa użytkownika i komunikat są wyświetlane na obu stronach natychmiastowo.</span><span class="sxs-lookup"><span data-stu-id="8b91a-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![komunikat wyświetlany w obu oknach przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b91a-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b91a-258">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b91a-259">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="8b91a-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8b91a-260">Zestaw .NET Core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="8b91a-260">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8b91a-261">[Node. js](https://nodejs.org/) z [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="8b91a-261">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b91a-262">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b91a-262">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="8b91a-263">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b91a-263">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8b91a-264">Zestaw .NET Core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="8b91a-264">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8b91a-265">C#dla Visual Studio Code w wersji 1.17.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="8b91a-265">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="8b91a-266">[Node. js](https://nodejs.org/) z [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="8b91a-266">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="8b91a-267">Tworzenie aplikacji sieci Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b91a-267">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b91a-268">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b91a-268">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8b91a-269">Skonfiguruj program Visual Studio, aby szukać npm w zmiennej środowiskowej *Path* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-269">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="8b91a-270">Domyślnie program Visual Studio używa wersji npm znajdującej się w katalogu instalacyjnym.</span><span class="sxs-lookup"><span data-stu-id="8b91a-270">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="8b91a-271">Wykonaj te instrukcje w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8b91a-271">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="8b91a-272">Przejdź do **opcji narzędzia** > **Opcje** > **projekty i rozwiązania** > **sieci Web zarządzanie pakietami** > **zewnętrznych narzędzi sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="8b91a-272">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="8b91a-273">Wybierz z listy wpis *$ (Path)* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-273">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="8b91a-274">Kliknij strzałkę w górę, aby przenieść wpis do drugiej pozycji na liście.</span><span class="sxs-lookup"><span data-stu-id="8b91a-274">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Konfiguracja programu Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="8b91a-276">Konfiguracja programu Visual Studio została ukończona.</span><span class="sxs-lookup"><span data-stu-id="8b91a-276">Visual Studio configuration is completed.</span></span> <span data-ttu-id="8b91a-277">Czas na utworzenie projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-277">It's time to create the project.</span></span>

1. <span data-ttu-id="8b91a-278">Użyj opcji **plik** > **Nowy** > **projekt** , a następnie wybierz szablon **aplikacja sieci Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-278">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="8b91a-279">Nazwij projekt *SignalRWebPack*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="8b91a-279">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="8b91a-280">Wybierz pozycję *.NET Core* z listy rozwijanej platforma docelowa, a następnie wybierz pozycję *ASP.NET Core 2,2* z listy rozwijanej selektora struktury.</span><span class="sxs-lookup"><span data-stu-id="8b91a-280">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="8b91a-281">Wybierz **pusty** szablon i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="8b91a-281">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b91a-282">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b91a-282">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8b91a-283">Uruchom następujące polecenie w **zintegrowanym terminalu**:</span><span class="sxs-lookup"><span data-stu-id="8b91a-283">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="8b91a-284">Pusta aplikacja sieci Web ASP.NET Core, ukierunkowana na .NET Core, jest tworzona w katalogu *SignalRWebPack* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-284">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="8b91a-285">Konfigurowanie pakietu WebPack i języka TypeScript</span><span class="sxs-lookup"><span data-stu-id="8b91a-285">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="8b91a-286">Poniższe kroki umożliwiają skonfigurowanie konwersji języka TypeScript na JavaScript i zgrupowanie zasobów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8b91a-286">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="8b91a-287">Wykonaj następujące polecenie w katalogu głównym projektu, aby utworzyć plik *Package. JSON* :</span><span class="sxs-lookup"><span data-stu-id="8b91a-287">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="8b91a-288">Dodaj wyróżnioną właściwość do pliku *Package. JSON* :</span><span class="sxs-lookup"><span data-stu-id="8b91a-288">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="8b91a-289">Ustawienie właściwości `private` na `true` uniemożliwia ostrzeżenia instalacji pakietu w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="8b91a-289">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="8b91a-290">Zainstaluj wymagane pakiety npm.</span><span class="sxs-lookup"><span data-stu-id="8b91a-290">Install the required npm packages.</span></span> <span data-ttu-id="8b91a-291">Wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-291">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="8b91a-292">Niektóre szczegóły polecenia do uwagi:</span><span class="sxs-lookup"><span data-stu-id="8b91a-292">Some command details to note:</span></span>

    * <span data-ttu-id="8b91a-293">Numer wersji jest zgodny ze znakiem `@` dla każdej nazwy pakietu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-293">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="8b91a-294">npm instaluje te określone wersje pakietu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-294">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="8b91a-295">Opcja `-E` wyłącza domyślne zachowanie npm podczas pisania operatorów zakresu [wersji semantycznej](https://semver.org/) w pliku *Package. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8b91a-295">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="8b91a-296">Na przykład `"webpack": "4.29.3"` jest używany zamiast `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-296">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="8b91a-297">Ta opcja Zapobiega niezamierzonym uaktualnianiu do nowszych wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-297">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="8b91a-298">Aby uzyskać więcej informacji, zobacz oficjalne dokumenty z [npm-Install](https://docs.npmjs.com/cli/install) .</span><span class="sxs-lookup"><span data-stu-id="8b91a-298">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="8b91a-299">Zastąp Właściwość `scripts` pliku *Package. JSON* następującym fragmentem kodu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-299">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="8b91a-300">Niektóre objaśnienia dotyczące skryptów:</span><span class="sxs-lookup"><span data-stu-id="8b91a-300">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="8b91a-301">`build`: pakietuje zasoby po stronie klienta w trybie tworzenia i czujki pod kątem zmian w pliku.</span><span class="sxs-lookup"><span data-stu-id="8b91a-301">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="8b91a-302">Obserwator plików powoduje, że pakiet jest generowany ponownie za każdym razem, gdy plik projektu jest zmieniany.</span><span class="sxs-lookup"><span data-stu-id="8b91a-302">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="8b91a-303">Opcja `mode` wyłącza optymalizacje produkcyjne, takie jak wstrząsanie i minifikacjaowanie drzewa.</span><span class="sxs-lookup"><span data-stu-id="8b91a-303">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="8b91a-304">`build` w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="8b91a-304">Only use `build` in development.</span></span>
    * <span data-ttu-id="8b91a-305">`release`: pakietuje zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="8b91a-305">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="8b91a-306">`publish`: uruchamia skrypt `release`, aby powiązać zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="8b91a-306">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="8b91a-307">Wywołuje polecenie [publikowania](/dotnet/core/tools/dotnet-publish) interfejs wiersza polecenia platformy .NET Core, aby opublikować aplikację.</span><span class="sxs-lookup"><span data-stu-id="8b91a-307">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="8b91a-308">Utwórz plik o nazwie *WebPack. config. js*w katalogu głównym projektu o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="8b91a-308">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="8b91a-309">Poprzedni plik konfiguruje kompilację pakietu WebPack.</span><span class="sxs-lookup"><span data-stu-id="8b91a-309">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="8b91a-310">Niektóre szczegóły konfiguracji do uwagi:</span><span class="sxs-lookup"><span data-stu-id="8b91a-310">Some configuration details to note:</span></span>

    * <span data-ttu-id="8b91a-311">Właściwość `output` przesłania domyślną wartość *rozkłu*.</span><span class="sxs-lookup"><span data-stu-id="8b91a-311">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="8b91a-312">Pakiet jest emitowany w katalogu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-312">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="8b91a-313">Tablica `resolve.extensions` zawiera *js* do zaimportowania klienta SignalR JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8b91a-313">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="8b91a-314">Utwórz nowy katalog *src* w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-314">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="8b91a-315">Celem jest przechowywanie zasobów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8b91a-315">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="8b91a-316">Utwórz *src/index.html* z następującą zawartością.</span><span class="sxs-lookup"><span data-stu-id="8b91a-316">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="8b91a-317">Powyższy kod HTML definiuje standardowe znaczniki strony głównej.</span><span class="sxs-lookup"><span data-stu-id="8b91a-317">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="8b91a-318">Utwórz nowy katalog *src/CSS* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-318">Create a new *src/css* directory.</span></span> <span data-ttu-id="8b91a-319">Celem jest przechowywanie plików *CSS* projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-319">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="8b91a-320">Utwórz *src/CSS/Main. css* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="8b91a-320">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="8b91a-321">Poprzedni plik *Main. css* jest stylem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b91a-321">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="8b91a-322">Utwórz plik *src/tsconfig. JSON* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="8b91a-322">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="8b91a-323">Poprzedni kod konfiguruje kompilator języka TypeScript w celu utworzenia kodu JavaScript zgodnego z [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="8b91a-323">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="8b91a-324">Utwórz *src/index. TS* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="8b91a-324">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="8b91a-325">Poprzedni kod TypeScript pobiera odwołania do elementów DOM i dołącza dwa programy obsługi zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="8b91a-325">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="8b91a-326">`keyup`: to zdarzenie jest wyzwalane, gdy użytkownik wpisze coś w polu tekstowym określonym jako `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-326">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="8b91a-327">Funkcja `send` jest wywoływana, gdy użytkownik naciśnie klawisz **Enter** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-327">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="8b91a-328">`click`: to zdarzenie jest wyzwalane, gdy użytkownik kliknie przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-328">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="8b91a-329">Funkcja `send` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8b91a-329">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="8b91a-330">Konfigurowanie aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b91a-330">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="8b91a-331">Kod podany w metodzie `Startup.Configure` wyświetla *Hello World!* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-331">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="8b91a-332">Zastąp wywołanie metody `app.Run` z wywołaniami do [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="8b91a-332">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="8b91a-333">Poprzedni kod pozwala serwerowi zlokalizować i obsłużyć plik *index. html* , niezależnie od tego, czy użytkownik wprowadza pełny adres URL, czy główny adres URL aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8b91a-333">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="8b91a-334">Wywołaj metodę [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) w metodzie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-334">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8b91a-335">Dodaje do projektu usługi SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b91a-335">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="8b91a-336">Zamapuj trasę */Hub* do centrum `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-336">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="8b91a-337">Dodaj następujące wiersze na końcu metody `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8b91a-337">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="8b91a-338">Utwórz nowy katalog o nazwie *Hubs*w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-338">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="8b91a-339">Celem jest przechowywanie centrum SignalR, które jest tworzone w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="8b91a-339">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="8b91a-340">Utwórz centra centrów */ChatHub. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-340">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="8b91a-341">Dodaj następujący kod w górnej części pliku *Startup.cs* , aby rozwiązać `ChatHub` odwołanie:</span><span class="sxs-lookup"><span data-stu-id="8b91a-341">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="8b91a-342">Włączanie komunikacji klienta i serwera</span><span class="sxs-lookup"><span data-stu-id="8b91a-342">Enable client and server communication</span></span>

<span data-ttu-id="8b91a-343">W aplikacji jest obecnie wyświetlany prosty formularz służący do wysyłania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="8b91a-343">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="8b91a-344">Nic się nie dzieje, gdy użytkownik spróbuje to zrobić.</span><span class="sxs-lookup"><span data-stu-id="8b91a-344">Nothing happens when you try to do so.</span></span> <span data-ttu-id="8b91a-345">Serwer nasłuchuje określonej trasy, ale nic nie robi z wysłanymi komunikatami.</span><span class="sxs-lookup"><span data-stu-id="8b91a-345">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="8b91a-346">Wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-346">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="8b91a-347">Poprzednie polecenie instaluje [SignalR klienta języka TypeScript](https://www.npmjs.com/package/@microsoft/signalr), który umożliwia klientowi wysyłanie komunikatów do serwera programu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-347">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="8b91a-348">Dodaj wyróżniony kod do pliku *src/index. TS* :</span><span class="sxs-lookup"><span data-stu-id="8b91a-348">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="8b91a-349">Poprzedni kod obsługuje otrzymywanie komunikatów z serwera.</span><span class="sxs-lookup"><span data-stu-id="8b91a-349">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="8b91a-350">Klasa `HubConnectionBuilder` tworzy nowy Konstruktor służący do konfigurowania połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="8b91a-350">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="8b91a-351">Funkcja `withUrl` konfiguruje adres URL centrum.</span><span class="sxs-lookup"><span data-stu-id="8b91a-351">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="8b91a-352"> umożliwia wymianę komunikatów między klientem a serwerem.</span><span class="sxs-lookup"><span data-stu-id="8b91a-352"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="8b91a-353">Każdy komunikat ma określoną nazwę.</span><span class="sxs-lookup"><span data-stu-id="8b91a-353">Each message has a specific name.</span></span> <span data-ttu-id="8b91a-354">Można na przykład mieć komunikaty o nazwie `messageReceived`, które wykonują logikę odpowiedzialną za wyświetlanie nowej wiadomości w strefie messages.</span><span class="sxs-lookup"><span data-stu-id="8b91a-354">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="8b91a-355">Nasłuchiwanie określonego komunikatu można wykonać za pomocą funkcji `on`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-355">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="8b91a-356">Można nasłuchiwać dowolnej liczby nazw komunikatów.</span><span class="sxs-lookup"><span data-stu-id="8b91a-356">You can listen to any number of message names.</span></span> <span data-ttu-id="8b91a-357">Możliwe jest również przekazywanie parametrów do wiadomości, takich jak nazwa autora i zawartość otrzymanej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="8b91a-357">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="8b91a-358">Po odebraniu komunikatu przez klienta zostanie utworzony nowy element `div` z nazwą autora i treścią komunikatu w jego atrybucie `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-358">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="8b91a-359">Jest on dodawany do głównego elementu `div` wyświetlającego komunikaty.</span><span class="sxs-lookup"><span data-stu-id="8b91a-359">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="8b91a-360">Teraz, gdy klient może odebrać komunikat, skonfiguruj go do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="8b91a-360">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="8b91a-361">Dodaj wyróżniony kod do pliku *src/index. TS* :</span><span class="sxs-lookup"><span data-stu-id="8b91a-361">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="8b91a-362">Wysyłanie komunikatu przez połączenie z usługą WebSockets wymaga wywołania metody `send`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-362">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="8b91a-363">Pierwszym parametrem metody jest nazwa komunikatu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-363">The method's first parameter is the message name.</span></span> <span data-ttu-id="8b91a-364">Dane komunikatu są nieodpowiednie dla innych parametrów.</span><span class="sxs-lookup"><span data-stu-id="8b91a-364">The message data inhabits the other parameters.</span></span> <span data-ttu-id="8b91a-365">W tym przykładzie komunikat identyfikowany jako `newMessage` jest wysyłany do serwera.</span><span class="sxs-lookup"><span data-stu-id="8b91a-365">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="8b91a-366">Komunikat składa się z nazwy użytkownika i danych wejściowych użytkownika z pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="8b91a-366">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="8b91a-367">Jeśli wysyłanie działa, wartość pola tekstowego jest wyczyszczona.</span><span class="sxs-lookup"><span data-stu-id="8b91a-367">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="8b91a-368">Dodaj podświetloną metodę do klasy `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="8b91a-368">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="8b91a-369">Poprzedni kod emituje odebrane komunikaty wszystkim połączonym użytkownikom po ich odebraniu przez serwer.</span><span class="sxs-lookup"><span data-stu-id="8b91a-369">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="8b91a-370">Nie jest konieczne posiadanie generycznej metody `on`, aby odbierać wszystkie komunikaty.</span><span class="sxs-lookup"><span data-stu-id="8b91a-370">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="8b91a-371">Metoda o nazwie po wystarczającej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8b91a-371">A method named after the message name suffices.</span></span>

    <span data-ttu-id="8b91a-372">W tym przykładzie klient TypeScript wysyła komunikat zidentyfikowany jako `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-372">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="8b91a-373">Metoda C# `NewMessage` oczekuje danych wysyłanych przez klienta.</span><span class="sxs-lookup"><span data-stu-id="8b91a-373">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="8b91a-374">Wykonano wywołanie metody [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) na [klientach. All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="8b91a-374">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="8b91a-375">Odebrane komunikaty są wysyłane do wszystkich klientów podłączonych do centrum.</span><span class="sxs-lookup"><span data-stu-id="8b91a-375">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="8b91a-376">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8b91a-376">Test the app</span></span>

<span data-ttu-id="8b91a-377">Upewnij się, że aplikacja działa z następującymi krokami.</span><span class="sxs-lookup"><span data-stu-id="8b91a-377">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b91a-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b91a-378">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8b91a-379">Uruchom pakiet WebPack w trybie *wydania* .</span><span class="sxs-lookup"><span data-stu-id="8b91a-379">Run Webpack in *release* mode.</span></span> <span data-ttu-id="8b91a-380">Za pomocą okna **konsoli Menedżera pakietów** wykonaj następujące polecenie w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-380">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="8b91a-381">Jeśli nie jesteś w katalogu głównym projektu, wprowadź `cd SignalRWebPack` przed wprowadzeniem polecenia.</span><span class="sxs-lookup"><span data-stu-id="8b91a-381">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="8b91a-382">Wybierz pozycję **debuguj** > **Rozpocznij bez debugowania** , aby uruchomić aplikację w przeglądarce bez dołączania debugera.</span><span class="sxs-lookup"><span data-stu-id="8b91a-382">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="8b91a-383">Plik *wwwroot/index.html* jest obsługiwany w `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-383">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="8b91a-384">Otwórz inne wystąpienie przeglądarki (dowolna przeglądarka).</span><span class="sxs-lookup"><span data-stu-id="8b91a-384">Open another browser instance (any browser).</span></span> <span data-ttu-id="8b91a-385">Wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-385">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8b91a-386">Wybierz opcję przeglądarka, wpisz coś w polu tekstowym **komunikat** , a następnie kliknij przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-386">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="8b91a-387">Unikatowa nazwa użytkownika i komunikat są wyświetlane na obu stronach natychmiastowo.</span><span class="sxs-lookup"><span data-stu-id="8b91a-387">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b91a-388">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b91a-388">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="8b91a-389">Uruchom pakiet WebPack w trybie *wydania* , wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-389">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="8b91a-390">Skompiluj i uruchom aplikację, wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="8b91a-390">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="8b91a-391">Serwer sieci Web uruchamia aplikację i udostępnia ją na hoście lokalnym.</span><span class="sxs-lookup"><span data-stu-id="8b91a-391">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="8b91a-392">Otwórz przeglądarkę, aby `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="8b91a-392">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="8b91a-393">Plik *wwwroot/index.html* jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="8b91a-393">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="8b91a-394">Skopiuj adres URL z paska adresu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-394">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="8b91a-395">Otwórz inne wystąpienie przeglądarki (dowolna przeglądarka).</span><span class="sxs-lookup"><span data-stu-id="8b91a-395">Open another browser instance (any browser).</span></span> <span data-ttu-id="8b91a-396">Wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8b91a-396">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8b91a-397">Wybierz opcję przeglądarka, wpisz coś w polu tekstowym **komunikat** , a następnie kliknij przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="8b91a-397">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="8b91a-398">Unikatowa nazwa użytkownika i komunikat są wyświetlane na obu stronach natychmiastowo.</span><span class="sxs-lookup"><span data-stu-id="8b91a-398">The unique user name and message are displayed on both pages instantly.</span></span>

---

![komunikat wyświetlany w obu oknach przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8b91a-400">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8b91a-400">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
