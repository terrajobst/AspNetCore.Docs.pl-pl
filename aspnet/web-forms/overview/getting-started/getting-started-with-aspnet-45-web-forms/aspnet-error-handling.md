---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Obsługa błędów platformy ASP.NET | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 8d6c19be7c77079b870261d1c4cf0ea62e0e2fd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807593"
---
<a name="aspnet-error-handling"></a>Obsługa błędów platformy ASP.NET
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku zmodyfikujesz przykładowej aplikacji Wingtip Toys w celu obsługi błędów i rejestrowania błędów. Obsługa błędów umożliwi aplikacji do bezproblemowej obsługi błędów i w związku z tym wyświetlane komunikaty o błędach. Rejestrowanie błędów umożliwiają znajdowanie i naprawianie błędów, które miały miejsce. Ten samouczek opiera się na poprzednim samouczku "Routingu adresów URL" i jest częścią serii samouczków firmy Wingtip Toys.

## <a name="what-youll-learn"></a>Zawartość:

- Jak dodać globalna Obsługa konfiguracji aplikacji błędów.
- Jak dodać obsługę na poziomach kodu aplikacji, strony i błędów.
- Jak rejestrować błędy do późniejszego przejrzenia.
- Jak wyświetlić komunikaty o błędach, które nie naruszyć bezpieczeństwo.
- Jak implementować moduły rejestrowania błędów i obsługi (ELMAH) rejestrowania błędów.

## <a name="overview"></a>Omówienie

Aplikacje ASP.NET musi mieć możliwość obsługi błędów występujących podczas wykonywania w spójny sposób. Aplikacja ASP.NET używa środowisko uruchomieniowe języka wspólnego (CLR), która zapewnia sposób powiadamiania aplikacji o błędach w jednolity sposób. Gdy wystąpi błąd, jest zgłaszany wyjątek. Wyjątek stanowi wszelkie błąd, warunku lub nieoczekiwane zachowanie, które napotyka aplikacji.

W .NET Framework, wyjątek jest obiektem, który dziedziczy z `System.Exception` klasy. Wyjątek jest generowany przy użyciu obszaru kodu, w którym wystąpił problem. Wyjątek jest przekazywany w górę stosu wywołań w miejscu, gdzie aplikacja udostępnia kod służący do obsługi wyjątku. Jeśli aplikacja nie obsługuje wyjątek, przeglądarka jest zmuszony do wyświetlania szczegółów błędu.

Najlepszym rozwiązaniem jest obsługiwać błędy na poziomie kodu w `Try` / `Catch` / `Finally` bloki w kodzie. Spróbuj umieścić te bloki, dzięki czemu użytkownik może rozwiązać problemy w kontekście, w którym występują. Jeśli bloki obsługi błędów są zbyt daleko od której wystąpił błąd, trudniej użytkownikom informacje, które są im potrzebne rozwiązać ten problem.

### <a name="exception-class"></a>Klasy wyjątków

Klasy wyjątków to klasy podstawowej, z którego dziedziczą wyjątków. Większość obiektów wyjątków są wystąpieniami niektórych klasy pochodnej klasy wyjątku, takie jak `SystemException` klasy `IndexOutOfRangeException` klasy lub `ArgumentNullException` klasy. Klasa wyjątku ma właściwości, takie jak `StackTrace` właściwości `InnerException` właściwości i `Message` właściwości, które zawierają szczegółowe informacje o błędzie, który wystąpił.

### <a name="exception-inheritance-hierarchy"></a>Hierarchia dziedziczenia wyjątków

Środowisko uruchomieniowe ma podstawowy zestaw wyjątki pochodzące z `SystemException` klasę, która środowisko uruchomieniowe zgłasza wyjątek, gdy występuje wyjątek. Większość klas, które dziedziczą z klasy wyjątku, takie jak `IndexOutOfRangeException` klasy i `ArgumentNullException` klasy, nie należy implementować dodatkowe elementy członkowskie. W związku z tym najważniejsze informacje o wyjątku znajdują się w hierarchii, wyjątki, nazwa wyjątku i informacje zawarte w wyjątku.

