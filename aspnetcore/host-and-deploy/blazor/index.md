---
title: Hostowanie i wdrażanie ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: d18abbf33c71dca5130bfc6b503b46c1d5bce537
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913925"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="c26ae-103">Hostowanie i wdrażanie ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="c26ae-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="c26ae-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c26ae-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="c26ae-105">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c26ae-105">Publish the app</span></span>

<span data-ttu-id="c26ae-106">Aplikacje są publikowane do wdrożenia w konfiguracji wydania.</span><span class="sxs-lookup"><span data-stu-id="c26ae-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c26ae-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c26ae-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c26ae-108">Wybierz pozycję **kompilacja** > **Opublikuj aplikację {aplikacja}** na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="c26ae-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="c26ae-109">Wybierz *element docelowy publikowania*.</span><span class="sxs-lookup"><span data-stu-id="c26ae-109">Select the *publish target*.</span></span> <span data-ttu-id="c26ae-110">Aby opublikować lokalnie, wybierz pozycję **folder**.</span><span class="sxs-lookup"><span data-stu-id="c26ae-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="c26ae-111">Zaakceptuj lokalizację domyślną w polu **Wybierz folder** lub określ inną lokalizację.</span><span class="sxs-lookup"><span data-stu-id="c26ae-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="c26ae-112">Wybierz przycisk **Publikuj** .</span><span class="sxs-lookup"><span data-stu-id="c26ae-112">Select the **Publish** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c26ae-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c26ae-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c26ae-114">Użyj [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia, aby opublikować aplikację z konfiguracją wydania:</span><span class="sxs-lookup"><span data-stu-id="c26ae-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="c26ae-115">Opublikowanie aplikacji wyzwala [przywracanie](/dotnet/core/tools/dotnet-restore) zależności projektu i kompiluje projekt przed [](/dotnet/core/tools/dotnet-build) utworzeniem zasobów do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="c26ae-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="c26ae-116">W ramach procesu kompilacji nieużywane metody i zestawy są usuwane w celu zmniejszenia rozmiaru pobierania aplikacji i czasów ładowania.</span><span class="sxs-lookup"><span data-stu-id="c26ae-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="c26ae-117">Aplikacja po stronie klienta Blazor jest publikowana w folderze */bin/Release/{Target Framework}/Publish/{Assembly Name}/dist* .</span><span class="sxs-lookup"><span data-stu-id="c26ae-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="c26ae-118">Aplikacja po stronie serwera Blazor jest publikowana w folderze */bin/Release/{Target Framework}/Publish* .</span><span class="sxs-lookup"><span data-stu-id="c26ae-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="c26ae-119">Zasoby w folderze są wdrażane na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c26ae-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="c26ae-120">Wdrożenie może być procesem ręcznym lub zautomatyzowanym w zależności od używanych narzędzi programistycznych.</span><span class="sxs-lookup"><span data-stu-id="c26ae-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="c26ae-121">wdrażania</span><span class="sxs-lookup"><span data-stu-id="c26ae-121">Deployment</span></span>

<span data-ttu-id="c26ae-122">Aby uzyskać wskazówki dotyczące wdrażania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="c26ae-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="c26ae-123">Blazor hostingu bezserwerowego za pomocą usługi Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c26ae-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="c26ae-124">Blazor aplikacje po stronie klienta mogą być obsługiwane z [usługi Azure Storage](https://azure.microsoft.com/services/storage/) jako zawartość statyczna bezpośrednio z kontenera magazynu.</span><span class="sxs-lookup"><span data-stu-id="c26ae-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="c26ae-125">Aby uzyskać więcej informacji, [zobacz Hostowanie i wdrażanie ASP.NET Core Blazor po stronie klienta (wdrożenie autonomiczne): Usługa Azure](xref:host-and-deploy/blazor/client-side#azure-storage)Storage.</span><span class="sxs-lookup"><span data-stu-id="c26ae-125">For more information, see [Host and deploy ASP.NET Core Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
