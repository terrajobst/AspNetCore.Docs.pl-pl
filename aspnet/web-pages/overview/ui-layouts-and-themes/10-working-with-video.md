---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Wyświetlanie wideo w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale opisano sposób wyświetlania wideo w stron ASP.NET Web Pages ze stroną składni Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153683"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="30bb6-103">Wyświetlanie wideo w sieci Web platformy ASP.NET (Razor) stron witryny</span><span class="sxs-lookup"><span data-stu-id="30bb6-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="30bb6-104">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="30bb6-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="30bb6-105">W tym artykule opisano sposób użycia odtwarzacza wideo (nośników) w witrynie sieci Web platformy ASP.NET Web Pages (Razor), aby zezwolić użytkownikom na wyświetlanie filmów wideo, które są przechowywane w witrynie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="30bb6-106">Strony ASP.NET Web Pages o składni Razor umożliwia Flash (*SWF*), Media Player (*wmv*) i Silverlight (*XAP*) wideo.</span><span class="sxs-lookup"><span data-stu-id="30bb6-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="30bb6-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="30bb6-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="30bb6-108">Jak wybrać odtwarzacza wideo.</span><span class="sxs-lookup"><span data-stu-id="30bb6-108">How to choose a video player.</span></span>
> - <span data-ttu-id="30bb6-109">Jak dodać wideo do strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="30bb6-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="30bb6-110">Jak ustawić atrybuty odtwarzacza wideo.</span><span class="sxs-lookup"><span data-stu-id="30bb6-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="30bb6-111">Są to ASP.NET Razor strony funkcje dodane w artykule:</span><span class="sxs-lookup"><span data-stu-id="30bb6-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="30bb6-112">`Video` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="30bb6-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="30bb6-113">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="30bb6-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="30bb6-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="30bb6-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="30bb6-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="30bb6-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="30bb6-116">W tym samouczku współdziała również z 3 programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="30bb6-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="30bb6-117">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="30bb6-117">Introduction</span></span>

<span data-ttu-id="30bb6-118">Możesz wyświetlić wideo w witrynie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-118">You might want to display a video on your site.</span></span> <span data-ttu-id="30bb6-119">Jednym ze sposobów w tym celu jest łącze do witryny, która ma już wideo, takich jak YouTube.</span><span class="sxs-lookup"><span data-stu-id="30bb6-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="30bb6-120">Jeśli chcesz osadzenie klipu wideo w tych witrynach bezpośrednio w własne strony można zwykle uzyskać kod znaczników HTML z witryny i skopiuj go na stronie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="30bb6-121">Na przykład poniższy przykład przedstawia sposób osadzenia YouTube wideo:</span><span class="sxs-lookup"><span data-stu-id="30bb6-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="30bb6-122">Chcąc odtwarzania wideo w serwisie (a nie w publicznej witryny udostępniania wideo), nie można bezpośrednio połączyć za pomocą znacznika osadzonego następująco.</span><span class="sxs-lookup"><span data-stu-id="30bb6-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="30bb6-123">Jednak odtwarzania wideo z witryny przy użyciu `Video` pomocnika, która renderuje Windows media player bezpośrednio na stronie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="30bb6-124">Wybieranie odtwarzacza wideo</span><span class="sxs-lookup"><span data-stu-id="30bb6-124">Choosing a Video Player</span></span>

<span data-ttu-id="30bb6-125">Istnieje wiele formatów plików wideo, a każdy format wymaga zwykle od innego i inny sposób, aby skonfigurować odtwarzacza.</span><span class="sxs-lookup"><span data-stu-id="30bb6-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="30bb6-126">Strony ASP.NET Razor można odtwarzanie wideo na stronie sieci web za pomocą `Video` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="30bb6-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="30bb6-127">`Video` Pomocnika upraszcza proces osadzanie klipów wideo na stronie sieci web, ponieważ automatycznie generuje `object` i `embed` elementów HTML, które zwykle są używane do dodawania wideo do strony.</span><span class="sxs-lookup"><span data-stu-id="30bb6-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="30bb6-128">`Video` Pomocnika obsługuje następujące odtwarzacze multimedialne:</span><span class="sxs-lookup"><span data-stu-id="30bb6-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="30bb6-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="30bb6-129">Adobe Flash</span></span>
- <span data-ttu-id="30bb6-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="30bb6-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="30bb6-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="30bb6-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="30bb6-132">Flash Player</span><span class="sxs-lookup"><span data-stu-id="30bb6-132">The Flash Player</span></span>

