---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Gęstość połączenia SignalR, testowanie za pomocą węzłówką | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Testowanie gęstości połączenia usługi SignalR za pomocą funkcji Crank
ms.author: riande
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 308fed51953b085506488c5e0dda1ced9f4d09fb
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287588"
---
<a name="signalr-connection-density-testing-with-crank"></a>Testowanie gęstości połączenia usługi SignalR za pomocą funkcji Crank
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym artykule opisano sposób użycia narzędzia węzłówką do testowania aplikacji przy użyciu wielu symulowanych klientów.


Gdy aplikacja jest uruchomiona w środowisku hostingu (albo usługi Azure web rolę, usługi IIS, lub może być samodzielnie hostowane przy użyciu Owin), należy przetestować wysoką gęstość połączenia przy użyciu narzędzia węzłówką odpowiedzi aplikacji. Środowisko hostingu może być serwer Internet Information Services (IIS), hosta Owin lub roli usługi sieci web platformy Azure. (Uwaga: Liczniki wydajności nie są dostępne w usłudze Azure App Service Web Apps, dlatego nie można uzyskać dane dotyczące wydajności z test gęstość połączenia.)

Gęstość połączenia odnosi się do liczbę równoczesnych połączeń TCP, nawiązywanych na serwerze. Każde połączenie TCP wiąże się własną obciążenie i otwierania dużej liczby bezczynnych połączeń po pewnym czasie spowoduje utworzenie "wąskie gardło" pamięci.

[SignalR codebase](https://github.com/signalr/signalr) zawiera narzędzie testowania obciążenia o nazwie **możliwość**. Najnowszą wersję węzłówką znajdują się w [gałąź Dev](https://github.com/SignalR/signalr/tree/dev) w witrynie GitHub. Możesz pobrać Zip codebase archiwum gałęzi Dev SignalR [tutaj](https://github.com/SignalR/SignalR/archive/dev.zip).

Węzłówką mogą służyć do w pełni saturate pamięci serwera, aby obliczyć sumę bezczynnych połączeń na sprzęt serwera. Alternatywnie można także użyć węzłówką na test obciążenia serwera w obszarze pewna ilość dużego wykorzystania pamięci, przez rozwijanie połączeń aż do osiągnięcia określonej liczby lub próg pamięci.

Podczas testowania, należy użyć zdalnego od klientów, aby uniknąć wszelkich konkurencję w odniesieniu do zasobów (np. połączenia protokołu TCP i pamięci). Monitoruj klientów, aby upewnić się, że nie zbliżają wszelkie wąskie gardła, które mogą uniemożliwić docieranie do całej pojemności (pamięci lub procesora CPU) serwera. Może być konieczne zwiększenie liczby klientów w celu pełnego obciążenia serwera.

### <a name="running-a-connection-density-test"></a>Uruchamianie testu gęstość połączenia

W tej sekcji opisano kroki niezbędne do uruchomienia testu gęstość połączenia aplikacji SignalR.

1. Pobierz i skompiluj [bazy kodu w gałęzi dev. biblioteki signalr](https://github.com/SignalR/SignalR/archive/dev.zip). W wierszu polecenia przejdź do &lt;katalogu projektu&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Wdróż aplikację w jej środowisku hostingu. Zanotuj punkt końcowy, który korzysta z aplikacji; na przykład w aplikacji utworzonych w [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md), punkt końcowy jest `http://<yourhost>:8080/signalr`.
3. Zainstaluj [liczników wydajności SignalR](signalr-performance.md#perfcounters) na serwerze. Jeśli aplikacja jest uruchomiona na platformie Azure, zobacz [przy użyciu liczników wydajności SignalR w roli sieci Web Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Został pobrany i tworzone bazy kodu i zainstalowanych liczników wydajności na hoście, węzłówką narzędzie wiersza polecenia można znaleźć w `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` folderu.

Dostępne opcje narzędzia węzłówką obejmują:

- **/?** : Pokazuje ekran pomocy. Dostępne opcje są także wyświetlane, gdy **adresu Url** parametr zostanie pominięty.
- **/ Adres Url**: Adres URL połączenia SignalR. Ten parametr jest wymagany. W przypadku aplikacji SignalR przy użyciu domyślnego mapowania ścieżki będą kończyć się na "/ signalr".
- **/ Transportu**: Nazwa transportu. Wartość domyślna to `auto`, który wybierze najbardziej protokołu. Opcje obejmują `WebSockets`, `ServerSentEvents`, i `LongPolling` (`ForeverFrame` nie jest opcją dla węzłówką, ponieważ klient modelu .NET, a nie jest używany program Internet Explorer). Aby uzyskać więcej informacji na temat sposobu SignalR wybiera transportu, zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: Liczba klientach dodawanych w każdej partii. Wartością domyślną jest 50.
- **/ ConnectInterval**: Interwał w milisekundach między dodawaniem połączeń. Wartość domyślna to 500.
- **/ Połączeń**: Liczba połączeń używanych do testu obciążenia aplikacji. Wartość domyślna to 100 000.
- **/ ConnectTimeout**: Limit czasu w ciągu kilku sekund przed przerwaniem testu. Wartość domyślna to 300.
- **MinServerMBytes**: Megabajtów serwerze z minimalną nawiązać połączenie. Wartość domyślna to 500.
- **SendBytes**: Rozmiar ładunku wysyłanych do serwera w bajtach. Wartość domyślna to 0.
- **SendInterval**: Opóźnienie w milisekundach między wiadomości do serwera. Wartość domyślna to 500.
- **Właściwości SendTimeout**: Limit czasu (w milisekundach) dla wiadomości do serwera. Wartość domyślna to 300.
- **ControllerUrl**: Adres Url, gdzie jeden klient będzie obsługiwać koncentrator kontrolera. Wartość domyślna to null (Brak Centrum kontrolera). Koncentrator kontrolera została uruchomiona, podczas uruchamiania sesji węzłówką; żadne dodatkowe kontakt między Koncentrator kontrolera i węzłówką jest nawiązywany.
- **NumClients**: Liczba symulowane klientom na łączenie się z aplikacją. Wartość domyślna to jeden.
- **Plik dziennika**: Nazwa pliku dla pliku dziennika dla przebiegu testu. Wartość domyślna to `crank.csv`.
- **SampleInterval**: Czas w milisekundach między próbek licznika wydajności. Wartość domyślna to 1000.
- **SignalRInstance**: Nazwa wystąpienia liczników wydajności na serwerze. Wartość domyślna to do wartości stanu połączenia klienta.

### <a name="example"></a>Przykład

Następujące polecenie spowoduje przetestowanie lokacji o nazwie `pfsignalr` na platformie Azure, który hostuje aplikację na porcie 8080 przy użyciu koncentratora o nazwie "ControllerHub", przy użyciu 100 połączeń.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
