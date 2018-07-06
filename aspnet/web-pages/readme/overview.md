---
uid: web-pages/readme/overview
title: Plik Readme programu WebMatrix | Dokumentacja firmy Microsoft
author: rick-anderson
description: Program WebMatrix i plik Readme programu ASP.NET Web Pages (Razor) wersji 1.0
ms.author: aspnetcontent
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 8aa0eedb3156ca33905c106b72d5fa553a917b16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822865"
---
<a name="webmatrix-readme"></a>Plik Readme programu WebMatrix
====================
13 stycznia 2011

## <a name="contents"></a>Spis treści

> [!NOTE]
> Ten plik readme ma zastosowanie do wersji 1.0 programu WebMatrix.


- [Omówienie](#Overview)
- [Instalacja](#Installation_Notes)
- [Sposób publikowania aplikacji](#InstructionsForPublishingApplications)
- [Zmiany oraz problemów](#ChangesAndIssues)

    - [Instalacja programu WebMatrix w wersji 1.0](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [Usługi IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalowanie aplikacji](#Known_Issues_Installing_Applications)
    - [Publikowanie aplikacji](#Known_Issues_Publishing_Applications)
- [Aby uzyskać więcej informacji](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Omówienie

> Microsoft WebMatrix w wersji 1.0 to bezpłatny internetowy stosu wdrożenia, który instaluje w ciągu kilku minut. Serwer sieci web jest zintegrowany z bazy danych i programowania, struktur, aby utworzyć jednym zintegrowanym interfejsie. Przy użyciu programu WebMatrix można dostosować sposób, w kodzie, testowanie i publikowanie własnej witrynie internetowej platformy ASP.NET i PHP, lub przy użyciu programu WebMatrix można uruchomić nowej witryny sieci Web przy użyciu popularnych aplikacji typu open source, takich jak DotNetNuke, Umbraco, WordPress i Joomla. Program WebMatrix korzysta w tym samym serwerze zaawansowane aplikacje internetowe, aparat bazy danych i środowiska struktur, które uruchomi witryny sieci Web w Internecie, co sprawia, że przejście od projektowania do produkcji płynne i bezproblemowe.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalacja

> Aby zainstalować program WebMatrix w wersji 1.0, należy najpierw zainstalować [3.0 Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/fwlink/?LinkID=194638). Po zainstalowaniu Instalatora platformy sieci Web służy do zainstalowania programu WebMatrix.
> 
> Jeśli masz problemy podczas instalacji, zapoznaj się [Rozwiązywanie problemów za pomocą Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Sposób publikowania aplikacji

> Zobacz [szczegółowe instrukcje dotyczące publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Zmiany oraz problemów

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemy z instalacją wersji 1.0 programu WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: Program WebMatrix w wersji 1.0 jest dostępny tylko na platformach, które obsługują program Microsoft .NET Framework 4

> .NET Framework w wersji 4 jest wymagany dla programu WebMatrix. W niektórych przypadkach Instalatora programu WebMatrix w wersji 1.0 będzie można spróbować zainstalować na platformie, która nie jest częścią zestawu obsługiwanej konfiguracji. W szczególności Windows Vista bez dodatku SP1 dla aktualizacji będzie można rozpocząć instalację programu WebMatrix, ale składnik .NET Framework 4 będą się nie powieść i blokowanie instalacji.
> 
> **Obejście problemu**  
> Zainstaluj na obsługiwanych platformach, w tym:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 z dodatkiem R2
> - Windows Vista z dodatkiem SP1 lub nowszym
> - Windows XP z dodatkiem SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: Nie można zainstalować program WebMatrix w wersji 1.0, jeśli zainstalowano program Microsoft Visual Studio 2008 bez programu Microsoft Visual Studio 2008 z dodatkiem SP1

> **Obejście problemu**  
> Zainstaluj [programu Microsoft Visual Studio 2008 z dodatkiem SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z Centrum pobierania Microsoft.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problem: Niektóre zestawy dla programu SQL Server Compact 4.0 nie są zainstalowane w GAC

> Zestawy zarządzane dla programu SQL Server Compact 4.0 nie są umieszczane w globalnej pamięci podręcznej zestawów (GAC), podczas instalacji programu SQL Server Compact 4.0 na komputerze 64-bitowym, a komputer ma tylko .NET Framework 3.5 z dodatkiem SP1 Client Profile zainstalowane. Zestawów zarządzanych, które nie są zainstalowane w GAC są:
> 
> - *System.Data.SqlServerCe.dll* (ADO.NET provider)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )
> 
> **Obejście problemu**  
> Odinstalowanie programu SQL Server Compact 4.0. Pobierz i zainstaluj pełną wersję programu .NET Framework 3.5 SP1 w następującej lokalizacji:  
>   
> [Microsoft .NET Framework 3.5 z dodatkiem Service pack 1 (pełny pakiet)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Następnie ponownie zainstalować program SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problem: Nie można odinstalować programu SQL Server Compact przy użyciu wiersza polecenia

> Używanie opcji wiersza polecenia programu SQL Server Compact dezinstalacji nie działa w tej wersji.
> 
> **Obejście problemu**  
> Użyj *programy i funkcje* w Panelu sterowania Windows, aby odinstalować program Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

Ta sekcja dokumentu opisano nowe funkcje, zmiany i znane problemy związane z wersji 1.0 składnika ASP.NET Web Pages o składni Razor.

- [Nowe funkcje](#NewFeatures)
- [Zmiany](#Changes)
- [Problemy](#Issues)

#### <a id="NewFeatures"></a>  Nowe funkcje

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nowość: Ustawienia konfiguracji są dodawane do wyłączenia Menedżera pakietów

> Nowy `asp:AdminManagerEnabled` klucz jest dostępny dla `<appSettings>` element *web.config* pliku, który pozwala w całkowicie wyłączyć Menedżera pakietów. Wartość domyślna dla tego elementu ma wartość true, co oznacza, że jeśli nie jest uwzględniony w *web.config* pliku, czyli Menedżer pakietów jest włączone. Aby wyłączyć Menedżera pakietów, Dodaj następujący element do *web.config* pliku w katalogu głównym witryny sieci Web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Zmiany

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Zmień: "webPages:AdminFolderVirtualPath" klucz zmieniona na "asp: AdminFolderVirtualPath"

> `webPages:AdminFolderVirtualPath` Klucza, które mogą być dodawane do *web.config* plik, aby określić lokalizację Menedżera pakietów została zmieniona na użyj `asp:` przestrzeń nazw, a `webPages` przestrzeni nazw. Jeśli używasz tego elementu, można zmienić jego nazwę w pliku konfiguracji.


#### <a id="Issues"></a>  Znane problemy

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problem: Hasła użytkowników członkostwa, już nie został rozpoznany.

> Algorytm tworzenia i przechowywania haseł członkostwa (identyfikator logowania) został zmieniony na większe bezpieczeństwo. W rezultacie nie zostanie rozpoznana haseł zapisanych dla członków (użytkownicy) utworzone w wersji Beta programu ASP.NET Razor. 
> 
> **Obejście** Jeśli witryny nie ma jeszcze umieszczone w środowisku produkcyjnym, należy usunąć rekordy użytkowników z bazy danych członkostwa. Jeśli baza danych znajduje się na żywo, programowe ponowne wygenerowanie istniejących haseł w bazie danych członkostwa.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: Nieoczekiwane zachowanie w przypadku używania tabeli użytkownika niestandardowego dla członkostwa

> Aby zainicjować dostawcy członkostwa ASP.NET Razor, witryny sieci Web, należy wywołać `WebSecurity.InitializeDatabaseConnection` metody. (W programie WebMatrix, w szablonie witryny początkowej zawiera wywołanie tej metody w  *\_AppStart.cshtml* pliku.) Jeśli `autoCreateTables` parametr tej metody jest ustawiona na wartość true (domyślnie jest ustawiona wartość true w szablonie witryny początkowej), i jeśli nazwa tabeli nierozpoznany jest przekazywany do metody (drugi parametr), metoda zgłasza błąd. Zamiast tego automatycznie tworzy tabelę.
> 
> Może to być problemem, jeśli zamierzasz używać tabeli użytkownika niestandardowego dla członkostwa, ale nazwy tabeli problem, aby przekazać `WebSecurity.InitializeDatabaseConnection` metody. Ponieważ metoda nie domyślnie Zgłoś błąd, jeśli nie ma tabeli, które określisz, a zamiast tego tworzy nową tabelę, aplikacja może się pojawić działa. Jednak kod aplikacji, która opiera się na tabeli użytkownika niestandardowego (i w polach w nim) po pewnym czasie może zakończyć się nieoczekiwanym błędem.
> 
> **Obejście problemu**  
> Upewnij się, że nazwa jest przekazywany w `InitializeDatabaseConnection` metoda dopasowania profilu użytkownika tabeli w bazie danych członkostwa lub upewnij się, że `autoCreateTables` parametr ma wartość false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problem: Komunikat o błędzie "moduł administratora wymaga dostępu do ~/App\_dane"

> W niektórych okolicznościach próby tworzenia użytkowników lub pracy z systemu członkostwa programu ASP.NET może spowodować błąd na stronie *modułu administratora wymaga dostępu do ~/App\_danych*. Dzieje się tak w przypadku konta, na której działają usługi IIS lub IIS Express nie ma uprawnień do tworzenia i zapisu *aplikacji\_danych* folder w katalogu głównym witryny sieci Web. 
> 
> **Obejście** ręcznie utworzyć *aplikacji\_danych* folderu witryny sieci Web. Następnie upewnij się, że konta Windows, której aplikacja działa w ramach (zwykle Usługa sieciowa) ma uprawnienia odczytu/zapisu dla folderów głównych aplikacji i podfoldery, takie jak aplikacja\_danych. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "nie można wygenerować wystąpienia użytkownika programu SQL Server" Błąd

> Aplikacja sieci Web programu WebMatrix korzysta z programu SQL Server Express jest zasilany z usług IIS 7.5 Windows 7 lub Windows Server 2008 R2, użytkownik może zostać wyświetlony komunikat o błędzie wskazujący, że programu SQL Server nie można pobrać ścieżki lokalnej aplikacji przez użytkownika w czasie wykonywania.
> 
> **Obejście** upewnij się, Windows działającą aplikację (zwykle Usługa sieciowa) kontu uprawnienia odczytu/zapisu dla folderów głównych aplikacji i jego podfolderach takich jak *aplikacji\_danych*. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problem: Pliki, które zawiera zasoby Menedżera pakietów lub haseł Menedżera pakietów są servable w ramach usług IIS 6.0 i starsze wersje

> Wdrażanie aplikacji ASP.NET Web Pages (Razor), który został zbudowany przy użyciu wersji RC2, a aplikacja zawiera *password.txt* lub *packagesources.txt* plik *App\_ Dane/admin*, usług IIS 6.0 posłuży pliku, jeśli jest to wymagane, potencjalnie udostępnianie haseł dla swojego wystąpienia Menedżera pakietów. 
> 
> **Obejście** Zmień nazwę *password.txt* lub *packagesources.txt* plik *password.config* lub *packagesources.config*. Domyślnie usługi IIS 6.0 nie służy plików, które mają *.config* rozszerzenia. (W usługach IIS 7, żaden z plików w *aplikacji\_danych* folderu są one obsługiwane, więc nie trzeba zmienić nazwy plików.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problem: Odinstalowywanie pakietów zainstalowanych przy użyciu wersji Beta 3 nie całkowicie usunąć składniki pakietu

> Jeśli zainstalowano pakiet, przy użyciu Menedżera pakietów w wersji Beta 3, a następnie spróbuj odinstalować go za pomocą bieżącej wersji, pakiet nie został całkowicie odinstalowany. Przy użyciu Menedżera pakietów **Odinstaluj** przycisku spowoduje usunięcie niektórych składników, ale pozostawia pakietu kodu biblioteki i nie powoduje aktualizacji *plików package.config* pliku.
> 
> **Obejście problemu**   
> Wykonaj następujące kroki:  
> 1. Usuń *aplikacji\_Data\packages* folderu. Spowoduje to usunięcie wszystkich pakietów.   
> 2. Usuń *packages.config* pliku w katalogu głównym witryny sieci Web.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problem: W programie Visual Studio, wywołując Menedżera pakietów opartych na sieci web przełącza do trybu offline

> Jeśli pracujesz w programie Visual Studio (nie program WebMatrix) i użyj  *\_administratora* funkcji, aby uruchomić Menedżera pakietów programu Visual Studio przełącza do trybu offline i publikuje *aplikacji\_ offline.htm* w katalogu głównym witryny sieci Web, która zakłóca możliwość korzystania z Menedżera pakietów.
> 
> [!NOTE]
> Mimo że będzie najczęściej widzisz to zachowanie podczas korzystania z interfejsu Menedżera pakietów opartych na sieci web, takie samo zachowanie występuje, gdy dodać, usunąć lub zmodyfikować pliki w *aplikacji\_danych* folderu.
> 
> **Obejście problemu**   
> Aby pracować z pakietów w programie Visual Studio, należy użyć rozszerzenia NuGet zamiast Menedżera pakietów opartych na sieci web. Aby uzyskać informacje, zobacz [dokumentacja programu NuGet](https://docs.microsoft.com/nuget/). Jeśli pracujesz z innymi plikami w *aplikacji\_danych* folderu, pomyśl o pozostawieniu plików, w innym miejscu, aby uniknąć tego problemu. Jeśli nie jest to praktyczne, Usuń *aplikacji\_offline.htm* pliku ręcznie lub poczekaj, aż lokacji powróci do trybu online automatycznie (domyślnie po 30 sekundach).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Program Visual Studio IntelliSense oraz szablony projektów dostępne tylko we wzorcu ASP.NET MVC w wersji 3

> Instalowanie składnika ASP.NET Web Pages nie także zainstalować narzędzia dla programu Visual Studio takie jak IntelliSense oraz szablony projektów dla aplikacji ASP.NET Web Pages.
> 
> **Obejście** Aby użyć funkcji IntelliSense oraz szablony projektów dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstaluj program ASP.NET MVC 3 RC za pomocą Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Odczytu źródła danych lub innych zewnętrznych danych za pośrednictwem serwera proxy

> Jeśli serwer z systemem lokacji znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacje o serwerze proxy w *web.config* pliku, aby można było odczytać informacje, które pochodzą z spoza witryny. Na przykład, jeśli używasz `ReCaptcha` pomocnika pomocnika komunikuje się z usługą reCAPTCHA, ale może być blokowana przez serwer proxy. Podobnie źródła danych, które są używane w składniku ASP.NET Web Pages, takie jak źródło danych używane przez Menedżera pakietów może wymagać konfiguracji serwera proxy.
> 
> Jeśli wystąpią problemy w pracy z zewnętrznej usługi lub nie działa z pakietem kanału informacyjnego, umieść następujące elementy katalogu głównego aplikacji *web.config* pliku:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Aby uzyskać więcej informacji na temat konfigurowania serwera proxy, zobacz [ &lt;proxy&gt; — Element (ustawienia sieci)](https://msdn.microsoft.com/library/sa91de1e.aspx) w witrynie MSDN w sieci Web.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Odinstalowywanie programu .NET Framework w wersji 4 wyłącza stron ASP.NET Web Pages o składni Razor

> Po odinstalowaniu programu .NET Framework w wersji 4 i zainstaluj go ponownie, ASP.NET Web Pages o składni Razor jest wyłączona. Strony z *.cshtml* rozszerzenia nie działać poprawnie. ASP.NET Web Pages rejestruje zestaw w katalogu głównym maszyny *web.config* plików i usunięcie programu .NET Framework spowoduje usunięcie tego pliku. Ponowne zainstalowanie programu .NET Framework instaluje nową wersję pliku konfiguracji, ale nie powoduje dodania odwołania dla zestawu stron sieci Web platformy ASP.NET.
> 
> **Obejście** po ponownym zainstalowaniu programu .NET Framework, należy ponownie zainstalować składnika ASP.NET Web Pages o składni Razor. Spowoduje to dodanie następującego elementu do *web.config* plik w folderze głównym komputera, który zazwyczaj znajduje się w następującej lokalizacji:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problem: Adresy URL bez rozszerzeń nie uważają, że pliki.cshtml/.vbhtml w usługach IIS 7 lub usług IIS 7.5

> W usługach IIS 7 lub usług IIS 7.5 żądań z adresu URL podobnie do poniższego nie będą mogli odnaleźć strony, które mają *.cshtml* lub *.vbhtml* rozszerzenia:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Problem pojawia się, ponieważ ponownego zapisywania adresów URL nie jest włączone domyślnie dla usług IIS 7 i IIS 7.5. Scenariusz likeliest jest, że nie ma problem podczas testowania lokalnie przy użyciu usług IIS Express, ale występują w przypadku wdrażania witryny sieci Web do hostowania witryny sieci Web.
> 
> **Obejście problemu**
> 
> - Jeśli masz kontrolę nad komputerem serwera, na komputerze serwera, zainstaluj aktualizację opisaną na stronie [aktualizacja jest dostępna, że umożliwia niektórych obsługi usług IIS 7.0 lub 7.5 usług IIS do obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).
> - Jeśli nie masz kontrolę nad komputerem serwera (na przykład wdrażasz do hostingu witryny sieci Web), Dodaj następujący kod do witryny sieci Web *web.config* pliku: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Wdrażanie aplikacji na komputerze, który nie ma zainstalowany program SQL Server Compact

> Aplikacje, które zawierają bazy danych programu SQL Server Compact mogą być uruchamiane na komputerze, na którym programu SQL Server Compact nie jest zainstalowany. Microsoft WebMatrix w wersji 1.0 automatycznie kopiuje te pliki binarne dla Ciebie i wykonuje odpowiednie *web.config* pliku przekształcenia.
> 
> **Obejście** Jeśli potrzebujesz kopiowania tych plików i *web.config* zmian w plikach ręcznie, wykonaj następujące czynności:
> 
> 1. Kopiowanie zestawów aparatu bazy danych, które mają *Bin* folderze (i jego podfolderach) w aplikacji na komputerze docelowym:  
> 
>    - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **Aby** *\Bin*
>    - Kopiuj <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>do</em></strong>\Bin\x86*
>    - Kopiuj <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>do</strong><em>\Bin\amd64</em>
> 
> 2. W folderze głównym witryny sieci Web, należy utworzyć lub otworzyć *web.config* pliku. (W wersji 1.0 programu WebMatrix, jest dostępna po kliknięciu tego typu pliku **wszystkich** w **wybierz typ pliku** okno dialogowe.)
> 3. Dodaj następujący element jako element podrzędny elementu `<configuration>` — element (poza `<system.web>` elementu):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: "Baza danych" i "WebGrid" pomocników nie działają w trybie średniego zaufania w języku Visual Basic

> Jeśli używasz języka Visual Basic (Tworzenie *.vbhtml* plików), `Database` i `WebGrid` pomocników nie będzie działać, jeśli aplikacja jest skonfigurowana do użycia w trybie średniego zaufania.
> 
> **Obejście problemu**  
> Jeśli używasz programu Visual Studio 2010, możesz rozwiązać ten problem, instalując wersji dodatku Service Pack 1. Do czasu udostępnienia ostateczną wersją wersji z dodatkiem SP1 możesz pobrać wersji Beta programu z dodatkiem SP1 z [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) stronie Microsoft Download Center.   
>   
> Jeśli nie jest to praktyczne, lub jeśli nie używasz programu Visual Studio 2010, można tymczasowo ustawić aplikacji można używać pełnego zaufania.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problem: "ApplicationPart" zasoby są dostępne z zewnątrz

> Jeśli zestaw zawiera obiekty, które jest pochodną `ApplicationPart` klasy, że zasoby zestawu są udostępniane przez `ResourceRouteHandler` klasy. Na przykład rozważmy następujący adres URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> To żądanie pobiera wszystkie ciągi zasobów w *System.Web.WebPages.Administration.dll* zestawu. Pobierane są wszystkie zasoby osadzone, (nawet te, które nie są przeznaczone do służyć jako zawartość statyczna). Jeśli zasobów osadzonych zawiera poufne informacje, to stanowią zagrożenie bezpieczeństwa. 
> 
> **Obejście problemu**   
> Jeśli tworzysz **ApplicationPart** obiektu, upewnij się, że zasoby osadzone skojarzony, **ApplicationPart** obiektu zestawu, nie zawierają informacji poufnych.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Aby uzyskać informacji na temat problemów z instalacją dla programu WebMatrix, zobacz [problemy z instalacją programu WebMatrix](#Known_Issues_Installation) we wcześniejszej części tego dokumentu.


W tej sekcji dokumentu opisano znane problemy dotyczące środowiska programistycznego programu WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problem: Zmiany nazwy użytkownika lub hasło parametrów połączenia bazy danych w pliku web.config nie są widoczne w obszarze roboczym baz danych

> **Obejście problemu**  
> 
> 1. W *web.config* plików, Zmień nazwę bazy danych w parametrach połączenia (na przykład dodać "1" do niego).
> 2. Zapisz *web.config* pliku.
> 3. Kliknij przycisk **baz danych** i odświeżania.
> 4. Zmień nazwę bazy danych w parametrach połączenia w *web.config* nazwę bazy danych w oryginalnym pliku.
> 5. Zapisz *web.config* pliku.
> 6. Kliknij przycisk **baz danych** i odświeżania.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problem: Nie można usunąć foldery utworzone przez program WebMatrix

> Jeśli program WebMatrix jest uruchomiona przy użyciu podniesionych uprawnień (czyli ułatwiają rozpoczęcie korzystania z programu WebMatrix **Uruchom jako Administrator** opcji Windows), nie można usunąć foldery, które są tworzone przez program WebMatrix za pomocą Eksploratora Windows.
> 
> **Obejście problemu**  
> Uruchom Eksploratora Windows, używając podniesionego poziomu uprawnień. Wykonaj następujące kroki:  
> 
> 1. W Windows, kliknij przycisk **Start**.
> 2. Wprowadź "Windows Explorer", a następnie kliknij prawym przyciskiem myszy wpis dla **Eksplorator Windows**.
> 3. Kliknij przycisk **Uruchom jako Administrator**. Następnie można usunąć folderów.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: Program WebMatrix w wersji 1.0 nie jest w stanie wykonywanie określonych zadań, które wymagają podniesionych uprawnień

> Program WebMatrix w wersji 1.0 nie jest w stanie wykonywanie określonych zadań, które wymagają podniesionych uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:
> 
> - W Windows Vista lub Windows 7 zalogowano się za pomocą konta, które nie ma uprawnienia administracyjne, a Kontrola konta użytkownika (UAC) jest wyłączona.
> - Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.
> 
> **Obejście problemu**  
> Większość zadań w programie WebMatrix w wersji 1.0 nie wymagają uprawnień administracyjnych. Dla osób, które wykonują można wykonać operacji, ponieważ administrator lub wykonaj następujące kroki:
> 
> - W Windows Vista lub Windows 7 włączenie funkcji Kontrola konta użytkownika.
> - Windows XP należy dodać użytkownika do grupy zabezpieczeń Administratorzy.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Witryny z internetowej galerii" jest wyłączona.

> **Witryny sieci Web galerii** opcja jest wyłączona, jeśli nie zainstalowano 3.0 Instalatora platformy sieci Web.
> 
> **Obejście problemu**  
> Zainstaluj [Instalatora platformy sieci Web firmy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problem: Google Chrome nie jest dostępna jako opcja wykonywania

> Google Chrome nie jest wyświetlana na liście przeglądarek, w obszarze **Uruchom** na **Home** kartę.
> 
> **Obejście problemu**  
> Niektóre wersje programu Google Chrome nie rejestrują się prawidłowo z tej funkcji domyślne programy w programie Windows. Obejść ten problem, uruchom program Google Chrome, kliknij przycisk *dostosowywanie i kontroli Google Chrome* menu, kliknij przycisk *opcje*, a następnie kliknij przycisk *upewnij Google Chrome w mojej przeglądarce domyślnej*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problem: Okno dialogowe "Klucz obcy" nie zezwala na wprowadzanie klucza podstawowego

> **Klucz obcy** okno dialogowe pozwala na wprowadzanie nazwy klucza podstawowego z tabeli klucza podstawowego.
> 
> **Obejście problemu**  
> Jest to zamierzone. Nie musisz wprowadzić nazwę klucza podstawowego z tabeli klucza podstawowego.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problem: Funkcja IntelliSense nie jest dostępne w programie WebMatrix Razor składni, C# lub Visual Basic

> Technologia IntelliSense jest obsługiwana w programie WebMatrix dla języków HTML i CSS. Jednak nie jest dostępny dla innych języków. 
> 
> **Obejście problemu**   
> Brak.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problem: Funkcja IntelliSense dla kodu HTML i CSS sugeruje elementy, które nie są odpowiednie kontekstowe

> Funkcja IntelliSense dla kodu znaczników w programie WebMatrix obsługuje HTML za pomocą [XHTML 1.0 przejściowe schematu](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) i przy użyciu CSS [schematu CSS 2.1](http://www.w3.org/TR/CSS2/). Ponieważ IntelliSense jest oparta na te określone schematy, niektóre znaczniki, atrybuty lub właściwości może być sugerowane, które nie są odpowiednie dla bieżącej definicji stylu lub strony. Dla kodu HTML może ona również prowadzić do nieoczekiwanych sugestie w zawartości, które mogą być interpretowane jako źle sformułowane XHTML (na przykład, gdy tagi nie zostały zamknięte). Ten problem może być bardziej zauważalne, jeśli punkt wstawiania znajduje się wewnątrz tagu niekompletne; w takim przypadku funkcja IntelliSense może Sugeruj nowe tagi otwierające lub oferują inne nieprawidłowe sugestie. 
> 
> **Obejście problemu**   
> Dla kodu HTML upewnij się, czy działają w obrębie strony XHTML pełną, poprawnie sformułowany. CSS nie ma sposobu obejścia.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problem: Funkcja IntelliSense nie jest wywoływany, podczas pisania

> W czasie funkcja IntelliSense nie może być wywołana, zgodnie z wprowadzanych HTML i CSS w edytorze. W szczególności to może się zdarzyć, gdy punkt wstawiania znajduje się bezpośrednio obok innego elementu, lub na końcu pliku. 
> 
> **Obejście problemu**   
> Upewnij się, że jest odstęp wokół punktu wstawiania, i czy punkt wstawiania nie jest na końcu pliku. Technologia IntelliSense można także wywoływać ręcznie, naciskając klawisze Ctrl + spacja.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problem: Nie interfejs użytkownika jest dostępny do wyłączania funkcji IntelliSense

> Program WebMatrix w wersji 1.0 zapewnia nie interfejsu użytkownika lub gest wyłączenie funkcji IntelliSense. 
> 
> **Obejście problemu**   
> Uruchom program WebMatrix, używając następującego polecenia, w tym przełącznik, który powoduje wyłączenie funkcji IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

Usługi IIS Express ma swój własny plik readme, który jest dostępny pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact ma swój własny plik readme, który jest dostępny pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Aby uzyskać informacje o problemach, które dotyczą instalowania programu SQL Server Compact jako część programu WebMatrix, zobacz [problemy z instalacją programu WebMatrix](#Known_Issues_Installation) we wcześniejszej części tego dokumentu.

### <a id="Known_Issues_Installing_Applications"></a>  Instalowanie aplikacji

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: Instalowanie aplikacji może zająć dużo czasu, jeśli folder Moje dokumenty użytkownika jest przekierowywany do udziału sieciowego

> **Obejście problemu**  
> Brak. Aplikacja może potrwać chwilę, aby zainstalować, ale zostanie zainstalowany poprawnie.


### <a id="Known_Issues_Publishing_Applications"></a>  Publikowanie aplikacji

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problem: "wymagane uprawnienia nie może zostać pobrany" błąd podczas publikowania bazy danych programu SQL Compact

> Program WebMatrix obsługuje w pełni wdrażanie obsługi danych binarnych dla programu SQL Server Compact do serwera z programem .NET Framework w wersji 3.5 i konfiguracji trybie średniego zaufania.
> 
> **Obejście problemu**  
> Preferowany obejście polega na zainstalowanie programu .NET Framework 4 na serwerze. Alternatywnie wykonaj następujące czynności:
> 
> 1. Dodaj następujące elementy do `SecurityClasses` sekcji *Web\_MediumTrust.config* pliku:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Utwórz nowy zestaw uprawnień *Web\_MediumTrust.config* plik z następujące wymagane uprawnienia:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Zastosuj zestaw uprawnień, aby program SQL Server Compact, umieszczając następujące elementy *Web\_MediumTrust.config* pliku:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problem: Galerii i PhpBB aplikacji sieci web wyświetla błąd "Usługa jest niedostępna" po opublikowaniu

> W niektórych okolicznościach publikowania aplikacji powoduje wystąpienie błędu "Usługa jest niedostępna".
> 
> **Obejście problemu**  
> W programie WebMatrix, należy dodać ukośnik odwrotny (\) na końcu nazwę serwera w **ustawień publikowania** okna, a następnie opublikować aplikację ponownie.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problem: Układu witryny sieci Web w Moodle i łącza nie działają po opublikowaniu

> Po opublikowaniu aplikacji Moodle, aplikacja nie działa prawidłowo.
> 
> **Obejście problemu**  
> W programie WebMatrix, należy dodać ukośnika (/) na końcu **Nazwa lokacji** pole **ustawień publikowania** okna, a następnie opublikować aplikację ponownie.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problem: Publikowanie witryny systemu nopCommerce zakończy się niepowodzeniem z powodu błędu bazy danych

> Publikowanie witryny systemu nopCommerce kończy się niepowodzeniem i zgłasza błąd bazy danych, takich jak "Wstawianie nop\_tabeli dziennika nie powiodło się."
> 
> **Obejście problemu**  
> 
> 1. W programie WebMatrix, kliknij przycisk **Uruchom** można uruchomić witryny systemu nopCommerce lokalnie.
> 2. Zaloguj się do strony administrowania.
> 3. Kliknij przycisk **systemu** menu.
> 4. Kliknij przycisk **dziennika** opcji.
> 5. Kliknij przycisk **Wyczyść dziennik** przycisku.
> 6. Program nopCommerce opublikuj go ponownie.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problem: Silverstripe CMS Wyświetla "HTTP 500 PHP FCGI błąd" po pobraniu opublikowanej witryny

> **Obejście problemu**  
> Po kliknięciu **pobierania opublikowano witrynę**, Pomiń `silverstripe-cache/manifest_main` w **Podgląd publikowania**. Ten plik jest używany na potrzeby buforowania i jest specyficzny dla każdego komputera.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problem: Podtekstu Wyświetla "Błąd serwera w aplikacji» /»" po pobraniu opublikowanej witryny

> **Obejście problemu**  
> Otwórz witrynę programu *web.config* i zastąp nazwę użytkownika i hasło w parametrach połączenia bazy danych przy użyciu poświadczeń administratora programu SQL Server (poświadczenia "sa").
> 
> Alternatywnie, wykonaj następujące kroki, aby nadać kontu użytkownika, użytkownik jest zalogowany przy użyciu `db_owner` uprawnienia:
> 
> 1. Zainstaluj program SQL Server Management Studio za pomocą Instalatora platformy sieci Web.
> 2. Łączenie z lokalnym wystąpieniem programu SQL Server Express (domyślnie `.\SQLEXPRESS`).
> 3. Kliknij przycisk **baz danych** &gt; *[localSubtextDatabase]* &gt; **zabezpieczeń** &gt; **użytkowników** &gt; *[localSubtextUser*] (wartość domyślna to `subtextuser`], kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **właściwości**.
> 4. Wybierz **db\_właściciela** w sekcji członkostwa roli.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problem: Lokacji może nie działać po opublikowaniu, jeśli pole "Docelowego adresu URL" nie jest poprzedzony znakiem http:// lub https://

> W **ustawień publikowania** okno dialogowe, jeśli docelowy adres URL rozpoczyna się od `http://` lub `https://`, witryna może nie działać po wdrożeniu.
> 
> **Obejście problemu**  
> Upewnij się, że przed opublikowaniem witryny, docelowy adres URL na **ustawień publikowania** zaczyna się okno dialogowe `http://` lub `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problem: Publikowanie bazy danych MySQL nie powiodło się z powodu błędu "nie można opublikować bazę danych. Możliwe, że zdalna baza danych nie można uruchomić skryptu."

> Ten błąd może wystąpić z kilku powodów. Jedną z przyczyn tego błędu są widoczne jest, jeśli skrypt bazy danych zawiera znak pojedynczego cudzysłowu ('), jak i docelowej bazy danych MySQL na domyślny zestaw znaków nie jest na UTF-8.
> 
> **Obejście problemu**  
> Ustaw domyślny zestaw znaków dla zdalnej bazy danych MySQL na UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problem: Niektóre łącza nie są widoczne w DotNetNuke po publikacji lub pobierania witryny

> Jeśli publikowanie lub pobrać witryny DotNetNuke, może być konieczne wyczyszczenie pamięci podręcznej, aby uzyskać nowe linki, które ma być wyświetlana w witrynie.
> 
> **Obejście problemu**
> 
> 1. Zaloguj się jako "Host".
> 2. Przejdź do menu hosta i wybierz **ustawienia hosta**.
> 3. Przewiń w dół i w obszarze **Zaawansowane ustawienia**, rozwiń węzeł **ustawienia wydajności**.
> 4. Kliknij przycisk **Wyczyść pamięć podręczną** link do strony.
> 5. Przejdź do dolnej części strony i ponownym uruchomieniu aplikacji.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problem: Niektóre linki w program AtomSite nie działają po pobraniu opublikowanej witryny

> **Obejście problemu**  
> W *service.config* pliku *users.config* pliku i wszystkie *.xml* pliki, Zastąp ciąg adresu URL (na przykład `http://myhost.com/atomsite`) przy użyciu lokalnego (na przykład `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problem: Aplikacji takich jak WordPress z MySQL nie można opublikować i zgłoś błąd bazy danych

> Domyślnie program WebMatrix instaluje MySQL z zestawem znaków UTF-8. Jeśli instalowanie bazy danych MySQL na własną rękę, a nie jest zestaw znaków UTF-8 (na przykład jest Latin1), proces publikowania dla baz danych może zakończyć się niepowodzeniem.
> 
> **Obejście problemu**
> 
> 1. Zmień zestaw MySQL na UTF-8 znaków. (Aby uzyskać więcej informacji, zobacz [zestaw znaków serwera i sortowania](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) w witrynie internetowej MySQL.)
> 2. Zainstaluj aplikację.
> 3. Ponownie opublikować aplikację.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problem: "Pobierania opublikowano witrynę" kończy się niepowodzeniem dla aplikacji, które skonfigurowano oparte na przeglądarce

> Niektóre aplikacje (na przykład Kentico CMS) wymagane jest ich uruchamiania w przeglądarce, aby można było wykonać po instalacji, takich jak tworzenie bazy danych. Jeśli spróbujesz opublikować aplikację tak, nie kończą działania Instalatora opartego na przeglądarce, próby pobrania tej samej lokacji z serwera zdalnego nie powiedzie się.
> 
> **Obejście problemu**  
> Zakończ Instalatora opartego na przeglądarce, przed opublikowaniem w witrynie.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problem: "Pobierania opublikowano witrynę" nie powiedzie się z powodu błędu bazy danych DotNetNuke i Kooboo CMS

> Jeśli spróbujesz pobrać aplikację z serwera i poświadczenia administratora w parametrach połączenia bazy danych w **ustawień publikowania** okno dialogowe, może się pojawić następujący błąd w dzienniku publikowania:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Obejście problemu**  
> Jeśli jest to praktyczne, ponownie opublikować witryny (lub opublikować) przy użyciu poświadczeń bez uprawnień administratora dla bazy danych.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Aby uzyskać więcej informacji

Aby uzyskać więcej informacji na temat programu WebMatrix w wersji 1.0 zobacz następujące witryny sieci Web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Wszelkie prawa zastrzeżone. [Warunki użytkowania](https://msdn.microsoft.cos/cc300389.aspx).
