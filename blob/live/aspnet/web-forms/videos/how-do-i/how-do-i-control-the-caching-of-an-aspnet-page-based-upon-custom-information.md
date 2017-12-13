---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Jak i.] Formant buforowanie strony ASP.NET na podstawie niestandardowych informacji | Dokumentacja firmy Microsoft'
author: rick-anderson
description: "W tym wideo Pels Krzysztof pokazuje, jak do sterowania kryteriami buforowania na podstawie informacji niestandardowej strony ASP.NET. Przykładowa strona jest tworzony, a następnie ogram."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="3c82b-104">[Jak i.] Formant buforowanie strony ASP.NET na podstawie niestandardowych informacji</span><span class="sxs-lookup"><span data-stu-id="3c82b-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="3c82b-105">przez [Pels Krzysztof](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="3c82b-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="3c82b-106">W tym wideo Pels Krzysztof pokazuje, jak do sterowania kryteriami buforowania na podstawie informacji niestandardowej strony ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3c82b-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="3c82b-107">Przykładowa strona jest tworzony i następnie dyrektywy OutputCache jest używany przy użyciu atrybutu element VaryByCustom, zawierający wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="3c82b-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="3c82b-108">Następnie metoda GetVaryCustomByString() zostanie przesłonięta w module global.asax, która zapewnia obsługę atrybutu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="3c82b-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="3c82b-109">W tej metodzie zostanie zwrócony ciąg unikatowo identyfikujący wersja buforowana strony.</span><span class="sxs-lookup"><span data-stu-id="3c82b-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="3c82b-110">Na koniec jest dyskusji o jak buforowanie przy użyciu niestandardowej wartości można używać na kilka sposobów dla witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="3c82b-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="3c82b-111">&#9654; Obejrzyj klip wideo (minuty 12)</span><span class="sxs-lookup"><span data-stu-id="3c82b-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
