---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalowanie pomocnika we wzorcu ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób instalowania pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). Pomocnik jest komponentów wielokrotnego użytku, obejmującą kodu i znaczników w celu na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 38290fd47355e7893eddd1f867f47b113b54ca7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361809"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="bb3e6-104">Instalowanie pomocnika w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="bb3e6-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="bb3e6-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bb3e6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bb3e6-106">W tym artykule opisano sposób instalowania pomocnika w witrynie internetowej ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="bb3e6-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="bb3e6-107">A *Pomocnika* to składnik wielokrotnego użytku, który zawiera kod i znaczników w celu wykonania zadania, które mogą być uciążliwe lub złożonych.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="bb3e6-108">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="bb3e6-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="bb3e6-109">Jak zainstalować pomocnika w witrynie sieci Web utworzonych za pomocą programu WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bb3e6-110">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="bb3e6-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bb3e6-111">Program WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="bb3e6-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="bb3e6-112">Omówienie wątków</span><span class="sxs-lookup"><span data-stu-id="bb3e6-112">Overview of Helpers</span></span>

<span data-ttu-id="bb3e6-113">Niektóre zadania, ludzie często chcemy na stronach sieci web, które wymagają dużej ilości kodu lub dodatkowe wiedzy.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="bb3e6-114">Przykłady obejmują wyświetlanie wykresu dla danych. umieszczanie przycisk "Obserwuj" Twitter na stronie; Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie i zmienianie rozmiaru obrazów; Korzystając z systemu PayPal, dla danej witryny.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="bb3e6-115">Aby ułatwić wykonaj te rodzaje elementów, ASP.NET Web Pages umożliwia korzystanie z *pomocników*.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="bb3e6-116">Pomocnicy są składniki instalowaną lokacją i umożliwiające możesz wykonywać typowe zadania przy użyciu tylko wiersz lub dwóch od kodu Razor.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="bb3e6-117">Strony ASP.NET Web Pages ma kilka pomocników wbudowane.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="bb3e6-118">Wiele wątków, są jednak dostępne w pakietach (dodatki), które są dostarczane przy użyciu Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="bb3e6-119">NuGet pozwala wybrać pakiet do zainstalowania, a następnie ta odpowiada za wszystkie szczegóły instalacji.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="bb3e6-120">Instalowanie pomocnika w programie WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="bb3e6-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="bb3e6-121">Program WebMatrix 3 kliknij **NuGet** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Okno dialogowe galerii pakietów NuGet w programie WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="bb3e6-123">To spowoduje uruchomienie Menedżera pakietów NuGet i wyświetla dostępnych pakietów.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="bb3e6-124">W polu wyszukiwania wprowadź słowo kluczowe pomocy, które chcesz zainstalować.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Okno dialogowe galerii pakietów NuGet w programie WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="bb3e6-126">Wybierz pakiet, a następnie kliknij przycisk **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="bb3e6-127">Kliknij przycisk **tak** po wyświetleniu monitu, jeśli chcesz zainstalować pakiet i wskazują, że akceptujesz jej warunki.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="bb3e6-128">Jeśli po raz pierwszy po zainstalowaniu pomocnika NuGet powoduje tworzenie folderów w swojej witrynie sieci Web dla kodu, który tworzy pomocnika.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="bb3e6-129">Aby odinstalować pomocnika, kliknij przycisk **galerii** przycisku, kliknij przycisk **zainstalowane** kartę, a następnie wybierz pakiet, który chcesz odinstalować.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="bb3e6-130">Instalowanie pomocnika usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="bb3e6-130">Installing the Twitter helper</span></span>

<span data-ttu-id="bb3e6-131">Najnowszą wersję interfejsu API usługi Twitter nie jest zgodny z elementem pomocniczym Twitter, które można zainstalować za pośrednictwem pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="bb3e6-132">Zamiast tego zobacz [Pomocnik usługi Twitter za pomocą programu WebMatrix](twitter-helper.md) tematu zawiera informacje dotyczące sposobu konfigurowania Pomocnik usługi Twitter w projekcie.</span><span class="sxs-lookup"><span data-stu-id="bb3e6-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="bb3e6-133">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bb3e6-133">Additional Resources</span></span>


[<span data-ttu-id="bb3e6-134">Przedstawiamy ASP.NET Web Pages 2 — podstawy programowania</span><span class="sxs-lookup"><span data-stu-id="bb3e6-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="bb3e6-135">Pomocnik usługi Twitter za pomocą programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="bb3e6-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
