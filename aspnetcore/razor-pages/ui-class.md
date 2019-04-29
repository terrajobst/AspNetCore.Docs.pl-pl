---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak tworzyć wielokrotnego użytku Razor interfejsu użytkownika przy użyciu widoków częściowych w bibliotece klas, w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/07/2018
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 383b7e49b5dbd0c0f13218aff20abe6421d785b8
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165160"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="941bb-103">Tworzenie interfejsu użytkownika wielokrotnego użytku, używając projektu biblioteki klas Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="941bb-103">Create reusable UI using the Razor Class Library project in ASP.NET Core</span></span>

<span data-ttu-id="941bb-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="941bb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="941bb-105">Widokami razor, strony, kontrolerów, modele strony [wyświetlanie składników](xref:mvc/views/view-components), a modeli danych może być kompilowany do biblioteki klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="941bb-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="941bb-106">RCL może spakowane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="941bb-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="941bb-107">Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="941bb-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="941bb-108">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web (*.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="941bb-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="941bb-109">Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="941bb-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="941bb-110">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="941bb-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="941bb-111">Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="941bb-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="941bb-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="941bb-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="941bb-113">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="941bb-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="941bb-114">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="941bb-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="941bb-115">Nazwa biblioteki (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="941bb-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="941bb-116">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="941bb-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="941bb-117">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="941bb-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="941bb-118">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="941bb-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="941bb-119">Biblioteki klas Razor ma następujący plik projektu:</span><span class="sxs-lookup"><span data-stu-id="941bb-119">A Razor Class Library has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="941bb-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="941bb-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="941bb-121">W wierszu polecenia Uruchom polecenie `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="941bb-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="941bb-122">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="941bb-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="941bb-123">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="941bb-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="941bb-124">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="941bb-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="941bb-125">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="941bb-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="941bb-126">Szablony ASP.NET Core przyjęto założenie, zawartość RCL znajduje się w *obszarów* folderu.</span><span class="sxs-lookup"><span data-stu-id="941bb-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="941bb-127">Zobacz [układ stron RCL](#afs) utworzyć RCL, który udostępnia zawartość `~/Pages` zamiast `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="941bb-127">See [RCL Pages layout](#afs) to create a RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="941bb-128">Odwoływanie się do zawartości biblioteki klas Razor</span><span class="sxs-lookup"><span data-stu-id="941bb-128">Referencing Razor Class Library content</span></span>

