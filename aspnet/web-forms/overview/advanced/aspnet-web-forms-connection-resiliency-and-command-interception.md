---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: Elastyczność połączenia formularzy sieci Web ASP.NET i przechwytywaniu polecenie | Dokumentacja firmy Microsoft
author: Erikre
description: Ten przewodnik opisuje sposób modyfikowania przykładowej aplikacji do obsługi opcji elastyczności połączenia i polecenia zatrzymania.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: d5c4e46209e1b21a303fdf1fb16c6c868b3ca923
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Elastyczność połączenia formularzy sieci Web ASP.NET i polecenia zatrzymania
====================
Przez [Erik Reitan](https://github.com/Erikre)

W tym samouczku należy zmodyfikować Wingtip Toys przykładowej aplikacji do obsługi opcji elastyczności połączenia i polecenia zatrzymania. Przez włączenie opcji elastyczności połączenia, do aplikacji przykładowej Wingtip Toys automatycznie ponowi wywołania danych, po wystąpieniu błędu przejściowego typowe dla środowiska chmury. Ponadto zaimplementowanie polecenie zatrzymania Wingtip Toys Przykładowa aplikacja będzie przechwytywać wszystkie zapytania SQL wysyłane do bazy danych w celu logowania się lub je zmienić.

> [!NOTE] 
> 
> W tym samouczku formularzy sieci Web została oparta na Dykstra Tomasz samouczek dla platformy MVC następujące:  
> [Elastyczność połączenia i przechwytywaniu polecenia Entity Framework w aplikacji platformy ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Zawartość:

- Jak zapewnić odporność połączenia.
- Jak zaimplementować polecenia zatrzymania.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz następujące oprogramowanie zainstalowane na komputerze:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie.
- Wingtip Toys przykładowe projektu, dzięki czemu można zaimplementować funkcje wymienione w tym samouczku w projekcie Wingtip Toys. Poniższe łącze szczegółowe pobierania:

    - [Wprowadzenie do platformy ASP.NET 4.5.1 sieci Web Forms - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Przed wykonanie kroków tego samouczka, należy wziąć pod uwagę przeglądu powiązanych serii samouczek, [wprowadzenie do formularzy sieci Web programu ASP.NET 4.5 i programu Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Samouczek serii ułatwiają zapoznanie się z **WingtipToys** projektu i kodu.

## <a name="connection-resiliency"></a>Elastyczność połączenia

Gdy należy wziąć pod uwagę wdrażania aplikacji w systemie Windows Azure co należy rozważyć możliwość wdrożenia bazy danych do **Windows** **bazy danych SQL Azure**, usługa bazy danych w chmurze. Błędów przejściowych połączenia są zwykle częstsze podczas łączenia z usługą w chmurze bazy danych niż podczas serwera sieci web i serwer bazy danych są bezpośrednio połączone w tym samym centrum danych. Nawet jeśli serwer sieci web w chmurze i usługi w chmurze bazy danych znajdują się w tym samym centrum danych, istnieje więcej połączeń sieciowych między nimi, które mogą wystąpić problemy, takie jak moduły równoważenia obciążenia.

Również usługa w chmurze jest zwykle udostępnione przez innych użytkowników, co oznacza, że jego czasu odpowiedzi mogą mieć wpływ je. I dostęp do bazy danych mogą być narażone na ograniczania. Ograniczanie oznacza, że usługa bazy danych zgłasza wyjątek wyjątków podczas próby dostępu do niego częściej niż jest dozwolone w Twojej *umową dotyczącą poziomu usług* (SLA).

Wiele lub większości problemów połączenia, które wystąpić, gdy uzyskujesz dostęp do usługi w chmurze są przejściowe, czyli rozpoznają się w krótkim czasie. Dlatego podczas próby operacji bazy danych pobrać typu błędu, który jest zwykle charakter przejściowy, możesz można spróbuj ponownie wykonać operację po krótkim oczekiwania, a operacja może zakończyć się pomyślnie. Zapewnienie dużo lepsze środowisko dla użytkowników w przypadku obsługi błędów przejściowych podejmując automatycznie ponownie, ukrywanie większość z nich do klienta. Funkcji odporności połączenia w programie Entity Framework 6 automatyzuje proces ponawianie próby niepowodzeniu zapytania SQL.

Funkcji odporności połączenia muszą być skonfigurowane odpowiednio dla usługi określona baza danych:

1. Musi wiedzieć, które wyjątki są może być przejściowy. Chcesz ponowić próbę błędy spowodowane do tymczasowej utraty w łączności sieciowej, nie błędy spowodowane przez błędy programu, na przykład.
2. Musi ona oczekiwać odpowiednią ilość czasu między kolejnymi próbami operacji nie powiodło się. Można poczekać już między kolejnymi próbami przetwarzania wsadowego niż jest to możliwe online strony sieci web, w którym użytkownik jest oczekiwania na odpowiedź.
3. Posiada ponowić próbę odpowiednią liczbę razy, zanim zrezygnuje. Możesz ponowić próbę więcej razy w przetwarzania wsadowego, które są w trybie online aplikacji.

Można skonfigurować te ustawienia ręcznie w każdym środowisku bazy danych, obsługiwane przez dostawcy programu Entity Framework.

Trzeba wykonać, aby włączyć elastyczności połączenia jest utworzyć klasę w używanego zestawu, która pochodzi z `DbConfiguration` klasy, a w tej klasie określania strategii wykonywania bazy danych SQL, które Entity Framework jest inna nazwa zasady ponawiania.

### <a name="implementing-connection-resiliency"></a>Implementowanie elastyczność połączenia

1. Pobierz i Otwórz [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) Przykładowa aplikacja formularzy sieci Web w programie Visual Studio.
2. W *logiki* folderu **WingtipToys** aplikacji, Dodaj plik klasy o nazwie *WingtipToysConfiguration.cs*.
3. Zastąp istniejący kod następujący kod:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework automatycznie uruchamia kod znajdzie się w klasie, która jest pochodną `DbConfiguration`. Można użyć `DbConfiguration` klasy do wykonywania zadań konfiguracji w kodzie, które w przeciwnym razie należy *Web.config* pliku. Aby uzyskać więcej informacji, zobacz [EntityFramework konfiguracji opartej na kodzie](https://msdn.microsoft.com/data/jj680699).

1. W *logiki* folder, otwórz *AddProducts.cs* pliku.
2. Dodaj `using` instrukcji dla `System.Data.Entity.Infrastructure` pokazany wyróżnionych kolorem żółtym:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Dodaj `catch` za pomocą bloku `AddProduct` metody, aby `RetryLimitExceededException` jest rejestrowany jako wyróżniane na żółty:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Dodając `RetryLimitExceededException` wyjątku, można zapewnić lepsze rejestrowanie lub wyświetlić komunikat o błędzie dla użytkownika, w którym można wybrać, aby spróbować ponownie uruchomić proces. Przez `RetryLimitExceededException` wyjątek, tylko błędy, które mogą być przejściowy będzie już zostały nastąpiła i nie można wielokrotnie. Faktyczny wyjątek, zwrócone zostaną opakowane w `RetryLimitExceededException` wyjątku. Ponadto również dodać bloku catch ogólne. Aby uzyskać więcej informacji na temat `RetryLimitExceededException` wyjątek, zobacz [Entity Framework połączenia odporności / ponów logiki](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Polecenie zatrzymania

Po włączeniu zasady ponawiania jak przetestowanie można zweryfikować, że działa zgodnie z oczekiwaniami? Nie jest tak proste wymusić Błąd przejściowy się stać, szczególnie gdy używasz lokalnie i być szczególnie trudne do integracji rzeczywiste błędów przejściowych testu jednostkowego automatyczne. Aby przetestować funkcje ochrony połączenia, należy przechwycić zapytań, które Entity Framework wysyła do serwera SQL i Zamień na typ wyjątku, który jest zwykle charakter przejściowy odpowiedź serwera SQL.

Przechwycenie zapytania można również użyć w celu wdrożenia najlepszym rozwiązaniem dla aplikacji w chmurze: dziennika opóźnienia i powodzenie lub niepowodzenie wszystkie wywołania do zewnętrznych usług takich jak usługi bazy danych.

W tej części samouczka użyjesz programu Entity Framework [ *funkcji zatrzymania* ](https://msdn.microsoft.com/data/dn469464) zarówno do rejestrowania i do symulowania błędów przejściowych.

### <a name="create-a-logging-interface-and-class"></a>Utwórz interfejs rejestrowania i klasy

Najlepszym rozwiązaniem dla rejestrowania jest to zrobić za pomocą [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) zamiast kodować wywołania `System.Diagnostics.Trace` lub klasy rejestrowania. Który ułatwia zmień mechanizm z rejestrowania w później, jeśli trzeba to zrobić. Dlatego w tej sekcji utworzysz interfejs rejestrowania i klasy do zaimplementowania go.

Oparte na powyższą procedurę, zostały pobrane i otworzyć **WingtipToys** przykładowej aplikacji w programie Visual Studio.

1. Utwórz folder w **WingtipToys** projektu i nadaj mu nazwę *rejestrowanie*.
2. W *rejestrowanie* folderu, Utwórz plik klasy o nazwie *ILogger.cs* i zastąpić domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   Interfejs zawiera trzy poziomy śledzenia, aby wskazać względną wagę dzienników i jedną zapewniają informacje opóźnienia w wywołaniach zewnętrznych usług takich jak kwerend bazy danych. Metody rejestrowania mają przeciążeń, które umożliwiają przekazywanie wyjątku. Jest tak, aby informacje o wyjątku, w tym wyjątki wewnętrzne i śledzenia stosu niezawodnie jest rejestrowane przez klasę, która implementuje interfejs, zdejmując to zadanie, na którym wykonywana w każdym wywołaniu metody rejestrowania w całej aplikacji.  
  
   `TraceApi` Metody umożliwiają śledzenie opóźnień każde wywołanie zewnętrznej usługi takie jak bazy danych SQL.
3. W *rejestrowanie* folderu, Utwórz plik klasy o nazwie *Logger.cs* i zastąpić domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Implementacja używa `System.Diagnostics` celu śledzenie. Jest to funkcja wbudowana platformy .NET, co ułatwia generowanie i używanie informacji śledzenia. Istnieje wiele &quot;odbiorników&quot; można używać z `System.Diagnostics` śledzenia, zapisywanie dzienników do plików, na przykład lub zapisać je do magazynu obiektów blob w systemie Windows Azure. Niektóre opcje i linki do innych zasobów, aby uzyskać więcej informacji, zobacz w [Rozwiązywanie problemów z systemu Windows Azure Web Sites w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). W tym samouczku będziesz tylko przyjrzymy się dzienniki w programie Visual Studio **dane wyjściowe** okna.

W aplikacji produkcyjnej warto wziąć pod uwagę przy użyciu struktury śledzenia innych niż `System.Diagnostics`i `ILogger` interfejs sprawia, że stosunkowo łatwa do przełączenie się do śledzenia inny mechanizm w przypadku podjęcia decyzji o tym.

### <a name="create-interceptor-classes"></a>Tworzenie klasy interceptora

Następnie utworzysz klasy, które zostanie wywołany programu Entity Framework za każdym razem, gdy ma wysyłać kwerendy do bazy danych, aby symulować błędów przejściowych i jeden do rejestrowania. Te klasy interceptora musi pochodzić od `DbCommandInterceptor` klasy. W tych obiektach należy napisać zamienników metod, które są nazywane automatycznie, gdy zapytanie ma zostać wykonana. W tych metod można zbadać lub dziennika zapytań, który jest wysyłany do bazy danych i można zmienić zapytania przed wysłaniem ich do bazy danych lub powrócić coś do narzędzia Entity Framework samodzielnie bez nawet przekazywanie zapytania do bazy danych.

1. Aby utworzyć klasę interceptora, która będzie rejestrować każdej kwerendy SQL przed wysłaniem ich do bazy danych, należy utworzyć plik klasy o nazwie *InterceptorLogging.cs* w *logiki* folderu i Zastąp domyślne kodu z następujący kod:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Pomyślne zapytania lub polecenia ten kod zapisuje informacje dziennika z informacjami o opóźnienia. Wyjątki tworzy dziennik błędów.
2. Aby utworzyć klasę interceptora generowany fikcyjny błędów przejściowych po wprowadzeniu &quot;Throw&quot; w **nazwa** pole tekstowe na stronie o nazwie *AdminPage.aspx*, Utwórz klasę plik o nazwie *InterceptorTransientErrors.cs* w *logiki* folderu i Zamień domyślny kod następującym kodem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Ten kod tylko przesłonięcia `ReaderExecuting` metodę, która jest wywoływana dla zapytań zwracających wiele wierszy danych. Jeśli chcesz sprawdzić odporność połączenia dla innych typów kwerend, można także zastępować `NonQueryExecuting` i `ScalarExecuting` metod jako interceptora rejestrowania jest.  
  
   Później będzie zalogować się jako "Admin" i wybierz **Admin** łącze na górnym pasku nawigacyjnym. Następnie na *AdminPage.aspx* strony spowoduje dodanie produkt o nazwie &quot;Throw&quot;. Kod tworzy fikcyjny wyjątek bazy danych SQL dla numer błędu 20, typem wiadomo, że są zwykle charakter przejściowy. Inne liczby błędów rozpoznanych jako przejściowy są 64, 233 10053, 10054, 10060, 10928, 10929, 40197, 40501 i 40613, ale te mogą ulec zmianie w nowej wersji bazy danych SQL. Produkt zostanie zmieniona na "TransientErrorExample", który może wystąpić w kodzie *InterceptorTransientErrors.cs* pliku.  
  
   Kod zwraca wyjątek do narzędzia Entity Framework zamiast uruchomienie zapytania i przekazywanie wyników wstecz. Zwracany jest wyjątek przejściowa *cztery* razy, a następnie kod wraca do normalnej procedury przekazywania zapytania do bazy danych.

    Ponieważ wszystko, co jest rejestrowane, będzie można zobaczyć, że próbuje wykonać cztery razy zapytania przed finally pomyślne Entity Framework, a jedyną różnicą w aplikacji jest, że trwa dłużej do renderowania strony z wynikami zapytania.  
  
   Ile razy ponowi programu Entity Framework jest konfigurowany; Kod określa cztery razy, ponieważ jest to wartość domyślna dla zasad wykonywania programu bazy danych SQL. W przypadku zmiany zasad wykonywania, trzeba było również zmiany tutaj kod, który określa, ile razy są generowane w przypadku błędów przejściowych. Można również zmienić kod można wygenerować więcej wyjątki, aby zgłosi Entity Framework `RetryLimitExceededException` wyjątku.
3. W *Global.asax*, Dodaj następujące instrukcje using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Następnie dodaj wyróżnione wiersze do `Application_Start` metody:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Te wiersze są, co powoduje interceptora kodu do uruchomienia, gdy Entity Framework wysyła zapytania do bazy danych. Należy zauważyć, że ponieważ utworzyć oddzielne interceptora klasy do symulacji błędu przejściowego i rejestrowania, można niezależnie włączyć i wyłączyć je.   
  
 Można dodać przy użyciu interceptory `DbInterception.Add` metody w kodzie; nie ma w `Application_Start` — metoda. Inną opcją, jeśli nie dodasz interceptory w `Application_Start` metody, może być aktualizacja lub Dodaj klasę o nazwie *WingtipToysConfiguration.cs* i umieść powyżej kod na końcu konstruktora `WingtipToysbConfiguration` klasy.

Wszędzie tam, gdzie możesz umieścić ten kod nie można wykonać `DbInterception.Add` dla tego samego interceptora więcej niż raz, lub uzyskać dodatkowe interceptora wystąpień. Na przykład jeśli dodasz interceptora rejestrowania dwa razy, zostanie wyświetlone dwa dzienniki dla każdego zapytania SQL.

Interceptory są wykonywane w kolejności rejestracji (w kolejności `DbInterception.Add` metoda jest wywoływana). Kolejność może być niezależnie od tego, w zależności od wykonywanych w interceptor. Na przykład interceptora może zmienić polecenie SQL, które otrzymuje w `CommandText` właściwości. Jeśli go zmienić polecenie SQL, dalej interceptora zostanie zmienione polecenia SQL nie oryginalnego polecenia SQL.

Kod symulacji błędu przejściowego napisanych w taki sposób, który umożliwia spowodować błędów przejściowych wprowadzając inną wartość w interfejsie użytkownika. Alternatywnie można napisać kod interceptora, aby zawsze Generuj sekwencji przejściowej wyjątków bez sprawdzania pod kątem wartość określonego parametru. Następnie można dodać interceptor tylko wtedy, gdy chcesz wygenerować błędów przejściowych. Jeśli to zrobisz, jednak nie należy dodawać interceptora dopiero po zakończeniu inicjowania bazy danych. Innymi słowy wykonaj co najmniej jedną bazę danych operacji, takiej jak zapytania na jednym z zestawów jednostek, przed rozpoczęciem generowania błędów przejściowych. Entity Framework wykonuje zapytania kilka podczas inicjowania bazy danych, a nie są wykonywane w ramach transakcji, dlatego błędy podczas inicjowania może spowodować, że kontekst do pobrania w niespójnym stanie.

## <a name="test-logging-and-connection-resiliency"></a>Test połączenia i rejestrowanie odporności

1. W programie Visual Studio, naciśnij klawisz **F5** do uruchomienia aplikacji w trybie debugowania, a następnie zaloguj się jako "Admin" jako hasło przy użyciu "word Pa$ $".
2. Wybierz **Admin** na pasku nawigacyjnym u góry.
3. Wprowadź nowy produkt o nazwie "Throw" z odpowiedniego pliku opisu, ceny i obrazu.
4. Naciśnij klawisz **Dodaj produkt** przycisku.  
   Można zauważyć przeglądarki jest prawdopodobnie zawieszenie przez kilka sekund, podczas Entity Framework ponawia zapytanie kilka razy. Pierwsza próba bardzo szybko się stanie, a następnie zwiększa czas oczekiwania przed ponowną próbą wykonania każdy dodatkowego. Ten proces już oczekiwania przed wywołaniem kolejnymi próbami *wykładniczego wycofywania* .
5. Poczekaj, aż strona nie jest już atttempting do załadowania.
6. Zatrzymaj projekt i przyjrzyj się programu Visual Studio **dane wyjściowe** okno, aby wyświetlić dane wyjściowe śledzenia. Można znaleźć **dane wyjściowe** okna, wybierając **debugowania**  - &gt; **Windows**  - &gt;  **Dane wyjściowe**. Może być konieczne przewinięcie poza kilka innych dzienników napisane przez użytkownika rejestratora.  
  
   Zwróć uwagę, można wyświetlić rzeczywiste zapytania SQL wysyłane do bazy danych. Zobaczysz niektórych początkowej zapytań i poleceń, które jest Entity Framework, aby rozpocząć, sprawdzania tabeli historii wersji i migracja bazy danych.   
    ![Okno Dane wyjściowe](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Należy pamiętać, że nie Powtórz ten test, chyba że Zatrzymaj aplikację i uruchom go ponownie. Jeśli chcesz mieć możliwość testowania połączenia odporności wiele razy w jednym przebiegu aplikacji, można napisać kod, aby zresetować licznik błędów w `InterceptorTransientErrors` .
7. Różnice strategia wykonywania (zasady ponawiania) wystawia, komentarz `SetExecutionStrategy` wiersz w *WingtipToysConfiguration.cs* w pliku *logiki* folderu Uruchom **administratora**  ponownie strony w trybie debugowania, a następnie dodaj produktu o nazwie &quot;Throw&quot; ponownie.  
  
   Teraz debuger zatrzymuje na pierwszy wygenerowany wyjątek od razu, podczas próby wykonania zapytania po raz pierwszy.  
    ![Debugowanie — szczegóły](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Usuń znaczniki komentarza `SetExecutionStrategy` wiersz w *WingtipToysConfiguration.cs* pliku.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób modyfikowania formularzy sieci Web przykładowej aplikacji do obsługi opcji elastyczności połączenia i polecenia zatrzymania.

## <a name="next-steps"></a>Następne kroki

Po przejrzeniu opcji elastyczności połączenia i przechwytywaniu polecenia w formularzach sieci Web ASP.NET, przejrzyj temat formularzy sieci Web ASP.NET [metod asynchronicznych w programie ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). Temat uczy podstaw konstruowania asynchroniczne aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio.
