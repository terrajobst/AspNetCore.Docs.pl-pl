---
title: Pomocnik tagu środowiska w programie ASP.NET Core
author: pkellner
description: Pomocnik tagu środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901003"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Pomocnik tagu środowiska w programie ASP.NET Core

Przez [Peter Kellner](http://peterkellner.net), [Ateya Hisham Bin](https://twitter.com/hishambinateya), i [Luke Latham](https://github.com/guardrex)

Pomocnik tagu środowiska warunkowo renderuje zawartość ujęty na podstawie bieżącego [Środowisko hostingu](xref:fundamentals/environments). Pomocnik tagu środowiska jeden atrybut `names`, znajduje się lista rozdzielonych przecinkami nazw środowiska. Jeśli żadnej z nazw dostarczonego środowiska zgodne bieżącego środowiska, ujęty zawartość jest wyświetlana.

Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

## <a name="environment-tag-helper-attributes"></a>Atrybuty Pomocnik tagu środowiska

### <a name="names"></a>nazwy

`names` Akceptuje pojedynczą nazwą środowiska hostingu lub rozdzielaną przecinkami listę hostingu nazwy środowiska, które mogą powodować renderowania ujęty zawartości.

Wartości środowiskowe są porównywane z bieżącej wartości zwracanej przez [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*). Porównanie, ignoruje wielkość liter.

W poniższym przykładzie użyto Pomocnik tagu środowiska. Zawartość jest wyświetlana, jeśli środowisko hostingu jest przejściowych lub produkcyjnych:

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a>Dołączanie i wykluczanie atrybutów

`include` & `exclude` atrybuty kontrolują renderowania zawartości ujęty na podstawie dołączone lub wykluczone hostingu środowiska nazw.

### <a name="include"></a>include

`include` Właściwość wykazuje zachowanie podobne do `names` atrybutu. Środowisko na liście `include` wartość atrybutu muszą być zgodne Środowisko hostingu aplikacji ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) do renderowania zawartości `<environment>` tagu.

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a>wykluczanie

W przeciwieństwie do `include` atrybutu zawartość `<environment>` renderowania tagu, gdy środowisko hostingu nie są zgodne środowisko na liście `exclude` wartość atrybutu.

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/environments>
