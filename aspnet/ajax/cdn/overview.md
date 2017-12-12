---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Dokumentacja firmy Microsoft
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="4f97f-102">Microsoft Ajax sieci dostarczania zawartości</span><span class="sxs-lookup"><span data-stu-id="4f97f-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="4f97f-103">Uwaga: Microsoft Ajax CDN ma umowy dotyczącej poziomu usług niż przy użyciu usługi Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="4f97f-104">Spis treści</span><span class="sxs-lookup"><span data-stu-id="4f97f-104">Table of Contents</span></span>

<span data-ttu-id="4f97f-105">**[AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="4f97f-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="4f97f-106">**[Obsługa .vsdoc programu Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="4f97f-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="4f97f-107">**[Za pomocą kodu ASP.NET Ajax z sieci CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="4f97f-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="4f97f-108">**[Przy użyciu jQuery z sieci CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="4f97f-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="4f97f-109">**[Przy użyciu interfejsu użytkownika z sieci CDN jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="4f97f-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="4f97f-110">**[Pliki innych firm w sieci CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="4f97f-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="4f97f-111">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="4f97f-112">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="4f97f-113">Dostępne wersje interfejsu użytkownika w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="4f97f-114">Dostępne wersje weryfikacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="4f97f-115">jQuery Mobile wersjach w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="4f97f-116">Dostępne wersje szablonów w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="4f97f-117">Dostępne wersje cyklu w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="4f97f-118">Dostępne wersje DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="4f97f-119">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="4f97f-120">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="4f97f-121">Wersje odcinania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="4f97f-122">Globalize wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="4f97f-123">Odpowiadać wersji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="4f97f-124">Wersje bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="4f97f-125">Wersje bootstrap TouchCarousel w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="4f97f-126">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="4f97f-127">Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="4f97f-128">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="4f97f-129">Wersje biblioteki SignalR platformy ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="4f97f-130">Microsoft Ajax sieci dostarczania zawartości (CDN) obsługuje bibliotek JavaScript popularnych innych firm, np. jQuery i umożliwia łatwe dodanie ich do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f97f-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="4f97f-131">Na przykład możesz rozpocząć używać jQuery, który znajduje się w tej sieci CDN po prostu dodając &lt;skryptu&gt; tag do strony, która wskazuje ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="4f97f-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="4f97f-132">Dzięki wykorzystaniu sieci CDN może znacznie poprawić wydajność aplikacji Ajax.</span><span class="sxs-lookup"><span data-stu-id="4f97f-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="4f97f-133">Zawartość CDN są buforowane na serwerów na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="4f97f-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="4f97f-134">Ponadto CDN umożliwia ponowne użycie innej buforowane pliki JavaScript dla witryn sieci web, które znajdują się w różnych domenach w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="4f97f-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="4f97f-135">CDN obsługuje SSL (HTTPS), w razie potrzeby do obsługi strony sieci web przy użyciu protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="4f97f-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="4f97f-136">Usługa CDN obsługuje następujące biblioteki skryptu innych firm, które zostały przekazane i są, licencjonowane przez właścicieli tych biblioteki:</span><span class="sxs-lookup"><span data-stu-id="4f97f-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="4f97f-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="4f97f-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="4f97f-138">jQuery interfejsu użytkownika (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="4f97f-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="4f97f-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="4f97f-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="4f97f-140">jQuery sprawdzania poprawności (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="4f97f-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="4f97f-141">jQuery cyklu (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="4f97f-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="4f97f-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="4f97f-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="4f97f-143">Usługa Microsoft Ajax CDN zawiera również następujące biblioteki, które zostały przekazane przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="4f97f-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="4f97f-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="4f97f-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="4f97f-145">Pliki języka JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4f97f-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="4f97f-146">Pliki ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="4f97f-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="4f97f-147">Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="4f97f-148">Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="4f97f-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="4f97f-149">Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="4f97f-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="4f97f-150">Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="4f97f-151">Jeśli chcesz przesłać biblioteki JavaScript i biblioteki jest jednym z góry bibliotek JavaScript (wymienione w http://trends.builtwith.com) lub rozszerzenia/wtyczek do tych bibliotek, które są () popularnych; lub b przydatne do użycia na platformie ASP.NET, a następnie skontaktuj się z AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="4f97f-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="4f97f-152">AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="4f97f-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="4f97f-153">CDN używany do używania nazwy domeny microsoft.com i została zmieniona do używania nazwy domeny aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="4f97f-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="4f97f-154">Ta zmiana została wprowadzona w celu zwiększenia wydajności, ponieważ podczas przeglądarką domenie microsoft.com, do których odwołuje się on będzie wysyłać żadnych plików cookie z tej domeny przez sieć z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="4f97f-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="4f97f-155">Zmieniając nazwę domeny inne niż microsoft.com można zwiększyć wydajność przez tyle 25%.</span><span class="sxs-lookup"><span data-stu-id="4f97f-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="4f97f-156">Uwaga ajax.microsoft.com będą nadal działać, ale ajax.aspnetcdn.com jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="4f97f-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="4f97f-157">Stary Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="4f97f-158">Nowy Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="4f97f-159">Obsługa .vsdoc programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f97f-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="4f97f-160">Do korzystania z plików .vsdoc prawidłowo z programu Visual Studio 2008, należy się upewnić, że masz VS 2008 z dodatkiem SP1 należy zainstalować i zainstalowana poprawka vsdoc plików.</span><span class="sxs-lookup"><span data-stu-id="4f97f-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="4f97f-161">Możesz uzyskać je w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="4f97f-161">You can get these from here:</span></span>

- [<span data-ttu-id="4f97f-162">Pobierz program Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="4f97f-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "pobierania programu Visual Studio 2008 z dodatkiem SP1")
- [<span data-ttu-id="4f97f-163">Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="4f97f-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "pobrać poprawkę .vsdoc dla programu Visual Studio 2008 z dodatkiem SP1")

<span data-ttu-id="4f97f-164">Program Visual Studio 2010 obsługuje pliki .vsdoc bez żadnych dodatkowych poprawek.</span><span class="sxs-lookup"><span data-stu-id="4f97f-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="4f97f-165">Za pomocą kodu ASP.NET Ajax z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="4f97f-166">Podczas korzystania z programu ASP.NET 4, można przekierowywać wszystkie żądania ASP.NET framework skryptów do sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="4f97f-167">Pobieranie skryptów z sieci CDN zamiast serwera sieci web lokalnego znacznie może poprawić wydajność publicznych witryn sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f97f-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="4f97f-168">Za pomocą właściwości ScriptManager EnableCDN Przekieruj żądania skryptu framework ASP.NET do programu Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="4f97f-169">Przy użyciu jQuery z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="4f97f-170">Za pomocą skryptów jQuery hostowanych w sieci CDN w aplikacji sieci Web, dodając następujący element skryptu do strony:</span><span class="sxs-lookup"><span data-stu-id="4f97f-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="4f97f-171">Usługa CDN zawiera również wersję zminimalizowany skryptu jQuery, którą można uzyskać za pomocą następującego elementu:</span><span class="sxs-lookup"><span data-stu-id="4f97f-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="4f97f-172">Aby zezwolić na stronę do powrotu do ładowania jQuery z ścieżkę lokalną na własne witryny sieci Web, jeśli sieć CDN jest niedostępny, Dodaj następujący element natychmiast po elemencie odwołujące się do sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="4f97f-173">Następujące przykładowe strony korzysta z wersji CDN biblioteki jQuery (z powrotu do lokalnej kopii) do wyświetlenia zawartości elementu div, gdy przycisk zostanie kliknięty.</span><span class="sxs-lookup"><span data-stu-id="4f97f-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="4f97f-174">Można dowiedzieć się więcej o jQuery i pobrać kopię lokalną jQuery odwiedzając [jQuery](http://jquery.com/) witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f97f-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="4f97f-175">Przy użyciu interfejsu użytkownika z sieci CDN jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="4f97f-176">Usługa CDN obsługuje również biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="4f97f-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="4f97f-177">Biblioteka interfejsu użytkownika jQuery zawiera bogaty zestaw elementów widget i efekty, których można używać w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f97f-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="4f97f-178">Na przykład następująca strona przedstawiono, jak jQuery selektora daty interfejsu użytkownika w kontekście aplikacji formularzy sieci Web programu ASP.NET można użyć do wyświetlenia wyskakującego kalendarza:</span><span class="sxs-lookup"><span data-stu-id="4f97f-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="4f97f-179">Gdy fokus zostanie przeniesiony do pola tekstowego przy użyciu klawiatury, zostanie wyświetlony kalendarz:</span><span class="sxs-lookup"><span data-stu-id="4f97f-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Utworzone za pomocą selektora daty kalendarza podręcznego](overview/_static/image1.png)

<span data-ttu-id="4f97f-181">Zwróć uwagę, że musi zawierać trzy pliki z sieci CDN w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="4f97f-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="4f97f-182">Biblioteki jQuery &mdash; biblioteki interfejsu użytkownika jQuery jest zależna od biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="4f97f-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="4f97f-183">Należy dodać biblioteki jQuery do strony przed dodaniem biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="4f97f-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="4f97f-184">Biblioteka interfejsu użytkownika jQuery &mdash; biblioteki interfejsu użytkownika jQuery zawiera elementy widget, takie jak używane na stronie powyżej widżetu selektora daty i efektów interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="4f97f-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="4f97f-185">Motyw interfejsu użytkownika jQuery &mdash; jQuery interfejsu użytkownika obsługuje różne kompozycje.</span><span class="sxs-lookup"><span data-stu-id="4f97f-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="4f97f-186">Strona sieci zawiera łącze do pliku CSS do zaimportowania Redmond motywu.</span><span class="sxs-lookup"><span data-stu-id="4f97f-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="4f97f-187">Wszystkie kompozycje interfejsu użytkownika jQuery standardowe znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="4f97f-188">[Odwiedź stronę tego](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN") do wyświetlania miniatur dla każdego motywu.</span><span class="sxs-lookup"><span data-stu-id="4f97f-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="4f97f-189">Aby dowiedzieć się więcej na temat biblioteki interfejsu użytkownika jQuery, można znaleźć w oficjalnym [witryny sieci Web interfejsu użytkownika jQuery](http://jQueryUI.com "witryny sieci Web interfejsu użytkownika jQuery").</span><span class="sxs-lookup"><span data-stu-id="4f97f-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="4f97f-190">Pliki innych firm w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="4f97f-191">Usługa CDN obsługuje niektóre najbardziej popularnych bibliotek JavaScript innych firm.</span><span class="sxs-lookup"><span data-stu-id="4f97f-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="4f97f-192">Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="4f97f-193">Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="4f97f-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="4f97f-194">Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="4f97f-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="4f97f-195">Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="4f97f-196">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="4f97f-197">Następujące wersje jQuery znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="4f97f-198">Wersja jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-198">jQuery version 3.2.1</span></span>
- <span data-ttu-id="4f97f-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="4f97f-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="4f97f-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="4f97f-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="4f97f-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="4f97f-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="4f97f-205">Wersja jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-205">jQuery version 3.2.0</span></span>

- <span data-ttu-id="4f97f-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="4f97f-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="4f97f-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="4f97f-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="4f97f-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="4f97f-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="4f97f-212">Wersja jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-212">jQuery version 3.1.1</span></span>

- <span data-ttu-id="4f97f-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="4f97f-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="4f97f-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="4f97f-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="4f97f-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="4f97f-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="4f97f-219">Wersja jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-219">jQuery version 3.1.0</span></span>

- <span data-ttu-id="4f97f-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="4f97f-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="4f97f-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="4f97f-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="4f97f-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="4f97f-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="4f97f-226">Wersja jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-226">jQuery version 3.0.0</span></span>

- <span data-ttu-id="4f97f-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="4f97f-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="4f97f-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="4f97f-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="4f97f-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="4f97f-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="4f97f-233">Wersja jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-233">jQuery version 2.2.4</span></span>

- <span data-ttu-id="4f97f-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="4f97f-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="4f97f-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="4f97f-237">Wersja jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-237">jQuery version 2.2.3</span></span>

- <span data-ttu-id="4f97f-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="4f97f-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="4f97f-240">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="4f97f-241">Wersja jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-241">jQuery version 2.2.2</span></span>

- <span data-ttu-id="4f97f-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="4f97f-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="4f97f-244">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="4f97f-245">Wersja jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-245">jQuery version 2.2.1</span></span>

- <span data-ttu-id="4f97f-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="4f97f-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="4f97f-248">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="4f97f-249">Wersja jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-249">jQuery version 2.2.0</span></span>

- <span data-ttu-id="4f97f-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="4f97f-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="4f97f-252">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="4f97f-253">Wersja jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-253">jQuery version 2.1.4</span></span>

- <span data-ttu-id="4f97f-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="4f97f-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="4f97f-256">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="4f97f-257">Wersja jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-257">jQuery version 2.1.3</span></span>

- <span data-ttu-id="4f97f-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="4f97f-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="4f97f-260">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="4f97f-261">Wersja jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-261">jQuery version 2.1.2</span></span>

- <span data-ttu-id="4f97f-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="4f97f-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="4f97f-264">Wersja jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-264">jQuery version 2.1.1</span></span>

- <span data-ttu-id="4f97f-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="4f97f-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="4f97f-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="4f97f-268">Wersja jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-268">jQuery version 2.1.0</span></span>

- <span data-ttu-id="4f97f-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="4f97f-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="4f97f-271">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="4f97f-273">Wersja jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-273">jQuery version 2.0.3</span></span>

- <span data-ttu-id="4f97f-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="4f97f-275">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="4f97f-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="4f97f-278">Wersja jQuery pkt 2.0.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-278">jQuery version 2.0.2</span></span>

- <span data-ttu-id="4f97f-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="4f97f-280">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="4f97f-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="4f97f-283">Wersja jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-283">jQuery version 2.0.1</span></span>

- <span data-ttu-id="4f97f-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="4f97f-285">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="4f97f-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="4f97f-288">jQuery wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-288">jQuery version 2.0.0</span></span>

- <span data-ttu-id="4f97f-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="4f97f-290">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="4f97f-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="4f97f-293">Wersja jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-293">jQuery version 1.12.4</span></span>

- <span data-ttu-id="4f97f-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="4f97f-295">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="4f97f-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="4f97f-297">Wersja jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-297">jQuery version 1.12.3</span></span>

- <span data-ttu-id="4f97f-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="4f97f-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="4f97f-300">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="4f97f-301">Wersja jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-301">jQuery version 1.12.2</span></span>

- <span data-ttu-id="4f97f-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="4f97f-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="4f97f-304">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="4f97f-305">Wersja jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-305">jQuery version 1.12.1</span></span>

- <span data-ttu-id="4f97f-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="4f97f-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="4f97f-308">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="4f97f-309">Wersja jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-309">jQuery version 1.12.0</span></span>

- <span data-ttu-id="4f97f-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="4f97f-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="4f97f-312">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="4f97f-313">Wersja jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-313">jQuery version 1.11.3</span></span>

- <span data-ttu-id="4f97f-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="4f97f-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="4f97f-316">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="4f97f-317">Wersja jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-317">jQuery version 1.11.2</span></span>

- <span data-ttu-id="4f97f-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="4f97f-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="4f97f-320">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="4f97f-321">Wersja jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-321">jQuery version 1.11.1</span></span>

- <span data-ttu-id="4f97f-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="4f97f-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="4f97f-324">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="4f97f-325">Wersja jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-325">jQuery version 1.11.0</span></span>

- <span data-ttu-id="4f97f-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="4f97f-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="4f97f-328">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="4f97f-330">Wersja jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-330">jQuery version 1.10.2</span></span>

- <span data-ttu-id="4f97f-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="4f97f-332">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="4f97f-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="4f97f-335">Wersja jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-335">jQuery version 1.10.1</span></span>

- <span data-ttu-id="4f97f-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="4f97f-337">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="4f97f-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="4f97f-340">Wersja jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-340">jQuery version 1.10.0</span></span>

- <span data-ttu-id="4f97f-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="4f97f-342">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="4f97f-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="4f97f-345">Wersja jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-345">jQuery version 1.9.1</span></span>

- <span data-ttu-id="4f97f-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="4f97f-347">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="4f97f-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="4f97f-350">Wersja jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-350">jQuery version 1.9.0</span></span>

- <span data-ttu-id="4f97f-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="4f97f-352">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="4f97f-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="4f97f-355">Wersja jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-355">jQuery version 1.8.3</span></span>

- <span data-ttu-id="4f97f-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="4f97f-357">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="4f97f-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="4f97f-359">Wersja jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-359">jQuery version 1.8.2</span></span>

- <span data-ttu-id="4f97f-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="4f97f-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="4f97f-362">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="4f97f-363">Wersja jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-363">jQuery version 1.8.1</span></span>

- <span data-ttu-id="4f97f-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="4f97f-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="4f97f-366">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="4f97f-367">Wersja jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-367">jQuery version 1.8.0</span></span>

- <span data-ttu-id="4f97f-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="4f97f-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="4f97f-370">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="4f97f-371">Wersja jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-371">jQuery version 1.7.2</span></span>

- <span data-ttu-id="4f97f-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="4f97f-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="4f97f-374">Wersja jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-374">jQuery version 1.7.1</span></span>

- <span data-ttu-id="4f97f-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="4f97f-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="4f97f-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="4f97f-378">Wersja jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="4f97f-378">jQuery version 1.7</span></span>

- <span data-ttu-id="4f97f-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="4f97f-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="4f97f-381">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="4f97f-382">Wersja jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-382">jQuery version 1.6.4</span></span>

- <span data-ttu-id="4f97f-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="4f97f-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="4f97f-385">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="4f97f-386">Wersja jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-386">jQuery version 1.6.3</span></span>

- <span data-ttu-id="4f97f-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="4f97f-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="4f97f-389">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="4f97f-390">jQuery wersji 1.6.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-390">jQuery version 1.6.2</span></span>

- <span data-ttu-id="4f97f-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="4f97f-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="4f97f-393">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="4f97f-394">jQuery wersji 1.6.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-394">jQuery version 1.6.1</span></span>

- <span data-ttu-id="4f97f-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="4f97f-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="4f97f-397">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="4f97f-398">jQuery w wersji 1.6</span><span class="sxs-lookup"><span data-stu-id="4f97f-398">jQuery version 1.6</span></span>

- <span data-ttu-id="4f97f-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="4f97f-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="4f97f-401">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="4f97f-402">Wersja jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-402">jQuery version 1.5.2</span></span>

- <span data-ttu-id="4f97f-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="4f97f-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="4f97f-405">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="4f97f-406">Wersja jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-406">jQuery version 1.5.1</span></span>

- <span data-ttu-id="4f97f-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="4f97f-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="4f97f-409">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="4f97f-410">jQuery w wersji 1.5</span><span class="sxs-lookup"><span data-stu-id="4f97f-410">jQuery version 1.5</span></span>

- <span data-ttu-id="4f97f-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="4f97f-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="4f97f-413">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="4f97f-414">Wersja jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-414">jQuery version 1.4.4</span></span>

- <span data-ttu-id="4f97f-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="4f97f-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="4f97f-417">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="4f97f-418">Wersja jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-418">jQuery version 1.4.3</span></span>

- <span data-ttu-id="4f97f-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="4f97f-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="4f97f-421">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="4f97f-422">Wersja jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-422">jQuery version 1.4.2</span></span>

- <span data-ttu-id="4f97f-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="4f97f-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="4f97f-425">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="4f97f-426">Wersja jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-426">jQuery version 1.4.1</span></span>

- <span data-ttu-id="4f97f-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="4f97f-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="4f97f-429">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="4f97f-430">jQuery w wersji 1.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-430">jQuery version 1.4</span></span>

- <span data-ttu-id="4f97f-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="4f97f-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="4f97f-433">Wersja jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-433">jQuery version 1.3.2</span></span>

- <span data-ttu-id="4f97f-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="4f97f-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="4f97f-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="4f97f-437">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="4f97f-438">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-438">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="4f97f-439">Następujące wersje jQuery migracji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-439">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="4f97f-440">jQuery migracji wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-440">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="4f97f-441">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="4f97f-442">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="4f97f-443">jQuery migracji wersji 1.2.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-443">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="4f97f-444">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="4f97f-445">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="4f97f-446">jQuery migracji wersji 1.2.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-446">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="4f97f-447">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="4f97f-448">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="4f97f-449">jQuery migracji wersji 1.1.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-449">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="4f97f-450">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="4f97f-451">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="4f97f-452">jQuery migracji wersji 1.1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-452">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="4f97f-453">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="4f97f-454">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="4f97f-455">jQuery migracji w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-455">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="4f97f-456">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="4f97f-457">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="4f97f-458">Dostępne wersje interfejsu użytkownika w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-458">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="4f97f-459">Następujące wersje biblioteki interfejsu użytkownika jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-459">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="4f97f-460">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="4f97f-460">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f97f-461">1.12.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-461">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery 1.12.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-462">1.12.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-462">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery 1.12.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-463">1.11.4 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-463">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery 1.11.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-464">1.11.3 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-464">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery 1.11.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-465">1.11.2 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-465">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery 1.11.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-466">1.11.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-466">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery 1.11.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-467">1.11.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-467">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery 1.11.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-468">1.10.4 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-468">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery 1.10.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-469">1.10.3 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-469">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery 1.10.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-470">jQuery 1.10.2 interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="4f97f-470">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-471">1.10.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-471">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery 1.10.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-472">1.10.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-472">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery 1.10.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-473">1.9.2 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-473">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery 1.9.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-474">1.9.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-474">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery 1.9.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-475">1.9.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-475">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery 1.9.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-476">1.8.24 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-476">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery 1.8.24 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-477">1.8.23 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-477">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery 1.8.23 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-478">1.8.22 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-478">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery 1.8.22 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-479">1.8.21 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-479">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery 1.8.21 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-480">1.8.20 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-480">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery 1.8.20 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-481">1.8.19 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-481">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery 1.8.19 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-482">1.8.18 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-482">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery 1.8.18 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-483">1.8.17 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-483">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery 1.8.17 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-484">1.8.16 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-484">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery 1.8.16 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-485">1.8.15 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-485">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery 1.8.15 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-486">1.8.14 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-486">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery 1.8.14 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-487">1.8.13 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-487">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery 1.8.13 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-488">1.8.12 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-488">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery 1.8.12 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-489">1.8.11 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-489">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery 1.8.11 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-490">1.8.10 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-490">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-491">1.8.9 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-491">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery 1.8.9 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-492">1.8.8 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-492">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery 1.8.8 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-493">1.8.7 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-493">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery 1.8.7 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-494">1.8.6 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-494">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery 1.8.6 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-495">1.8.5 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-495">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery 1.8.5 interfejsu użytkownika")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="4f97f-496">Dostępne wersje weryfikacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-496">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="4f97f-497">Następujące wersje weryfikacji biblioteki jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-497">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="4f97f-498">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="4f97f-498">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f97f-499">Sprawdź poprawność 1.17.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-499">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 1.17.0 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-500">Sprawdź poprawność 1.16.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-500">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery 1.16.0 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-501">Sprawdź poprawność 1.15.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-501">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 1.15.1 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-502">Sprawdź poprawność 1.15.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-502">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 1.15.0 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-503">Sprawdź poprawność 1.14.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-503">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 1.14.0 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-504">Sprawdź poprawność 1.13.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-504">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 1.13.1 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-505">Sprawdź poprawność 1.13.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-505">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 1.13.0 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-506">Sprawdź poprawność 1.12.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-506">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-507">Sprawdź poprawność 1.11.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-507">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-508">Sprawdź poprawność 1.11.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-508">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery 1.11.0 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-509">Sprawdź poprawność 1.10.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-509">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 sprawdzania poprawności")
- [<span data-ttu-id="4f97f-510">Sprawdź poprawność 1.9 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-510">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate wersji 1.9")
- [<span data-ttu-id="4f97f-511">Sprawdź poprawność 1.8.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-511">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "wersji jquery.validate 1.8.1")
- [<span data-ttu-id="4f97f-512">Sprawdź poprawność 1.8 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-512">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate wersji 1.8")
- [<span data-ttu-id="4f97f-513">Sprawdź poprawność 1.7 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-513">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate wersji 1.7")
- [<span data-ttu-id="4f97f-514">jQuery weryfikacji 1.6</span><span class="sxs-lookup"><span data-stu-id="4f97f-514">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery weryfikacji w wersji 1.6")
- [<span data-ttu-id="4f97f-515">Sprawdź poprawność 1.5.5 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f97f-515">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery weryfikacji 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="4f97f-516">jQuery Mobile wersjach w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-516">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="4f97f-517">Następujące wersje biblioteki jQuery przenośnych znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-517">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="4f97f-518">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="4f97f-518">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f97f-519">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="4f97f-519">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-520">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-520">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-521">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-521">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-522">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-522">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-523">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-523">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-524">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-524">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-525">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-525">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-526">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-526">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-527">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-527">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-528">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-528">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-529">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-529">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-530">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="4f97f-530">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-531">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-531">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-532">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-532">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-533">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="4f97f-533">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-534">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="4f97f-534">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="4f97f-535">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="4f97f-535">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 w sieci Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="4f97f-536">Dostępne wersje szablonów w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-536">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="4f97f-537">Następujące wersje dodatku szablony jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-537">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="4f97f-538">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="4f97f-538">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f97f-539">jQuery szablonów w wersji Beta 1</span><span class="sxs-lookup"><span data-stu-id="4f97f-539">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery szablonów w wersji Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="4f97f-540">Dostępne wersje cyklu w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-540">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="4f97f-541">Następujące wersje dodatku cyklu jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-541">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="4f97f-542">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="4f97f-542">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f97f-543">jQuery 2.99 cyklu</span><span class="sxs-lookup"><span data-stu-id="4f97f-543">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 cyklu")
- [<span data-ttu-id="4f97f-544">jQuery 2,94 cyklu</span><span class="sxs-lookup"><span data-stu-id="4f97f-544">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2,94 cyklu")
- [<span data-ttu-id="4f97f-545">jQuery 2,88 cyklu</span><span class="sxs-lookup"><span data-stu-id="4f97f-545">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 cyklu")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="4f97f-546">Dostępne wersje DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-546">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="4f97f-547">Następujące wersje dodatku DataTables jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-547">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="4f97f-548">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="4f97f-548">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f97f-549">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="4f97f-549">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="4f97f-550">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-550">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="4f97f-551">jQuery DataTables pytanie 1.9.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-551">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables pytanie 1.9.4")
- [<span data-ttu-id="4f97f-552">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-552">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="4f97f-553">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-553">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="4f97f-554">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-554">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="4f97f-555">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-555">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="4f97f-556">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-556">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="4f97f-557">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-557">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="4f97f-558">Następujące wersje programu [Modernizr](http://www.modernizr.com "Modernizr") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-558">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="4f97f-559">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="4f97f-560">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="4f97f-561">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="4f97f-562">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="4f97f-563">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-1.7-Development-Only.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="4f97f-564">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.0.6-Development-Only.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="4f97f-565">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-565">JSHint Releases on the CDN</span></span>

<span data-ttu-id="4f97f-566">Następujące wersje programu [JSHint](http://www.jshint.com "JSHint") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-566">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="4f97f-567">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="4f97f-568">Wersje odcinania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-568">Knockout Releases on the CDN</span></span>

<span data-ttu-id="4f97f-569">Następujące wersje programu [Knockout](http://www.knockoutjs.com "Knockout") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-569">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="4f97f-570">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="4f97f-571">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="4f97f-572">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="4f97f-573">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="4f97f-574">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="4f97f-575">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="4f97f-576">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="4f97f-577">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="4f97f-578">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="4f97f-579">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="4f97f-580">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="4f97f-581">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="4f97f-582">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="4f97f-583">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="4f97f-584">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="4f97f-585">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="4f97f-586">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="4f97f-587">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="4f97f-588">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="4f97f-589">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="4f97f-590">Globalize wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-590">Globalize Releases on the CDN</span></span>

<span data-ttu-id="4f97f-591">Następujące wersje programu [Globalize](https://github.com/jquery/globalize "Globalize") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-591">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="4f97f-592">Globalize w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-592">Globalize version 1.0.0</span></span>

- <span data-ttu-id="4f97f-593">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="4f97f-594">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/node-main.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="4f97f-595">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="4f97f-596">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="4f97f-597">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="4f97f-598">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="4f97f-599">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="4f97f-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="4f97f-601">Globalize wersji 0.1.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-601">Globalize version 0.1.1</span></span>

- <span data-ttu-id="4f97f-602">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="4f97f-603">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="4f97f-604">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="4f97f-605">wszystkie kultur</span><span class="sxs-lookup"><span data-stu-id="4f97f-605">all cultures</span></span>
- <span data-ttu-id="4f97f-606">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.Culture. js {kultury code}</span><span class="sxs-lookup"><span data-stu-id="4f97f-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="4f97f-607">Zastąp "{Kod kultury}" z kodem żądaną kulturę, np. Microsoft globalize.culture.en GB.js== plików w sieci CDN == te biblioteki zostały przekazane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4f97f-607">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="4f97f-608">Odpowiadać wersji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-608">Respond Releases on the CDN</span></span>

<span data-ttu-id="4f97f-609">Następujące wersje programu [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") odpowiedź znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-609">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="4f97f-610">Odpowiadać wersji 1.4.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-610">Respond version 1.4.2</span></span>

- <span data-ttu-id="4f97f-611">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="4f97f-612">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="4f97f-613">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="4f97f-614">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="4f97f-615">Odpowiadać wersji 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-615">Respond version 1.4.1</span></span>

- <span data-ttu-id="4f97f-616">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="4f97f-617">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="4f97f-618">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="4f97f-619">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="4f97f-620">Odpowiadać wersji 1.4.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-620">Respond version 1.4.0</span></span>

- <span data-ttu-id="4f97f-621">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="4f97f-622">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="4f97f-623">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="4f97f-624">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="4f97f-625">Odpowiadać wersji 1.3.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-625">Respond version 1.3.0</span></span>

- <span data-ttu-id="4f97f-626">http://AJAX.aspnetcdn.com/AJAX/respond/1.3.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="4f97f-627">Odpowiadać wersji 1.2.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-627">Respond version 1.2.0</span></span>

- <span data-ttu-id="4f97f-628">http://AJAX.aspnetcdn.com/AJAX/respond/1.2.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="4f97f-629">Wersje bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-629">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="4f97f-630">Następujące wersje programu [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-630">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="4f97f-631">Wersja bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="4f97f-631">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="4f97f-632">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-633">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-634">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-635">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-636">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-637">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-638">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-639">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-640">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-641">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-642">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-643">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="4f97f-644">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="4f97f-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="4f97f-645">Wersja bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="4f97f-645">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="4f97f-646">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-647">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-648">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-649">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-650">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-651">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-652">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-653">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-654">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-655">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-656">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-657">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="4f97f-658">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="4f97f-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="4f97f-659">Bootstrap wersji 3.3.5</span><span class="sxs-lookup"><span data-stu-id="4f97f-659">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="4f97f-660">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-661">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-662">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-663">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-664">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-665">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-666">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-667">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-668">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-669">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-670">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-671">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="4f97f-672">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="4f97f-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="4f97f-673">Wersja bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-673">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="4f97f-674">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-675">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-676">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-677">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-678">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-679">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-680">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-681">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-682">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-683">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-684">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-685">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="4f97f-686">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="4f97f-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="4f97f-687">Wersja bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-687">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="4f97f-688">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-689">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-690">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-691">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-692">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-693">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-694">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-695">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-696">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-697">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-698">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-699">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="4f97f-700">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="4f97f-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="4f97f-701">Wersja bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-701">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="4f97f-702">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-703">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-704">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-705">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-706">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-707">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-708">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-709">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-710">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-711">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-712">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-713">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="4f97f-714">Wersja bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-714">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="4f97f-715">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-716">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-717">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-718">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-719">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-720">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-721">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-722">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-723">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-724">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-725">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-726">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="4f97f-727">Wersja bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-727">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="4f97f-728">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-729">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-730">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-731">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-732">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-733">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-734">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-735">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-736">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-737">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-738">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-739">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="4f97f-740">Wersja bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-740">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="4f97f-741">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-742">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-743">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-744">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-745">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-746">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-747">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-748">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-749">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-750">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-751">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-752">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="4f97f-753">Wersja bootstrap 3.1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-753">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="4f97f-754">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-755">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-756">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-757">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="4f97f-758">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-759">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-760">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="4f97f-761">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-762">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-763">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-764">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-765">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="4f97f-766">Wersja bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-766">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="4f97f-767">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-768">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-769">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-770">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-771">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-772">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-773">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-774">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-775">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-776">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="4f97f-777">Wersja bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-777">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="4f97f-778">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-779">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-780">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-781">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-782">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-783">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-784">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-785">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-786">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-787">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="4f97f-788">Bootstrap wersji 3.0.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-788">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="4f97f-789">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-790">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-791">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-792">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-793">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-794">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-795">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-796">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-797">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-798">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="4f97f-799">Wersja bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-799">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="4f97f-800">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-801">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-802">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-803">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-804">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="4f97f-805">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="4f97f-806">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="4f97f-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="4f97f-807">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="4f97f-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="4f97f-808">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="4f97f-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="4f97f-809">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="4f97f-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="4f97f-810">Wersja bootstrap 2.3.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-810">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="4f97f-811">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-812">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-813">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-814">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-815">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="4f97f-816">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap-responsive.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="4f97f-817">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="4f97f-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="4f97f-818">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="4f97f-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="4f97f-819">Wersja bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-819">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="4f97f-820">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="4f97f-821">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="4f97f-822">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="4f97f-823">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="4f97f-824">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="4f97f-825">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap-responsive.min.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="4f97f-826">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="4f97f-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="4f97f-827">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="4f97f-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="4f97f-828">Wersje bootstrap TouchCarousel w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-828">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="4f97f-829">Następujące wersje programu [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel wersjach znajdują się w sieci CDN :</span><span class="sxs-lookup"><span data-stu-id="4f97f-829">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="4f97f-830">Bootstrap TouchCarousel wersji 0.8.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-830">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="4f97f-831">http://AJAX.aspnetcdn.com/AJAX/bootstrap-Touch-carousel/0.8.0/CSS/bootstrap-Touch-carousel.css</span><span class="sxs-lookup"><span data-stu-id="4f97f-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="4f97f-832">http://AJAX.aspnetcdn.com/AJAX/bootstrap-Touch-carousel/0.8.0/js/bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="4f97f-833">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-833">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="4f97f-834">Następujące wersje programu [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js wersjach znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-834">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="4f97f-835">Wersja hammer.js 2.0.4</span><span class="sxs-lookup"><span data-stu-id="4f97f-835">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="4f97f-836">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="4f97f-837">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="4f97f-838">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="4f97f-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="4f97f-839">Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-839">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="4f97f-840">Następujących wersji biblioteki ASP.NET Ajax znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f97f-840">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="4f97f-841">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="4f97f-841">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f97f-842">Formularze sieci Web ASP.NET i Ajax wersji 4.5.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-842">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "formularzy sieci Web ASP.NET i Ajax 4.5.2")
- [<span data-ttu-id="4f97f-843">Formularze sieci Web ASP.NET i Ajax w wersji 4</span><span class="sxs-lookup"><span data-stu-id="4f97f-843">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "formularzy sieci Web ASP.NET i Ajax 4")
- [<span data-ttu-id="4f97f-844">ASP.NET Ajax w wersji 3.5</span><span class="sxs-lookup"><span data-stu-id="4f97f-844">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="4f97f-845">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-845">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="4f97f-846">Następujące pliki platformy ASP.NET MVC JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-846">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="4f97f-847">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-847">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="4f97f-848">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="4f97f-849">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="4f97f-850">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-850">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="4f97f-851">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="4f97f-852">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="4f97f-853">ASP.NET MVC W WERSJI 5.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-853">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="4f97f-854">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="4f97f-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="4f97f-856">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-856">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="4f97f-857">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="4f97f-858">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="4f97f-859">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-859">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="4f97f-860">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jquery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="4f97f-861">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jquery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="4f97f-862">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="4f97f-863">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="4f97f-864">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="4f97f-865">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="4f97f-866">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-866">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="4f97f-867">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="4f97f-868">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="4f97f-869">ASP.NET MVC W WERSJI 1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-869">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="4f97f-870">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="4f97f-871">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="4f97f-872">Wersje biblioteki SignalR platformy ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f97f-872">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="4f97f-873">Następujące pliki ASP.NET SignalR JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f97f-873">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="4f97f-874">Biblioteka SignalR platformy ASP.NET 2.2.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-874">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="4f97f-875">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="4f97f-876">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="4f97f-877">Biblioteka SignalR platformy ASP.NET 2.2.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-877">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="4f97f-878">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="4f97f-879">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="4f97f-880">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-880">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="4f97f-881">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="4f97f-882">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="4f97f-883">Biblioteka SignalR platformy ASP.NET 2.1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-883">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="4f97f-884">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="4f97f-885">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="4f97f-886">Biblioteka SignalR platformy ASP.NET 2.0.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-886">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="4f97f-887">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="4f97f-888">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="4f97f-889">Biblioteka SignalR platformy ASP.NET pkt 2.0.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-889">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="4f97f-890">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="4f97f-891">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="4f97f-892">Biblioteka SignalR platformy ASP.NET 2.0.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-892">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="4f97f-893">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="4f97f-894">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="4f97f-895">Biblioteka SignalR platformy ASP.NET 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-895">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="4f97f-896">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="4f97f-897">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="4f97f-898">Biblioteka SignalR platformy ASP.NET 1.1.3</span><span class="sxs-lookup"><span data-stu-id="4f97f-898">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="4f97f-899">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="4f97f-900">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="4f97f-901">Biblioteka SignalR platformy ASP.NET 1.1.2</span><span class="sxs-lookup"><span data-stu-id="4f97f-901">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="4f97f-902">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="4f97f-903">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="4f97f-904">Biblioteka SignalR platformy ASP.NET 1.1.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-904">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="4f97f-905">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="4f97f-906">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="4f97f-907">Biblioteka SignalR platformy ASP.NET 1.1.0</span><span class="sxs-lookup"><span data-stu-id="4f97f-907">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="4f97f-908">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="4f97f-909">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="4f97f-910">Biblioteka SignalR platformy ASP.NET 1.0.1</span><span class="sxs-lookup"><span data-stu-id="4f97f-910">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="4f97f-911">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="4f97f-912">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="4f97f-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="4f97f-913">Aby dowiedzieć się, warunki użytkowania CDN, zobacz [Microsoft Ajax CDN warunkom użytkowania](https://www.asp.net/terms-of-use "Microsoft Ajax CDN warunkom użytkowania").</span><span class="sxs-lookup"><span data-stu-id="4f97f-913">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
