---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Obsługa błędów programu ASP.NET | Dokumentacja firmy Microsoft
author: Erikre
description: Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: ac5508334bf6d471471a719b98618bdcd3214fb5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30888932"
---
<a name="aspnet-error-handling"></a>Obsługa błędów ASP.NET
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


W tym samouczku należy zmodyfikować Wingtip Toys przykładowej aplikacji do obsługi błędów i rejestrowania błędów. Obsługa błędów umożliwi aplikacji pod kątem poprawnego działania obsługi błędów i odpowiednio wyświetlane komunikaty o błędach. Błąd rejestrowania umożliwiają Znajdź i napraw błędy, które wystąpiły. W tym samouczku jest oparty na poprzednich samouczka "Routingu adresów URL" i jest częścią serii samouczek Wingtip Toys.

## <a name="what-youll-learn"></a>Zawartość:

- Jak dodać globalne obsługę błędów do konfiguracji aplikacji.
- Jak dodać obsługę na poziomach kodu aplikacji, strony i błędów.
- Jak rejestrować błędy, aby uzyskać nowsze przeglądu.
- Jak wyświetlić komunikaty o błędach, które nie naruszyć bezpieczeństwo.
- Jak zaimplementować moduły rejestrowania błędów i obsługi (ELMAH) rejestrowania błędów.

## <a name="overview"></a>Omówienie

Aplikacje ASP.NET musi mieć możliwość obsługi błędów występujących podczas wykonywania w sposób ciągły. Program ASP.NET używa środowisko uruchomieniowe języka wspólnego (CLR), która zapewnia sposób zgłaszania aplikacji błędów w jednolity sposób. Gdy wystąpi błąd, jest zgłaszany wyjątek. Wyjątek jest żadnych błąd, warunku lub nieoczekiwanego zachowania, które napotyka aplikacji.

W programie .NET Framework wyjątek jest obiekt, który dziedziczy `System.Exception` klasy. Wyjątek z obszaru kodu, w którym wystąpił problem. Wyjątek jest przekazywany górę stosu wywołań w miejscu, gdzie aplikacja udostępnia kod obsługujący wyjątek. Jeśli aplikacja nie obsługuje wyjątek, aby wyświetlić szczegóły błędu wymusza przeglądarki.

Najlepszym rozwiązaniem obsługi błędów w poziomie kodu w `Try` / `Catch` / `Finally` bloków kodu. Spróbuj umieścić te bloki, dzięki czemu użytkownik może rozwiązać problemy, w tym kontekście, w którym występują. Bloki obsługi błędów są zbyt daleko od której wystąpił błąd, staje się trudno jest zapewnić użytkownikom informacje, które należy rozwiązać problem.

### <a name="exception-class"></a>Klasy wyjątków

Klasa wyjątku jest klasy podstawowej, z której dziedziczy wyjątków. Większość obiektów wyjątków są wystąpieniami klasy niektóre klasy pochodnej klasy wyjątku, takie jak `SystemException` klasy `IndexOutOfRangeException` klasy lub `ArgumentNullException` klasy. Klasa wyjątku ma właściwości, takie jak `StackTrace` właściwość `InnerException` właściwości oraz `Message` właściwości, które zawierają szczegółowe informacje dotyczące błędu, który wystąpił.

### <a name="exception-inheritance-hierarchy"></a>Hierarchia dziedziczenia wyjątków

Środowisko wykonawcze ma zestaw podstawowy wyjątków wynikających z `SystemException` klasy, która zgłasza środowiska uruchomieniowego po napotkaniu Wystąpił wyjątek. Większość klas, które dziedziczą z klasy wyjątku, takie jak `IndexOutOfRangeException` klasy i `ArgumentNullException` klasa, nie należy implementować dodatkowych członków. W związku z tym najważniejsze informacje dla wyjątku znajdują się w hierarchii wyjątki, nazwa wyjątku i informacje zawarte w wyjątek.

### <a name="exception-handling-hierarchy"></a>Hierarchia Obsługa wyjątków

