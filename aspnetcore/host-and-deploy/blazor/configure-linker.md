---
title: Skonfiguruj konsolidator dla ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak kontrolować konsolidator języka pośredniego (IL) podczas kompilowania aplikacji Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: b30669a7ca02c756fa10c8cf9973ef87e29e7bd4
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963616"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a>Skonfiguruj konsolidator dla ASP.NET Core Blazor

Autor [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor wykonuje konsolidację [języka pośredniego (IL)](/dotnet/standard/managed-code#intermediate-language--execution) podczas kompilacji wydania, aby usunąć niepotrzebny kod IL z zestawów wyjściowych aplikacji.

Łączenie zestawu sterującego przy użyciu jednej z następujących metod:

* Wyłącz konsolidację globalnie za pomocą [Właściwości programu MSBuild](#disable-linking-with-a-msbuild-property).
* Kontrolowanie łączenia poszczególnych zestawów z [plikiem konfiguracyjnym](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Wyłącz łączenie z właściwością programu MSBuild

Konsolidacja jest włączona domyślnie w trybie wydania, gdy aplikacja jest skompilowana, co obejmuje publikowanie. Aby wyłączyć łączenie dla wszystkich zestawów, ustaw właściwość programu MSBuild `BlazorLinkOnBuild` na `false` w pliku projektu:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Kontrola łączenia z plikiem konfiguracji

Kontroluj łączenie dla poszczególnych zestawów, dostarczając plik konfiguracyjny XML i określając plik jako element programu MSBuild w pliku projektu:

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

*Konsolidator. XML*:

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

Aby uzyskać więcej informacji, zobacz [Il konsolidator: Składnia deskryptora XML](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).
