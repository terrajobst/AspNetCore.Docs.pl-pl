---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 przełomowych zmianach | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano zmiany, które zostały wprowadzone dla wersji .NET Framework w wersji 4, która może wpłynąć na aplikacje, które zostały utworzone przy użyciu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: bf9968c49e904c064338d7123405cb6578a915f2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378653"
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 fundamentalne zmiany
====================
> W tym dokumencie opisano zmiany, które zostały wprowadzone dla wersji .NET Framework w wersji 4, która może wpłynąć na aplikacje, które zostały utworzone za pomocą wcześniejszych wersji, w tym wersje platformy ASP.NET 4 Beta 1 i 2 w wersji Beta.
> 
> [Pobierz ten oficjalny dokument](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Spis treści

[Ustawienie ControlRenderingCompatibilityVersion w pliku Web.config](#0.1__Toc256770141 "_Toc256770141")  
[Zmiany ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode i teraz UrlEncode kodowanie pojedynczy cudzysłów](#0.1__Toc256770143 "_Toc256770143")  
[Strona ASP.NET (aspx) Analizator jest Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Zaktualizowane pliki definicji przeglądarki](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll usunięte z pliku konfiguracji sieci Web głównego](#0.1__Toc256770146 "_Toc256770146")  
[Weryfikacja żądania programu ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[Domyślne algorytmem wyznaczania wartości skrótu jest teraz HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Błędy konfiguracji związane z nowej konfiguracji programu ASP.NET 4 głównych](#0.1__Toc256770149 "_Toc256770149")  
[Aplikacje programu ASP.NET 4 podrzędnych zakończyć się niepowodzeniem do ekranu startowego w ramach programu ASP.NET 2.0 lub ASP.NET aplikacji w wersji 3.5](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 witryn sieci Web nie można uruchomić na komputerach, na którym zainstalowano program SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[The HttpRequest.FilePath Property No Longer Includes PathInfo Values](#0.1__Toc256770152 "_Toc256770152")  
[Program ASP.NET 2.0 aplikacji może generować httpexception — błędy, które odwołują się eurl.axd](#0.1__Toc256770153 "_Toc256770153")  
[Programy obsługi zdarzeń nie może zostać nie wywołane dokumentu domyślnego w IIS 7.5 lub IIS 7 trybie zintegrowanym](#0.1__Toc256770154 "_Toc256770154")  
[Zmienia się na implementacji zabezpieczenia dostępu kodu platformy ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[Zostały przeniesione MembershipUser a innymi typami danych w Namespace System.Web.Security](#0.1__Toc256770156 "_Toc256770156")  
[Dane wyjściowe różnią się w pamięci podręcznej zmiany \* nagłówka HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Typy System.Web.Security na potrzeby usługi Passport są przestarzałe](#0.1__Toc256770158 "_Toc256770158")  
[Właściwość MenuItem.PopOutImageUrl nie powiedzie się, by renderować obraz na platformie ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl i Menu.DynamicPopOutImageUrl nie będą renderowane obrazów w przypadku, gdy ścieżki zawierać ukośników odwrotnych](#0.1__Toc256770160 "_Toc256770160")  
[Zastrzeżenie](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Ustawienie ControlRenderingCompatibilityVersion w pliku Web.config

Kontrolki ASP.NET zostały zmodyfikowane w .NET Framework w wersji 4 w celu umożliwiają określenie, bardziej precyzyjne jak ich renderowania kodu znaczników. W poprzednich wersjach programu .NET Framework niektóre kontrolki emitowany kod znaczników, który ma możliwość wyłączenia. Domyślnie generowany jest już platformy ASP.NET 4 tego typu znaczników.

Jeśli używasz programu Visual Studio 2010 do uaktualnienia aplikacji ASP.NET w wersji 2.0 lub ASP.NET 3.5, narzędzie automatycznie dodaje ustawienie, aby `Web.config` pliku, który zachowuje renderowania starszej wersji. Jednak jeśli zaktualizujesz aplikację, zmieniając puli aplikacji w usługach IIS pod kątem programu .NET Framework 4, platformy ASP.NET używa domyślnie nowy tryb renderowania. Aby wyłączyć nowy tryb renderowania, Dodaj następujące ustawienie w `Web.config` pliku:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Zmiany głównej renderowania, które zapewnia nowe zachowanie są następujące:

- **Obraz** i **ImageButton** już renderowanie kontrolek `border="0"` atrybutu.
- **BaseValidator** klasy oraz sprawdzanie poprawności formantów, które od niej pochodzić renderowania czerwony tekst nie jest już domyślnie.
- **HtmlForm** kontrolki nie jest renderowana **nazwa** atrybutu.
- **Tabeli** kontrolować już renderuje `border="0"` atrybutu.
- Formanty, które nie są przeznaczone na dane wejściowe użytkownika (na przykład **etykiety** kontroli) nie jest już renderowania `disabled="disabled"` atrybutu, jeśli ich **włączone** właściwość jest ustawiona na **false**(lub jeśli dziedziczą one tego ustawienia z poziomu kontroli kontenera).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>Zmiany ClientIDMode

**ClientIDMode** ustawienie w ASP.NET 4 pozwala określić, jak program ASP.NET generuje **identyfikator** atrybutów elementów HTML. W poprzednich wersjach programu ASP.NET, zachowanie domyślne zostało odpowiednikiem **identyfikator automatyczny** ustawienie **ClientIDMode**. Ustawieniem domyślnym jest jednak teraz **przewidywalny**.

Jeśli używasz programu Visual Studio 2010 do uaktualnienia aplikacji ASP.NET w wersji 2.0 lub ASP.NET 3.5, narzędzie automatycznie dodaje ustawienie, aby `Web.config` pliku, który zachowuje zachowanie wcześniejszych wersji programu .NET Framework. Jednak jeśli zaktualizujesz aplikację, zmieniając puli aplikacji w usługach IIS pod kątem programu .NET Framework 4, platformy ASP.NET używa domyślnie nowy tryb. Aby wyłączyć nowy tryb Identyfikatora klienta, Dodaj następujące ustawienie w `Web.config` pliku:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode i teraz UrlEncode kodowanie znaki pojedynczego cudzysłowu

W technologii ASP.NET 4 **HtmlEncode** i **UrlEncode** metody **HttpUtility** i **HttpServerUtility** klasy zostały zaktualizowane do kodowanie znaków pojedynczego cudzysłowu (') w następujący sposób:

- **HtmlEncode** metoda koduje wystąpień pojedynczego cudzysłowu jako.
- **UrlEncode** metoda koduje wystąpień pojedynczego cudzysłowu jako % 27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Strona ASP.NET (aspx) Analizator jest Stricter

Parser strony dla stron ASP.NET (`.aspx` plików) i kontrolki użytkownika (`.ascx` plików) jest bardziej restrykcyjny w ASP.NET 4 i odrzuci większej liczby wystąpień nieprawidłowe znaczniki. Na przykład następujące dwa fragmenty kodu będzie pomyślnie przeanalizować we wcześniejszych wersjach programu ASP.NET, ale teraz będzie zgłaszać błędy parsera w ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Zwróć uwagę, nieprawidłowy średnikami, na końcu **formant HiddenField** tagu.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Zwróć uwagę, niezamknięty **styl** atrybut, który jest uruchamiany w **CssClass** atrybutu.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Zaktualizowane pliki definicji przeglądarki

Przeglądarka plików definicji zostały zaktualizowane do uwzględnienia informacji o nowych i zaktualizowanych przeglądarek i urządzeń. Starsze przeglądarki i urządzenia, takich jak Netscape Navigator zostały usunięte i nowszych przeglądarkach i urządzeniach, takich jak Google Chrome i Apple iPhone zostały dodane.

Jeśli aplikacja zawiera definicje niestandardowego przeglądarki, które dziedziczą z jednej z definicji przeglądarki, które zostały usunięte, zostanie wyświetlony błąd. Na przykład jeśli `App_Browsers` folder zawiera definicję przeglądarki, która dziedziczy z definicji przeglądarki IE2, zostanie wyświetlony następujący komunikat o błędzie konfiguracji:

- Nie można odnaleźć elementu przeglądarki lub bramy o identyfikatorze "IE2".

> [!NOTE]
> **HttpBrowserCapabilities** obiektu (który jest udostępniany przez strony **Request.Browser** właściwość) jest wymuszany przez pliki definicji przeglądarki. W związku z tym informacje zwrócone przez uzyskanie dostępu do właściwości tego obiektu w programie ASP.NET 4 może być inny niż informacje zwrócone we wcześniejszej wersji programu ASP.NET.


Możesz przywrócić stare pliki definicji przeglądarki, kopiując pliki definicji przeglądarki z następującego folderu:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Skopiuj pliki do odpowiednich `\CONFIG\Browsers` folder dla platformy ASP.NET 4. Po skopiowaniu plików Uruchom Aspnet\_regbrowsers.exe narzędzie wiersza polecenia.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll usunięte z pliku konfiguracji sieci Web katalogu głównego

W poprzednich wersjach programu ASP.NET, odwołanie do zestawu System.Web.Mobile.dll została uwzględniona w katalogu głównym `Web.config` w pliku **zestawy** sekcji. Aby zwiększyć wydajność, odwołanie do tego zestawu został usunięty.

Moduł System.Web.Mobile.dll jest dołączony do platformy ASP.NET 4, ale jest przestarzały. Jeśli chcesz, by używał typów z zestawu System.Web.Mobile.dll, Dodaj odwołanie do tego zestawu, albo w katalogu głównym `Web.config` plików lub aplikacji `Web.config` pliku. Na przykład, jeśli chcesz używać dowolnych formanty mobilne ASP.NET (przestarzałe) możesz należy dodać odwołanie do zestawu System.Web.Mobile.dll `Web.config` pliku.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Weryfikacja żądania programu ASP.NET

Funkcję weryfikacji żądania w programie ASP.NET zapewnia pewien stopień domyślną ochronę przed atakami skryptów między witrynami (XSS). W poprzednich wersjach programu ASP.NET Weryfikacja żądania został włączony domyślnie. Jednak stosowane wyłącznie do stron ASP.NET (`.aspx` plików i ich pliki klas) i tylko gdy wykonywały te strony.

ASP.NET 4, domyślnie Weryfikacja żądania jest włączona dla wszystkich żądań, ponieważ jest ona włączona przed **BeginRequest** faza żądania HTTP. W wyniku weryfikacji żądań ma zastosowanie do żądań dla wszystkich zasobów platformy ASP.NET, a nie tylko żądań strony .aspx. Dotyczy to żądań, takich jak wywołania usługi sieci Web i niestandardowe programy obsługi HTTP. Weryfikacja żądania jest aktywny również w przypadku, gdy niestandardowe moduły HTTP podczas odczytu treści żądania HTTP.

W rezultacie żądań, które wcześniej nie był przyczyną błędów teraz mogą wystąpić błędy sprawdzania poprawności żądania. Aby przywrócić zachowanie funkcji walidacji żądania programu ASP.NET 2.0, Dodaj następujące ustawienie w `Web.config` pliku:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Firma Microsoft zaleca jednak analizowania wszystkie błędy weryfikacji żądania, aby ustalić, czy istniejące programy obsługi, modułów lub innych niestandardowy kod uzyskuje dostęp do potencjalnie niebezpiecznych danych wejściowych HTTP, które mogłyby zostać XSS ataki wektorów.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Domyślne algorytmem wyznaczania wartości skrótu jest teraz HMACSHA256

Aplikacja ASP.NET używa szyfrowanie i algorytmy wyznaczania wartości skrótu, aby ułatwić zabezpieczanie danych, takich jak pliki cookie uwierzytelniania formularzy oraz stanu widoku. Domyślnie program ASP.NET 4 teraz używa algorytmu HMACSHA256 dla operacji mieszania na pliki cookie i stan widoku. Starsze wersje programu ASP.NET używały starsze algorytm HMACSHA1.

Aplikacje może to mieć wpływ po uruchomieniu mieszane 2.0/ASP.NET platformy ASP.NET 4 środowiska, w którym dane, takie jak pliki cookie uwierzytelniania formularzy musi działać across.NET Framework w wersji. Aby skonfigurować aplikację sieci Web programu ASP.NET 4, aby używać starszych algorytmu HMACSHA1, Dodaj następujące ustawienie w `Web.config` pliku:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Błędy konfiguracji związane z nowej konfiguracji programu ASP.NET 4 głównego

Pliki konfiguracji głównego ( `machine.config` plików i katalog główny `Web.config` pliku) dla .NET Framework 4 (i w związku z tym platformy ASP.NET 4) zostały zaktualizowanie, aby uwzględnić większość informacji o programie ASP.NET 3.5 została znaleziona w konfiguracji standardowy Aplikacja `Web.config` plików. Ze względu na złożoność systemów zarządzanych konfiguracji usług IIS 7 i IIS 7.5 uruchamiania aplikacji ASP.NET 3.5 w ramach platformy ASP.NET 4 i w ramach usług IIS 7 i IIS 7.5 może spowodować ASP.NET lub IIS błędy konfiguracji.

Zaleca się uaktualnienie ASP.NET 3.5 aplikacji platformy ASP.NET 4 za pomocą narzędzia uaktualniania projektu w programie Visual Studio 2010, jeśli jest to możliwe. Program Visual Studio 2010 automatycznie modyfikuje aplikacji ASP.NET 3.5 `Web.config` plik będzie zawierał odpowiednie ustawienia dla platformy ASP.NET 4.

Jednak jest to obsługiwany scenariusz, do uruchamiania aplikacji ASP.NET 3.5 przy użyciu programu .NET Framework 4 bez ponownej kompilacji. W takim przypadku trzeba ręcznie zmodyfikować aplikacji `Web.config` plików przed uruchomieniem aplikacji w ramach programu .NET Framework 4 i w ramach usług IIS 7 lub usług IIS 7.5.

W dwóch następnych sekcjach opisano zmiany, które użytkownik może być konieczne dla różnych kombinacji oprogramowania.

**Windows Vista z dodatkiem SP1 lub Windows Server 2008 z dodatkiem SP1, gdzie są zainstalowane poprawki KB958854 ani z dodatkiem SP2.** W tej konfiguracji system konfiguracji usług IIS 7 niepoprawnie scala zarządzanych konfiguracji aplikacji na porównując na poziomie aplikacji `Web.config` plik do programu ASP.NET 2.0 `machine.config` plików. Ze względu na to, dodatku poziomu aplikacji `Web.config` pliki z programu .NET Framework 3.5 lub nowszego muszą mieć **system.web.extensions** definicja sekcji konfiguracji (element), aby nie spowodować niepowodzenie sprawdzania poprawności usług IIS 7.

Jednak ręcznie zmodyfikować poziom aplikacji `Web.config` we wpisach w plikach, które nie są dokładnie zgodne oryginalnego standardowy sekcji definicji konfiguracji, które zostały wprowadzone w programie Visual Studio 2008 spowoduje, że błędy konfiguracji platformy ASP.NET. (Domyślnych wpisów konfiguracji, które są generowane przez program Visual Studio 2008 działa poprawnie). Jest to powszechny problem ręcznie zmodyfikować `Web.config` Opuść pliki **allowDefinition** i **requirePermission** atrybutów konfiguracji, które znajdują się na różnych sekcji konfiguracji definicje. Powoduje to, że jest niezgodny z sekcji konfiguracji skróconej w poziomie aplikacji `Web.config` plików i kompletną definicję w ASP.NET 4 `machine.config` pliku. W rezultacie w czasie wykonywania systemu konfiguracji platformy ASP.NET 4 zgłasza błąd konfiguracji.

**Windows Vista z dodatkiem SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 i również Windows Vista z dodatkiem SP1 i Windows Server 2008 SP1, na którym zainstalowano poprawkę KB958854.**

W tym scenariuszu systemu macierzystego konfiguracji usług IIS 7 i IIS 7.5 zwraca błąd konfiguracji, ponieważ wykonuje porównanie tekstowe na **typu** atrybut, który jest zdefiniowany dla obsługi sekcji zarządzanych konfiguracji. Ponieważ wszystkie `Web.config` pliki, które są generowane przez program Visual Studio 2008 i programu Visual Studio 2008 z dodatkiem SP1 mają "3.5" w ciągu typu **system.web.extensions** (i pokrewnych) obsługi sekcji konfiguracji, a ponieważ platformy ASP.NET 4 `machine.config` w pliku jest "4.0" **typu** atrybutu dla tej samej konfiguracji sekcji programów obsługi, aplikacje, które są generowane w programie Visual Studio 2008 lub Visual Studio 2008 z dodatkiem SP1, zawsze niepowodzenia walidacji konfiguracji w usługach IIS 7 i IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Rozwiązywanie problemów

Obejście problemu w przypadku pierwszego scenariusza jest zaktualizowanie na poziomie aplikacji `Web.config` plików, umieszczając standardowy tekst konfiguracji z `Web.config` pliku, który został wygenerowany automatycznie przez program Visual Studio 2008.

Jest jeszcze inne obejście dla pierwszego scenariusza, aby zainstalować dodatek Service Pack 2 dla Vista lub Windows Server 2008 na komputerze lub zainstaluj poprawkę KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) Aby rozwiązać problem zachowania niepoprawna konfiguracja i scalania System konfiguracji usług IIS. Jednak po wykonaniu jednej z tych akcji aplikacji prawdopodobnie wystąpi błąd konfiguracji z powodu problemu opisanego w drugim scenariuszu.

Obejście dla drugi scenariusz polega na usunięcia lub Dodaj komentarz wszystkich **system.web.extensions** definicje sekcji konfiguracji i sekcji konfiguracji grupy definicje z poziomu aplikacji `Web.config` pliku. Te definicje są zwykle w górnej części na poziomie aplikacji `Web.config` plików i mogą być identyfikowane przez **configSections** elementu i jego elementów podrzędnych.

W obydwu scenariuszach, zalecane jest, że możesz również ręcznie usunąć **system.codedom** sekcji, chociaż nie jest to wymagane.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Aplikacje programu ASP.NET 4 podrzędnych zakończyć się niepowodzeniem do ekranu startowego w ramach programu ASP.NET 2.0 lub ASP.NET aplikacji w wersji 3.5

ASP.NET 4 aplikacje, które są skonfigurowane jako elementy podrzędne w aplikacji, które są uruchomione wcześniejsze wersje programu ASP.NET może nie można uruchomić z powodu błędów kompilacji lub konfiguracji. Poniższy przykład pokazuje strukturę katalogu dla aplikacji, których to dotyczy.

`/parentwebapp` (skonfigurowane do używania programu ASP.NET 2.0 lub ASP.NET 3.5)  
`/childwebapp` (skonfigurowana do używania programu ASP.NET 4)

Stosowanie w `childwebapp` folderu zakończy się niepowodzeniem do uruchamiania usług IIS 7 lub usług IIS 7.5 i będzie raportu błąd konfiguracji. Tekst błędu będzie zawierać komunikat podobny do następującego:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

W usługach IIS 6, aplikacji w `childwebapp` folderu również nie zostanie uruchomiony, ale zgłosi on różnych błędów. Na przykład tekst błędu może podać następujące czynności:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Te scenariusze być fakt, że informacje o konfiguracji z aplikacji nadrzędnej w `parentwebapp` folder jest częścią hierarchii informacje o konfiguracji, który określa ustawienia konfiguracji końcowej scalone, które są używane przez internetowy podrzędne w `childwebapp` folderu. W zależności od tego, czy aplikacja sieci Web platformy ASP.NET 4 jest uruchomiona, w usługach IIS 7 (lub usług IIS 7.5) lub w usługach IIS 6 systemu konfiguracyjnego usług IIS lub systemu kompilacji platformy ASP.NET 4 zwróci błąd.

Kroki, które należy wykonać, aby rozwiązać ten problem i uzyskać elementu podrzędnego do działania aplikacji programu ASP.NET 4 zależą od tego, czy aplikacji platformy ASP.NET 4 działa w usługach IIS 6 lub IIS 7 (lub usług IIS 7.5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Krok 1 (usługi IIS 7 lub tylko IIS 7.5)

Ten krok jest niezbędny tylko w systemach operacyjnych, które są uruchamiane usługi IIS 7 i IIS 7.5, w tym Windows Vista, Windows Server 2008, Windows 7 i Windows Server 2008 R2.

Przenieś **configSections** definicji w `Web.config` pliku aplikacji nadrzędnej (aplikacja jest uruchamiana w wersji 2.0 programu ASP.NET lub ASP.NET 3.5) w katalogu głównym `Web.config` pliku dla środowiska.NET Framework 2.0. Skanuje system natywnych konfiguracji usług IIS 7 i IIS 7.5 **configSections** elementu w przypadku łączy on hierarchii plików konfiguracyjnych. Przenoszenie **configSections** definicji z aplikacji sieci Web elementu nadrzędnego `Web.config` pliku w folderze głównym `Web.config` pliku skutecznie ukrywa element z procesu scalania konfiguracji, który występuje podrzędnego, platformy ASP.NET 4 aplikacja.

W 32-bitowym systemie operacyjnym lub dla pul aplikacji 32-bitowych, katalog główny `Web.config` plik dla programu ASP.NET 2.0 i ASP.NET 3.5 zwykle znajduje się w następującym folderze:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

W 64-bitowym systemie operacyjnym lub dla pul aplikacji 64-bitowych, katalog główny `Web.config` plik dla programu ASP.NET 2.0 i ASP.NET 3.5 zwykle znajduje się w następującym folderze:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Jeśli uruchamiasz 32-bitowych i 64-bitowych aplikacji sieci Web na komputerze 64-bitowym, należy przenieść **configSections** element w górę do katalogu głównego `Web.config` plików dla 32-bitowych i 64-bitowych systemach.

Po umieszczeniu **configSections** element w katalogu głównym `Web.config` plików, Wklej sekcji natychmiast po **konfiguracji** elementu. Poniższy przykład pokazuje, jakie górnej części katalogu głównego `Web.config` plik powinien wyglądać po zakończeniu przenoszenia elementów.

> [!NOTE]
> W poniższym przykładzie zostały opakowane wierszy dla czytelności.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Krok 2 (wszystkie wersje usług IIS)

Ten krok jest wymagany, czy element podrzędny aplikacji sieci Web platformy ASP.NET 4 jest uruchomiona w usługach IIS 6 lub IIS 7 (lub usług IIS 7.5).

W `Web.config` pliku nadrzędnego aplikację sieci Web, która jest uruchomiona, 2 platformy ASP.NET lub ASP.NET 3.5 Dodaj **lokalizacji** tag, który jawnie określa (dla systemów konfiguracji usług IIS i platformy ASP.NET), tylko wpisy konfiguracji mają zastosowanie do aplikacji sieci Web elementu nadrzędnego. Poniższy przykład pokazuje składnię **lokalizacji** elementu do dodania:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

W poniższym przykładzie pokazano sposób, w jaki **lokalizacji** tag jest używany do opakowywania wszystkich sekcji konfiguracji, począwszy od **appSettings** sekcji, a kończąc na **system.webServer**sekcji.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Po ukończeniu kroków 1 i 2, aplikacji sieci Web platformy ASP.NET 4 podrzędnych rozpocznie się bez błędów.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 witryn sieci Web nie można uruchomić na komputerach, na którym zainstalowano program SharePoint

Serwery sieci Web, z programem SharePoint mają `Web.config` pliku, który jest wdrażany w katalogu głównym witryny sieci Web programu SharePoint (na przykład `c:\inetpub\wwwroot\web.config` dla domyślnej witryny sieci Web). W tym `Web.config` pliku SharePoint ustawia niestandardowe częściowego zaufania poziomu o nazwie WSS\_minimalny.

Jeśli zostanie podjęta próba uruchomienia witryny sieci Web programu ASP.NET 4, który jest wdrażany jako element podrzędny tego rodzaju witryny sieci Web programu SharePoint, zostanie wyświetlony następujący błąd:

`Could not find permission set named 'ASP.NET'.`

Ten błąd występuje, ponieważ infrastruktury zabezpieczenia dostępu kodu platformy ASP.NET 4 wyszukuje zestaw uprawnień o nazwie ASP.NET. Jednak częściowego zaufania pliku konfiguracji, który odwołuje się do niej WSS\_minimalny nie zawiera żadnych zestawów uprawnień o tej nazwie.

Obecnie nie ma wersji programu SharePoint dostępne, która jest zgodna z programem ASP.NET. W rezultacie możesz nie należy próbować uruchamiać witrynę sieci Web programu ASP.NET 4 jako lokację podrzędną względem poniżej sieci Web programu SharePoint z witryny.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>Właściwość HttpRequest.FilePath nie zawiera już PATHINFO zawiera wartości

Poprzednie wersje programu ASP.NET uwzględnione **PATHINFO zawiera** wartości w wartości zwracanej z różnych związanym ze ścieżką właściwości pliku, w tym **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, i **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 nie zawiera już **PATHINFO zawiera** wartości zwracanej wartości z tych właściwości. Zamiast tego **PATHINFO zawiera** informacje są dostępne w **HttpRequest.PathInfo**. Na przykład Wyobraź sobie następujący fragmentu adresu URL:

`/testapp/Action.mvc/SomeAction`

We wcześniejszych wersjach programu ASP.NET **HttpRequest** właściwości mieć następujące wartości:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (pusty)

W technologii ASP.NET 4 **HttpRequest** właściwości zamiast mieć następujące wartości:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>Program ASP.NET 2.0 aplikacji może generować httpexception — błędy, które odwołują się eurl.axd

Po włączeniu programu ASP.NET 4 w usługach IIS 6 aplikacje programu ASP.NET 2.0, które działają w usługach IIS 6 (w systemie Windows Server 2003 lub Windows Server 2003 R2) może generować błędy, takie jak następujące:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Ten błąd występuje, ponieważ gdy ASP.NET wykryje, że witryny sieci Web jest skonfigurowany do użycia platformy ASP.NET 4, natywne składnik ASP.NET 4 przekazuje adresu URL bez rozszerzenia części zarządzanego programu ASP.NET do dalszego przetwarzania. Jednak jeśli katalogi wirtualne, które znajdują się poniżej witryny sieci Web programu ASP.NET 4 są skonfigurowane do używania programu ASP.NET 2.0, przetwarzania adresu URL bez rozszerzenia w ten sposób skutkuje zmodyfikowany adres URL, który zawiera ciąg "eurl.axd". Zmodyfikowany adres URL jest następnie wysyłana do aplikacji programu ASP.NET 2.0. Program ASP.NET 2.0 nie rozpoznaje formatu "eurl.axd". W związku z tym, próbuje odnaleźć pliku o nazwie ASP.NET 2.0 `eurl.axd` i wykonaj go. Ponieważ taki plik nie istnieje, żądanie kończy się niepowodzeniem z **httpexception —** wyjątku.

Można obejść ten problem przy użyciu jednej z następujących opcji.

### <a name="option-1"></a>Opcja 1

Jeśli platformy ASP.NET 4 nie jest wymagane do uruchomienia witryny sieci Web, należy ponownie zamapować lokacji zamiast tego użyć programu ASP.NET 2.0.

### <a name="option-2"></a>Opcja 2

Jeśli platformy ASP.NET 4 jest wymagany, aby można było uruchomić witryny sieci Web, należy przenieść wszelkich podrzędnych katalogów wirtualnych ASP.NET 2.0 do innej witryny sieci Web, który jest mapowany na platformie ASP.NET 2.0.

### <a name="option-3"></a>Opcja 3

Jeśli nie jest praktyczne, aby ponownie zamapować witryny sieci Web programu ASP.NET 2.0 lub aby zmienić lokalizację katalogu wirtualnego, jawnie Wyłącz adresu URL bez rozszerzenia, przetwarzanie w ASP.NET 4. Użyj poniższej procedury:

1. W rejestrze systemu Windows należy otworzyć następującego węzła:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Utwórz nową **DWORD** wartość o nazwie **EnableExtensionlessUrls**.
2. Ustaw **EnableExtensionlessUrls** na 0. Powoduje to wyłączenie zachowanie adresu URL bez rozszerzenia.
3. Zapisz wartość rejestru, a następnie zamknij Edytor rejestru.
4. Uruchom **iisreset** narzędzie wiersza polecenia, co powoduje, że usługi IIS, aby odczytać nowej wartości rejestru.

> [!NOTE]
> Ustawienie **EnableExtensionlessUrls** 1 umożliwia zachowanie adresu URL bez rozszerzenia. To ustawienie domyślne, jeśli nie określono wartości.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Programy obsługi zdarzeń nie może zostać nie wywołane dokumentu domyślnego w IIS 7.5 lub IIS 7 trybie zintegrowanym

ASP.NET 4 zawiera modyfikacje, które zmieniają sposób, w jaki **akcji** atrybutu HTML **formularza** element jest renderowany, gdy jest rozpoznawana jako dokument domyślny adres URL bez rozszerzenia. Przykładem adresu URL bez rozszerzenia rozpoznawania dokument domyślny może być [ http://contoso.com/ ](http://contoso.com/), dając w żądaniu skierowanym do [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

ASP.NET 4 teraz renderuje kod HTML **formularza** elementu **akcji** wartość atrybutu jako ciąg pusty, po wysłaniu żądania do bez rozszerzenia adresu URL, który ma do niej przyporządkowany dokument domyślny. Na przykład we wcześniejszych wersjach programu ASP.NET, żądanie [ http://contoso.com ](http://contoso.com) mogłoby spowodować żądanie `Default.aspx`. W tym dokumencie otwarcia **formularza** pozbawi znaczników, jak w poniższym przykładzie:

`<form action="Default.aspx" />`

ASP.NET 4, żądanie [ http://contoso.com ](http://contoso.com) również wyniki w żądaniu skierowanym do `Default.aspx`. Jednak platforma ASP.NET teraz renderuje otwierania HTML **formularza** znaczników, jak w poniższym przykładzie:

`<form action="" />`

Różnica w sposób, w jaki **akcji** renderowania atrybutów mogą spowodować wprowadzono subtelne zmiany w przetwarzaniu post formularza przez usługi IIS i platformy ASP.NET. Gdy **akcji** atrybut jest pustym ciągiem, IIS **DefaultDocumentModule** obiektu będzie utworzyć żądania podrzędnego do `Default.aspx`. W większości okolicznościach, to żądanie podrzędne jest niewidoczne dla kodu aplikacji i `Default.aspx` strona działa normalnie.

Potencjalne interakcji między kodem zarządzanym i trybie usług IIS 7 lub IIS 7.5 zintegrowane może jednak spowodować strony .aspx zarządzanych przestaną działać poprawnie podczas żądania podrzędnego. Jeśli wystąpią następujące warunki, żądanie podrzędne `Default.aspx` dokumentu spowoduje błąd lub nieoczekiwane zachowanie:

1. Na stronie .aspx jest wysyłany do przeglądarki przy użyciu **formularza** elementu **akcji** ustawioną wartość atrybutu "".
2. Formularz jest publikowany do platformy ASP.NET.
3. Moduł HTTP zarządzany odczytuje część treści jednostki. Na przykład moduł odczytuje **Request.Form** lub **Request.Params**. Powoduje to treści jednostki żądania POST do odczytu do pamięci zarządzanej. W rezultacie treści jednostki nie jest już dostępna dla wszelkich modułów kodu natywnego, które działają w trybie usługi IIS 7 lub IIS 7.5 zintegrowane.
4. Usługi IIS **DefaultDocumentModule** obiektu po pewnym czasie działa i tworzy żądanie podrzędne `Default.aspx` dokumentu. Ponieważ treści jednostki już zostało odczytane przez fragment kodu zarządzanego, nie jest jednak nie można wysyłać żądania podrzędnego treści jednostki.
5. Podczas wykonywania potoku HTTP dla żądania podrzędnego, obsługa `.aspx` plików, który jest uruchamiany w fazie handler-execute.
6. Ponieważ nie ma żadnych treści jednostki, istnieją żadnych zmiennych formularza i bez stanu widoku, a w związku z tym nie ma dostępnych informacji obsługi strony .aspx ustalić, jakie zdarzenie (jeśli istnieje) powinien zostać wywołane. W rezultacie Brak programów obsługi zdarzeń zwrotu strony .aspx dotyczy Uruchom.

Możesz obejść ten problem, w następujący sposób:

- Identyfikowanie modułu HTTP, który uzyskuje dostęp do treści jednostki żądania podczas żądań dokumentu domyślnego i ustalić, czy można skonfigurować do uruchamiania tylko w przypadku zarządzanych żądań. W trybie zintegrowanym zarówno dla usług IIS 7 i 7.5 usług IIS, można oznaczyć moduły HTTP do uruchamiania tylko w przypadku zarządzanych żądań, dodając następujący atrybut w głównej **system.webServer/modules** wpis:

- `precondition="managedHandler"`

- Ta wyłącza ustawienia modułu dla żądań, czy usługi IIS 7 i IIS 7.5 określić jako nie jest zarządzany żądań. Dla żądań dokumentu domyślnego pierwsze żądanie jest adres URL bez rozszerzenia. W związku z tym usługi IIS nie jest uruchomiony modułów zarządzanych, które są oznaczone o wstępnym zarządzanego programu obsługi podczas przetwarzania żądania wstępnego. W wyniku modułów zarządzanych nie zostaną przypadkowo odczytywał element body encji i dlatego treści jednostki są nadal dostępne i jest przekazywany do żądania podrzędnego i dokument domyślny.

- Jeśli trzeba uruchomić dla wszystkich żądań problematyczne moduły HTTP (plików statycznych adresów URL bez rozszerzenia, które nawiązują do **DefaultDocumentModule** obiektu zarządzanego żądań, itp.), jawnie modyfikowanie stron aspx wykorzystywanych przez ustawienie **akcji** właściwości tej strony **System.Web.UI.HtmlControls.HtmlForm** kontroli na pusty ciąg. Na przykład, jeśli dokument domyślny jest `Default.aspx`, modyfikować kodu strony, aby jawnie ustawić **HtmlForm** kontrolki **akcji** właściwość "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Zmiany w implementacji zabezpieczenia dostępu kodu platformy ASP.NET

Program ASP.NET 2.0, i przy użyciu rozszerzenia funkcji programu ASP.NET, które zostały dodane w wersji 3.5 używać .NET Framework 1.1 i 2.0 kodu dostępu zabezpieczeń (CAS) modelu. Jednak implementacja urzędów certyfikacji w ASP.NET 4 został znacznie overhauled. W rezultacie częściowego zaufania aplikacji ASP.NET, które zależą od zaufanego kodu w globalnej pamięci podręcznej zestawów (GAC) może się nie powieść z różnych wyjątków zabezpieczeń. Aplikacje częściowego zaufania, które zależą od rozległych modyfikacji na komputerze zasady CAS może również zakończyć się niepowodzeniem z wyjątków zabezpieczeń.

Możesz przywrócić aplikacje programu ASP.NET 4 częściowego zaufania do zachowania ASP.NET 1.1 i 2.0 za pomocą nowego **legacyCasModel** atrybutu w **zaufania** element konfiguracji, jak pokazano w poniższym przykładzie :

`<trust level= "Medium" legacyCasModel="true" />`

Po przywróceniu do starszego modelu urzędów certyfikacji, są włączone następujące starego zachowania urzędów certyfikacji:

- Zasady CAS maszyny zostanie uznane.
- Dopuszcza się wielu zestawów różnych uprawnień w domenie pojedynczej aplikacji.
- Potwierdzenia jawne uprawnienia nie są wymagane dla zestawów w pamięci podręcznej GAC, które są wywoływane, gdy tylko ASP.NET lub inny kod .NET Framework znajduje się na stosie.

Nie można przywrócić jeden scenariusz w programie .NET Framework 4: aplikacje częściowo zaufane Web nie może wywołać niektórych interfejsów API w System.Web.dll i System.Web.Extensions.dll. W poprzednich wersjach programu .NET Framework, było możliwe innych częściowego zaufania aplikacji należy jawnie przyznać <strong>AspNetHostingPermission</strong> uprawnienia. Aplikacje te można następnie użyć <strong>System.Web.HttpUtility</strong>, typy w <strong>System.Web.ClientServices.\< / strong > * obszary nazw i typy związane z członkostwa, ról i profilów. Wywoływanie tych typów z aplikacji sieci Web częściowej relacji zaufania nie jest już obsługiwana w programie .NET Framework 4.

> [!NOTE]
> **HtmlEncode** i **HtmlDecode** funkcjonalność **System.Web.HttpUtility** klasy został przeniesiony do nowej platformy .NET Framework 4  **System.Net.WebUtility** klasy. Jeśli było tylko funkcje platformy ASP.NET, które było używane, należy zmodyfikować kod aplikacji, aby użyć nowego **WebUtility** klasy zamiast tego.


Poniżej przedstawiono podsumowanie wysokiego poziomu zmian Domyślna implementacja urzędów certyfikacji w ASP.NET 4:

- Domeny aplikacji ASP.NET są teraz domen jednorodnych aplikacji. Tylko zestawy grant częściowego zaufania i pełnego zaufania są dostępne w domenie aplikacji.
- ASP.NET grant częściowym zaufaniu zestawy są niezależne od klasy korporacyjnej, poziomie komputera lub użytkownika na poziomie zasady CAS.
- Zestawy platformy ASP.NET, które dostarczana z 3.5 i 3.5 z dodatkiem SP1 został przekonwertowany na używa modelu przezroczystości .NET Framework 4.
- Korzystanie z platformy ASP.NET **AspNetHostingPermission** znacznie zmniejszyć atrybutu. Większość wystąpień tego atrybutu zostały usunięte z publicznych interfejsów API platformy ASP.NET.
- Dynamicznie skompilowanych zestawów, które są tworzone przez dostawców kompilacji platformy ASP.NET zostały zaktualizowanie, aby wyraźnie oznaczyć zestawów jako przezroczysty.
- Wszystkie zestawy platformy ASP.NET zostały oznaczone w taki sposób, że atrybut aptca jest honorowane tylko w środowiskach hostingu w sieci Web. Częściowo zaufanych-Web hostingu środowiskach, takich jak ClickOnce nie będzie wywoływać zestawów ASP.NET.

Aby uzyskać więcej informacji na temat nowy model zabezpieczeń dostępu kodu platformy ASP.NET 4, zobacz [przy użyciu zabezpieczeń dostępu kodu w aplikacjach ASP.NET](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) w witrynie MSDN w sieci Web.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>Zostały przeniesione MembershipUser a innymi typami danych w Namespace System.Web.Security

Niektóre typy, które są używane w członkostwa ASP.NET zostały przeniesione z `System.Web.dll` do nowego zestawu System.Web.ApplicationServices.dll. Typy zostało przeniesione, aby można było rozpoznać warstw architektury zależności między typami w kliencie, jak i w postaci jednostek SKU rozszerzonej platformy .NET Framework.

Projektów witryny sieci Web nie występują problemy, w wyniku przeniesienie tych typów, ponieważ System.Web.ApplicationServices.dll został dodany do listy zestawów, do którego istnieje odwołanie, używany domyślnie przez system kompilacji platformy ASP.NET. Jeśli zaktualizujesz projekt witryny sieci Web, który został utworzony przy użyciu wcześniejszej wersji programu ASP.NET do ASP.NET 4, otwierając go w programie Visual Studio 2010, projekt zostanie skompilowany bez błędów.

Podobnie jeśli zaktualizujesz projekt aplikacji sieci Web, który został utworzony we wcześniejszej wersji programu ASP.NET do ASP.NET 4, otwierając go w programie Visual Studio 2010, proces uaktualniania dodaje odwołanie do System.Web.ApplicationServices.dll do projektu. W związku z tym uaktualnione projekty aplikacji również zostanie skompilowana bez błędów w sieci Web.

Skompilowane pliki (binarne), które zostały utworzone we wcześniejszych wersjach programu ASP.NET również uruchomią się bez błędów na platformie ASP.NET 4, nawet jeśli typy członkostwa zostały przeniesione do innego zestawu. Typ przekazywania informacji został dodany do wersji platformy ASP.NET 4 `System.Web.dll` odwołania do środowiska wykonawczego dla tych typów, automatycznie kieruje do nowej lokalizacji dla typów.

Jednak bibliotek klas, które korzystają z członkostwa w określonych typach i uaktualniono z wcześniejszych wersji programu ASP.NET zakończy się niepowodzeniem do kompilowania, gdy jest używana w projektach programu ASP.NET 4. Na przykład projekt biblioteki klas może zakończyć się niepowodzeniem do kompilowania i zgłoś błąd, takie jak następujące:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Ten problem można obejść, dodając odwołanie w projekcie biblioteki klas System.Web.ApplicationServices.dll.

Poniższa lista przedstawia *System.Web.Security* typy, które zostały przeniesione z `System.Web.dll` do System.Web.ApplicationServices.dll:

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Dane wyjściowe różnią się w pamięci podręcznej zmiany \* nagłówka HTTP

W wersji 1.0 programu ASP.NET, stron, które określono w pamięci podręcznej usterka spowodowana `Location="ServerAndClient"` jako dane wyjściowe — ustawienie pamięci podręcznej do emitowania `Vary:*` nagłówek odpowiedzi HTTP. To w efekcie informuje przeglądarki klienta nigdy nie pamięci podręcznej na stronie lokalnie.

W ASP.NET 1.1 **System.Web.HttpCachePolicy.SetOmitVaryStar** został dodany metody, które można wywołać, aby pominąć `Vary:*` nagłówka. Ta metoda wybrano, ponieważ zmiana emitowany nagłówka HTTP zostało uznane za potencjalnie istotne zmiany w czasie. Jednak deweloperzy są mylące zachowanie na platformie ASP.NET i raporty o usterkach zgłaszane sugerują, deweloperzy i znają istniejącej **SetOmitVaryStar** zachowanie.

W technologii ASP.NET 4 podjęto decyzję, aby rozwiązać ten problem głównego. `Vary:*` z odpowiedzi, które określają następujące dyrektywy, nie jest już jest emitowany nagłówka HTTP:

`<%@OutputCache Location="ServerAndClient" %>`

W rezultacie **SetOmitVaryStar** jest już potrzebne, aby pominąć `Vary:*` nagłówka.

W aplikacji, które określają `Location="ServerAndClient"` w **@ OutputCache** dyrektywy na stronie, zostanie wyświetlona zachowanie też dorozumianych przez nazwę **lokalizacji** wartość atrybutu —, będzie stron podlega buforowaniu, w przeglądarce bez konieczności, który można wywoływać **SetOmitVaryStar** metody.

Jeśli na stronach aplikacji musi wysyłać właściwość `Vary:*`, wywołaj **AppendHeader** metody, jak w poniższym przykładzie:

`HttpResponse.AppendHeader("Vary","*");`

Alternatywnie, można zmienić wartość buforowania danych wyjściowych **lokalizacji** atrybut "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Typy System.Web.Security na potrzeby usługi Passport są przestarzałe

Passport wbudowaną w ASP.NET 2.0 została obsługiwany przez kilka lat z powodu zmian w Passport (teraz LiveID). W rezultacie pięć typów związane z usługi Passport w **System.Web.Security** zostały oznaczone za pomocą **ObsoleteAttribute** atrybutu.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Właściwość MenuItem.PopOutImageUrl nie powiedzie się, by renderować obraz na platformie ASP.NET 4

W programie ASP.NET 3.5 *MenuItem.PopOutImageUrl* Właściwość pozwala określić adres URL obrazu, który jest wyświetlany w elemencie menu, aby wskazać, czy element menu ma dynamiczny podmenu. Poniższy przykład pokazuje, jak można określić tę właściwość w znacznikach w ASP.NET 3.5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

W wyniku zmiany projektu w programie ASP.NET 4, żadne dane wyjściowe nie jest renderowany *PopOutImageUrl* Jeśli właściwość jest ustawiona dla *MenuItem* klasy. Zamiast tego należy określić adres URL obrazu bezpośrednio w *Menu* kontrolować za pomocą *StaticPopOutImageUrl* właściwości lub *DynamicPopOutImageUrl* właściwości. Podczas pracy z menu statyczne, *Menu.StaticPopOutImageUrl* właściwość określa adres URL obrazu, który jest wyświetlany w celu wskazania, że element menu statyczne ma podmenu, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Jeśli pracujesz z menu dynamiczne, możesz użyć *Menu.DynamicPopOutImageUrl* właściwość, aby określić adres URL obrazu, który wskazuje, że element menu dynamiczne ma podmenu. Poniższy przykład jest podobny do poprzedniego, ale pokazuje, jak ustawić *DynamicPopOutImageUrl* właściwość menu dynamiczne.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Jeśli *Menu.DynamicPopOutImageUrl* nie ustawiono właściwości i *Menu.DynamicEnableDefaultPopOutImage* właściwość jest ustawiona na *false*, obraz nie jest wyświetlany. Podobnie jeśli *StaticPopOutImageUrl* nie ustawiono właściwości i *StaticEnableDefaultPopOutImage* właściwość jest ustawiona na *false*, obraz nie jest wyświetlany.

Podczas ustawiania ścieżek tych właściwości należy użyć ukośnika (/) zamiast ukośnik odwrotny (\). Aby uzyskać więcej informacji, zobacz [Menu.StaticPopOutImageUrl i Menu.DynamicPopOutImageUrl nie będą renderowanie obrazów podczas ścieżki zawierać ukośników odwrotnych](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") gdzie indziej w tym dokumencie.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl i Menu.DynamicPopOutImageUrl się nie powieść się renderowanie obrazów, gdy ścieżki zawierać ukośników odwrotnych

ASP.NET 4, obrazy, które można określić za pomocą *Menu.StaticPopOutImageUrl* i *Menu.DynamicPopOutImageUrl* właściwości nie będą renderowane, jeżeli ścieżka zawiera backlashes (\). Jest to zmiana z wcześniejszych wersji programu ASP.NET.

Poniższy przykład *Menu* kontrolować pokazuje znaczników *StaticPopOutImageUrl* właściwością przy użyciu ścieżki, która zawiera ukośnik odwrotny. ASP.NET 4 określonego we właściwości obrazu nie będzie renderowana.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Aby rozwiązać ten problem, zmień wartości ścieżek, które są określone w *StaticPopOutImageUrl* i *DynamicPopOutImageUrl* właściwości, aby używać ukośników (/). Poniższy przykład pokazuje tę zmianę:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Należy pamiętać, że aplikacje, które zostały zmigrowane z wcześniejszych wersji programu ASP.NET do ASP.NET 4 może również mieć wpływ, ponieważ *MenuItem.PopOutImageUrl* właściwość została zmieniona. Aby uzyskać więcej informacji, zobacz [MenuItem.PopOutImageUrl właściwości nie można renderować obraz w programie ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") gdzie indziej w tym dokumencie.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

To jest dokument wstępny i może ulec znacznym zmianom przed ostatecznym wydaniem komercyjnym oprogramowania opisanych tutaj materiałach.

Informacje zawarte w tym dokumencie reprezentuje bieżący widok firmy Microsoft Corporation na problemy omówione w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki na rynku, nie powinny być rozumiane jako zobowiązanie ze strony firmy Microsoft, a firma Microsoft nie gwarantuje dokładności informacji po opublikowaniu.

Ten dokument jest tylko do celów informacyjnych. MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH ANI USTAWOWYCH, W ODNIESIENIU DO INFORMACJI W TYM DOKUMENCIE.

Zgodnie z obowiązującymi przepisami prawa autorskiego jest odpowiedzialny za użytkownika. Bez ograniczenia, praw autorskich, żadna część tego dokumentu może być odtworzyć, przechowywane w lub wprowadzone do systemu wyszukiwania lub przekazywanych w jakiejkolwiek formie lub za pomocą jakichkolwiek środków (elektronicznych, mechanicznych, fotokopiowania, nagrywania lub w przeciwnym razie) lub w jakimkolwiek celu, bez wyraźnej pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może być patentów, wniosków patentowych, znaków towarowych, praw autorskich lub innych praw własności intelektualnej dotyczące elementów zawartych w tym dokumencie. Wyraźnie określone w umowach licencyjnych firmy Microsoft, udostępnienie tego dokumentu nie mu żadnych licencji dotyczących tych patentów, znaków towarowych, praw autorskich i innej własności intelektualnej.

Jeśli nie określono inaczej, przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia wymienione w tym dokumencie są fikcyjne i żadne skojarzenia z rzeczywistymi firmami, organizacji, produktu, nazwa domeny, adres e-mail adres, logo, osoby, miejsca lub zdarzeń jest przeznaczone lub powinny być zakładane.

© 2010 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Firma Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stanach Zjednoczonych i/lub innych krajach.

Nazwy firm i produktów wymienione w niniejszym dokumencie mogą być znakami przez ich właścicieli.
