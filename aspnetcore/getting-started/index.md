---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "38216216"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="bffc3-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffc3-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="bffc3-104">Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="bffc3-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="bffc3-105">Tworzenie projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bffc3-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="bffc3-106">Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bffc3-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="bffc3-107">[! OBEJMUJĄ [] (~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="bffc3-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="bffc3-108">Zaufanie certyfikatu deweloperskiego HTTPS:</span><span class="sxs-lookup"><span data-stu-id="bffc3-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bffc3-109">Windows</span><span class="sxs-lookup"><span data-stu-id="bffc3-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="bffc3-110">macOS</span><span class="sxs-lookup"><span data-stu-id="bffc3-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="bffc3-111">Linux</span><span class="sxs-lookup"><span data-stu-id="bffc3-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="bffc3-112">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="bffc3-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="bffc3-113">Przejdź do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="bffc3-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="bffc3-114">Kliknij przycisk **Akceptuj** zaakceptować zasady ochrony prywatności i plików cookie.</span><span class="sxs-lookup"><span data-stu-id="bffc3-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="bffc3-115">Ta aplikacja nie zachowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="bffc3-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="bffc3-116">Otwórz *Pages/About.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="bffc3-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="bffc3-117">Przejdź do [ http://localhost:5001/About ](http://localhost:5001/About) i Sprawdź zmiany są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="bffc3-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="bffc3-118">Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="bffc3-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="bffc3-119">Utwórz nowy projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bffc3-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="bffc3-120">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="bffc3-120">Open a command shell.</span></span> <span data-ttu-id="bffc3-121">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bffc3-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="bffc3-122">Uruchom aplikację za pomocą następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="bffc3-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="bffc3-123">Przejdź do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="bffc3-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="bffc3-124">Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world!</span><span class="sxs-lookup"><span data-stu-id="bffc3-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="bffc3-125">Czas na serwerze jest @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="bffc3-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="bffc3-126">Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) a następnie zweryfikować zmiany.</span><span class="sxs-lookup"><span data-stu-id="bffc3-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="bffc3-127">Zainstaluj program .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="bffc3-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="bffc3-128">Utwórz folder dla nowego projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bffc3-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="bffc3-129">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="bffc3-129">Open a command shell.</span></span> <span data-ttu-id="bffc3-130">Wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="bffc3-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="bffc3-131">Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* plik, aby zaznaczyć 1.0.4 zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="bffc3-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="bffc3-132">Utwórz nowy projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bffc3-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="bffc3-133">Przywróć pakiety.</span><span class="sxs-lookup"><span data-stu-id="bffc3-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="bffc3-134">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="bffc3-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="bffc3-135">[Dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="bffc3-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="bffc3-136">Przejdź do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bffc3-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
