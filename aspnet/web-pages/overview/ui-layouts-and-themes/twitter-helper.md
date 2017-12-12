---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocnik ze stronami sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: tfitzmac
description: "Ten temat i aplikacji pokazują, jak dodać usługi Twitter pomocnika do projektu 3 programu WebMatrix. Zawiera kod Pomocnik usługi Twitter i pokazuje sposób wywoływania pomocnika..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="576f4-104">Pomocnik usługi Twitter ze stronami sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="576f4-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="576f4-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="576f4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="576f4-106">Ten temat i aplikacji pokazują, jak dodać usługi Twitter pomocnika do projektu 3 programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="576f4-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="576f4-107">Zawiera kod Pomocnik usługi Twitter i przedstawia sposób wywołania metody pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="576f4-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="576f4-108">Ten kod do pliku Twitter.cshtml został opracowany przez **przesuwanie niż** firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="576f4-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="576f4-109">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="576f4-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="576f4-110">Strony sieci Web platformy ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="576f4-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="576f4-111">W tym samouczku współdziała również z programu ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="576f4-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="576f4-112">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="576f4-112">Introduction</span></span>

<span data-ttu-id="576f4-113">W tym temacie przedstawiono sposób dodawania usługi Twitter pomocnika do aplikacji i użyć składni Razor do wywołania metody pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="576f4-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="576f4-114">Pomocnik usługi Twitter można łatwo zastosować przycisków Twitter i elementy widget w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="576f4-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="576f4-115">Aby użyć elementu widget Twitter, takie jak harmonogram użytkownika lub wyniki wyszukiwania hasztagiem, należy najpierw utworzyć [elementu widget w serwisie Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="576f4-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="576f4-116">Po utworzeniu sieci widget, otrzymasz identyfikator elementu widget. Ten identyfikator elementu widget należy przekazać jako parametru podczas wywoływania metody pomocnicze, które pokazują elementu widget.</span><span class="sxs-lookup"><span data-stu-id="576f4-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="576f4-117">Ten temat został zapisany w wersji 1.1 interfejs API usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="576f4-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="576f4-118">Dodając bezpośrednio kodu Pomocnik usługi Twitter do projektu, należy zaktualizować kodu pomocnika w przypadku zmiany interfejs API usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="576f4-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="576f4-119">Aby uzyskać informacje o instalowaniu programu WebMatrix, zobacz [wprowadzenie ASP.NET stron sieci Web 2 — wprowadzenie](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="576f4-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="576f4-120">Dodaj do projektu Pomocnik usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="576f4-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="576f4-121">Aby dodać Pomocnik usługi Twitter, najpierw Dodaj folder o nazwie **aplikacji\_kod** do projektu.</span><span class="sxs-lookup"><span data-stu-id="576f4-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="576f4-122">Następnie utwórz plik o nazwie **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="576f4-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Folderze App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="576f4-124">Zastąp następujący kod w kodzie domyślnym w Twitter.cshtml.</span><span class="sxs-lookup"><span data-stu-id="576f4-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="576f4-125">Wywołanie metody Twitter ze stron sieci web</span><span class="sxs-lookup"><span data-stu-id="576f4-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="576f4-126">Poniższy przykład przedstawia użycie metody Pomocnik usługi Twitter ze strony w projekcie.</span><span class="sxs-lookup"><span data-stu-id="576f4-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="576f4-127">W projekcie można zastąpić wartości parametru z wartościami, które są odpowiednie do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="576f4-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="576f4-128">Identyfikatory podanego elementu widget można używać, aby poznać sposób działania metody, ale można generować własne elementy widget dla projektu.</span><span class="sxs-lookup"><span data-stu-id="576f4-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="576f4-129">Nie wszystkie parametry pokazano poniżej są wymagane.</span><span class="sxs-lookup"><span data-stu-id="576f4-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="576f4-130">Parametry opcjonalne są używane do dostosowywania wyświetlania przycisku lub elementu widget.</span><span class="sxs-lookup"><span data-stu-id="576f4-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="576f4-131">Na przykład przycisk Wykonaj wymaga tylko nazwy użytkownika, które należy wykonać, ale w przykładzie pokazano sposób obejmują liczbę adherentami i jak określić rozmiar przycisku i języka.</span><span class="sxs-lookup"><span data-stu-id="576f4-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="576f4-132">Zobacz wyniki</span><span class="sxs-lookup"><span data-stu-id="576f4-132">See the results</span></span>

<span data-ttu-id="576f4-133">Powyższy kod tworzy następujące przyciski i elementy widget.</span><span class="sxs-lookup"><span data-stu-id="576f4-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="576f4-134">Tych przycisków i elementy widget są w pełni funkcjonalna, nie zrzuty ekranu.</span><span class="sxs-lookup"><span data-stu-id="576f4-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="576f4-135">Przycisk Wykonaj jest wyświetlany w języku hiszpańskim, ponieważ ustawiono parametr language **es**.</span><span class="sxs-lookup"><span data-stu-id="576f4-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="576f4-136">Postępuj zgodnie z przycisku</span><span class="sxs-lookup"><span data-stu-id="576f4-136">Follow Button</span></span>

<span data-ttu-id="576f4-137">[Postępuj zgodnie z @aspnet)](https://twitter.com/aspnet)<script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "script", "twitter wjs");</script></span><span class="sxs-lookup"><span data-stu-id="576f4-137">[Follow @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script></span></span>

### <a name="tweet-button"></a><span data-ttu-id="576f4-138">Przycisk tweet</span><span class="sxs-lookup"><span data-stu-id="576f4-138">Tweet Button</span></span>

<span data-ttu-id="576f4-139">[Tweet](https://twitter.com/share)<script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "script", "twitter wjs");</script></span><span class="sxs-lookup"><span data-stu-id="576f4-139">[Tweet](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script></span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="576f4-140">Oś czasu użytkownika (profil)</span><span class="sxs-lookup"><span data-stu-id="576f4-140">User Timeline (Profile)</span></span>

<span data-ttu-id="576f4-141">[Tweetów przez @aspnet ](https://twitter.com/aspnet) <script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "skrypt", "twitter wjs");</script></span><span class="sxs-lookup"><span data-stu-id="576f4-141">[Tweets by @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="favorites"></a><span data-ttu-id="576f4-142">Ulubione</span><span class="sxs-lookup"><span data-stu-id="576f4-142">Favorites</span></span>

<span data-ttu-id="576f4-143">[Ulubione Tweetów przez @Microsoft ](https://twitter.com/Microsoft/favorites) <script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "skrypt", "twitter wjs");</script></span><span class="sxs-lookup"><span data-stu-id="576f4-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="list"></a><span data-ttu-id="576f4-144">Lista</span><span class="sxs-lookup"><span data-stu-id="576f4-144">List</span></span>

<span data-ttu-id="576f4-145">[Tweetów z @Microsoft/MS \_konsumenta\_pasma](https://twitter.com/microsoft/ms-consumer-brands/)<script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "skrypt", "twitter wjs");</script></span><span class="sxs-lookup"><span data-stu-id="576f4-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="search"></a><span data-ttu-id="576f4-146">Wyszukaj</span><span class="sxs-lookup"><span data-stu-id="576f4-146">Search</span></span>

<span data-ttu-id="576f4-147">[Tweetów o &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "skrypt", "twitter wjs");</script></span><span class="sxs-lookup"><span data-stu-id="576f4-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>
