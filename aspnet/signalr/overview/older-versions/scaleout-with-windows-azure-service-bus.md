---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: "Skalowania SignalR z usługi Azure Service Bus (SignalR 1.x) | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Skalowania SignalR z usługi Azure Service Bus (SignalR 1.x)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

W tym samouczku wdrożysz aplikacji SignalR, do roli sieci Web systemu Windows Azure, przy użyciu płyty montażowej usługi Service Bus do dystrybucji wiadomości do każdego wystąpienia roli.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Wymagania wstępne:

- Konto systemu Windows Azure.
- [Systemu Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

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
4. Dodaj następujący kod do pliku Global.asax do skonfigurowania systemu backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

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

W **nowej usługi systemu Windows Azure Cloud** okno dialogowe, wybierz rolę sieci Web platformy ASP.NET MVC 4. Kliknij przycisk strzałki w prawo (**&gt;**) Dodaj rolę do rozwiązania.

Umieść kursor myszy nad nową rolę więc widoczne ikonę ołówka. Kliknij tę ikonę, aby zmienić nazwę roli. Nazwy roli "SignalRChat" i kliknij przycisk **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

W **nowy projekt programu ASP.NET MVC 4** kreatora wybierz **aplikacji internetowej**. Kliknij przycisk **OK**. Kreator projektu tworzy dwa projekty:

- ChatService: Ten projekt jest aplikacją systemu Windows Azure. Definiuje Azure ról i innych opcji konfiguracji.
- SignalRChat: Ten projekt jest projekt programu ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Tworzenie aplikacji czatu SignalR

Aby utworzyć aplikację rozmowy, wykonaj kroki opisane w samouczku [wprowadzenie SignalR i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Umożliwia instalowanie wymaganych bibliotek NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenia:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Użyj `-ProjectName` możliwość zainstalowania pakietów do projektu programu ASP.NET MVC, a nie projekt systemu Windows Azure.

## <a name="configure-the-backplane"></a>Konfiguruj płyty montażowej

W pliku Global.asax aplikacji Dodaj następujący kod:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Następnie należy pobrać parametry połączenia magistrali usługi. W portalu Azure wybierz przestrzeń nazw magistrali usług, który został utworzony, a następnie kliknij ikonę klucza dostępu.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Skopiuj parametry połączenia do Schowka, a następnie wklej ją do *connectionString* zmiennej.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W Eksploratorze rozwiązań rozwiń **ról** folder wewnątrz projektu ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Kliknij prawym przyciskiem myszy rolę SignalRChat i wybierz **właściwości**. Wybierz **konfiguracji** kartę. W obszarze **wystąpień** wybierz 2. Rozmiar maszyny Wirtualnej można również ustawić, **dodatkowe małych**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Zapisz zmiany.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ChatService. Wybierz **publikowania**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Jeśli jest to pierwszy czasu publikowania w systemie Windows Azure, należy pobrać poświadczenia. W **publikowania** kreatora, kliknij przycisk "Zaloguj się do pobierania poświadczenia". To spowoduje wyświetlenie monitu zaloguj się do portalu Windows Azure i Pobierz plik ustawień publikowania.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Kliknij przycisk **importu** i wybierz plik ustawień publikowania, który został pobrany.

Kliknij przycisk **Dalej**. W **ustawień publikowania** okna dialogowego, w obszarze **usługi w chmurze**, wybierz usługę w chmurze, który został utworzony wcześniej.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Kliknij przycisk **publikowania**. Może upłynąć kilka minut, aby wdrożyć aplikację i uruchom maszyny wirtualne.

Teraz po uruchomieniu aplikacji czatu wystąpień roli komunikują się za pośrednictwem usługi Azure Service Bus, przy użyciu tematu usługi Service Bus. Temat jest kolejki komunikatów, która umożliwia wielu subskrybentów.

Systemu backplane automatycznie tworzy tematu i subskrypcji. Aby zobaczyć subskrypcje i działania komunikatu, otwórz Azure portal, wybierz obszar nazw usługi Service Bus i wybierz polecenie "Tematy".

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

To, że to potrwać kilka minut dla działania komunikatu wyświetlani na pulpicie nawigacyjnym.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR zarządza czasem istnienia tematu. Tak długo, jak aplikacja jest wdrażana, nie należy próbować ręcznie usuń tematy lub zmień ustawienia w temacie.
