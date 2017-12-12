---
title: "Kompilowanie projektów z narzędzia Yeoman w ASP.NET Core"
author: spboyer
description: "W tym artykule przedstawiono tworzenie aplikacji sieci web przy użyciu narzędzia Yeoman platformy ASP.NET Core generator na macOS."
keywords: "Platformy ASP.NET Core, narzędzia Yeoman, Cross Platform yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="3274d-104">Wprowadzenie do tworzenia projektów z narzędzia Yeoman w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3274d-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="3274d-105">[Narzędzia yeoman](http://yeoman.io/) jest system szkieletów projektu do tworzenia wielu rodzajów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3274d-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="3274d-106">Narzędzia Yeoman generator dla platformy ASP.NET Core zawiera różne szablony projektu do uruchomienia nowej sieci web, MVC i aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="3274d-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="3274d-107">Instalowanie środowiska Node.js, npm i narzędzia Yeoman</span><span class="sxs-lookup"><span data-stu-id="3274d-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="3274d-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3274d-108">Prerequisites</span></span>

<span data-ttu-id="3274d-109">Node.js i npm są wymagane dla narzędzia Yeoman.</span><span class="sxs-lookup"><span data-stu-id="3274d-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="3274d-110">Pobierz z [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="3274d-110">Download from [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="3274d-111">Instalator zawiera [Node.js](https://nodejs.org/) i [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="3274d-111">The installer includes [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="3274d-112">Bower jest również wymagany do zainstalowania platformy interfejsu użytkownika, takie jak ładowania początkowego.</span><span class="sxs-lookup"><span data-stu-id="3274d-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="3274d-113">Aby zainstalować narzędzia Yeoman i Bower, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3274d-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="3274d-114">Jeśli zostanie wyświetlony błąd `npm ERR! Please try running this command again as root/Administrator.` na macOS, uruchom następujące polecenia przy użyciu [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="3274d-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="3274d-115">W wierszu polecenia należy zainstalować generatora platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3274d-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="3274d-116">Jeśli zostanie wyświetlony błąd uprawnień, uruchom polecenie w obszarze `sudo` zgodnie z powyższym opisem.</span><span class="sxs-lookup"><span data-stu-id="3274d-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="3274d-117">`–g` Flagi instaluje globalnie, generator, dzięki czemu można używać z dowolną ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="3274d-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="3274d-118">Tworzenie aplikacji platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3274d-118">Create an ASP.NET app</span></span>

<span data-ttu-id="3274d-119">Uruchom generatora platformy ASP.NET opartych na narzędzia Yeoman:</span><span class="sxs-lookup"><span data-stu-id="3274d-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="3274d-120">Generator Wyświetla menu.</span><span class="sxs-lookup"><span data-stu-id="3274d-120">The generator displays a menu.</span></span> <span data-ttu-id="3274d-121">Strzałka w dół do **sieci Web aplikacji podstawowe [bez członkostwa i autoryzacji]** projekt i wybierz **Enter**:</span><span class="sxs-lookup"><span data-stu-id="3274d-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![Okno polecenia: typ aplikacji, czy chcesz utworzyć?](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="3274d-124">Wybierz Bootstrap jako platforma interfejsu użytkownika, a następnie wybierz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="3274d-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="3274d-125">Użyj "**MyWebApp**" dla nazwy aplikacji, a następnie naciśnij przycisk **Enter**.</span><span class="sxs-lookup"><span data-stu-id="3274d-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="3274d-126">Narzędzia yeoman będzie szkieletu projektu i jego pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="3274d-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="3274d-127">W formularzu polecenia podawane są również Sugerowane następne kroki.</span><span class="sxs-lookup"><span data-stu-id="3274d-127">Suggested next steps are also provided in the form of commands.</span></span>

![Okno polecenia: co to jest nazwa aplikacji ASP.NET?](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="3274d-130">[ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) tworzy platformy ASP.NET Core projektów, które mogą być ładowane do programu Visual Studio Code, Visual Studio lub uruchom w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="3274d-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="3274d-131">Przywracanie, tworzenie i uruchamianie</span><span class="sxs-lookup"><span data-stu-id="3274d-131">Restore, build, and run</span></span>

<span data-ttu-id="3274d-132">Wykonaj polecenia sugerowane zmieniając katalogi do `MyWebApp` katalogu.</span><span class="sxs-lookup"><span data-stu-id="3274d-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="3274d-133">Następnie uruchom `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="3274d-133">Then run `dotnet restore`.</span></span>

![okno Polecenie](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="3274d-135">Tworzenie i uruchamianie aplikacji w programie `dotnet build` i `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="3274d-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![okno Polecenie](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="3274d-137">W tym momencie można przejść do adresu URL do przetestowania aplikacji platformy ASP.NET Core nowo utworzony.</span><span class="sxs-lookup"><span data-stu-id="3274d-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="3274d-138">Pakiety po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="3274d-138">Client-side packages</span></span>

<span data-ttu-id="3274d-139">Frontonu zasoby są dostarczane przez Szablony z narzędzia Yeoman za pomocą generatora [Bower](xref:client-side/bower) Menedżera pakietów po stronie klienta, dodawanie *bower.json* i *.bowerrc* pliki pod kątem przywracania pakietów po stronie klienta za pomocą rozwiązania Bower.</span><span class="sxs-lookup"><span data-stu-id="3274d-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="3274d-140">[BundlerMinifier](xref:client-side/bundling-and-minification) składnika dołączony jest również domyślnie łatwość łączenia (grupowania) i minimalizację CSS, JavaScript i HTML.</span><span class="sxs-lookup"><span data-stu-id="3274d-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="3274d-141">Tworzenie i uruchamianie z programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3274d-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="3274d-142">Możesz załadować wygenerowanego projektu sieci web platformy ASP.NET Core bezpośrednio w programie Visual Studio, a następnie skompilować i uruchomić projekt w.</span><span class="sxs-lookup"><span data-stu-id="3274d-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="3274d-143">Postępuj zgodnie z powyższymi instrukcjami, aby utworzyć szkielet nowej aplikacji platformy ASP.NET Core za pomocą narzędzia Yeoman.</span><span class="sxs-lookup"><span data-stu-id="3274d-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="3274d-144">Tym razem wybierz **aplikacji sieci Web** z menu i nazwy aplikacji `MyWebApp`.</span><span class="sxs-lookup"><span data-stu-id="3274d-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="3274d-145">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3274d-145">Open Visual Studio.</span></span> <span data-ttu-id="3274d-146">Z menu Plik wybierz Otwórz ‣ projektu/rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="3274d-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="3274d-147">W oknie dialogowym Otwórz projekt, przejdź do *.csproj* , zaznacz go, a następnie kliknij przycisk **Otwórz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="3274d-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="3274d-148">W Eksploratorze rozwiązań projektu powinien wyglądać jak na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="3274d-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![Pliki i foldery nowy projekt w Eksploratorze rozwiązań](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="3274d-150">Narzędzia yeoman scaffolds aplikacji sieci web MVC, zarówno Pełna obsługa serwera i klienta kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3274d-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="3274d-151">Zależności po stronie serwera są wyświetlane w obszarze **zależności/NuGet** węzeł i zależności po stronie klienta w **zależności/Bower** węzła Eksploratora rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="3274d-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="3274d-152">Zależności zostaną przywrócone automatycznie po załadowaniu projektu.</span><span class="sxs-lookup"><span data-stu-id="3274d-152">Dependencies are restored automatically when the project is loaded.</span></span>

![W węźle zależności w widoku drzewa Eksploratora rozwiązań Bower folder jest otwarty, wyświetlania jego zależności.](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="3274d-154">Gdy wszystkie zależności są przywracane, naciśnij klawisz **F5** uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="3274d-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="3274d-155">Wyświetla domyślną stronę główną w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="3274d-155">The default home page displays in the browser.</span></span>

![Otwórz w programie Microsoft Edge aplikacji sieci Web](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="3274d-157">Przywracanie, kompilowania i hostingu z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="3274d-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="3274d-158">Możesz przygotowanie i hostowania aplikacji sieci web przy użyciu interfejsu wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3274d-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="3274d-159">W wierszu polecenia Zmień bieżący katalog na folder zawierający projekt (to znaczy, że folder zawierający *.csproj* pliku):</span><span class="sxs-lookup"><span data-stu-id="3274d-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="3274d-160">Przywróć zależności pakietów NuGet dla projektu:</span><span class="sxs-lookup"><span data-stu-id="3274d-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="3274d-161">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="3274d-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="3274d-162">Obsługujący wiele platform [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web rozpocznie się nasłuchiwanie na porcie 5000.</span><span class="sxs-lookup"><span data-stu-id="3274d-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="3274d-163">Otwórz przeglądarkę sieci web i przejdź do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3274d-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![Otwórz w programie Microsoft Edge aplikacji sieci Web](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="3274d-165">Dodawanie do projektu za pomocą generatory sub</span><span class="sxs-lookup"><span data-stu-id="3274d-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="3274d-166">Przy użyciu narzędzia Yeoman [sub generatory](https://github.com/omnisharp/generator-aspnet), można dodać `nuget.config` lub `web.config` po utworzeniu projektu.</span><span class="sxs-lookup"><span data-stu-id="3274d-166">Using Yeoman [sub generators](https://github.com/omnisharp/generator-aspnet), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="3274d-167">Na przykład uruchom następujące polecenie z katalogu, w którym można utworzyć pliku:</span><span class="sxs-lookup"><span data-stu-id="3274d-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="3274d-168">Wynik jest plik konfiguracji NuGet o nazwie `nuget.config` o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="3274d-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="3274d-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3274d-169">Additional resources</span></span>

* [<span data-ttu-id="3274d-170">Serwery (Kestrel i WebListener)</span><span class="sxs-lookup"><span data-stu-id="3274d-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="3274d-171">Podstawowe założenia</span><span class="sxs-lookup"><span data-stu-id="3274d-171">Fundamentals</span></span>](xref:fundamentals/index)
