---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: Rejestrowanie szczegóły błędu z ELMAH (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Błąd rejestrowania modułów i obsługi (ELMAH) oferuje innego podejścia do rejestrowania błędów czasu wykonywania w środowisku produkcyjnym. ELMAH błędu wolnego typu open source...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: cd91c745624f09d01a326a445bea2bb756576688
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="logging-error-details-with-elmah-c"></a>Szczegóły rejestrowania błędów ELMAH (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Błąd rejestrowania modułów i obsługi (ELMAH) oferuje innego podejścia do rejestrowania błędów czasu wykonywania w środowisku produkcyjnym. ELMAH jest biblioteką rejestrowania Błąd wolnego typu open source, która obejmuje funkcje, takie jak błąd filtrowanie i możliwości Wyświetl dziennik błędów ze strony sieci web jako źródła danych RSS lub pobrać go w formacie rozdzielanym przecinkami. Ten samouczek przeprowadzi Cię przez pobranie i konfigurowanie ELMAH.


## <a name="introduction"></a>Wprowadzenie

[Poprzedniego samouczek](logging-error-details-with-asp-net-health-monitoring-cs.md) zbadane ASP. Monitorowania systemu, które oferuje poza biblioteki pole dla rejestrowanych zdarzeń sieci Web szerokiej gamy kondycji w sieci. Deweloperzy wiele za pomocą monitorowania do logowania i wysłanie szczegółów nieobsługiwanych wyjątków kondycji. Istnieje jednak kilka słabe punkty z tym systemem. Najpierw jest brak dowolny rodzaj interfejs użytkownika służący do wyświetlania informacji o zarejestrowane zdarzenia. Jeśli chcesz wyświetlić podsumowanie 10 ostatnich błędów lub wyświetlić szczegóły błędu, które wystąpiły w ostatnim tygodniu, musi albo umieszczanie za pośrednictwem bazy danych, skanowania za pośrednictwem skrzynki odbiorczej poczty e-mail lub tworzenia strony sieci web, który wyświetla informacje z `aspnet_WebEvent_Events` tabeli.

Inny punkt słabe koncentruje się wokół monitorowanie kondycji złożoności. Ponieważ monitorowanie kondycji służy do rejestrowania nadmiar różnych zdarzeń, a istnieją różne opcje poinstruowanie, jak i kiedy zdarzenia są rejestrowane, poprawne skonfigurowanie kondycji systemu monitorowania może być uciążliwe zadań. Na koniec występują problemy ze zgodnością. Ponieważ monitorowanie kondycji najpierw został dodany do programu .NET Framework w wersji 2.0, nie jest dostępny dla starszych aplikacji sieci web utworzony za pomocą wersji platformy ASP.NET 1.x. Ponadto `SqlWebEventProvider` klasy, które zostały użyte podczas poprzedniego w samouczku szczegóły błędu dzienniki do bazy danych, działa tylko w przypadku baz danych programu Microsoft SQL Server. Należy utworzyć klasę dostawcy dziennik niestandardowy potrzebujesz rejestrować błędy do magazynu danych, takich jak plik XML lub z bazą danych Oracle.

Alternatywą do monitorowania systemu kondycji jest błąd rejestrowania modułów i obsługi (ELMAH), system rejestrowania błędów bezpłatne, open source utworzonych przez [Atif Aziz](http://www.raboof.com/). Najbardziej znacząca różnica między tymi dwoma systemami jest ELAMH jego zdolność do wyświetlania listy błędów i szczegóły określonego błędu ze strony sieci web, a jako źródła danych RSS. ELMAH jest łatwiejsze do skonfigurowania niż monitorowanie kondycji, ponieważ tylko rejestrowania błędów. Ponadto ELMAH obsługuje 1.x ASP.NET, aplikacji programu ASP.NET 2.0 i programu ASP.NET 3.5 i jest dostarczany z różnych dostawców źródła dziennika.

Ten samouczek przeprowadzi Cię przez kroki związane z dodaniem ELMAH do aplikacji ASP.NET. Dzieła!

> [!NOTE]
> Kondycja systemu i ELMAH monitorowania mają własne zestawy zalet i wad. I zachęca do spróbuj obu systemów i zdecyduj, jakie jeden odpowiedni potrzeb.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Dodawanie ELMAH do aplikacji sieci Web ASP.NET

Integracja ELMAH do nowej lub istniejącej aplikacji ASP.NET jest łatwe i prostego procesu w obszarze pięć minut. Mówiąc obejmuje ona czterech prostych krokach:

1. Pobierz ELMAH i dodać `Elmah.dll` zestawu do aplikacji sieci web
2. Zarejestruj ELMAH przez moduły HTTP i obsługi w `Web.config`,
3. Określ opcje konfiguracji w ELMAH i
4. Tworzenie infrastruktury źródła dziennika błędów, w razie potrzeby.

Przejdźmy każdego z tych czterech kroków, pojedynczo.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Krok 1: Pobieranie ELMAH plików projektu i dodawanie`Elmah.dll`do aplikacji sieci Web

ELMAH 1.0 BETA 3 (kompilacji 10617) najnowszej wersji w czasie zapisywania, znajduje się w pobierania dostępne z tego samouczka. Alternatywnie możesz odwiedzić [ELMAH witryny sieci Web](https://code.google.com/p/elmah/) można pobrać najnowszą wersję lub pobrać kodu źródłowego. Wyodrębnij pobierania ELMAH do folderu na pulpicie i zlokalizuj plik zestawu ELMAH (`Elmah.dll`).

> [!NOTE]
> `Elmah.dll` Plik znajduje się w operacji pobierania `Bin` folder, który ma podfoldery dla różnych wersji .NET Framework i kompilacji wydania i debugowania. Użyj kompilacji wydania dla odpowiednich framework w wersji. Na przykład, jeśli tworzysz aplikację sieci web platformy ASP.NET 3.5, skopiuj `Elmah.dll` plik z `Bin\net-3.5\Release` folderu.


Następnie otwórz program Visual Studio i dodać zestaw do projektu, klikając prawym przyciskiem myszy nazwę witryny sieci Web w Eksploratorze rozwiązań i wybierając pozycję Dodaj odwołanie z menu kontekstowego. Spowoduje to wyświetlenie okna dialogowego Dodaj odwołanie. Przejdź do karty Przeglądaj i wybierz polecenie `Elmah.dll` pliku. Ta akcja dodaje `Elmah.dll` pliku do aplikacji sieci web `Bin` folderu.

> [!NOTE]
> Typ projektu aplikacji sieci Web (WAP) nie są wyświetlane `Bin` folder w Eksploratorze rozwiązań. Zamiast tego zawiera listę tych elementów w folderze odwołań.


`Elmah.dll` Zestaw zawiera klasy używane przez ELMAH system. Te klasy dzielą się na trzy kategorie:

- **Moduły HTTP** — moduł protokołu HTTP jest klasa, która definiuje obsługi zdarzeń `HttpApplication` zdarzenia, takie jak `Error` zdarzeń. ELMAH zawiera wiele modułów HTTP, te trzy najbardziej istotnego znaczenia jest: 

    - `ErrorLogModule` -Źródło dziennika rejestruje nieobsługiwanych wyjątków.
    - `ErrorMailModule` -Wysyła szczegóły nieobsługiwany wyjątek w wiadomości e-mail.
    - `ErrorFilterModule` -stosuje określony developer filtrów, aby ustalić, jakie wyjątków są rejestrowane i co te są ignorowane.
- **Programy obsługi HTTP** — program obsługi protokołu HTTP jest klasa, która jest odpowiedzialny za wygenerowanie kodu znaczników dla określonego typu żądania. ELMAH obejmuje programów obsługi HTTP, renderowanie szczegóły błędu, jako strony sieci web, źródła danych RSS lub jako pliku rozdzielanego przecinkami (CSV).
- **Błąd dziennika źródeł** — fabrycznej ELMAH może rejestrować błędy pamięci, bazą danych programu Microsoft SQL Server, bazy danych programu Microsoft Access, do bazy danych programu Oracle do pliku XML, do bazy danych SQLite lub z bazą danych Vista DB. Jak kondycji systemu monitorowania architektura firmy ELMAH został zbudowany przy użyciu modelu dostawcy, co oznacza, można tworzyć i w razie potrzeby są bezproblemowo integrowane własnych dostawców źródło dziennika niestandardowego.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Krok 2: Rejestrowanie ELMAH przez moduł HTTP i obsługi

Gdy `Elmah.dll` plik zawiera moduły HTTP i obsługi potrzebne do automatycznego rejestrowania nieobsługiwanych wyjątków i wyświetlić szczegóły błędu ze strony sieci web, te muszą być jawnie rejestrowane w konfiguracji aplikacji sieci web. `ErrorLogModule` Moduł protokołu HTTP, po zarejestrowaniu subskrybuje `HttpApplication`w `Error` zdarzeń. To zdarzenie jest wywoływane zawsze, gdy `ErrorLogModule` rejestruje szczegóły wyjątku źródło określony dziennik. Zajmiemy się tym, jak zdefiniować dostawcę źródła dziennika w następnej sekcji "Konfigurowanie ELMAH". `ErrorLogPageFactory` Fabryka programów obsługi HTTP jest odpowiedzialny za wygenerowanie znaczników, przeglądając dziennik błędów ze strony sieci web.

Określone składnia rejestrowania modułów HTTP i obsługi zależy od serwera sieci web, która jest włączanie lokacji. ASP.NET Development Server i firmy Microsoft IIS w wersji 6.0 i starsze wersje, moduły HTTP i programy obsługi są zarejestrowane w `<httpModules>` i `<httpHandlers>` sekcje, które znajdują się w obrębie `<system.web>` elementu. Jeśli korzystasz z usług IIS 7.0, muszą być zarejestrowane w `<system.webServer>` elementu `<modules>` i `<handlers>` sekcje. Na szczęście można zdefiniować moduły HTTP i programy obsługi zdarzeń w *zarówno* umieszcza niezależnie od używanego serwera sieci web. Ta opcja jest najbardziej przenośnych jak pozwala na taką samą konfigurację do użycia w środowiskach rozwoju i produkcji, niezależnie od używanego serwera sieci web.

Uruchom rejestrując `ErrorLogModule` moduł HTTP i `ErrorLogPageFactory` programu obsługi HTTP w `<httpModules>` i `<httpHandlers>` sekcji `<system.web>`. Jeśli konfiguracji już definiuje tych dwóch elementów następnie po prostu Dołącz `<add>` element ELMAH przez moduł HTTP i obsługi.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

Następnie zarejestrować ELMAH przez moduł HTTP i obsługi w `<system.webServer>` elementu. Jak wcześniej Jeśli ten element nie jest już obecny w konfiguracji dodawaniu go.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

Domyślnie usługi IIS 7 narzeka, jeśli moduły HTTP i programy obsługi są rejestrowane w `<system.web>` sekcji. `validateIntegratedModeConfiguration` Atrybutu w `<validation>` elementu powoduje, że usługi IIS 7 do pomijania takie komunikaty o błędach.

Należy pamiętać, że składnia rejestrowania `ErrorLogPageFactory` obejmuje program obsługi HTTP `path` atrybut, który ma ustawioną wartość `elmah.axd`. Ten atrybut informuje aplikacji sieci web, że jeśli żądanie dociera do strony o nazwie `elmah.axd` , a następnie żądanie powinny być przetwarzane przez `ErrorLogPageFactory` programu obsługi HTTP. Zajmiemy się tym `ErrorLogPageFactory` programu obsługi HTTP, w akcji w dalszej części tego samouczka.

### <a name="step-3-configuring-elmah"></a>Krok 3: Konfigurowanie ELMAH

ELMAH szuka jego opcje konfiguracji w witrynie sieci Web `Web.config` pliku w sekcji Konfiguracja niestandardowa o nazwie `<elmah>`. Aby można było używać niestandardowych części `Web.config` najpierw musi być zdefiniowana w `<configSections>` elementu. Otwórz `Web.config` i Dodaj następujący kod do `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> Jeśli konfigurujesz ELMAH 1.x aplikacji ASP.NET, Usuń `requirePermission="false"` atrybutu z `<section>` elementów wymienionych powyżej.


Powyższej składni rejestruje niestandardowego `<elmah>` sekcji i jego podpunkty: `<security>`, `<errorLog>`, `<errorMail>`, i `<errorFilter>`.

Następnie dodaj `<elmah>` sekcji do `Web.config`. W tej sekcji powinien pojawiać się na tym samym poziomie co `<system.web>` elementu. Wewnątrz `<elmah>` Dodaj sekcję `<security>` i `<errorLog>` sekcje w następujący sposób:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

`<security>` Sekcji `allowRemoteAccess` atrybut wskazuje, czy jest dozwolony dostęp zdalny. Jeśli ta wartość jest równa 0, następnie strony sieci web dziennika błędów mogą być przeglądane tylko lokalnie. Jeśli ten atrybut jest ustawiona na 1 strony sieci web dziennika błędów jest włączone dla gości zarówno zdalnych i lokalnych. Teraz załóżmy wyłączyć strony sieci web dziennik błędów dla zdalnego odwiedzających. Firma Microsoft będzie zezwala na zdalny dostęp później po będziemy mieć możliwość omówienia bezpieczeństwem w ten sposób.

`<errorLog>` Sekcja definiuje źródło dziennika błędów, które określają, gdzie są rejestrowane szczegóły błędu; jest podobna do `<providers>` sekcji do monitorowania kondycji. Określa powyższej składni `SqlErrorLog` klasy jako źródło dziennika błędów, które rejestruje błędy z bazą danych programu Microsoft SQL Server, określony przez `connectionStringName` wartość atrybutu.

> [!NOTE]
> ELMAH jest dostarczany z dostawcami dziennika dodatkowe błędów, które mogą służyć do logowania błędy do pliku XML, bazy danych programu Microsoft Access z bazą danych Oracle i innych magazynów danych. Zapoznaj się z przykładem `Web.config` pliku, który jest dołączony do pobierania ELMAH informacji na temat korzystania z tych dostawców dziennika błędów alternatywny.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Krok 4: Tworzenie infrastruktury źródła dziennika błędów

W ELMAH `SqlErrorLog` dostawcy rejestruje informacje o błędzie do określonej bazy danych programu Microsoft SQL Server. `SqlErrorLog` Dostawcy oczekuje, że ta baza danych ma tabeli o nazwie `ELMAH_Error` i trzech procedur składowanych: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, i `ELMAH_LogError`. Pobieranie ELMAH zawiera plik o nazwie `SQLServer.sql` w `db` folder zawierający T-SQL do tworzenia tej tabeli i tych procedur składowanych. Musisz uruchomić te instrukcje na bazę danych, aby użyć `SqlErrorLog` dostawcy.

**Rysunek 1** i **2** Pokaż Eksploratora bazy danych w programie Visual Studio po obiektów bazy danych wymaganych przez `SqlErrorLog` dodano dostawcę.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Rysunek 1**: `SqlErrorLog` dostawcy rejestruje błędy, aby `ELMAH_Error` tabeli

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Rysunek 2**: `SqlErrorLog` dostawca używa trzech procedur składowanych

## <a name="elmah-in-action"></a>ELMAH w akcji

W tym momencie dodaliśmy ELMAH do aplikacji sieci web, w zarejestrowany `ErrorLogModule` moduł HTTP i `ErrorLogPageFactory` programu obsługi HTTP, określony przez ELMAH opcje konfiguracji w `Web.config`i dodaje obiekty bazy danych wymagane dla `SqlErrorLog` Dostawca dziennika błędów. Jesteśmy teraz Zobacz ELMAH w akcji! Odwiedź witrynę sieci Web przeglądami książki i odwiedź stronę, który generuje błąd w czasie wykonywania, takie jak `Genre.aspx?ID=foo`, lub nieistniejącą strony, takich jak `NoSuchPage.aspx`. Zobacz podczas odwiedzania strony zależy od `<customErrors>` konfiguracji i w przypadku odwiedzania lokalnie lub zdalnie. (Odwołują się do [ *wyświetlania strony błędu niestandardowego* samouczek](displaying-a-custom-error-page-cs.md) dla odświeżacza na ten temat.)

ELMAH nie ma wpływu na zawartość jest pokazywana użytkownikowi, gdy wystąpi nieobsługiwany wyjątek; rejestruje tylko jego szczegóły. Ten dziennik błędów jest dostępny na stronie sieci web `elmah.axd` z katalogu głównego witryny sieci Web, takich jak `http://localhost/BookReviews/elmah.axd`. (Ten plik nie fizycznie istnieje w projekcie, ale jeśli żądanie pochodzi przypadku `elmah.axd` środowiska uruchomieniowego wysyła go do `ErrorLogPageFactory` obsługi HTTP, który generuje kod znaczników, wysyłane z powrotem do przeglądarki.)

> [!NOTE]
> Można również użyć `elmah.axd` stronę, aby poinstruować ELMAH wygenerować błąd testu. Odwiedzający `elmah.axd/test` (podobnie jak w `http://localhost/BookReviews/elmah.axd/test`) powoduje, że ELMAH zostać zgłoszony wyjątek typu `Elmah.TestException`, który zawiera komunikat o błędzie: "Jest wyjątek testu, który można bezpiecznie zignorować".


**Rysunek 3** zawiera dziennik błędów podczas odwiedzania `elmah.axd` środowiska programowania.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Rysunek 3**: `Elmah.axd` zawiera dziennik błędów ze strony sieci Web  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-cs/_static/image7.png))

Błąd logowania **rysunek 3** zawiera sześć zapisy błędów. Każdy wpis zawiera kod stanu HTTP (404 lub 500 błędów), typ, opis, nazwa zalogowanego użytkownika w momencie wystąpienia błędu, Data i godzina. Kliknięcie łącza Szczegóły Wyświetla stronę zawierającą sam komunikat o błędzie wyświetlany w błąd szczegóły żółty ekranem śmierci (zobacz **4 rysunek**) oraz wartości zmiennych serwera w chwili wystąpienia błędu (zobacz  **Rysunek 5**). Można również wyświetlić XML raw, w którym zapisywane są szczegóły błędu, zawierający dodatkowe informacje, takie jak wartości nagłówka HTTP POST.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Rysunek 4**: Wyświetl szczegóły błędu YSOD  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Rysunek 5**: Poznaj wartości kolekcji zmiennych serwera w chwili wystąpienia błędu  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-cs/_static/image13.png))

