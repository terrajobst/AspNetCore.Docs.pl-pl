---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: getting-started
ms.openlocfilehash: 29a328b610b0a6e1616cd6ebc70a8fa3e515eb92
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861709"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="1d1bd-103">Samouczek: Rozpoczynanie pracy z platformą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d1bd-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="1d1bd-104">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu interfejsu wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="1d1bd-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1d1bd-106">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-106">Create a web app project.</span></span>
> * <span data-ttu-id="1d1bd-107">Włączanie obsługi protokołu HTTPS w lokalnym.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="1d1bd-108">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-108">Run the app.</span></span>
> * <span data-ttu-id="1d1bd-109">Edytuj stronę Razor.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-109">Edit a Razor page.</span></span>

<span data-ttu-id="1d1bd-110">Po zakończeniu będziesz mieć działającą aplikację sieci web uruchomiony na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Strona główna aplikacji sieci Web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="1d1bd-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1d1bd-112">Prerequisites</span></span>

<span data-ttu-id="1d1bd-113">Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="1d1bd-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="1d1bd-114">Tworzenie projektu aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="1d1bd-114">Create a web app project</span></span>

<span data-ttu-id="1d1bd-115">Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="1d1bd-116">Włączanie obsługi protokołu HTTPS w lokalnym</span><span class="sxs-lookup"><span data-stu-id="1d1bd-116">Enable local HTTPS</span></span>

<span data-ttu-id="1d1bd-117">Zaufanie certyfikatu deweloperskiego HTTPS:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1d1bd-118">Windows</span><span class="sxs-lookup"><span data-stu-id="1d1bd-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="1d1bd-119">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-119">The preceding command displays the following dialog:</span></span>

![Okno dialogowe ostrzeżenia o zabezpieczeniach](_static/cert.png)

<span data-ttu-id="1d1bd-121">Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="1d1bd-122">macOS</span><span class="sxs-lookup"><span data-stu-id="1d1bd-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="1d1bd-123">Poprzednie polecenie wyświetli następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="1d1bd-124">*Zażądano ufające certyfikatu deweloperskiego protokołu HTTPS. Jeśli certyfikat nie jest już zaufany możemy uruchom następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
<span data-ttu-id="1d1bd-125">\* To polecenie może monitować o hasło, aby zainstalować certyfikat w pęku kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>

<span data-ttu-id="1d1bd-126">Hasło: \*</span><span class="sxs-lookup"><span data-stu-id="1d1bd-126">Password:\*</span></span>

<span data-ttu-id="1d1bd-127">Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="1d1bd-128">Linux</span><span class="sxs-lookup"><span data-stu-id="1d1bd-128">Linux</span></span>](#tab/linux)

<span data-ttu-id="1d1bd-129">Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

## <a name="run-the-app"></a><span data-ttu-id="1d1bd-130">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1d1bd-130">Run the app</span></span>

<span data-ttu-id="1d1bd-131">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="1d1bd-132">Przejdź do [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="1d1bd-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="1d1bd-133">Kliknij przycisk **Akceptuj**, aby zaakceptować zasady ochrony prywatności i plików cookie.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="1d1bd-134">Ta aplikacja nie zachowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="1d1bd-135">Edytuj stronę Razor</span><span class="sxs-lookup"><span data-stu-id="1d1bd-135">Edit a Razor page</span></span>

<span data-ttu-id="1d1bd-136">Otwórz *Pages/About.cshtml* i zmodyfikuj stronę w wyróżnionym miejscu w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

<span data-ttu-id="1d1bd-137">Przejdź do [https://localhost:5001/About](https://localhost:5001/About) i sprawdź, czy zmiany są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d1bd-138">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="1d1bd-138">Next steps</span></span>

<span data-ttu-id="1d1bd-139">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1d1bd-140">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-140">Create a web app project.</span></span>
> * <span data-ttu-id="1d1bd-141">Włączanie obsługi protokołu HTTPS w lokalnym.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="1d1bd-142">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-142">Run the project.</span></span>
> * <span data-ttu-id="1d1bd-143">Wprowadź zmiany.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-143">Make a change.</span></span>

<span data-ttu-id="1d1bd-144">Aby dowiedzieć się więcej na temat platformy ASP.NET Core, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="1d1bd-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>

> [!NOTE]
> <span data-ttu-id="1d1bd-145">Testujemy użyteczność proponowaną nową strukturę dla platformy ASP.NET Core spis treści.</span><span class="sxs-lookup"><span data-stu-id="1d1bd-145">We're testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span> <span data-ttu-id="1d1bd-146">Jeśli masz kilka minut, aby wypróbować ćwiczenie znajdowanie siedmiu różnych tematów w bieżącej lub proponowanej spis treści, [kliknij tutaj, aby wziąć udział w badaniach](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="1d1bd-146">If you have a few minutes to try an exercise of finding seven different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>
