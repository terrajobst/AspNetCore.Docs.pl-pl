---
title: ASP.NET Core Web SDK
author: Rick-Anderson
description: Omówienie Microsoft. NET. Sdk. Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 49e21e1a432149409a01550452cedf4009dcfba7
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830675"
---
# <a name="aspnet-core-web-sdk"></a>ASP.NET Core Web SDK

### <a name="overview"></a>Omówienie

`Microsoft.NET.Sdk.Web` to [zestaw SDK projektu MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) do kompilowania aplikacji ASP.NET Core. Istnieje możliwość utworzenia aplikacji ASP.NET Core bez tego zestawu SDK, ale zestaw SDK sieci Web:

* Dostosowany do zapewniania środowiska pierwszej klasy.
* Zalecany element docelowy dla większości użytkowników.

Użyj zestawu Web. SDK w projekcie:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

Funkcje włączone przy użyciu zestawu SDK sieci Web:

* Projekty ukierunkowane na platformę .NET Core 3,0 lub nowsze niejawnie:

  * [ASP.NET Core udostępnionej platformy](xref:fundamentals/metapackage-app).
  * [Analizatory](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) zaprojektowane do kompilowania aplikacji ASP.NET Core.
* WebSDK włącza cele programu MSBuild, które umożliwiają korzystanie z profilów publikowania i publikowanie za pomocą narzędzia webdeploy.

### <a name="properties"></a>Właściwości

| Właściwość | Opis |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | Wyłącza niejawne odwołanie do `Microsoft.AspNetCore.App` udostępnionej platformy. |
| `DisableImplicitAspNetCoreAnalyzers` | Wyłącza niejawne odwołanie do analizatorów ASP.NET Core. |
| `DisableImplicitComponentsAnalyzers` | Wyłącza niejawne odwołanie do analizatorów składników Razor podczas kompilowania aplikacji Blazor (serwerowych). |
