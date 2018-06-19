---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Testy jednostkowe aplikacji SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano sposób użycia funkcji testów jednostkowych 2.0 SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: cff866716cb1179e02b930f33cb0f8c33d4a6cf0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870846"
---
<a name="unit-testing-signalr-applications"></a>Jednostka testowania aplikacji SignalR
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym artykule opisano, za pomocą funkcji testów jednostkowych 2 SignalR. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
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
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Testy jednostkowe aplikacji SignalR

Funkcje testu jednostki w SignalR 2 służy do tworzenia testów jednostkowych dla aplikacji SignalR. SignalR 2 zawiera [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfejs, który może służyć do tworzenia obiektu zasymulować symulowanie testowanie metody koncentratora.

W tej sekcji dodasz testów jednostkowych dla aplikacji utworzonych w [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu [XUnit.net](https://github.com/xunit/xunit) i [Moq](https://github.com/Moq/moq4).

XUnit.net będzie można użyć do kontrolowania testu; Moq będzie używane do tworzenia [mock](http://en.wikipedia.org/wiki/Mock_object) obiektu do testowania. Inne struktury mocking można w razie potrzeby; [NSubstitute](http://nsubstitute.github.io/) jest również dobrym rozwiązaniem. W tym samouczku pokazano, jak skonfigurować zasymulować obiektu na dwa sposoby: najpierw przy użyciu `dynamic` obiektu (wprowadzona w programie .NET Framework 4), a druga Strona, przy użyciu interfejsu.

### <a name="contents"></a>Spis treści

Ten samouczek zawiera następujące sekcje.

- [Testowanie jednostkowe na platformie dynamiczne](#dynamic)
- [Jednostka testowania według typu](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Testowanie jednostkowe na platformie dynamiczne

W tej sekcji dodasz testu jednostkowego dla aplikacji utworzonych w [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu obiekt dynamiczny.

1. Zainstaluj [rozszerzenie modułu uruchamiającego XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) dla programu Visual Studio 2013.
2. Wykonać [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md), lub pobrać ukończona aplikacja z [galerii kodu MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Jeśli używasz wersji pobierania aplikacji wprowadzenie, otwórz **Konsola Menedżera pakietów** i kliknij przycisk **przywrócić** Aby dodać pakiet SignalR do projektu.

    ![Przywracanie pakietów](unit-testing-signalr-applications/_static/image1.png)
4. Dodaj projekt do rozwiązania dla testu jednostkowego. Kliknij prawym przyciskiem myszy rozwiązanie w **Eksploratora rozwiązań** i wybierz **Dodaj**, **nowy projekt...** . W obszarze **C#** węzła, wybierz opcję **Windows** węzła. Wybierz **Biblioteka klas**. Nazwa nowego projektu **TestLibrary** i kliknij przycisk **OK**.

    ![Utwórz bibliotekę testu](unit-testing-signalr-applications/_static/image2.png)
5. Dodaj odwołanie do projektu biblioteki testów w projekcie SignalRChat. Kliknij prawym przyciskiem myszy **TestLibrary** projekt i wybierz **Dodaj**, **odwołania...** . Wybierz **projekty** węźle **rozwiązania** węzeł, a także sprawdź **SignalRChat**. Kliknij przycisk **OK**.

    ![Dodaj odwołanie do projektu](unit-testing-signalr-applications/_static/image3.png)
6. Dodawanie pakietów SignalR, Moq i XUnit do **TestLibrary** projektu. W **Konsola Menedżera pakietów**ustaw **domyślny projekt** listy rozwijanej, aby **TestLibrary**. Uruchom następujące polecenia w oknie konsoli:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Instalowanie pakietów](unit-testing-signalr-applications/_static/image4.png)
7. Utwórz plik testu. Kliknij prawym przyciskiem myszy **TestLibrary** projekt i kliknij przycisk **Dodaj...** , **Klasy**. Nazwa nowej klasy **Tests.cs**.
8. Zastąp zawartość Tests.cs z następującym kodem.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    W powyższym kodzie klienta testowego jest tworzony przy użyciu `Mock` obiekt z [Moq](https://github.com/Moq/moq4) biblioteki typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, przypisywanie `dynamic` dla typu Parametr). `IHubCallerConnectionContext` Interfejs jest obiekt serwera proxy, z którym wywołania metody na kliencie. `broadcastMessage` Funkcja następnie jest zdefiniowany dla zasymulować klienta, dzięki czemu może być wywoływany przez `ChatHub` klasy. Następnie wywołuje aparat testów `Send` metody `ChatHub` klasy, która z kolei wywołuje mocked `broadcastMessage` funkcji.
9. Skompiluj rozwiązanie, naciskając klawisz **F6**.
10. Uruchamianie testu jednostkowego. W programie Visual Studio, wybierz **testu**, **Windows**, **Eksploratora testów**. W oknie Eksploratora testów, kliknij prawym przyciskiem myszy **HubsAreMockableViaDynamic** i wybierz **Uruchom wybrane testy**.

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image5.png)
11. Upewnij się, że test przekazany przez sprawdzanie dolnym okienku w oknie Eksploratora testów. W oknie będą wyświetlane, że przekazany testu.

    ![Test zakończył się powodzeniem](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Jednostka testowania według typu

W tej sekcji dodasz testu dla aplikacji utworzonych w [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu interfejsu, który zawiera metody do sprawdzenia.

1. Wykonać kroki od 1 do 7 w [testowanie jednostkowe na platformie dynamicznie](#dynamic) samouczek powyżej.
2. Zastąp zawartość Tests.cs z następującym kodem.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    W powyższym kodzie utworzono interfejs definiujący podpis `broadcastMessage` metody, dla której aparat testów utworzy zasymulować klienta. Zasymulować klienta jest tworzony przy użyciu `Mock` obiektu typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, przypisywanie `dynamic` dla parametru typu.) `IHubCallerConnectionContext` Interfejs jest obiekt serwera proxy, z którym wywołania metody na kliencie.

    Testu następnie tworzy wystąpienie `ChatHub`, a następnie tworzy wersję makiety `broadcastMessage` metodę, która z kolei jest wywoływany przez wywołanie metody `Send` metody koncentratora.
3. Skompiluj rozwiązanie, naciskając klawisz **F6**.
4. Uruchamianie testu jednostkowego. W programie Visual Studio, wybierz **testu**, **Windows**, **Eksploratora testów**. W oknie Eksploratora testów, kliknij prawym przyciskiem myszy **HubsAreMockableViaDynamic** i wybierz **Uruchom wybrane testy**.

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image7.png)
5. Upewnij się, że test przekazany przez sprawdzanie dolnym okienku w oknie Eksploratora testów. W oknie będą wyświetlane, że przekazany testu.

    ![Test zakończył się powodzeniem](unit-testing-signalr-applications/_static/image8.png)
