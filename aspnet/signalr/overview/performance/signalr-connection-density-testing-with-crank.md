---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Testowanie za pomocą węzłówką gęstość połączenia SignalR | Dokumentacja firmy Microsoft
author: tfitzmac
description: Testowanie za pomocą węzłówką gęstość połączenia SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/21/2017
ms.locfileid: "26575540"
---
<a name="signalr-connection-density-testing-with-crank"></a>Testowanie za pomocą węzłówką gęstość połączenia SignalR
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób użycia narzędzia węzłówką do testowania aplikacji z wielu klientów symulowane.


Gdy aplikacja jest uruchomiona w jego środowisko macierzyste (web ról, usług IIS, albo platformy Azure lub samodzielnego hostingu za pomocą Owin), można sprawdzić odpowiedzi aplikacji wysokiego poziomu przy użyciu narzędzia węzłówką gęstość połączenia. Środowisko macierzyste można serwer Internet Information Services (IIS), hosta Owin lub roli sieci web platformy Azure. (Uwaga: liczniki wydajności nie są dostępne na aplikacje sieci Web usługi aplikacji Azure, więc nie można uzyskać danych wydajności z testu gęstość połączenia.)

Gęstość połączenia odwołuje się do liczbę równoczesnych połączeń TCP, które można ustanowić na serwerze. Każde połączenie TCP wiąże własną obciążenie i otwieranie dużą liczbę bezczynności połączenia ostatecznie utworzy wąskie gardło pamięci.

[SignalR codebase](https://github.com/signalr/signalr) zawiera narzędzie testów obciążenia o nazwie **możliwość**. Najnowszą wersję węzłówką znajdują się w [gałęzi deweloperów](https://github.com/SignalR/signalr/tree/dev) w witrynie GitHub. Możesz pobrać Zip codebase archiwum gałęzi deweloperów SignalR [tutaj](https://github.com/SignalR/SignalR/archive/dev.zip).

Węzłówką może służyć do pełni nasycenia pamięci serwera, aby obliczyć całkowitą liczbę bezczynności połączenia na sprzęt serwera. Alternatywnie można także użyć węzłówką do test obciążenia serwera w określonym wykorzystania pamięci przez rozwijanie połączenia do momentu osiągnięcia określonej liczby lub próg pamięci.

Podczas testowania, należy użyć zdalnego klientów w celu uniknięcia konkurencji żadnych zasobów (np. połączenia TCP i pamięci). Monitorowanie klientów, aby upewnić się, że nie są one naciśnięcie wąskie gardła, które mogą uniemożliwić osiągnięcia całej pojemności (pamięci lub Procesora) serwera. Konieczne może być zwiększenie liczby klientów w celu załadowania pełni serwera.

### <a name="running-a-connection-density-test"></a>Wykonywanie testu gęstość połączenia

W tej sekcji opisano kroki potrzebne do uruchomienia testu gęstość połączenia w aplikacji SignalR.

1. Pobieranie i tworzenie [codebase deweloperów gałęzi SignalR](https://github.com/SignalR/SignalR/archive/dev.zip). W wierszu polecenia przejdź do &lt;katalogu projektu&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Wdrażanie aplikacji w usłudze jego danego środowiska macierzystego. Zanotuj punktu końcowego, który korzysta z aplikacji; na przykład w aplikacji utworzony w [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md), punkt końcowy jest `http://<yourhost>:8080/signalr`.
3. Zainstaluj [liczniki wydajności SignalR](signalr-performance.md#perfcounters) na serwerze. Jeśli aplikacja jest uruchomiona na platformie Azure, zobacz [przy użyciu liczniki wydajności SignalR w roli sieci Web Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Po zostały pobrane i wbudowane codebase i zainstalował liczniki wydajności na hoście, narzędzie wiersza polecenia węzłówką znajdują się w `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` folderu.

Dostępne opcje narzędzia węzłówką to:

- **/?** : Pokazuje ekran pomocy. Dostępne opcje są również wyświetlana, jeśli **adres Url** parametr zostanie pominięty.
- **/ Adres Url**: adres URL dla połączenia SignalR. Ten parametr jest wymagany. W przypadku aplikacji SignalR za pomocą domyślnego mapowania ścieżki skończy się za "/ signalr".
- **/ Transportu**: Nazwa transportu. Wartość domyślna to `auto`, która będzie wybierać najlepiej protokołu. Dostępne są następujące opcje `WebSockets`, `ServerSentEvents`, i `LongPolling` (`ForeverFrame` nie jest opcją węzłówką, od klienta .NET, a nie jest używany program Internet Explorer). Aby uzyskać więcej informacji na jak SignalR wybiera transportów, zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: liczba klientów, dodano w każdej z partii. Wartością domyślną jest 50.
- **/ ConnectInterval**: interwał w milisekundach między dodawaniem połączenia. Wartość domyślna to 500.
- **/ Połączeń**: liczba połączeń używanych do testu obciążenia aplikacji. Wartość domyślna to 100 000.
- **/ ConnectTimeout**: limit czasu w sekundach przed przerwaniem testu. Wartość domyślna to 300.
- **MinServerMBytes**: megabajtów minimalna serwera nawiązać połączenia. Wartość domyślna to 500.
- **SendBytes**: rozmiar ładunku wysłane do serwera w bajtach. Wartość domyślna to 0.
- **SendInterval**: opóźnienie w milisekundach między wiadomości do serwera. Wartość domyślna to 500.
- **Właściwości SendTimeout**: limit czasu (w milisekundach) dla wiadomości do serwera. Wartość domyślna to 300.
- **ControllerUrl**: adres Url, w której jeden klient będzie obsługiwać koncentrator kontrolera. Wartość domyślna to null (nie Koncentrator kontrolera). Koncentrator kontrolera jest uruchomiona, podczas uruchamiania sesji węzłówką; żadne dodatkowe nawiązuje kontakt między kontrolera koncentratora i węzłówką.
- **NumClients**: liczba symulowane klientom na łączenie się z aplikacją. Wartość domyślna to jeden.
- **Plik dziennika**: Nazwa pliku dla pliku dziennika dla uruchomienia testu. Wartość domyślna to `crank.csv`.
- **SampleInterval**: czas w milisekundach między próbek licznika wydajności. Wartość domyślna to 1000.
- **SignalRInstance**: Nazwa wystąpienia liczników wydajności na serwerze. Wartość domyślna to do korzystania ze stanu połączenia klienta.

### <a name="example"></a>Przykład

Następujące polecenie spowoduje przetestowanie lokacji o nazwie `pfsignalr` na platformie Azure, które obsługuje aplikację na porcie 8080 przy użyciu koncentratora o nazwie "ControllerHub", przy użyciu połączenia o szybkości 100.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
