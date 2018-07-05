---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Tworzenie adresów URL można odczytać we wzorcu ASP.NET Web Pages witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano, routing w witrynie sieci Web ASP.NET Web Pages (Razor) i jak dzięki temu można używać adresów URL, które były bardziej czytelne i lepszym miejscem dla optymalizacji dla aparatów wyszukiwania. Po otwarciu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d027be2924f24c1080deb4ee2deacc235bbd1001
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385751"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="fc017-104">Tworzenie adresów URL do odczytu w witrynach ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="fc017-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="fc017-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fc017-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fc017-106">W tym artykule opisano, routing w witrynie sieci Web ASP.NET Web Pages (Razor) i jak dzięki temu można używać adresów URL, które były bardziej czytelne i lepszym miejscem dla optymalizacji dla aparatów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="fc017-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="fc017-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="fc017-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="fc017-108">Jak aplikacja ASP.NET używa routingu pozwala używać bardziej czytelne i którą można przeszukiwać adresów URL.</span><span class="sxs-lookup"><span data-stu-id="fc017-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fc017-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="fc017-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="fc017-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="fc017-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="fc017-111">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="fc017-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="fc017-112">Temat routingu</span><span class="sxs-lookup"><span data-stu-id="fc017-112">About Routing</span></span>

<span data-ttu-id="fc017-113">Adresy URL dla strony w witrynie sieci może mieć wpływ na stopnia działania lokacji.</span><span class="sxs-lookup"><span data-stu-id="fc017-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="fc017-114">Adres URL to &quot;przyjazna&quot; może ułatwić użytkownicy będą mogli korzystać z witryny.</span><span class="sxs-lookup"><span data-stu-id="fc017-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="fc017-115">Może on również okazać pomocny przy użyciu optymalizacji dla aparatów wyszukiwania (SEO) dla tej witryny.</span><span class="sxs-lookup"><span data-stu-id="fc017-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="fc017-116">Witryn sieci Web platformy ASP.NET to możliwość używania przyjazne adresy URL automatycznie.</span><span class="sxs-lookup"><span data-stu-id="fc017-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="fc017-117">ASP.NET umożliwia tworzenie istotnych adresów URL, które opisują akcje użytkownika, a nie po prostu wskazuje plik na serwerze.</span><span class="sxs-lookup"><span data-stu-id="fc017-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="fc017-118">Należy wziąć pod uwagę te adresy URL dla fikcyjnej blog:</span><span class="sxs-lookup"><span data-stu-id="fc017-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="fc017-119">Porównaj te adresy URL do poniższych:</span><span class="sxs-lookup"><span data-stu-id="fc017-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="fc017-120">Pierwszy parę, użytkownik musi wiedzieć, że blogu jest wyświetlana przy użyciu *blog.cshtml* strony, a następnie miałby do konstruowania ciągu zapytania, który pobiera odpowiednie zakresu kategorii lub daty.</span><span class="sxs-lookup"><span data-stu-id="fc017-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="fc017-121">Drugi zestaw przykładów jest znacznie łatwiejsze, pojmować i tworzenia.</span><span class="sxs-lookup"><span data-stu-id="fc017-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="fc017-122">Adresy URL, na przykład pierwszy też wskazywać bezpośrednio do określonego pliku (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="fc017-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="fc017-123">Jeśli z jakiegoś powodu blogu zostały przeniesione do innego folderu na serwerze lub blogu zostały przepisane, aby użyć innej strony, linki może być nieprawidłowa.</span><span class="sxs-lookup"><span data-stu-id="fc017-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="fc017-124">Drugi zestaw adresów URL nie wskazuje na konkretnej stronie, nawet jeśli zmiany w implementacji blogu lub lokalizacji, adresy URL nadal będzie nieprawidłowa.</span><span class="sxs-lookup"><span data-stu-id="fc017-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="fc017-125">W składniku ASP.NET Web Pages można utworzyć bardziej przyjaznej adresy URL, podobnie jak w powyższych przykładach ponieważ aplikacja ASP.NET używa *routingu*.</span><span class="sxs-lookup"><span data-stu-id="fc017-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="fc017-126">Routing tworzy mapowanie logicznych z adres URL strony (lub stron), który można wykonać żądania.</span><span class="sxs-lookup"><span data-stu-id="fc017-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="fc017-127">Ponieważ mapowanie jest logiczną (nie fizycznej, do konkretnego pliku), routing zapewnia dużą elastyczność w sposób definiowania adresów URL dla witryny.</span><span class="sxs-lookup"><span data-stu-id="fc017-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="fc017-128">Jak działa Routing</span><span class="sxs-lookup"><span data-stu-id="fc017-128">How Routing Works</span></span>

