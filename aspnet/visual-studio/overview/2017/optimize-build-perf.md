---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optymalizuj wydajność kompilacji dla rozwiązania
author: tfitzmac
description: Optymalizuj wydajność kompilacji dla rozwiązania
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910005"
---
# <a name="optimize-build-performance-for-solution"></a>Optymalizuj wydajność kompilacji dla rozwiązania
Visual Studio 2017 15.8 i później dodać nowy element menu, w obszarze **kompilacji > kompilacji platformy ASP.NET > zoptymalizować wydajność kompilacji dla rozwiązania**.

![Zrzut ekranu przedstawiający nowy element menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

Program ASP.NET kompiluje swoją opinię w czasie wykonywania, co oznacza, że projekt ASP.NET niesie ze sobą kopię kompilator. Jednak na komputerze dewelopera podczas kopiowania kompilatora nie jest zgodna kopia programu Visual Studio, wydajność kompilacji ma wpływ na rzędu kilku 1 – 3 sekundy na kompilacji przyrostowej. Ta funkcja spowoduje zaktualizowanie projektu kopię kompilator, aby dopasować programu Visual Studio której powinna przyspieszyć kompilacje przyrostowe.

Dotyczy to tylko dla projektów środowiska ASP.NET Framework, natomiast nie odnoszą się do platformy ASP.NET Core.
