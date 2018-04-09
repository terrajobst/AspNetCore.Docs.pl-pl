---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Wyświetlanie mapy w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób wyświetlania interaktywnego mapy na stronach witryny sieci Web ASP.NET Web Pages (Razor) oparte na mapowania usług świadczonych przez Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="8d23a-103">Wyświetlanie mapy witryny (Razor) stron sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8d23a-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="8d23a-104">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8d23a-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8d23a-105">W tym artykule opisano sposób wyświetlania interaktywnego mapy na stronach witryny sieci Web ASP.NET Web Pages (Razor) oparte na mapowania usług świadczonych przez Bing, MapQuest, Google i Yahoo.</span><span class="sxs-lookup"><span data-stu-id="8d23a-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="8d23a-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="8d23a-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8d23a-107">Jak wygenerować mapy na podstawie adresu.</span><span class="sxs-lookup"><span data-stu-id="8d23a-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="8d23a-108">Jak wygenerować mapę oparte na współrzędne geograficzne.</span><span class="sxs-lookup"><span data-stu-id="8d23a-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="8d23a-109">Jak zarejestrować konto deweloperów map Bing i uzyskiwanie klucza do użycia z usługą mapy Bing.</span><span class="sxs-lookup"><span data-stu-id="8d23a-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="8d23a-110">Jest to funkcja ASP.NET, wprowadzona w artykule:</span><span class="sxs-lookup"><span data-stu-id="8d23a-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="8d23a-111">`Maps` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="8d23a-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8d23a-112">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="8d23a-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8d23a-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="8d23a-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="8d23a-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="8d23a-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="8d23a-115">W tym samouczku współdziała również z 3 programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="8d23a-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="8d23a-116">Strony sieci Web, map można wyświetlić na stronie przy użyciu `Maps` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="8d23a-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="8d23a-117">Można wygenerować na podstawie adresu lub zestaw długości i szerokości geograficznej współrzędnych mapy.</span><span class="sxs-lookup"><span data-stu-id="8d23a-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="8d23a-118">`Maps` Klasy umożliwia wywołują aparaty popularnych mapy, w tym Bing, MapQuest, Google i Yahoo.</span><span class="sxs-lookup"><span data-stu-id="8d23a-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="8d23a-119">Procedura dodawania mapowania do strony są takie same niezależnie od tego, który aparatów mapy wywołania.</span><span class="sxs-lookup"><span data-stu-id="8d23a-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="8d23a-120">Po prostu Dodaj odwołanie pliku JavaScript, która sprawia, że dostępne metody, aby wyświetlić mapę, a następnie wywołać metody `Maps` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="8d23a-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="8d23a-121">Wybierz usługę mapy na podstawie którego `Maps` używanej metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="8d23a-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="8d23a-122">Można użyć dowolnego z nich:</span><span class="sxs-lookup"><span data-stu-id="8d23a-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="8d23a-123">Instalowanie elementy, które są potrzebne</span><span class="sxs-lookup"><span data-stu-id="8d23a-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="8d23a-124">Do wyświetlenia mapy, potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="8d23a-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="8d23a-125">`Maps` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="8d23a-125">The `Maps` helper.</span></span> <span data-ttu-id="8d23a-126">Tego pomocnika jest w wersji 2 bibliotekę pomocników platformy ASP.NET sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d23a-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="8d23a-127">Jeśli nie został jeszcze dodany biblioteki, można zainstalować go w witrynie jako pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="8d23a-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="8d23a-128">Aby uzyskać więcej informacji, zobacz [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="8d23a-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="8d23a-129">(W galerii, wyszukaj `microsoft-web-helpers` pakietu.)</span><span class="sxs-lookup"><span data-stu-id="8d23a-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="8d23a-130">Biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="8d23a-130">The jQuery library.</span></span> <span data-ttu-id="8d23a-131">Kilka szablonów witryn program WebMatrix zawiera biblioteki jQuery w ich *skryptu* folderów.</span><span class="sxs-lookup"><span data-stu-id="8d23a-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="8d23a-132">Jeśli nie masz tych bibliotek, możesz pobrać najnowsze biblioteki jQuery bezpośrednio z [jQuery.org](http://jQuery.org) lokacji.</span><span class="sxs-lookup"><span data-stu-id="8d23a-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="8d23a-133">Lub utworzyć nową lokację przy użyciu szablonu (na przykład **witryny początkowej** szablonu), a następnie skopiuj pliki jQuery z tej lokacji do bieżącej witryny.</span><span class="sxs-lookup"><span data-stu-id="8d23a-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="8d23a-134">Ponadto jeśli chcesz użyć mapy Bing, najpierw musisz utworzyć konto (bezpłatnie) i uzyskiwanie klucza.</span><span class="sxs-lookup"><span data-stu-id="8d23a-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="8d23a-135">Aby uzyskać klucz, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="8d23a-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="8d23a-136">Tworzenie konta na [konta dewelopera mapy Bing](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d23a-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="8d23a-137">Musi mieć konto Microsoft (Windows Live ID) również.</span><span class="sxs-lookup"><span data-stu-id="8d23a-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="8d23a-138">Można określić, czy chcesz użyć klucza dla **oceny i testowania**.</span><span class="sxs-lookup"><span data-stu-id="8d23a-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="8d23a-139">Jeśli testujesz funkcja mapowania na własnym komputerze za pomocą programu WebMatrix i usług IIS Express, przejdź **lokacji** obszaru roboczego i zanotuj adres URL witryny sieci (na przykład `http://localhost:50408`, mimo że numer portu, prawdopodobnie będą inne).</span><span class="sxs-lookup"><span data-stu-id="8d23a-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="8d23a-140">Możesz użyć tej funkcji *localhost* adres witryny podczas rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="8d23a-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="8d23a-141">Po zarejestrowaniu dla konta, przejdź do Centrum konta map Bing i kliknij przycisk **Create lub widok kluczy**:</span><span class="sxs-lookup"><span data-stu-id="8d23a-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![Mapowanie-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="8d23a-143">Zarejestruj klucz, który tworzy Bing.</span><span class="sxs-lookup"><span data-stu-id="8d23a-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="8d23a-144">Tworzenie mapy na podstawie adresu (przy użyciu usługi Google)</span><span class="sxs-lookup"><span data-stu-id="8d23a-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="8d23a-145">Poniższy przykład przedstawia sposób tworzenia strony, który renderuje mapy na podstawie adresu.</span><span class="sxs-lookup"><span data-stu-id="8d23a-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="8d23a-146">Ten przykład przedstawia sposób użycia map programu Google.</span><span class="sxs-lookup"><span data-stu-id="8d23a-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="8d23a-147">Utwórz plik o nazwie *MapAddress.cshtml* w katalogu głównym witryny.</span><span class="sxs-lookup"><span data-stu-id="8d23a-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="8d23a-148">Ta strona wygeneruje mapy na podstawie adresu, który przekazywania do niej.</span><span class="sxs-lookup"><span data-stu-id="8d23a-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="8d23a-149">Skopiuj następujący kod do pliku, zastępując istniejącej zawartości.</span><span class="sxs-lookup"><span data-stu-id="8d23a-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="8d23a-150">Zwróć uwagę, następujące funkcje strony:</span><span class="sxs-lookup"><span data-stu-id="8d23a-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="8d23a-151">`<script>` Element `<head>` elementu.</span><span class="sxs-lookup"><span data-stu-id="8d23a-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="8d23a-152">W tym przykładzie `<script>` odwołania do elementu *jquery 1.6.4.min.js* pliku, który jest zminimalizowany (skompresowane) wersji biblioteki jQuery, wersja 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="8d23a-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="8d23a-153">Należy pamiętać, że odwołanie zakłada się, że *js* plik znajduje się w *skryptów* folder witryny.</span><span class="sxs-lookup"><span data-stu-id="8d23a-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="8d23a-154">Jeśli używasz innej wersji biblioteki jQuery po prostu upewnij się, że wskazuje tej wersji poprawnie.</span><span class="sxs-lookup"><span data-stu-id="8d23a-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="8d23a-155">Wywołanie `@Maps.GetGoogleHtml` w treści strony.</span><span class="sxs-lookup"><span data-stu-id="8d23a-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="8d23a-156">Aby mapować adres, należy podać ciąg adresu.</span><span class="sxs-lookup"><span data-stu-id="8d23a-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="8d23a-157">Metody silników mapy działa w podobny sposób (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="8d23a-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="8d23a-158">Uruchom strony, a następnie wprowadź adres.</span><span class="sxs-lookup"><span data-stu-id="8d23a-158">Run the page and enter an address.</span></span> <span data-ttu-id="8d23a-159">Na stronie są wyświetlane mapy, oparte na map programu Google, pokazujący określonej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="8d23a-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![Mapowanie-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="8d23a-161">Tworzenie mapy oparte na współrzędne geograficzne współrzędne (przy użyciu usługi Bing)</span><span class="sxs-lookup"><span data-stu-id="8d23a-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="8d23a-162">W tym przykładzie pokazano, jak utworzyć na podstawie współrzędnych mapy.</span><span class="sxs-lookup"><span data-stu-id="8d23a-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="8d23a-163">Ten przykład przedstawia sposób użycia mapy Bing i sposobu uwzględniania klucza usługi Bing.</span><span class="sxs-lookup"><span data-stu-id="8d23a-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="8d23a-164">(Można utworzyć mapę na podstawie współrzędnych przy użyciu innych aparatów mapy także bez korzystania z usługi Bing klucza).</span><span class="sxs-lookup"><span data-stu-id="8d23a-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="8d23a-165">Utwórz plik o nazwie *MapCoordinates.cshtml* w folderze głównym lokacji i Zastąp istniejącą zawartość z następującymi kodu i znaczników:</span><span class="sxs-lookup"><span data-stu-id="8d23a-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="8d23a-166">Zastąp `your-key-here` z kluczem usługi mapy Bing, wcześniej wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="8d23a-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="8d23a-167">Uruchom *MapCoordinates.cshtml* wprowadź współrzędne geograficzne, a następnie kliknij pozycję **mapy go!**</span><span class="sxs-lookup"><span data-stu-id="8d23a-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="8d23a-168">button.</span><span class="sxs-lookup"><span data-stu-id="8d23a-168">button.</span></span> <span data-ttu-id="8d23a-169">(Jeśli nie znasz wszystkie współrzędne, wykonaj następujące czynności.</span><span class="sxs-lookup"><span data-stu-id="8d23a-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="8d23a-170">Jest to lokalizacja w firmy Microsoft Redmond).</span><span class="sxs-lookup"><span data-stu-id="8d23a-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="8d23a-171">Szerokość: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="8d23a-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="8d23a-172">Longitude: -122.158317565918</span><span class="sxs-lookup"><span data-stu-id="8d23a-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="8d23a-173">Ta strona jest wyświetlana, przy użyciu współrzędnych, określone przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8d23a-173">The page is displayed using the coordinates that you specified.</span></span>

     ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="8d23a-175">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8d23a-175">Additional Resources</span></span>


[<span data-ttu-id="8d23a-176">Dokumentacja interfejsu API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="8d23a-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
