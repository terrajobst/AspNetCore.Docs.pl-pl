---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Źródło sterowania (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 12c695b65a21452fdc4a31e821854253bccec0d7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368978"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Kontroli źródła (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Kontrola źródła ma zasadnicze znaczenie dla wszystkich projektów rozwoju chmury, nie tylko środowiska zespołowe. W takich sytuacjach przydałaby traktować edycji kodu źródłowego lub nawet dokumentu programu Word bez funkcji cofania i automatycznego tworzenia kopii zapasowej i kontroli źródła zapewnia te funkcje na poziomie projektu, w którym można zapisać jeszcze więcej czasu, gdy coś pójdzie nie tak. Dzięki usługom kontroli źródła w chmurze nie masz już martwić się o Konfigurowanie skomplikowane, a następnie można użyć programu Visual Studio Online kontroli źródła jest bezpłatna dla maksymalnie 5 użytkowników.

Pierwsza część w tym rozdziale opisano trzy kluczowe najlepszych rozwiązań, aby pamiętać:

- [Traktuj skryptów automatyzacji, jak kod źródłowy](#scripts) i wersji je wraz z kodu aplikacji.
- [Nigdy nie ewidencjonuj w wpisów tajnych](#secrets) (poufne dane takie jak poświadczeń) w repozytorium kodu źródłowego.
- [Konfigurowanie gałęzi źródłowych](#devops) umożliwiające przepływ pracy DevOps.

W pozostałej części rozdział zapewnia niektóre przykładowe implementacje tych wzorców w programie Visual Studio, Azure i programu Visual Studio Online:

- [Dodaj skrypty do kontroli źródła w programie Visual Studio](#vsscripts)
- [Store poufnych danych na platformie Azure](#appsettings)
- [Za pomocą narzędzia Git w programie Visual Studio i Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Traktuj skryptów automatyzacji, jak kod źródłowy

Podczas pracy nad projektem w chmurze często zmieniasz rzeczy, i chcesz móc szybko reagować na problemy zgłaszane przez klientów. Szybkie reagowanie polega na użyciu skryptów automatyzacji, jak wyjaśniono w [Automatyzowanie wszystkiego](automate-everything.md) rozdziale. Wszystkie skrypty, które służy do tworzenia środowiska, wdrażać, Skaluj ją itp., muszą być zsynchronizowane z kodu źródłowego aplikacji.

Aby zsynchronizować skryptów przy użyciu kodu, należy przechowywać je w systemie kontroli źródła. Następnie w razie konieczności wycofania zmian lub wprowadzić szybkiej poprawki w kodzie produkcyjnym, która różni się od tworzenia kodu, nie trzeba marnowania czasu próby śledzenie ustawienia, które zostały zmienione lub których członkowie zespołu mają kopii wersji, które są potrzebne. Możesz teraz pewność, że skrypty, potrzebne są zsynchronizowane z bazy kodu, należy je w celu, która jest pewność, że wszyscy członkowie zespołu pracy przy użyciu tych samych skryptów. Następnie czy trzeba zautomatyzować testowanie i wdrażanie poprawki do produkcji lub opracowanie nowych funkcji, będziesz mieć prawo skryptu dla kodu, który musi zostać zaktualizowany.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Nie można zaewidencjonować wpisów tajnych

Repozytorium kodu źródłowego jest zazwyczaj dostępny zbyt wiele osób dla niego się odpowiednio bezpieczne miejsce, poufnych danych, takie jak hasła. Jeśli skrypty wpisy tajne, takie jak hasła, należy zdefiniować parametry te ustawienia, które nie są zapisywane w kodzie źródłowym oraz przechowywanie wpisów tajnych gdzie indziej.

Na przykład Azure umożliwia pobieranie plików, które zawierają opublikować ustawienia w celu automatyzacji tworzenia profilów publikowania. Te pliki obejmują nazwy użytkownika i hasła, które są autoryzowane do zarządzania usługami platformy Azure. Jeśli używasz tego metodą tworzenia profilów publikowania, a po zaewidencjonowaniu tych plików do kontroli źródła, każda osoba mająca dostęp do repozytorium zobaczyć te nazwy użytkownika i hasła. Można bezpiecznie przechowywać hasło w profilu publikowania, sam, ponieważ jest on zaszyfrowany i znajduje się w *. pubxml.user* pliku, która domyślnie nie jest uwzględniony w kontroli źródła.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Struktura gałęzi źródłowych w celu ułatwienia przepływ pracy DevOps

Jak zaimplementować gałęzi w repozytorium ma wpływ na możliwości rozwijania nowych funkcji i rozwiązywanie problemów w środowisku produkcyjnym. Poniżej przedstawiono wzorzec, że wiele średniej wielkości zespoły wykorzystują:

![Struktura gałęzi źródłowej](source-control/_static/image1.png)

Gałąź główna jest zawsze zgodny kod, który znajduje się w środowisku produkcyjnym. Gałęzie poniżej wzorca odpowiadają różnych etapach cyklu rozwoju. Gałąź rozwoju polega na tym, gdzie wdrożyć nowe funkcje. Dla małych zespołów może być konieczne jest tylko główny i rozwoju, ale często zaleca się osób tymczasowej gałęzi między środowiskami deweloperskim i master. Przemieszczania służy do końcowego integracji testowania przed aktualizacji jest przenoszony do środowiska produkcyjnego.

Dla dużych zespołów mogą być osobnych oddziałach dla każdej nowej funkcji; dla mniejszych zespołów Niewykluczone, że wszyscy ewidencjonowanie z gałęzią rozwoju.

Jeśli masz gałąź dla każdej funkcji, gdy funkcja A jest gotowy możesz scalania jego zmiany kodu źródłowego się do rozwoju gałęzi i w dół na inne gałęzie funkcji. Ten kod źródłowy procesu scalania może być czasochłonne i aby uniknąć tej pracy, jednocześnie zachowując osobne funkcje, niektóre zespoły wdrożenia o nazwie zamiast *[Włącza lub wyłącza funkcję](http://en.wikipedia.org/wiki/Feature_toggle)* (znanego również jako *flag funkcji*). Oznacza to, cały kod dla wszystkich funkcji znajduje się w tej samej gałęzi, ale można włączyć lub wyłączyć poszczególne funkcje przy użyciu przełączników w kodzie. Na przykład załóżmy, że funkcja A jest nowe pole dla napraw go zadania związane z aplikacjami, a funkcja B dodaje funkcje pamięci podręcznej. Kod dla obu funkcji mogą znajdować się w gałęzi rozwoju, ale wyświetlanie tylko aplikacji będzie nowego pola, gdy zmienna jest ustawiona na wartość true, a będzie używać tylko buforowania, gdy różne zmienna jest ustawiona na wartość true. Jeśli funkcja A nie jest gotowy do zajęcia miejsca, ale funkcja B jest gotowy, możesz podwyższyć poziom całego kodu do środowiska produkcyjnego z przełącznikiem funkcji A wyłączone, a funkcja B przełączyć się. Można następnie Zakończ funkcja A programu i Promuj go później, wszystko nie scalania kodu źródłowego.

Czy używasz gałęzi lub włącza lub wyłącza funkcje, rozgałęziona struktura następująco umożliwia przepływ kodu od projektowania do produkcji w sposób agile i powtarzalne.

Ta struktura pozwala również szybko reagować na opinie klientów. Jeśli musisz wprowadzić szybkiej poprawki do środowiska produkcyjnego, możesz również tworzyć, efektywnie elastyczne. Możesz utworzyć gałąź zniżki w stosunku do głównego lub tymczasowego i kiedy będzie gotowy scalić się master i w dół do gałęzi funkcji i rozwoju.

![W gałęzi poprawek](source-control/_static/image2.png)

Bez rozgałęziona struktura następująco z jego separacji dla środowiska produkcyjnego, gałęzie problem produkcji umieścić możesz w pozycji o podwyższenie poziomu nowego kodu funkcji wraz z produkcji rozwiązanie problemu. Nowy kod funkcji może nie być w pełni przetestowane i gotowe do produkcji i trzeba będzie wykonać dużo pracy tworzenia kopii wprowadzonych zmian, które nie są gotowe. Możesz też opóźnienie poprawkę w taki sposób, aby można było przetestować zmiany i przygotowania ich do wdrożenia.

Następnie zobaczysz przykłady sposobu wdrożenia tych trzech wzorców w programie Visual Studio, Azure i programu Visual Studio Online. Oto przykłady zamiast szczegółowe instrukcje krok po kroku jak-to-it; Aby uzyskać szczegółowe instrukcje, zapewniających wszystkie niezbędne kontekstu, zobacz [zasobów](#resources) sekcji na końcu rozdziale.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Dodaj skrypty do kontroli źródła w programie Visual Studio

Istnieje możliwość dodania skryptów z kontrolą źródła w programie Visual Studio, umieszczając je w folderze rozwiązania programu Visual Studio (przy założeniu, że projekt jest w kontroli źródła). Oto jeden ze sposobów, aby to zrobić.

Utwórz folder dla skryptów w folderze rozwiązania (tym samym folderze, który ma swoje *.sln* pliku).

![Folder usługi Automation](source-control/_static/image3.png)

Skopiuj pliki skryptów do folderu.

![Zawartość folderu automatyzacji](source-control/_static/image4.png)

W programie Visual Studio należy dodać folder rozwiązania do projektu.

![Nowy Folder rozwiązania menu wyboru](source-control/_static/image5.png)

I Dodaj pliki skryptów do folderu rozwiązania.

![Dodaj istniejący element menu wyboru](source-control/_static/image6.png)

![Dodaj istniejący element — okno dialogowe](source-control/_static/image7.png)

Pliki skryptów znajdują się teraz w projekcie i ich zmiany wersji wraz z odpowiedniej zmiany kodu źródłowego służy do śledzenia kontroli źródła.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Store poufnych danych na platformie Azure

Po uruchomieniu aplikacji w witrynie sieci Web systemu Azure jest jednym ze sposobów, aby uniknąć przechowywania poświadczeń w kontroli źródła, przechowywania ich na platformie Azure.

Na przykład aplikacja naprawić przechowuje w jego pliku Web.config pliku dwa ciągi połączeń, które będą miały haseł w środowisku produkcyjnym i klucz, który zapewnia dostęp do konta usługi Azure storage.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Jeśli umieścisz rzeczywistej produkcji wartości tych ustawień w Twojej *Web.config* pliku, czy umieść je w *Web.Release.config* pliku, aby skonfigurować przekształcenia pliku Web.config, aby wstawić je podczas wdrażania będą one przechowywane, w repozytorium źródłowym. Jeśli możesz wprowadzić parametry połączenia bazy danych do produkcji profil publikowania, hasło nie będzie w swojej *.pubxml* pliku. (Można wykluczyć *.pubxml* plików z kontroli źródła, ale następnie utracisz korzyści wynikające z innych ustawień wdrażania związanych z udostępnianiem.)

Platforma Azure zapewnia alternatywę dla **appSettings** i połączenia ciągi sekcje *Web.config* pliku. Oto odpowiednia część **konfiguracji** kartę dla witryny sieci web w portalu zarządzania systemu Azure:

![sekcji appSettings i connectionStrings w portalu](source-control/_static/image8.png)

Podczas wdrażania projektu do tej witryny sieci web i uruchamiania aplikacji, niezależnie od wartości, które mają być przechowywane na platformie Azure zastąpienie wszelkich wartości znajdują się w pliku Web.config.

Te wartości zostaną ustawione na platformie Azure przy użyciu portalu zarządzania lub skryptów. Skrypt automatyzacji tworzenia środowiska przedstawionego [Automatyzowanie wszystkiego](automate-everything.md) rozdział tworzy usługi Azure SQL Database, pobiera magazynu i parametry połączenia bazy danych SQL i przechowuje tych kluczy tajnych w ustawieniach witryny sieci web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Należy zauważyć, że skrypty są parametryzowane tak, aby rzeczywiste wartości nie uzyskać utrwalone w repozytorium źródłowym.

Po uruchomieniu w środowisku projektowym lokalnie aplikacji odczytuje lokalny plik Web.config i połączenie punktów ciąg do bazy danych LocalDB programu SQL Server w *aplikacji\_danych* folderu projektu sieci web. Po uruchomieniu aplikacji na platformie Azure i aplikacja próbuje odczytać te wartości z pliku Web.config, co otrzymuje i używa są wartości przechowywane dla witryny sieci Web, a nie co faktycznie znajduje się w pliku Web.config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Za pomocą narzędzia Git w programie Visual Studio i Visual Studio Online

Do zaimplementowania DevOps rozgałęziona struktura przedstawiony wcześniej, można użyć dowolnego środowiska kontroli źródła. Aby rozproszone zespoły [Rozproszony system kontroli wersji](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) mogą działać najlepiej; w przypadku innych zespołów [scentralizowane system](http://en.wikipedia.org/wiki/Revision_control) może działać lepiej.

[Git](http://git-scm.com/) jest DVCS, który jest stał się bardzo popularna. Kiedy używasz Git do kontroli źródła, masz pełną kopię repozytorium z całą historią na komputerze lokalnym. Wiele osób wolą, ponieważ jest łatwiejsza zatwierdzenia, aby kontynuować pracę, gdy nie masz połączenia z siecią — może być nadal wykonywać i wycofywanie zmian, Utwórz i przełączania gałęzi i tak dalej. Nawet wtedy, gdy masz połączenie z siecią, jest to, łatwiej i szybciej tworzyć gałęzie i przełączania gałęzi, gdy wszystko jest lokalny. Możesz również tworzyć lokalnych zatwierdzeń i wycofywanie zmian, bez mających wpływ na innym deweloperom. I wsadowego zatwierdzenia przed wysłaniem ich do serwera.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), wcześniej znane jako usługi Team Foundation Service oferuje zarówno usługi Git i [Team Foundation Version Control](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; scentralizowane kontroli źródła). W tym miejscu w firmie Microsoft w grupie Azure niektóre zespoły używają scentralizowanej kontroli źródła, użyj dystrybucji, a w niektórych mieszanego (scentralizowane i widoczne dla niektórych projektów i dystrybucji w innych projektach,). Usługa VSO jest dostępna bezpłatnie dla maksymalnie 5 użytkowników. Możesz zarejestrować się do planu free [tutaj](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 obejmuje najwyższej klasy wbudowane [obsługi systemu Git](https://msdn.microsoft.com/library/hh850437.aspx); Oto krótki pokaz usługi jak to działa.

Mając otwarty w programie Visual Studio 2013 projekt, kliknij prawym przyciskiem myszy rozwiązanie w **Eksploratora rozwiązań**i wybierz polecenie **Dodaj rozwiązanie do kontroli źródła**.

![Dodaj rozwiązanie do kontroli źródła](source-control/_static/image9.png)

Visual Studio Wyświetla prośbę, jeśli chcesz użyć (scentralizowany system kontroli wersji) TFVC lub Git.

![Wybierz kontrolę źródła](source-control/_static/image10.png)

Po wybierz Git i kliknięciu **OK**, program Visual Studio tworzy nowego lokalnego repozytorium Git w folderze rozwiązania. Nowe repozytorium nie ma jeszcze; plików należy dodać je do repozytorium, wykonując zatwierdzeń usługi Git. Kliknij prawym przyciskiem myszy rozwiązanie w **Eksploratora rozwiązań**, a następnie kliknij przycisk **zatwierdzić**.

![Zatwierdzenia](source-control/_static/image11.png)

Program Visual Studio automatycznie przygotowuje wszystkie pliki projektu do zatwierdzenia i wyświetla je w **Team Explorer** w **uwzględnione zmiany** okienka. (Jeśli wystąpiły niektóre nie chcesz, aby uwzględnić w zatwierdzeniu, można wybrać je, kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **wykluczyć**.)

![Team Explorer](source-control/_static/image12.png)

Wprowadź komentarz zatwierdzenia, a następnie kliknij przycisk **zatwierdzenia**, i Visual Studio wykonuje zatwierdzenia i wyświetla identyfikator zatwierdzenia.

![Team Explorer zmiany](source-control/_static/image13.png)

Teraz po zmianie kodu, aby go różni się od co znajduje się w repozytorium łatwo wyświetlać różnice. Kliknij prawym przyciskiem myszy plik, który zmieniono, wybierz **porównania z Unmodified**, i Uzyskaj wyświetlania porównanie, który zawiera niezatwierdzone zmiany.

![Porównaj z niezmodyfikowanym](source-control/_static/image14.png)

![Diff wyświetlanie zmiany](source-control/_static/image15.png)

Można łatwo sprawdzenia, jakie zmiany wprowadzania i zaewidencjonuj je.

Załóżmy, że należy wprowadzić gałąź — możesz to zrobić w programie Visual Studio za. W **Team Explorer**, kliknij przycisk **nową gałąź**.

![Team Explorer nowa gałąź](source-control/_static/image16.png)

Wprowadź nazwę gałęzi, kliknij przycisk **utwórz gałąź**, a w przypadku wybrania **Wyewidencjonuj gałąź**, Visual Studio automatycznie sprawdza out nowej gałęzi.

![Team Explorer nowa gałąź](source-control/_static/image17.png)

Można teraz wprowadzić zmiany do plików i zaewidencjonować je w tej gałęzi. I można łatwo przełączać między gałęziami i programu Visual Studio automatycznie synchronizuje pliki do gałęzi, w której zostały wyewidencjonowane. W tym przykładzie tytułu strony sieci web, w  *\_Layout.cshtml* została zmieniona na "Odpowiednia poprawka 1" w Poprawka1 gałęzi.

![Poprawka1 gałęzi](source-control/_static/image18.png)

Po przejściu do poziomu głównego gałęzi, zawartość  *\_Layout.cshtml* pliku automatycznie przywrócić są w gałęzi głównej.

![Gałąź główna](source-control/_static/image19.png)

Ten prosty przykład jak szybko utworzyć gałąź i przerzucanie i z powrotem między gałęziami. Ta funkcja umożliwia bardzo elastyczne przepływu pracy przy użyciu strukturę gałęzi i skrypty automatyzacji są prezentowane w [Automatyzowanie wszystkiego](automate-everything.md) rozdziale. Na przykład może być pracy w gałęzi rozwoju, tworzenie gałęzi poprawki zniżki w stosunku do głównego, przełącz się do nowej gałęzi, wprowadzać w nim zmian i zatwierdzić je i przejdź z powrotem do gałęzi rozwoju i kontynuować wykonywanych wcześniej czynności.

Opisane w tym miejscu jest sposób pracy z lokalnego repozytorium Git w programie Visual Studio. W środowisku zespołowym, można zwykle także Wypchnij zmiany typowe repozytorium. Narzędzia programu Visual Studio umożliwiają również wskaż zdalne repozytorium Git. W tym celu można użyć GitHub.com, możesz też [Git w programie Visual Studio Online](https://msdn.microsoft.com/library/hh850437.aspx) zintegrowana z usługą wszystkie inne usługi Visual Studio Online funkcje takie jak element roboczy i śledzenia usterek.

Nie jest to jedyny sposób można zaimplementować agile strategii rozgałęziania, oczywiście. Można włączyć tego samego przepływu pracy agile, przy użyciu repozytorium kontroli źródła scentralizowany.

## <a name="summary"></a>Podsumowanie

Ma być mierzony sukces system kontroli źródła na podstawie jak szybko wprowadzić zmianę i pobierz go na żywo w sposób bezpieczny i przewidywalne. Jeśli zaczniesz Przerażony wprowadzić zmiany, ponieważ trzeba dzień lub dwa testami ręcznymi w nim, możesz zadawać sobie co musisz zrobić process-wise lub test-wise, tak aby było wprowadzać zmiany w ciągu kilku minut lub najgorszy już niż godzina. Jedną ze strategii dla tych czynności jest do zaimplementowania ciągłej integracji i ciągłego dostarczania, które omówiono w [następny rozdział](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Resources

[Programu Visual Studio Online](https://www.visualstudio.com/) portal zawiera dokumentację i usług pomocy technicznej i możesz zasubskrybować konta. Jeśli masz program Visual Studio 2012 i w związku z tym chce za pomocą narzędzia Git, zobacz [Visual Studio Tools for Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Aby uzyskać więcej informacji na temat (scentralizowany system kontroli wersji) TFVC i Git (rozproszonej kontroli wersji) zobacz następujące zasoby:

- [Którego systemu kontroli wersji należy używać: TFVC czy Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) Dokumentacja MSDN zawiera tabelę zawierającą podsumowanie różnic między systemami TFVC i Git.
- [Dobrze podoba mi się Team Foundation Server i podoba mi się Git, ale która jest lepsze?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Porównanie usług Git i TFVC.

Aby uzyskać więcej informacji na temat strategii rozgałęziania zobacz następujące zasoby:

- [Tworzenie potoku wersji przy użyciu serwera Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Dokumentacja firmy Microsoft Patterns and Practices. Zobacz rozdział 6 dyskusję na temat strategii rozgałęziania. Funkcja ambasadorzy przełącza za pośrednictwem gałęzie funkcji i gałęzie funkcji są używane, promuje zapewnienie ich krótkotrwałe (godzin lub dni, co najwyżej).
- [Przewodnik kontroli wersji](https://aka.ms/vsarsolutions). Przewodnik dotyczący strategii rozgałęziania przez ALM Rangers. Zobacz rozgałęzianie Strategies.pdf na kartę pliki do pobrania.
- [Tworzenie oprogramowania za pomocą przełączników funkcji](https://msdn.microsoft.com/magazine/dn683796.aspx). Artykuł w MSDN Magazine.
- [Funkcja przełączania](http://martinfowler.com/bliki/FeatureToggle.html). Wprowadzenie do funkcji włącza/wyłącza / flagi funkcji w blogu Martina Fowlera.
- [Włącza lub wyłącza vs gałęzie funkcji są wyposażone w](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Inny wpis w blogu Włącza lub wyłącza funkcję, przez Dylan Smith.

Aby uzyskać więcej informacji na temat obsługi poufne informacje, które nie powinny być przechowywane w poziomie repozytoriów kontroli źródła zobacz następujące zasoby:

- [Najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i usłudze Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Usługa witryny sieci Web: Jak Application Strings and połączenia działania ciągów](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Wyjaśnia, funkcja platformy Azure, która zastępuje `appSettings` i `connectionStrings` dane w *Web.config* pliku.
- [Niestandardowe ustawienia konfiguracji i aplikacji w usłudze Azure witryn sieci Web — za pomocą Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Aby uzyskać informacje o innych metodach przechowywanie poufnych informacji poza kontrolą źródła, zobacz [platformy ASP.NET MVC: Zachowaj prywatnej ustawienia z kontroli źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Poprzednie](automate-everything.md)
> [dalej](continuous-integration-and-continuous-delivery.md)
