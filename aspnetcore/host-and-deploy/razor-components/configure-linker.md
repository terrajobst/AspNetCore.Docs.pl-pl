---
title: Konfigurowanie konsolidatora dla Blazor
author: guardrex
description: Dowiedz się, jak kontrolować konsolidatora języka pośredniego (IL), podczas kompilowania aplikacji Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: c3c38ec2509344cc02f3895d5d0c2d35059d1d8e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668176"
---
# <a name="configure-the-linker-for-blazor"></a>Konfigurowanie konsolidatora dla Blazor

Przez [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Wykonuje Blazor [języka pośredniego (IL)](/dotnet/standard/managed-code#intermediate-language--execution) łączenia podczas każdej kompilacji tryb wersji, można usunąć niepotrzebne IL z zestawów danych wyjściowych.

Można kontrolować zestawu konsolidowanie za pomocą jednej z następujących metod:

* Wyłącz konsolidowanie globalnie za pomocą właściwości programu MSBuild.
* Kontrolka konsolidacji na podstawie poszczególnych zestawów przy użyciu pliku konfiguracji.

## <a name="disable-linking-with-an-msbuild-property"></a>Wyłącz konsolidowanie za pomocą właściwości programu MSBuild

Łączenie jest domyślnie włączanych w trybie wydania kompilowana jest aplikacja, która obejmuje publikowania. Aby wyłączyć łączenie dla wszystkich zestawów, należy ustawić `<BlazorLinkOnBuild>` właściwości programu MSBuild `false` w pliku projektu:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Łączenie się z plikiem konfiguracyjnym kontroli

Łączenie mogą być kontrolowane na podstawie poszczególnych zestawów, zapewniając plik konfiguracyjny XML i określenie pliku jako element programu MSBuild w pliku projektu.

Poniżej przedstawiono przykładowy plik konfiguracji (*Linker.xml*):

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

Aby dowiedzieć się więcej o formacie pliku dla pliku konfiguracji, zobacz [IL konsolidatora: Składnia xml deskryptora](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor).

Określ plik konfiguracji w pliku projektu za pomocą `BlazorLinkerDescriptor` elementu:

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
