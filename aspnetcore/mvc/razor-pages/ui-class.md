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
ms.openlocfilehash: b6658186adf5ccd74e1d0f0e925627f50bad250c
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687554"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="b08e1-103">Utwórz wielokrotnego użytku interfejsu użytkownika przy użyciu projektu biblioteki klas Razor w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b08e1-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="b08e1-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b08e1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b08e1-105">Widokami razor, stron, kontrolerów, modeli strony [wyświetlić składniki](xref:mvc/views/view-components), i modeli danych mogą być wbudowane w bibliotece klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="b08e1-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="b08e1-106">RCL można umieszczone i użyć ponownie.</span><span class="sxs-lookup"><span data-stu-id="b08e1-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="b08e1-107">Aplikacje można obejmują RCL i zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="b08e1-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="b08e1-108">Gdy widok, widok częściowy lub Razor strony nie został znaleziony w aplikacji sieci web i RCL, znaczników Razor (*.cshtml* pliku) w sieci web, pierwszeństwo ma aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b08e1-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="b08e1-109">Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="b08e1-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="b08e1-110">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b08e1-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="b08e1-111">Utwórz bibliotekę klasy zawierające Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="b08e1-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b08e1-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b08e1-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b08e1-113">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b08e1-114">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b08e1-115">Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b08e1-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b08e1-116">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b08e1-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b08e1-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b08e1-118">W wierszu polecenia, uruchom `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="b08e1-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="b08e1-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b08e1-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="b08e1-120">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="b08e1-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="b08e1-121">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="b08e1-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="b08e1-122">Firma Microsoft zaleca RCL zawartości Przejdź w *obszarów* folderu.</span><span class="sxs-lookup"><span data-stu-id="b08e1-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="b08e1-123">Odwołanie do biblioteki klas Razor zawartości</span><span class="sxs-lookup"><span data-stu-id="b08e1-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="b08e1-124">RCL może odwoływać się:</span><span class="sxs-lookup"><span data-stu-id="b08e1-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="b08e1-125">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="b08e1-125">NuGet package.</span></span> <span data-ttu-id="b08e1-126">Zobacz [pakietów NuGet tworzenie](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="b08e1-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="b08e1-127">*{Nazwa_projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="b08e1-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="b08e1-128">Zobacz [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="b08e1-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="b08e1-129">Dostęp do plików z częściowa w RCL</span><span class="sxs-lookup"><span data-stu-id="b08e1-129">Partial files access in the RCL</span></span>

<span data-ttu-id="b08e1-130">Zawartości poza RCL środowisko uruchomieniowe platformy ASP.NET Core nie wyszukiwania plików częściowe w RCL.</span><span class="sxs-lookup"><span data-stu-id="b08e1-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="b08e1-131">Na przykład w pobieranie próbki *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* można widoku częściowego **nie** odwoływać się do *WebApp1\Pages\About.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="b08e1-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="b08e1-132">Jednak stron w RCL ( *RazorUIClassLib /* **można** dostępu *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b08e1-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="b08e1-133">Wskazówki: Tworzenie projektu biblioteki klas Razor i korzystać z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="b08e1-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="b08e1-134">Możesz pobrać [kompletnego projektu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) i przetestować go zamiast go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="b08e1-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="b08e1-135">Pobieranie próbki zawiera dodatkowy kod i linki, które umożliwiają łatwe testowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="b08e1-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="b08e1-136">Możesz pozostawić opinii w [ten problem GitHub](https://github.com/aspnet/Docs/issues/6098) z komentarzami na pobieranie próbek i instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="b08e1-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="b08e1-137">Testowanie aplikacji pobierania</span><span class="sxs-lookup"><span data-stu-id="b08e1-137">Test the download app</span></span>

<span data-ttu-id="b08e1-138">Jeśli nie zostały pobrane ukończonej aplikacji, a raczej spowodowałoby utworzenie projektu wskazówki, przejdź do [następnej sekcji](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="b08e1-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b08e1-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b08e1-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b08e1-140">Otwórz *.sln* pliku w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b08e1-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="b08e1-141">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b08e1-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b08e1-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b08e1-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b08e1-143">Z wiersza polecenia w *cli* katalogu kompilacji RCL i sieci web aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b08e1-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="b08e1-144">Przenieś do *WebApp1* katalogu i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="b08e1-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="b08e1-145">Postępuj zgodnie z instrukcjami [WebApp1 testu](#test)</span><span class="sxs-lookup"><span data-stu-id="b08e1-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="b08e1-146">Utwórz bibliotekę klas Razor</span><span class="sxs-lookup"><span data-stu-id="b08e1-146">Create a Razor Class Library</span></span>

<span data-ttu-id="b08e1-147">W tej sekcji jest tworzony biblioteki klasy Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="b08e1-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="b08e1-148">Pliki razor zostaną dodane do RCL.</span><span class="sxs-lookup"><span data-stu-id="b08e1-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b08e1-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b08e1-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b08e1-150">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="b08e1-150">Create the RCL project:</span></span>

* <span data-ttu-id="b08e1-151">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b08e1-152">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b08e1-153">Nazwa aplikacji **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="b08e1-154">Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b08e1-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b08e1-155">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="b08e1-156">Tworzenie aplikacji sieci web Razor strony:</span><span class="sxs-lookup"><span data-stu-id="b08e1-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="b08e1-157">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="b08e1-158">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b08e1-159">Nazwa aplikacji **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="b08e1-160">Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b08e1-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b08e1-161">Wybierz **aplikacji sieci Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="b08e1-162">Dodaj Razor pliki i foldery do projektu.</span><span class="sxs-lookup"><span data-stu-id="b08e1-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="b08e1-163">Dodaj widok częściowy Razor plik o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b08e1-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="b08e1-164">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b08e1-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="b08e1-165">Kopiuj *_ViewStart.cshtml* plik z projektu WebApp1 *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b08e1-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="b08e1-166">[Viewstart](xref:mvc/views/layout#running-code-before-each-view) plik jest wymagany do użycia układ projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="b08e1-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b08e1-167">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b08e1-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b08e1-168">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b08e1-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="b08e1-169">Poprzednie polecenia:</span><span class="sxs-lookup"><span data-stu-id="b08e1-169">The preceding commands:</span></span>

* <span data-ttu-id="b08e1-170">Tworzy `RazorUIClassLib` Razor biblioteki klas (RCL).</span><span class="sxs-lookup"><span data-stu-id="b08e1-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="b08e1-171">Tworzy stronę _Message Razor i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="b08e1-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="b08e1-172">`-np` Parametr tworzy tę stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="b08e1-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="b08e1-173">Tworzy [viewstart](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="b08e1-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="b08e1-174">Plik viewstart jest wymagany do użycia układ projektu stron Razor (który został dodany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="b08e1-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="b08e1-175">Aktualizacja stron Razor:</span><span class="sxs-lookup"><span data-stu-id="b08e1-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="b08e1-176">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b08e1-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="b08e1-177">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b08e1-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="b08e1-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Musisz wybrać widok częściowy (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="b08e1-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="b08e1-179">Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="b08e1-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b08e1-180">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b08e1-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="b08e1-181">Aby uzyskać więcej informacji o viewimports, zobacz [importowanie dyrektywy udostępnionych](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="b08e1-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="b08e1-182">Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:</span><span class="sxs-lookup"><span data-stu-id="b08e1-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="b08e1-183">Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="b08e1-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="b08e1-184">*RazorUIClassLib.Views.dll* skompilowanych zawartość Razor.</span><span class="sxs-lookup"><span data-stu-id="b08e1-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="b08e1-185">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="b08e1-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b08e1-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b08e1-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b08e1-187">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="b08e1-188">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **zależności kompilacji** > **zależności projektu**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="b08e1-189">Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="b08e1-190">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Dodaj** > **odwołania**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="b08e1-191">W **Menedżera odwołań** dialogowym wyboru **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b08e1-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="b08e1-192">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b08e1-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b08e1-193">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b08e1-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b08e1-194">Tworzenie aplikacji sieci web Razor strony i plik rozwiązania zawierający stron Razor aplikacji i biblioteki klas Razor:</span><span class="sxs-lookup"><span data-stu-id="b08e1-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="b08e1-195">Tworzenie i uruchamianie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="b08e1-195">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="b08e1-196">WebApp1 testu</span><span class="sxs-lookup"><span data-stu-id="b08e1-196">Test WebApp1</span></span>

<span data-ttu-id="b08e1-197">Sprawdź, czy jest używana biblioteka klas Razor interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b08e1-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="b08e1-198">Przejdź do `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="b08e1-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="b08e1-199">Zastąpienie widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="b08e1-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="b08e1-200">Gdy widoku, widok częściowy lub Razor strony znajduje się zarówno w aplikacji sieci web, jak i w bibliotece klas programu Razor, znaczników Razor (*.cshtml* pliku) w sieci web, pierwszeństwo ma aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b08e1-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="b08e1-201">Na przykład dodać *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* do WebApp1, a strona 1 w WebApp1 mają wyższy priorytet niż Page1in biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="b08e1-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="b08e1-202">Do pobrania na stronie próbki, należy zmienić *WebApp1/obszarów/MyFeature2* do *MyFeature-WebApp1/obszarów* do testowania pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="b08e1-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="b08e1-203">Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b08e1-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="b08e1-204">Zaktualizuj kod znaczników, aby wskazać, do nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b08e1-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="b08e1-205">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="b08e1-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
