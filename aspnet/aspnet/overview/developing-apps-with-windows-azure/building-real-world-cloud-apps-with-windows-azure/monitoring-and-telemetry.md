---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitorowanie i Telemetria (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: b2c91b26fddc21986bf90957f7e4ef0a493d5837
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396764"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitorowanie i Telemetria (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Wiele osób zależą od klientów z informacją o tym, gdy ich aplikacja znajduje się w dół. Który nie jest tak naprawdę najlepszym rozwiązaniem jest dowolnym miejscu, a w szczególności nie znajduje się w chmurze. Nie ma żadnej gwarancji szybkiego powiadomień, a po użytkownik jest informowany, uzyskasz często minimalny lub błędne dane dotyczące co się stało. Z dobre systemy telemetrii i rejestrowania można wiedzieć co się dzieje, za pomocą aplikacji oraz natychmiast informują i udostępniają przydatne informacje dotyczące rozwiązywania problemów, pracować z przejść.

## <a name="buy-or-rent-a-telemetry-solution"></a>Kup lub wynajmować rozwiązanie telemetryczne

> [!NOTE]
> W tym artykule został napisany przed [usługi Application Insights](https://azure.microsoft.com/services/application-insights/) został wydany. Usługa Application Insights jest preferowanym podejściem telemetrii rozwiązań na platformie Azure. Zobacz [Konfigurowanie usługi Application Insights dla witryny sieci Web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) Aby uzyskać więcej informacji.


Jedną z rzeczy, czyli doskonałą temat środowiska chmury jest tak naprawdę łatwo kupować ani wypożyczać ułatwiający zwycięstwa. Dane telemetryczne znajduje się przykład. Bez wiele wysiłku możesz pobrać system telemetrii naprawdę dobre włączona i uruchomiona, bardzo ekonomiczne. Istnieje wiele doskonałe partnerów, które integrują się z platformą Azure, a niektóre z nich mają bezpłatnymi warstwami —, dlatego możesz też uzyskać podstawowe dane telemetryczne nothing. Oto kilka z nich dostępne obecnie na platformie Azure:

- [Narzędzie New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

Począwszy od marca 2015 r. [Microsoft Application Insights dla Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) nie została jeszcze wydana, ale jest dostępna w wersji zapoznawczej, możesz wypróbować. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) obejmuje również funkcji monitorowania.

Szybko przeprowadzimy przez proces konfigurowania usługi New Relic, aby pokazać, jak łatwo można używać system telemetrii.

W portalu zarządzania Azure utworzyć konto usługi. Kliknij przycisk **New**, a następnie kliknij przycisk **Store**. **Wybierz dodatek** pojawi się okno dialogowe. Przewiń w dół i kliknij przycisk **usługi New Relic**.

![Wybierz dodatek](monitoring-and-telemetry/_static/image1.png)

Kliknij strzałkę w prawo, a następnie wybierz warstwę usługi, które chcesz. Dla tej wersji demonstracyjnej użyjemy w warstwie bezpłatna.

![Personalizowanie dodatku](monitoring-and-telemetry/_static/image2.png)

Kliknij strzałkę w prawo, potwierdź "Kup", a narzędzie New Relic wyświetlane jako dodatek w portalu.

![Przejrzyj zakup](monitoring-and-telemetry/_static/image3.png)

![Nowy dodatek Relic w portalu zarządzania](monitoring-and-telemetry/_static/image4.png)

Kliknij przycisk **informacje o połączeniu**, a następnie skopiuj klucz licencji.

![Informacje o połączeniu](monitoring-and-telemetry/_static/image5.png)

Przejdź do **Konfiguruj** kartę dla aplikacji sieci web w portalu, ustaw **Performance Monitoring** do **dodatek**i ustaw **wybierz dodatek** listy rozwijanej, aby **usługi New Relic**. Następnie kliknij przycisk **Zapisz**.

![Narzędzie New Relic na karcie Konfiguracja](monitoring-and-telemetry/_static/image6.png)

W programie Visual Studio zainstaluj pakiet nowych Relic NuGet w swojej aplikacji.

![Rozszerzenie Developer analytics na karcie Konfiguracja](monitoring-and-telemetry/_static/image7.png)

Wdrażanie aplikacji na platformie Azure i rozpocząć korzystanie z niej. Utwórz kilka rozwiązać go zadań, aby zapewnić jakieś działania, narzędzie New Relic do monitorowania.

Następnie wróć do **usługi New Relic** strony w **dodatki** w portalu, a następnie kliknij na karcie **Zarządzaj**. Portal wysyła do portalu zarządzania usługi New Relic przy użyciu logowania jednokrotnego do uwierzytelniania, aby nie musieli wprowadzać poświadczeń ponownie. Na stronie Przegląd przedstawia informacje o różnych statystyk wydajności. (Kliknij obraz, aby zobaczyć pełny rozmiar strony Przegląd).

[![Nowa karta Relic monitorowania](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Poniżej przedstawiono kilka statystyk, które można zobaczyć:

- Średni czas odpowiedzi o różnych porach dnia.

    ![Czas odpowiedzi](monitoring-and-telemetry/_static/image10.png)
- Przepustowość (w żądań na minutę) o różnych porach dnia.

    ![Przepływność](monitoring-and-telemetry/_static/image11.png)
- Czas procesora CPU serwera poświęcony obsługi różnych żądań HTTP.

    ![Godziny transakcji sieci Web](monitoring-and-telemetry/_static/image12.png)
- Czas procesora CPU poświęcony w różnych częściach kodu aplikacji:

    ![Szczegóły śledzenia](monitoring-and-telemetry/_static/image13.png)
- Statystyki historyczne.

    ![W przeszłości](monitoring-and-telemetry/_static/image14.png)
- Wywołania usług zewnętrznych, takich jak usługi obiektów Blob i statystyk określających sposób niezawodny i elastyczny została usługi.

    ![Usługi zewnętrzne](monitoring-and-telemetry/_static/image15.png)

    ![Usługi zewnętrzne](monitoring-and-telemetry/_static/image16.png)

    ![Usługi zewnętrzne](monitoring-and-telemetry/_static/image17.png)
- Informacje o tym, gdzie w świecie lub w aplikacji sieci web w Stanach Zjednoczonych ruchu pochodzenia.

    ![Lokalizacja geograficzna](monitoring-and-telemetry/_static/image18.png)

Możesz również skonfigurować raporty i zdarzenia. Na przykład można podać wszystkie uruchomienia są wyświetlane błędy, Wyślij wiadomość e-mail do personelu obsługi alertów do problemu.

![Raporty](monitoring-and-telemetry/_static/image19.png)

New Relic to tylko jeden przykład system telemetrii; wszystko to można uzyskać z innych usług, jak również. Zaletą korzystania z chmury jest to, że bez konieczności pisania kodu, a na minimalny lub nie wydatków nagle uzyskać znacznie więcej informacji na temat sposobu korzystania z aplikacji i co klientów są rzeczywiście występuje.

<a id="log"></a>
## <a name="log-for-insight"></a>Dziennik, aby uzyskać szczegółowe informacje

Pakiet danych telemetrycznych jest dobre, pierwszym krokiem, ale nadal jest konieczne instrumentować swój własny kod. Usługa telemetrii informujący o tym, gdy występuje problem i informuje, jakie występują klientów, ale jej mogą odbiegać od mnóstwo szczegółowych co się dzieje w kodzie.

Nie chcesz mieć zdalny do serwera produkcyjnego, aby zobaczyć, jakie działania aplikacji. Może to być praktyczne, masz jeden serwer, ale co w przypadku, gdy była skalowana, aby setki serwerów i nie wiesz, które z nich należy zdalne? Usługi rejestrowania powinien zapewniać wystarczającej ilości informacji, które nigdy nie należy zdalne na serwerach produkcyjnych, aby analizowanie i debugowanie problemów. Możesz powinna się zalogować wystarczających informacji, dzięki czemu można wyizolować wyłącznie za pośrednictwem dzienniki.

### <a name="log-in-production"></a>Zaloguj się w środowisku produkcyjnym

Wiele osób włączyć śledzenie w środowisku produkcyjnym, tylko wtedy, gdy występuje problem, a chce on do debugowania. Może to wprowadzić znaczne opóźnienie między czasu, który masz świadomość problem i uzyskać przydatne informacje dotyczące rozwiązywania problemów, na jego temat czas. I uzyskasz informacje mogą być przydatne w przypadku sporadyczne błędy.

Firma Microsoft zaleca w środowisku chmury, w którym magazyn to tania jest zawsze pozostawienie logowanie w środowisku produkcyjnym. W ten sposób, w przypadku błędów już je rejestrowane i mieć dane historyczne, które mogą pomóc analizować problemy, które opracować wraz z upływem czasu, lub być regularnie w różnym czasie. Można zautomatyzować procesu przeczyszczania, aby usunąć stare dzienniki, ale może się okazać, że jest bardziej kosztowne skonfigurować taki proces niż na potrzeby przechowywania dzienników.

Dodano wydatków rejestrowania jest proste, w stosunku do rozwiązywania problemów, czas i pieniądze, której można zapisywać przez wszystkie informacje potrzebne już dostępne, gdy coś pójdzie nie tak. Następnie, gdy ktoś poda mieli oni błędy losowe jakiś czas około 8:00 w nocy, ale ich nie pamiętasz błąd, można łatwo znaleźć problemem był.

Dla mniej niż 4 USD miesiącu, w którym można zachować 50 gigabajtów dzienników dostępnych i wpływ na wydajność rejestrowania jest proste tak długo, jak przechowywać jedno pamiętać — Aby uniknąć problemów z wydajnością, upewnij się, że Twoja Biblioteka rejestrowania jest asynchroniczna.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Odróżnienie dzienników, które informują z dzienników, które wymagają akcji

Dzienniki są przeznaczone do POINFORMUJ (Chcę można dowiedzieć się coś) lub ACT (Chcę możesz dokonać czegoś). Uważaj tylko zapisywanie dzienników ACT dla problemów wymagających rzeczywiście osoba lub zautomatyzowanego procesu podejmowania działań. Za dużo dzienników ACT utworzy szumu, wymaga zbyt dużo pracy do przesiania wszystko można znaleźć oryginalnego problemów. A jeśli dzienników ACT automatycznie wyzwoli niektóre akcje, takie jak wysyłanie wiadomości e-mail do personelu obsługi, należy unikać, umożliwiając tysięcy takich czynności wyzwolone przez pojedynczego problemu.

W .NET System.Diagnostics śledzenia, dzienniki można przypisać błąd, ostrzeżenie, informacje i debugowania/Verbose poziom. ACT pozwala odróżnić od dzienników POINFORMUJ zarezerwowanie poziom błędu ACT dzienników i używając niższe poziomy POINFORMUJ dzienników.

![Poziomy rejestrowania](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Konfigurowanie poziomów rejestrowania w czasie wykonywania

Gdy warto mieć logowanie zawsze w środowisku produkcyjnym, inny najlepszym rozwiązaniem jest implementowanie struktury rejestrowania, który można dostosować w czasie wykonywania poziom szczegółowości, która jest rejestruje, bez konieczności ponownego wdrażania lub ponownego uruchamiania aplikacji. Na przykład kiedy używać funkcji śledzenia w `System.Diagnostics` można utworzyć błąd, ostrzeżenie, informacje i debugowania/pełne dzienniki. Firma Microsoft zaleca zawsze Zaloguj się błąd, ostrzeżenie, a informacje o dzienniki w środowisku produkcyjnym i warto mieć możliwość dynamicznego dodawania rejestrowanie debugowania/Verbose Rozwiązywanie problemów w przypadku przez.

Aplikacje sieci Web w usłudze Azure App Service ma wbudowaną obsługę pisania `System.Diagnostics` dzienników do systemu plików, Table storage lub Blob storage. Możesz wybrać poziomów rejestrowania różnych dla każdego miejsca docelowego przechowywania, a poziom rejestrowania na bieżąco można zmienić bez ponownego uruchamiania aplikacji. Obsługa magazynu obiektów Blob ułatwia uruchamianie [HDInsight](https://docs.microsoft.com/azure/hdinsight/) analizy zadania na dzienniki aplikacji, ponieważ HDInsight wie, jak pracować z usługą Blob storage bezpośrednio.

### <a name="log-exceptions"></a>Rejestruje wyjątki

Po prostu nie umieszczaj *wyjątku. ToString()* w kodzie rejestrowania. Który powoduje, że informacje kontekstowe. W przypadku wystąpienia błędów programu SQL instalacja pozostawia się numer błędu SQL. Dla wszystkich wyjątków zawierają informacje o kontekście, wyjątków, sama i wyjątków wewnętrznych, aby upewnić się, że udostępniasz wszystko, które będą potrzebne do rozwiązywania problemów. Na przykład informacje o kontekście może obejmować nazwę serwera, identyfikator transakcji i nazwę użytkownika (ale nie hasło lub jakichkolwiek kluczy tajnych!).

Jeśli polegać na każdy Deweloper w celu właściwe, z wyjątkiem rejestrowania, niektóre z nich nie. Aby upewnić się, że to dzieje po prawej stronie sposób za każdym razem, tworzyć obsługi bezpośredni dostęp do interfejsu rejestratora wyjątku: Przekaż obiekt wyjątku do klasy rejestratora i zaloguj się dane wyjątku prawidłowo klasy rejestratora.

### <a name="log-calls-to-services"></a>Dziennik wywołania usług

Zdecydowanie zaleca się, którą piszesz dziennika za każdym razem, gdy aplikacja odwołuje z usługą czy w bazie danych lub interfejsu API REST lub dowolnej usługi zewnętrznej. Dołącz dzienniki nie tylko dane dotyczące powodzenia lub niepowodzenia, ale czas trwania każdego żądania. W środowisku chmury często zobaczysz problemy związane z slow-downs, a nie pełne awarii. Coś, co zazwyczaj zajmuje to 10 milisekund nagle start, biorąc sekundy. Gdy użytkownik dowiaduje się, że Twoja aplikacja jest powolne, chcesz mieć możliwość Przyjrzyj się narzędzie New Relic lub niezależnie od usługi danych telemetrycznych mają i sprawdzić ich środowisko pracy, a następnie ma być możliwe do wyszukiwania są własne dzienniki, aby od razu do szczegółów, dlaczego jest powolne.

### <a name="use-an-ilogger-interface"></a>Jest używany interfejs ILogger

Co firma Microsoft zaleca wykonanie podczas tworzenia aplikacji produkcyjnych jest utworzenie prostej *ILogger* interfejsu i trzymaj niektóre metody w nim. Dzięki temu można łatwo zmienić później implementacja rejestrowania i nie musi przechodzić przez wszystkie swój kod, aby to zrobić. Firma Microsoft może używać `System.Diagnostics.Trace` klasy w całej aplikacji naprawić, ale zamiast tego używamy go dzieje się w tle w klasie rejestrowania, który implementuje *ILogger*, a firma Microsoft *ILogger* wywołania metod w całym aplikacja.

Dzięki temu, jeśli kiedykolwiek chcesz wprowadzić bardziej rozbudowane, usługi rejestrowania można zastąpić [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) z dowolnego mechanizmu rejestrowania. Na przykład wzrostem aplikacji można zdecydować ma używać bardziej kompleksowe pakietu rejestrowania, takich jak [NLog](http://nlog-project.org/) lub [Enterprise biblioteki Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) jest innej struktury popularnych rejestrowania, ale nie robi rejestrowania asynchronicznego.)

Jedną z możliwych przyczyn przy użyciu struktury, takiej jak NLog jest ułatwienie podział, logując się dane wyjściowe do oddzielnych mocno obciążające i o wysokiej wartości magazynów danych. Który ułatwia efektywne przechowywania dużych ilości danych POINFORMUJ, który nie jest potrzebny do wykonywania zapytań względem, przy zachowaniu szybki dostęp do danych ACT.

### <a name="semantic-logging"></a>Rejestrowania semantycznego

Dla stosunkowo nowym sposobie wykonania rejestrowania, które mogą wytwarzać bardziej użyteczne informacje diagnostyczne, zobacz [Enterprise biblioteki Semantic Logging Application Block (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Używa SLAB [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) i [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) pomocy technicznej w .NET 4.5, aby umożliwić tworzenie więcej dzienniki strukturalnych i obsługą zapytań. Należy zdefiniować inną metodę dla każdego typu zdarzenia, które możesz się zalogować, co pozwala na dostosowywanie informacje, które piszesz. Na przykład, aby rejestrować błąd bazy danych SQL może wywołać `LogSQLDatabaseError` metody. Dla tego rodzaju wyjątku wiesz, że kluczowych informacji jest numer błędu, dzięki czemu może zawierać parametr z liczbą błędów w podpisie metody i Zapisz numer błędu jako oddzielne pola w rekordzie dziennika, którą piszesz. Ponieważ liczba znajduje się w oddzielnych pola łatwiejsze i bardziej niezawodny sposób będą Ci raportów opartych na numery błędu SQL, niż w przypadku zostały po prostu złączenie numer błędu w ciąg wiadomości.

## <a name="logging-in-the-fix-it-app"></a>Rejestrowanie w poprawkę aplikacji

### <a name="the-ilogger-interface"></a>Interfejs ILogger

Oto *ILogger* interfejsu w aplikacji naprawić.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Te metody pozwalają na zapisywanie dzienników w tej samej cztery poziomy obsługiwane przez *System.Diagnostics*. Metody TraceApi są rejestrowania wywołania usług zewnętrznych z informacjami o opóźnieniu. Można również dodać zestaw metod do debugowania/Verbose poziom.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Rejestrator implementacji interfejsu ILogger

Implementacja interfejsu jest naprawdę proste. Zasadniczo po prostu wywołuje w postaci standardowych *System.Diagnostics* metody. Poniższy fragment kodu pokazuje wszystkie trzy metody informacji i jeden dla każdej z pozostałych.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Wywoływanie metod ILogger

Za każdym razem, gdy kod w aplikacji naprawić wyłapuje wyjątek, wywołuje *ILogger* metody do rejestrowania szczegółów wyjątku. I za każdym razem, gdy go nawiązuje połączenie bazy danych, usługi obiektów Blob lub interfejsu API REST, stopera przed wywołaniem uruchamia, zatrzymuje stoper po powrocie z usługi i rejestruje czas, jaki upłynął wraz z informacjami o powodzeniu lub niepowodzeniu.

Należy zauważyć, że komunikat dziennika zawiera nazwę klasy i nazwy metody. Jest dobrą praktyką, aby upewnić się, czy komunikaty w dzienniku zidentyfikować, jaka część kodu aplikacji napisał je.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Więc teraz za każdym razem, gdy aplikacja naprawić podejścia biznesowego uczyniło wywołania do bazy danych SQL można sprawdzić, wywołanie metody, która nazwała go, a dokładnie ile czasu zajęło.

![Zapytanie bazy danych SQL w dziennikach](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Jeśli Przejdź Przeglądanie dzienników, zostanie wyświetlony, że czas wywołań bazy danych jest zmienna. Czy informacje mogą być przydatne: ponieważ aplikacja rejestruje wszystkie te można analizować trendy historyczne jak działa usługa bazy danych wraz z upływem czasu. Na przykład usługa może być szybko w większości przypadków, ale żądań może zakończyć się niepowodzeniem lub odpowiedzi może spowolnić w pewnych porach dnia.

Możesz zrobić to samo dla usługi Blob —, każdym razem, gdy aplikacja przekazuje nowy plik, dziennik i możesz zobaczyć dokładnie, jak długo do przekazywania wszystkich plików.

![Dziennik przekazywania obiektu blob](monitoring-and-telemetry/_static/image23.png)

Konieczne jest kilku dodatkowe wiersze kodu do zapisania za każdym razem, gdy wywoływanie usługi i teraz zawsze, gdy ktoś mówi, że ich napotkał problem, możesz zapoznanie się z dokładnie ten problem, czy był błąd, lub nawet, jeśli była uruchomiona po prostu wolno. Można zidentyfikować źródło problemu bez konieczności zdalny do serwera lub włączyć rejestrowanie po błędu się dzieje i mamy nadzieję utworzyć ją ponownie.

## <a name="dependency-injection-in-the-fix-it-app"></a>Wstrzykiwanie zależności w rozwiązać go aplikacji

Być może zastanawiasz się, jak Konstruktor repozytorium w powyższym przykładzie pobiera Rejestrator implementacji interfejsu:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Aby Podłączanie interfejs do implementacji aplikacji używa [wstrzykiwanie zależności](http://en.wikipedia.org/wiki/Dependency_injection)(DI) za pomocą [AutoFac](http://autofac.org/). Iniekcja zależności pozwala używać obiektu w oparciu o interfejs w wielu miejscach w kodzie oraz tylko do określenia w jednym miejscu wdrożenia, który jest używany podczas tworzenia wystąpienia interfejsu. To sprawia, że łatwiej Zmień implementację: na przykład możesz chcieć zastąpić rejestratora System.Diagnostics NLog rejestratora. Lub potrzeby zautomatyzowanego testowania możesz chcieć zastąpić wersję makiety obiektu rejestratora.

Aplikacja naprawić korzysta z DI wszystkich repozytoriów i wszystkich kontrolerów. Pobierz konstruktory klas kontrolera *ITaskRepository* interfejs ten sam sposób, w repozytorium, pobiera interfejs rejestratora:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Aplikacja używa biblioteki AutoFac DI automatycznie zapewnienie *TaskRepository* i *rejestratora* wystąpień dla tych konstruktorów.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Ten kod po prostu mówi, że dowolne miejsce konstruktora musi *ILogger* interfejsu, przekaż wystąpienie *rejestratora* klasy i potrzebny *IFixItTaskRepository*interfejsu, przekaż wystąpienie *FixItTaskRepository* klasy.

[AutoFac](http://autofac.org/) jest jednym z wiele struktur iniekcji zależności, które są dostępne. Jest inny popularnych [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), która jest zalecana i obsługiwane przez Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Obsługa wbudowane funkcje rejestrowania na platformie Azure

Platforma Azure obsługuje następujące rodzaje z [rejestrowania dla usługi Web Apps w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Śledzenie System.Diagnostics (możesz włączać i wyłączać i ustawić poziomy na bieżąco bez ponownego uruchomienia witryny).
- Zdarzenia Windows.
- Dzienniki usług IIS (HTTP/FREB).

Platforma Azure obsługuje następujące rodzaje z [rejestrowania w usługach w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Śledzenie System.Diagnostics.
- Liczniki wydajności.
- Zdarzenia Windows.
- Dzienniki usług IIS (HTTP/FREB).
- Monitorowanie niestandardowym katalogu.

Aplikacja rozwiązać go używa śledzenia System.Diagnostics. Wszystko, co należy zrobić, aby włączyć System.Diagnostics rejestrowania w aplikacji sieci web jest przerzucić przełącznika w portalu lub wywołania interfejsu API REST. W portalu, kliknij przycisk **konfiguracji** dla danej witryny, a następnie przewiń w dół zobacz **programu Application Diagnostics** sekcji. Można Włączanie lub wyłączanie rejestrowania, a następnie wybierz żądany poziom rejestrowania. Może mieć zapisu w dziennikach systemu plików lub konta magazynu platformy Azure.

![Diagnostyka aplikacji i Diagnostyka witryny, na karcie Konfiguracja](monitoring-and-telemetry/_static/image24.png)

Po włączeniu rejestrowania na platformie Azure, można wyświetlić dzienniki w oknie programu Visual Studio danych wyjściowych, jak są one tworzone.

![Przesyłanie strumieniowe dzienników menu](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Przesyłanie strumieniowe dzienników menu](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Może również mieć Dzienniki zapisane na koncie magazynu i wyświetl je za pomocą dowolnego narzędzia dostęp do usługi Azure Storage Table, takie jak **Eksploratora serwera** w programie Visual Studio lub [Eksploratora usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

![Dzienniki w Eksploratorze serwera](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Podsumowanie

Jest naprawdę proste Implementowanie system telemetrii out-of--box, Instrumentacja rejestrowania we własnym kodzie i skonfigurować rejestrowanie na platformie Azure. A jeśli masz problemy w środowisku produkcyjnym, kombinacja system telemetrii i niestandardowe dzienniki pomoże Ci szybko rozwiązać problemy, zanim staną się one poważne problemy dla swoich klientów.

W [następny rozdział](transient-fault-handling.md) omówimy sposób obsługi błędów przejściowych, dzięki czemu nie staną się problemy w środowisku produkcyjnym, które wymagają zbadania.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji zobacz następujące zasoby.

Dokumentacja głównie o telemetrii:

- [Microsoft Patterns and Practices — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki dotyczące Instrumentacji i Telemetrii, pomiarów usługi wskazówki, wzorzec monitorowania punktu końcowego kondycji i wzorzec ponownej konfiguracji środowiska uruchomieniowego.
- [Penny Uszczypnięć w chmurze: Włączanie monitorowania w usłudze Azure Websites nowe wydajności Relic](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usługach Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument — Markiem Simmsem i Michael Thomassy. W sekcji Telemetria i Diagnostyka.
- [Tworzenie następnej generacji za pomocą usługi Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artykuł w MSDN Magazine.

Dokumentacja przede wszystkim dotyczące rejestrowania:

- [Blok aplikacji rejestrowania semantycznego (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie przedstawia w przypadku rejestrowania semantycznego z SLAB.
- [Tworzenie dzienniki ze strukturą i zrozumiała, za pomocą funkcji rejestrowania semantycznego](https://channel9.msdn.com/Events/Build/2013/3-336). (Wideo) Dominguez juliański przedstawia w przypadku rejestrowania semantycznego z SLAB.
- [EF6 Rejestrowanie SQL — część 1: rejestrowanie proste](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers pokazuje, jak rejestrować zapytania wykonywane przez program Entity Framework 6 EF.
- [Połączeń i przejmowanie poleceń z platformą Entity Framework w aplikacji ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Czwarty w dziewięć części serii samouczków pokazano, jak na potrzeby funkcji przejmowanie poleceń programów EF 6 rejestrowania poleceń SQL wysyłane do bazy danych przez program Entity Framework.
- [Poprawa rejestrowania przy użyciu języka C# w wersji 5.0 Caller — atrybuty informacji](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Jak łatwo rejestrować nazwę wywoływania metody bez kodować je do literały lub pobrać ją ręcznie przy użyciu odbicia.

Dokumentacja głównie dotyczących rozwiązywania problemów:

- [Azure Troubleshooting &amp; debugowania blogu](https://blogs.msdn.com/b/kwill/).
- [AzureTools — narzędzie do diagnostyki używane przez zespół pomocy technicznej platformy Azure dla deweloperów](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Wprowadza i udostępnia link pobierania dla narzędzia, który może służyć w Maszynie wirtualnej platformy Azure do pobierania i uruchamiania szerokiej gamy narzędzi diagnostycznych i monitorowania. Parametr jest przydatne, gdy trzeba zdiagnozować problem na konkretnej maszyny Wirtualnej.
- [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Samouczek krok po kroku dla klientów zaczynających się od System.Diagnostics śledzenia i debugowania zdalnego.

Filmy wideo:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Dziewięć serii Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Przedstawia informacje o szczegółowo pojęcia i zasady dotyczące architektury w sposób bardzo dostępny i interesujące z historii z doświadczenia zespołu doradczego klientów firmy Microsoft (CAT) z klientów. Odcinki 4 i 9 są dotyczące monitorowania i telemetrii. 9 odcinek zawiera omówienie monitorowania usług MetricsHub, AppDynamics i New Relic oraz usługi PagerDuty.
- [Tworzenie Big: Wnioski wyciągnięte z klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). — Markiem Simmsem omawia projektowanie pod kątem błędów i instrumentacji wszystko. Podobnie jak przed uszkodzeniami serii, ale zbliża się bardziej szczegółowe informacje z instrukcjami.

Przykładowy kod:

- [Podstawy usługi na platformie Azure w chmurze](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja utworzona przez zespół doradczy klientów firmy Microsoft Azure. Pokazuje telemetrii i rejestrowania praktyk, zgodnie z opisem w następujących artykułach. Przykład implementuje rejestrowanie aplikacji przy użyciu [NLog](http://nlog-project.org/). Powiązane dokumentację można znaleźć [serię czterech artykuły wiki TechNet na temat telemetrii i rejestrowania](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Poprzednie](design-to-survive-failures.md)
> [dalej](transient-fault-handling.md)