### <a name="exception-handling-hierarchy"></a>Hierarchia obsługi wyjątków

W aplikacji formularzy sieci Web ASP.NET wyjątki mogą być obsługiwane na podstawie hierarchii obsługi określonych. Wyjątek mogą być obsługiwane na następujących poziomach:

- Poziom aplikacji
- Poziomu strony
- Na poziomie kodu

Jeśli aplikacja obsługuje wyjątki, dodatkowe informacje o wyjątku, który jest dziedziczony z klasy wyjątku często można je pobrać i wyświetlane użytkownikowi. Oprócz aplikacji, strony i na poziomie kodu może również obsługiwać wyjątki, na poziomie modułu HTTP i za pomocą niestandardowego programu obsługi usług IIS.

### <a name="application-level-error-handling"></a>Obsługa błąd na poziomie aplikacji

Może obsługiwać błędy domyślny na poziomie aplikacji, modyfikując konfiguracji aplikacji lub dodając `Application_Error` obsługi w *Global.asax* pliku aplikacji.

Może obsługiwać błędy domyślne i błędy HTTP, dodając `customErrors` sekcji *Web.config* pliku. `customErrors` Sekcji pozwala określić domyślną stronę, na który użytkownicy zostaną przekierowani do po wystąpieniu błędu. W tym obszarze pozwala również określić poszczególnych stron błędów kodu określonego stanu.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Niestety korzystając z konfiguracji przekierowanie użytkownika do innej strony, nie masz szczegółowe informacje o błędzie, który wystąpił.

Jednak przechwytywania błędów występujących w dowolnym miejscu w aplikacji, dodając kod do `Application_Error` obsługi w *Global.asax* pliku.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Obsługa zdarzeń błędów poziomu strony

Obsługa na poziomie strony zwraca użytkownika do strony w przypadku, gdy wystąpił błąd, ale ponieważ wystąpienia kontrolki nie są obsługiwane, będą już wszystko na stronie. Aby podać szczegóły błędu użytkownika aplikacji, możesz jawnie zapisać szczegóły błędu do strony.

Zazwyczaj należy użyć obsługi błędu na poziomie strony służące do rejestrowania nieobsługiwanych błędów lub do wykonania użytkownika do strony, który może wyświetlać informacje przydatne.

Ten przykład kodu pokazuje obsługi dla zdarzenia błędów na stronie sieci Web platformy ASP.NET. Ten program obsługi przechwytuje wszystkie wyjątki, które nie są już obsługiwane w ramach `try` / `catch` blokuje na stronie.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Po można obsłużyć błąd, należy usunąć go przez wywołanie metody `ClearError` metody obiektu serwera (`HttpServerUtility` klasy), w przeciwnym razie zostanie wyświetlony błąd, który wystąpił wcześniej.

### <a name="code-level-error-handling"></a>Kodu obsługi błędów w poziomie

Instrukcję try-catch, który składa się z blokiem try następuje jedna lub więcej catch zdań, która określa programy obsługi dla różnych wyjątków. Gdy wyjątek jest zgłaszany, środowisko uruchomieniowe języka wspólnego (CLR) wyszukuje instrukcji catch, która obsługuje ten wyjątek. Jeśli aktualnie wykonywanej metody nie zawiera blok catch, środowisko CLR patrzy na metody, która wywołała bieżącą metodę i tak dalej, w górę stosu wywołań. Jeśli blok catch, nie zostanie znaleziony, CLR zostanie wyświetlony komunikat nieobsługiwany wyjątek użytkownikowi i zatrzymuje wykonywanie programu.

Poniższy przykład kodu pokazuje typowy sposób użycia `try` / `catch` / `finally` do obsługi błędów.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

W powyższym kodzie bloku try zawiera kod, który ma być chronione przed możliwym wyjątkiem. Blok jest wykonywany, dopóki nie zostanie zgłoszony wyjątek lub bloku zakończyło się powodzeniem. Jeśli `FileNotFoundException` wyjątek lub `IOException` wystąpi wyjątek, wykonanie jest przekazywana do innej strony. Następnie kod źródłowy znajdujący się na końcu bloku jest wykonywany, czy wystąpił błąd, czy nie.

