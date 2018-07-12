---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia sposób tworzenia wielokrotnego użytku Razor interfejsu użytkownika w bibliotece klas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 8190302a15670b0a7474445f7b11d4cba46981db
ms.sourcegitcommit: 19cbda409bdbbe42553dc385ea72d2a8e246509c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992852"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="5f871-103">Tworzenie interfejsu użytkownika wielokrotnego użytku, używając projektu biblioteki klas Razor w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5f871-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="5f871-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5f871-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5f871-105">Widokami razor, strony, kontrolerów, modele strony [wyświetlanie składników](xref:mvc/views/view-components), a modeli danych może być kompilowany do biblioteki klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="5f871-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="5f871-106">RCL może spakowane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="5f871-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="5f871-107">Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="5f871-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="5f871-108">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web (*.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="5f871-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="5f871-109">Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="5f871-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="5f871-110">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f871-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="5f871-111">Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="5f871-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f871-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f871-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5f871-113">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="5f871-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5f871-114">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5f871-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="5f871-115">Nazwa biblioteki (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f871-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="5f871-116">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="5f871-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="5f871-117">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="5f871-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="5f871-118">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f871-118">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f871-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f871-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5f871-120">W wierszu polecenia, uruchom polecenie `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="5f871-120">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="5f871-121">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5f871-121">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="5f871-122">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="5f871-122">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="5f871-123">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="5f871-123">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="5f871-124">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="5f871-124">Add Razor files to the RCL.</span></span>

<span data-ttu-id="5f871-125">Firma Microsoft zaleca RCL zawartości go w *obszarów* folderu.</span><span class="sxs-lookup"><span data-stu-id="5f871-125">We recommend RCL content go in the *Areas* folder.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="5f871-126">Odwoływanie się do zawartości biblioteki klas Razor</span><span class="sxs-lookup"><span data-stu-id="5f871-126">Referencing Razor Class Library content</span></span>

<span data-ttu-id="5f871-127">RCL mogą być przywoływane przez:</span><span class="sxs-lookup"><span data-stu-id="5f871-127">The RCL can be referenced by:</span></span>

* <span data-ttu-id="5f871-128">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="5f871-128">NuGet package.</span></span> <span data-ttu-id="5f871-129">Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="5f871-129">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="5f871-130">*{Nazwa_projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="5f871-130">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="5f871-131">Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="5f871-131">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="5f871-132">Przewodnik: Tworzenie projektu biblioteki klas Razor i korzystać z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="5f871-132">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="5f871-133">Możesz pobrać [kompletnego projektu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestuj go, a nie jej tworzenia.</span><span class="sxs-lookup"><span data-stu-id="5f871-133">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="5f871-134">Pobierz przykładowy zawiera dodatkowy kod i łącza, które ułatwiają Testowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="5f871-134">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="5f871-135">Możesz pozostawić opinię w [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/6098) z komentarzami pobierania próbek i instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="5f871-135">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="5f871-136">Testowanie aplikacji pobierania</span><span class="sxs-lookup"><span data-stu-id="5f871-136">Test the download app</span></span>

<span data-ttu-id="5f871-137">Jeśli nie zostały pobrane ukończonej aplikacji i raczej utworzyć projekt wskazówki, przejdź do [następnej sekcji](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="5f871-137">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f871-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f871-138">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5f871-139">Otwórz *.sln* pliku w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5f871-139">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="5f871-140">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5f871-140">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f871-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f871-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5f871-142">Z poziomu wiersza polecenia w *interfejsu wiersza polecenia* katalogu, tworzenie RCL i aplikacja sieci web.</span><span class="sxs-lookup"><span data-stu-id="5f871-142">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="5f871-143">Przenieś do *WebApp1* katalogu i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="5f871-143">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="5f871-144">Postępuj zgodnie z instrukcjami w [WebApp1 testu](#test)</span><span class="sxs-lookup"><span data-stu-id="5f871-144">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="5f871-145">Tworzenie biblioteki klas Razor</span><span class="sxs-lookup"><span data-stu-id="5f871-145">Create a Razor Class Library</span></span>

<span data-ttu-id="5f871-146">W tej sekcji jest tworzony biblioteki klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="5f871-146">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="5f871-147">Pliki razor są dodawane do RCL.</span><span class="sxs-lookup"><span data-stu-id="5f871-147">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f871-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f871-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5f871-149">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="5f871-149">Create the RCL project:</span></span>

* <span data-ttu-id="5f871-150">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="5f871-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5f871-151">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5f871-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="5f871-152">Określanie nazwy aplikacji **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f871-152">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="5f871-153">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="5f871-153">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="5f871-154">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f871-154">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="5f871-155">Dodaj plik do widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5f871-155">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f871-156">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f871-156">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5f871-157">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5f871-157">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="5f871-158">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="5f871-158">The preceding commands:</span></span>

* <span data-ttu-id="5f871-159">Tworzy `RazorUIClassLib` biblioteki klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="5f871-159">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="5f871-160">Tworzy stronę _Message Razor i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="5f871-160">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="5f871-161">`-np` Parametr tworzy tę stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="5f871-161">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="5f871-162">Tworzy [viewstart](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="5f871-162">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="5f871-163">Plik viewstart jest wymagany do układ stron Razor projektu, (który został dodany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="5f871-163">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

------

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="5f871-164">Dodaj Razor plików i folderów do projektu.</span><span class="sxs-lookup"><span data-stu-id="5f871-164">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="5f871-165">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5f871-165">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="5f871-166">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5f871-166">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="5f871-167">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` wymagane jest wprowadzenie widoku częściowego (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="5f871-167">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="5f871-168">Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="5f871-168">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="5f871-169">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5f871-169">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="5f871-170">Aby uzyskać więcej informacji na temat viewimports, zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="5f871-170">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="5f871-171">Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:</span><span class="sxs-lookup"><span data-stu-id="5f871-171">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="5f871-172">Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="5f871-172">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="5f871-173">*RazorUIClassLib.Views.dll* zawiera skompilowanej zawartości Razor.</span><span class="sxs-lookup"><span data-stu-id="5f871-173">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="5f871-174">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="5f871-174">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f871-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f871-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5f871-176">Tworzenie aplikacji sieci web stron Razor:</span><span class="sxs-lookup"><span data-stu-id="5f871-176">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="5f871-177">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="5f871-177">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="5f871-178">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5f871-178">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="5f871-179">Określanie nazwy aplikacji **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="5f871-179">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="5f871-180">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="5f871-180">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="5f871-181">Wybierz **aplikacji sieci Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f871-181">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="5f871-182">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="5f871-182">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="5f871-183">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **zależności kompilacji** > **zależności projektu**.</span><span class="sxs-lookup"><span data-stu-id="5f871-183">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="5f871-184">Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="5f871-184">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="5f871-185">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Dodaj** > **odwołania**.</span><span class="sxs-lookup"><span data-stu-id="5f871-185">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="5f871-186">W **Menadżer odwołań** okno dialogowe, sprawdź **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f871-186">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="5f871-187">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5f871-187">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f871-188">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f871-188">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5f871-189">Tworzenie, aplikacja internetowa ze stronami Razor i plikiem rozwiązania zawierającego aplikację stron Razor i biblioteki klas Razor:</span><span class="sxs-lookup"><span data-stu-id="5f871-189">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

<span data-ttu-id="5f871-190">Kompilowanie i uruchamianie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="5f871-190">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="5f871-191">WebApp1 testu</span><span class="sxs-lookup"><span data-stu-id="5f871-191">Test WebApp1</span></span>

<span data-ttu-id="5f871-192">Sprawdź, czy jest on używany biblioteki klas Razor interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5f871-192">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="5f871-193">Przejdź do `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="5f871-193">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="5f871-194">Zastąp widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="5f871-194">Override views, partial views, and pages</span></span>

<span data-ttu-id="5f871-195">Gdy widoku, widoku częściowego lub strona Razor znajduje się w aplikacji sieci web i biblioteki klas Razor znaczników Razor (*.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="5f871-195">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="5f871-196">Na przykład dodać *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* do WebApp1, a strona 1 w WebApp1 mają wyższy priorytet niż Page1in biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="5f871-196">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="5f871-197">Do pobrania próbki, Zmień nazwę *WebApp1/obszarów/MyFeature2* do *WebApp1/obszarów/MyFeature* do testowania pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="5f871-197">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="5f871-198">Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5f871-198">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="5f871-199">Zaktualizuj znaczników, aby wskazać nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="5f871-199">Update the markup to indicate the new location.</span></span> <span data-ttu-id="5f871-200">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="5f871-200">Build and run the app to verify the app's version of the partial is being used.</span></span>
