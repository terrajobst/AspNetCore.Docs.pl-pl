---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: Połączeń i przejmowanie poleceń z platformą Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: bacc49ff60b758b729c200f7943ff654f614ac4c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394797"
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Połączeń i przejmowanie poleceń z platformą Entity Framework w aplikacji ASP.NET MVC
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [Pobierz plik PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i Visual Studio 2013. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Do tej pory aplikacja była uruchomiona lokalnie w usługach IIS Express na komputerze deweloperskim. Aby udostępnić rzeczywistej aplikacji innym osobom korzystanie przez Internet, należy wdrożyć je do dostawcy usług hosta sieci web, a trzeba wdrożyć bazę danych serwera bazy danych.

W tym samouczku dowiesz się, jak korzystać z dwóch funkcji programu Entity Framework 6, które są szczególnie przydatne w przypadku wdrażania w środowisku chmury: połączenia (Automatyczne ponawianie prób dla błędów przejściowych) i przejmowanie poleceń (catch, wszystkie zapytania SQL wysyłane do bazy danych, aby można było zalogować się lub je zmienić).

Ten samouczek przejmowanie połączenia, jak odporność i polecenia jest opcjonalne. Jeśli pominiesz ten samouczek kilku drobnych korekt musi nastąpić w kolejnych samouczkach.

## <a name="enable-connection-resiliency"></a>Włączanie połączeń

Podczas wdrażania aplikacji na platformie Windows Azure to usługa bazy danych w chmurze dla systemu Windows Azure SQL Database będzie wdrożyć bazę danych. Błędy przejściowe połączenia są zazwyczaj częściej, po nawiązaniu połączenia z usługą bazy danych w chmurze niż gdy serwer sieci web i serwera bazy danych są bezpośrednio połączonych ze sobą w tym samym centrum danych. Nawet wtedy, gdy serwer sieci web w chmurze i usługi w chmurze bazy danych znajdują się w tym samym centrum danych, istnieje więcej połączeń sieciowych między nimi, które mogą mieć problemy, takie jak moduły równoważenia obciążenia.

Również usługi w chmurze jest zwykle udostępnione przez innych użytkowników, co oznacza, że jego czas reakcji mogą mieć wpływ je. A może podlegać ograniczania dostępu do bazy danych. Ograniczanie oznacza, że usługa bazy danych zgłasza wyjątek wyjątków podczas próby dostępu do niego więcej często nie jest dozwolona w Twojej umowy poziomu usług (SLA).

Wiele lub większości problemów z połączeniem gdy uzyskujesz dostęp do usługi w chmurze są przejściowe, czyli rozpoznają się w krótkim czasie. Dlatego kiedy sprawdzasz operacji bazy danych i uzyskać typ błędu, który jest zwykle charakter przejściowy, możesz spróbować ponownie wykonać operację po krótkim czasie oczekiwania, a operacja może zakończyć się pomyślnie. Zapewnienie znacznie lepsze środowisko dla użytkowników w przypadku obsługi błędów przejściowych, automatycznie podjęcie ponownej próby wykonania, ukrywanie większość z nich do klienta. Tej funkcji odporności połączenia w programie Entity Framework 6 automatyzuje proces podejmowania próby ponownego wykonywania zapytań SQL się niepowodzeniem.

Funkcji elastyczności połączenia musi być skonfigurowany odpowiednio dla konkretnej bazy danych usługi:

- Musi wiedzieć, które wyjątki są może być błędem przejściowym. Chcesz ponowić próbę błędy spowodowane do tymczasowej utraty w łączności sieciowej nie błędy spowodowane przez błędy programu, na przykład.
- Ma czekać odpowiednią ilość czasu między kolejnymi próbami operacji nie powiodło się. Możesz poczekać już między kolejnymi próbami dla przetwarzania wsadowego nie można uzyskać online strony sieci web, w którym użytkownik jest oczekiwanie na odpowiedź.
- Posiada ponowić próbę odpowiednią liczbę razy, zanim zrezygnuje. Być może chcesz ponowić próbę więcej razy w procesie przetwarzania wsadowego, który będzie w trybie online aplikacji.

Można skonfigurować te ustawienia ręcznie dla dowolnego środowiska bazy danych obsługiwane przez dostawcę programu Entity Framework, ale wartości domyślne, które zazwyczaj działają dobrze sprawdza się w trybie online aplikacji korzystającej z Windows Azure SQL Database zostały już skonfigurowane, i to są ustawienia które będzie implementowana dla aplikacji Contoso University.

Wystarczy zrobić, aby włączyć elastyczność połączenia jest utworzyć klasę w swoim zestawie, która pochodzi od klasy [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) klasy, a w tej klasy należy ustawić bazy danych SQL *strategii wykonywania*, co w programie EF jest inna nazwa *zasady ponawiania*.

1. W folderze DAL, Dodaj plik klasy o nazwie *SchoolConfiguration.cs*.
2. Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework automatycznie uruchamia kod znajdzie się w klasie, która pochodzi od klasy `DbConfiguration`. Możesz użyć `DbConfiguration` klasie w celu wykonania zadania konfiguracji w kodzie, które w przeciwnym razie należy *Web.config* pliku. Aby uzyskać więcej informacji, zobacz [EntityFramework konfiguracja na podstawie kodu](https://msdn.microsoft.com/data/jj680699).
3. W *StudentController.cs*, Dodaj `using` poufności informacji dotyczące `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Zmienianie ich wszystkich `catch` bloki catch tego `DataException` wyjątki, aby ich catch `RetryLimitExceededException` wyjątków zamiast tego. Na przykład:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Były używane `DataException` aby spróbować zidentyfikować błędy, które mogą być przejściowe, aby dać przyjazny komunikat "spróbuj ponownie". Ale teraz, gdy została włączona dla zasad ponawiania, tylko błędy, które mogą być przejściowe będzie już mieć próbowała i nie powiodło się kilka razy, a faktyczny wyjątek, zwrócone zostaną opakowane w `RetryLimitExceededException` wyjątku.

Aby uzyskać więcej informacji, zobacz [elastyczność połączenia programu Entity Framework / logika ponowień](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Włącz przejmowanie poleceń

Teraz, gdy została włączona dla zasad ponawiania, jak możesz przetestować Aby sprawdzić, czy działa ona zgodnie z oczekiwaniami? Nie jest tak proste wymusić Błąd przejściowy do wykonania, szczególnie gdy korzystasz lokalnie i może być szczególnie trudne do integracji rzeczywiste błędy przejściowe testów automatycznych jednostkowych. Aby przetestować połączenie funkcji odporności, potrzebujesz sposobu przechwycenia zapytań, które platformy Entity Framework wysyła do serwera SQL i Zamień na typ wyjątku, który jest zwykle charakter przejściowy odpowiedzi programu SQL Server.

Przejmowanie zapytania można również użyć w celu wdrożenia najlepszym rozwiązaniem dla aplikacji w chmurze: [Zaloguj się opóźnienie i powodzenie lub niepowodzenie wszystkie wywołania usług zewnętrznych](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) takich jak bazy danych usługi. Udostępnia EF6 [rejestrowanie interfejsu API w wersji dedykowanej](https://msdn.microsoft.com/data/dn469464) która może ułatwić celu rejestrowania, ale w tej części samouczka dowiesz się, jak używać programu Entity Framework [funkcji przejmowanie](https://msdn.microsoft.com/data/dn469464) bezpośrednio, zarówno dla aplikacji Rejestrowanie i symulowania błędów przejściowych.

### <a name="create-a-logging-interface-and-class"></a>Tworzenie interfejsu rejestrowania i klasy

A [najlepszymi rozwiązaniami w zakresie rejestrowania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) jest zrobienie tego przy użyciu interfejsu zamiast kodować wywołania System.Diagnostics.Trace lub klasa rejestrowania. Ułatwia zmienić swoje mechanizmie rejestrowania później, jeśli kiedykolwiek trzeba to zrobić. Aby w tej sekcji utworzysz interfejs rejestrowania i klasy do zaimplementowania go/p > 

1. Utwórz folder w projekcie i nadaj mu nazwę *rejestrowania*.
2. W *rejestrowania* folderze utwórz plik klasy o nazwie *ILogger.cs*i Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Interfejs zawiera trzy poziomy śledzenia, aby wskazać względne znaczenie dzienniki i jeden zapewniają informacje opóźnienie do wywołań usług zewnętrznych, takich jak zapytania do bazy danych. Metody rejestrowania mają przeciążenia, które umożliwiają przekazywanie wyjątek. To dlatego, że informacje o wyjątku, w tym wyjątki wewnętrzne i ślad stosu jest niezawodny sposób będą rejestrowane przez klasę, która implementuje interfejs, zamiast polegać na tym wykonywana w każdym wywołaniu metody rejestrowania w całej aplikacji.

    Metody TraceApi pozwalają śledzić czas oczekiwania na każde wywołanie usługi zewnętrznej, takich jak SQL Database.
3. W *rejestrowania* folderze utwórz plik klasy o nazwie *Logger.cs*i Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Implementacja używa System.Diagnostics celu śledzenie. Jest to wbudowana funkcja .NET, która ułatwia generowanie i używanie informacji śledzenia. Istnieje wiele "odbiorniki" za pomocą śledzenia System.Diagnostics na zapisywanie dzienników do przechowywania plików, na przykład lub zapisać je do magazynu obiektów blob na platformie Azure. Niektóre opcje i linki do innych zasobów, aby uzyskać więcej informacji, zobacz w [Rozwiązywanie problemów z witryn sieci Web platformy Azure w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). W tym samouczku możesz tylko przyjrzymy się dzienników w programie Visual Studio **dane wyjściowe** okna.

    W aplikacji produkcyjnej warto wziąć pod uwagę śledzenie pakietów innych niż System.Diagnostics i interfejs ILogger sprawia, że stosunkowo łatwo zmienić mechanizm śledzenia inny, jeśli zdecydujesz się to zrobić.

### <a name="create-interceptor-classes"></a>Tworzenie klas interceptor

Następnie utworzysz klas, które platformy Entity Framework będzie wywoływać za każdym razem, gdy będzie znajdował się wysłanie zapytania do bazy danych — jedną symulowanie błędów przejściowych, a jeden w celu rejestrowania. Te klasy interceptor muszą pochodzić od `DbCommandInterceptor` klasy. W tych można napisać zastąpienia metody, które automatycznie są wywoływane, gdy zapytanie ma zostać wykonana. W tych metodach można zbadać lub zaloguj się zapytanie, które są wysyłane do bazy danych i można zmienić kwerendę, przed wysłaniem ich do bazy danych lub zwrócić coś w programie Entity Framework samodzielnie bez nawet przekazywania zapytania do bazy danych.

1. Aby utworzyć klasę interceptor, która zarejestruje każdego zapytania SQL, które są wysyłane do bazy danych, Utwórz plik klasy o nazwie *SchoolInterceptorLogging.cs* w *DAL* folder i Zastąp kod szablonu Poniższy kod:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Pomyślne zapytania lub polecenia ten kod zapisuje w dzienniku informacji z informacjami o opóźnieniu. Wyjątki tworzy dziennik błędów.
2. Aby utworzyć klasę interceptor, który zostanie wygenerowany fikcyjnego błędów przejściowych, po wprowadzeniu "Throw" w **wyszukiwania** Utwórz plik klasy o nazwie *SchoolInterceptorTransientErrors.cs* w *DAL* folder i Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Ten kod tylko zastąpienia `ReaderExecuting` metody, która jest wywoływana dla zapytań zwracających wiele wierszy danych. Chcąc sprawdzić elastyczność połączenia dla innych typów zapytań, można także Przesłoń `NonQueryExecuting` i `ScalarExecuting` metod jako interceptor rejestrowania wykonuje.

    Po uruchomieniu strony dla uczniów i wprowadź "Throw" w postaci ciągu wyszukiwania, ten kod tworzy fikcyjnego wyjątek bazy danych SQL, aby numer błędu 20, typem wiadomo, że są zwykle charakter przejściowy. Inne liczby błędów, obecnie jest rozpoznawany jako przejściowe są 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 i 40613, ale te mogą ulec zmianie w nowych wersji bazy danych SQL.

    Kod zwraca wyjątek w programie Entity Framework zamiast uruchamiania zapytania i przekazywanie wyników zapytania Wstecz. Zwracany jest wyjątek przejściowy, cztery razy, a następnie kod wraca do normalnej procedury przekazywania zapytania do bazy danych.

    Ponieważ wszystko jest rejestrowane, będzie można zobaczyć, że platformy Entity Framework próbuje wykonać zapytanie cztery razy przed na koniec pomyślne wykonanie, a jedyną różnicą w aplikacji, zajmuje więcej czasu renderowania strony z wynikami zapytania.

    Liczba przypadków, gdy spróbuje ponowić operację platformy Entity Framework jest konfigurowany; Kod ten określa cztery razy, ponieważ jest to wartość domyślna dla zasad wykonywania programu bazy danych SQL. W przypadku zmiany zasad wykonywania programu, trzeba było również zmiany tutaj kod, który określa, ile razy są generowane błędy przejściowe. Można również zmienić kod można wygenerować więcej wyjątków, aby zgłosi Entity Framework `RetryLimitExceededException` wyjątku.

    Wartość wprowadzoną w polu wyszukiwania będą znajdować się w `command.Parameters[0]` i `command.Parameters[1]` (jeden służy do imienia i jeden dla nazwisko). Po znalezieniu wartość "% Throw %", "Throw" zastępuje w tych parametrów "", aby pewne studenci zostanie znalezione i zwrócony.

    Jest to po prostu wygodny sposób testowanie odporności połączenia, w zależności od zmieniających się dane wejściowe do aplikacji interfejsu użytkownika. Można także napisać kod, który generuje błędy przejściowe dla wszystkich zapytań lub aktualizacje, zgodnie z opisem w dalszej części komentarze dotyczące *DbInterception.Add* metody.
3. W *Global.asax*, Dodaj następujący kod `using` instrukcji:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Dodaj wyróżnione wiersze w celu `Application_Start` metody:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Następujące wiersze kodu są na tym, co powoduje, że kod interceptor do uruchomienia programu Entity Framework wysyła zapytania do bazy danych. Należy zauważyć, że ponieważ utworzono oddzielne interceptor klas na potrzeby symulacji błędu przejściowego i rejestrowania, można niezależnie włączać i wyłączać je.

    Możesz dodać za pomocą interceptory `DbInterception.Add` metody dowolnym miejscu w kodzie; nie musi znajdować się w `Application_Start` metody. Innym rozwiązaniem jest umieszczenie tego kodu w klasie DbConfiguration, która została utworzona wcześniej do konfigurowania zasad wykonywania programu.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Wszędzie tam, gdzie należy umieścić ten kod nie należy do wykonania `DbInterception.Add` dla tego samego interceptor więcej niż jeden raz lub możesz uzyskać interceptor dodatkowych wystąpień. Na przykład jeśli dodasz dwa razy interceptor rejestrowanie, zobaczysz dwa dzienniki dla każdego zapytania SQL.

    Interceptory są wykonywane zgodnie z kolejnością rejestracji (kolejność, w której `DbInterception.Add` wywoływana jest metoda). Kolejność może znaczenia, w zależności od tego, co robisz w interceptor. Na przykład interceptor mogą ulec zmianie polecenia SQL, które otrzymuje w `CommandText` właściwości. Jeśli zmienia się polecenia SQL, dalej interceptor zostanie zmienione polecenia SQL nie oryginalne polecenie SQL.

    Kodzie symulacji błędu przejściowego napisanych w taki sposób, że można spowodować błędy przejściowe, wprowadzając inną wartość w interfejsie użytkownika. Alternatywnie można napisać kod interceptor, aby zawsze Generuj sekwencji przejściowych wyjątków bez sprawdzania pod kątem wartości określonego parametru. Następnie można dodać interceptor tylko wtedy, gdy chcesz wygenerować błędów przejściowych. Jeśli to zrobisz, jednak nie dodać interceptor do momentu po ukończeniu inicjowania bazy danych. Innymi słowy należy wykonać co najmniej jedna baza danych operacji, takich jak zapytania na jednym z zestawów encji, przed rozpoczęciem generowania błędów przejściowych. Entity Framework wykonuje kilka zapytań podczas inicjowania bazy danych, a nie są one wykonywane w ramach transakcji, więc błędy podczas inicjowania może spowodować, że kontekst do pobrania w niespójnym stanie.

## <a name="test-logging-and-connection-resiliency"></a>Test połączenia i rejestrowania odporności

1. Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania, a następnie kliknij przycisk **studentów** kartę.
2. Przyjrzyj się programu Visual Studio **dane wyjściowe** okno, aby wyświetlić dane wyjściowe śledzenia. Może być konieczne przewinięcie ostatnie niektóre błędy języka JavaScript, aby uzyskać dostęp do dzienników napisany przez użytkownika rejestratora.

    Zwróć uwagę, możliwość wyświetlenia rzeczywiste zapytania SQL wysyłane do bazy danych. Zobaczysz niektóre początkowej zapytań i poleceń, które obsługuje platformy Entity Framework, aby rozpocząć pracę, sprawdzanie wersji bazy danych i tabeli historii migracji (poznasz migracji w następnym samouczku). Zobacz zapytanie dotyczące stronicowania, aby dowiedzieć się, jak wiele studentów, i na koniec zobaczysz zapytanie, które pobiera dane dla uczniów.

    ![Rejestrowanie dla kwerendy normalny](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. W **studentów** strony, wprowadź "Throw" w postaci ciągu wyszukiwania, a następnie kliknij przycisk **wyszukiwania**.

    ![Throw ciąg wyszukiwania](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Zauważysz, przeglądarka prawdopodobnie zawieszanie przez kilka sekund, natomiast Entity Framework ponawia próbę zapytanie kilka razy. Pierwszym ponowieniem próby odbywa się bardzo szybko, następnie czas oczekiwania przed zwiększa przed każdym dodatkowe ponowieniem próby. Ten proces jest już oczekiwania nosi nazwę każdego ponawiania *wykładniczego wycofywania*.

    Kiedy zostanie wyświetlona strona, przedstawiający uczniów, którzy mają "na" w nazwach, Przyjrzyj się w oknie danych wyjściowych, a zobaczysz, że tego samego zapytania podjęto pięciokrotnie pierwsze cztery razy zwróceniem przejściowych wyjątków. Dla każdego błędu przejściowego zostaną wyświetlone w dzienniku, który napiszesz podczas generowania błędu przejściowego w `SchoolInterceptorTransientErrors` klasy ("zwrócenie przejściowy błąd polecenia..."), aby zobaczyć dziennika zapisywane kiedy `SchoolInterceptorLogging` pobiera wyjątek.

    ![Dane wyjściowe rejestrowanie przedstawiający liczbę ponownych prób](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Zapytanie, które zwraca dane dla uczniów jest sparametryzowane, ponieważ wprowadzony ciąg wyszukiwania:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Nie masz rejestruje wartości parametrów, ale można to zrobić. Jeśli chcesz wyświetlić wartości parametrów, można napisać kod rejestrowania, aby uzyskać wartości parametrów z `Parameters` właściwość `DbCommand` obiekt, który otrzymujesz za pomocą metod interceptor.

    Należy pamiętać, że nie Powtórz ten test, chyba że Zatrzymaj aplikację i uruchom go ponownie. Jeśli chcesz testować elastyczność połączenia wiele razy w jednym przebiegu aplikacji, można napisać kod, aby zresetować licznik błędów w `SchoolInterceptorTransientErrors`.
4. Aby zobaczyć różnicę strategii wykonywania (zasady ponawiania) sprawia, że, komentarz `SetExecutionStrategy` linię *SchoolConfiguration.cs*, a następnie ponownie uruchom strony studentów w trybie debugowania i ponownie wyszukać "Throw".

    Tym razem debuger zatrzymuje się na pierwszy wyjątek wygenerowane natychmiast, gdy próbuje wykonać zapytanie po raz pierwszy.

    ![Fikcyjny wyjątku](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Usuń znaczniki komentarza *SetExecutionStrategy* linię *SchoolConfiguration.cs*.

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazaliśmy już, jak dziennika poleceń SQL, które platformy Entity Framework Redaguj i wysyła do bazy danych i Włącz elastyczność połączenia. W następnym samouczku wdrożysz aplikację z Internetem, użycia migracje Code First na potrzeby wdrażania bazy danych.

Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię. Możesz również poprosić o nowe tematy w [Pokaż mi jak za pomocą kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
