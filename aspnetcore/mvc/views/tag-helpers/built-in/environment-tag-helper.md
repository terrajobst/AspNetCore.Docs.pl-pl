---
title: "Pomocnik Tag środowiska w platformy ASP.NET Core"
author: pkellner
description: "Pomocnika Tag środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Pomocnik Tag środowiska w platformy ASP.NET Core

Przez [Kellner Peterowi](http://peterkellner.net) i [Ateya Hisham Bin](https://twitter.com/hishambinateya)

Pomocnik Tag środowiska warunkowo renderuje zawartość objętego na podstawie bieżącego środowiska hostingu. Jego pojedynczy atrybut `names` jest rozdzielana przecinkami lista środowiska nazw, które, gdy dowolne pasuje do bieżącego środowiska wyzwoli objętego zawartości do renderowania.

## <a name="environment-tag-helper-attributes"></a>Atrybuty pomocnika Tag środowiska

### <a name="names"></a>nazwy

Akceptuje pojedynczą nazwą Środowisko hostingu lub rozdzielaną przecinkami listę hosting nazwy środowiska, wyzwalających renderowania zawartości zamknięte.

Te wartości są porównywane z bieżącą wartość zwracana z właściwości statycznej platformy ASP.NET Core `HostingEnvironment.EnvironmentName`.  Ta wartość jest jedną z następujących: **przemieszczania**; **Programowanie** lub **produkcji**. Porównanie ignoruje wielkość liter.

Przykład prawidłowego `environment` pomocnika tagów jest:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Dołącz i Wyklucz atrybutów

Platformy ASP.NET Core dodaje 2.x `include`  &  `exclude` atrybutów. Te atrybuty kontrolują renderowania objętego zawartości na podstawie dołączone lub wykluczone hostingu środowiska nazw.

### <a name="include-aspnet-core-20-and-later"></a>obejmują platformy ASP.NET Core 2.0 lub nowszy

`include` Właściwość ma podobne zachowania `names` atrybutu w ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>Wyklucz platformy ASP.NET Core 2.0 lub nowszy

Z kolei `exclude` umożliwia właściwości `EnvironmentTagHelper` renderowanie objętego zawartości dla wszystkich nazw Środowisko hostingu, z wyjątkiem wskaż, określony.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
