---
title: Używanie biblioteki SignalR platformy ASP.NET Core za pomocą TypeScript i Webpack
author: ssougnez
description: W tym samouczku skonfigurujesz Webpack połączyć w paczkę i Utwórz aplikację sieci web biblioteki SignalR platformy ASP.NET Core, którego klient został napisany w TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 04/23/2019
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: b186dffd724fb2fed49bbc2587ec066db319dffc
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610437"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="a2935-103">Używanie biblioteki SignalR platformy ASP.NET Core za pomocą TypeScript i Webpack</span><span class="sxs-lookup"><span data-stu-id="a2935-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="a2935-104">Przez [Sébastien Sougnez](https://twitter.com/ssougnez) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="a2935-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="a2935-105">[Webpack](https://webpack.js.org/) umożliwia deweloperom połączyć w paczkę i tworzyć zasoby po stronie klienta aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a2935-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="a2935-106">Ten samouczek pokazuje, za pomocą Webpack w aplikacji sieci web biblioteki SignalR platformy ASP.NET Core, którego klient został napisany w [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="a2935-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="a2935-107">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a2935-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a2935-108">Tworzenia szkieletu początkową aplikację biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2935-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="a2935-109">Konfigurowanie klienta SignalR TypeScript</span><span class="sxs-lookup"><span data-stu-id="a2935-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="a2935-110">Konfigurowanie potoku kompilacji przy użyciu Webpack</span><span class="sxs-lookup"><span data-stu-id="a2935-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="a2935-111">Konfigurowanie serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="a2935-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="a2935-112">Włącz komunikację między klientem i serwerem</span><span class="sxs-lookup"><span data-stu-id="a2935-112">Enable communication between client and server</span></span>

<span data-ttu-id="a2935-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a2935-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a2935-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a2935-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2935-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2935-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a2935-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="a2935-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a2935-117">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a2935-117">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a2935-118">[Node.js](https://nodejs.org/) z [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a2935-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2935-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2935-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a2935-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2935-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a2935-121">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a2935-121">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a2935-122">C#dla wersji programu Visual Studio Code 1.17.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="a2935-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="a2935-123">[Node.js](https://nodejs.org/) z [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a2935-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="a2935-124">Tworzenie aplikacji sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2935-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2935-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2935-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a2935-126">Konfigurowanie programu Visual Studio, aby wyszukać pakiety npm w usłudze *ścieżki* zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="a2935-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="a2935-127">Domyślnie program Visual Studio korzysta z wersji npm znajduje się w jego katalogu instalacji.</span><span class="sxs-lookup"><span data-stu-id="a2935-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="a2935-128">Wykonaj te instrukcje w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a2935-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="a2935-129">Przejdź do **narzędzia** > **opcje** > **projekty i rozwiązania** > **sieci Web zarządzania pakietami**  >  **Zewnętrzne narzędzia sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="a2935-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="a2935-130">Wybierz *$(PATH)* wpis na liście.</span><span class="sxs-lookup"><span data-stu-id="a2935-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="a2935-131">Kliknij strzałkę w górę, aby przenieść wpis do drugiej pozycji na liście.</span><span class="sxs-lookup"><span data-stu-id="a2935-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio Configuration](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="a2935-133">Visual Studio zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="a2935-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="a2935-134">Nadszedł czas, aby utworzyć projekt.</span><span class="sxs-lookup"><span data-stu-id="a2935-134">It's time to create the project.</span></span>

1. <span data-ttu-id="a2935-135">Użyj **pliku** > **New** > **projektu** menu opcji, a następnie wybierz **aplikacji sieci Web programu ASP.NET Core** szablon.</span><span class="sxs-lookup"><span data-stu-id="a2935-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="a2935-136">Nadaj projektowi nazwę *SignalRWebPack*i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2935-136">Name the project *SignalRWebPack*, and select **OK**.</span></span>
1. <span data-ttu-id="a2935-137">Wybierz *platformy .NET Core* z listy rozwijanej i wybierz platformę docelową *platformy ASP.NET Core 2.2* z listy rozwijanej selektorów framework.</span><span class="sxs-lookup"><span data-stu-id="a2935-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="a2935-138">Wybierz **pusty** szablonu, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2935-138">Select the **Empty** template, and select **OK**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2935-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2935-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a2935-140">Uruchom następujące polecenie **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="a2935-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="a2935-141">Pusta aplikacja internetowa platformy ASP.NET Core, przeznaczonych dla platformy .NET Core jest tworzony w *SignalRWebPack* katalogu.</span><span class="sxs-lookup"><span data-stu-id="a2935-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="a2935-142">Konfigurowanie Webpack i TypeScript</span><span class="sxs-lookup"><span data-stu-id="a2935-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="a2935-143">Poniższe kroki konfigurowania konwersji TypeScript w kodzie JavaScript i paczki zasoby po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a2935-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="a2935-144">Wykonaj następujące polecenie w katalogu głównym projektu, aby utworzyć *package.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="a2935-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="a2935-145">Dodaj wyróżnione właściwość do *package.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="a2935-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="a2935-146">Ustawienie `private` właściwość `true` zapobiega ostrzeżeń instalacji pakietu w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="a2935-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="a2935-147">Zainstaluj pakiety npm wymagane.</span><span class="sxs-lookup"><span data-stu-id="a2935-147">Install the required npm packages.</span></span> <span data-ttu-id="a2935-148">Wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="a2935-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="a2935-149">Niektóre szczegóły polecenia należy pamiętać:</span><span class="sxs-lookup"><span data-stu-id="a2935-149">Some command details to note:</span></span>

    * <span data-ttu-id="a2935-150">Numer wersji jest zgodna `@` podpisywania dla każdej nazwy pakietu.</span><span class="sxs-lookup"><span data-stu-id="a2935-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="a2935-151">npm instaluje te wersje określonego pakietu.</span><span class="sxs-lookup"><span data-stu-id="a2935-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="a2935-152">`-E` Opcja wyłącza npm domyślne zachowanie dotyczące pisania [wersji semantycznej](https://semver.org/) zakresu operatorom *package.json*.</span><span class="sxs-lookup"><span data-stu-id="a2935-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="a2935-153">Na przykład `"webpack": "4.29.3"` jest używana zamiast `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="a2935-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="a2935-154">Ta opcja uniemożliwia niezamierzone uaktualnienia do nowszej wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="a2935-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="a2935-155">Można znaleźć w oficjalnej [npm install](https://docs.npmjs.com/cli/install) docs, aby uzyskać więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="a2935-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="a2935-156">Zastąp `scripts` właściwość *package.json* pliku następującym fragmentem kodu:</span><span class="sxs-lookup"><span data-stu-id="a2935-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="a2935-157">Niektórych wyjaśnienie skrypty:</span><span class="sxs-lookup"><span data-stu-id="a2935-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="a2935-158">`build`: Razem z zasobami klienta w trybie projektowania, a następnie oczekuje na zmiany w plikach.</span><span class="sxs-lookup"><span data-stu-id="a2935-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="a2935-159">Obserwator plików powoduje, że pakiet ponownie wygenerować każdorazowo zmiany w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="a2935-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="a2935-160">`mode` Opcja wyłącza optymalizacje, produkcji, takie jak potrząsając drzewa i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="a2935-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="a2935-161">Używaj tylko `build` w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="a2935-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="a2935-162">`release`: Zawiera zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="a2935-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="a2935-163">`publish`: Przebiegi `release` skrypt, aby powiązać zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="a2935-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="a2935-164">Wywołuje .NET Core interfejsu wiersza polecenia [publikowania](/dotnet/core/tools/dotnet-publish) polecenie, aby opublikować aplikację.</span><span class="sxs-lookup"><span data-stu-id="a2935-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="a2935-165">Utwórz plik o nazwie *webpack.config.js*, w katalogu głównym projektu o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="a2935-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="a2935-166">Poprzedni plik konfiguruje kompilacji Webpack.</span><span class="sxs-lookup"><span data-stu-id="a2935-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="a2935-167">Niektóre szczegóły konfiguracji należy pamiętać:</span><span class="sxs-lookup"><span data-stu-id="a2935-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="a2935-168">`output` Właściwość zastępuje wartość domyślną *dist*.</span><span class="sxs-lookup"><span data-stu-id="a2935-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="a2935-169">Zamiast tego znacznikowej pakietu w *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="a2935-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="a2935-170">`resolve.extensions` Tablica zawiera *js* do zaimportowania klienta SignalR JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a2935-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="a2935-171">Utwórz nową *src* katalogu w folderze głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="a2935-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="a2935-172">Jego celem jest do przechowywania zasobów po stronie klienta projektu.</span><span class="sxs-lookup"><span data-stu-id="a2935-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="a2935-173">Tworzenie *src/index.html* o następującej zawartości.</span><span class="sxs-lookup"><span data-stu-id="a2935-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="a2935-174">Poprzedni kod HTML definiuje znaczników standardowy na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="a2935-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="a2935-175">Utwórz nową *src/css* katalogu.</span><span class="sxs-lookup"><span data-stu-id="a2935-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="a2935-176">Jej celem jest zapisanie projektu *.css* plików.</span><span class="sxs-lookup"><span data-stu-id="a2935-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="a2935-177">Tworzenie *src/css/main.css* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="a2935-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="a2935-178">Poprzedni *main.css* pliku style aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a2935-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="a2935-179">Tworzenie *src/tsconfig.json* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="a2935-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="a2935-180">Powyższy kod konfiguruje kompilatora TypeScript, aby wygenerować [ECMAScript](https://wikipedia.org/wiki/ECMAScript) zgodnego z 5 JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a2935-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="a2935-181">Tworzenie *src/index.ts* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="a2935-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="a2935-182">Poprzedni TypeScript pobiera odwołania do elementów modelu DOM i dołącza dwóch programów obsługi zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="a2935-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="a2935-183">`keyup`: To zdarzenie jest uruchamiana, gdy użytkownik wpisze coś w pole tekstowe zidentyfikowane jako `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="a2935-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="a2935-184">`send` Funkcja jest wywoływana, gdy użytkownik naciśnie **Enter** klucza.</span><span class="sxs-lookup"><span data-stu-id="a2935-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="a2935-185">`click`: To zdarzenie jest uruchamiana, gdy użytkownik kliknie **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a2935-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="a2935-186">`send` Funkcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="a2935-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="a2935-187">Konfigurowanie aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2935-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="a2935-188">Kod zawarty w `Startup.Configure` metoda Wyświetla *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="a2935-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="a2935-189">Zastąp `app.Run` wywołanie metody z wywołaniami [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="a2935-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="a2935-190">Powyższy kod zezwala serwerowi do lokalizowania i obsługiwać *index.html* pliku, czy użytkownik musi wprowadzić jej pełny adres URL lub adres URL katalogu głównego aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a2935-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="a2935-191">Wywołaj [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) w `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="a2935-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a2935-192">Usługi SignalR dodaje do projektu.</span><span class="sxs-lookup"><span data-stu-id="a2935-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="a2935-193">Mapa */hub* kierować do `ChatHub` koncentratora.</span><span class="sxs-lookup"><span data-stu-id="a2935-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="a2935-194">Dodaj następujące wiersze na końcu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="a2935-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="a2935-195">Utwórz nowy katalog o nazwie *koncentratory*, w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="a2935-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="a2935-196">Jego celem jest do przechowywania Centrum SignalR, który jest tworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="a2935-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="a2935-197">Utwórz koncentrator *Hubs/ChatHub.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a2935-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="a2935-198">Dodaj następujący kod w górnej części *Startup.cs* plik, aby rozwiązać `ChatHub` odwołania:</span><span class="sxs-lookup"><span data-stu-id="a2935-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="a2935-199">Włącz komunikację z klientem i serwerem</span><span class="sxs-lookup"><span data-stu-id="a2935-199">Enable client and server communication</span></span>

<span data-ttu-id="a2935-200">Aplikacja Wyświetla obecnie prosty formularz do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a2935-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="a2935-201">Podczas próby zrobić nic się nie dzieje.</span><span class="sxs-lookup"><span data-stu-id="a2935-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="a2935-202">Serwer nasłuchuje do określonej trasy, ale nie robi nic za pomocą wysłanych komunikatów.</span><span class="sxs-lookup"><span data-stu-id="a2935-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="a2935-203">Wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="a2935-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="a2935-204">Poprzednie polecenie instaluje [klienta SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), co pozwala klientowi wysłać wiadomości do serwera.</span><span class="sxs-lookup"><span data-stu-id="a2935-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="a2935-205">Dodaj wyróżniony kod do *src/index.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="a2935-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="a2935-206">Powyższy kod obsługuje odbieranie komunikatów z serwera.</span><span class="sxs-lookup"><span data-stu-id="a2935-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="a2935-207">`HubConnectionBuilder` Klasy tworzy nowy konstruktor do skonfigurowania połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a2935-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="a2935-208">`withUrl` Funkcja konfiguruje adres URL koncentratora.</span><span class="sxs-lookup"><span data-stu-id="a2935-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="a2935-209">SignalR umożliwia wymianę wiadomości między klientem a serwerem.</span><span class="sxs-lookup"><span data-stu-id="a2935-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="a2935-210">Każdy komunikat ma określoną nazwę.</span><span class="sxs-lookup"><span data-stu-id="a2935-210">Each message has a specific name.</span></span> <span data-ttu-id="a2935-211">Na przykład masz wiadomości o nazwie `messageReceived` , wykonana logiki odpowiada za wyświetlanie nowy komunikat w strefie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a2935-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="a2935-212">Nasłuchiwanie szczegółowy komunikat o błędzie może odbywać się za pośrednictwem `on` funkcji.</span><span class="sxs-lookup"><span data-stu-id="a2935-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="a2935-213">Można słuchać dowolną liczbę nazw wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a2935-213">You can listen to any number of message names.</span></span> <span data-ttu-id="a2935-214">Istnieje również możliwość przekazania parametrów do wiadomości, takie jak imię i nazwisko autora i zawartość odebrany komunikat.</span><span class="sxs-lookup"><span data-stu-id="a2935-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="a2935-215">Gdy klient odbierze komunikat nową `div` zostanie utworzony element o nazwie autora i komunikat zawartość w jego `innerHTML` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a2935-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="a2935-216">Jest ona dodawana do głównej `div` element wyświetlania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="a2935-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="a2935-217">Teraz, gdy klient może otrzymać komunikat, należy skonfigurować go do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a2935-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="a2935-218">Dodaj wyróżniony kod do *src/index.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="a2935-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="a2935-219">Wysyłanie wiadomości przez połączenie Websocket wymaga wywołania `send` metody.</span><span class="sxs-lookup"><span data-stu-id="a2935-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="a2935-220">Pierwszy parametr metody jest nazwa komunikatu.</span><span class="sxs-lookup"><span data-stu-id="a2935-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="a2935-221">Dane wiadomości zamieszkuje innych parametrów.</span><span class="sxs-lookup"><span data-stu-id="a2935-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="a2935-222">W tym przykładzie wiadomość zidentyfikowane jako `newMessage` są wysyłane do serwera.</span><span class="sxs-lookup"><span data-stu-id="a2935-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="a2935-223">Komunikat składa się z nazwy użytkownika i dane wejściowe z pola tekstowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a2935-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="a2935-224">Jeśli działa wysyłania, wartość pola tekstowego jest wyczyszczone.</span><span class="sxs-lookup"><span data-stu-id="a2935-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="a2935-225">Dodaj metodę wyróżnione `ChatHub` klasy:</span><span class="sxs-lookup"><span data-stu-id="a2935-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="a2935-226">Powyższy kod emituje Odebrane komunikaty do wszystkich połączonych użytkowników, gdy serwer otrzymuje je.</span><span class="sxs-lookup"><span data-stu-id="a2935-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="a2935-227">Nie jest konieczne zapewnienie ogólnej `on` metodę, aby otrzymywać wszystkie komunikaty.</span><span class="sxs-lookup"><span data-stu-id="a2935-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="a2935-228">Metodę o nazwie po sufiksy nazwa komunikatu.</span><span class="sxs-lookup"><span data-stu-id="a2935-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="a2935-229">W tym przykładzie klient TypeScript wysyła komunikat, który został zidentyfikowany jako `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="a2935-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="a2935-230">C# `NewMessage` metoda oczekuje, że dane wysłane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="a2935-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="a2935-231">Połączenie jest nawiązywane w przypadku [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metody [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="a2935-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="a2935-232">Odebrane komunikaty są wysyłane do wszystkich klientów podłączone do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="a2935-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="a2935-233">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="a2935-233">Test the app</span></span>

<span data-ttu-id="a2935-234">Upewnij się, że aplikacja działa wykonując następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="a2935-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2935-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2935-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a2935-236">Uruchamianie Webpack w *wersji* trybu.</span><span class="sxs-lookup"><span data-stu-id="a2935-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="a2935-237">Za pomocą **Konsola Menedżera pakietów** okna, wykonaj następujące polecenie w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="a2935-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="a2935-238">Jeśli nie jesteś w katalogu głównym projektu, wprowadź `cd SignalRWebPack` przed wpisaniem polecenia.</span><span class="sxs-lookup"><span data-stu-id="a2935-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="a2935-239">Wybierz **debugowania** > **Uruchom bez debugowania** do uruchomienia aplikacji w przeglądarce, bez dołączania debugera.</span><span class="sxs-lookup"><span data-stu-id="a2935-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="a2935-240">*Wwwroot/index.html* plików jest obsługiwany w `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="a2935-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="a2935-241">Otwórz inne wystąpienie przeglądarki (w dowolnej przeglądarce).</span><span class="sxs-lookup"><span data-stu-id="a2935-241">Open another browser instance (any browser).</span></span> <span data-ttu-id="a2935-242">Wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="a2935-242">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a2935-243">Wybierz albo przeglądarki, wpisz coś w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a2935-243">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="a2935-244">Unikatowa nazwa użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="a2935-244">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2935-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2935-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a2935-246">Uruchamianie Webpack w *wersji* trybu, wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="a2935-246">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="a2935-247">Tworzenie i uruchamianie aplikacji, wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="a2935-247">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="a2935-248">Serwer sieci web uruchamia aplikację i udostępnia je na hoście lokalnym.</span><span class="sxs-lookup"><span data-stu-id="a2935-248">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="a2935-249">Otwórz w przeglądarce `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="a2935-249">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="a2935-250">*Wwwroot/index.html* plików jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="a2935-250">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="a2935-251">Skopiuj adres URL z paska adresu.</span><span class="sxs-lookup"><span data-stu-id="a2935-251">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="a2935-252">Otwórz inne wystąpienie przeglądarki (w dowolnej przeglądarce).</span><span class="sxs-lookup"><span data-stu-id="a2935-252">Open another browser instance (any browser).</span></span> <span data-ttu-id="a2935-253">Wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="a2935-253">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a2935-254">Wybierz albo przeglądarki, wpisz coś w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a2935-254">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="a2935-255">Unikatowa nazwa użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="a2935-255">The unique user name and message are displayed on both pages instantly.</span></span>

---

![komunikat wyświetlany w oba okna przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="a2935-257">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a2935-257">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
