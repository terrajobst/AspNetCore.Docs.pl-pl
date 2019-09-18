---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak tworzyć wielokrotnego użytku Razor interfejsu użytkownika przy użyciu widoków częściowych w bibliotece klas, w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/22/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 92c04c1ac4c70c6245accf272753bc914aaab860
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081870"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="7d6d2-103">Utwórz interfejs użytkownika wielokrotnego użytku przy użyciu projektu biblioteki klas Razor w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d6d2-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="7d6d2-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d6d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d6d2-105">Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="7d6d2-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="7d6d2-106">RCL może spakowane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="7d6d2-107">Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="7d6d2-108">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="7d6d2-109">Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="7d6d2-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="7d6d2-110">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7d6d2-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="7d6d2-111">Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="7d6d2-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7d6d2-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d6d2-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7d6d2-113">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7d6d2-114">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="7d6d2-115">Nazwa biblioteki (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="7d6d2-116">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="7d6d2-117">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="7d6d2-118">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="7d6d2-119">RCL ma następujący plik projektu:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7d6d2-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d6d2-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7d6d2-121">W wierszu polecenia Uruchom polecenie `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="7d6d2-122">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="7d6d2-123">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="7d6d2-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="7d6d2-124">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="7d6d2-125">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="7d6d2-126">Szablony ASP.NET Core przyjęto założenie, zawartość RCL znajduje się w *obszarów* folderu.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="7d6d2-127">Zobacz [układ stron RCL](#afs) , aby utworzyć RCL, który uwidacznia zawartość `~/Pages` zamiast. `~/Areas/Pages`</span><span class="sxs-lookup"><span data-stu-id="7d6d2-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="7d6d2-128">Odwołanie do zawartości RCL</span><span class="sxs-lookup"><span data-stu-id="7d6d2-128">Referencing RCL content</span></span>

<span data-ttu-id="7d6d2-129">RCL mogą być przywoływane przez:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="7d6d2-130">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-130">NuGet package.</span></span> <span data-ttu-id="7d6d2-131">Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="7d6d2-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="7d6d2-132">*{Nazwa_projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="7d6d2-133">Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="7d6d2-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="7d6d2-134">Przewodnik: Tworzenie projektu RCL i korzystanie z projektu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7d6d2-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="7d6d2-135">Możesz pobrać [kompletnego projektu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestuj go, a nie jej tworzenia.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="7d6d2-136">Pobierz przykładowy zawiera dodatkowy kod i łącza, które ułatwiają Testowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="7d6d2-137">Możesz pozostawić opinię w [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) z komentarzami pobierania próbek i instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="7d6d2-138">Testowanie aplikacji pobierania</span><span class="sxs-lookup"><span data-stu-id="7d6d2-138">Test the download app</span></span>

<span data-ttu-id="7d6d2-139">Jeśli nie zostały pobrane ukończonej aplikacji i raczej utworzyć projekt wskazówki, przejdź do [następnej sekcji](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="7d6d2-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7d6d2-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d6d2-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7d6d2-141">Otwórz *.sln* pliku w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="7d6d2-142">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7d6d2-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d6d2-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7d6d2-144">Z poziomu wiersza polecenia w *interfejsu wiersza polecenia* katalogu, tworzenie RCL i aplikacja sieci web.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="7d6d2-145">Przenieś do *WebApp1* katalogu i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-145">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="7d6d2-146">Postępuj zgodnie z instrukcjami w [WebApp1 testu](#test)</span><span class="sxs-lookup"><span data-stu-id="7d6d2-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="7d6d2-147">Utwórz RCL</span><span class="sxs-lookup"><span data-stu-id="7d6d2-147">Create an RCL</span></span>

<span data-ttu-id="7d6d2-148">W tej sekcji jest tworzony RCL.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-148">In this section, an RCL is created.</span></span> <span data-ttu-id="7d6d2-149">Pliki razor są dodawane do RCL.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7d6d2-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d6d2-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7d6d2-151">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-151">Create the RCL project:</span></span>

* <span data-ttu-id="7d6d2-152">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7d6d2-153">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="7d6d2-154">Określanie nazwy aplikacji **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="7d6d2-155">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="7d6d2-156">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="7d6d2-157">Dodaj plik do widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7d6d2-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d6d2-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7d6d2-159">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-159">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="7d6d2-160">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-160">The preceding commands:</span></span>

* <span data-ttu-id="7d6d2-161">`RazorUIClassLib` Tworzy RCL.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="7d6d2-162">Tworzy stronę _Message Razor i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="7d6d2-163">`-np` Parametr tworzy tę stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="7d6d2-164">Tworzy [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="7d6d2-165">*_ViewStart.cshtml* pliku jest wymagana do używania układ stron Razor projektu, (który został dodany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="7d6d2-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="7d6d2-166">Dodawanie Razor plików i folderów do projektu</span><span class="sxs-lookup"><span data-stu-id="7d6d2-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="7d6d2-167">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="7d6d2-168">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="7d6d2-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` wymagane jest wprowadzenie widoku częściowego (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="7d6d2-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="7d6d2-170">Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="7d6d2-171">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-171">For example:</span></span>

```dotnetcli
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="7d6d2-172">Aby uzyskać więcej informacji na temat *_ViewImports.cshtml*, zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="7d6d2-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="7d6d2-173">Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-173">Build the class library to verify there are no compiler errors:</span></span>

```dotnetcli
dotnet build RazorUIClassLib
```

<span data-ttu-id="7d6d2-174">Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="7d6d2-175">*RazorUIClassLib.Views.dll* zawiera skompilowanej zawartości Razor.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="7d6d2-176">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="7d6d2-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7d6d2-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d6d2-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7d6d2-178">Tworzenie aplikacji sieci web stron Razor:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="7d6d2-179">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="7d6d2-180">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="7d6d2-181">Określanie nazwy aplikacji **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="7d6d2-182">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="7d6d2-183">Wybierz **aplikacji sieci Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="7d6d2-184">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="7d6d2-185">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **zależności kompilacji** > **zależności projektu**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="7d6d2-186">Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="7d6d2-187">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Dodaj** > **odwołania**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="7d6d2-188">W **Menadżer odwołań** okno dialogowe, sprawdź **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="7d6d2-189">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7d6d2-190">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d6d2-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7d6d2-191">Utwórz Razor Pages aplikację sieci Web i plik rozwiązania zawierający aplikację Razor Pages i RCL:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="7d6d2-192">Kompilowanie i uruchamianie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-192">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="7d6d2-193">WebApp1 testu</span><span class="sxs-lookup"><span data-stu-id="7d6d2-193">Test WebApp1</span></span>

<span data-ttu-id="7d6d2-194">Sprawdź, czy biblioteka klas interfejsu użytkownika Razor jest używana:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="7d6d2-195">Przejdź do `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="7d6d2-196">Zastąp widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="7d6d2-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="7d6d2-197">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="7d6d2-198">Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="7d6d2-199">Do pobrania próbki, Zmień nazwę *WebApp1/obszarów/MyFeature2* do *WebApp1/obszarów/MyFeature* do testowania pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="7d6d2-200">Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="7d6d2-201">Zaktualizuj znaczników, aby wskazać nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="7d6d2-202">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="7d6d2-203">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="7d6d2-203">RCL Pages layout</span></span>

<span data-ttu-id="7d6d2-204">Odwołanie RCL zawartości, jakby była częścią aplikacji sieci web *stron* folderu, Utwórz projekt RCL o następującej strukturze pliku:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="7d6d2-205">*RazorUIClassLib/stron*</span><span class="sxs-lookup"><span data-stu-id="7d6d2-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="7d6d2-206">*RazorUIClassLib/stron/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="7d6d2-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="7d6d2-207">Załóżmy, że *RazorUIClassLib/stron/Shared* zawiera dwa pliki częściowa: *_Header.cshtml* i *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="7d6d2-208">`<partial>` Tagów może zostać dodany do *_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="7d6d2-209">Tworzenie RCL przy użyciu statycznych zasobów</span><span class="sxs-lookup"><span data-stu-id="7d6d2-209">Create an RCL with static assets</span></span>

<span data-ttu-id="7d6d2-210">RCL może wymagać pomocnika zasobów statycznych, do których może odwoływać się aplikacja zużywana przez RCL.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="7d6d2-211">ASP.NET Core umożliwia tworzenie RCLs zawierających statyczne zasoby, które są dostępne dla aplikacji zużywanej.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="7d6d2-212">Aby dołączyć zasoby towarzyszące jako część RCL, Utwórz folder *wwwroot* w bibliotece klas i Dołącz wszystkie wymagane pliki w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="7d6d2-213">Podczas pakowania RCL wszystkie zasoby towarzyszące w folderze *wwwroot* zostaną automatycznie dołączone do pakietu.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-213">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="7d6d2-214">Wyklucz zasoby statyczne</span><span class="sxs-lookup"><span data-stu-id="7d6d2-214">Exclude static assets</span></span>

<span data-ttu-id="7d6d2-215">Aby wykluczyć statyczne elementy zawartości, Dodaj żądaną ścieżkę wykluczenia do `$(DefaultItemExcludes)` grupy właściwości w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-215">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="7d6d2-216">Oddzielaj wpisy średnikami (`;`).</span><span class="sxs-lookup"><span data-stu-id="7d6d2-216">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="7d6d2-217">W poniższym przykładzie arkusz stylów *lib. css* w folderze *wwwroot* nie jest traktowany jako statyczny zasób i nie jest uwzględniony w opublikowanym RCL:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-217">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="7d6d2-218">Integracja języka TypeScript</span><span class="sxs-lookup"><span data-stu-id="7d6d2-218">Typescript integration</span></span>

<span data-ttu-id="7d6d2-219">Aby dołączyć pliki TypeScript do RCL:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-219">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="7d6d2-220">Umieść pliki TypeScript ( *. TS*) poza folderem *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="7d6d2-220">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="7d6d2-221">Na przykład Umieść pliki w folderze *Client* .</span><span class="sxs-lookup"><span data-stu-id="7d6d2-221">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="7d6d2-222">Skonfiguruj dane wyjściowe kompilacji TypeScript dla folderu *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="7d6d2-222">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="7d6d2-223">Ustaw właściwość wewnątrz elementu `PropertyGroup` w pliku projektu: `TypescriptOutDir`</span><span class="sxs-lookup"><span data-stu-id="7d6d2-223">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="7d6d2-224">Dołącz obiekt docelowy TypeScript jako zależność `ResolveCurrentProjectStaticWebAssets` obiektu docelowego, dodając następujący element docelowy wewnątrz `PropertyGroup` pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-224">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="7d6d2-225">Korzystanie z zawartości z RCL, do którego istnieje odwołanie</span><span class="sxs-lookup"><span data-stu-id="7d6d2-225">Consume content from a referenced RCL</span></span>

<span data-ttu-id="7d6d2-226">Pliki znajdujące się w folderze *WWWROOT* RCL są uwidocznione dla aplikacji zużywanej pod prefiksem `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-226">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="7d6d2-227">Na przykład biblioteka o nazwie *Razor. Class. lib* skutkuje ścieżką do zawartości statycznej pod adresem `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-227">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="7d6d2-228">Aplikacja, która zużywa odwołania `<script>` `<img>`, odwołuje się do statycznych zasobów udostępnianych przez bibliotekę z, `<style>`, i innymi tagami HTML.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-228">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="7d6d2-229">Aplikacja zużywana musi mieć włączoną [obsługę pliku statycznego](xref:fundamentals/static-files) w `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-229">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="7d6d2-230">W przypadku uruchamiania aplikacji zużywającej dane wyjściowe kompilacji (`dotnet run`) statyczne zasoby sieci Web są domyślnie włączone w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-230">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="7d6d2-231">Aby zapewnić obsługę zasobów w innych środowiskach podczas uruchamiania z danych wyjściowych `UseStaticWebAssets` kompilacji, należy wywołać konstruktora hosta w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-231">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="7d6d2-232">Wywołanie `UseStaticWebAssets` nie jest wymagane w przypadku uruchamiania aplikacji z opublikowanych danych`dotnet publish`wyjściowych ().</span><span class="sxs-lookup"><span data-stu-id="7d6d2-232">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="7d6d2-233">Przepływ programistyczny dla wieloprojektowych</span><span class="sxs-lookup"><span data-stu-id="7d6d2-233">Multi-project development flow</span></span>

<span data-ttu-id="7d6d2-234">Po uruchomieniu aplikacji zużywanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="7d6d2-234">When the consuming app runs:</span></span>

* <span data-ttu-id="7d6d2-235">Zasoby w RCL pozostają w swoich oryginalnych folderach.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-235">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="7d6d2-236">Elementy zawartości nie są przenoszone do aplikacji zużywanej.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-236">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="7d6d2-237">Wszelkie zmiany w folderze *WWWROOT* RCL są odzwierciedlone w aplikacji zużywanej po odtworzeniu RCL i niepomyślnym skompilowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-237">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="7d6d2-238">Po skompilowaniu RCL jest tworzony manifest, który opisuje lokalizacje statycznego elementu zawartości sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-238">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="7d6d2-239">Aplikacja, która korzysta z aplikacji, odczytuje manifest w czasie wykonywania, aby wykorzystać zasoby z przywoływanych projektów i pakietów.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-239">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="7d6d2-240">Po dodaniu nowego elementu zawartości do RCL należy ponownie skompilować RCL, aby zaktualizować jego manifest, zanim aplikacja zużywa dostęp do nowego elementu zawartości.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-240">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="7d6d2-241">Publikowanie</span><span class="sxs-lookup"><span data-stu-id="7d6d2-241">Publish</span></span>

<span data-ttu-id="7d6d2-242">Po opublikowaniu aplikacji składniki towarzyszące ze wszystkich przywoływanych projektów i pakietów są kopiowane do folderu *wwwroot* opublikowanej aplikacji w obszarze `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="7d6d2-242">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end
