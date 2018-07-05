---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocnik ze strony sieci Web ASP.NET w usłudze Twitter | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym temacie i aplikacji pokazują, jak dodać Pomocnik usługi Twitter do projektu program WebMatrix 3. Zawiera kod Pomocnik usługi Twitter i przedstawiono sposób wywoływania pomocnika...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 2c84a986a39f6802a78df53510847cb70efbb0f2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373026"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="bcac7-104">Pomocnik usługi Twitter za pomocą wzorca ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="bcac7-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="bcac7-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bcac7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bcac7-106">W tym temacie i aplikacji pokazują, jak dodać Pomocnik usługi Twitter do projektu program WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="bcac7-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="bcac7-107">Zawiera kod Pomocnik usługi Twitter i przedstawiono sposób wywoływania metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="bcac7-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="bcac7-108">Ten kod do pliku Twitter.cshtml został opracowany przez **Pan niż** przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bcac7-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bcac7-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="bcac7-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bcac7-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="bcac7-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="bcac7-111">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="bcac7-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="bcac7-112">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="bcac7-112">Introduction</span></span>

<span data-ttu-id="bcac7-113">W tym temacie przedstawiono sposób dodawania Pomocnik usługi Twitter do aplikacji i użyć składni Razor do wywołania metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="bcac7-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="bcac7-114">Pomocnik usługi Twitter ułatwia przycisków w usłudze Twitter i widżetów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bcac7-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="bcac7-115">Aby użyć elementu widget Twitter, takie jak osi czasu użytkownika lub wyniki wyszukiwania dla hasztagiem, należy najpierw utworzyć [widżetów w serwisie Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="bcac7-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="bcac7-116">Po utworzeniu usługi widżet, zostanie wyświetlony identyfikator elementu widget. Przekażesz ten widżet identyfikator jako parametr podczas wywoływania metody pomocnika, które pokazują elementu widget.</span><span class="sxs-lookup"><span data-stu-id="bcac7-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="bcac7-117">Ten temat został napisany dla wersji 1.1 interfejsu API usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="bcac7-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="bcac7-118">Dodając bezpośrednio kodu Pomocnik usługi Twitter do projektu, należy zaktualizować kod pomocnika zmiany interfejsu API usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="bcac7-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="bcac7-119">Aby uzyskać informacje o instalowaniu programu WebMatrix, zobacz [Przedstawiamy ASP.NET Web Pages 2 — wprowadzenie](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="bcac7-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="bcac7-120">Dodaj Pomocnik usługi Twitter do projektu</span><span class="sxs-lookup"><span data-stu-id="bcac7-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="bcac7-121">Aby dodać Pomocnik usługi Twitter, należy najpierw dodać folder o nazwie **aplikacji\_kodu** do projektu.</span><span class="sxs-lookup"><span data-stu-id="bcac7-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="bcac7-122">Następnie utwórz plik o nazwie **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="bcac7-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Folderze App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="bcac7-124">Zamień domyślny kod w Twitter.cshtml następujący kod.</span><span class="sxs-lookup"><span data-stu-id="bcac7-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="bcac7-125">Wywoływanie metod Twitter ze stron sieci web</span><span class="sxs-lookup"><span data-stu-id="bcac7-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="bcac7-126">Poniższy przykład przedstawia sposób użycia metody Pomocnik usługi Twitter ze strony w twoim projekcie.</span><span class="sxs-lookup"><span data-stu-id="bcac7-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="bcac7-127">W projekcie należy zastąpić wartości parametru z wartościami, które mają zastosowanie do Twoich potrzeb.</span><span class="sxs-lookup"><span data-stu-id="bcac7-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="bcac7-128">Identyfikatory podana, widżetów można użyć, aby poznać sposób działania metody, ale chcesz wygenerować własne elementy widget w projekcie.</span><span class="sxs-lookup"><span data-stu-id="bcac7-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="bcac7-129">Nie wszystkie parametry poniżej są wymagane.</span><span class="sxs-lookup"><span data-stu-id="bcac7-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="bcac7-130">Następujące parametry opcjonalne są używane do dostosować sposób wyświetlania przycisku lub elementu widget.</span><span class="sxs-lookup"><span data-stu-id="bcac7-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="bcac7-131">Na przykład przycisk Obserwuj wymaga tylko nazwę użytkownika, aby wykonać, ale w przykładzie pokazano sposób obejmują liczbę obserwatorów i jak określić rozmiar przycisku i język.</span><span class="sxs-lookup"><span data-stu-id="bcac7-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="bcac7-132">Zobacz wyniki</span><span class="sxs-lookup"><span data-stu-id="bcac7-132">See the results</span></span>

<span data-ttu-id="bcac7-133">Powyższy kod generuje następujące przyciski i elementy widget.</span><span class="sxs-lookup"><span data-stu-id="bcac7-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="bcac7-134">Te przyciski i elementy widget są w pełni funkcjonalne, nie zrzuty ekranu.</span><span class="sxs-lookup"><span data-stu-id="bcac7-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="bcac7-135">Postępuj zgodnie z przycisku jest wyświetlany w języku hiszpańskim, ponieważ ustawiono parametr języka **es**.</span><span class="sxs-lookup"><span data-stu-id="bcac7-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="bcac7-136">Postępuj zgodnie z przycisku</span><span class="sxs-lookup"><span data-stu-id="bcac7-136">Follow Button</span></span>

<span data-ttu-id="bcac7-137">[Postępuj zgodnie z @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="bcac7-137">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="bcac7-138">Przycisk tweetu</span><span class="sxs-lookup"><span data-stu-id="bcac7-138">Tweet Button</span></span>

<span data-ttu-id="bcac7-139">[Tweetu](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="bcac7-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="bcac7-140">Oś czasu użytkownika (profil)</span><span class="sxs-lookup"><span data-stu-id="bcac7-140">User Timeline (Profile)</span></span>

<span data-ttu-id="bcac7-141">[Tweety według @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="bcac7-141">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="bcac7-142">Ulubione</span><span class="sxs-lookup"><span data-stu-id="bcac7-142">Favorites</span></span>

<span data-ttu-id="bcac7-143">[Ulubione Tweety według @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="bcac7-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="bcac7-144">Lista</span><span class="sxs-lookup"><span data-stu-id="bcac7-144">List</span></span>

<span data-ttu-id="bcac7-145">[Opublikuje z @Microsoft/MS \_konsumenta\_Bollingera](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="bcac7-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="bcac7-146">Wyszukaj</span><span class="sxs-lookup"><span data-stu-id="bcac7-146">Search</span></span>

<span data-ttu-id="bcac7-147">[Opublikuje tweet dotyczący &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="bcac7-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
