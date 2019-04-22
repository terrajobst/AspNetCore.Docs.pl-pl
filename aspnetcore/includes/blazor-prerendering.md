---
ms.openlocfilehash: 03ed846171097e30e4954cf632875c0249697665
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983063"
---
Podczas aplikacji po stronie serwera Blazor jest prerendering, nie są określone akcje, takie jak wywołanie JavaScript, z możliwe, ponieważ nie został utworzony połączenia za pośrednictwem przeglądarki. Składniki może być konieczne renderowania inaczej, gdy prerendered.

Opóźnienie JavaScript interop wywołuje aż po nawiązaniu połączenia za pośrednictwem przeglądarki, możesz użyć `OnAfterRenderAsync` zdarzenia cyklu życia składników. To zdarzenie jest wywoływane tylko, po aplikacji jest w pełni renderowany i nawiązaniu połączenia klienta.

Aby warunkowo renderować różną zawartość w oparciu o tego, czy aplikacja jest obecnie prerendering zawartości, należy użyć `IsConnected` właściwość `IComponentContext` usługi. Podczas uruchamiania po stronie serwera, `IsConnected` zwraca tylko `true` w przypadku aktywnego połączenia do klienta. Zawsze zwraca `true` podczas uruchamiania po stronie klienta.

```cshtml
@page "/isconnected-example"
@using Microsoft.AspNetCore.Components.Services
@inject IComponentContext ComponentContext

<h1>IsConnected Example</h1>

<p>
    Current state:
    <strong id="connected-state">
        @(ComponentContext.IsConnected ? "connected" : "not connected")
    </strong>
</p>

<p>
    Clicks:
    <strong id="count">@count</strong>
    <button id="increment-count" onclick="@(() => count++)">Click me</button>
</p>

@functions {
    private int count;
}
```
