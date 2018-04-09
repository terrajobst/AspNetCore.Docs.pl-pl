---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Dokumentacja firmy Microsoft
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: bc5f40746ad6b1ed8a74bcb75def9ff8f08fb789
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="7070f-102">Microsoft Ajax sieci dostarczania zawartości</span><span class="sxs-lookup"><span data-stu-id="7070f-102">Microsoft Ajax Content Delivery Network</span></span>
====================
> [!WARNING]
> <span data-ttu-id="7070f-103">Aplikacje produkcyjne nie powinna przyjmować twardych zależności na zasoby sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="7070f-104">Aplikacje powinny testu dla zasobów sieci CDN w warstwie, do których odwołuje się i użyj rezerwowy zasobów, jeśli element CDN jest niedostępny.</span><span class="sxs-lookup"><span data-stu-id="7070f-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span> 
>
> <span data-ttu-id="7070f-105">Usługa Microsoft Ajax CDN ma umowy dotyczącej poziomu usług niż przy użyciu usługi Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="7070f-106">Użyj [ten problem GitHub](https://github.com/aspnet/Docs/issues/5832) na zgłaszanie problemów z usługi Microsoft Ajax CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-106">Use [this GitHub issue](https://github.com/aspnet/Docs/issues/5832) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="7070f-107">Spis treści</span><span class="sxs-lookup"><span data-stu-id="7070f-107">Table of Contents</span></span>

<span data-ttu-id="7070f-108">**[AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="7070f-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="7070f-109">**[Obsługa .vsdoc programu Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="7070f-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="7070f-110">**[Za pomocą kodu ASP.NET Ajax z sieci CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="7070f-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="7070f-111">**[Przy użyciu jQuery z sieci CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="7070f-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="7070f-112">**[Przy użyciu interfejsu użytkownika z sieci CDN jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="7070f-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="7070f-113">**[Pliki innych firm w sieci CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="7070f-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="7070f-114">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="7070f-115">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="7070f-116">Dostępne wersje interfejsu użytkownika w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="7070f-117">Dostępne wersje weryfikacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="7070f-118">jQuery Mobile wersjach w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="7070f-119">Dostępne wersje szablonów w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="7070f-120">Dostępne wersje cyklu w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="7070f-121">Dostępne wersje DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="7070f-122">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="7070f-123">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="7070f-124">Wersje odcinania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="7070f-125">Globalize wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="7070f-126">Odpowiadać wersji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="7070f-127">Wersje bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="7070f-128">Wersje bootstrap TouchCarousel w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="7070f-129">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="7070f-130">Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="7070f-131">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="7070f-132">Wersje biblioteki SignalR platformy ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="7070f-133">Microsoft Ajax sieci dostarczania zawartości (CDN) obsługuje bibliotek JavaScript popularnych innych firm, np. jQuery i umożliwia łatwe dodanie ich do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7070f-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="7070f-134">Na przykład możesz rozpocząć używać jQuery, który znajduje się w tej sieci CDN po prostu dodając &lt;skryptu&gt; tag do strony, która wskazuje ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="7070f-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="7070f-135">Dzięki wykorzystaniu sieci CDN może znacznie poprawić wydajność aplikacji Ajax.</span><span class="sxs-lookup"><span data-stu-id="7070f-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="7070f-136">Zawartość CDN są buforowane na serwerów na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="7070f-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="7070f-137">Ponadto CDN umożliwia ponowne użycie innej buforowane pliki JavaScript dla witryn sieci web, które znajdują się w różnych domenach w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="7070f-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="7070f-138">CDN obsługuje SSL (HTTPS), w razie potrzeby do obsługi strony sieci web przy użyciu protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="7070f-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="7070f-139">Usługa CDN obsługuje następujące biblioteki skryptu innych firm, które zostały przekazane i są, licencjonowane przez właścicieli tych biblioteki:</span><span class="sxs-lookup"><span data-stu-id="7070f-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="7070f-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="7070f-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="7070f-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="7070f-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="7070f-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="7070f-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="7070f-143">jQuery sprawdzania poprawności (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="7070f-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="7070f-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="7070f-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="7070f-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="7070f-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="7070f-146">Usługa Microsoft Ajax CDN zawiera również następujące biblioteki, które zostały przekazane przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7070f-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="7070f-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="7070f-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="7070f-148">Pliki języka JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7070f-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="7070f-149">Pliki ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="7070f-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="7070f-150">Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="7070f-151">Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="7070f-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="7070f-152">Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="7070f-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="7070f-153">Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="7070f-154">Jeśli chcesz przesłać biblioteki JavaScript i biblioteki jest jednym z najwyższym bibliotek JavaScript (wymienione w http://trends.builtwith.com) lub rozszerzenia/wtyczek do tych bibliotek, które są () popularnych; lub (b) przydatne do użycia na platformie ASP.NET, a następnie skontaktuj się z pomocą AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="7070f-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="7070f-155">AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="7070f-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="7070f-156">CDN używany do używania nazwy domeny microsoft.com i została zmieniona do używania nazwy domeny aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="7070f-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="7070f-157">Ta zmiana została wprowadzona w celu zwiększenia wydajności, ponieważ podczas przeglądarką domenie microsoft.com, do których odwołuje się on będzie wysyłać żadnych plików cookie z tej domeny przez sieć z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="7070f-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="7070f-158">Zmieniając nazwę domeny inne niż microsoft.com można zwiększyć wydajność przez tyle 25%.</span><span class="sxs-lookup"><span data-stu-id="7070f-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="7070f-159">Uwaga ajax.microsoft.com będą nadal działać, ale ajax.aspnetcdn.com jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="7070f-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="7070f-160">Stary Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="7070f-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="7070f-161">Nowy Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="7070f-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="7070f-162">Obsługa .vsdoc programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7070f-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="7070f-163">Do korzystania z plików .vsdoc prawidłowo z programu Visual Studio 2008, należy się upewnić, że masz VS 2008 z dodatkiem SP1 należy zainstalować i zainstalowana poprawka vsdoc plików.</span><span class="sxs-lookup"><span data-stu-id="7070f-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="7070f-164">Możesz uzyskać je w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="7070f-164">You can get these from here:</span></span>

- [<span data-ttu-id="7070f-165">Pobierz program Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="7070f-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "pobierania programu Visual Studio 2008 z dodatkiem SP1")
- [<span data-ttu-id="7070f-166">Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="7070f-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "pobrać poprawkę .vsdoc dla programu Visual Studio 2008 z dodatkiem SP1")

<span data-ttu-id="7070f-167">Program Visual Studio 2010 obsługuje pliki .vsdoc bez żadnych dodatkowych poprawek.</span><span class="sxs-lookup"><span data-stu-id="7070f-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="7070f-168">Za pomocą kodu ASP.NET Ajax z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="7070f-169">Podczas korzystania z programu ASP.NET 4, można przekierowywać wszystkie żądania ASP.NET framework skryptów do sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="7070f-170">Pobieranie skryptów z sieci CDN zamiast serwera sieci web lokalnego znacznie może poprawić wydajność publicznych witryn sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7070f-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="7070f-171">Za pomocą właściwości ScriptManager EnableCDN Przekieruj żądania skryptu framework ASP.NET do programu Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="7070f-172">Przy użyciu jQuery z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="7070f-173">Za pomocą skryptów jQuery hostowanych w sieci CDN w aplikacji sieci Web, dodając następujący element skryptu do strony:</span><span class="sxs-lookup"><span data-stu-id="7070f-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="7070f-174">Usługa CDN zawiera również wersję zminimalizowany skryptu jQuery, którą można uzyskać za pomocą następującego elementu:</span><span class="sxs-lookup"><span data-stu-id="7070f-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="7070f-175">Aby zezwolić na stronę do powrotu do ładowania jQuery z ścieżkę lokalną na własne witryny sieci Web, jeśli sieć CDN jest niedostępny, Dodaj następujący element natychmiast po elemencie odwołujące się do sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="7070f-176">Następujące przykładowe strony korzysta z wersji CDN biblioteki jQuery (z powrotu do lokalnej kopii) do wyświetlenia zawartości elementu div, gdy przycisk zostanie kliknięty.</span><span class="sxs-lookup"><span data-stu-id="7070f-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="7070f-177">Można dowiedzieć się więcej o jQuery i pobrać kopię lokalną jQuery odwiedzając [jQuery](http://jquery.com/) witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7070f-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="7070f-178">Przy użyciu interfejsu użytkownika z sieci CDN jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="7070f-179">Usługa CDN obsługuje również biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="7070f-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="7070f-180">Biblioteka interfejsu użytkownika jQuery zawiera bogaty zestaw elementów widget i efekty, których można używać w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7070f-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="7070f-181">Na przykład następująca strona przedstawiono, jak jQuery selektora daty interfejsu użytkownika w kontekście aplikacji formularzy sieci Web programu ASP.NET można użyć do wyświetlenia wyskakującego kalendarza:</span><span class="sxs-lookup"><span data-stu-id="7070f-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="7070f-182">Gdy fokus zostanie przeniesiony do pola tekstowego przy użyciu klawiatury, zostanie wyświetlony kalendarz:</span><span class="sxs-lookup"><span data-stu-id="7070f-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Utworzone za pomocą selektora daty kalendarza podręcznego](overview/_static/image1.png)

<span data-ttu-id="7070f-184">Zwróć uwagę, że musi zawierać trzy pliki z sieci CDN w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="7070f-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="7070f-185">Biblioteki jQuery &mdash; biblioteki interfejsu użytkownika jQuery jest zależna od biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="7070f-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="7070f-186">Należy dodać biblioteki jQuery do strony przed dodaniem biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="7070f-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="7070f-187">Biblioteka interfejsu użytkownika jQuery &mdash; biblioteki interfejsu użytkownika jQuery zawiera elementy widget, takie jak używane na stronie powyżej widżetu selektora daty i efektów interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="7070f-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="7070f-188">Motyw interfejsu użytkownika jQuery &mdash; jQuery interfejsu użytkownika obsługuje różne kompozycje.</span><span class="sxs-lookup"><span data-stu-id="7070f-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="7070f-189">Strona sieci zawiera łącze do pliku CSS do zaimportowania Redmond motywu.</span><span class="sxs-lookup"><span data-stu-id="7070f-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="7070f-190">Wszystkie kompozycje interfejsu użytkownika jQuery standardowe znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="7070f-191">[Odwiedź stronę tego](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN") do wyświetlania miniatur dla każdego motywu.</span><span class="sxs-lookup"><span data-stu-id="7070f-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="7070f-192">Aby dowiedzieć się więcej na temat biblioteki interfejsu użytkownika jQuery, można znaleźć w oficjalnym [witryny sieci Web interfejsu użytkownika jQuery](http://jQueryUI.com "witryny sieci Web interfejsu użytkownika jQuery").</span><span class="sxs-lookup"><span data-stu-id="7070f-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="7070f-193">Pliki innych firm w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="7070f-194">Usługa CDN obsługuje niektóre najbardziej popularnych bibliotek JavaScript innych firm.</span><span class="sxs-lookup"><span data-stu-id="7070f-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="7070f-195">Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="7070f-196">Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="7070f-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="7070f-197">Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="7070f-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="7070f-198">Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="7070f-199">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="7070f-200">Następujące wersje jQuery znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="7070f-201">Wersja jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="7070f-201">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="7070f-202">Wersja jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="7070f-202">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="7070f-203">Wersja jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="7070f-203">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="7070f-204">Wersja jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="7070f-204">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="7070f-205">Wersja jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-205">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="7070f-206">Wersja jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7070f-206">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="7070f-207">Wersja jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="7070f-207">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="7070f-208">Wersja jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="7070f-208">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="7070f-209">Wersja jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="7070f-209">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="7070f-210">Wersja jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="7070f-210">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="7070f-211">Wersja jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="7070f-211">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="7070f-212">Wersja jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="7070f-212">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="7070f-213">Wersja jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="7070f-213">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="7070f-214">Wersja jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="7070f-214">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="7070f-215">Wersja jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="7070f-215">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="7070f-216">Wersja jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-216">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="7070f-217">Wersja jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="7070f-217">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="7070f-218">Wersja jQuery pkt 2.0.2</span><span class="sxs-lookup"><span data-stu-id="7070f-218">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="7070f-219">Wersja jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="7070f-219">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="7070f-220">jQuery wersji 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7070f-220">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="7070f-221">Wersja jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="7070f-221">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="7070f-222">Wersja jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="7070f-222">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="7070f-223">Wersja jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="7070f-223">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="7070f-224">Wersja jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="7070f-224">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="7070f-225">Wersja jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="7070f-225">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="7070f-226">Wersja jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="7070f-226">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="7070f-227">Wersja jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="7070f-227">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="7070f-228">Wersja jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="7070f-228">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="7070f-229">Wersja jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="7070f-229">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="7070f-230">Wersja jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="7070f-230">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="7070f-231">Wersja jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="7070f-231">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="7070f-232">Wersja jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="7070f-232">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="7070f-233">Wersja jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="7070f-233">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="7070f-234">Wersja jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="7070f-234">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="7070f-235">Wersja jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="7070f-235">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="7070f-236">Wersja jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="7070f-236">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="7070f-237">Wersja jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="7070f-237">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="7070f-238">Wersja jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="7070f-238">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="7070f-239">Wersja jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="7070f-239">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="7070f-240">Wersja jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="7070f-240">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="7070f-241">Wersja jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="7070f-241">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="7070f-242">Wersja jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="7070f-242">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="7070f-243">Wersja jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="7070f-243">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="7070f-244">jQuery wersji 1.6.2</span><span class="sxs-lookup"><span data-stu-id="7070f-244">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="7070f-245">jQuery wersji 1.6.1</span><span class="sxs-lookup"><span data-stu-id="7070f-245">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="7070f-246">jQuery w wersji 1.6</span><span class="sxs-lookup"><span data-stu-id="7070f-246">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="7070f-247">Wersja jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="7070f-247">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="7070f-248">Wersja jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="7070f-248">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="7070f-249">jQuery w wersji 1.5</span><span class="sxs-lookup"><span data-stu-id="7070f-249">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="7070f-250">Wersja jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="7070f-250">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="7070f-251">Wersja jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="7070f-251">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="7070f-252">Wersja jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="7070f-252">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="7070f-253">Wersja jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="7070f-253">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="7070f-254">jQuery w wersji 1.4</span><span class="sxs-lookup"><span data-stu-id="7070f-254">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="7070f-255">Wersja jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="7070f-255">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="7070f-256">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-256">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="7070f-257">Następujące wersje jQuery migracji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-257">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="7070f-258">jQuery migracji wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7070f-258">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="7070f-259">jQuery migracji wersji 1.2.1</span><span class="sxs-lookup"><span data-stu-id="7070f-259">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="7070f-260">jQuery migracji wersji 1.2.0</span><span class="sxs-lookup"><span data-stu-id="7070f-260">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="7070f-261">jQuery migracji wersji 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7070f-261">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="7070f-262">jQuery migracji wersji 1.1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-262">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="7070f-263">jQuery migracji w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="7070f-263">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="7070f-264">Dostępne wersje interfejsu użytkownika w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-264">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="7070f-265">Następujące wersje biblioteki interfejsu użytkownika jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-265">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="7070f-266">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="7070f-266">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7070f-267">1.12.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-267">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery 1.12.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-268">1.12.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-268">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery 1.12.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-269">1.11.4 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-269">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery 1.11.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-270">1.11.3 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-270">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery 1.11.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-271">1.11.2 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-271">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery 1.11.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-272">1.11.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-272">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery 1.11.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-273">1.11.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-273">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery 1.11.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-274">1.10.4 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-274">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery 1.10.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-275">1.10.3 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-275">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery 1.10.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-276">jQuery 1.10.2 interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="7070f-276">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-277">1.10.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-277">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery 1.10.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-278">1.10.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-278">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery 1.10.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-279">1.9.2 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-279">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery 1.9.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-280">1.9.1 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-280">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery 1.9.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-281">1.9.0 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-281">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery 1.9.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-282">1.8.24 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-282">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery 1.8.24 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-283">1.8.23 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-283">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery 1.8.23 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-284">1.8.22 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-284">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery 1.8.22 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-285">1.8.21 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-285">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery 1.8.21 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-286">1.8.20 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-286">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery 1.8.20 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-287">1.8.19 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-287">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery 1.8.19 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-288">1.8.18 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-288">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery 1.8.18 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-289">1.8.17 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-289">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery 1.8.17 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-290">1.8.16 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-290">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery 1.8.16 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-291">1.8.15 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-291">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery 1.8.15 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-292">1.8.14 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-292">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery 1.8.14 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-293">1.8.13 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-293">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery 1.8.13 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-294">1.8.12 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-294">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery 1.8.12 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-295">1.8.11 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-295">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery 1.8.11 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-296">1.8.10 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-296">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-297">1.8.9 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-297">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery 1.8.9 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-298">1.8.8 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-298">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery 1.8.8 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-299">1.8.7 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-299">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery 1.8.7 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-300">1.8.6 interfejsu użytkownika jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-300">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery 1.8.6 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-301">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="7070f-301">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="7070f-302">Dostępne wersje weryfikacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-302">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="7070f-303">Następujące wersje weryfikacji biblioteki jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-303">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="7070f-304">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="7070f-304">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7070f-305">Sprawdź poprawność 1.17.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-305">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 1.17.0 sprawdzania poprawności")
- [<span data-ttu-id="7070f-306">Sprawdź poprawność 1.16.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-306">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery 1.16.0 sprawdzania poprawności")
- [<span data-ttu-id="7070f-307">Sprawdź poprawność 1.15.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-307">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 1.15.1 sprawdzania poprawności")
- [<span data-ttu-id="7070f-308">Sprawdź poprawność 1.15.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-308">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 1.15.0 sprawdzania poprawności")
- [<span data-ttu-id="7070f-309">Sprawdź poprawność 1.14.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-309">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 1.14.0 sprawdzania poprawności")
- [<span data-ttu-id="7070f-310">Sprawdź poprawność 1.13.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-310">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 1.13.1 sprawdzania poprawności")
- [<span data-ttu-id="7070f-311">Sprawdź poprawność 1.13.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-311">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 1.13.0 sprawdzania poprawności")
- [<span data-ttu-id="7070f-312">Sprawdź poprawność 1.12.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-312">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 sprawdzania poprawności")
- [<span data-ttu-id="7070f-313">Sprawdź poprawność 1.11.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-313">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 sprawdzania poprawności")
- [<span data-ttu-id="7070f-314">Sprawdź poprawność 1.11.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-314">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery 1.11.0 sprawdzania poprawności")
- [<span data-ttu-id="7070f-315">Sprawdź poprawność 1.10.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-315">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 sprawdzania poprawności")
- [<span data-ttu-id="7070f-316">Sprawdź poprawność 1.9 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-316">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate wersji 1.9")
- [<span data-ttu-id="7070f-317">Sprawdź poprawność 1.8.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-317">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "wersji jquery.validate 1.8.1")
- [<span data-ttu-id="7070f-318">Sprawdź poprawność 1.8 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-318">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate wersji 1.8")
- [<span data-ttu-id="7070f-319">Sprawdź poprawność 1.7 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-319">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate wersji 1.7")
- [<span data-ttu-id="7070f-320">jQuery weryfikacji 1.6</span><span class="sxs-lookup"><span data-stu-id="7070f-320">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery weryfikacji w wersji 1.6")
- [<span data-ttu-id="7070f-321">Sprawdź poprawność 1.5.5 jQuery</span><span class="sxs-lookup"><span data-stu-id="7070f-321">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery weryfikacji 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="7070f-322">jQuery Mobile wersjach w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-322">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="7070f-323">Następujące wersje biblioteki jQuery przenośnych znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-323">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="7070f-324">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="7070f-324">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7070f-325">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="7070f-325">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-326">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="7070f-326">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-327">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="7070f-327">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-328">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="7070f-328">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-329">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="7070f-329">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-330">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="7070f-330">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-331">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="7070f-331">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-332">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="7070f-332">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-333">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="7070f-333">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-334">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7070f-334">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-335">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-335">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-336">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="7070f-336">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-337">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="7070f-337">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-338">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-338">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-339">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="7070f-339">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-340">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="7070f-340">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 w sieci Microsoft Ajax CDN")
- [<span data-ttu-id="7070f-341">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="7070f-341">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 w sieci Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="7070f-342">Dostępne wersje szablonów w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-342">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="7070f-343">Następujące wersje dodatku szablony jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-343">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="7070f-344">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="7070f-344">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7070f-345">jQuery szablonów w wersji Beta 1</span><span class="sxs-lookup"><span data-stu-id="7070f-345">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery szablonów w wersji Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="7070f-346">Dostępne wersje cyklu w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-346">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="7070f-347">Następujące wersje dodatku cyklu jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-347">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="7070f-348">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="7070f-348">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7070f-349">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="7070f-349">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="7070f-350">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="7070f-350">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="7070f-351">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="7070f-351">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="7070f-352">Dostępne wersje DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-352">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="7070f-353">Następujące wersje dodatku DataTables jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-353">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="7070f-354">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="7070f-354">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7070f-355">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="7070f-355">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="7070f-356">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="7070f-356">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="7070f-357">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="7070f-357">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="7070f-358">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="7070f-358">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="7070f-359">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="7070f-359">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="7070f-360">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="7070f-360">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="7070f-361">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="7070f-361">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="7070f-362">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="7070f-362">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="7070f-363">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-363">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="7070f-364">Następujące wersje programu [Modernizr](http://www.modernizr.com "Modernizr") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-364">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="7070f-365">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-365">JSHint Releases on the CDN</span></span>

<span data-ttu-id="7070f-366">Następujące wersje programu [JSHint](http://www.jshint.com "JSHint") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-366">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="7070f-367">Wersje odcinania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-367">Knockout Releases on the CDN</span></span>

<span data-ttu-id="7070f-368">Następujące wersje programu [Knockout](http://www.knockoutjs.com "Knockout") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-368">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="7070f-369">Globalize wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-369">Globalize Releases on the CDN</span></span>

<span data-ttu-id="7070f-370">Następujące wersje programu [Globalize](https://github.com/jquery/globalize "Globalize") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-370">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="7070f-371">Globalize w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="7070f-371">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="7070f-372">Globalize wersji 0.1.1</span><span class="sxs-lookup"><span data-stu-id="7070f-372">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="7070f-373">wszystkie kultur</span><span class="sxs-lookup"><span data-stu-id="7070f-373">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="7070f-374">Zastąp "{Kod kultury}" z kodem żądaną kulturę, np. Microsoft globalize.culture.en GB.js== plików w sieci CDN == te biblioteki zostały przekazane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7070f-374">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="7070f-375">Odpowiadać wersji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-375">Respond Releases on the CDN</span></span>

<span data-ttu-id="7070f-376">Następujące wersje programu [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") odpowiedź znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-376">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="7070f-377">Odpowiadać wersji 1.4.2</span><span class="sxs-lookup"><span data-stu-id="7070f-377">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="7070f-378">Odpowiadać wersji 1.4.1</span><span class="sxs-lookup"><span data-stu-id="7070f-378">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="7070f-379">Odpowiadać wersji 1.4.0</span><span class="sxs-lookup"><span data-stu-id="7070f-379">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="7070f-380">Odpowiadać wersji 1.3.0</span><span class="sxs-lookup"><span data-stu-id="7070f-380">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="7070f-381">Odpowiadać wersji 1.2.0</span><span class="sxs-lookup"><span data-stu-id="7070f-381">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="7070f-382">Wersje bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-382">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="7070f-383">Następujące wersje programu [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-383">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="7070f-384">Wersja bootstrap 4.0.0</span><span class="sxs-lookup"><span data-stu-id="7070f-384">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="7070f-385">Wersja bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="7070f-385">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="7070f-386">Wersja bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="7070f-386">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="7070f-387">Bootstrap wersji 3.3.5</span><span class="sxs-lookup"><span data-stu-id="7070f-387">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="7070f-388">Wersja bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="7070f-388">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="7070f-389">Wersja bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="7070f-389">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="7070f-390">Wersja bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="7070f-390">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="7070f-391">Wersja bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="7070f-391">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="7070f-392">Wersja bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="7070f-392">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="7070f-393">Wersja bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="7070f-393">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="7070f-394">Wersja bootstrap 3.1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-394">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="7070f-395">Wersja bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="7070f-395">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="7070f-396">Wersja bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="7070f-396">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="7070f-397">Bootstrap wersji 3.0.1</span><span class="sxs-lookup"><span data-stu-id="7070f-397">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="7070f-398">Wersja bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7070f-398">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="7070f-399">Wersja bootstrap 2.3.2</span><span class="sxs-lookup"><span data-stu-id="7070f-399">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="7070f-400">Wersja bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="7070f-400">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="7070f-401">Wersje bootstrap TouchCarousel w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-401">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="7070f-402">Następujące wersje programu [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") Bootstrap TouchCarousel wersjach znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-402">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="7070f-403">Bootstrap TouchCarousel wersji 0.8.0</span><span class="sxs-lookup"><span data-stu-id="7070f-403">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="7070f-404">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-404">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="7070f-405">Następujące wersje programu [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js wersjach znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-405">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="7070f-406">Wersja hammer.js 2.0.4</span><span class="sxs-lookup"><span data-stu-id="7070f-406">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="7070f-407">Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-407">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="7070f-408">Następujących wersji biblioteki ASP.NET Ajax znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="7070f-408">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="7070f-409">Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.</span><span class="sxs-lookup"><span data-stu-id="7070f-409">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7070f-410">Formularze sieci Web ASP.NET i Ajax wersji 4.5.2</span><span class="sxs-lookup"><span data-stu-id="7070f-410">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "formularzy sieci Web ASP.NET i Ajax 4.5.2")
- [<span data-ttu-id="7070f-411">Formularze sieci Web ASP.NET i Ajax w wersji 4</span><span class="sxs-lookup"><span data-stu-id="7070f-411">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "formularzy sieci Web ASP.NET i Ajax 4")
- [<span data-ttu-id="7070f-412">ASP.NET Ajax w wersji 3.5</span><span class="sxs-lookup"><span data-stu-id="7070f-412">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="7070f-413">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-413">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="7070f-414">Następujące pliki platformy ASP.NET MVC JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-414">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="7070f-415">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="7070f-415">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="7070f-416">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="7070f-416">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="7070f-417">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="7070f-417">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="7070f-418">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="7070f-418">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="7070f-419">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="7070f-419">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="7070f-420">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="7070f-420">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="7070f-421">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-421">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="7070f-422">Wersje biblioteki SignalR platformy ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="7070f-422">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="7070f-423">Następujące pliki ASP.NET SignalR JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="7070f-423">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="7070f-424">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="7070f-424">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="7070f-425">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="7070f-425">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="7070f-426">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="7070f-426">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="7070f-427">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-427">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="7070f-428">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="7070f-428">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="7070f-429">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="7070f-429">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="7070f-430">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="7070f-430">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="7070f-431">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7070f-431">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="7070f-432">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="7070f-432">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="7070f-433">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="7070f-433">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="7070f-434">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7070f-434">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="7070f-435">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="7070f-435">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="7070f-436">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="7070f-436">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="7070f-437">Aby dowiedzieć się, warunki użytkowania CDN, zobacz [Microsoft Ajax CDN warunkom użytkowania](https://www.asp.net/terms-of-use "Microsoft Ajax CDN warunkom użytkowania").</span><span class="sxs-lookup"><span data-stu-id="7070f-437">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