Wdrażanie ELMAH do produkcji witryny sieci Web wymaga:

- Kopiowanie `Elmah.dll` pliku `Bin` folderu w środowisku produkcyjnym,
- Kopiowanie ELMAH specyficzne ustawienia konfiguracji do `Web.config` plik używany w środowisku produkcyjnym, i
- Dodawanie infrastrukturze źródłowej dziennik błędów w produkcyjnej bazie danych.

Firma Microsoft zostały przedstawione techniki kopiowania plików od projektowania do produkcji w poprzednim samouczki. Być może Najprostszym sposobem pobrania infrastrukturze źródłowej dziennik błędów w produkcyjnej bazie danych jest użycie programu SQL Server Management Studio do połączenia z bazą danych w środowisku produkcyjnym, a następnie wykonaj `SqlServer.sql` pliku skryptu, który spowoduje utworzenie tabeli potrzebne i przechowywane procedury.

### <a name="viewing-the-error-details-page-on-production"></a>Wyświetlanie na stronie Szczegóły błędu w środowisku produkcyjnym

Po wdrożeniu witryny do środowiska produkcyjnego, odwiedź witrynę sieci Web produkcyjnym i generować nieobsługiwany wyjątek. W środowisku programistycznym ELMAH nie ma wpływu na stronę błędu wyświetlane, gdy wystąpi nieobsługiwany wyjątek; Zamiast tego jedynie rejestruje błąd. Jeśli spróbujesz odwiedź stronę błędu dziennika (`elmah.axd`) ze środowiska produkcyjnego, będzie można greeted ze stroną zabroniony pokazano **rysunek 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Rysunek 6**: domyślnie zdalnego gości nie można wyświetlić strony sieci Web dziennika błędów  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-cs/_static/image16.png))

