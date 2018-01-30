---
title: "Pomocnik Tag środowiska w platformy ASP.NET Core"
author: pkellner
description: "Pomocnika Tag środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości"
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
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
