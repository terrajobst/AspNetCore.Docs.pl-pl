---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Tworzenie czytelnych adresów URL w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano routingu w witrynie sieci Web platformy ASP.NET Web Pages (Razor) i jak to pozwala używać adresów URL, które są bardziej czytelny i lepsze pod kątem Wyszukiwarek. Po otwarciu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573230"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="874fb-104">Tworzenie czytelnych adresów URL witryny sieci Web ASP.NET (Razor) stron</span><span class="sxs-lookup"><span data-stu-id="874fb-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="874fb-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="874fb-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="874fb-106">W tym artykule opisano routingu w witrynie sieci Web platformy ASP.NET Web Pages (Razor) i jak to pozwala używać adresów URL, które są bardziej czytelny i lepsze pod kątem Wyszukiwarek.</span><span class="sxs-lookup"><span data-stu-id="874fb-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="874fb-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="874fb-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="874fb-108">Jak program ASP.NET używa routingu do pozwala korzystać z bardziej czytelny i można wyszukiwać adresów URL.</span><span class="sxs-lookup"><span data-stu-id="874fb-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="874fb-109">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="874fb-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="874fb-110">Strony sieci Web platformy ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="874fb-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="874fb-111">W tym samouczku współdziała również z programu ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="874fb-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="874fb-112">Informacje routingu</span><span class="sxs-lookup"><span data-stu-id="874fb-112">About Routing</span></span>

<span data-ttu-id="874fb-113">Adresy URL dla stron w witrynie mogą mieć wpływ na jak witryna będzie działać.</span><span class="sxs-lookup"><span data-stu-id="874fb-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="874fb-114">Adres URL to jest &quot;przyjazną&quot; można ułatwić użytkownikom korzystania z witryny.</span><span class="sxs-lookup"><span data-stu-id="874fb-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="874fb-115">Może również pomóc z optymalizacji dla aparatów wyszukiwania (SEO) dla tej lokacji.</span><span class="sxs-lookup"><span data-stu-id="874fb-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="874fb-116">Witryny sieci Web programu ASP.NET umożliwiają automatycznie używać przyjaznych adresów URL.</span><span class="sxs-lookup"><span data-stu-id="874fb-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="874fb-117">ASP.NET umożliwia tworzenie łatwy do rozpoznania adresów URL, które opisują akcje użytkownika, a nie tylko wskazuje plik na serwerze.</span><span class="sxs-lookup"><span data-stu-id="874fb-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="874fb-118">Należy rozważyć te adresy URL dla fikcyjnej blogu:</span><span class="sxs-lookup"><span data-stu-id="874fb-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="874fb-119">Porównanie tych adresów URL do następujących protokołów:</span><span class="sxs-lookup"><span data-stu-id="874fb-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="874fb-120">W pierwszym pary, użytkownik musi wiedzieć, że blogu jest wyświetlany za pomocą *blog.cshtml* strony i będzie zmuszony do skonstruowania ciągu zapytania, który pobiera prawo zakres kategorii lub daty.</span><span class="sxs-lookup"><span data-stu-id="874fb-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="874fb-121">Drugi zestaw przykłady jest znacznie łatwiejsze do uzyskiwania i tworzenia.</span><span class="sxs-lookup"><span data-stu-id="874fb-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="874fb-122">Adresy URL, na przykład pierwszy także wskazywać bezpośrednio do określonego pliku (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="874fb-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="874fb-123">Jeśli z jakiegoś powodu blogu zostały przeniesione do innego folderu na serwerze lub blogu zostały ulegną używać innej strony, łącza może być problem.</span><span class="sxs-lookup"><span data-stu-id="874fb-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="874fb-124">Drugi zestaw adresów URL nie wskazuje na określoną stronę, nawet jeśli zmieni się implementacja blogu lub lokalizacji, adresy URL nadal będzie ważny.</span><span class="sxs-lookup"><span data-stu-id="874fb-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="874fb-125">W składniku ASP.NET Web Pages można utworzyć bardziej przyjazny adresów URL, podobnie jak w powyższych przykładach, ponieważ program ASP.NET używa *routingu*.</span><span class="sxs-lookup"><span data-stu-id="874fb-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="874fb-126">Routing tworzy mapowanie logicznych z adres URL do strony (lub stron), który można wykonać żądania.</span><span class="sxs-lookup"><span data-stu-id="874fb-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="874fb-127">Ponieważ mapowanie jest logiczną (nie fizyczne, do określonego pliku), routing zapewnia dużą elastyczność w sposób definiowania adresów URL dla witryny.</span><span class="sxs-lookup"><span data-stu-id="874fb-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="874fb-128">Jak działa Routing</span><span class="sxs-lookup"><span data-stu-id="874fb-128">How Routing Works</span></span>

