---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: "Tworzenie pakietów i zminimalizowania liczby zasobów w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft"
author: microsoft
description: "Tworzenie pakietów i minimalizowanie metodami przyspieszyć witryny. Tworzenie pakietów umożliwia łączone wiele plików JavaScript (js) lub wielu kaskadowy arkusz stylów (...)"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="964bf-104">Tworzenie pakietów i zminimalizowania liczby zasobów witryny ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="964bf-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="964bf-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="964bf-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="964bf-106">Tworzenie pakietów i minimalizowanie metodami przyspieszyć witryny.</span><span class="sxs-lookup"><span data-stu-id="964bf-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="964bf-107">Tworzenie pakietów umożliwia łączenie wielu JavaScript (*js*) pliki lub wiele kaskadowy arkusz stylów (*.css*) plików, dzięki czemu mogą być pobierane jako jednostki, a nie pojedynczo.</span><span class="sxs-lookup"><span data-stu-id="964bf-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="964bf-108">Minimalizowanie powstaną limit odstępu i wykonuje innych typów kompresji dokonanie potencjalnie pobranych plików, jak mała.</span><span class="sxs-lookup"><span data-stu-id="964bf-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="964bf-109">W wersji RC ASP.NET Web Pages 2 nie obsługuje tworzenie pakietów i minimalizowanie ponieważ pakiet, który zawiera wymagane elementy nie jest jeszcze dostępna w Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="964bf-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="964bf-110">Przepraszamy za niedogodności.</span><span class="sxs-lookup"><span data-stu-id="964bf-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="964bf-111">Pakiet powinien być dostępny w ostatecznej wersji programu ASP.NET Web Pages 2 i programu WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="964bf-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
