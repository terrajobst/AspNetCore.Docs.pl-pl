---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Skalowania SignalR z pamięci podręcznej Redis (SignalR 1.x) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 8376c6537d693841a621158358cc8f69cda0a1d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28035738"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a>Skalowania SignalR z pamięci podręcznej Redis (SignalR 1.x)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

W tym samouczku użyjesz [Redis](http://redis.io/) przy dystrybucji wiadomości w aplikacji SignalR, który jest wdrożony na dwa różne wystąpienia usług IIS.

Redis jest magazyn kluczy i wartości w pamięci. Obsługuje ona również system obsługi wiadomości z modelem publikowania/subskrypcji. Płyty montażowej SignalR Redis korzysta z funkcji pub/sub do przekazywania wiadomości na inne serwery.

![](scaleout-with-redis/_static/image1.png)

W tym samouczku użyjesz trzech serwerów:

- Dwa serwery z systemem Windows, który będzie używany do wdrażania aplikacji SignalR.
- Jeden serwer z systemem Linux, który będzie używany do uruchamiania Redis. Zrzuty ekranu w tym samouczku użyte I Ubuntu 12.04 TLS.

Jeśli nie masz trzech serwerów fizycznych do użycia, można utworzyć maszyny wirtualne funkcji Hyper-v. Innym rozwiązaniem jest tworzenie maszyn wirtualnych na platformie Azure.

Chociaż w tym samouczku korzysta z oficjalnego implementacji Redis, dostępna jest również [systemu Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech. Instalacja i konfiguracja są różne, a w przeciwnym razie kroki są takie same.

> [!NOTE] 
> 
> Skalowania SignalR z pamięci podręcznej Redis nie obsługuje klastry Redis.


## <a name="overview"></a>Omówienie

Przed uzyskujemy do szczegółowy samouczek, w tym miejscu jest to szybki przegląd będzie wykonywać.

1. Zainstaluj usługę Redis i uruchomić serwera Redis.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Tworzenie aplikacji SignalR.
4. Dodaj następujący kod do pliku Global.asax do skonfigurowania systemu backplane: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu w funkcji Hyper-V

Za pomocą funkcji Hyper-V systemu Windows, można łatwo utworzyć maszyny Wirtualnej systemu Ubuntu w systemie Windows Server.

Pobierz plik ISO Ubuntu od [http://www.ubuntu.com](http://www.ubuntu.com/).

W funkcji Hyper-V należy dodać nowej maszyny Wirtualnej. W **Podłączanie wirtualnego dysku twardego** krok, wybierz opcję **Utwórz wirtualny dysk twardy**.

![](scaleout-with-redis/_static/image2.png)

W **opcje instalacji** krok, wybierz opcję **plik obrazu (.iso)**, kliknij przycisk **Przeglądaj**i przejdź do instalacji Ubuntu ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Zainstaluj usługę Redis

Wykonaj kroki opisane w temacie [http://redis.io/download](http://redis.io/download) do pobrania i Tworzenie pamięci podręcznej Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

To z kolei pliki binarne Redis `src` katalogu.

Domyślnie Redis nie wymaga hasła. Aby ustawić hasło, należy edytować `redis.conf` pliku, który znajduje się w katalogu głównym kodu źródłowego. (Utwórz kopię zapasową pliku, przed przystąpieniem do edytowania!) Dodaj następujące dyrektywy `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Teraz można uruchomić serwera Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Otwórz port 6379, który jest domyślnym portem, który Redis nasłuchuje. (Można zmienić numer portu w pliku konfiguracji.)

## <a name="create-the-signalr-application"></a>Tworzenie aplikacji SignalR

Utwórz aplikację SignalR, wykonując jedną z tych samouczki:

- [Wprowadzenie do korzystania z biblioteki SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Wprowadzenie do korzystania z SignalR i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Firma Microsoft będzie następnie zmodyfikować rozmów aplikację do obsługi skalowania w poziomie z pamięci podręcznej Redis. Najpierw Dodaj pakiet SignalR.Redis NuGet do projektu. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Następnie otwórz plik Global.asax. Dodaj następujący kod do **aplikacji\_Start** metody:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server" jest nazwą serwera, który działa Redis.
- *port* numer portu to
- "password" oznacza hasło, które są zdefiniowane w pliku redis.conf.
- "AppName" jest dowolny ciąg. SignalR tworzy kanał Redis pub/sub o tej nazwie.

Na przykład:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Wdrażanie i uruchamianie aplikacji

Przygotuj swoich wystąpień systemu Windows Server, aby wdrożyć aplikację SignalR.

Dodaj rolę usług IIS. Obejmują funkcje "Programowanie aplikacji", takie jak protokół WebSocket.

![](scaleout-with-redis/_static/image5.png)

Obejmuje to też usługi zarządzania (wymienione w obszarze "Narzędzia do zarządzania").

![](scaleout-with-redis/_static/image6.png)

**Zainstaluj narzędzie Web Deploy 3.0.** Uruchom Menedżera usług IIS, zostanie wyświetlony monit do zainstalowania platformy Microsoft Web lub można [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). W Instalatorze platformy wyszukiwanie narzędzia Web Deploy i instalowanie usługi sieci Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

Sprawdź, czy jest uruchomiona usługa zarządzania siecią Web. Jeśli nie, należy uruchomić usługę. (Usługa zarządzania siecią Web nie jest widoczny na liście usług systemu Windows, aby się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)

Domyślnie usługa zarządzania siecią Web nasłuchuje na porcie TCP 8172. W Zaporze systemu Windows utwórz nową regułę ruchu przychodzącego zezwalająca na ruch TCP na porcie 8172. Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Jeśli są hostingu maszyn wirtualnych na platformie Azure, można w tym bezpośrednio w portalu Azure. Zobacz [jak skonfigurować punkty końcowe z maszyną wirtualną](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Teraz można przystąpić do wdrażania projektu programu Visual Studio z komputerze deweloperskim z serwerem. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **publikowania**.

Aby uzyskać bardziej szczegółowe dokumentacji dotyczące wdrożenia sieci web, zobacz [Mapa zawartości wdrożenia sieci Web dla platformy ASP.NET i Visual Studio](../../../whitepapers/aspnet-web-deployment-content-map.md).

W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz, każdy z nich odbierać wiadomości SignalR z innych. (Oczywiście w środowisku produkcyjnym, dwa serwery będą sit za modułem równoważenia obciążenia.)

![](scaleout-with-redis/_static/image8.png)

Jeśli zastanawiasz się wyświetlanie komunikatów, które są wysyłane do serwera Redis, można użyć **redis-cli** klienta, który instaluje z pamięci podręcznej Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
