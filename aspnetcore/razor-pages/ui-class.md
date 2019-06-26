---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak tworzyć wielokrotnego użytku Razor interfejsu użytkownika przy użyciu widoków częściowych w bibliotece klas, w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/24/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 96ef8fc055a6b92cd0808d02031d917b8446f305
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394745"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="b5959-103">Tworzenie interfejsu użytkownika do wielokrotnego użytku, przy użyciu projektu biblioteki klas Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5959-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="b5959-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b5959-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b5959-105">Widokami razor, strony, kontrolerów, modele strony [składniki Razor](xref:blazor/class-libraries), [wyświetlanie składników](xref:mvc/views/view-components), i modeli danych może zostać wbudowana w bibliotekę klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="b5959-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="b5959-106">RCL może spakowane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="b5959-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="b5959-107">Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="b5959-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="b5959-108">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="b5959-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="b5959-109">Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="b5959-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="b5959-110">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b5959-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="b5959-111">Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="b5959-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5959-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5959-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b5959-113">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b5959-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b5959-114">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b5959-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b5959-115">Nazwa biblioteki (na przykład "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5959-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="b5959-116">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b5959-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="b5959-117">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b5959-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b5959-118">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5959-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="b5959-119">RCL ma następujący plik projektu:</span><span class="sxs-lookup"><span data-stu-id="b5959-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b5959-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b5959-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b5959-121">W wierszu polecenia Uruchom polecenie `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="b5959-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="b5959-122">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b5959-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="b5959-123">Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="b5959-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="b5959-124">Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.</span><span class="sxs-lookup"><span data-stu-id="b5959-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="b5959-125">Dodaj pliki Razor do RCL.</span><span class="sxs-lookup"><span data-stu-id="b5959-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="b5959-126">Szablony ASP.NET Core przyjęto założenie, zawartość RCL znajduje się w *obszarów* folderu.</span><span class="sxs-lookup"><span data-stu-id="b5959-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="b5959-127">Zobacz [układ stron RCL](#afs) utworzyć RCL, który udostępnia zawartość `~/Pages` zamiast `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="b5959-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="b5959-128">Odwoływanie się do zawartości RCL</span><span class="sxs-lookup"><span data-stu-id="b5959-128">Referencing RCL content</span></span>

<span data-ttu-id="b5959-129">RCL mogą być przywoływane przez:</span><span class="sxs-lookup"><span data-stu-id="b5959-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="b5959-130">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="b5959-130">NuGet package.</span></span> <span data-ttu-id="b5959-131">Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="b5959-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="b5959-132">*{Nazwa_projektu} .csproj*.</span><span class="sxs-lookup"><span data-stu-id="b5959-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="b5959-133">Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="b5959-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="b5959-134">Przewodnik: Utwórz projekt RCL i korzystać z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="b5959-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="b5959-135">Możesz pobrać [kompletnego projektu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestuj go, a nie jej tworzenia.</span><span class="sxs-lookup"><span data-stu-id="b5959-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="b5959-136">Pobierz przykładowy zawiera dodatkowy kod i łącza, które ułatwiają Testowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="b5959-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="b5959-137">Możesz pozostawić opinię w [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) z komentarzami pobierania próbek i instrukcje krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="b5959-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="b5959-138">Testowanie aplikacji pobierania</span><span class="sxs-lookup"><span data-stu-id="b5959-138">Test the download app</span></span>

<span data-ttu-id="b5959-139">Jeśli nie zostały pobrane ukończonej aplikacji i raczej utworzyć projekt wskazówki, przejdź do [następnej sekcji](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="b5959-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5959-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5959-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b5959-141">Otwórz *.sln* pliku w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b5959-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="b5959-142">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b5959-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b5959-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b5959-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b5959-144">Z poziomu wiersza polecenia w *interfejsu wiersza polecenia* katalogu, tworzenie RCL i aplikacja sieci web.</span><span class="sxs-lookup"><span data-stu-id="b5959-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="b5959-145">Przenieś do *WebApp1* katalogu i uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="b5959-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="b5959-146">Postępuj zgodnie z instrukcjami w [WebApp1 testu](#test)</span><span class="sxs-lookup"><span data-stu-id="b5959-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="b5959-147">Utwórz RCL</span><span class="sxs-lookup"><span data-stu-id="b5959-147">Create an RCL</span></span>

<span data-ttu-id="b5959-148">W tej sekcji RCL jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="b5959-148">In this section, an RCL is created.</span></span> <span data-ttu-id="b5959-149">Pliki razor są dodawane do RCL.</span><span class="sxs-lookup"><span data-stu-id="b5959-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5959-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5959-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b5959-151">Utwórz projekt RCL:</span><span class="sxs-lookup"><span data-stu-id="b5959-151">Create the RCL project:</span></span>

* <span data-ttu-id="b5959-152">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b5959-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b5959-153">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b5959-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b5959-154">Określanie nazwy aplikacji **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5959-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="b5959-155">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b5959-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b5959-156">Wybierz **biblioteki klas Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5959-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="b5959-157">Dodaj plik do widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b5959-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b5959-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b5959-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b5959-159">W wierszu polecenia Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b5959-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="b5959-160">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="b5959-160">The preceding commands:</span></span>

* <span data-ttu-id="b5959-161">Tworzy `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="b5959-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="b5959-162">Tworzy stronę _Message Razor i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="b5959-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="b5959-163">`-np` Parametr tworzy tę stronę bez `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="b5959-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="b5959-164">Tworzy [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.</span><span class="sxs-lookup"><span data-stu-id="b5959-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="b5959-165">*_ViewStart.cshtml* pliku jest wymagana do używania układ stron Razor projektu, (który został dodany w następnej sekcji).</span><span class="sxs-lookup"><span data-stu-id="b5959-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="b5959-166">Dodawanie Razor plików i folderów do projektu</span><span class="sxs-lookup"><span data-stu-id="b5959-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="b5959-167">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b5959-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="b5959-168">Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b5959-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="b5959-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` wymagane jest wprowadzenie widoku częściowego (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="b5959-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="b5959-170">Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="b5959-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b5959-171">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b5959-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="b5959-172">Aby uzyskać więcej informacji na temat *_ViewImports.cshtml*, zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="b5959-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="b5959-173">Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:</span><span class="sxs-lookup"><span data-stu-id="b5959-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="b5959-174">Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="b5959-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="b5959-175">*RazorUIClassLib.Views.dll* zawiera skompilowanej zawartości Razor.</span><span class="sxs-lookup"><span data-stu-id="b5959-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="b5959-176">Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="b5959-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5959-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5959-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b5959-178">Tworzenie aplikacji sieci web stron Razor:</span><span class="sxs-lookup"><span data-stu-id="b5959-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="b5959-179">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="b5959-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="b5959-180">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b5959-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b5959-181">Określanie nazwy aplikacji **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="b5959-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="b5959-182">Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b5959-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="b5959-183">Wybierz **aplikacji sieci Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5959-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="b5959-184">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.</span><span class="sxs-lookup"><span data-stu-id="b5959-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="b5959-185">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **zależności kompilacji** > **zależności projektu**.</span><span class="sxs-lookup"><span data-stu-id="b5959-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="b5959-186">Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="b5959-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="b5959-187">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Dodaj** > **odwołania**.</span><span class="sxs-lookup"><span data-stu-id="b5959-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="b5959-188">W **Menadżer odwołań** okno dialogowe, sprawdź **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5959-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="b5959-189">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b5959-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b5959-190">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b5959-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b5959-191">Tworzenie, aplikacja internetowa ze stronami Razor i plikiem rozwiązania zawierającego aplikację stron Razor i RCL:</span><span class="sxs-lookup"><span data-stu-id="b5959-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="b5959-192">Kompilowanie i uruchamianie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="b5959-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="b5959-193">WebApp1 testu</span><span class="sxs-lookup"><span data-stu-id="b5959-193">Test WebApp1</span></span>

<span data-ttu-id="b5959-194">Sprawdź, czy biblioteki klas Razor interfejsu użytkownika jest w użyciu:</span><span class="sxs-lookup"><span data-stu-id="b5959-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="b5959-195">Przejdź do `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="b5959-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="b5959-196">Zastąp widoki, widoki częściowe i strony</span><span class="sxs-lookup"><span data-stu-id="b5959-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="b5959-197">Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="b5959-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="b5959-198">Na przykład dodać *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* do WebApp1, a strona 1 w WebApp1 mają wyższy priorytet niż strona 1 w RCL.</span><span class="sxs-lookup"><span data-stu-id="b5959-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="b5959-199">Do pobrania próbki, Zmień nazwę *WebApp1/obszarów/MyFeature2* do *WebApp1/obszarów/MyFeature* do testowania pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="b5959-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="b5959-200">Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b5959-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="b5959-201">Zaktualizuj znaczników, aby wskazać nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b5959-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="b5959-202">Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.</span><span class="sxs-lookup"><span data-stu-id="b5959-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="b5959-203">Układ stron RCL</span><span class="sxs-lookup"><span data-stu-id="b5959-203">RCL Pages layout</span></span>

<span data-ttu-id="b5959-204">Odwołanie RCL zawartości, jakby była częścią aplikacji sieci web *stron* folderu, Utwórz projekt RCL o następującej strukturze pliku:</span><span class="sxs-lookup"><span data-stu-id="b5959-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="b5959-205">*RazorUIClassLib/stron*</span><span class="sxs-lookup"><span data-stu-id="b5959-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="b5959-206">*RazorUIClassLib/stron/udostępnione*</span><span class="sxs-lookup"><span data-stu-id="b5959-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="b5959-207">Załóżmy, że *RazorUIClassLib/stron/Shared* zawiera dwa pliki częściowa: *_Header.cshtml* i *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b5959-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="b5959-208">`<partial>` Tagów może zostać dodany do *_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="b5959-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="b5959-209">Tworzenie RCL przy użyciu statycznych zasobów</span><span class="sxs-lookup"><span data-stu-id="b5959-209">Create an RCL with static assets</span></span>

<span data-ttu-id="b5959-210">RCL może wymagać pomocnika statycznych zasobów, które mogą być przywoływane przez aplikację odbierającą RCL.</span><span class="sxs-lookup"><span data-stu-id="b5959-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="b5959-211">Platforma ASP.NET Core umożliwia tworzenie RCLs, które obejmują zasoby statyczne, które są dostępne dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b5959-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="b5959-212">Aby dołączyć zasoby pomocnika jako część RCL, utworzyć *wwwroot* folder w ułatwieniach i zawierać wszystkie wymagane pliki w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="b5959-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="b5959-213">Podczas pakowania RCL, wszystkie pomocnika zasobów w *wwwroot* folderów są automatycznie uwzględnione w pakiecie i są dostępne dla aplikacji, które odwołuje się do pakietu.</span><span class="sxs-lookup"><span data-stu-id="b5959-213">When packing an RCL, all companion assets in the *wwwroot* folder are included in the package automatically and are made available to apps referencing the package.</span></span>

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="b5959-214">Używać zawartości od RCL odwołania</span><span class="sxs-lookup"><span data-stu-id="b5959-214">Consume content from a referenced RCL</span></span>

<span data-ttu-id="b5959-215">Pliki zawarte w *wwwroot* folderu RCL są widoczne dla aplikacji w ramach prefiksu `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="b5959-215">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="b5959-216">`{LIBRARY NAME}` Nazwa projektu biblioteki jest konwertowany na małe litery z kropek (`.`) usunięte.</span><span class="sxs-lookup"><span data-stu-id="b5959-216">`{LIBRARY NAME}` is the library project name converted to lowercase with periods (`.`) removed.</span></span> <span data-ttu-id="b5959-217">Na przykład, biblioteka o nazwie *Razor.Class.Lib* wyników w ścieżce do zawartości statycznej w `_content/razorclasslib/`.</span><span class="sxs-lookup"><span data-stu-id="b5959-217">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/razorclasslib/`.</span></span>

<span data-ttu-id="b5959-218">Aplikacja odbierająca komunikaty odwołuje się do statycznych zasobów dostarczone przez bibliotekę z `<script>`, `<style>`, `<img>`, a inne tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="b5959-218">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="b5959-219">Aplikacja odbierająca komunikaty musi mieć [obsługi plików statycznych](xref:fundamentals/static-files) włączone.</span><span class="sxs-lookup"><span data-stu-id="b5959-219">The consuming app must have [static file support](xref:fundamentals/static-files) enabled.</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="b5959-220">Przepływ rozwoju wielu projektów</span><span class="sxs-lookup"><span data-stu-id="b5959-220">Multi-project development flow</span></span>

<span data-ttu-id="b5959-221">Podczas wykonywania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b5959-221">When the consuming app runs:</span></span>

* <span data-ttu-id="b5959-222">Zasoby w pobytu RCL w ich oryginalnych folderów.</span><span class="sxs-lookup"><span data-stu-id="b5959-222">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="b5959-223">Zasoby nie są przenoszone do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b5959-223">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="b5959-224">Wszelkie zmiany w ramach RCL *wwwroot* folderu znajduje odzwierciedlenie w aplikacji po odbudowaniu RCL i bez odbudowywania odbierającą aplikację.</span><span class="sxs-lookup"><span data-stu-id="b5959-224">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="b5959-225">Podczas kompilowania RCL manifest jest generowany, opisujący lokalizacje zasobów statyczną sieci web.</span><span class="sxs-lookup"><span data-stu-id="b5959-225">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="b5959-226">Aplikacja odbierająca komunikaty odczytuje manifest w czasie wykonywania, korzystanie z zasobów w projektach odwołania i pakietów.</span><span class="sxs-lookup"><span data-stu-id="b5959-226">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="b5959-227">Po dodaniu nowego elementu zawartości RCL RCL musi zostać zrekompilowany, aby zaktualizować manifest, zanim aplikacja odbierająca komunikaty mogą uzyskiwać dostęp do nowego elementu zawartości.</span><span class="sxs-lookup"><span data-stu-id="b5959-227">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="b5959-228">Publikowanie</span><span class="sxs-lookup"><span data-stu-id="b5959-228">Publish</span></span>

<span data-ttu-id="b5959-229">Po opublikowaniu aplikacji zasoby pomocnika z wszystkie przywoływane projekty i pakiety są kopiowane do *wwwroot* folderu opublikowanej aplikacji, w obszarze `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="b5959-229">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>
