---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Rejestrowanie szczegółów błędów za pomocą biblioteki ELMAH (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Błąd rejestrowania moduły i programy obsługi (ELMAH) oferuje innego podejścia do rejestrowania błędów środowiska uruchomieniowego w środowisku produkcyjnym. ELMAH błędu bezpłatnej, typu open source...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: ef87ae342f660a84c9359c7a1674001ed578a68e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752794"
---
<a name="logging-error-details-with-elmah-vb"></a>Rejestrowanie szczegółów błędów za pomocą biblioteki ELMAH (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Błąd rejestrowania moduły i programy obsługi (ELMAH) oferuje innego podejścia do rejestrowania błędów środowiska uruchomieniowego w środowisku produkcyjnym. ELMAH jest biblioteką rejestrowania Błąd bezpłatnej, typu open source, która zawiera funkcje, takie jak filtrowanie błędów i możliwość Wyświetl dziennik błędów, ze strony sieci web jako źródła danych RSS lub pobrać go w formacie rozdzielanym przecinkami. Ten samouczek przeprowadzi pobieranie i konfigurowanie ELMAH.


## <a name="introduction"></a>Wprowadzenie

[Poprzedni Samouczek](logging-error-details-with-asp-net-health-monitoring-vb.md) zbadane ASP. Kondycja firmy NET monitorowania systemu, które oferuje poza bibliotekę okno rejestrowania szeroką gamę zdarzenia sieci Web. Wielu programistów korzystać z programu health monitorowania do logowania się i wiadomości e-mail szczegółowe informacje o nieobsługiwanych wyjątków. Istnieje jednak kilka słabe punkty z tym systemem. Najpierw jest brak dowolny rodzaj interfejs użytkownika służący do wyświetlania informacji dotyczących zarejestrowanych zdarzeń. Jeśli chcesz wyświetlić podsumowanie 10 ostatnich błędów lub wyświetlić szczegóły błędu, które wystąpiły w ostatnim tygodniu, musisz albo umieszczanie za pośrednictwem bazy danych, skanowania za pośrednictwem skrzynki odbiorczej poczty e-mail lub tworzenia strony sieci web, który wyświetla informacje z `aspnet_WebEvent_Events` tabeli.

Inny punkt ból koncentruje się wokół złożoności monitorowanie kondycji. Ponieważ monitorowanie kondycji może służyć do rejestrowania mnóstwo różnych zdarzeń, a istnieją różne opcje instruowania, jak i kiedy zdarzenia są rejestrowane, poprawne skonfigurowanie kondycji systemu monitorowania może być uciążliwe. Na koniec występują problemy ze zgodnością. Ponieważ monitorowanie kondycji najpierw został dodany do programu .NET Framework w wersji 2.0, nie jest dostępna dla starszych aplikacji sieci web utworzone przy użyciu platformy ASP.NET w wersji 1.x. Ponadto `SqlWebEventProvider` klasy, której użyto w poprzednim samouczku, aby szczegóły błędu dzienniki do bazy danych działa tylko z baz danych programu Microsoft SQL Server. Należy utworzyć klasę dostawcy dziennika niestandardowego, potrzebujesz rejestrować błędy do magazynu danych alternatywnych, takich jak plik XML lub Oracle database.

Alternatywa dla kondycji systemu monitorowania jest błąd rejestrowania moduły i programy obsługi (ELMAH), system rejestrowania błędów bezpłatny, open source, utworzone przez [Atif Aziz](http://www.raboof.com/). Najbardziej znacząca różnica między dwoma systemami jest zdolność ELAMH firmy, aby wyświetlić listę błędów oraz szczegóły dotyczące określonego błędu ze strony sieci web i źródła danych RSS. ELMAH jest łatwiejsze do skonfigurowania niż monitorowanie kondycji, ponieważ tylko rejestruje błędy. Ponadto ELMAH obsługuje 1.x ASP.NET, aplikacji programu ASP.NET 2.0 i programie ASP.NET 3.5 i dostarczany z różnych dostawców źródło dziennika.

W tym samouczku przedstawiono kroki związane z dodawania ELMAH do aplikacji ASP.NET. Zaczynajmy!

> [!NOTE]
> Kondycja monitorowania systemu i ELMAH mają własne zestawy zalety i wady. Czy mogę zachęcamy do spróbuj obu systemów i zdecyduj, jakie jeden najlepiej odpowiadające Twoich potrzeb.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Dodawanie biblioteki ELMAH do aplikacji sieci Web platformy ASP.NET

Integrowanie ELMAH nowej lub istniejącej aplikacji ASP.NET jest to proste i łatwe proces, który zajmuje mniej niż pięć minut. Mówiąc wymaga czterech prostych kroków:

1. Pobierz biblioteki ELMAH i Dodaj `Elmah.dll` zestawu do aplikacji sieci web
2. Rejestrowanie firmy ELMAH moduły HTTP i obsługi w `Web.config`,
3. Określ opcje konfiguracji ELMAH firmy, a
4. Utwórz infrastrukturze źródłowej dziennik błędów, jeśli to konieczne.

Przejdźmy teraz przez każdy z tych czterech kroków pojedynczo.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Krok 1: Pobieranie ELMAH plików projektu i dodawanie`Elmah.dll`do aplikacji sieci Web

ELMAH 1.0 BETA 3 (Tworzenie 10617) najnowszej wersji w czasie pisania, znajduje się do pobrania, które są dostępne z tego samouczka. Alternatywnie, możesz odwiedzić [ELMAH witryny sieci Web](https://code.google.com/p/elmah/) Aby uzyskać najbardziej aktualną wersję lub pobrać kod źródłowy. Wyodrębnij pobierania ELMAH do folderu na komputerze, a następnie zlokalizuj plik zestawu biblioteki ELMAH (`Elmah.dll`).

> [!NOTE]
> `Elmah.dll` Znajduje się plik do pobrania `Bin` folder, który zawiera podfoldery dla różnych wersji programu .NET Framework i wersji wydania i debugowe kompilacji. Na użytek odpowiednią wersję kompilacji wydania. Na przykład jeśli tworzysz aplikację sieci web ASP.NET 3.5, skopiuj `Elmah.dll` plik wchodzącej w skład `Bin\net-3.5\Release` folderu.


Następnie otwórz program Visual Studio i dodać zestaw do projektu, klikając prawym przyciskiem myszy nazwę witryny sieci Web w Eksploratorze rozwiązań i wybierając pozycję Dodaj odwołanie z menu kontekstowego. Wywołuje okno dialogowe Dodaj odwołanie. Przejdź do karty Przeglądaj, a następnie wybierz `Elmah.dll` pliku. Ta akcja spowoduje dodanie `Elmah.dll` plików do aplikacji sieci web `Bin` folderu.

> [!NOTE]
> Typ projektu aplikacji sieci Web (WAP) nie są wyświetlane `Bin` folder w Eksploratorze rozwiązań. Zamiast tego zawiera listę tych elementów w folderze odwołania.


`Elmah.dll` Zestawu zawierają klasy używane przez ELMAH system. Te klasy można podzielić na trzy kategorie:

- **Moduły HTTP** — moduł HTTP to klasa, która definiuje obsługę zdarzeń dla `HttpApplication` zdarzenia, takie jak `Error` zdarzeń. ELMAH zawiera wiele modułów HTTP, trzy najbardziej istotnego te są: 

    - `ErrorLogModule` -Źródło dziennika rejestruje nieobsługiwanych wyjątków.
    - `ErrorMailModule` -Wysyła szczegóły nieobsługiwany wyjątek w wiadomości e-mail.
    - `ErrorFilterModule` -stosuje określony dla deweloperów filtry, aby ustalić, jakie wyjątki są rejestrowane i co te są ignorowane.
- **Programy obsługi HTTP** -programu obsługi HTTP, to klasa, która jest odpowiedzialny za generowanie kodu znaczników dla danego typu żądania. ELMAH obejmuje funkcje obsługi protokołu HTTP, renderowanie szczegóły błędu, jako stronę sieci web, jako źródło danych RSS lub jako pliku rozdzielanego przecinkami (CSV).
- **Źródła do dziennika błędów** — gotowych ELMAH mogą logować się błędów pamięci, w bazie danych programu Microsoft SQL Server, bazy danych Microsoft Access, aby baza danych Oracle na plik XML do bazy danych SQLite lub z bazą danych Vista DB. Np. monitorowania kondycji systemu architektura ELMAH firmy został skompilowany przy użyciu modelu dostawcy, co oznacza, że można tworzyć i bezproblemową integrację własnych dostawców źródła dzienników niestandardowych, jeśli to konieczne.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Krok 2: Zarejestrowanie ELMAH przez moduł HTTP i obsługi

Gdy `Elmah.dll` plik zawiera moduły HTTP i obsługi potrzebne do automatycznego rejestrowania nieobsługiwanych wyjątków i wyświetlić szczegóły błędu ze strony sieci web, te muszą być jawnie rejestrowane w konfiguracji aplikacji sieci web. `ErrorLogModule` Subskrybuje moduł HTTP, gdy zarejestrowana, `HttpApplication`firmy `Error` zdarzeń. To zdarzenie jest wywoływane zawsze, gdy `ErrorLogModule` rejestruje szczegółowe informacje o wyjątku do źródła określonego dziennika. Zobaczymy, jak zdefiniować dzienników dostawcy źródła w następnej sekcji "Konfigurowanie ELMAH." `ErrorLogPageFactory` Fabryka programów obsługi HTTP jest odpowiedzialny za Generowanie języka znaczników, wyświetlając dziennik błędów, ze strony sieci web.

Określonej składni rejestrowania moduły HTTP do programów obsługi, zależy od serwera sieci web, która jest najważniejsza lokacji. ASP.NET Development Server i firmy Microsoft IIS w wersji 6.0 i starsze, moduły HTTP do programów obsługi są zarejestrowane w usłudze `<httpModules>` i `<httpHandlers>` sekcje, które są wyświetlane w ramach `<system.web>` elementu. Jeśli używasz usług IIS 7.0, a następnie muszą być zarejestrowane w `<system.webServer>` elementu `<modules>` i `<handlers>` sekcje. Na szczęście możesz zdefiniować moduły HTTP i obsługi w *zarówno* umieszcza niezależnie od tego, serwer sieci web używane. Ta opcja jest większość przenośnych jeden, ponieważ pozwala ono taką samą konfigurację można używać w środowiskach deweloperskich i produkcyjnych, niezależnie od tego, serwer sieci web używane.

Rozpocznij, rejestrując `ErrorLogModule` moduł HTTP i `ErrorLogPageFactory` programu obsługi HTTP w `<httpModules>` i `<httpHandlers>` sekcji `<system.web>`. Jeśli Twoja konfiguracja już definiuje tych dwóch elementach następnie po prostu Dołącz `<add>` elementem ELMAH przez moduł HTTP i obsługi.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

Następnie Zarejestruj moduł HTTP ELMAH firmy i obsługi w `<system.webServer>` elementu. Tak jak poprzednio Jeśli ten element nie jest już obecny w konfiguracji następnie dodać go.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

Domyślnie usługi IIS 7 narzeka, jeśli moduły HTTP do programów obsługi są zarejestrowane w usłudze `<system.web>` sekcji. `validateIntegratedModeConfiguration` Atrybutu w `<validation>` elementu powoduje, że usługi IIS 7, aby pominąć takich komunikatów o błędach.

Należy pamiętać, że składnia rejestrowania `ErrorLogPageFactory` zawiera program obsługi HTTP `path` atrybut, który jest równa `elmah.axd`. Ten atrybut informuje aplikację sieci web, że jeśli żądanie dociera do strony o nazwie `elmah.axd` , a następnie można przetworzyć żądania, przez `ErrorLogPageFactory` programu obsługi HTTP. Zobaczymy `ErrorLogPageFactory` programu obsługi HTTP w działaniu w dalszej części tego samouczka.

### <a name="step-3-configuring-elmah"></a>Krok 3: Konfigurowanie ELMAH

ELMAH wyszukuje dostępne opcje konfiguracji w witrynie internetowej `Web.config` pliku w sekcji Konfiguracja niestandardowa o nazwie `<elmah>`. Aby można było używać niestandardowego tematu `Web.config` najpierw musi być zdefiniowany w `<configSections>` elementu. Otwórz `Web.config` pliku i Dodaj następujący kod do `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Jeśli konfigurujesz ELMAH dla aplikacji ASP.NET 1.x następnie usuń `requirePermission="false"` atrybut z `<section>` elementów wymienionych powyżej.


Powyższej składni rejestruje niestandardowej `<elmah>` sekcji i jego podsekcje: `<security>`, `<errorLog>`, `<errorMail>`, i `<errorFilter>`.

Następnie dodaj `<elmah>` sekcji `Web.config`. W tej sekcji powinien pojawić się na tym samym poziomie co `<system.web>` elementu. Wewnątrz `<elmah>` sekcji Dodaj `<security>` i `<errorLog>` sekcje w następujący sposób:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

`<security>` Sekcji `allowRemoteAccess` atrybut wskazuje, czy jest dozwolony dostęp zdalny. Jeśli ta wartość jest równa 0, następnie na stronie sieci web dziennik błędów mogą być przeglądane tylko lokalnie. Jeśli ten atrybut jest ustawiony na wartość 1 na stronie sieci web dziennik błędów jest włączone dla osoby odwiedzające zarówno zdalnych i lokalnych. Na razie Przyjrzyjmy Wyłącz strony sieci web dziennik błędów dla zdalnego użytkowników zewnętrznych. Firma Microsoft będzie zezwala na zdalny dostęp później po wygenerowaniu szansy sprzedaży w celu omówienia ich bezpieczeństwem w ten sposób.

`<errorLog>` Sekcja definiuje źródło dziennika błędów, które określają, w którym rejestrowane są szczegóły błędu; jest on podobny do `<providers>` sekcji kondycji systemu monitorowania. Określa powyższej składni `SqlErrorLog` klasy jako źródło dziennika błędów, które rejestruje błędy z bazą danych programu Microsoft SQL Server, określony przez `connectionStringName` wartość atrybutu.

> [!NOTE]
> ELMAH jest dostarczany z dostawców dzienników dodatkowe błędów, które mogą służyć do logowania błędy do pliku XML, bazę danych Microsoft Access, bazy danych Oracle i innych magazynów danych. Zobacz przykład `Web.config` pliku dostarczonego z pobranymi ELMAH, aby uzyskać informacje dotyczące sposobu używania tych dostawców dziennika błędów alternatywne.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Krok 4: Tworzenie infrastruktury źródło dziennika błędów

Firmy ELMAH `SqlErrorLog` dostawcy rejestruje informacje o błędzie do określonej bazy danych programu Microsoft SQL Server. `SqlErrorLog` Dostawcy oczekuje, że masz tabelę o nazwie tej bazy danych `ELMAH_Error` i trzech procedur składowanych: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, i `ELMAH_LogError`. Pobierany ELMAH zawiera plik o nazwie `SQLServer.sql` w `db` folder, który zawiera języka T-SQL do tworzenia tej tabeli oraz tych procedur składowanych. Musisz uruchomić te instrukcje w bazie danych do użycia `SqlErrorLog` dostawcy.

**Rysunek 1** i **2** Pokaż Eksplorator bazy danych w programie Visual Studio po obiekty bazy danych wymagane przez `SqlErrorLog` dodano dostawcę.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Rysunek 1**: `SqlErrorLog` dostawcy rejestruje błędy, aby `ELMAH_Error` tabeli

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Rysunek 2**: `SqlErrorLog` dostawca używa trzech procedur składowanych

## <a name="elmah-in-action"></a>ELMAH w działaniu

W tym momencie dodaliśmy ELMAH do aplikacji sieci web, zarejestrowane `ErrorLogModule` moduł HTTP i `ErrorLogPageFactory` programu obsługi HTTP, określony przez ELMAH opcje konfiguracji w `Web.config`i dodano obiekty bazy danych potrzebne do `SqlErrorLog` Dostawca dziennika błędów. Teraz jesteśmy gotowi zobaczyć ELMAH w działaniu! Odwiedź witrynę sieci Web książki przeglądów i odwiedź stronę, która generuje błąd w czasie wykonywania, takie jak `Genre.aspx?ID=foo`, lub nieistniejącej strony, takich jak `NoSuchPage.aspx`. Zobacz podczas odwiedzania tych stron jest zależna od `<customErrors>` konfiguracji i w przypadku odwiedzania lokalnie lub zdalnie. (Zobacz [ *wyświetlania strony błędu niestandardowego* samouczek](displaying-a-custom-error-page-vb.md) dla przypomnienia informacji na ten temat.)

ELMAH nie ma wpływu na zawartość, jest wyświetlany użytkownikowi, gdy wystąpi nieobsługiwany wyjątek; rejestruje tylko jego szczegóły. Ten dziennik błędów jest dostępny na stronie sieci web `elmah.axd` z katalogu głównego witryny sieci Web, takich jak `http://localhost/BookReviews/elmah.axd`. (Ten plik nie fizycznie istnieje w projekcie, ale gdy nadejdzie żądanie `elmah.axd` środowisko uruchomieniowe wywołuje jego `ErrorLogPageFactory` obsługi HTTP, który generuje znaczników wysyłanych z powrotem do przeglądarki.)

> [!NOTE]
> Można również użyć `elmah.axd` strony, aby nakazać ELMAH wygenerować błąd testu. Odwiedzający `elmah.axd/test` (na przykład `http://localhost/BookReviews/elmah.axd/test`) powoduje, że ELMAH zgłosić wyjątek typu `Elmah.TestException`, która zawiera komunikat o błędzie: "To jest wyjątek testu, który można bezpiecznie zignorować."


**Rysunek 3** zawiera dziennik błędów podczas odwiedzania `elmah.axd` ze środowiska projektowego.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Rysunek 3**: `Elmah.axd` Wyświetla dziennik błędów, ze strony sieci Web  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image7.png))

Zaloguj się do niego błąd **rysunek 3** zawiera sześć zapisy błędów. Każdy wpis zawiera kod stanu HTTP (404 lub 500 błędów), typ, opis, nazwa zalogowanego użytkownika w momencie wystąpienia błędu, Data i godzina. Kliknięcie linku szczegóły zawiera strona, która zawiera ten sam komunikat o błędzie objętego błąd szczegóły żółty ekranem śmierci (zobacz **rysunek 4**) wraz z wartości zmiennych serwera w momencie wystąpienia błędu (zobacz  **Rysunek 5**). Można również wyświetlić XML raw, w którym zapisywane są informacje o błędzie, który zawiera dodatkowe informacje, takie jak wartości w nagłówku żądania HTTP POST.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Rysunek 4**: wyświetlić szczegóły błędów YSOD  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Rysunek 5**: Zapoznaj się z wartości kolekcji zmiennych serwera w momencie wystąpienia błędu  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image13.png))

