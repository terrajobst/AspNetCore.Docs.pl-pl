---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Typowe różnice konfiguracji między środowiskami deweloperskim i produkcyjnym (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W samouczkach wcześniej wdrożyliśmy naszej witryny sieci Web, kopiując wszystkie odpowiednie pliki ze środowiska projektowego do środowiska produkcyjnego. Jednak po...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3476cdb25a0fc87ce233d7e26de094a0b7b17c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808911"
---
<a name="common-configuration-differences-between-development-and-production-c"></a>Typowe różnice konfiguracji między środowiskami deweloperskim i produkcyjnym (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> W samouczkach wcześniej wdrożyliśmy naszej witryny sieci Web, kopiując wszystkie odpowiednie pliki ze środowiska projektowego do środowiska produkcyjnego. Jednak nie jest niczym niezwykłym istniała różnice konfiguracji między środowiskami, które wymaga, że każde środowisko ma unikatowy pliku Web.config. W tym samouczku sprawdza różnice w typowej konfiguracji i analizuje strategie utrzymania informacji konfiguracji.


## <a name="introduction"></a>Wprowadzenie


Ostatnie dwa samouczki przeprowadzony przez wdrażanie prostej aplikacji sieci web. [ *Wdrażanie witryny przy użyciu klienta FTP* ](deploying-your-site-using-an-ftp-client-cs.md) samouczku pokazano, jak za pomocą autonomicznego klienta FTP skopiuj niezbędne pliki ze środowiska projektowego do produkcji. Poprzedni Samouczek [ *wdrażanie Twojej witryny przy użyciu programu Visual Studio*](deploying-your-site-using-visual-studio-cs.md), przyjrzano się wdrożenie za pomocą narzędzia kopiowania witryny sieci Web programu Visual Studio i opcję Publikuj. W obu samouczki wszystkich plików w środowisku produkcyjnym była kopia pliku w środowisku programistycznym. Jednak nie jest niczym niezwykłym dla plików konfiguracyjnych w środowisku produkcyjnym mogą się różnić od tych w środowisku programistycznym. Konfiguracja aplikacji sieci web jest przechowywana w `Web.config` pliku i zwykle zawiera informacje na temat zasobów zewnętrznych, takich jak bazy danych, sieci web i serwerów poczty e-mail. Opisujemy również zachowanie aplikacji w niektórych sytuacjach, takich jak kurs akcję podejmowaną po wystąpieniu nieobsługiwanego wyjątku.

Wdrażanie aplikacji sieci web jest ważne, czy informacje o prawidłowej konfiguracji znajdą się w środowisku produkcyjnym. W większości przypadków `Web.config` nie można skopiować pliku w środowisku projektowym w środowisku produkcyjnym jako-to. Zamiast tego dostosowaną wersję `Web.config` musi zostać przekazany do środowiska produkcyjnego. Ten samouczek obejmuje krótki przegląd niektóre bardziej typowe różnice konfiguracji; zawiera podsumowanie również kilka technik do przechowywania informacji o różnych konfiguracji między środowiskami.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Typowa konfiguracja różnice między środowiskami deweloperskim i produkcyjnym.

`Web.config` Plik zawiera pewną liczbę informacji o konfiguracji dla aplikacji ASP.NET. Niektóre z tych informacji o konfiguracji jest taki sam, niezależnie od tego środowiska. Na przykład ustawienia uwierzytelniania i reguły autoryzacji adresów URL województw w `Web.config` pliku `<authentication>` i `<authorization>` elementy zazwyczaj są takie same, niezależnie od tego środowiska. Jednak inne informacje o konfiguracji — takie jak informacje o zasoby zewnętrzne — zwykle różni się w zależności od środowiska.

Parametry połączenia bazy danych to podstawowy przykład informacje o konfiguracji, która różni się opartych na środowisku. Gdy aplikacja sieci web komunikuje się z serwerem bazy danych je najpierw nawiązać połączenie i które odbywa się za pośrednictwem [parametry połączenia](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Choć jest możliwe, aby trwale kodować parametry połączenia bazy danych bezpośrednio na stronach sieci web lub kod, który nawiązuje połączenie z bazą danych, najlepiej umieścić `Web.config`firmy [ `<connectionStrings>` elementu](https://msdn.microsoft.com/library/bf7sd233.aspx) tak, aby parametry połączenia informacje są w jednym, scentralizowanej lokalizacji. Często innej bazy danych jest używany podczas tworzenia nie jest używana w środowisku produkcyjnym; w związku z tym informacje o parametrach połączenia musi być unikatowy dla każdego środowiska.

> [!NOTE]
> Samouczki przyszłych Eksplorowanie, wdrażanie aplikacji opartych na danych, w tym momencie będziesz przejdziemy do szczegółowych informacji o jak parametry połączenia bazy danych są przechowywane w pliku konfiguracji.


Znacznie różni się to oczekiwane zachowanie w środowiskach deweloperskich i produkcyjnych. Aplikacji sieci web w środowisku deweloperskim jest tworzony, przetestowane i debugowania przez małe grupy deweloperów. W środowisku produkcyjnym, czy ta sama aplikacja jest są kontrolowane przez wielu równoczesnych użytkowników. Program ASP.NET zawiera pewną liczbę funkcji, które pomagają deweloperom testowanie i debugowanie aplikacji, ale te funkcje powinny być wyłączone dla bezpieczeństwo i wydajność w środowisku produkcyjnym. Przyjrzyjmy się kilku takie ustawienia konfiguracji.

### <a name="configuration-settings-that-impact-performance"></a>Ustawienia konfiguracji, które mają wpływ na wydajność

W przypadku odwiedzenia strony ASP.NET po raz pierwszy (lub po raz pierwszy po jej zmianie), jego oznaczeniu deklaracyjnym muszą zostać przekonwertowane na klasę i muszą być skompilowane tej klasy. Jeśli aplikacja sieci web używa automatycznych kompilacji klasy CodeBehind strony musi można skompilować za. Można skonfigurować różne opcje kompilacji za pomocą `Web.config` pliku [ `<compilation>` elementu](https://msdn.microsoft.com/library/s10awwz0.aspx).

Atrybut debug jest jednym z najważniejszych atrybutów w `<compilation>` elementu. Jeśli `debug` atrybut jest ustawiony na "true", a następnie skompilowanych zestawów dołączać symbole debugowania, które są potrzebne w przypadku debugowania aplikacji w programie Visual Studio. Ale symbole debugowania zwiększają rozmiar zestawu i nakłada wymagań dodatkowej pamięci podczas uruchamiania kodu. Ponadto, jeśli `debug` atrybut jest ustawiony na "true" żadnej zawartości, zwracany przez `WebResource.axd` nie są buforowane, co oznacza każdorazowo odwiedzać strony, należy ponownie pobrać zawartości statycznej, zwracany przez `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` wbudowany program obsługi HTTP wprowadzono w programie ASP.NET 2.0, aby pobrać zasoby osadzone, takie jak pliki skryptów, obrazy, pliki CSS i innej zawartości, użyj formantów serwera. Aby uzyskać więcej informacji na temat `WebResource.axd` działa i jak go używać dostępu do zasobów osadzonych w swojej niestandardowych kontrolek serwera, zobacz [uzyskiwania dostępu do osadzonych zasobów za pomocą adresu URL przy użyciu `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


`<compilation>` Elementu `debug` atrybut jest zazwyczaj równa "true" w środowisku programistycznym. W rzeczywistości ten atrybut musi być równa "true", aby debugować aplikację sieci web; Jeśli podczas próby debugowania aplikacji ASP.NET w programie Visual Studio i `debug` atrybut jest ustawiony na "false", programu Visual Studio wyświetli komunikat wyjaśniający, dopóki nie można debugować aplikację `debug` atrybut jest ustawiony na "true" która ma oferty wprowadzić tę zmianę automatycznie.

Należy **nigdy nie** mają `debug` atrybut na wartość "true" w środowisku produkcyjnym ze względu na jej wpływ na wydajność. Uzyskać dokładniejsze omówienie na ten temat można znaleźć [Scott Guthrie](https://weblogs.asp.net/scottgu/)firmy wpis w blogu [nie Uruchom produkcyjne ASP.NET aplikacji za pomocą `debug="true"` włączone](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Błędy niestandardowe i śledzenie

Po wystąpieniu nieobsługiwanego wyjątku w aplikacji ASP.NET rozpropagowany środowiska uruchomieniowego, w tym momencie jedno z trzech zdarzeń sytuacji:

- Runtime ogólny komunikat o błędzie jest wyświetlany. Ta strona informuje użytkownika, który był błąd w czasie wykonywania, ale nie zapewnia żadnych szczegółów dotyczących błędu.
- Wyświetlany jest komunikat szczegóły wyjątku, który zawiera informacje na temat wyjątków, który właśnie został zgłoszony.
- Zostanie wyświetlona strona błędu niestandardowego, czyli strony ASP.NET, możesz utworzyć wyświetlającą każdy komunikat, który chcesz.

Co się dzieje w przypadku nieobsługiwanego wyjątku jest zależna od `Web.config` pliku [ `<customErrors>` sekcji](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Podczas opracowywania i testowania aplikacji pomaga szczegółowe informacje o wszelkich wyjątków w przeglądarce. Wyświetlanie szczegółów wyjątku w aplikacji w środowisku produkcyjnym jest jednak potencjalne zagrożenie bezpieczeństwa. Ponadto jest on unflattering i sprawia, że witryny sieci Web wyglądają nieprofesjonalnie. W idealnym przypadku nieobsługiwanego wyjątku aplikacji sieci web w środowisku programistycznym pokaże szczegóły wyjątku podczas tej samej aplikacji w środowisku produkcyjnym wyświetli stronę błędu niestandardowego.

> [!NOTE]
> Wartość domyślna `<customErrors>` ustawienie sekcji przedstawiono szczegóły wyjątku komunikatu tylko wtedy, gdy strona jest odwiedzana za pośrednictwem hosta lokalnego i pokazuje stronę błędu ogólnego środowiska uruchomieniowego, w przeciwnym razie. To nie jest najlepszym rozwiązaniem, ale jest zapewniając, aby dowiedzieć się, że domyślne zachowanie nie zawiera szczegóły wyjątku dla osób odwiedzających inną niż lokalna. Sprawdza, czy przyszłych zapoznać się z samouczkiem `<customErrors>` sekcji bardziej szczegółowo i pokazuje, jak mają niestandardowej strony błędu pokazano, w przypadku wystąpienia błędu w środowisku produkcyjnym.


Śledzenie jest inna funkcja platformy ASP.NET, które są przydatne podczas programowania. Śledzenie, jeśli włączone, rejestruje informacje o poszczególnych żądań przychodzących i zapewnia specjalną stroną sieci web, `Trace.axd`, w celu wyświetlania najnowsze informacje dotyczące żądania. Można włączyć i skonfigurować śledzenie za pomocą [ `<trace>` elementu](https://msdn.microsoft.com/library/6915t83k.aspx) w `Web.config`.

Po włączeniu śledzenia upewnij się, że jej jest wyłączona w środowisku produkcyjnym. Informacje o śledzeniu zawiera pliki cookie, dane sesji i inne potencjalnie poufne informacje, należy wyłączyć śledzenie w środowisku produkcyjnym. Dobra wiadomość jest taka, że domyślnie śledzenie jest wyłączone i `Trace.axd` plik jest tylko dostępne za pośrednictwem hosta lokalnego. Jeśli możesz zmienić te ustawienia domyślne w rozwoju upewnij się, że są wyłączone ponownie w środowisku produkcyjnym.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniki obsługi informacji o konfiguracji

Posiadanie różnych ustawień konfiguracji w środowisku deweloperskim i produkcyjnym komplikuje procesu wdrażania. W poprzednich samouczkach dwóch proces wdrażania obejmuje kopiowanie wszystkie niezbędne pliki od projektowania do produkcji, ale ta metoda działa tylko, jeśli informacje o konfiguracji są takie same w obu środowiskach. Istnieją różne techniki wdrażania aplikacji przy użyciu różnych informacji o konfiguracji. Spróbujmy wykazu te opcje hostowane aplikacje sieci web.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Ręczne wdrażanie pliku konfiguracji środowiska produkcyjnego

Najprostszą metodą jest obsługa dwie wersje `Web.config` pliku: jeden dla środowiska programistycznego i jeden dla środowiska produkcyjnego. Wdrażanie witryny do środowiska produkcyjnego pociąga za sobą kopiowanie wszystkich plików na serwer produkcyjny w środowisku programistycznym *z wyjątkiem* dla `Web.config` pliku. Zamiast tego konkretnego środowiska produkcyjnego `Web.config` plik jest kopiowany do produkcji.

To podejście nie jest bardzo zaawansowane, ale można łatwo zaimplementować, ponieważ informacje o konfiguracji zmieniają się rzadko. Działa najlepiej w przypadku aplikacji o mały zespół deweloperów, które są hostowane na jednym serwerze sieci web i którego informacje o konfiguracji rzadko jest zmieniany. Może to być najbardziej firmy tenable, w przypadku ręcznego wdrażania plików aplikacji przy użyciu klienta FTP autonomicznego. Podczas korzystania z narzędzia kopiowania witryny sieci Web programu Visual Studio lub opcji Publikuj musisz najpierw wymienić charakterystyczne dla wdrożenia `Web.config` plik z jednym specyficzne dla produkcji, przed wdrożeniem i zamień je ponownie, po ukończeniu wdrażania.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Zmień konfigurację podczas kompilacji lub proces wdrażania

Dyskusje dotychczasowych mają zakłada, że ad-hoc kompilacją i procesem wdrażania. Dużo większe projekty oprogramowania mają więcej sformalizowane procesy, użyj typu "open-source" rozwijane w domu, lub narzędzi innych firm. W przypadku takich projektów prawdopodobnie można dostosować proces kompilacji lub wdrożenia, aby odpowiednio zmodyfikować informacje o konfiguracji, zanim zostanie przypisany do środowiska produkcyjnego. W przypadku tworzenia usługi sieci web aplikacji za pomocą [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), lub inne narzędzie kompilację, prawdopodobnie Dodaj krok kompilacji, aby zmodyfikować `Web.config` pliku, aby uwzględnić ustawienia specyficzne dla produkcji. Wdrażanie przepływu pracy można programowo nawiązać połączenie z serwerem kontroli źródła i pobrać odpowiednią `Web.config` pliku.

Rzeczywiste podejście w celu uzyskania informacji o odpowiedniej konfiguracji do środowiska produkcyjnego zależy powszechnie przepływ pracy i narzędzia. Jako takie firma Microsoft będzie nie delve do dalszego w tym temacie. Jeśli używasz narzędzia kompilacji popularne, takiego jak programu MSBuild lub NAnt można znaleźć wdrażania i samouczki specyficzne dla tych narzędzi za pomocą funkcji wyszukiwania w sieci web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Zarządzanie różnice konfiguracji za pomocą wdrażania w Internecie projektu dodatku

W 2006 roku firma Microsoft opublikowała dodatek projekt rozwoju sieci Web programu Visual Studio 2005. Dodatek dla programu Visual Studio 2008 został wydany w 2008 roku. Ten dodatek umożliwia dla deweloperów platformy ASP.NET utworzyć oddzielny Projekt wdrożenia sieci Web, wraz z ich projektu aplikacji sieci web, podczas kompilowania jawnie kompiluje aplikację sieci web i kopiuje pliki do wdrożenia w katalogu lokalnym danych wyjściowych. Projekt aplikacji sieci Web używa programu MSBuild w tle.

Domyślnie środowisko programistyczne firmy `Web.config` plik jest kopiowany do katalogu wyjściowego, ale można skonfigurować projektu wdrożenia w Internecie do dostosowywania

informacje o konfiguracji, który pobiera skopiowany do tego katalogu w następujący sposób:

- Za pomocą `Web.config` plików zastąpienie sekcji, w której zostaną określone sekcji, aby zastąpić i pliku XML, który zawiera tekst zastępczy.
- Podając ścieżkę do pliku konfiguracji zewnętrznego źródła. Ta opcja jest zaznaczona, projektu wdrożenia w Internecie kopiuje określonego `Web.config` plik do katalogu wyjściowego (zamiast `Web.config` plik używany w środowisku programistycznym).
- Przez dodanie reguły niestandardowe do pliku programu MSBuild używany przez projektu wdrożenia w Internecie.

Aby wdrożyć kompilację aplikacji sieci web projektu wdrożenia w Internecie, a następnie skopiuj pliki z folderu danych wyjściowych projektu do środowiska produkcyjnego.

Aby dowiedzieć się więcej o korzystaniu z projektu wdrożenia w Internecie, sprawdź [w tym artykule projekty wdrażania w Internecie](https://msdn.microsoft.com/magazine/cc163448.aspx) z emisji kwietnia 2007 [magazynu MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), lub zapoznaj się z linków w sekcji dalsze informacje zakończenie tego samouczka.

> [!NOTE]
> Nie można użyć projektu wdrażania sieci Web za pomocą programu Visual Web Developer, ponieważ projektu wdrożenia w Internecie jest implementowany jako Visual Studio dodatek programu i program Visual Studio Express (w tym Visual Web Developer) nie obsługują dodatków.


## <a name="summary"></a>Podsumowanie

Zasoby zewnętrzne i zachowanie aplikacji sieci web w taki sposób, w trakcie opracowywania są zazwyczaj inny niż po tej samej aplikacji w środowisku produkcyjnym. Na przykład parametry połączenia bazy danych, opcji kompilacji i zachowanie, gdy wystąpił nieobsługiwany wyjątek występuje często różnią się między środowiskami. Proces wdrażania należy uwzględnić te różnice. Jak wspomniano w tym samouczku najprostszą metodą jest ręcznie skopiować plik konfiguracji alternatywnej do środowiska produkcyjnego. Możliwe są bardziej elegancki rozwiązania przy użyciu dodatku projektu wdrażania sieci Web lub z bardziej formalny proces kompilacji lub wdrożenia, który może obsłużyć takie dostosowania.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Parametry połączenia, które wyjaśniono](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Połączenie z bazą danych ciągów @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Nie uruchamiaj produkcyjnych aplikacji ASP.NET przy użyciu `debug="true"` włączone](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Bez problemu zmieniała odpowiadanie na nieobsługiwane wyjątki — wyświetlanie strony błędów przyjazny dla użytkownika](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Jak mogę Użycie programu Visual Studio 2008 projektu sieci Web wdrożenia?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Ustawienia konfiguracji klucza podczas wdrażania bazy danych](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 wdrażania projektów pobieranie z sieci Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 wdrażania projektów pobieranie z sieci Web](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Projekty wdrażania w Internecie 2008 VS](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 wdrożenie projektu do pomocy przez Internet wydania](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projekty wdrażania w Internecie](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-your-site-using-visual-studio-cs.md)
> [dalej](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
