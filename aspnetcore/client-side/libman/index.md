---
title: Biblioteka klienta przejęcia w programie ASP.NET Core za pomocą LibMan
author: scottaddie
description: Dowiedz się, jak zainstalować zasoby biblioteki po stronie klienta w projektach programu ASP.NET Core za pomocą Menedżera biblioteki (LibMan).
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900151"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a><span data-ttu-id="2bd3d-103">Biblioteka klienta przejęcia w programie ASP.NET Core za pomocą LibMan</span><span class="sxs-lookup"><span data-stu-id="2bd3d-103">Client-side library acquisition in ASP.NET Core with LibMan</span></span>

<span data-ttu-id="2bd3d-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="2bd3d-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="2bd3d-105">Menedżer biblioteki (LibMan) jest narzędziem przejęcia biblioteki lekki, po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="2bd3d-105">Library Manager (LibMan) is a lightweight, client-side library acquisition tool.</span></span> <span data-ttu-id="2bd3d-106">LibMan pobiera popularnych bibliotek i struktur, z systemu plików lub [sieci dostarczania zawartości (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span><span class="sxs-lookup"><span data-stu-id="2bd3d-106">LibMan downloads popular libraries and frameworks from the file system or from a [content delivery network (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span></span> <span data-ttu-id="2bd3d-107">Obsługiwane CDN obejmują [CDNJS](https://cdnjs.com/) i [unpkg](https://unpkg.com/#/).</span><span class="sxs-lookup"><span data-stu-id="2bd3d-107">The supported CDNs include [CDNJS](https://cdnjs.com/) and [unpkg](https://unpkg.com/#/).</span></span> <span data-ttu-id="2bd3d-108">Pliki wybranej biblioteki są pobierane i umieszczane w odpowiedniej lokalizacji w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bd3d-108">The selected library files are fetched and placed in the appropriate location within the ASP.NET Core project.</span></span>

## <a name="libman-use-cases"></a><span data-ttu-id="2bd3d-109">LibMan przypadki użycia</span><span class="sxs-lookup"><span data-stu-id="2bd3d-109">LibMan use cases</span></span>

<span data-ttu-id="2bd3d-110">LibMan oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="2bd3d-110">LibMan offers the following benefits:</span></span>

* <span data-ttu-id="2bd3d-111">Pobierane są pliki biblioteki, które są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="2bd3d-111">Only the library files you need are downloaded.</span></span>
* <span data-ttu-id="2bd3d-112">Dodatkowe narzędzia, takie jak [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), i [WebPack](https://webpack.js.org), nie jest konieczne w celu uzyskania podzbiór plików w bibliotece.</span><span class="sxs-lookup"><span data-stu-id="2bd3d-112">Additional tooling, such as [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), and [WebPack](https://webpack.js.org), isn't necessary to acquire a subset of files in a library.</span></span>
* <span data-ttu-id="2bd3d-113">Pliki można umieścić w określonej lokalizacji, bez konieczności uciekania się do zadań kompilacji lub kopiowania plików ręczne.</span><span class="sxs-lookup"><span data-stu-id="2bd3d-113">Files can be placed in a specific location without resorting to build tasks or manual file copying.</span></span>

<span data-ttu-id="2bd3d-114">Aby uzyskać więcej informacji na temat korzyści firmy LibMan Obejrzyj [nowoczesnych frontonu sieci web development w programie Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span><span class="sxs-lookup"><span data-stu-id="2bd3d-114">For more information about LibMan's benefits, watch [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span></span>

<span data-ttu-id="2bd3d-115">LibMan jest system zarządzania pakietami.</span><span class="sxs-lookup"><span data-stu-id="2bd3d-115">LibMan isn't a package management system.</span></span> <span data-ttu-id="2bd3d-116">Jeśli już używasz Menedżera pakietów, takich jak npm lub [yarn](https://yarnpkg.com), dalej.</span><span class="sxs-lookup"><span data-stu-id="2bd3d-116">If you're already using a package manager, such as npm or [yarn](https://yarnpkg.com), continue doing so.</span></span> <span data-ttu-id="2bd3d-117">LibMan nie został opracowany, aby zastąpić tych narzędzi.</span><span class="sxs-lookup"><span data-stu-id="2bd3d-117">LibMan wasn't developed to replace those tools.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bd3d-118">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2bd3d-118">Additional resources</span></span>

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="2bd3d-119">Repozytorium LibMan GitHub</span><span class="sxs-lookup"><span data-stu-id="2bd3d-119">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
