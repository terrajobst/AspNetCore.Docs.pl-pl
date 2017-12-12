---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Elastyczność połączenia i przechwytywaniu polecenia Entity Framework w aplikacji platformy ASP.NET MVC | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fecdd582918a61f3d01519c75d159f9c601c8223
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Elastyczność połączenia i przechwytywaniu polecenia Entity Framework w aplikacji platformy ASP.NET MVC
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Do tej pory aplikacja była uruchomiona lokalnie w usługach IIS Express na komputerze deweloperskim. Aby udostępnić rzeczywistej aplikacji dla innych osób do użycia w Internecie, należy wdrożyć ją do usługi hosta sieci web i jest wdrożenie bazy danych na serwerze bazy danych.

W tym samouczku dowiesz się, jak używać dwóch funkcji Entity Framework 6, które są szczególnie przydatne w przypadku wdrażania w środowisku chmury: odporność połączenia (Automatyczne ponawianie prób przypadku błędów przejściowych) i przechwytywaniu polecenia (catch wszystkie zapytania SQL wysyłane do bazy danych w celu logowania się lub je zmienić).

W tym samouczku zatrzymania połączenia, jak odporność i polecenia jest opcjonalna. Jeśli pominiesz ten samouczek kilka niewielkich dostosowań będzie musiał zostać wprowadzone w kolejnych samouczkach.

## <a name="enable-connection-resiliency"></a>Włącz elastyczność połączenia

Podczas wdrażania aplikacji w systemie Windows Azure będzie wdrażania bazy danych do usługi Windows Azure SQL Database, usługa bazy danych w chmurze. Błędów przejściowych połączenia są zwykle częstsze podczas łączenia z usługą w chmurze bazy danych niż podczas serwera sieci web i serwer bazy danych są bezpośrednio połączone w tym samym centrum danych. Nawet jeśli serwer sieci web w chmurze i usługi w chmurze bazy danych znajdują się w tym samym centrum danych, istnieje więcej połączeń sieciowych między nimi, które mogą wystąpić problemy, takie jak moduły równoważenia obciążenia.

Również usługa w chmurze jest zwykle udostępnione przez innych użytkowników, co oznacza, że jego czasu odpowiedzi mogą mieć wpływ je. I dostęp do bazy danych mogą być narażone na ograniczania. Ograniczanie oznacza, że usługa bazy danych zgłasza wyjątków podczas próby dostępu do niego więcej często nie jest dozwolona w Twojej usługi Umowa dotycząca poziomu (SLA).

Wiele lub większości problemów połączenia, gdy uzyskujesz dostęp do usługi w chmurze są przejściowe, czyli rozpoznają się w krótkim czasie. Dlatego podczas próby operacji bazy danych pobrać typu błędu, który jest zwykle charakter przejściowy, możesz można spróbuj ponownie wykonać operację po krótkim oczekiwania, a operacja może zakończyć się pomyślnie. Zapewnienie dużo lepsze środowisko dla użytkowników w przypadku obsługi błędów przejściowych podejmując automatycznie ponownie, ukrywanie większość z nich do klienta. Funkcji odporności połączenia w programie Entity Framework 6 automatyzuje proces ponawianie próby niepowodzeniu zapytania SQL.

Funkcji odporności połączenia muszą być skonfigurowane odpowiednio dla usługi określona baza danych:

- Musi wiedzieć, które wyjątki są może być przejściowy. Chcesz ponowić próbę błędy spowodowane do tymczasowej utraty w łączności sieciowej, nie błędy spowodowane przez błędy programu, na przykład.
- Musi ona oczekiwać odpowiednią ilość czasu między kolejnymi próbami operacji nie powiodło się. Można poczekać już między kolejnymi próbami przetwarzania wsadowego niż jest to możliwe online strony sieci web, w którym użytkownik jest oczekiwania na odpowiedź.
- Posiada ponowić próbę odpowiednią liczbę razy, zanim zrezygnuje. Możesz ponowić próbę więcej razy w przetwarzania wsadowego, które są w trybie online aplikacji.

