---
title: Pomocnik tagu środowiska w programie ASP.NET Core
author: pkellner
description: Pomocnik tagu środowiska ASP.NET Core zdefiniowane w tym wszystkie właściwości
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: e2e038fe69da696b67f7aef61795e23dc8512fdf
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856135"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Pomocnik tagu środowiska w programie ASP.NET Core

Przez [Peter Kellner](https://peterkellner.net), [Ateya Hisham Bin](https://twitter.com/hishambinateya), i [Luke Latham](https://github.com/guardrex)

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
