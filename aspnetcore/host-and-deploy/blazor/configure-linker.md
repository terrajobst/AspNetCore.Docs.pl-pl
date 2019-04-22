---
title: Konfigurowanie konsolidatora dla Blazor
author: guardrex
description: Dowiedz się, jak kontrolować konsolidatora języka pośredniego (IL), podczas kompilowania aplikacji Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 77016b6b1542becb2ccd49f0c0c5ba63da434a6c
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982617"
---
# <a name="configure-the-linker-for-blazor"></a>Konfigurowanie konsolidatora dla Blazor

Przez [Luke Latham](https://github.com/guardrex)

Wykonuje Blazor [języka pośredniego (IL)](/dotnet/standard/managed-code#intermediate-language--execution) łączenia podczas każdej kompilacji tryb wersji usuwanie niepotrzebnych IL aplikacji danych wyjściowych zestawów.

Zestaw Kontrola połączeń przy użyciu jednej z następujących metod:

* Wyłącz konsolidowanie globalnie za pomocą [właściwość MSBuild](#disable-linking-with-a-msbuild-property).
* Formant konsolidacji na podstawie na zestawie [pliku konfiguracyjnego](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Wyłącz konsolidowanie za pomocą właściwości programu MSBuild

Łączenie jest domyślnie włączanych w trybie wydania kompilowana jest aplikacja, która obejmuje publikowania. Aby wyłączyć łączenie dla wszystkich zestawów, należy ustawić `<BlazorLinkOnBuild>` właściwości programu MSBuild `false` w pliku projektu:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Łączenie się z plikiem konfiguracyjnym kontroli

Kontrola konsolidacji na podstawie poszczególnych zestawów, zapewniając plik konfiguracyjny XML i określenie pliku jako element programu MSBuild w pliku projektu:

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

*Linker.XML*:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/aspnet/Blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

Aby uzyskać więcej informacji, zobacz [IL konsolidatora: Składnia xml deskryptora](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).
