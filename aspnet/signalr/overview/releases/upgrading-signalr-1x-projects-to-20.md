---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Uaktualnianie projektów SignalR 1.x do wersji 2 | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano sposób uaktualniania istniejący projekt SignalR 1.x do SignalR 2.x oraz sposób rozwiązywania problemów, które mogą wystąpić podczas procesu uaktualniania...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: 450ddedb520035cc05a0dbcca1a2666dd1ba24c7
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910554"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Uaktualnianie projektów SignalR 1.x do wersji 2
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym temacie opisano sposób uaktualniania istniejący projekt SignalR 1.x do SignalR 2.x oraz sposób rozwiązywania problemów, które mogą wystąpić podczas procesu uaktualniania.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR w wersji 1 i 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Z tego samouczka przy użyciu programu Visual Studio 2012
>
>
> Aby użyć programu Visual Studio 2012 za pomocą tego samouczka, wykonaj następujące czynności:
>
> - Aktualizacja usługi [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.
> - Zainstaluj [Instalator platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - Instalator platformy sieci Web, wyszukiwanie i instalowanie **platformy ASP.NET i Web Tools 2013.1 dla programu Visual Studio 2012**. Szablony programu Visual Studio dla klas SignalR spowoduje to zainstalowanie takich jak **Centrum**.
> - Niektóre szablony (takie jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku użyj pliku klasy.
>
>
> ## <a name="questions-and-comments"></a>Pytania i komentarze
>
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 oferuje w jednolitym środowisku projektowym na platformach serwera przy użyciu [OWIN](http://owin.org). W tym artykule opisano kilka kroków, które są potrzebne, aby zaktualizować aplikację SignalR 1.x do wersji 2.

Chociaż zaleca się uaktualnienie aplikacji SignalR 2, SignalR 1.x będą nadal obsługiwane.

W tym samouczku opisano sposób uaktualniania aplikacji hostowanej w sieci web z SignalR 2. Własne aplikacje, (te, które będzie hostować serwer w aplikacji konsoli, usługa Windows lub inny proces) są teraz obsługiwane zgodnie z SignalR 2. Aby uzyskać informacje na temat sposobu Rozpocznij tworzenie aplikacji samodzielnie hostowany przy użyciu SignalR 2, zobacz [samouczek: Host samodzielny SignalR](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Spis treści

W poniższych sekcjach opisano zadania związane z uaktualnianiem projektów SignalR i jak rozwiązywać problemy, które mogą wystąpić.

- [Przykład: Uaktualnienie samouczka Wprowadzenie do SignalR 2](#example)
- [Rozwiązywanie problemów z błędami podczas uaktualniania](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Przykład: Uaktualnianie aplikacji samouczka Wprowadzenie do SignalR 2

W tej sekcji zostaną zaktualizowane aplikacja utworzona w [wersji biblioteki SignalR 1.x Samouczek wprowadzający](../older-versions/index.md) do korzystania z SignalR 2.

1. Po zakończeniu samouczka Wprowadzenie, kliknij prawym przyciskiem myszy nad projektem i wybierz **właściwości**. Upewnij się, że **platformę docelową** ustawiono **.NET Framework 4.5.**
2. Otwórz konsolę Menedżera pakietów. Usuń SignalR 1.x z projektu, używając następującego polecenia:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Zainstaluj signalr2 na użyciu następującego polecenia:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. Na stronie HTML zaktualizuj odwołanie do skryptu dla elementu SignalR w celu dopasowania do wersji skryptu teraz zawarty w projekcie.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. W klasie globalnej aplikacji Usuń wywołanie funkcji MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj**, **nowy element...** . W oknie dialogowym wybierz **klasy początkowej Owin**. Nadaj nowej klasie **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Zastąp zawartość pliku Startup.cs następującym kodem:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    Atrybutu zestawu Dodaje klasę do procesu uruchamiania firmy Owin, które wykonuje `Configuration` metoda podczas uruchamiania Owin. To z kolei wywołuje `MapSignalR` metody, która tworzy trasy do wszystkich centrów SignalR w aplikacji.
8. Uruchom projekt, a następnie skopiuj adres URL strony głównej do innej przeglądarki lub okienku przeglądarki, jak poprzednio. Każda strona będzie monitować o nazwę użytkownika i komunikatów wysyłanych z każdej strony powinny być widoczne w okienkami przeglądarki.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Rozwiązywanie problemów z błędami podczas uaktualniania

W tej sekcji opisano problemy, które mogą wystąpić podczas uaktualniania. Aby bardziej kompleksowe listę błędów i problemów, które mogą wystąpić w przypadku aplikacji SignalR, zobacz [Rozwiązywanie problemów z SignalR](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>"Wywołanie jest niejednoznaczne między następujące metody lub właściwości"

Ten błąd wystąpi, jeśli odwołanie do `Microsoft.AspNet.SignalR.Owin` nie został usunięty. Ten pakiet jest przestarzały; Odwołanie muszą zostać usunięte i wersji 1.x host własny pakiet musi zostać odinstalowane.

### <a name="hub-methods-fail-silently"></a>Metod koncentratora zakończyć się niepowodzeniem dyskretnie

Sprawdź, czy odwołania do skryptu w swoim kliencie aktualny i że `OwinStartup` atrybutu dla klasy uruchamiania ma poprawne klasy i nazwy zestawu w projekcie. Ponadto spróbuj otworzyć adres koncentratorów (/ signalr/koncentratory) w przeglądarce; wszelkie błędy, który pojawia się zaoferuje dowiedzieć się więcej o tym, co będzie problem.