<span data-ttu-id="30bb6-133">`Flash` Odtwarzacza `Video` pomocnika umożliwiają odtwarzania wideo Flash (*SWF* pliki) na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="30bb6-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="30bb6-134">Co najmniej należy podać ścieżkę do pliku wideo.</span><span class="sxs-lookup"><span data-stu-id="30bb6-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="30bb6-135">Jeśli określisz nic, ale ścieżka odtwarzacz korzysta z wartości domyślnych, które są ustawiane przez bieżącą wersję Flash.</span><span class="sxs-lookup"><span data-stu-id="30bb6-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="30bb6-136">Typowy domyślne ustawienia są:</span><span class="sxs-lookup"><span data-stu-id="30bb6-136">Typical default settings are:</span></span>

- <span data-ttu-id="30bb6-137">Przy użyciu domyślnej szerokości i wysokości wyświetlania wideo i bez kolor tła.</span><span class="sxs-lookup"><span data-stu-id="30bb6-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="30bb6-138">Odtwarzanie odbywa się automatycznie podczas ładowania strony.</span><span class="sxs-lookup"><span data-stu-id="30bb6-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="30bb6-139">Wideo wykonuje pętlę w sposób ciągły, dopóki nie zostanie jawnie zatrzymana.</span><span class="sxs-lookup"><span data-stu-id="30bb6-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="30bb6-140">Aby wyświetlić wszystkie wideo, a nie Przycinanie wideo do określonego rozmiaru skalowania wideo.</span><span class="sxs-lookup"><span data-stu-id="30bb6-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="30bb6-141">Odtwarzania wideo w oknie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="30bb6-142">Odtwarzacz MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="30bb6-142">The MediaPlayer Player</span></span>

<span data-ttu-id="30bb6-143">`MediaPlayer` Odtwarzacza `Video` pomocnika umożliwia odtwarzania wideo Windows Media (*wmv* plików), Windows Media audio (*.wma* plików) i MP3 (*MP3* pliki) na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="30bb6-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="30bb6-144">Musi zawierać ścieżkę do pliku multimediów do odtwarzania; wszystkie inne parametry są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="30bb6-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="30bb6-145">Jeśli określisz tylko ścieżka odtwarzacz korzysta z domyślnych ustawień ustawić przez bieżącą wersję MediaPlayer, takich jak:</span><span class="sxs-lookup"><span data-stu-id="30bb6-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="30bb6-146">Zawartość wideo jest wyświetlana, przy użyciu domyślnej szerokości i wysokości.</span><span class="sxs-lookup"><span data-stu-id="30bb6-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="30bb6-147">Odtwarzanie odbywa się automatycznie podczas ładowania strony.</span><span class="sxs-lookup"><span data-stu-id="30bb6-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="30bb6-148">Wideo jest odtwarzany raz (go nie pętla).</span><span class="sxs-lookup"><span data-stu-id="30bb6-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="30bb6-149">Odtwarzacz przedstawia pełny zestaw kontrolek interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="30bb6-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="30bb6-150">Odtwarzania wideo w oknie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="30bb6-151">Odtwarzacz Silverlight</span><span class="sxs-lookup"><span data-stu-id="30bb6-151">The Silverlight Player</span></span>

