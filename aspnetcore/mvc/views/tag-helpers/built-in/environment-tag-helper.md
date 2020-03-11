---
title: Pomocnik tagu środowiska w ASP.NET Core
author: pkellner
description: Zdefiniowano pomocnika tagów środowiska ASP.NET Core, w tym wszystkie właściwości
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 308e7db47104ebd4d6bb8d08c64f14bbd118898b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663990"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Pomocnik tagu środowiska w ASP.NET Core

Według [Piotr Kellner](https://peterkellner.net) i [Hisham bin Ateya](https://twitter.com/hishambinateya)

Pomocnik tagu środowiska warunkowo renderuje zawartą zawartość w oparciu o bieżące [środowisko hostingu](xref:fundamentals/environments). Pojedynczy atrybut pomocnika tagu środowiska, `names`, jest rozdzielaną przecinkami listą nazw środowiska. Jeśli dowolna z podanych nazw środowiska jest zgodna z bieżącym środowiskiem, załączona zawartość jest renderowana.

Aby zapoznać się z omówieniem pomocników tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

## <a name="environment-tag-helper-attributes"></a>Atrybuty pomocnika tagów środowiska

### <a name="names"></a>nazwy

`names` akceptuje jedną nazwę środowiska hostingu lub rozdzieloną przecinkami listę nazw środowisk hostingu, które wyzwalają renderowanie załączonej zawartości.

Wartości środowiskowe są porównywane z bieżącą wartością zwracaną przez [IHostingEnvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*). Porównanie ignoruje wielkość liter.

W poniższym przykładzie jest używana pomocnik tagów środowiska. Zawartość jest renderowana, jeśli środowisko hostingu jest przejściowe lub produkcyjne:

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a>Dołączanie i wykluczanie atrybutów

`include` & `exclude` atrybutów, renderowanie zawartości zawartej w odniesieniu do zawartych lub wykluczonych nazw środowisk macierzystych.

### <a name="include"></a>include

Właściwość `include` wykazuje podobne zachowanie w atrybucie `names`. Środowisko wymienione w wartości atrybutu `include` musi być zgodne ze środowiskiem hostingu aplikacji ([IHostingEnvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)), aby renderować zawartość tagu `<environment>`.

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a>wykluczanie

W przeciwieństwie do atrybutu `include`, zawartość tagu `<environment>` jest renderowana, gdy środowisko hostingu nie jest zgodne ze środowiskiem wymienionym w `exclude` wartości atrybutu.

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/environments>