<span data-ttu-id="941bb-129">RCL mogą być przywoływane przez:</span><span class="sxs-lookup"><span data-stu-id="941bb-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="941bb-130">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="941bb-130">NuGet package.</span></span> <span data-ttu-id="941bb-131">Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="941bb-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="941bb-132">*{Nazwa_projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="941bb-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="941bb-133">Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="941bb-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="941bb-134">Przewodnik: Utwórz projekt biblioteki klas Razor i korzystać z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="941bb-134">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="941bb-135">Możesz pobrać [kompletnego projektu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestuj go, a nie jej tworzenia.</span><span class="sxs-lookup"><span data-stu-id="941bb-135">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="941bb-136">Pobierz przykładowy zawiera dodatkowy kod i łącza, które ułatwiają Testowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="941bb-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="941bb-137">Możesz pozostawić opinię w [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/6098) z komentarzami pobierania próbek i instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="941bb-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="941bb-138">Testowanie aplikacji pobierania</span><span class="sxs-lookup"><span data-stu-id="941bb-138">Test the download app</span></span>

<span data-ttu-id="941bb-139">Jeśli nie zostały pobrane ukończonej aplikacji i raczej utworzyć projekt wskazówki, przejdź do [następnej sekcji](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="941bb-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="941bb-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="941bb-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="941bb-141">Otwórz *.sln* pliku w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="941bb-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="941bb-142">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="941bb-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="941bb-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="941bb-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="941bb-144">Z poziomu wiersza polecenia w *interfejsu wiersza polecenia* katalogu, tworzenie RCL i aplikacja sieci web.</span><span class="sxs-lookup"><span data-stu-id="941bb-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="941bb-145">Przenieś do *WebApp1* katalogu i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="941bb-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="941bb-146">Postępuj zgodnie z instrukcjami w [WebApp1 testu](#test)</span><span class="sxs-lookup"><span data-stu-id="941bb-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="941bb-147">Tworzenie biblioteki klas Razor</span><span class="sxs-lookup"><span data-stu-id="941bb-147">Create a Razor Class Library</span></span>

<span data-ttu-id="941bb-148">W tej sekcji jest tworzony biblioteki klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="941bb-148">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="941bb-149">Pliki razor są dodawane do RCL.</span><span class="sxs-lookup"><span data-stu-id="941bb-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="941bb-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="941bb-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="941bb-151">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="941bb-151">Create the RCL project:</span></span>

* <span data-ttu-id="941bb-152">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="941bb-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="941bb-153">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="941bb-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="941bb-154">Określanie nazwy aplikacji **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="941bb-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="941bb-155">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="941bb-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="941bb-156">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="941bb-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="941bb-157">Dodaj plik do widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="941bb-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="941bb-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="941bb-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="941bb-159">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="941bb-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="941bb-160">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="941bb-160">The preceding commands:</span></span>

* <span data-ttu-id="941bb-161">Tworzy `RazorUIClassLib` biblioteki klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="941bb-161">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="941bb-162">Tworzy stronę _Message Razor i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="941bb-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="941bb-163">`-np` Parametr tworzy tę stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="941bb-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="941bb-164">Tworzy [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="941bb-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="941bb-165">*_ViewStart.cshtml* pliku jest wymagana do używania układ stron Razor projektu, (który został dodany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="941bb-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="941bb-166">Dodawanie Razor plików i folderów do projektu</span><span class="sxs-lookup"><span data-stu-id="941bb-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="941bb-167">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="941bb-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="941bb-168">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="941bb-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="941bb-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` wymagane jest wprowadzenie widoku częściowego (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="941bb-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="941bb-170">Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="941bb-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="941bb-171">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="941bb-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="941bb-172">Aby uzyskać więcej informacji na temat *_ViewImports.cshtml*, zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="941bb-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="941bb-173">Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:</span><span class="sxs-lookup"><span data-stu-id="941bb-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="941bb-174">Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="941bb-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="941bb-175">*RazorUIClassLib.Views.dll* zawiera skompilowanej zawartości Razor.</span><span class="sxs-lookup"><span data-stu-id="941bb-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="941bb-176">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="941bb-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="941bb-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="941bb-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="941bb-178">Tworzenie aplikacji sieci web stron Razor:</span><span class="sxs-lookup"><span data-stu-id="941bb-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="941bb-179">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="941bb-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="941bb-180">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="941bb-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="941bb-181">Określanie nazwy aplikacji **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="941bb-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="941bb-182">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="941bb-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="941bb-183">Wybierz **aplikacji sieci Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="941bb-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="941bb-184">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="941bb-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="941bb-185">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **zależności kompilacji** > **zależności projektu**.</span><span class="sxs-lookup"><span data-stu-id="941bb-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="941bb-186">Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="941bb-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="941bb-187">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Dodaj** > **odwołania**.</span><span class="sxs-lookup"><span data-stu-id="941bb-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="941bb-188">W **Menadżer odwołań** okno dialogowe, sprawdź **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="941bb-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="941bb-189">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="941bb-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="941bb-190">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="941bb-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="941bb-191">Tworzenie, aplikacja internetowa ze stronami Razor i plikiem rozwiązania zawierającego aplikację stron Razor i biblioteki klas Razor:</span><span class="sxs-lookup"><span data-stu-id="941bb-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="941bb-192">Kompilowanie i uruchamianie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="941bb-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="941bb-193">WebApp1 testu</span><span class="sxs-lookup"><span data-stu-id="941bb-193">Test WebApp1</span></span>

<span data-ttu-id="941bb-194">Sprawdź, czy jest on używany biblioteki klas Razor interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="941bb-194">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="941bb-195">Przejdź do `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="941bb-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="941bb-196">Zastąp widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="941bb-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="941bb-197">Gdy widoku, widoku częściowego lub strona Razor znajduje się w aplikacji sieci web i biblioteki klas Razor znaczników Razor (*.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="941bb-197">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="941bb-198">Na przykład dodać *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* do WebApp1, a strona 1 w WebApp1 mają wyższy priorytet niż strona 1 w bibliotece klas Razor.</span><span class="sxs-lookup"><span data-stu-id="941bb-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the Razor Class Library.</span></span>

<span data-ttu-id="941bb-199">Do pobrania próbki, Zmień nazwę *WebApp1/obszarów/MyFeature2* do *WebApp1/obszarów/MyFeature* do testowania pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="941bb-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="941bb-200">Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="941bb-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="941bb-201">Zaktualizuj znaczników, aby wskazać nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="941bb-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="941bb-202">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="941bb-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="941bb-203">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="941bb-203">RCL Pages layout</span></span>

<span data-ttu-id="941bb-204">Odwołanie RCL zawartości, jakby była częścią aplikacji sieci web *stron* folderu, Utwórz projekt RCL o następującej strukturze pliku:</span><span class="sxs-lookup"><span data-stu-id="941bb-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="941bb-205">*RazorUIClassLib/stron*</span><span class="sxs-lookup"><span data-stu-id="941bb-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="941bb-206">*RazorUIClassLib/stron/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="941bb-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="941bb-207">Załóżmy, że *RazorUIClassLib/stron/Shared* zawiera dwa pliki częściowa: *_Header.cshtml* i *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="941bb-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="941bb-208">`<partial>` Tagów może zostać dodany do *_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="941bb-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```
