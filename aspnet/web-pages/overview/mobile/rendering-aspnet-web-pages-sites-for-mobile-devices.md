---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderowanie sieci Web ASP.NET stron witryny (Razor) dla urządzeń przenośnych | Dokumentacja firmy Microsoft
author: tfitzmac
description: 'W tym artykule opisano sposób tworzenia stron w witrynie stron sieci Web platformy ASP.NET (Razor), który będzie renderowany odpowiednio na urządzeniach przenośnych. Dowiesz się: jak możesz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 641798c5b835be959d02dd0d854b61ca21d83016
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897322"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Renderowanie witryny sieci Web ASP.NET (Razor) stron dla urządzeń przenośnych
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób tworzenia stron w witrynie stron sieci Web platformy ASP.NET (Razor), który będzie renderowany odpowiednio na urządzeniach przenośnych.
> 
> Zawartość:
> 
> - Jak używać konwencji nazewnictwa, aby określić, że strona jest przeznaczony specjalnie dla urządzeń przenośnych.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


Strony ASP.NET Web Pages umożliwia tworzenie niestandardowych służy do renderowania zawartości na telefon komórkowy lub innych urządzeń.

Najprostszym sposobem tworzenia urządzenia strony w witrynie stron ASP.NET Web Pages jest przy użyciu wzorca nazewnictwa plików następująco: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Można utworzyć dwie wersje strony (na przykład jedną o nazwie <em>MyFile.cshtml</em> i jedną o nazwie <em>MyFile.Mobile.cshtml</em>). W czasie, gdy urządzenie przenośne żąda wykonywania <em>MyFile.cshtml</em>, platforma ASP.NET renderuje zawartość z <em>MyFile.Mobile.cshtml</em>. W przeciwnym razie <em>MyFile.cshtml</em> jest renderowany.

Poniższy przykład pokazuje, jak umożliwiające renderowanie przenośnych przez dodanie strony zawartości dla urządzeń przenośnych. *Page1.cshtml* zawiera zawartości oraz pasek boczny nawigacji. *Page1.Mobile.cshtml* zawiera tę samą zawartość, ale pominięto paska bocznego.

1. W lokacji stron ASP.NET Web Pages, Utwórz plik o nazwie *Page1.cshtml* i Zastąp następujący kod znaczników bieżącej zawartości.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Utwórz plik o nazwie *Page1.Mobile.cshtml* i zastąpić istniejącą zawartość następujących znaczników. Zwróć uwagę, że wersja urządzenia przenośne strony pomija sekcję nawigacji dla lepszej renderowania na ekranie mniejsze.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Uruchom przeglądarkę dla komputerów, a następnie przejdź do *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Uruchom przeglądarkę dla telefonów (lub w emulatorze urządzenia przenośnego) i przejdź do *Page1.cshtml*. (Zwróć uwagę, że nie ma *.mobile.* w ramach adresu URL). Nawet jeśli żądanie dotyczy *Page1.cshtml*, platforma ASP.NET renderuje *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Aby przetestować stron dla urządzeń przenośnych, można użyć symulator urządzeń przenośnych, uruchomioną na komputerze stacjonarnym. To narzędzie umożliwia testowanie stron sieci web, jak wyglądają na urządzeniach przenośnych (oznacza to zwykle z dużo mniejszym wyświetlenie obszaru). Przykład symulatora [przełącznik agenta użytkownika dodatku](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) programie Mozilla Firefox, co umożliwia emulować różnych przeglądarkach dla urządzeń przenośnych z wersji pulpitu Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Emulator Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
