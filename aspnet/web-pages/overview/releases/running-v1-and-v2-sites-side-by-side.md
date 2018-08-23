---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Uruchamianie różnych wersji składnika ASP.NET Web Pages (Razor) obok siebie | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób uruchamiania witryn sieci Web ASP.NET Web Pages (Razor) na tym samym komputerze lub serwerze, gdy witryn sieci Web są skonfigurowane do używania różnych wersji...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 9021f9b7a68b8b20f7f2fbcd5649cc7226401a1b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754592"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="01aa2-103">Równoległe uruchamianie różnych wersji wzorca ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="01aa2-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="01aa2-104">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="01aa2-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="01aa2-105">W tym artykule opisano sposób uruchamiania witryn sieci Web ASP.NET Web Pages (Razor) na tym samym komputerze lub serwerze, gdy witryn sieci Web są skonfigurowane do używania różnych wersji składnika ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="01aa2-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="01aa2-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="01aa2-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="01aa2-107">Domyślne zachowanie nowości w programie ASP.NET: w przypadku działania witryn utworzonych przy użyciu stron ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="01aa2-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="01aa2-108">Jak skonfigurować nową lokację do uruchamiania w starszej wersji składnika ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="01aa2-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="01aa2-109">Jest to ASP.NET wprowadzonej w artykule:</span><span class="sxs-lookup"><span data-stu-id="01aa2-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="01aa2-110">`webPages:Version` Ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="01aa2-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="01aa2-111">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="01aa2-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="01aa2-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="01aa2-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="01aa2-113">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2 i stron sieci Web platformy ASP.NET w wersji 1.0.</span><span class="sxs-lookup"><span data-stu-id="01aa2-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="01aa2-114">Strony ASP.NET Web Pages obsługuje możliwość uruchamiania witryn sieci Web obok siebie.</span><span class="sxs-lookup"><span data-stu-id="01aa2-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="01aa2-115">Dzięki temu można w dalszym ciągu uruchamiać starsze aplikacje ASP.NET Web Pages, tworzyć nowe aplikacje ASP.NET Web Pages i uruchomić wszystkie z nich na tym samym komputerze.</span><span class="sxs-lookup"><span data-stu-id="01aa2-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="01aa2-116">Oto kilka rzeczy do zapamiętania, po zainstalowaniu na stronach sieci Web za pomocą programu WebMatrix:</span><span class="sxs-lookup"><span data-stu-id="01aa2-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="01aa2-117">Domyślnie istniejących aplikacji stron sieci Web działa jako najnowszej wersji na komputerze.</span><span class="sxs-lookup"><span data-stu-id="01aa2-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="01aa2-118">(Zestawy są zainstalowane w globalnej pamięci podręcznej zestawów (GAC) i są automatycznie używane).</span><span class="sxs-lookup"><span data-stu-id="01aa2-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="01aa2-119">Jeśli chcesz uruchomić witrynę, przy użyciu różnych wersji składnika ASP.NET Web Pages można skonfigurować witryny, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="01aa2-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="01aa2-120">Jeśli witryna nie ma jeszcze *web.config* plików w katalogu głównym witryny, Utwórz nową i skopiuj następujący kod XML do niego, zastępując istniejącą zawartość.</span><span class="sxs-lookup"><span data-stu-id="01aa2-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="01aa2-121">Jeśli witryna zawiera już *web.config* Dodaj `<appSettings>` element podobny do następującego do `<configuration>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="01aa2-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="01aa2-122">"— Jeśli nie określisz wersji w *web.config* pliku lokacji jest wdrażany jako najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="01aa2-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="01aa2-123">(Zestawy są kopiowane do *bin* folder w witrynie wdrożone.)</span><span class="sxs-lookup"><span data-stu-id="01aa2-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="01aa2-124">Nowe aplikacje, które możesz utworzyć przy użyciu szablonów witryn w sieci Web macierzy zawierają zestawy wersję stron sieci Web w tej witrynie *bin* folderu.</span><span class="sxs-lookup"><span data-stu-id="01aa2-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="01aa2-125">Ogólnie rzecz biorąc, zawsze można kontrolować wersję stron sieci Web za pomocą witryny za pomocą pakietu NuGet do instalowania odpowiednich zestawów w witrynie *bin* folderu.</span><span class="sxs-lookup"><span data-stu-id="01aa2-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="01aa2-126">Pakietów można znaleźć [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="01aa2-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01aa2-127">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="01aa2-127">Additional Resources</span></span>

[<span data-ttu-id="01aa2-128">Najważniejsze funkcje we wzorcu ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="01aa2-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
