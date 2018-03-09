---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: "Uruchomione różne wersje stron sieci Web platformy ASP.NET (Razor) obok siebie | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym artykule opisano sposób uruchamiania stron sieci Web platformy ASP.NET (Razor) witryn sieci Web na tym samym komputerze lub serwerze, gdy witryn sieci Web są skonfigurowane do używania różnych wersji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: c11399b0bde59d18fa378ed48c15844454c1f956
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="940e1-103">Równolegle z różnymi wersjami stron sieci Web platformy ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="940e1-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="940e1-104">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="940e1-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="940e1-105">W tym artykule opisano sposób uruchamiania stron sieci Web platformy ASP.NET (Razor) witryn sieci Web na tym samym komputerze lub serwerze, gdy witryn sieci Web są skonfigurowane do używania różnych wersji składnika ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="940e1-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="940e1-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="940e1-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="940e1-107">Domyślne zachowanie nowości w programie ASP.NET w przypadku witryn utworzonych z ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="940e1-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="940e1-108">Jak skonfigurować nową lokację do uruchamiania przy użyciu starszej wersji składnika ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="940e1-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="940e1-109">Jest to funkcja ASP.NET, wprowadzona w artykule:</span><span class="sxs-lookup"><span data-stu-id="940e1-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="940e1-110">`webPages:Version` Ustawienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="940e1-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="940e1-111">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="940e1-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="940e1-112">Strony sieci Web platformy ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="940e1-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="940e1-113">W tym samouczku współdziała również z ASP.NET Web Pages 2 i stron sieci Web ASP.NET w wersji 1.0.</span><span class="sxs-lookup"><span data-stu-id="940e1-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="940e1-114">Strony sieci Web ASP.NET obsługuje możliwości korzystania z witryn sieci Web obok siebie.</span><span class="sxs-lookup"><span data-stu-id="940e1-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="940e1-115">Dzięki temu można nadal uruchamiać starsze aplikacje ASP.NET Web Pages, tworzenie nowej aplikacji ASP.NET Web Pages i uruchomić wszystkie z nich na tym samym komputerze.</span><span class="sxs-lookup"><span data-stu-id="940e1-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="940e1-116">Poniżej przedstawiono niektóre czynności podczas instalacji stron sieci Web za pomocą programu WebMatrix:</span><span class="sxs-lookup"><span data-stu-id="940e1-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="940e1-117">Domyślnie istniejących aplikacji stron sieci Web zostanie uruchomiony jako najnowszą wersję na komputerze.</span><span class="sxs-lookup"><span data-stu-id="940e1-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="940e1-118">(Zestawy są zainstalowane w globalnej pamięci podręcznej zestawów (GAC) i są automatycznie używane).</span><span class="sxs-lookup"><span data-stu-id="940e1-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="940e1-119">Jeśli chcesz uruchomić lokacji za pomocą innej wersji składnika ASP.NET Web Pages, można skonfigurować witryny, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="940e1-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="940e1-120">Jeśli witryna nie ma jeszcze *web.config* plików w katalogu głównym witryny, Utwórz nową i skopiuj następujący kod XML, zastępowanie istniejącej zawartości.</span><span class="sxs-lookup"><span data-stu-id="940e1-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="940e1-121">Jeśli witryna zawiera już *web.config* plików, dodawanie `<appSettings>` element podobny do następującego do `<configuration>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="940e1-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
<span data-ttu-id="940e1-122">"— Jeśli nie określisz wersji w *web.config* pliku lokacji jest wdrożona jako najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="940e1-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="940e1-123">(Zestawy są kopiowane do *bin* folder w witrynie wdrożonej.)</span><span class="sxs-lookup"><span data-stu-id="940e1-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="940e1-124">Nowe aplikacje tworzone przy użyciu szablonów witryn w sieci Web macierzy zawierają zestawy wersję stron sieci Web w tej witrynie *bin* folderu.</span><span class="sxs-lookup"><span data-stu-id="940e1-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="940e1-125">Ogólnie rzecz biorąc, zawsze można kontrolować wersję stron sieci Web do użycia z lokacji za pomocą NuGet odpowiednich zestawów do witryny *bin* folderu.</span><span class="sxs-lookup"><span data-stu-id="940e1-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="940e1-126">Pakietów można znaleźć [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="940e1-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="940e1-127">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="940e1-127">Additional Resources</span></span>

[<span data-ttu-id="940e1-128">Najważniejsze funkcje w składniku ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="940e1-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
