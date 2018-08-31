---
title: Metodyka DevOps z platformą ASP.NET Core i platformy Azure | Narzędzia i pliki do pobrania
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: a63e97d9ab9eb0ed2fbd30e8c2e033f0c048d33e
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312304"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="524e3-103">Narzędzia i pliki do pobrania</span><span class="sxs-lookup"><span data-stu-id="524e3-103">Tools and downloads</span></span>

<span data-ttu-id="524e3-104">Platforma Azure oferuje kilka interfejsów do inicjowania obsługi administracyjnej zasobów i zarządzania, takich jak [witryny Azure portal](https://portal.azure.com), [wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/), [programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [w chmurze platformy Azure Shell](https://shell.azure.com/bash)i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="524e3-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="524e3-105">Ten przewodnik minimalistyczny podejście i korzysta z usługi Azure Cloud Shell zawsze, gdy jest to możliwe zmniejszenie kroki wymagane.</span><span class="sxs-lookup"><span data-stu-id="524e3-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="524e3-106">Jednak należy użyć witryny Azure portal dla niektórych części.</span><span class="sxs-lookup"><span data-stu-id="524e3-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="524e3-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="524e3-107">Prerequisites</span></span>

<span data-ttu-id="524e3-108">Wymagane jest spełnienie następujących subskrypcji:</span><span class="sxs-lookup"><span data-stu-id="524e3-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="524e3-109">Azure &mdash; Jeśli nie masz konta, [bezpłatna wersja próbna](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="524e3-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="524e3-110">Visual Studio Team Services (VSTS) &mdash; to konto jest tworzone w rozdziale 4.</span><span class="sxs-lookup"><span data-stu-id="524e3-110">Visual Studio Team Services (VSTS) &mdash; This account is created in Chapter 4.</span></span>
* <span data-ttu-id="524e3-111">GitHub &mdash; Jeśli nie masz konta, [Utwórz bezpłatne konto](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="524e3-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="524e3-112">Wymagane są następujące narzędzia:</span><span class="sxs-lookup"><span data-stu-id="524e3-112">The following tools are required:</span></span>

* <span data-ttu-id="524e3-113">[Git](https://git-scm.com/downloads) &mdash; powinieneś rozumieć podstawy systemu Git jest zalecane w przypadku tego przewodnika.</span><span class="sxs-lookup"><span data-stu-id="524e3-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="524e3-114">Przegląd [dokumentacja usługi Git](https://git-scm.com/doc), konkretnie [git zdalnego](https://git-scm.com/docs/git-remote) i [wypchnięcia narzędzia git](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="524e3-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="524e3-115">[Zestaw .NET core SDK](https://www.microsoft.com/net/download/) &mdash; wersji 2.1.300 lub nowszy jest wymagany do kompilowania i uruchamiania przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="524e3-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="524e3-116">Jeśli zainstalowano program Visual Studio za pomocą **programowanie dla wielu platform .NET Core** obciążenia .NET Core SDK jest już zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="524e3-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="524e3-117">Zweryfikuj instalację zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="524e3-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="524e3-118">Otwórz powłokę wiersza polecenia, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="524e3-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="524e3-119">Zalecanych narzędzi (tylko Windows)</span><span class="sxs-lookup"><span data-stu-id="524e3-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="524e3-120">[Program Visual Studio](https://www.visualstudio.com/)użytkownika niezawodne narzędzia platformy Azure zapewnia graficzny interfejs użytkownika dla większości funkcji opisanych w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="524e3-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="524e3-121">W każdej wersji programu Visual Studio będzie działać, łącznie z bezpłatnego programu Visual Studio Community Edition.</span><span class="sxs-lookup"><span data-stu-id="524e3-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="524e3-122">Samouczki są zapisywane do zademonstrowania programowania, wdrażania i metodyki DevOps z usługą i bez programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="524e3-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="524e3-123">Upewnij się, że program Visual Studio zawiera następujące [obciążeń](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="524e3-123">Confirm that Visual Studio has the following [workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="524e3-124">ASP.NET i tworzenie aplikacji internetowych</span><span class="sxs-lookup"><span data-stu-id="524e3-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="524e3-125">Programowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="524e3-125">Azure development</span></span>
  * <span data-ttu-id="524e3-126">Programowanie dla wielu platform .NET core</span><span class="sxs-lookup"><span data-stu-id="524e3-126">.NET Core cross-platform development</span></span>
