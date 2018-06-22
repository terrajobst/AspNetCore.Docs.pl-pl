---
title: Wprowadzenie do platformy ASP.NET Core SignalR
author: rachelappel
description: Dowiedz się, jak biblioteka ASP.NET Core SignalR ułatwia dodawanie w czasie rzeczywistym funkcjonalności do aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 0c833acea139d22883a69a02c2357a71f3ac8db8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277907"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Wprowadzenie do platformy ASP.NET Core SignalR

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
* Skalowalne.

Połączenie między klientem a serwerem jest trwała, w przeciwieństwie do połączenia HTTP.

## <a name="transports"></a>Transporty

Abstract SignalR przez kilka technik tworzenia aplikacji sieci web czasu rzeczywistego. [Protokół WebSockets](https://tools.ietf.org/html/rfc7118) jest optymalna transportu, ale innych technik, takich jak zdarzenia Server-Sent i długi sondowania można użyć w przypadku te nie są dostępne. SignalR automatycznie wykryje i zainicjować odpowiednie transportu oparte na funkcji serwera i klienta są obsługiwane.

## <a name="hubs"></a>Koncentratory

Biblioteka SignalR używa koncentratory do komunikacji między klientami a serwerami.

Koncentrator jest wysokiego poziomu potok, który umożliwia klienta i serwera, wywoływanie metod na siebie. SignalR obsługuje wysyłki poza granicami maszyny automatycznie, co pozwala klientom wywoływać metod na serwerze jako łatwo jako metody lokalne i na odwrót. Koncentratory zezwala na przekazywanie jednoznacznie parametrów do metod, co pozwala wiązania modelu. Biblioteka SignalR udostępnia dwa protokoły wbudowanych koncentratora: protokół tekst na podstawie JSON i protokół binarny na podstawie [MessagePack](https://msgpack.org/).  MessagePack tworzy zazwyczaj wiadomości mniejszych niż przy użyciu formatu JSON. Starsze przeglądarki musi obsługiwać [XHR poziom 2](https://caniuse.com/#feat=xhr2) do obsługi protokołu MessagePack.

Koncentratory wywoływać kod po stronie klienta przez wysyłanie wiadomości przy użyciu aktywny transport. Komunikaty zawierają nazwę i parametry metody po stronie klienta. Obiekty wysyłane jako parametry metody są deserializacji za pomocą protokołu skonfigurowany. Klient próbuje jest zgodna z nazwą metody w kodzie po stronie klienta. W przypadku dopasowania w metodzie klienta jest wykonywane przy użyciu danych parametru zdeserializowany.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Rozpoczynanie pracy z SignalR dla platformy ASP.NET Core](xref:tutorials/signalr)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
