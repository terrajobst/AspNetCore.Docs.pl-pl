---
title: Wprowadzenie do ASP.NET Core SignalR
author: bradygaster
description: Dowiedz się, w jaki sposób biblioteka SignalR ASP.NET Core upraszcza Dodawanie funkcji do aplikacji w czasie rzeczywistym.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/27/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 635431abf9263c2dff261aea47e6f8324061763f
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829286"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a>Wprowadzenie do ASP.NET Core SignalR

## <a name="what-is-opno-locsignalr"></a>Co to jest usługa SignalR?

ASP.NET Core SignalR jest biblioteką Open Source, która upraszcza Dodawanie funkcji sieci Web w czasie rzeczywistym do aplikacji. Funkcja sieci Web w czasie rzeczywistym umożliwia serwerowi natychmiastowe wypychanie zawartości do klientów.

Dobre kandydaci dla SignalR:

* Aplikacje, które wymagają częstych aktualizacji z serwera. Są to na przykład gry i aplikacje do obsługi sieci społecznościowych, głosowania, aukcji, map i nawigacji GPS.
* Pulpity nawigacyjne i aplikacje do monitorowania. Przykładami mogą być firmowe pulpity nawigacyjne, błyskawiczne powiadomienia o sprzedaży lub alerty dotyczące podróży.
* Aplikacje do współpracy. Przykładami aplikacji do współpracy są aplikacje tablicy i oprogramowanie do spotkań zespołowych.
* Aplikacje, które wymagają powiadomień. Wiele aplikacji korzysta z powiadomień, w tym gry i aplikacje do obsługi sieci społecznościowych, poczty e-mail, czatów i alertów dotyczących podróży.

SignalR udostępnia interfejs API do tworzenia [zdalnych wywołań procedur serwer-klient (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Wywołania RPC wywołują funkcje JavaScript na klientach z kodu .NET Core po stronie serwera.

Poniżej przedstawiono niektóre funkcje SignalR dla ASP.NET Core:

* Obsługuje automatyczne zarządzanie połączeniami.
* Wysyła komunikaty do wszystkich połączonych klientów jednocześnie. Na przykład pokój rozmów.
* Wysyła komunikaty do określonych klientów lub grup klientów.
* Skaluje się do obsługi zwiększania ruchu.

Źródło jest hostowane w [repozytoriumSignalR w serwisie GitHub](https://github.com/dotnet/AspNetCore/tree/master/src/SignalR).

## <a name="transports"></a>Transporty

SignalR obsługuje następujące techniki obsługi komunikacji w czasie rzeczywistym (w kolejności łagodnej powrotu):

* [Obiekty WebSocket](https://tools.ietf.org/html/rfc7118)
* Zdarzenia wysłane przez serwer
* Długotrwałe sondowanie

SignalR automatycznie wybiera najlepszą metodę transportu, która znajduje się w możliwościach serwera i klienta.

## <a name="hubs"></a>Centra

SignalR używa *centrów* do komunikacji między klientami a serwerami.

Koncentrator jest potokiem wysokiego poziomu, który umożliwia klientowi i serwerowi wywoływanie metod ze sobą. SignalR obsługuje automatyczne wysyłanie między granicami maszyn, umożliwiając klientom wywoływanie metod na serwerze i odwrotnie. Parametry z jednoznacznie określonymi typami można przekazać do metod, które umożliwiają powiązanie modelu. SignalR oferuje dwa wbudowane protokoły centrum: protokół tekstowy oparty na formacie JSON i protokół binarny oparty na [MessagePack](https://msgpack.org/).  MessagePack zazwyczaj tworzy mniejsze komunikaty w porównaniu z formatem JSON. Starsze przeglądarki muszą obsługiwać [XHR Level 2](https://caniuse.com/#feat=xhr2) , aby zapewnić obsługę protokołu MessagePack.

Centra wywołują kod po stronie klienta, wysyłając komunikaty zawierające nazwę i parametry metody po stronie klienta. Obiekty wysyłane jako parametry metody są deserializowane przy użyciu skonfigurowanego protokołu. Klient próbuje dopasować nazwę do metody w kodzie po stronie klienta. Gdy klient znajdzie dopasowanie, wywołuje metodę i przekazuje do niej dane parametrów, które są deserializowane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do SignalR dla ASP.NET Core](xref:tutorials/signalr)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
