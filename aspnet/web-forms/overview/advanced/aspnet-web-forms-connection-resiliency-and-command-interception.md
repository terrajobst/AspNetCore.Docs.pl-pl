---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: Elastyczność połączenia formularzy sieci Web platformy ASP.NET i przejmowanie poleceń | Dokumentacja firmy Microsoft
author: Erikre
description: W tym samouczku opisano sposób modyfikowania przykładowej aplikacji do obsługi połączeń i przejmowanie poleceń.
ms.author: aspnetcontent
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 9a66b297608a5a8cd536b9af2a9ae4bb600a6bbb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807092"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Elastyczność połączenia formularzy sieci Web platformy ASP.NET i przejmowanie poleceń
====================
przez [Erik Reitan](https://github.com/Erikre)

W tym samouczku zmodyfikujesz przykładowej aplikacji Wingtip Toys w celu obsługi połączeń i przejmowanie poleceń. Po włączeniu elastyczność połączenia przykładowej aplikacji Wingtip Toys automatycznie ponowi próbę wywołania danych, po wystąpieniu błędów przejściowych, które są typowe dla środowiska chmury. Ponadto, wdrażając przejmowanie poleceń, przykładowej aplikacji Wingtip Toys będzie przechwytywać wszystkie zapytania SQL wysyłane do bazy danych, aby można było zalogować się lub je zmienić.

> [!NOTE] 
> 
> W tym samouczku formularzy sieci Web został oparty na Tom Dykstra następującego samouczka MVC:  
> [Połączeń i przejmowanie poleceń z platformą Entity Framework w aplikacji ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Zawartość:

- Jak zapewnić elastyczność połączenia.
- Jak zaimplementować przejmowanie poleceń.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz zainstalowane na komputerze następujące oprogramowanie:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie.
- Firmy Wingtip Toys przykładowe projektu, dzięki czemu można zaimplementować funkcje wymienione w tym samouczku w obrębie projektu o nazwie Wingtip Toys. Kliknięcie następującego łącza zawiera szczegóły pobierania:

    - [Wprowadzenie do ASP.NET 4.5.1 Web Forms — firmy Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Przed ukończenie tego samouczka, należy wziąć pod uwagę przeglądu powiązanych serii samouczków [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Seria samouczków ułatwi zapoznanie się z **WingtipToys** projektów i kodu.

## <a name="connection-resiliency"></a>Elastyczność połączenia

Gdy należy wziąć pod uwagę wdrażanie aplikacji na platformie Windows Azure jedną opcją do rozważenia jest wdrożenie bazy danych do **Windows** **usługi Azure SQL Database**, to usługa bazy danych w chmurze. Błędy przejściowe połączenia są zazwyczaj częściej, po nawiązaniu połączenia z usługą bazy danych w chmurze niż gdy serwer sieci web i serwera bazy danych są bezpośrednio połączonych ze sobą w tym samym centrum danych. Nawet wtedy, gdy serwer sieci web w chmurze i usługi w chmurze bazy danych znajdują się w tym samym centrum danych, istnieje więcej połączeń sieciowych między nimi, które mogą mieć problemy, takie jak moduły równoważenia obciążenia.

Również usługi w chmurze jest zwykle udostępnione przez innych użytkowników, co oznacza, że jego czas reakcji mogą mieć wpływ je. A może podlegać ograniczania dostępu do bazy danych. Ograniczanie oznacza, że usługa bazy danych zgłasza wyjątek wyjątków podczas próby dostępu do niego częściej niż jest dozwolony w Twojej *Umowa dotycząca poziomu usług* (SLA).

Wiele lub większości problemy z połączeniem, które występują, gdy uzyskujesz dostęp do usługi w chmurze są przejściowe, czyli rozpoznają się w krótkim czasie. Dlatego kiedy sprawdzasz operacji bazy danych i uzyskać typ błędu, który jest zwykle charakter przejściowy, możesz spróbować ponownie wykonać operację po krótkim czasie oczekiwania, a operacja może zakończyć się pomyślnie. Zapewnienie znacznie lepsze środowisko dla użytkowników w przypadku obsługi błędów przejściowych, automatycznie podjęcie ponownej próby wykonania, ukrywanie większość z nich do klienta. Tej funkcji odporności połączenia w programie Entity Framework 6 automatyzuje proces podejmowania próby ponownego wykonywania zapytań SQL się niepowodzeniem.

Funkcji elastyczności połączenia musi być skonfigurowany odpowiednio dla konkretnej bazy danych usługi:

1. Musi wiedzieć, które wyjątki są może być błędem przejściowym. Chcesz ponowić próbę błędy spowodowane do tymczasowej utraty w łączności sieciowej nie błędy spowodowane przez błędy programu, na przykład.
2. Ma czekać odpowiednią ilość czasu między kolejnymi próbami operacji nie powiodło się. Możesz poczekać już między kolejnymi próbami dla przetwarzania wsadowego nie można uzyskać online strony sieci web, w którym użytkownik jest oczekiwanie na odpowiedź.
3. Posiada ponowić próbę odpowiednią liczbę razy, zanim zrezygnuje. Być może chcesz ponowić próbę więcej razy w procesie przetwarzania wsadowego, który będzie w trybie online aplikacji.

Można skonfigurować te ustawienia ręcznie dla dowolnego środowiska bazy danych obsługiwane przez dostawcę programu Entity Framework.

Należy wykonać, aby umożliwić elastyczność połączenia wystarczy utworzyć klasę w swoim zestawie, która pochodzi od klasy `DbConfiguration` klasy, a w tej klasie Ustawianie strategii wykonywania bazy danych SQL, czyli platformy Entity Framework inny termin dla zasad ponawiania prób.

### <a name="implementing-connection-resiliency"></a>Implementowanie elastyczność połączenia

1. Pobierz i Otwórz [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) Przykładowa aplikacja formularzy sieci Web w programie Visual Studio.
2. W *logiki* folderu **WingtipToys** aplikacji, Dodaj plik klasy o nazwie *WingtipToysConfiguration.cs*.
3. Zastąp istniejący kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework automatycznie uruchamia kod znajdzie się w klasie, która pochodzi od klasy `DbConfiguration`. Możesz użyć `DbConfiguration` klasie w celu wykonania zadania konfiguracji w kodzie, które w przeciwnym razie należy *Web.config* pliku. Aby uzyskać więcej informacji, zobacz [EntityFramework konfiguracja na podstawie kodu](https://msdn.microsoft.com/data/jj680699).

1. W *logiki* folder, otwórz *AddProducts.cs* pliku.
2. Dodaj `using` poufności informacji dotyczące `System.Data.Entity.Infrastructure` pokazany wyróżnione na żółto:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Dodaj `catch` za pomocą bloku `AddProduct` metody, aby `RetryLimitExceededException` jest rejestrowany jako wyróżniane na żółty:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Dodając `RetryLimitExceededException` wyjątku, można zapewnić lepsze rejestrowanie lub wyświetlić komunikat o błędzie do użytkownika, w którym można wybrać ponowienie próby procesu. Przez Przechwytywanie `RetryLimitExceededException` wyjątek, tylko błędy, które mogą być przejściowe będzie już mieć próbowała i nie powiodło się kilka razy. Faktyczny wyjątek, zwrócone zostaną opakowane w `RetryLimitExceededException` wyjątku. Ponadto dodano także ogólnego bloku catch. Aby uzyskać więcej informacji na temat `RetryLimitExceededException` wyjątków, zobacz [elastyczność połączenia programu Entity Framework / logika ponowień](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Przejmowanie poleceń

Teraz, gdy została włączona dla zasad ponawiania, jak możesz przetestować Aby sprawdzić, czy działa ona zgodnie z oczekiwaniami? Nie jest tak proste wymusić Błąd przejściowy do wykonania, szczególnie gdy korzystasz lokalnie i może być szczególnie trudne do integracji rzeczywiste błędy przejściowe testów automatycznych jednostkowych. Aby przetestować połączenie funkcji odporności, potrzebujesz sposobu przechwycenia zapytań, które platformy Entity Framework wysyła do serwera SQL i Zamień na typ wyjątku, który jest zwykle charakter przejściowy odpowiedzi programu SQL Server.

Przejmowanie zapytania można również użyć w celu wdrożenia najlepszym rozwiązaniem dla aplikacji w chmurze: dziennik opóźnienia i powodzenie lub niepowodzenie wszystkie wywołania do zewnętrznych usługach, takich jak usługi bazy danych.

W tej części samouczka użyjesz programu Entity Framework [ *funkcji przejmowanie* ](https://msdn.microsoft.com/data/dn469464) do rejestrowania i symulowania błędów przejściowych.

### <a name="create-a-logging-interface-and-class"></a>Tworzenie interfejsu rejestrowania i klasy

Najlepszym rozwiązaniem dla rejestrowania jest zrobienie tego przy użyciu [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) zamiast kodować wywołania `System.Diagnostics.Trace` lub klasy rejestrowania. Ułatwia zmienić swoje mechanizmie rejestrowania później, jeśli kiedykolwiek trzeba to zrobić. Dlatego w tej sekcji utworzysz interfejs rejestrowania i klasa nad jego implementacją.

Oparte na powyższą procedurę, został pobrany i otwarty **WingtipToys** przykładowej aplikacji w programie Visual Studio.

1. Utwórz folder w **WingtipToys** projektu i nadaj mu nazwę *rejestrowania*.
2. W *rejestrowania* folderze utwórz plik klasy o nazwie *ILogger.cs* i zastąpić domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   Interfejs zawiera trzy poziomy śledzenia, aby wskazać względne znaczenie dzienniki i jeden zapewniają informacje opóźnienie do wywołań usług zewnętrznych, takich jak zapytania do bazy danych. Metody rejestrowania mają przeciążenia, które umożliwiają przekazywanie wyjątek. To dlatego, że informacje o wyjątku, w tym wyjątki wewnętrzne i ślad stosu jest niezawodny sposób będą rejestrowane przez klasę, która implementuje interfejs, zamiast polegać na tym wykonywana w każdym wywołaniu metody rejestrowania w całej aplikacji.  
  
   `TraceApi` Metody pozwalają śledzić czas oczekiwania na każde wywołanie usługi zewnętrznej, takich jak SQL Database.
3. W *rejestrowania* folderze utwórz plik klasy o nazwie *Logger.cs* i zastąpić domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

W implementacji użyto `System.Diagnostics` celu śledzenie. Jest to wbudowana funkcja .NET, która ułatwia generowanie i używanie informacji śledzenia. Istnieje wiele &quot;odbiorników&quot; za pomocą `System.Diagnostics` śledzenia, do przechowywania plików, na przykład tylko zapisywanie dzienników lub zapisać je do magazynu obiektów blob na platformie Windows Azure. Niektóre opcje i linki do innych zasobów, aby uzyskać więcej informacji, zobacz w [Rozwiązywanie problemów z Windows Azure Web Sites w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Na potrzeby tego samouczka możesz tylko przyjrzymy się dzienniki programu Visual Studio **dane wyjściowe** okna.

W aplikacji produkcyjnej warto wziąć pod uwagę przy użyciu śledzenia platform innych niż `System.Diagnostics`i `ILogger` interfejs sprawia, że stosunkowo łatwo zmienić mechanizm śledzenia inny, jeśli zdecydujesz się to zrobić.

### <a name="create-interceptor-classes"></a>Tworzenie klas interceptor

Następnie utworzysz klas, które platformy Entity Framework będzie wywoływać za każdym razem, gdy będzie znajdował się wysłanie zapytania do bazy danych — jedną symulowanie błędów przejściowych, a jeden w celu rejestrowania. Te klasy interceptor muszą pochodzić od `DbCommandInterceptor` klasy. W nich można napisać zastąpienia metody, które automatycznie są wywoływane, gdy zapytanie ma zostać wykonana. W tych metodach można zbadać lub zaloguj się zapytanie, które są wysyłane do bazy danych i można zmienić kwerendę, przed wysłaniem ich do bazy danych lub zwrócić coś w programie Entity Framework samodzielnie bez nawet przekazywania zapytania do bazy danych.

1. Aby utworzyć klasę interceptor, która zarejestruje każdego zapytania SQL przed wysłaniem ich do bazy danych, Utwórz plik klasy o nazwie *InterceptorLogging.cs* w *logiki* folder i Zastąp domyślny kod Poniższy kod:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Pomyślne zapytania lub polecenia ten kod zapisuje w dzienniku informacji z informacjami o opóźnieniu. Wyjątki tworzy dziennik błędów.
2. Aby utworzyć klasę interceptor, który zostanie wygenerowany fikcyjnego błędów przejściowych, po wprowadzeniu &quot;Throw&quot; w **nazwa** pole tekstowe na stronie o nazwie *AdminPage.aspx*, Utwórz klasę plik o nazwie *InterceptorTransientErrors.cs* w *logiki* folder i Zastąp domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Ten kod tylko zastąpienia `ReaderExecuting` metody, która jest wywoływana dla zapytań zwracających wiele wierszy danych. Chcąc sprawdzić elastyczność połączenia dla innych typów zapytań, można także Przesłoń `NonQueryExecuting` i `ScalarExecuting` metod jako interceptor rejestrowania wykonuje.  
  
   Później będzie zalogować się jako "Admin" i wybierz **administratora** łącze na górnym pasku nawigacyjnym. Następnie na *AdminPage.aspx* strony spowoduje dodanie produkt o nazwie &quot;Throw&quot;. Ten kod tworzy fikcyjnego wyjątek bazy danych SQL dla numer błędu 20, typem wiadomo, że są zwykle charakter przejściowy. Inne liczby błędów, obecnie jest rozpoznawany jako przejściowe są 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 i 40613, ale te mogą ulec zmianie w nowych wersji bazy danych SQL. Produkt zostanie zmieniona na "TransientErrorExample", który może wystąpić w kodzie *InterceptorTransientErrors.cs* pliku.  
  
   Kod zwraca wyjątek w programie Entity Framework zamiast uruchamiania zapytania i przekazywanie wyników wstecz. Zwracany jest wyjątek przejściowy *cztery* razy, a następnie kod wraca do normalnej procedury przekazywania zapytania do bazy danych.

    Ponieważ wszystko jest rejestrowane, będzie można zobaczyć, że platformy Entity Framework próbuje wykonać zapytanie cztery razy przed na koniec pomyślne wykonanie, a jedyną różnicą w aplikacji, zajmuje więcej czasu renderowania strony z wynikami zapytania.  
  
   Liczba przypadków, gdy spróbuje ponowić operację platformy Entity Framework jest konfigurowany; Kod ten określa cztery razy, ponieważ jest to wartość domyślna dla zasad wykonywania programu bazy danych SQL. W przypadku zmiany zasad wykonywania programu, trzeba było również zmiany tutaj kod, który określa, ile razy są generowane błędy przejściowe. Można również zmienić kod można wygenerować więcej wyjątków, aby zgłosi Entity Framework `RetryLimitExceededException` wyjątku.
3. W *Global.asax*, Dodaj następujące instrukcje using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Następnie dodaj wyróżnione wiersze w celu `Application_Start` metody:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Następujące wiersze kodu są na tym, co powoduje, że kod interceptor do uruchomienia programu Entity Framework wysyła zapytania do bazy danych. Należy zauważyć, że ponieważ utworzono oddzielne interceptor klas na potrzeby symulacji błędu przejściowego i rejestrowania, można niezależnie włączać i wyłączać je.   
  
 Możesz dodać za pomocą interceptory `DbInterception.Add` metody dowolnym miejscu w kodzie; nie musi znajdować się w `Application_Start` metody. Innym rozwiązaniem, jeśli nie dodasz interceptory w `Application_Start` metody, byłoby zaktualizuj lub Dodaj klasę o nazwie *WingtipToysConfiguration.cs* i umieścić powyższy kod na końcu konstruktora `WingtipToysbConfiguration` klasy.

Wszędzie tam, gdzie należy umieścić ten kod nie należy do wykonania `DbInterception.Add` dla tego samego interceptor więcej niż jeden raz lub możesz uzyskać interceptor dodatkowych wystąpień. Na przykład jeśli dodasz dwa razy interceptor rejestrowanie, zobaczysz dwa dzienniki dla każdego zapytania SQL.

Interceptory są wykonywane zgodnie z kolejnością rejestracji (kolejność, w której `DbInterception.Add` wywoływana jest metoda). Kolejność może znaczenia, w zależności od tego, co robisz w interceptor. Na przykład interceptor mogą ulec zmianie polecenia SQL, które otrzymuje w `CommandText` właściwości. Jeśli zmienia się polecenia SQL, dalej interceptor zostanie zmienione polecenia SQL nie oryginalne polecenie SQL.

Kodzie symulacji błędu przejściowego napisanych w taki sposób, że można spowodować błędy przejściowe, wprowadzając inną wartość w interfejsie użytkownika. Alternatywnie można napisać kod interceptor, aby zawsze Generuj sekwencji przejściowych wyjątków bez sprawdzania pod kątem wartości określonego parametru. Następnie można dodać interceptor tylko wtedy, gdy chcesz wygenerować błędów przejściowych. Jeśli to zrobisz, jednak nie dodać interceptor do momentu po ukończeniu inicjowania bazy danych. Innymi słowy należy wykonać co najmniej jedna baza danych operacji, takich jak zapytania na jednym z zestawów encji, przed rozpoczęciem generowania błędów przejściowych. Entity Framework wykonuje kilka zapytań podczas inicjowania bazy danych, a nie są one wykonywane w ramach transakcji, więc błędy podczas inicjowania może spowodować, że kontekst do pobrania w niespójnym stanie.

## <a name="test-logging-and-connection-resiliency"></a>Test połączenia i rejestrowania odporności

1. W programie Visual Studio, naciśnij klawisz **F5** do uruchamiania aplikacji w trybie debugowania, a następnie zaloguj się jako "Admin" przy użyciu "Wyrazu Pa$ $" jako hasło.
2. Wybierz **administratora** na pasku nawigacyjnym u góry.
3. Wprowadź nowy produkt o nazwie "Throw" za pomocą odpowiedniego pliku opis i cena obrazu.
4. Naciśnij klawisz **Dodaj produkt** przycisku.  
   Zauważysz, przeglądarka prawdopodobnie zawieszanie przez kilka sekund, natomiast Entity Framework ponawia próbę zapytanie kilka razy. Pierwszym ponowieniem próby przebiega bardzo szybko, a następnie zwiększa się czas oczekiwania przed każdym dodatkowe ponowieniem próby. Ten proces jest już oczekiwania nosi nazwę każdego ponawiania *wykładniczego wycofywania* .
5. Poczekaj, aż strona nie jest już atttempting do załadowania.
6. Zatrzymaj projekt i przyjrzyj się programu Visual Studio **dane wyjściowe** okno, aby wyświetlić dane wyjściowe śledzenia. Możesz znaleźć **dane wyjściowe** okna, wybierając **debugowania**  - &gt; **Windows**  - &gt;  **Dane wyjściowe**. Może być konieczne przewiń ekran za kilka innych dzienników napisany przez użytkownika rejestratora.  
  
   Zwróć uwagę, możliwość wyświetlenia rzeczywiste zapytania SQL wysyłane do bazy danych. Zobaczysz niektóre początkowej zapytań i poleceń, które obsługuje platformy Entity Framework, aby rozpocząć pracę, sprawdzania tabeli historii wersji i migracji bazy danych.   
    ![Okno Dane wyjściowe](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Należy pamiętać, że nie Powtórz ten test, chyba że Zatrzymaj aplikację i uruchom go ponownie. Jeśli chcesz testować elastyczność połączenia wiele razy w jednym przebiegu aplikacji, można napisać kod, aby zresetować licznik błędów w `InterceptorTransientErrors` .
7. Aby zobaczyć różnicę strategii wykonywania (zasady ponawiania) sprawia, że, komentarz `SetExecutionStrategy` linię *WingtipToysConfiguration.cs* w pliku *logiki* folder, uruchom **administratora**  strony w trybie debugowania, a następnie Dodaj produkt o nazwie &quot;Throw&quot; ponownie.  
  
   Tym razem debuger zatrzymuje się na pierwszy wyjątek wygenerowane natychmiast, gdy próbuje wykonać zapytanie po raz pierwszy.  
    ![Debugowanie — szczegóły](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Usuń znaczniki komentarza `SetExecutionStrategy` linię *WingtipToysConfiguration.cs* pliku.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób modyfikowania przykładowej aplikacji formularzy sieci Web do obsługi połączeń i przejmowanie poleceń.

## <a name="next-steps"></a>Następne kroki

Po przejrzeniu połączeń i przejmowanie poleceń w formularzach sieci Web platformy ASP.NET, przejrzyj temat wzorca ASP.NET Web Forms [metod asynchronicznych w programie ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). Temat nauczyć podstawy tworzenia asynchronicznych aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio.
