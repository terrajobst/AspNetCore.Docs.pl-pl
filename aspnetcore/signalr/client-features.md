---
title: Funkcje klienta sygnalizującego
author: bradygaster
description: Dowiedz się, które funkcje są obsługiwane przez różnych klientów ASP.NET Core sygnalizujących.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 55086673e0c9f9b73f07730ea25c3fa322f7fd98
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187468"
---
# <a name="aspnet-core-signalr-client-features"></a>Funkcje klienta sygnalizującego ASP.NET Core

## <a name="feature-distribution"></a>Dystrybucja funkcji

W poniższej tabeli przedstawiono funkcje i obsługę klientów, którzy oferują pomoc techniczną w czasie rzeczywistym.

| Funkcja | .NET Core | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| [Przesyłanie strumieniowe między serwerami i klientami](xref:signalr/streaming)          |✔|✔|✔|
| [Przesyłanie strumieniowe klient-serwer](xref:signalr/streaming)          |✔|✔|✔|
| Automatyczne ponowne łączenie ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do programu sygnalizującego dla ASP.NET Core](xref:tutorials/signalr)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
