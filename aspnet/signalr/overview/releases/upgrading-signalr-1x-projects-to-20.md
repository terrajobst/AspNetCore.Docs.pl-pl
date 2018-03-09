---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "Uaktualnianie projektów 1.x SignalR do wersji 2 | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym temacie opisano sposób uaktualniania istniejącego projektu 1.x SignalR do SignalR 2.x i jak rozwiązywać problemy, które mogą wystąpić podczas procesu uaktualniania..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Uaktualnianie projektów 1.x SignalR do wersji 2
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym temacie opisano sposób uaktualniania istniejącego projektu 1.x SignalR do SignalR 2.x i jak rozwiązywać problemy, które mogą wystąpić podczas procesu uaktualniania.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 1 i 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Z tego samouczka przy użyciu programu Visual Studio 2012
> 
> 
> Aby używać programu Visual Studio 2012 z tego samouczka, wykonaj następujące czynności:
> 
> - Aktualizacja Twojego [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.
> - Zainstaluj [sieci Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Instalator platformy sieci Web, wyszukiwanie i instalowanie **ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012**. Spowoduje to zainstalowanie szablony programu Visual Studio dla biblioteki SignalR klas takich jak **Centrum**.
> - Niektóre szablony (takich jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku powinien użyć pliku klasy.
> 
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 oferuje środowisko spójne opracowywanie platformach serwera przy użyciu [OWIN](http://owin.org). W tym artykule opisano kilka kroków, które są potrzebne, aby zaktualizować aplikację 1.x SignalR do wersji 2.

Zaleca się uaktualnienie aplikacji SignalR 2, SignalR 1.x, będzie nadal być obsługiwane.

Ten przewodnik opisuje sposób uaktualnić aplikację sieci web hostowanych na SignalR 2. Hostowanie Samoobsługowe aplikacji, (te, które serwerze w aplikacji konsoli, usługa systemu Windows lub inny proces hosta) są teraz obsługiwane 2 SignalR. Informacje na temat sposobu rozpocząć tworzenie aplikacji siebie z SignalR 2, zobacz [samouczek: SignalR hosta samodzielnego](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Spis treści

W poniższych sekcjach opisano zadania związane z Uaktualnianie projektów SignalR i jak rozwiązywać problemy, które mogą wystąpić.

- [Przykład: Uaktualnienie samouczka Wprowadzenie do SignalR 2](#example)
- [Rozwiązywanie problemów z błędami podczas uaktualniania](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Przykład: Uaktualnianie Wprowadzenie samouczek aplikacji SignalR 2

W tej sekcji będzie aktualizowana aplikacji utworzonych w [SignalR wersja 1.x Samouczek wprowadzający](../older-versions/index.md) do użycia SignalR 2.

1. Po zakończeniu samouczka Wprowadzenie, kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. Sprawdź, czy **platformy docelowej** ustawiono **.NET Framework 4.5.**
2. Otwórz konsolę Menedżera pakietów. Usuń SignalR 1.x z projektu za pomocą następującego polecenia:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Zainstaluj 2 SignalR za pomocą następującego polecenia:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. Na stronie HTML zaktualizować odwołanie do skryptu dla biblioteki SignalR do zgodna z wersją skryptu teraz zawarta w projekcie.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Klasy globalne aplikacji Usuń wywołanie MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj**, **nowy element...** . W oknie dialogowym wybierz **klasy początkowej Owin**. Nazwa nowej klasy **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Zastąp zawartość pliku Startup.cs następujący kod:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    Atrybut zestawu Dodaje klasę do jego Owin uruchomienia procesu, który wykonuje `Configuration` metody podczas uruchamiania Owin. To z kolei wywołuje `MapSignalR` metodę, która tworzy trasy dla wszystkich koncentratorów SignalR w aplikacji.
8. Uruchom projekt i skopiuj adres URL strony głównej do innej przeglądarki lub okienko przeglądarki, jak wcześniej. Każdej stronie poprosi o nazwę użytkownika i komunikatów wysyłanych z każdej strony powinny być widoczne w obu okienka w przeglądarce.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Rozwiązywanie problemów z błędami podczas uaktualniania

W tej sekcji opisano problemy, które mogą wystąpić podczas uaktualniania. Na pełniejsze listy błędów i problemów, które mogą wystąpić przy użyciu aplikacji SignalR, zobacz [SignalR Rozwiązywanie problemów z](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>"Wywołanie jest niejednoznaczne między następujące metody lub właściwości"

Ten błąd wystąpi, jeśli odniesienie do `Microsoft.AspNet.SignalR.Owin` nie zostanie usunięta. Ten pakiet jest przestarzały; należy usunąć odwołanie i wersja 1.x pakietu SelfHost musi zostać odinstalowane.

### <a name="hub-methods-fail-silently"></a>Metody koncentratora nie w trybie dyskretnym

Sprawdź, czy aktualny i że odwołań do skryptów w kliencie `OwinStartup` atrybutu dla klasy uruchomienia ma poprawne klas i nazwy zestawu dla projektu. Ponadto spróbuj otwierania centra adresu (/ signalr/hubs) w przeglądarce; jakiegokolwiek błędu, który pojawia się będą oferować więcej informacji na temat co się dzieje niewłaściwy.
