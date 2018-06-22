---
title: Pomocnik Tag środowiska w platformy ASP.NET Core
author: pkellner
description: Pomocnika Tag środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 05c07b06a4fedac0b0ff39d168807f5e2e6996cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276919"
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