W aplikacji formularzy sieci Web ASP.NET wyjątki mogą być obsługiwane na podstawie hierarchii określonym obsługi. Wystąpił wyjątek mogą być obsługiwane na następujących poziomach:

- Poziomie aplikacji
- Poziomu strony
- Poziom kodu

Jeśli aplikacja obsługuje wyjątki, dodatkowe informacje o wyjątku, który jest dziedziczona z klasy wyjątku często można je pobrać i wyświetlane użytkownikowi. Oprócz aplikacji, strony i poziom kod mogą również obsługi wyjątków na poziomie modułu HTTP i przy użyciu niestandardowego programu obsługi usług IIS.

### <a name="application-level-error-handling"></a>Obsługa błędów poziomu aplikacji

Domyślne błędy na poziomie aplikacji może obsłużyć, modyfikując konfiguracji aplikacji lub dodając `Application_Error` obsługi w *Global.asax* pliku aplikacji.

Domyślne błędy i komunikaty o błędach HTTP może obsłużyć, dodając `customErrors` sekcji do *Web.config* pliku. `customErrors` Sekcja umożliwia określenie domyślnej strony, które użytkownicy będą przekierowywani po wystąpieniu błędu. Można też określić poszczególnych stron błędów kodu określonego stanu.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Niestety gdy używasz konfigurację kierujący użytkownika do innej strony nie masz szczegóły błędu, który wystąpił.

Jednak pułapki błędów występujących w dowolnym miejscu w aplikacji przez dodawanie kodu do `Application_Error` obsługi w *Global.asax* pliku.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Obsługa zdarzeń błędów poziomu strony

Program obsługi na poziomie strony zwraca użytkownika do strony w przypadku, gdy wystąpił błąd, ale ponieważ wystąpień formanty nie są obsługiwane, będzie już niczego na stronie. Aby zapewnić szczegóły błędu dla użytkownika aplikacji, musisz jawnie zapisać szczegóły błędu do strony.

Zwykle używasz obsługi na poziomie strony błędu rejestrować błędy nieobsługiwany lub przenieść użytkownika do strony, która umożliwia wyświetlanie przydatne informacje.

Ten przykładowy kod przedstawia obsługi dla zdarzenia błędu na stronie sieci Web ASP.NET. Ten program obsługi przechwytuje wszystkie wyjątki, które nie są już obsługiwane w ramach `try` / `catch` blokuje na stronie.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Po obsługi błędu, należy go wyczyścić, wywołując `ClearError` metoda obiektu Server (`HttpServerUtility` klasy), w przeciwnym razie zostanie wyświetlony komunikat o błędzie, wcześniej wystąpienia.

### <a name="code-level-error-handling"></a>Obsługa błędów poziomu kodu

Instrukcji try-catch składa się z bloku try jednego lub więcej przechwycić postanowienia Określ obsługę różnych wyjątki. Wyjątek środowisko uruchomieniowe języka wspólnego (CLR) szuka instrukcji catch, która obsługuje ten wyjątek. Jeśli aktualnie wykonywane metoda zawiera blok catch, CLR analizuje nazywanych bieżącej metody i tak dalej, górę stosu wywołań metody. Jeśli zostanie znaleziony nie bloku catch, CLR jest wyświetlany komunikat nieobsługiwany wyjątek i zatrzymuje wykonywanie programu.

Poniższy przykładowy kod przedstawia typowy sposób użycia `try` / `catch` / `finally` do obsługi błędów.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

W powyższym kodzie bloku try zawiera kod, który ma być chroniony przed dopuszczalnych wyjątków. Blok jest wykonywany, aż do zgłoszenia wyjątku lub bloku zakończyło się powodzeniem. Jeśli dowolny `FileNotFoundException` wyjątek lub `IOException` wystąpi wyjątek, wykonanie jest przenoszona do innej strony. Następnie kod źródłowy znajdujący się na koniec bloku jest wykonywany, czy wystąpił błąd lub nie.

