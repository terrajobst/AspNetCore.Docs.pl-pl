---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Krótki samouczek, który tworzy i uruchamia podstawową aplikację Hello world przy użyciu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: 047fd7a74d3d53f68a730d67b63c65fe6bda529f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658467"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="a14ad-103">Samouczek: wprowadzenie do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a14ad-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="a14ad-104">W tym samouczku pokazano, jak utworzyć i uruchomić aplikację sieci Web ASP.NET Core przy użyciu interfejs wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a14ad-104">This tutorial shows how to create and run an ASP.NET Core web app using the .NET Core CLI.</span></span>

<span data-ttu-id="a14ad-105">Omawiane tematy:</span><span class="sxs-lookup"><span data-stu-id="a14ad-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a14ad-106">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a14ad-106">Create a web app project.</span></span>
> * <span data-ttu-id="a14ad-107">Zaufaj certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="a14ad-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="a14ad-108">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="a14ad-108">Run the app.</span></span>
> * <span data-ttu-id="a14ad-109">Edytuj stronę Razor.</span><span class="sxs-lookup"><span data-stu-id="a14ad-109">Edit a Razor page.</span></span>

<span data-ttu-id="a14ad-110">Na końcu będziesz mieć działającą aplikację sieci Web na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="a14ad-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Strona główna aplikacji sieci Web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="a14ad-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a14ad-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="a14ad-113">Tworzenie projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="a14ad-113">Create a web app project</span></span>

<span data-ttu-id="a14ad-114">Otwórz powłokę poleceń i wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a14ad-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="a14ad-115">Poprzednie polecenie:</span><span class="sxs-lookup"><span data-stu-id="a14ad-115">The preceding command:</span></span>

* <span data-ttu-id="a14ad-116">Tworzy nową aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a14ad-116">Creates a new web app.</span></span>  
* <span data-ttu-id="a14ad-117">Parametr `-o aspnetcoreapp` tworzy katalog o nazwie *aspnetcoreapp* z plikami źródłowymi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a14ad-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="a14ad-118">Ufanie certyfikatowi Deweloperskiemu</span><span class="sxs-lookup"><span data-stu-id="a14ad-118">Trust the development certificate</span></span>

<span data-ttu-id="a14ad-119">Ufaj certyfikatowi deweloperskim HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a14ad-119">Trust the HTTPS development certificate:</span></span>

# <a name="windows"></a>[<span data-ttu-id="a14ad-120">Windows</span><span class="sxs-lookup"><span data-stu-id="a14ad-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="a14ad-121">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="a14ad-121">The preceding command displays the following dialog:</span></span>

![Okno dialogowe ostrzeżenia o zabezpieczeniach](~/getting-started/_static/cert.png)

<span data-ttu-id="a14ad-123">Wybierz opcję **tak** , jeśli zgadzasz się ufać certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="a14ad-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macos"></a>[<span data-ttu-id="a14ad-124">macOS</span><span class="sxs-lookup"><span data-stu-id="a14ad-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="a14ad-125">Poprzednie polecenie wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="a14ad-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="a14ad-126">*Zażądano zaufania certyfikatu deweloperskiego https. Jeśli certyfikat nie jest już zaufany, zostanie uruchomione następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="a14ad-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted, we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="a14ad-127">To polecenie może wyświetlać monit o podanie hasła w celu zainstalowania certyfikatu w łańcuchu kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="a14ad-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="a14ad-128">Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="a14ad-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linux"></a>[<span data-ttu-id="a14ad-129">Linux</span><span class="sxs-lookup"><span data-stu-id="a14ad-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="a14ad-130">Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a14ad-130">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="a14ad-131">Aby uzyskać więcej informacji, zobacz artykuł [ufanie certyfikatowi Deweloperskiemu protokołu HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="a14ad-131">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a14ad-132">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="a14ad-132">Run the app</span></span>

<span data-ttu-id="a14ad-133">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a14ad-133">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="a14ad-134">Po powłoce poleceń wskazuje, że aplikacja została uruchomiona, przejdź do [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="a14ad-134">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="a14ad-135">Edytowanie strony Razor</span><span class="sxs-lookup"><span data-stu-id="a14ad-135">Edit a Razor page</span></span>

<span data-ttu-id="a14ad-136">Otwórz stronę *Pages/index. cshtml* i zmodyfikuj ją i Zapisz przy użyciu następującego wyróżnionego znacznika:</span><span class="sxs-lookup"><span data-stu-id="a14ad-136">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="a14ad-137">Przejdź do [https://localhost:5001](https://localhost:5001), Odśwież stronę i sprawdź, czy są wyświetlane zmiany.</span><span class="sxs-lookup"><span data-stu-id="a14ad-137">Browse to [https://localhost:5001](https://localhost:5001), refresh the page, and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a14ad-138">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="a14ad-138">Next steps</span></span>

<span data-ttu-id="a14ad-139">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a14ad-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a14ad-140">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a14ad-140">Create a web app project.</span></span>
> * <span data-ttu-id="a14ad-141">Zaufaj certyfikatowi Deweloperskiemu.</span><span class="sxs-lookup"><span data-stu-id="a14ad-141">Trust the development certificate.</span></span>
> * <span data-ttu-id="a14ad-142">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="a14ad-142">Run the project.</span></span>
> * <span data-ttu-id="a14ad-143">Wprowadź zmianę.</span><span class="sxs-lookup"><span data-stu-id="a14ad-143">Make a change.</span></span>

<span data-ttu-id="a14ad-144">Aby dowiedzieć się więcej na temat ASP.NET Core, zapoznaj się ze zalecaną ścieżką szkoleniową we wprowadzeniu:</span><span class="sxs-lookup"><span data-stu-id="a14ad-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
