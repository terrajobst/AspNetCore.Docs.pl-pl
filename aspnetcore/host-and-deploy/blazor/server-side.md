---
title: Hostowanie i wdrażanie Blazor po stronie serwera
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie aplikacji po stronie serwera Blazor, przy użyciu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 39e4f0634d40e8c720fa5a6a76bc0843487a4e3c
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982591"
---
# <a name="host-and-deploy-blazor-server-side"></a>Hostowanie i wdrażanie Blazor po stronie serwera

Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Wartości konfiguracji hosta

Aplikacje serwerowe, które używają [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side) może akceptować [wartości konfiguracji hosta ogólny](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>wdrażania

Za pomocą [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side), Blazor jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.

Aplikacja jest dołączony do aplikacji ASP.NET Core w opublikowanych danych wyjściowych, a dwie aplikacje wdrażane razem. Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core. Wdrożenia po stronie serwera, program Visual Studio obejmuje **Blazor (po stronie serwera)** szablonu projektu (`blazorserverside` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenie).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.

Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.
