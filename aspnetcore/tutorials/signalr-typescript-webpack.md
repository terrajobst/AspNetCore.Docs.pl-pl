---
title: SignalR platformy ASP.NET Core za pomocą języka TypeScript i Webpack
author: ssougnez
description: W tym samouczku skonfigurujesz Webpack połączyć w paczkę i tworzenie aplikacji sieci web platformy ASP.NET Core SignalR, którego klient są zapisywane na maszynie.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: e4b00fa61ea0becba7d678a0b7c94d2d30f06740
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126432"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="6471e-103">SignalR platformy ASP.NET Core za pomocą języka TypeScript i Webpack</span><span class="sxs-lookup"><span data-stu-id="6471e-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="6471e-104">Przez [Sébastien Sougnez](https://twitter.com/ssougnez) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="6471e-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="6471e-105">[Webpack](https://webpack.js.org/) umożliwia deweloperom pakietu i utworzyć zasobów aplikacji sieci web po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="6471e-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="6471e-106">Ten samouczek pokazuje, przy użyciu Webpack w aplikacji sieci web platformy ASP.NET Core SignalR, którego klient są zapisywane w [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="6471e-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="6471e-107">Z tego samouczka, dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="6471e-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6471e-108">Tworzenie szkieletu początkową aplikację ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6471e-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="6471e-109">Konfigurowanie klienta SignalR TypeScript</span><span class="sxs-lookup"><span data-stu-id="6471e-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="6471e-110">Konfigurowanie procesu kompilacji, przy użyciu Webpack</span><span class="sxs-lookup"><span data-stu-id="6471e-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="6471e-111">Konfigurowanie serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="6471e-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="6471e-112">Włącz komunikację między klientem a serwerem</span><span class="sxs-lookup"><span data-stu-id="6471e-112">Enable communication between client and server</span></span>

<span data-ttu-id="6471e-113">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6471e-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6471e-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6471e-114">Prerequisites</span></span>

<span data-ttu-id="6471e-115">Należy zainstalować następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="6471e-115">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6471e-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6471e-116">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="6471e-117">Oprogramowanie .NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="6471e-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6471e-118">[Node.js](https://nodejs.org/) z [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="6471e-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>
* <span data-ttu-id="6471e-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7 lub nowszym z **ASP.NET i sieć web development** obciążenia</span><span class="sxs-lookup"><span data-stu-id="6471e-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6471e-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6471e-120">.NET Core CLI</span></span>](#tab/netcore-cli)

* [<span data-ttu-id="6471e-121">Oprogramowanie .NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="6471e-121">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6471e-122">[Node.js](https://nodejs.org/) z [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="6471e-122">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="6471e-123">Tworzenie aplikacji sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6471e-123">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6471e-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6471e-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6471e-125">Konfigurowanie programu Visual Studio, aby wyszukać npm w *ścieżki* zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="6471e-125">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="6471e-126">Domyślnie program Visual Studio korzysta z wersji programu npm znajdujące się w jego katalogu instalacji.</span><span class="sxs-lookup"><span data-stu-id="6471e-126">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="6471e-127">Wykonaj te instrukcje w Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6471e-127">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="6471e-128">Przejdź do **narzędzia** > **opcje** > **projekty i rozwiązania** > **sieci Web pakietu zarządzania**  >  **Narzędzi zewnętrznych sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="6471e-128">Navigate to **Tools** > **Options** > **Projects and solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="6471e-129">Wybierz *$(PATH)* wpisu z listy.</span><span class="sxs-lookup"><span data-stu-id="6471e-129">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="6471e-130">Kliknij strzałkę w górę, aby przenieść wpis drugi pozycji na liście.</span><span class="sxs-lookup"><span data-stu-id="6471e-130">Click the up arrow to move the entry to the second position in the list.</span></span> <span data-ttu-id="6471e-131">Jako Zarezerwuj pierwszej pozycji odwołuje się do lokalnego pakietów projektu.</span><span class="sxs-lookup"><span data-stu-id="6471e-131">As an aside, the first entry refers to the project's local packages.</span></span>

    ![Konfiguracja programu Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="6471e-133">Visual Studio zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="6471e-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="6471e-134">Nadszedł czas, aby utworzyć projekt.</span><span class="sxs-lookup"><span data-stu-id="6471e-134">It's time to create the project.</span></span>

1. <span data-ttu-id="6471e-135">Użyj **pliku** > **nowy** > **projektu** menu opcję i wybierz polecenie **aplikacji sieci Web platformy ASP.NET Core** szablon.</span><span class="sxs-lookup"><span data-stu-id="6471e-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="6471e-136">Nazwij projekt *SignalRWebPack*i kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="6471e-136">Name the project *SignalRWebPack*, and click the **OK** button.</span></span>
1. <span data-ttu-id="6471e-137">Wybierz *.NET Core* z listy rozwijanej i wybierz platformę docelową *platformy ASP.NET Core 2.1* z listy rozwijanej selektora framework.</span><span class="sxs-lookup"><span data-stu-id="6471e-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.1* from the framework selector drop-down.</span></span> <span data-ttu-id="6471e-138">Wybierz **pusty** szablonu, a następnie kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="6471e-138">Select the **Empty** template, and click the **OK** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6471e-139">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6471e-139">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6471e-140">Uruchom następujące polecenie **zintegrowane Terminal**:</span><span class="sxs-lookup"><span data-stu-id="6471e-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="6471e-141">Pusty aplikacji sieci web platformy ASP.NET Core, przeznaczonych dla platformy .NET Core jest tworzony w *SignalRWebPack* katalogu.</span><span class="sxs-lookup"><span data-stu-id="6471e-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="6471e-142">Skonfiguruj Webpack i TypeScript</span><span class="sxs-lookup"><span data-stu-id="6471e-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="6471e-143">Poniższe kroki konfigurowania konwersji TypeScript do języka JavaScript i paczki zasobów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="6471e-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="6471e-144">Uruchom następujące polecenie w katalogu głównym projektu, aby utworzyć *package.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="6471e-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="6471e-145">Dodaj wyróżnione właściwość do *package.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="6471e-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="6471e-146">Ustawienie `private` właściwości `true` uniemożliwia ostrzeżenia instalacji pakietu w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="6471e-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="6471e-147">Instalowanie pakietów npm wymagane.</span><span class="sxs-lookup"><span data-stu-id="6471e-147">Install the required npm packages.</span></span> <span data-ttu-id="6471e-148">Uruchom następujące polecenie z katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="6471e-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="6471e-149">Niektóre szczegóły polecenia należy pamiętać:</span><span class="sxs-lookup"><span data-stu-id="6471e-149">Some command details to note:</span></span>

    * <span data-ttu-id="6471e-150">Liczba wersji następuje `@` podpisywania dla każdej nazwy pakietu.</span><span class="sxs-lookup"><span data-stu-id="6471e-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="6471e-151">npm instaluje te wersje określonego pakietu.</span><span class="sxs-lookup"><span data-stu-id="6471e-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="6471e-152">`-E` Opcja powoduje wyłączenie na npm domyślne zachowanie zapisu [wersjonowania semantycznego](https://semver.org/) operatory, które mają należeć do zakresu *package.json*.</span><span class="sxs-lookup"><span data-stu-id="6471e-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="6471e-153">Na przykład `"webpack": "4.12.0"` jest używany zamiast `"webpack": "^4.12.0"`.</span><span class="sxs-lookup"><span data-stu-id="6471e-153">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="6471e-154">Ta opcja uniemożliwia niezamierzone uaktualnienia do nowszych wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="6471e-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="6471e-155">Można znaleźć w oficjalnej [npm install](https://docs.npmjs.com/cli/install) dokumentów, aby uzyskać więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="6471e-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="6471e-156">Zastąp `scripts` właściwość *package.json* pliku następującym fragmentem kodu:</span><span class="sxs-lookup"><span data-stu-id="6471e-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="6471e-157">Wyjaśnienie niektórych skryptów:</span><span class="sxs-lookup"><span data-stu-id="6471e-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="6471e-158">`build`: Zawiera zasoby po stronie klienta w trybie projektowania i oczekuje na zmiany pliku.</span><span class="sxs-lookup"><span data-stu-id="6471e-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="6471e-159">Obserwator pliku powoduje, że pakiet można ponownie wygenerować zawsze zmiany w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="6471e-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="6471e-160">`mode` Opcja wyłącza optymalizacje produkcji, takie jak potrząsając drzewa i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="6471e-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="6471e-161">Należy używać tylko `build` do rozwoju.</span><span class="sxs-lookup"><span data-stu-id="6471e-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="6471e-162">`release`: Zawiera zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6471e-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="6471e-163">`publish`: Uruchamia `release` skryptu pakietów zasobów po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6471e-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="6471e-164">Wywołuje .NET Core CLI [publikowania](/dotnet/core/tools/dotnet-publish) polecenie w celu opublikowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6471e-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="6471e-165">Utwórz plik o nazwie *webpack.config.js*, w katalogu głównym projektu o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="6471e-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="6471e-166">Poprzedniego pliku konfiguruje Webpack kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6471e-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="6471e-167">Niektóre szczegóły konfiguracji należy zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="6471e-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="6471e-168">`output` Właściwość zastępuje domyślną wartość *dist*.</span><span class="sxs-lookup"><span data-stu-id="6471e-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="6471e-169">Zamiast tego emitowanego pakietu w *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="6471e-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="6471e-170">`resolve.extensions` Tablica zawiera *js* do zaimportowania klienta SignalR JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6471e-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="6471e-171">Utwórz nową *src* katalogu w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="6471e-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="6471e-172">Jej celem jest do przechowywania zasobów projektu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="6471e-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="6471e-173">Utwórz *src/index.html* o następującej zawartości.</span><span class="sxs-lookup"><span data-stu-id="6471e-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="6471e-174">Poprzedni kod HTML definiuje znaczników umożliwiającego stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="6471e-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="6471e-175">Utwórz nową *src/css* katalogu.</span><span class="sxs-lookup"><span data-stu-id="6471e-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="6471e-176">Jej celem jest do przechowywania projektu *.css* plików.</span><span class="sxs-lookup"><span data-stu-id="6471e-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="6471e-177">Utwórz *src/css/main.css* o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="6471e-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="6471e-178">Poprzedni *main.css* pliku style aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6471e-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="6471e-179">Utwórz *src/tsconfig.json* o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="6471e-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="6471e-180">Poprzedni kod konfiguruje kompilatora języka TypeScript w celu utworzenia [ECMAScript](https://wikipedia.org/wiki/ECMAScript) zgodnych z 5 JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6471e-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="6471e-181">Utwórz *src/index.ts* o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="6471e-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="6471e-182">TypeScript poprzedniego pobiera odwołania do elementów modelu DOM i dołącza dwie procedury obsługi zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="6471e-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="6471e-183">`keyup`: To zdarzenie generowane, gdy użytkownik wpisze coś w polu tekstowym zidentyfikowane jako `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="6471e-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="6471e-184">`send` Funkcja jest wywoływana, gdy użytkownik naciśnie **Enter** klucza.</span><span class="sxs-lookup"><span data-stu-id="6471e-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="6471e-185">`click`: To zdarzenie generowane, gdy użytkownik kliknie **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="6471e-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="6471e-186">`send` Funkcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="6471e-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="6471e-187">Konfigurowanie aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6471e-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="6471e-188">Kod zawarty w `Startup.Configure` metoda Wyświetla *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="6471e-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="6471e-189">Zastąp `app.Run` wywołanie metody wywołania [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="6471e-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="6471e-190">Poprzedni kod umożliwia serwerowi zlokalizować i obsługiwać *index.html* pliku, czy użytkownik musi wprowadzić jego pełny adres URL lub adres URL katalogu głównego aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="6471e-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="6471e-191">Wywołanie [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) w `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="6471e-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6471e-192">Dodaje do projektu usługi SignalR.</span><span class="sxs-lookup"><span data-stu-id="6471e-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="6471e-193">Mapa */hub* kierować do `ChatHub` koncentratora.</span><span class="sxs-lookup"><span data-stu-id="6471e-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="6471e-194">Dodaj następujące wiersze na końcu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="6471e-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="6471e-195">Utwórz nowy katalog o nazwie *koncentratory*, w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="6471e-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="6471e-196">Jego celem jest przechowywanie koncentratora SignalR, który jest tworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="6471e-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="6471e-197">Tworzenie Centrum *Hubs/ChatHub.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6471e-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="6471e-198">Dodaj następujący kod w górnej części *Startup.cs* pliku `ChatHub` odwołania:</span><span class="sxs-lookup"><span data-stu-id="6471e-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="6471e-199">Włącz komunikację klienta i serwera</span><span class="sxs-lookup"><span data-stu-id="6471e-199">Enable client and server communication</span></span>

<span data-ttu-id="6471e-200">Aplikacja Wyświetla obecnie prostego formularza do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6471e-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="6471e-201">Podczas próby zrobić nic się nie dzieje.</span><span class="sxs-lookup"><span data-stu-id="6471e-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="6471e-202">Serwer nasłuchuje do określonej trasy, ale nie działa z wysyłane wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6471e-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="6471e-203">Uruchom następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="6471e-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="6471e-204">Poprzednie polecenie instaluje [klienta SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), co pozwala klientowi wysłanie wiadomości do serwera.</span><span class="sxs-lookup"><span data-stu-id="6471e-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="6471e-205">Dodaj wyróżniony kod, aby *src/index.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="6471e-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="6471e-206">Poprzedni kod obsługuje odbieranie komunikatów z serwera.</span><span class="sxs-lookup"><span data-stu-id="6471e-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="6471e-207">`HubConnectionBuilder` Klasy tworzy nowego Konstruktora do konfigurowania połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6471e-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="6471e-208">`withUrl` Funkcja konfiguruje adres URL koncentratora.</span><span class="sxs-lookup"><span data-stu-id="6471e-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="6471e-209">SignalR umożliwia wymianę wiadomości między klientem serwerem.</span><span class="sxs-lookup"><span data-stu-id="6471e-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="6471e-210">Każdy komunikat ma określoną nazwę.</span><span class="sxs-lookup"><span data-stu-id="6471e-210">Each message has a specific name.</span></span> <span data-ttu-id="6471e-211">Na przykład masz wiadomości o nazwie `messageReceived` który wykonywania logiki odpowiada za wyświetlanie nowy komunikat w strefie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6471e-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="6471e-212">Nasłuchiwanie określonego komunikatu może odbywać się za pośrednictwem `on` funkcji.</span><span class="sxs-lookup"><span data-stu-id="6471e-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="6471e-213">Można słuchać dowolną liczbą nazw wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6471e-213">You can listen to any number of message names.</span></span> <span data-ttu-id="6471e-214">Istnieje również możliwość do przekazania parametrów do wiadomości, takie jak nazwisko autora i zawartość odebranego komunikatu.</span><span class="sxs-lookup"><span data-stu-id="6471e-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="6471e-215">Gdy klient odbierze komunikat nowy `div` element jest tworzony z nazwisko autora i komunikat zawartości w jego `innerHTML` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6471e-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="6471e-216">Jest ona dodawana do głównego `div` element wyświetlanie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6471e-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="6471e-217">Teraz, klient może odbierać wiadomości, jest skonfigurowana do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6471e-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="6471e-218">Dodaj wyróżniony kod, aby *src/index.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="6471e-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="6471e-219">Wysyłanie wiadomości przez połączenie Websocket wymaga wywołania `send` metody.</span><span class="sxs-lookup"><span data-stu-id="6471e-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="6471e-220">Pierwszy parametr metody jest nazwą wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6471e-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="6471e-221">Dane komunikatów zamieszkuje innych parametrów.</span><span class="sxs-lookup"><span data-stu-id="6471e-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="6471e-222">W tym przykładzie komunikat zidentyfikowane jako `newMessage` są wysyłane do serwera.</span><span class="sxs-lookup"><span data-stu-id="6471e-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="6471e-223">Komunikat składa się z nazwy użytkownika i dane wejściowe z pola tekstowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6471e-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="6471e-224">Jeśli działa wysyłania, wartość pola tekstowego jest wyczyszczone.</span><span class="sxs-lookup"><span data-stu-id="6471e-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="6471e-225">Dodaj metodę wyróżnione `ChatHub` klasy:</span><span class="sxs-lookup"><span data-stu-id="6471e-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="6471e-226">Poprzedni kod emituje odebranych komunikatów do wszystkich połączonych użytkowników, gdy serwer odbierze je.</span><span class="sxs-lookup"><span data-stu-id="6471e-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="6471e-227">Nie trzeba mieć ogólnego `on` metodę, aby odbierać wszystkie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6471e-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="6471e-228">Metodę o nazwie po sufiksy z nazwy komunikatu.</span><span class="sxs-lookup"><span data-stu-id="6471e-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="6471e-229">W tym przykładzie klient maszynie wysyła komunikat, który został zidentyfikowany jako `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="6471e-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="6471e-230">C# `NewMessage` metoda oczekuje danych wysłanych przez klienta.</span><span class="sxs-lookup"><span data-stu-id="6471e-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="6471e-231">Połączenie jest nawiązywane w przypadku [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metoda [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="6471e-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="6471e-232">Odebrane komunikaty są wysyłane do wszystkich klientów podłączonych do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="6471e-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="6471e-233">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6471e-233">Test the app</span></span>

<span data-ttu-id="6471e-234">Upewnij się, że aplikacja działa z następujących kroków.</span><span class="sxs-lookup"><span data-stu-id="6471e-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6471e-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6471e-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6471e-236">Uruchom Webpack *wersji* tryb.</span><span class="sxs-lookup"><span data-stu-id="6471e-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="6471e-237">Przy użyciu **Konsola Menedżera pakietów** okna, uruchom następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="6471e-237">Using the **Package Manager Console** window, execute the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6471e-238">Wybierz **debugowania** > **uruchomienie bez debugowania** można uruchomić aplikację w przeglądarce bez dołączanie debugera.</span><span class="sxs-lookup"><span data-stu-id="6471e-238">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="6471e-239">*Wwwroot/index.html* plik jest obsługiwany w `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="6471e-239">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="6471e-240">Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="6471e-240">Open another browser instance (any browser).</span></span> <span data-ttu-id="6471e-241">Wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="6471e-241">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6471e-242">Wybierz albo przeglądarkę, wpisz dowolny tekst w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="6471e-242">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6471e-243">Unikatowej nazwy użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="6471e-243">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6471e-244">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6471e-244">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="6471e-245">Uruchom Webpack *wersji* tryb, wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="6471e-245">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="6471e-246">Tworzenie i uruchamianie aplikacji, wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="6471e-246">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="6471e-247">Serwer sieci web uruchamia aplikację i udostępnia ją na hoście lokalnym.</span><span class="sxs-lookup"><span data-stu-id="6471e-247">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="6471e-248">Otwórz w przeglądarce `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="6471e-248">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="6471e-249">*Wwwroot/index.html* plik jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="6471e-249">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="6471e-250">Skopiuj adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="6471e-250">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="6471e-251">Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="6471e-251">Open another browser instance (any browser).</span></span> <span data-ttu-id="6471e-252">Wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="6471e-252">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6471e-253">Wybierz albo przeglądarkę, wpisz dowolny tekst w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="6471e-253">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="6471e-254">Unikatowej nazwy użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="6471e-254">The unique user name and message are displayed on both pages instantly.</span></span>

---

![komunikat wyświetlany w oba okna przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="6471e-256">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6471e-256">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>