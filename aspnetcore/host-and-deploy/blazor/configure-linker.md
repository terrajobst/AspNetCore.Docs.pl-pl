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
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a><span data-ttu-id="c5dfe-103">Skonfiguruj konsolidator dla ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="c5dfe-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="c5dfe-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c5dfe-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="c5dfe-105"> wykonuje konsolidację [języka pośredniego (IL)](/dotnet/standard/managed-code#intermediate-language--execution) podczas kompilacji, aby usunąć niepotrzebny kod IL z zestawów wyjściowych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c5dfe-105"> performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="c5dfe-106">Łączenie zestawu sterującego przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="c5dfe-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="c5dfe-107">Wyłącz konsolidację globalnie za pomocą [Właściwości programu MSBuild](#disable-linking-with-a-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="c5dfe-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="c5dfe-108">Kontrolowanie łączenia poszczególnych zestawów z [plikiem konfiguracyjnym](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="c5dfe-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="c5dfe-109">Wyłącz łączenie z właściwością programu MSBuild</span><span class="sxs-lookup"><span data-stu-id="c5dfe-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="c5dfe-110">Konsolidacja jest domyślnie włączona po skompilowaniu aplikacji, która obejmuje publikowanie.</span><span class="sxs-lookup"><span data-stu-id="c5dfe-110">Linking is enabled by default when an app is built, which includes publishing.</span></span> <span data-ttu-id="c5dfe-111">Aby wyłączyć łączenie dla wszystkich zestawów, ustaw właściwość `BlazorLinkOnBuild` MSBuild na `false` w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="c5dfe-111">To disable linking for all assemblies, set the `BlazorLinkOnBuild` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="c5dfe-112">Kontrola łączenia z plikiem konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c5dfe-112">Control linking with a configuration file</span></span>

<span data-ttu-id="c5dfe-113">Kontroluj łączenie dla poszczególnych zestawów, dostarczając plik konfiguracyjny XML i określając plik jako element programu MSBuild w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="c5dfe-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="c5dfe-114">*Konsolidator. XML*:</span><span class="sxs-lookup"><span data-stu-id="c5dfe-114">*Linker.xml*:</span></span>

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

<span data-ttu-id="c5dfe-115">Aby uzyskać więcej informacji, zobacz [Il konsolidator: Składnia deskryptora XML](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="c5dfe-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="c5dfe-116">Konfigurowanie konsolidatora dla celów wielojęzycznych</span><span class="sxs-lookup"><span data-stu-id="c5dfe-116">Configure the linker for internationalization</span></span>

<span data-ttu-id="c5dfe-117">Domyślnie konfiguracja konsolidatora Blazordla Blazor aplikacji webassembly umożliwia rozłączenie informacji o danych wielojęzycznych z wyjątkiem lokalizacji lokalnych jawnie żądanych.</span><span class="sxs-lookup"><span data-stu-id="c5dfe-117">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="c5dfe-118">Usunięcie tych zestawów minimalizuje rozmiar aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c5dfe-118">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="c5dfe-119">Aby kontrolować, które zestawy I18N są zachowywane, ustaw właściwość `<MonoLinkerI18NAssemblies>` MSBuild w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="c5dfe-119">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="c5dfe-120">Wartość regionu</span><span class="sxs-lookup"><span data-stu-id="c5dfe-120">Region Value</span></span>     | <span data-ttu-id="c5dfe-121">Zestaw regionów mono</span><span class="sxs-lookup"><span data-stu-id="c5dfe-121">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="c5dfe-122">Uwzględnione wszystkie zestawy</span><span class="sxs-lookup"><span data-stu-id="c5dfe-122">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="c5dfe-123">*I18N. CJK. dll*</span><span class="sxs-lookup"><span data-stu-id="c5dfe-123">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="c5dfe-124">*I18N. Środkowy wschód. dll*</span><span class="sxs-lookup"><span data-stu-id="c5dfe-124">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="c5dfe-125">`none` (wartość domyślna)</span><span class="sxs-lookup"><span data-stu-id="c5dfe-125">`none` (default)</span></span> | <span data-ttu-id="c5dfe-126">Brak</span><span class="sxs-lookup"><span data-stu-id="c5dfe-126">None</span></span>                    |
| `other`          | <span data-ttu-id="c5dfe-127">*I18N. Inne. dll*</span><span class="sxs-lookup"><span data-stu-id="c5dfe-127">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="c5dfe-128">*I18N. Rzadki. dll*</span><span class="sxs-lookup"><span data-stu-id="c5dfe-128">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="c5dfe-129">*I18N. Zachodni. dll*</span><span class="sxs-lookup"><span data-stu-id="c5dfe-129">*I18N.West.dll*</span></span>         |

<span data-ttu-id="c5dfe-130">Użyj przecinka, aby rozdzielić wiele wartości (na przykład `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="c5dfe-130">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="c5dfe-131">Aby uzyskać więcej informacji, zobacz [i18n: Pnetlib — Biblioteka struktury wielojęzycznej (repozytorium usługi GitHub/mono)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="c5dfe-131">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
