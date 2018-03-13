---
title: Wprowadzenie do SignalR platformy ASP.NET Core
author: rachelappel
description: "Dowiedz się, jak biblioteka ASP.NET Core SignalR ułatwia dodawanie do aplikacji funkcji sieci web w czasie rzeczywistym."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction-signalr-core
ms.openlocfilehash: d4ad9bb1910a3339ac8d0d8ff740417f4e7262b7
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-signalr"></a>Wprowadzenie do SignalR

Przez [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Co to jest SignalR?

SignalR platformy ASP.NET Core to biblioteki, która ułatwia dodawanie funkcji sieci web w czasie rzeczywistym do aplikacji. Funkcje sieci web w czasie rzeczywistym umożliwia natychmiastowe kod po stronie serwera do wypychania zawartości do klientów.

Nadaje SignalR:

* Aplikacje, które wymagają wysokiej częstotliwości aktualizacji z serwera. Przykłady są gier, sieci społecznościowych głosowania, aukcji, map i GPS aplikacji.
* Pulpity nawigacyjne i monitorowania aplikacji. Przykładem mogą być firmy pulpity nawigacyjne, błyskawiczne aktualizacji sprzedaży, lub przesyłane alerty.
* Aplikacje współpracy. Tablica aplikacji i zespołu spotkania oprogramowania to przykłady współpracy aplikacji.
* Aplikacje, które wymagają powiadomienia. Sieci społecznościowych, poczty e-mail, rozmowy, gier, alerty podróży i wiele innych aplikacji użyj powiadomień.

Biblioteka SignalR udostępnia interfejs API do tworzenia klienta serwera [zdalnych wywołań procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). RPC wywołują funkcje JavaScript na komputerach klienckich z kodu .NET Core po stronie serwera.

Biblioteka SignalR dla platformy ASP.NET Core:

* Obsługuje zarządzanie połączeniami automatycznie.
* Umożliwia emituje wiadomości jednocześnie do wszystkich połączonych klientów. Na przykład pokoju rozmów.
* Umożliwia wysyłanie komunikatów do określonych klientów lub grupy klientów.
* Jest open-powierzając jej ich konserwację na [GitHub](https://github.com/aspnet/signalr).
* Skaluje dobrze.

Połączenie między klientem a serwerem jest trwała, w przeciwieństwie do połączenia HTTP.

## <a name="transports"></a>Transporty

Abstract SignalR przez kilka technik tworzenia aplikacji sieci web czasu rzeczywistego. [Protokół WebSockets](https://tools.ietf.org/html/rfc7118) jest optymalna transportu, ale innych technik, takich jak zdarzenia Server-Sent i długi sondowania można użyć w przypadku te nie są dostępne. SignalR automatycznie wykryje i zainicjować odpowiednie transportu oparte na funkcji serwera i klienta są obsługiwane.

## <a name="hubs-and-endpoints"></a>Koncentratory i punkty końcowe

Biblioteka SignalR używa punktów końcowych i koncentratory do komunikacji między klientami a serwerami. Interfejs API koncentratory obejmuje większości scenariuszy.

Koncentrator jest oparty na interfejs API punktu końcowego, który umożliwia klienta i serwera, wywoływanie metod od siebie wzajemnie potoku wysokiego poziomu. SignalR obsługuje wysyłki poza granicami maszyny automatycznie, co pozwala klientom wywoływać metod na serwerze jako łatwo jako metody lokalne i na odwrót. Koncentratory zezwala na przekazywanie jednoznacznie parametrów do metod, co pozwala wiązania modelu. Biblioteka SignalR udostępnia dwa protokoły wbudowanych koncentratora: protokół tekst na podstawie JSON i protokół binarny na podstawie [MessagePack](https://msgpack.org/).  MessagePack tworzy zazwyczaj wiadomości mniejszych niż przy użyciu formatu JSON. Starsze przeglądarki musi obsługiwać [XHR poziom 2](https://caniuse.com/#feat=xhr2) do obsługi protokołu MessagePack.

Koncentratory wywoływać kod po stronie klienta przez wysyłanie wiadomości przy użyciu aktywny transport. Komunikaty zawierają nazwę i parametry metody po stronie klienta. Obiekty wysyłane jako parametry metody są deserializacji za pomocą protokołu skonfigurowany. Klient próbuje jest zgodna z nazwą metody w kodzie po stronie klienta. W przypadku dopasowania w metodzie klienta jest wykonywane przy użyciu danych parametru zdeserializowany.

Punkty końcowe Podaj raw API przypominającej gniazda, włączanie ich do odczytu i zapisu z klienta. To deweloperom grupowania, emisji i inne funkcje. Interfejs API koncentratory jest oparty na warstwie punktów końcowych.

Na poniższym diagramie przedstawiono relację między koncentratorów, punktów końcowych i klientów.

![Mapa SignalR](introduction-signalr-core/_static/signalr-core-architecture.png)

## <a name="related-resources"></a>Zasoby pokrewne

[Rozpoczynanie pracy z SignalR dla platformy ASP.NET Core](xref:signalr/get-started-signalr-core)
