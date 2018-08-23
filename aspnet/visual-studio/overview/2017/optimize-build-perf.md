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
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="d4770-103">Optymalizuj wydajność kompilacji dla rozwiązania</span><span class="sxs-lookup"><span data-stu-id="d4770-103">Optimize build performance for solution</span></span>
<span data-ttu-id="d4770-104">Visual Studio 2017 15.8 i później dodać nowy element menu, w obszarze **kompilacji > kompilacji platformy ASP.NET > zoptymalizować wydajność kompilacji dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="d4770-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![Zrzut ekranu przedstawiający nowy element menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="d4770-106">Program ASP.NET kompiluje swoją opinię w czasie wykonywania, co oznacza, że projekt ASP.NET niesie ze sobą kopię kompilator.</span><span class="sxs-lookup"><span data-stu-id="d4770-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="d4770-107">Jednak na komputerze dewelopera podczas kopiowania kompilatora nie jest zgodna kopia programu Visual Studio, wydajność kompilacji ma wpływ na rzędu kilku 1 – 3 sekundy na kompilacji przyrostowej.</span><span class="sxs-lookup"><span data-stu-id="d4770-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="d4770-108">Ta funkcja spowoduje zaktualizowanie projektu kopię kompilator, aby dopasować programu Visual Studio której powinna przyspieszyć kompilacje przyrostowe.</span><span class="sxs-lookup"><span data-stu-id="d4770-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="d4770-109">Dotyczy to tylko dla projektów środowiska ASP.NET Framework, natomiast nie odnoszą się do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4770-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
