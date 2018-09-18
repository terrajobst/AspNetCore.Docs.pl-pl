---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 06129200834607188052f44a888749c51662f638
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011608"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="b6eac-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6eac-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="b6eac-104">Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="b6eac-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="b6eac-105">Tworzenie projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6eac-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="b6eac-106">Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b6eac-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="b6eac-107">Zaufanie certyfikatu deweloperskiego HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b6eac-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6eac-108">Windows</span><span class="sxs-lookup"><span data-stu-id="b6eac-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="b6eac-109">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="b6eac-109">The preceding command displays the following dialog:</span></span>

   ![Okno dialogowe ostrzeżenia o zabezpieczeniach](_static/cert.png)

   <span data-ttu-id="b6eac-111">Wybierz **tak** Jeśli zgadzasz się ufać certyfikatowi rozwoju.</span><span class="sxs-lookup"><span data-stu-id="b6eac-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="b6eac-112">macOS</span><span class="sxs-lookup"><span data-stu-id="b6eac-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="b6eac-113">Poprzednie polecenie wyświetli następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="b6eac-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="b6eac-114">*Zażądano ufające certyfikatu deweloperskiego protokołu HTTPS. Jeśli certyfikat nie jest zaufany, firma Microsoft będzie uruchom następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="b6eac-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>
   <span data-ttu-id="b6eac-115">*To polecenie może monitować o hasło, aby zainstalować certyfikat w pęku kluczy systemu. Hasło:*</span><span class="sxs-lookup"><span data-stu-id="b6eac-115">*This command might prompt you for your password to install the certificate on the system keychain. Password:*</span></span>

   <span data-ttu-id="b6eac-116">Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi rozwoju.</span><span class="sxs-lookup"><span data-stu-id="b6eac-116">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6eac-117">Linux</span><span class="sxs-lookup"><span data-stu-id="b6eac-117">Linux</span></span>](#tab/linux)

   <span data-ttu-id="b6eac-118">W dokumentacji dla Twojej dystrybucji systemu Linux na temat zaufania certyfikatu deweloperskiego protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="b6eac-118">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
   
---

4. <span data-ttu-id="b6eac-119">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="b6eac-119">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="b6eac-120">Przejdź do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="b6eac-120">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="b6eac-121">Kliknij przycisk **Akceptuj** zaakceptować zasady ochrony prywatności i plików cookie.</span><span class="sxs-lookup"><span data-stu-id="b6eac-121">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="b6eac-122">Ta aplikacja nie zachowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="b6eac-122">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="b6eac-123">Otwórz *Pages/About.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="b6eac-123">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="b6eac-124">Przejdź do [ http://localhost:5001/About ](http://localhost:5001/About) i Sprawdź zmiany są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="b6eac-124">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="b6eac-125">Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="b6eac-125">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="b6eac-126">Utwórz nowy projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6eac-126">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="b6eac-127">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b6eac-127">Open a command shell.</span></span> <span data-ttu-id="b6eac-128">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b6eac-128">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="b6eac-129">Uruchom aplikację za pomocą następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="b6eac-129">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="b6eac-130">Przejdź do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="b6eac-130">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="b6eac-131">Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world!</span><span class="sxs-lookup"><span data-stu-id="b6eac-131">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="b6eac-132">Czas na serwerze jest @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="b6eac-132">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="b6eac-133">Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) a następnie zweryfikować zmiany.</span><span class="sxs-lookup"><span data-stu-id="b6eac-133">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="b6eac-134">Zainstaluj program .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="b6eac-134">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="b6eac-135">Utwórz folder dla nowego projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6eac-135">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="b6eac-136">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b6eac-136">Open a command shell.</span></span> <span data-ttu-id="b6eac-137">Wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b6eac-137">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="b6eac-138">Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* plik, aby zaznaczyć 1.0.4 zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="b6eac-138">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="b6eac-139">Utwórz nowy projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6eac-139">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="b6eac-140">Przywróć pakiety.</span><span class="sxs-lookup"><span data-stu-id="b6eac-140">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="b6eac-141">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b6eac-141">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="b6eac-142">[Dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="b6eac-142">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="b6eac-143">Przejdź do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b6eac-143">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