## <a name="adding-error-logging-support"></a>Dodawanie obsługi rejestrowania błędów

Przed dodaniem obsługi do aplikacji przykładowej Wingtip Toys błędów, zostanie dodana obsługa rejestrowania błędów, dodając `ExceptionUtility` klasy do *logiki* folderu. Dzięki temu, zawsze aplikacja obsługuje błąd, szczegóły błędu zostanie dodany do pliku dziennika błędów.

1. Kliknij prawym przyciskiem myszy *logiki* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **kod** grupy szablonów po lewej stronie. Następnie wybierz opcję **klasy**ze środka listy i nadaj mu nazwę **ExceptionUtility.cs**.
3. Wybierz **dodać**. Zostanie wyświetlony nowy plik klasy.
4. Zastąp istniejący kod poniżej:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

W przypadku wystąpienia wyjątku wyjątek można zapisać pliku dziennika wyjątku przez wywołanie metody `LogException` metody. Ta metoda przyjmuje dwa parametry: obiekt wyjątku i ciąg zawierający szczegółowe informacje o źródle wyjątku. Wyjątek dziennik jest zapisywany do *ErrorLog.txt* w pliku *aplikacji\_danych* folderu.

### <a name="adding-an-error-page"></a>Dodawanie strony błędu

W przykładowej aplikacji Wingtip Toys jedną stronę zostanie użyty do wyświetlenia błędów. Strona błędu jest przeznaczona do Pokaż komunikat o błędzie bezpieczny do użytkowników witryny. Jednak jeśli użytkownik jest dewelopera wprowadzania żądanie HTTP, która jest wykonywana lokalnie na komputerze, w którym znajduje się kod, szczegóły błędu dodatkowe będzie wyświetlana na stronę błędu.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**Wingtip Toys**) w **Eksploratora rozwiązań** i wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Wybierz z listy środkowej **formularza sieci Web ze stroną wzorcową**i nadaj mu nazwę **ErrorPage.aspx**.
3. Kliknij przycisk **Dodaj**.
4. Wybierz *Site.Master* pliku jako strony głównej, a następnie wybierz **OK**.
5. Zastąp istniejący kod znaczników następujące czynności:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Zastąp istniejący kod kodu powiązanego (*ErrorPage.aspx.cs*), aby był wyświetlany w następujący sposób:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Po wyświetleniu strony błędu `Page_Load` program obsługi zdarzeń jest wykonywany. W `Page_Load` obsługi, jest określany lokalizację gdzie najpierw został obsłużony błąd. Następnie ostatniego błędu, który wystąpił, jest określany przez wywołanie `GetLastError` metoda obiektu Server. Jeśli wyjątek już nie istnieje, zostanie utworzony wyjątków ogólnych. Następnie jeśli żądanie HTTP zostało utworzone lokalnie, zostaną wyświetlone wszystkie szczegóły błędu. W takim przypadku tylko komputera lokalnego uruchamiania aplikacji sieci web będą widzieć te szczegóły błędu. Po wyświetleniu informacje o błędzie Błąd zostanie dodany do pliku dziennika, a błąd są czyszczone z serwera.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Wyświetlanie komunikatów nieobsługiwany błąd aplikacji

Dodając `customErrors` sekcji do *Web.config* pliku, możesz szybko obsługi błędów proste w całej aplikacji. Można również określić sposób obsługi błędów na podstawie ich wartości Kod stanu, takie jak 404 — Nie znaleziono pliku.

#### <a name="update-the-configuration"></a>Zaktualizuj konfigurację

Zaktualizuj konfigurację, dodając `customErrors` sekcji do *Web.config* pliku.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *Web.config* pliku w katalogu głównym aplikacji przykładowej Wingtip Toys.
2. Dodaj `customErrors` sekcji do *Web.config* pliku w ramach `<system.web>` węzła w następujący sposób:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Zapisz *Web.config* pliku.

