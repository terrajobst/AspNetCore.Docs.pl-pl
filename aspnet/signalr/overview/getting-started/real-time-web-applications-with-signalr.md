---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Wskazówki laboratorium: Aplikacje sieci Web czasu rzeczywistego z SignalR | Dokumentacja firmy Microsoft'
author: rick-anderson
description: Aplikacje sieci Web w czasie rzeczywistym funkcji możliwość wypychania zawartości do połączonych klientów, zdarza się, w czasie rzeczywistym po stronie serwera. Dla deweloperów platformy ASP.NET i ASP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5a2bc120ded18ad2302fd6c5cde65a5323e86ca8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878051"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Wskazówki laboratorium: Aplikacje sieci Web czasu rzeczywistego z SignalR
====================
Przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](http://aka.ms/webcamps-training-kit)

> Aplikacje sieci Web w czasie rzeczywistym funkcji możliwość wypychania zawartości do połączonych klientów, zdarza się, w czasie rzeczywistym po stronie serwera. Dla deweloperów platformy ASP.NET **ASP.NET SignalR** jest biblioteki, aby dodać funkcje sieci web w czasie rzeczywistym do swoich aplikacji. Trwa skorzystać z kilku transportów, automatyczne wybieranie najlepiej transportu dostępne danego klienta i dostępne transportu najlepiej serwera. Korzysta z **WebSocket**, interfejs API HTML5, który umożliwia komunikację dwukierunkową między przeglądarką i serwerem.
> 
> **SignalR** udostępnia prostą, ogólny interfejs API dla ten serwer do klienta zdalnego wywoływania procedur (wywołują funkcje JavaScript w przeglądarkach klientów z kodu .NET po stronie serwera) w aplikacji platformy ASP.NET, a także dodawanie przydatne punkty zaczepienia umożliwiający zarządzanie połączeniami takie jak połączyć rozłączyć zdarzeń, grupowanie połączeń i autoryzację.
> 
> **SignalR** umieszczeniu abstrakcję niektóre transportów, które są wymagane do pracy w czasie rzeczywistym między klientem i serwerem. A **SignalR** połączenia rozpoczyna się jako HTTP, a następnie jest podwyższany do **WebSocket** połączenia, jeśli jest dostępna. **WebSocket** jest idealny transportu dla **SignalR**, ponieważ ułatwia najbardziej efektywne wykorzystanie pamięci serwera ma najniższym opóźnieniu i ma większość funkcji (takich jak pełnego dupleksu komunikacji między klientem i serwerów), ale ma także najbardziej rygorystyczne wymagania: **WebSocket** wymaga, aby serwer używa **systemu Windows Server 2012** lub **systemu Windows 8**, wraz z **.NET framework 4.5**. Jeśli te wymagania nie są spełnione, **SignalR** podejmie próbę użycia innych transportów dokonanie jego połączenia (takie jak *Ajax długi sondowania*).
> 
> **SignalR** interfejs API zawiera dwa modele komunikacji między klientami a serwerami: **połączeń trwałych** i **koncentratory**. A **połączenia** reprezentuje proste punktu końcowego dla wysyłania jednego adresata pogrupowane lub wiadomości emisji. A **Centrum** jest bardziej ogólne potoku wbudowane w system API połączenia, który pozwala klienta i serwera, bezpośrednie wywoływanie metod na siebie.
> 
> ![Architektura SignalR](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Wysyłanie powiadomień z serwera do klienta przy użyciu SignalR.
- Skalowanie w poziomie z SignalR aplikacji przy użyciu **programu SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym laboratorium praktycznego, należy najpierw skonfigurować środowiska.

1. Otwórz okno Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który skonfigurujesz środowisko i zainstalować wstawki kodu programu Visual Studio dla tego laboratorium.
3. Jeśli wyświetlane jest okno dialogowe kontroli konta użytkownika, potwierdź tę akcję, aby kontynuować.

> [!NOTE]
> Upewnij się, że zaznaczono wszystkie zależności dla tego laboratorium przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Korzystania z wstawek kodu

W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu. Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązania, znajdujących się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdej wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać do czasu wykonywania. W kodzie źródłowym wykonywania można również znaleźć **zakończenia** folderu zawierającego rozwiązanie Visual Studio z kodem, który powoduje wykonanie czynności opisane w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy w pracy za pośrednictwem tego laboratorium praktycznego, można użyć tych rozwiązań jako wskazówki.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje następujących czynnościach:

1. [Praca z danymi w czasie rzeczywistym za pomocą biblioteki SignalR](#Exercise1)
2. [Skalowanie w poziomie przy użyciu programu SQL Server](#Exercise2)

Szacowany czas trwania tego laboratorium: **60 minut**

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, musisz wybrać jeden z wstępnie zdefiniowanych ustawień kolekcji. Każda kolekcja wstępnie zdefiniowanych zaprojektowano w celu stylem rozwoju i określa układów okien, zachowanie edytora wstawki kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym laboratorium opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólne ustawienia środowiska deweloperskiego** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Ćwiczenie 1: Praca z danymi w czasie rzeczywistym za pomocą biblioteki SignalR

Gdy rozmowa jest często używana jako przykład, można przeprowadzać całej partii więcej o funkcji sieci Web w czasie rzeczywistym. Ilekroć użytkownik odświeża strony sieci web, aby zobaczyć nowe dane lub implementuje stronę Ajax długi sondowania można pobrać nowe dane, można użyć SignalR.

Obsługuje SignalR **wypychania serwera** lub **emisji** funkcji; obsługuje zarządzanie połączeniami automatycznie. W klasycznym połączeń HTTP dla komunikacji klient serwer, ponownym nawiązaniu połączenia dla każdego żądania, ale Biblioteka SignalR udostępnia trwałe połączenie między klientem i serwerem. W bibliotece SignalR kod serwera wywołania do kodu klienta w przeglądarce, za pomocą zdalnego wywołania procedury (RPC) zamiast modelu żądań i odpowiedzi wiemy dzisiaj.

W tym ćwiczeniu zostanie skonfigurowana **Geek quizu** aplikację SignalR do wyświetlania pulpitu nawigacyjnego statystyki o zaktualizowanych metryki bez konieczności odświeżenia dla całej strony.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Zadanie 1 — eksploracji strona statystyk Geek kwizu

W tym zadaniu zostanie przejść przez aplikację i sprawdzić, jak przedstawiono na stronie Statystyka i jak można ulepszyć sposób informacje jest aktualizowany.

1. Otwórz **programu Visual Studio Express 2013 for Web** , a następnie otwórz **GeekQuiz.sln** rozwiązania, znajdujących się w **Source\Ex1 WorkingWithRealTimeData\Begin** folderu.
2. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie. **Zaloguj** strony powinna zostać wyświetlona w przeglądarce.

    ![Uruchomiona rozwiązania](real-time-web-applications-with-signalr/_static/image2.png "systemem rozwiązania")

    *Uruchomiona rozwiązania*
3. Kliknij przycisk **zarejestrować** w prawym górnym rogu strony, aby utworzyć nowego użytkownika w aplikacji.

    ![Zarejestruj łącze](real-time-web-applications-with-signalr/_static/image3.png "łącze rejestru")

    *Zarejestruj łącza*
4. W **zarejestrować** wprowadź **nazwy użytkownika** i **hasło**, a następnie kliknij przycisk **zarejestrować**.

    ![Rejestrowanie użytkownika](real-time-web-applications-with-signalr/_static/image4.png "rejestracja użytkownika")

    *Rejestracja użytkownika*
5. Aplikacja rejestruje nowego konta i użytkownik jest uwierzytelniony i przekierowany z powrotem do strony głównej przedstawiający pierwsze pytanie testu.
6. Otwórz **statystyki** strony w nowym oknie i umieszcza **Home** strony i **statystyki** side-by-side strony.

    ![Windows Side-by-side](real-time-web-applications-with-signalr/_static/image5.png "po stronie przez system windows po stronie")

    *Side-by-side systemu windows*
7. W **Home** strony, Odpowiedz na pytanie, klikając jedną z opcji.

    ![Udzielenie odpowiedzi na pytanie](real-time-web-applications-with-signalr/_static/image6.png "udzielenie odpowiedzi na pytania")

    *Udzielenie odpowiedzi na pytania*
8. Po kliknięciu przycisku jeden z przycisków, powinna zostać wyświetlona odpowiedź.

    ![Poprawne odpowiedzi na pytanie](real-time-web-applications-with-signalr/_static/image7.png "poprawne odpowiedzi na pytanie")

    *Prawidłowej odpowiedzi na pytania*
9. Zwróć uwagę, czy informacje podane na stronie statystyki są nieaktualne. Odśwież stronę, aby wyświetlić zaktualizowane wyniki.

    ![Strona statystyk](real-time-web-applications-with-signalr/_static/image8.png "strona statystyk")

    *Strona statystyk*
10. Wróć do programu Visual Studio i Zatrzymaj debugowanie.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Zadanie 2 — Dodawanie SignalR do kwizu Geek pokazanie wykresy Online

W tym zadaniu zostanie Dodaj SignalR do rozwiązania i aktualizacje automatyczne wysyłanie do klientów podczas nową odpowiedź jest wysyłane do serwera.

1. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**.
2. W **Konsola Menedżera pakietów** okna, uruchom następujące polecenie:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Instalacja pakietu SignalR](real-time-web-applications-with-signalr/_static/image9.png "instalacji pakietu SignalR")

    *Instalacja pakietu SignalR*

   > [!NOTE]
   > Podczas instalowania **SignalR** wersji pakietów NuGet pkt 2.0.2 z nowego aplikacji MVC 5, należy ręcznie zaktualizować **OWIN** pakietów do wersji 2.0.1 (lub nowszej) przed zainstalowaniem SignalR. Aby to zrobić, należy wykonać następujący skrypt w **Konsola Menedżera pakietów**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > W przyszłym wydaniu SignalR zależności OWIN zostanie automatycznie zaktualizowana.
3. W **Eksploratora rozwiązań**, rozwiń węzeł **skryptów** folderu i zwróć uwagę, że SignalR *js* pliki zostały dodane do rozwiązania.

    ![SignalR JavaScript odwołuje się do](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript odwołuje się do")

    *Odwołania SignalR JavaScript*
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **GeekQuiz** projektu, zaznacz **Dodaj** | **nowy Folder**i nazywa je  **Koncentratory**.
5. Kliknij prawym przyciskiem myszy **koncentratory** i wybierz polecenie **Dodaj | Nowy element**.

    ![Dodaj nowy element](real-time-web-applications-with-signalr/_static/image11.png "Dodaj nowy element")

    *Dodaj nowy element*
6. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# | Sieci Web | SignalR** węzła w okienku po lewej stronie wybierz **klasy koncentratora SignalR (v2)** w środkowym okienku Nazwij plik **StatisticsHub.cs** i kliknij przycisk **Dodaj**.

    ![Okno dialogowe Dodaj nowy element](real-time-web-applications-with-signalr/_static/image12.png "okno dialogowe Dodaj nowy element")

    *Dodaj nowy element — okno dialogowe*
7. Zastąp kod w **StatisticsHub** klasy następującym kodem.

    (Fragment - kodu *StatisticsHubClass RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Otwórz **Startup.cs** i Dodaj następujący wiersz na końcu **konfiguracji** metody.

    (Fragment - kodu *MapSignalR RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Otwórz **StatisticsService.cs** strony wewnątrz **usług** folderu i dodaj następującą przy użyciu dyrektyw.

    (Fragment - kodu *UsingDirectives RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Aby powiadomić podłączeni klienci aktualizacje, należy najpierw pobrać **kontekstu** obiektu dla bieżącego połączenia. **Centrum** obiektu zawiera metody do wysyłania komunikatów do jednego klienta lub emisji do wszystkich połączonych klientów. Dodaj następującą metodę do **StatisticsService** klasy wysyłać dane statystyk.

    (Fragment - kodu *NotifyUpdatesMethod RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > W powyższym kodzie używasz nazwę metody dowolnego do wywoływania funkcji na kliencie (np.: *updateStatistics*). Określona nazwa metody jest interpretowany jako obiekt dynamiczny, co oznacza, że nie istnieje IntelliSense lub weryfikacji kompilacji. Wyrażenie jest obliczane w czasie wykonywania. Podczas wywołania metody, SignalR wysyła nazwę metody i wartości parametrów do klienta. Jeśli klient ma metodę, która jest zgodna z nazwą, czy metoda jest wywoływana i wartości parametrów są przekazywane do niego. Jeśli odpowiadającej metody znajduje się na komputerze klienckim, nie błąd. Aby uzyskać więcej informacji, zapoznaj się [Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET](../guide-to-the-api/hubs-api-guide-server.md).
11. Otwórz **TriviaController.cs** strony wewnątrz **kontrolerów** folderu i dodaj następującą przy użyciu dyrektyw.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Dodaj następujący wyróżniony kod, aby **Post** metody akcji.

    (Fragment - kodu *NotifyUpdatesCall RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Otwórz **Statistics.cshtml** strony wewnątrz **widoków | Strona główna** folderu. Zlokalizuj **skrypty** i dodaj następujące odwołania do skryptu na początku sekcji.

    (Fragment - kodu *SignalRScriptReferences RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Po dodaniu SignalR i innych bibliotek skryptu do projektu programu Visual Studio Menedżera pakietów mogą zainstalować wersję pliku skryptu SignalR, która jest nowsza niż wersja wyświetlane w tym temacie. Upewnij się, że odwołanie do skryptu w kodzie zgodna z wersją biblioteki skryptu zainstalowane w projekcie.
14. Dodaj następujący kod wyróżnione połączenia klienta z Centrum SignalR i aktualizacji danych statystyk po odebraniu nowego komunikatu z koncentratora.

    (Fragment - kodu *SignalRClientCode RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    W tym kodzie tworzonej serwer Proxy koncentratora i rejestrowania programu obsługi zdarzeń, aby nasłuchiwać komunikatów wysłanych przez serwer. W takim przypadku nasłuchiwać komunikatów wysyłanych za pośrednictwem *updateStatistics* metody.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — uruchomiona rozwiązania

W tym zadaniu zostanie uruchomiony rozwiązania, aby sprawdzić, czy statystyki widok jest aktualizowany automatycznie po odebraniu nowego zapytania przy użyciu SignalR.

1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.

    > [!NOTE]
    > Jeśli nie jest jeszcze zalogowany do aplikacji, zaloguj się za pomocą utworzonego w ramach zadania 1 użytkownika.
2. Otwórz **statystyki** strony w nowym oknie i umieszcza **Home** strony i **statystyki** strony side-by-side, tak jak w zadaniu 1.
3. W **Home** strony, Odpowiedz na pytanie, klikając jedną z opcji.

    ![Udzielenie odpowiedzi na pytanie innego](real-time-web-applications-with-signalr/_static/image13.png "udzielenie odpowiedzi na pytanie innego")

    *Udzielenie odpowiedzi na pytanie innego*
4. Po kliknięciu przycisku jeden z przycisków, powinna zostać wyświetlona odpowiedź. Należy zauważyć, że informacje statystyczne na stronie jest aktualizowane automatycznie po odpowiedzi na pytanie o zaktualizowane informacje bez konieczności odświeżenia dla całej strony.

    ![Strona statystyk odświeżone po odpowiedzi](real-time-web-applications-with-signalr/_static/image14.png "strona statystyk odświeżone po odpowiedzi")

    *Strona statystyk odświeżone po odpowiedzi*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Ćwiczenie 2: Skalowanie w poziomie przy użyciu programu SQL Server

Podczas skalowania aplikacji sieci web, zwykle można wybrać między *skalowaniu* i *skalowanie w poziomie* opcje. *Skalowanie w górę* oznacza przy użyciu na większy serwer więcej zasobów (procesora CPU, pamięci RAM, itp.) podczas *skalowanie* oznacza dodanie więcej serwerów do obsługi obciążenia. Problem z jego jest, że klienci mogą uzyskać kierowane do różnych serwerów. Klient, który jest podłączony do jednego serwera nie będą otrzymywać wiadomości wysyłane z innego serwera.

Możesz rozwiązać te problemy przy użyciu składnik o nazwie *płyty montażowej*, aby przesyłać dalej komunikatów między serwerami. W środowisku IDE włączone każdego wystąpienia aplikacji wysyła komunikaty do systemu backplane i systemu backplane przekazuje je do wystąpienia aplikacji.

Obecnie istnieją trzy typy montażowych dla biblioteki SignalR:

- **Windows Azure Service Bus**. Usługa Service Bus jest infrastruktury obsługi wiadomości, umożliwiający składników do wysyłania wiadomości luźno powiązanych.
- **Program SQL Server**. Środowiska IDE programu SQL Server zapisuje komunikaty do tabel SQL. Systemu backplane używa brokera usług dla wydajność obsługi wiadomości. Jednak działa także jeśli Service Broker jest wyłączona.
- **Redis**. Redis jest magazyn kluczy i wartości w pamięci. Redis obsługuje wzorzec publikowania/subskrypcji ("pub/sub") do wysyłania wiadomości.

Każdy komunikat jest wysyłany za pośrednictwem magistrali komunikatów. Implementuje magistrali komunikatów [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfejsu, która dostarcza abstrakcji publikowania/subskrypcji. Montażowych pracy przez zastąpienie domyślnie **IMessageBus** z magistralą przeznaczone dla tego systemu backplane.

Każde wystąpienie serwera łączy do systemu backplane za pośrednictwem magistrali. Po wysłaniu komunikatu trafia do systemu backplane i wysyła go do każdego serwera systemu backplane. Gdy serwer odbiera komunikat z systemu backplane, zapisuje komunikat w lokalnej pamięci podręcznej. Serwer następnie dostarcza klientom z lokalnej pamięci podręcznej.

Aby uzyskać więcej informacji na temat płyty montażowej SignalR działania, przeczytaj [artykułu](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Istnieją sytuacje, w którym płyty montażowej może stać się wąskiego gardła. Poniżej przedstawiono kilka typowych scenariuszy SignalR:
> 
> - [Emisja serwera](tutorial-server-broadcast-with-signalr.md) (np. giełdowych): montażowych działa dobrze w tym scenariuszu, ponieważ serwer określa szybkość, z jaką komunikaty.
> - [Klient do klienta](tutorial-getting-started-with-signalr.md) (np. rozmowę): W tym scenariuszu systemu backplane może być wąskiego gardła, jeśli liczba komunikatów skaluje o liczbie klientów; oznacza to, jeśli liczba komunikatów rozwoju przyłączyć się proporcjonalnie jako większej liczby klientów.
> - [Wysoka częstotliwość w czasie rzeczywistym](tutorial-high-frequency-realtime-with-signalr.md) (np. w czasie rzeczywistym gry): środowisku IDE nie jest zalecane dla tego scenariusza.


W tym ćwiczeniu użyjesz **programu SQL Server** do dystrybucji wiadomości między **Geek quizu** aplikacji. Te zadania będą uruchamiane na maszynie pojedynczy test więcej informacji na temat konfiguracji, ale aby uzyskać pełny efekt, będzie konieczne wdrażanie aplikacji SignalR do co najmniej dwa serwery. SQL Server należy również zainstalować na jednym serwerze lub na osobnym serwerze dedykowanym.

![Skalowanie w poziomie przy użyciu programu SQL Server diagramu](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Zadanie 1 — Opis scenariusza

To zadanie uruchomi 2 wystąpienia **Geek quizu** wystąpienia symulowanie wielu usług IIS na komputerze lokalnym. W tym scenariuszu, gdy udzielenie odpowiedzi na pytania elementy towarzyszące składni w jednej aplikacji aktualizacji nie zostaną powiadomieni na stronie statystyki drugie wystąpienie. Ta symulacja podobny wdrożonym aplikacji na wiele wystąpień środowiska i przy użyciu modułu równoważenia obciążenia do komunikowania się z nimi.

1. Otwórz **Begin.sln** rozwiązania, znajdujących się w **źródło/Ex2-ScalingOutWithSQLServer/Begin** folderu. Po załadowaniu, można zauważyć na **Eksploratora serwera** czy rozwiązania zawiera dwa projekty o identycznych struktury, ale o innej nazwy. Spowoduje to symulować uruchomione dwa wystąpienia tej samej aplikacji na komputerze lokalnym.

    ![Rozpocząć rozwiązania symulując 2 wystąpienia kwizu Geek](real-time-web-applications-with-signalr/_static/image16.png "rozpocząć rozwiązania symulując 2 wystąpienia Geek kwizu")

    *Rozpocznij rozwiązania symulując 2 wystąpienia Geek kwizu*
2. Otwórz stronę właściwości rozwiązania prawym przyciskiem myszy węzeł rozwiązanie i wybierając **właściwości**. W obszarze **projekt startowy**, wybierz pozycję **wiele projektów startowych** i zmienić **akcji** wartość dla obu projektów do *Start*.

    ![Uruchamianie wielu projektów](real-time-web-applications-with-signalr/_static/image17.png "uruchamianie wielu projektów")

    *Uruchamianie wielu projektów*
3. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie. Aplikacja uruchomi dwa wystąpienia **quizu Geek** w różnych portów, co stanowi symulację wiele wystąpień tej samej aplikacji. Kod PIN jednego przeglądarek po lewej stronie oraz innych po prawej stronie ekranu. Zaloguj się przy użyciu poświadczeń lub zarejestrować nowego użytkownika. Po zalogowaniu, Zachowaj elementy towarzyszące składni strony po lewej stronie i przejdź do **statystyki** strony w przeglądarce po prawej stronie.

    ![Geek kwizu obok siebie](real-time-web-applications-with-signalr/_static/image18.png)

    *Geek kwizu obok siebie*

    ![Kwizu Geek w różnych portów](real-time-web-applications-with-signalr/_static/image19.png)

    *Kwizu Geek w różnych portów*
4. Uruchom odpowiadanie na pytania w przeglądarce po lewej stronie i będzie można zauważyć, że **statystyki** strony w przeglądarce prawo nie jest aktualizowana. Jest to spowodowane **SignalR** używa lokalnej pamięci podręcznej do dystrybucji wiadomości przez klientów i symuluje wiele wystąpień tego scenariusza, w związku z tym pamięci podręcznej nie jest udostępniana między nimi. Sprawdź, czy **SignalR** działa testowania czynności, ale przy użyciu jednej aplikacji. W następujących zadań skonfigurujesz IDE, aby replikować komunikaty w wystąpieniach.
5. Wróć do programu Visual Studio i Zatrzymaj debugowanie.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Zadanie 2 — Tworzenie płyty montażowej serwera SQL

W ramach tego zadania spowoduje utworzenie bazy danych, który będzie służyć jako płyty montażowej dla **Geek quizu** aplikacji. Użyjesz **Eksplorator obiektów SQL Server** przeglądania serwera i zainicjuj bazę danych. Ponadto umożliwi **Service Broker**.

1. W **programu Visual Studio**, otwórz menu **widoku** i wybierz **Eksplorator obiektów SQL Server**.
2. Połącz z wystąpieniem LocalDB klikając prawym przyciskiem myszy **programu SQL Server** węzła i wybierając **dodawania serwera SQL...**  opcji.

    ![Dodawanie wystąpienia programu SQL Server](real-time-web-applications-with-signalr/_static/image20.png "dodanie wystąpienia programu SQL Server")

    *Dodanie wystąpienia programu SQL Server do Eksplorator obiektów SQL Server*
3. Ustaw **nazwy serwera** do *(localdb) \v11.0* i pozostawić **uwierzytelniania systemu Windows** jako tryb uwierzytelniania. Kliknij przycisk **Connect** aby kontynuować.

    ![Łączenie z LocalDB](real-time-web-applications-with-signalr/_static/image21.png "połączenie do LocalDB")

    *Łączenie do LocalDB*
4. Teraz, gdy masz połączenie z wystąpieniem LocalDB, należy utworzyć bazę danych, reprezentujące środowiska IDE programu SQL Server dla elementu SignalR. Aby to zrobić, kliknij prawym przyciskiem myszy **baz danych** a następnie wybierz węzeł **Dodaj nową bazę danych**.

    ![Dodawanie nowej bazy danych](real-time-web-applications-with-signalr/_static/image22.png "Dodawanie nowej bazy danych")

    *Dodawanie nowej bazy danych*
5. Ustaw nazwę bazy danych na *SignalR* i kliknij przycisk **OK** go utworzyć.

    ![Tworzenie bazy danych SignalR](real-time-web-applications-with-signalr/_static/image23.png "tworzenie bazy danych biblioteki SignalR")

    *Tworzenie bazy danych biblioteki SignalR*

    > [!NOTE]
    > Można wybrać dowolną nazwę bazy danych.
6. Aby otrzymywać aktualizacje wydajniej z systemu backplane, zalecane jest Włącz program Service Broker dla bazy danych. Usługa Service Broker zapewnia macierzystą obsługę dla obsługi wiadomości i kolejkowania w programie SQL Server. Systemu backplane działa bez brokera usług. Otwórz nowe zapytanie, klikając prawym przyciskiem myszy bazę danych i wybierz **nowe zapytanie**.

    ![Otwieranie nowej kwerendy](real-time-web-applications-with-signalr/_static/image24.png "Otwieranie nowej kwerendy")

    *Otwieranie nowej kwerendy*
7. Aby sprawdzić, czy Usługa Service Broker jest włączona, zapytanie **jest\_brokera\_włączone** kolumny w **sys.databases** widoku katalogu. Uruchom następujący skrypt w oknie zapytania ostatnio otwartą.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Badanie stanu brokera usługi](real-time-web-applications-with-signalr/_static/image25.png "badanie stanu brokera usługi")

    *Badanie stanu brokera usługi*
8. Jeśli wartość **jest\_brokera\_włączone** kolumny w bazie danych jest &quot;0&quot;, użyj następującego polecenia, aby ją włączyć. Zastąp **&lt;bazy danych YOUR&gt;** o nazwie ustawione podczas tworzenia bazy danych (np.: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Włączenie brokera usług](real-time-web-applications-with-signalr/_static/image26.png "włączenie brokera usług")

    *Włączenie brokera usług*

    > [!NOTE]
    > Jeśli to zapytanie wydaje się do zakleszczenia, upewnij się, że nie ma żadnych aplikacji, połączony z bazą danych.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Zadanie 3 — Konfigurowanie aplikacji SignalR

W tym zadaniu zostanie skonfigurowana **Geek quizu** nawiązać środowiska IDE programu SQL Server. Najpierw dodamy **SignalR.SqlServer** pakietu NuGet i zestaw połączenie ciągu do płyty montażowej bazy danych.

1. Otwórz **Konsola Menedżera pakietów** z **narzędzia** | **Menedżer pakietów biblioteki**. Upewnij się, że **GeekQuiz** projekt jest wybrany w **domyślny projekt** listy rozwijanej. Wpisz następujące polecenie, aby zainstalować **Microsoft.AspNet.SignalR.SqlServer** pakietu NuGet.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Powtórz poprzedni krok, lecz tym razem dla projektu **GeekQuiz2**.
3. Aby skonfigurować środowiska IDE programu SQL Server, otwórz **Startup.cs** pliku **GeekQuiz** projektu i Dodaj następujący kod, aby **Konfiguruj** — metoda. Zastąp **&lt;bazy danych YOUR&gt;** nazwą bazy danych używane podczas tworzenia środowiska IDE programu SQL Server. Powtórz ten krok w przypadku **GeekQuiz2** projektu.

    (Fragment - kodu *StartupConfiguration RealTimeSignalR - Ex2 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Teraz, gdy oba projekty są skonfigurowane do używania środowiska IDE programu SQL Server, naciśnij klawisz **F5** ich uruchamiać jednocześnie.
5. Ponownie **programu Visual Studio** uruchomi dwa wystąpienia **quizu Geek** w różnych portów. Przypnij jedną z przeglądarek po lewej, a druga z prawej strony ekranu i zaloguj się przy użyciu poświadczeń. Zachowaj elementy towarzyszące składni strony po lewej stronie i przejdź do **statystyki** ładowanie stron do pamięci prawo przeglądarki.
6. Uruchom udzielenie odpowiedzi na pytania w przeglądarce po lewej stronie. Teraz, **statystyki** strona zostanie zaktualizowana dzięki systemu backplane. Przełączanie między aplikacjami (**statystyki** jest teraz, po lewej stronie, a **elementy towarzyszące składni** po prawej) i powtórz test, aby sprawdzić, czy działa ona zarówno wystąpień. Systemu backplane służy jako *pamięć podręczna* komunikatów dla każdego połączenia serwera, a każdy serwer zapisze komunikaty w ich własnej lokalnej pamięci podręcznej do dystrybucji do połączonych klientów.
7. Wróć do programu Visual Studio i Zatrzymaj debugowanie.
8. Składnik środowiska IDE programu SQL Server automatycznie generuje potrzebne tabele z określonej bazy danych. W **Eksplorator obiektów SQL Server** panelu, otwórz bazę danych utworzoną dla systemu backplane (np.: SignalR) i rozwiń listę tabel. Powinny pojawić się następujące tabele:

    ![Płyty montażowej wygenerowany tabel](real-time-web-applications-with-signalr/_static/image27.png)

    *Płyty montażowej wygenerowany tabel*
9. Kliknij prawym przyciskiem myszy **SignalR.Messages\_0** tabeli i wybierz **danych widoku**.

    ![Tabela komunikatów SignalR płyty montażowej widoku](real-time-web-applications-with-signalr/_static/image28.png)

    *Tabela komunikatów SignalR płyty montażowej widoku*
10. Widać, inne komunikaty wysyłane do **Centrum** podczas odpowiadania na pytania elementy towarzyszące składni. Systemu backplane dystrybuuje te komunikaty do dowolnego wystąpienia połączonych.

    ![Tabela komunikatów środowiska IDE](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabela komunikatów środowiska IDE*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium praktycznego ma przedstawiono sposób dodawania **SignalR** do aplikacji i wysyłania powiadomienia z serwera do połączonych klientów przy użyciu **koncentratory**. Ponadto przedstawiono sposób skalowania aplikacji za pomocą *płyty montażowej* składnika, po wdrożeniu w wielu wystąpień usług IIS aplikacji.
