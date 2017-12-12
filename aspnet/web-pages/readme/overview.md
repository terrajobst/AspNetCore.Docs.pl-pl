---
uid: web-pages/readme/overview
title: Plik Readme programu WebMatrix | Dokumentacja firmy Microsoft
author: rick-anderson
description: Program WebMatrix i plik Readme programu ASP.NET Web Pages (Razor) wersji 1.0
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 90f24550d2bb50147bab6be545be63c1838f312a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="webmatrix-readme"></a>Plik Readme programu WebMatrix
====================
13 stycznia 2011

## <a name="contents"></a>Spis treści

> [!NOTE]
> Ten plik readme dotyczy wersji 1.0 programu WebMatrix.


- [Omówienie](#Overview)
- [Instalacja](#Installation_Notes)
- [Sposób publikowania aplikacji](#InstructionsForPublishingApplications)
- [Zmiany i problemów](#ChangesAndIssues)

    - [Instalacja programu WebMatrix w wersji 1.0](#Known_Issues_Installation)
    - [Strony sieci Web ASP.NET](#Known_Issues_ASPNET)
    - [Program WebMatrix](#Known_Issues_WebMatrix)
    - [Usługi IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalowanie aplikacji](#Known_Issues_Installing_Applications)
    - [Publikowanie aplikacji](#Known_Issues_Publishing_Applications)
- [Aby uzyskać więcej informacji](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Omówienie

> Microsoft WebMatrix 1.0 jest stos programowanie wolnej sieci web, który instaluje w minutach. Serwer sieci web jest zintegrowany z bazy danych i programowania platformy do utworzenia jednym zintegrowanym interfejsie. Program WebMatrix umożliwia upraszcza sposób kodu, testowania i publikowanie własnych ASP.NET i PHP witryny sieci Web, lub można użyć programu WebMatrix można uruchomić nowej witryny sieci Web przy użyciu popularnych aplikacji open source, takich jak DotNetNuke, Umbraco, WordPress lub Joomla. Program WebMatrix korzysta z tego samego zaawansowanego serwera, aparatu bazy danych i środowiska struktur, które zostanie uruchomione witryny sieci Web w Internecie, dzięki czemu przejście od projektowania do produkcji płynne i bezproblemowe.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalacja

> Aby zainstalować program WebMatrix w wersji 1.0, należy najpierw zainstalować [3.0 Instalatora platformy sieci Web Microsoft](https://go.microsoft.com/fwlink/?LinkID=194638). Po zainstalowaniu Instalatora platformy sieci Web, można ją zainstalować program WebMatrix.
> 
> Jeśli masz problemy podczas instalacji, zapoznaj się [Rozwiązywanie problemów dotyczących Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Sposób publikowania aplikacji

> Zobacz [instrukcje krok po kroku dotyczące publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Zmiany i problemów

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemy z instalacją 1.0 programu WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: WebMatrix 1.0 jest dostępna tylko na platformach, które obsługuje program Microsoft .NET Framework 4

> .NET Framework w wersji 4 jest wymagany dla programu WebMatrix. W niektórych przypadkach Instalator programu WebMatrix 1.0 umożliwi spróbuje zainstalować na platformie, która nie wchodzi w skład zestawu obsługiwanej konfiguracji. W szczególności Windows Vista bez dodatku SP1 dla aktualizacji będzie można rozpocząć instalację programu WebMatrix, ale składników .NET Framework 4 spowoduje niepowodzenie i zablokowanie instalacji.
> 
> **Obejście problemu**  
> Zainstaluj na obsługiwanych platform, w tym:
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

> Podczas instalowania programu SQL Server Compact 4.0 na komputerze 64-bitowy komputer ma tylko .NET Framework 3.5 SP1 Client Profile zainstalowany zarządzanych zestawów dla programu SQL Server Compact 4.0 nie są umieszczane w globalnej pamięci podręcznej zestawów (GAC). Zarządzanych zestawów, które nie są zainstalowane w GAC są:
> 
> - *System.Data.SqlServerCe.dll* (dostawcy ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Obejście problemu**  
> Odinstaluj program SQL Server Compact 4.0. Pobierz i zainstaluj pełną wersję programu .NET Framework 3.5 z dodatkiem SP1 z następującej lokalizacji:  
>   
> [Microsoft .NET Framework 3.5 z dodatkiem Service pack 1 (pełny pakiet)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Następnie ponownie zainstalować program SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problem: Nie można odinstalować za pomocą wiersza polecenia programu SQL Server Compact

> Dezinstalacja przy użyciu opcji wiersza polecenia programu SQL Server Compact nie działa w tej wersji.
> 
> **Obejście problemu**  
> Użyj *programy i funkcje* w Panelu sterowania systemu Windows, aby odinstalować program Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Strony sieci Web ASP.NET

W tej sekcji dokumentu opisano nowe funkcje, zmiany i znane problemy z wersji 1.0 ASP.NET Web Pages o składni Razor.

- [Nowe funkcje](#NewFeatures)
- [Zmiany](#Changes)
- [Problemy](#Issues)

#### <a id="NewFeatures"></a>Nowe funkcje

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nowy: Ustawienie konfiguracji dodane do wyłączenia Menedżera pakietów

> Nowy `asp:AdminManagerEnabled` klucz jest dostępny dla `<appSettings>` element *web.config* pliku, który można całkowicie wyłączyć Menedżera pakietów. Wartością domyślną dla tego elementu ma wartość true, co oznacza, że jeśli nie jest uwzględniony w *web.config* pliku Menedżera pakietów jest włączone. Aby wyłączyć Menedżera pakietów, Dodaj następujący element do *web.config* w katalogu głównym witryny sieci Web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>Zmiany

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Zmień: klucz "webPages:AdminFolderVirtualPath" zmieniona na "asp: AdminFolderVirtualPath"

> `webPages:AdminFolderVirtualPath` Klucz, który można dodać do *web.config* plik, aby określić lokalizację Menedżera pakietów została zmieniona na użyj `asp:` przestrzeni nazw zamiast `webPages` przestrzeni nazw. Jeśli używasz tego elementu, można zmienić jego nazwę w pliku konfiguracji.


#### <a id="Issues"></a>Znane problemy

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problem: Haseł użytkowników członkostwa nie został rozpoznany

> Algorytm tworzenie i przechowywanie haseł członkostwa (logowanie) został zmieniony na większe bezpieczeństwo. W związku z tym nie zostanie rozpoznana haseł przechowywanych dla elementów członkowskich (użytkownicy) utworzone w wersji Beta programu ASP.NET Razor. 
> 
> **Obejście** lokacji nie ma jeszcze wprowadzone do produkcji, usunięcie rekordów użytkowników z bazy danych członkostwa. Jeśli baza danych znajduje się na żywo, programowo ponownie wygenerować istniejące hasła w bazie danych członkostwa.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: Nieoczekiwanego zachowania w przypadku używania tabeli użytkownika niestandardowego dla członkostwa

> Aby zainicjować dostawcy członkostwa dla witryny sieci Web platformy ASP.NET Razor, należy wywołać `WebSecurity.InitializeDatabaseConnection` metody. (W programie WebMatrix, w szablonie witryny początkowej zawiera wywołanie tej metody w  *\_AppStart.cshtml* pliku.) Jeśli `autoCreateTables` parametr tej metody jest ustawiony na wartość true (domyślnie jest ustawiona wartość true w szablonie witryny początkowej), i jeśli nazwa tabeli nierozpoznany jest przekazywany do metody (drugi parametr), metoda nie zgłasza błąd. Zamiast tego automatycznie tworzy tabeli.
> 
> Może to być spowodowane problemem, jeśli zamierzasz używać tabeli użytkownika niestandardowego dla członkostwa, ale przekazywania do nazwy tabeli niewłaściwy `WebSecurity.InitializeDatabaseConnection` metody. Ponieważ metoda nie domyślnie Zgłoś błąd, jeśli tabela, które określisz nie istnieje, a zamiast tego tworzy nową tabelę, aplikacja może się pojawić się działać. Jednak kod aplikacji, która opiera się na tabeli użytkownika niestandardowego (i pól w nim) po pewnym czasie mogą nie być z nieoczekiwane błędy.
> 
> **Obejście problemu**  
> Upewnij się, że nazwa przekazano `InitializeDatabaseConnection` zgodna metoda profilu użytkownika tabeli w bazie danych członkostwa lub upewnij się, że `autoCreateTables` parametr ma wartość false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problem: Komunikat o błędzie "Module administrator wymaga dostępu do ~/App\_danych"

> W pewnych okolicznościach, w trakcie tworzenia użytkowników lub pracy z systemu członkostwa programu ASP.NET może spowodować błąd na stronie *modułu administrator wymaga dostępu do ~/App\_danych*. Dzieje się tak, jeśli konto, które uruchamiania usług IIS lub usług IIS Express nie ma uprawnień do tworzenia i zapisywania *aplikacji\_danych* folder w katalogu głównym witryny sieci Web. 
> 
> **Obejście** ręcznie utworzyć *aplikacji\_danych* folderu witryny sieci Web. Następnie upewnij się, że konto systemu Windows w obszarze (zazwyczaj Usługa sieciowa) jest uruchomiona aplikacja ma uprawnienia odczytu/zapisu dla folderów głównych aplikacji i podfoldery aplikacja do obróbki\_danych. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemów z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Błąd "Nie można wygenerować wystąpienia użytkownika programu SQL Server" problemu:

> Jeśli aplikacja sieci Web programu WebMatrix korzysta z programu SQL Server Express i działa IIS 7.5 w systemie Windows 7 lub Windows Server 2008 R2, można napotkać komunikat o błędzie wskazujący, że programu SQL Server nie można pobrać ścieżki lokalnej aplikacji użytkownika w czasie wykonywania.
> 
> **Obejście** upewnij się, czy konto systemu Windows działającą aplikację (zazwyczaj Usługa sieciowa) ma uprawnienia odczytu/zapisu dla folderów głównych aplikacji i podfoldery takich jak *aplikacji\_danych*. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemów z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problem: Pliki, które zawiera Menedżera pakietów zasobów lub Menedżera pakietów hasła są servable w usługach IIS 6.0 i starsze wersje

> Wdrażanie aplikacji stron sieci Web platformy ASP.NET (Razor), która została skompilowana przy użyciu wersji RC2, a ta aplikacja zawiera *password.txt* lub *packagesources.txt* plików w obszarze *App\_ Data/admin*, usług IIS 6.0 posłuży pliku, jeśli jest to wymagane, potencjalnie udostępnianie haseł dla swojego wystąpienia Menedżera pakietów. 
> 
> **Obejście** zmienić *password.txt* lub *packagesources.txt* pliku *password.config* lub *packagesources.config*. Domyślnie usługi IIS 6.0 nie obsługiwać pliki, których *.config* rozszerzenia. (W usługach IIS 7, żadne pliki w *aplikacji\_danych* folderu są obsługiwane, dzięki czemu nie trzeba zmienić nazwy plików.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problem: Odinstalowanie pakietów zainstalowanych przy użyciu wersji Beta 3 nie całkowicie usunąć składniki pakietu

> Jeśli zainstalowany pakiet w wersji Beta 3 za pomocą Menedżera pakietów, a następnie spróbuj odinstalować przy użyciu bieżącej wersji, pakiet nie został całkowicie odinstalowany. Za pomocą Menedżera pakietów **Odinstaluj** przycisku spowoduje usunięcie niektórych składników, ale pozostawia kod biblioteki pakietu i aktualizuje *package.config* pliku.
> 
> **Obejście problemu**   
> Wykonaj następujące kroki:  
> 1. Usuń *aplikacji\_Data\packages* folderu. Spowoduje to usunięcie wszystkich pakietów.   
> 2. Usuń *packages.config* w katalogu głównym witryny sieci Web.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problem: W programie Visual Studio, wywoływania Menedżera pakietów opartych na sieci web przełącza do trybu offline

> Jeśli pracujesz w programie Visual Studio (nie WebMatrix) i użyj  *\_admin* funkcji, aby uruchomić Menedżera pakietów, Visual Studio przełącza do trybu offline i przesyła *aplikacji\_ offline.htm* w katalogu głównym witryny sieci Web, która zakłóca możliwość korzystania z Menedżera pakietów.
> 
> [!NOTE]
> Mimo że najczęściej zobaczysz tego zachowania, korzystając z interfejsu Menedżera pakietów opartych na sieci web, takie samo zachowanie występuje, gdy dodać, usunąć ani zmodyfikować plików w *aplikacji\_danych* folderu.
> 
> **Obejście problemu**   
> Aby pracować z pakietami w programie Visual Studio, należy użyć rozszerzenia NuGet zamiast Menedżera pakietów opartych na sieci web. Aby uzyskać informacje, zobacz [dokumentacji NuGet](https://docs.microsoft.com/nuget/). Jeśli pracujesz z innymi plikami w *aplikacji\_danych* folderu, pomyśl o pozostawieniu plików, aby uniknąć tego problemu w innych miejscach. Jeśli nie jest praktyczne, Usuń *aplikacji\_offline.htm* pliku ręcznie lub poczekaj, aż lokacji wraca do trybu online automatycznie (domyślnie 30 sekund).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Visual Studio IntelliSense projektu szablonów i dostępna tylko na platformie ASP.NET MVC w wersji 3

> Instalowanie składnika ASP.NET Web Pages nie również zainstalować narzędzi dla programu Visual Studio takie jak szablony IntelliSense i projektu dla aplikacji ASP.NET Web Pages.
> 
> **Obejście** Aby używać szablonów IntelliSense i projektu dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstalować platformy ASP.NET MVC 3 RC albo za pomocą Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Odczytywanie źródła danych lub innych danych zewnętrznych za pośrednictwem serwera proxy

> Jeśli serwer z systemem lokacji znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacje dotyczące serwera proxy w *web.config* pliku, aby można było odczytać informacje, które pochodzą z poza witryną. Na przykład, jeśli używasz `ReCaptcha` pomocnika, pomocnika komunikuje się z usługą reCAPTCHA, ale może zostać zablokowany przez serwer proxy. Podobnie źródła danych, które są używane w programie ASP.NET Web Pages, np. źródła danych używane przez Menedżera pakietów, mogą wymagać konfiguracji serwera proxy.
> 
> Jeśli masz problemy w pracy z zewnętrznej usługi lub Praca z pakietem źródła danych należy umieścić następujące elementy w katalogu głównego aplikacji *web.config* pliku:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Aby uzyskać więcej informacji na temat konfigurowania serwera proxy, zobacz [ &lt;proxy&gt; elementu (ustawienia sieciowe)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) w witrynie MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Odinstalowywanie programu .NET Framework w wersji 4 wyłącza stron ASP.NET Web Pages o składni Razor

> Po odinstalowaniu programu .NET Framework w wersji 4 i zainstaluj go ponownie, ASP.NET Web Pages o składni Razor jest wyłączona. Strony z *.cshtml* rozszerzenia nie działać prawidłowo. Strony ASP.NET Web Pages rejestruje zestawu w folderze głównym maszyny *web.config* plików i usunięcie programu .NET Framework spowoduje usunięcie tego pliku. Ponowna instalacja programu .NET Framework instaluje nową wersję pliku konfiguracji, ale nie dodać odwołanie do zestawu stron sieci Web programu ASP.NET.
> 
> **Obejście** po ponownym zainstalowaniu programu .NET Framework, zainstaluj ponownie stron ASP.NET Web Pages o składni Razor. Spowoduje to dodanie następujący element do *web.config* w katalogu głównym komputera, który zazwyczaj znajduje się w następującej lokalizacji:  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problem: Adresy URL bez rozszerzeń nie zostanie znaleziony.cshtml/.vbhtml pliki w usługach IIS 7 i IIS 7.5

> W usługach IIS 7 lub usług IIS 7.5 żądania o adresie URL podobnie do następującej nie są w stanie odnaleźć strony, które mają *.cshtml* lub *.vbhtml* rozszerzenia:  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> Problem występuje, ponieważ ponowne zapisywanie adresów URL nie jest włączona domyślnie dla usług IIS 7 i IIS 7.5. Scenariusz likeliest jest, że nie ma problem podczas testowania, lokalnie za pomocą usług IIS Express, ale wystąpić podczas wdrażania witryny sieci Web do obsługi witryny sieci Web.
> 
> **Obejście problemu**
> 
> - Jeśli masz kontrolę nad komputerem serwera na komputerze serwera, zainstaluj aktualizację opisaną w [aktualizacja jest dostępna, że umożliwia niektórych obsługi usług IIS 7.0 lub 7.5 usług IIS do obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).
> - Jeśli nie masz kontrolę nad komputerem serwera (na przykład wdrażasz do obsługi witryny sieci Web), dodaj następującą wartość do witryny sieci Web *web.config* pliku: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Wdrażanie aplikacji na komputerze, na którym nie ma zainstalowany program SQL Server Compact

> Aplikacje, które obejmują baz danych programu SQL Server Compact można uruchomić na komputerze, na którym program SQL Server Compact nie jest zainstalowany. Microsoft WebMatrix 1.0 automatycznie kopiuje tych plików binarnych dla Ciebie i wykonuje odpowiednie *web.config* pliku transformacji.
> 
> **Obejście** Jeśli musisz skopiować te pliki i upewnić *web.config* zmiany pliku ręcznie, wykonaj następujące czynności:
> 
> 1. Kopiowanie zestawów aparatu bazy danych do *Bin* folderze (i jego podfolderach) aplikacji na komputerze docelowym:  
> 
>     - Kopiuj *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>         **Aby** *\Bin*
>     - Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*** do***\Bin\x86*
>     - Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **do***\Bin\amd64*
> 2. W folderze głównym witryny sieci Web, należy utworzyć lub otworzyć *web.config* pliku. (W wersji 1.0 programu WebMatrix, ten typ pliku jest dostępna po kliknięciu **wszystkie** w **wybierz typ pliku** okno dialogowe.)
> 3. Dodaj następujący element jako element podrzędny `<configuration>` elementu (nie znajduje się w `<system.web>` element):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: "Baza danych" i "WebGrid" pomocników nie działają w trybie średniego zaufania w języku Visual Basic

> Jeśli używasz programu Visual Basic (Tworzenie *.vbhtml* plików), `Database` i `WebGrid` pomocników nie będzie działać, jeśli aplikacja jest skonfigurowana do użycia w trybie średniego zaufania.
> 
> **Obejście problemu**  
> Jeśli używasz programu Visual Studio 2010, ten problem można rozwiązać przez zainstalowanie wersji dodatku Service Pack 1. Dopóki z ostateczną wersją wersji z dodatkiem SP1 jest dostępny, można pobrać wersji Beta programu z dodatkiem SP1 z [Microsoft Visual Studio 2010 z dodatkiem Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) stronie w witrynie Microsoft Download Center.   
>   
> Jeśli to nie jest praktyczne lub jeśli nie używasz programu Visual Studio 2010, można tymczasowo ustawić aplikacji do korzystania z pełnego zaufania.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problem: "ApplicationPart" zasoby są dostępne z zewnątrz

> Jeśli zestaw zawiera obiekty, które pochodzi z `ApplicationPart` klasy, że zasobów zestawu są udostępniane przez `ResourceRouteHandler` klasy. Rozważmy na przykład następujący adres URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> To żądanie pobiera wszystkie ciągi zasobów w *System.Web.WebPages.Administration.dll* zestawu. Pobierane są wszystkie zasoby osadzone (nawet te, które nie mają być obsługiwane jako zawartość statyczna). Jeśli zasoby osadzone zawierają poufne informacje, to stanowią zagrożenie bezpieczeństwa. 
> 
> **Obejście problemu**   
> W przypadku utworzenia **ApplicationPart** obiektów, upewnij się, że zasoby osadzone skojarzone z tym **ApplicationPart** obiektu zestawu nie zawierają informacji poufnych.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>Program WebMatrix

> [!NOTE]
> Aby uzyskać informacje dotyczące problemów z instalacją dla programu WebMatrix, zobacz [problemy z instalacją programu WebMatrix](#Known_Issues_Installation) we wcześniejszej części tego dokumentu.


W tej sekcji dokumentu opisano znane problemy dotyczące środowiska programowania programu WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problem: Zmiany w nazwa użytkownika lub hasło ciągu połączenia bazy danych w pliku web.config nie są widoczne w obszarze roboczym baz danych

> **Obejście problemu**  
> 
> 1. W *web.config* pliku, Zmień nazwę bazy danych w parametrach połączenia (na przykład dodać "1" do niego).
> 2. Zapisz *web.config* pliku.
> 3. Kliknij przycisk **baz danych** i Odśwież.
> 4. Zmień nazwę bazy danych w parametrach połączenia w *web.config* pliku do oryginalnej nazwy bazy danych.
> 5. Zapisz *web.config* pliku.
> 6. Kliknij przycisk **baz danych** i Odśwież.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problem: Nie można usunąć folderami utworzonych przez program WebMatrix

> Jeśli program WebMatrix jest uruchomiona, korzystając z podwyższonym poziomem uprawnień (to znaczy uruchomiona za pomocą programu WebMatrix **Uruchom jako Administrator** opcji w systemie Windows), folderów, które zostały utworzone przy użyciu programu WebMatrix, nie można usunąć przy użyciu Eksploratora Windows.
> 
> **Obejście problemu**  
> Uruchom Eksploratora Windows, korzystając z podwyższonym poziomem uprawnień. Wykonaj następujące kroki:  
> 
> 1. W systemie Windows, kliknij przycisk **Start**.
> 2. Wprowadź "Eksploratora Windows", a następnie kliknij prawym przyciskiem myszy pozycję **Eksploratora Windows**.
> 3. Kliknij przycisk **Uruchom jako Administrator**. Następnie można usunąć foldery.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: WebMatrix 1.0 nie jest w stanie do wykonania niektórych zadań, które wymagają podniesionych uprawnień

> Program WebMatrix w wersji 1.0 nie jest w stanie do wykonania niektórych zadań, które wymagają podniesionych uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:
> 
> - W systemie Windows Vista lub Windows 7 zalogowano się za pomocą konta, które nie ma uprawnień administracyjnych i kontroli konta użytkownika (UAC) jest wyłączona.
> - Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.
> 
> **Obejście problemu**  
> Większość zadań w programie WebMatrix 1.0 nie wymaga uprawnień administracyjnych. Dla tych, które wykonują można wykonać operacji jako administrator lub wykonaj następujące kroki:
> 
> - W systemie Windows Vista lub Windows 7 należy włączyć funkcji Kontrola konta użytkownika.
> - W systemie Windows XP należy dodać użytkownika do grupy zabezpieczeń Administratorzy.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Lokacji z galerii" jest wyłączona.

> **Witryny sieci Web galerii** opcja jest wyłączona, jeśli Instalator platformy sieci Web 3.0 nie jest zainstalowany.
> 
> **Obejście problemu**  
> Zainstaluj [Instalatora platformy sieci Web firmy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problem: Google Chrome nie jest dostępna jako opcja wykonywania

> Google Chrome nie jest wyświetlana na liście przeglądarek w obszarze **Uruchom** na **Home** kartę.
> 
> **Obejście problemu**  
> Niektóre wersje programu Google Chrome nie rejestrują się poprawnie z funkcją programy domyślne w systemie Windows. Jako obejście, należy uruchomić Google Chrome, kliknij polecenie *Dostosuj i kontroli Google Chrome* menu, kliknij przycisk *opcje*, a następnie kliknij przycisk *upewnij Google Chrome przeglądarce domyślne*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problem: Okno dialogowe "Klucz obcy" nie zezwala na wprowadzenie klucza podstawowego

> **Klucz obcy** okno dialogowe nie pozwala na wprowadzanie nazwy klucza podstawowego z tabeli klucza podstawowego.
> 
> **Obejście problemu**  
> Jest to zamierzone. Nie trzeba wprowadzić nazwę klucza podstawowego z tabeli klucza podstawowego.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problem: IntelliSense nie jest dostępna w programie WebMatrix dla Razor składni języka C# i Visual Basic

> IntelliSense jest obsługiwana w programie WebMatrix dla kodu HTML i CSS. Jednak nie jest dostępna dla innych języków. 
> 
> **Obejście problemu**   
> Brak.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problem: Elementy, które nie są odpowiednie kontekstowej sugeruje IntelliSense dla kodu HTML i CSS

> IntelliSense dla znaczników w programie WebMatrix obsługuje HTML za pomocą [XHTML 1.0 przejściowe schematu](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) i CSS przy użyciu [schematu CSS 2.1](http://www.w3.org/TR/CSS2/). Ponieważ IntelliSense jest oparta na tych określonych schematów, niektórych znaczników, atrybutów i właściwości mogą sugerowane, które nie są odpowiednie dla bieżącej definicji stylu lub strony. Dla kodu HTML może on również prowadzić do sugestie nieoczekiwany w zawartości, które mogą być interpretowane jako źle sformułowane XHTML (na przykład gdy tagi nie są zamknięte). Ten problem może być bardziej zauważalne, jeśli punkt wstawiania znajduje się wewnątrz tagu niekompletne; w takim przypadku IntelliSense może sugerować otwierania nowych znaczników lub oferuje inne nieprawidłowe sugestie. 
> 
> **Obejście problemu**   
> Dla kodu HTML upewnij się, że pracujesz na stronie XHTML poprawnie sformułowany, pełną. CSS nie istnieje obejście.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problem: IntelliSense nie zostanie wywołany, podczas pisania

> W czasie IntelliSense nie może być wywołany, jako wprowadzania w edytorze HTML i CSS. W szczególności to może się zdarzyć, gdy punkt wstawiania znajduje się bezpośrednio obok inny element lub na końcu pliku. 
> 
> **Obejście problemu**   
> Upewnij się, że jest odstępem wokół punktu wstawiania i nie jest punkt wstawiania na końcu pliku. IntelliSense można również ręcznie wywoływać, naciskając klawisze Ctrl + spacja.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problem: Bez interfejsu użytkownika jest dostępna dla wyłączenie IntelliSense

> 1.0 dla programu WebMatrix zapewnia nie interfejsu użytkownika lub gestu wyłączenie funkcji IntelliSense. 
> 
> **Obejście problemu**   
> Uruchom program WebMatrix za pomocą następujące polecenie, w tym przełącznika, które wyłączają IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>Usługi IIS Express

Usługi IIS Express ma własny plik readme, który jest dostępny pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkId=207675&amp;clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact ma własny plik readme, który jest dostępny pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkId=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Aby uzyskać informacje o problemach, które dotyczą instalowania programu SQL Server Compact jako część programu WebMatrix, zobacz [problemy z instalacją programu WebMatrix](#Known_Issues_Installation) we wcześniejszej części tego dokumentu.

### <a id="Known_Issues_Installing_Applications"></a>Instalowanie aplikacji

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: Instalowanie aplikacji może długo trwać, jeśli folder Moje dokumenty użytkownika jest przekierowywany do udziału sieciowego

> **Obejście problemu**  
> Brak. Aplikacja może potrwać kilka minut, aby zainstalować, ale zostanie zainstalowany poprawnie.


### <a id="Known_Issues_Publishing_Applications"></a>Publikowanie aplikacji

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problem: "wymaga się, że nie można uzyskać uprawnienia" błąd podczas publikowania bazy danych SQL Compact

> Program WebMatrix nie w pełni obsługuje wdrażanie obsługi plików binarnych dla programu SQL Server Compact do serwera z programem .NET Framework w wersji 3.5 i konfiguracji trybie średniego zaufania.
> 
> **Obejście problemu**  
> Preferowany obejście polega na zainstalowaniu programu .NET Framework 4 na serwerze. Można również wykonać następujące czynności:
> 
> 1. Dodaj następujące elementy do `SecurityClasses` sekcji *Web\_MediumTrust.config* pliku:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Utwórz nowy zestaw uprawnień na *Web\_MediumTrust.config* pliku wymagane następujące uprawnienia:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Zastosuj uprawnienia do programu SQL Server Compact, ustawiając następujące elementy *Web\_MediumTrust.config* pliku:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problem: Galerii i PhpBB aplikacji sieci web wyświetla błąd "Usługa jest niedostępna" po opublikowaniu

> W pewnych okolicznościach publikowania aplikacji powoduje błąd "Usługa jest niedostępna".
> 
> **Obejście problemu**  
> W programie WebMatrix, Dodaj ukośnik odwrotny (\) na końcu nazwy serwera w **ustawień publikowania** okna, a następnie opublikować ponownie aplikację.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problem: Układ WWW Moodle i łącza są przerwane po opublikowaniu

> Po opublikowaniu aplikacji Moodle, aplikacja nie działa prawidłowo.
> 
> **Obejście problemu**  
> W programie WebMatrix, Dodaj ukośnika (/) na końcu **nazwa witryny** w **ustawień publikowania** okna, a następnie opublikować ponownie aplikację.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problem: Publikowanie nopCommerce zakończy się niepowodzeniem z powodu błędu bazy danych

> Program nopCommerce publikowania nie powiedzie się i zgłasza błąd bazy danych, takich jak "wstawiony nop\_tabeli dziennika nie powiodło się."
> 
> **Obejście problemu**  
> 
> 1. W programie WebMatrix, kliknij przycisk **Uruchom** można uruchomić program nopCommerce lokalnie.
> 2. Zaloguj się do strony Administracja.
> 3. Kliknij przycisk **systemu** menu.
> 4. Kliknij przycisk **dziennika** opcji.
> 5. Kliknij przycisk **Wyczyść dziennik** przycisku.
> 6. Program nopCommerce ponownie opublikować.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problem: Silverstripe CMS Wyświetla "HTTP 500 PHP FCGI Error" po pobraniu opublikowanej witryny

> **Obejście problemu**  
> Po kliknięciu **pobierania opublikowane lokacji**, Pomiń `silverstripe-cache/manifest_main` w **Podgląd publikowania**. Ten plik jest używany na potrzeby buforowania i jest specyficzna dla każdego komputera.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problem: "Błąd serwera w"/"aplikacja" Wyświetla Subtext podczas pobierania opublikowanej witryny

> **Obejście problemu**  
> Otwórz witrynę *web.config* plików i zastąp nazwę użytkownika i hasło w ciągu połączenia bazy danych przy użyciu poświadczeń administratora programu SQL Server (poświadczenia "sa").
> 
> Alternatywnie, wykonaj następujące kroki, aby nadać kontu użytkownika, użytkownik jest zalogowany przy użyciu `db_owner` uprawnienia:
> 
> 1. Zainstaluj program SQL Server Management Studio za pomocą Instalatora platformy sieci Web.
> 2. Połączenie z lokalnym wystąpieniem programu SQL Server Express (domyślnie `.\SQLEXPRESS`).
> 3. Kliknij przycisk **baz danych** &gt; *[localSubtextDatabase]* &gt; **zabezpieczeń** &gt; **użytkowników** &gt; *[localSubtextUser*] (domyślnie jest `subtextuser`], kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **właściwości**.
> 4. Wybierz **db\_właściciela** w sekcji członkostwo roli.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problem: Lokacji może nie działać po opublikowaniu, jeśli w polu "Docelowy adres URL" nie jest poprzedzona http:// lub https://

> W **ustawienia publikowania** okno dialogowe, jeśli docelowy adres URL nie rozpoczyna się od `http://` lub `https://`, lokacji może nie działać po wdrożeniu.
> 
> **Obejście problemu**  
> Upewnij się, że przed opublikowaniem witryny, docelowy adres URL na **ustawień publikowania** okno dialogowe rozpoczyna się od `http://` lub `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problem: Publikowanie bazy danych MySQL zakończy się niepowodzeniem z powodu błędu "nie można opublikować bazy danych. Może to nastąpić w przypadku zdalnej bazy danych nie można uruchomić skryptu."

> Błąd może wystąpić z kilku powodów. Jedną z przyczyn, zostanie wyświetlony ten błąd jest Jeśli skryptu bazy danych zawiera znak pojedynczego cudzysłowu ('), a domyślny zestaw znaków miejsce docelowe danych MySQL nie jest UTF-8.
> 
> **Obejście problemu**  
> Ustaw domyślny zestaw znaków dla zdalnej bazy danych MySQL na UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problem: Niektóre łącza nie są widoczne w DotNetNuke po publikowania lub pobierania witryny

> Jeśli publikowanie lub pobrać witryny DotNetNuke, konieczne może być wyczyścić pamięć podręczną, aby uzyskać nowe łącza pojawiać się w witrynie.
> 
> **Obejście problemu**
> 
> 1. Zaloguj się jako "Host".
> 2. Przejdź do menu hosta i wybierz **ustawienia hosta**.
> 3. Przewiń w dół i w obszarze **Zaawansowane ustawienia**, rozwiń węzeł **ustawienia wydajności**.
> 4. Kliknij przycisk **Wyczyść pamięć podręczną** łącze do strony.
> 5. Przejdź do dolnej części strony, a następnie ponownie uruchom aplikację.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problem: Niektóre łącza w program AtomSite są przerwane po pobraniu opublikowanej witryny

> **Obejście problemu**  
> W *service.config* pliku *users.config* plik i wszystkie *.xml* plików, Zastąp ciąg adresu URL (na przykład `http://myhost.com/atomsite`) z lokalna (na przykład `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problem: Aplikacji opartych na MySQL, takich jak WordPress nie można opublikować i zgłoś błąd bazy danych

> Domyślnie program WebMatrix instaluje MySQL z zestawem znaków UTF-8. Jeżeli instalujesz MySQL samodzielnie, i nie jest zestaw znaków UTF-8 (na przykład jest Latin1), proces publikowania dla baz danych może zakończyć się niepowodzeniem.
> 
> **Obejście problemu**
> 
> 1. Zmień zestaw MySQL UTF-8 znaków. (Aby uzyskać więcej informacji, zobacz [zestaw znaków serwera i sortowanie](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) w witrynie sieci Web MySQL.)
> 2. Zainstaluj ponownie aplikację.
> 3. Ponownie opublikować aplikację.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problem: "Pobierania opublikowane lokacji" kończy się niepowodzeniem dla aplikacji, które skonfigurowano w przeglądarce

> Niektóre aplikacje (na przykład Kentico CMS) trzeba uruchomić je w przeglądarce w celu przeprowadzenia instalacji po instalacji, takich jak tworzenie bazy danych. Jeśli spróbujesz opublikować aplikację, jak to bez zakończeniu instalacji opartej na przeglądarce, próby pobrania tej samej lokacji z serwerem zdalnym zakończy się niepowodzeniem.
> 
> **Obejście problemu**  
> Zakończenie instalacji opartej na przeglądarce przed opublikowaniem tej witryny.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problem: "Pobierania opublikowane lokacji" kończy się niepowodzeniem z powodu błędu bazy danych DotNetNuke i Kooboo CMS

> Jeśli próba pobrania aplikacji z serwera i poświadczenia administratora w ciągu połączenia bazy danych w **ustawień publikowania** okna dialogowego, może się pojawić następujący błąd w dzienniku publikowania:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Obejście problemu**  
> Jeśli jest to możliwe, ponownie opublikować lokacji (lub opublikować) przy użyciu poświadczeń administratora innych niż dla bazy danych.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Aby uzyskać więcej informacji

Aby uzyskać więcej informacji o programie WebMatrix w wersji 1.0 zobacz następujące witryny sieci Web:

- [Program IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/Web](https://www.microsoft.com/web)

© Microsoft 2011 Corporation. Wszelkie prawa zastrzeżone. [Warunki użytkowania](https://msdn.microsoft.com/en-us/cc300389.aspx).
