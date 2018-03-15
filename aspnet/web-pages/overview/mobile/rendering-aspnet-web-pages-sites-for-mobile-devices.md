---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "Renderowanie sieci Web ASP.NET stron witryny (Razor) dla urządzeń przenośnych | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym artykule opisano sposób tworzenia stron w witrynie stron sieci Web platformy ASP.NET (Razor), który będzie renderowany odpowiednio na urządzeniach przenośnych. Dowiesz się: jak możesz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="62d18-104">Renderowanie witryny sieci Web ASP.NET (Razor) stron dla urządzeń przenośnych</span><span class="sxs-lookup"><span data-stu-id="62d18-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="62d18-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="62d18-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="62d18-106">W tym artykule opisano sposób tworzenia stron w witrynie stron sieci Web platformy ASP.NET (Razor), który będzie renderowany odpowiednio na urządzeniach przenośnych.</span><span class="sxs-lookup"><span data-stu-id="62d18-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="62d18-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="62d18-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="62d18-108">Jak używać konwencji nazewnictwa, aby określić, że strona jest przeznaczony specjalnie dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="62d18-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="62d18-109">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="62d18-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="62d18-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="62d18-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="62d18-111">W tym samouczku współdziała również z programu ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="62d18-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="62d18-112">Strony ASP.NET Web Pages umożliwia tworzenie niestandardowych służy do renderowania zawartości na telefon komórkowy lub innych urządzeń.</span><span class="sxs-lookup"><span data-stu-id="62d18-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="62d18-113">Najprostszym sposobem tworzenia urządzenia strony w witrynie stron ASP.NET Web Pages jest przy użyciu wzorca nazewnictwa plików następująco: *FileName. **Mobile**.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62d18-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="62d18-114">Można utworzyć dwie wersje strony (na przykład jedną o nazwie *MyFile.cshtml* i jedną o nazwie *MyFile.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="62d18-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="62d18-115">W czasie, gdy urządzenie przenośne żąda wykonywania *MyFile.cshtml*, platforma ASP.NET renderuje zawartość z *MyFile.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62d18-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="62d18-116">W przeciwnym razie *MyFile.cshtml* jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="62d18-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="62d18-117">Poniższy przykład pokazuje, jak umożliwiające renderowanie przenośnych przez dodanie strony zawartości dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="62d18-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="62d18-118">*Page1.cshtml* zawiera zawartości oraz pasek boczny nawigacji.</span><span class="sxs-lookup"><span data-stu-id="62d18-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="62d18-119">*Page1.Mobile.cshtml* zawiera tę samą zawartość, ale pominięto paska bocznego.</span><span class="sxs-lookup"><span data-stu-id="62d18-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="62d18-120">W lokacji stron ASP.NET Web Pages, Utwórz plik o nazwie *Page1.cshtml* i Zastąp następujący kod znaczników bieżącej zawartości.</span><span class="sxs-lookup"><span data-stu-id="62d18-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="62d18-121">Utwórz plik o nazwie *Page1.Mobile.cshtml* i zastąpić istniejącą zawartość następujących znaczników.</span><span class="sxs-lookup"><span data-stu-id="62d18-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="62d18-122">Zwróć uwagę, że wersja urządzenia przenośne strony pomija sekcję nawigacji dla lepszej renderowania na ekranie mniejsze.</span><span class="sxs-lookup"><span data-stu-id="62d18-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="62d18-123">Uruchom przeglądarkę dla komputerów, a następnie przejdź do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62d18-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="62d18-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="62d18-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="62d18-125">Uruchom przeglądarkę dla telefonów (lub w emulatorze urządzenia przenośnego) i przejdź do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62d18-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="62d18-126">(Zwróć uwagę, że nie ma *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="62d18-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="62d18-127">w ramach adresu URL). Nawet jeśli żądanie dotyczy *Page1.cshtml*, platforma ASP.NET renderuje *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62d18-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="62d18-129">Aby przetestować stron dla urządzeń przenośnych, można użyć symulator urządzeń przenośnych, uruchomioną na komputerze stacjonarnym.</span><span class="sxs-lookup"><span data-stu-id="62d18-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="62d18-130">To narzędzie umożliwia testowanie stron sieci web, jak wyglądają na urządzeniach przenośnych (oznacza to zwykle z dużo mniejszym wyświetlenie obszaru).</span><span class="sxs-lookup"><span data-stu-id="62d18-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="62d18-131">Przykład symulatora [przełącznik agenta użytkownika dodatku](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) programie Mozilla Firefox, co umożliwia emulować różnych przeglądarkach dla urządzeń przenośnych z wersji pulpitu Firefox.</span><span class="sxs-lookup"><span data-stu-id="62d18-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="62d18-132">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="62d18-132">Additional Resources</span></span>


<span data-ttu-id="62d18-133">[Emulator Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="62d18-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
