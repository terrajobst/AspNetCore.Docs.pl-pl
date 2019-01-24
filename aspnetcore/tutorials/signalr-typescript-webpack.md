---
title: Używanie biblioteki SignalR platformy ASP.NET Core za pomocą TypeScript i Webpack
author: ssougnez
description: W tym samouczku skonfigurujesz Webpack połączyć w paczkę i Utwórz aplikację sieci web biblioteki SignalR platformy ASP.NET Core, którego klient został napisany w TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 8292ab2e0ad1f5c67ac7f15c280b49700f6717ad
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836327"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="161c2-103">Używanie biblioteki SignalR platformy ASP.NET Core za pomocą TypeScript i Webpack</span><span class="sxs-lookup"><span data-stu-id="161c2-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="161c2-104">Przez [Sébastien Sougnez](https://twitter.com/ssougnez) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="161c2-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="161c2-105">[Webpack](https://webpack.js.org/) umożliwia deweloperom połączyć w paczkę i tworzyć zasoby po stronie klienta aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="161c2-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="161c2-106">Ten samouczek pokazuje, za pomocą Webpack w aplikacji sieci web biblioteki SignalR platformy ASP.NET Core, którego klient został napisany w [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="161c2-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="161c2-107">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="161c2-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="161c2-108">Tworzenia szkieletu początkową aplikację biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="161c2-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="161c2-109">Konfigurowanie klienta SignalR TypeScript</span><span class="sxs-lookup"><span data-stu-id="161c2-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="161c2-110">Konfigurowanie potoku kompilacji przy użyciu Webpack</span><span class="sxs-lookup"><span data-stu-id="161c2-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="161c2-111">Konfigurowanie serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="161c2-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="161c2-112">Włącz komunikację między klientem i serwerem</span><span class="sxs-lookup"><span data-stu-id="161c2-112">Enable communication between client and server</span></span>

<span data-ttu-id="161c2-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="161c2-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-vs-vsc-2.2.md)]

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="161c2-114">Tworzenie aplikacji sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="161c2-114">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="161c2-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="161c2-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="161c2-116">Konfigurowanie programu Visual Studio, aby wyszukać pakiety npm w usłudze *ścieżki* zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="161c2-116">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="161c2-117">Domyślnie program Visual Studio korzysta z wersji npm znajduje się w jego katalogu instalacji.</span><span class="sxs-lookup"><span data-stu-id="161c2-117">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="161c2-118">Wykonaj te instrukcje w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="161c2-118">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="161c2-119">Przejdź do **narzędzia** > **opcje** > **projekty i rozwiązania** > **sieci Web zarządzania pakietami**  >  **Zewnętrzne narzędzia sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="161c2-119">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="161c2-120">Wybierz *$(PATH)* wpis na liście.</span><span class="sxs-lookup"><span data-stu-id="161c2-120">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="161c2-121">Kliknij strzałkę w górę, aby przenieść wpis do drugiej pozycji na liście.</span><span class="sxs-lookup"><span data-stu-id="161c2-121">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio Configuration](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="161c2-123">Visual Studio zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="161c2-123">Visual Studio configuration is completed.</span></span> <span data-ttu-id="161c2-124">Nadszedł czas, aby utworzyć projekt.</span><span class="sxs-lookup"><span data-stu-id="161c2-124">It's time to create the project.</span></span>

1. <span data-ttu-id="161c2-125">Użyj **pliku** > **New** > **projektu** menu opcji, a następnie wybierz **aplikacji sieci Web programu ASP.NET Core** szablon.</span><span class="sxs-lookup"><span data-stu-id="161c2-125">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="161c2-126">Nadaj projektowi nazwę *SignalRWebPack*i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="161c2-126">Name the project *SignalRWebPack*, and select **OK**.</span></span>
1. <span data-ttu-id="161c2-127">Wybierz *platformy .NET Core* z listy rozwijanej i wybierz platformę docelową *platformy ASP.NET Core 2.2* z listy rozwijanej selektorów framework.</span><span class="sxs-lookup"><span data-stu-id="161c2-127">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="161c2-128">Wybierz **pusty** szablonu, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="161c2-128">Select the **Empty** template, and select **OK**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="161c2-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="161c2-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="161c2-130">Uruchom następujące polecenie **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="161c2-130">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="161c2-131">Pusta aplikacja internetowa platformy ASP.NET Core, przeznaczonych dla platformy .NET Core jest tworzony w *SignalRWebPack* katalogu.</span><span class="sxs-lookup"><span data-stu-id="161c2-131">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="161c2-132">Konfigurowanie Webpack i TypeScript</span><span class="sxs-lookup"><span data-stu-id="161c2-132">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="161c2-133">Poniższe kroki konfigurowania konwersji TypeScript w kodzie JavaScript i paczki zasoby po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="161c2-133">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="161c2-134">Wykonaj następujące polecenie w katalogu głównym projektu, aby utworzyć *package.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="161c2-134">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="161c2-135">Dodaj wyróżnione właściwość do *package.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="161c2-135">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="161c2-136">Ustawienie `private` właściwość `true` zapobiega ostrzeżeń instalacji pakietu w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="161c2-136">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="161c2-137">Zainstaluj pakiety npm wymagane.</span><span class="sxs-lookup"><span data-stu-id="161c2-137">Install the required npm packages.</span></span> <span data-ttu-id="161c2-138">Wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="161c2-138">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="161c2-139">Niektóre szczegóły polecenia należy pamiętać:</span><span class="sxs-lookup"><span data-stu-id="161c2-139">Some command details to note:</span></span>

    * <span data-ttu-id="161c2-140">Numer wersji jest zgodna `@` podpisywania dla każdej nazwy pakietu.</span><span class="sxs-lookup"><span data-stu-id="161c2-140">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="161c2-141">npm instaluje te wersje określonego pakietu.</span><span class="sxs-lookup"><span data-stu-id="161c2-141">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="161c2-142">`-E` Opcja wyłącza npm domyślne zachowanie dotyczące pisania [wersji semantycznej](https://semver.org/) zakresu operatorom *package.json*.</span><span class="sxs-lookup"><span data-stu-id="161c2-142">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="161c2-143">Na przykład `"webpack": "4.12.0"` jest używana zamiast `"webpack": "^4.12.0"`.</span><span class="sxs-lookup"><span data-stu-id="161c2-143">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="161c2-144">Ta opcja uniemożliwia niezamierzone uaktualnienia do nowszej wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="161c2-144">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="161c2-145">Można znaleźć w oficjalnej [npm install](https://docs.npmjs.com/cli/install) docs, aby uzyskać więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="161c2-145">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="161c2-146">Zastąp `scripts` właściwość *package.json* pliku następującym fragmentem kodu:</span><span class="sxs-lookup"><span data-stu-id="161c2-146">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="161c2-147">Niektórych wyjaśnienie skrypty:</span><span class="sxs-lookup"><span data-stu-id="161c2-147">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="161c2-148">`build`: Razem z zasobami klienta w trybie projektowania, a następnie oczekuje na zmiany w plikach.</span><span class="sxs-lookup"><span data-stu-id="161c2-148">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="161c2-149">Obserwator plików powoduje, że pakiet ponownie wygenerować każdorazowo zmiany w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="161c2-149">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="161c2-150">`mode` Opcja wyłącza optymalizacje, produkcji, takie jak potrząsając drzewa i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="161c2-150">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="161c2-151">Używaj tylko `build` w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="161c2-151">Only use `build` in development.</span></span>
    * <span data-ttu-id="161c2-152">`release`: Zawiera zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="161c2-152">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="161c2-153">`publish`: Przebiegi `release` skrypt, aby powiązać zasoby po stronie klienta w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="161c2-153">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="161c2-154">Wywołuje .NET Core interfejsu wiersza polecenia [publikowania](/dotnet/core/tools/dotnet-publish) polecenie, aby opublikować aplikację.</span><span class="sxs-lookup"><span data-stu-id="161c2-154">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="161c2-155">Utwórz plik o nazwie *webpack.config.js*, w katalogu głównym projektu o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="161c2-155">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="161c2-156">Poprzedni plik konfiguruje kompilacji Webpack.</span><span class="sxs-lookup"><span data-stu-id="161c2-156">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="161c2-157">Niektóre szczegóły konfiguracji należy pamiętać:</span><span class="sxs-lookup"><span data-stu-id="161c2-157">Some configuration details to note:</span></span>

    * <span data-ttu-id="161c2-158">`output` Właściwość zastępuje wartość domyślną *dist*.</span><span class="sxs-lookup"><span data-stu-id="161c2-158">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="161c2-159">Zamiast tego znacznikowej pakietu w *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="161c2-159">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="161c2-160">`resolve.extensions` Tablica zawiera *js* do zaimportowania klienta SignalR JavaScript.</span><span class="sxs-lookup"><span data-stu-id="161c2-160">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="161c2-161">Utwórz nową *src* katalogu w folderze głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="161c2-161">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="161c2-162">Jego celem jest do przechowywania zasobów po stronie klienta projektu.</span><span class="sxs-lookup"><span data-stu-id="161c2-162">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="161c2-163">Tworzenie *src/index.html* o następującej zawartości.</span><span class="sxs-lookup"><span data-stu-id="161c2-163">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="161c2-164">Poprzedni kod HTML definiuje znaczników standardowy na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="161c2-164">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="161c2-165">Utwórz nową *src/css* katalogu.</span><span class="sxs-lookup"><span data-stu-id="161c2-165">Create a new *src/css* directory.</span></span> <span data-ttu-id="161c2-166">Jej celem jest zapisanie projektu *.css* plików.</span><span class="sxs-lookup"><span data-stu-id="161c2-166">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="161c2-167">Tworzenie *src/css/main.css* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="161c2-167">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="161c2-168">Poprzedni *main.css* pliku style aplikacji.</span><span class="sxs-lookup"><span data-stu-id="161c2-168">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="161c2-169">Tworzenie *src/tsconfig.json* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="161c2-169">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="161c2-170">Powyższy kod konfiguruje kompilatora TypeScript, aby wygenerować [ECMAScript](https://wikipedia.org/wiki/ECMAScript) zgodnego z 5 JavaScript.</span><span class="sxs-lookup"><span data-stu-id="161c2-170">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="161c2-171">Tworzenie *src/index.ts* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="161c2-171">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="161c2-172">Poprzedni TypeScript pobiera odwołania do elementów modelu DOM i dołącza dwóch programów obsługi zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="161c2-172">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="161c2-173">`keyup`: To zdarzenie jest uruchamiana, gdy użytkownik wpisze coś w pole tekstowe zidentyfikowane jako `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="161c2-173">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="161c2-174">`send` Funkcja jest wywoływana, gdy użytkownik naciśnie **Enter** klucza.</span><span class="sxs-lookup"><span data-stu-id="161c2-174">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="161c2-175">`click`: To zdarzenie jest uruchamiana, gdy użytkownik kliknie **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="161c2-175">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="161c2-176">`send` Funkcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="161c2-176">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="161c2-177">Konfigurowanie aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="161c2-177">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="161c2-178">Kod zawarty w `Startup.Configure` metoda Wyświetla *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="161c2-178">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="161c2-179">Zastąp `app.Run` wywołanie metody z wywołaniami [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="161c2-179">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="161c2-180">Powyższy kod zezwala serwerowi do lokalizowania i obsługiwać *index.html* pliku, czy użytkownik musi wprowadzić jej pełny adres URL lub adres URL katalogu głównego aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="161c2-180">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="161c2-181">Wywołaj [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) w `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="161c2-181">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="161c2-182">Usługi SignalR dodaje do projektu.</span><span class="sxs-lookup"><span data-stu-id="161c2-182">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="161c2-183">Mapa */hub* kierować do `ChatHub` koncentratora.</span><span class="sxs-lookup"><span data-stu-id="161c2-183">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="161c2-184">Dodaj następujące wiersze na końcu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="161c2-184">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="161c2-185">Utwórz nowy katalog o nazwie *koncentratory*, w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="161c2-185">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="161c2-186">Jego celem jest do przechowywania Centrum SignalR, który jest tworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="161c2-186">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="161c2-187">Utwórz koncentrator *Hubs/ChatHub.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="161c2-187">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="161c2-188">Dodaj następujący kod w górnej części *Startup.cs* plik, aby rozwiązać `ChatHub` odwołania:</span><span class="sxs-lookup"><span data-stu-id="161c2-188">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="161c2-189">Włącz komunikację z klientem i serwerem</span><span class="sxs-lookup"><span data-stu-id="161c2-189">Enable client and server communication</span></span>

<span data-ttu-id="161c2-190">Aplikacja Wyświetla obecnie prosty formularz do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="161c2-190">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="161c2-191">Podczas próby zrobić nic się nie dzieje.</span><span class="sxs-lookup"><span data-stu-id="161c2-191">Nothing happens when you try to do so.</span></span> <span data-ttu-id="161c2-192">Serwer nasłuchuje do określonej trasy, ale nie robi nic za pomocą wysłanych komunikatów.</span><span class="sxs-lookup"><span data-stu-id="161c2-192">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="161c2-193">Wykonaj następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="161c2-193">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="161c2-194">Poprzednie polecenie instaluje [klienta SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), co pozwala klientowi wysłać wiadomości do serwera.</span><span class="sxs-lookup"><span data-stu-id="161c2-194">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="161c2-195">Dodaj wyróżniony kod do *src/index.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="161c2-195">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="161c2-196">Powyższy kod obsługuje odbieranie komunikatów z serwera.</span><span class="sxs-lookup"><span data-stu-id="161c2-196">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="161c2-197">`HubConnectionBuilder` Klasy tworzy nowy konstruktor do skonfigurowania połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="161c2-197">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="161c2-198">`withUrl` Funkcja konfiguruje adres URL koncentratora.</span><span class="sxs-lookup"><span data-stu-id="161c2-198">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="161c2-199">SignalR umożliwia wymianę wiadomości między klientem a serwerem.</span><span class="sxs-lookup"><span data-stu-id="161c2-199">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="161c2-200">Każdy komunikat ma określoną nazwę.</span><span class="sxs-lookup"><span data-stu-id="161c2-200">Each message has a specific name.</span></span> <span data-ttu-id="161c2-201">Na przykład masz wiadomości o nazwie `messageReceived` , wykonana logiki odpowiada za wyświetlanie nowy komunikat w strefie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="161c2-201">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="161c2-202">Nasłuchiwanie szczegółowy komunikat o błędzie może odbywać się za pośrednictwem `on` funkcji.</span><span class="sxs-lookup"><span data-stu-id="161c2-202">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="161c2-203">Można słuchać dowolną liczbę nazw wiadomości.</span><span class="sxs-lookup"><span data-stu-id="161c2-203">You can listen to any number of message names.</span></span> <span data-ttu-id="161c2-204">Istnieje również możliwość przekazania parametrów do wiadomości, takie jak imię i nazwisko autora i zawartość odebrany komunikat.</span><span class="sxs-lookup"><span data-stu-id="161c2-204">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="161c2-205">Gdy klient odbierze komunikat nową `div` zostanie utworzony element o nazwie autora i komunikat zawartość w jego `innerHTML` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="161c2-205">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="161c2-206">Jest ona dodawana do głównej `div` element wyświetlania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="161c2-206">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="161c2-207">Teraz, gdy klient może otrzymać komunikat, należy skonfigurować go do wysyłania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="161c2-207">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="161c2-208">Dodaj wyróżniony kod do *src/index.ts* pliku:</span><span class="sxs-lookup"><span data-stu-id="161c2-208">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="161c2-209">Wysyłanie wiadomości przez połączenie Websocket wymaga wywołania `send` metody.</span><span class="sxs-lookup"><span data-stu-id="161c2-209">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="161c2-210">Pierwszy parametr metody jest nazwa komunikatu.</span><span class="sxs-lookup"><span data-stu-id="161c2-210">The method's first parameter is the message name.</span></span> <span data-ttu-id="161c2-211">Dane wiadomości zamieszkuje innych parametrów.</span><span class="sxs-lookup"><span data-stu-id="161c2-211">The message data inhabits the other parameters.</span></span> <span data-ttu-id="161c2-212">W tym przykładzie wiadomość zidentyfikowane jako `newMessage` są wysyłane do serwera.</span><span class="sxs-lookup"><span data-stu-id="161c2-212">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="161c2-213">Komunikat składa się z nazwy użytkownika i dane wejściowe z pola tekstowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="161c2-213">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="161c2-214">Jeśli działa wysyłania, wartość pola tekstowego jest wyczyszczone.</span><span class="sxs-lookup"><span data-stu-id="161c2-214">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="161c2-215">Dodaj metodę wyróżnione `ChatHub` klasy:</span><span class="sxs-lookup"><span data-stu-id="161c2-215">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="161c2-216">Powyższy kod emituje Odebrane komunikaty do wszystkich połączonych użytkowników, gdy serwer otrzymuje je.</span><span class="sxs-lookup"><span data-stu-id="161c2-216">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="161c2-217">Nie jest konieczne zapewnienie ogólnej `on` metodę, aby otrzymywać wszystkie komunikaty.</span><span class="sxs-lookup"><span data-stu-id="161c2-217">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="161c2-218">Metodę o nazwie po sufiksy nazwa komunikatu.</span><span class="sxs-lookup"><span data-stu-id="161c2-218">A method named after the message name suffices.</span></span>

    <span data-ttu-id="161c2-219">W tym przykładzie klient TypeScript wysyła komunikat, który został zidentyfikowany jako `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="161c2-219">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="161c2-220">C# `NewMessage` metoda oczekuje, że dane wysłane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="161c2-220">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="161c2-221">Połączenie jest nawiązywane w przypadku [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metody [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="161c2-221">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="161c2-222">Odebrane komunikaty są wysyłane do wszystkich klientów podłączone do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="161c2-222">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="161c2-223">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="161c2-223">Test the app</span></span>

<span data-ttu-id="161c2-224">Upewnij się, że aplikacja działa wykonując następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="161c2-224">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="161c2-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="161c2-225">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="161c2-226">Uruchamianie Webpack w *wersji* trybu.</span><span class="sxs-lookup"><span data-stu-id="161c2-226">Run Webpack in *release* mode.</span></span> <span data-ttu-id="161c2-227">Za pomocą **Konsola Menedżera pakietów** okna, wykonaj następujące polecenie w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="161c2-227">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="161c2-228">Jeśli nie jesteś w katalogu głównym projektu, wprowadź `cd SignalRWebPack` przed wpisaniem polecenia.</span><span class="sxs-lookup"><span data-stu-id="161c2-228">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="161c2-229">Wybierz **debugowania** > **Uruchom bez debugowania** do uruchomienia aplikacji w przeglądarce, bez dołączania debugera.</span><span class="sxs-lookup"><span data-stu-id="161c2-229">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="161c2-230">*Wwwroot/index.html* plików jest obsługiwany w `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="161c2-230">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="161c2-231">Otwórz inne wystąpienie przeglądarki (w dowolnej przeglądarce).</span><span class="sxs-lookup"><span data-stu-id="161c2-231">Open another browser instance (any browser).</span></span> <span data-ttu-id="161c2-232">Wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="161c2-232">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="161c2-233">Wybierz albo przeglądarki, wpisz coś w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="161c2-233">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="161c2-234">Unikatowa nazwa użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="161c2-234">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="161c2-235">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="161c2-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="161c2-236">Uruchamianie Webpack w *wersji* trybu, wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="161c2-236">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="161c2-237">Tworzenie i uruchamianie aplikacji, wykonując następujące polecenie w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="161c2-237">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="161c2-238">Serwer sieci web uruchamia aplikację i udostępnia je na hoście lokalnym.</span><span class="sxs-lookup"><span data-stu-id="161c2-238">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="161c2-239">Otwórz w przeglądarce `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="161c2-239">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="161c2-240">*Wwwroot/index.html* plików jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="161c2-240">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="161c2-241">Skopiuj adres URL z paska adresu.</span><span class="sxs-lookup"><span data-stu-id="161c2-241">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="161c2-242">Otwórz inne wystąpienie przeglądarki (w dowolnej przeglądarce).</span><span class="sxs-lookup"><span data-stu-id="161c2-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="161c2-243">Wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="161c2-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="161c2-244">Wybierz albo przeglądarki, wpisz coś w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="161c2-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="161c2-245">Unikatowa nazwa użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="161c2-245">The unique user name and message are displayed on both pages instantly.</span></span>

---

![komunikat wyświetlany w oba okna przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="161c2-247">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="161c2-247">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
