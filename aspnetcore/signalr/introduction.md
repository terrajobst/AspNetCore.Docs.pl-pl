---
title: Wprowadzenie do SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się, jak biblioteki biblioteki SignalR platformy ASP.NET Core ułatwia dodawanie funkcji w czasie rzeczywistym do aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 673efafce60dfa46cb99f9537fda2bca42bf9822
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898753"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Wprowadzenie do SignalR platformy ASP.NET Core

## <a name="what-is-signalr"></a>Co to jest SignalR?

SignalR platformy ASP.NET Core to biblioteka typu open source, która ułatwia dodawanie funkcji internetowych w czasie rzeczywistym do aplikacji. Funkcje sieci web w czasie rzeczywistym umożliwia kodu po stronie serwera do wypychania zawartości do klientów natychmiast.

Dobrymi kandydatami do SignalR:

* Aplikacje, które wymagają wysokiej częstotliwości aktualizacji z serwera. Przykładami są gier, sieci społecznościowych, głosowanie, aukcji, map i aplikacji GPS.
* Pulpity nawigacyjne i monitorowania aplikacji. Przykłady obejmują firmowe pulpity nawigacyjne natychmiastowej aktualizacji sprzedaży, lub przesyłane alerty.
* Aplikacje współpracy. Aplikacje tablicy i zespołu spotkania oprogramowania to przykłady aplikacji współpracy.
* Aplikacje, które wymagają powiadomienia. Użycie powiadomień, sieci społecznościowych, poczty e-mail, rozmowy, gry, podróży alertów i wielu innych aplikacji.

Biblioteka SignalR udostępnia interfejs API do tworzenia klienta serwera [zdalnych wywołań procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Zdalnych wywołań procedury wywołują funkcje JavaScript na komputerach klienckich z kodu platformy .NET Core po stronie serwera.

Poniżej przedstawiono niektóre funkcje SignalR dla platformy ASP.NET Core:

* Automatycznie obsługuje zarządzanie połączeniami.
* Wysyła komunikaty do wszystkich połączonych klientów jednocześnie. Na przykład pokoju rozmów.
* Wysyła komunikaty do określonych klientów lub grup klientów.
* Możliwość skalowania do obsługi ruchu rosnąca.

Źródło znajduje się w [SignalR repozytorium w serwisie GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).

## <a name="transports"></a>Transporty

SignalR obsługuje kilka technik do obsługi komunikacji w czasie rzeczywistym:

* [Obiekty WebSocket](https://tools.ietf.org/html/rfc7118)
* Zdarzenia wysłanego przez serwer
* Długiego sondowania

SignalR automatycznie wybiera najlepszą metodę transportu, który znajduje się w funkcji serwera i klienta.

## <a name="hubs"></a>Centra

Korzysta z biblioteki SignalR *koncentratory* do komunikacji między klientami a serwerami.

Centrum to wysokiego poziomu potok, który umożliwia klientowi i serwerowi wywoływać metody dla siebie nawzajem. SignalR obsługuje wysyłania w granicach maszyny automatycznie, umożliwiając klientom wywoływać metody na serwerze i na odwrót. Silnie typizowane parametry można przekazać do metody, które umożliwia wiązania modelu. Biblioteka SignalR udostępnia dwa protokoły wbudowanych Centrum: protokół tekstowy dostępny na podstawie JSON i binarny protokołu, na podstawie [MessagePack](https://msgpack.org/).  MessagePack zazwyczaj tworzy mniejsze wiadomości w porównaniu do formatu JSON. Starsze przeglądarki musi obsługiwać [XHR poziom 2](https://caniuse.com/#feat=xhr2) do obsługi protokołu MessagePack.

Koncentratory wywołać kod po stronie klienta, wysyłając komunikaty, które zawierają nazwę i parametry metody po stronie klienta. Wysyłane jako parametry metody obiekty są deserializacji za pomocą skonfigurowanego protokołu. Klient próbuje dopasować nazwę metody w kodzie po stronie klienta. Jeśli klient znajdzie dopasowanie, wywołuje metodę i przekazuje do niego dane parametru po deserializacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do SignalR dla platformy ASP.NET Core](xref:tutorials/signalr)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
