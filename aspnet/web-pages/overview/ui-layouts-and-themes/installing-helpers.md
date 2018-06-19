---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalowanie pomocnika w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób instalowania pomocnika w witrynie sieci Web platformy ASP.NET Web Pages (Razor). Pomocnik jest składnikiem wielokrotnego użytku, który zawiera kod i znaczników na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896785"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="04e5c-104">Instalowanie pomocnika w lokacji (Razor) stron sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="04e5c-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="04e5c-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="04e5c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="04e5c-106">W tym artykule opisano sposób instalowania pomocnika w witrynie sieci Web platformy ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="04e5c-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="04e5c-107">A *Pomocnika* jest składnikiem wielokrotnego użytku, który zawiera kod i znaczników do wykonania zadania, które mogą być niewygodny lub złożonych.</span><span class="sxs-lookup"><span data-stu-id="04e5c-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="04e5c-108">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="04e5c-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="04e5c-109">Jak zainstalować pomocnika w witrynie sieci Web utworzony za pomocą programu WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="04e5c-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="04e5c-110">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="04e5c-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="04e5c-111">Program WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="04e5c-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="04e5c-112">Omówienie wątków</span><span class="sxs-lookup"><span data-stu-id="04e5c-112">Overview of Helpers</span></span>

<span data-ttu-id="04e5c-113">Niektóre zadania, które osoby często mają być wykonane na stronach sieci web wymaga dużo kodu lub wymagają dodatkowych wiedzy.</span><span class="sxs-lookup"><span data-stu-id="04e5c-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="04e5c-114">Przykłady obejmują wyświetlanie wykresu dla danych. umieszczanie przycisk "Obserwuj" Twitter na stronie; Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie lub zmiana rozmiaru obrazów; przy użyciu usługi PayPal dla witryny.</span><span class="sxs-lookup"><span data-stu-id="04e5c-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="04e5c-115">Aby ułatwić czy rodzaju rzeczy, ASP.NET Web Pages umożliwia używanie *pomocników*.</span><span class="sxs-lookup"><span data-stu-id="04e5c-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="04e5c-116">Pomocnicy są składniki zainstalowanie dla witryny, które umożliwiają można wykonywać typowe zadania za pomocą tylko wiersz lub dwóch kodu Razor.</span><span class="sxs-lookup"><span data-stu-id="04e5c-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="04e5c-117">Strony ASP.NET Web Pages ma kilka wątków wbudowane.</span><span class="sxs-lookup"><span data-stu-id="04e5c-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="04e5c-118">Wiele wątków są jednak dostępne w pakietach (dodatki), które znajdują się za pomocą Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="04e5c-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="04e5c-119">NuGet pozwala wybrać pakiet do zainstalowania, a następnie dba o szczegóły instalacji.</span><span class="sxs-lookup"><span data-stu-id="04e5c-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="04e5c-120">Instalowanie pomocnika w programie WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="04e5c-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="04e5c-121">W programie WebMatrix 3 kliknij **NuGet** przycisku.</span><span class="sxs-lookup"><span data-stu-id="04e5c-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Okno dialogowe galerii NuGet w programie WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="04e5c-123">To spowoduje uruchomienie Menedżera pakietów NuGet i wyświetlenie dostępnych pakietów.</span><span class="sxs-lookup"><span data-stu-id="04e5c-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="04e5c-124">W polu wyszukiwania wprowadź słowo kluczowe pomocnika, którą chcesz zainstalować.</span><span class="sxs-lookup"><span data-stu-id="04e5c-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Okno dialogowe galerii NuGet w programie WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="04e5c-126">Wybierz pakiet, a następnie kliknij przycisk **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="04e5c-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="04e5c-127">Kliknij przycisk **tak** po otrzymaniu monitu, jeśli chcesz zainstalować pakiet i zaakceptuj postanowienia.</span><span class="sxs-lookup"><span data-stu-id="04e5c-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="04e5c-128">Jeśli po raz pierwszy po zainstalowaniu pomocnika NuGet powoduje tworzenie folderów w witryny sieci Web kod, który stanowi pomocnika.</span><span class="sxs-lookup"><span data-stu-id="04e5c-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="04e5c-129">Aby odinstalować pomocnika, kliknij przycisk **galerii** , kliknij **zainstalowana** , a następnie wybierz pakiet, którego chcesz odinstalować.</span><span class="sxs-lookup"><span data-stu-id="04e5c-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="04e5c-130">Instalowanie Pomocnik usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="04e5c-130">Installing the Twitter helper</span></span>

<span data-ttu-id="04e5c-131">Najnowszą wersję interfejsu API usługi Twitter nie jest zgodny z elementem pomocniczym Twitter, które będą instalowane za pośrednictwem pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="04e5c-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="04e5c-132">Zamiast tego zobacz [Pomocnik usługi Twitter, za pomocą programu WebMatrix](twitter-helper.md) tematu zawiera informacje dotyczące sposobu konfigurowania Pomocnik usługi Twitter w projekcie.</span><span class="sxs-lookup"><span data-stu-id="04e5c-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="04e5c-133">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="04e5c-133">Additional Resources</span></span>


[<span data-ttu-id="04e5c-134">Introducing ASP.NET Web Pages 2 — podstawy programowania</span><span class="sxs-lookup"><span data-stu-id="04e5c-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="04e5c-135">Pomocnik usługi Twitter, za pomocą programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="04e5c-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
