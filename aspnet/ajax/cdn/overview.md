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
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="0aac0-102">Microsoft Ajax sieci dostarczania zawartości</span><span class="sxs-lookup"><span data-stu-id="0aac0-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="0aac0-103">Uwaga: Microsoft Ajax CDN ma umowy dotyczącej poziomu usług niż przy użyciu usługi Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="0aac0-104">Spis treści</span><span class="sxs-lookup"><span data-stu-id="0aac0-104">Table of Contents</span></span>

<span data-ttu-id="0aac0-105">**[AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="0aac0-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="0aac0-106">**[Obsługa .vsdoc programu Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="0aac0-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="0aac0-107">**[Za pomocą kodu ASP.NET Ajax z sieci CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="0aac0-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="0aac0-108">**[Przy użyciu jQuery z sieci CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="0aac0-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="0aac0-109">**[Przy użyciu interfejsu użytkownika z sieci CDN jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="0aac0-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="0aac0-110">**[Pliki innych firm w sieci CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="0aac0-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="0aac0-111">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="0aac0-112">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="0aac0-113">Dostępne wersje interfejsu użytkownika w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="0aac0-114">Dostępne wersje weryfikacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="0aac0-115">jQuery Mobile wersjach w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="0aac0-116">Dostępne wersje szablonów w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="0aac0-117">Dostępne wersje cyklu w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="0aac0-118">Dostępne wersje DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="0aac0-119">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="0aac0-120">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="0aac0-121">Wersje odcinania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="0aac0-122">Globalize wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="0aac0-123">Odpowiadać wersji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="0aac0-124">Wersje bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="0aac0-125">Wersje bootstrap TouchCarousel w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="0aac0-126">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="0aac0-127">Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="0aac0-128">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="0aac0-129">Wersje biblioteki SignalR platformy ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="0aac0-130">Microsoft Ajax sieci dostarczania zawartości (CDN) obsługuje bibliotek JavaScript popularnych innych firm, np. jQuery i umożliwia łatwe dodanie ich do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0aac0-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="0aac0-131">Na przykład możesz rozpocząć używać jQuery, który znajduje się w tej sieci CDN po prostu dodając &lt;skryptu&gt; tag do strony, która wskazuje ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="0aac0-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="0aac0-132">Dzięki wykorzystaniu sieci CDN może znacznie poprawić wydajność aplikacji Ajax.</span><span class="sxs-lookup"><span data-stu-id="0aac0-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="0aac0-133">Zawartość CDN są buforowane na serwerów na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="0aac0-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="0aac0-134">Ponadto CDN umożliwia ponowne użycie innej buforowane pliki JavaScript dla witryn sieci web, które znajdują się w różnych domenach w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="0aac0-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="0aac0-135">CDN obsługuje SSL (HTTPS), w razie potrzeby do obsługi strony sieci web przy użyciu protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="0aac0-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="0aac0-136">Usługa CDN obsługuje następujące biblioteki skryptu innych firm, które zostały przekazane i są, licencjonowane przez właścicieli tych biblioteki:</span><span class="sxs-lookup"><span data-stu-id="0aac0-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="0aac0-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="0aac0-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="0aac0-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="0aac0-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="0aac0-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="0aac0-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="0aac0-140">jQuery sprawdzania poprawności (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="0aac0-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="0aac0-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="0aac0-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="0aac0-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="0aac0-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="0aac0-143">Usługa Microsoft Ajax CDN zawiera również następujące biblioteki, które zostały przekazane przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="0aac0-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="0aac0-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="0aac0-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="0aac0-145">Pliki języka JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0aac0-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="0aac0-146">Pliki ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="0aac0-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="0aac0-147">Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="0aac0-148">Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="0aac0-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="0aac0-149">Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="0aac0-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="0aac0-150">Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="0aac0-151">Jeśli chcesz przesłać biblioteki JavaScript i biblioteki jest jednym z najwyższym bibliotek JavaScript (wymienione w http://trends.builtwith.com) lub rozszerzenia/wtyczek do tych bibliotek, które są () popularnych; lub (b) przydatne do użycia na platformie ASP.NET, a następnie skontaktuj się z pomocą AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="0aac0-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="0aac0-152">AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="0aac0-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="0aac0-153">CDN używany do używania nazwy domeny microsoft.com i została zmieniona do używania nazwy domeny aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="0aac0-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="0aac0-154">Ta zmiana została wprowadzona w celu zwiększenia wydajności, ponieważ podczas przeglądarką domenie microsoft.com, do których odwołuje się on będzie wysyłać żadnych plików cookie z tej domeny przez sieć z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="0aac0-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="0aac0-155">Zmieniając nazwę domeny inne niż microsoft.com można zwiększyć wydajność przez tyle 25%.</span><span class="sxs-lookup"><span data-stu-id="0aac0-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="0aac0-156">Uwaga ajax.microsoft.com będą nadal działać, ale ajax.aspnetcdn.com jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="0aac0-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="0aac0-157">Stary Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="0aac0-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="0aac0-158">Nowy Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="0aac0-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="0aac0-159">Obsługa .vsdoc programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0aac0-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="0aac0-160">Do korzystania z plików .vsdoc prawidłowo z programu Visual Studio 2008, należy się upewnić, że masz VS 2008 z dodatkiem SP1 należy zainstalować i zainstalowana poprawka vsdoc plików.</span><span class="sxs-lookup"><span data-stu-id="0aac0-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="0aac0-161">Możesz uzyskać je w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="0aac0-161">You can get these from here:</span></span>

- [<span data-ttu-id="0aac0-162">Pobierz program Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="0aac0-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "pobierania programu Visual Studio 2008 z dodatkiem SP1")
- [<span data-ttu-id="0aac0-163">Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="0aac0-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "pobrać poprawkę .vsdoc dla programu Visual Studio 2008 z dodatkiem SP1")

<span data-ttu-id="0aac0-164">Program Visual Studio 2010 obsługuje pliki .vsdoc bez żadnych dodatkowych poprawek.</span><span class="sxs-lookup"><span data-stu-id="0aac0-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="0aac0-165">Za pomocą kodu ASP.NET Ajax z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="0aac0-166">Podczas korzystania z programu ASP.NET 4, można przekierowywać wszystkie żądania ASP.NET framework skryptów do sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="0aac0-167">Pobieranie skryptów z sieci CDN zamiast serwera sieci web lokalnego znacznie może poprawić wydajność publicznych witryn sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0aac0-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="0aac0-168">Za pomocą właściwości ScriptManager EnableCDN Przekieruj żądania skryptu framework ASP.NET do programu Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="0aac0-169">Przy użyciu jQuery z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="0aac0-170">Za pomocą skryptów jQuery hostowanych w sieci CDN w aplikacji sieci Web, dodając następujący element skryptu do strony:</span><span class="sxs-lookup"><span data-stu-id="0aac0-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="0aac0-171">Usługa CDN zawiera również wersję zminimalizowany skryptu jQuery, którą można uzyskać za pomocą następującego elementu:</span><span class="sxs-lookup"><span data-stu-id="0aac0-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="0aac0-172">Aby zezwolić na stronę do powrotu do ładowania jQuery z ścieżkę lokalną na własne witryny sieci Web, jeśli sieć CDN jest niedostępny, Dodaj następujący element natychmiast po elemencie odwołujące się do sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="0aac0-173">Następujące przykładowe strony korzysta z wersji CDN biblioteki jQuery (z powrotu do lokalnej kopii) do wyświetlenia zawartości elementu div, gdy przycisk zostanie kliknięty.</span><span class="sxs-lookup"><span data-stu-id="0aac0-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="0aac0-174">Można dowiedzieć się więcej o jQuery i pobrać kopię lokalną jQuery odwiedzając [jQuery](http://jquery.com/) witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0aac0-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="0aac0-175">Przy użyciu interfejsu użytkownika z sieci CDN jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="0aac0-176">Usługa CDN obsługuje również biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="0aac0-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="0aac0-177">Biblioteka interfejsu użytkownika jQuery zawiera bogaty zestaw elementów widget i efekty, których można używać w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0aac0-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="0aac0-178">Na przykład następująca strona przedstawiono, jak jQuery selektora daty interfejsu użytkownika w kontekście aplikacji formularzy sieci Web programu ASP.NET można użyć do wyświetlenia wyskakującego kalendarza:</span><span class="sxs-lookup"><span data-stu-id="0aac0-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="0aac0-179">Gdy fokus zostanie przeniesiony do pola tekstowego przy użyciu klawiatury, zostanie wyświetlony kalendarz:</span><span class="sxs-lookup"><span data-stu-id="0aac0-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Utworzone za pomocą selektora daty kalendarza podręcznego](overview/_static/image1.png)

<span data-ttu-id="0aac0-181">Zwróć uwagę, że musi zawierać trzy pliki z sieci CDN w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="0aac0-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="0aac0-182">Biblioteki jQuery &mdash; biblioteki interfejsu użytkownika jQuery jest zależna od biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="0aac0-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="0aac0-183">Należy dodać biblioteki jQuery do strony przed dodaniem biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="0aac0-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="0aac0-184">Biblioteka interfejsu użytkownika jQuery &mdash; biblioteki interfejsu użytkownika jQuery zawiera elementy widget, takie jak używane na stronie powyżej widżetu selektora daty i efektów interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="0aac0-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="0aac0-185">Motyw interfejsu użytkownika jQuery &mdash; jQuery interfejsu użytkownika obsługuje różne kompozycje.</span><span class="sxs-lookup"><span data-stu-id="0aac0-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="0aac0-186">Strona sieci zawiera łącze do pliku CSS do zaimportowania Redmond motywu.</span><span class="sxs-lookup"><span data-stu-id="0aac0-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="0aac0-187">Wszystkie kompozycje interfejsu użytkownika jQuery standardowe znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="0aac0-188">[Odwiedź stronę tego](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN") do wyświetlania miniatur dla każdego motywu.</span><span class="sxs-lookup"><span data-stu-id="0aac0-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="0aac0-189">Aby dowiedzieć się więcej na temat biblioteki interfejsu użytkownika jQuery, można znaleźć w oficjalnym [witryny sieci Web interfejsu użytkownika jQuery](http://jQueryUI.com "witryny sieci Web interfejsu użytkownika jQuery").</span><span class="sxs-lookup"><span data-stu-id="0aac0-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="0aac0-190">Pliki innych firm w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="0aac0-191">Usługa CDN obsługuje niektóre najbardziej popularnych bibliotek JavaScript innych firm.</span><span class="sxs-lookup"><span data-stu-id="0aac0-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="0aac0-192">Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="0aac0-193">Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="0aac0-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="0aac0-194">Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="0aac0-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="0aac0-195">Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="0aac0-196">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="0aac0-197">Następujące wersje jQuery znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="0aac0-198">Wersja jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="0aac0-199">Wersja jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="0aac0-200">Wersja jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="0aac0-201">Wersja jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="0aac0-202">Wersja jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="0aac0-203">Wersja jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="0aac0-204">Wersja jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="0aac0-205">Wersja jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="0aac0-206">Wersja jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="0aac0-207">Wersja jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="0aac0-208">Wersja jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="0aac0-209">Wersja jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="0aac0-210">Wersja jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="0aac0-211">Wersja jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="0aac0-212">Wersja jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="0aac0-213">Wersja jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="0aac0-214">Wersja jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="0aac0-215">Wersja jQuery pkt 2.0.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="0aac0-216">Wersja jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="0aac0-217">jQuery wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="0aac0-218">Wersja jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="0aac0-219">Wersja jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="0aac0-220">Wersja jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="0aac0-221">Wersja jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="0aac0-222">Wersja jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="0aac0-223">Wersja jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="0aac0-224">Wersja jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="0aac0-225">Wersja jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="0aac0-226">Wersja jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="0aac0-227">Wersja jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="0aac0-228">Wersja jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="0aac0-229">Wersja jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="0aac0-230">Wersja jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="0aac0-231">Wersja jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="0aac0-232">Wersja jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="0aac0-233">Wersja jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="0aac0-234">Wersja jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="0aac0-235">Wersja jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="0aac0-236">Wersja jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="0aac0-237">Wersja jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="0aac0-238">Wersja jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="0aac0-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="0aac0-239">Wersja jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="0aac0-240">Wersja jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="0aac0-241">jQuery wersji 1.6.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="0aac0-242">jQuery wersji 1.6.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="0aac0-243">jQuery w wersji 1.6</span><span class="sxs-lookup"><span data-stu-id="0aac0-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="0aac0-244">Wersja jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="0aac0-245">Wersja jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="0aac0-246">jQuery w wersji 1.5</span><span class="sxs-lookup"><span data-stu-id="0aac0-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="0aac0-247">Wersja jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="0aac0-248">Wersja jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="0aac0-249">Wersja jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="0aac0-250">Wersja jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="0aac0-251">jQuery w wersji 1.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="0aac0-252">Wersja jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="0aac0-253">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="0aac0-254">Następujące wersje jQuery migracji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="0aac0-255">jQuery migracji wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="0aac0-256">jQuery migracji wersji 1.2.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="0aac0-257">jQuery migracji wersji 1.2.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="0aac0-258">jQuery migracji wersji 1.1.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="0aac0-259">jQuery migracji wersji 1.1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="0aac0-260">jQuery migracji w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="0aac0-261">Dostępne wersje interfejsu użytkownika w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="0aac0-262">Następujące wersje biblioteki interfejsu użytkownika jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="0aac0-263">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="0aac0-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0aac0-264">1.12.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery 1.12.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-265">1.12.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery 1.12.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-266">1.11.4 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery 1.11.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-267">1.11.3 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery 1.11.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-268">1.11.2 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery 1.11.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-269">1.11.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery 1.11.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-270">1.11.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery 1.11.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-271">1.10.4 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery 1.10.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-272">1.10.3 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery 1.10.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-273">jQuery 1.10.2 interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="0aac0-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-274">1.10.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery 1.10.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-275">1.10.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery 1.10.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-276">1.9.2 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery 1.9.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-277">1.9.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery 1.9.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-278">1.9.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery 1.9.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-279">1.8.24 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery 1.8.24 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-280">1.8.23 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery 1.8.23 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-281">1.8.22 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery 1.8.22 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-282">1.8.21 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery 1.8.21 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-283">1.8.20 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery 1.8.20 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-284">1.8.19 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery 1.8.19 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-285">1.8.18 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery 1.8.18 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-286">1.8.17 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery 1.8.17 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-287">1.8.16 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery 1.8.16 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-288">1.8.15 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery 1.8.15 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-289">1.8.14 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery 1.8.14 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-290">1.8.13 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery 1.8.13 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-291">1.8.12 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery 1.8.12 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-292">1.8.11 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery 1.8.11 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-293">1.8.10 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-294">1.8.9 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery 1.8.9 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-295">1.8.8 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery 1.8.8 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-296">1.8.7 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery 1.8.7 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-297">1.8.6 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery 1.8.6 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="0aac0-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="0aac0-299">Dostępne wersje weryfikacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="0aac0-300">Następujące wersje weryfikacji biblioteki jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="0aac0-301">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="0aac0-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0aac0-302">Sprawdź poprawność 1.17.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 1.17.0 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-303">Sprawdź poprawność 1.16.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery 1.16.0 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-304">Sprawdź poprawność 1.15.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 1.15.1 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-305">Sprawdź poprawność 1.15.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 1.15.0 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-306">Sprawdź poprawność 1.14.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 1.14.0 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-307">Sprawdź poprawność 1.13.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 1.13.1 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-308">Sprawdź poprawność 1.13.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 1.13.0 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-309">Sprawdź poprawność 1.12.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-310">Sprawdź poprawność 1.11.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-311">Sprawdź poprawność 1.11.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery 1.11.0 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-312">Sprawdź poprawność 1.10.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 sprawdzania poprawności")
- [<span data-ttu-id="0aac0-313">Sprawdź poprawność 1.9 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate wersji 1.9")
- [<span data-ttu-id="0aac0-314">Sprawdź poprawność 1.8.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "wersji jquery.validate 1.8.1")
- [<span data-ttu-id="0aac0-315">Sprawdź poprawność 1.8 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate wersji 1.8")
- [<span data-ttu-id="0aac0-316">Sprawdź poprawność 1.7 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate wersji 1.7")
- [<span data-ttu-id="0aac0-317">jQuery weryfikacji 1.6</span><span class="sxs-lookup"><span data-stu-id="0aac0-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery weryfikacji w wersji 1.6")
- [<span data-ttu-id="0aac0-318">Sprawdź poprawność 1.5.5 jQuery</span><span class="sxs-lookup"><span data-stu-id="0aac0-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery weryfikacji 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="0aac0-319">jQuery Mobile wersjach w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="0aac0-320">Następujące wersje biblioteki jQuery przenośnych znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="0aac0-321">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="0aac0-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0aac0-322">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="0aac0-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-323">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-324">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-325">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-326">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-327">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-328">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-329">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-330">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-331">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-332">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-333">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="0aac0-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-334">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-335">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-336">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="0aac0-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-337">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="0aac0-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="0aac0-338">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="0aac0-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 w sieci Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="0aac0-339">Dostępne wersje szablonów w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="0aac0-340">Następujące wersje dodatku szablony jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="0aac0-341">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="0aac0-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0aac0-342">jQuery szablonów w wersji Beta 1</span><span class="sxs-lookup"><span data-stu-id="0aac0-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery szablonów w wersji Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="0aac0-343">Dostępne wersje cyklu w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="0aac0-344">Następujące wersje dodatku cyklu jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="0aac0-345">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="0aac0-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0aac0-346">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="0aac0-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="0aac0-347">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="0aac0-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="0aac0-348">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="0aac0-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="0aac0-349">Dostępne wersje DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="0aac0-350">Następujące wersje dodatku DataTables jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="0aac0-351">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="0aac0-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0aac0-352">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="0aac0-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="0aac0-353">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="0aac0-354">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="0aac0-355">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="0aac0-356">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="0aac0-357">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="0aac0-358">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="0aac0-359">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="0aac0-360">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="0aac0-361">Następujące wersje programu [Modernizr](http://www.modernizr.com "Modernizr") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="0aac0-362">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="0aac0-363">Następujące wersje programu [JSHint](http://www.jshint.com "JSHint") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="0aac0-364">Wersje odcinania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="0aac0-365">Następujące wersje programu [Knockout](http://www.knockoutjs.com "Knockout") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="0aac0-366">Globalize wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="0aac0-367">Następujące wersje programu [Globalize](https://github.com/jquery/globalize "Globalize") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="0aac0-368">Globalize w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="0aac0-369">Globalize wersji 0.1.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="0aac0-370">wszystkie kultur</span><span class="sxs-lookup"><span data-stu-id="0aac0-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="0aac0-371">Zastąp "{Kod kultury}" z kodem żądaną kulturę, np. Microsoft globalize.culture.en GB.js== plików w sieci CDN == te biblioteki zostały przekazane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0aac0-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="0aac0-372">Odpowiadać wersji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="0aac0-373">Następujące wersje programu [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") odpowiedź znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="0aac0-374">Odpowiadać wersji 1.4.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="0aac0-375">Odpowiadać wersji 1.4.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="0aac0-376">Odpowiadać wersji 1.4.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="0aac0-377">Odpowiadać wersji 1.3.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="0aac0-378">Odpowiadać wersji 1.2.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="0aac0-379">Wersje bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="0aac0-380">Następujące wersje programu [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="0aac0-381">Wersja bootstrap 4.0.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="0aac0-382">Wersja bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="0aac0-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="0aac0-383">Wersja bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="0aac0-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="0aac0-384">Bootstrap wersji 3.3.5</span><span class="sxs-lookup"><span data-stu-id="0aac0-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="0aac0-385">Wersja bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="0aac0-386">Wersja bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="0aac0-387">Wersja bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="0aac0-388">Wersja bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="0aac0-389">Wersja bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="0aac0-390">Wersja bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="0aac0-391">Wersja bootstrap 3.1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="0aac0-392">Wersja bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="0aac0-393">Wersja bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="0aac0-394">Bootstrap wersji 3.0.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="0aac0-395">Wersja bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="0aac0-396">Wersja bootstrap 2.3.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="0aac0-397">Wersja bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="0aac0-398">Wersje bootstrap TouchCarousel w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="0aac0-399">Następujące wersje programu [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") Bootstrap TouchCarousel wersjach znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="0aac0-400">Bootstrap TouchCarousel wersji 0.8.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="0aac0-401">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="0aac0-402">Następujące wersje programu [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js wersjach znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="0aac0-403">Wersja hammer.js 2.0.4</span><span class="sxs-lookup"><span data-stu-id="0aac0-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="0aac0-404">Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="0aac0-405">Następujących wersji biblioteki ASP.NET Ajax znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="0aac0-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="0aac0-406">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="0aac0-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0aac0-407">Formularze sieci Web ASP.NET i Ajax wersji 4.5.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "formularzy sieci Web ASP.NET i Ajax 4.5.2")
- [<span data-ttu-id="0aac0-408">Formularze sieci Web ASP.NET i Ajax w wersji 4</span><span class="sxs-lookup"><span data-stu-id="0aac0-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "formularzy sieci Web ASP.NET i Ajax 4")
- [<span data-ttu-id="0aac0-409">ASP.NET Ajax w wersji 3.5</span><span class="sxs-lookup"><span data-stu-id="0aac0-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="0aac0-410">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="0aac0-411">Następujące pliki platformy ASP.NET MVC JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="0aac0-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="0aac0-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="0aac0-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="0aac0-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="0aac0-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="0aac0-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="0aac0-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="0aac0-419">Wersje biblioteki SignalR platformy ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="0aac0-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="0aac0-420">Następujące pliki ASP.NET SignalR JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="0aac0-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="0aac0-421">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="0aac0-422">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="0aac0-423">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="0aac0-424">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="0aac0-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="0aac0-426">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="0aac0-427">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="0aac0-428">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="0aac0-429">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="0aac0-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="0aac0-430">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="0aac0-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="0aac0-431">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="0aac0-432">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="0aac0-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="0aac0-433">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="0aac0-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="0aac0-434">Aby dowiedzieć się, warunki użytkowania CDN, zobacz [Microsoft Ajax CDN warunkom użytkowania](https://www.asp.net/terms-of-use "Microsoft Ajax CDN warunkom użytkowania").</span><span class="sxs-lookup"><span data-stu-id="0aac0-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
