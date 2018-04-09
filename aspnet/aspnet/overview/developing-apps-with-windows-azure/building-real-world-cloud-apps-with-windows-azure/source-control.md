---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Źródła formantu (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 0022458fa89a3be7ee8303750ad0e072df3b1bab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Kontroli źródła (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Kontroli źródła jest niezbędne dla wszystkich projektów programowanie chmury, nie tylko środowiska zespołu. Nie należy traktować edycji kodu źródłowego lub nawet dokument programu Word bez funkcja cofania i automatycznego tworzenia kopii zapasowej i kontroli źródła zawiera te funkcje na poziomie projektu, w którym można zapisać nawet dłużej, jeśli jakaś nieprawidłowość. Z usługi kontroli źródła w chmurze masz już martwić się o konfiguracji skomplikowane i można użyć programu Visual Studio Online bezpłatnie do maksymalnie 5 użytkowników kontroli źródła.

Pierwsza część w tym rozdziale opisano trzy kluczowe najlepsze rozwiązania należy wziąć pod uwagę:

- [Traktuj skryptów automatyzacji jako kod źródłowy](#scripts) i wersji ich razem z kodu aplikacji.
- [Nigdy nie Szukaj w kluczy tajnych](#secrets) (poufne dane takie jak poświadczenia) do repozytorium kodu źródłowego.
- [Konfigurowanie gałęzi źródłowej](#devops) umożliwiające DevOps przepływu pracy.

W pozostałej części rozdziału zawiera niektóre implementacje próbki tych wzorców w Visual Studio, Azure i programu Visual Studio Online:

- [Dodawanie skryptów do kontroli źródła w programie Visual Studio](#vsscripts)
- [Przechowywanie danych poufnych na platformie Azure](#appsettings)
- [Przy użyciu narzędzia Git w programie Visual Studio i Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Traktuj skryptów automatyzacji jako kodu źródłowego

Podczas pracy nad projektem chmury często zmieniasz rzeczy i chcesz mieć możliwość szybkiego reagowania na problemy zgłaszane przez klientów. Odpowiada szybko polega na użyciu skryptów automatyzacji, zgodnie z objaśnieniem w [zautomatyzować wszystko](automate-everything.md) działu. Wszystkie skrypty, które służy do tworzenia środowiska, wdrożyć ją skali go itp., muszą być zsynchronizowane z kodu źródłowego aplikacji.

Aby zachować synchronizację z kodem skrypty, zapisanie ich w systemie kontroli źródła. Następnie trzeba wycofać zmiany lub wprowadzić szybkiej poprawki w kodzie produkcyjnym, który jest inny niż kod programowanie, masz marnować czas na próby śledzenia ustawienia, które zostały zmienione lub którzy członkowie zespołu są kopie wersji, które są potrzebne. W przypadku pewność, że skrypty, które są potrzebne są zsynchronizowane z potrzebne dla, która jest zapewniona, czy wszyscy członkowie zespołu pracy ze skryptami tej samej ścieżki bazowej kodu. Następnie czy potrzebujesz automatyzację testowania i wdrażania poprawki do produkcji, lub opracowanie nowych funkcji, będziesz mieć prawo skryptu do kodu, który musi zostać zaktualizowany.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Nie wyszukuj w kluczy tajnych

Repozytorium kodu źródłowego są zwykle dostępne dla zbyt wiele osób dla niego się odpowiednio bezpieczne miejsce dla danych poufnych, takie jak hasła. Jeśli skrypty bazuje na klucze tajne, takie jak hasła, parametryzacja tych ustawień, aby nie są zapisywane w kodzie źródłowym i przechowywać tajnych gdzie indziej.

Na przykład Azure umożliwia pobieranie plików, które zawierają opublikować ustawienia w celu automatyzacji tworzenia profilów publikowania. Te pliki zawierają nazwy użytkownika i hasła, które są autoryzowane do zarządzania usługami Azure. Jeśli możesz użyć tej metody do tworzenia profilów publikowania i po zaewidencjonowaniu tych plików do kontroli źródła, kto ma dostęp do repozytorium, zobacz te nazwy użytkownika i hasła. Można bezpiecznie przechowywać hasło w profil publikowania, ponieważ jest on zaszyfrowany i znajduje się w *. pubxml.user* plików, które domyślnie nie są uwzględnione w kontroli źródła.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Struktura źródła gałęzie, które mają ułatwić DevOps przepływu pracy

Jak zaimplementować gałęzi w repozytorium wpływa na możliwość zarówno rozwijania nowych funkcji i rozwiązywanie problemów w środowisku produkcyjnym. Oto czy partii ze średnim o rozmiarze zespoły Użyj wzorca:

![Struktura gałęzi źródłowej](source-control/_static/image1.png)

Gałęzi głównej zawsze zgodny kod, który znajduje się w środowisku produkcyjnym. Różne etapy odpowiada gałęzi poniżej wzorca w cyklu. Programowanie gałęzi jest, gdzie implementowania nowych funkcji. Dla małych zespołów może po prostu mają wzorca i rozwoju, ale często zalecamy czytelników tymczasowej gałęzi między programowanie i głównym. Tymczasowego służącego do końcowego integracji testowania przed update zostanie przeniesiony do produkcji.

Dla zespołów duży może być oddzielne gałęzi dla każdej nowej funkcji; mniejsze zespół może być wszyscy sprawdzanie w gałęzi programowanie.

Jeśli masz gałęzi dla każdej funkcji, gdy funkcja A jest gotowy możesz scalania jego zmiany kodu źródłowego się do rozwoju gałęzi i w dół na inne gałęzie funkcji. Ten kod źródłowy scalanie proces może zająć dużo czasu i aby uniknąć tej pracy, jednocześnie zachowując osobne funkcje, niektóre zespoły zaimplementować zamiast o nazwie *[funkcji przełącza](http://en.wikipedia.org/wiki/Feature_toggle)* (określane również jako *funkcji flagi*). Oznacza to, cały kod dla wszystkich funkcji znajduje się w tym samym oddziale, ale można włączyć lub wyłączyć poszczególne funkcje za pomocą przełączników w kodzie. Na przykład załóżmy, że funkcja A jest nowe pole poprawka aplikacji zadań, a funkcja B dodaje funkcjonalność buforowania. Kod obie funkcje mogą być w gałęzi programowanie, ale wyświetlanie tylko aplikacji będzie nowego pola, gdy zmienna ma ustawioną wartość true, a będzie używana tylko podczas różnych zmienna jest ustawiona na true. Jeśli funkcja A nie jest gotowa do można podwyższyć poziomu, ale funkcja B jest gotowy, możesz podwyższyć poziom cały kod do środowiska produkcyjnego w przełączniku funkcji A poza i przełączania B funkcji. Można następnie Zakończ A funkcji i Podwyższ go później, wszystkie nie scalanie kodu źródłowego.

Czy używać gałęzi lub przełącza dla funkcji, rozgałęziania strukturę to umożliwia przepływu kodu od projektowania do produkcji w sposób agile i repeatable.

Ta struktura pozwala również szybko reagować na opinie klientów. Jeśli trzeba wprowadzić szybkiej poprawki w środowisku produkcyjnym, możesz również to zrobić wydajnie w sposób elastyczne. Można utworzyć gałąź poza głównym lub przemieszczania, a kiedy będzie gotowy scalić ją do wzorca i w dół do gałęzi programowanie i funkcji.

![Poprawka gałęzi](source-control/_static/image2.png)

Bez struktury rozgałęziania takie z jego oddzielenie produkcyjnego i rozwoju gałęzie problem produkcyjnym można w zachowaniu pozycja konieczności podwyższyć nowy kod funkcji wraz z poprawka produkcji. Nowy kod funkcja może nie być w pełni przetestowane i gotowe do produkcji, i może być konieczne dużo pracy kopii zmiany, które nie są gotowe. Możesz też opóźnienie sieci poprawka przetestowane zmiany w celu ich przygotowania do wdrożenia.

Następnie zostanie wyświetlony przykłady sposobu wdrażania tych trzech wzorców w Visual Studio, Azure i programu Visual Studio Online. Oto przykłady zamiast szczegółowe instrukcje krok po kroku how-do-it; Aby uzyskać szczegółowe instrukcje zapewniających wszystkie niezbędne kontekstu, zobacz [zasobów](#resources) sekcji na końcu działu.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Dodawanie skryptów do kontroli źródła w programie Visual Studio

Istnieje możliwość dodania skryptów do kontroli źródła w programie Visual Studio, umieszczając je w folderze rozwiązania Visual Studio (przy założeniu, że Twój projekt jest w kontroli źródła). Oto jeden z tych metod.

Utwórz folder dla skryptów w folderze rozwiązania (tym samym folderze, który ma Twojej *.sln* pliku).

![Folder automatyzacji](source-control/_static/image3.png)

Kopiowanie plików skryptów do folderu.

![Zawartości folderu automatyzacji](source-control/_static/image4.png)

W programie Visual Studio należy dodać folder rozwiązania do projektu.

![Nowy Folder rozwiązania menu wyboru](source-control/_static/image5.png)

I Dodaj pliki skryptów do folderu rozwiązania.

![Dodaj istniejący element menu Zaznaczenie](source-control/_static/image6.png)

![Dodaj istniejący element — okno dialogowe](source-control/_static/image7.png)

Pliki skryptów są teraz zawarta w projekcie i śledzi zmiany wersji oraz odpowiednie zmiany kodu źródłowego w kontroli źródła.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Przechowywanie danych poufnych na platformie Azure

Jeśli uruchomienie aplikacji w witrynie sieci Web Azure sposobem uniknięcia przechowywania poświadczeń w kontroli źródła jest przechowywać je na platformie Azure.

Na przykład aplikacji Usuń są przechowywane w jego Web.config pliku dwa parametry połączenia mających hasła w środowisku produkcyjnym i klucz, który zapewnia dostęp do konta magazynu Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Wprowadza wartości rzeczywistej produkcji tych ustawień w Twojej *Web.config* pliku, lub jeśli umieszczone w *Web.Release.config* plik do skonfigurowania przekształcenie pliku Web.config, aby wstawić ich podczas wdrażania będą one przechowywane w repozytorium źródłowe. Wprowadź parametry połączenia bazy danych w środowisku produkcyjnym profil publikowania, hasło będzie w Twojej *.pubxml* pliku. (Można wykluczyć *.pubxml* plik z kontroli źródła, ale następnie utracone korzyści udostępniania wszystkie inne ustawienia wdrażania.)

Azure umożliwia alternatywę dla **appSettings** i parametry połączenia z sekcji *Web.config* pliku. Oto odpowiedniej części **konfiguracji** karty dla witryny sieci web w portalu zarządzania platformy Azure:

![appSettings i connectionStrings w portalu](source-control/_static/image8.png)

Podczas wdrażania projektu w tej witrynie sieci web i jest uruchamiana aplikacja, niezależnie od wartości są przechowywane na platformie Azure zastąpienie, niezależnie od wartości znajdują się w pliku Web.config.

Te wartości zostaną ustawione na platformie Azure za pomocą portalu zarządzania lub skryptów. Skrypt automatyzacji tworzenia środowiska opisany w [zautomatyzować wszystko](automate-everything.md) rozdział tworzy bazę danych SQL Azure, pobiera magazynu i parametry połączenia bazy danych SQL i przechowuje te klucze tajne w ustawieniach witryny sieci web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Zwróć uwagę, że skrypty mają zdefiniowane tak, aby repozytorium źródłowe nie pobrać utrwalone rzeczywiste wartości.

Po lokalnym uruchomieniu w środowisku projektowania aplikacji odczytuje lokalny plik Web.config i połączenia punktów ciąg do bazy danych LocalDB programu SQL Server w *aplikacji\_danych* folderu projektu sieci web. Po uruchomieniu aplikacji na platformie Azure i próbuje odczytać wartości z pliku Web.config aplikacji, co otrzymuje w odpowiedzi i używa są wartości przechowywanych dla witryny sieci Web, co jest rzeczywiście w pliku Web.config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Przy użyciu narzędzia Git w programie Visual Studio i Visual Studio Online

Wszystkie środowiska kontroli źródła służy do wdrożenia struktura rozgałęziania DevOps przedstawione wcześniej. Dla zespołów rozproszonej [Rozproszony system kontroli wersji](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) może najlepiej; dla innych zespołów [scentralizowane systemu](http://en.wikipedia.org/wiki/Revision_control) może działać lepiej.

[Git](http://git-scm.com/) jest DVCS, który jest stała się bardzo popularna. Korzystając z programu Git do kontroli źródła, masz pełną kopię repozytorium i wszystkie jego historii na komputerze lokalnym. Wiele osób preferowane czy ponieważ łatwiej aby kontynuować pracę, gdy nie masz połączenia z siecią — może być nadal wykonywać zatwierdza i wycofywanie zmian, Utwórz i przełączania gałęzi i tak dalej. Nawet wtedy, gdy masz połączenie z siecią, jest łatwiejsze i szybsze tworzenie gałęzi i przełączania gałęzi, gdy wszystko jest lokalny. Można także utworzyć lokalnego zatwierdzeniami i wycofań bez mających wpływ na inne deweloperów. I wsadowego zatwierdzeń przed ich wysłaniem do serwera.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), wcześniej znana jako Team Foundation Service oferuje zarówno Git i [Team Foundation Version Control](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; scentralizowane kontroli źródła). Tutaj w firmie Microsoft w grupie Azure niektóre zespoły użyć scentralizowane kontroli wersji, niektóre Użyj dystrybucji, a w niektórych mieszanego (scentralizowane dla niektórych projektów i dystrybuowane do innych projektów). Usługi VSO jest bezpłatna dla użytkowników do 5. Możesz utworzyć konto dla planu free [tutaj](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 obejmuje wbudowane najwyższej jakości [obsługi systemu Git](https://msdn.microsoft.com/library/hh850437.aspx); poniżej przedstawiono krótkie pokaz jak to zrobić.

Z projektem Otwórz w programie Visual Studio 2013, kliknij prawym przyciskiem myszy rozwiązanie w **Eksploratora rozwiązań**i wybierz polecenie **Dodaj rozwiązanie do kontroli źródła**.

![Dodaj rozwiązanie do kontroli źródła](source-control/_static/image9.png)

Visual Studio zapyta, jeśli chcesz użyć programu TFVC (kontrola wersji scentralizowane) lub Git.

![Wybierz kontroli źródła](source-control/_static/image10.png)

Gdy wybierz Git i kliknij przycisk **OK**, Visual Studio tworzy nowego lokalnego repozytorium Git w folderze rozwiązania. Nowe repozytorium nie ma jeszcze; plików należy dodać je do repozytorium, wykonując zatwierdzania Git. Kliknij prawym przyciskiem myszy rozwiązanie w **Eksploratora rozwiązań**, a następnie kliknij przycisk **zatwierdzić**.

![Zatwierdź](source-control/_static/image11.png)

Program Visual Studio automatycznie przygotowuje wszystkie pliki projektu do zatwierdzenia i wyświetla je w **Team Explorer** w **uwzględnione zmiany** okienka. (Jeśli zostały niektóre nie mają zostać uwzględnione w zatwierdzeniu, może wybrać je, kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **wykluczyć**.)

![Team Explorer](source-control/_static/image12.png)

Wprowadź komentarz zatwierdzania i kliknij przycisk **zatwierdzania**, i Visual Studio wykonuje zatwierdzenia i wyświetla identyfikator zatwierdzania.

![Team Explorer zmiany](source-control/_static/image13.png)

Teraz, aby była inna niż nowości w repozytorium w przypadku zmiany kodu, można łatwo przeglądać różnice. Kliknij prawym przyciskiem myszy plik, który zmieniono, wybierz **porównania z Unmodified**, możesz uzyskać wyświetlania porównania, która zawiera niezatwierdzone zmiany.

![Porównaj z zostały zmodyfikowane](source-control/_static/image14.png)

![Zmiany przedstawiający różnicowego](source-control/_static/image15.png)

Można łatwo sprawdzenia, jakie zmiany wprowadzania i wyewidencjonowywania.

Załóżmy, że konieczne jest wprowadzenie gałąź — można to zrobić w programie Visual Studio za. W **Team Explorer**, kliknij przycisk **nowa gałąź**.

![Team Explorer nowa gałąź](source-control/_static/image16.png)

Wprowadź nazwę gałęzi, kliknij przycisk **utwórz gałąź**, a w przypadku wybrania **gałęzi wyewidencjonowania**, Visual Studio automatycznie sprawdza się nowa gałąź.

![Team Explorer nowa gałąź](source-control/_static/image17.png)

Teraz można wprowadzić zmiany do plików i zaewidencjonować je do tego rozgałęzienia. I można łatwo przełączać się między gałęzie i Visual Studio automatycznie synchronizacje pliki rozgałęzić zależności można mieć wyewidencjonowane. W tym przykładzie tytuł strony sieci web, w  *\_Layout.cshtml* został zmieniony na "Poprawki 1" w HotFix1 gałęzi.

![Hotfix1 gałęzi](source-control/_static/image18.png)

Po przełączeniu do poziomu głównego gałęzi, zawartość  *\_Layout.cshtml* pliku automatycznie przywrócić co to są w gałęzi głównej.

![Głównej gałęzi](source-control/_static/image19.png)

Ten prosty przykład jak szybko utwórz gałąź i przerzucanie i z powrotem między gałęzi. Ta funkcja umożliwia bardzo elastyczne przepływu pracy przy użyciu struktury gałęzi i skryptów automatyzacji przedstawionych w [zautomatyzować wszystko](automate-everything.md) działu. Na przykład może być Praca w gałęzi programowanie, utwórz gałąź poprawki poza głównym, przejdź do nowej gałęzi, wprowadź zmiany i zatwierdzić je i następnie przełączyć z powrotem do gałęzi programowanie i kontynuować działania.

Opisane w tym miejscu jest sposób pracy z lokalnego repozytorium Git w programie Visual Studio. W środowisku zespołu można zwykle również Wypchnij zmiany do wspólnego repozytorium. Narzędzia Visual Studio umożliwiają również wskaż zdalnego repozytorium Git. Służy do tego celu witryną GitHub.com lub skorzystać z [Git w Visual Studio Online](https://msdn.microsoft.com/library/hh850437.aspx) zintegrowany z wszystkie inne Visual Studio Online funkcje takie jak elementu roboczego i śledzenia błędów.

Nie jest to jedyny sposób można zaimplementować strategię rozgałęziania agile oczywiście. Można włączyć tego samego przepływu pracy agile, przy użyciu repozytorium kontroli źródła scentralizowane.

## <a name="summary"></a>Podsumowanie

Zmierzenie powodzenia system kontroli źródła na podstawie jak szybko zmiany i pobrać go na żywo w sposób bezpieczny i przewidywalne. Jeśli okaże się Przerażony próbę dokonania zmiany, ponieważ musisz wykonać dzień lub dwa testowania ręcznego w nim, użytkownik może poprosić samodzielnie szablonu należy process-wise lub test-wise, dzięki czemu można wprowadzić zmiany w ciągu minut lub o najgorszej nie dłużej niż godzinę. Jedną ze strategii dla operacją jest zaimplementowanie ciągłej integracji i ciągłego dostarczania, które omówione zostaną następujące czynności w [następny rozdział](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Zasoby

[Visual Studio Online](https://www.visualstudio.com/) portal zawiera dokumentacja i usług pomocy technicznej i można założyć dla konta. Jeśli masz program Visual Studio 2012 i chcesz przy użyciu narzędzia Git, zobacz [Visual Studio Tools for Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Aby uzyskać więcej informacji na temat TFVC (kontrola wersji scentralizowane) i Git (kontrola wersji rozproszonej) zobacz następujące zasoby:

- [System kontroli wersji, które powinny używać: TFVC lub Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) Dokumentacji MSDN zawiera tabelę podsumowania różnice między TFVC i Git.
- [Również podoba Team Foundation Server i podoba Git, ale który jest lepszym rozwiązaniem?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Porównanie Git i TFVC.

Aby uzyskać więcej informacji na temat strategii rozgałęziania zobacz następujące zasoby:

- [Tworzenie potoku wersji z Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Dokumentacja firmy Microsoft Patterns and Practices. Zobacz rozdział 6 Omówienie strategii rozgałęziania. Funkcja obronie przełącza za pośrednictwem funkcji gałęzie i użycie gałęzi dla funkcji zaleca zachować ich krótkim okresie (godzin lub dni co najwyżej).
- [Kontrola wersji — przewodnik](https://aka.ms/vsarsolutions). Przewodnik po rozgałęziania strategii przez ALM Rangers. Na karcie pliki do pobrania, zobacz Strategies.pdf rozgałęziania.
- [Tworzenie oprogramowania z Włącza lub wyłącza funkcję](https://msdn.microsoft.com/magazine/dn683796.aspx). Artykuł MSDN Magazine.
- [Funkcja przełączania](http://martinfowler.com/bliki/FeatureToggle.html). Wprowadzenie do funkcji przełącza / funkcja flagi na blogu Fowler pole.
- [Funkcja przełącza vs gałęzie funkcji](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Inny wpis w blogu o Włącza lub wyłącza funkcję, przez Dylan Smith.

Aby uzyskać więcej informacji na temat obsługi poufne informacje, które nie powinny być przechowywane w repozytoria kontroli źródła zobacz następujące zasoby:

- [Najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Witryny sieci Web platformy Azure: Jak połączenia i ciągi aplikacji ciągi pracy](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Zawiera opis funkcji Azure zastępujący `appSettings` i `connectionStrings` danych w *Web.config* pliku.
- [Niestandardowe ustawienia konfiguracji i aplikacji w usłudze Azure witryn sieci Web — z Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Aby uzyskać informacje o innych metodach porządkowania poufnych informacji poza kontrolą źródła, zobacz [ASP.NET MVC: Zachowaj prywatnej ustawienia z kontroli źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Poprzednie](automate-everything.md)
> [dalej](continuous-integration-and-continuous-delivery.md)