<span data-ttu-id="fc017-129">ASP.NET przetwarza żądanie, odczytywanych jest adres URL, aby określić sposób kierowania go.</span><span class="sxs-lookup"><span data-stu-id="fc017-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="fc017-130">Aplikacja ASP.NET próbuje dopasować poszczególnych segmentów adresu URL do plików na dysku, przechodząc od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="fc017-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="fc017-131">W przypadku dopasowania niczego pozostały w adresie URL jest przekazywany do strony jako *informacji o ścieżce*.</span><span class="sxs-lookup"><span data-stu-id="fc017-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="fc017-132">Wyobraź sobie, że ktoś sprawia, że żądanie przy użyciu tego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="fc017-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="fc017-133">Wyszukiwanie wykracza następująco:</span><span class="sxs-lookup"><span data-stu-id="fc017-133">The search goes like this:</span></span>

1. <span data-ttu-id="fc017-134">Czy istnieje plik o ścieżkę i nazwę */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fc017-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="fc017-135">Jeśli tak, uruchom tę stronę i przekaż żadnych informacji do niego.</span><span class="sxs-lookup"><span data-stu-id="fc017-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="fc017-136">W przeciwnym razie...</span><span class="sxs-lookup"><span data-stu-id="fc017-136">Otherwise ...</span></span>
2. <span data-ttu-id="fc017-137">Czy istnieje plik o ścieżkę i nazwę */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fc017-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="fc017-138">Jeśli tak, uruchom tę stronę i przekaż wartość `c` do niego.</span><span class="sxs-lookup"><span data-stu-id="fc017-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="fc017-139">W przeciwnym razie...</span><span class="sxs-lookup"><span data-stu-id="fc017-139">Otherwise …</span></span>
3. <span data-ttu-id="fc017-140">Czy istnieje plik o ścieżkę i nazwę */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fc017-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="fc017-141">Jeśli tak, uruchom tę stronę i przekaż wartość `b/c` do niego.</span><span class="sxs-lookup"><span data-stu-id="fc017-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="fc017-142">Jeśli wyszukiwanie znaleziono dokładne nie jest zgodna dla *.cshtml* pliki w ich określonych folderów, ASP.NET kontynuuje wyszukiwanie te pliki z kolei:</span><span class="sxs-lookup"><span data-stu-id="fc017-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="fc017-143">*/a/b/c/default.cshtml* (nie informacji o ścieżce).</span><span class="sxs-lookup"><span data-stu-id="fc017-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="fc017-144">*/a/b/c/index.cshtml* (nie informacji o ścieżce).</span><span class="sxs-lookup"><span data-stu-id="fc017-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="fc017-145">Aby być niejasne, żądania dotyczące określonych stron (oznacza to, że te żądania, które obejmują *.cshtml* rozszerzenie nazwy pliku) działają tak samo, jak można oczekiwać.</span><span class="sxs-lookup"><span data-stu-id="fc017-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="fc017-146">Żądanie, takie jak `http://www.contoso.com/a/b.cshtml` uruchomią się stronę *b.cshtml* porządku.</span><span class="sxs-lookup"><span data-stu-id="fc017-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="fc017-147">Wewnątrz strony, można uzyskać informacji o ścieżce za pomocą strony `UrlData` właściwość, która jest słownikiem.</span><span class="sxs-lookup"><span data-stu-id="fc017-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="fc017-148">Wyobraź sobie, że masz plik o nazwie *ViewCustomers.cshtml* i lokacji pobiera tego żądania:</span><span class="sxs-lookup"><span data-stu-id="fc017-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="fc017-149">Zgodnie z opisem w powyższych reguł, żądanie przejdzie do strony.</span><span class="sxs-lookup"><span data-stu-id="fc017-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="fc017-150">Na stronie można użyć kodu, jak pokazano poniżej, aby pobrać i wyświetlić informacje o ścieżce (w tym przypadku wartość &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="fc017-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="fc017-151">Ponieważ routingu nie zawiera pełnej nazwy pliku, może istnieć niejednoznaczności w przypadku stron, które mają taką samą nazwę, ale inne rozszerzenia nazwy pliku (na przykład *MyPage.cshtml* i *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="fc017-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="fc017-152">Aby uniknąć problemów z routingiem, najlepiej upewnij się, że nie masz strony w witrynie, których nazwy różnią się tylko w ich rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="fc017-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="fc017-153">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fc017-153">Additional Resources</span></span>

<span data-ttu-id="fc017-154">[Program WebMatrix - adresy URL, UrlData i routingu pod kątem Wyszukiwarek](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="fc017-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="fc017-155">Ten wpis w blogu przez Mike'a Brind zapewnia pewne dodatkowe szczegóły dotyczące działa jak routing w składniku ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="fc017-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