<span data-ttu-id="874fb-129">Kiedy ASP.NET przetwarza żądanie, otrzymuje adres URL, aby określić sposób kierowania go.</span><span class="sxs-lookup"><span data-stu-id="874fb-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="874fb-130">Aplikacja ASP.NET próbuje odpowiada poszczególnych segmentów adresu URL do plików na dysku, przechodząc od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="874fb-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="874fb-131">Jeśli istnieje dopasowanie, niczego pozostałych w adresie URL jest przekazywany do strony jako *informacje o ścieżce*.</span><span class="sxs-lookup"><span data-stu-id="874fb-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="874fb-132">Załóżmy, że ktoś wysyła żądanie przy użyciu tego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="874fb-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="874fb-133">Wyszukiwanie przechodzi następująco:</span><span class="sxs-lookup"><span data-stu-id="874fb-133">The search goes like this:</span></span>

1. <span data-ttu-id="874fb-134">Istnieje już plik ze ścieżką i nazwą */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="874fb-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="874fb-135">Jeśli tak, uruchom na tej stronie i przekazywania do niej żadne informacje.</span><span class="sxs-lookup"><span data-stu-id="874fb-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="874fb-136">W przeciwnym razie...</span><span class="sxs-lookup"><span data-stu-id="874fb-136">Otherwise ...</span></span>
2. <span data-ttu-id="874fb-137">Istnieje już plik ze ścieżką i nazwą */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="874fb-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="874fb-138">Jeśli tak, uruchom na tej stronie i przekaż wartość `c` do niego.</span><span class="sxs-lookup"><span data-stu-id="874fb-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="874fb-139">W przeciwnym razie...</span><span class="sxs-lookup"><span data-stu-id="874fb-139">Otherwise …</span></span>
3. <span data-ttu-id="874fb-140">Istnieje już plik ze ścieżką i nazwą */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="874fb-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="874fb-141">Jeśli tak, uruchom na tej stronie i przekaż wartość `b/c` do niego.</span><span class="sxs-lookup"><span data-stu-id="874fb-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="874fb-142">Jeśli wyszukiwania znaleziono dokładne nie pasuje do *.cshtml* plików w ich określonych folderach, ASP.NET kontynuuje wyszukiwanie tych plików z kolei:</span><span class="sxs-lookup"><span data-stu-id="874fb-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="874fb-143">*/a/b/c/default.cshtml* (nie informacji o ścieżce).</span><span class="sxs-lookup"><span data-stu-id="874fb-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="874fb-144">*/a/b/c/index.cshtml* (nie informacji o ścieżce).</span><span class="sxs-lookup"><span data-stu-id="874fb-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="874fb-145">Być wyczyszczone, żądania dotyczące określonych stron (czyli żądań, które obejmują *.cshtml* rozszerzenie nazwy pliku) działają tak samo, jak można oczekiwać.</span><span class="sxs-lookup"><span data-stu-id="874fb-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="874fb-146">Żądanie, takich jak `http://www.contoso.com/a/b.cshtml` uruchomi strony *b.cshtml* tylko poprawnie.</span><span class="sxs-lookup"><span data-stu-id="874fb-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="874fb-147">Wewnątrz strony, możesz uzyskać informacje o ścieżce za pośrednictwem strony `UrlData` właściwość, która jest słownikiem.</span><span class="sxs-lookup"><span data-stu-id="874fb-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="874fb-148">Załóżmy, że plik o nazwie *ViewCustomers.cshtml* i lokacji pobiera tego żądania:</span><span class="sxs-lookup"><span data-stu-id="874fb-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="874fb-149">Zgodnie z opisem w powyższych reguł, żądanie przejdzie do strony.</span><span class="sxs-lookup"><span data-stu-id="874fb-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="874fb-150">Wewnątrz strony, aby pobrać i wyświetlić informacje o ścieżce można użyć kodu podobne do poniższych (w tym przypadku wartość &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="874fb-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="874fb-151">Ponieważ routingu nie zawiera pełnej nazwy pliku, może istnieć niejednoznaczności w przypadku stron, które mają takie same nazwy, ale inne rozszerzenia nazwy pliku (na przykład *MyPage.cshtml* i *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="874fb-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="874fb-152">Aby uniknąć problemów z routingiem, najlepiej upewnij się, że nie masz strony w witrynie, których nazwy różnią się jedynie ich rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="874fb-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="874fb-153">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="874fb-153">Additional Resources</span></span>

<span data-ttu-id="874fb-154">[Program WebMatrix - adresów URL, UrlData i Routing pod kątem Wyszukiwarek](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="874fb-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="874fb-155">Ten wpis w blogu przez Brind Jan zawiera niektóre dodatkowe szczegóły dotyczące działania jak routingu w składniku ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="874fb-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