## <a name="adding-error-logging-support"></a>Dodanie obsługi rejestrowania błędów

Przed dodaniem obsługi do przykładowej aplikacji Wingtip Toys błędów, spowoduje dodanie obsługi rejestrowania błędów, dodając `ExceptionUtility` klasy *logiki* folderu. Dzięki temu, każdym razem, aplikacja obsługuje błąd szczegóły błędu zostanie dodany do pliku dziennika błędów.

1. Kliknij prawym przyciskiem myszy *logiki* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **kodu** grupy szablonów po lewej stronie. Następnie wybierz **klasy**ze środka listy i nadaj mu nazwę **ExceptionUtility.cs**.
3. Wybierz **Dodaj**. Zostanie wyświetlony nowy plik klasy.
4. Zastąp istniejący kod następujących czynności:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Gdy wystąpi wyjątek, wyjątek mogą być zapisywane do pliku dziennika wyjątku przez wywołanie `LogException` metody. Ta metoda przyjmuje dwa parametry: obiekt wyjątku i ciąg zawierający szczegółowe informacje o źródle wyjątku. Dziennik wyjątku jest zapisywany do *ErrorLog.txt* w pliku *aplikacji\_danych* folderu.

### <a name="adding-an-error-page"></a>Dodawanie strony błędu

W przykładowej aplikacji Wingtip Toys po jednej stronie będzie służyć do wyświetlania błędów. Strona błędu jest przeznaczony do Pokaż komunikat o błędzie bezpiecznego użytkownikom witryny. Jednak jeśli użytkownik jest deweloperem, dzięki czemu żądania HTTP, która jest wykonywana lokalnie na komputerze, gdzie znajduje się kod, dodatkowe szczegóły dotyczące błędu zostanie wyświetlony na stronie błędu.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**Wingtip Toys**) w **Eksploratora rozwiązań** i wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Wybierz z listy środkowej **formularz sieci Web ze stroną wzorcową**i nadaj mu nazwę **ErrorPage.aspx**.
3. Kliknij przycisk **Dodaj**.
4. Wybierz *Site.Master* jako stronę wzorcową, a następnie wybierz **OK**.
5. Zastąp istniejący kod znaczników następujących czynności:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Zastąp istniejący kod związany z kodem (*ErrorPage.aspx.cs*) tak, aby wyglądał następująco:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Po wyświetleniu strony błędu `Page_Load` program obsługi zdarzeń jest wykonywane. W `Page_Load` obsługi, lokalizację, gdzie błąd został obsłużony najpierw jest określana. Następnie ostatniego błędu, który wystąpił, jest określana przez wywołanie `GetLastError` metody obiektu serwera. Jeśli wyjątek nie jest już istnieje, zostanie utworzony wyjątek ogólny. Następnie jeśli żądanie HTTP zostało utworzone lokalnie, są wyświetlane wszystkie szczegóły błędu. W takim przypadku tylko komputera lokalnego uruchamiania aplikacji sieci web zobaczą te informacje o błędzie. Po wyświetleniu informacje o błędzie błędu jest dodawana do pliku dziennika, a błąd zostanie usunięte z serwera.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Wyświetlanie komunikatów nieobsługiwany błąd aplikacji

Dodając `customErrors` sekcji *Web.config* pliku, można szybko obsługiwać proste błędów, które występują w całej aplikacji. Można również określić sposób obsługi błędów na podstawie ich wartości Kod stanu, takie jak 404 — Nie można odnaleźć pliku.

#### <a name="update-the-configuration"></a>Zaktualizuj konfigurację

Zaktualizuj konfigurację, dodając `customErrors` sekcji *Web.config* pliku.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *Web.config* pliku w katalogu głównym przykładowej aplikacji Wingtip Toys.
2. Dodaj `customErrors` sekcji *Web.config* plików w ramach `<system.web>` węzła w następujący sposób:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Zapisz *Web.config* pliku.

