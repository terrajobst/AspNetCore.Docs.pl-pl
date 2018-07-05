---
uid: web-pages/readme/beta3
title: I plik Readme programu ASP.NET Web Pages (Razor) w wersji Beta 3 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Macierz sieci Web i plik Readme ASP.NET Web Pages (Razor) w wersji Beta 3
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5aa609bf31499485dc7a1298fa689f3a7cee4774
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363536"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Macierz sieci Web i plik Readme ASP.NET Web Pages (Razor) w wersji Beta 3
====================
> Macierz sieci Web i plik Readme ASP.NET Web Pages (Razor) w wersji Beta 3

9 listopada 2010

## <a name="contents"></a>Spis treści

- [Omówienie](#Overview)
- [Instalacja](#Installation_Notes)
- [Nowe funkcje, zmiany i znane problemy w wersji Beta 3](#Known_Issues)

    - [Problemy z instalacją programu WebMatrix](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalowanie aplikacji](#Known_Issues_Installing_Applications)
    - [Publikowanie aplikacji](#Known_Issues_Publishing_Applications)
    - [Inne problemy](#Known_Issues_Other_Issues)
- [Aby uzyskać więcej informacji](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Omówienie

> Microsoft WebMatrix Beta to bezpłatny internetowy stosu wdrożenia, który instaluje w ciągu kilku minut. Serwer sieci web jest zintegrowany z bazy danych i programowania, struktur, aby utworzyć jednym zintegrowanym interfejsie. Program WebMatrix w wersji Beta umożliwia usprawnienie sposób kodu, testowania i publikowania własnej witrynie internetowej platformy ASP.NET i PHP lub wersji Beta programu WebMatrix można użyć do uruchomienia nowej witryny sieci Web przy użyciu popularnych aplikacji typu open source, takich jak DotNetNuke, Umbraco, WordPress i Joomla. Program WebMatrix w wersji Beta korzysta z tym samym serwerze zaawansowane aplikacje internetowe, aparat bazy danych i środowiska struktur, które uruchomi witryny sieci Web w Internecie, co sprawia, że przejście od projektowania do produkcji płynne i bezproblemowe.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalacja

> Aby zainstalować program WebMatrix w wersji Beta 3, należy użyć [3.0 Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/fwlink/?LinkID=194638). Po zainstalowaniu Instalatora platformy sieci Web możesz można użyć, aby zainstalować program WebMatrix w wersji Beta 3.
> 
> Jeśli masz problemy podczas instalacji, zapoznaj się [Rozwiązywanie problemów za pomocą Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instrukcje dotyczące publikowania aplikacji

> Zobacz [szczegółowe instrukcje dotyczące publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nowe funkcje i zmiany, andKnown problemy

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalacja wersji Beta programu WebMatrix 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: Program WebMatrix w wersji Beta 3 jest dostępna tylko na platformach, które obsługują program Microsoft .NET Framework 4

> .NET Framework w wersji 4 jest wymagany dla programu WebMatrix w wersji Beta. W niektórych przypadkach Instalator programu WebMatrix w wersji Beta będzie można spróbować zainstalować na platformie, która nie jest częścią zestawu obsługiwanej konfiguracji. W szczególności Windows Vista bez dodatku SP1 dla aktualizacji będzie można rozpocząć instalację programu WebMatrix w wersji Beta, ale składnik .NET Framework 4 będą się nie powieść i blokowanie instalacji.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: Nie można zainstalować program WebMatrix w wersji Beta 3, jeśli zainstalowano program Microsoft Visual Studio 2008 bez programu Microsoft Visual Studio 2008 z dodatkiem SP1

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

Ta sekcja dokumentu opisano nowe funkcje, zmiany i znane problemy związane z wersji Beta 3 programu ASP.NET Web Pages o składni Razor.

- [Nowe funkcje](#NewFeatures)
- [Zmiany](#Changes)
- [Problemy](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nowe funkcje w wersji Beta 3 dla stron ASP.NET Web Pages o składni Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nowość: "Html.Raw" Metoda renderuje niekodowany kod znaczników

> Nowy `Html.Raw` metoda umożliwia renderowania kodu znaczników HTML jako kod znaczników, zamiast renderowania zakodowanych danych wyjściowych. (Domyślnie ASP.NET Razor koduje ciągi przed ich renderowaniem.) Składnia jest następująca:
> 
> `Html.Raw(value)`
> 
> Poniższy przykład pokazuje, jak używać `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Zmiany w wersji Beta 3 dla stron ASP.NET Web Pages o składni Razor

#### <a name="change-hrefattribute-method-removed"></a>Zmiana: Metoda "HrefAttribute" usunięte

> `HrefAttribute` Metody `WebPage` klasa została usunięta. Tego pomocnika był używany do kodowania znaków niebezpiecznych w adresach URL. Jest już wymagany, ponieważ ASP.NET Razor automatycznie koduje ciągi. (Nowe `Html.Raw` metody do renderowania ciągi niezaszyfrowana.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Zmiana: Składnia deklaratywne "@helper" pomocników zmienione

> W wersji Beta 3 programu ASP.NET zmieni się sposób jej analizuje pomocników, które są tworzone przy użyciu `@helper` składni. W zasadzie `@helper` składni teraz jest analizowany jako blok kodu, a nie jako kod znaczników, który może zawierać kod w bloku. W związku z tym, kod w Pomocniku nie musi być ujęte w nawiasy `@{ }` bloków. Z drugiej strony, znaczników w Pomocniku musi być jawnie uwzględnione w elementach HTML lub ASP.NET Razor `<text></text>` tagów.
> 
> Na przykład następująca `@helper` składni działa w wersji Beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> W wersji Beta 3 należy zmienić tego pomocnika, aby wyglądał jak na poniższym przykładzie:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Należy zauważyć, że `@{ }` znaków wokół kod początkowy w Pomocniku nie jest już używana. Jest to spowodowane zawartość pomocników są traktowane jako blok kodu, domyślnie. Pomocnik renderuje kod znaczników, który rozpoczyna się od otwarcia `<a>` tagu. Jeśli pomocnika musi renderowania zwykłego tekstu lub tagi, które nie zawierają tagu zamykającego (na przykład `<meta>` tagi), zawartości do renderowania musi znajdować się w `<text></text>` tagów.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Change: "WebPageContext.HttpContext" removed

> `WebPageContext.HttpContext` Właściwości zostały usunięte. Zamiast nich należy używać słów kluczowych `HttpContext.Current`. ( `WebPageContext.HttpContext` Właściwości po prostu opakowane to.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Zmiana: Pomocnika "Facebook" przeniesiona do nowego pakietu

> `Facebook` Pomocnika została przeniesiona do *Facebook.Helper* biblioteki, która obejmuje `Facebook` pomocnika oraz dodatkowe funkcje. Należy zainstalować tę bibliotekę jako oddzielny pakiet, zgodnie z opisem w "Instalowanie pomocników przy użyciu Menedżera pakietów" w tym samouczku [wprowadzenie do strony ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Zmiana: Członkostwa, ról i bezpieczeństwo typów przenosi do nowego zestawu

> Następujące typy zostały przeniesione do `WebMatrix.WebData` zestawu:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Zmiana: Klasa "TagBuilder" przeniesiona do zestawu System.Web.WebPages.dll

> `TagBuilde` Klasy r został przeniesiony do zestawu System.Web.WebPages.dll. Wcześniej taka konfiguracja była w zestawie, które było częścią platformy ASP.NET MVC. Ta zmiana oznacza, że jest konieczne instalowanie platformy ASP.NET MVC, aby można było używać `TagBuilder` klasy.
> 
> Jednak klasy nadal znajduje się w `System.Web.Mvc` przestrzeni nazw. Aby można było używać `TagBuilder` klasy (na przykład w niestandardowych ASP.NET Razor pomocnika), musi odwoływać się przestrzeń nazw (na przykład poprzez dodanie `@using System.Web.Mvc` w kodzie).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Zmiany: Składnia sprawdzania poprawności, które zostały zmienione; żądanie Klasa "Weryfikacji" usunięte

> W wersji Beta 3, aby wyłączyć sprawdzanie poprawności dla poszczególnych pól lub zestaw pól, możesz wywołać `Validation.Exclude` metody, przekazując nazwę lub nazwy pola do wykluczenia z weryfikacji. Nowa składnia jest dostępna w wersji Beta 3 w celu pominięcia weryfikacji. `Validation` Metodą używaną w wersji Beta 3 został usunięty.
> 
> > [!NOTE]
> > Jeśli nie można wyłączyć weryfikację żądań, jeśli użytkownicy podejmą próbę przekazania kod znaczników HTML (np. przy użyciu edytora tekstu sformatowanego na stronie), witryny sieci Web zgłosi błąd, taki jak *wartością zawartości potencjalnie niebezpiecznych Request.Form zostało wykryte w kliencie*i dane wejściowe użytkownika nie jest akceptowane. Jeśli wyłączysz weryfikację żądań, należy ręcznie sprawdzić dane wejściowe użytkownika, aby upewnić się, że nie zawierają potencjalnie niebezpiecznego kodu znaczników lub skrypt, używając na przykład [biblioteki skryptów programu Microsoft zapobieganie wielu lokacji V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Aby wyłączyć automatycznego żądania weryfikacji, należy wywołać `Request.Unvalidated` metody, podając mu nazwę pola lub inny obiekt wpis, który chcesz pominąć weryfikację żądań dla. Ta metoda służy do pominięcia weryfikacji dla wszystkich elementów w `Form`, `QueryString`, `Cookies`, i `ServerVariables` kolekcji. W poniższych przykładach pokazano sposób użycia `Unvalidated` metody:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Znane problemy dotyczące wzorca ASP.NET Web Pages o składni Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: Nieoczekiwane zachowanie w przypadku używania tabeli użytkownika niestandardowego dla członkostwa

> Aby zainicjować dostawcy członkostwa ASP.NET Razor, witryny sieci Web, należy wywołać `WebSecurity.InitializeDatabaseConnection` metody. (W programie WebMatrix, w szablonie witryny początkowej zawiera wywołanie tej metody w  *\_AppStart.cshtml* pliku.) Jeśli `autoCreateTables` parametr tej metody jest ustawiona na wartość true (domyślnie jest ustawiona wartość true w szablonie witryny początkowej), i jeśli nazwa tabeli nierozpoznany jest przekazywany do metody (drugi parametr), metoda zgłasza błąd. Zamiast tego automatycznie tworzy tabelę.
> 
> Może to być problemem, jeśli zamierzasz używać tabeli użytkownika niestandardowego dla członkostwa, ale nazwy tabeli problem, aby przekazać `WebSecurity.InitializeDatabaseConnection` metody. Ponieważ metoda nie domyślnie Zgłoś błąd, jeśli nie ma tabeli, które określisz, a zamiast tego tworzy nową tabelę, aplikacja może się pojawić działa. Jednak kod aplikacji, która opiera się na tabeli użytkownika niestandardowego (i w polach w nim) po pewnym czasie może zakończyć się nieoczekiwanym błędem.
> 
> **Obejście problemu**  
> Upewnij się, że nazwa jest przekazywany w `InitializeDatabaseConnection` metoda dopasowania profilu użytkownika tabeli w bazie danych członkostwa lub upewnij się, że `autoCreateTables` parametr ma wartość false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "nie można wygenerować wystąpienia użytkownika programu SQL Server" Błąd

> Aplikacja sieci Web programu WebMatrix korzysta z programu SQL Server Express jest zasilany z usług IIS 7.5 Windows 7 lub Windows Server 2008 R2, użytkownik może zostać wyświetlony komunikat o błędzie wskazujący, że programu SQL Server nie można pobrać ścieżki lokalnej aplikacji przez użytkownika w czasie wykonywania.
> 
> **Obejście** upewnij się, Windows działającą aplikację (zwykle Usługa sieciowa) kontu uprawnienia odczytu/zapisu dla folderów głównych aplikacji i jego podfolderach takich jak *aplikacji\_danych*. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problem: W programie Visual Studio, przestrzenie nazw zestawów niestandardowych (dll) nie są importowane automatycznie

> Jeśli używasz niestandardowych zestawów w projekcie w programie Visual Studio, przestrzenie nazw zadeklarowanych w te zestawy nie są importowane automatycznie w czasie projektowania. W rezultacie odwołania do typów niestandardowych mogą nie być rozpoznawane w czasie projektowania i są oznaczone jako nie został rozpoznany w programie Visual Studio ("wężyk"). Ten problem występuje tylko w czasie projektowania w programie Visual Studio; sama aplikacja działa prawidłowo.
> 
> **Obejście problemu**  
> Obejmują `using` — instrukcja (`imports` w języku Visual Basic), odwołuje się do jednostek, które nie są rozpoznawane w czasie projektowania.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Program Visual Studio IntelliSense oraz szablony projektów dostępne tylko we wzorcu ASP.NET MVC w wersji 3

> Instalowanie składnika ASP.NET Web Pages nie także zainstalować narzędzia dla programu Visual Studio takie jak IntelliSense oraz szablony projektów dla aplikacji ASP.NET Web Pages.
> 
> **Obejście** Aby użyć funkcji IntelliSense oraz szablony projektów dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstaluj program ASP.NET MVC 3 RC za pomocą Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problem: "&lt;Pomocnika&gt; nie można odnaleźć klasy" Błąd

> Po uaktualnieniu do wersji Beta 3, zostanie wyświetlony błąd, klasa pomocy (na przykład `Facebook` klasy) nie można odnaleźć. Począwszy od wersji Beta 2 i kontynuowanie w wersji Beta 3, pomocników zostały przeniesione do pakietów, które jawnie należy zainstalować. Istniejące witryny nie są uaktualniane do uwzględnienia tych pakietów; obejmuje to lokacje w *\My Documents\IISExpress* lub *\My Documents\My witryn sieci Web* folderów. W szczególności, zostanie wyświetlony ten błąd, jeśli używasz witryny domyślnej w *obszarze Moje witryny* (witryna "website1"), który zawiera odwołanie do `Twitter` pomocnika.
> 
> **Obejście problemu**  
> Komentarz wywołania wszystkie obiekty pomocnicze w danej lokacji, uruchom  *\_administratora* strony, a następnie zainstaluj pakiet lub pakiety zawierające wątków, które chcesz użyć. Po zainstalowaniu pakietu usuniesz komentarz wiersze, które odwołują się pomocników.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problem: Wdrażanie zestawów w wersji Beta 3 ASP.NET Razor do folderu Bin może nie działać na hostowanie witryn

> W przypadku wdrożenia w witrynie sieci Web ASP.NET Web Pages do hostingu witryny i wdrażanie zestawów ASP.NET Razor w wersji Beta 3 w witrynie *Bin* folderu, mogą wystąpić błędy, w tym następujące:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Może to nastąpić, jeśli dostawca hostingu zainstalował zestawów ASP.NET Web Pages Beta 1 w pamięci podręcznej globalnej aplikacji serwera (GAC). Zestawy w pamięci podręcznej GAC przeprowadzanie pierwszeństwo przy użyciu zestawów zainstalowanych lokalnie w *Bin* folderu.
> 
> **Obejście** skontaktuj się z dostawcą hostingu, aby upewnić się, czy błędy są wyświetlane ze względu na konflikt między wersjami dostawcy zestawów i należy do Ciebie. Jeśli tak, żądania, że dostawca hostingu zaktualizować zestawów w pamięci podręcznej GAC serwera.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Odczytu źródła danych lub innych zewnętrznych danych za pośrednictwem serwera proxy

> Jeśli serwer z systemem lokacji znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacje o serwerze proxy w *Web.config* pliku, aby można było odczytać informacje, które pochodzą z spoza witryny. Na przykład, jeśli używasz `ReCaptcha` pomocnika pomocnika komunikuje się z usługą reCAPTCHA, ale może być blokowana przez serwer proxy. Podobnie źródła danych, które są używane w składniku ASP.NET Web Pages, takie jak źródło danych używane przez Menedżera pakietów może wymagać konfiguracji serwera proxy.
> 
> Jeśli wystąpią problemy w pracy z zewnętrznej usługi lub nie działa z pakietem kanału informacyjnego, umieść następujące elementy katalogu głównego aplikacji *Web.config* pliku:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Aby uzyskać więcej informacji na temat konfigurowania serwera proxy, zobacz [ &lt;proxy&gt; — Element (ustawienia sieci)](https://msdn.microsoft.com/library/sa91de1e.aspx) w witrynie MSDN w sieci Web.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problem: Błąd "Nie można załadować Microsoft.Web.Infrastructure.dll"

> Jeśli wcześniej zainstalowana wersja Beta 1 programu ASP.NET Web Pages o składni Razor, a następnie zainstalowanie wersji Beta 3 wszystkich odpowiednich zestawów są zainstalowane w globalnej pamięci podręcznej zestawów, z wyjątkiem *Microsoft.Web.Infrastructure.dll*. W konsekwencji podczas uruchamiania ASP.NET Razor strony zostanie wyświetlony błąd, który wskazuje, że *Microsoft.Web.Infrastructure.dll* nie można go załadować.
> 
> Ten problem nie występuje po załadowaniu wersji Beta 3 na oczyszczonym komputerze.
> 
> **Obejście problemu**  
> W Panelu sterowania odinstaluj stron ASP.NET Web Pages. Następnie ponownie zainstalować wersji Beta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Odinstalowywanie programu .NET Framework w wersji 4 wyłącza stron ASP.NET Web Pages o składni Razor

> Po odinstalowaniu programu .NET Framework w wersji 4 i zainstaluj go ponownie, ASP.NET Web Pages o składni Razor jest wyłączona. Strony z *.cshtml* rozszerzenia nie działać poprawnie. ASP.NET Web Pages rejestruje zestaw w katalogu głównym maszyny *Web.config* plików i usunięcie programu .NET Framework spowoduje usunięcie tego pliku. Ponowne zainstalowanie programu .NET Framework instaluje nową wersję pliku konfiguracji, ale nie powoduje dodania odwołania dla zestawu stron sieci Web platformy ASP.NET.
> 
> **Obejście** po ponownym zainstalowaniu programu .NET Framework, należy ponownie zainstalować składnika ASP.NET Web Pages o składni Razor. Spowoduje to dodanie następującego elementu do *Web.config* plik w folderze głównym komputera, który zazwyczaj znajduje się w następującej lokalizacji:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problem: Aplikacje, które wcześniej wdrożona za pomocą zestawów platformy ASP.NET z folderu Bin występują błędy

> Podczas wdrażania, kopie zestawów stron ASP.NET Web Pages (na przykład *Microsoft.WebPages.dll*) do *Bin* folderu witryny sieci Web na serwerze. (To może być spowodowane tym automatycznie podczas wdrażania lub ponieważ Deweloper jawnie skopiowany zestawów.) Jednak po zainstalowaniu wersji Beta 3, błędy występuje, takie jak błędy, których nie można znaleźć niektórych typów. Dzieje się tak, ponieważ wiele typów stron sieci Web platformy ASP.NET zostały przeniesione do innej przestrzeni nazw w wersji Beta 3.
> 
> **Obejście problemu**   
> Wyczyść *Bin* folderu wdrożonej aplikacji, skopiuj nowych zestawów do folderu (lub ponownego wdrażania aplikacji), a następnie ponownie uruchom aplikację.


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
> - Jeśli nie masz kontrolę nad komputerem serwera (na przykład wdrażasz do hostingu witryny sieci Web), Dodaj następujący kod do witryny sieci Web *Web.config* pliku:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problem: Przy użyciu stron ASP.NET MVC i ASP.NET Web lub projekt aplikacji sieci Web w tej samej aplikacji

> Jeśli używano stron ASP.NET Web Pages w projekcie aplikacji sieci Web lub w aplikacji ASP.NET MVC, zostanie wyświetlony błąd, *WebPageHttpApplication* nie można odnaleźć.
> 
> **Obejście problemu**  
> Jeśli ten błąd, Zmień klasę bazową, z którego pochodzi aplikacja. W *Global.asax* plików, zmień następujący wiersz:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Następujące zmiany:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Ta opcja powoduje obowiązywać zmiany, która została wprowadzona w wersji Beta 1 programu ASP.NET Web Pages ze składnią Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Wdrażanie aplikacji na komputerze, który nie ma zainstalowany program SQL Server Compact

> Aplikacje, które zawierają bazy danych programu SQL Server Compact mogą być uruchamiane na komputerze, na którym programu SQL Server Compact nie jest zainstalowany. Microsoft WebMatrix w wersji Beta 3 automatycznie kopiuje te pliki binarne dla Ciebie i wykonuje odpowiednie *Web.config* pliku przekształcenia.
> 
> **Obejście** Jeśli potrzebujesz kopiowania tych plików i *Web.config* zmian w plikach ręcznie, wykonaj następujące czynności:
> 
> 1. Kopiowanie zestawów aparatu bazy danych, które mają *Bin* folderze (i jego podfolderach) w aplikacji na komputerze docelowym: 
> 
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*
>     - Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **do** *\Bin\x86*
>     - Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **do** *\Bin\amd64*
> 2. W folderze głównym witryny sieci Web, należy utworzyć lub otworzyć *Web.config* pliku. (W programie WebMatrix w wersji Beta 3 jest dostępna po kliknięciu tego typu pliku **wszystkich** w **wybierz typ pliku** okno dialogowe.)
> 3. Dodaj następujący element jako element podrzędny elementu **&lt;konfiguracji&gt;** — element (poza **&lt;system.web&gt;** elementu):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: Bazy danych i WebGrid pomocników nie działają w trybie średniego zaufania w języku Visual Basic

> Jeśli używasz języka Visual Basic (Tworzenie *.vbhtml* plików), `Database` i `WebGrid` pomocników nie będzie działać, jeśli aplikacja jest skonfigurowana do użycia w trybie średniego zaufania.
> 
> **Obejście problemu**  
> Tymczasowe ustawienie aplikacji można używać pełnego zaufania.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problem: Właściwość "Szyfruj" nie został rozpoznany

> SQL Server Compact 4.0 nie rozpoznaje `Encrypt` właściwość `SqlCeConnection` klasy. Nie należy używać tej właściwości do szyfrowania plików bazy danych. `Encrypt` Właściwość została zakończona w SQL Server Compact 3.5 wersji i została zachowana tylko w przypadku zgodności z poprzednimi wersjami. 
> 
> **Obejście problemu**  
> Użyj `Encryption Mode` właściwość `SqlCeConnection` klasy do szyfrowania plików bazy danych programu SQL Server Compact 4.0. Poniższy przykład pokazuje, jak utworzyć zaszyfrowane programu SQL Server Compact 4.0 bazy danych przy użyciu `Encryption Mode` właściwości:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Aby zmienić tryb szyfrowania do istniejącej bazy danych programu SQL Server Compact 4.0, wykonaj następujące czynności:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Aby zaszyfrować niezaszyfrowane bazy danych programu SQL Server Compact 4.0, wykonaj następujące czynności:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problem: Bibliotek środowiska uruchomieniowego programu Microsoft Visual C++ 2008 są wymagane

> Natywnych bibliotek DLL programu SQL Server Compact 4.0 muszą programu Microsoft Visual C++ 2008 bibliotek środowiska uruchomieniowego (x 86, IA64 i x 64), z dodatkiem Service Pack 1.
> 
> **Obejście problemu**  
> Zainstaluj program .NET Framework 3.5 SP1. Spowoduje to również zainstalowanie Visual C++ 2008 środowiska uruchomieniowego bibliotek z dodatkiem SP1. Biblioteki można pobrać z następującej lokalizacji:   
>   
> [Aktualizacja zabezpieczeń biblioteki ATL do programu Microsoft Visual C++ 2008 z dodatkiem Service Pack 1 pakietu redystrybucyjnego](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Należy pamiętać, instalowanie programu .NET Framework 2.0, 3.0, lub nie 4 *nie* Zainstaluj Visual C++ 2008 środowiska uruchomieniowego bibliotek z dodatkiem SP1.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problem: Jeśli program SQL Server Compact jest zainstalowany przed zainstalowaniem programu .NET Framework na komputerze, jej nazwa niezmienna dostawcy nie jest zarejestrowany w pliku machine.config .NET Framework

> SQL Server Compact można zainstalować na komputerze, na którym nie zainstalowano programu .NET Framework zainstalowana, ponieważ program SQL Server Compact wymagają programu .NET framework. Jeśli zainstalowano ani .NET Framework w wersji 3.5 i 4 przed zainstalowaniem programu SQL Server Compact, konfiguracji programu SQL Server Compact nie rejestruje jej nazwa niezmienna dostawcy w *machine.config* pliku. Każda aplikacja, która korzysta z programu SQL Server Compact wpisu w *machine.config* pliku zakończy się niepowodzeniem. Wpis rejestracji niezmiennej nazwie w *machine.config* wygląda następująco:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Obejście problemu**  
> Odinstaluj program SQL Server Compact 4.0 CTP1. Pobierz i zainstaluj pełne wersje programu .NET Framework z następującej lokalizacji:
> 
> [Microsoft .NET Framework 3.5 z dodatkiem Service pack 1 (pełny pakiet)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4.0 wersji (pełny pakiet)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Następnie zainstaluj ponownie [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalowanie aplikacji

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: Instalowanie aplikacji może zająć dużo czasu, jeśli folder Moje dokumenty użytkownika jest przekierowywany do udziału sieciowego

> **Obejście problemu**  
> Brak. Aplikacja może potrwać chwilę, aby zainstalować, ale zostanie zainstalowany poprawnie.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publikowanie aplikacji

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Inne problemy

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problem: Filtr wyszukiwania/nie działa w raportach Group by: typ problemu

> Po uruchomieniu raportu dla lokacji, po wprowadzeniu tekstu w *Filtruj według adresu URL* pole, a następnie kliknij przycisk *wyszukiwania*, nic się nie dzieje. Jest to spowodowane ten formant nie działa podczas *Group By* stan raportu jest ustawiony na *typ problemu*, co jest ustawieniem domyślnym.
> 
> **Obejście** w *Group By* karty na wstążce kliknij *adresu URL* do grupowania wpisy według ich źródłowy adres URL. Pole tekstowe i przycisk do filtrowania pozycji działają podczas w tym stanie.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problem: Aplikacji WCF uruchomienie nie powiedzie się z programem IIS Express

> Przechodzenie do aplikacji WCF powoduje błąd podobny do następującego:
> 
> *Nie można załadować pliku lub zestawu ' Microsoft.Web.Administration, wersja = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 "lub jednej z jego zależności. System nie może odnaleźć określonego pliku.*
> 
> Dzieje się tak, ponieważ wersja usług IIS Express w wersji Beta nie obsługuje WCF domyślnie.
> 
> **Obejście** użyj jednej z poniższych obejść (obejście #2 wymaga systemu Microsoft Windows Vista lub nowszy):
> 
> 
> 1. Kopiuj *Microsoft.Web.dll* i *biblioteki Microsoft.Web.Administration.dll* zestawów z lokalizacji instalacji programu WebMatrix w celu *bin* katalogu programu WCF aplikacja. Domyślnie program WebMatrix jest zainstalowany w *Microsoft WebMatrix* podfolderu w folderze systemu *Program Files* folderu.
> 2. W systemie Microsoft Windows Vista lub nowszym, Utwórz Link symboliczny do zestawów w *bin* katalogu przy użyciu następujących poleceń. (To podejście ma tę zaletę, że nie tworzy kopii zestawów).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Dwa zestawy należy zainstalować w GAC. W wierszu polecenia z podwyższonym poziomem uprawnień uruchom następujące polecenia:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: Program WebMatrix w wersji Beta 3 nie jest w stanie wykonywanie określonych zadań, które wymagają podniesionych uprawnień

> Program WebMatrix w wersji Beta 3 jest w stanie wykonać niektóre zadania, które wymagają podniesionych uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:
> 
> - W Windows Vista lub Windows 7 zalogowano się za pomocą konta, które nie ma uprawnienia administracyjne, a Kontrola konta użytkownika (UAC) jest wyłączona.
> - Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.
> 
> **Obejście problemu**  
> Większość zadań w programie WebMatrix w wersji Beta 3 nie wymagają uprawnień administracyjnych. Dla osób, które wykonują można wykonać operacji, ponieważ administrator lub wykonaj następujące kroki:
> 
> - W Windows Vista lub Windows 7 włączenie funkcji Kontrola konta użytkownika.
> - Windows XP należy dodać użytkownika do grupy zabezpieczeń Administratorzy.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Witryny z internetowej galerii" jest wyłączona.

> **Witryny sieci Web galerii** opcja jest wyłączona, jeśli nie zainstalowano 3.0 Instalatora platformy sieci Web.
> 
> **Obejście problemu**  
> Zainstaluj [Instalatora platformy sieci Web firmy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problem: W systemie Windows Server 2003, usługi IIS Express nie rozpoczyna się dla użytkownika niebędącego administratorem

> W systemie Windows Server 2003 podczas uruchamiania stronę lub uruchom usługi IIS Express, usługi IIS Express nie można uruchomić. Dla stron sieci Web zostanie wyświetlony błąd wskazujący, że aplikacja została uruchomiona przez użytkownika innych niż administracyjne.
> 
> **Obejście problemu**  
> Uruchom program WebMatrix w wersji Beta 3 jako użytkownik z uprawnieniami administracyjnymi. Aby uzyskać więcej informacji zobacz następujący artykuł bazy wiedzy:  
>   
> [Aplikacja, która jest uruchamiana przez użytkowników innych niż administracyjne nie może nasłuchiwać na ruch HTTP, komputera, na którym aplikacja jest uruchomiona w Windows Vista, Windows Server 2003 lub Windows XP.](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>Problem: Przycisk "Relacji" jest wyłączony.

> **Relacje** przycisku w obszarze **tabeli** karcie **baz danych** obszar roboczy jest wyłączona w przypadku baz danych programu SQL Server Compact.
> 
> **Obejście problemu**  
> Brak. SQL Server Compact nie obsługuje relacje między tabelami.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problem: Zapytania SQL — sparametryzowane zgłaszanie wyjątków

> W SQL Server Compact 4.0, jeśli nie określisz typ danych takich jak `SqlDbType` lub `DbType` parametrów w zapytaniach parametrycznych, zgłaszany jest wyjątek podczas wykonywania kwerendy.
> 
> **Obejście problemu**  
> Jawnie ustawić typ danych dla parametrów, takich jak `SqlDbType` lub `DbType`. Jest to istotne w przypadku typów danych obiektów BLOB (`image` i `ntext`). Należy użyć kodu, jak pokazano poniżej:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Aby uzyskać więcej informacji

Aby uzyskać więcej informacji na temat programu WebMatrix w wersji Beta 3 zobacz następujące witryny sieci Web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Wszelkie prawa zastrzeżone. [Warunki użytkowania](https://msdn.microsoft.cos/cc300389.aspx).
