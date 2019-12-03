---
title: Co nowego w ASP.NET Core 3,1
author: rick-anderson
description: Dowiedz się więcej o nowych funkcjach w ASP.NET Core 3,1.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: 4737747f84a59780fe70f63195f7580bd812e4de
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734027"
---
# <a name="whats-new-in-aspnet-core-31"></a>Co nowego w ASP.NET Core 3,1

W tym artykule przedstawiono najbardziej znaczące zmiany w ASP.NET Core 3,1 z linkami do odpowiedniej dokumentacji.

## <a name="partial-class-support-for-razor-components"></a>Obsługa częściowej klasy dla składników Razor

Składniki Razor są teraz generowane jako klasy częściowe. Kod dla składnika Razor można napisać przy użyciu pliku związanego z kodem zdefiniowanego jako Klasa częściowa zamiast definiować cały kod dla składnika w pojedynczym pliku. Aby uzyskać więcej informacji, zobacz temat [Obsługa klasy częściowej](xref:blazor/components#partial-class-support).

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor pomocnika tagów składników i przekazywania parametrów do składników najwyższego poziomu

W Blazor z ASP.NET Core 3,0 składniki były renderowane na stronach i w widokach za pomocą pomocnika HTML (`Html.RenderComponentAsync`). W ASP.NET Core 3,1 Renderuj składnik ze strony lub widoku przy użyciu nowego pomocnika tagów składnika:

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

Pomocnik HTML pozostaje obsługiwany w ASP.NET Core 3,1, ale zaleca się pomocnika tagów składnika.

aplikacje serwera Blazor umożliwiają teraz przekazywanie parametrów do składników najwyższego poziomu podczas początkowego renderowania. Wcześniej można było przekazać parametry do składnika najwyższego poziomu za pomocą elementu [RenderMode. static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static). W tej wersji obsługiwane są zarówno metody [RenderMode. Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) , jak i [RenderModel. ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) . Wszystkie określone wartości parametrów są serializowane jako kod JSON i zawarte w początkowej odpowiedzi.

Na przykład wyprerender składnik `Counter` z ilością przyrostu (`IncrementAmount`):

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Aby uzyskać więcej informacji, zobacz [integrowanie składników w aplikacjach Razor Pages i MVC](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).

## <a name="support-for-shared-queues-in-httpsys"></a>Obsługa kolejek udostępnionych w pliku HTTP. sys

[Http. sys](xref:fundamentals/servers/httpsys) obsługuje tworzenie anonimowych kolejek żądań. W ASP.NET Core 3,1 dodaliśmy do możliwości tworzenia lub dołączania istniejącej nazwanej kolejki żądań HTTP. sys. Utworzenie lub dołączenie istniejącej kolejki żądań HTTP. sys umożliwia scenariusze, w których jest to protokół HTTP. Proces kontrolera sys, który jest właścicielem kolejki, jest niezależny od procesu odbiornika. Ta niezależność umożliwia zachowywanie istniejących połączeń i zakolejce żądań między ponownymi uruchomieniami procesu odbiornika:

[!code-csharp[](sample/Program.cs?name=snippet)]

<!-- TODO
## Breaking changes for SameSite cookies
-->

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a>Zapobiegaj domyślnym akcjom dla zdarzeń w aplikacjach Blazor

Użyj atrybutu dyrektywy `@on{EVENT}:preventDefault`, aby zapobiec domyślnej akcji dla zdarzenia. W poniższym przykładzie jest blokowane domyślne działanie wyświetlania znaku klucza w polu tekstowym:

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

Aby uzyskać więcej informacji, zobacz [zapobieganie domyślnym akcjom](xref:blazor/components#prevent-default-actions).

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a>Zatrzymaj propagację zdarzeń w aplikacjach Blazor

Użyj atrybutu dyrektywy `@on{EVENT}:stopPropagation`, aby zatrzymać propagację zdarzeń. W poniższym przykładzie, zaznaczając pole wyboru, Zapobiegaj kliknięciu zdarzeń z elementu podrzędnego `<div>` od propagowania do `<div>`nadrzędnego:

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

Aby uzyskać więcej informacji, zobacz sekcję [Zatrzymaj propagację zdarzeń](xref:blazor/components#stop-event-propagation).

## <a name="detailed-errors-during-opno-locblazor-app-development"></a>Szczegóły błędów podczas tworzenia aplikacji Blazor

Gdy aplikacja Blazor nie działa prawidłowo podczas opracowywania, otrzymywanie szczegółowych informacji o błędach z aplikacji pomaga w rozwiązywaniu problemów i rozwiązaniu problemu. Gdy wystąpi błąd, Blazor aplikacje wyświetlają złoty pasek u dołu ekranu:

* W trakcie programowania złoty pasek kieruje użytkownika do konsoli przeglądarki, gdzie można zobaczyć wyjątek.
* W środowisku produkcyjnym złoty pasek powiadamia użytkownika o wystąpieniu błędu i zaleca odświeżenie przeglądarki.

Aby uzyskać więcej informacji, zobacz [szczegóły błędów podczas opracowywania](xref:blazor/handle-errors#detailed-errors-during-development).
