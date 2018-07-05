---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Wyświetlanie map we wzorcu ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób wyświetlania mapy interaktywne na stronach w witrynie internetowej ASP.NET Web Pages (Razor) na podstawie związane z mapowaniem usług udostępnianych przez usługi Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 210cc37427024a4e8cae309634141900bb28b55e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368838"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="9f63e-103">Wyświetlanie map w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="9f63e-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="9f63e-104">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9f63e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9f63e-105">W tym artykule opisano sposób wyświetlania mapy interaktywne na stronach w witrynie internetowej ASP.NET Web Pages (Razor) na podstawie związane z mapowaniem usług udostępnianych przez usługi Bing, Google, MapQuest i Yahoo.</span><span class="sxs-lookup"><span data-stu-id="9f63e-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="9f63e-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="9f63e-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="9f63e-107">Jak można wygenerować mapy na podstawie adresu.</span><span class="sxs-lookup"><span data-stu-id="9f63e-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="9f63e-108">Jak można wygenerować mapy na podstawie współrzędnych szerokości i długości geograficznej.</span><span class="sxs-lookup"><span data-stu-id="9f63e-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="9f63e-109">Jak zarejestrować konto dla deweloperów map Bing i uzyskiwanie klucza do użycia z usługą mapy Bing.</span><span class="sxs-lookup"><span data-stu-id="9f63e-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="9f63e-110">Jest to ASP.NET wprowadzonej w artykule:</span><span class="sxs-lookup"><span data-stu-id="9f63e-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="9f63e-111">`Maps` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="9f63e-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9f63e-112">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="9f63e-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9f63e-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="9f63e-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="9f63e-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="9f63e-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="9f63e-115">W tym samouczku współpracuje również z programu WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="9f63e-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="9f63e-116">Na stronach sieci Web, można wyświetlić mapy na stronie, używając `Maps` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="9f63e-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="9f63e-117">Można wygenerować mapy albo na podstawie adresu lub zbiór współrzędne długości i szerokości geograficznej.</span><span class="sxs-lookup"><span data-stu-id="9f63e-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="9f63e-118">`Maps` Klasy pozwala wywoływać aparatów popularnych mapy, takich jak Bing, Google, MapQuest i Yahoo.</span><span class="sxs-lookup"><span data-stu-id="9f63e-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="9f63e-119">Kroki, aby dodać mapowanie do strony są takie same niezależnie od tego, który aparatów mapy wywołania.</span><span class="sxs-lookup"><span data-stu-id="9f63e-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="9f63e-120">Wystarczy dodać odwołanie do pliku JavaScript, która sprawia, że dostępne metody, aby wyświetlić mapę, a następnie wywołać metody `Maps` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="9f63e-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="9f63e-121">Wybierz usługę mapy na podstawie którego `Maps` używanej metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="9f63e-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="9f63e-122">Można użyć dowolnego z nich:</span><span class="sxs-lookup"><span data-stu-id="9f63e-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="9f63e-123">Instalowanie elementów, których potrzebujesz</span><span class="sxs-lookup"><span data-stu-id="9f63e-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="9f63e-124">Do wyświetlania mapy, potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="9f63e-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="9f63e-125">`Maps` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="9f63e-125">The `Maps` helper.</span></span> <span data-ttu-id="9f63e-126">Tego pomocnika jest w wersji 2 bibliotekę pomocników platformy ASP.NET sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9f63e-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="9f63e-127">Jeśli nie został jeszcze dodany biblioteki, można zainstalować go w witrynie, jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="9f63e-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="9f63e-128">Aby uzyskać więcej informacji, zobacz [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="9f63e-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="9f63e-129">(W galerii, wyszukaj `microsoft-web-helpers` pakietu.)</span><span class="sxs-lookup"><span data-stu-id="9f63e-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="9f63e-130">Biblioteka jQuery.</span><span class="sxs-lookup"><span data-stu-id="9f63e-130">The jQuery library.</span></span> <span data-ttu-id="9f63e-131">Niektóre szablony witryn programu WebMatrix już zawierają biblioteki jQuery w ich *skryptu* folderów.</span><span class="sxs-lookup"><span data-stu-id="9f63e-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="9f63e-132">Jeśli nie masz tych bibliotek, możesz pobrać najnowsze biblioteki jQuery bezpośrednio z [jQuery.org](http://jQuery.org) lokacji.</span><span class="sxs-lookup"><span data-stu-id="9f63e-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="9f63e-133">Lub możesz utworzyć nową witrynę za pomocą szablonu (na przykład **witryny początkowej** szablonu) a następnie skopiuj pliki jQuery z tej lokacji, do bieżącej lokacji.</span><span class="sxs-lookup"><span data-stu-id="9f63e-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="9f63e-134">Ponadto jeśli chcesz użyć usługi mapy Bing, należy najpierw utworzyć konto (wersja bezpłatna) i uzyskiwanie klucza.</span><span class="sxs-lookup"><span data-stu-id="9f63e-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="9f63e-135">Aby uzyskać klucz, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="9f63e-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="9f63e-136">Utwórz konto na [mapy Bing dla konta dewelopera](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f63e-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="9f63e-137">Musi mieć konto Microsoft (Windows Live ID) również.</span><span class="sxs-lookup"><span data-stu-id="9f63e-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="9f63e-138">Można określić chcesz użyć klucza dla **oceny i testowania**.</span><span class="sxs-lookup"><span data-stu-id="9f63e-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="9f63e-139">Jeśli testujesz funkcji mapowania na swoim komputerze przy użyciu programu WebMatrix i usług IIS Express, przejdź **witryny** obszaru roboczego i zanotuj adres URL witryny (na przykład `http://localhost:50408`, mimo że numer portu, prawdopodobnie będą inne).</span><span class="sxs-lookup"><span data-stu-id="9f63e-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="9f63e-140">Możesz użyć tej funkcji *localhost* adres witryny podczas rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="9f63e-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="9f63e-141">Po zarejestrowaniu konta, przejdź do Centrum konta map Bing i kliknij **Utwórz lub Wyświetl klucze**:</span><span class="sxs-lookup"><span data-stu-id="9f63e-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![Mapowanie 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="9f63e-143">Zapisz klucz, który tworzy Bing.</span><span class="sxs-lookup"><span data-stu-id="9f63e-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="9f63e-144">Tworzenie mapy na podstawie adresu (przy użyciu Google)</span><span class="sxs-lookup"><span data-stu-id="9f63e-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="9f63e-145">Poniższy przykład pokazuje, jak utworzyć stronę, która renderuje mapy na podstawie adresu.</span><span class="sxs-lookup"><span data-stu-id="9f63e-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="9f63e-146">W tym przykładzie pokazano, jak używać mapy Google.</span><span class="sxs-lookup"><span data-stu-id="9f63e-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="9f63e-147">Utwórz plik o nazwie *MapAddress.cshtml* w katalogu głównym witryny.</span><span class="sxs-lookup"><span data-stu-id="9f63e-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="9f63e-148">Na tej stronie spowoduje wygenerowanie mapy na podstawie adresu i przekażesz do niego.</span><span class="sxs-lookup"><span data-stu-id="9f63e-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="9f63e-149">Skopiuj następujący kod do pliku, zastępując istniejącą zawartość.</span><span class="sxs-lookup"><span data-stu-id="9f63e-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="9f63e-150">Zwróć uwagę, następujące funkcje strony:</span><span class="sxs-lookup"><span data-stu-id="9f63e-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="9f63e-151">`<script>` Element `<head>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9f63e-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="9f63e-152">W tym przykładzie `<script>` odwołania do elementu *jquery 1.6.4.min.js* pliku, który jest zminimalizowany (skompresowane) wersję biblioteki jQuery, wersja 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="9f63e-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="9f63e-153">Należy pamiętać, odwołanie założono, że *js* plik znajduje się w *skrypty* folder witryny.</span><span class="sxs-lookup"><span data-stu-id="9f63e-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="9f63e-154">Jeśli używasz innej wersji biblioteki jQuery po prostu upewnij się, że wskazuje do tej wersji poprawnie.</span><span class="sxs-lookup"><span data-stu-id="9f63e-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="9f63e-155">Wywołanie `@Maps.GetGoogleHtml` w treści strony.</span><span class="sxs-lookup"><span data-stu-id="9f63e-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="9f63e-156">Aby zamapować adresu, należy przekazać ciąg adresu.</span><span class="sxs-lookup"><span data-stu-id="9f63e-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="9f63e-157">Metody silników mapy działają w podobny sposób (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="9f63e-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="9f63e-158">Uruchom stronę, a następnie wprowadź adres.</span><span class="sxs-lookup"><span data-stu-id="9f63e-158">Run the page and enter an address.</span></span> <span data-ttu-id="9f63e-159">Zostanie wyświetlona strona mapie, według map programu Google, pokazujący określonej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="9f63e-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![Mapowanie 1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="9f63e-161">Tworzenie mapy, w oparciu o długość i szerokość geograficzną służy do koordynowania (przy użyciu usługi Bing)</span><span class="sxs-lookup"><span data-stu-id="9f63e-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="9f63e-162">Ten przykład przedstawia sposób tworzenia na podstawie współrzędnych mapy.</span><span class="sxs-lookup"><span data-stu-id="9f63e-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="9f63e-163">Ten przykład pokazuje, jak używać usługi mapy Bing i sposobu uwzględniania klucza usługi Bing.</span><span class="sxs-lookup"><span data-stu-id="9f63e-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="9f63e-164">(Można utworzyć mapę na podstawie współrzędnych przy użyciu innych silników mapy także bez korzystania z kluczem usługi Bing).</span><span class="sxs-lookup"><span data-stu-id="9f63e-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="9f63e-165">Utwórz plik o nazwie *MapCoordinates.cshtml* w katalogu głównym witryny i Zastąp istniejącą zawartość z następującymi kodu i znaczników:</span><span class="sxs-lookup"><span data-stu-id="9f63e-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="9f63e-166">Zastąp `your-key-here` za pomocą klucza usługi mapy Bing, wcześniej wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="9f63e-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="9f63e-167">Uruchom *MapCoordinates.cshtml* strony, wprowadzić współrzędne geograficzne, a następnie kliknij przycisk **mapy ją!**</span><span class="sxs-lookup"><span data-stu-id="9f63e-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="9f63e-168">przycisk.</span><span class="sxs-lookup"><span data-stu-id="9f63e-168">button.</span></span> <span data-ttu-id="9f63e-169">(Jeśli nie znasz wszystkie współrzędne, spróbuj wykonać następujące czynności.</span><span class="sxs-lookup"><span data-stu-id="9f63e-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="9f63e-170">Jest to lokalizacja na uczelniach Microsoft Redmond).</span><span class="sxs-lookup"><span data-stu-id="9f63e-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="9f63e-171">Szerokość geograficzna: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="9f63e-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="9f63e-172">Longitude: -122.158317565918</span><span class="sxs-lookup"><span data-stu-id="9f63e-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="9f63e-173">Ta strona jest wyświetlana przy użyciu określonych współrzędnych.</span><span class="sxs-lookup"><span data-stu-id="9f63e-173">The page is displayed using the coordinates that you specified.</span></span>

     ![Mapowanie 3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="9f63e-175">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9f63e-175">Additional Resources</span></span>


[<span data-ttu-id="9f63e-176">Dokumentacja interfejsu API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="9f63e-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
