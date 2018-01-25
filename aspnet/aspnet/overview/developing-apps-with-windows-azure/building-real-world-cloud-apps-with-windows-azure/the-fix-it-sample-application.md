---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: "Dodatek: Poprawkę go aplikacji przykładowej (Tworzenie aplikacji w chmurze rzeczywistych z platformy Azure) | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: c98e79bf8e9a1fe0899ed6d952c3e411ca472f7e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Dodatek: Poprawkę go aplikacji przykładowej (Tworzenie aplikacji w chmurze rzeczywistych z platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz napraw go projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Ten dodatek do tworzenia rzeczywistych aplikacji w chmurze z Azure e-book zawiera następujące sekcje, które zapewniają dodatkowe informacje na temat poprawka przykładowej aplikacji, którą można pobrać:

- [Znane problemy](#knownissues)
- [Najlepsze praktyki](#bestpractices)
- [Jak uruchomić aplikację w programie Visual Studio na komputerze lokalnym](#run-in-vs)
- [Sposób wdrażania podstawowej aplikacji do aplikacji sieci Web usługi aplikacji Azure za pomocą skryptów środowiska Windows PowerShell](#deploybase)
- [Rozwiązywanie problemów w skryptach środowiska Windows PowerShell](#troubleshooting)
- [Jak wdrożyć aplikację z kolejką przetwarzania do aplikacji sieci Web usługi aplikacji Azure i usługi w chmurze platformy Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Znane problemy

Aby zilustrować ułatwianiu niektóre wzorce przedstawionych w tym Książka elektroniczna opracowany z aplikacji Usuń. Jednak ponieważ Książka elektroniczna o tworzeniu aplikacji rzeczywistych, możemy poddane kodu napraw przeglądu i proces testowania, podobnie jak co możemy sposób jak wydanego oprogramowania. Znaleziono wiele problemów, a tak jak w przypadku dowolnej aplikacji rzeczywistych niektóre z nich usunięto i niektóre z nich, możemy odłożone do nowszej wersji.

Poniżej opisano problemy, które powinny być kierowane w aplikacji produkcyjnej, ale w jednym z powodów lub innego zdecydowaliśmy się nie na adres w wersji początkowej poprawka przykładowej aplikacji.

### <a name="security"></a>Zabezpieczenia

- Upewnij się, że nie można przypisać zadania do właściciela nie istnieje.
- Upewnij się, że można tylko przeglądać i modyfikować zadania utworzone lub są przypisane do Ciebie.
- Stron logowania i plików cookie uwierzytelniania jest używany protokół HTTPS.
- Określ limit czasu plików cookie uwierzytelniania.

### <a name="input-validation"></a>Sprawdzania poprawności danych wejściowych

Ogólnie rzecz biorąc, sposób jak aplikacji produkcyjnej więcej wejściowych niż aplikacji Usuń sprawdzania poprawności. Przykładowo rozmiar obrazu / dozwoloną dla przekazywania powinna być ograniczona rozmiar pliku obrazu.

### <a name="administrator-functionality"></a>Administrator funkcji

Administrator powinien móc zmienić własności na istniejące zadania. Na przykład twórca zadania mogą pozostać firmy, pozostawiając nikt z urzędem, aby zachować zadanie, jeśli nie włączono dostępu administracyjnego.

### <a name="queue-message-processing"></a>Przetwarzanie komunikatów z kolejki

Komunikat z kolejki przetwarzania w aplikacji Usuń został opracowany jako proste w celu zilustrowania wzorzec skoncentrowane kolejki pracy z minimalną ilością kodu. Ten prosty kod nie są odpowiednie dla aplikacji rzeczywistej produkcji.

- Kod nie gwarantuje co najwyżej raz przetwarzania każdego komunikatu w kolejce. Gdy pojawi się komunikat z kolejki, istnieje limit czasu, w którym jest niewidoczny dla innych odbiorników kolejki wiadomości. Limit czasu wygaśnięcia przed usunięciem komunikatu, komunikat staje się widoczna ponownie. W związku z tym jeśli wystąpienie roli procesu roboczego zużywa dużo czasu przetwarzania komunikatu, prawdopodobnie teoretycznie dla tego samego komunikatu na przetworzenie dwa razy, co zduplikowane zadania w bazie danych. Aby uzyskać więcej informacji na temat tego problemu, zobacz [przy użyciu kolejki magazynu Azure](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Logika sondowania kolejki może być bardziej ekonomiczne rozwiązanie przez przetwarzanie wsadowe pobieranie wiadomości. Za każdym razem, gdy jest wywoływana [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), brak kosztów transakcji. Zamiast tego można wywołać [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (należy zwrócić uwagę na liczbę mnogą "), która pobiera wiele komunikatów w ramach jednej transakcji. Koszty transakcji dla kolejek magazynu Azure są bardzo niskie, więc nie jest znaczny, w większości przypadków wpływ na koszty.
- Ścisłej pętli w kodzie przetwarzania komunikatów kolejki powoduje, że koligacji procesora CPU, który nie korzysta z maszyn wirtualnych w wielordzeniowych wydajnie. Lepszego projektowania użyje równoległość zadań, aby równolegle wielu zadań asynchronicznych.
- Przetwarzanie komunikatów w kolejce ma tylko podstawowe wyjątków. Na przykład kod nie obsługuje [zanieczyszczonych komunikatów](https://msdn.microsoft.com/library/ms789028.aspx). (Podczas przetwarzania komunikatu powoduje zgłoszenie wyjątku, trzeba błąd i usuwanie wiadomości, lub roli proces roboczy spróbuje ponownie przetworzyć, i pętli będzie nieskończoność.)

### <a name="sql-queries-are-unbounded"></a>Zapytania SQL są niepowiązanego

Poprawka kodu bieżącej umieszcza limitu na liczbę wierszy, które mogą zwracać zapytania dla indeksu strony. W przypadku dużej liczby zadań jest wprowadzany do bazy danych, rozmiaru list Odebrano może spowodować problemy z wydajnością. To rozwiązanie do implementowania stronicowania. Na przykład zobacz [sortowanie, filtrowanie i stronicowania Entity Framework w aplikacji platformy ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Wyświetlanie modeli zalecane

Aplikacja napraw używa klasy jednostki FixItTask do przekazywania informacji między kontrolerem a widokiem. Najlepszym rozwiązaniem jest używanie modeli widoku. Model domeny (np. Klasa jednostki FixItTask) jest zaprojektowana dla potrzebna trwałości danych, gdy model widoku mogą służyć do prezentacji danych. Aby uzyskać więcej informacji, zobacz [12 ASP.NET MVC: najlepsze rozwiązania](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Obiekt blob obrazu bezpiecznego zalecane

Poprawka przechowywanych w aplikacji przekazany jako public, obrazy, co oznacza, że każda osoba, która znajdzie adres URL dostępu obrazów. Obrazy można zabezpieczone zamiast publicznego.

### <a name="no-powershell-automation-scripts-for-queues"></a>Nie skryptów automatyzacji programu PowerShell dla kolejek

Przykładowe skrypty automatyzacji środowiska PowerShell zostały napisane tylko dla wersja podstawowa, usuń go całkowicie w aplikacji sieci Web usługi aplikacji Azure. Nie podaliśmy skryptów do instalowania i wdrażania aplikacji sieci web oraz usługi w chmurze środowiska wymaganej do przetwarzania kolejki.

### <a name="special-handling-for-html-codes-in-user-input"></a>Obsługa specjalnej kodów HTML w danych wejściowych użytkownika

ASP.NET jest automatycznie zapobiega wiele sposobów, w których złośliwych użytkowników może podejmować atakami skryptów między witrynami, wprowadzając skrypt w polach tekstowych wejściowych użytkownika. I MVC `DisplayFor` pomocnika używany do wyświetlania zadań tytuły i uwagi dotyczące automatycznie koduje HTML wartości, które wysyła do przeglądarki. Jednak w aplikacji produkcyjnej warto zastosować dodatkowe środki. Aby uzyskać więcej informacji, zobacz [żądania weryfikacji w programie ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Najlepsze praktyki

Poniżej przedstawiono niektóre problemy, które zostały usunięte po odnalezione Przegląd kodu i testowania z oryginalną wersją aplikacji rozwiązać. Niektóre zostały spowodowane przez koder oryginalnej, nie wiedziały określonego najlepszych praktyk, niektóre po prostu ponieważ kod został napisany szybko i nie był przeznaczony dla oprogramowania. Firma Microsoft jest lista problemy, w tym miejscu, w przypadku braku coś, co opisano w tym przeglądzie i testowania, które mogą być przydatne do innych użytkowników, którzy są również tworzenie aplikacji sieci web.

### <a name="dispose-the-database-repository"></a>Usuwanie bazy danych repozytorium

`FixItTaskRepository` Klasy musi dysponować programu Entity Framework `DbContext` wystąpienia. Robiliśmy to zaimplementowanie `IDisposable` w `FixItTaskRepository` klasy:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Należy pamiętać, że automatycznie zlikwiduje AutoFac `FixItTaskRepository` wystąpienie, dlatego nie należy go jawnie usunąć.

Innym rozwiązaniem jest, aby usunąć `DbContext` zmiennej członkowskiej z `FixItTaskRepository`i zamiast tego utworzyć lokalnego `DbContext` zmiennej w ramach każdej metody repozytorium wewnątrz `using` instrukcji. Na przykład:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Zarejestruj pojedynczych wystąpień w związku z Podpisane

Ponieważ tylko jedno wystąpienie `PhotoService` klasy i `Logger` klasy jest potrzebna, te klasy powinny być [zarejestrowany jako pojedynczego wystąpienia iniekcji zależności](https://code.google.com/p/autofac/wiki/InstanceScope) w *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Zabezpieczenia: Nie pokazuj szczegóły błędu dla użytkowników

Oryginalny poprawka aplikacji strona ogólny błąd i nie zostały tylko let wszystkie wyjątki bąbelków do interfejsu użytkownika, więc niektóre wyjątki, takie jak błędów połączenia bazy danych może spowodować ślad stosu pełne, będzie wyświetlany w przeglądarce. Szczegółowe informacje o błędzie czasami mogą ułatwić atakami złośliwych użytkowników. Rozwiązanie jest dziennika szczegóły wyjątku i wyświetlić stronę błędu dla użytkownika, który nie zawiera szczegóły błędu. Już logowania aplikacji Usuń, a aby wyświetlić stronę błędu, dodaliśmy `<customErrors mode=On>` w pliku Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Domyślnie powoduje to *Views\Shared\Error.cshtml* ma być wyświetlany dla błędów. Można dostosować *Error.cshtml* lub utworzyć własne widoku strony błędów i dodanie `defaultRedirect` atrybutu. Można również określić inny błąd strony dla określonych błędów.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Zabezpieczenia: Zezwalaj tylko zadania, które ma być edytowany przez twórcy

Strona indeksu pulpit nawigacyjny zawiera tylko zadania utworzone przez zalogowanego użytkownika, ale złośliwy użytkownik może utworzyć adres URL z Identyfikatorem zadania przez innego użytkownika. Dodano kod w *DashboardController.cs* do zwrócenia 404 w takiej sytuacji:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Nie swallow wyjątków

Oryginalna poprawka aplikacji tylko zwróciła wartość null po zalogowaniu wyjątek, który jest wynikiem kwerendy SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Może to być wyświetlany dla użytkownika tak, jakby zapytanie zostało ukończone pomyślnie, ale po prostu nie zwróciła żadnych wierszy. Rozwiązanie to ponowne zgłoszenie wyjątku po przechwytywanie i rejestrowanie:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Przechwytywanie wszystkich wyjątków w roli proces roboczy

Wszelkie nieobsługiwanych wyjątków w roli procesu roboczego spowoduje, że maszyna wirtualna zostanie poddanych recyklingowi, więc ma być zawijany wszystko, co zrobić w bloku try-catch i obsługiwać wszystkie wyjątki.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Określ długość ciągu właściwości klas jednostki

Aby wyświetlić kod proste, z oryginalną wersją aplikacji Usuń nie określ długość pola jednostki FixItTask, a w związku z tym zostały zdefiniowane jako varchar(max) w bazie danych. W związku z tym interfejsu użytkownika będzie akceptować prawie każdego ilość danych wejściowych. Określanie limity zestawów długości, które mają zastosowanie zarówno do użytkownika danych wejściowych w strony sieci web i rozmiar kolumny w bazie danych:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Oznacz prywatne elementy członkowskie jako tylko do odczytu, gdy nie są one oczekiwane zmiany

Na przykład w `DashboardController` wystąpienia klasy `FixItTaskRepository` zostanie utworzona i nie jest oczekiwany można zmienić, więc go jako zdefiniowanego [tylko do odczytu](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Użyj listy. Any() zamiast listy. Funkcji Count() &gt; 0

Jeśli użytkownik Cię interesuje czy jeden lub więcej elementów na liście dopasowania określone kryteria, użyj [żadnych](https://msdn.microsoft.com/library/bb534972.aspx) metody, ponieważ zwraca ono jak kryteria dopasowywania element zostanie znaleziony, podczas gdy `Count` iteracyjne ma zawsze — metoda do każdego elementu. Pulpit nawigacyjny *Index.cshtml* pliku była pierwotnie ten kod:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Możemy zmienić do poniższego:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generowania adresów URL w widoków MVC za pomocą pomocników MVC

Dla **napraw go utworzyć** przycisk na stronie głównej poprawka aplikacji twardych kodowanego element zakotwiczenia:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Dla łącza widoku/akcji, takich jak to jest lepiej jest użyć [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) pomocnika kodu HTML, na przykład:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Użyj Task.Delay zamiast Thread.Sleep w roli procesu roboczego

Nowy projekt szablonu umieszcza `Thread.Sleep` w próbce kod roli proces roboczy, ale powoduje wątku w stan uśpienia może spowodować puli wątków uruchomić dodatkowe wątki niepotrzebne. Można uniknąć który za pomocą [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) zamiast tego.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Unikaj async void

W przypadku metoda asynchroniczna nie musi zwracać wartości, zwraca `Task` typu zamiast `void`.

W tym przykładzie pochodzi z `FixItQueueManager` klasy: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Należy używać `async void` tylko dla programów obsługi zdarzeń najwyższego poziomu. W przypadku definiowania metodę jako `async void`, obiekt wywołujący nie **await** metody lub przechwycić wszelkie wyjątki, metoda wygeneruje. Aby uzyskać więcej informacji, zobacz [programowania asynchronicznych — najlepsze praktyki](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Użyj token anulowania, aby przerwać pętlę roli procesu roboczego

Zazwyczaj **Uruchom** metody w roli procesu roboczego zawiera Pętla nieskończona. Gdy rola proces roboczy jest zatrzymywana, [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) metoda jest wywoływana. Należy użyć tej metody do anulowania pracy, która jest wykonywana wewnątrz **Uruchom** metody i wyjścia bezpiecznie zamknąć. W przeciwnym razie proces mogą być zakończone w trakcie wykonywania operacji.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Opcja rezygnacji z procedury automatyczne wykrywanie MIME

W niektórych przypadkach program Internet Explorer raporty typ MIME jest inny niż typ określony przez serwer sieci web. Na przykład jeśli program Internet Explorer znajdzie HTML zawartości w pliku oferowane przez nagłówka odpowiedzi HTTP Content-Type: zwykły tekst, program Internet Explorer określa, czy zawartość ma być renderowany jako HTML. Niestety ta "— Wykrywanie MIME" również może prowadzić do problemów z zabezpieczeniami dla serwerów obsługujących niezaufaną zawartość. Zwalczania ten problem, programu Internet Explorer 8 ma wprowadzono wiele zmian kodu określenie typu MIME i umożliwia deweloperom aplikacji [zrezygnować z wykrywanie MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Następujący kod został dodany do *Web.config* pliku.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Włącz tworzenie pakietów i minimalizowanie

Gdy program Visual Studio tworzy nowy projekt sieci web, tworzenie pakietów i minimalizowanie pliki JavaScript nie jest włączona domyślnie. Dodaliśmy wiersza kodu w BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Ustawianie limitu czasu wygaśnięcia uwierzytelniania plików cookie

Domyślnie pliki cookie uwierzytelniania wygaśnie po upływie dwóch tygodni. Krótszy czas jest bardziej bezpieczne. Możesz zmienić to ustawienie w *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Jak uruchomić aplikację w programie Visual Studio na komputerze lokalnym

Istnieją dwa sposoby uruchamiania aplikacji napraw:

- Uruchom podstawowej aplikacji, który zapisuje nowe zadania bezpośrednio do bazy danych SQL.
- Uruchom aplikację przy użyciu kolejki i usługi wewnętrznej bazy danych do utworzenia zadania. Wzorzec kolejki opisanej w rozdziale [skoncentrowane kolejki wzorzec pracy](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Uruchom podstawowej aplikacji

1. Zainstaluj [Visual Studio 2013 lub Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads).
2. Zainstaluj [Azure SDK dla platformy .NET dla programu Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Pobierz plik zip z [galerii kodu MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. W Eksploratorze plików kliknij prawym przyciskiem myszy plik zip i kliknij polecenie Właściwości, a następnie w oknie dialogowym właściwości kliknij przycisk Odblokuj.
5. Rozpakuj plik.
6. Kliknij dwukrotnie plik .sln, aby uruchomić program Visual Studio.
7. W menu Narzędzia kliknij pozycję Menedżer pakietów biblioteki, a następnie Konsola Menedżera pakietów.
8. W konsoli Menedżera pakietów (PMC), kliknij przycisk Przywróć.
9. Zamknij program Visual Studio.
10. Uruchom [emulatora magazynu Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Uruchom ponownie program Visual Studio, otwierając plik rozwiązania zamknięte w poprzednim kroku.
12. Upewnij się, że projekt automatyczne jest ustawiony jako projekt startowy, a następnie naciśnij klawisze CTRL + F5, aby uruchomić projekt.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Uruchom aplikację z kolejki przetwarzania

1. Postępuj zgodnie z instrukcjami dla [Uruchom podstawowej aplikacji](#runbase), a następnie zamknij przeglądarkę i zamknij program Visual Studio.
2. Uruchom program Visual Studio z uprawnieniami administratora. (Należy używać emulatora obliczeń platformy Azure, i który wymaga uprawnień administratora).
3. W aplikacji *Web.config* w pliku *MyFixIt* projekt (projekt sieci web), zmień wartość `appSettings/UseQueues` na wartość "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Jeśli [emulatora magazynu Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) nie jest nadal uruchomiona, uruchom go ponownie.
5. Automatyczne projektu sieci web i projektu MyFixItCloudService być uruchomione jednocześnie.

    Za pomocą programu Visual Studio 2013:

    1. Naciśnij klawisz F5, aby uruchomić projekt automatyczne rozwiązywanie problemu.
    2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt MyFixItCloudService, a następnie kliknij przycisk **debugowania** -- **uruchomić nowe wystąpienie**.

    Za pomocą programu Visual Studio Express 2013 for Web:

    1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie automatyczne, a następnie wybierz **właściwości**.
    2. Wybierz **wiele projektów startowych**...
    3. W **akcji** wybierz z listy rozwijanej w obszarze MyFixIt i MyFixItCloudService, **Start**.
    4. Kliknij przycisk **OK**.
    5. Naciśnij klawisz F5, aby uruchomić oba projekty.

    Po uruchomieniu MyFixItCloudService projektu programu Visual Studio rozpoczyna emulatora obliczeń platformy Azure. W zależności od konfiguracji zapory konieczne może być Zezwalaj emulatora przez zaporę.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Sposób wdrażania podstawowej aplikacji do aplikacji sieci Web usługi aplikacji Azure za pomocą skryptów środowiska Windows PowerShell

Aby zilustrować [zautomatyzować wszystko](automate-everything.md) wzorcu aplikacji Usuń jest dostarczany z skrypty, które konfigurowania środowiska na platformie Azure i wdrażanie projektu do nowego środowiska. Poniższe instrukcje wyjaśniają sposób użycia skryptów.

Jeśli chcesz uruchomić na platformie Azure, bez korzystania z kolejek, a zostały wprowadzone zmiany, aby uruchomić lokalnie z kolejkami, upewnij się, że wartość UseQueues appSetting ponownie na wartość false przed wykonaniem poniższych instrukcji.

W poniższych instrukcjach przyjęto zostały już pobrane i uruchamianie rozwiązania rozwiązać lokalnie, i czy masz Azure konta lub posiadania subskrypcji platformy Azure, które są autoryzowane do zarządzania.

1. Zainstaluj **programu Azure PowerShell** konsoli. Aby uzyskać instrukcje, zobacz [jak instalowanie i konfigurowanie programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Ta konsola dostosowane jest skonfigurowane do pracy z subskrypcją platformy Azure. Zainstalowano modułu platformy Azure w *Program Files* katalogu i jest importowany automatycznie w każdej za pomocą konsoli programu Azure PowerShell.

    Jeśli wolisz pracować w programie inny host, na przykład programu Windows PowerShell ISE, należy użyć [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) polecenia cmdlet, aby zaimportować moduł Azure lub użyj polecenia w Azure module do wyzwalania automatycznego importowania modułu.
2. Uruchom program Azure PowerShell z **Uruchom jako administrator** opcji.
3. Uruchom [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) polecenia cmdlet, aby ustawić zasady wykonywania programu Azure PowerShell `RemoteSigned`. Wprowadź **Y** (tak) w przypadku aby dokonać zmiany zasad.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    To ustawienie umożliwia uruchamianie skryptów lokalnych, które nie są podpisane cyfrowo. (Możesz także ustawić zasady wykonywania `Unrestricted`, który będzie, eliminując konieczność stosowania krok Odblokuj później, ale nie jest to zalecane ze względów bezpieczeństwa.)
4. Uruchom `Add-AzureAccount` polecenia cmdlet, aby skonfigurować programu PowerShell przy użyciu poświadczeń dla konta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Te poświadczenia wygaśnie po upływie pewnego czasu i ponownie uruchom `Add-AzureAccount` polecenia cmdlet. Jak Książka elektroniczna jest zapisywana, limit czasu wygaśnięcia poświadczeń wynosi 12 godzin.
5. Jeśli masz wiele subskrypcji, należy użyć polecenia cmdlet AzureSubscription wybierz, aby określić subskrypcję, którą chcesz utworzyć w środowisku testowym.
6. Zaimportuj certyfikat zarządzania dla tej samej subskrypcji platformy Azure przy użyciu `Get-AzurePublishSettingsFile` i `Import-AzurePublishSettingsFile` polecenia cmdlet. Pierwsza z tych poleceń cmdlet pobiera plik certyfikatu, a w drugim Określ lokalizację tego pliku w celu zaimportowania. > [!IMPORTANT]
 > Zachować pobrany plik w bezpiecznym miejscu, lub usuń go, gdy wszystko będzie gotowe, ponieważ zawiera on certyfikat, który może służyć do zarządzania usługami Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Ten certyfikat jest używany dla wywołania interfejsu API REST, który wykryje adresu IP na komputerze deweloperskim, aby ustawić regułę zapory na serwerze bazy danych SQL.
7. Uruchom [lokalizacją zestawu](https://go.microsoft.com/fwlink/p/?linkid=293912) polecenia cmdlet (aliasy `cd`, `chdir`, i `sl`) przejdź do katalogu zawierającego skryptów. (Znajdują w *automatyzacji* folderu w folderze rozwiązania poprawka.) Jeśli którakolwiek z nazwy katalogów zawiera spacje, umieść ścieżki w cudzysłowy. Na przykład, aby przejść do `c:\Sample Apps\FixIt\Automation` katalogu można wprowadzić następujące polecenie:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Aby zezwolić na uruchamianie tych skryptów środowiska Windows PowerShell, użyj [odblokowanie pliku](https://go.microsoft.com/fwlink/p/?linkid=294021) polecenia cmdlet. (Skrypty są blokowane, ponieważ zostały one pobrane z Internetu.)

    > [!WARNING]
    > Zabezpieczenia — przed uruchomieniem `Unblock-File` na dowolny skrypt lub plik wykonywalny, otwórz plik w programie Notatnik, sprawdź poleceń i sprawdź, czy nie zawierają złośliwego kodu.

    Na przykład następujące polecenie uruchamia `Unblock-File` polecenia cmdlet na wszystkie skrypty w bieżącym katalogu.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Do utworzenia aplikacji sieci web dla podstawy (kolejek przetwarzania) Napraw aplikację, uruchom skrypt tworzenia środowiska.

    Wymagane `Name` parametr określa nazwę bazy danych, a także jest używane dla konta magazynu, która tworzy skrypt. Nazwa musi być globalnie unikatowa w domenie azurewebsites.net. Jeśli określono nazwę, która nie jest unikatowa, takich jak automatyczne lub testu (lub nawet tak jak w przykładzie, fixitdemo) `New-AzureWebsite` polecenie cmdlet nie powiedzie się z powodu błędu wewnętrznego, który zgłasza konflikt. Skrypt konwertuje nazwę napisany małymi literami, zgodnie z wymaganiami nazwę dla aplikacji sieci web, konta magazynu i bazy danych.

    Wymagane `SqlDatabasePassword` parametr określa hasło dla konta administratora, które zostaną utworzone dla bazy danych SQL. Hasło nie zawiera specjalne znaki XML (&amp; &lt; &gt; ;). Jest to ograniczenie sposób skrypty napisanych nie ograniczenia Azure.

    Jeśli chcesz utworzyć aplikację sieci web o nazwie "fixitdemo" i użyj hasła administratora serwera SQL "Passw0rd1", można na przykład, wprowadź następujące polecenie:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Nazwa musi być unikatowa w domenie azurewebsites.net, a hasło musi spełniać wymagania bazy danych SQL dla złożoności hasła. (Przykład Passw0rd1 spełnia wymagań).

    Należy pamiętać, że polecenie rozpoczyna się od ". \". Aby zapobiec złośliwego wykonywania skryptów, programu Windows PowerShell wymaga podać w pełni kwalifikowana ścieżka do pliku skryptu w przypadku uruchamiania skryptu. Można użyć pojedynczego znaku kropki wskazuje bieżący katalog (".\") lub podaj pełną ścieżkę, takich jak:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Aby uzyskać więcej informacji na temat skryptu, użyj `Get-Help` polecenia cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Można użyć `Detailed`, `Full`, `Parameters`, i `Examples` parametry polecenia cmdlet Get-Help, aby filtrować pomocy, która jest zwracana.

    Jeśli skrypt zakończy się niepowodzeniem lub generuje błędy, takie jak "AzureWebsite nowy: wywołanie AzureSubscription zestawu i wybierz AzureSubscription najpierw" nie została ukończona konfiguracji programu Azure PowerShell.

    Po zakończeniu działania skryptu, aby wyświetlić zasoby, które zostały utworzone, jak pokazano w można użyć portalu zarządzania Azure [zautomatyzować wszystko](automate-everything.md) działu.
10. Aby wdrożyć projekt automatyczne nowego środowiska platformy Azure, użyj *AzureWebsite.ps1* skryptu. Na przykład:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Po zakończeniu wdrażania przeglądarce zostanie otwarty z napraw go, działające na platformie Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Rozwiązywanie problemów w skryptach środowiska Windows PowerShell

Typowe błędy podczas uruchamiania tych skryptów są powiązane uprawnienia. Upewnij się, że `Add-AzureAccount` i `Import-AzurePublishSettingsFile` zostały pomyślnie i używać tej samej subskrypcji platformy Azure. Nawet jeśli `Add-AzureAccount` został pomyślnie trzeba będzie uruchomić go ponownie. Uprawnienia dodane przez `Add-AzureAccount` wygaśnie w ciągu 12 godzin.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Odwołanie do obiektu nie jest ustawione na wystąpienie obiektu.

Jeśli skrypt zwraca błędy, takie jak "Odwołanie do obiektu nie jest ustawione na wystąpienie obiektu" co oznacza, że programu Windows PowerShell nie można odnaleźć obiektu do procesu (jest to wyjątek odwołania zerowego), uruchom `Add-AzureAccount` polecenia cmdlet i spróbuj ponownie skrypt.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Serwer napotkał błąd wewnętrzny.

`New-AzureWebsite` Polecenie cmdlet zwraca błąd, gdy nazwa nie jest unikatowa w domenie azurewebsites.net. Aby rozwiązać problem, użyj innej wartości dla nazwy, która jest parametr Name *AzureWebsiteEnv.ps1 nowy*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Ponowne uruchomienie skryptu

Jeśli zachodzi potrzeba ponownego uruchomienia *AzureWebsiteEnv.ps1 nowy* skryptu ponieważ nie powiodło się przed wydrukowaniem go komunikat "Skryptu została zakończona", może chcesz usunąć zasoby, które zatrzymana skryptu przed go utworzyć. Na przykład jeśli skrypt jest już utworzony ContosoFixItDemo aplikacji sieci web i ponownie uruchomić skrypt o tej samej nazwie, skrypt zakończy się niepowodzeniem, ponieważ nazwa jest używana.

Aby określić zasoby skryptu utworzonego przed został przerwany, użyj następujących poleceń cmdlet:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Aby uruchomić to polecenie cmdlet, należy przekazać nazwę serwera bazy danych, aby `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Aby usunąć te zasoby, użyj następujących poleceń. Pamiętaj, że po usunięciu serwera bazy danych zostanie automatycznie usunięcia skojarzone z serwerem bazy danych.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Jak wdrożyć aplikację z kolejką przetwarzania do aplikacji sieci Web usługi aplikacji Azure i usługi w chmurze platformy Azure

Aby włączyć kolejek, wprowadź następujące zmiany w pliku MyFixIt\Web.config. W obszarze `appSettings`, zmień wartość `UseQueues` na wartość "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Następnie wdrażanie aplikacji MVC dla aplikacji sieci web w usłudze Azure App Service, zgodnie z opisem [wcześniejszych](#deploybase).

Następnie należy utworzyć nową usługę w chmurze Azure. Skrypty dołączony aplikacji Usuń nie tworzenia lub wdrażania usługi w chmurze, to trzeba używać portalu Azure. W portalu kliknij **nowy** -- **obliczeniowe** — **usługi w chmurze** -- **szybkie tworzenie**, a następnie wprowadź adres URL i lokalizację centrum danych. Użyj tego samego centrum danych, której została wdrożona aplikacja sieci web.

![](the-fix-it-sample-application/_static/image1.png)

Przed wdrożeniem usługi w chmurze, musisz zaktualizować niektóre pliki konfiguracji.

W MyFixIt.WorkerRoler\app.config w obszarze `connectionStrings`, zastąp wartość `appdb` ciągu połączenia za pomocą rzeczywistych parametrów połączenia z bazą danych SQL. Parametry połączenia można pobrać z portalu. W portalu kliknij **baz danych SQL** - **appdb** - **parametry połączenia bazy danych SQL widoku ADO .net, PHP, ODBC i JDBC**. Skopiuj parametry połączenia ADO.NET i Wklej wartości w pliku app.config. Zastąp "{Twojej\_hasło\_tutaj}" z hasła bazy danych. (Zakładając, że skrypty są używane do wdrażania aplikacji MVC, określić hasło w `SqlDatabasePassword` skryptu parametru.)

Wynik powinien wyglądać następująco:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

W tym samym pliku MyFixIt.WorkerRoler\app.config w obszarze `appSettings`, Zastąp dwóch wartości symbolu zastępczego dla konta magazynu platformy Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Klucz dostępu można pobrać z portalu. Zobacz [jak zarządzać kontami magazynu](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

W MyFixItCloudService\ServiceConfiguration.Cloud.cscfg Zastąp tego samego dwóch wartości zastępcze dla konta magazynu platformy Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Teraz można przystąpić do wdrażania usługi w chmurze. W zapoznać się z rozwiązania, kliknij prawym przyciskiem myszy projekt MyFixItCloudService, a następnie wybierz **publikowania**. Aby uzyskać więcej informacji, zobacz "[wdrażanie aplikacji w usłudze Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", która znajduje się w części 2 [w tym samouczku](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

>[!div class="step-by-step"]
[Poprzednie](more-patterns-and-guidance.md)
