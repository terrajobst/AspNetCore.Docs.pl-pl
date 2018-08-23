---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Ćwiczenia praktyczne: Aplikacje internetowe czasu rzeczywistego przy użyciu SignalR | Dokumentacja firmy Microsoft'
author: rick-anderson
description: Możliwość Wypchnij zawartość do dołączonych klientów, jak to się dzieje, w czasie rzeczywistym po stronie serwera są wyposażone w aplikacji sieci Web w czasie rzeczywistym. Dla deweloperów platformy ASP.NET i ASP...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a3f6174049ffddae4bb2a1819e3684bcdec1b55f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755786"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Ćwiczenia praktyczne: Aplikacje internetowe czasu rzeczywistego przy użyciu SignalR
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](http://aka.ms/webcamps-training-kit)

> Możliwość Wypchnij zawartość do dołączonych klientów, jak to się dzieje, w czasie rzeczywistym po stronie serwera są wyposażone w aplikacji sieci Web w czasie rzeczywistym. Dla deweloperów platformy ASP.NET **ASP.NET SignalR** jest biblioteką, aby dodać funkcje sieci web w czasie rzeczywistym do aplikacji. Wykorzystuje ona transportów kilka automatyczne wybieranie najlepiej transportu dostępnych danego klienta i serwera najlepsze dostępne mechanizm transportu. Wykorzystuje ona **WebSocket**, interfejs API języka HTML5, która umożliwia komunikację dwukierunkową między przeglądarką i serwerem.
> 
> **SignalR** udostępnia także prosty i ogólny interfejs API ten serwer do klienta RPC (wywołują funkcje JavaScript w przeglądarkach klientów z kodu .NET po stronie serwera) w aplikacji ASP.NET, a także dodawanie przydatne punkty zaczepienia umożliwiający zarządzanie połączeniami takie jak łączenie/rozłączanie połączeń zdarzenia, grupowanie połączeń i autoryzację.
> 
> **SignalR** jest warstwą abstrakcji nałożoną przez niektóre transportu, które są wymagane do pracy w czasie rzeczywistym między klientem i serwerem. A **SignalR** połączeniu rozpoczyna się jako HTTP, a następnie zostanie podwyższony do **WebSocket** połączenia, jeśli jest dostępny. **WebSocket** jest idealnym rozwiązaniem transport dla **SignalR**, ponieważ zapewnia najbardziej efektywne wykorzystanie pamięci serwera ma najniższe opóźnienie i ma większość podstawowych funkcji (takich jak komunikację pełnodupleksową między klientem i Server), ale ma również najbardziej rygorystyczne wymagania: **WebSocket** wymaga serwera, aby korzystać z **systemu Windows Server 2012** lub **systemu Windows 8**, wraz z **.NET framework 4.5**. Jeśli te wymagania nie są spełnione, **SignalR** podejmie próbę użycia innego transportu się jego połączenia (takich jak *Ajax długim sondowaniem*).
> 
> **SignalR** interfejs API zawiera dwa modele do komunikacji między klientami a serwerami: **połączeń trwałych** i **koncentratory**. A **połączenia** reprezentuje punkt końcowy proste wysyłanie pojedynczego adresata pogrupowane lub wiadomości emisji. A **Centrum** jest bardziej ogólny potokiem utworzonych na podstawie interfejsu API połączenia, który umożliwia klienta i serwera, bezpośrednie wywoływanie metod na siebie nawzajem.
> 
> ![Architektura SignalR](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Wysyłanie powiadomień z serwera do klienta przy użyciu SignalR.
- Skalowanie w poziomie swojej SignalR aplikacji za pomocą **programu SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym praktyczne laboratorium, należy najpierw skonfigurować swoje środowisko.

1. Otwórz okno Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **plik Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który będzie skonfigurować środowisko i zainstalować fragmenty kodu programu Visual Studio, w tym środowisku laboratoryjnym.
3. Jeśli zostanie wyświetlone okno dialogowe kontroli konta użytkownika, upewnij się, działania, aby kontynuować.

> [!NOTE]
> Upewnij się, że wszystkie zależności w tym środowisku laboratoryjnym sprawdzeniu przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Za pomocą fragmentów kodu

W dokumencie laboratorium należy poinstruować można wstawiać bloki kodu. Dla wygody większość ten kod jest dostarczany jako Visual Studio fragmenty kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązanie znajduje się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdego wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać, dopóki nie zakończysz wykonywania. Wewnątrz kodu źródłowego dla ćwiczenia, można również znaleźć **zakończenia** folderu zawierającego rozwiązania programu Visual Studio z kodem, który powstały na skutek wykonaniu kroków w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy, gdy pracujesz za pośrednictwem tego laboratorium praktycznego, można użyć jako wskazówki dotyczące tych rozwiązań.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje następujących czynnościach:

1. [Praca z danymi w czasie rzeczywistym przy użyciu biblioteki SignalR](#Exercise1)
2. [Skalowanie w poziomie przy użyciu programu SQL Server](#Exercise2)

Szacowany czas do ukończenia tego laboratorium: **60 minut**

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, należy wybrać jedną z kolekcji wstępnie zdefiniowanych ustawień. Każda kolekcja wstępnie zdefiniowanych służy do dopasowywania style rozwoju i określa układy okna, zachowanie edytora, fragmenty kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym środowisku laboratoryjnym opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólnych ustawieniach projektowych** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Ćwiczenie 1: Praca z danymi w czasie rzeczywistym przy użyciu biblioteki SignalR

Podczas rozmowy jest często używana jako przykład, można zrobić całości wiele więcej za pomocą funkcji sieci Web w czasie rzeczywistym. Ilekroć użytkownika odświeża stronę sieci web, aby zobaczyć nowe dane lub implementuje strony Ajax długim sondowaniem pobrać nowe dane, możesz użyć SignalR.

Obsługuje SignalR **wypychania przez serwer** lub **emisji** funkcjonalności; obsługuje ona zarządzanie połączeniami automatycznie. W klasycznym połączeń HTTP dla komunikacji klient serwer, ponownym nawiązaniu połączenia dla każdego żądania, ale Biblioteka SignalR udostępnia trwałe połączenie między klientem i serwerem. W bibliotece SignalR, który wywołuje kod serwera dla kodu klienta, w przeglądarce, za pomocą zdalnego wywołania procedury (RPC) zamiast modelu odpowiedź na żądanie Wiemy już dziś.

W tym ćwiczeniu skonfigurujesz **Quiz maniaków komputerowych** aplikacji na używanie biblioteki SignalR, aby wyświetlić pulpit nawigacyjny statystyki za pomocą zaktualizowanych metryk, bez konieczności odświeżenia całej strony.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Zadanie 1 — poznawanie strona statystyk Quiz maniaków komputerowych

W ramach tego zadania będzie przechodzą przez aplikację i sprawdzić, jak przedstawiono na stronie statystyki i jak można poprawić sposób informacje są aktualizowane.

1. Otwórz **Visual Studio Express 2013 for Web** , a następnie otwórz **GeekQuiz.sln** rozwiązanie znajduje się w **Source\Ex1 WorkingWithRealTimeData\Begin** folderu.
2. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie. **Zaloguj** strony powinna zostać wyświetlona w przeglądarce.

    ![Uruchamianie rozwiązania](real-time-web-applications-with-signalr/_static/image2.png "uruchamianie rozwiązania")

    *Uruchamianie rozwiązania*
3. Kliknij przycisk **zarejestrować** w prawym górnym rogu strony Aby utworzyć nowego użytkownika w aplikacji.

    ![Zarejestruj łącze](real-time-web-applications-with-signalr/_static/image3.png "link Zarejestruj")

    *Link Zarejestruj*
4. W **zarejestrować** wpisz **nazwa_użytkownika** i **hasło**, a następnie kliknij przycisk **zarejestrować**.

    ![Rejestrowanie użytkownika](real-time-web-applications-with-signalr/_static/image4.png "użytkownika")

    *Rejestrowanie użytkownika*
5. Aplikacja rejestruje nowego konta i użytkownik jest uwierzytelniony i przekierowanie z powrotem do strony głównej, przedstawiający na pierwsze pytanie quizu.
6. Otwórz **statystyki** strony w nowym oknie, a następnie umieść **Home** strony i **statystyki** stronie side-by-side.

    ![Windows Side-by-side](real-time-web-applications-with-signalr/_static/image5.png "po stronie przez system windows po stronie")

    *Side-by-side systemu windows*
7. W **Home** strony, Odpowiedz na pytanie, klikając jedną z opcji.

    ![Odpowiadanie na pytania](real-time-web-applications-with-signalr/_static/image6.png "odpowiadanie na pytania")

    *Odpowiadanie na pytania*
8. Po kliknięciu jednego z przycisków, powinna zostać wyświetlona odpowiedź.

    ![Prawidłowe odpowiedzi na pytanie](real-time-web-applications-with-signalr/_static/image7.png "prawidłowe odpowiedzi na pytanie")

    *Prawidłowej odpowiedzi na pytania*
9. Zwróć uwagę, czy informacje podane na stronie statystyki są nieaktualne. Odśwież stronę, aby wyświetlić zaktualizowane wyniki.

    ![Strona statystyk](real-time-web-applications-with-signalr/_static/image8.png "strona statystyk")

    *Strona statystyk*
10. Wróć do programu Visual Studio i zatrzymać debugowanie.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Zadanie 2 — Dodawanie SignalR do quizu maniaków komputerowych, aby wyświetlić wykresy Online

W ramach tego zadania możesz dodać SignalR do rozwiązania i automatycznie Wyślij aktualizacje klientów, gdy nową odpowiedź jest wysyłana do serwera.

1. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**.
2. W **Konsola Menedżera pakietów** okna, wykonaj następujące polecenie:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Instalacja pakietu SignalR](real-time-web-applications-with-signalr/_static/image9.png "instalacji pakietu SignalR")

    *Instalacja pakietu SignalR*

   > [!NOTE]
   > Podczas instalowania **SignalR** wersji pakietów NuGet pkt 2.0.2 z zupełnie nowych aplikacji MVC 5, należy ręcznie zaktualizować **OWIN** pakiety do wersji 2.0.1 (lub nowszej) przed zainstalowaniem SignalR. Aby to zrobić, można wykonać poniższy skrypt w **Konsola Menedżera pakietów**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > W przyszłej wersji biblioteki SignalR zależności OWIN zostaną automatycznie zaktualizowane.
3. W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty** folderu i zwróć uwagę, że SignalR *js* pliki zostały dodane do rozwiązania.

    ![Odwołuje się do biblioteki SignalR JavaScript](real-time-web-applications-with-signalr/_static/image10.png "odwołuje się do biblioteki SignalR JavaScript")

    *Odwołania do biblioteki SignalR JavaScript*
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **GeekQuiz** projektu, wybierz opcję **Dodaj** | **nowy Folder**i nadaj mu  **Koncentratory**.
5. Kliknij prawym przyciskiem myszy **Hubs** i wybierz polecenie **Dodaj | Nowy element**.

    ![Dodaj nowy element](real-time-web-applications-with-signalr/_static/image11.png "Dodaj nowy element")

    *Dodaj nowy element*
6. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# | W sieci Web | SignalR** węzła w okienku po lewej stronie wybierz **klasa Centrum SignalR (v2)** w środkowym okienku, nadaj plikowi nazwę **StatisticsHub.cs** i kliknij przycisk **Dodaj**.

    ![Okno dialogowe Dodaj nowy element](real-time-web-applications-with-signalr/_static/image12.png "okno dialogowe Dodaj nowy element")

    *Dodaj nowy element, okno dialogowe*
7. Zastąp kod w **StatisticsHub** klasy z następującym kodem.

    (Code Snippet — *StatisticsHubClass RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Otwórz **Startup.cs** i Dodaj następujący wiersz na końcu **konfiguracji** metody.

    (Code Snippet — *MapSignalR RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Otwórz **StatisticsService.cs** stronie wewnątrz **usług** folderu i dodaj następujące dyrektywy using.

    (Code Snippet — *UsingDirectives RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Aby powiadomić podłączonych klientów aktualizacji, należy najpierw pobrać **kontekstu** obiektu dla bieżącego połączenia. **Centrum** obiekt zawiera metody służące do wysyłania komunikatów do pojedynczego klienta lub emisji do wszystkich połączonych klientów. Dodaj następującą metodę do **StatisticsService** klasy wysyłać dane statystyk.

    (Code Snippet — *NotifyUpdatesMethod RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > W powyższym kodzie używane są nazwa dowolnego metody do wywołania funkcji na komputerze klienckim (czyli: *updateStatistics*). Nazwa metody, które określisz, jest interpretowany jako obiekt dynamiczny, co oznacza, że brak funkcji IntelliSense i weryfikacji kompilacji. Wyrażenie jest obliczane w czasie wykonywania. Podczas wywołania metody które wykonuje, SignalR wysyła nazwy metody i wartości parametrów do klienta. Jeśli klient ma metodę, która jest zgodna z nazwą, Metoda ta jest wywoływana, i wartości parametrów są przekazywane do niego. Jeśli odpowiadającej metody znajduje się na komputerze klienckim, zgłaszany jest błąd braku. Aby uzyskać więcej informacji, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET](../guide-to-the-api/hubs-api-guide-server.md).
11. Otwórz **TriviaController.cs** stronie wewnątrz **kontrolerów** folderu i dodaj następujące dyrektywy using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Dodaj następujący wyróżniony kod do **wpis** metody akcji.

    (Code Snippet — *NotifyUpdatesCall RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Otwórz **Statistics.cshtml** stronie wewnątrz **widoków | Strona główna** folderu. Znajdź **skrypty** sekcji i dodaj następujące odwołania do skryptu na początku sekcji.

    (Code Snippet — *SignalRScriptReferences RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Po dodaniu SignalR i inne biblioteki skryptu do projektu programu Visual Studio, Menedżera pakietów może zainstalować wersję pliku skryptu SignalR, która jest nowsza niż wersja przedstawione w tym temacie. Upewnij się, że odwołanie do skryptu w kodzie zgodny z wersją biblioteki skryptów zainstalowana w twoim projekcie.
14. Dodaj następujący wyróżniony kod, aby nawiązać połączenie klienta do Centrum SignalR i zaktualizować dane statystyk, po odebraniu nowej wiadomości z Centrum.

    (Code Snippet — *SignalRClientCode RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    W tym kodzie są serwer Proxy koncentratora do tworzenia i rejestrowania programu obsługi zdarzeń, aby nasłuchiwać komunikatów wysłanych przez serwer. W takim przypadku możesz nasłuchiwać komunikatów wysyłanych za pośrednictwem *updateStatistics* metody.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — uruchamianie rozwiązania

W tym zadaniu należy uruchomić rozwiązanie, aby sprawdzić, że widok statystyki jest aktualizowana automatycznie po udzieleniu odpowiedzi na nowe pytanie przy użyciu SignalR.

1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.

    > [!NOTE]
    > Jeśli jeszcze nie zalogował się do aplikacji, zaloguj się użytkownik, który został utworzony w zadaniu 1.
2. Otwórz **statystyki** strony w nowym oknie, a następnie umieść **Home** strony i **statystyki** stronie side-by-side, tak jak w zadaniu 1.
3. W **Home** strony, Odpowiedz na pytanie, klikając jedną z opcji.

    ![Odpowiadanie na następne pytanie](real-time-web-applications-with-signalr/_static/image13.png "odpowiadanie na następne pytanie")

    *Odpowiadanie na następne pytanie*
4. Po kliknięciu jednego z przycisków, powinna zostać wyświetlona odpowiedź. Należy zauważyć, że informacje statystyczne na stronie jest aktualizowane automatycznie po udzieleniu odpowiedzi na pytanie przy użyciu zaktualizowanych informacji bez konieczności odświeżenia całej strony.

    ![Strona statystyk odświeżone po odpowiedzi](real-time-web-applications-with-signalr/_static/image14.png "strona statystyk odświeżone po odpowiedzi")

    *Strona statystyk odświeżone po odpowiedzi*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Ćwiczenie 2: Skalowanie w poziomie przy użyciu programu SQL Server

Podczas skalowania aplikacji sieci web, zazwyczaj można wybrać między *skalowanie w górę* i *skalowanie w poziomie* opcje. *Skalowanie w górę* oznacza, że korzystasz z większych serwera więcej zasobów (procesor CPU, pamięci RAM, itp.), podczas *skalowanie w poziomie* oznacza dodawanie kolejnych serwerów w celu obsługi obciążenia. Problem z jego polega na tym, że klienci mogą uzyskać kierowane do różnych serwerów. Klient, który jest podłączony do jednego serwera nie będą otrzymywać wiadomości wysłanych z innego serwera.

Te problemy można rozwiązać za pomocą składnik o nazwie *płyty montażowej*w celu przekazywania komunikatów między serwerami. Z płyty montażowej włączone każde wystąpienie aplikacji wysyła komunikaty do systemu backplane. Ponadto systemu backplane przekazuje je do innych wystąpień aplikacji.

Obecnie istnieją trzy typy montażowych dla elementu SignalR:

- **Windows Azure Service Bus**. Service Bus to infrastruktura obsługi komunikatów, umożliwiający składników do wysyłania wiadomości luźno powiązane.
- **Program SQL Server**. Płyty montażowej programu SQL Server zapisuje komunikaty do tabel SQL. Systemu backplane używa brokera usług dla komunikatów wydajne. Jednak działa Jeśli programu Service Broker nie jest włączona.
- **Redis**. Redis jest przechowywanie par klucz wartość w pamięci. Usługa redis obsługuje wzorzec publikowania/subskrybowania ("pub/sub") do wysyłania wiadomości.

Każdy komunikat jest wysyłany za pośrednictwem magistrali komunikatów. Implementuje w magistrali komunikatów [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfejs, który udostępnia abstrakcji publikowania/subskrybowania. Montażowych pracy, zastępując domyślne **IMessageBus** z magistralą przeznaczone dla tego systemu backplane.

Każde wystąpienie serwera łączy do systemu backplane za pośrednictwem magistrali. Po wysłaniu komunikatu przejdzie do systemu backplane, a następnie wysyła je do każdego serwera systemu backplane. Gdy serwer otrzymuje komunikat z systemu backplane, wiadomości są przechowywane w lokalnej pamięci podręcznej. Serwer następnie dostarcza komunikaty do klientów z lokalnej pamięci podręcznej.

Aby uzyskać więcej informacji na temat sposobu płyty montażowej SignalR działania, przeczytaj ten artykuł [artykułu](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Istnieją sytuacje, w którym płyty montażowej może stać się wąskim gardłem. Poniżej przedstawiono kilka typowych scenariuszy SignalR:
> 
> - [Emisja serwera](tutorial-server-broadcast-with-signalr.md) (np. giełdowej): montażowych zadziałać dla tego scenariusza, ponieważ serwer kontroluje szybkość, z jaką komunikaty są wysyłane.
> - [Klient do klienta](tutorial-getting-started-with-signalr.md) (np. chat): W tym scenariuszu systemu backplane może być "wąskie gardło", jeśli liczba komunikatów jest skalowana o liczbie klientów; oznacza to, jeśli liczba komunikatów rośnie przyłączyć proporcjonalnie, ponieważ coraz więcej klientów.
> - [O wysokiej częstotliwości w czasie rzeczywistym](tutorial-high-frequency-realtime-with-signalr.md) (np. w czasie rzeczywistym gry): płyty montażowej nie jest zalecane w przypadku tego scenariusza.


W tym ćwiczeniu zostanie użyty **programu SQL Server** do dystrybucji wiadomości między **Quiz maniaków komputerowych** aplikacji. Te zadania będą uruchamiane na maszynie jeden test dowiesz się, jak ustawić konfigurację, ale w celu uzyskania pełnego wpływu, konieczne będzie wdrożenie aplikacji SignalR do co najmniej dwóch serwerów. SQL Server należy również zainstalować na jednym serwerze lub na oddzielnym serwerze dedykowanym.

![Skalowanie w poziomie przy użyciu programu SQL Server diagramu](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Zadanie 1 — Opis scenariusza

W tym zadaniu zostanie uruchomiony 2 wystąpienia **Quiz maniaków komputerowych** symulowania IIS wielu wystąpień na komputerze lokalnym. W tym scenariuszu podczas odpowiadania na pytania elementy towarzyszące składni z jednej aplikacji nie zostaną powiadomieni aktualizacji, na stronie statystyki drugie wystąpienie. Tej symulacji przypomina środowisko, w którym aplikacja jest wdrażana w wielu wystąpieniach i komunikować się z nimi przy użyciu modułu równoważenia obciążenia.

1. Otwórz **Begin.sln** rozwiązanie znajduje się w **/Ex2-ScalingOutWithSQLServer/początkowy w źródle** folderu. Po załadowaniu można zauważyć na **Eksploratora serwera** że rozwiązanie zawiera dwa projekty o identycznych struktur, ale o innej nazwy. Spowoduje to zasymulować, uruchomione dwa wystąpienia tej samej aplikacji na komputerze lokalnym.

    ![Rozpocznij rozwiązania, symulując 2 wystąpienia Quiz maniaków komputerowych](real-time-web-applications-with-signalr/_static/image16.png "rozpocząć rozwiązania, symulując 2 wystąpienia Quiz maniaków komputerowych")

    *Rozpocznij rozwiązania, symulując 2 wystąpienia Quiz maniaków komputerowych*
2. Otwórz stronę właściwości rozwiązania kliknij prawym przyciskiem myszy węzeł rozwiązania i wybierając **właściwości**. W obszarze **projekt startowy**, wybierz opcję **wiele projektów startowych** i zmień **akcji** wartość dla obu projektów na *Start*.

    ![Uruchomienie wielu projektów](real-time-web-applications-with-signalr/_static/image17.png "uruchomienie wielu projektów")

    *Uruchomienie wielu projektów*
3. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie. Aplikacja uruchomi dwa wystąpienia **Quiz maniaków komputerowych** w innych portów, symulując wielu wystąpień tej samej aplikacji. Po lewej stronie oraz inne po prawej stronie ekranu, Przypina jeden z przeglądarki. Zaloguj się przy użyciu poświadczeń lub rejestrowanie nowego użytkownika. Po zalogowaniu nadal na stronie elementy towarzyszące składni po lewej stronie i przejdź do **statystyki** strony w przeglądarce po prawej stronie.

    ![Quiz maniaków komputerowych obok siebie](real-time-web-applications-with-signalr/_static/image18.png)

    *Quiz maniaków komputerowych obok siebie*

    ![Quiz maniaków komputerowych w różnych portów](real-time-web-applications-with-signalr/_static/image19.png)

    *Quiz maniaków komputerowych w różnych portów*
4. Uruchom odpowiadanie na pytania w przeglądarce po lewej stronie i będzie można zauważyć, że **statystyki** strony w przeglądarce prawym nie jest aktualizowana. Jest to spowodowane **SignalR** używa lokalnej pamięci podręcznej dystrybuować komunikaty do swoich klientów i symuluje wiele wystąpień tego scenariusza, w związku z tym pamięci podręcznej nie jest udostępniana między nimi. Sprawdź, czy **SignalR** działa przez testowanie te same czynności, ale przy użyciu pojedynczej aplikacji. W następujących zadań, którą skonfigurujesz płyty montażowej do replikowania wiadomości między wystąpieniami.
5. Wróć do programu Visual Studio i zatrzymać debugowanie.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Zadanie 2 — Tworzenie płyty montażowej serwera SQL

W tym zadaniu utworzysz bazę danych, która będzie służyć jako płyty montażowej do **Quiz maniaków komputerowych** aplikacji. Użyjesz **Eksplorator obiektów SQL Server** przeglądania serwera i zainicjować bazy danych. Ponadto umożliwi **programu Service Broker**.

1. W **programu Visual Studio**, otwórz menu **widoku** i wybierz **Eksplorator obiektów SQL Server**.
2. Nawiąż połączenie z wystąpieniem usługi LocalDB, klikając prawym przyciskiem myszy **programu SQL Server** węzła i wybierając polecenie **dodawania serwera SQL...**  opcji.

    ![Dodawanie wystąpienia programu SQL Server](real-time-web-applications-with-signalr/_static/image20.png "dodanie wystąpienia programu SQL Server")

    *Dodawanie wystąpienia programu SQL Server do Eksplorator obiektów SQL Server*
3. Ustaw **nazwy serwera** do *(localdb) \v11.0* i pozostawić **uwierzytelniania Windows** z trybem uwierzytelniania. Kliknij przycisk **Connect** aby kontynuować.

    ![Nawiązywanie połączenia z LocalDB](real-time-web-applications-with-signalr/_static/image21.png "nawiązywania połączenia z LocalDB")

    *Nawiązywanie połączenia z LocalDB*
4. Skoro masz połączenie z wystąpieniem LocalDB, należy utworzyć bazę danych, która będzie reprezentowała płyty montażowej programu SQL Server dla elementu SignalR. Aby to zrobić, kliknij prawym przyciskiem myszy **baz danych** a następnie wybierz węzeł **Dodaj nową bazę danych**.

    ![Dodawanie nowej bazy danych](real-time-web-applications-with-signalr/_static/image22.png "dodanie nowej bazy danych")

    *Dodawanie nowej bazy danych*
5. Ustaw nazwę bazy danych na *SignalR* i kliknij przycisk **OK** do jego utworzenia.

    ![Tworzenie bazy danych SignalR](real-time-web-applications-with-signalr/_static/image23.png "tworzenie bazy danych biblioteki SignalR")

    *Tworzenie bazy danych biblioteki SignalR*

    > [!NOTE]
    > Możesz wybrać dowolną nazwę bazy danych.
6. Do systemu backplane wydajniej otrzymywać aktualizacje, zalecane jest aby włączyć brokera usługi dla bazy danych. Usługa Service Broker zapewnia macierzystą obsługę komunikatów i kolejkowania w programie SQL Server. Bez programu Service Broker działa również systemu backplane. Otwórz nowe zapytanie, klikając prawym przyciskiem myszy bazę danych i wybierz pozycję **nowe zapytanie**.

    ![Otwieranie nowej kwerendy](real-time-web-applications-with-signalr/_static/image24.png "Otwieranie nowej kwerendy")

    *Otwieranie nowej kwerendy*
7. Aby sprawdzić, czy programu Service Broker jest włączona, należy zbadać **jest\_brokera\_włączone** kolumny w **sys.databases** widok katalogu. Uruchom następujący skrypt w oknie zapytania ostatnio otwarte.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Badanie stanu brokera usługi](real-time-web-applications-with-signalr/_static/image25.png "badanie stanu brokera usług")

    *Badanie stanu brokera usług*
8. Jeśli wartość **jest\_brokera\_włączone** kolumna w bazie danych jest &quot;0&quot;, użyj następującego polecenia, aby go włączyć. Zastąp **&lt;YOUR DATABASE&gt;** o nazwie jest ustawiany podczas tworzenia bazy danych (np.: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Włączanie programu Service Broker](real-time-web-applications-with-signalr/_static/image26.png "włączenie brokera usług")

    *Włączanie programu Service Broker*

    > [!NOTE]
    > Jeśli wydaje się zakleszczenie, upewnij się, to zapytanie nie istnieją żadne aplikacje połączone z bazą danych.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Zadanie 3 — Konfigurowanie aplikacji SignalR

W tym zadaniu skonfigurujesz **Quiz maniaków komputerowych** połączyć się z płyty montażowej programu SQL Server. Należy najpierw dodać **SignalR.SqlServer** pakietu NuGet i zestawu połączenie ciąg do bazy danych systemu backplane.

1. Otwórz **Konsola Menedżera pakietów** z **narzędzia** | **Menedżer pakietów biblioteki**. Upewnij się, że **GeekQuiz** projekt jest wybrany w **projekt domyślny** listy rozwijanej. Wpisz następujące polecenie, aby zainstalować **Microsoft.AspNet.SignalR.SqlServer** pakietu NuGet.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Powtórz poprzedni krok, ale tym razem dla projektu **GeekQuiz2**.
3. Aby skonfigurować płyty montażowej programu SQL Server, otwórz **Startup.cs** pliku **GeekQuiz** projektu, a następnie dodaj następujący kod do **Konfiguruj** metody. Zastąp **&lt;YOUR DATABASE&gt;** z Twoją nazwą bazy danych, którego użyto podczas tworzenia płyty montażowej programu SQL Server. Powtórz ten krok w przypadku **GeekQuiz2** projektu.

    (Code Snippet — *StartupConfiguration RealTimeSignalR - Ex2 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Teraz, gdy oba projekty są skonfigurowane do korzystania z systemu backplane programu SQL Server, naciśnij klawisz **F5** do jednoczesnego uruchamiania.
5. Ponownie **programu Visual Studio** spowoduje uruchomienie dwa wystąpienia **Quiz maniaków komputerowych** w różnych portów. Przypina jeden z przeglądarki na po lewej stronie, a druga po prawej stronie ekranu, a następnie zaloguj się przy użyciu poświadczeń. Zachowaj na stronie elementy towarzyszące składni po lewej stronie i przejdź do **statystyki** ładowania strony przeglądarki prawo.
6. Rozpocznij, odpowiadanie na pytania w przeglądarce po lewej stronie. Tym razem **statystyki** strona zostanie zaktualizowana dzięki gotowej do systemu backplane. Przełączanie się między aplikacjami (**statystyki** jest teraz, po lewej stronie, a **elementy towarzyszące składni** po prawej) i powtórz test, aby sprawdzić, czy działa ona dla obu wystąpień. Systemu backplane służy jako *pamięć podręczna* wiadomości dla każdego połączonego serwera, a każdy serwer będzie przechowywać wiadomości w własnej lokalnej pamięci podręcznej do dystrybucji do połączonych klientów.
7. Wróć do programu Visual Studio i zatrzymać debugowanie.
8. Składnik systemu backplane programu SQL Server automatycznie generuje niezbędne tabele z określonej bazy danych. W **Eksplorator obiektów SQL Server** panelu, otwórz bazę danych utworzoną w ramach systemu backplane (np.: SignalR) i rozwiń jego tabel. Powinny zostać wyświetlone następujące tabele:

    ![Płyty montażowej generowane tabel](real-time-web-applications-with-signalr/_static/image27.png)

    *Płyty montażowej generowane tabel*
9. Kliknij prawym przyciskiem myszy **SignalR.Messages\_0** tabeli, a następnie wybierz pozycję **dane widoku**.

    ![Tabela komunikatów SignalR płyty montażowej widoku](real-time-web-applications-with-signalr/_static/image28.png)

    *Tabela komunikatów SignalR płyty montażowej widoku*
10. Można zobaczyć różne komunikaty wysyłane do **Centrum** podczas odpowiadania na pytania elementy towarzyszące składni. Systemu backplane dystrybuuje te komunikaty do dowolnego wystąpienia połączonych.

    ![Tabela komunikatów płyty montażowej](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabela komunikatów płyty montażowej*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym praktyczne laboratorium ma pokazaliśmy, jak dodać **SignalR** do aplikacji i wysyłania powiadomień z serwera do połączonych klientów przy użyciu **koncentratory**. Ponadto pokazano, jak skalować aplikację za pomocą *płyty montażowej* składnika, gdy aplikacja jest wdrażana w wielu wystąpieniach programu IIS.
