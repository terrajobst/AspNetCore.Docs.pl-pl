---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[Jak i.]  Pamięć podręczna strony ASP.NET na podstawie informacji w nagłówku HTTP | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym wideo Pels Krzysztof pokazano, jak zachować strony w pamięci podręcznej danych wyjściowych programu ASP.NET, na podstawie informacji w nagłówkach HTTP. Pierwszy, potencjalne nagłówków HTTP..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: ce5ea10396d0fe31d72425e2431102a0cb0c3bd0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="c1eeb-104">[Jak i.]  Strony ASP.NET na podstawie informacji w nagłówku HTTP pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="c1eeb-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="c1eeb-105">przez [Pels Krzysztof](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="c1eeb-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="c1eeb-106">W tym wideo Pels Krzysztof pokazano, jak zachować strony w pamięci podręcznej danych wyjściowych programu ASP.NET, na podstawie informacji w nagłówkach HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1eeb-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="c1eeb-107">Po pierwsze zostaną zweryfikowane potencjalnych wartości nagłówka HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1eeb-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="c1eeb-108">Następnie przykładową stronę jest tworzony i następnie dyrektywy OutputCache jest używany z atrybutem VaryByHeader, który zawiera wartość "Zaakceptuj języka", nagłówka HTTP, aby kontrolować buforowanie oparte na język przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c1eeb-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="c1eeb-109">Strony przykładowe są wyświetlane w programie Internet Explorer, która jest ustawiona na język angielski, a następnie w programie FireFox, która jest ustawiona na język francuski.</span><span class="sxs-lookup"><span data-stu-id="c1eeb-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="c1eeb-110">Ponadto omówiono opcję, aby przenieść definicji pamięci podręcznej CacheProfile w pliku web.config.</span><span class="sxs-lookup"><span data-stu-id="c1eeb-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="c1eeb-111">&#9654; Obejrzyj klip wideo (minuty 12)</span><span class="sxs-lookup"><span data-stu-id="c1eeb-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
