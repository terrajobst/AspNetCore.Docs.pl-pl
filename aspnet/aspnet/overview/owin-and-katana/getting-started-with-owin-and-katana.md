---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Wprowadzenie do korzystania z OWIN i Katana | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/28/2018
ms.locfileid: "30257681"
---
<a name="getting-started-with-owin-and-katana"></a>Wprowadzenie do korzystania z OWIN i Katana
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Otwórz interfejs sieci Web dla platformy .NET (OWIN)](http://owin.org/) definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web. Dzięki rozdzieleniu serwera sieci web z aplikacji, OWIN ułatwia tworzenie oprogramowania pośredniczącego do tworzenia aplikacji sieci web platformy .NET. Ponadto OWIN ułatwia portu aplikacji sieci web na inne hosty&#8212;na przykład hostingu samodzielnego usługi systemu Windows lub inny proces.

OWIN jest specyfikacją należących do społeczności, nie implementację. Projekt Katana jest zestaw składników OWIN open source opracowany przez firmę Microsoft. Aby uzyskać ogólne OWIN i Katana, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md). W tym artykule będzie I przejść bezpośrednio do kodu, aby rozpocząć pracę.

W tym samouczku używana [programu Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ale może również używać programu Visual Studio 2012. Kilka kroków różnią się w programie Visual Studio 2012, który I uwaga poniżej.

## <a name="host-owin-in-iis"></a>Host OWIN w usługach IIS

W tej sekcji firma Microsoft będzie udostępniać OWIN w usługach IIS. Ta opcja zapewnia elastyczność i możliwości potoku OWIN wraz z zestawu dojrzałe funkcji usług IIS. Aplikacja OWIN za pomocą tej opcji, działa w potoku żądania ASP.NET.

Najpierw utwórz nowy projekt aplikacji sieci Web ASP.NET. (W programie Visual Studio 2012, użyj typu projektu pusta aplikacja sieci Web ASP.NET).

![](getting-started-with-owin-and-katana/_static/image1.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Dodawanie pakietów NuGet

Następnie dodaj wymagane pakiety NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Dodaj klasę uruchamiania

Następnie Dodaj klasę początkową OWIN. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz pozycję **nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasy początkowej Owin**. Aby uzyskać więcej informacji na temat konfigurowania Klasa początkowa, zobacz [OWIN uruchamiania klasy wykrywania](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Dodaj następujący kod do `Startup1.Configuration` metody:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Ten kod dodaje prosty część oprogramowania pośredniczącego do potoku OWIN zaimplementowane jako funkcja, która odbiera **Microsoft.Owin.IOwinContext** wystąpienia. Kiedy serwer odbiera żądanie HTTP, potoku OWIN wywołuje oprogramowanie pośredniczące. Oprogramowanie pośredniczące ustawia typ zawartości odpowiedzi i zapisuje treść odpowiedzi.

> [!NOTE]
> Szablon klasy OWIN uruchomienia jest dostępne w programie Visual Studio 2013. Jeśli używasz programu Visual Studio 2012, po prostu Dodaj nową klasę pusty o nazwie `Startup1`i wklej poniższy kod:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby rozpocząć debugowanie. Visual Studio zostanie otwarte okno przeglądarki na `http://localhost:*port*/`. Strona powinna wyglądać następująco:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Host samodzielny OWIN w aplikacji konsoli

To proste przekonwertować tę aplikację z hostowanie usług IIS do hostingu samodzielnego w procesie niestandardowych. Z hostowanie usług IIS, usługi IIS działa jako serwer HTTP, a proces, który jest hostem usługi. Z samodzielnej obsługi aplikacji tworzy proces i używa **HttpListener** klasy jako serwer HTTP.

W programie Visual Studio Utwórz nową aplikację konsoli. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Dodaj `Startup1` klasy z tego samouczka, część 1 do projektu. Nie należy modyfikować tej klasy.

Wdrożenie aplikacji `Main` metody w następujący sposób.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Po uruchomieniu aplikacji konsoli serwera rozpoczyna nasłuchiwanie `http://localhost:9000`. Jeśli przejdziesz do tego adresu w przeglądarce sieci web, zostanie wyświetlona strona "Hello world".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Dodaj OWIN Diagnostics

Pakietu Microsoft.Owin.Diagnostics zawiera oprogramowanie pośredniczące, który przechwytuje nieobsługiwanych wyjątków i wyświetlenie strony HTML przy użyciu szczegółów błędu. Funkcje tej strony podobnie jak strona błędów programu ASP.NET, która jest czasem nazywany "[żółty ekran](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Podobnie jak YSOD stronę błędu Katana jest przydatne podczas programowania, ale jest dobrym rozwiązaniem, aby ją wyłączyć w trybie produkcyjnym.

Aby zainstalować pakiet diagnostyki w projekcie, wpisz następujące polecenie w oknie konsoli Menedżera pakietów:

`install-package Microsoft.Owin.Diagnostics –Pre`

Zmień kod w Twojej `Startup1.Configuration` metody w następujący sposób:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Teraz używać CTRL + F5, aby uruchomić aplikację bez debugowania, tak, aby nie będę powodować utraty na wyjątek programu Visual Studio. Aplikacja działa tak samo jak wcześniej, dopóki przejdź do `http://localhost/fail`, w którym aplikacja zgłasza wyjątek. Oprogramowanie pośredniczące strony błędu będzie catch wyjątku i wyświetlenia strony HTML z informacjami o tym błędzie. Możesz kliknąć odpowiednie karty, aby zobaczyć stos, ciąg zapytania, plików cookie, nagłówek żądania i zmiennych środowiskowych OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Następne kroki

- [Wykrywanie klasy początkowej OWIN](owin-startup-class-detection.md)
- [Umożliwia hosta samodzielnego ASP.NET Web API OWIN](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Host samodzielny SignalR za pomocą OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
