---
title: Pomocnik tagu środowiska w programie ASP.NET Core
author: pkellner
description: Pomocnik tagu środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342227"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Pomocnik tagu środowiska w programie ASP.NET Core

Przez [Peter Kellner](http://peterkellner.net) i [Ateya Hisham pojemnika](https://twitter.com/hishambinateya)

Pomocnik tagu środowiska warunkowo renderuje zawartość ujęty na podstawie bieżącego środowiska hostingu. Jego jeden atrybut `names` jest rozdzielana przecinkami lista środowiska nazwy, że jeśli dowolny pasuje do bieżącego środowiska, wywoła ujęty zawartości do renderowania.

## <a name="environment-tag-helper-attributes"></a>Atrybuty Pomocnik tagu środowiska

### <a name="names"></a>nazwy

Akceptuje pojedynczą nazwą środowiska hostingu lub rozdzielaną przecinkami listę hostingu nazwy środowiska, które mogą powodować renderowania ujęty zawartości.

Te wartości są porównywane z bieżącą wartość zwracana z właściwości statycznej platformy ASP.NET Core `HostingEnvironment.EnvironmentName`.  Ta wartość jest jedną z następujących: **przemieszczania**; **Rozwoju** lub **produkcji**. Porównanie, ignoruje wielkość liter.

Przykładem prawidłowego `environment` Pomocnik tagu:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Dołączanie i wykluczanie atrybutów

Platforma ASP.NET Core 2.x dodaje `include`  &  `exclude` atrybutów. Te atrybuty kontrolują renderowania zawartości ujęty na podstawie dołączone lub wykluczone hostingu środowiska nazw.

### <a name="include-aspnet-core-20-and-later"></a>obejmują platformy ASP.NET Core 2.0 i nowsze wersje

`include` Właściwość ma podobne zachowanie `names` atrybutu w programie ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>Wyklucz platformy ASP.NET Core 2.0 i nowsze wersje

Z kolei `exclude` właściwości `EnvironmentTagHelper` renderowania ujęty zawartości dla wszystkich nazw środowiska hostingu, z wyjątkiem wskaż, który określiłeś.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/environments>
