---
title: Wprowadzenie do SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak biblioteki biblioteki SignalR platformy ASP.NET Core ułatwia dodawanie funkcji w czasie rzeczywistym do aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095392"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Wprowadzenie do SignalR platformy ASP.NET Core

Przez [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Co to jest SignalR?

SignalR platformy ASP.NET Core to bibliotekę, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji. Funkcje sieci web w czasie rzeczywistym umożliwia kodu po stronie serwera do wypychania zawartości do klientów natychmiast.

Dobrymi kandydatami do SignalR:

* Aplikacje, które wymagają wysokiej częstotliwości aktualizacji z serwera. Przykładami są gier, sieci społecznościowych, głosowanie, aukcji, map i aplikacji GPS.
* Pulpity nawigacyjne i monitorowania aplikacji. Przykłady obejmują firmowe pulpity nawigacyjne natychmiastowej aktualizacji sprzedaży, lub przesyłane alerty.
* Aplikacje współpracy. Aplikacje tablicy i zespołu spotkania oprogramowania to przykłady aplikacji współpracy.
* Aplikacje, które wymagają powiadomienia. Użycie powiadomień, sieci społecznościowych, poczty e-mail, rozmowy, gry, podróży alertów i wielu innych aplikacji.

Biblioteka SignalR udostępnia interfejs API do tworzenia klienta serwera [zdalnych wywołań procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Zdalnych wywołań procedury wywołują funkcje JavaScript na komputerach klienckich z kodu platformy .NET Core po stronie serwera.

Biblioteka SignalR dla programu ASP.NET Core:

* Automatycznie obsługuje zarządzanie połączeniami.
* Włącza emituje komunikaty do wszystkich połączonych klientów jednocześnie. Na przykład pokoju rozmów.
* Umożliwia wysyłanie komunikatów do określonych klientów lub grup klientów.
* To open source w [GitHub](https://github.com/aspnet/signalr).
* Skalowalność.

Połączenie między klientem i serwerem jest trwała, w przeciwieństwie do połączeń HTTP.

## <a name="transports"></a>Transporty

Abstract SignalR kilka technik do tworzenia aplikacji internetowych w czasie rzeczywistym. [Gniazda Websocket](https://tools.ietf.org/html/rfc7118) jest optymalne transportu, ale można użyć innych technik, takich jak zdarzenia Server-Sent i długi sondowania, gdy te nie są dostępne. SignalR automatycznie wykryje i zainicjować odpowiednie transportu, oparte na funkcji serwera i klienta są obsługiwane.

## <a name="hubs"></a>Koncentratory

SignalR używa koncentratory do komunikacji między klientami a serwerami.

Centrum to wysokiego poziomu potok, który umożliwia klienta i serwera, wywoływanie metod na siebie nawzajem. SignalR obsługuje wysyłania w granicach maszyny automatycznie, umożliwiając klientom wywoływać metody na serwerze, jak łatwo jako metody lokalne i na odwrót. Koncentratory umożliwiają przekazywania silnie typizowane parametry do metod, co pozwala wiązania modelu. Biblioteka SignalR udostępnia dwa protokoły wbudowanych Centrum: protokół tekstowy dostępny na podstawie JSON i binarny protokołu, na podstawie [MessagePack](https://msgpack.org/).  MessagePack zazwyczaj tworzy wiadomości mniejszych niż przy użyciu formatu JSON. Starsze przeglądarki musi obsługiwać [XHR poziom 2](https://caniuse.com/#feat=xhr2) do obsługi protokołu MessagePack.

Koncentratory wywołać kod po stronie klienta, wysyłając komunikatów za pomocą aktywny transport. Komunikaty zawierają nazwę i parametry metody po stronie klienta. Wysyłane jako parametry metody obiekty są deserializacji za pomocą skonfigurowanego protokołu. Klient próbuje dopasować nazwę metody w kodzie po stronie klienta. W przypadku dopasowania metoda klienta uruchamia się przy użyciu danych zdeserializowany parametrów.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do SignalR dla platformy ASP.NET Core](xref:tutorials/signalr)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