`customErrors` Sekcji Określa tryb, który ma ustawioną wartość "Na". Określa również `defaultRedirect`, który informuje aplikacji stronę, która umożliwia przejście do po wystąpieniu błędu. Ponadto dodano element błędu, który określa sposób obsługi błędu 404, gdy strona nie zostanie znaleziona. W dalszej części tego samouczka możesz spowoduje to dodanie dodatkowych błąd obsługi, który będzie przechwytywać szczegóły błędu na poziomie aplikacji.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację teraz, aby zobaczyć zaktualizowane trasy.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji przykładowej Wingtip Toys.  
 W przeglądarce zostanie otwarty i przedstawia *Default.aspx* strony.
2. Wprowadź następujący adres URL w przeglądarce (należy użyć **Twojego** numer portu):  
    `https://localhost:44300/NoPage.aspx`
3. Przegląd *ErrorPage.aspx* wyświetlany w przeglądarce. 

    ![Obsługa błędów platformy ASP.NET — błąd: nie odnaleziono strony](aspnet-error-handling/_static/image1.png)

Żądanie *NoPage.aspx* strony, która nie istnieje, stronę błędu wyświetli komunikat błędu proste i szczegółowe informacje o błędzie jeśli są dostępne dodatkowe szczegóły. Jednak jeśli użytkownik zażądał strony nieistniejącą z lokalizacji zdalnej, stronę błędu czy tylko Pokaż komunikat o błędzie na czerwono.

### <a name="including-an-exception-for-testing-purposes"></a>Wystąpił wyjątek w tym do celów testowych

Aby sprawdzić, jak aplikacja będzie działać, jeśli błąd wystąpi, celowo można utworzyć warunki błędów w programie ASP.NET. W przykładowej aplikacji Wingtip Toys spowoduje zgłoszenie wyjątku testu po załadowaniu domyślnej strony, aby zobaczyć, co się stanie.

1. Otwórz kodem z *Default.aspx* strony w programie Visual Studio.   
   *Default.aspx.cs* zostanie wyświetlona strona związane z kodem.
2. W `Page_Load` obsługi, Dodaj kod, tak aby program obsługi wygląda następująco:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Istnieje możliwość tworzenia różnych różnych typów wyjątków. W powyższym kodzie tworzysz `InvalidOperationException` podczas *Default.aspx* załadowanej strony.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację, aby zobaczyć, jak aplikacja obsługuje wyjątek.

1. Naciśnij klawisz **CTRL + F5** do uruchomienia aplikacji przykładowej Wingtip Toys.  
 Aplikacja zgłasza InvalidOperationException. 

    > [!NOTE] 
    > 
    > Użytkownik musi nacisnąć **CTRL + F5** Aby wyświetlić stronę bez przerywania do kodu, aby wyświetlić źródła błędu w programie Visual Studio.
2. Przegląd *ErrorPage.aspx* wyświetlany w przeglądarce. 

    ![Obsługa błędów platformy ASP.NET — strona błędu](aspnet-error-handling/_static/image2.png)

Jak widać w szczegóły błędu, wyjątek zostało spowodowane `customError` sekcji *Web.config* pliku.

### <a name="adding-application-level-error-handling"></a>Dodawanie obsługi błędów na poziomie aplikacji

Zamiast pułapek wyjątków za pomocą `customErrors` sekcji *Web.config* plików, których możesz uzyskać niewielka ilość informacji o wyjątku, można wyłapać błąd na poziomie aplikacji i pobrać szczegółów błędu.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *Global.asax.cs* pliku.
2. Dodaj **aplikacji\_błąd** obsługi, tak że wygląda następująco:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Po wystąpieniu błędu w aplikacji `Application_Error` nosi nazwę programu obsługi. Ostatnim wyjątkiem ten program obsługi jest pobierane i przejrzeć. Jeśli wyjątek nieobsługiwany wyjątek i wyjątek zawiera szczegóły wyjątku wewnętrznego (to znaczy `InnerException` nie ma wartości null), aplikacji przenosi wykonanie do strony błędu którym wyświetlane są szczegóły wyjątku.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację, aby zobaczyć szczegóły błędu dodatkowe podał obsługi wyjątków na poziomie aplikacji.

