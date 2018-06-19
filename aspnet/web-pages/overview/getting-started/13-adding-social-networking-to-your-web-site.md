---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Dodawanie sieci społecznościowych w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale opisano sposób zintegrowanie witryny z usługami sieci społecznościowych. W tym rozdziale dowiesz się, jak umożliwia użytkownikom zakładki/link witryny sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899346"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="78f94-104">Dodawanie sieci społecznościowych do stron sieci Web platformy ASP.NET (Razor) witryn</span><span class="sxs-lookup"><span data-stu-id="78f94-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="78f94-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="78f94-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="78f94-106">W tym artykule opisano sposób dodawania łączy sieci społecznościowych dla usługi Facebook, Twitter, Reddit i Digg do stron w witrynie sieci Web platformy ASP.NET Web Pages (Razor) i sposobu uwzględniania Twitter źródeł danych, karty graczy Xbox i Gravatar obrazów.</span><span class="sxs-lookup"><span data-stu-id="78f94-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="78f94-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="78f94-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="78f94-108">Jak umożliwia użytkownikom zakładki/link lokacji.</span><span class="sxs-lookup"><span data-stu-id="78f94-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="78f94-109">Jak dodać Kanał informacyjny usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="78f94-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="78f94-110">Jak dodać Facebook **jak** przycisku do stron.</span><span class="sxs-lookup"><span data-stu-id="78f94-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="78f94-111">Sposób renderowania Gravatar.com obrazów.</span><span class="sxs-lookup"><span data-stu-id="78f94-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="78f94-112">Jak wyświetlić karty graczy Xbox w witrynie.</span><span class="sxs-lookup"><span data-stu-id="78f94-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="78f94-113">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="78f94-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="78f94-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="78f94-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="78f94-115">Biblioteka pomocnika sieci Web platformy ASP.NET (pakiet NuGet)</span><span class="sxs-lookup"><span data-stu-id="78f94-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="78f94-116">W tym samouczku współdziała również z 3 stron sieci Web ASP.NET, z wyjątkiem części korzystające z biblioteki pomocnika sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="78f94-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="78f94-117">Łączenie witryny sieci Web w witrynach sieci społecznościowych</span><span class="sxs-lookup"><span data-stu-id="78f94-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="78f94-118">Jeśli osoby lubią coś w swojej witrynie, często chcą udostępniać znajomych.</span><span class="sxs-lookup"><span data-stu-id="78f94-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="78f94-119">Umożliwia to łatwe przez wyświetlanie symboli (ikony), które umożliwiającego udostępnianie strony na Digg, Reddit, Facebook, Twitter lub podobne witryny.</span><span class="sxs-lookup"><span data-stu-id="78f94-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="78f94-120">Aby wyświetlić te symbole, Dodaj `LinkSharecode` pomocnika do strony.</span><span class="sxs-lookup"><span data-stu-id="78f94-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="78f94-121">Osób odwiedzających stronę, można kliknąć poszczególne symbolu.</span><span class="sxs-lookup"><span data-stu-id="78f94-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="78f94-122">Jeśli użytkownicy mają konta z tej witryny sieci społecznościowych, one następnie przesłanie łącze do strony w tej witrynie.</span><span class="sxs-lookup"><span data-stu-id="78f94-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Obraz 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="78f94-124">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie został jeszcze dodany niego — Utwórz stronę o nazwie *ListLinkShare.cshtml* i Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="78f94-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="78f94-125">W tym przykładzie gdy `LinkShare` pomocnika działa, tytuł strony jest przekazywana jako parametr, który z kolei przekazuje tytuł strony w witrynie sieci społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="78f94-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="78f94-126">Jednak można przekazać w dowolny ciąg, który ma.</span><span class="sxs-lookup"><span data-stu-id="78f94-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="78f94-127">W tym przykładzie również określa, które witrynami sieci społecznościowych w celu dołączenia do listy.</span><span class="sxs-lookup"><span data-stu-id="78f94-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="78f94-128">Można określić witrynami sieci społecznościowych, które mają zastosowanie do witryny.</span><span class="sxs-lookup"><span data-stu-id="78f94-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="78f94-129">Uruchom *ListLinkShare.cshtml* strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="78f94-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="78f94-130">(Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.)</span><span class="sxs-lookup"><span data-stu-id="78f94-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="78f94-131">Kliknij symbol dla jednej z witryn, które jest zalogowany w usłudze.</span><span class="sxs-lookup"><span data-stu-id="78f94-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="78f94-132">Link prowadzi do strony w witrynie wybranej sieci społecznościowych, gdzie można udostępniać link.</span><span class="sxs-lookup"><span data-stu-id="78f94-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="78f94-133">Na przykład kliknięcie łącza Reddit, użytkownik jest przekierowanie do `submit to reddit` w witrynie Reddit.</span><span class="sxs-lookup"><span data-stu-id="78f94-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![Obraz 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="78f94-135">Dodawanie Twitter źródła danych</span><span class="sxs-lookup"><span data-stu-id="78f94-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="78f94-136">Aby dowiedzieć się, jak przy użyciu Pomocnika Twitter, która jest zgodna z bieżącą wersją interfejsu API usługi Twitter, zobacz [Pomocnik usługi Twitter](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="78f94-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="78f94-137">Ten przykład przedstawia, jak napisać własne pomocnika, więc można łatwo używać kod z wielu stron.</span><span class="sxs-lookup"><span data-stu-id="78f94-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="78f94-138">Wyświetlania serwisu Facebook &quot;jak&quot; przycisku</span><span class="sxs-lookup"><span data-stu-id="78f94-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="78f94-139">W niektórych przypadkach z najlepszym rozwiązaniem jest uzyskanie kodu bezpośrednio od dostawcy sieci społecznościowej zamiast polegania na pomocnika.</span><span class="sxs-lookup"><span data-stu-id="78f94-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="78f94-140">Jest to szczególnie w przypadku dostawcy sieci społecznościowej aktualizuje jego opcje szybciej niż pomocnika jest aktualizowany.</span><span class="sxs-lookup"><span data-stu-id="78f94-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="78f94-141">Aby dodać funkcje Facebook (takich jak przycisk) do swojej witryny, można pobrać fragmentów kodu z [developers.facebook.com](https://developers.facebook.com/) lokacji.</span><span class="sxs-lookup"><span data-stu-id="78f94-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="78f94-142">W witrynie Facebook używać swoich narzędzi do generowania fragmenty kodu, które ma zastosowanie do witryny.</span><span class="sxs-lookup"><span data-stu-id="78f94-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="78f94-143">Następujący wyróżniony kod jest kod, który został pobrany z narzędzia takie jak przycisk w witrynie developers.facebook.com.</span><span class="sxs-lookup"><span data-stu-id="78f94-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="78f94-144">Należy podać własne identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="78f94-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="78f94-145">Renderowanie obrazu Gravatar</span><span class="sxs-lookup"><span data-stu-id="78f94-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="78f94-146">A *Gravatar* ( &quot;globalnie rozpoznanym awatara&quot;) jest obraz, który może być używany w wielu witryn sieci Web jako Awatar &#8212; oznacza to, że obraz, który reprezentuje użytkownik.</span><span class="sxs-lookup"><span data-stu-id="78f94-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="78f94-147">Na przykład można zidentyfikować osoby w wpis na forum w komentarzu blog, Gravatar i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="78f94-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="78f94-148">(Możesz zarejestrować własne Gravatar w witrynie Gravatar w [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Jeśli chcesz wyświetlić obrazy obok nazwy lub adresy e-mail użytkowników w witrynie sieci Web, można użyć pomocnika Gravatar.</span><span class="sxs-lookup"><span data-stu-id="78f94-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="78f94-149">W tym przykładzie jest używany pojedynczy Gravatar, reprezentujący samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="78f94-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="78f94-150">Innym sposobem użycia Gravatar jest innym użytkownikom, podaj swój adres Gravatar po zarejestrowaniu się w witrynie.</span><span class="sxs-lookup"><span data-stu-id="78f94-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="78f94-151">(Można poznać sposoby innym użytkownikom zarejestrowanie w [Dodawanie zabezpieczeń i członkostwo w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).) Następnie przy każdym możesz wyświetlić informacje dotyczące tego użytkownika, do których wyświetlania nazwę użytkownika można dodać tylko Gravatar.</span><span class="sxs-lookup"><span data-stu-id="78f94-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="78f94-152">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.</span><span class="sxs-lookup"><span data-stu-id="78f94-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="78f94-153">Tworzenie nowej strony sieci web o nazwie *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78f94-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="78f94-154">Dodaj następujący kod do pliku:</span><span class="sxs-lookup"><span data-stu-id="78f94-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="78f94-155">`Gravatar.GetHtml` Metoda Wyświetla Gravatar obrazu na stronie.</span><span class="sxs-lookup"><span data-stu-id="78f94-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="78f94-156">Aby zmienić rozmiar obrazu, może zawierać wiele jako drugiego parametru.</span><span class="sxs-lookup"><span data-stu-id="78f94-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="78f94-157">Rozmiar domyślny to 80.</span><span class="sxs-lookup"><span data-stu-id="78f94-157">The default size is 80.</span></span> <span data-ttu-id="78f94-158">Liczby mniejszej niż 80 upewnij obrazu mniejsze.</span><span class="sxs-lookup"><span data-stu-id="78f94-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="78f94-159">Liczby większe niż 80 powiększenia obrazu.</span><span class="sxs-lookup"><span data-stu-id="78f94-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="78f94-160">W `Gravatar.GetHtml` metody, zastąpić `<Your Gravatar account here>` z adresem e-mail używanego konta Gravatar.</span><span class="sxs-lookup"><span data-stu-id="78f94-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="78f94-161">(Jeśli nie masz konta Gravatar, można użyć adresu e-mail osoby, która jest).</span><span class="sxs-lookup"><span data-stu-id="78f94-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="78f94-162">Uruchom strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="78f94-162">Run the page in your browser.</span></span> <span data-ttu-id="78f94-163">Zostaje wyświetlona strona dwa obrazy Gravatar dla określonego adresu e-mail.</span><span class="sxs-lookup"><span data-stu-id="78f94-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="78f94-164">Drugi obrazu jest mniejszy niż pierwszy.</span><span class="sxs-lookup"><span data-stu-id="78f94-164">The second image is smaller than the first.</span></span> 

    ![Obraz 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="78f94-166">Wyświetlanie kart graczy Xbox</span><span class="sxs-lookup"><span data-stu-id="78f94-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="78f94-167">Podczas osób gier Microsoft Xbox online, każdy użytkownik ma unikatowy identyfikator.</span><span class="sxs-lookup"><span data-stu-id="78f94-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="78f94-168">Statystyki są przechowywane dla każdego player w formularzu graczy karty, która zawiera ich reputacji, wynik graczy i ostatnio odtwarzane gry.</span><span class="sxs-lookup"><span data-stu-id="78f94-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="78f94-169">Jeśli jesteś graczy Xbox karty graczy można wyświetlać na stronach witryny sieci za pomocą `GamerCard` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="78f94-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="78f94-170">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.</span><span class="sxs-lookup"><span data-stu-id="78f94-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="78f94-171">Utwórz nową stronę o nazwie *XboxGamer.cshtml* i Dodaj następujący kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="78f94-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="78f94-172">Możesz użyć `GamerCard.GetHtml` właściwości w celu określenia alias graczy karty do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="78f94-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="78f94-173">Uruchom strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="78f94-173">Run the page in your browser.</span></span> <span data-ttu-id="78f94-174">Na stronie są wyświetlane określonej karty graczy Xbox.</span><span class="sxs-lookup"><span data-stu-id="78f94-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Obraz 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
