---
ms.openlocfilehash: 03ed846171097e30e4954cf632875c0249697665
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983063"
---
<span data-ttu-id="51547-101">Podczas aplikacji po stronie serwera Blazor jest prerendering, nie są określone akcje, takie jak wywołanie JavaScript, z możliwe, ponieważ nie został utworzony połączenia za pośrednictwem przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="51547-101">While a Blazor server-side app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="51547-102">Składniki może być konieczne renderowania inaczej, gdy prerendered.</span><span class="sxs-lookup"><span data-stu-id="51547-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="51547-103">Opóźnienie JavaScript interop wywołuje aż po nawiązaniu połączenia za pośrednictwem przeglądarki, możesz użyć `OnAfterRenderAsync` zdarzenia cyklu życia składników.</span><span class="sxs-lookup"><span data-stu-id="51547-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="51547-104">To zdarzenie jest wywoływane tylko, po aplikacji jest w pełni renderowany i nawiązaniu połączenia klienta.</span><span class="sxs-lookup"><span data-stu-id="51547-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

<span data-ttu-id="51547-105">Aby warunkowo renderować różną zawartość w oparciu o tego, czy aplikacja jest obecnie prerendering zawartości, należy użyć `IsConnected` właściwość `IComponentContext` usługi.</span><span class="sxs-lookup"><span data-stu-id="51547-105">To conditionally render different content based on whether the app is currently prerendering content, use the `IsConnected` property on the `IComponentContext` service.</span></span> <span data-ttu-id="51547-106">Podczas uruchamiania po stronie serwera, `IsConnected` zwraca tylko `true` w przypadku aktywnego połączenia do klienta.</span><span class="sxs-lookup"><span data-stu-id="51547-106">When running server-side, `IsConnected` only returns `true` if there's an active connection to the client.</span></span> <span data-ttu-id="51547-107">Zawsze zwraca `true` podczas uruchamiania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="51547-107">It always returns `true` when running client-side.</span></span>

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
