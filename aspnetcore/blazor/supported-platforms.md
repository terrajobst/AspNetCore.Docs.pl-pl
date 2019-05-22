---
title: Platformy obsługiwane przez Blazor platformy ASP.NET Core
author: guardrex
description: Dowiedz się więcej o obsługiwanych platform dla platformy ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/21/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 12ef3885044cfca17c2e7ffdb248fdebef26c48a
ms.sourcegitcommit: e67356f5e643a5d43f6d567c5c998ce6002bdeb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005346"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Platformy obsługiwane przez Blazor platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Wymagania dotyczące przeglądarki

### <a name="blazor-client-side"></a>Blazor po stronie klienta

| Przeglądarka                          | Wersja               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | bieżący               |
| Mozilla Firefox                  | bieżący               |
| Google Chrome, łącznie z systemami Android | bieżący               |
| Safari, łącznie z systemem iOS            | bieżący               |
| Microsoft Internet Explorer      | Nie jest obsługiwany&dagger; |

&dagger;Program Microsoft Internet Explorer nie obsługuje [format WebAssembly](http://webassembly.org).

Aby uzyskać więcej informacji na temat modelu hostingu Blazor po stronie klienta, zobacz <xref:blazor/hosting-models#client-side>.

### <a name="blazor-server-side"></a>Blazor po stronie serwera

| Przeglądarka                          | Wersja    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | bieżący    |
| Mozilla Firefox                  | bieżący    |
| Google Chrome, łącznie z systemami Android | bieżący    |
| Safari, łącznie z systemem iOS            | bieżący    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Wymagane są dodatkowe polyfills (na przykład można dodać obietnic za pośrednictwem [Polyfill.io](https://polyfill.io/v3/) pakietu).

Aby uzyskać więcej informacji na temat modelu hostingu Blazor po stronie serwera, zobacz <xref:blazor/hosting-models#server-side>.
