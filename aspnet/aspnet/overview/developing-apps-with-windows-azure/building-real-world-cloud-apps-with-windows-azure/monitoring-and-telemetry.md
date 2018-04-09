---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitorowanie i dane telemetryczne (tworzenia rzeczywistych aplikacji w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: d58c495b3888c146a2a9bc831865cf7cc0d94c7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitorowanie i dane telemetryczne (tworzenia rzeczywistych aplikacji w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Wiele osób zależne od klientów, aby go poinformować, gdy ich stosowania jest wyłączony. Który nie jest najlepszym rozwiązaniem dowolnego miejsca, a szczególnie nie znajduje się w chmurze. Nie ma żadnej gwarancji szybkie powiadomień, a gdy użytkownik jest informowany, często uzyskać minimalnego lub błędne dane dotyczące co się stało. Z dobrym telemetrii i rejestrowanie systemów, które można świadomość co się dzieje z aplikacją, a gdy coś udaje dowiedzieć się od razu i mieć przydatne informacje dotyczące rozwiązywania problemów do pracy z.

## <a name="buy-or-rent-a-telemetry-solution"></a>Kup lub wynajmować rozwiązania telemetrii

> [!NOTE]
> Ten artykuł dotyczy przed [usługi Application Insights](https://azure.microsoft.com/services/application-insights/) został zwolniony. Usługa Application Insights jest to preferowane rozwiązanie telemetrii rozwiązania na platformie Azure. Zobacz [skonfiguruj usługę Application Insights dla witryny sieci Web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) Aby uzyskać więcej informacji.


Jednym z elementów, które wyróżnia środowiska chmury jest naprawdę łatwo kupić lub wynajmować rozpocząć zwycięstwa. Przykładem jest telemetrii. Bez wiele wysiłku włączone i uruchomione, bardzo ekonomicznym można uzyskać system naprawdę dobrze telemetrii. Istnieją grupy dużą partnerów, które integrują się z platformą Azure, a niektóre z nich ma warstwy bezpłatna —, więc możesz też uzyskać podstawowe dane telemetryczne nothing. Poniżej przedstawiono kilka te dostępnych na platformie Azure:

- [Usługi New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

Począwszy od marca 2015 [Microsoft Application Insights dla programu Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) nie jest jeszcze zwolnione, ale są dostępne w wersji zapoznawczej możesz wypróbować usługę. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) obejmuje również funkcji monitorowania.

Szybko pomożemy przy konfigurowaniu usługi New Relic pokazanie, jak łatwo można ją przy użyciu systemu telemetrii.

W portalu zarządzania Azure Utwórz konto usługi. Kliknij przycisk **nowy**, a następnie kliknij przycisk **magazynu**. **Wybierz dodatek** zostanie wyświetlone okno dialogowe. Przewiń w dół i kliknij przycisk **usługi New Relic**.

![Wybierz dodatek](monitoring-and-telemetry/_static/image1.png)

Kliknij strzałkę w prawo, a następnie wybierz pozycję warstwy usług, które mają. Dla tego pokazu użyjemy warstwę bezpłatna.

![Personalizowanie dodatku](monitoring-and-telemetry/_static/image2.png)

Kliknij strzałkę w prawo, potwierdź "zakupu" i usługi New Relic teraz wyświetlany jako dodatek w portalu.

![Przejrzyj zakupu](monitoring-and-telemetry/_static/image3.png)

![Nowy dodatek Relic w portalu zarządzania](monitoring-and-telemetry/_static/image4.png)

Kliknij przycisk **informacje o połączeniu**i skopiuj klucz licencji.

![Informacje o połączeniu](monitoring-and-telemetry/_static/image5.png)

Przejdź do **Konfiguruj** karcie dla aplikacji sieci web w portalu, należy ustawić **monitorowania wydajności** do **dodatek**i ustaw **wybierz dodatek** listy rozwijanej, aby **usługi New Relic**. Następnie kliknij przycisk **zapisać**.

![Usługi New Relic na karcie Konfiguracja](monitoring-and-telemetry/_static/image6.png)

W programie Visual Studio zainstaluj pakiet NuGet Relic nowych w aplikacji.

![Developer analytics na karcie Konfiguracja](monitoring-and-telemetry/_static/image7.png)

Wdrażanie aplikacji na platformie Azure i rozpocząć korzystanie z niej. Utwórz kilka poprawka zadania zapewnienie niektóre działania dla usługi New Relic do monitorowania.

Następnie przejdź wstecz do **usługi New Relic** strony **dodatki** portalu i kliknij na karcie **Zarządzaj**. Portal wysyła do portalu zarządzania usługi New Relic przy użyciu logowania jednokrotnego do uwierzytelniania, dzięki czemu nie trzeba ponownie wprowadzić swoje poświadczenia. Strony Przegląd przedstawia informacje o różnych statystyki. (Kliknij obraz, aby zobaczyć pełną rozmiar strony Przegląd).

[![Nowa karta Relic monitorowania](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Poniżej przedstawiono kilka widoczna statystyki:

- Średni czas odpowiedzi o różnych porach dnia.

    ![Czas odpowiedzi](monitoring-and-telemetry/_static/image10.png)
- Przepustowość (w żądaniach na minutę) o różnych porach dnia.

    ![Przepływność](monitoring-and-telemetry/_static/image11.png)
- Czas procesora CPU serwera poświęconego obsługę różnych żądań HTTP.

    ![Czasy transakcji sieci Web](monitoring-and-telemetry/_static/image12.png)
- Czas Procesora poświęcony na różne części kodu aplikacji:

    ![Szczegółowe informacje śledzenia](monitoring-and-telemetry/_static/image13.png)
- Statystyki historycznych.

    ![W przeszłości](monitoring-and-telemetry/_static/image14.png)
- Wywołania usług zewnętrznych, takich jak usługa Blob i statystyka sposób niezawodny i elastyczny została usługi.

    ![Usług zewnętrznych](monitoring-and-telemetry/_static/image15.png)

    ![Usług zewnętrznych](monitoring-and-telemetry/_static/image16.png)

    ![Zewnętrzna usługa](monitoring-and-telemetry/_static/image17.png)
- Informacje o tym, gdzie w świecie lub gdy w aplikacji sieci web USA ruchu sieciowego pochodzi z.

    ![Lokalizacja geograficzna](monitoring-and-telemetry/_static/image18.png)

Można również skonfigurować raporty i zdarzeń. Można na przykład powiedzieć w dowolnym momencie możesz zacząć się wyświetlać błędy, Wyślij wiadomość e-mail do personelu pomocy technicznej alertu problemu.

![Raporty](monitoring-and-telemetry/_static/image19.png)

Usługi New Relic jest tylko jeden przykład systemu telemetrii; wszystkie te można uzyskać z innych usług, jak również. Zaletą chmury jest to, że bez konieczności pisania kodu, a także minimalnego lub nie wydatków nagle Ci znacznie więcej informacji na temat sposobu używania aplikacji i co klientów faktycznie występują.

<a id="log"></a>
## <a name="log-for-insight"></a>Dziennik, aby uzyskać szczegółowe informacje o

Pakiet danych telemetrycznych jest dobrym, ale nadal jest konieczne Instrumentacja swoim własnym kodem. Usługi telemetrii informuje, gdy występuje problem i informuje, jakie występują klientów, ale jego może nie zapewniają wiele analizę co się dzieje w kodzie.

Nie chcesz do zdalnego na serwerze produkcyjnym, aby wyświetlić czynności aplikacji. Co może być praktyczne, gdy masz jeden serwer, a co około zostały skalowane w celu setki serwerów i nie wiadomo, które z nich należy do zdalnego? Twoje rejestrowania powinien umożliwi zebranie informacji, które nigdy nie należy do serwerów produkcyjnych do analizowania i debugowania zdalnego problemów. Możesz powinien rejestrowania za mało informacji, dzięki czemu można wyizolować problemy wyłącznie za pośrednictwem dzienniki.

### <a name="log-in-production"></a>Zaloguj się w środowisku produkcyjnym

Wiele osób Włącz śledzenie w środowisku produkcyjnym, tylko wtedy, gdy problem i chcą debugowania. Może to powodować znaczne opóźnienie między czasem znane problem i uzyskać przydatne informacje dotyczące rozwiązywania problemów dotyczących jej czas. I uzyskasz informacje mogą być przydatne do sporadyczne błędy.

Firma Microsoft zaleca w środowisku chmury, w którym magazyn to tanie jest zawsze pozostawienie logowania w środowisku produkcyjnym. Dzięki temu, gdy wystąpi błędy ich rejestrowane już istnieje i ma danych historycznych, które mogą ułatwić analizę problemów, które tworzenie w czasie lub być regularnie w różnym czasie. Można zautomatyzować proces przeczyszczania, aby usunąć stare dzienniki, ale może okazać się bardziej kosztowne do skonfigurowania takiego procesu, nie należy przechowywać w dziennikach.

Dodano wydatków rejestrowania jest proste, w porównaniu do ilości Rozwiązywanie problemów z czas i pieniądze, można zapisać przez wszystkie informacje potrzebne już dostępne w przypadku wystąpienia problemów. Następnie, gdy ktoś informuje miało losowe błąd jakimś około 8:00 w nocy ostatnich, ale ich nie zapamiętuj błąd, można łatwo sprawdzić problem został.

Mniej niż 4 $ miesiąc, który umożliwia zachowanie 50 GB dzienników dostępnych i wpływu na wydajność rejestrowania jest proste tak długo, jak zachować rzecz pamiętać — Aby uniknąć wąskich gardeł wydajności, upewnij się, że biblioteka rejestrowania jest asynchroniczne.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Rozróżnianie dzienniki, które informują z dzienników, które wymagają akcji

Dzienniki są przeznaczone do INFORM (Chcę znajomości coś) lub ACT (Chcę do). Należy zachować ostrożność tylko zapisywanie dzienników ACT problemów, które rzeczywiście wymagają przez osobę lub zautomatyzowany proces podejmowania żadnych działań. Za dużo dzienniki ACT utworzy szumu wymagające za dużo pracy do przesiania wszystko to można znaleźć oryginalnego problemy. A jeśli dzienniki ACT automatycznie wyzwoli niektóre akcje, takie jak wysyłanie wiadomości e-mail do personelu obsługi, dzięki czemu tysiące takie akcje wyzwalane przez jeden problem.

W śledzenia .NET System.Diagnostics dzienniki można przypisać błąd, ostrzeżenie, informacje i debugowania/Verbose poziom. Działanie pozwala odróżnić od dzienniki INFORM rezerwowania poziom błędu dzienników ACT, a następnie użyć niższych poziomach INFORM dzienników.

![Poziomy rejestrowania](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Konfigurowanie poziomów rejestrowania w czasie wykonywania

Gdy warto mieć logowania zawsze w środowisku produkcyjnym, innego najlepszym rozwiązaniem jest wdrożenie struktury rejestrowania, który można dostosować w czasie wykonywania, poziom szczegółowości, który jest rejestrowanie, bez ponownego wdrażania lub ponownego uruchamiania aplikacji. Na przykład jeśli używasz funkcji śledzenia w `System.Diagnostics` można utworzyć błąd, ostrzeżenie, informacje i dzienników debugowania/Verbose. Firma Microsoft zaleca zawsze dziennika błąd, ostrzeżenie, informacje logowania w środowisku produkcyjnym i warto mieć możliwość dynamicznego dodawania debugowania/pełne rejestrowanie dla rozwiązywania problemów w przypadku.

Aplikacje sieci Web w usłudze Azure App Service ma wbudowaną obsługę zapisu `System.Diagnostics` dzienniki systemu plików, Magazyn tabel lub magazynie obiektów Blob. Możesz wybrać poziomy rejestrowania różne dla każdego miejscem docelowym magazynu, a poziom rejestrowania na bieżąco można zmienić bez ponownego uruchamiania aplikacji. Obsługa magazynu obiektów Blob ułatwia Uruchom [HDInsight](https://docs.microsoft.com/azure/hdinsight/) analizy zadania na dzienniki aplikacji, ponieważ HDInsight wie, jak pracować bezpośrednio z magazynem obiektów Blob.

### <a name="log-exceptions"></a>Wyjątki dziennika

Nie należy umieszczać po prostu *wyjątku. ToString()* w kodzie rejestrowania. Który powoduje, że informacje kontekstowe. W przypadku błędów SQL wydostaje się numer błędu SQL. Wszystkie wyjątki zawierają informacje o kontekście, wyjątek sam i wyjątków wewnętrznych, aby upewnić się, że udostępniasz wszystko, które będą potrzebne do rozwiązywania problemów. Na przykład informacje o kontekście mogą obejmować nazwę serwera, identyfikator transakcji i nazwę użytkownika (, ale nie hasło lub żadnych kluczy tajnych!).

Jeśli użytkownik korzysta z wszystkich deweloperów robić, co z wyjątkiem rejestrowania, niektóre z nich nie. Aby upewnić się, że go pobiera wykonywane po prawej sposób za każdym razem, Tworzenie wyjątków, obsługa prawo do interfejsu rejestratora: Przekaż obiekt wyjątku do klasy rejestratora i zaloguj ponownie dane wyjątku prawidłowo klasy rejestratora.

### <a name="log-calls-to-services"></a>Dziennik wywołania usług

Zdecydowanie zaleca się Zapisz dziennik każdorazowego aplikacji uwidacznia do usługi, czy jest do bazy danych lub interfejsu API REST lub zewnętrznej usługi. Umieszczone w dziennikach nie tylko informacje dotyczące powodzenia lub błędu, ale jak długo trwało każdego żądania. W środowisku chmury często zobaczysz problemy związane z slow-downs zamiast pełnej awarii. Rzecz, która zazwyczaj zajmuje to 10 milisekund nagle start, biorąc sekundy. Gdy ktoś informuje, że aplikacja jest powolne, chcesz mieć możliwość przyjrzeć się usługi New Relic, lub niezależnie od usługi telemetrii ma i weryfikacji ich obsługi, a następnie ma być możliwe do wyszukiwania są własne dzienniki, aby przejść do szczegółów dlaczego jest powolne.

### <a name="use-an-ilogger-interface"></a>Użyj interfejsu ILogger

Co zaleca się zrobienie podczas tworzenia aplikacji produkcyjnych jest utworzenie prostego *ILogger* interfejsu i kształcie niektóre metody w nim. Dzięki temu można łatwo zmienić później implementacji rejestrowania i nie musi przechodzić przez wszystkie swój kod, aby to zrobić. Firma Microsoft może używać `System.Diagnostics.Trace` klasy w całej aplikacji rozwiązać, ale zamiast tego używamy go w obszarze obejmuje w klasie rejestrowania, który implementuje *ILogger*, i wykonujemy *ILogger* wywołania metod w ciągu aplikacja.

Dzięki temu, jeśli kiedykolwiek mają być bardziej rozbudowane, Twoje rejestrowania można zastąpić [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) z dowolnego mechanizmu rejestrowania. Na przykład, wraz z rozwojem aplikacji można zdecydować mają używać pakietu bardziej szczegółowe rejestrowanie, takich jak [NLog](http://nlog-project.org/) lub [bloku aplikacji rejestrowania biblioteki Enterprise](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) jest inny struktury rejestrowania popularnych, ale nie wykonuje asynchroniczne rejestrowania.)

Jedną z możliwych przyczyn przy użyciu platformy takie jak NLog funkcji jest ułatwienie podziału rejestrowania danych wyjściowych do magazynów oddzielnego danych dużych i wysokiej wartości. Który ułatwia efektywne przechowywania dużych ilości danych INFORM, który nie jest konieczne wykonywanie szybkiej zapytań względem, przy zachowaniu szybki dostęp do danych ACT.

### <a name="semantic-logging"></a>Rejestrowanie semantycznego

Dla stosunkowo nowy sposób rejestrowania, który może tworzyć bardziej użyteczne informacje diagnostyczne czy, zobacz [Enterprise biblioteki semantycznego rejestrowania aplikacji bloku (PŁYCIE)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Używa PŁYCIE [śledzenia zdarzeń dla systemu Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) i [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) obsługi w programie .NET 4.5, aby umożliwić tworzenie więcej dzienników strukturalnych i kolejność. Należy zdefiniować inną metodę dla każdego typu zdarzenia logowania, dzięki czemu można tak dostosować informacje zostanie zapisany. Na przykład, aby rejestrować błąd bazy danych SQL może wywołać `LogSQLDatabaseError` metody. Tego rodzaju wyjątku znasz klucza część informacji jest kod błędu może zawierać parametr liczby błędów w podpisie metody i Zapisz jako osobne pole w rekordzie dziennika zostanie zapisany numer błędu. Ponieważ kod jest w osobnym polu łatwiejsze i bardziej niezawodny sposób uzyskania raportów opartych na numery błąd SQL niż w przypadku były tylko łączenie numer błędu w ciąg komunikatu.

## <a name="logging-in-the-fix-it-app"></a>Rejestrowanie w poprawkę aplikacji

### <a name="the-ilogger-interface"></a>Interfejs ILogger

Oto *ILogger* interfejsu w aplikacji rozwiązać.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Te metody umożliwiają zapisywanie dzienników w tym samym czterech poziomów obsługiwanych przez *System.Diagnostics*. Metody TraceApi są rejestrowanie zewnętrznej usługi wywołania z informacjami o opóźnienia. Można również dodać zestaw metod dla debugowania/Verbose poziomu.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Implementacja interfejsu ILogger rejestratora

Implementacja interfejsu jest naprawdę proste. Zasadniczo po prostu wywołuje do standardowego *System.Diagnostics* metody. Poniższy fragment kodu przedstawia wszystkie trzy metody informacji i jeden dla każdej z pozostałych.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Wywoływanie metody ILogger

Za każdym razem, gdy wyjątek zostanie przechwycony kodu w aplikacji rozwiązać, wywołuje *ILogger* metody logowania szczegóły wyjątku. I za każdym razem, gdy go nawiązuje połączenie bazy danych, usługa Blob lub interfejsu API REST, stopera przed wywołaniem uruchamia, zatrzymuje stopera, po powrocie z usługi i rejestruje czas, który wraz z informacjami o powodzeniu lub niepowodzeniu.

Zwróć uwagę, że komunikat dziennika zawiera nazwę klasy i nazwę metody. Jest dobrym rozwiązaniem, aby upewnić się, że komunikaty w dzienniku zidentyfikować, jaka część kodu aplikacji zapisał je.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Dlatego teraz raz co aplikacji Usuń dokonała wywołania bazy danych SQL można sprawdzić wywołania metody, która wywołuje się, i trwało dokładnie, jak dużo czasu.

![Zapytanie bazy danych SQL w dziennikach](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Przejdź Przeglądanie dzienników, widać że czas podjęcia wywołania bazy danych jest zmienna. Czy informacje mogą być przydatne: ponieważ dzienników aplikacji, to można analizować trendy historyczne w sposób działa usługa bazy danych wraz z upływem czasu. Na przykład usługi może być szybkiego w większości przypadków, ale żądań może zakończyć się niepowodzeniem lub odpowiedzi może spowolnić w pewnych porach dnia.

Możesz to zrobić to samo dla usługi Blob — za każdym razem aplikacji przekazuje nowy plik, Brak dziennika i widać dokładnie, jak długo trwało Przekaż każdego pliku.

![Obiekt blob przekazywania dziennika](monitoring-and-telemetry/_static/image23.png)

Jest kilka dodatkowych wierszy kodu można zapisać za każdym razem wywoływanie usługi sieci i teraz zawsze, gdy ktoś stwierdzający, że ich napotkał problem, możesz zapoznanie się z dokładnie problem, czy błąd lub nawet, jeśli była uruchomiona tak wolno. Można zidentyfikować źródło problemu bez konieczności zdalnego na serwerze lub włączyć rejestrowanie po błąd wykonywany i nadzieję, utwórz go ponownie.

## <a name="dependency-injection-in-the-fix-it-app"></a>Iniekcji zależności w poprawkę go aplikacji

Może zastanawiasz się, jak w powyższym przykładzie konstruktora repozytorium pobiera Rejestrator implementacji interfejsu:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Aby okablować interfejs do implementacji aplikacja używa [iniekcji zależności](http://en.wikipedia.org/wiki/Dependency_injection)(Podpisane) z [AutoFac](http://autofac.org/). Iniekcja zależności pozwala mogą używać obiektu oparte na interfejsie w wielu miejscach w kodzie i mają tylko określone w jednym miejscu wdrożenia, który jest używany podczas tworzenia wystąpienia klasy interfejsu. To ułatwia Zmień implementację: na przykład możesz chcieć Zamień rejestratora System.Diagnostics NLog rejestratora. Lub do testów automatycznych może mają zostać podstawione wersję makiety rejestratora.

Aplikacja napraw go używa Podpisane wszystkie repozytoriów i wszystkich kontrolerów. Pobierz konstruktorów klas kontrolera *ITaskRepository* interfejsu tak samo repozytorium pobiera interfejs rejestratora:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Aplikacja używa biblioteki Podpisane AutoFac można automatycznie udostępnić *TaskRepository* i *rejestratora* wystąpień dla tych konstruktorów.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Ten kod zasadniczo mówi, że dowolnym konstruktora musi *ILogger* interfejsu, przekaż wystąpienie *rejestratora* klasy, oraz kiedy musi *IFixItTaskRepository*interfejsu, przekaż wystąpienie *FixItTaskRepository* klasy.

[AutoFac](http://autofac.org/) jest jednym z wielu platform iniekcji zależności mogą używających. Inna popularnych jest [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), co jest zalecane i obsługiwane przez Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Obsługa wbudowanych rejestrowania na platformie Azure

Azure obsługuje następujące rodzaje z [rejestrowanie dla aplikacji sieci Web w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Śledzenie System.Diagnostics (można włączyć lub wyłączyć i skonfigurować poziomy na bieżąco bez ponownego uruchamiania witryny).
- Zdarzenia systemu Windows.
- Dzienniki programu IIS (HTTP/FREB).

Azure obsługuje następujące rodzaje z [rejestrowania usług w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Śledzenie System.Diagnostics.
- Liczniki wydajności.
- Zdarzenia systemu Windows.
- Dzienniki programu IIS (HTTP/FREB).
- Katalog niestandardowych monitorowania.

Aplikacja napraw korzysta z System.Diagnostics śledzenia. Wszystko, co należy zrobić, aby włączyć System.Diagnostics rejestrowania w aplikacji sieci web jest Przerzucanie przełącznika w portalu lub wywołań interfejsu API REST. W portalu kliknij **konfiguracji** karcie dla witryny, a następnie przewiń w dół aby zobaczyć **programu Application Diagnostics** sekcji. Możesz Włączanie lub wyłączanie rejestrowania i wybierz żądany poziom rejestrowania. Program może zapisać w dziennikach systemu plików lub konto magazynu Azure.

![Diagnostyka aplikacji i Diagnostyka lokacji na karcie Konfiguracja](monitoring-and-telemetry/_static/image24.png)

Po włączeniu rejestrowania na platformie Azure, dzienniki w oknie programu Visual Studio dane wyjściowe są widoczne, zgodnie z ich tworzenia.

![Menu Dzienniki przesyłania strumieniowego](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu Dzienniki przesyłania strumieniowego](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Może także zawierać dzienniki zapisywane na koncie magazynu i wyświetl je za pomocą dowolnego narzędzia, które mogą uzyskiwać dostęp do usługi tabel magazynu Azure, takich jak **Eksploratora serwera** w programie Visual Studio lub [Eksploratora usługi Storage Azure](https://azure.microsoft.com/features/storage-explorer/).

![Dzienniki w Eksploratorze serwera](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Podsumowanie

Jest naprawdę proste wdrożenie systemu poza pole telemetrii, Instrumentacja logowania w swoim własnym kodem i Konfiguruj rejestrowanie w usłudze Azure. A jeśli masz problemy produkcji kombinacja systemu telemetrii i dzienników niestandardowych pomoże Ci szybkie rozwiązywanie problemów, zanim staną się poważne problemy dla klientów.

W [następnego rozdziału](transient-fault-handling.md) wyjaśniono sposób obsługi błędów przejściowych, więc nie staną się problemy produkcji, które należy zbadać.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji zobacz następujące zasoby.

Dokumentacja głównie o telemetrii:

- [Microsoft Patterns and Practices - Azure wskazówki](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki Instrumentacji i dane telemetryczne, zliczania usługi wskazówki wzorzec monitorowania kondycji punktu końcowego i wzorzec ponownej konfiguracji środowiska wykonawczego.
- [Wartość punkty zaciskające w chmurze: Włączanie wydajności usługi New Relic na witryn sieci Web Azure monitorowanie](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę na usług w chmurze Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument przez moduły SIMM znaku i Michael Thomassy. Zobacz sekcję Telemetrii i informacji diagnostycznych.
- [Tworzenie nowej generacji za pomocą usługi Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artykuł MSDN Magazine.

Dokumentacja przede wszystkim dotyczące rejestrowania:

- [Bloku aplikacji rejestrowania semantyczne (PŁYCIE)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). W przypadku rejestrowania semantycznego z płyty przedstawia informacje o Neil Mackenzie.
- [Tworzenie strukturalnych i łatwy do rozpoznania dzienniki z rejestrowaniem semantycznego](https://channel9.msdn.com/Events/Build/2013/3-336). (Klip wideo) W przypadku rejestrowania semantycznego z płyty przedstawia informacje o Dominguez juliański.
- [EF6 Rejestrowanie SQL — część 1: rejestrowanie proste](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers pokazano, jak dziennika zapytań wykonywanych przez Entity Framework w EF 6.
- [Elastyczność połączenia i przechwytywaniu polecenia Entity Framework w aplikacji platformy ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Czwarty w serii samouczek dziewięć części pokazano, jak na potrzeby funkcji EF 6 polecenie zatrzymania rejestrowania poleceń SQL wysyłane do bazy danych przez program Entity Framework.
- [Poprawa rejestrowanie przy użyciu języka C# w wersji 5.0 Caller — atrybuty informacji](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Jak rejestrowanie nazwa wywołania metody bez kodować je w literałach lub pobrać go ręcznie za pomocą odbicia.

Dokumentacja przede wszystkim dotyczące rozwiązywania problemów:

- [Rozwiązywanie problemów Azure &amp; debugowania blogu](https://blogs.msdn.com/b/kwill/).
- [AzureTools — diagnostycznych narzędzie używane przez zespół pomocy technicznej Developer Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Wprowadzono i udostępnia łącza pobierania narzędzia, który pozwala na maszynie Wirtualnej platformy Azure Pobierz i uruchom szeroką gamę narzędzi diagnostycznych i monitorowania. Przydatne, gdy potrzebne do diagnozowania problemu na określonej maszynie Wirtualnej.
- [Rozwiązywanie problemów z aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Samouczek krok po kroku na temat rozpoczynania pracy z System.Diagnostics śledzenia i debugowania zdalnego.

Filmy wideo:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Przedstawia informacje o szczegółowo pojęcia i architektury zasad w sposób bardzo dostępny i interesujące z wątków z doświadczenia zespół Advisory klienta firmy Microsoft (CAT) z konkretnymi klientami. Odcinki 4 i 9 są dotyczących monitorowania i danych telemetrycznych. Odcinek 9 zawiera omówienie monitorowania usług MetricsHub, AppDynamics usługi New Relic i PagerDuty.
- [Kompilowanie Big: Wnioski uzyskane od klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). Oznacz moduły SIMM wspomniana projektowanie pod kątem awarii i Instrumentacja wszystko. Podobnie jak seria przed uszkodzeniami, ale przechodzi w szczegółowe instrukcje.

Przykładowy kod:

- [Podstawy usługi na platformie Azure w chmurze](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja utworzone przez zespół doradczy klientów firmy Microsoft Azure. Pokazuje zarówno dane telemetryczne, jak i rejestrowania rozwiązania, zgodnie z objaśnieniem w następujących artykułach. Przykład implementuje rejestrowanie aplikacji przy użyciu [NLog](http://nlog-project.org/). Dokumentacja pokrewna dla [serii cztery artykułach typu wiki w witrynie TechNet o telemetrii i rejestrowanie](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Poprzednie](design-to-survive-failures.md)
> [dalej](transient-fault-handling.md)
