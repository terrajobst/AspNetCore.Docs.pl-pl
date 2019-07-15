---
title: Hostowanie i wdrażanie platformy ASP.NET Core Blazor po stronie serwera
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie aplikacji po stronie serwera Blazor, przy użyciu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 56a03ff583bf85497e2b3bacc70123845a046e3d
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67892695"
---
# <a name="host-and-deploy-blazor-server-side"></a>Hostowanie i wdrażanie Blazor po stronie serwera

Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Wartości konfiguracji hosta

Aplikacje serwerowe, które używają [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side) może akceptować [wartości konfiguracji hosta ogólny](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>wdrażania

Za pomocą [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side), Blazor jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.

Wymagany jest serwer sieci web, zdolne do obsługi aplikacji ASP.NET Core. Program Visual Studio obejmuje **Blazor Server App** szablonu projektu (`blazorserverside` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenie).

## <a name="connection-scale-out"></a>Skalowanie do połączenia

Aplikacje serwerowe Blazor wymagają jednego aktywnego połączenia SignalR dla każdego użytkownika. Produkcyjne wdrożenie po stronie serwera Blazor wymaga rozwiązanie do obsługi dowolnej liczby jednoczesnych połączeń, zgodnie z wymaganiami aplikacji. [Usługi Azure SignalR Service](/azure/azure-signalr/) obsługuje skalowanie połączeń i jest zalecana jako rozwiązanie skalowania dla aplikacji po stronie serwera Blazor. Aby uzyskać więcej informacji, zobacz <xref:signalr/publish-to-azure-web-app>.

## <a name="signalr-configuration"></a>Konfiguracja SignalR

Dla najbardziej typowych scenariuszy serwerowych Blazor skonfigurowano SignalR, ASP.NET Core. Niestandardowe i zaawansowane scenariusze, można znaleźć w artykułach SignalR w [dodatkowe zasoby](#additional-resources) sekcji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/introduction>
* [Dokumentacja usługi Azure SignalR Service](/azure/azure-signalr/)
* [Szybki start: Tworzenie pokoju rozmów przy użyciu usługi SignalR](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
