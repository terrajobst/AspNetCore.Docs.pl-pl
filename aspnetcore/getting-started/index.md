---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Krótki samouczek, który tworzy i uruchamia podstawową aplikację Hello world przy użyciu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: c806bd1e79dea9119f1c9e99d0a2b9742a10987a
ms.sourcegitcommit: ef1720cb733908f36a54825d84c3461c5280bdbe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737480"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="d292f-103">Samouczek: wprowadzenie do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d292f-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="d292f-104">W tym samouczku pokazano, jak utworzyć i uruchomić ASP.NET Core aplikację sieci Web przy użyciu interfejsu wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d292f-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="d292f-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="d292f-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d292f-106">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d292f-106">Create a web app project.</span></span>
> * <span data-ttu-id="d292f-107">Zaufaj certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="d292f-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="d292f-108">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d292f-108">Run the app.</span></span>
> * <span data-ttu-id="d292f-109">Edytuj stronę Razor.</span><span class="sxs-lookup"><span data-stu-id="d292f-109">Edit a Razor page.</span></span>

<span data-ttu-id="d292f-110">Na końcu będziesz mieć działającą aplikację sieci Web na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="d292f-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Strona główna aplikacji internetowej](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="d292f-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d292f-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="d292f-113">Tworzenie projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="d292f-113">Create a web app project</span></span>

<span data-ttu-id="d292f-114">Otwórz powłokę poleceń i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d292f-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="d292f-115">Poprzednie polecenie:</span><span class="sxs-lookup"><span data-stu-id="d292f-115">The preceding command:</span></span>

* <span data-ttu-id="d292f-116">Tworzy nową aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d292f-116">Creates a new web app.</span></span>  
* <span data-ttu-id="d292f-117">Parametr `-o aspnetcoreapp` tworzy katalog o nazwie *aspnetcoreapp* z plikami źródłowymi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d292f-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="d292f-118">Ufanie certyfikatowi Deweloperskiemu</span><span class="sxs-lookup"><span data-stu-id="d292f-118">Trust the development certificate</span></span>

<span data-ttu-id="d292f-119">Ufaj certyfikatowi deweloperskim HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d292f-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="d292f-120">Windows</span><span class="sxs-lookup"><span data-stu-id="d292f-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="d292f-121">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="d292f-121">The preceding command displays the following dialog:</span></span>

![Okno dialogowe ostrzeżenia o zabezpieczeniach](~/getting-started/_static/cert.png)

<span data-ttu-id="d292f-123">Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="d292f-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d292f-124">macOS</span><span class="sxs-lookup"><span data-stu-id="d292f-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="d292f-125">Poprzednie polecenie wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="d292f-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="d292f-126">*Zażądano zaufania certyfikatu deweloperskiego https. Jeśli certyfikat nie jest już zaufany, zostanie uruchomione następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="d292f-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted, we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="d292f-127">To polecenie może wyświetlać monit o podanie hasła w celu zainstalowania certyfikatu w łańcuchu kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="d292f-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="d292f-128">Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="d292f-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d292f-129">Linux</span><span class="sxs-lookup"><span data-stu-id="d292f-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="d292f-130">Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d292f-130">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="d292f-131">Aby uzyskać więcej informacji, zobacz artykuł [ufanie certyfikatowi Deweloperskiemu protokołu HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="d292f-131">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d292f-132">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d292f-132">Run the app</span></span>

<span data-ttu-id="d292f-133">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d292f-133">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="d292f-134">Po powłoce poleceń wskazuje, że aplikacja została uruchomiona, przejdź do [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="d292f-134">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="d292f-135">Edytowanie strony Razor</span><span class="sxs-lookup"><span data-stu-id="d292f-135">Edit a Razor page</span></span>

<span data-ttu-id="d292f-136">Otwórz stronę *Pages/index. cshtml* i zmodyfikuj ją i Zapisz przy użyciu następującego wyróżnionego znacznika:</span><span class="sxs-lookup"><span data-stu-id="d292f-136">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="d292f-137">Przejdź do [https://localhost:5001](https://localhost:5001), Odśwież stronę i sprawdź, czy są wyświetlane zmiany.</span><span class="sxs-lookup"><span data-stu-id="d292f-137">Browse to [https://localhost:5001](https://localhost:5001), refresh the page, and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d292f-138">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d292f-138">Next steps</span></span>

<span data-ttu-id="d292f-139">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="d292f-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d292f-140">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d292f-140">Create a web app project.</span></span>
> * <span data-ttu-id="d292f-141">Zaufaj certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="d292f-141">Trust the development certificate.</span></span>
> * <span data-ttu-id="d292f-142">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="d292f-142">Run the project.</span></span>
> * <span data-ttu-id="d292f-143">Wprowadź zmianę.</span><span class="sxs-lookup"><span data-stu-id="d292f-143">Make a change.</span></span>

<span data-ttu-id="d292f-144">Aby dowiedzieć się więcej na temat ASP.NET Core, zapoznaj się ze zalecaną ścieżką szkoleniową we wprowadzeniu:</span><span class="sxs-lookup"><span data-stu-id="d292f-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
