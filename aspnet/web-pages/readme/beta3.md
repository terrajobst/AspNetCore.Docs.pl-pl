---
uid: web-pages/readme/beta3
title: Sieci Web macierzy i plik Readme programu ASP.NET w wersji Beta 3 stron sieci Web (Razor) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Macierzy sieci Web i plik Readme sieci Web ASP.NET w wersji Beta 3 stron (Razor)
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5fad4b659dafe5470aeb84d320ff711b8840d1e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Macierzy sieci Web i plik Readme sieci Web ASP.NET w wersji Beta 3 stron (Razor)
====================
> Macierzy sieci Web i plik Readme sieci Web ASP.NET w wersji Beta 3 stron (Razor)

9 listopada 2010

## <a name="contents"></a>Spis treści

- [Omówienie](#Overview)
- [Instalacja](#Installation_Notes)
- [Nowe funkcje, zmiany i znane problemy w wersji Beta 3](#Known_Issues)

    - [Problemy z instalacją programu WebMatrix](#Known_Issues_Installation)
    - [Strony sieci Web ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalowanie aplikacji](#Known_Issues_Installing_Applications)
    - [Publikowanie aplikacji](#Known_Issues_Publishing_Applications)
    - [Inne problemy](#Known_Issues_Other_Issues)
- [Aby uzyskać więcej informacji](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Omówienie

> Microsoft WebMatrix Beta jest stos programowanie wolnej sieci web, który instaluje w minutach. Serwer sieci web jest zintegrowany z bazy danych i programowania platformy do utworzenia jednym zintegrowanym interfejsie. Program WebMatrix w wersji Beta umożliwia upraszcza sposób kodu, testowania i publikowanie własnych sieci Web ASP.NET i PHP, lub można użyć programu WebMatrix w wersji Beta można uruchomić nowej witryny sieci Web przy użyciu popularnych aplikacji open source, takich jak DotNetNuke, Umbraco, WordPress lub Joomla. Program WebMatrix w wersji Beta używa tego samego zaawansowanego serwera, aparatu bazy danych i środowiska struktur, które zostanie uruchomione witryny sieci Web w Internecie, dzięki czemu przejście od projektowania do produkcji płynne i bezproblemowe.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalacja

> Aby zainstalować program WebMatrix w wersji Beta 3, należy użyć [3.0 Instalatora platformy sieci Web Microsoft](https://go.microsoft.com/fwlink/?LinkID=194638). Po zainstalowaniu Instalatora platformy sieci Web, można ją zainstalować program WebMatrix w wersji Beta 3.
> 
> Jeśli masz problemy podczas instalacji, zapoznaj się [Rozwiązywanie problemów dotyczących Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instrukcje dotyczące publikowania aplikacji

> Zobacz [instrukcje krok po kroku dotyczące publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nowe funkcje i zmiany, problemy andKnown

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalacja wersji Beta programu WebMatrix 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: Program WebMatrix w wersji Beta 3 jest dostępna tylko na platformach, które obsługuje program Microsoft .NET Framework 4

> .NET Framework w wersji 4 jest wymagany dla programu WebMatrix w wersji Beta. W niektórych przypadkach Instalator programu WebMatrix w wersji Beta umożliwią spróbuje zainstalować na platformie, która nie wchodzi w skład zestawu obsługiwanej konfiguracji. W szczególności Windows Vista bez dodatku SP1 dla aktualizacji będzie można rozpocząć instalację programu WebMatrix w wersji Beta, ale składników .NET Framework 4 spowoduje niepowodzenie i zablokowanie instalacji.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: Nie można zainstalować program WebMatrix w wersji Beta 3, jeśli zainstalowano program Microsoft Visual Studio 2008 bez programu Microsoft Visual Studio 2008 z dodatkiem SP1

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

W tej sekcji dokumentu opisano nowe funkcje, zmiany i znane problemy z wersji Beta 3 programu ASP.NET Web Pages o składni Razor.

- [Nowe funkcje](#NewFeatures)
- [Zmiany](#Changes)
- [Problemy](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nowe funkcje w wersji Beta 3 dla stron ASP.NET Web Pages o składni Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nowy: Metoda "Html.Raw" renderuje niekodowany kod znaczników

> Nowe `Html.Raw` metoda umożliwia renderowania kodu znaczników HTML jako kod znaczników zamiast renderowania zakodowanego wyjścia. (Domyślnie ASP.NET Razor koduje ciągi przed ich renderowaniem.) Składnia jest następująca:
> 
> `Html.Raw(value)`
> 
> Poniższy przykład przedstawia użycie `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Zmiany w wersji Beta 3 dla stron ASP.NET Web Pages o składni Razor

#### <a name="change-hrefattribute-method-removed"></a>Zmień: Metoda "HrefAttribute" usunięte

> `HrefAttribute` Metody `WebPage` klasy został usunięty. Tego pomocnika użytego do kodowania znaków niebezpieczny w adresach URL. Nie jest już wymagane, ponieważ ASP.NET Razor automatycznie koduje ciągi. (Nowe `Html.Raw` metody do renderowania niekodowany ciągów.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Zmiany: Składnia deklaratywne "@helper" pomocników zmienione

> W wersji Beta 3 programu ASP.NET zmienia sposób analizuje wątków, które są tworzone przy użyciu `@helper` składni. W zasadzie `@helper` składni teraz być analizowana jako blok kodu a nie jako kod znaczników, który może zawierać kod w bloku. W związku z tym kodu wewnątrz elementu pomocniczego nie musi być ujęta w `@{ }` bloków. Z drugiej strony, znaczników wewnątrz elementu pomocniczego musi być jawnie uwzględnione w elementów HTML lub ASP.NET Razor `<text></text>` tagów.
> 
> Na przykład następująca `@helper` składni działa w wersji Beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> W wersji Beta 3 tego pomocnika należy zmienić, aby wyglądały jak w poniższym przykładzie:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Zwróć uwagę, że `@{ }` znaków wokół początkowej kodu pomocnika nie jest już używana. Jest to spowodowane zawartość pomocników są traktowane jako blok kodu domyślnie. Pomocnik renderuje kod znaczników, który rozpoczyna się od otwarcia `<a>` tagu. Jeśli pomocnika musi renderowania zwykłego tekstu lub tagi, które nie ma tagu zamykającego (na przykład `<meta>` tagów), musi należeć do renderowania zawartości `<text></text>` tagów.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Zmiany: Usunięte "WebPageContext.HttpContext"

> `WebPageContext.HttpContext` Właściwość została usunięta. Zamiast nich należy używać słów kluczowych `HttpContext.Current`. ( `WebPageContext.HttpContext` Właściwości po prostu to opakowana.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Zmień: "Facebook" pomocnika przeniesiona do nowego pakietu

> `Facebook` Pomocnika został przeniesiony do *Facebook.Helper* biblioteki, która obejmuje `Facebook` pomocnika i dodatkowe funkcje. Należy zainstalować tę bibliotekę jako osobny pakiet, zgodnie z opisem w "Instalowanie pomocników Menedżera pakietów" samouczka [wprowadzenie stron ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Zmień: Typy członkostwo roli i zabezpieczeń przenosi do nowego zestawu

> Następujące typy zostały przeniesione do `WebMatrix.WebData` zestawu:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Zmiany: Przeniesione do zestawu System.Web.WebPages.dll klasy "TagBuilder"

> `TagBuilde` Klasy r został przeniesiony do zestawu System.Web.WebPages.dll. Wcześniej to w zestawie, do którego jest częścią programu ASP.NET MVC. Ta zmiana oznacza, że jest konieczne instalowanie platformy ASP.NET MVC, aby można było używać `TagBuilder` klasy.
> 
> Klasa jest jednak nadal `System.Web.Mvc` przestrzeni nazw. Aby można było używać `TagBuilder` klasy (na przykład w niestandardowych ASP.NET Razor pomocnika), odwołanie do przestrzeni nazw (na przykład, dodając `@using System.Web.Mvc` kodu).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Zmiany: Składnia sprawdzania poprawności zostały zmienione; żądanie Klasa "Weryfikacja" usunięte

> W wersji Beta 3, aby wyłączyć sprawdzanie poprawności dla poszczególnych pól lub zestaw pól, można wywołać `Validation.Exclude` metody, przekazując nazwy pola do wykluczenia z weryfikacji. Nowej składni jest dostępna w wersji Beta 3 dla pomijanie sprawdzania poprawności. `Validation` Metodę używaną w wersji Beta 3 został usunięty.
> 
> > [!NOTE]
> > Jeśli nie można wyłączyć sprawdzania poprawności żądania, jeśli użytkownicy spróbuj przekazać kod znaczników HTML (na przykład za pomocą edytora tekstu sformatowanego, na stronie), witryna sieci Web wyświetli komunikat o błędzie, takich jak *wykryto potencjalnie niebezpieczną wartość Request.Form z klienta*i dane wejściowe użytkownika nie zostanie zaakceptowany. Jeśli wyłączysz weryfikację żądań, należy ręcznie sprawdzić danych wejściowych użytkownika, aby upewnić się, że nie zawierają potencjalnie niebezpiecznego kodu znaczników lub skrypt, za pomocą wyglądać mniej więcej tak [skryptów V4.0 biblioteki Microsoft przed granic lokacji](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Aby wyłączyć automatyczne żądania weryfikacji, należy wywołać `Request.Unvalidated` metody przekazanie jej nazwę pola lub innego obiektu post, który chcesz pominąć weryfikacji żądania. Ta metoda służy do pominięcia weryfikacji dla wszystkich elementów w `Form`, `QueryString`, `Cookies`, i `ServerVariables` kolekcji. W poniższych przykładach pokazano, jak używać `Unvalidated` metody:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Znane problemy dotyczące stron ASP.NET Web Pages o składni Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: Nieoczekiwanego zachowania w przypadku używania tabeli użytkownika niestandardowego dla członkostwa

> Aby zainicjować dostawcy członkostwa dla witryny sieci Web platformy ASP.NET Razor, należy wywołać `WebSecurity.InitializeDatabaseConnection` metody. (W programie WebMatrix, w szablonie witryny początkowej zawiera wywołanie tej metody w  *\_AppStart.cshtml* pliku.) Jeśli `autoCreateTables` parametr tej metody jest ustawiony na wartość true (domyślnie jest ustawiona wartość true w szablonie witryny początkowej), i jeśli nazwa tabeli nierozpoznany jest przekazywany do metody (drugi parametr), metoda nie zgłasza błąd. Zamiast tego automatycznie tworzy tabeli.
> 
> Może to być spowodowane problemem, jeśli zamierzasz używać tabeli użytkownika niestandardowego dla członkostwa, ale przekazywania do nazwy tabeli niewłaściwy `WebSecurity.InitializeDatabaseConnection` metody. Ponieważ metoda nie domyślnie Zgłoś błąd, jeśli tabela, które określisz nie istnieje, a zamiast tego tworzy nową tabelę, aplikacja może się pojawić się działać. Jednak kod aplikacji, która opiera się na tabeli użytkownika niestandardowego (i pól w nim) po pewnym czasie mogą nie być z nieoczekiwane błędy.
> 
> **Obejście problemu**  
> Upewnij się, że nazwa przekazano `InitializeDatabaseConnection` zgodna metoda profilu użytkownika tabeli w bazie danych członkostwa lub upewnij się, że `autoCreateTables` parametr ma wartość false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Błąd "Nie można wygenerować wystąpienia użytkownika programu SQL Server" problemu:

> Jeśli aplikacja sieci Web programu WebMatrix korzysta z programu SQL Server Express i działa IIS 7.5 w systemie Windows 7 lub Windows Server 2008 R2, można napotkać komunikat o błędzie wskazujący, że programu SQL Server nie można pobrać ścieżki lokalnej aplikacji użytkownika w czasie wykonywania.
> 
> **Obejście** upewnij się, czy konto systemu Windows działającą aplikację (zazwyczaj Usługa sieciowa) ma uprawnienia odczytu/zapisu dla folderów głównych aplikacji i podfoldery takich jak *aplikacji\_danych*. Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemów z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problem: W programie Visual Studio, przestrzeni nazw dla zestawów niestandardowych (dll) nie są importowane automatycznie

> Jeśli używasz zestawów niestandardowych w projekcie w programie Visual Studio, przestrzeni nazw, zadeklarowanej w tych zestawów nie są automatycznie importowane w czasie projektowania. W związku z tym odwołania do typów niestandardowych mogą nie być rozpoznawane w czasie projektowania i są oznaczone jako nie został rozpoznany. w programie Visual Studio (przy użyciu "wężyk"). Ten problem występuje tylko w czasie projektowania w programie Visual Studio; sama aplikacja działa prawidłowo.
> 
> **Obejście problemu**  
> Obejmują `using` instrukcji (`imports` w języku Visual Basic), która odwołuje się jednostek, które nie są rozpoznawane w czasie projektowania.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Visual Studio IntelliSense projektu szablonów i dostępna tylko na platformie ASP.NET MVC w wersji 3

> Instalowanie składnika ASP.NET Web Pages nie również zainstalować narzędzi dla programu Visual Studio takie jak szablony IntelliSense i projektu dla aplikacji ASP.NET Web Pages.
> 
> **Obejście** Aby używać szablonów IntelliSense i projektu dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstalować platformy ASP.NET MVC 3 RC albo za pomocą Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problem: "&lt;Pomocnika&gt; nie można odnaleźć klasy" Błąd

> Po uaktualnieniu do wersji Beta 3, zostanie wyświetlony błąd który klasę pomocy (na przykład `Facebook` klasy) nie można odnaleźć. Począwszy od wersji Beta 2 oraz kontynuowanie w wersji Beta 3, pomocników zostały przeniesione do pakietów, które jawnie należy zainstalować. Aby uwzględnić te pakiety; nie są uaktualnieni istniejących lokacji dotyczy to również lokacji w *\My Documents\IISExpress* lub *\My Documents\My witryn sieci Web* folderów. W szczególności zostanie wyświetlony ten błąd, użycie domyślnej witryny w *witryny* (WebSite1), który zawiera odwołanie do `Twitter` pomocnika.
> 
> **Obejście problemu**  
> Komentarz dla wywołań dowolnej pomocników w lokacji, uruchom  *\_Admin* strony i zainstaluj pakiet lub pakiety zawierające wątków, które chcesz użyć. Po zainstalowaniu pakietu można usuń znaczniki komentarza wierszy, które odwołują się pomocników.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problem: Wdrażanie zestawów w wersji Beta 3 ASP.NET Razor do folderu Bin może nie działać na witrynami

> W przypadku wdrożenia do hostowania lokacji witryny sieci Web platformy ASP.NET Web Pages i zestawy ASP.NET Razor w wersji Beta 3 w przypadku wdrożenia w witrynie *Bin* folderu, mogą wystąpić błędy, takie jak:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Może to nastąpić, jeśli dostawca hostingu zainstalował zestawy ASP.NET Web Pages Beta 1 w pamięci podręcznej globalnego aplikacji serwera (GAC). Zestawy w pamięci GAC uzyskać pierwszeństwo nad zestawów zainstalowanych lokalnie w *Bin* folderu.
> 
> **Obejście** skontaktuj się z dostawcą hostingu, aby potwierdzić, czy błędy pojawia się z powodu konfliktu między wersjami dostawcy zestawów i należy do Ciebie. Jeśli tak, żądanie, że dostawca hostingu zaktualizować zestawów w pamięci podręcznej GAC serwera.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Odczytywanie źródła danych lub innych danych zewnętrznych za pośrednictwem serwera proxy

> Jeśli serwer z systemem lokacji znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacje dotyczące serwera proxy w *Web.config* pliku, aby można było odczytać informacje, które pochodzą z poza witryną. Na przykład, jeśli używasz `ReCaptcha` pomocnika, pomocnika komunikuje się z usługą reCAPTCHA, ale może zostać zablokowany przez serwer proxy. Podobnie źródła danych, które są używane w programie ASP.NET Web Pages, np. źródła danych używane przez Menedżera pakietów, mogą wymagać konfiguracji serwera proxy.
> 
> Jeśli masz problemy w pracy z zewnętrznej usługi lub Praca z pakietem źródła danych należy umieścić następujące elementy w katalogu głównego aplikacji *Web.config* pliku:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Aby uzyskać więcej informacji na temat konfigurowania serwera proxy, zobacz [ &lt;proxy&gt; elementu (ustawienia sieciowe)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) w witrynie MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problemu: Błąd "Nie można załadować Microsoft.Web.Infrastructure.dll"

> Jeśli wcześniej zainstalowana wersja Beta 1 programu ASP.NET Web Pages o składni Razor, a następnie zainstalować wersję Beta 3, wszystkie odpowiednie zestawy są zainstalowane w GAC, z wyjątkiem *Microsoft.Web.Infrastructure.dll*. W rezultacie, po uruchomieniu ASP.NET Razor strony, zostanie wyświetlony błąd wskazuje, że *Microsoft.Web.Infrastructure.dll* nie można go załadować.
> 
> Ten problem nie występuje po załadowaniu w wersji Beta 3 na oczyszczonym komputerze.
> 
> **Obejście problemu**  
> W Panelu sterowania odinstaluj stron ASP.NET Web Pages. Zainstaluj ponownie w wersji Beta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Odinstalowywanie programu .NET Framework w wersji 4 wyłącza stron ASP.NET Web Pages o składni Razor

> Po odinstalowaniu programu .NET Framework w wersji 4 i zainstaluj go ponownie, ASP.NET Web Pages o składni Razor jest wyłączona. Strony z *.cshtml* rozszerzenia nie działać prawidłowo. Strony ASP.NET Web Pages rejestruje zestawu w folderze głównym maszyny *Web.config* plików i usunięcie programu .NET Framework spowoduje usunięcie tego pliku. Ponowna instalacja programu .NET Framework instaluje nową wersję pliku konfiguracji, ale nie dodać odwołanie do zestawu stron sieci Web programu ASP.NET.
> 
> **Obejście** po ponownym zainstalowaniu programu .NET Framework, zainstaluj ponownie stron ASP.NET Web Pages o składni Razor. Spowoduje to dodanie następujący element do *Web.config* w katalogu głównym komputera, który zazwyczaj znajduje się w następującej lokalizacji:  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
>   
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problem: Aplikacje wdrożone wcześniej z zestawami ASP.NET z folderu Bin występują błędy

> Podczas wdrażania, kopii stron ASP.NET Web Pages zestawów (na przykład *Microsoft.WebPages.dll*) do *Bin* folderu witryny sieci Web na serwerze. (To może się to zdarzyć automatycznie podczas wdrażania lub ponieważ dewelopera jawnie skopiowany zestawy.) Jednak po zainstalowaniu wersji Beta 3 błędy zachodzi, takich jak błędy, których nie można znaleźć niektórych typów. Dzieje się tak, ponieważ liczba stron ASP.NET Web Pages typy zostały przeniesione w różnych przestrzeniach nazw w wersji Beta 3.
> 
> **Obejście problemu**   
> Wyczyść *Bin* folderu wdrożonej aplikacji skopiuj nowy zestawów do folderu (lub ponownego wdrażania aplikacji), a następnie ponownie uruchom aplikację.


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
> - Jeśli nie masz kontrolę nad komputerem serwera (na przykład wdrażasz do obsługi witryny sieci Web), dodaj następującą wartość do witryny sieci Web *Web.config* pliku:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problem: Przy użyciu stron ASP.NET MVC i platformy ASP.NET w sieci Web lub projektu aplikacji sieci Web w tej samej aplikacji

> Jeśli były używane strony sieci Web ASP.NET w projekt aplikacji sieci Web lub aplikacji ASP.NET MVC, zostanie wyświetlony błąd który *WebPageHttpApplication* nie można odnaleźć.
> 
> **Obejście problemu**  
> Jeśli ten błąd, Zmień klasę podstawową, z którego pochodzi aplikacja. W *Global.asax* zmień następujący wiersz:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> następujące zmiany:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Ta opcja powoduje w celu zmiany, która została wprowadzona w wersji Beta 1 programu ASP.NET Web Pages o składni Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Wdrażanie aplikacji na komputerze, na którym nie ma zainstalowany program SQL Server Compact

> Aplikacje, które obejmują baz danych programu SQL Server Compact można uruchomić na komputerze, na którym program SQL Server Compact nie jest zainstalowany. Microsoft WebMatrix w wersji Beta 3 automatycznie kopiuje tych plików binarnych dla Ciebie i wykonuje odpowiednie *Web.config* pliku transformacji.
> 
> **Obejście** Jeśli musisz skopiować te pliki i upewnić *Web.config* zmiany pliku ręcznie, wykonaj następujące czynności:
> 
> 1. Kopiowanie zestawów aparatu bazy danych do *Bin* folderze (i jego podfolderach) aplikacji na komputerze docelowym: 
> 
>     - Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **do** *\Bin*
>     - Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **do** *\Bin\x86*
>     - Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **do** *\Bin\amd64*
> 2. W folderze głównym witryny sieci Web, należy utworzyć lub otworzyć *Web.config* pliku. (W programie WebMatrix w wersji Beta 3, ten typ pliku jest dostępna po kliknięciu **wszystkie** w **wybierz typ pliku** okno dialogowe.)
> 3. Dodaj następujący element jako element podrzędny  **&lt;konfiguracji&gt;**  elementu (nie znajduje się w  **&lt;system.web&gt;**  element):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: Bazy danych i WebGrid pomocników nie działają w trybie średniego zaufania w języku Visual Basic

> Jeśli używasz programu Visual Basic (Tworzenie *.vbhtml* plików), `Database` i `WebGrid` pomocników nie będzie działać, jeśli aplikacja jest skonfigurowana do użycia w trybie średniego zaufania.
> 
> **Obejście problemu**  
> Tymczasowo ustawić aplikacji do korzystania z pełnego zaufania.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problem: Właściwość "Szyfruj" nie został rozpoznany

> SQL Server Compact 4.0 nie rozpoznaje `Encrypt` właściwość `SqlCeConnection` klasy. Ta właściwość nie należy używać do szyfrowania plików bazy danych. `Encrypt` Właściwość została uznana za przestarzałą w programu SQL Server Compact 3.5 wersji i została zachowana tylko w przypadku zgodności z poprzednimi wersjami. 
> 
> **Obejście problemu**  
> Użyj `Encryption Mode` właściwość `SqlCeConnection` klasy do szyfrowania plików bazy danych programu SQL Server Compact 4.0. Poniższy przykład przedstawia sposób tworzenia zaszyfrowane programu SQL Server Compact 4.0 bazy danych przy użyciu `Encryption Mode` właściwości:
>  
> [!code-csharp[Main](beta3/samples/sample11.cs)]
>  
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Aby zmienić tryb szyfrowania z istniejącej bazy danych programu SQL Server Compact 4.0, wykonaj następujące czynności:
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

> Natywnych bibliotek DLL programu SQL Server Compact 4.0 wymagają programu Microsoft Visual C++ 2008 bibliotek środowiska uruchomieniowego (x 86, IA64 i x 64), z dodatkiem Service Pack 1.
> 
> **Obejście problemu**  
> Zainstaluj program .NET Framework 3.5 SP1. Spowoduje to również zainstalowanie Visual C++ 2008 środowiska uruchomieniowego bibliotek z dodatkiem SP1. Biblioteki można pobrać z następującej lokalizacji:   
>   
> [Aktualizacja zabezpieczeń biblioteki ATL do programu Microsoft Visual C++ 2008 z dodatkiem Service Pack 1 pakietu redystrybucyjnego](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Należy pamiętać, instalowanie programu .NET Framework 2.0, 3.0, lub nie 4 *nie* zainstalować Visual C++ 2008 środowiska uruchomieniowego bibliotek z dodatkiem SP1.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problem: Jeśli program SQL Server Compact jest zainstalowany przed zainstalowaniem programu .NET Framework na komputerze, niezmiennej nazwy dostawcy nie jest zarejestrowany w pliku machine.config .NET Framework

> SQL Server Compact można zainstalować na komputerze, na którym nie ma zainstalowany, ponieważ program SQL Server Compact wymagają programu .NET framework .NET Framework. Jeśli .NET Framework w wersji 3.5 ani 4 jest zainstalowany, przed zainstalowaniem programu SQL Server Compact, Instalator programu SQL Server Compact nie rejestruje jego niezmienna Nazwa dostawcy w *machine.config* pliku. Każda aplikacja, która korzysta z programu SQL Server Compact wpisu w *machine.config* pliku zakończy się niepowodzeniem. Niezmienna Nazwa wpisu rejestracji w *machine.config* wygląda jak w poniższym przykładzie:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Obejście problemu**  
> Odinstaluj program SQL Server Compact 4.0 CTP1. Pobierz i zainstaluj pełnej wersji programu .NET Framework z następującej lokalizacji:
> 
> [Microsoft .NET Framework 3.5 z dodatkiem Service pack 1 (pełny pakiet)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Program Microsoft .NET Framework 4.0 zlecenia (pełny pakiet)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Następnie zainstaluj ponownie [programu SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalowanie aplikacji

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: Instalowanie aplikacji może długo trwać, jeśli folder Moje dokumenty użytkownika jest przekierowywany do udziału sieciowego

> **Obejście problemu**  
> Brak. Aplikacja może potrwać kilka minut, aby zainstalować, ale zostanie zainstalowany poprawnie.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publikowanie aplikacji

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Inne problemy

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problem: Filtr/wyszukiwania nie działa w raportach Grupuj według: typ problemu

> Po uruchomieniu raportu dla lokacji, jeśli wprowadzony tekst w *Filtruj według adresu URL* polu i kliknij przycisk *wyszukiwania*, nic się nie dzieje. Wynika to z faktu ten formant nie działa podczas *Group By* ustawiony stan raportu *typu problemu*, co jest ustawieniem domyślnym.
> 
> **Obejście** w *Group By* kartę na wstążce kliknij *adres URL* do grupowania wpisy według ich źródłowy adres URL. Pole tekstowe i przycisk do filtrowania pozycji są działa w tym stanie.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problem: Aplikacji WCF nie udało się uruchomić z programem IIS Express

> Przeglądanie aplikacji WCF powoduje błąd podobny do następującego:
> 
> *Nie można załadować pliku lub zestawu ' Microsoft.Web.Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 "lub jednej z jego zależności. System nie może odnaleźć określonego pliku.*
> 
> Dzieje się tak, ponieważ usługi IIS Express w wersji beta nie obsługuje WCF domyślnie.
> 
> **Obejście** użyj jednej z poniższych rozwiązań (obejście #2 wymaga systemu Microsoft Windows Vista lub nowszy):
> 
> 
> 1. Kopiuj *Microsoft.Web.dll* i *biblioteki Microsoft.Web.Administration.dll* zestawy z lokalizacji instalacji programu WebMatrix w celu *bin* katalog usługi WCF aplikacja. Domyślnie program WebMatrix jest zainstalowany w *Microsoft WebMatrix* podfolderze systemu *Program Files* folderu.
> 2. W systemie Microsoft Windows Vista lub nowszego, należy utworzyć łącza symbolicznego do zestawów w *bin* katalogu przy użyciu następujących poleceń. (To rozwiązanie ma tę zaletę, że nie tworzy kopię zestawy).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Zainstalowanie dwóch zestawów w pamięci podręcznej GAC. W wierszu z podwyższonym poziomem uprawnień uruchom następujące polecenia:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: Program WebMatrix w wersji Beta 3 nie jest w stanie do wykonania niektórych zadań, które wymagają podniesionych uprawnień

> Program WebMatrix w wersji Beta 3 jest w stanie do wykonania niektórych zadań, które wymagają podniesionych uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:
> 
> - W systemie Windows Vista lub Windows 7 zalogowano się za pomocą konta, które nie ma uprawnień administracyjnych i kontroli konta użytkownika (UAC) jest wyłączona.
> - Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.
> 
> **Obejście problemu**  
> Większość zadań w programie WebMatrix w wersji Beta 3 nie wymaga uprawnień administracyjnych. Dla tych, które wykonują można wykonać operacji jako administrator lub wykonaj następujące kroki:
> 
> - W systemie Windows Vista lub Windows 7 należy włączyć funkcji Kontrola konta użytkownika.
> - W systemie Windows XP należy dodać użytkownika do grupy zabezpieczeń Administratorzy.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Lokacji z galerii" jest wyłączona.

> **Witryny sieci Web galerii** opcja jest wyłączona, jeśli Instalator platformy sieci Web 3.0 nie jest zainstalowany.
> 
> **Obejście problemu**  
> Zainstaluj [Instalatora platformy sieci Web firmy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problem: W systemie Windows Server 2003, usługi IIS Express nie uruchamia się dla użytkowników innych niż administracyjne

> W systemie Windows Server 2003 podczas uruchamiania stronę lub uruchom usługi IIS Express, usługi IIS Express nie można uruchomić. Dla stron sieci Web zostanie wyświetlony błąd wskazujący, że aplikacja została uruchomiona przez użytkownika niebędącego administratorem.
> 
> **Obejście problemu**  
> Uruchom program WebMatrix w wersji Beta 3 jako użytkownik z uprawnieniami administracyjnymi. Aby uzyskać więcej informacji zobacz następujący artykuł bazy wiedzy:  
>   
> [Aplikacja, która jest uruchomiona przez użytkownika niebędącego administratorem nie może nasłuchiwać na ruch HTTP komputera, na którym aplikacja jest uruchomiona w systemie Windows Vista, Windows Server 2003 lub Windows XP.](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>Problem: Przycisk "Relacji" jest wyłączony.

> **Relacje** przycisku w obszarze **tabeli** karcie **baz danych** obszaru roboczego jest wyłączone dla bazy danych programu SQL Server Compact.
> 
> **Obejście problemu**  
> Brak. SQL Server Compact nie obsługuje relacji między tabelami.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problem: Zapytania parametrycznego SQL zgłaszanie wyjątków

> W przypadku programu SQL Server Compact 4.0, jeśli nie określisz typu danych takich jak `SqlDbType` lub `DbType` dla parametrów w zapytaniach parametrycznych, jest zgłaszany wyjątek podczas wykonywania kwerendy.
> 
> **Obejście problemu**  
> Jawnie ustawić typ danych dla parametrów, takich jak `SqlDbType` lub `DbType`. Jest to szczególnie ważne w przypadku typów danych obiektów BLOB (`image` i `ntext`). Użyj kodu podobne do poniższych:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
>  
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Aby uzyskać więcej informacji

Aby uzyskać więcej informacji na temat programu WebMatrix w wersji Beta 3 zobacz następujące witryny sieci Web:

- [Program IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/Web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Wszelkie prawa zastrzeżone. [Warunki użytkowania](https://msdn.microsoft.com/en-us/cc300389.aspx).
