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
ms.openlocfilehash: a7233082e9262e6976cec3b086900a5dda069213
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730441"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="3f869-103">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f869-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="3f869-104">Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="3f869-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="3f869-105">Tworzenie projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f869-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="3f869-106">Otwórz powłokę wiersza polecenia i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3f869-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

3. <span data-ttu-id="3f869-107">Ufać certyfikatowi programowanie HTTPS:</span><span class="sxs-lookup"><span data-stu-id="3f869-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="3f869-108">Windows</span><span class="sxs-lookup"><span data-stu-id="3f869-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](getting-started/_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="3f869-109">macOS</span><span class="sxs-lookup"><span data-stu-id="3f869-109">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="3f869-110">Linux</span><span class="sxs-lookup"><span data-stu-id="3f869-110">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="3f869-111">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="3f869-111">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="3f869-112">Przejdź do [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="3f869-112">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="3f869-113">Kliknij przycisk **Akceptuj** do Zaakceptuj zasady ochrony prywatności i plików cookie.</span><span class="sxs-lookup"><span data-stu-id="3f869-113">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="3f869-114">Ta aplikacja nie zachowanie poufności informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="3f869-114">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="3f869-115">Otwórz *Pages/About.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="3f869-115">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    <span data-ttu-id="3f869-116">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="3f869-116">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9)]</span></span>

7. <span data-ttu-id="3f869-117">Przejdź do [ http://localhost:5001/About ](http://localhost:5001/About) i Sprawdź zmiany są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="3f869-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="3f869-118">Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="3f869-118">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="3f869-119">Utwórz nowy projekt platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f869-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="3f869-120">Otwórz powłokę poleceń.</span><span class="sxs-lookup"><span data-stu-id="3f869-120">Open a command shell.</span></span> <span data-ttu-id="3f869-121">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3f869-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="3f869-122">Uruchom aplikację za pomocą następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="3f869-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="3f869-123">Przejdź do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="3f869-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="3f869-124">Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world!</span><span class="sxs-lookup"><span data-stu-id="3f869-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="3f869-125">Czas na serwerze jest @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="3f869-125">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="3f869-126">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="3f869-126">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="3f869-127">Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) i zweryfikować zmiany.</span><span class="sxs-lookup"><span data-stu-id="3f869-127">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="3f869-128">Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="3f869-128">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="3f869-129">Utwórz folder dla nowego projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f869-129">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="3f869-130">Otwórz powłokę poleceń.</span><span class="sxs-lookup"><span data-stu-id="3f869-130">Open a command shell.</span></span> <span data-ttu-id="3f869-131">Wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="3f869-131">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="3f869-132">Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* pliku, aby wybrać 1.0.4 zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="3f869-132">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="3f869-133">Utwórz nowy projekt platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f869-133">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="3f869-134">Przywracanie pakietów.</span><span class="sxs-lookup"><span data-stu-id="3f869-134">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="3f869-135">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="3f869-135">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="3f869-136">[Dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="3f869-136">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="3f869-137">Przejdź do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3f869-137">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
