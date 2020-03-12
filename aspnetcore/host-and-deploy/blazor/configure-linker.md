---
title: Skonfiguruj konsolidator dla ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak kontrolować konsolidator języka pośredniego (IL) podczas kompilowania aplikacji Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: b08ec26fb8d139223c57774600bc3cb19a56ac49
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083289"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>Skonfiguruj konsolidator dla ASP.NET Core Blazor

Autor [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor webassembly wykonuje konsolidację [języka pośredniego (IL)](/dotnet/standard/managed-code#intermediate-language--execution) podczas kompilacji, aby przyciąć niepotrzebny kod IL z zestawów wyjściowych aplikacji. Konsolidator jest wyłączony podczas kompilowania w konfiguracji debugowania. Aplikacje muszą kompilować w konfiguracji wydania, aby umożliwić konsolidator. Zalecamy Kompilowanie w wersji podczas wdrażania aplikacji webassembly Blazor. 

Łączenie aplikacji jest zoptymalizowane pod kątem rozmiaru, ale mogą one mieć szkodliwe skutki. Aplikacje korzystające z odbicia lub powiązane funkcje dynamiczne mogą być przerywane po przycięciu, ponieważ konsolidator nie wie o tym zachowaniu dynamicznym i nie może w ogóle określić, które typy są wymagane do odbicia w czasie wykonywania. Aby przyciąć takie aplikacje, konsolidator musi być informowany o wszelkich typach wymaganych przez odbicie w kodzie oraz w pakietach lub strukturach, od których zależy aplikacja. 

Aby upewnić się, że przycięta aplikacja działa prawidłowo po wdrożeniu, ważne jest, aby przetestować kompilacje wydań aplikacji często podczas opracowywania.

Łączenie aplikacji Blazor można skonfigurować za pomocą tych funkcji MSBuild:

* Skonfiguruj konsolidację globalnie za pomocą [Właściwości programu MSBuild](#control-linking-with-an-msbuild-property).
* Kontrolowanie łączenia poszczególnych zestawów z [plikiem konfiguracyjnym](#control-linking-with-a-configuration-file).

## <a name="control-linking-with-an-msbuild-property"></a>Sterowanie łączeniem z właściwością programu MSBuild

Łączenie jest włączone, gdy aplikacja jest wbudowana w `Release` konfiguracji. Aby to zmienić, skonfiguruj Właściwość `BlazorWebAssemblyEnableLinking` MSBuild w pliku projektu:

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
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

Domyślnie konfiguracja konsolidatora Blazor dla aplikacji Blazor webassembly umożliwia rozłączenie informacji o danych wielojęzycznych z wyjątkiem lokalizacji lokalnych jawnie żądanych. Usunięcie tych zestawów minimalizuje rozmiar aplikacji.

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
