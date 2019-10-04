---
title: Funkcje klienta SignalR
author: bradygaster
description: Dowiedz się, które funkcje są obsługiwane przez różnych klientów ASP.NET Core sygnalizujących.
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 6718722cdbcfae500026fcd429ca6b9de8f0f103
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925356"
---
# <a name="aspnet-core-signalr-client-features"></a>Funkcje klienta sygnalizującego ASP.NET Core

## <a name="feature-distribution"></a>Dystrybucja funkcji

W poniższej tabeli przedstawiono funkcje i obsługę klientów, którzy oferują pomoc techniczną w czasie rzeczywistym. Dla każdej funkcji jest wyświetlana *minimalna* wersja obsługująca tę funkcję. Jeśli żadna wersja nie jest wymieniona, ta funkcja nie jest obsługiwana.

| Cecha | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Obsługa usługi sygnałów platformy Azure |1.0.0|1.0.0|1.0.0|
| [Przesyłanie strumieniowe między serwerami i klientami](xref:signalr/streaming)          |1.0.0|1.0.0|1.0.0|
| [Przesyłanie strumieniowe klient-serwer](xref:signalr/streaming)          |3.0.0|3.0.0|3.0.0|
| Automatyczne ponowne łączenie ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |3.0.0|3.0.0|❌|
| Transport gniazd WebSockets |1.0.0|1.0.0|1.0.0|
| Transport zdarzeń wysłanych przez serwer |1.0.0|1.0.0|❌|
| Długotrwały transport sondowania |1.0.0|1.0.0|3.0.0|
| Protokół centrum JSON |1.0.0|1.0.0|1.0.0|
| Protokół centrum MessagePack |1.0.0|1.0.0|❌|

Obsługa automatycznego ponownego łączenia w kliencie Java jest śledzona w [naszym monitorze problemów](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do programu sygnalizującego dla ASP.NET Core](xref:tutorials/signalr)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
