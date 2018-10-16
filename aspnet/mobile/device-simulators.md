---
uid: mobile/device-simulators
title: Symulowanie testowanie popularnych urządzeń przenośnych | Dokumentacja firmy Microsoft
author: rick-anderson
description: Możesz pobrać emulatory dla popularnych urządzeń przenośnych i przeglądarek, wykonując te linki
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348380"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>Symulowanie testowanie popularnych urządzeń przenośnych

> Możesz pobrać emulatory dla popularnych urządzeń przenośnych i przeglądarki, postępując zgodnie z poniższych linków.

| Urządzenia lub przeglądarki | Emulatorze / symulatorze |
| --- | --- |
| BrowserStack hostowanych wirtualizacji przeglądarki ![BrowserStack hostowanych wirtualizacji przeglądarki](device-simulators/_static/image1.png) | [Wirtualizacji przeglądarki hostowanej BrowserStack](http://browserstack.com) testów środowisku lokalnym lub w środowisku produkcyjnym w dowolnej przeglądarce, na dowolnej platformie. Można utworzyć tunel między komputera i sieci BrowserStack własne hostowanej maszynie wirtualnej. Upewnij się uzyskać [rozszerzenie programu Visual Studio BrowserStack](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack) dla bardziej bezproblemowe. |
| urządzenia iPhone / iPod / urządzenia iPad | [Electric Mobile Studio](http://www.electricplum.com/studio.aspx) urządzenia iPhone i iPad symulatorów dla Windows, a także dynamiczny Projekt narzędzia. |
| Android | [Program android Studio](https://developer.android.com/studio/) lub [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Emulator Opera Mobile klasycznego](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Zestaw narzędzi dla deweloperów Windows Mobile ppkt 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Uwaga: Aby udzielić dostępu do sieci telefonicznej, również należy karty sieciowej VPC objęte [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Do łączenia z programu Internet Explorer na telefonie do Twojego serwera wdrożeniowego programu Visual Studio, zobacz [wpis w blogu Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |

> [!NOTE]
> Jeśli chcesz wyświetlić aplikację na urządzeniu rzeczywiste przenośnym, (która jest jedyną opcją do testowania w pełni iPhone lub iPad, ponieważ nie ma żadnych true emulatora dla Windows) musisz hostować swoją aplikację w usługach IIS lub IIS Express. Visual Studio web wbudowanego serwera nie można użyć w tym celu łatwo, ponieważ nie będzie odpowiadać na żądania, z innych komputerów.