Wdrażanie biblioteki ELMAH produkcyjnej witrynie internetowej pociąga za sobą:

- Kopiowanie `Elmah.dll` plik `Bin` folderu w środowisku produkcyjnym
- Kopiowanie ustawień konfiguracyjnych specyficznych dla biblioteki ELMAH `Web.config` plik używany w środowisku produkcyjnym, i
- Dodawanie infrastrukturze źródłowej dziennika błędów do produkcyjnej bazy danych.

Po rozważyliśmy technik kopiowania plików od projektowania do produkcji w poprzednich samouczkach. Być może Najprostszym sposobem, aby zyskać infrastrukturę źródło dziennika błędów w produkcyjnej bazy danych jest Użyj programu SQL Server Management Studio, aby nawiązać połączenie z produkcyjną bazą danych, a następnie wykonaj `SqlServer.sql` pliku skryptu, który spowoduje utworzenie tabeli wymagane i przechowywane procedury.

### <a name="viewing-the-error-details-page-on-production"></a>Wyświetlanie strony szczegółów błędu w środowisku produkcyjnym

Po wdrożeniu witryny do środowiska produkcyjnego, odwiedź witrynę sieci Web produkcji i generować nieobsługiwany wyjątek. Tak jak w środowisku deweloperskim ELMAH nie ma wpływu na stronę błędu, wyświetlane po wystąpieniu nieobsługiwanego wyjątku; Zamiast tego należy jedynie rejestruje błąd. Jeśli użytkownik podejmie próbę można znaleźć na stronie dziennik błędów (`elmah.axd`) w środowisku produkcyjnym będzie można dostępnych jest ze stroną zabronione wyświetlane w **rysunek 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Rysunek 6**: domyślnie odwiedzających zdalnego nie można wyświetlić strony sieci Web dziennik błędów  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image16.png))

