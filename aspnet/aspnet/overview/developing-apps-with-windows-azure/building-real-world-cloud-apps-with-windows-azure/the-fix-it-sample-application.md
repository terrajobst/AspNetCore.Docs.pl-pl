---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Dodatek: Rozwiązać go przykładowej aplikacji (Tworzenie aplikacji w chmurze w rzeczywistych warunkach Dzięki platformie Azure) | Dokumentacja firmy Microsoft'
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: baf46a87155e6368d9a81c5c5b777a491117d7b6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754356"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Dodatek: Rozwiązać go przykładowej aplikacji (Tworzenie aplikacji w chmurze w rzeczywistych warunkach Dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę go projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Ten dodatek do tworzenia rzeczywistych aplikacji w chmurze za pomocą platformy Azure e-book, zawiera następujące sekcje, które zapewniają dodatkowe informacje na temat rozwiązać go przykładową aplikację, którą można pobrać:

- [Znane problemy](#knownissues)
- [Najlepsze rozwiązania](#bestpractices)
- [Jak uruchomić aplikację w programie Visual Studio na komputerze lokalnym](#run-in-vs)
- [Sposób wdrażania podstawowej aplikacji w usłudze Azure App Service Web Apps za pomocą skryptów środowiska Windows PowerShell](#deploybase)
- [Rozwiązywanie problemów z skryptów programu Windows PowerShell](#troubleshooting)
- [Jak wdrożyć aplikację za pomocą kolejki przetwarzania do usługi Azure App Service Web Apps i usługi w chmurze platformy Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Znane problemy

Aplikacja naprawić pierwotnie został opracowany w celu zilustrowania ułatwianiu niektóre wzorce znajdujące się w tej książce elektronicznej. Jednak ponieważ książki elektronicznej dotyczących tworzenia aplikacji w rzeczywistych warunkach, firma Microsoft poddane kodu naprawić przeglądu i proces testowania, podobnie jak co możemy jak w przypadku wydania oprogramowania. Znaleźliśmy wiele problemów i podobnie jak w przypadku dowolnej aplikacji w rzeczywistych warunkach, niektóre z nich naprawiliśmy, a niektóre z nich, firma Microsoft odroczone do nowszej wersji.

Poniższa lista zawiera problemy, które powinny być kierowane w aplikacji produkcyjnej, ale w jednym z powodów lub innym, który firma zrezygnowała z adresu w początkowym wydaniu rozwiązać go przykładowej aplikacji.

### <a name="security"></a>Zabezpieczenia

- Upewnij się, że nie można przypisać zadanie do właściciela nieistniejące.
- Upewnij się, że można jedynie wyświetlać i modyfikować zadania utworzone lub są przypisane do użytkownika.
- Używać protokołu HTTPS dla stron logowania i pliki cookie uwierzytelniania.
- Określ limit czasu plików cookie uwierzytelniania.

### <a name="input-validation"></a>Walidacja danych wejściowych

Ogólnie rzecz biorąc, jak w aplikacji produkcyjnej weryfikacji więcej wejściowych niż aplikacja naprawić. Przykładowo rozmiar obrazu / image rozmiar pliku dozwolona w przypadku przekazywania powinny być ograniczone.

### <a name="administrator-functionality"></a>Administrator funkcji

Administrator powinien móc zmienić własności dla istniejących zadań. Na przykład osoba, która utworzyła zadanie może odejść z firmy, nie pozostawiając do dyspozycji urząd, aby obsługiwać zadania, jeśli nie włączono dostępu administracyjnego.

### <a name="queue-message-processing"></a>Przetwarzanie komunikatów w kolejce

Kolejki przetwarzania komunikatów w aplikacji naprawić zostało zaprojektowane jako prosty, aby zilustrować wzorzec pracy skoncentrowany na kolejkach, za pomocą minimalnej ilości kodu. Ten prosty kod nie będzie odpowiednia dla aplikacji rzeczywistej produkcji.

- Kod nie gwarantuje co najwyżej raz przetwarzania każdego komunikatu w kolejce. Gdy zostanie wyświetlony komunikat z kolejki, istnieje okres limitu czasu, przez który komunikat jest niewidoczne dla innych nasłuchujących kolejki. Limit czasu upłynie, zanim komunikat jest usuwany, komunikat staje się widoczna ponownie. W związku z tym jeśli wystąpienie roli procesu roboczego spędzony przez długi czas przetwarzania komunikatu, użytkownik może teoretycznie dla tej samej wiadomości są przetwarzane dwa razy, skutkuje zduplikowane zadania w bazie danych. Aby uzyskać więcej informacji na temat tego problemu, zobacz [za pomocą kolejek usługi Azure Storage](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Logika sondowania kolejki może być bardziej ekonomiczne, przetwarzanie wsadowe odebrania komunikatu. Za każdym razem, gdy wywołujesz [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), istnieje koszt transakcji. Zamiast tego możesz wywołać [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (należy pamiętać, plural "), która pobiera wiele komunikatów w ramach jednej transakcji. Koszty transakcji dla kolejek usługi Azure Storage są bardzo niskie, więc nie jest istotne w większości przypadków negatywny wpływ na koszty.
- Pętli w kodzie przetwarzanie komunikatów kolejki powoduje, że koligacje Procesora, który nie używa maszyn wirtualnych w wielordzeniowych wydajnie. Projekt lepiej użyć równoległość zadań do równoległego uruchamiania wielu zadań asynchronicznych.
- Przetwarzanie komunikatów w kolejce ma tylko podstawowe wyjątków. Na przykład, kod nie obsługuje [skażonymi komunikatami](https://msdn.microsoft.com/library/ms789028.aspx). (Podczas przetwarzania komunikatu, powoduje wyjątek, będzie konieczne błąd i usuwanie wiadomości, lub roli procesu roboczego podejmie próbę przetwarzać go ponownie i pętli będzie kontynuowane bez ograniczenia czasowego.)

### <a name="sql-queries-are-unbounded"></a>Zapytania SQL są nieograniczone

Poprawka kod bieżącej umieszcza bez ograniczeń na liczbę wierszy, które może zwrócić zapytania dotyczące strony indeksu. W przypadku dużej liczby zadań jest wprowadzany do bazy danych, rozmiar listy wynikowy otrzymane może spowodować problemy z wydajnością. To rozwiązanie jest implementowanie stronicowania. Aby uzyskać przykład, zobacz [sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Wyświetl modele zalecane

Aplikacja naprawić używa klasy jednostki FixItTask do przekazywania informacji między kontrolerem a widokiem. Najlepszym rozwiązaniem jest używanie modeli w widoku. Model domeny (np. Klasa jednostki FixItTask) została zaprojektowana wokół co jest potrzebne do stanu trwałego danych, gdy model widoku można zaprojektować tak, aby obejrzeć prezentację danych. Aby uzyskać więcej informacji, zobacz [12 platformy ASP.NET MVC: najlepsze rozwiązania](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Obiekt blob obrazu bezpieczny, zaleca się

Napraw go sklepów z aplikacjami przekazane obrazy jako publiczne, co oznacza, że każda osoba, która znajdzie adres URL mają dostęp do obrazów. Obrazy może zostać zabezpieczony zamiast publicznego.

### <a name="no-powershell-automation-scripts-for-queues"></a>Brak skryptów automatyzacji programu PowerShell dla kolejek

Przykładowe skrypty automatyzacji programu PowerShell zostały napisane tylko w przypadku wersji podstawowy napraw go, uruchamiana całkowite w usłudze Azure App Service Web Apps. Firma Microsoft nie zapewniały skrypty do konfigurowania i wdrażania aplikacji sieci web oraz usługi w chmurze środowisko wymaganej do kolejki przetwarzania.

### <a name="special-handling-for-html-codes-in-user-input"></a>Specjalna obsługa kody HTML w danych wejściowych użytkownika

Program ASP.NET automatycznie zapobiega wiele sposobów, w których złośliwych użytkowników może podejmować ataki z użyciem skryptów między witrynami, wprowadzając skryptu w polach tekstowych danych wejściowych użytkownika. I MVC `DisplayFor` pomocnika używany do wyświetlania zadań tytułów i informacje o automatycznie koduje HTML wartości, które wysyła do przeglądarki. Ale w aplikacji produkcyjnej warto zastosować dodatkowe środki. Aby uzyskać więcej informacji, zobacz [żądania weryfikacji w programie ASP.NET:](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Najlepsze rozwiązania

Poniżej przedstawiono niektóre problemy, które zostały rozwiązane po odnalezione w przeglądzie kodu i testowanie oryginalnej wersji aplikacji naprawić. Niektóre zostały spowodowane przez koder oryginalnej, nie wiedziały określonego najlepszych praktyk, niektóre po prostu ponieważ kod został napisany w szybko, a nie był przeznaczony dla wydania oprogramowania. Firma Microsoft jest lista problemów, w tym miejscu, w przypadku, gdy jest coś, czego dowiedzieliśmy się z tym przeglądzie, a testy, które mogą być pomocne dla innych użytkowników, którzy tworzą aplikacje sieci web.

### <a name="dispose-the-database-repository"></a>Usuń bazę danych repozytorium

`FixItTaskRepository` Klasy musi dysponować Entity Framework `DbContext` wystąpienia. Zrobiliśmy to zaimplementowanie `IDisposable` w `FixItTaskRepository` klasy:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Należy zauważyć, że automatycznie zlikwiduje AutoFac `FixItTaskRepository` wystąpienia, więc nie trzeba jawnie usuń go.

Innym rozwiązaniem jest usunięcie `DbContext` zmienną elementu członkowskiego z `FixItTaskRepository`i zamiast tego utworzyć lokalną `DbContext` zmiennej w ramach każdej metody repozytorium wewnątrz `using` instrukcji. Na przykład:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Zarejestrowanie pojedynczych elementów jako takie DI

Ponieważ tylko jedno wystąpienie `PhotoService` klasy i `Logger` klasy jest wymagana, te klasy powinny być [zarejestrowany jako pojedynczego wystąpienia do wstrzykiwania zależności](https://code.google.com/p/autofac/wiki/InstanceScope) w *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Zabezpieczenia: Nie pokazuj więcej szczegółów błędu dla użytkowników

Oryginalnej poprawka aplikacji nie mają strony ogólny błąd i po prostu umożliwiają wszystkich bąbelków wyjątków do interfejsu użytkownika, więc niektóre wyjątki, takie jak błędy połączenia bazy danych może spowodować pełen ślad stosu są wyświetlane w przeglądarce. Szczegółowe informacje o błędzie czasami mogą ułatwić atakami złośliwych użytkowników. Rozwiązanie jest rejestrować szczegóły wyjątku i wyświetlić stronę błędu dla użytkownika, nie dołączaj szczegóły błędu. Aplikacja naprawić został już rejestrowanie i aby wyświetlić stronę błędu, dodaliśmy `<customErrors mode=On>` w pliku Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Domyślnie powoduje to, że *Views\Shared\Error.cshtml* ma być wyświetlany dla błędów. Można dostosować *Error.cshtml* lub utworzyć własny widok strony błędów i dodanie `defaultRedirect` atrybutu. Można również określić strony różnych błędów dla określonych błędów.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Zabezpieczenia: Zezwalaj tylko na zadania do edycji przez jej twórcę

Strony indeksu pulpitu nawigacyjnego przedstawia tylko zadania utworzone przez zalogowanego użytkownika, ale złośliwy użytkownik może utworzyć adres URL o identyfikatorze zadania przez innego użytkownika. Dodaliśmy kod w *DashboardController.cs* zwrócić błąd 404 — w takiej sytuacji:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Nie wchłonąć wyjątków

Oryginalny poprawka aplikacji po prostu zwróciła wartość null po zalogowaniu się wyjątek, który jest wynikiem zapytania SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Dzięki temu upewnisz się to do użytkownika tak, jakby zapytania zakończyło się pomyślnie, ale po prostu nie zwróciła żadnych wierszy. Rozwiązaniem jest ponownie zgłosić wyjątek po przechwytywanie i rejestrowanie:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Przechwytuj wszystkie wyjątki w ról procesów roboczych

Nieobsłużone wyjątki w roli procesu roboczego spowoduje, że maszyna wirtualna może zostać odtworzone, dlatego powinna opakować wszystko, co zrobić w bloku try / catch i obsługiwać wszystkie wyjątki.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Określ długość właściwości ciągów w klas jednostek

W celu wyświetlenia prostego kodu, oryginalną wersję aplikacji naprawić nie określił długości dla pól jednostki FixItTask, a w rezultacie zostały zdefiniowane jako varchar(max) w bazie danych. W rezultacie interfejsu użytkownika będzie akceptować niemal dowolnej ilości danych wejściowych. Określanie limitów zestawy długości, które mają zastosowanie zarówno do użytkownika danych wejściowych w stronę sieci web i rozmiar kolumny w bazie danych:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Oznacz prywatnych elementów członkowskich jako tylko do odczytu, gdy nie są one powinna się zmienić

Na przykład w `DashboardController` wystąpienia klasy `FixItTaskRepository` jest tworzony i nie powinien zmienić, więc go jako zdefiniowaliśmy [tylko do odczytu](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Użyj listy. Funkcja any() zamiast listy. Count() &gt; 0

Jeśli należy zadbać o to czy co najmniej jeden element na liście dopasować określone kryteria, użyj [wszelkie](https://msdn.microsoft.com/library/bb534972.aspx) metody, ponieważ zwraca ono wartość zaraz zostanie znaleziony element, który pasuje do kryteriów, natomiast `Count` metoda ma zawsze do iteracji za pomocą każdego elementu. Pulpit nawigacyjny *Index.cshtml* pliku pierwotnie miały ten kod:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Zmieniliśmy go do poniższego:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generowania adresów URL w widoków MVC za pomocą pomocników MVC

Dla **rozwiązać go utworzyć** przycisk na stronie głównej poprawka aplikacji twardych kodowanego elementu zakotwiczenia:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Łączy widoku/Akcja tak jest lepiej używać [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) pomocnika kodu HTML, na przykład:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Użyj Task.Delay zamiast Thread.Sleep w roli procesu roboczego

Szablon nowego projektu umieszcza `Thread.Sleep` w przykładzie kodu dla roli procesu roboczego, ale powoduje wątku w tryb uśpienia, może spowodować puli wątków zduplikować dodatkowe wątki niepotrzebne. Można uniknąć, za pomocą [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) zamiast tego.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Należy unikać async void

Jeśli metoda asynchroniczna nie musi zwracać wartość, zwracają `Task` typu zamiast `void`.

W tym przykładzie pochodzi z `FixItQueueManager` klasy: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Należy używać `async void` tylko dla programów obsługi zdarzeń najwyższego poziomu. Jeśli zdefiniujesz metodę jako `async void`, obiekt wywołujący nie **await** metody lub przechwycić wyjątki, które metoda wygeneruje. Aby uzyskać więcej informacji, zobacz [najlepszych rozwiązań dotyczących Asynchronous Programming](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Użycia tokenu anulowania, aby przerwać z pętli roli procesu roboczego

Zazwyczaj **Uruchom** metody w roli procesu roboczego zawiera wejścia w nieskończoną pętlę. Gdy rola procesu roboczego jest zatrzymywane, [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) metoda jest wywoływana. Należy używać tej metody można anulować pracy, która jest wykonywana w ramach **Uruchom** metody i wyjścia bezpiecznie. W przeciwnym razie proces może być zakończony w trakcie wykonywania operacji.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Zrezygnować z procedury automatyczne wykrywanie MIME

W niektórych przypadkach program Internet Explorer raporty typ MIME, różni się od typu określonego przez serwer sieci web. Na przykład jeśli program Internet Explorer znajdzie HTML zawartości w pliku dostarczane z nagłówka odpowiedzi HTTP Content-Type: text/plain programu Internet Explorer określa, czy zawartość ma być renderowany jako kod HTML. Niestety to "— Wykrywanie MIME" może także prowadzić do problemów z zabezpieczeniami dla serwerów hostujących niezaufaną zawartość. Aby walczyć ten problem, Internet Explorer 8 wprowadził szereg zmian do kodu określenie typu MIME i umożliwia deweloperom [zrezygnować z wykrywanie MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Następujący kod został dodany do *Web.config* pliku.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Włącz tworzenie pakietów i minimalizowanie

Gdy program Visual Studio tworzy nowy projekt sieci web, tworzenie pakietów i minimalizowanie plików JavaScript nie włączono domyślnie. Dodaliśmy wiersza kodu w BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Ustawianie limitu czasu wygaśnięcia uwierzytelniania plików cookie

Domyślnie pliki cookie uwierzytelniania wygasa w ciągu dwóch tygodni. Krótszy czas jest bezpieczniejszy. Możesz zmienić to ustawienie w *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Jak uruchomić aplikację w programie Visual Studio na komputerze lokalnym

Istnieją dwa sposoby uruchamiania aplikacji napraw:

- Uruchom aplikację podstawowy, który zapisuje nowe zadania bezpośrednio do bazy danych SQL.
- Uruchom aplikację za pomocą kolejki, a także usługi zaplecza do utworzenia zadania. Wzorzec kolejki jest opisany w rozdziale [wzorzec pracy skoncentrowany na kolejkach](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Uruchamianie podstawowych aplikacji

1. Zainstaluj [Visual Studio 2013 lub Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads).
2. Zainstaluj [zestaw Azure SDK dla platformy .NET dla programu Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Pobierz plik zip z [galerii kodu MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. W Eksploratorze plików kliknij prawym przyciskiem myszy plik zip i kliknij polecenie Właściwości, a następnie w oknie dialogowym właściwości kliknij odblokowania.
5. Rozpakuj plik.
6. Kliknij dwukrotnie plik SLN, aby uruchomić program Visual Studio.
7. W menu Narzędzia kliknij Menedżer pakietów biblioteki, a następnie Konsola Menedżera pakietów.
8. W konsoli Menedżera pakietów (PMC), kliknij przycisk Przywróć.
9. Zamknij program Visual Studio.
10. Rozpocznij [emulatora usługi Azure storage](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Uruchom ponownie program Visual Studio, otwierając plik rozwiązania zamknięte w poprzednim kroku.
12. Upewnij się, że projekt automatyczne jest ustawiony jako projekt startowy, a następnie naciśnij klawisze CTRL + F5, aby uruchomić projekt.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Uruchamianie aplikacji z przetwarzaniem kolejki

1. Postępuj zgodnie ze wskazówkami dotyczącymi [uruchamiania podstawowej aplikacji](#runbase), a następnie zamknij przeglądarkę i zamknij program Visual Studio.
2. Uruchom program Visual Studio z uprawnieniami administratora. (Będziesz korzystać z emulatora obliczeń platformy Azure, i która wymaga uprawnień administratora).
3. W aplikacji *Web.config* w pliku *MyFixIt* projektu (projekt sieci web), zmień wartość właściwości `appSettings/UseQueues` na wartość "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Jeśli [emulatora usługi Azure storage](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) nie jest nadal uruchomione, uruchom go ponownie.
5. Uruchom projekt sieci web automatyczne i projekt MyFixItCloudService jednocześnie.

    Przy użyciu programu Visual Studio 2013:

   1. Naciśnij klawisz F5, aby uruchomić projekt automatyczne rozwiązywanie problemu.
   2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt MyFixItCloudService, a następnie kliknij **debugowania** -- **Uruchom nowe wystąpienie**.

      Instalacja programu Visual Studio 2013 Express for Web:

   3. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie automatyczne, a następnie wybierz **właściwości**.
   4. Wybierz **wiele projektów startowych**...
   5. W **akcji** wybierz z listy rozwijanej w obszarze MyFixIt i MyFixItCloudService, **Start**.
   6. Kliknij przycisk **OK**.
   7. Naciśnij klawisz F5, aby uruchomić oba projekty.

      Po uruchomieniu projektu MyFixItCloudService uruchomieniu programu Visual Studio emulator obliczeń platformy Azure. W zależności od konfiguracji zapory konieczne może być Zezwalaj na emulator przez zaporę.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Sposób wdrażania podstawowej aplikacji w usłudze Azure App Service Web Apps za pomocą skryptów środowiska Windows PowerShell

Aby zilustrować [Automatyzowanie wszystkiego](automate-everything.md) wzorca aplikacji naprawić jest dostarczany za pomocą skryptów, które konfigurowania środowiska na platformie Azure i wdrażanie projektu do nowego środowiska. Poniższe instrukcje wyjaśniają, jak za pomocą skryptów.

Jeśli chcesz uruchomić na platformie Azure bez przy użyciu funkcji kolejek i zostały wprowadzone zmiany, aby uruchomić lokalnie za pomocą kolejek, upewnij się, że wartość UseQueues appSetting ponownie na wartość false, przed dalszym wykonywaniem poniższych instrukcji.

W poniższych instrukcjach przyjęto zostały już pobrane i rozwiązanie naprawić lokalnie, i czy masz Azure konta lub subskrypcji platformy Azure, masz uprawnienia do zarządzania.

1. Zainstaluj **programu Azure PowerShell** konsoli. Aby uzyskać instrukcje, zobacz [jak zainstalować i skonfigurować program Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Ta konsola dostosowane jest skonfigurowane do pracy z subskrypcją platformy Azure. Moduł usługi Azure jest zainstalowany w *Program Files* katalogu i jest importowany automatycznie co korzystający z konsoli programu Azure PowerShell.

    Jeśli wolisz pracować w programie do innego hosta, takich jak Windows PowerShell ISE, należy użyć [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) polecenia cmdlet, aby zaimportować moduł platformy Azure lub użyj polecenia modułu usługi Azure do wyzwolenia automatycznego importowania modułu.
2. Uruchom program Azure PowerShell za pomocą **Uruchom jako administrator** opcji.
3. Uruchom [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) polecenia cmdlet, aby ustawić zasady wykonywania programu Azure PowerShell `RemoteSigned`. Wprowadź **Y** (dla tak) do ukończenia zmiany zasad.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    To ustawienie umożliwia uruchamianie skryptów lokalnych, które nie są podpisane cyfrowo. (Można również ustawić zasady wykonywania `Unrestricted`, który może wyeliminować konieczność stosowania kroku odblokowanie później, ale nie jest to zalecane ze względów bezpieczeństwa.)
4. Uruchom `Add-AzureAccount` polecenia cmdlet i konfigurowanie programu PowerShell przy użyciu poświadczeń dla konta usługi.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Te poświadczenia tracą ważność po upływie pewnego czasu i ponownie uruchom `Add-AzureAccount` polecenia cmdlet. Jak ta Książka elektroniczna jest zapisywana, limit czasu wygaśnięcia poświadczeń jest 12 godzin.
5. Jeśli masz wiele subskrypcji, należy użyć polecenia cmdlet Select-AzureSubscription, aby określić subskrypcję, której chcesz utworzyć środowisko testowe w.
6. Zaimportuj certyfikat zarządzania dla tej samej subskrypcji platformy Azure przy użyciu `Get-AzurePublishSettingsFile` i `Import-AzurePublishSettingsFile` polecenia cmdlet. Pierwsza z tych poleceń cmdlet spowoduje pobranie pliku certyfikatu, a w drugim określisz lokalizacja tego pliku, aby można było go zaimportować. > [!IMPORTANT]
   > Zachowaj pobrany plik w bezpiecznym miejscu, lub usuwasz, gdy wszystko będzie gotowe, ponieważ zawiera on certyfikat, który może służyć do zarządzania usługami platformy Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Certyfikat jest używany do wywołania interfejsu API REST, który wykrywa adres IP na komputerze deweloperskim, aby ustawić regułę zapory na serwerze bazy danych SQL.
7. Uruchom [Ustawianie lokalizacji](https://go.microsoft.com/fwlink/p/?linkid=293912) polecenia cmdlet (aliasy `cd`, `chdir`, i `sl`) przejdź do katalogu, który zawiera skrypty. (Znajdują się one w *automatyzacji* folderu w folderze rozwiązania rozwiązać go.) Jeśli którakolwiek z nazwy katalogów zawiera spacje, umieść ścieżkę w cudzysłowie. Na przykład, aby przejść do `c:\Sample Apps\FixIt\Automation` katalogu można wprowadzić następujące polecenie:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Aby umożliwić programu Windows PowerShell, aby uruchamiać te skrypty, należy użyć [odblokowanie pliku](https://go.microsoft.com/fwlink/p/?linkid=294021) polecenia cmdlet. (Skrypty są blokowane, ponieważ zostały one pobrane z Internetu.)

    > [!WARNING]
    > Zabezpieczenia — przed uruchomieniem `Unblock-File` na dowolny skrypt lub plik wykonywalny, otwórz plik w programie Notatnik, sprawdź polecenia i sprawdź, czy nie zawierają złośliwego kodu.

    Na przykład następujące polecenie uruchamia `Unblock-File` polecenie cmdlet na wszystkie skrypty w bieżącym katalogu.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Aby utworzyć aplikację sieci web dla podstawy (nie kolejki przetwarzania) go naprawić aplikację, uruchom skrypt tworzenia środowiska.

    Wymagane `Name` parametr określa nazwę bazy danych, a także jest używane dla konta magazynu, który tworzy skrypt. Nazwa musi być globalnie unikatowa w domenie azurewebsites.net. Jeśli określisz nazwę, która nie jest unikatowa, takich jak automatyczne lub testów (lub nawet tak jak w przykładzie fixitdemo) `New-AzureWebsite` polecenie cmdlet kończy się niepowodzeniem z powodu błędu wewnętrznego, zgłosi konflikt. Skrypt konwertuje nazwę literami, aby spełnić wymagania dotyczące nazw dla aplikacji sieci web, kont magazynu i baz danych.

    Wymagane `SqlDatabasePassword` parametr określa hasło dla konta administratora, który zostanie utworzony dla usługi SQL Database. Nie dołączaj specjalne znaki XML w haśle (&amp; &lt; &gt; ;). Jest to ograniczenie sposobu, w jaki skrypty zostały napisane, nie ograniczenia platformy Azure.

    Na przykład jeśli chcesz utworzyć aplikację sieci web o nazwie "fixitdemo" i użyć "Passw0rd1" hasło administratora programu SQL Server, można wprowadzić następujące polecenie:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Nazwa musi być unikatowa w domenie azurewebsites.net, a hasło musi spełniać wymagania bazy danych SQL dla złożoności hasła. (Przykład Passw0rd1 spełnia wymagań).

    Należy zauważyć, że polecenie rozpoczyna się od ". \". Aby zapobiec wykonania złośliwych skryptów, programu Windows PowerShell wymaga podać pełną ścieżkę do pliku skryptu w przypadku uruchamiania skryptu. Można użyć pojedynczego znaku kropki wskazuje bieżący katalog (".\") lub podaj pełną ścieżkę, taką jak:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Aby uzyskać więcej informacji na temat skryptu użyj `Get-Help` polecenia cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Możesz użyć `Detailed`, `Full`, `Parameters`, i `Examples` parametry polecenia cmdlet Get-Help, aby filtrować pomocy, która jest zwracana.

    Jeśli skrypt zakończy się niepowodzeniem lub generuje błędy, takie jak "New-AzureWebsite: wywołanie Set-AzureSubscription i Select-AzureSubscription najpierw" nie została ukończona konfiguracja programu Azure PowerShell.

    Po zakończeniu działania skryptu można użyć portalu zarządzania systemu Azure, aby wyświetlić zasoby, które zostały utworzone, jak pokazano na [Automatyzowanie wszystkiego](automate-everything.md) rozdziale.
10. Aby wdrożyć projekt automatyczne do nowego środowiska platformy Azure, użyj *AzureWebsite.ps1* skryptu. Na przykład:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Po zakończeniu wdrażania przeglądarki zostanie otwarta z napraw go, działające na platformie Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Rozwiązywanie problemów z skryptów programu Windows PowerShell

Typowe błędy występujące podczas uruchamiania tych skryptów są powiązane uprawnienia. Upewnij się, że `Add-AzureAccount` i `Import-AzurePublishSettingsFile` zakończyły się pomyślnie, a używany dla tej samej subskrypcji platformy Azure. Nawet wtedy, gdy `Add-AzureAccount` został pomyślnie trzeba będzie uruchomić go ponownie. Uprawnienia dodane przez `Add-AzureAccount` wygaśnie w ciągu 12 godzin.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Odwołanie do obiektu nie zostały ustawione na wystąpienie obiektu.

Jeśli skrypt zwraca informacje o błędach, takie jak "Odwołanie do obiektu nie ustawione na wystąpienie obiektu" oznacza to, że program Windows PowerShell nie można odnaleźć obiektu, do procesu (jest to wyjątek odwołania o wartości null), uruchom `Add-AzureAccount` polecenia cmdlet, a następnie ponownie spróbuj skryptu.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>Błąd wewnętrzny: Serwer napotkał błąd wewnętrzny.

`New-AzureWebsite` Polecenie cmdlet zwraca błąd wewnętrzny, gdy nazwa nie jest unikatowa w domenie azurewebsites.net. Aby naprawić błąd, należy użyć innej wartości dla nazwy, która znajduje się w parametrze Name *New AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Ponowne uruchamianie skryptu

Jeśli zachodzi potrzeba ponownego uruchomienia *New AzureWebsiteEnv.ps1* skryptu ponieważ nie powiodło się, zanim ją wydrukowana komunikat "Skrypt będzie gotowy", możesz chcieć usunąć zasoby, które zatrzymane skrypt utworzony przed. Na przykład skrypt już utworzono ContosoFixItDemo aplikacji sieci web i następnie ponownie uruchom skrypt o takiej samej nazwie, skrypt zakończy się niepowodzeniem, ponieważ nazwa jest używana.

Aby określić, które zasoby skrypt utworzony przed ona zatrzymana, użyj następujących poleceń cmdlet:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Aby uruchomić to polecenie cmdlet, należy przekazać nazwę serwera bazy danych, aby `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Aby usunąć te zasoby, użyj następujących poleceń. Należy pamiętać o tym, po usunięciu serwera bazy danych, automatyczne usunięcie bazy danych skojarzonej z serwerem.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Jak wdrożyć aplikację za pomocą kolejki przetwarzania do usługi Azure App Service Web Apps i usługi w chmurze platformy Azure

Aby włączyć kolejek, wprowadź następującą zmianę w pliku MyFixIt\Web.config. W obszarze `appSettings`, zmień wartość właściwości `UseQueues` na wartość "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Wdrożenia aplikacji MVC w aplikacji sieci web w usłudze Azure App Service, zgodnie z opisem [wcześniej](#deploybase).

Następnie należy utworzyć nową usługę w chmurze platformy Azure. Skryptach dołączonych za pomocą aplikacji naprawić nie tworzą ani nie wdrożyć usługę w chmurze, więc należy użyć witryny Azure portal, w tym. W portalu, kliknij przycisk **nowy** -- **obliczenia** — **usługi w chmurze** -- **szybkie tworzenie**, a następnie wprowadź adres URL i lokalizację centrum danych. Użyj tego samego centrum danych, w której została wdrożona w aplikacji sieci web.

![](the-fix-it-sample-application/_static/image1.png)

Zanim będzie można wdrożyć usługę w chmurze, musisz zaktualizować niektóre pliki konfiguracji.

W MyFixIt.WorkerRoler\app.config w obszarze `connectionStrings`, zastąp wartość `appdb` parametrów połączenia parametrami połączenia dla bazy danych SQL. Parametry połączenia można uzyskać z portalu. W portalu, kliknij przycisk **baz danych SQL** - **appdb** - **parametry połączenia bazy danych SQL w widoku na potrzeby ADO .net, ODBC i PHP oraz JDBC**. Skopiuj parametry połączenia ADO.NET i wklej tę wartość do pliku app.config. Zastąp "{Twoja\_hasło\_tutaj}" za pomocą hasła bazy danych. (Zakładając, że skrypty są używane do wdrażania aplikacji MVC, określone hasło bazy danych w `SqlDatabasePassword` skryptu parametru.)

Wynik powinien wyglądać następująco:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

W tym samym pliku MyFixIt.WorkerRoler\app.config w obszarze `appSettings`, Zastąp dwie wartości symboli zastępczych dla konta usługi Azure storage.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Z poziomu portalu można uzyskać klucz dostępu. Zobacz [sposób zarządzania kontami magazynu](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

W MyFixItCloudService\ServiceConfiguration.Cloud.cscfg Zastąp ten sam dwie wartości symboli zastępczych dla konta usługi Azure storage.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Teraz możesz przystąpić do wdrażania usługi w chmurze. W Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy projekt MyFixItCloudService, a następnie wybierz **Publikuj**. Aby uzyskać więcej informacji, zobacz "[wdrażanie aplikacji na platformie Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", która znajduje się w części 2 [w tym samouczku](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Poprzednie](more-patterns-and-guidance.md)
