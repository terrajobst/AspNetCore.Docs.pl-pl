---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Jak mogę] Kontrolka buforowania strony ASP.NET na podstawie niestandardowych informacji | Dokumentacja firmy Microsoft'
author: rick-anderson
description: W tym wideo pikseli Chris pokazuje, jak do sterowania kryteriami do buforowania strony ASP.NET na podstawie niestandardowych informacji. Przykładowa strona jest tworzony i następnie a...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d2c8e2384d39255f66c11f1cc303398750229779
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376023"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="257b6-104">[Jak mogę] Kontrolka buforowania strony ASP.NET na podstawie niestandardowych informacji</span><span class="sxs-lookup"><span data-stu-id="257b6-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="257b6-105">przez [Chris pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="257b6-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="257b6-106">W tym wideo pikseli Chris pokazuje, jak do sterowania kryteriami do buforowania strony ASP.NET na podstawie niestandardowych informacji.</span><span class="sxs-lookup"><span data-stu-id="257b6-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="257b6-107">Przykładowa strona jest tworzony i następnie dyrektywy OutputCache atrybutem Element VaryByCustom, który zawiera niestandardową wartość.</span><span class="sxs-lookup"><span data-stu-id="257b6-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="257b6-108">Następnie metoda GetVaryCustomByString() zostanie przesłonięta w module global.asax, która zapewnia obsługę atrybutu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="257b6-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="257b6-109">W tej metodzie ciąg zostaje zwrócony, który jednoznacznie identyfikuje zbuforowaną wersję strony.</span><span class="sxs-lookup"><span data-stu-id="257b6-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="257b6-110">Na koniec jest dyskusji o tym jak buforowanie, przy użyciu niestandardowej wartości można używać na kilka sposobów dla witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="257b6-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="257b6-111">&#9654;Obejrzyj film wideo (12 minut)</span><span class="sxs-lookup"><span data-stu-id="257b6-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
