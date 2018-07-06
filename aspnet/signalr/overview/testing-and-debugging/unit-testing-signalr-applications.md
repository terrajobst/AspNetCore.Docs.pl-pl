---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Testowanie aplikacji SignalR jednostek | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano, jak używać funkcji testów jednostkowych SignalR w wersji 2.0.
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: f46c6edd2002bcc6e62f4e4fe313c9e50e35aaff
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840916"
---
<a name="unit-testing-signalr-applications"></a>Testowanie jednostkowe aplikacji SignalR
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym artykule opisano korzystanie z funkcji testów jednostkowych SignalR 2. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używaną w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Testowanie aplikacji SignalR jednostek

Funkcji testów jednostkowych w SignalR 2 umożliwia tworzenie testów jednostkowych dla aplikacji SignalR. SignalR 2 zawiera [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfejs, który może służyć do tworzenia obiektu makiety symulowanie testowanie metody koncentratora.

W tej sekcji dodasz testy jednostkowe dla aplikacji utworzonych w [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu [XUnit.net](https://github.com/xunit/xunit) i [Moq](https://github.com/Moq/moq4).

XUnit.net będzie służyć do kontrolowania badania; Moq będzie służyć do tworzenia [testowanie](http://en.wikipedia.org/wiki/Mock_object) obiektu do testowania. Inne struktury pozorowania może służyć w razie potrzeby; [NSubstitute](http://nsubstitute.github.io/) jest również dobrym wyborem. W tym samouczku pokazano, jak skonfigurować makiety obiektu na dwa sposoby: po pierwsze, za pomocą `dynamic` obiektu (zostanie wprowadzony w programie .NET Framework 4), a drugi, przy użyciu interfejsu.

### <a name="contents"></a>Spis treści

Ten samouczek zawiera następujące sekcje.

- [Testowanie jednostek za pomocą dynamiczny](#dynamic)
- [Testowanie jednostek, typ](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Testowanie jednostek za pomocą dynamiczny

W tej sekcji dodasz testu jednostkowego dla aplikacji utworzonych w [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu obiekt dynamiczny.

1. Zainstaluj [rozszerzenie modułu uruchamiającego XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) dla programu Visual Studio 2013.
2. Z założenia wykonać [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md), lub pobrać gotową aplikację z [galerii kodu MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Jeśli używasz wersji pobierania wprowadzenie do aplikacji, otwórz **Konsola Menedżera pakietów** i kliknij przycisk **przywrócić** Dodaj pakiet SignalR do projektu.

    ![Przywracanie pakietów](unit-testing-signalr-applications/_static/image1.png)
4. Dodaj projekt do rozwiązania dla testu jednostkowego. Kliknij prawym przyciskiem myszy rozwiązania w **Eksploratora rozwiązań** i wybierz **Dodaj**, **nowy projekt...** . W obszarze **C#** węzeł **Windows** węzła. Wybierz **Biblioteka klas**. Nazwa nowego projektu **TestLibrary** i kliknij przycisk **OK**.

    ![Tworzenie biblioteki testu](unit-testing-signalr-applications/_static/image2.png)
5. Dodaj odwołanie w projekcie biblioteki testów w projekcie SignalRChat. Kliknij prawym przyciskiem myszy **TestLibrary** projektu, a następnie wybierz **Dodaj**, **odwołanie...** . Wybierz **projektów** węźle **rozwiązania** węzła i sprawdź **SignalRChat**. Kliknij przycisk **OK**.

    ![Dodaj odwołanie do projektu](unit-testing-signalr-applications/_static/image3.png)
6. Dodawanie pakietów SignalR, Moq i struktury XUnit **TestLibrary** projektu. W **Konsola Menedżera pakietów**ustaw **domyślny projekt** listę rozwijaną, aby **TestLibrary**. W oknie konsoli, uruchom następujące polecenia:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Instalowanie pakietów](unit-testing-signalr-applications/_static/image4.png)
7. Utwórz plik testu. Kliknij prawym przyciskiem myszy **TestLibrary** projektu, a następnie kliknij przycisk **Dodaj...** , **Klasy**. Nadaj nowej klasie **Tests.cs**.
8. Zastąp zawartość Tests.cs następującym kodem.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    W powyższym kodzie klienta testowego jest tworzony przy użyciu `Mock` obiektu z [Moq](https://github.com/Moq/moq4) biblioteki typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (w SignalR 2.1, należy przypisać `dynamic` dla typu Parametr). `IHubCallerConnectionContext` Interfejs jest obiekt serwera proxy, za pomocą którego można wywołać metody na kliencie. `broadcastMessage` Funkcja następnie jest zdefiniowana makiety klienta, dzięki czemu mogą być wywoływane przez `ChatHub` klasy. Następnie wywołuje aparat testów `Send` metody `ChatHub` klasy, która z kolei wywołuje pozorowane `broadcastMessage` funkcji.
9. Skompiluj rozwiązanie, naciskając klawisz **F6**.
10. Uruchom test jednostkowy. W programie Visual Studio, wybierz **testu**, **Windows**, **Eksplorator testów**. W oknie Eksploratora testów, kliknij prawym przyciskiem myszy **HubsAreMockableViaDynamic** i wybierz **Uruchom wybrane testy**.

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image5.png)
11. Upewnij się, że test zakończył się powodzeniem, sprawdzając w dolnym okienku w oknie Eksploratora testów. W oknie będą wyświetlane, aby test zakończył się powodzeniem.

    ![Test zakończony pomyślnie](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Testowanie jednostek, typ

W tej sekcji dodasz testu dla aplikacji utworzonych w [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu interfejsu, który zawiera metodę, która ma zostać przetestowana.

1. Wykonaj kroki 1 – 7 w [testowanie jednostek za pomocą dynamiczne](#dynamic) samouczek powyżej.
2. Zastąp zawartość Tests.cs następującym kodem.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    W powyższym kodzie jest tworzony interfejs definiujący podpis `broadcastMessage` metody, dla którego aparat testów utworzy makiety klienta. Makiety klienta jest tworzony przy użyciu `Mock` obiektu typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (w SignalR 2.1, należy przypisać `dynamic` dla parametru typu.) `IHubCallerConnectionContext` Interfejs jest obiekt serwera proxy, za pomocą którego można wywołać metody na kliencie.

    Test następnie tworzy wystąpienie `ChatHub`, a następnie tworzy wersję makiety obiektu `broadcastMessage` metody, która z kolei jest wywoływany przez wywołanie metody `Send` metody koncentratora.
3. Skompiluj rozwiązanie, naciskając klawisz **F6**.
4. Uruchom test jednostkowy. W programie Visual Studio, wybierz **testu**, **Windows**, **Eksplorator testów**. W oknie Eksploratora testów, kliknij prawym przyciskiem myszy **HubsAreMockableViaDynamic** i wybierz **Uruchom wybrane testy**.

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image7.png)
5. Upewnij się, że test zakończył się powodzeniem, sprawdzając w dolnym okienku w oknie Eksploratora testów. W oknie będą wyświetlane, aby test zakończył się powodzeniem.

    ![Test zakończony pomyślnie](unit-testing-signalr-applications/_static/image8.png)
