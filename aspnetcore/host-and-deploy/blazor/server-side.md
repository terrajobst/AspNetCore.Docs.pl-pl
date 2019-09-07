---
title: Hostowanie i wdrażanie ASP.NET Core Blazor po stronie serwera
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor po stronie serwera przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8da71faf6abc5929d6cd43d42fd896e378d99ef6
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773570"
---
# <a name="host-and-deploy-blazor-server-side"></a>Hostowanie i wdrażanie Blazor po stronie serwera

[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Wartości konfiguracji hosta

Aplikacje po stronie serwera, które używają [modelu hostingu po stronie serwera](xref:blazor/hosting-models#server-side) , mogą akceptować [ogólne wartości konfiguracji hosta](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>wdrażania

[Model hostingu po stronie serwera](xref:blazor/hosting-models#server-side)Blazor jest wykonywany na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [sygnalizujące](xref:signalr/introduction) .

Wymagany jest serwer sieci Web obsługujący aplikację ASP.NET Core. Program Visual Studio zawiera szablon projektu **aplikacji Blazor Server** (`blazorserverside` szablon w przypadku używania polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ).

## <a name="connection-scale-out"></a>Skalowanie w poziomie połączenia

Blazor aplikacje po stronie serwera wymagają jednego aktywnego połączenia sygnalizującego dla każdego użytkownika. Wdrożenie po stronie serwera produkcyjnego Blazor wymaga rozwiązania do obsługi tylu połączeń współbieżnych wymaganych przez aplikację. [Usługa Azure Signal](/azure/azure-signalr/) obsługuje skalowanie połączeń i jest zalecana jako rozwiązanie do skalowania dla aplikacji po stronie serwera Blazor. Aby uzyskać więcej informacji, zobacz <xref:signalr/publish-to-azure-web-app>.

## <a name="signalr-configuration"></a>Konfiguracja sygnalizującego

Program sygnalizujący jest konfigurowany przez ASP.NET Core dla najpopularniejszych scenariuszy po stronie serwera Blazor. W przypadku scenariuszy niestandardowych i zaawansowanych zapoznaj się z artykułami dotyczącymi sygnałów w sekcji [dodatkowe zasoby](#additional-resources) .

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/introduction>
* [Dokumentacja usługi Azure sygnalizującego](/azure/azure-signalr/)
* [Szybki start: Tworzenie pokoju rozmów przy użyciu usługi sygnalizującej](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Wdróż ASP.NET Core wersji zapoznawczej do Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