1. Naciśnij klawisz **CTRL + F5** do uruchomienia aplikacji przykładowej Wingtip Toys.  
 Aplikacja zgłasza `InvalidOperationException` .
2. Przegląd *ErrorPage.aspx* wyświetlany w przeglądarce. 

    ![Obsługa błędów platformy ASP.NET — błąd poziomu aplikacji](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Dodawanie obsługi błędów poziomu strony

Można dodać obsługi błędów poziomu strony do strony przy użyciu Dodawanie `ErrorPage` atrybutu `@Page` w dyrektywie strony lub dodając `Page_Error` program obsługi zdarzeń do kodem strony. W tej sekcji dodasz `Page_Error` obsługi zdarzeń, które zostaną przeniesione do wykonania *ErrorPage.aspx* strony.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *Default.aspx.cs* pliku.
2. Dodaj `Page_Error` obsługi tak, aby CodeBehind pojawia się jako następuje:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Gdy wystąpi błąd na stronie `Page_Error` nosi nazwę obsługi zdarzeń. Ostatnim wyjątkiem ten program obsługi jest pobierane i przejrzeć. Jeśli `InvalidOperationException` wystąpienia `Page_Error` obsługi zdarzeń przenosi wykonanie do strony błędu którym wyświetlane są szczegóły wyjątku.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację teraz, aby zobaczyć zaktualizowane trasy.

1. Naciśnij klawisz **CTRL + F5** do uruchomienia aplikacji przykładowej Wingtip Toys.  
 Aplikacja zgłasza `InvalidOperationException` .
2. Przegląd *ErrorPage.aspx* wyświetlany w przeglądarce. 

    ![Obsługa błędów platformy ASP.NET — błąd poziomu strony](aspnet-error-handling/_static/image4.png)
3. Zamknij okno przeglądarki.

### <a name="removing-the-exception-used-for-testing"></a>Usuwanie wyjątków do testowania

Aby umożliwić działanie bez generowania wyjątku dodanego wcześniej w tym samouczku aplikacji przykładowych Wingtip Toys, Usuń wyjątek.

1. Otwórz kodem z *Default.aspx* strony.
2. W `Page_Load` obsługi, Usuń kod, który zgłasza wyjątek, tak aby program obsługi wygląda następująco:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Dodawanie rejestrowania błędów poziomie kodu

Jak wspomniano wcześniej w tym samouczku, można dodać instrukcje try/catch próbuje uruchomić sekcji kodu i obsługiwać pierwszego błędu, który występuje. W tym przykładzie zostanie tylko zapisu szczegóły błędu do pliku dziennika błędów, tak aby później można przeglądać błąd.

1. W **Eksploratora rozwiązań**w *logiki* folderu, Znajdź i Otwórz *PayPalFunctions.cs* pliku.
2. Aktualizacja `HttpCall` metody, aby kod był wyświetlany jako następuje:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Powyżej kod wywołuje `LogException` metodę, która jest zawarta w `ExceptionUtility` klasy. Możesz dodać *ExceptionUtility.cs* pliku klasy do *logiki* folderu we wcześniejszej części tego samouczka. `LogException` Metoda przyjmuje dwa parametry. Pierwszym parametrem jest obiekt wyjątku. Drugi parametr jest ciąg używany do rozpoznawania źródła błędu.

### <a name="inspecting-the-error-logging-information"></a>Sprawdzanie rejestrowania informacje o błędzie

Jak wspomniano wcześniej, służy dziennik błędów do określenia, które błędy w aplikacji należy najpierw ustalić. Oczywiście będą rejestrowane tylko błędy, które zostały kolor i zapisywane w dzienniku błędów.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *ErrorLog.txt* w pliku *aplikacji\_danych* folderu.   
 Musisz wybrać "**Pokaż wszystkie pliki**" opcji lub "**Odśwież**" opcja od góry **Eksploratora rozwiązań** wyświetlić *ErrorLog.txt*pliku.
2. Przejrzyj dziennik błędów, wyświetlane w programie Visual Studio: 

    ![Obsługa błędów ASP.NET - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Bezpieczne komunikaty

Jest **pamiętać** że gdy aplikacja wyświetla komunikaty o błędach, nie przekazuje on optymalizacji informacji przez złośliwego użytkownika mogą być przydatne podczas zaatakowania aplikacji. Na przykład jeśli aplikacja próbuje niepomyślnie zapisu bazie danych, nie należy go wyświetlić komunikat o błędzie, który obejmuje nazwę użytkownika, który jest używany. Z tego powodu do użytkownika zostanie wyświetlony komunikat ogólny błąd kolorem czerwonym. Wszystkie dodatkowe szczegóły są wyświetlane tylko deweloperowi na komputerze lokalnym.

## <a name="using-elmah"></a>Przy użyciu ELMAH

ELMAH (moduły rejestrowania błędów i obsługi) jest rejestrowanie błędów Podłącz do aplikację ASP.NET jako pakietu NuGet. ELMAH oferuje następujące możliwości:

- Rejestrowania nieobsługiwanych wyjątków.
- Strona sieci web, aby wyświetlić cały dziennik materiał zarejestrowany nieobsługiwanych wyjątków.
- Strony sieci web, aby wyświetlić szczegółowe informacje dotyczące każdego rejestrowane wyjątku.
- Powiadomienie e-mail dla każdego błędu w czasie, gdy pojawi się.
- Źródło danych RSS ostatnich 15 błędy w dzienniku.

Przed rozpoczęciem pracy z ELMAH, należy go zainstalować. Jest to łatwe przy użyciu *NuGet* pakiet Instalatora. Jak wspomniano wcześniej w tym samouczku, NuGet jest rozszerzenie programu Visual Studio, który ułatwia instalacji i aktualizacji biblioteki typu open source i narzędzia programu Visual Studio.

1. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**  - &gt; **Zarządzaj pakietami NuGet dla rozwiązania**. 

    ![Obsługa błędów ASP.NET - Zarządzaj pakietami NuGet dla rozwiązania](aspnet-error-handling/_static/image6.png)
2. **Zarządzaj pakietami NuGet** zostanie wyświetlone okno dialogowe w programie Visual Studio.
3. W **Zarządzaj pakietami NuGet** okna dialogowego rozwiń **Online** po lewej stronie, a następnie wybierz **nuget.org**. Następnie znajdź i zainstaluj **ELMAH** pakiet z listy dostępnych pakietów w trybie online. 

    ![Obsługa błędów platformy ASP.NET — pakiet NuGet ELMA](aspnet-error-handling/_static/image7.png)
4. Musisz mieć połączenie internetowe, aby pobrać pakiet.
5. W **Wybierz projekty** okna dialogowego upewnij się, **WingtipToys** wyboru jest zaznaczone, a następnie kliknij przycisk **OK**. 

    ![Obsługa błędów platformy ASP.NET — Wybierz projekty, okno dialogowe](aspnet-error-handling/_static/image8.png)
6. Kliknij przycisk **Zamknij** w **Zarządzaj pakietami NuGet** okno dialogowe, w razie potrzeby.
7. Jeśli program Visual Studio żąda, Załaduj ponownie wszystkie otwarte pliki, wybierz "**tak, aby wszystkie**".
8. Pakiet ELMAH Dodaje wpisy dla siebie w *Web.config* pliku w katalogu głównym projektu. Jeśli program Visual Studio pyta, jeśli chcesz ponownie załadować zmodyfikowanych *Web.config* plików, kliknij **tak**.

ELMAH jest teraz gotowy do przechowywania nieobsługiwany występujące błędy.

### <a name="viewing-the-elmah-log"></a>Wyświetlanie dziennika ELMAH

Wyświetlanie dziennika ELMAH jest proste, ale najpierw należy utworzyć Wystąpił nieobsługiwany wyjątek, które będą rejestrowane w dzienniku ELMAH.

1. Naciśnij klawisz **CTRL + F5** do uruchomienia aplikacji przykładowej Wingtip Toys.
2. Aby napisać nieobsługiwany wyjątek w dzienniku ELMAH, przejdź w przeglądarce pod następujący adres URL (przy użyciu numeru portu):  
    `https://localhost:44300/NoPage.aspx` Wyświetli się Strona błędu.
3. Aby wyświetlić dziennik ELMAH, przejdź w przeglądarce pod następujący adres URL (przy użyciu numeru portu):  
    `https://localhost:44300/elmah.axd`

    ![Obsługa błędów platformy ASP.NET — dziennik błędów ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku uzyskanych o obsługi błędów na poziomie aplikacji, na poziomie strony i poziomu kodu. Również zapoznaniu rejestrować błędy obsłużonych i nieobsłużonych do nowszej przeglądu. Po dodaniu ELMAH narzędzia rejestrowania wyjątku i powiadomienia do aplikacji przy użyciu narzędzia NuGet. Ponadto kiedy znasz już o znaczeniu bezpieczne komunikaty.

## <a name="tutorial-series-conclusion"></a>Samouczek serii zawierania

*Dziękujemy za następujące wzdłuż. Mam nadzieję, że ten zestaw samouczki pomogła możesz zapoznanie się ze składnika ASP.NET Web Forms. Aby uzyskać więcej informacji na temat funkcji formularzy sieci Web dostępnych w programie ASP.NET 4.5 i programu Visual Studio 2013, zobacz* [ *ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013 informacje o wersji* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Należy także Spójrz na samouczek wspomnianego* ***następne kroki *** sekcji i defintely wypróbowanie* [ *bezpłatnej wersji próbnej Azure* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Dziękujemy - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o wdrażaniu aplikacji sieci web do systemu Microsoft Azure, zobacz [wdrożyć zabezpieczenia aplikacji sieci Web ASP.NET formularzy z członkostwa, OAuth i bazy danych SQL do witryny sieci Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Bezpłatna wersja próbna

[Microsoft Azure — bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/)  
 Publikowanie witryny sieci Web Microsoft Azure może zaoszczędzić czas, obsługi i wydatków. Jest procesem szybkie wdrażanie aplikacji sieci web na platformie Azure. Gdy potrzebne do utrzymywania i monitorowania aplikacji sieci web Azure oferuje wiele narzędzi i usług. Zarządzać danych, ruchu, tożsamości i tworzenia kopii zapasowych wiadomości, nośniki i wydajności na platformie Azure. A wszystko to znajduje się w bardzo ekonomiczne rozwiązanie.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Szczegóły błędu rejestrowania z monitorowania kondycji programu ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Potwierdzeń

Chcę Dziękujemy następujących osób istotny wkład do zawartości z tego samouczka serii:

- [Alberto Poblacion, MVP &amp; MCT, (Hiszpania)](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alexowi Thissen, Holandia](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brazylia](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Artur dos Santos, Brazylia](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jan Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Jan krzyżyki, USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Jan Pope
- [Mitchel sprzedawców, USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tomasz Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Społeczność

- Mendick Grahamowi ([@grahammendick](http://twitter.com/grahammendick))  
  Przykładowy kod w witrynie MSDN dotyczące programu Visual Studio 2012: [nawigacji Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- Chaney Kuba ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Przykładowy kod w witrynie MSDN dotyczące programu Visual Studio 2012: [ASP.NET 4.5 sieci Web Forms samouczek serii w języku Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - techniczne współautora odbiorców firmy Microsoft (twitter: @driazevedo)  
  Visual Studio 2012 tłumaczenia: [com Iniciando Introdução 4.5 formularzy sieci Web ASP.NET - Parte 1 - e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Poprzednie](url-routing.md)
