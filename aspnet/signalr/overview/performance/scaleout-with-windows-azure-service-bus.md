---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: "Skalowania SignalR z usługi Azure Service Bus | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Wersje oprogramowania używanego w tej wersji programu Visual Studio 2013 .NET 4.5 SignalR tematu 2 poprzednie wersje tego tematu, ta wersja 1.x dla SignalR tematu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 7cb68d578fee8d6ee036f8fb096ba45e0c8ef3d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus"></a>Skalowania SignalR z usługi Azure Service Bus
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

W tym samouczku wdrożysz aplikacji SignalR, do roli sieci Web systemu Windows Azure, przy użyciu płyty montażowej usługi Service Bus do dystrybucji wiadomości do każdego wystąpienia roli. (Można również użyć płyty montażowej usługi Service Bus z [sieci web apps w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Wymagania wstępne:

- Konto systemu Windows Azure.
- [Systemu Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Program Visual Studio 2012 lub 2013.

Płyty montażowej magistrali usług jest również zgodna z [Usługa Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), wersja 1.1. Jednak nie jest zgodny z wersją 1.0 Usługa Service Bus dla systemu Windows Server.

## <a name="pricing"></a>Cennik

Płyty montażowej usługi Service Bus używa tematów do wysłania wiadomości. Najnowszy cenowa informacji, zobacz [usługi Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). W momencie pisania tego dokumentu możesz wysłać 1 000 000 wiadomości miesięcznie mniej niż 1 $. Systemu backplane wysyła komunikat magistrali usług dla każdego wywołania metody koncentratora SignalR. Dostępne są także niektóre komunikaty kontroli dla połączeń, rozłączeń łącząca lub zostawianie grup i tak dalej. W większości aplikacji większość ruchu komunikat będzie wywołań metod koncentratora.

## <a name="overview"></a>Omówienie

Przed uzyskujemy do szczegółowy samouczek, w tym miejscu jest to szybki przegląd będzie wykonywać.

1. Umożliwia utworzenie nowej przestrzeni nazw usługi Service Bus w portalu Windows Azure.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Tworzenie aplikacji SignalR.
4. Dodaj następujący kod do pliku Startup.cs do skonfigurowania systemu backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Ten kod konfiguruje systemu backplane z wartościami domyślnymi dla [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajności SignalR: metryki skalowania](signalr-performance.md#scaleout_metrics).

Dla każdej aplikacji wybierz inną wartość dla "YourAppName". Nie należy używać tej samej wartości dla wielu aplikacji.

## <a name="create-the-azure-services"></a>Tworzenie usług platformy Azure

Tworzenie usługi w chmurze, zgodnie z opisem w [sposobu tworzenia i wdrażania usługi w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Wykonaj kroki opisane w sekcji "porady: Tworzenie usługi w chmurze przy użyciu szybkie tworzenie". W tym samouczku nie trzeba przekazać certyfikat.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Tworzenie nowej przestrzeni nazw usługi Service Bus, zgodnie z opisem w [jak magistrali usługi do użycia tematy/subskrypcje](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Wykonaj kroki opisane w sekcji "Tworzenie Namespace usługi".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Upewnij się wybrać usługę w chmurze i przestrzeni nazw usługi Service Bus tym samym regionie.


## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

Uruchom program Visual Studio. Z **pliku** menu, kliknij przycisk **nowy projekt**.

W **nowy projekt** okna dialogowego rozwiń **Visual C#**. W obszarze **zainstalowane szablony**, wybierz pozycję **chmury** , a następnie wybierz **usługi w chmurze Windows Azure**. Zachowaj domyślne .NET Framework 4.5. Nadaj nazwę aplikacji ChatService, a następnie kliknij przycisk **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

W **nowej usługi systemu Windows Azure Cloud** okno dialogowe, wybierz rolę sieci Web ASP.NET. Kliknij przycisk strzałki w prawo (**&gt;**) Dodaj rolę do rozwiązania.

Umieść kursor myszy nad nową rolę więc widoczne ikonę ołówka. Kliknij tę ikonę, aby zmienić nazwę roli. Nazwy roli "SignalRChat" i kliknij przycisk **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC**i kliknij przycisk OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Kreator projektu tworzy dwa projekty:

- ChatService: Ten projekt jest aplikacją systemu Windows Azure. Definiuje Azure ról i innych opcji konfiguracji.
- SignalRChat: Ten projekt jest projekt programu ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Tworzenie aplikacji czatu SignalR

Aby utworzyć aplikację rozmowy, wykonaj kroki opisane w samouczku [wprowadzenie SignalR i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Umożliwia instalowanie wymaganych bibliotek NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenia:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Użyj `-ProjectName` możliwość zainstalowania pakietów do projektu programu ASP.NET MVC, a nie projekt systemu Windows Azure.

## <a name="configure-the-backplane"></a>Konfiguruj płyty montażowej

W pliku Startup.cs aplikacji Dodaj następujący kod:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Następnie należy pobrać parametry połączenia magistrali usługi. W portalu Azure wybierz przestrzeń nazw magistrali usług, który został utworzony, a następnie kliknij ikonę klucza dostępu.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Skopiuj parametry połączenia do Schowka, a następnie wklej ją do *connectionString* zmiennej.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W Eksploratorze rozwiązań rozwiń **ról** folder wewnątrz projektu ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Kliknij prawym przyciskiem myszy rolę SignalRChat i wybierz **właściwości**. Wybierz **konfiguracji** kartę. W obszarze **wystąpień** wybierz 2. Rozmiar maszyny Wirtualnej można również ustawić, **dodatkowe małych**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Zapisz zmiany.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ChatService. Wybierz **publikowania**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Jeśli jest to pierwszy czasu publikowania w systemie Windows Azure, należy pobrać poświadczenia. W **publikowania** kreatora, kliknij przycisk "Zaloguj się do pobierania poświadczenia". To spowoduje wyświetlenie monitu zaloguj się do portalu Windows Azure i Pobierz plik ustawień publikowania.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Kliknij przycisk **importu** i wybierz plik ustawień publikowania, który został pobrany.

Kliknij przycisk **Dalej**. W **ustawień publikowania** okna dialogowego, w obszarze **usługi w chmurze**, wybierz usługę w chmurze, który został utworzony wcześniej.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Kliknij przycisk **publikowania**. Może upłynąć kilka minut, aby wdrożyć aplikację i uruchom maszyny wirtualne.

Teraz po uruchomieniu aplikacji czatu wystąpień roli komunikują się za pośrednictwem usługi Azure Service Bus, przy użyciu tematu usługi Service Bus. Temat jest kolejki komunikatów, która umożliwia wielu subskrybentów.

Systemu backplane automatycznie tworzy tematu i subskrypcji. Aby zobaczyć subskrypcje i działania komunikatu, otwórz Azure portal, wybierz obszar nazw usługi Service Bus i wybierz polecenie "Tematy".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

To, że to potrwać kilka minut dla działania komunikatu wyświetlani na pulpicie nawigacyjnym.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR zarządza czasem istnienia tematu. Tak długo, jak aplikacja jest wdrażana, nie należy próbować ręcznie usuń tematy lub zmień ustawienia w temacie.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

**System.InvalidOperationException "Tylko IsolationLevel obsługiwane jest"IsolationLevel.Serializable"."**

Ten błąd może wystąpić, jeśli poziomu transakcji do operacji ustawiono coś innego niż `Serializable`. Upewnij się, że żadne operacje nie są wykonywane z innych poziomach transakcji.
