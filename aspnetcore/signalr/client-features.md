---
title: Funkcje klienta SignalR
author: bradygaster
description: Dowiedz się, które funkcje są obsługiwane przez różnych klientów ASP.NET Core sygnalizujących.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301190"
---
# <a name="aspnet-core-signalr-client-features"></a>Funkcje klienta sygnalizującego ASP.NET Core

## <a name="feature-distribution"></a>Dystrybucja funkcji

W poniższej tabeli przedstawiono funkcje i obsługę klientów, którzy oferują pomoc techniczną w czasie rzeczywistym.

| Funkcja | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Obsługa usługi sygnałów platformy Azure |✔|✔|✔|
| [Przesyłanie strumieniowe między serwerami i klientami](xref:signalr/streaming)          |✔|✔|✔|
| [Przesyłanie strumieniowe klient-serwer](xref:signalr/streaming)          |✔|✔|✔|
| Automatyczne ponowne łączenie ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |
| Transport gniazd WebSockets |✔|✔|✔|
| Transport zdarzeń wysłanych przez serwer |✔|✔| |
| Długotrwały transport sondowania |✔|✔|✔|
| Protokół centrum JSON |✔|✔|✔|
| Protokół centrum MessagePack |✔|✔| |

Obsługa automatycznego ponownego łączenia w kliencie Java jest śledzona w [naszym monitorze problemów](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do programu sygnalizującego dla ASP.NET Core](xref:tutorials/signalr)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
