---
uid: mobile/device-simulators
title: Symulowanie testowanie popularnych urządzeń przenośnych | Dokumentacja firmy Microsoft
author: rick-anderson
description: Możesz pobrać emulatory dla popularnych urządzeń przenośnych i przeglądarek, wykonując te linki
ms.author: riande
ms.date: 01/28/2011
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 9498c370003d20ba0b8a835a8d4cd86961c3bf3c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757283"
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Symulowanie testowanie popularnych urządzeń przenośnych
====================
> Możesz pobrać emulatory dla popularnych urządzeń przenośnych i przeglądarek, wykonując te linki


| Urządzenia lub przeglądarki | Emulatorze / symulatorze |
| --- | --- |
| BrowserStack hostowanych wirtualizacji przeglądarki ![BrowserStack hostowanych wirtualizacji przeglądarki](device-simulators/_static/image1.png) | [Wirtualizacji przeglądarki hostowanej BrowserStack](http://browserstack.com) testów środowisku lokalnym lub w środowisku produkcyjnym w dowolnej przeglądarce, na dowolnej platformie. Można utworzyć tunel między komputera i sieci BrowserStack własne hostowanej maszynie wirtualnej. Upewnij się uzyskać [rozszerzenie programu Visual Studio BrowserStack](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) dla bardziej bezproblemowe. |
| Windows Phone | [Windows Phone SDK pobiera](https://dev.windowsphone.com/downloadsdk) Windows Phone Software Development Kit (SDK) obejmuje wszystkie narzędzia potrzebne do tworzenia aplikacji i gier dla Windows Phone |
| urządzenia iPhone / iPod / urządzenia iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) urządzenia iPhone i iPad symulatorów dla Windows, a także dynamiczny Projekt narzędzia. Można zintegrować z opcją "Przeglądaj z.." VS 2012. |
| Android | [Android SDK strony głównej](https://developer.android.com/sdk) |
| Opera Mobile / Opera Mini | Najnowsze wersje: [głównego narzędzia programistyczne Opera](http://www.opera.com/developer/tools/) Opera Mini 4.2: [symulator opartych na języku Java w trybie Online](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Zestaw narzędzi dla deweloperów Windows Mobile ppkt 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Uwaga: Aby udzielić dostępu do sieci telefonicznej, również należy karty sieciowej VPC objęte [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Do łączenia z programu Internet Explorer na telefonie do Twojego serwera wdrożeniowego programu Visual Studio, zobacz [wpis w blogu Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [Obrazy emulatora dla programu Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

Należy pamiętać, że jeśli chcesz wyświetlić aplikację na urządzeniu rzeczywiste przenośnym, (która jest jedyną opcją do testowania w pełni iPhone lub iPad, ponieważ nie ma żadnych true emulatora dla Windows) musisz hostować swoją aplikację w usługach IIS lub IIS Express. Visual Studio web wbudowanego serwera nie można użyć w tym celu łatwo, ponieważ nie będzie odpowiadać na żądania, z innych komputerów.
