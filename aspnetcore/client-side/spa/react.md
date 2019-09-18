---
title: Użyj szablonu projektu reagowanie z ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak rozpocząć pracę z szablonem projektu aplikacji jednostronicowej (SPA) ASP.NET Core na potrzeby reakcji i tworzenia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: spa/react
ms.openlocfilehash: 0e61c5b3e31a0b050d356b8f8e16306dc1e2a7f3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080415"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="c05af-103">Użyj szablonu projektu reagowanie z ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c05af-103">Use the React project template with ASP.NET Core</span></span>

<span data-ttu-id="c05af-104">Zaktualizowany szablon projektu dotyczącego reakcji umożliwia korzystanie z dogodnych punktów początkowych dla ASP.NET Core aplikacji przy użyciu konwencji reagowania i [tworzenia aplikacji](https://github.com/facebookincubator/create-react-app) (CRA) w celu zaimplementowania rozbudowanego interfejsu użytkownika po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c05af-104">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="c05af-105">Szablon jest równoznaczny z tworzeniem projektu ASP.NET Core, który działa jako zaplecze interfejsu API, a standardowy projekt reakcji CRA reaguje na działanie jako interfejs użytkownika, ale z wygodą hostingu zarówno w jednym projekcie aplikacji, który można skompilować i opublikować jako pojedynczą jednostkę.</span><span class="sxs-lookup"><span data-stu-id="c05af-105">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="c05af-106">Tworzenie nowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="c05af-106">Create a new app</span></span>

<span data-ttu-id="c05af-107">Jeśli zainstalowano ASP.NET Core 2,1, nie ma potrzeby instalowania szablonu projektu z reagowaniem.</span><span class="sxs-lookup"><span data-stu-id="c05af-107">If you have ASP.NET Core 2.1 installed, there's no need to install the React project template.</span></span>

<span data-ttu-id="c05af-108">Utwórz nowy projekt z wiersza polecenia przy użyciu polecenia `dotnet new react` w pustym katalogu.</span><span class="sxs-lookup"><span data-stu-id="c05af-108">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="c05af-109">Na przykład następujące polecenia tworzą aplikację w katalogu *My-New-App* i przełączają się do tego katalogu:</span><span class="sxs-lookup"><span data-stu-id="c05af-109">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```dotnetcli
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="c05af-110">Uruchom aplikację z poziomu programu Visual Studio lub interfejs wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="c05af-110">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c05af-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c05af-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c05af-112">Otwórz wygenerowany plik *csproj* i uruchom aplikację w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="c05af-112">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="c05af-113">Proces kompilacji przywraca zależności npm w pierwszym przebiegu, co może potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="c05af-113">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="c05af-114">Kolejne kompilacje są znacznie szybsze.</span><span class="sxs-lookup"><span data-stu-id="c05af-114">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c05af-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c05af-115">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c05af-116">Upewnij się, że masz zmienną środowiskową o `ASPNETCORE_Environment` nazwie `Development`z wartością.</span><span class="sxs-lookup"><span data-stu-id="c05af-116">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="c05af-117">W systemie Windows (w komunikatach innych niż programu PowerShell `SET ASPNETCORE_Environment=Development`) Uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="c05af-117">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="c05af-118">W systemie Linux lub macOS Uruchom `export ASPNETCORE_Environment=Development`polecenie.</span><span class="sxs-lookup"><span data-stu-id="c05af-118">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="c05af-119">Uruchom [kompilację dotnet](/dotnet/core/tools/dotnet-build) , aby sprawdzić, czy aplikacja jest poprawnie skompilowana.</span><span class="sxs-lookup"><span data-stu-id="c05af-119">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="c05af-120">W pierwszym uruchomieniu proces kompilacji przywraca zależności npm, co może potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="c05af-120">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="c05af-121">Kolejne kompilacje są znacznie szybsze.</span><span class="sxs-lookup"><span data-stu-id="c05af-121">Subsequent builds are much faster.</span></span>

<span data-ttu-id="c05af-122">Uruchom [uruchomienie programu dotnet](/dotnet/core/tools/dotnet-run) , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="c05af-122">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="c05af-123">Szablon projektu tworzy aplikację ASP.NET Core i aplikację do reagowania.</span><span class="sxs-lookup"><span data-stu-id="c05af-123">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="c05af-124">Aplikacja ASP.NET Core jest przeznaczona do użycia na potrzeby dostępu do danych, autoryzacji i innych zagadnień po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="c05af-124">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="c05af-125">Aplikacja do reagowania znajdująca się w podkatalogu *ClientApp* jest przeznaczona do użycia w przypadku wszystkich problemów z interfejsem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c05af-125">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="c05af-126">Dodawanie stron, obrazów, stylów, modułów itp.</span><span class="sxs-lookup"><span data-stu-id="c05af-126">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="c05af-127">Katalog *ClientApp* jest standardową aplikacją do reagowania w CRA.</span><span class="sxs-lookup"><span data-stu-id="c05af-127">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="c05af-128">Aby uzyskać więcej informacji, zobacz oficjalną [dokumentację CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) .</span><span class="sxs-lookup"><span data-stu-id="c05af-128">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="c05af-129">Istnieją niewielkie różnice między aplikacją reakcji utworzoną przez ten szablon a tym utworzonym przez CRA. jednak możliwości aplikacji nie są zmieniane.</span><span class="sxs-lookup"><span data-stu-id="c05af-129">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="c05af-130">Aplikacja utworzona przez szablon zawiera układ oparty na [Bootstrap](https://getbootstrap.com/)i podstawowy przykład routingu.</span><span class="sxs-lookup"><span data-stu-id="c05af-130">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="c05af-131">Zainstaluj pakiety npm</span><span class="sxs-lookup"><span data-stu-id="c05af-131">Install npm packages</span></span>

<span data-ttu-id="c05af-132">Aby zainstalować pakiety npm innych firm, należy użyć wiersza polecenia w podkatalogu *ClientApp* .</span><span class="sxs-lookup"><span data-stu-id="c05af-132">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="c05af-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c05af-133">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="c05af-134">Publikowanie i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="c05af-134">Publish and deploy</span></span>

<span data-ttu-id="c05af-135">W trakcie opracowywania aplikacja jest uruchamiana w trybie zoptymalizowanym pod kątem wygody dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="c05af-135">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="c05af-136">Na przykład pakiety JavaScript zawierają mapy źródłowe (w przypadku debugowania można zobaczyć oryginalny kod źródłowy).</span><span class="sxs-lookup"><span data-stu-id="c05af-136">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="c05af-137">Aplikacja obserwuje zmiany plików JavaScript, HTML i CSS na dysku, a następnie automatycznie ponownie kompiluje i ładuje je, gdy zobaczy zmiany tych plików.</span><span class="sxs-lookup"><span data-stu-id="c05af-137">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="c05af-138">W środowisku produkcyjnym można korzystać z wersji aplikacji zoptymalizowanej pod kątem wydajności.</span><span class="sxs-lookup"><span data-stu-id="c05af-138">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="c05af-139">To jest skonfigurowane tak, aby były wykonywane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c05af-139">This is configured to happen automatically.</span></span> <span data-ttu-id="c05af-140">Podczas publikowania, konfiguracja kompilacji emituje zminimalizowanego, przenoszącą wbudowaną kompilację kodu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c05af-140">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="c05af-141">W przeciwieństwie do kompilacji deweloperskiej kompilacja produkcyjna nie wymaga zainstalowania na serwerze programu Node. js.</span><span class="sxs-lookup"><span data-stu-id="c05af-141">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="c05af-142">Można użyć standardowych [ASP.NET Core metod hostingu i wdrażania](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="c05af-142">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="c05af-143">Uruchom serwer CRA niezależnie</span><span class="sxs-lookup"><span data-stu-id="c05af-143">Run the CRA server independently</span></span>

<span data-ttu-id="c05af-144">Projekt jest skonfigurowany tak, aby uruchamiał własne wystąpienie serwera CRA Development w tle podczas uruchamiania aplikacji ASP.NET Core w trybie tworzenia.</span><span class="sxs-lookup"><span data-stu-id="c05af-144">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="c05af-145">Jest to wygodne, ponieważ oznacza to, że nie trzeba ręcznie uruchamiać oddzielnego serwera.</span><span class="sxs-lookup"><span data-stu-id="c05af-145">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="c05af-146">Istnieje zwrot do tej konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="c05af-146">There's a drawback to this default setup.</span></span> <span data-ttu-id="c05af-147">Za każdym razem, gdy C# modyfikujesz kod, a aplikacja ASP.NET Core wymaga ponownego uruchomienia, serwer CRA zostanie uruchomiony ponownie.</span><span class="sxs-lookup"><span data-stu-id="c05af-147">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="c05af-148">Aby rozpocząć tworzenie kopii zapasowej, wymagane są kilka sekund.</span><span class="sxs-lookup"><span data-stu-id="c05af-148">A few seconds are required to start back up.</span></span> <span data-ttu-id="c05af-149">Jeśli wprowadzasz częste C# zmiany kodu i nie chcesz czekać na ponowne uruchomienie serwera CRA, uruchom serwer CRA na zewnątrz niezależnie od procesu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c05af-149">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="c05af-150">Aby to zrobić:</span><span class="sxs-lookup"><span data-stu-id="c05af-150">To do so:</span></span>

1. <span data-ttu-id="c05af-151">Dodaj plik *ENV* do podkatalogu *ClientApp* przy użyciu następującego ustawienia:</span><span class="sxs-lookup"><span data-stu-id="c05af-151">Add a *.env* file to the *ClientApp* subdirectory with the following setting:</span></span>

    ```
    BROWSER=none
    ```

    <span data-ttu-id="c05af-152">Uniemożliwi to otwieranie przeglądarki sieci Web podczas zewnętrznego uruchamiania serwera CRA.</span><span class="sxs-lookup"><span data-stu-id="c05af-152">This will prevent your web browser from opening when starting the CRA server externally.</span></span>

2. <span data-ttu-id="c05af-153">W wierszu polecenia przejdź do podkatalogu *ClientApp* i uruchom serwer deweloperski CRA:</span><span class="sxs-lookup"><span data-stu-id="c05af-153">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

3. <span data-ttu-id="c05af-154">Zmodyfikuj aplikację ASP.NET Core tak, aby korzystała z zewnętrznego wystąpienia serwera CRA, zamiast uruchamiania jednego z nich.</span><span class="sxs-lookup"><span data-stu-id="c05af-154">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="c05af-155">W klasie *uruchomieniowej* Zastąp `spa.UseReactDevelopmentServer` wywołanie następującym:</span><span class="sxs-lookup"><span data-stu-id="c05af-155">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="c05af-156">Po uruchomieniu aplikacji ASP.NET Core nie zostanie uruchomiony serwer CRA.</span><span class="sxs-lookup"><span data-stu-id="c05af-156">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="c05af-157">Wystąpienie, które zostało uruchomione ręcznie, jest używane w zamian.</span><span class="sxs-lookup"><span data-stu-id="c05af-157">The instance you started manually is used instead.</span></span> <span data-ttu-id="c05af-158">Pozwala to na szybkie uruchamianie i ponowne uruchamianie.</span><span class="sxs-lookup"><span data-stu-id="c05af-158">This enables it to start and restart faster.</span></span> <span data-ttu-id="c05af-159">Nie czeka już na odbudowanie aplikacji z reagowaniem za każdym razem.</span><span class="sxs-lookup"><span data-stu-id="c05af-159">It's no longer waiting for your React app to rebuild each time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c05af-160">"Renderowanie po stronie serwera" nie jest obsługiwaną funkcją tego szablonu.</span><span class="sxs-lookup"><span data-stu-id="c05af-160">"Server-side rendering" is not a supported feature of this template.</span></span> <span data-ttu-id="c05af-161">Naszym celem tego szablonu jest spełnienie warunków dotyczących "Tworzenie — reagowanie aplikacji".</span><span class="sxs-lookup"><span data-stu-id="c05af-161">Our goal with this template is to meet parity with "create-react-app".</span></span> <span data-ttu-id="c05af-162">W związku z tym scenariusze i funkcje, które nie są uwzględnione w projekcie "Create-rereagować na aplikacje" (na przykład SSR), nie są obsługiwane i pozostają jako ćwiczenie dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c05af-162">As such, scenarios and features not included in a "create-react-app" project (such as SSR) are not supported and are left as an exercise for the user.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c05af-163">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c05af-163">Additional resources</span></span>

* <xref:security/authentication/identity/spa>
