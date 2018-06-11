---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 5/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 0b5de87b99e57df16f663f13d41e750a29ea6276
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252051"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="05905-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05905-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="05905-104">Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="05905-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="05905-105">Tworzenie projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05905-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="05905-106">Otwórz powłokę wiersza polecenia i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="05905-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="05905-108">Ufać certyfikatowi programowanie HTTPS:</span><span class="sxs-lookup"><span data-stu-id="05905-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="05905-109">Windows</span><span class="sxs-lookup"><span data-stu-id="05905-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](getting-started/_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="05905-110">macOS</span><span class="sxs-lookup"><span data-stu-id="05905-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="05905-111">Linux</span><span class="sxs-lookup"><span data-stu-id="05905-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="05905-112">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="05905-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="05905-113">Przejdź do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="05905-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="05905-114">Kliknij przycisk **Akceptuj** do Zaakceptuj zasady ochrony prywatności i plików cookie.</span><span class="sxs-lookup"><span data-stu-id="05905-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="05905-115">Ta aplikacja nie zachowanie poufności informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="05905-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="05905-116">Otwórz *Pages/About.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="05905-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    <span data-ttu-id="05905-117">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="05905-117">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9)]</span></span>

7. <span data-ttu-id="05905-118">Przejdź do [ http://localhost:5001/About ](http://localhost:5001/About) i Sprawdź zmiany są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="05905-118">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="05905-119">Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="05905-119">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="05905-120">Utwórz nowy projekt platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05905-120">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="05905-121">Otwórz powłokę poleceń.</span><span class="sxs-lookup"><span data-stu-id="05905-121">Open a command shell.</span></span> <span data-ttu-id="05905-122">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="05905-122">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="05905-123">Uruchom aplikację za pomocą następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="05905-123">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="05905-124">Przejdź do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="05905-124">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="05905-125">Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world!</span><span class="sxs-lookup"><span data-stu-id="05905-125">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="05905-126">Czas na serwerze jest @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="05905-126">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="05905-127">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="05905-127">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="05905-128">Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) i zweryfikować zmiany.</span><span class="sxs-lookup"><span data-stu-id="05905-128">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="05905-129">Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="05905-129">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="05905-130">Utwórz folder dla nowego projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05905-130">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="05905-131">Otwórz powłokę poleceń.</span><span class="sxs-lookup"><span data-stu-id="05905-131">Open a command shell.</span></span> <span data-ttu-id="05905-132">Wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="05905-132">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="05905-133">Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* pliku, aby wybrać 1.0.4 zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="05905-133">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="05905-134">Utwórz nowy projekt platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05905-134">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="05905-135">Przywracanie pakietów.</span><span class="sxs-lookup"><span data-stu-id="05905-135">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="05905-136">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="05905-136">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="05905-137">[Dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="05905-137">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="05905-138">Przejdź do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="05905-138">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
