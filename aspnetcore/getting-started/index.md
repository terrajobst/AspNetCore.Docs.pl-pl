---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4c899ff3c087f1b569521c6e2e8458fca01d6061
ms.sourcegitcommit: bdfba5e7575b2a786ef27c0edf688c7dbd09ee95
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52288645"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="ee794-103">Samouczek: Rozpoczynanie pracy z platformą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee794-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="ee794-104">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu interfejsu wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee794-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span> <span data-ttu-id="ee794-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="ee794-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee794-106">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ee794-106">Create a web app project.</span></span>
> * <span data-ttu-id="ee794-107">Włączanie obsługi protokołu HTTPS w lokalnym.</span><span class="sxs-lookup"><span data-stu-id="ee794-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="ee794-108">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ee794-108">Run the app.</span></span>
> * <span data-ttu-id="ee794-109">Edytuj stronę Razor.</span><span class="sxs-lookup"><span data-stu-id="ee794-109">Edit a Razor page.</span></span>

<span data-ttu-id="ee794-110">Po zakończeniu będziesz mieć działającą aplikację sieci web uruchomiony na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="ee794-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Strona główna aplikacji sieci Web](_static/home-page.png)


## <a name="prerequisites"></a><span data-ttu-id="ee794-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ee794-112">Prerequisites</span></span>

* <span data-ttu-id="ee794-113">Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="ee794-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="ee794-114">Tworzenie projektu aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="ee794-114">Create a web app project</span></span>

* <span data-ttu-id="ee794-115">Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ee794-115">Open a command shell, and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a><span data-ttu-id="ee794-116">Włączanie obsługi protokołu HTTPS w lokalnym</span><span class="sxs-lookup"><span data-stu-id="ee794-116">Enable local HTTPS</span></span>

* <span data-ttu-id="ee794-117">Zaufanie certyfikatu deweloperskiego HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ee794-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ee794-118">Windows</span><span class="sxs-lookup"><span data-stu-id="ee794-118">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="ee794-119">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="ee794-119">The preceding command displays the following dialog:</span></span>

  ![Okno dialogowe ostrzeżenia o zabezpieczeniach](_static/cert.png)

  <span data-ttu-id="ee794-121">Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="ee794-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ee794-122">macOS</span><span class="sxs-lookup"><span data-stu-id="ee794-122">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="ee794-123">Poprzednie polecenie wyświetli następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="ee794-123">The preceding command displays the following message:</span></span>

  <span data-ttu-id="ee794-124">*Zażądano ufające certyfikatu deweloperskiego protokołu HTTPS. Jeśli certyfikat nie jest już zaufany możemy uruchom następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="ee794-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="ee794-125">\* To polecenie może monitować o hasło, aby zainstalować certyfikat w pęku kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="ee794-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="ee794-126">Hasło: \*</span><span class="sxs-lookup"><span data-stu-id="ee794-126">Password:\*</span></span>

  <span data-ttu-id="ee794-127">Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="ee794-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ee794-128">Linux</span><span class="sxs-lookup"><span data-stu-id="ee794-128">Linux</span></span>](#tab/linux)

  <span data-ttu-id="ee794-129">Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ee794-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

## <a name="run-the-app"></a><span data-ttu-id="ee794-130">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ee794-130">Run the app</span></span>

* <span data-ttu-id="ee794-131">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ee794-131">Run the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* <span data-ttu-id="ee794-132">Przejdź do [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="ee794-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="ee794-133">Kliknij przycisk **Akceptuj**, aby zaakceptować zasady ochrony prywatności i plików cookie.</span><span class="sxs-lookup"><span data-stu-id="ee794-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="ee794-134">Ta aplikacja nie zachowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="ee794-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="ee794-135">Edytuj stronę Razor</span><span class="sxs-lookup"><span data-stu-id="ee794-135">Edit a Razor page</span></span>

* <span data-ttu-id="ee794-136">Otwórz *Pages/About.cshtml* i zmodyfikuj stronę w wyróżnionym miejscu w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="ee794-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* <span data-ttu-id="ee794-137">Przejdź do [https://localhost:5001/About](https://localhost:5001/About) i sprawdź, czy zmiany są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="ee794-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee794-138">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ee794-138">Next steps</span></span>

<span data-ttu-id="ee794-139">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="ee794-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee794-140">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ee794-140">Create a web app project.</span></span>
> * <span data-ttu-id="ee794-141">Włączanie obsługi protokołu HTTPS w lokalnym.</span><span class="sxs-lookup"><span data-stu-id="ee794-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="ee794-142">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="ee794-142">Run the project.</span></span>
> * <span data-ttu-id="ee794-143">Wprowadź zmiany.</span><span class="sxs-lookup"><span data-stu-id="ee794-143">Make a change.</span></span>

<span data-ttu-id="ee794-144">Aby dowiedzieć się więcej na temat platformy ASP.NET Core, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="ee794-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>



> [!NOTE]
> <span data-ttu-id="ee794-145">Testujemy użyteczność proponowaną nową strukturę dla platformy ASP.NET Core spis treści.</span><span class="sxs-lookup"><span data-stu-id="ee794-145">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="ee794-146">Jeśli masz kilka minut, spróbuj wykonywania, znajdowanie 7 różne tematy w bieżącej lub proponowanej spis treści, [kliknij tutaj, aby wziąć udział w badaniach](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="ee794-146">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>