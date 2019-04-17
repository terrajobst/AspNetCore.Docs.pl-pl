---
title: Konfigurowanie konsolidatora dla Blazor
author: guardrex
description: Dowiedz się, jak kontrolować konsolidatora języka pośredniego (IL), podczas kompilowania aplikacji Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 01e18498a16e86392755b02b92ffda929669cb7d
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614839"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="c3f7f-103">Konfigurowanie konsolidatora dla Blazor</span><span class="sxs-lookup"><span data-stu-id="c3f7f-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="c3f7f-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c3f7f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="c3f7f-105">Wykonuje Blazor [języka pośredniego (IL)](/dotnet/standard/managed-code#intermediate-language--execution) łączenia podczas każdej kompilacji tryb wersji usuwanie niepotrzebnych IL aplikacji danych wyjściowych zestawów.</span><span class="sxs-lookup"><span data-stu-id="c3f7f-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="c3f7f-106">Zestaw Kontrola połączeń przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="c3f7f-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="c3f7f-107">Wyłącz konsolidowanie globalnie za pomocą [właściwość MSBuild](#disable-linking-with-a-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="c3f7f-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="c3f7f-108">Formant konsolidacji na podstawie na zestawie [pliku konfiguracyjnego](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="c3f7f-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="c3f7f-109">Wyłącz konsolidowanie za pomocą właściwości programu MSBuild</span><span class="sxs-lookup"><span data-stu-id="c3f7f-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="c3f7f-110">Łączenie jest domyślnie włączanych w trybie wydania kompilowana jest aplikacja, która obejmuje publikowania.</span><span class="sxs-lookup"><span data-stu-id="c3f7f-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="c3f7f-111">Aby wyłączyć łączenie dla wszystkich zestawów, należy ustawić `<BlazorLinkOnBuild>` właściwości programu MSBuild `false` w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="c3f7f-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="c3f7f-112">Łączenie się z plikiem konfiguracyjnym kontroli</span><span class="sxs-lookup"><span data-stu-id="c3f7f-112">Control linking with a configuration file</span></span>

<span data-ttu-id="c3f7f-113">Kontrola konsolidacji na podstawie poszczególnych zestawów, zapewniając plik konfiguracyjny XML i określenie pliku jako element programu MSBuild w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="c3f7f-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="c3f7f-114">*Linker.XML*:</span><span class="sxs-lookup"><span data-stu-id="c3f7f-114">*Linker.xml*:</span></span>

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

<span data-ttu-id="c3f7f-115">Aby uzyskać więcej informacji, zobacz [IL konsolidatora: Składnia xml deskryptora](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="c3f7f-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>
