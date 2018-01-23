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
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="a0e88-102">Microsoft Ajax sieci dostarczania zawartości</span><span class="sxs-lookup"><span data-stu-id="a0e88-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="a0e88-103">Uwaga: Microsoft Ajax CDN ma umowy dotyczącej poziomu usług niż przy użyciu usługi Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="a0e88-104">Spis treści</span><span class="sxs-lookup"><span data-stu-id="a0e88-104">Table of Contents</span></span>

<span data-ttu-id="a0e88-105">**[AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="a0e88-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="a0e88-106">**[Obsługa .vsdoc programu Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="a0e88-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="a0e88-107">**[Za pomocą kodu ASP.NET Ajax z sieci CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="a0e88-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="a0e88-108">**[Przy użyciu jQuery z sieci CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="a0e88-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="a0e88-109">**[Przy użyciu interfejsu użytkownika z sieci CDN jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="a0e88-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="a0e88-110">**[Pliki innych firm w sieci CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="a0e88-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="a0e88-111">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="a0e88-112">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="a0e88-113">Dostępne wersje interfejsu użytkownika w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="a0e88-114">Dostępne wersje weryfikacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="a0e88-115">jQuery Mobile wersjach w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="a0e88-116">Dostępne wersje szablonów w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="a0e88-117">Dostępne wersje cyklu w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="a0e88-118">Dostępne wersje DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="a0e88-119">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="a0e88-120">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="a0e88-121">Wersje odcinania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="a0e88-122">Globalize wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="a0e88-123">Odpowiadać wersji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="a0e88-124">Wersje bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="a0e88-125">Wersje bootstrap TouchCarousel w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="a0e88-126">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="a0e88-127">Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="a0e88-128">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="a0e88-129">Wersje biblioteki SignalR platformy ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="a0e88-130">Microsoft Ajax sieci dostarczania zawartości (CDN) obsługuje bibliotek JavaScript popularnych innych firm, np. jQuery i umożliwia łatwe dodanie ich do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a0e88-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="a0e88-131">Na przykład możesz rozpocząć używać jQuery, który znajduje się w tej sieci CDN po prostu dodając &lt;skryptu&gt; tag do strony, która wskazuje ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="a0e88-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="a0e88-132">Dzięki wykorzystaniu sieci CDN może znacznie poprawić wydajność aplikacji Ajax.</span><span class="sxs-lookup"><span data-stu-id="a0e88-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="a0e88-133">Zawartość CDN są buforowane na serwerów na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="a0e88-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="a0e88-134">Ponadto CDN umożliwia ponowne użycie innej buforowane pliki JavaScript dla witryn sieci web, które znajdują się w różnych domenach w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="a0e88-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="a0e88-135">CDN obsługuje SSL (HTTPS), w razie potrzeby do obsługi strony sieci web przy użyciu protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="a0e88-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="a0e88-136">Usługa CDN obsługuje następujące biblioteki skryptu innych firm, które zostały przekazane i są, licencjonowane przez właścicieli tych biblioteki:</span><span class="sxs-lookup"><span data-stu-id="a0e88-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="a0e88-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="a0e88-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="a0e88-138">jQuery interfejsu użytkownika (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="a0e88-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="a0e88-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="a0e88-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="a0e88-140">jQuery sprawdzania poprawności (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="a0e88-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="a0e88-141">jQuery cyklu (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="a0e88-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="a0e88-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="a0e88-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="a0e88-143">Usługa Microsoft Ajax CDN zawiera również następujące biblioteki, które zostały przekazane przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="a0e88-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="a0e88-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="a0e88-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="a0e88-145">Pliki języka JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a0e88-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="a0e88-146">Pliki ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="a0e88-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="a0e88-147">Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="a0e88-148">Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="a0e88-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="a0e88-149">Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="a0e88-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="a0e88-150">Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="a0e88-151">Jeśli chcesz przesłać biblioteki JavaScript i biblioteki jest jednym z góry bibliotek JavaScript (wymienione w http://trends.builtwith.com) lub rozszerzenia/wtyczek do tych bibliotek, które są () popularnych; lub b przydatne do użycia na platformie ASP.NET, a następnie skontaktuj się z AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="a0e88-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="a0e88-152">AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="a0e88-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="a0e88-153">CDN używany do używania nazwy domeny microsoft.com i została zmieniona do używania nazwy domeny aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="a0e88-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="a0e88-154">Ta zmiana została wprowadzona w celu zwiększenia wydajności, ponieważ podczas przeglądarką domenie microsoft.com, do których odwołuje się on będzie wysyłać żadnych plików cookie z tej domeny przez sieć z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="a0e88-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="a0e88-155">Zmieniając nazwę domeny inne niż microsoft.com można zwiększyć wydajność przez tyle 25%.</span><span class="sxs-lookup"><span data-stu-id="a0e88-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="a0e88-156">Uwaga ajax.microsoft.com będą nadal działać, ale ajax.aspnetcdn.com jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="a0e88-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="a0e88-157">Stary Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="a0e88-158">Nowy Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="a0e88-159">Obsługa .vsdoc programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0e88-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="a0e88-160">Do korzystania z plików .vsdoc prawidłowo z programu Visual Studio 2008, należy się upewnić, że masz VS 2008 z dodatkiem SP1 należy zainstalować i zainstalowana poprawka vsdoc plików.</span><span class="sxs-lookup"><span data-stu-id="a0e88-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="a0e88-161">Możesz uzyskać je w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="a0e88-161">You can get these from here:</span></span>

- [<span data-ttu-id="a0e88-162">Pobierz program Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="a0e88-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "pobierania programu Visual Studio 2008 z dodatkiem SP1")
- [<span data-ttu-id="a0e88-163">Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="a0e88-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "pobrać poprawkę .vsdoc dla programu Visual Studio 2008 z dodatkiem SP1")

<span data-ttu-id="a0e88-164">Program Visual Studio 2010 obsługuje pliki .vsdoc bez żadnych dodatkowych poprawek.</span><span class="sxs-lookup"><span data-stu-id="a0e88-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="a0e88-165">Za pomocą kodu ASP.NET Ajax z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="a0e88-166">Podczas korzystania z programu ASP.NET 4, można przekierowywać wszystkie żądania ASP.NET framework skryptów do sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="a0e88-167">Pobieranie skryptów z sieci CDN zamiast serwera sieci web lokalnego znacznie może poprawić wydajność publicznych witryn sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a0e88-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="a0e88-168">Za pomocą właściwości ScriptManager EnableCDN Przekieruj żądania skryptu framework ASP.NET do programu Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="a0e88-169">Przy użyciu jQuery z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="a0e88-170">Za pomocą skryptów jQuery hostowanych w sieci CDN w aplikacji sieci Web, dodając następujący element skryptu do strony:</span><span class="sxs-lookup"><span data-stu-id="a0e88-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="a0e88-171">Usługa CDN zawiera również wersję zminimalizowany skryptu jQuery, którą można uzyskać za pomocą następującego elementu:</span><span class="sxs-lookup"><span data-stu-id="a0e88-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="a0e88-172">Aby zezwolić na stronę do powrotu do ładowania jQuery z ścieżkę lokalną na własne witryny sieci Web, jeśli sieć CDN jest niedostępny, Dodaj następujący element natychmiast po elemencie odwołujące się do sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="a0e88-173">Następujące przykładowe strony korzysta z wersji CDN biblioteki jQuery (z powrotu do lokalnej kopii) do wyświetlenia zawartości elementu div, gdy przycisk zostanie kliknięty.</span><span class="sxs-lookup"><span data-stu-id="a0e88-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="a0e88-174">Można dowiedzieć się więcej o jQuery i pobrać kopię lokalną jQuery odwiedzając [jQuery](http://jquery.com/) witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a0e88-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="a0e88-175">Przy użyciu interfejsu użytkownika z sieci CDN jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="a0e88-176">Usługa CDN obsługuje również biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="a0e88-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="a0e88-177">Biblioteka interfejsu użytkownika jQuery zawiera bogaty zestaw elementów widget i efekty, których można używać w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a0e88-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="a0e88-178">Na przykład następująca strona przedstawiono, jak jQuery selektora daty interfejsu użytkownika w kontekście aplikacji formularzy sieci Web programu ASP.NET można użyć do wyświetlenia wyskakującego kalendarza:</span><span class="sxs-lookup"><span data-stu-id="a0e88-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="a0e88-179">Gdy fokus zostanie przeniesiony do pola tekstowego przy użyciu klawiatury, zostanie wyświetlony kalendarz:</span><span class="sxs-lookup"><span data-stu-id="a0e88-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Utworzone za pomocą selektora daty kalendarza podręcznego](overview/_static/image1.png)

<span data-ttu-id="a0e88-181">Zwróć uwagę, że musi zawierać trzy pliki z sieci CDN w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="a0e88-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="a0e88-182">Biblioteki jQuery &mdash; biblioteki interfejsu użytkownika jQuery jest zależna od biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="a0e88-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="a0e88-183">Należy dodać biblioteki jQuery do strony przed dodaniem biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="a0e88-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="a0e88-184">Biblioteka interfejsu użytkownika jQuery &mdash; biblioteki interfejsu użytkownika jQuery zawiera elementy widget, takie jak używane na stronie powyżej widżetu selektora daty i efektów interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="a0e88-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="a0e88-185">Motyw interfejsu użytkownika jQuery &mdash; jQuery interfejsu użytkownika obsługuje różne kompozycje.</span><span class="sxs-lookup"><span data-stu-id="a0e88-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="a0e88-186">Strona sieci zawiera łącze do pliku CSS do zaimportowania Redmond motywu.</span><span class="sxs-lookup"><span data-stu-id="a0e88-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="a0e88-187">Wszystkie kompozycje interfejsu użytkownika jQuery standardowe znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="a0e88-188">[Odwiedź stronę tego](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN") do wyświetlania miniatur dla każdego motywu.</span><span class="sxs-lookup"><span data-stu-id="a0e88-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="a0e88-189">Aby dowiedzieć się więcej na temat biblioteki interfejsu użytkownika jQuery, można znaleźć w oficjalnym [witryny sieci Web interfejsu użytkownika jQuery](http://jQueryUI.com "witryny sieci Web interfejsu użytkownika jQuery").</span><span class="sxs-lookup"><span data-stu-id="a0e88-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="a0e88-190">Pliki innych firm w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="a0e88-191">Usługa CDN obsługuje niektóre najbardziej popularnych bibliotek JavaScript innych firm.</span><span class="sxs-lookup"><span data-stu-id="a0e88-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="a0e88-192">Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="a0e88-193">Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="a0e88-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="a0e88-194">Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="a0e88-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="a0e88-195">Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="a0e88-196">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="a0e88-197">Następujące wersje jQuery znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="a0e88-198">Wersja jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="a0e88-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="a0e88-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.3.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="a0e88-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.3.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="a0e88-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.3.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="a0e88-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.3.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="a0e88-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.3.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="a0e88-205">Wersja jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="a0e88-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="a0e88-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="a0e88-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="a0e88-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="a0e88-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="a0e88-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="a0e88-212">Wersja jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="a0e88-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="a0e88-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="a0e88-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="a0e88-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="a0e88-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="a0e88-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.2.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="a0e88-219">Wersja jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="a0e88-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="a0e88-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="a0e88-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="a0e88-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="a0e88-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="a0e88-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.1.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="a0e88-226">Wersja jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="a0e88-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="a0e88-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="a0e88-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="a0e88-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="a0e88-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="a0e88-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.1.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="a0e88-233">Wersja jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="a0e88-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="a0e88-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="a0e88-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="a0e88-237">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.SLIM.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="a0e88-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.SLIM.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="a0e88-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-3.0.0.SLIM.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="a0e88-240">Wersja jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="a0e88-241">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="a0e88-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="a0e88-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="a0e88-244">Wersja jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="a0e88-245">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="a0e88-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="a0e88-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="a0e88-248">Wersja jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="a0e88-249">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="a0e88-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="a0e88-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="a0e88-252">Wersja jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="a0e88-253">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="a0e88-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="a0e88-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="a0e88-256">Wersja jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="a0e88-257">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="a0e88-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="a0e88-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="a0e88-260">Wersja jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="a0e88-261">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="a0e88-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="a0e88-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="a0e88-264">Wersja jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="a0e88-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="a0e88-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="a0e88-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="a0e88-268">Wersja jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="a0e88-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="a0e88-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="a0e88-271">Wersja jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="a0e88-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="a0e88-273">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="a0e88-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="a0e88-275">Wersja jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="a0e88-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="a0e88-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="a0e88-278">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="a0e88-280">Wersja jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="a0e88-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="a0e88-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="a0e88-283">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="a0e88-285">Wersja jQuery pkt 2.0.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="a0e88-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="a0e88-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="a0e88-288">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="a0e88-290">Wersja jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="a0e88-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="a0e88-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="a0e88-293">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="a0e88-295">jQuery wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="a0e88-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="a0e88-297">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="a0e88-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="a0e88-300">Wersja jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="a0e88-301">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="a0e88-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="a0e88-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="a0e88-304">Wersja jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="a0e88-305">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="a0e88-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="a0e88-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="a0e88-308">Wersja jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="a0e88-309">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="a0e88-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="a0e88-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="a0e88-312">Wersja jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="a0e88-313">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="a0e88-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="a0e88-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="a0e88-316">Wersja jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="a0e88-317">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="a0e88-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="a0e88-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="a0e88-320">Wersja jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="a0e88-321">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="a0e88-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="a0e88-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="a0e88-324">Wersja jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="a0e88-325">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="a0e88-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="a0e88-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="a0e88-328">Wersja jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="a0e88-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="a0e88-330">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="a0e88-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="a0e88-332">Wersja jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="a0e88-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="a0e88-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="a0e88-335">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="a0e88-337">Wersja jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="a0e88-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="a0e88-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="a0e88-340">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="a0e88-342">Wersja jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="a0e88-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="a0e88-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="a0e88-345">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="a0e88-347">Wersja jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="a0e88-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="a0e88-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="a0e88-350">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="a0e88-352">Wersja jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="a0e88-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="a0e88-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="a0e88-355">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="a0e88-357">Wersja jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="a0e88-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="a0e88-359">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="a0e88-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="a0e88-362">Wersja jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="a0e88-363">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="a0e88-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="a0e88-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="a0e88-366">Wersja jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="a0e88-367">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="a0e88-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="a0e88-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="a0e88-370">Wersja jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="a0e88-371">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="a0e88-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="a0e88-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="a0e88-374">Wersja jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="a0e88-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="a0e88-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="a0e88-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="a0e88-378">Wersja jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="a0e88-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="a0e88-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="a0e88-381">Wersja jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="a0e88-382">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="a0e88-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="a0e88-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="a0e88-385">Wersja jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="a0e88-385">jQuery version 1.7</span></span>

- <span data-ttu-id="a0e88-386">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="a0e88-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="a0e88-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="a0e88-389">Wersja jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="a0e88-390">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="a0e88-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="a0e88-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="a0e88-393">Wersja jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="a0e88-394">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="a0e88-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="a0e88-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="a0e88-397">jQuery wersji 1.6.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="a0e88-398">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="a0e88-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="a0e88-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="a0e88-401">jQuery wersji 1.6.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="a0e88-402">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="a0e88-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="a0e88-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="a0e88-405">jQuery w wersji 1.6</span><span class="sxs-lookup"><span data-stu-id="a0e88-405">jQuery version 1.6</span></span>

- <span data-ttu-id="a0e88-406">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="a0e88-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="a0e88-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="a0e88-409">Wersja jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="a0e88-410">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="a0e88-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="a0e88-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="a0e88-413">Wersja jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="a0e88-414">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="a0e88-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="a0e88-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="a0e88-417">jQuery w wersji 1.5</span><span class="sxs-lookup"><span data-stu-id="a0e88-417">jQuery version 1.5</span></span>

- <span data-ttu-id="a0e88-418">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="a0e88-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="a0e88-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="a0e88-421">Wersja jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="a0e88-422">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="a0e88-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="a0e88-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="a0e88-425">Wersja jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="a0e88-426">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="a0e88-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="a0e88-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="a0e88-429">Wersja jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="a0e88-430">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="a0e88-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="a0e88-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="a0e88-433">Wersja jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="a0e88-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="a0e88-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="a0e88-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="a0e88-437">jQuery w wersji 1.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-437">jQuery version 1.4</span></span>

- <span data-ttu-id="a0e88-438">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="a0e88-439">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="a0e88-440">Wersja jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="a0e88-441">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="a0e88-442">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="a0e88-443">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="a0e88-444">http://AJAX.aspnetcdn.com/AJAX/jQuery/jquery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="a0e88-445">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="a0e88-446">Następujące wersje jQuery migracji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="a0e88-447">jQuery migracji wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="a0e88-448">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="a0e88-449">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="a0e88-450">jQuery migracji wersji 1.2.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="a0e88-451">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="a0e88-452">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="a0e88-453">jQuery migracji wersji 1.2.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="a0e88-454">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="a0e88-455">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="a0e88-456">jQuery migracji wersji 1.1.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="a0e88-457">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="a0e88-458">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="a0e88-459">jQuery migracji wersji 1.1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="a0e88-460">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="a0e88-461">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="a0e88-462">jQuery migracji w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="a0e88-463">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="a0e88-464">http://AJAX.aspnetcdn.com/AJAX/jquery.migrate/jquery-migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="a0e88-465">Dostępne wersje interfejsu użytkownika w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="a0e88-466">Następujące wersje biblioteki interfejsu użytkownika jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="a0e88-467">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="a0e88-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a0e88-468">1.12.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery 1.12.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-469">1.12.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery 1.12.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-470">1.11.4 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery 1.11.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-471">1.11.3 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery 1.11.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-472">1.11.2 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery 1.11.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-473">1.11.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery 1.11.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-474">1.11.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery 1.11.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-475">1.10.4 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery 1.10.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-476">1.10.3 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery 1.10.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-477">jQuery 1.10.2 interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="a0e88-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-478">1.10.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery 1.10.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-479">1.10.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery 1.10.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-480">1.9.2 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery 1.9.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-481">1.9.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery 1.9.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-482">1.9.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery 1.9.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-483">1.8.24 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery 1.8.24 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-484">1.8.23 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery 1.8.23 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-485">1.8.22 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery 1.8.22 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-486">1.8.21 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery 1.8.21 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-487">1.8.20 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery 1.8.20 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-488">1.8.19 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery 1.8.19 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-489">1.8.18 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery 1.8.18 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-490">1.8.17 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery 1.8.17 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-491">1.8.16 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery 1.8.16 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-492">1.8.15 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery 1.8.15 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-493">1.8.14 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery 1.8.14 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-494">1.8.13 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery 1.8.13 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-495">1.8.12 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery 1.8.12 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-496">1.8.11 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery 1.8.11 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-497">1.8.10 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-498">1.8.9 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery 1.8.9 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-499">1.8.8 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery 1.8.8 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-500">1.8.7 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery 1.8.7 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-501">1.8.6 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery 1.8.6 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-502">1.8.5 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery 1.8.5 interfejsu użytkownika")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="a0e88-503">Dostępne wersje weryfikacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="a0e88-504">Następujące wersje weryfikacji biblioteki jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="a0e88-505">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="a0e88-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a0e88-506">Sprawdź poprawność 1.17.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 1.17.0 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-507">Sprawdź poprawność 1.16.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery 1.16.0 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-508">Sprawdź poprawność 1.15.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 1.15.1 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-509">Sprawdź poprawność 1.15.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 1.15.0 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-510">Sprawdź poprawność 1.14.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 1.14.0 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-511">Sprawdź poprawność 1.13.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 1.13.1 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-512">Sprawdź poprawność 1.13.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 1.13.0 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-513">Sprawdź poprawność 1.12.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-514">Sprawdź poprawność 1.11.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-515">Sprawdź poprawność 1.11.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery 1.11.0 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-516">Sprawdź poprawność 1.10.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 sprawdzania poprawności")
- [<span data-ttu-id="a0e88-517">Sprawdź poprawność 1.9 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate wersji 1.9")
- [<span data-ttu-id="a0e88-518">Sprawdź poprawność 1.8.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "wersji jquery.validate 1.8.1")
- [<span data-ttu-id="a0e88-519">Sprawdź poprawność 1.8 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate wersji 1.8")
- [<span data-ttu-id="a0e88-520">Sprawdź poprawność 1.7 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate wersji 1.7")
- [<span data-ttu-id="a0e88-521">jQuery weryfikacji 1.6</span><span class="sxs-lookup"><span data-stu-id="a0e88-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery weryfikacji w wersji 1.6")
- [<span data-ttu-id="a0e88-522">Sprawdź poprawność 1.5.5 jQuery</span><span class="sxs-lookup"><span data-stu-id="a0e88-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery weryfikacji 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="a0e88-523">jQuery Mobile wersjach w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="a0e88-524">Następujące wersje biblioteki jQuery przenośnych znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="a0e88-525">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="a0e88-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a0e88-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="a0e88-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-532">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-537">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="a0e88-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-540">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="a0e88-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-541">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="a0e88-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="a0e88-542">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="a0e88-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 w sieci Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="a0e88-543">Dostępne wersje szablonów w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="a0e88-544">Następujące wersje dodatku szablony jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="a0e88-545">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="a0e88-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a0e88-546">jQuery szablonów w wersji Beta 1</span><span class="sxs-lookup"><span data-stu-id="a0e88-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery szablonów w wersji Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="a0e88-547">Dostępne wersje cyklu w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="a0e88-548">Następujące wersje dodatku cyklu jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="a0e88-549">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="a0e88-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a0e88-550">jQuery 2.99 cyklu</span><span class="sxs-lookup"><span data-stu-id="a0e88-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 cyklu")
- [<span data-ttu-id="a0e88-551">jQuery 2,94 cyklu</span><span class="sxs-lookup"><span data-stu-id="a0e88-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2,94 cyklu")
- [<span data-ttu-id="a0e88-552">jQuery 2,88 cyklu</span><span class="sxs-lookup"><span data-stu-id="a0e88-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 cyklu")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="a0e88-553">Dostępne wersje DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="a0e88-554">Następujące wersje dodatku DataTables jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="a0e88-555">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="a0e88-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a0e88-556">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="a0e88-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="a0e88-557">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="a0e88-558">jQuery DataTables pytanie 1.9.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables pytanie 1.9.4")
- [<span data-ttu-id="a0e88-559">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="a0e88-560">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="a0e88-561">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="a0e88-562">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="a0e88-563">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="a0e88-564">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="a0e88-565">Następujące wersje programu [Modernizr](http://www.modernizr.com "Modernizr") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="a0e88-566">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="a0e88-567">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="a0e88-568">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="a0e88-569">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="a0e88-570">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-1.7-Development-Only.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="a0e88-571">http://AJAX.aspnetcdn.com/AJAX/modernizr/modernizr-2.0.6-Development-Only.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="a0e88-572">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="a0e88-573">Następujące wersje programu [JSHint](http://www.jshint.com "JSHint") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="a0e88-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="a0e88-575">Wersje odcinania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="a0e88-576">Następujące wersje programu [Knockout](http://www.knockoutjs.com "Knockout") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="a0e88-577">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="a0e88-578">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="a0e88-579">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="a0e88-580">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="a0e88-581">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="a0e88-582">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="a0e88-583">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="a0e88-584">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="a0e88-585">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="a0e88-586">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="a0e88-587">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="a0e88-588">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="a0e88-589">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="a0e88-590">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="a0e88-591">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="a0e88-592">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="a0e88-593">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="a0e88-594">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="a0e88-595">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="a0e88-596">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="a0e88-597">Globalize wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="a0e88-598">Następujące wersje programu [Globalize](https://github.com/jquery/globalize "Globalize") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="a0e88-599">Globalize w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="a0e88-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="a0e88-601">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/node-main.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="a0e88-602">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="a0e88-603">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="a0e88-604">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="a0e88-605">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="a0e88-606">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="a0e88-607">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="a0e88-608">Globalize wersji 0.1.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="a0e88-609">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="a0e88-610">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="a0e88-611">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="a0e88-612">wszystkie kultur</span><span class="sxs-lookup"><span data-stu-id="a0e88-612">all cultures</span></span>
- <span data-ttu-id="a0e88-613">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.Culture. js {kultury code}</span><span class="sxs-lookup"><span data-stu-id="a0e88-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="a0e88-614">Zastąp "{Kod kultury}" z kodem żądaną kulturę, np. Microsoft globalize.culture.en GB.js== plików w sieci CDN == te biblioteki zostały przekazane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a0e88-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="a0e88-615">Odpowiadać wersji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="a0e88-616">Następujące wersje programu [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") odpowiedź znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="a0e88-617">Odpowiadać wersji 1.4.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="a0e88-618">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="a0e88-619">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="a0e88-620">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="a0e88-621">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.2/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="a0e88-622">Odpowiadać wersji 1.4.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="a0e88-623">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="a0e88-624">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="a0e88-625">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="a0e88-626">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.1/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="a0e88-627">Odpowiadać wersji 1.4.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="a0e88-628">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="a0e88-629">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="a0e88-630">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="a0e88-631">http://AJAX.aspnetcdn.com/AJAX/respond/1.4.0/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="a0e88-632">Odpowiadać wersji 1.3.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="a0e88-633">http://AJAX.aspnetcdn.com/AJAX/respond/1.3.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="a0e88-634">Odpowiadać wersji 1.2.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="a0e88-635">http://AJAX.aspnetcdn.com/AJAX/respond/1.2.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="a0e88-636">Wersje bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="a0e88-637">Następujące wersje programu [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="a0e88-638">Wersja bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="a0e88-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="a0e88-639">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-640">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-641">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-642">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-643">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-644">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-645">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-646">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-647">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-648">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-649">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-650">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a0e88-651">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a0e88-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="a0e88-652">Wersja bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="a0e88-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="a0e88-653">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-654">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-655">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-656">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-657">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-658">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-659">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-660">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-661">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-662">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-663">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-664">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a0e88-665">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a0e88-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="a0e88-666">Bootstrap wersji 3.3.5</span><span class="sxs-lookup"><span data-stu-id="a0e88-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="a0e88-667">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-668">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-669">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-670">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-671">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-672">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-673">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-674">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-675">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-676">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-677">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-678">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a0e88-679">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a0e88-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="a0e88-680">Wersja bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="a0e88-681">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-682">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-683">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-684">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-685">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-686">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-687">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-688">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-689">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-690">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-691">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-692">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a0e88-693">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a0e88-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="a0e88-694">Wersja bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="a0e88-695">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-696">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-697">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-698">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-699">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-700">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-701">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-702">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-703">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-704">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-705">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-706">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a0e88-707">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a0e88-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="a0e88-708">Wersja bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="a0e88-709">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-710">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-711">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-712">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-713">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-714">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-715">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-716">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-717">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-718">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-719">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-720">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="a0e88-721">Wersja bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="a0e88-722">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-723">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-724">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-725">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-726">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-727">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-728">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-729">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-730">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-731">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-732">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-733">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="a0e88-734">Wersja bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="a0e88-735">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-736">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-737">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-738">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-739">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-740">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-741">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-742">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-743">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-744">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-745">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-746">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="a0e88-747">Wersja bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="a0e88-748">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-749">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-750">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-751">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-752">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-753">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-754">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-755">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-756">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-757">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-758">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-759">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="a0e88-760">Wersja bootstrap 3.1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="a0e88-761">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-762">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-763">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-764">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a0e88-765">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-766">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-767">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a0e88-768">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-769">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-770">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-771">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-772">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="a0e88-773">Wersja bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="a0e88-774">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-775">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-776">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-777">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-778">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-779">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-780">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-781">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-782">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-783">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="a0e88-784">Wersja bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="a0e88-785">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-786">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-787">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-788">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-789">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-790">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-791">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-792">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-793">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-794">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="a0e88-795">Bootstrap wersji 3.0.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="a0e88-796">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-797">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-798">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-799">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-800">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-801">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-802">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-803">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-804">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-805">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="a0e88-806">Wersja bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="a0e88-807">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-808">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-809">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-810">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-811">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a0e88-812">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a0e88-813">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="a0e88-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a0e88-814">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a0e88-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a0e88-815">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a0e88-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a0e88-816">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="a0e88-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="a0e88-817">Wersja bootstrap 2.3.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="a0e88-818">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-819">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-820">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-821">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-822">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="a0e88-823">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap-responsive.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="a0e88-824">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="a0e88-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="a0e88-825">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="a0e88-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="a0e88-826">Wersja bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="a0e88-827">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="a0e88-828">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="a0e88-829">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="a0e88-830">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a0e88-831">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="a0e88-832">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap-responsive.min.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="a0e88-833">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="a0e88-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="a0e88-834">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="a0e88-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="a0e88-835">Wersje bootstrap TouchCarousel w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="a0e88-836">Następujące wersje programu [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel wersjach znajdują się w sieci CDN :</span><span class="sxs-lookup"><span data-stu-id="a0e88-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="a0e88-837">Bootstrap TouchCarousel wersji 0.8.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="a0e88-838">http://AJAX.aspnetcdn.com/AJAX/bootstrap-Touch-carousel/0.8.0/CSS/bootstrap-Touch-carousel.css</span><span class="sxs-lookup"><span data-stu-id="a0e88-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="a0e88-839">http://AJAX.aspnetcdn.com/AJAX/bootstrap-Touch-carousel/0.8.0/js/bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="a0e88-840">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="a0e88-841">Następujące wersje programu [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js wersjach znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="a0e88-842">Wersja hammer.js 2.0.4</span><span class="sxs-lookup"><span data-stu-id="a0e88-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="a0e88-843">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="a0e88-844">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="a0e88-845">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="a0e88-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="a0e88-846">Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="a0e88-847">Następujących wersji biblioteki ASP.NET Ajax znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0e88-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="a0e88-848">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="a0e88-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a0e88-849">Formularze sieci Web ASP.NET i Ajax wersji 4.5.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "formularzy sieci Web ASP.NET i Ajax 4.5.2")
- [<span data-ttu-id="a0e88-850">Formularze sieci Web ASP.NET i Ajax w wersji 4</span><span class="sxs-lookup"><span data-stu-id="a0e88-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "formularzy sieci Web ASP.NET i Ajax 4")
- [<span data-ttu-id="a0e88-851">ASP.NET Ajax w wersji 3.5</span><span class="sxs-lookup"><span data-stu-id="a0e88-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="a0e88-852">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="a0e88-853">Następujące pliki platformy ASP.NET MVC JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="a0e88-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="a0e88-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a0e88-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="a0e88-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="a0e88-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a0e88-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="a0e88-860">ASP.NET MVC W WERSJI 5.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="a0e88-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a0e88-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="a0e88-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="a0e88-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a0e88-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="a0e88-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="a0e88-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jquery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="a0e88-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jquery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="a0e88-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jquery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a0e88-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jquery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="a0e88-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="a0e88-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="a0e88-873">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="a0e88-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="a0e88-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="a0e88-876">ASP.NET MVC W WERSJI 1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="a0e88-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="a0e88-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="a0e88-879">Wersje biblioteki SignalR platformy ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="a0e88-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="a0e88-880">Następujące pliki ASP.NET SignalR JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="a0e88-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="a0e88-881">Biblioteka SignalR platformy ASP.NET 2.2.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="a0e88-882">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="a0e88-883">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="a0e88-884">Biblioteka SignalR platformy ASP.NET 2.2.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="a0e88-885">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="a0e88-886">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="a0e88-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="a0e88-888">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="a0e88-889">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="a0e88-890">Biblioteka SignalR platformy ASP.NET 2.1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="a0e88-891">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="a0e88-892">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="a0e88-893">Biblioteka SignalR platformy ASP.NET 2.0.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="a0e88-894">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="a0e88-895">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="a0e88-896">Biblioteka SignalR platformy ASP.NET pkt 2.0.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="a0e88-897">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="a0e88-898">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="a0e88-899">Biblioteka SignalR platformy ASP.NET 2.0.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="a0e88-900">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="a0e88-901">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="a0e88-902">Biblioteka SignalR platformy ASP.NET 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="a0e88-903">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="a0e88-904">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="a0e88-905">Biblioteka SignalR platformy ASP.NET 1.1.3</span><span class="sxs-lookup"><span data-stu-id="a0e88-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="a0e88-906">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="a0e88-907">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="a0e88-908">Biblioteka SignalR platformy ASP.NET 1.1.2</span><span class="sxs-lookup"><span data-stu-id="a0e88-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="a0e88-909">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="a0e88-910">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="a0e88-911">Biblioteka SignalR platformy ASP.NET 1.1.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="a0e88-912">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="a0e88-913">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="a0e88-914">Biblioteka SignalR platformy ASP.NET 1.1.0</span><span class="sxs-lookup"><span data-stu-id="a0e88-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="a0e88-915">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="a0e88-916">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="a0e88-917">Biblioteka SignalR platformy ASP.NET 1.0.1</span><span class="sxs-lookup"><span data-stu-id="a0e88-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="a0e88-918">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="a0e88-919">http://AJAX.aspnetcdn.com/AJAX/signalr/jquery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="a0e88-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="a0e88-920">Aby dowiedzieć się, warunki użytkowania CDN, zobacz [Microsoft Ajax CDN warunkom użytkowania](https://www.asp.net/terms-of-use "Microsoft Ajax CDN warunkom użytkowania").</span><span class="sxs-lookup"><span data-stu-id="a0e88-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