`customErrors` Sekcja określa tryb, w którym jest ustawiona na wartość "Włączone". Określa również `defaultRedirect`, informuje aplikację, którą stronę i przejdź do, po wystąpieniu błędu. Ponadto dodano element wystąpienia określonego błędu, który określa sposób obsługi błędu 404, gdy strona nie zostanie znaleziony. W dalszej części tego samouczka opisano sposób dodania dodatkowego błędu obsługi, który będzie przechwytywać szczegóły błędu na poziomie aplikacji.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Można uruchomić aplikację teraz, aby zobaczyć zaktualizowane trasy.

1. Naciśnij klawisz **F5** uruchamianie przykładowej aplikacji Wingtip Toys.  
 Przeglądarki otwiera się i pokazuje *Default.aspx* strony.
2. Wprowadź następujący adres URL do przeglądarki (należy użyć **swoje** numer portu):  
    `https://localhost:44300/NoPage.aspx`
3. Przegląd *ErrorPage.aspx* wyświetlane w przeglądarce. 

    ![Obsługa błędów platformy ASP.NET — błąd: nie odnaleziono strony](aspnet-error-handling/_static/image1.png)

Gdy użytkownik zażąda *NoPage.aspx* strony, która nie istnieje, stronę błędu wyświetli komunikat o błędzie proste i szczegółowe informacje o błędzie, jeśli są dostępne dodatkowe szczegóły. Jednak jeśli użytkownik zażądał nieistniejącej strony z lokalizacji zdalnej, stronę błędu tylko pokazywałaby komunikat o błędzie w kolorze czerwonym.

### <a name="including-an-exception-for-testing-purposes"></a>W tym wyjątek do celów testowych

Aby sprawdzić, jak aplikacja będzie działać, jeśli błąd wystąpi, celowo tworzenia warunków błędów w programie ASP.NET. W przykładowej aplikacji Wingtip Toys spowoduje zgłoszenie wyjątku testu po załadowaniu strony domyślnej, aby zobaczyć, co się dzieje.

1. Otwórz związanym kodzie *Default.aspx* strony w programie Visual Studio.   
   *Default.aspx.cs* zostanie wyświetlona strona związanym z kodem.
2. W `Page_Load` obsługi, Dodaj kod, aby program obsługi wygląda następująco:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Istnieje możliwość tworzenia różnych typów różnych wyjątków. Powyższy kod służy do utworzenia `InvalidOperationException` podczas *Default.aspx* strona została załadowana.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Można uruchomić aplikacji, aby zobaczyć, jak aplikacja obsługuje wyjątek.

1. Naciśnij klawisz **kombinację klawiszy CTRL + F5** uruchamianie przykładowej aplikacji Wingtip Toys.  
 Aplikacja generuje wyjątek InvalidOperationException. 

    > [!NOTE] 
    > 
    > Należy nacisnąć klawisz **kombinację klawiszy CTRL + F5** do wyświetlenia strony bez przerywania do kodu, aby wyświetlić źródło błędu w programie Visual Studio.
2. Przegląd *ErrorPage.aspx* wyświetlane w przeglądarce. 

    ![Obsługa błędów platformy ASP.NET — strona błędu](aspnet-error-handling/_static/image2.png)

Jak widać w szczegółach błędu, wyjątek zostało to spowodowane `customError` sekcji *Web.config* pliku.

### <a name="adding-application-level-error-handling"></a>Dodawanie obsługi błędów na poziomie aplikacji

Zamiast pułapek wyjątków za pomocą `customErrors` sekcji *Web.config* pliku, gdzie możesz uzyskać nieco informacji o wyjątku, można wyłapać błąd na poziomie aplikacji i pobrać szczegóły błędu.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *Global.asax.cs* pliku.
2. Dodaj **aplikacji\_błąd** program obsługi, aby była ona wyświetlana w następujący sposób:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Po wystąpieniu błędu w aplikacji, `Application_Error` program obsługi jest wywoływany. Ostatni wyjątek ten program obsługi jest pobrać i przejrzeć. Jeśli wyjątek nieobsługiwany wyjątek zawiera szczegóły wewnętrznego wyjątku (czyli `InnerException` nie ma wartości null), aplikacja przenosi wykonanie do strony błędu gdzie są wyświetlane szczegóły wyjątku.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację, aby wyświetlić dodatkowe szczegóły dotyczące błędu dostarczone przez obsługi wyjątków na poziomie aplikacji.

