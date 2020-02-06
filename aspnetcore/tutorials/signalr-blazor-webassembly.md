---
title: Korzystanie z ASP.NET Core SignalR z programem Blazor webassembly
author: guardrex
description: Utwórz aplikację czatu korzystającą z ASP.NET Core SignalR z zestawem webassembly Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: d3605c0823e9ec3ce34fb781da66a7470aa00622
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034179"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="ef515-103">Korzystanie z ASP.NET Core sygnalizującego z zestawem webassembly Blazor</span><span class="sxs-lookup"><span data-stu-id="ef515-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="ef515-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ef515-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ef515-105">W tym samouczku przedstawiono podstawowe informacje na temat tworzenia aplikacji w czasie rzeczywistym przy użyciu usługi sygnalizującej z zestawem webBlazor.</span><span class="sxs-lookup"><span data-stu-id="ef515-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="ef515-106">Omawiane kwestie:</span><span class="sxs-lookup"><span data-stu-id="ef515-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ef515-107">Tworzenie projektu hostowanej aplikacji sieci webassembly Blazor</span><span class="sxs-lookup"><span data-stu-id="ef515-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="ef515-108">Dodawanie biblioteki klienta sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="ef515-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="ef515-109">Dodawanie centrum sygnałów</span><span class="sxs-lookup"><span data-stu-id="ef515-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="ef515-110">Dodaj usługi sygnalizujące i punkt końcowy centrum sygnałów</span><span class="sxs-lookup"><span data-stu-id="ef515-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="ef515-111">Dodawanie kodu składnika Razor dla rozmowy</span><span class="sxs-lookup"><span data-stu-id="ef515-111">Add Razor component code for chat</span></span>

