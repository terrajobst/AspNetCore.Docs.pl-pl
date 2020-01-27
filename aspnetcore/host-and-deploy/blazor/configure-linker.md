---
title: Skonfiguruj konsolidator dla ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak kontrolować konsolidator języka pośredniego (IL) podczas kompilowania aplikacji Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 263b85a3213c1da233e4c96095faaf39d0a8e13f
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726772"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a>Skonfiguruj konsolidator dla ASP.NET Core Blazor

Przez [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor wykonuje konsolidację [języka pośredniego (IL)](/dotnet/standard/managed-code#intermediate-language--execution) podczas kompilacji, aby usunąć niepotrzebny kod IL z zestawów wyjściowych aplikacji.

Łączenie zestawu sterującego przy użyciu jednej z następujących metod:

* Wyłącz konsolidację globalnie za pomocą [Właściwości programu MSBuild](#disable-linking-with-a-msbuild-property).
* Kontrolowanie łączenia poszczególnych zestawów z [plikiem konfiguracyjnym](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Wyłącz łączenie z właściwością programu MSBuild

Konsolidacja jest domyślnie włączona po skompilowaniu aplikacji, która obejmuje publikowanie. Aby wyłączyć łączenie dla wszystkich zestawów, ustaw właściwość `BlazorLinkOnBuild` MSBuild na `false` w pliku projektu:

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
      Fixes: https://github.com/dotnet/blazor/issues/239
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

### <a name="configure-the-linker-for-internationalization"></a>Konfigurowanie konsolidatora dla celów wielojęzycznych

Domyślnie konfiguracja konsolidatora Blazordla Blazor aplikacji webassembly umożliwia rozłączenie informacji o danych wielojęzycznych z wyjątkiem lokalizacji lokalnych jawnie żądanych. Usunięcie tych zestawów minimalizuje rozmiar aplikacji.

Aby kontrolować, które zestawy I18N są zachowywane, ustaw właściwość `<MonoLinkerI18NAssemblies>` MSBuild w pliku projektu:

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| Wartość regionu     | Zestaw regionów mono    |
| ---------------- | ----------------------- |
| `all`            | Uwzględnione wszystkie zestawy |
| `cjk`            | *I18N. CJK. dll*          |
| `mideast`        | *I18N. Środkowy wschód. dll*      |
| `none` (wartość domyślna) | Brak                    |
| `other`          | *I18N. Inne. dll*        |
| `rare`           | *I18N. Rzadki. dll*         |
| `west`           | *I18N. Zachodni. dll*         |

Użyj przecinka, aby rozdzielić wiele wartości (na przykład `mideast,west`).

Aby uzyskać więcej informacji, zobacz [i18n: Pnetlib — Biblioteka struktury wielojęzycznej (repozytorium usługi GitHub/mono)](https://github.com/mono/mono/tree/master/mcs/class/I18N).