1. Naciśnij klawisz **kombinację klawiszy CTRL + F5** uruchamianie przykładowej aplikacji Wingtip Toys.  
 Aplikacja zgłasza `InvalidOperationException` .
2. Przegląd *ErrorPage.aspx* wyświetlane w przeglądarce. 

    ![Obsługa błędów platformy ASP.NET — błąd na poziomie aplikacji](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Dodawanie obsługi błędów na poziomie strony

Można dodać obsługę błędów na poziomie strony do strony za pomocą dodawania `ErrorPage` atrybutu `@Page` w dyrektywie strony lub dodając `Page_Error` procedurę obsługi zdarzeń do kodem strony. W tej sekcji dodasz `Page_Error` program obsługi zdarzeń, który przenosi wykonanie do *ErrorPage.aspx* strony.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *Default.aspx.cs* pliku.
2. Dodaj `Page_Error` obsługi tak, aby związanym z kodem, który jest wyświetlany jako poniżej:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Gdy wystąpi błąd na stronie `Page_Error` program obsługi zdarzeń jest wywoływany. Ostatni wyjątek ten program obsługi jest pobrać i przejrzeć. Jeśli `InvalidOperationException` problem wystąpi, `Page_Error` program obsługi zdarzeń przenosi wykonanie do strony błędu gdzie są wyświetlane szczegóły wyjątku.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Można uruchomić aplikację teraz, aby zobaczyć zaktualizowane trasy.

1. Naciśnij klawisz **kombinację klawiszy CTRL + F5** uruchamianie przykładowej aplikacji Wingtip Toys.  
 Aplikacja zgłasza `InvalidOperationException` .
2. Przegląd *ErrorPage.aspx* wyświetlane w przeglądarce. 

    ![Obsługa błędów platformy ASP.NET — błąd na poziomie strony](aspnet-error-handling/_static/image4.png)
3. Zamknij okno przeglądarki.

### <a name="removing-the-exception-used-for-testing"></a>Usuwanie wyjątków do testowania

Aby zezwolić na przykładowej aplikacji Wingtip Toys działanie bez zgłaszania wyjątku, który dodano wcześniej w tym samouczku, Usuń wyjątek.

1. Otwórz związanym kodzie *Default.aspx* strony.
2. W `Page_Load` obsługi, Usuń kod, który zgłasza wyjątek, aby program obsługi wygląda następująco:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Dodawanie rejestrowania błędów na poziomie kodu

Jak wspomniano wcześniej w tym samouczku, można dodać instrukcje bloku try/catch, aby spróbować uruchomić sekcji kodu i obsługi pierwszego błędu, który występuje. W tym przykładzie tylko napiszesz szczegóły błędu w dzienniku błędów, aby później można wyświetlić błędu.

1. W **Eksploratora rozwiązań**w *logiki* folder, Znajdź i Otwórz *PayPalFunctions.cs* pliku.
2. Aktualizacja `HttpCall` metody, aby kod był wyświetlany jako poniżej:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Kod wywołuje powyższe `LogException` metodę, która jest zawarta w `ExceptionUtility` klasy. Możesz dodać *ExceptionUtility.cs* plik klasy do *logiki* folderu we wcześniejszej części tego samouczka. `LogException` Metoda przyjmuje dwa parametry. Pierwszy parametr jest obiekt wyjątku. Drugi parametr jest ciąg używany do rozpoznawania źródła błędu.

### <a name="inspecting-the-error-logging-information"></a>Sprawdzanie rejestrowania informacje o błędzie

Jak wspomniano wcześniej, można użyć dziennik błędów, aby określić, które błędy w aplikacji należy najpierw ustalić. Oczywiście będą rejestrowane tylko błędy, które zostały zablokował i zapisywane w dzienniku błędów.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *ErrorLog.txt* w pliku *aplikacji\_danych* folderu.   
 Musisz wybrać "**Pokaż wszystkie pliki**" opcja lub "**Odśwież**" opcja od górnej krawędzi **Eksploratora rozwiązań** się *ErrorLog.txt*pliku.
2. Przejrzyj dziennik błędów, wyświetlane w programie Visual Studio: 

    ![Obsługa błędów platformy ASP.NET — ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Komunikaty o błędach bezpieczne

Jest **pamiętać** , kiedy aplikacja wyświetla komunikaty o błędach, jego powinna nie oddawać informacje, które złośliwy użytkownik może się okazać przydatne w atakiem aplikacji. Na przykład jeśli Twoja aplikacja próbuje niepomyślnie zapisu bazie danych, nie powinna zostać wyświetlona komunikat o błędzie, który zawiera nazwę użytkownika, który jest używany. Z tego powodu ogólny komunikat o błędzie w kolorze czerwonym jest wyświetlany użytkownikowi. Wszystkie dodatkowe szczegóły dotyczące błędu są wyświetlane tylko dla dewelopera na komputerze lokalnym.

## <a name="using-elmah"></a>Za pomocą biblioteki ELMAH

Biblioteki ELMAH (moduły rejestrowania błędów i obsługi) jest rejestrowanie błędów podłączyć do aplikacji platformy ASP.NET jako pakiet NuGet. ELMAH zapewnia następujące możliwości:

- Rejestrowania nieobsługiwanych wyjątków.
- Strona sieci web, aby wyświetlić cały dziennik zapisane nieobsługiwanych wyjątków.
- Strony sieci web, aby wyświetlić pełne szczegóły każdego z nich rejestrowane wyjątku.
- Powiadomienie e-mail dla każdego błędu w czasie, gdy pojawi się.
- Źródło danych RSS z ostatnich 15 błędy w dzienniku.

Przed rozpoczęciem pracy za pomocą biblioteki ELMAH, należy go zainstalować. Jest to łatwe za pomocą *NuGet* pakiet Instalatora. Jak wspomniano we wcześniejszej części tej serii samouczków, NuGet jest rozszerzenie programu Visual Studio, która ułatwia do instalowania i aktualizowania bibliotek typu open source i narzędzi w programie Visual Studio.

1. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**  - &gt; **Zarządzaj pakietami NuGet dla rozwiązania**. 

    ![Obsługa błędów platformy ASP.NET — Zarządzaj pakietami NuGet dla rozwiązania](aspnet-error-handling/_static/image6.png)
2. **Zarządzaj pakietami NuGet** w programie Visual Studio wyświetlane jest okno dialogowe.
3. W **Zarządzaj pakietami NuGet** okna dialogowego rozwiń **Online** po lewej stronie, a następnie wybierz pozycję **nuget.org**. Następnie znajdź i zainstaluj **ELMAH** pakiet z listy dostępnych pakietów w trybie online. 

    ![Obsługa błędów platformy ASP.NET — pakiet NuGet ELMA](aspnet-error-handling/_static/image7.png)
4. Musisz mieć połączenie z Internetem Aby pobrać pakiet.
5. W **Wybierz projekty** okna dialogowego pole, upewnij się, że **WingtipToys** wyboru jest zaznaczone, a następnie kliknij przycisk **OK**. 

    ![Obsługa błędów platformy ASP.NET — Wybierz projekty, okno dialogowe](aspnet-error-handling/_static/image8.png)
6. Kliknij przycisk **Zamknij** w **Zarządzaj pakietami NuGet** okno dialogowe, jeśli to konieczne.
7. Jeśli program Visual Studio żądań, Załaduj ponownie wszystkie otwarte pliki, wybierz pozycję "**tak na wszystko**".
8. Pakiet biblioteki ELMAH Dodaje wpisy dla siebie w *Web.config* pliku w folderze głównym projektu. Jeśli program Visual Studio pyta, czy chcesz ponownie załadować zmodyfikowanego *Web.config* plików, kliknij **tak**.

ELMAH jest gotowy do przechowywania nieobsługiwany występujące błędy.

### <a name="viewing-the-elmah-log"></a>Wyświetlanie dziennika ELMAH

Wyświetlanie dziennika ELMAH jest łatwe, ale najpierw należy utworzyć nieobsłużony wyjątek, które będą rejestrowane w dzienniku ELMAH.

1. Naciśnij klawisz **kombinację klawiszy CTRL + F5** uruchamianie przykładowej aplikacji Wingtip Toys.
2. Aby zapisać nieobsługiwany wyjątek w dzienniku ELMAH, przejdź w przeglądarce do witryny sieci Web następujący adres URL (przy użyciu numeru portu):  
    `https://localhost:44300/NoPage.aspx` Zostanie wyświetlona strona błędu.
3. Aby wyświetlić dziennik ELMAH, przejdź w przeglądarce do witryny sieci Web następujący adres URL (przy użyciu numeru portu):  
    `https://localhost:44300/elmah.axd`

    ![Obsługa błędów platformy ASP.NET — dziennik błędów ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono obsługi błędów na poziomie aplikacji, na poziomie strony i na poziomie kodu. Masz również pokazaliśmy, jak rejestrować błędy obsługiwanych i nieobsługiwanych do późniejszego przejrzenia. Po dodaniu narzędzie ELMAH rejestrowaniem wyjątków i powiadomień do aplikacji za pomocą narzędzia NuGet. Ponadto przedstawiono znaczenie komunikaty o błędach bezpieczne.

## <a name="tutorial-series-conclusion"></a>Podsumowanie serii samouczków

*Dziękujemy za przechodzenia. Mam nadzieję, że pomogła tym zestawie samouczków, możesz lepiej zapoznać się z wzorca ASP.NET Web Forms. Aby uzyskać więcej informacji na temat funkcji formularzy sieci Web, które są dostępne w programach ASP.NET 4.5 i programu Visual Studio 2013, zobacz* [ *ASP.NET and Web Tools dla programu Visual Studio 2013 Release Notes* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Należy także zapoznaj się z samouczkiem wymienione w* ***następne kroki *** sekcji i defintely wypróbować* [ *bezpłatnej wersji próbnej platformy Azure* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Dziękuję — Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat wdrażania aplikacji sieci web w systemie Microsoft Azure, zobacz [wdrażanie zabezpieczyć aplikację internetową ASP.NET formularzy przy użyciu członkostwa, uwierzytelnianiem OAuth i bazą danych SQL do witryny sieci Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Bezpłatna wersja próbna

[Microsoft Azure — bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)  
 Publikowanie witryny sieci Web w systemie Microsoft Azure pozwoli zaoszczędzić czas, konserwacja i wydatków. Jest szybkie proces wdrażania aplikacji sieci web na platformie Azure. Gdy zachodzi potrzeba obsługi i monitorowania aplikacji sieci web, platforma Azure oferuje wiele narzędzi i usług. Zarządzanie danymi, ruch, tożsamość, kopii zapasowych, obsługi komunikatów, nośniki i wydajności na platformie Azure. A wszystko to znajduje się w bardzo opłacalne podejście.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Rejestrowanie szczegółów błędów za pomocą monitorowania kondycji ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Potwierdzanie

Chcę otrzymywać podziękować następujących osób, które istotny wkład w zawartości w tej serii samouczków:

- [Alberto Poblacion, MVP &amp; MCT, Hiszpania](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Holandia](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brazylia](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brazylia](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jan Galloway'em, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael krzyżyki, USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Pope
- [Mitchel sprzedawców, USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Kod wniesiony przez społeczność

- Mendick Grahamowi ([@grahammendick](http://twitter.com/grahammendick))  
  Program Visual Studio 2012 powiązane przykładowy kod w witrynie MSDN: [nawigacji o nazwie Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Program Visual Studio 2012 powiązane przykładowy kod w witrynie MSDN: [ASP.NET 4.5 Web Forms samouczek serii w języku Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo — Microsoft techniczne Contributor grupy odbiorców (twitter: @driazevedo)  
  Visual Studio 2012 tłumaczenia: [com Iniciando Introdução ASP.NET Web Forms 4.5 - Parte 1 - e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Poprzednie](url-routing.md)
