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
<a name="microsoft-ajax-content-delivery-network"></a>Microsoft Ajax sieci dostarczania zawartości
====================
Uwaga: Microsoft Ajax CDN ma umowy dotyczącej poziomu usług niż przy użyciu usługi Azure CDN.

## <a name="table-of-contents"></a>Spis treści

**[AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**  
**[Obsługa .vsdoc programu Visual Studio](#Visual_Studio_vsdoc_Support_19)**  
**[Za pomocą kodu ASP.NET Ajax z sieci CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**  
**[Przy użyciu jQuery z sieci CDN](#Using_jQuery_from_the_CDN_21)**  
**[Przy użyciu interfejsu użytkownika z sieci CDN jQuery](#Using_jQuery_UI_from_the_CDN_22)**  
**[Pliki innych firm w sieci CDN](#Third-Party_Files_on_the_CDN_23)**  
  
 [Wersje dostępne w sieci CDN](#jQuery_Releases_on_the_CDN_0)  
 [Wersje jQuery migracji w sieci CDN](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [Dostępne wersje interfejsu użytkownika w sieci CDN](#jQuery_UI_Releases_on_the_CDN_2)  
 [Dostępne wersje weryfikacji w sieci CDN](#jQuery_Validation_Releases_on_the_CDN_3)  
 [jQuery Mobile wersjach w sieci CDN](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [Dostępne wersje szablonów w sieci CDN](#jQuery_Templates_Releases_on_the_CDN_5)  
 [Dostępne wersje cyklu w sieci CDN](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [Dostępne wersje DataTables w sieci CDN](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [Wersje Modernizr w sieci CDN](#Modernizr_Releases_on_the_CDN_8)  
 [Wersje JSHint w sieci CDN](#JSHint_Releases_on_the_CDN_10)  
 [Wersje odcinania w sieci CDN](#Knockout_Releases_on_the_CDN_11)  
 [Globalize wersje w sieci CDN](#Globalize_Releases_on_the_CDN_12)  
 [Odpowiadać wersji w sieci CDN](#Respond_Releases_on_the_CDN_13)  
 [Wersje bootstrap w sieci CDN](#Bootstrap_Releases_on_the_CDN_14)  
 [Wersje bootstrap TouchCarousel w sieci CDN](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [Wersje hammer.js w sieci CDN](#Hammerjs_Releases_on_the_CDN_19)  
 [Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [ASP.NET MVC udostępnia w sieci CDN](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [Wersje biblioteki SignalR platformy ASP.NET w sieci CDN](#ASPNET_SignalR_Releases_on_the_CDN_17)

Microsoft Ajax sieci dostarczania zawartości (CDN) obsługuje bibliotek JavaScript popularnych innych firm, np. jQuery i umożliwia łatwe dodanie ich do aplikacji sieci Web. Na przykład możesz rozpocząć używać jQuery, który znajduje się w tej sieci CDN po prostu dodając &lt;skryptu&gt; tag do strony, która wskazuje ajax.aspnetcdn.com.

Dzięki wykorzystaniu sieci CDN może znacznie poprawić wydajność aplikacji Ajax. Zawartość CDN są buforowane na serwerów na całym świecie. Ponadto CDN umożliwia ponowne użycie innej buforowane pliki JavaScript dla witryn sieci web, które znajdują się w różnych domenach w przeglądarkach.

CDN obsługuje SSL (HTTPS), w razie potrzeby do obsługi strony sieci web przy użyciu protokołu SSL.

Usługa CDN obsługuje następujące biblioteki skryptu innych firm, które zostały przekazane i są, licencjonowane przez właścicieli tych biblioteki:

- jQuery (www.jquery.com)
- jQuery UI (www.jqueryui.com)
- jQuery Mobile (www.jquerymobile.com)
- jQuery sprawdzania poprawności (www.jquery.com)
- jQuery Cycle (www.malsup.com/jquery/cycle/)
- jQuery DataTables (http://datatables.net/)

Usługa Microsoft Ajax CDN zawiera również następujące biblioteki, które zostały przekazane przez firmę Microsoft:

- ASP.NET Ajax
- Pliki języka JavaScript ASP.NET MVC
- Pliki ASP.NET SignalR JavaScript

Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN. Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie. Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich. Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.

Jeśli chcesz przesłać biblioteki JavaScript i biblioteki jest jednym z najwyższym bibliotek JavaScript (wymienione w http://trends.builtwith.com) lub rozszerzenia/wtyczek do tych bibliotek, które są () popularnych; lub (b) przydatne do użycia na platformie ASP.NET, a następnie skontaktuj się z pomocą AjaxCDNSubmission@Microsoft.com.

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a>AJAX.microsoft.com zmieniona na ajax.aspnetcdn.com

CDN używany do używania nazwy domeny microsoft.com i została zmieniona do używania nazwy domeny aspnetcdn.com. Ta zmiana została wprowadzona w celu zwiększenia wydajności, ponieważ podczas przeglądarką domenie microsoft.com, do których odwołuje się on będzie wysyłać żadnych plików cookie z tej domeny przez sieć z każdym żądaniem. Zmieniając nazwę domeny inne niż microsoft.com można zwiększyć wydajność przez tyle 25%. Uwaga ajax.microsoft.com będą nadal działać, ale ajax.aspnetcdn.com jest zalecane.

- Stary Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js
- Nowy Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a>Obsługa .vsdoc programu Visual Studio

Do korzystania z plików .vsdoc prawidłowo z programu Visual Studio 2008, należy się upewnić, że masz VS 2008 z dodatkiem SP1 należy zainstalować i zainstalowana poprawka vsdoc plików. Możesz uzyskać je w tym miejscu:

- [Pobierz program Visual Studio 2008 z dodatkiem SP1](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "pobierania programu Visual Studio 2008 z dodatkiem SP1")
- [Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "pobrać poprawkę .vsdoc dla programu Visual Studio 2008 z dodatkiem SP1")

Program Visual Studio 2010 obsługuje pliki .vsdoc bez żadnych dodatkowych poprawek.

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a>Za pomocą kodu ASP.NET Ajax z sieci CDN

Podczas korzystania z programu ASP.NET 4, można przekierowywać wszystkie żądania ASP.NET framework skryptów do sieci CDN. Pobieranie skryptów z sieci CDN zamiast serwera sieci web lokalnego znacznie może poprawić wydajność publicznych witryn sieci Web ASP.NET.

Za pomocą właściwości ScriptManager EnableCDN Przekieruj żądania skryptu framework ASP.NET do programu Microsoft Ajax CDN:

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a>Przy użyciu jQuery z sieci CDN

Za pomocą skryptów jQuery hostowanych w sieci CDN w aplikacji sieci Web, dodając następujący element skryptu do strony:

[!code-html[Main](overview/samples/sample2.html)]

Usługa CDN zawiera również wersję zminimalizowany skryptu jQuery, którą można uzyskać za pomocą następującego elementu:

[!code-html[Main](overview/samples/sample3.html)]

Aby zezwolić na stronę do powrotu do ładowania jQuery z ścieżkę lokalną na własne witryny sieci Web, jeśli sieć CDN jest niedostępny, Dodaj następujący element natychmiast po elemencie odwołujące się do sieci CDN:

[!code-html[Main](overview/samples/sample4.html)]

Następujące przykładowe strony korzysta z wersji CDN biblioteki jQuery (z powrotu do lokalnej kopii) do wyświetlenia zawartości elementu div, gdy przycisk zostanie kliknięty.

[!code-html[Main](overview/samples/sample5.html)]

Można dowiedzieć się więcej o jQuery i pobrać kopię lokalną jQuery odwiedzając [jQuery](http://jquery.com/) witryny sieci Web.

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a>Przy użyciu interfejsu użytkownika z sieci CDN jQuery

Usługa CDN obsługuje również biblioteki interfejsu użytkownika jQuery. Biblioteka interfejsu użytkownika jQuery zawiera bogaty zestaw elementów widget i efekty, których można używać w aplikacji ASP.NET. Na przykład następująca strona przedstawiono, jak jQuery selektora daty interfejsu użytkownika w kontekście aplikacji formularzy sieci Web programu ASP.NET można użyć do wyświetlenia wyskakującego kalendarza:

[!code-aspx[Main](overview/samples/sample6.aspx)]

Gdy fokus zostanie przeniesiony do pola tekstowego przy użyciu klawiatury, zostanie wyświetlony kalendarz:

![Utworzone za pomocą selektora daty kalendarza podręcznego](overview/_static/image1.png)

Zwróć uwagę, że musi zawierać trzy pliki z sieci CDN w powyższym kodzie:

- Biblioteki jQuery &mdash; biblioteki interfejsu użytkownika jQuery jest zależna od biblioteki jQuery. Należy dodać biblioteki jQuery do strony przed dodaniem biblioteki interfejsu użytkownika jQuery.
- Biblioteka interfejsu użytkownika jQuery &mdash; biblioteki interfejsu użytkownika jQuery zawiera elementy widget, takie jak używane na stronie powyżej widżetu selektora daty i efektów interfejsu użytkownika jQuery.
- Motyw interfejsu użytkownika jQuery &mdash; jQuery interfejsu użytkownika obsługuje różne kompozycje. Strona sieci zawiera łącze do pliku CSS do zaimportowania Redmond motywu.

Wszystkie kompozycje interfejsu użytkownika jQuery standardowe znajdują się w sieci CDN. [Odwiedź stronę tego](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN") do wyświetlania miniatur dla każdego motywu.

Aby dowiedzieć się więcej na temat biblioteki interfejsu użytkownika jQuery, można znaleźć w oficjalnym [witryny sieci Web interfejsu użytkownika jQuery](http://jQueryUI.com "witryny sieci Web interfejsu użytkownika jQuery").

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a>Pliki innych firm w sieci CDN

Usługa CDN obsługuje niektóre najbardziej popularnych bibliotek JavaScript innych firm. Microsoft nie rości sobie praw własności żadnych bibliotek innych firm hostowanych na tym CDN. Właściciele praw autorskich bibliotek są licencjonowania te biblioteki do Ciebie. Wszelkie prawa, które może być konieczne pobranie i użycie tych bibliotek są przyznawane wyłącznie przez właścicieli praw autorskich. Ponieważ nie są one biblioteki Microsoft, firma Microsoft udostępnia żadnych gwarancji ani licencji praw własności intelektualnej (w tym Brak domyślnych praw patentowe) dla bibliotek innej hostowanych na tym CDN.

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a>Wersje dostępne w sieci CDN

Następujące wersje jQuery znajdują się w sieci CDN:

#### <a name="jquery-version-331"></a>Wersja jQuery 3.3.1
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a>Wersja jQuery 3.2.1
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a>Wersja jQuery 3.2.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a>Wersja jQuery 3.1.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a>Wersja jQuery 3.1.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a>Wersja jQuery 3.0.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a>Wersja jQuery 2.2.4

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a>Wersja jQuery 2.2.3

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a>Wersja jQuery 2.2.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a>Wersja jQuery 2.2.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a>Wersja jQuery 2.2.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a>Wersja jQuery 2.1.4

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a>Wersja jQuery 2.1.3

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a>Wersja jQuery 2.1.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a>Wersja jQuery 2.1.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a>Wersja jQuery 2.1.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a>Wersja jQuery 2.0.3

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a>Wersja jQuery pkt 2.0.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a>Wersja jQuery 2.0.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a>jQuery wersji 2.0.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a>Wersja jQuery 1.12.4

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a>Wersja jQuery 1.12.3

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a>Wersja jQuery 1.12.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a>Wersja jQuery 1.12.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a>Wersja jQuery 1.12.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a>Wersja jQuery 1.11.3

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a>Wersja jQuery 1.11.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a>Wersja jQuery 1.11.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a>Wersja jQuery 1.11.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a>Wersja jQuery 1.10.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a>Wersja jQuery 1.10.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a>Wersja jQuery 1.10.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a>Wersja jQuery 1.9.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a>Wersja jQuery 1.9.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a>Wersja jQuery 1.8.3

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a>Wersja jQuery 1.8.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a>Wersja jQuery 1.8.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a>Wersja jQuery 1.8.0

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a>Wersja jQuery 1.7.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a>Wersja jQuery 1.7.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a>Wersja jQuery 1.7

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a>Wersja jQuery 1.6.4

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a>Wersja jQuery 1.6.3

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a>jQuery wersji 1.6.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a>jQuery wersji 1.6.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a>jQuery w wersji 1.6

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a>Wersja jQuery 1.5.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a>Wersja jQuery 1.5.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a>jQuery w wersji 1.5

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a>Wersja jQuery 1.4.4

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a>Wersja jQuery 1.4.3

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a>Wersja jQuery 1.4.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a>Wersja jQuery 1.4.1

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a>jQuery w wersji 1.4

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a>Wersja jQuery 1.3.2

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a>Wersje jQuery migracji w sieci CDN

Następujące wersje jQuery migracji znajdują się w sieci CDN:

#### <a name="jquery-migrate-version-300"></a>jQuery migracji wersji 3.0.0

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a>jQuery migracji wersji 1.2.1

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

jQuery migracji wersji 1.2.0

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a>jQuery migracji wersji 1.1.1

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a>jQuery migracji wersji 1.1.0

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a>jQuery migracji w wersji 1.0.0

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a>Dostępne wersje interfejsu użytkownika w sieci CDN

Następujące wersje biblioteki interfejsu użytkownika jQuery znajdują się w tej sieci CDN. Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.

- [1.12.1 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1121.md "jQuery 1.12.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.12.0 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1120.md "jQuery 1.12.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.11.4 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1114.md "jQuery 1.11.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.11.3 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1113.md "jQuery 1.11.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.11.2 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1112.md "jQuery 1.11.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.11.1 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1111.md "jQuery 1.11.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.11.0 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1110.md "jQuery 1.11.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.10.4 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1104.md "jQuery 1.10.4 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.10.3 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1103.md "jQuery 1.10.3 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [jQuery 1.10.2 interfejsu użytkownika](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.10.1 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1101.md "jQuery 1.10.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.10.0 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1100.md "jQuery 1.10.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.9.2 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui192.md "jQuery 1.9.2 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.9.1 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui191.md "jQuery 1.9.1 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.9.0 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui190.md "jQuery 1.9.0 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.24 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1824.md "jQuery 1.8.24 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.23 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1823.md "jQuery 1.8.23 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.22 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1822.md "jQuery 1.8.22 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.21 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1821.md "jQuery 1.8.21 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.20 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1820.md "jQuery 1.8.20 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.19 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1819.md "jQuery 1.8.19 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.18 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1818.md "jQuery 1.8.18 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.17 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1817.md "jQuery 1.8.17 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.16 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1816.md "jQuery 1.8.16 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.15 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1815.md "jQuery 1.8.15 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.14 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1814.md "jQuery 1.8.14 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.13 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1813.md "jQuery 1.8.13 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.12 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1812.md "jQuery 1.8.12 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.11 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1811.md "jQuery 1.8.11 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.10 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui1910.md "jQuery 1.8.10 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.9 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui189.md "jQuery 1.8.9 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.8 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui188.md "jQuery 1.8.8 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.7 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui187.md "jQuery 1.8.7 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [1.8.6 interfejsu użytkownika jQuery](jquery-ui/cdnjqueryui186.md "jQuery 1.8.6 interfejsu użytkownika w sieci Microsoft Ajax CDN")
- [jQuery UI 1.8.5](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a>Dostępne wersje weryfikacji w sieci CDN

Następujące wersje weryfikacji biblioteki jQuery znajdują się w tej sieci CDN. Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.

- [Sprawdź poprawność 1.17.0 jQuery](jquery-validate/cdnjqueryvalidate1170.md "jQuery 1.17.0 sprawdzania poprawności")
- [Sprawdź poprawność 1.16.0 jQuery](jquery-validate/cdnjqueryvalidate1160.md "jQuery 1.16.0 sprawdzania poprawności")
- [Sprawdź poprawność 1.15.1 jQuery](jquery-validate/cdnjqueryvalidate1151.md "jQuery 1.15.1 sprawdzania poprawności")
- [Sprawdź poprawność 1.15.0 jQuery](jquery-validate/cdnjqueryvalidate1150.md "jQuery 1.15.0 sprawdzania poprawności")
- [Sprawdź poprawność 1.14.0 jQuery](jquery-validate/cdnjqueryvalidate1140.md "jQuery 1.14.0 sprawdzania poprawności")
- [Sprawdź poprawność 1.13.1 jQuery](jquery-validate/cdnjqueryvalidate1131.md "jQuery 1.13.1 sprawdzania poprawności")
- [Sprawdź poprawność 1.13.0 jQuery](jquery-validate/cdnjqueryvalidate1130.md "jQuery 1.13.0 sprawdzania poprawności")
- [Sprawdź poprawność 1.12.0 jQuery](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 sprawdzania poprawności")
- [Sprawdź poprawność 1.11.1 jQuery](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 sprawdzania poprawności")
- [Sprawdź poprawność 1.11.0 jQuery](jquery-validate/cdnjqueryvalidate111.md "jQuery 1.11.0 sprawdzania poprawności")
- [Sprawdź poprawność 1.10.0 jQuery](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 sprawdzania poprawności")
- [Sprawdź poprawność 1.9 jQuery](jquery-validate/cdnjqueryvalidate19.md "jquery.validate wersji 1.9")
- [Sprawdź poprawność 1.8.1 jQuery](jquery-validate/cdnjqueryvalidate181.md "wersji jquery.validate 1.8.1")
- [Sprawdź poprawność 1.8 jQuery](jquery-validate/cdnjqueryvalidate18.md "jquery.validate wersji 1.8")
- [Sprawdź poprawność 1.7 jQuery](jquery-validate/cdnjqueryvalidate17.md "jquery.validate wersji 1.7")
- [jQuery weryfikacji 1.6](jquery-validate/cdnjqueryvalidate16.md "jQuery weryfikacji w wersji 1.6")
- [Sprawdź poprawność 1.5.5 jQuery](jquery-validate/cdnjqueryvalidate155.md "jQuery weryfikacji 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a>jQuery Mobile wersjach w sieci CDN

Następujące wersje biblioteki jQuery przenośnych znajdują się w tej sieci CDN. Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.

- [jQuery Mobile 1.4.5](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.4.2](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.4.1](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.4.0](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.3.2](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.3.1](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.3.0](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.2.0](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.1.2](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.1.1](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.1.0](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.1.0 RC 2](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.0.1](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.0](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.0 RC 2](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.0 RC 1](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 w sieci Microsoft Ajax CDN")
- [jQuery Mobile 1.0 beta 3](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 w sieci Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a>Dostępne wersje szablonów w sieci CDN

Następujące wersje dodatku szablony jQuery znajdują się w tej sieci CDN. Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.

- [jQuery szablonów w wersji Beta 1](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery szablonów w wersji Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a>Dostępne wersje cyklu w sieci CDN

Następujące wersje dodatku cyklu jQuery znajdują się w tej sieci CDN. Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.

- [jQuery Cycle 2.99](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [jQuery Cycle 2.94](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [jQuery Cycle 2.88](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a>Dostępne wersje DataTables w sieci CDN

Następujące wersje dodatku DataTables jQuery znajdują się w tej sieci CDN. Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.

- [jQuery DataTables 1.10.5](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [jQuery DataTables 1.10.4](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [jQuery DataTables 1.9.4](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [jQuery DataTables 1.9.3](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [jQuery DataTables 1.9.2](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [jQuery DataTables 1.9.1](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [jQuery DataTables 1.9.0](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [jQuery DataTables 1.8.2](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a>Wersje Modernizr w sieci CDN

Następujące wersje programu [Modernizr](http://www.modernizr.com "Modernizr") znajdują się w sieci CDN:

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a>Wersje JSHint w sieci CDN

Następujące wersje programu [JSHint](http://www.jshint.com "JSHint") znajdują się w sieci CDN:

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a>Wersje odcinania w sieci CDN

Następujące wersje programu [Knockout](http://www.knockoutjs.com "Knockout") znajdują się w sieci CDN:

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

### <a name="globalize-releases-on-the-cdn"></a>Globalize wersje w sieci CDN

Następujące wersje programu [Globalize](https://github.com/jquery/globalize "Globalize") znajdują się w sieci CDN:

#### <a name="globalize-version-100"></a>Globalize w wersji 1.0.0

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a>Globalize wersji 0.1.1

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - wszystkie kultur
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - Zastąp "{Kod kultury}" z kodem żądaną kulturę, np. Microsoft globalize.culture.en GB.js== plików w sieci CDN == te biblioteki zostały przekazane przez firmę Microsoft.

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a>Odpowiadać wersji w sieci CDN

Następujące wersje programu [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") odpowiedź znajdują się w sieci CDN:

#### <a name="respond-version-142"></a>Odpowiadać wersji 1.4.2

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a>Odpowiadać wersji 1.4.1

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a>Odpowiadać wersji 1.4.0

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a>Odpowiadać wersji 1.3.0

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a>Odpowiadać wersji 1.2.0

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a>Wersje bootstrap w sieci CDN

Następujące wersje programu [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap znajdują się w sieci CDN:

#### <a name="bootstrap-version-400"></a>Wersja bootstrap 4.0.0

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

#### <a name="bootstrap-version-337"></a>Wersja bootstrap 3.3.7

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

#### <a name="bootstrap-version-336"></a>Wersja bootstrap 3.3.6

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

#### <a name="bootstrap-version-335"></a>Bootstrap wersji 3.3.5

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

#### <a name="bootstrap-version-334"></a>Wersja bootstrap 3.3.4

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

#### <a name="bootstrap-version-332"></a>Wersja bootstrap 3.3.2

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

#### <a name="bootstrap-version-331"></a>Wersja bootstrap 3.3.1

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

#### <a name="bootstrap-version-330"></a>Wersja bootstrap 3.3.0

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

#### <a name="bootstrap-version-320"></a>Wersja bootstrap 3.2.0

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

#### <a name="bootstrap-version-311"></a>Wersja bootstrap 3.1.1

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

#### <a name="bootstrap-version-310"></a>Wersja bootstrap 3.1.0

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

#### <a name="bootstrap-version-303"></a>Wersja bootstrap 3.0.3

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

#### <a name="bootstrap-version-302"></a>Wersja bootstrap 3.0.2

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

#### <a name="bootstrap-version-301"></a>Bootstrap wersji 3.0.1

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

#### <a name="bootstrap-version-300"></a>Wersja bootstrap 3.0.0

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

#### <a name="bootstrap-version-232"></a>Wersja bootstrap 2.3.2

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a>Wersja bootstrap 2.3.1

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a>Wersje bootstrap TouchCarousel w sieci CDN

Następujące wersje programu [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") Bootstrap TouchCarousel wersjach znajdują się w sieci CDN:

#### <a name="bootstrap-touchcarousel-version-080"></a>Bootstrap TouchCarousel wersji 0.8.0

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a>Wersje hammer.js w sieci CDN

Następujące wersje programu [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js wersjach znajdują się w sieci CDN:

#### <a name="hammerjs-version-204"></a>Wersja hammer.js 2.0.4

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a>Formularze sieci Web ASP.NET i Ajax zwalnia w sieci CDN

Następujących wersji biblioteki ASP.NET Ajax znajdują się w sieci CDN. Kliknij każdy łącze, aby wyświetlić rzeczywiste listy plików.

- [Formularze sieci Web ASP.NET i Ajax wersji 4.5.2](cdnajax452.md "formularzy sieci Web ASP.NET i Ajax 4.5.2")
- [Formularze sieci Web ASP.NET i Ajax w wersji 4](cdnajax4.md "formularzy sieci Web ASP.NET i Ajax 4")
- [ASP.NET Ajax w wersji 3.5](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a>ASP.NET MVC udostępnia w sieci CDN

Następujące pliki platformy ASP.NET MVC JavaScript znajdują się w tej sieci CDN:

#### <a name="aspnet-mvc-523"></a>ASP.NET MVC 5.2.3

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a>ASP.NET MVC 5.1

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a>ASP.NET MVC 5.0

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a>ASP.NET MVC 4.0

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a>ASP.NET MVC 3.0

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a>ASP.NET MVC 2.0

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a>ASP.NET MVC 1.0

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a>Wersje biblioteki SignalR platformy ASP.NET w sieci CDN

Następujące pliki ASP.NET SignalR JavaScript znajdują się w tej sieci CDN:

#### <a name="aspnet-signalr-222"></a>ASP.NET SignalR 2.2.2

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a>ASP.NET SignalR 2.2.1

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a>ASP.NET SignalR 2.2.0

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a>ASP.NET SignalR 2.1.0

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a>ASP.NET SignalR 2.0.3

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a>ASP.NET SignalR 2.0.1

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a>ASP.NET SignalR 2.0.0

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a>ASP.NET SignalR 1.1.3

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a>ASP.NET SignalR 1.1.2

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a>ASP.NET SignalR 1.1.1

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a>ASP.NET SignalR 1.1.0

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a>ASP.NET SignalR 1.0.1

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

Aby dowiedzieć się, warunki użytkowania CDN, zobacz [Microsoft Ajax CDN warunkom użytkowania](https://www.asp.net/terms-of-use "Microsoft Ajax CDN warunkom użytkowania").
