---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Krótki samouczek, który tworzy i uruchamia podstawową aplikację Hello world przy użyciu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: 798f1ee87c05d886d8991e3f0230c8ebc6341ba8
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925104"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="21d75-103">Samouczek: Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21d75-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="21d75-104">W tym samouczku pokazano, jak utworzyć i uruchomić ASP.NET Core aplikację sieci Web przy użyciu interfejsu wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="21d75-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="21d75-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="21d75-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21d75-106">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="21d75-106">Create a web app project.</span></span>
> * <span data-ttu-id="21d75-107">Zaufaj certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="21d75-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="21d75-108">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="21d75-108">Run the app.</span></span>
> * <span data-ttu-id="21d75-109">Edytuj stronę Razor.</span><span class="sxs-lookup"><span data-stu-id="21d75-109">Edit a Razor page.</span></span>

<span data-ttu-id="21d75-110">Na końcu będziesz mieć działającą aplikację sieci Web na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="21d75-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Strona główna aplikacji sieci Web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="21d75-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="21d75-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="21d75-113">Tworzenie projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="21d75-113">Create a web app project</span></span>

<span data-ttu-id="21d75-114">Otwórz powłokę poleceń i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="21d75-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="21d75-115">Poprzednie polecenie:</span><span class="sxs-lookup"><span data-stu-id="21d75-115">The preceding command:</span></span>

* <span data-ttu-id="21d75-116">Tworzy nową aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="21d75-116">Creates a new web app.</span></span>  
* <span data-ttu-id="21d75-117">Parametr tworzy katalog o nazwie aspnetcoreapp z plikami źródłowymi aplikacji. `-o aspnetcoreapp`</span><span class="sxs-lookup"><span data-stu-id="21d75-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="21d75-118">Ufanie certyfikatowi Deweloperskiemu</span><span class="sxs-lookup"><span data-stu-id="21d75-118">Trust the development certificate</span></span>

<span data-ttu-id="21d75-119">Ufaj certyfikatowi deweloperskim HTTPS:</span><span class="sxs-lookup"><span data-stu-id="21d75-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="21d75-120">Windows</span><span class="sxs-lookup"><span data-stu-id="21d75-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="21d75-121">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="21d75-121">The preceding command displays the following dialog:</span></span>

![Okno dialogowe ostrzeżenia o zabezpieczeniach](~/getting-started/_static/cert.png)

<span data-ttu-id="21d75-123">Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="21d75-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="21d75-124">macOS</span><span class="sxs-lookup"><span data-stu-id="21d75-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="21d75-125">Poprzednie polecenie wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="21d75-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="21d75-126">*Zażądano zaufania certyfikatu deweloperskiego HTTPS. Jeśli certyfikat nie jest już zaufany, zostanie uruchomione następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="21d75-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="21d75-127">To polecenie może wyświetlać monit o podanie hasła w celu zainstalowania certyfikatu w łańcuchu kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="21d75-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="21d75-128">Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="21d75-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="21d75-129">Linux</span><span class="sxs-lookup"><span data-stu-id="21d75-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="21d75-130">W przypadku podsystemu Windows dla systemu Linux Zobacz temat [zaufanie certyfikatu HTTPS z podsystemu Windows dla systemu Linux](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="21d75-130">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="21d75-131">Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="21d75-131">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="21d75-132">Aby uzyskać więcej informacji, zobacz artykuł [ufanie certyfikatowi Deweloperskiemu protokołu HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="21d75-132">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="21d75-133">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="21d75-133">Run the app</span></span>

<span data-ttu-id="21d75-134">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="21d75-134">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="21d75-135">Po powłoce poleceń wskazuje, że aplikacja została uruchomiona, przejdź do [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="21d75-135">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="21d75-136">Edytowanie strony Razor</span><span class="sxs-lookup"><span data-stu-id="21d75-136">Edit a Razor page</span></span>

<span data-ttu-id="21d75-137">Otwórz stronę *Pages/index. cshtml* i zmodyfikuj ją i Zapisz przy użyciu następującego wyróżnionego znacznika:</span><span class="sxs-lookup"><span data-stu-id="21d75-137">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="21d75-138">Przejdź do [https://localhost:5001](https://localhost:5001)strony, Odśwież stronę i sprawdź, czy są wyświetlane zmiany.</span><span class="sxs-lookup"><span data-stu-id="21d75-138">Browse to [https://localhost:5001](https://localhost:5001), refresh the page and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21d75-139">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="21d75-139">Next steps</span></span>

<span data-ttu-id="21d75-140">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="21d75-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21d75-141">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="21d75-141">Create a web app project.</span></span>
> * <span data-ttu-id="21d75-142">Zaufaj certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="21d75-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="21d75-143">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="21d75-143">Run the project.</span></span>
> * <span data-ttu-id="21d75-144">Wprowadź zmianę.</span><span class="sxs-lookup"><span data-stu-id="21d75-144">Make a change.</span></span>

<span data-ttu-id="21d75-145">Aby dowiedzieć się więcej na temat ASP.NET Core, zapoznaj się ze zalecaną ścieżką szkoleniową we wprowadzeniu:</span><span class="sxs-lookup"><span data-stu-id="21d75-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