Można skonfigurować te ustawienia ręcznie w każdym środowisku bazy danych, obsługiwane przez dostawcy programu Entity Framework, ale zostały już skonfigurowane wartości domyślne, które zwykle działa dobrze w przypadku aplikacji online, która używa bazy danych SQL Azure z systemem Windows, a są to ustawienia, które będzie wdrożenia dla aplikacji Contoso University.

Wszystkie trzeba wykonać, aby włączyć elastyczności połączenia jest utworzyć klasę w używanego zestawu, która pochodzi z [DbConfiguration](https://msdn.microsoft.com/en-us/data/jj680699.aspx) klasy, a w tej klasie ustawić bazy danych SQL *strategia wykonywania*, która w EF jest inna nazwa *zasady ponawiania*.

1. W folderze DAL, Dodaj plik klasy o nazwie *SchoolConfiguration.cs*.
2. Zastąp kod szablonu z następującym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework automatycznie uruchamia kod znajdzie się w klasie, która jest pochodną `DbConfiguration`. Można użyć `DbConfiguration` klasy do wykonywania zadań konfiguracji w kodzie, które w przeciwnym razie należy *Web.config* pliku. Aby uzyskać więcej informacji, zobacz [EntityFramework konfiguracji opartej na kodzie](https://msdn.microsoft.com/en-us/data/jj680699).
3. W *StudentController.cs*, Dodaj `using` instrukcji dla `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Zmień wszystkie `catch` bloki catch tego `DataException` wyjątki, aby ich catch `RetryLimitExceededException` wyjątki zamiast tego. Na przykład:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    W przypadku używania `DataException` celu zidentyfikowania błędów, które mogą być przejściowy, aby zapewnić przyjazny komunikat "spróbuj ponownie". Ale teraz, gdy włączono zasady ponawiania tylko błędy, które mogą być przejściowy będzie już zostały nastąpiła i nie można wielokrotnie i faktyczny wyjątek, zwrócone zostaną opakowane w `RetryLimitExceededException` wyjątku.

Aby uzyskać więcej informacji, zobacz [Entity Framework połączenia odporności / ponów logiki](https://msdn.microsoft.com/en-us/data/dn456835).

## <a name="enable-command-interception"></a>Włącz polecenie zatrzymania

Po włączeniu zasady ponawiania jak przetestowanie można zweryfikować, że działa zgodnie z oczekiwaniami? Nie jest tak proste wymusić Błąd przejściowy się stać, szczególnie gdy używasz lokalnie i być szczególnie trudne do integracji rzeczywiste błędów przejściowych testu jednostkowego automatyczne. Aby przetestować funkcje ochrony połączenia, należy przechwycić zapytań, które Entity Framework wysyła do serwera SQL i Zamień na typ wyjątku, który jest zwykle charakter przejściowy odpowiedź serwera SQL.

Przechwycenie zapytania można również użyć w celu wdrożenia najlepszym rozwiązaniem dla aplikacji w chmurze: [dziennika opóźnienia i powodzenie lub niepowodzenie wszystkie wywołania usług zewnętrznych](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) takich jak usługi bazy danych. Udostępnia EF6 [rejestrowania interfejsu API w wersji dedykowanej](https://msdn.microsoft.com/en-us/data/dn469464) która może ułatwić do rejestrowania, ale w tej części samouczka dowiesz się, jak używać programu Entity Framework [funkcji zatrzymania](https://msdn.microsoft.com/en-us/data/dn469464) bezpośrednio, zarówno dla Rejestrowanie i symulowanie błędów przejściowych.

### <a name="create-a-logging-interface-and-class"></a>Utwórz interfejs rejestrowania i klasy

A [najlepsze praktyki w zakresie rejestrowania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) jest to zrobić przy użyciu interfejsu zamiast kodować wywołania System.Diagnostics.Trace lub klasy rejestrowania. Który ułatwia zmień mechanizm z rejestrowania w później, jeśli trzeba to zrobić. Dlatego w tej sekcji utworzysz interfejs rejestrowania i klasę, aby zaimplementować z niej/p > 

1. Utwórz folder w projekcie i nadaj jej nazwę *rejestrowanie*.
2. W *rejestrowanie* folderu, Utwórz plik klasy o nazwie *ILogger.cs*i Zastąp kod szablonu z następującym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Interfejs zawiera trzy poziomy śledzenia, aby wskazać względną wagę dzienników i jedną zapewniają informacje opóźnienia w wywołaniach zewnętrznych usług takich jak kwerend bazy danych. Metody rejestrowania mają przeciążeń, które umożliwiają przekazywanie wyjątku. Jest tak, aby informacje o wyjątku, w tym wyjątki wewnętrzne i śledzenia stosu niezawodnie jest rejestrowane przez klasę, która implementuje interfejs, zdejmując to zadanie, na którym wykonywana w każdym wywołaniu metody rejestrowania w całej aplikacji.

    Metody TraceApi umożliwiają śledzenie opóźnień każde wywołanie zewnętrznej usługi takie jak bazy danych SQL.
3. W *rejestrowanie* folderu, Utwórz plik klasy o nazwie *Logger.cs*i Zastąp kod szablonu z następującym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Implementacja używa System.Diagnostics celu śledzenie. Jest to funkcja wbudowana platformy .NET, co ułatwia generowanie i używanie informacji śledzenia. Istnieje wiele "odbiorników" można użyć z włączonym śledzeniem System.Diagnostics, zapisywanie dzienników do plików, na przykład lub zapisać je do magazynu obiektów blob platformy Azure. Niektóre opcje i linki do innych zasobów, aby uzyskać więcej informacji, zobacz w [Rozwiązywanie problemów z witryn sieci Web platformy Azure w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Dla tego samouczka będziesz tylko przyjrzyj dzienniki w programie Visual Studio **dane wyjściowe** okna.

    W aplikacji produkcyjnej warto wziąć pod uwagę śledzenie pakietów innych niż System.Diagnostics i interfejs ILogger ułatwia stosunkowo przełączenie się do śledzenia inny mechanizm w przypadku podjęcia decyzji o tym.

### <a name="create-interceptor-classes"></a>Tworzenie klasy interceptora

Następnie utworzysz klasy, które zostanie wywołany programu Entity Framework za każdym razem, gdy ma wysyłać kwerendy do bazy danych, aby symulować błędów przejściowych i jeden do rejestrowania. Te klasy interceptora musi pochodzić od `DbCommandInterceptor` klasy. W tych pisania zamienników metod, które są nazywane automatycznie, gdy zapytanie ma zostać wykonana. W tych metod można zbadać lub dziennika zapytań, który jest wysyłany do bazy danych i można zmienić zapytania przed wysłaniem ich do bazy danych lub powrócić coś do narzędzia Entity Framework samodzielnie bez nawet przekazywanie zapytania do bazy danych.

1. Aby utworzyć klasę interceptora, która będzie rejestrować co zapytanie SQL, które są wysyłane do bazy danych, należy utworzyć plik klasy o nazwie *SchoolInterceptorLogging.cs* w *DAL* folder i Zastąp szablon kodu z następujący kod:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Pomyślne zapytania lub polecenia ten kod zapisuje informacje dziennika z informacjami o opóźnienia. Wyjątki tworzy dziennik błędów.
2. Aby utworzyć klasę interceptora generowany fikcyjny błędów przejściowych po wprowadzeniu "Throw" w **wyszukiwania** Utwórz plik klasy o nazwie *SchoolInterceptorTransientErrors.cs* w *DAL* folderu i Zastąp kod szablonu z następującym kodem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Ten kod tylko przesłonięcia `ReaderExecuting` metodę, która jest wywoływana dla zapytań zwracających wiele wierszy danych. Jeśli chcesz sprawdzić odporność połączenia dla innych typów kwerend, można także zastępować `NonQueryExecuting` i `ScalarExecuting` metod jako interceptora rejestrowania jest.

    Po uruchomieniu strony dla użytkowników domowych i wprowadź "Throw" jako ciąg wyszukiwania, ten kod tworzy fikcyjny wyjątek bazy danych SQL dla numer błędu 20, typem wiadomo, że są zwykle charakter przejściowy. Inne liczby błędów rozpoznanych jako przejściowy są 64, 233 10053, 10054, 10060, 10928, 10929, 40197, 40501 i 40613, ale te mogą ulec zmianie w nowej wersji bazy danych SQL.

    Kod zwraca wyjątek do narzędzia Entity Framework zamiast uruchomienie zapytania i przekazywanie wyników zapytania Wstecz. Cztery razy zwracany jest wyjątek przejściowy, a następnie kod wraca do normalnej procedury przekazywania zapytania do bazy danych.

    Ponieważ wszystko, co jest rejestrowane, będzie można zobaczyć, że próbuje wykonać cztery razy zapytania przed finally pomyślne Entity Framework, a jedyną różnicą w aplikacji jest, że trwa dłużej do renderowania strony z wynikami zapytania.

    Ile razy ponowi programu Entity Framework jest konfigurowany; Kod określa cztery razy, ponieważ jest to wartość domyślna dla zasad wykonywania programu bazy danych SQL. W przypadku zmiany zasad wykonywania, trzeba było również zmiany tutaj kod, który określa, ile razy są generowane w przypadku błędów przejściowych. Można również zmienić kod można wygenerować więcej wyjątki, aby zgłosi Entity Framework `RetryLimitExceededException` wyjątku.

    Wartość, należy wprowadzić w polu wyszukiwania będzie w `command.Parameters[0]` i `command.Parameters[1]` (jeden służy do imienia i jeden dla nazwy ostatniego). Gdy ma wartość "% Throw %", "Throw" zastępuje w tych parametrów "", aby niektóre studentów zostanie znaleziony i zwrócony.

    Jest to po prostu wygodny sposób do testowania połączenia odporności oparte na zmianę niektórych danych wejściowych do interfejsu użytkownika aplikacji. Można również napisać kod, który generuje błędów przejściowych dla wszystkich zapytań lub aktualizacji, jak wyjaśniono dalej w komentarze dotyczące *DbInterception.Add* metody.
3. W *Global.asax*, Dodaj następujący `using` instrukcji:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Dodaj wyróżnione wiersze do `Application_Start` metody:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Te wiersze są, co powoduje interceptora kodu do uruchomienia, gdy Entity Framework wysyła zapytania do bazy danych. Należy zauważyć, że ponieważ utworzyć oddzielne interceptora klasy do symulacji błędu przejściowego i rejestrowania, można niezależnie włączyć i wyłączyć je.

    Można dodać przy użyciu interceptory `DbInterception.Add` metody w kodzie; nie ma w `Application_Start` — metoda. Inną możliwością jest wykonanie umieść ten kod w klasie DbConfiguration utworzonego wcześniej do konfigurowania zasad wykonywania.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Wszędzie tam, gdzie możesz umieścić ten kod nie można wykonać `DbInterception.Add` dla tego samego interceptora więcej niż raz, lub uzyskać dodatkowe interceptora wystąpień. Na przykład jeśli dodasz interceptora rejestrowania dwa razy, zostanie wyświetlone dwa dzienniki dla każdego zapytania SQL.

    Interceptory są wykonywane w kolejności rejestracji (w kolejności `DbInterception.Add` metoda jest wywoływana). Kolejność może być niezależnie od tego, w zależności od wykonywanych w interceptor. Na przykład interceptora może zmienić polecenie SQL, które otrzymuje w `CommandText` właściwości. Jeśli go zmienić polecenie SQL, dalej interceptora zostanie zmienione polecenia SQL nie oryginalnego polecenia SQL.

    Kod symulacji błędu przejściowego napisanych w taki sposób, który umożliwia spowodować błędów przejściowych wprowadzając inną wartość w interfejsie użytkownika. Alternatywnie można napisać kod interceptora, aby zawsze Generuj sekwencji przejściowej wyjątków bez sprawdzania pod kątem wartość określonego parametru. Następnie można dodać interceptor tylko wtedy, gdy chcesz wygenerować błędów przejściowych. Jeśli to zrobisz, jednak nie należy dodawać interceptora dopiero po zakończeniu inicjowania bazy danych. Innymi słowy wykonaj co najmniej jedną bazę danych operacji, takiej jak zapytania na jednym z zestawów jednostek, przed rozpoczęciem generowania błędów przejściowych. Entity Framework wykonuje zapytania kilka podczas inicjowania bazy danych, a nie są wykonywane w ramach transakcji, dlatego błędy podczas inicjowania może spowodować, że kontekst do pobrania w niespójnym stanie.

## <a name="test-logging-and-connection-resiliency"></a>Test połączenia i rejestrowanie odporności

1. Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania, a następnie kliknij przycisk **studentów** kartę.
2. Szukaj w Visual Studio **dane wyjściowe** okno, aby wyświetlić dane wyjściowe śledzenia. Może być konieczne przewinięcie poza błędy JavaScript na uzyskanie dostępu do dzienników napisane przez użytkownika rejestratora.

    Zwróć uwagę, można wyświetlić rzeczywiste zapytania SQL wysyłane do bazy danych. Zostanie wyświetlony niektóre początkowej zapytań i poleceń, które Entity Framework jest aby rozpocząć, sprawdzanie wersji bazy danych i tabeli historii migracji (dowiesz się, migracje w następnym samouczku). Zobacz zapytanie dotyczące stronicowania, aby dowiedzieć się, jak wiele studentów, i na koniec Zobacz kwerendę, która pobiera dane dla użytkowników domowych.

    ![Rejestrowanie dla normalnej zapytania](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. W **studentów** strony, wprowadź "Throw" jako ciąg wyszukiwania, a następnie kliknij przycisk **wyszukiwania**.

    ![Throw ciąg wyszukiwania](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Można zauważyć przeglądarki jest prawdopodobnie zawieszenie przez kilka sekund, podczas Entity Framework ponawia zapytanie kilka razy. Pierwsza próba odbywa się bardzo szybko, następnie oczekiwania przed zwiększa przed ponowną próbą wykonania każdy dodatkowego. Ten proces już oczekiwania przed wywołaniem kolejnymi próbami *wykładniczego wycofywania*.

    Gdy zostanie wyświetlona strona, przedstawiający uczniów lub studentów, którzy mają "jako" w ich nazw Obejrzyj w oknie danych wyjściowych i zobaczysz, że tego samego zapytania podjęto pięć razy, najpierw cztery razy zwróceniem przejściowej wyjątków. Dla każdego błędu przejściowego zobaczysz dziennik zapisu podczas generowania Błąd przejściowy w `SchoolInterceptorTransientErrors` klasy ("zwrócenie przejściowy błąd polecenia..."), aby zobaczyć dziennika, gdy zapisywane `SchoolInterceptorLogging` pobiera wyjątek.

    ![Dane wyjściowe rejestrowania przedstawiający ponownych prób](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Ponieważ wprowadzony ciąg wyszukiwania jest sparametryzowane zapytania, która zwraca dane dla użytkowników domowych:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Nie jest rejestrowanie wartości parametrów, ale możesz to zrobić. Jeśli chcesz zobaczyć wartości parametru, można napisać kod rejestrowanie w celu uzyskania wartości parametrów z `Parameters` właściwość `DbCommand` obiektu, który można pobrać metody interceptora.

    Należy pamiętać, że nie Powtórz ten test, chyba że Zatrzymaj aplikację i uruchom go ponownie. Jeśli chcesz mieć możliwość testowania połączenia odporności wiele razy w jednym przebiegu aplikacji, można napisać kod, aby zresetować licznik błędów w `SchoolInterceptorTransientErrors`.
4. Różnice strategia wykonywania (zasady ponawiania) wystawia, komentarz `SetExecutionStrategy` wiersz w *SchoolConfiguration.cs*, a następnie ponownie uruchom strony studentów w trybie debugowania i ponowić wyszukiwanie "Throw".

    Teraz debuger zatrzymuje na pierwszy wygenerowany wyjątek od razu, podczas próby wykonania zapytania po raz pierwszy.

    ![Wyjątek zastępczego](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Usuń znaczniki komentarza *SetExecutionStrategy* wiersz w *SchoolConfiguration.cs*.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono Włączanie opcji elastyczności połączenia i dziennika poleceń SQL, które Entity Framework Redaguj i wysyła do bazy danych. W następnym samouczku przedstawiono wdrażania aplikacji z Internetem przy użyciu migracje Code First do wdrażania bazy danych.

Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć. Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Poprzednie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[dalej](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
