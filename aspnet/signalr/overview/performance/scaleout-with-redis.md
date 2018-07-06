---
uid: signalr/overview/performance/scaleout-with-redis
title: SignalR — skalowanie w poziomie przy użyciu usługi Redis | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania, używaną w tym temacie dodatku .NET 4.5 SignalR dla programu Visual Studio 2013 w wersji 2, poprzednie wersje w tym temacie informacji o wcześniejszych wersjach...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 8c125070aa2bc8f0070d9933b77e09a5c84b797a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831009"
---
<a name="signalr-scaleout-with-redis"></a>SignalR — skalowanie w poziomie przy użyciu usługi Redis
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używaną w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


W tym samouczku użyjesz [Redis](http://redis.io/) aby dystrybuować komunikaty aplikacji SignalR, która została wdrożona w dwóch osobnych wystąpień usługi IIS.

Redis jest przechowywanie par klucz wartość w pamięci. Obsługuje ona również system obsługi komunikatów za pomocą modelu publikowania/subskrybowania. Płyty montażowej Redis w SignalR używa funkcji publikowania/subskrybowania do przekazywania wiadomości na inne serwery.

![](scaleout-with-redis/_static/image1.png)

W tym samouczku użyjesz trzech serwerów:

- Dwa serwery, systemem Windows, który będzie używany do wdrażania aplikacji SignalR.
- Jeden serwer z systemem Linux, który będzie używany do uruchamiania usługi Redis. Zrzuty ekranu w tym samouczku używany Ubuntu 12.04 TLS.

Jeśli nie masz trzech serwerów fizycznych do użycia, można utworzyć maszyny wirtualne funkcji Hyper-v. Innym rozwiązaniem jest tworzenie maszyn wirtualnych na platformie Azure.

Chociaż w tym samouczku korzysta z oficjalnego implementacja pamięci podręcznej Redis, dostępna jest również [Windows port Redis](https://github.com/MSOpenTech/redis) z MSOpenTech. Instalacja i konfiguracja są różne, ale w przeciwnym razie kroki są takie same.

> [!NOTE] 
> 
> SignalR — skalowanie w poziomie przy użyciu usługi Redis nie obsługuje klastrów usługi Redis.


## <a name="overview"></a>Omówienie

Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.

1. Zainstaluj usługę Redis i uruchomić serwera Redis.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Tworzenie aplikacji SignalR.
4. Dodaj następujący kod do pliku Startup.cs w celu skonfigurowania systemu backplane: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu na funkcji Hyper-V

Za pomocą funkcji Windows Hyper-V, można łatwo utworzyć Maszynę wirtualną Ubuntu w systemie Windows Server.

Pobierz obraz ISO systemu Ubuntu z [ http://www.ubuntu.com ](http://www.ubuntu.com/).

W funkcji Hyper-V Dodaj nową maszynę Wirtualną. W **Podłączanie wirtualnego dysku twardego** kroku, wybierz pozycję **Utwórz wirtualny dysk twardy**.

![](scaleout-with-redis/_static/image2.png)

W **opcje instalacji** kroku, wybierz pozycję **pliku obrazu (.iso)**, kliknij przycisk **Przeglądaj**, a następnie przejdź do instalacji systemu Ubuntu ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Zainstaluj usługę Redis

Wykonaj kroki opisane w temacie [ http://redis.io/download ](http://redis.io/download) do pobrania i Tworzenie pamięci podręcznej Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

To tworzy pliki binarne pamięci podręcznej Redis `src` katalogu.

Domyślnie usługa Redis nie wymaga hasła. Aby ustawić hasło, należy edytować `redis.conf` pliku, który znajduje się w katalogu głównym kodu źródłowego. (Utwórz kopię zapasową pliku, przed przystąpieniem do edytowania)! Dodaj następujące dyrektywy `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Teraz uruchom serwer Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Otwarty port 6379, który jest domyślnym portem, który Redis nasłuchuje. (Można zmienić numer portu w pliku konfiguracji.)

## <a name="create-the-signalr-application"></a>Tworzenie aplikacji SignalR

Tworzenie aplikacji SignalR, wykonując dowolną z następujących samouczków:

- [Wprowadzenie do SignalR w wersji 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Wprowadzenie do SignalR w wersji 2.0 i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Następnie zmodyfikujemy aplikacji rozmów w celu obsługi skalowania w poziomie przy użyciu usługi Redis. Najpierw Dodaj pakiet SignalR.Redis NuGet do projektu. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Następnie otwórz plik Startup.cs. Dodaj następujący kod do **konfiguracji** metody:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server" jest nazwą serwera, na którym jest uruchomiona usługa Redis.
- *port* jest numerem portu
- "password" jest hasło, które są zdefiniowane w pliku redis.conf.
- "AppName" jest dowolny ciąg. SignalR tworzy kanał Redis pub/sub o tej nazwie.

Na przykład:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Wdrażanie i uruchamianie aplikacji

Przygotowanie do wdrożenia aplikacji SignalR wystąpień systemu Windows Server.

Dodaj rolę usług IIS. Obejmują funkcje "Opracowywanie zawartości dla aplikacji", w tym protokołu WebSocket.

![](scaleout-with-redis/_static/image5.png)

Również obejmować usługę zarządzania (wymienione w obszarze "Narzędzia do zarządzania").

![](scaleout-with-redis/_static/image6.png)

**Zainstaluj narzędzie Web Deploy 3.0.** Podczas uruchamiania Menedżera usług IIS, zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub możesz [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). W Instalatorze platformy wyszukiwania dla narzędzia Web Deploy i zainstalować Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

Sprawdź, czy usługa zarządzania sieci Web jest uruchomiona. Jeśli nie, uruchom usługę. (Jeśli nie widzisz usługi zarządzania siecią Web, na liście usług Windows, upewnij się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)

Domyślnie usługi zarządzania siecią Web nasłuchuje na porcie TCP 8172. W Zaporze Windows utwórz nową regułę dla ruchu przychodzącego, aby umożliwić ruch TCP na porcie 8172. Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Jeśli są hostingu maszyn wirtualnych na platformie Azure, możesz to zrobić bezpośrednio w witrynie Azure portal. Zobacz [jak skonfigurować punkty końcowe do maszyny wirtualnej](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Teraz wszystko jest gotowe do wdrożenia projektu programu Visual Studio z komputera deweloperskiego z serwerem. W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Publikuj**.

Aby uzyskać bardziej szczegółową dokumentację na temat wdrażania w Internecie, zobacz [zawartości mapy wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz ich odbierania komunikatów SignalR w innej. (Oczywiście w środowisku produkcyjnym, dwa serwery będą znajdują się za modułem równoważenia obciążenia.)

![](scaleout-with-redis/_static/image8.png)

Jeśli zastanawiasz się wyświetlić komunikaty, które są wysyłane do pamięci Redis, można użyć **redis-cli** klienta, który instaluje przy użyciu usługi Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
