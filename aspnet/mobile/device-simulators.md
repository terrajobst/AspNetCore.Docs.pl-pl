---
uid: mobile/device-simulators
title: "Symulowanie testowanie popularnych urządzeń przenośnych | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Wykonując te łącza można pobrać emulatory popularnych urządzeń przenośnych i przeglądarki"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2011
ms.topic: article
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 48145b15b4983d6a143a8c53c9e6e8b4639da91e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Symulowanie testowanie popularnych urządzeń przenośnych
====================
> Wykonując te łącza można pobrać emulatory popularnych urządzeń przenośnych i przeglądarki


| Urządzenia lub przeglądarki | Emulatorze / symulatorze |
| --- | --- |
| BrowserStack obsługiwane przeglądarki wirtualizacji ![BrowserStack obsługiwane przeglądarki wirtualizacji](device-simulators/_static/image1.png) | [Wirtualizacja przeglądarki hostowanej BrowserStack](http://browserstack.com) użytkownika lokalnego lub produkcyjnego środowiska testowego w dowolnej przeglądarce na dowolnej platformie. Tunel między komputera i sieci BrowserStack można tworzyć własne hostowanej maszynie wirtualnej. Upewnij się uzyskać [BrowserStack programu Visual Studio rozszerzenia](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) jeszcze bardziej usprawnić środowisko. |
| Windows Phone | [Windows Phone SDK pobiera](https://dev.windowsphone.com/en-us/downloadsdk) Windows Phone Software Development Kit (SDK) zawiera wszystkie narzędzia potrzebne do opracowywania aplikacji i gier dla Windows Phone |
| urządzenia iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) iPhone i iPad firmy dla systemu Windows, a także Responsive narzędzie projektowania. Można zintegrować z opcją "Przeglądaj z.." VS 2012. |
| Android | [Android — strona główna zestawu SDK](https://developer.android.com/sdk) |
| Opera Mobile / Opera Mini | Najnowsze wersje: [Opera Developer Tools macierzystego](http://www.opera.com/developer/tools/) Opera Mini 4.2: [symulatora opartych na języku Java w trybie Online](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile ppkt 6.5.3 | [Zestaw narzędzi dewelopera systemu Windows Mobile ppkt 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Uwaga: Aby udzielić dostępu do sieci telefonicznej, również należy karty sieciowej VPC objęte [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Aby połączyć programu Internet Explorer na telefonie do serwera wdrożeniowego programu Visual Studio, zobacz [Kiran Patil wpis w blogu](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [Obrazy emulatora dla programu Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

Należy pamiętać, aby wyświetlić aplikację na urządzeniu przenośnych rzeczywistego (która jest jedyną opcją do testowania pełni iPhone lub iPad, ponieważ nie istnieje żadne true emulator systemu Windows) konieczne będzie hostowania aplikacji w usługach IIS lub usług IIS Express. Serwer sieci web wbudowanych programu Visual Studio nie można użyć w tym celu łatwo, ponieważ nie będzie on odpowiadać na żądania z innych komputerów.
