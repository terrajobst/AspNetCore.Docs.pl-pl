---
title: Szablon projektu platformy React za pomocą platformy ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak rozpocząć pracę przy użyciu szablonu projektu ASP.NET Core jednej strony aplikacji (SPA) dla platformy React i utworzyć react aplikacji.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react
ms.openlocfilehash: d83bff8abcd5b59d8bc4a51a101510755394f0c4
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667690"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="8580c-103">Szablon projektu platformy React za pomocą platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8580c-103">Use the React project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="8580c-104">Ta dokumentacja nie jest o szablon projektu platformy React zawarty w ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="8580c-104">This documentation isn't about the React project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="8580c-105">Chodzi o nowszych szablonu platformy React, do której można zaktualizować ręcznie.</span><span class="sxs-lookup"><span data-stu-id="8580c-105">It's about the newer React template to which you can update manually.</span></span> <span data-ttu-id="8580c-106">Szablon jest domyślnie w programie ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8580c-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="8580c-107">Zaktualizowany szablon projektu platformy React udostępnia dogodny punkt początkowy dla platformy ASP.NET Core z aplikacji przy użyciu platformy React i [tworzenie platformy react aplikacji](https://github.com/facebookincubator/create-react-app) konwencje (CRA), aby zaimplementować interfejs rozbudowane, po stronie klienta użytkownika (UI).</span><span class="sxs-lookup"><span data-stu-id="8580c-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="8580c-108">Szablon jest odpowiednikiem Tworzenie projektu ASP.NET Core jako zaplecza interfejsu API i standardowy projekt kanadyjskiej administracji podatkowej reagować na działanie jako interfejsu użytkownika, ale zapewniające wygodę hostingu, zarówno w projekcie pojedynczej aplikacji, który może być skompilowane i opublikowane jako pojedyncza jednostka.</span><span class="sxs-lookup"><span data-stu-id="8580c-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="8580c-109">Tworzenie nowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="8580c-109">Create a new app</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8580c-110">Jeśli używasz programu ASP.NET Core 2.0, upewnij się, masz [zainstalowany zaktualizowany szablon projektu platformy React](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="8580c-110">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8580c-111">Jeśli masz zainstalowany program ASP.NET Core 2.1, nie ma potrzeby instalowania szablon projektu platformy React.</span><span class="sxs-lookup"><span data-stu-id="8580c-111">If you have ASP.NET Core 2.1 installed, there's no need to install the React project template.</span></span>

::: moniker-end

<span data-ttu-id="8580c-112">Utwórz nowy projekt z wiersza polecenia za pomocą polecenia `dotnet new react` w pustym katalogu.</span><span class="sxs-lookup"><span data-stu-id="8580c-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="8580c-113">Na przykład, następujące polecenia tworzą aplikację w *Moje aplikacyjnego nowe* katalogu i przełącz się do tego katalogu:</span><span class="sxs-lookup"><span data-stu-id="8580c-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="8580c-114">Uruchom aplikację z programu Visual Studio lub platformy .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="8580c-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8580c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8580c-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8580c-116">Otwórz wygenerowany *.csproj* pliku i uruchom aplikację w zwykły z tego miejsca.</span><span class="sxs-lookup"><span data-stu-id="8580c-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="8580c-117">Proces kompilacji przywraca zależności rozwiązania npm przy pierwszym uruchomieniu może potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="8580c-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="8580c-118">Kolejne kompilacje są znacznie szybciej.</span><span class="sxs-lookup"><span data-stu-id="8580c-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8580c-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8580c-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8580c-120">Upewnij się, że zmienna środowiskowa o nazwie `ASPNETCORE_Environment` wartością `Development`.</span><span class="sxs-lookup"><span data-stu-id="8580c-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="8580c-121">Na Windows (w monity-PowerShell), uruchom `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="8580c-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="8580c-122">W systemie Linux lub macOS Uruchom `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="8580c-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="8580c-123">Uruchom [kompilacji dotnet](/dotnet/core/tools/dotnet-build) Aby sprawdzić, aplikacja poprawnie się kompiluje.</span><span class="sxs-lookup"><span data-stu-id="8580c-123">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="8580c-124">Przy pierwszym uruchomieniu procesu kompilacji przywraca zależności rozwiązania npm, co może zająć kilka minut.</span><span class="sxs-lookup"><span data-stu-id="8580c-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="8580c-125">Kolejne kompilacje są znacznie szybciej.</span><span class="sxs-lookup"><span data-stu-id="8580c-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="8580c-126">Uruchom [dotnet, uruchom](/dotnet/core/tools/dotnet-run) Aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="8580c-126">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="8580c-127">Szablon projektu umożliwia utworzenie aplikacji platformy ASP.NET Core i aplikacji w języku React.</span><span class="sxs-lookup"><span data-stu-id="8580c-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="8580c-128">Aplikacja platformy ASP.NET Core jest przeznaczona do użytku z dostępem do danych, autoryzację i inne problemy po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="8580c-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="8580c-129">Aplikacja platformy React, znajdującej się w *ClientApp* podkatalogu, jest przeznaczona do użycia dla wszystkich obaw interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8580c-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="8580c-130">Dodawanie stron, obrazów, style, moduły, itp.</span><span class="sxs-lookup"><span data-stu-id="8580c-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="8580c-131">*ClientApp* katalog jest standardowa aplikacja kanadyjskiej administracji podatkowej reagować.</span><span class="sxs-lookup"><span data-stu-id="8580c-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="8580c-132">Można znaleźć w oficjalnej [dokumentacji kanadyjskiej administracji podatkowej](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8580c-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="8580c-133">Istnieją drobne różnice między aplikacją platformy React, utworzony za pomocą tego szablonu i utworzonym przez kanadyjskiej administracji podatkowej. jednak możliwości aplikacji nie uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="8580c-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="8580c-134">Aplikacja utworzona przez szablon zawiera [Bootstrap](https://getbootstrap.com/)— na podstawie układu oraz przykład podstawowej routingu.</span><span class="sxs-lookup"><span data-stu-id="8580c-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="8580c-135">Instalowanie pakietów npm</span><span class="sxs-lookup"><span data-stu-id="8580c-135">Install npm packages</span></span>

<span data-ttu-id="8580c-136">Aby zainstalować pakiety npm innych firm, należy użyć wiersza polecenia w *ClientApp* podkatalogu.</span><span class="sxs-lookup"><span data-stu-id="8580c-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="8580c-137">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8580c-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="8580c-138">Publikowanie i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="8580c-138">Publish and deploy</span></span>

<span data-ttu-id="8580c-139">Na etapie opracowywania aplikacji jest uruchamiany w trybie zoptymalizowane pod kątem dla wygody deweloperów.</span><span class="sxs-lookup"><span data-stu-id="8580c-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="8580c-140">Na przykład pakiety języka JavaScript obejmują map źródeł (tak, aby podczas debugowania, możesz zobaczyć oryginalnego kodu źródłowego).</span><span class="sxs-lookup"><span data-stu-id="8580c-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="8580c-141">Aplikacja oczekuje JavaScript, HTML i CSS, zmiany plików na dysku i automatycznie następuje rekompilacja i ponowne załadowanie po wykryciu tych plików, zmiany.</span><span class="sxs-lookup"><span data-stu-id="8580c-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="8580c-142">W środowisku produkcyjnym obsługiwać wersję aplikacji, która jest zoptymalizowana pod kątem wydajności.</span><span class="sxs-lookup"><span data-stu-id="8580c-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="8580c-143">Jest ona konfigurowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="8580c-143">This is configured to happen automatically.</span></span> <span data-ttu-id="8580c-144">Podczas publikowania, konfigurację kompilacji emituje zminimalizowany, kompilacja transpiled kodu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8580c-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="8580c-145">W przeciwieństwie do tworzenia kompilacji kompilacja w produkcji nie wymaga środowiska Node.js można zainstalować na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8580c-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="8580c-146">Można użyć standardowego [metody hosting i wdrażanie platformy ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="8580c-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="8580c-147">Uruchom serwer kanadyjskiej administracji podatkowej niezależnie</span><span class="sxs-lookup"><span data-stu-id="8580c-147">Run the CRA server independently</span></span>

<span data-ttu-id="8580c-148">Projekt jest skonfigurowana do uruchamiania wystąpienia serwera wdrożeniowego programu kanadyjskiej administracji podatkowej w tle, gdy aplikacja platformy ASP.NET Core jest uruchamiany w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="8580c-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="8580c-149">Jest to wygodne, ponieważ oznacza to, że nie trzeba ręcznie uruchomić oddzielny serwer.</span><span class="sxs-lookup"><span data-stu-id="8580c-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="8580c-150">Brak wadą tego ustawienia domyślnego.</span><span class="sxs-lookup"><span data-stu-id="8580c-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="8580c-151">Po każdej zmianie kodu C# i ASP.NET Core aplikacja wymaga ponownego uruchomienia, serwer kanadyjskiej administracji podatkowej zostanie ponownie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="8580c-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="8580c-152">Kilka sekund, należy uruchomić tworzenie kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="8580c-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="8580c-153">Jeśli wprowadzasz częste C# kodu edycji i nie chcesz czekać, aż serwer kanadyjskiej administracji podatkowej ponownie uruchomić, uruchom serwer kanadyjskiej administracji podatkowej zewnętrznie, niezależnie od procesu, ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8580c-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="8580c-154">Aby to zrobić:</span><span class="sxs-lookup"><span data-stu-id="8580c-154">To do so:</span></span>

1. <span data-ttu-id="8580c-155">Dodaj *ENV* plik *ClientApp* podkatalogu z następującymi ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="8580c-155">Add a *.env* file to the *ClientApp* subdirectory with the following setting:</span></span>

    ```
    BROWSER=none
    ```
    
    <span data-ttu-id="8580c-156">Uniemożliwi to przeglądarki sieci web otwierania podczas uruchamiania serwera kanadyjskiej administracji podatkowej zewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="8580c-156">This will prevent your web browser from opening when starting the CRA server externally.</span></span>

2. <span data-ttu-id="8580c-157">W wierszu polecenia przejdź do *ClientApp* podkatalogu, a następnie uruchom serwera wdrożeniowego programu kanadyjskiej administracji podatkowej:</span><span class="sxs-lookup"><span data-stu-id="8580c-157">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

3. <span data-ttu-id="8580c-158">Zmodyfikować aplikację platformy ASP.NET Core w taki sposób, aby użyć wystąpienia zewnętrznego serwera kanadyjskiej administracji podatkowej zamiast uruchamiania jednego z własnych.</span><span class="sxs-lookup"><span data-stu-id="8580c-158">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="8580c-159">W swojej *uruchamiania* klasy, Zastąp `spa.UseReactDevelopmentServer` wywołania następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8580c-159">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="8580c-160">Po uruchomieniu aplikacji platformy ASP.NET Core, nie będzie on uruchomienia serwera kanadyjskiej administracji podatkowej.</span><span class="sxs-lookup"><span data-stu-id="8580c-160">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="8580c-161">Wystąpienie, które można uruchomić ręcznie jest używana zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="8580c-161">The instance you started manually is used instead.</span></span> <span data-ttu-id="8580c-162">To umożliwia uruchamianie i ponowne uruchamianie szybciej.</span><span class="sxs-lookup"><span data-stu-id="8580c-162">This enables it to start and restart faster.</span></span> <span data-ttu-id="8580c-163">Oczekuje się już na aplikację platformy React, aby odbudować każdorazowo.</span><span class="sxs-lookup"><span data-stu-id="8580c-163">It's no longer waiting for your React app to rebuild each time.</span></span>
