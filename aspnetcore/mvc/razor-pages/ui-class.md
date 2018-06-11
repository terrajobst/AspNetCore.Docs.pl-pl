---
title: Wielokrotnego użytku UI Razor w bibliotekach klas podstawowych ASP.NET
author: Rick-Anderson
description: Wyjaśnia sposób tworzenia wielokrotnego użytku Razor interfejsu użytkownika w bibliotece klas.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 1321164d683439709ed2a219aa2d784094bae7cf
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252324"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="1a7aa-103">Utwórz wielokrotnego użytku interfejsu użytkownika przy użyciu projektu biblioteki klas Razor w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="1a7aa-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a7aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a7aa-105">Widokami razor, stron, kontrolerów, modeli strony [wyświetlić składniki](xref:mvc/views/view-components), i modeli danych mogą być wbudowane w bibliotece klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="1a7aa-106">RCL można umieszczone i użyć ponownie.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="1a7aa-107">Aplikacje można obejmują RCL i zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="1a7aa-108">Gdy widok, widok częściowy lub Razor strony nie został znaleziony w aplikacji sieci web i RCL, znaczników Razor (*.cshtml* pliku) w sieci web, pierwszeństwo ma aplikacja.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="1a7aa-109">Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="1a7aa-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="1a7aa-110">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1a7aa-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="1a7aa-111">Utwórz bibliotekę klasy zawierające Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="1a7aa-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a7aa-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a7aa-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1a7aa-113">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1a7aa-114">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1a7aa-115">Nazwa biblioteki (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="1a7aa-116">Aby uniknąć kolizję nazw plików z biblioteką wygenerowanego widoku, upewnij się, nazwa biblioteki nie kończyć się znakiem `.Views`.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="1a7aa-117">Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1a7aa-118">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-118">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a7aa-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a7aa-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a7aa-120">W wierszu polecenia, uruchom `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-120">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="1a7aa-121">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-121">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="1a7aa-122">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-122">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="1a7aa-123">Aby uniknąć kolizję nazw plików z biblioteką wygenerowanego widoku, upewnij się, nazwa biblioteki nie kończyć się znakiem `.Views`.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-123">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="1a7aa-124">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-124">Add Razor files to the RCL.</span></span>

<span data-ttu-id="1a7aa-125">Firma Microsoft zaleca RCL zawartości Przejdź w *obszarów* folderu.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-125">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="1a7aa-126">Odwołanie do biblioteki klas Razor zawartości</span><span class="sxs-lookup"><span data-stu-id="1a7aa-126">Referencing Razor Class Library content</span></span>

<span data-ttu-id="1a7aa-127">RCL może odwoływać się:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-127">The RCL can be referenced by:</span></span>

* <span data-ttu-id="1a7aa-128">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-128">NuGet package.</span></span> <span data-ttu-id="1a7aa-129">Zobacz [pakietów NuGet tworzenie](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-129">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="1a7aa-130">*{Nazwa_projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-130">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="1a7aa-131">Zobacz [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-131">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="1a7aa-132">Wskazówki: Tworzenie projektu biblioteki klas Razor i korzystać z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="1a7aa-132">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="1a7aa-133">Możesz pobrać [kompletnego projektu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) i przetestować go zamiast go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-133">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="1a7aa-134">Pobieranie próbki zawiera dodatkowy kod i linki, które umożliwiają łatwe testowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-134">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="1a7aa-135">Możesz pozostawić opinii w [ten problem GitHub](https://github.com/aspnet/Docs/issues/6098) z komentarzami na pobieranie próbek i instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-135">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="1a7aa-136">Testowanie aplikacji pobierania</span><span class="sxs-lookup"><span data-stu-id="1a7aa-136">Test the download app</span></span>

<span data-ttu-id="1a7aa-137">Jeśli nie zostały pobrane ukończonej aplikacji, a raczej spowodowałoby utworzenie projektu wskazówki, przejdź do [następnej sekcji](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-137">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a7aa-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a7aa-138">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1a7aa-139">Otwórz *.sln* pliku w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-139">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="1a7aa-140">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-140">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a7aa-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a7aa-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a7aa-142">Z wiersza polecenia w *cli* katalogu kompilacji RCL i sieci web aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-142">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="1a7aa-143">Przenieś do *WebApp1* katalogu i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-143">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="1a7aa-144">Postępuj zgodnie z instrukcjami [WebApp1 testu](#test)</span><span class="sxs-lookup"><span data-stu-id="1a7aa-144">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="1a7aa-145">Utwórz bibliotekę klas Razor</span><span class="sxs-lookup"><span data-stu-id="1a7aa-145">Create a Razor Class Library</span></span>

<span data-ttu-id="1a7aa-146">W tej sekcji jest tworzony biblioteki klasy Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-146">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="1a7aa-147">Pliki razor zostaną dodane do RCL.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-147">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a7aa-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a7aa-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1a7aa-149">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-149">Create the RCL project:</span></span>

* <span data-ttu-id="1a7aa-150">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1a7aa-151">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1a7aa-152">Nazwa aplikacji **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-152">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="1a7aa-153">Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-153">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1a7aa-154">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-154">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="1a7aa-155">Tworzenie aplikacji sieci web Razor strony:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-155">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="1a7aa-156">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-156">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="1a7aa-157">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-157">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1a7aa-158">Nazwa aplikacji **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-158">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="1a7aa-159">Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-159">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="1a7aa-160">Wybierz **aplikacji sieci Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-160">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="1a7aa-161">Dodaj Razor pliki i foldery do projektu.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-161">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="1a7aa-162">Dodaj widok częściowy Razor plik o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-162">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="1a7aa-163">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-163">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="1a7aa-164">Kopiuj *_ViewStart.cshtml* plik z projektu WebApp1 *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-164">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="1a7aa-165">[Viewstart](xref:mvc/views/layout#running-code-before-each-view) plik jest wymagany do użycia układ projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-165">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a7aa-166">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a7aa-166">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a7aa-167">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-167">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="1a7aa-168">Poprzednie polecenia:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-168">The preceding commands:</span></span>

* <span data-ttu-id="1a7aa-169">Tworzy `RazorUIClassLib` Razor biblioteki klas (RCL).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-169">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="1a7aa-170">Tworzy stronę _Message Razor i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-170">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="1a7aa-171">`-np` Parametr tworzy tę stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-171">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="1a7aa-172">Tworzy [viewstart](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-172">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="1a7aa-173">Plik viewstart jest wymagany do użycia układ projektu stron Razor (który został dodany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-173">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="1a7aa-174">Aktualizacja stron Razor:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-174">Update the Razor Pages:</span></span>

* <span data-ttu-id="1a7aa-175">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-175">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="1a7aa-176">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="1a7aa-177">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Musisz wybrać widok częściowy (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="1a7aa-177">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="1a7aa-178">Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-178">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1a7aa-179">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-179">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="1a7aa-180">Aby uzyskać więcej informacji o viewimports, zobacz [importowanie dyrektywy udostępnionych](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="1a7aa-180">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="1a7aa-181">Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-181">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="1a7aa-182">Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-182">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="1a7aa-183">*RazorUIClassLib.Views.dll* skompilowanych zawartość Razor.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-183">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="1a7aa-184">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="1a7aa-184">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a7aa-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a7aa-185">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1a7aa-186">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-186">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="1a7aa-187">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **zależności kompilacji** > **zależności projektu**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-187">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="1a7aa-188">Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-188">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="1a7aa-189">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Dodaj** > **odwołania**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-189">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="1a7aa-190">W **Menedżera odwołań** dialogowym wyboru **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-190">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="1a7aa-191">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-191">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a7aa-192">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1a7aa-192">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a7aa-193">Tworzenie aplikacji sieci web Razor strony i plik rozwiązania zawierający stron Razor aplikacji i biblioteki klas Razor:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-193">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

<span data-ttu-id="1a7aa-194">Tworzenie i uruchamianie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="1a7aa-194">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="1a7aa-195">WebApp1 testu</span><span class="sxs-lookup"><span data-stu-id="1a7aa-195">Test WebApp1</span></span>

<span data-ttu-id="1a7aa-196">Sprawdź, czy jest używana biblioteka klas Razor interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-196">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="1a7aa-197">Przejdź do `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-197">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="1a7aa-198">Zastąpienie widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="1a7aa-198">Override views, partial views, and pages</span></span>

<span data-ttu-id="1a7aa-199">Gdy widoku, widok częściowy lub Razor strony znajduje się zarówno w aplikacji sieci web, jak i w bibliotece klas programu Razor, znaczników Razor (*.cshtml* pliku) w sieci web, pierwszeństwo ma aplikacja.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-199">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="1a7aa-200">Na przykład dodać *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* do WebApp1, a strona 1 w WebApp1 mają wyższy priorytet niż Page1in biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-200">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="1a7aa-201">Do pobrania na stronie próbki, należy zmienić *WebApp1/obszarów/MyFeature2* do *MyFeature-WebApp1/obszarów* do testowania pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-201">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="1a7aa-202">Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-202">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="1a7aa-203">Zaktualizuj kod znaczników, aby wskazać, do nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-203">Update the markup to indicate the new location.</span></span> <span data-ttu-id="1a7aa-204">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="1a7aa-204">Build and run the app to verify the app's version of the partial is being used.</span></span>
