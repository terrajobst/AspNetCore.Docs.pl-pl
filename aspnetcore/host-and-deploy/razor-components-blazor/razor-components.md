---
title: Hostowanie i wdrażanie składników Razor
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie aplikacji składniki Razor przy użyciu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/razor-components
ms.openlocfilehash: 18b09dfc80e2f517c7750ce0fe1510641624aea7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069775"
---
# <a name="host-and-deploy-razor-components"></a>Hostowanie i wdrażanie składników Razor

Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Wartości konfiguracji hosta

Razor składniki aplikacje, które używają [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model) może akceptować [wartości konfiguracji hosta ogólny](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>wdrażania

Za pomocą [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model), składniki Razor jest wykonywane na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.

Aplikacja jest dołączony do aplikacji ASP.NET Core w opublikowanych danych wyjściowych, a dwie aplikacje wdrażane razem. Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core. Wdrożenia po stronie serwera, program Visual Studio obejmuje **składniki Razor** szablonu projektu (`razorcomponents` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.

Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.
