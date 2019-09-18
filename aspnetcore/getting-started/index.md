---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Krótki samouczek, który tworzy i uruchamia podstawową aplikację Hello world przy użyciu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/15/2019
uid: getting-started
ms.openlocfilehash: d1edf91f1b37ba2b69732471dc6c1f306ac5ad24
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081129"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="781ab-103">Samouczek: Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="781ab-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="781ab-104">W tym samouczku pokazano, jak utworzyć i uruchomić ASP.NET Core aplikację sieci Web przy użyciu interfejsu wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="781ab-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="781ab-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="781ab-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="781ab-106">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="781ab-106">Create a web app project.</span></span>
> * <span data-ttu-id="781ab-107">Zaufaj certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="781ab-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="781ab-108">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="781ab-108">Run the app.</span></span>
> * <span data-ttu-id="781ab-109">Edytuj stronę Razor.</span><span class="sxs-lookup"><span data-stu-id="781ab-109">Edit a Razor page.</span></span>

<span data-ttu-id="781ab-110">Na końcu będziesz mieć działającą aplikację sieci Web na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="781ab-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Strona główna aplikacji sieci Web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="781ab-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="781ab-112">Prerequisites</span></span>

* [<span data-ttu-id="781ab-113">Zestaw SDK platformy .NET Core 2,2</span><span class="sxs-lookup"><span data-stu-id="781ab-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="781ab-114">Tworzenie projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="781ab-114">Create a web app project</span></span>

<span data-ttu-id="781ab-115">Otwórz powłokę poleceń i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="781ab-115">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a><span data-ttu-id="781ab-116">Ufanie certyfikatowi Deweloperskiemu</span><span class="sxs-lookup"><span data-stu-id="781ab-116">Trust the development certificate</span></span>

<span data-ttu-id="781ab-117">Ufaj certyfikatowi deweloperskim HTTPS:</span><span class="sxs-lookup"><span data-stu-id="781ab-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="781ab-118">Windows</span><span class="sxs-lookup"><span data-stu-id="781ab-118">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="781ab-119">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="781ab-119">The preceding command displays the following dialog:</span></span>

![Okno dialogowe ostrzeżenia o zabezpieczeniach](~/getting-started/_static/cert.png)

<span data-ttu-id="781ab-121">Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="781ab-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="781ab-122">macOS</span><span class="sxs-lookup"><span data-stu-id="781ab-122">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="781ab-123">Poprzednie polecenie wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="781ab-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="781ab-124">*Zażądano zaufania certyfikatu deweloperskiego HTTPS. Jeśli certyfikat nie jest już zaufany, zostanie uruchomione następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="781ab-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="781ab-125">To polecenie może wyświetlać monit o podanie hasła w celu zainstalowania certyfikatu w łańcuchu kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="781ab-125">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="781ab-126">Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="781ab-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="781ab-127">Linux</span><span class="sxs-lookup"><span data-stu-id="781ab-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="781ab-128">W przypadku podsystemu Windows dla systemu Linux Zobacz temat [zaufanie certyfikatu HTTPS z podsystemu Windows dla systemu Linux](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="781ab-128">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="781ab-129">Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="781ab-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="781ab-130">Aby uzyskać więcej informacji, zobacz artykuł [ufanie certyfikatowi Deweloperskiemu protokołu HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="781ab-130">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="781ab-131">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="781ab-131">Run the app</span></span>

<span data-ttu-id="781ab-132">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="781ab-132">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="781ab-133">Po powłoce poleceń wskazuje, że aplikacja została uruchomiona, przejdź do [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="781ab-133">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="781ab-134">Kliknij przycisk **Akceptuj**, aby zaakceptować zasady ochrony prywatności i plików cookie.</span><span class="sxs-lookup"><span data-stu-id="781ab-134">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="781ab-135">Ta aplikacja nie zachowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="781ab-135">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="781ab-136">Edytowanie strony Razor</span><span class="sxs-lookup"><span data-stu-id="781ab-136">Edit a Razor page</span></span>

<span data-ttu-id="781ab-137">Otwórz stronę *Pages/index. cshtml* i zmodyfikuj ją, korzystając z następującego wyróżnionego znacznika:</span><span class="sxs-lookup"><span data-stu-id="781ab-137">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="781ab-138">Przejdź do [https://localhost:5001](https://localhost:5001)okna i sprawdź, czy zmiany są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="781ab-138">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="781ab-139">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="781ab-139">Next steps</span></span>

<span data-ttu-id="781ab-140">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="781ab-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="781ab-141">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="781ab-141">Create a web app project.</span></span>
> * <span data-ttu-id="781ab-142">Zaufaj certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="781ab-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="781ab-143">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="781ab-143">Run the project.</span></span>
> * <span data-ttu-id="781ab-144">Wprowadź zmianę.</span><span class="sxs-lookup"><span data-stu-id="781ab-144">Make a change.</span></span>

<span data-ttu-id="781ab-145">Aby dowiedzieć się więcej na temat ASP.NET Core, zapoznaj się ze zalecaną ścieżką szkoleniową we wprowadzeniu:</span><span class="sxs-lookup"><span data-stu-id="781ab-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
