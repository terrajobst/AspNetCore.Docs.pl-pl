---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: "Typowych konfiguracji różnice między rozwoju i produkcji (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W samouczkach wcześniejszych wdrożyliśmy naszą witrynę sieci Web przez skopiowanie wszystkie odpowiednie pliki środowiska programistycznego do środowiska produkcyjnego. Jednak i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 092362e3811213047820dab08efc16e1a1e75020
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="common-configuration-differences-between-development-and-production-c"></a>Typowych konfiguracji różnice między rozwoju i produkcji (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> W samouczkach wcześniejszych wdrożyliśmy naszą witrynę sieci Web przez skopiowanie wszystkie odpowiednie pliki środowiska programistycznego do środowiska produkcyjnego. Jednak nie jest rzadko w celu zapewnienia konfiguracji różnice między środowiskami, które wymaga, że każde środowisko ma unikatowy plik Web.config. W tym samouczku sprawdza różnice typowej konfiguracji i analizuje strategii do przechowywania informacji o konfiguracji oddzielne.


## <a name="introduction"></a>Wprowadzenie


Ostatnie dwa samouczki udał kroków wdrażania prostą aplikację sieci web. [ *Wdrażanie witryny przy użyciu klienta FTP* ](deploying-your-site-using-an-ftp-client-cs.md) samouczku przedstawiono sposób użycia autonomicznego klient FTP do skopiowania niezbędne pliki środowiska programistycznego do produkcji. Samouczek poprzedniego [ *wdrażanie Your lokacji za pomocą programu Visual Studio*](deploying-your-site-using-visual-studio-cs.md), przeglądał wdrożenia przy użyciu narzędzia kopiowanie witryny sieci Web i Publikuj Visual Studio. W obu samouczki wszystkich plików w środowisku produkcyjnym było kopię pliku na środowisko deweloperskie. Jednak nie jest nietypowe dla plików konfiguracyjnych w środowisku produkcyjnym w celu różnią się od tych w środowisku programistycznym. Konfiguracja aplikacji sieci web jest przechowywana w `Web.config` pliku i zwykle zawiera informacje na temat zasobów zewnętrznych, takich jak bazy danych, sieci web i serwerach poczty e-mail. Wyszczególnia ona również zachowanie aplikacji, w niektórych sytuacjach, takich jak sposób postępowania w sytuacji, gdy wystąpi nieobsługiwany wyjątek.

W przypadku wdrażania aplikacji sieci web jest ważne, czy informacje o prawidłowej konfiguracji end w środowisku produkcyjnym. W większości przypadków `Web.config` nie można skopiować pliku w środowisku programistycznym do środowiska produkcyjnego jako — jest. Zamiast tego dostosowaną wersję `Web.config` musi zostać przekazana do produkcji. W tym samouczku krótko przegląda niektóre z najczęściej różnice konfiguracji; także podsumowanie niektóre techniki do przechowywania informacji o różnych konfiguracji między środowiskami.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Typowa konfiguracja różnice między opracowywania i środowisk produkcyjnych

`Web.config` Plik zawiera pewną liczbę informacji o konfiguracji dla aplikacji ASP.NET. Niektóre z tych informacji o konfiguracji jest taki sam, niezależnie od tego środowiska. Na przykład ustawienia uwierzytelniania i reguł autoryzacji adresów URL zapisane w `Web.config` pliku `<authentication>` i `<authorization>` elementy zazwyczaj są takie same, niezależnie od tego środowiska. Jednak inne informacje o konfiguracji — takie jak informacje dotyczące zasobów zewnętrznych — zwykle zależy od tego środowiska.

Parametry połączenia bazy danych są podstawowym przykład informacje o konfiguracji, która różni się na podstawie środowiska. Gdy aplikacji sieci web komunikuje się z serwerem bazy danych musi nawiązania połączenia i który odbywa się za pośrednictwem [ciąg połączenia](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Mimo że jest możliwe kodowane parametry połączenia bazy danych bezpośrednio w stron sieci web lub kod, który nawiązuje połączenie z bazą danych, najlepiej umieścić `Web.config`w [ `<connectionStrings>` elementu](https://msdn.microsoft.com/library/bf7sd233.aspx) , aby parametry połączenia informacje są w jednej, scentralizowanej lokalizacji. Często innej bazy danych jest używany podczas tworzenia nie jest używany w środowisku produkcyjnym; w związku z tym informacje o parametrach połączenia musi być unikatowa dla każdego środowiska.

> [!NOTE]
> Samouczki przyszłych Poznaj wdrażanie aplikacji opartych na danych, w którym firma Microsoft będzie Poznaj specyfice jak parametry połączenia bazy danych są przechowywane w pliku konfiguracji.


Znacznie różni się to oczekiwane zachowanie środowiska projektowania i produkcji. Aplikacji sieci web w środowisku programistycznym jest tworzone, przetestowany i debugowanym przez niewielka grupa deweloperów. W środowisku produkcyjnym wielu równoczesnych użytkowników jest odwiedzana tej samej aplikacji. Program ASP.NET zawiera szereg funkcji, które ułatwiają deweloperom testowanie i debugowanie aplikacji, ale te funkcje powinny być wyłączone dla bezpieczeństwo i wydajność w środowisku produkcyjnym. Oto kilka takie ustawienia konfiguracji.

### <a name="configuration-settings-that-impact-performance"></a>Ustawienia konfiguracji, które mają wpływ na wydajność

W przypadku odwiedzenia strony ASP.NET po raz pierwszy (lub po raz pierwszy po zmianie jego), jego deklaratywne znaczników muszą zostać przekonwertowane na klasę i muszą być skompilowane tej klasy. Jeśli aplikacja sieci web używa automatyczne kompilowanie klasie związanej z kodem strony musi można skompilować za. Można skonfigurować różne opcje kompilacji za pomocą `Web.config` pliku [ `<compilation>` elementu](https://msdn.microsoft.com/library/s10awwz0.aspx).

Atrybut debugowania jest jednym z najważniejszych atrybutów w `<compilation>` elementu. Jeśli `debug` atrybut ma wartość "true", a następnie skompilowane zestawy dołączać symbole debugowania, które są potrzebne podczas debugowania aplikacji w programie Visual Studio. Jednak symboli debugowania zwiększyć rozmiar zestawu i nałożyć wymagań dodatkowej pamięci podczas uruchamiania kodu. Ponadto, jeśli `debug` atrybut ma wartość "true" żadnej zawartości zwrócony przez `WebResource.axd` nie są buforowane, co oznacza zawsze odwiedzać strony, będzie musiał ponownie pobrać zawartość statyczną zwrócony przez `WebResource.axd`.

> [!NOTE]
> `WebResource.axd`wbudowana obsługa HTTP wprowadzony w programie ASP.NET 2.0 kontrolki serwera służy do pobierania zasoby osadzone, takie jak pliki skryptów, obrazów, plików CSS i innej zawartości. Aby uzyskać więcej informacji na temat `WebResource.axd` działa i jak umożliwia dostęp do zasobów osadzonych z Twojej niestandardowe kontrolki serwera, zobacz [podczas uzyskiwania dostępu do osadzonych zasobów za pomocą adresu URL za pomocą `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


`<compilation>` Elementu `debug` atrybut zazwyczaj ma wartość "true" w środowisku programistycznym. W rzeczywistości ten atrybut musi mieć ustawioną "true", aby debugować aplikację sieci web; Jeśli spróbujesz debugowania aplikacji ASP.NET w programie Visual Studio i `debug` atrybut ma ustawioną wartość "false", Visual Studio będzie wyświetlany komunikat wyjaśniający, że nie można debugować aplikacji do `debug` atrybut ma wartość "true" która ma Oferta dokonać tej zmiany.

Należy **nigdy nie** ma `debug` atrybut na wartość "true" w środowisku produkcyjnym ze względu na jego wpływ na wydajność. Bardziej szczegółowe omówienie na ten temat, zapoznaj się [Scott Guthrie](https://weblogs.asp.net/scottgu/)w blogu, [nie Uruchom produkcji ASP.NET aplikacji z `debug="true"` włączone](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Błędy niestandardowe i śledzenie

Gdy wystąpi nieobsługiwany wyjątek w aplikacji ASP.NET propaguje go do środowiska uruchomieniowego w takim przypadku jedna z trzech zdarzeń sytuacji:

- Zostanie wyświetlony komunikat błędu ogólnego środowiska wykonawczego. Ta strona informuje użytkownika, dla którego wystąpił błąd środowiska uruchomieniowego, ale nie zapewnia żadnych informacji o tym błędzie.
- Zostanie wyświetlony komunikat szczegóły wyjątek, który zawiera informacje o wyjątku, który właśnie został zgłoszony.
- Zostanie wyświetlona strona błędu niestandardowego, który jest strony ASP.NET utworzonego wyświetlający wszystkie wiadomości, którą chcesz.

Co się stanie w wypadku nieobsługiwany wyjątek zależy od `Web.config` pliku [ `<customErrors>` sekcji](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Podczas tworzenia i testowania aplikacji pozwala wyświetlić szczegóły wyjątku w przeglądarce. Jednak wyświetlanie szczegółów wyjątku w aplikacji produkcyjnych jest potencjalne zagrożenie dla bezpieczeństwa. Ponadto jest unflattering i sprawia, że wygląd szkodzi witryny sieci Web. Najlepiej, jeśli w przypadku nieobsługiwany wyjątek aplikacji sieci web w środowisku programistycznym zostaną wyświetlone szczegóły wyjątku podczas tej samej aplikacji w środowisku produkcyjnym wyświetli stronę błędu niestandardowego.

> [!NOTE]
> Wartość domyślna `<customErrors>` ustawienie sekcji przedstawiono szczegóły wyjątku wiadomości tylko wtedy, gdy strona jest odwiedzana za pośrednictwem hosta lokalnego i pokazuje stronę błędu ogólnego środowiska uruchomieniowego w przeciwnym razie wartość. To nie jest najlepszym rozwiązaniem, ale jest zapewniając, aby dowiedzieć się, że domyślne zachowanie nie zawiera szczegóły wyjątku do innego niż lokalne odwiedzający. Samouczek przyszłych sprawdza `<customErrors>` sekcji bardziej szczegółowo i pokazuje, jak strona błędów niestandardowych wyświetlane, gdy wystąpi błąd w środowisku produkcyjnym.


Śledzenie jest inna funkcja ASP.NET, które są przydatne podczas programowania. Śledzenie, jeśli włączone, rejestruje informacje o poszczególnych żądań przychodzących i zapewnia specjalną stroną sieci web, `Trace.axd`, podczas wyświetlania szczegółów ostatniego żądania. Można włączyć i skonfigurować śledzenie za pośrednictwem [ `<trace>` elementu](https://msdn.microsoft.com/library/6915t83k.aspx) w `Web.config`.

Jeśli zostanie włączone śledzenie upewnij się, że jego jest wyłączona w środowisku produkcyjnym. Ponieważ informacje o śledzeniu obejmuje plików cookie, dane sesji i inne potencjalnie wrażliwe informacje, należy wyłączyć śledzenie w środowisku produkcyjnym. Dobre wieści jest, że domyślnie śledzenie jest wyłączone i `Trace.axd` plik tylko jest dostępny za pośrednictwem hosta lokalnego. Jeśli zmienisz te ustawienia domyślne w rozwoju upewnij się, że są wyłączone ponownie w środowisku produkcyjnym.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniki do przechowywania informacji o konfiguracji oddzielne

Posiadanie różnych ustawień konfiguracji środowiska projektowania i produkcji komplikuje procesu wdrażania. W poprzednich dwóch samouczki procesu wdrażania związane z kopiowanie wszystkie niezbędne pliki od projektowania do produkcji, ale ta metoda działa tylko, jeśli informacje o konfiguracji jest taka sama w obu środowiskach. Istnieją różne metody wdrażania aplikacji przy użyciu różnych informacji o konfiguracji. Załóżmy katalogu niektóre z tych opcji dla aplikacji sieci web hostowanej.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Ręczne wdrażanie pliku konfiguracji środowiska produkcyjnego

Najprostszym sposobem jest obsługa dwie wersje `Web.config` plik: jeden dla środowiska programowania i jeden w środowisku produkcyjnym. Wdrażanie w środowisku produkcyjnym witryny pociąga za sobą kopiowanie wszystkich plików na serwerze produkcyjnym w środowisku programistycznym *z wyjątkiem* dla `Web.config` pliku. Zamiast tego określonego środowiska produkcyjnego `Web.config` pliku jest kopiowany do środowiska produkcyjnego.

Ta metoda nie jest bardzo zaawansowane, ale jest łatwa do wdrożenia, ponieważ informacje o konfiguracji zmienia się rzadko. Działa on najlepsze dla aplikacji z zespołem małych programowanie na jednym serwerze sieci web hostowanych i którego informacje o konfiguracji jest rzadko zmieniane. Jest najbardziej firmy tenable w przypadku ręcznego wdrażania pliki aplikacji przy użyciu klienta FTP autonomicznego. Podczas korzystania z witryny sieci Web kopię narzędzia Visual Studio lub opcji Publikuj musisz najpierw wymienić dotyczące wdrażania `Web.config` plik z elementem specyficzne dla produkcji, przed wdrożeniem, a następnie zamienić je ponownie po zakończeniu wdrożenia.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Zmień konfigurację podczas kompilacji lub proces wdrażania

Dyskusji dotychczasowych że ad hoc kompilacją i procesem wdrażania. Wiele projektów większych oprogramowania ma więcej sformalizowane procesów, które Użyj open source, rozwijane w domu, lub narzędzi innych firm. Dla projektów prawdopodobnie można dostosować kompilację lub wdrożenie procesu odpowiednio modyfikowania informacji o konfiguracji, zanim zostanie przypisany do środowiska produkcyjnego. W przypadku tworzenia używa aplikacja sieci web [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), lub inne narzędzie kompilacji, prawdopodobnie dodać kroku kompilacji, aby zmodyfikować `Web.config` pliku, aby uwzględnić ustawienia specyficzne dla produkcji. Lub przepływ pracy wdrażania programowego można nawiązać połączenia z serwerem kontroli źródła i pobrać odpowiednią `Web.config` pliku.

Metoda pobierania informacji o prawidłowej konfiguracji w środowisku produkcyjnym zależy powszechnie przepływ pracy i narzędzia. Tak firma Microsoft będzie nie delve do dalszego w tym temacie. Jeśli używasz narzędzia kompilacji popularnych, takiego jak MSBuild lub NAnt można znaleźć wdrażania i samouczki dotyczące tych narzędzi, za pomocą wyszukiwania w sieci web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Zarządzanie różnice konfiguracji za pośrednictwem sieci Web wdrażania projektu dodatku

W 2006 roku firma Microsoft opublikowała Web Development projektu dodatek dla programu Visual Studio 2005. Dodatek dla programu Visual Studio 2008 został wydany w 2008. Ten dodatek umożliwia dla deweloperów platformy ASP.NET utworzyć oddzielny Projekt wdrożenia sieci Web obok ich projektu aplikacji sieci web, podczas tworzenia, jawnie kompiluje aplikacji sieci web i kopiuje pliki do wdrożenia w katalogu lokalnym danych wyjściowych. Projekt aplikacji sieci Web używa programu MSBuild w tle.

Domyślnie środowisko projektowe w `Web.config` plik jest kopiowany do katalogu wyjściowego, ale można skonfigurować wdrożenie projektu sieci Web do dostosowania

informacje o konfiguracji, które są kopiowane do tego katalogu, w następujący sposób:

- Za pomocą `Web.config` plików zastąpienie sekcji, w którym można określić sekcji, aby zastąpić i pliku XML, który zawiera tekst zastępczy.
- Podając ścieżkę do pliku konfiguracyjnego zewnętrzne źródło. W przypadku wybrania tej opcji wdrażania projektu sieci Web kopiuje określonego `Web.config` plik do katalogu wyjściowego (a nie `Web.config` plik używany w środowisku programistycznym).
- Przez dodanie reguły niestandardowe do plików programu MSBuild używany przez wdrożenie projektu sieci Web.

Aby wdrożyć kompilację aplikacji sieci web wdrażanie projektu sieci Web, a następnie skopiuj pliki z folderu wyjściowego projektu do środowiska produkcyjnego.

Aby dowiedzieć się więcej o korzystaniu z sieci Web projektu wdrożenia wyewidencjonować [w tym artykule projekty wdrażania Web](https://msdn.microsoft.com/magazine/cc163448.aspx) z kwietnia 2007 emisji [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), lub skontaktuj się z łączy w sekcji dalsze informacje na zakończenie tego samouczka.

> [!NOTE]
> Wdrażanie projektu sieci Web nie można użyć z programu Visual Web Developer, ponieważ projekt wdrożenia sieci Web jest zaimplementowany jako Visual Studio Add-In i wersje Visual Studio Express (w tym Visual Web Developer) nie obsługują Add-Ins.


## <a name="summary"></a>Podsumowanie

Zasoby zewnętrzne i zachowanie aplikacji sieci web do rozwoju są zazwyczaj inne niż gdy ta sama aplikacja jest w środowisku produkcyjnym. Na przykład parametry połączenia bazy danych, opcje kompilacji i zachowania, gdy wystąpił nieobsługiwany wyjątek występuje często różnią się między środowiskami. Proces wdrażania należy uwzględnić te różnice. Jak wspomniano w tym samouczku Najprostszym rozwiązaniem jest ręcznie skopiować plik konfiguracji alternatywnej do środowiska produkcyjnego. Możliwe są bardziej elegancki rozwiązań za pomocą dodatku projektu wdrożenia sieci Web lub z procesem bardziej sformalizowane kompilacji lub wdrożenia, która może obsłużyć takie dostosowania.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Parametry połączenia wyjaśniono](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com parametry połączenia z bazą danych](http://www.connectionstrings.com/)
- [Nie uruchamiaj aplikacji ASP.NET produkcyjnych z `debug="true"` włączone](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Bezpiecznie odpowiada na żądania nieobsługiwanych wyjątków — wyświetlanie stron błędów przyjazną dla użytkownika](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Jak I: Użyj programu Visual Studio 2008 projektu sieci Web wdrożenia?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Ustawienia konfiguracji klucza podczas wdrażania bazy danych](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 Web wdrażania projektów pobierania](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [programu Visual Studio 2005 wdrażania projektów pobieranie z sieci Web](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Projekty wdrażania Web 2008 VS](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [wydane VS 2008 Web wdrażania projektu obsługi](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projekty wdrażania w Internecie](https://msdn.microsoft.com/magazine/cc163448.aspx)

>[!div class="step-by-step"]
[Poprzednie](deploying-your-site-using-visual-studio-cs.md)
[dalej](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
