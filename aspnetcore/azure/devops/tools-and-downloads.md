---
title: Metodyka DevOps z platformą ASP.NET Core i platformy Azure | Narzędzia i pliki do pobrania
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 5529068b83db475315784571fbf4151d7ecd0d5d
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340163"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="431c1-103">Narzędzia i pliki do pobrania</span><span class="sxs-lookup"><span data-stu-id="431c1-103">Tools and downloads</span></span>

<span data-ttu-id="431c1-104">Platforma Azure oferuje kilka interfejsów do inicjowania obsługi administracyjnej zasobów i zarządzania, takich jak [witryny Azure portal](https://portal.azure.com), [wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/), [programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [w chmurze platformy Azure Shell](https://shell.azure.com/bash)i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="431c1-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="431c1-105">Ten przewodnik minimalistyczny podejście i korzysta z usługi Azure Cloud Shell zawsze, gdy jest to możliwe zmniejszenie kroki wymagane.</span><span class="sxs-lookup"><span data-stu-id="431c1-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="431c1-106">Jednak należy użyć witryny Azure portal dla niektórych części.</span><span class="sxs-lookup"><span data-stu-id="431c1-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="431c1-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="431c1-107">Prerequisites</span></span>

<span data-ttu-id="431c1-108">Wymagane jest spełnienie następujących subskrypcji:</span><span class="sxs-lookup"><span data-stu-id="431c1-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="431c1-109">Azure &mdash; Jeśli nie masz konta, [bezpłatna wersja próbna](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="431c1-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="431c1-110">Usługi Azure DevOps &mdash; subskrypcji DevOps platformy Azure i organizacji jest tworzony w rozdziale 4.</span><span class="sxs-lookup"><span data-stu-id="431c1-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="431c1-111">GitHub &mdash; Jeśli nie masz konta, [Utwórz bezpłatne konto](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="431c1-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="431c1-112">Wymagane są następujące narzędzia:</span><span class="sxs-lookup"><span data-stu-id="431c1-112">The following tools are required:</span></span>

* <span data-ttu-id="431c1-113">[Git](https://git-scm.com/downloads) &mdash; powinieneś rozumieć podstawy systemu Git jest zalecane w przypadku tego przewodnika.</span><span class="sxs-lookup"><span data-stu-id="431c1-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="431c1-114">Przegląd [dokumentacja usługi Git](https://git-scm.com/doc), konkretnie [git zdalnego](https://git-scm.com/docs/git-remote) i [wypchnięcia narzędzia git](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="431c1-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="431c1-115">[Zestaw .NET core SDK](https://www.microsoft.com/net/download/) &mdash; wersji 2.1.300 lub nowszy jest wymagany do kompilowania i uruchamiania przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="431c1-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="431c1-116">Jeśli zainstalowano program Visual Studio za pomocą **programowanie dla wielu platform .NET Core** obciążenia .NET Core SDK jest już zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="431c1-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="431c1-117">Zweryfikuj instalację zestawu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="431c1-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="431c1-118">Otwórz powłokę wiersza polecenia, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="431c1-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="431c1-119">Zalecanych narzędzi (tylko Windows)</span><span class="sxs-lookup"><span data-stu-id="431c1-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="431c1-120">[Program Visual Studio](https://www.visualstudio.com/)użytkownika niezawodne narzędzia platformy Azure zapewnia graficzny interfejs użytkownika dla większości funkcji opisanych w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="431c1-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="431c1-121">W każdej wersji programu Visual Studio będzie działać, łącznie z bezpłatnego programu Visual Studio Community Edition.</span><span class="sxs-lookup"><span data-stu-id="431c1-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="431c1-122">Samouczki są zapisywane do zademonstrowania programowania, wdrażania i metodyki DevOps z usługą i bez programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="431c1-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="431c1-123">Upewnij się, że program Visual Studio zawiera następujące [obciążeń](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="431c1-123">Confirm that Visual Studio has the following [workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="431c1-124">ASP.NET i tworzenie aplikacji internetowych</span><span class="sxs-lookup"><span data-stu-id="431c1-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="431c1-125">Programowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="431c1-125">Azure development</span></span>
  * <span data-ttu-id="431c1-126">Programowanie dla wielu platform .NET core</span><span class="sxs-lookup"><span data-stu-id="431c1-126">.NET Core cross-platform development</span></span>