Odwołaj który w konfiguracji ELMAH `<security>` sekcji możemy ustawić `allowRemoteAccess` atrybut na wartość 0, co użytkownicy nie mogą zdalne wyświetlanie dziennika błędów. Należy zakazać anonimowe odwiedzających wyświetlanie dziennik błędów, jak Szczegóły błędu może ujawnić luk w zabezpieczeniach lub inne poufne informacje. Jeśli zdecydujesz się wartość tego atrybutu 1 i włączyć dostęp zdalny do dziennika błędów, ważne jest, aby blokowania `elmah.axd` ścieżki, tak że tylko autoryzowane osoby odwiedzające do niego dostęp. Można to osiągnąć przez dodanie `<location>` elementu `Web.config` pliku.

Następująca konfiguracja zezwala na tylko dla użytkowników w roli administratora dostępu do strony sieci web dziennik błędów:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Rola administratora i trzech użytkowników w systemie - Scott Jisun i Alicja — dodano w [ *Konfigurowanie witryny sieci Web czy korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-cs.md). Scott użytkowników i Jisun są użytkownicy o roli administratora. Aby uzyskać więcej informacji o uwierzytelnianiu i autoryzacji, zapoznaj się Moje [samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Teraz można wyświetlić dziennik błędów w środowisku produkcyjnym przez użytkowników zdalnych; Odwołaj się do **3 rysunki**, **4**, i **5** dla zrzuty ekranu strony sieci web, dziennik błędów. Jednak jeśli użytkownik anonimowy lub bez uprawnień administratora próbuje wyświetlić stronę błędu dziennika zostanie automatycznie przekierowany do strony logowania (`Login.aspx`), jako **na rysunku nr 7** zawiera.

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Rysunek 7**: nieautoryzowani użytkownicy są automatycznie przekierowany do strony logowania  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Programowo rejestrowanie błędów

W ELMAH `ErrorLogModule` źródło określony dziennik moduł HTTP automatycznie rejestruje nieobsługiwanych wyjątków. Alternatywnie można rejestrować błąd bez konieczności podniesienia nieobsługiwany wyjątek przy użyciu `ErrorSignal` klasy i jej `Raise` metody. `Raise` Metody jest przekazywany `Exception` obiektu i rejestruje go tak, jakby ten wyjątek ma zostać zgłoszony i osiągnął środowiska uruchomieniowego ASP.NET nie jest obsługiwane. Różnica, jest jednak, że żądanie nadal wykonywane zwykle po `Raise` metoda została wywołana, element zgłaszany, nieobsługiwany wyjątek przerwania normalne wykonywanie żądania i powoduje, że moduł wykonawczy platformy ASP.NET można wyświetlić skonfigurowanych Strona błędu.

`ErrorSignal` Klasy jest przydatne w sytuacjach, gdy istnieje niektóre akcje, które może zakończyć się niepowodzeniem, ale jego uszkodzenia nie jest krytycznego do ogólnej wykonywanej operacji. Na przykład witryna sieci Web może zawierać formularz, który przyjmuje danych wejściowych użytkownika, jest on przechowywany w bazie danych, a następnie wysyła użytkownika wiadomość e-mail informująca że przetworzono informacji. Co powinno się zdarzyć, jeśli informacje są zapisywane w bazie danych pomyślnie, ale występuje błąd podczas wysyłania wiadomości e-mail? Jedną z opcji będzie Zgłoś wyjątek i wysłać do użytkownika do strony błędu. Jednak może to mylić użytkownika do planowania, który nie został zapisany wprowadzone informacje. Innym rozwiązaniem byłoby dziennika błędów związanych z pocztą e-mail, ale nie mogą zmieniać środowisko użytkownika w dowolny sposób. Jest to, gdy `ErrorSignal` przydaje się klasy.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>Powiadomienie o błędzie za pośrednictwem poczty E-mail

Wraz z rejestrowania błędów do bazy danych można również skonfigurować do określonego adresata poczty e-mail szczegóły błędów ELMAH. Ta funkcja jest zapewniana przez `ErrorMailModule` modułu HTTP; w związku z tym należy zarejestrować ten moduł HTTP w `Web.config` Aby wysyłać szczegóły błędu za pośrednictwem poczty e-mail.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

Następnie określ informacje dotyczące wiadomości e-mail błąd w `<elmah>` elementu `<errorMail>` sekcji i wskazujący nadawca i odbiorca, temat, adresu e-mail i określa, czy wiadomości e-mail są wysyłane asynchronicznie.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Z powyższymi ustawieniami w miejscu, gdy błąd środowiska uruchomieniowego występuje ELMAH wysyła wiadomość e-mail do support@example.com szczegóły błędu. ELMAH na błąd w wiadomości e-mail zawiera te same informacje wyświetlany błąd szczegóły stronie sieci web, czyli komunikat o błędzie, ślad stosu i zmiennych serwera (odwołują się do **rysunki 4** i **5**). Błąd wiadomości e-mail zawiera również zawartości wyjątku szczegóły żółty ekranem śmierci jako załącznik (`YSOD.html`).

**Rysunek 8** pokazuje ELMAH przez błąd e-mail generowane po przejściu na stronę `Genre.aspx?ID=foo`. Gdy **rysunek 8** pokazuje tylko błąd komunikat i stos śledzenia, zmiennych serwera są dołączone dodatkowe w dół w treści wiadomości e-mail.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Rysunek 8**: można skonfigurować ELMAH wysłać informacje o błędzie za pośrednictwem poczty E-mail  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Tylko rejestrowania błędów zainteresowań

Domyślnie ELMAH rejestruje szczegóły dotyczące każdej nieobsłużony wyjątek, w tym 404 oraz innych błędów HTTP. Możesz wydać ELMAH, aby zignorować to lub inne typy błędów za pomocą filtrowania błędu. Filtrowania logiki odbywa się przez jego ELMAH `ErrorFilterModule` modułu protokołu HTTP, musisz zarejestrować w `Web.config` aby można było używać logiki filtrowania. Reguły filtrowania są określone w `<errorFilter>` sekcji.

Następujący kod znaczników nakazuje ELMAH nie rejestrować błędy 404.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> Pamiętaj, aby można było używać błąd filtrowania, należy zarejestrować `ErrorFilterModule` moduł protokołu HTTP.


`<equal>` Element wewnątrz `<test>` sekcji jest określany jako potwierdzenia. Jeśli potwierdzenie zwraca wartość true, błąd są filtrowane przez ELMAH dziennika. Brak dostępnych innych potwierdzeń, w tym: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`i tak dalej. Można także połączyć potwierdzenia używając `<and>` i `<or>` operatorów logicznych. Co więcej można nawet obejmują proste wyrażenie JavaScript jako potwierdzenie, lub napisać własny potwierdzenia w języku C# lub Visual Basic.

Aby uzyskać więcej informacji na temat błędu ELMAH jego możliwości filtrowania odwoływać się do [filtrowania błąd sekcji](https://code.google.com/p/elmah/wiki/ErrorFiltering) w [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Podsumowanie

ELMAH udostępnia proste, ale mechanizm rejestrowania błędów w aplikacji sieci web ASP.NET. Jak system monitorowania kondycji firmy Microsoft ELMAH można rejestrować błędy bazy danych i może wysyłać szczegóły błędu deweloperowi za pośrednictwem poczty e-mail. W przeciwieństwie do systemu monitorowania zdrowia ELMAH obejmuje pomoc techniczna pole dla szerszej grupy magazynów danych dziennika błędów, w tym: Microsoft SQL Server, programu Microsoft Access, Oracle, pliki XML i kilka innych. Ponadto ELMAH zapewnia wbudowany mechanizm wyświetlanie dziennik błędów i szczegółowe informacje na temat określonego błędu ze strony sieci web `elmah.axd`. `elmah.axd` Strony można również renderować informacje o błędzie jako źródła danych RSS lub pliku wartości rozdzielanych przecinkami (CSV), który może odczytywać przy użyciu programu Microsoft Excel. Można także skonfigurować ELMAH błędy filtr z dziennika przy użyciu potwierdzenia deklaratywne lub programowego. I ELMAH mogą być używane z aplikacji 1.x wersji platformy ASP.NET.

Każdej wdrożonej aplikacji powinien mieć mechanizmu automatycznego rejestrowania nieobsługiwanych wyjątków i wysyłania powiadomień do zespołu deweloperów. Określa, czy to odbywa się przy użyciu monitorowania kondycji lub ELMAH jest dodatkowej. Innymi słowy rzeczywiście nie ma znaczenia znacznie przy użyciu monitorowania kondycji lub ELMAH; Ocena obu systemów, a następnie wybierz jedną, która najlepiej odpowiada Twoim potrzebom. Zasadniczo ważne jest umieścić mechanizmu w miejscu do rejestrowania nieobsługiwanych wyjątków w środowisku produkcyjnym.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [ELMAH — moduły rejestrowania błędów i obsługi](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Strona projektu ELMAH](https://code.google.com/p/elmah/) (źródła kodu, próbek, stron typu wiki)
- [Podłączanie ELMAH do aplikacji sieci Web można przechwycić nieobsługiwane wyjątki](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (klip wideo)
- [Strony dziennika błędów zabezpieczeń](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Tworzenie składników ASP.NET podłączany przy użyciu moduły HTTP i obsługi](https://msdn.microsoft.com/library/aa479332.aspx)
- [Samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Poprzednie](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [dalej](precompiling-your-website-cs.md)
