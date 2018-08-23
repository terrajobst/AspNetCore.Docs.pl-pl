---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Rozpoczęcie korzystania z OWIN i Katana | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: f400ec887bdee123084f582d18016fb5ed3f2165
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754335"
---
<a name="getting-started-with-owin-and-katana"></a>Rozpoczęcie korzystania z OWIN i Katana
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Otwórz interfejs sieci Web platformy .NET (OWIN)](http://owin.org/) definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web. Dzięki rozdzieleniu serwer sieci web z aplikacji, OWIN ułatwia tworzenie oprogramowania pośredniczącego do tworzenia aplikacji internetowych .NET. Ponadto OWIN ułatwia port aplikacji sieci web do innych hostów&#8212;na przykład hostingu samodzielnego w usłudze Windows lub inny proces.

OWIN to specyfikacja należących do społeczności, nie implementację. Projektu Katana to zestaw składników OWIN typu open source opracowany przez firmę Microsoft. Aby uzyskać ogólne omówienie OWIN i Katana, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md). W tym artykule zostanie mogę przejść bezpośrednio do kodu, aby rozpocząć pracę.

W tym samouczku [Visual Studio 2013 w wersji Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ale może również używać programu Visual Studio 2012. Kilka kroków różnią się w programie Visual Studio 2012, który I uwaga poniżej.

## <a name="host-owin-in-iis"></a>Hostowanie OWIN w usługach IIS

W tej sekcji będziemy hostować OWIN w usługach IIS. Ta opcja zapewnia elastyczność i możliwości tworzenia potoku OWIN wraz z zestawu dojrzała funkcji usług IIS. Korzystając z tej opcji aplikacji OWIN jest uruchamiany w Potok żądań ASP.NET.

Najpierw utwórz nowy projekt aplikacji sieci Web ASP.NET. (W programie Visual Studio 2012, używają typu projektu pusta aplikacja sieci Web platformy ASP.NET).

![](getting-started-with-owin-and-katana/_static/image1.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Dodawanie pakietów NuGet

Następnie dodaj wymagane pakiety NuGet. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Dodaj klasę uruchamiania

Następnie Dodaj klasę początkową OWIN. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz **nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Klasa początkowa Owin**. Aby uzyskać więcej informacji na temat konfigurowania klasa startowa. zobacz [wykrywanie klasy początkowej OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Dodaj następujący kod do `Startup1.Configuration` metody:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Ten kod dodaje proste część oprogramowania pośredniczącego do potoku OWIN zaimplementowane jako funkcja, która odbiera **Microsoft.Owin.IOwinContext** wystąpienia. Gdy serwer otrzymuje żądania HTTP, potok OWIN wywołuje oprogramowanie pośredniczące. Oprogramowanie pośredniczące ustawia typ zawartości odpowiedzi i zapisuje treść odpowiedzi.

> [!NOTE]
> Szablon Klasa początkowa OWIN jest dostępny w programie Visual Studio 2013. Jeśli używasz programu Visual Studio 2012, wystarczy dodać nową pustą klasę o nazwie `Startup1`i wklej następujący kod:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby rozpocząć debugowanie. Program Visual Studio otworzy okno przeglądarki, aby `http://localhost:*port*/`. Strona powinna wyglądać następująco:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Hosta samodzielnego OWIN w aplikacji konsoli

To proste przekonwertować tę aplikację z hostowanie usług IIS do hostingu samodzielnego w procesie niestandardowym. Za pomocą hostowanie usług IIS, usługi IIS działa jako serwer HTTP, a proces, który hostuje usługę. Za pomocą hostingu samodzielnego tworzenia procesu i korzysta z aplikacji **HttpListener** klasy jako serwer HTTP.

W programie Visual Studio Utwórz nową aplikację konsoli. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Dodaj `Startup1` klasy z części 1 tego samouczka do projektu. Nie należy modyfikować tej klasy.

Wdrażanie aplikacji `Main` metody w następujący sposób.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Po uruchomieniu aplikacji konsoli, serwer rozpoczyna nasłuchiwanie `http://localhost:9000`. Jeśli przejdziesz do tego adresu w przeglądarce sieci web, zostanie wyświetlona strona "Hello world".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Dodaj diagnostyki OWIN

Pakiet Microsoft.Owin.Diagnostics zawiera oprogramowanie pośredniczące, który przechwytuje nieobsługiwanych wyjątków i wyświetlenie strony HTML przy użyciu szczegółów błędu. Tej funkcji strony podobnie jak strona błędów programu ASP.NET, która jest czasami nazywane "[żółty ekran](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Podobnie jak YSOD stronę błędu Katana jest przydatne podczas tworzenia aplikacji, ale jest dobrym rozwiązaniem, aby ją wyłączyć w trybie produkcyjnym.

Aby zainstalować pakiet diagnostyki w projekcie, wpisz następujące polecenie w oknie Konsola Menedżera pakietów:

`install-package Microsoft.Owin.Diagnostics –Pre`

Zmień kod w swojej `Startup1.Configuration` metody w następujący sposób:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Teraz należy używać CTRL + F5, aby uruchomić aplikację bez debugowania, tak, aby program Visual Studio nie będę powodować w drodze wyjątku. Aplikacja działa tak samo jak wcześniej, aż przejdziesz do `http://localhost/fail`, w tym momencie aplikacji zgłasza wyjątek. Oprogramowanie pośredniczące strony błędu będzie wyłapania wyjątku i wyświetlenia strony HTML przy użyciu informacji o błędzie. Po kliknięciu karty, aby zobaczyć stos, ciąg zapytania, plików cookie, nagłówek żądania i zmiennych środowiskowych OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Następne kroki

- [Wykrywanie klasy początkowej OWIN](owin-startup-class-detection.md)
- [Korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu API sieci Web platformy ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Korzystanie z OWIN na potrzeby samodzielnego hostowania SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
