---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania używany w tej wersji programu Visual Studio 2013 .NET 4.5 SignalR tematu 2 poprzednie wersje tego tematu dla SignalR 1.x wersję tego tematu...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 0aaed1d6235688563a446a6834b5188dc9e45aff
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808852"
---
<a name="signalr-scaleout-with-azure-service-bus"></a>SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

W tym samouczku wdrożysz aplikację SignalR, do roli sieci Web Windows Azure, przy użyciu systemu backplane usługi Service Bus, aby dystrybuować komunikaty do każdego wystąpienia roli. (Możesz również użyć płyty montażowej usługi Service Bus za pomocą [aplikacji sieci web w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Wymagania wstępne:

- Konto usługi Windows Azure.
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 lub 2013.

Płyty montażowej magistrali usług jest również zgodna z [usługi Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), wersja 1.1. Jednak nie jest zgodny z wersją 1.0 usługi Service Bus dla systemu Windows Server.

## <a name="pricing"></a>Cennik

Płyty montażowej usługi Service Bus używa tematów do wysłania wiadomości. Najnowsze informacje cenach, zobacz [usługi Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). W momencie pisania tego dokumentu można wysłać wiadomości 1 000 000 na miesiąc dla mniej niż 1 USD. Systemu backplane wysyła komunikat usługi service bus dla każdego wywołania metody koncentratora SignalR. Istnieją również pewne komunikaty sterujące dla połączeń, odłączenia, przyłączany lub opuścić grupy i tak dalej. W większości aplikacji większość ruchu komunikat będzie wywołań metod koncentratora.

## <a name="overview"></a>Omówienie

Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.

1. Użyj portalu platformy Windows Azure, aby utworzyć nową przestrzeń nazw usługi Service Bus.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) lub [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Tworzenie aplikacji SignalR.
4. Dodaj następujący kod do pliku Startup.cs w celu skonfigurowania systemu backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Ten kod konfiguruje systemu backplane z wartościami domyślnymi dla [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajność SignalR: metryki skalowania](signalr-performance.md#scaleout_metrics).

Dla każdej aplikacji wybierz inną wartość dla "YourAppName". Nie należy używać tej samej wartości dla wielu aplikacji.

## <a name="create-the-azure-services"></a>Tworzenie usług platformy Azure

Utwórz usługę w chmurze, zgodnie z opisem w [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Postępuj zgodnie z instrukcjami w sekcji "jak: Tworzenie usługi w chmurze przy użyciu szybkiego tworzenia". W tym samouczku nie musisz przekazać certyfikat.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Utwórz nową przestrzeń nazw usługi Service Bus, zgodnie z opisem w [jak do użycia usługi Service Bus tematów/subskrypcji](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Postępuj zgodnie z instrukcjami w sekcji "Tworzenie Namespace usługi".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Upewnij się wybrać ten sam region dla usługi w chmurze i przestrzeni nazw usługi Service Bus.


## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

Uruchom program Visual Studio. Z **pliku** menu, kliknij przycisk **nowy projekt**.

W **nowy projekt** okna dialogowego rozwiń **Visual C#**. W obszarze **zainstalowane szablony**, wybierz opcję **chmury** , a następnie wybierz **usłudze Windows Azure Cloud Services**. Zachowaj domyślne .NET Framework 4.5. Nazwij aplikację ChatService, a następnie kliknij przycisk **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

W **nową usługę w chmurze Azure Windows** okno dialogowe, wybierz rolę sieci Web platformy ASP.NET. Kliknij przycisk strzałki w prawo (**&gt;**) aby dodać rolę do rozwiązania.

Umieść kursor myszy nad nową rolę, więc widoczne ikonę ołówka. Kliknij tę ikonę, aby zmienić nazwę roli. Nazwa roli "SignalRChat", a następnie kliknij przycisk **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC**i kliknij przycisk OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Kreator projektu tworzy dwa projekty:

- ChatService: Ten projekt jest aplikacją platformy Windows Azure. Definiuje ról platformy Azure i inne opcje konfiguracji.
- SignalRChat: Ten projekt jest projektu ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Tworzenie aplikacji czatu SignalR

Aby utworzyć aplikację rozmowy, postępuj zgodnie z instrukcjami w tym samouczku [wprowadzenie do SignalR i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

NuGet umożliwia instalowanie wymaganych bibliotek. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenia:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Użyj `-ProjectName` opcję, aby zainstalować te pakiety w projekcie ASP.NET MVC, a nie projekt platformy Windows Azure.

## <a name="configure-the-backplane"></a>Konfigurowanie systemu Backplane

W pliku Startup.cs Twojej aplikacji Dodaj następujący kod:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Teraz musisz pobrać parametry połączenia usługi service bus. W witrynie Azure portal wybierz przestrzeń nazw magistrali usług, który został utworzony, a następnie kliknij ikonę klucza dostępu.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Skopiuj parametry połączenia do Schowka, a następnie wklej go do *connectionString* zmiennej.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W Eksploratorze rozwiązań rozwiń **role** folder wewnątrz projektu ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Kliknij prawym przyciskiem myszy rolę SignalRChat, a następnie wybierz pozycję **właściwości**. Wybierz **konfiguracji** kartę. W obszarze **wystąpień** wybierz opcję 2. Rozmiar maszyny Wirtualnej można również ustawić, **wystąpieniom bardzo małe**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Zapisz zmiany.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ChatService. Wybierz **publikowania**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Jeśli jest to pierwszy czasu publikowania na platformie Windows Azure, należy pobrać swoje poświadczenia. W **Publikuj** kreatora, kliknij przycisk "Zaloguj się do pobierania poświadczenia". To spowoduje wyświetlenie monitu zaloguj się do portalu usługi Windows Azure i Pobierz plik ustawień publikowania.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Kliknij przycisk **importu** i zaznacz plik ustawień publikowania, który został pobrany.

Kliknij przycisk **Dalej**. W **ustawień publikowania** okna dialogowego, w obszarze **usługi w chmurze**, wybierz usługę w chmurze, która została utworzona wcześniej.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Kliknij przycisk **publikowania**. Może upłynąć kilka minut, aby wdrożyć aplikację i uruchomić maszyny wirtualne.

Teraz po uruchomieniu aplikacji rozmów z wystąpień roli komunikują się za pośrednictwem usługi Azure Service Bus, przy użyciu tematu usługi Service Bus. Temat jest kolejki komunikatów, która umożliwia wielu subskrybentów.

Systemu backplane automatycznie tworzy, tematu i subskrypcji. Aby zobaczyć subskrypcje i działania komunikatu, otwórz witrynę Azure portal, wybierz przestrzeń nazw usługi Service Bus i wybierz polecenie "Tematów".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

To, że po kilku minutach dla działania komunikatu do wyświetlenia na pulpicie nawigacyjnym.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR zarządza czasem istnienia tematu. Tak długo, jak Twoja aplikacja zostanie wdrożona, nie należy próbować ręcznie usuń tematy lub zmienić ustawienia w tej dziedzinie.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

**System.InvalidOperationException "Tylko IsolationLevel obsługiwane jest"IsolationLevel.Serializable"."**

Ten błąd może wystąpić, jeśli poziom transakcji dla operacji jest równa coś innego niż `Serializable`. Sprawdź, czy żadne operacje są wykonywane przy użyciu innych poziomów transakcji.