<span data-ttu-id="ef515-112">Na końcu tego samouczka będziesz mieć działającą aplikację czatu.</span><span class="sxs-lookup"><span data-stu-id="ef515-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="ef515-113">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef515-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef515-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ef515-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef515-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef515-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef515-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef515-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef515-117">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ef515-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ef515-118">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ef515-118">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="ef515-119">Utwórz projekt aplikacji hostowanej Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="ef515-119">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="ef515-120">Zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) .</span><span class="sxs-lookup"><span data-stu-id="ef515-120">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template.</span></span> <span data-ttu-id="ef515-121">Pakiet [Microsoft. AspNetCore. Blazor. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, podczas gdy Blazor webassembly jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="ef515-121">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span> <span data-ttu-id="ef515-122">W powłoce poleceń wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ef515-122">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
```

<span data-ttu-id="ef515-123">Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:</span><span class="sxs-lookup"><span data-stu-id="ef515-123">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef515-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef515-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ef515-125">Tworzenie nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="ef515-125">Create a new project.</span></span>

1. <span data-ttu-id="ef515-126">Wybierz pozycję **aplikacja Blazor** i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="ef515-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="ef515-127">Wpisz "BlazorSignalRApp" w polu **Nazwa projektu** .</span><span class="sxs-lookup"><span data-stu-id="ef515-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="ef515-128">Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="ef515-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="ef515-129">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="ef515-129">Select **Create**.</span></span>

1. <span data-ttu-id="ef515-130">Wybierz szablon **aplikacji Webassembly Blazor** .</span><span class="sxs-lookup"><span data-stu-id="ef515-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="ef515-131">W obszarze **Zaawansowane**zaznacz pole wyboru **hostowane ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="ef515-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="ef515-132">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="ef515-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="ef515-133">Jeśli uaktualniono lub zainstalowano nową wersję programu Visual Studio, a szablon Blazor webassembly nie jest wyświetlany w interfejsie użytkownika programu VS, należy ponownie zainstalować szablon przy użyciu podanego wcześniej polecenia `dotnet new`.</span><span class="sxs-lookup"><span data-stu-id="ef515-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef515-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef515-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ef515-135">W powłoce poleceń wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ef515-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="ef515-136">W Visual Studio Code Otwórz folder projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef515-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="ef515-137">Gdy pojawi się okno dialogowe dodawania zasobów do kompilowania i debugowania aplikacji, wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="ef515-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="ef515-138">Visual Studio Code automatycznie dodaje folder *. programu vscode* z wygenerowanymi plikami *Launch. JSON* i *Tasks. JSON* .</span><span class="sxs-lookup"><span data-stu-id="ef515-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef515-139">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ef515-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ef515-140">W powłoce poleceń wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ef515-140">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="ef515-141">W Visual Studio dla komputerów Mac otwórz projekt, przechodząc do folderu projektu i otwierając plik rozwiązania projektu ( *. sln*).</span><span class="sxs-lookup"><span data-stu-id="ef515-141">In Visual Studio for Mac, open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ef515-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ef515-142">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="ef515-143">W powłoce poleceń wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ef515-143">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="ef515-144">Dodawanie biblioteki klienta sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="ef515-144">Add the SignalR client library</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef515-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef515-145">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="ef515-146">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **BlazorSignalRApp. Client** i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ef515-146">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="ef515-147">W oknie dialogowym **Zarządzanie pakietami NuGet** upewnij się, że **Źródło pakietów** jest ustawione na *NuGet.org*.</span><span class="sxs-lookup"><span data-stu-id="ef515-147">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="ef515-148">Po wybraniu **przycisku Przeglądaj** wpisz "Microsoft. AspNetCore. signaler. Client" w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="ef515-148">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="ef515-149">W wynikach wyszukiwania wybierz pakiet `Microsoft.AspNetCore.SignalR.Client` i wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="ef515-149">In the search results, select the `Microsoft.AspNetCore.SignalR.Client` package and select **Install**.</span></span>

1. <span data-ttu-id="ef515-150">Jeśli zostanie wyświetlone okno dialogowe **Podgląd zmian** , wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="ef515-150">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="ef515-151">Jeśli zostanie wyświetlone okno dialogowe **Akceptacja licencji** , wybierz pozycję **Akceptuję** , jeśli akceptujesz postanowienia licencyjne.</span><span class="sxs-lookup"><span data-stu-id="ef515-151">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef515-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef515-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="ef515-153">W **zintegrowanym terminalu** (**Wyświetl** > **Terminal** na pasku narzędzi) wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ef515-153">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef515-154">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ef515-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ef515-155">Na pasku bocznym **rozwiązania** kliknij prawym przyciskiem myszy projekt **BlazorSignalRApp. Client** i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ef515-155">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="ef515-156">W oknie dialogowym **Zarządzanie pakietami NuGet** upewnij się, że na liście rozwijanej źródła jest ustawiona wartość *NuGet.org*.</span><span class="sxs-lookup"><span data-stu-id="ef515-156">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="ef515-157">Po wybraniu **przycisku Przeglądaj** wpisz "Microsoft. AspNetCore. signaler. Client" w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="ef515-157">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="ef515-158">W wynikach wyszukiwania zaznacz pole wyboru obok pakietu `Microsoft.AspNetCore.SignalR.Client` a następnie wybierz pozycję **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="ef515-158">In the search results, select the check box next to the `Microsoft.AspNetCore.SignalR.Client` package and select **Add Package**.</span></span>

1. <span data-ttu-id="ef515-159">Jeśli zostanie wyświetlone okno dialogowe **Akceptacja licencji** , wybierz pozycję **Akceptuj** , jeśli akceptujesz postanowienia licencyjne.</span><span class="sxs-lookup"><span data-stu-id="ef515-159">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ef515-160">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ef515-160">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="ef515-161">W powłoce poleceń wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ef515-161">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="ef515-162">Dodawanie centrum sygnałów</span><span class="sxs-lookup"><span data-stu-id="ef515-162">Add a SignalR hub</span></span>

<span data-ttu-id="ef515-163">W projekcie **BlazorSignalRApp. Server** Utwórz folder *Hubs* (plural) i Dodaj następującą klasę `ChatHub` (*Hubs/ChatHub. cs*):</span><span class="sxs-lookup"><span data-stu-id="ef515-163">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="ef515-164">Dodaj usługi sygnalizujące i punkt końcowy centrum sygnałów</span><span class="sxs-lookup"><span data-stu-id="ef515-164">Add SignalR services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="ef515-165">W projekcie **BlazorSignalRApp. Server** otwórz plik *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="ef515-165">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="ef515-166">Dodaj przestrzeń nazw dla klasy `ChatHub` na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="ef515-166">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="ef515-167">Dodaj usługi sygnalizujące do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ef515-167">Add the SignalR services to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddSignalR();
   ```

1. <span data-ttu-id="ef515-168">W `Startup.Configure` między punktami końcowymi dla trasy domyślnego kontrolera i powrotu po stronie klienta należy dodać punkt końcowy dla centrum:</span><span class="sxs-lookup"><span data-stu-id="ef515-168">In `Startup.Configure` between the endpoints for the default controller route and the client-side fallback, add an endpoint for the hub:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="ef515-169">Dodawanie kodu składnika Razor dla rozmowy</span><span class="sxs-lookup"><span data-stu-id="ef515-169">Add Razor component code for chat</span></span>

1. <span data-ttu-id="ef515-170">W projekcie **BlazorSignalRApp. Client** Otwórz plik *Pages/index. Razor* .</span><span class="sxs-lookup"><span data-stu-id="ef515-170">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="ef515-171">Zastąp znacznik następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ef515-171">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="ef515-172">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ef515-172">Run the app</span></span>

1. <span data-ttu-id="ef515-173">Postępuj zgodnie ze wskazówkami dotyczącymi narzędzi:</span><span class="sxs-lookup"><span data-stu-id="ef515-173">Follow the guidance for your tooling:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef515-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef515-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ef515-175">W **Eksplorator rozwiązań**wybierz projekt **BlazorSignalRApp. Server** .</span><span class="sxs-lookup"><span data-stu-id="ef515-175">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="ef515-176">Naciśnij **klawisze CTRL + F5** , aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="ef515-176">Press **Ctrl+F5** to run the app without debugging.</span></span>

1. <span data-ttu-id="ef515-177">Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="ef515-177">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ef515-178">Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="ef515-178">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="ef515-179">Nazwa i komunikat są wyświetlane na obu stronach natychmiast:</span><span class="sxs-lookup"><span data-stu-id="ef515-179">The name and message are displayed on both pages instantly:</span></span>

   ![Przykładowa aplikacja usługi Blazor webassembly otwiera się w dwóch oknach przeglądarki, w których wyświetlane są komunikaty wymieniane.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="ef515-181">Cudzysłowy: *gwiazdka Trek VI: niewykrywalny kraj* &copy;[1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="ef515-181">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef515-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef515-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ef515-183">Wybierz pozycję **debuguj** > **Uruchom bez debugowania** na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="ef515-183">Select **Debug** > **Run Without Debugging** from the toolbar.</span></span>

1. <span data-ttu-id="ef515-184">Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="ef515-184">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ef515-185">Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="ef515-185">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="ef515-186">Nazwa i komunikat są wyświetlane na obu stronach natychmiast:</span><span class="sxs-lookup"><span data-stu-id="ef515-186">The name and message are displayed on both pages instantly:</span></span>

   ![Przykładowa aplikacja usługi Blazor webassembly otwiera się w dwóch oknach przeglądarki, w których wyświetlane są komunikaty wymieniane.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="ef515-188">Cudzysłowy: *gwiazdka Trek VI: niewykrywalny kraj* &copy;[1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="ef515-188">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef515-189">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ef515-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ef515-190">Na pasku bocznym **rozwiązania** wybierz projekt **BlazorSignalRApp. Server** .</span><span class="sxs-lookup"><span data-stu-id="ef515-190">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="ef515-191">Z menu wybierz polecenie **uruchom** > **Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="ef515-191">From the menu, select **Run** > **Start Without Debugging**.</span></span>

1. <span data-ttu-id="ef515-192">Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="ef515-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ef515-193">Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="ef515-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="ef515-194">Nazwa i komunikat są wyświetlane na obu stronach natychmiast:</span><span class="sxs-lookup"><span data-stu-id="ef515-194">The name and message are displayed on both pages instantly:</span></span>

   ![Przykładowa aplikacja usługi Blazor webassembly otwiera się w dwóch oknach przeglądarki, w których wyświetlane są komunikaty wymieniane.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="ef515-196">Cudzysłowy: *gwiazdka Trek VI: niewykrywalny kraj* &copy;[1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="ef515-196">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ef515-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ef515-197">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="ef515-198">W powłoce poleceń wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ef515-198">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="ef515-199">Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="ef515-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ef515-200">Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij** .</span><span class="sxs-lookup"><span data-stu-id="ef515-200">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="ef515-201">Nazwa i komunikat są wyświetlane na obu stronach natychmiast:</span><span class="sxs-lookup"><span data-stu-id="ef515-201">The name and message are displayed on both pages instantly:</span></span>

   ![Przykładowa aplikacja usługi Blazor webassembly otwiera się w dwóch oknach przeglądarki, w których wyświetlane są komunikaty wymieniane.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="ef515-203">Cudzysłowy: *gwiazdka Trek VI: niewykrywalny kraj* &copy;[1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="ef515-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="ef515-204">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ef515-204">Next steps</span></span>

<span data-ttu-id="ef515-205">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ef515-205">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ef515-206">Tworzenie projektu hostowanej aplikacji Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="ef515-206">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="ef515-207">Dodawanie SignalRej biblioteki klienta</span><span class="sxs-lookup"><span data-stu-id="ef515-207">Add the SignalR client library</span></span>
> * <span data-ttu-id="ef515-208">Dodawanie centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="ef515-208">Add a SignalR hub</span></span>
> * <span data-ttu-id="ef515-209">Dodaj SignalR usługi i punkt końcowy centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="ef515-209">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="ef515-210">Dodawanie kodu składnika Razor dla rozmowy</span><span class="sxs-lookup"><span data-stu-id="ef515-210">Add Razor component code for chat</span></span>

<span data-ttu-id="ef515-211">Aby dowiedzieć się więcej na temat tworzenia aplikacji Blazor, zapoznaj się z dokumentacją Blazor:</span><span class="sxs-lookup"><span data-stu-id="ef515-211">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="ef515-212">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ef515-212">Additional resources</span></span>

* <xref:signalr/introduction>
