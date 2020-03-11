---
title: Narzędzia i pliki do pobrania — metodyki DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Narzędzia i pliki do pobrania wymagane dla metodyki DevOps z platformą ASP.NET Core i platformy Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 25ce311373b0aaddfa3bc2728c39e503acbca69d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659489"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="1e10e-103">Narzędzia i pliki do pobrania</span><span class="sxs-lookup"><span data-stu-id="1e10e-103">Tools and downloads</span></span>

<span data-ttu-id="1e10e-104">Platforma Azure ma kilka interfejsów do aprowizacji zasobów i zarządzania nimi, takich jak [Azure Portal](https://portal.azure.com), [interfejsu wiersza polecenia platformy Azure](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash)i programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e10e-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="1e10e-105">Ten przewodnik minimalistyczny podejście i korzysta z usługi Azure Cloud Shell zawsze, gdy jest to możliwe zmniejszenie kroki wymagane.</span><span class="sxs-lookup"><span data-stu-id="1e10e-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="1e10e-106">Jednak należy użyć witryny Azure portal dla niektórych części.</span><span class="sxs-lookup"><span data-stu-id="1e10e-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e10e-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1e10e-107">Prerequisites</span></span>

<span data-ttu-id="1e10e-108">Wymagane jest spełnienie następujących subskrypcji:</span><span class="sxs-lookup"><span data-stu-id="1e10e-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="1e10e-109">Usługa Azure &mdash;, jeśli nie masz konta, [Uzyskaj bezpłatną wersję próbną](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1e10e-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="1e10e-110">Azure DevOps Services &mdash; subskrypcję usługi Azure DevOps i organizacja została utworzona w rozdziale 4.</span><span class="sxs-lookup"><span data-stu-id="1e10e-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="1e10e-111">GitHub &mdash;, jeśli nie masz konta, [zarejestruj się bezpłatnie](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="1e10e-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="1e10e-112">Wymagane są następujące narzędzia:</span><span class="sxs-lookup"><span data-stu-id="1e10e-112">The following tools are required:</span></span>

* <span data-ttu-id="1e10e-113">Narzędzie [git](https://git-scm.com/downloads) &mdash; w tym przewodniku zalecamy podstawowe zrozumienie usługi git.</span><span class="sxs-lookup"><span data-stu-id="1e10e-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="1e10e-114">Przejrzyj [dokumentację usługi git](https://git-scm.com/doc), w tym [narzędzia Git Remote](https://git-scm.com/docs/git-remote) i [git push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="1e10e-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="1e10e-115">Do kompilowania i uruchamiania przykładowej aplikacji wymagane jest [zestaw .NET Core SDK](https://www.microsoft.com/net/download/) &mdash; w wersji 2.1.300 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="1e10e-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="1e10e-116">Jeśli program Visual Studio jest zainstalowany z obciążeniem **programistycznym dla wielu platform .NET Core** , zestaw .NET Core SDK jest już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="1e10e-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="1e10e-117">Zweryfikuj instalację zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="1e10e-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="1e10e-118">Otwórz powłokę wiersza polecenia, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1e10e-118">Open a command shell, and run the following command:</span></span>

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="1e10e-119">Zalecanych narzędzi (tylko Windows)</span><span class="sxs-lookup"><span data-stu-id="1e10e-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="1e10e-120">Zaawansowane narzędzia platformy Azure dla [programu Visual Studio](https://visualstudio.microsoft.com)zapewniają graficzny interfejs użytkownika dla większości funkcji opisanych w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="1e10e-120">[Visual Studio](https://visualstudio.microsoft.com)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="1e10e-121">W każdej wersji programu Visual Studio będzie działać, łącznie z bezpłatnego programu Visual Studio Community Edition.</span><span class="sxs-lookup"><span data-stu-id="1e10e-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="1e10e-122">Samouczki są zapisywane do zademonstrowania programowania, wdrażania i metodyki DevOps z usługą i bez programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e10e-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="1e10e-123">Upewnij się, że program Visual Studio ma zainstalowane następujące [obciążenia](/visualstudio/install/modify-visual-studio) :</span><span class="sxs-lookup"><span data-stu-id="1e10e-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="1e10e-124">Tworzenie aplikacji na platformie ASP.NET i aplikacji internetowych</span><span class="sxs-lookup"><span data-stu-id="1e10e-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="1e10e-125">Tworzenie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="1e10e-125">Azure development</span></span>
  * <span data-ttu-id="1e10e-126">Tworzenie aplikacji dla wielu platform w środowisku .NET Core</span><span class="sxs-lookup"><span data-stu-id="1e10e-126">.NET Core cross-platform development</span></span>
