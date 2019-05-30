---
title: Hostowanie i wdrażanie Blazor
author: guardrex
description: Dowiedz się, jak udostępniać i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 0fc7643c65b93a63d7a594d35e4013eab76e9db8
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376388"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="2d9d6-103">Hostowanie i wdrażanie Blazor</span><span class="sxs-lookup"><span data-stu-id="2d9d6-103">Host and deploy Blazor</span></span>

<span data-ttu-id="2d9d6-104">Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="2d9d6-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="2d9d6-105">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2d9d6-105">Publish the app</span></span>

<span data-ttu-id="2d9d6-106">Aplikacje są publikowane do wdrożenia w konfiguracji wydania.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d9d6-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d9d6-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2d9d6-108">Wybierz **kompilacji** > **publikowania {aplikacja}** na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="2d9d6-109">Wybierz *publikowania docelowej*.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-109">Select the *publish target*.</span></span> <span data-ttu-id="2d9d6-110">Aby opublikować lokalnie, wybierz **folderu**.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="2d9d6-111">Zaakceptuj lokalizację domyślną **wybierz folder** pola lub określić inną lokalizację.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="2d9d6-112">Wybierz **Publikuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-112">Select the **Publish** button.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="2d9d6-113">Program Visual Studio Code / .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="2d9d6-113">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="2d9d6-114">Użyj [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie w celu opublikowania aplikacji przy użyciu konfiguracji wydania:</span><span class="sxs-lookup"><span data-stu-id="2d9d6-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="2d9d6-115">Publikowanie wyzwalacze aplikacji [przywrócić](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompilacje](/dotnet/core/tools/dotnet-build) projekcie przed utworzeniem zasobów dla wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="2d9d6-116">Jako część procesu kompilacji Aby zmniejszyć rozmiar pobierania aplikacji i czasów ładowania są usuwane nieużywane metod i zestawów.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="2d9d6-117">Opublikowaniu aplikacji po stronie klienta Blazor */bin/wersji / {struktury docelowej} /publish/ {Nazwa zestawu} / dist* folderu.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="2d9d6-118">Opublikowaniu aplikacji po stronie serwera Blazor */bin/wersji / {struktury docelowej} / publish* folderu.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="2d9d6-119">Serwer sieci web są wdrażane zasoby w folderze.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="2d9d6-120">Wdrożenie może być ręcznego lub zautomatyzowanego procesu w zależności od poziomu narzędzi deweloperskich w użyciu.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="2d9d6-121">wdrażania</span><span class="sxs-lookup"><span data-stu-id="2d9d6-121">Deployment</span></span>

<span data-ttu-id="2d9d6-122">Aby uzyskać wskazówki dotyczące wdrażania zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="2d9d6-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="2d9d6-123">Blazor bez użycia serwera hostingu z usługą Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2d9d6-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="2d9d6-124">Aplikacje klienta Blazor mogą być udostępniane przez [usługi Azure Storage](https://azure.microsoft.com/services/storage/) jako zawartość statyczną bezpośrednio z kontenera magazynu.</span><span class="sxs-lookup"><span data-stu-id="2d9d6-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="2d9d6-125">Aby uzyskać więcej informacji, zobacz [hosta i wdrażanie Blazor po stronie klienta (wdrożenie autonomicznego): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span><span class="sxs-lookup"><span data-stu-id="2d9d6-125">For more information, see [Host and deploy Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
