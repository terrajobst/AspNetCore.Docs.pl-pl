---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderowania ASP.NET Web Pages witryny (Razor) dla urządzeń przenośnych | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie renderowana odpowiednio na urządzeniach przenośnych. Dowiesz się: jak dla Ciebie...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dd06a54d396bd85eeef7c004ee115828d541a2c1
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020940"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="6e27b-104">Renderowanie witrynach ASP.NET Web Pages (Razor) dla urządzeń przenośnych</span><span class="sxs-lookup"><span data-stu-id="6e27b-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="6e27b-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6e27b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6e27b-106">W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie renderowana odpowiednio na urządzeniach przenośnych.</span><span class="sxs-lookup"><span data-stu-id="6e27b-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="6e27b-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="6e27b-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="6e27b-108">Jak używać konwencji nazewnictwa, aby określić, że strona jest zaprojektowany specjalnie dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="6e27b-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6e27b-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="6e27b-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6e27b-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="6e27b-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="6e27b-111">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="6e27b-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="6e27b-112">Strony ASP.NET Web Pages umożliwia tworzenie niestandardowych służy do renderowania zawartości na telefon komórkowy lub innych urządzeń.</span><span class="sxs-lookup"><span data-stu-id="6e27b-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="6e27b-113">Najprostszym sposobem utworzenia strony specyficznych dla urządzenia w witrynie ASP.NET Web Pages jest przy użyciu wzorca nazewnictwa plików następująco: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="6e27b-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="6e27b-114">Możesz utworzyć dwie wersje strony (na przykład jeden o nazwie <em>MyFile.cshtml</em> i jedną o nazwie <em>MyFile.Mobile.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="6e27b-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="6e27b-115">W czasie, gdy urządzenie przenośne żąda wykonywania <em>MyFile.cshtml</em>, platforma ASP.NET renderuje zawartość z <em>MyFile.Mobile.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="6e27b-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="6e27b-116">W przeciwnym razie <em>MyFile.cshtml</em> jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="6e27b-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="6e27b-117">Poniższy przykład pokazuje, jak umożliwiające renderowanie mobilnych, dodając strony zawartości dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="6e27b-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="6e27b-118">*Page1.cshtml* zawiera zawartość oraz pasek boczny nawigacji.</span><span class="sxs-lookup"><span data-stu-id="6e27b-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="6e27b-119">*Page1.Mobile.cshtml* zawiera tę samą zawartość, ale pomija pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="6e27b-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="6e27b-120">W witrynie ASP.NET Web Pages, Utwórz plik o nazwie *Page1.cshtml* i Zastąp zawartość bieżącego następujące znaczniki.</span><span class="sxs-lookup"><span data-stu-id="6e27b-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="6e27b-121">Utwórz plik o nazwie *Page1.Mobile.cshtml* i zastąpić istniejącą zawartość następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="6e27b-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="6e27b-122">Należy zauważyć, że mobilnej wersji strony pomija lepsze renderowanie na mniejsze ekranu można znaleźć w sekcji nawigacji.</span><span class="sxs-lookup"><span data-stu-id="6e27b-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="6e27b-123">Uruchom przeglądarkę dla komputerów, a następnie przejdź do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6e27b-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="6e27b-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6e27b-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="6e27b-125">Uruchom w przeglądarce dla urządzeń przenośnych (lub w emulatorze urządzenia przenośnego), a następnie przejdź do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6e27b-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="6e27b-126">(Zwróć uwagę, że nie zostanie uwzględniony *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="6e27b-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="6e27b-127">jako część adresu URL). Nawet jeśli żądanie dotyczy *Page1.cshtml*, platforma ASP.NET renderuje *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6e27b-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="6e27b-129">Aby przetestować stron dla urządzeń przenośnych, można użyć symulatora urządzenia przenośnego, uruchomionym na komputerze stacjonarnym.</span><span class="sxs-lookup"><span data-stu-id="6e27b-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="6e27b-130">To narzędzie umożliwia testowanie stron sieci web tak, jak będą wyglądały na urządzeniach przenośnych (oznacza to, zwykle za pomocą znacznie mniejszy powoduje wyświetlenie obszaru).</span><span class="sxs-lookup"><span data-stu-id="6e27b-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="6e27b-131">Przykład symulatora [przełącznik agenta użytkownika dodatku](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) dla Mozilla Firefox, który umożliwia emulowanie różnych przeglądarek dla urządzeń przenośnych z wersji klasycznej programu Firefox.</span><span class="sxs-lookup"><span data-stu-id="6e27b-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="6e27b-132">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6e27b-132">Additional Resources</span></span>


<span data-ttu-id="6e27b-133">[Emulator Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="6e27b-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