Pamiętamy w konfiguracji biblioteki ELMAH `<security>` sekcji ustawimy `allowRemoteAccess` atrybut na wartość 0, co użytkownicy nie mogą zdalne wyświetlanie dziennika błędów. Jest ważne uniemożliwić anonimowe osobom odwiedzającym przeglądanie dziennik błędów, jako szczegóły błędu może ujawnić luk w zabezpieczeniach lub innych informacji poufnych. Jeśli zdecydujesz się ten atrybut jest ustawiony na 1 i włączyć dostęp zdalny do dziennika błędów, to ważne jest, aby blokowanie `elmah.axd` ścieżki, tak że tylko autoryzowane osoby odwiedzające do niego dostęp. Można to osiągnąć przez dodanie `<location>` elementu `Web.config` pliku.

Następująca konfiguracja zezwala na tylko użytkownicy w roli administratora, aby uzyskać dostęp do strony sieci web dziennika błędów:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> Rola administratora i trzech użytkowników w systemie — Scott, Jisun i Alicja — zostały dodane w [ *Konfigurowanie witryny sieci Web, korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-vb.md). Scott użytkowników i Jisun należą do roli administratora. Aby uzyskać więcej informacji na temat uwierzytelniania i autoryzacji, zobacz mój [samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Teraz można wyświetlić dziennik błędów w środowisku produkcyjnym przez użytkowników zdalnych; Odwołaj się do **rysunki 3**, **4**, i **5** do zrzutów ekranu, strony sieci web, dziennik błędów. Jednak jeśli użytkownik anonimowy lub inny administrator próbuje wyświetlić na stronie dziennik błędów zostanie automatycznie przekierowany do strony logowania (`Login.aspx`), jako **rysunek 7** pokazuje.

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Rysunek 7**: nieautoryzowani użytkownicy są automatycznie przekierowywane do strony logowania  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Programowe rejestrowanie błędów

Firmy ELMAH `ErrorLogModule` źródło określony dziennik modułu HTTP automatycznie rejestruje nieobsługiwanych wyjątków. Alternatywnie można zarejestrować błąd, bez konieczności podniesienia nieobsługiwany wyjątek przy użyciu `ErrorSignal` klasy i jego `Raise` metody. `Raise` Metody jest przekazywana `Exception` obiektu i rejestruje go tak, jakby ten wyjątek gdyby były zgłaszane i osiągnął środowiska uruchomieniowego programu ASP.NET nie jest obsługiwany. Różnica, jest jednak, że żądanie nadal normalnie po wykonywania `Raise` wywołaniu metody, natomiast wyjątek zgłoszony, nieobsługiwany przerywa wykonywanie normalne żądania i powoduje, że środowisko uruchomieniowe ASP.NET wyświetlić ze skonfigurowanym ustawieniem Strona błędu.

`ErrorSignal` Klasa jest przydatna w sytuacjach, w której istnieje pewne akcje, które może zakończyć się niepowodzeniem, ale jego awarii nie jest krytycznego do ogólnej wykonywanej operacji. Na przykład witryny sieci Web mogą zawierać formularz, który akceptuje dane wejściowe użytkownika, zapisuje go w bazie danych, a następnie wysyła użytkownikowi wiadomość e-mail informującą ich o, informacji została przetworzona. Co ma się zdarzyć, jeśli informacje są zapisywane do bazy danych pomyślnie, ale występuje błąd podczas wysyłania wiadomości e-mail? Jedną z opcji jest zgłoszenie wyjątku i wysłać użytkownika do strony błędu. Jednak może to mylić użytkownika w przekonaniu, że informacje wprowadzone nie został zapisany. Innym rozwiązaniem byłoby dziennika błędów związanych z pocztą e-mail, ale nie zmieniają środowisko użytkownika w dowolny sposób. Jest to miejsce `ErrorSignal` klasa jest przydatna.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Powiadomienia o błędzie za pośrednictwem poczty E-mail

Wraz z rejestrowanie błędów do bazy danych biblioteki ELMAH można również skonfigurować do określonego adresata poczty e-mail szczegóły błędu. Ta funkcjonalność jest dostarczana przez `ErrorMailModule` modułu HTTP; w związku z tym, należy zarejestrować ten moduł HTTP w `Web.config` wysyłanie szczegółów błędów za pośrednictwem poczty e-mail.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

Następnie podaj informacje o błędzie wiadomość e-mail w `<elmah>` elementu `<errorMail>` sekcji wskazujący adresu e-mail nadawcy i adresata, temat, czy zostanie wysłana wiadomość e-mail asynchronicznie.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Przy użyciu powyższych ustawień w miejscu, kiedy środowisko uruchomieniowe pojawia się błąd ELMAH wysyła wiadomość e-mail na adres support@example.com przy użyciu szczegółów błędu. Wiadomość e-mail z błędów ELMAH firmy zawiera te same informacje, które są wyświetlane na stronie błędu web szczegółowe informacje, a mianowicie komunikat o błędzie, śladu stosu i zmiennych serwera (odnoszą się do **rysunki 4** i **5**). Błąd wiadomości e-mail zawiera również zawartość wyjątku szczegóły żółty ekranem śmierci jako załącznik (`YSOD.html`).

**Rysunek 8** pokazuje e-mail błędów ELMAH firmy wygenerowane, odwiedzając stronę `Genre.aspx?ID=foo`. Gdy **rysunek 8** pokazuje tylko błąd wiadomości i ślad stosu, zmiennych serwera są uwzględniane w dalszej części w treści wiadomości e-mail.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Rysunek 8**: można skonfigurować ELMAH, aby wysłać szczegóły błędu, za pośrednictwem poczty E-mail  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Tylko rejestrowanie błędów zainteresowania

Domyślnie ELMAH rejestruje szczegółowe informacje o każdej nieobsłużony wyjątek, w tym 404 i inne błędy HTTP. Można poinstruować ELMAH do zignorowania tych lub innych rodzajów błędów za pomocą filtrowanie błędów. Logikę filtrowania odbywa się przez firmy ELMAH `ErrorFilterModule` moduł HTTP, który należy zarejestrować w `Web.config` aby można było używać logikę filtrowania. Reguły filtrowania są określone w `<errorFilter>` sekcji.

Następujące znaczniki powoduje, że ELMAH nie rejestrować błędy 404.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> Pamiętaj, aby można było używać filtrowanie błędów, należy zarejestrować `ErrorFilterModule` moduł HTTP.


`<equal>` Element wewnątrz `<test>` sekcja nazywa się potwierdzenie. Jeśli potwierdzenia zwraca wartość true, błąd jest filtrowana ELMAH w dzienniku. Istnieją inne potwierdzenia niedostępnych, w tym: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`i tak dalej. Można także połączyć potwierdzenia używając `<and>` i `<or>` operatorów logicznych. Co więcej można nawet zawierać proste wyrażenie języka JavaScript jako potwierdzenie, lub napisać własny asercji w języku C# lub Visual Basic.

Aby uzyskać więcej informacji na temat błędów ELMAH firmy możliwości filtrowania, zobacz [sekcji Filtrowanie błędów](https://code.google.com/p/elmah/wiki/ErrorFiltering) w [witryny typu wiki biblioteki ELMAH](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Podsumowanie

ELMAH zapewnia proste, ale zaawansowany mechanizm rejestrowanie błędów w aplikacji sieci web ASP.NET. System monitorowania kondycji firmy Microsoft, np. biblioteki ELMAH może rejestrować błędy z bazą danych i może wysyłać szczegóły błędów do deweloperów za pośrednictwem poczty e-mail. W przeciwieństwie do monitorowania kondycji systemu, ELMAH obejmuje poza pole obsługę szerszego zakresu magazyny danych dziennika błędów, w tym: Microsoft SQL Server, Microsoft Access, Oracle, pliki XML i kilka innych. Ponadto ELMAH oferuje wbudowany mechanizm do wyświetlania dziennik błędów i szczegółowe informacje dotyczące określonego błędu ze strony sieci web `elmah.axd`. `elmah.axd` Strony umożliwia również renderowanie informacje o błędzie jako źródła danych RSS lub pliku wartości rozdzielanych przecinkami (CSV), który może odczytać, za pomocą programu Microsoft Excel. Można również poinstruować ELMAH filtrować błędy w dzienniku za pomocą potwierdzenia zaznacza deklaratywne lub programowy. I ELMAH mogą być używane z aplikacji programu ASP.NET w wersji 1.x.

Każdej wdrożonej aplikacji powinna mieć pewien mechanizm dla automatycznego rejestrowania nieobsługiwanych wyjątków i wysyłania powiadomień do zespołu programistycznego. Czy to odbywa się za pomocą monitorowania kondycji lub ELMAH to dodatkowa baza danych. Innymi słowy tak naprawdę nie ma znaczenia znacznie tego, czy używasz monitorowanie kondycji lub ELMAH; Ocena obu systemów, a następnie wybierz ten, który najlepiej spełnia Twoje potrzeby. Co to jest zasadniczo ważne jest umieszczana mechanizmu w miejscu do rejestrowania nieobsługiwanych wyjątków w środowisku produkcyjnym.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [ELMAH — moduły rejestrowania błędów i procedury obsługi](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Strona projektu biblioteki ELMAH](https://code.google.com/p/elmah/) (źródła kodu, próbek, stron typu wiki)
- [Podłączanie ELMAH do aplikacji sieci Web można przechwycić nieobsługiwane wyjątki](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (wideo)
- [Strony dziennika błędów zabezpieczeń](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Tworzenie składników podłączanych ASP.NET przy użyciu moduły HTTP do programów obsługi](https://msdn.microsoft.com/library/aa479332.aspx)
- [Samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Poprzednie](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [dalej](precompiling-your-website-vb.md)