<span data-ttu-id="30bb6-152">`Silverlight` Odtwarzacza `Video` pomocnika umożliwia odtwarzanie wideo Windows Media (*wmv* plików), Windows Media Audio (*.wma* plików) i MP3 (*MP3* pliki).</span><span class="sxs-lookup"><span data-stu-id="30bb6-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="30bb6-153">Należy ustawić parametr path, aby wskazać pakiet aplikacji oparte na technologii Silverlight (*XAP* pliku).</span><span class="sxs-lookup"><span data-stu-id="30bb6-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="30bb6-154">Należy również ustawić parametrów szerokości i wysokości.</span><span class="sxs-lookup"><span data-stu-id="30bb6-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="30bb6-155">Wszystkie inne parametry są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="30bb6-155">All other parameters are optional.</span></span> <span data-ttu-id="30bb6-156">Korzystając z Silverlight odtwarzacza wideo, jeśli ustawisz wymaganych parametrów, Silverlight wyświetlane wideo bez kolor tła.</span><span class="sxs-lookup"><span data-stu-id="30bb6-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="30bb6-157">Jeśli nie znasz już program Silverlight: *XAP* pliku to skompresowany plik, który zawiera instrukcje układu w *.xaml* pliku kodu zarządzanego w zestawy i opcjonalne zasoby.</span><span class="sxs-lookup"><span data-stu-id="30bb6-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="30bb6-158">Można utworzyć *XAP* pliku w programie Visual Studio jako projekt aplikacji Silverlight.</span><span class="sxs-lookup"><span data-stu-id="30bb6-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="30bb6-159">`Silverlight` Odtwarzacza wideo używa zarówno ustawienia, które zapewniają odtwarzacza i ustawienia, które zostały opublikowane w *XAP* pliku.</span><span class="sxs-lookup"><span data-stu-id="30bb6-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="30bb6-160">Typy MIME</span><span class="sxs-lookup"><span data-stu-id="30bb6-160">MIME Types</span></span>
> 
> <span data-ttu-id="30bb6-161">Gdy przeglądarka pobierze plik, przeglądarka sprawdza, czy typ pliku zgodny typ MIME, który jest określony dla dokumentu, który jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="30bb6-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="30bb6-162">Typ MIME jest typu lub nośnik typ zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="30bb6-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="30bb6-163">`Video` Pomocnika korzysta z następujących typów MIME:</span><span class="sxs-lookup"><span data-stu-id="30bb6-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="30bb6-164">Odtwarzanie wideo Flash (SWF)</span><span class="sxs-lookup"><span data-stu-id="30bb6-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="30bb6-165">Ta procedura pokazuje, jak odtwarzanie wideo Flash o nazwie *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="30bb6-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="30bb6-166">W procedurze założono, że masz folder o nazwie *nośnika* w witrynie i *SWF* plik znajduje się w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="30bb6-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="30bb6-167">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie zostało dodane.</span><span class="sxs-lookup"><span data-stu-id="30bb6-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="30bb6-168">W witrynie sieci Web, należy dodać stronę i nadaj mu nazwę *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="30bb6-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="30bb6-169">Dodaj następujący kod do strony:</span><span class="sxs-lookup"><span data-stu-id="30bb6-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="30bb6-170">Uruchom strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="30bb6-170">Run the page in a browser.</span></span> <span data-ttu-id="30bb6-171">(Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.) Ta strona jest wyświetlana i odtwarzanie odbywa się automatycznie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="30bb6-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="30bb6-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="30bb6-173">Można ustawić `quality` parametr Flash wideo, aby `low`, `autolow`, `autohigh`, `medium`, `high`, i `best`:</span><span class="sxs-lookup"><span data-stu-id="30bb6-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="30bb6-174">Możesz zmienić Flash wideo, aby odtworzyć za pomocą określonego rozmiaru `scale` parametr, który można ustawić dla następujących:</span><span class="sxs-lookup"><span data-stu-id="30bb6-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="30bb6-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="30bb6-175">`showall`.</span></span> <span data-ttu-id="30bb6-176">Spowoduje to uruchomienie całego wideo widoczne, zachowując jego oryginalny współczynnik proporcji.</span><span class="sxs-lookup"><span data-stu-id="30bb6-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="30bb6-177">Jednak może być na końcu obramowań na każdej stronie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="30bb6-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="30bb6-178">`noorder`.</span></span> <span data-ttu-id="30bb6-179">To skaluje wideo, zachowując jego oryginalny współczynnik proporcji, ale może zostać obcięty.</span><span class="sxs-lookup"><span data-stu-id="30bb6-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="30bb6-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="30bb6-180">`exactfit`.</span></span> <span data-ttu-id="30bb6-181">Spowoduje to uruchomienie całego wideo widoczne nie zachowując jego oryginalny współczynnik proporcji, ale mogą wystąpić zakłócenia.</span><span class="sxs-lookup"><span data-stu-id="30bb6-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="30bb6-182">Jeśli nie określisz `scale` parametru całe wideo będzie widoczny i jego oryginalny współczynnik proporcji będzie przechowywany bez żadnych przycinania.</span><span class="sxs-lookup"><span data-stu-id="30bb6-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="30bb6-183">Poniższy przykład przedstawia użycie `scale` parametru:</span><span class="sxs-lookup"><span data-stu-id="30bb6-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="30bb6-184">Flash player obsługuje tryb wideo, ustawienie o nazwie `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="30bb6-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="30bb6-185">Możesz ustawić `window`, `opaque`, i `transparent`.</span><span class="sxs-lookup"><span data-stu-id="30bb6-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="30bb6-186">Domyślnie `windowMode` ma ustawioną wartość `window`, które powoduje wyświetlenie wideo w osobnym oknie na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="30bb6-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="30bb6-187">`opaque` Ustawienie ukrywa wszystko za wideo na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="30bb6-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="30bb6-188">`transparent` Umożliwia ustawienie tła strony sieci web są widoczne wideo, zakładając, że wszystkie części klipu wideo jest niewidoczny.</span><span class="sxs-lookup"><span data-stu-id="30bb6-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="30bb6-189">Odtwarzanie MediaPlayer (*wmv*) wideo</span><span class="sxs-lookup"><span data-stu-id="30bb6-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="30bb6-190">Poniższej procedurze wyjaśniono, jak odtwarzanie wideo multimediów okna o nazwie *sample.wmv* w *nośnika* folderu.</span><span class="sxs-lookup"><span data-stu-id="30bb6-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="30bb6-191">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.</span><span class="sxs-lookup"><span data-stu-id="30bb6-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="30bb6-192">Utwórz nową stronę o nazwie *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="30bb6-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="30bb6-193">Dodaj następujący kod do strony:</span><span class="sxs-lookup"><span data-stu-id="30bb6-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="30bb6-194">Uruchom strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="30bb6-194">Run the page in a browser.</span></span> <span data-ttu-id="30bb6-195">Wideo ładuje i odtwarzany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="30bb6-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="30bb6-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="30bb6-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="30bb6-197">Można ustawić `playCount` na liczbę całkowitą, która wskazuje, ile razy, aby automatycznie odtwarzania wideo:</span><span class="sxs-lookup"><span data-stu-id="30bb6-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="30bb6-198">`uiMode` Parametr umożliwia określenie, które kontrolki widoczne w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="30bb6-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="30bb6-199">Można ustawić `uiMode` do `invisible`, `none`, `mini`, lub `full`.</span><span class="sxs-lookup"><span data-stu-id="30bb6-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="30bb6-200">Jeśli nie określisz `uiMode` parametru wideo będą wyświetlane w oknie stanu, wyszukiwanie, kontrolować przycisków i głośności oprócz okna wideo.</span><span class="sxs-lookup"><span data-stu-id="30bb6-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="30bb6-201">Formanty te będą również wyświetlane użycie odtwarzacza do odtwarzania pliku audio.</span><span class="sxs-lookup"><span data-stu-id="30bb6-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="30bb6-202">Poniżej przedstawiono przykład sposobu użycia `uiMode` parametru:</span><span class="sxs-lookup"><span data-stu-id="30bb6-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="30bb6-203">Domyślnie audio znajduje się na podczas odtwarzania wideo.</span><span class="sxs-lookup"><span data-stu-id="30bb6-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="30bb6-204">Dźwięk można wyciszanie przez ustawienie `mute` parametru na wartość true:</span><span class="sxs-lookup"><span data-stu-id="30bb6-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="30bb6-205">Można kontrolować poziom audio, wideo MediaPlayer przez ustawienie `volume` parametru na wartość z zakresu od 0 do 100.</span><span class="sxs-lookup"><span data-stu-id="30bb6-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="30bb6-206">Wartością domyślną jest 50.</span><span class="sxs-lookup"><span data-stu-id="30bb6-206">The default value is 50.</span></span> <span data-ttu-id="30bb6-207">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="30bb6-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="30bb6-208">Odtwarzanie wideo Silverlight</span><span class="sxs-lookup"><span data-stu-id="30bb6-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="30bb6-209">Ta procedura pokazuje, jak odtwarzanie wideo zawarte w programie Silverlight *.xap* strony w folderze o nazwie *nośnika*.</span><span class="sxs-lookup"><span data-stu-id="30bb6-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="30bb6-210">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.</span><span class="sxs-lookup"><span data-stu-id="30bb6-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="30bb6-211">Utwórz nową stronę o nazwie *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="30bb6-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="30bb6-212">Dodaj następujący kod do strony:</span><span class="sxs-lookup"><span data-stu-id="30bb6-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="30bb6-213">Uruchom strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="30bb6-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="30bb6-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="30bb6-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="30bb6-215">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="30bb6-215">Additional Resources</span></span>


<span data-ttu-id="30bb6-216">[Omówienie technologii Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="30bb6-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="30bb6-217">Atrybuty tagu Flash obiekt i osadzania</span><span class="sxs-lookup"><span data-stu-id="30bb6-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="30bb6-218">[Windows Media Player 11 SDK PARAM tagów](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="30bb6-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
