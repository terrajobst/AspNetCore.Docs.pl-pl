---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 istotne zmiany | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym dokumencie opisano zmiany, które zostały wprowadzone dla programu .NET Framework w wersji 4 release, które mogą wpłynąć na aplikacje, które zostały utworzone przy użyciu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: a0f25ed3c996b73e362177b196539c6f2b143739
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 najważniejszych zmian
====================
> W tym dokumencie opisano zmiany, które zostały wprowadzone dla programu .NET Framework w wersji 4 zlecenia, które mogą wpłynąć na aplikacje, które zostały utworzone przy użyciu wcześniejszych wersjach, w tym wersje platformy ASP.NET 4 Beta 1 i 2 w wersji Beta.
> 
> [Pobierz ten dokument](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Spis treści

[Ustawienie ControlRenderingCompatibilityVersion w pliku Web.config](#0.1__Toc256770141 "_Toc256770141")  
[Zmiany ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode i teraz UrlEncode kodowania pojedynczy cudzysłów](#0.1__Toc256770143 "_Toc256770143")  
[Strony ASP.NET (aspx) Analizator jest Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Pliki definicji przeglądarki zaktualizowane](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll usunięte z pliku konfiguracji sieci Web głównego](#0.1__Toc256770146 "_Toc256770146")  
[Weryfikacja żądania ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[Domyślne algorytmem wyznaczania wartości skrótu jest obecnie HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Błędy konfiguracji związane z nowej konfiguracji głównego 4 ASP.NET](#0.1__Toc256770149 "_Toc256770149")  
[Aplikacji programu ASP.NET 4 podrzędnych nie Start w ASP.NET 2.0 lub ASP.NET 3.5 Applications](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 witryn sieci Web nie można uruchomić na komputerach, na którym jest zainstalowany program SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[Właściwość HttpRequest.FilePath nie zawiera już PATHINFO zawiera wartości](#0.1__Toc256770152 "_Toc256770152")  
[Platforma ASP.NET 2.0 aplikacji może generować błędy HttpException, które odwołują się eurl.axd](#0.1__Toc256770153 "_Toc256770153")  
[Programy obsługi zdarzeń nie może zostać nie wywołane w dokumentu domyślnego w usługach IIS 7 lub usług IIS 7.5 zintegrowane tryb](#0.1__Toc256770154 "_Toc256770154")  
[Zmiany w implementacji zabezpieczeń (CAS) dostępu do kodu ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[Zostały przeniesione MembershipUser i innych typów w Namespace System.Web.Security](#0.1__Toc256770156 "_Toc256770156")  
[Dane wyjściowe buforowania zmiany różnią się \* nagłówka HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Typy System.Web.Security na potrzeby usługi Passport są przestarzałe](#0.1__Toc256770158 "_Toc256770158")  
[Właściwość MenuItem.PopOutImageUrl nie powiedzie się, by renderować obraz w programie ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl i niepowodzenie Menu.DynamicPopOutImageUrl do realizacji obrazów, gdy ścieżki zawierać ukośników odwrotnych](#0.1__Toc256770160 "_Toc256770160")  
[Zastrzeżenie](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Ustawienie ControlRenderingCompatibilityVersion w pliku Web.config

Kontrolki ASP.NET został zmodyfikowany w programie .NET Framework w wersji 4 celu pozwalają określić dokładnie sposób ich renderowania kodu znaczników. W poprzednich wersjach programu .NET Framework niektóre formanty emitowane znaczników, który ma możliwość wyłączenia. Domyślnie nie jest generowany programu ASP.NET 4 tego rodzaju znaczników.

Jeśli używasz programu Visual Studio 2010 do uaktualniania aplikacji programu ASP.NET w wersji 2.0 lub ASP.NET 3.5, narzędzie automatycznie dodaje ustawienie `Web.config` pliku, który zachowuje renderowania starszej wersji. Jednak jeśli aplikacja jest uaktualniana przez zmianę puli aplikacji w usługach IIS pod kątem programu .NET Framework 4, program ASP.NET używa domyślnie nowy tryb renderowania. Aby wyłączyć nowy tryb renderowania, Dodaj następujące ustawienie w `Web.config` pliku:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Zmiany głównych renderowania, które oferuje nowe zachowanie są następujące:

- **Obrazu** i **ImageButton** formanty już renderować `border="0"` atrybutu.
- **BaseValidator** klasy i sprawdzanie poprawności formantów, które dziedziczą z niego renderowania czerwony tekst nie jest już domyślnie.
- **HtmlForm** formantu nie jest renderowana **nazwa** atrybutu.
- **Tabeli** kontrolować już renderuje `border="0"` atrybutu.
- Formanty, które nie są przeznaczone dla danych wejściowych użytkownika (na przykład **etykiety** kontroli) nie jest już renderowania `disabled="disabled"` atrybut, jeśli ich **włączone** właściwość jest ustawiona na **false**(lub jeśli to ustawienie, dziedziczą z formantu kontenera).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode zmiany

**ClientIDMode** ustawienie platformy ASP.NET 4 pozwala określić, jak program ASP.NET generuje **identyfikator** atrybutów elementów HTML. W poprzednich wersjach programu ASP.NET została odpowiednikiem domyślne zachowanie **identyfikator automatyczny** ustawienie **ClientIDMode**. Ustawieniem domyślnym jest jednak obecnie **przewidywalny**.

Jeśli używasz programu Visual Studio 2010 do uaktualniania aplikacji programu ASP.NET w wersji 2.0 lub ASP.NET 3.5, narzędzie automatycznie dodaje ustawienie `Web.config` pliku, który zachowuje zachowanie wcześniejszych wersji programu .NET Framework. Jednak jeśli aplikacja jest uaktualniana przez zmianę puli aplikacji w usługach IIS pod kątem programu .NET Framework 4, program ASP.NET używa domyślnie nowy tryb. Aby wyłączyć trybu Identyfikatora klienta, Dodaj następujące ustawienie w `Web.config` pliku:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode i teraz UrlEncode kodowania znaków pojedynczego cudzysłowu

W przypadku programu ASP.NET 4 **HtmlEncode** i **UrlEncode** metody **HttpUtility** i **HttpServerUtility** klasy zostały zaktualizowane w usłudze kodowanie znaków pojedynczego cudzysłowu (') w następujący sposób:

- **HtmlEncode** metoda koduje wystąpień pojedynczy cudzysłów jako.
- **UrlEncode** metoda koduje wystąpień pojedynczy cudzysłów jako % 27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Strony ASP.NET (aspx) Analizator jest Stricter

Analizator strony dla stron ASP.NET (`.aspx` plików) i kontrolki użytkownika (`.ascx` plików) jest bardziej restrykcyjne w ASP.NET 4 i spowoduje odrzucenie więcej wystąpień nieprawidłowy kod znaczników. Na przykład następujące dwa fragmenty kodu może pomyślnie przeanalizować we wcześniejszych wersjach programu ASP.NET, ale teraz zgłosi błędy analizatora składni ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Zwróć uwagę, nieprawidłowy średnik na końcu **pole ukryte** tagu.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Zwróć uwagę, niezamknięty **styl** atrybut, który jest uruchamiany w **CssClass** atrybutu.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Zaktualizowane pliki definicji przeglądarki

Aby dołączyć informacje o nowych i zaktualizowanych przeglądarki i urządzenia zostały zaktualizowane pliki definicji przeglądarki. Starszych przeglądarek i urządzeń, takich jak Netscape Navigator zostały usunięte i nowszych przeglądarkach i urządzeniach, takich jak Google Chrome i Apple iPhone zostały dodane.

Jeśli aplikacja zawiera definicje przeglądarki niestandardowych, które dziedziczą z jednej z definicji przeglądarki, które zostały usunięte, zostanie wyświetlony błąd. Na przykład jeśli `App_Browsers` folder zawiera definicji przeglądarki, która dziedziczy IE2 definicji przeglądarki, zostanie wyświetlony następujący komunikat o błędzie dla konfiguracji:

- Nie można znaleźć elementu przeglądarki lub bramy o identyfikatorze "IE2".

> [!NOTE]
> **HttpBrowserCapabilities** obiektu (który jest udostępniany przez strony **Request.Browser** właściwości) jest wymuszany przez pliki definicji przeglądarki. W związku z tym informacje zwracane przez uzyskiwania dostępu do właściwości tego obiektu w ASP.NET 4 może różnić się od informacje zwracane w poprzednich wersjach programu ASP.NET.


Możesz przywrócić stare pliki definicji przeglądarki przez skopiowanie plików definicji przeglądarki z następującym folderze:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Skopiuj pliki do odpowiadającego `\CONFIG\Browsers` folderu dla programu ASP.NET 4. Po skopiowaniu plików, uruchom Aspnet\_regbrowsers.exe narzędzia wiersza polecenia.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll usunięte z głównym pliku konfiguracji sieci Web

W poprzednich wersjach programu ASP.NET, odwołanie do zestawu System.Web.Mobile.dll została uwzględniona w folderze głównym `Web.config` w pliku **zestawy** w obszarze. Aby zwiększyć wydajność, usunięto odwołanie do tego zestawu.

Zestaw System.Web.Mobile.dll jest dołączony do platformy ASP.NET 4, ale jest przestarzały. Jeśli chcesz użyć typów z zestawu System.Web.Mobile.dll, Dodaj odwołanie do tego zestawu, albo w katalogu głównym `Web.config` plików lub aplikacji `Web.config` pliku. Na przykład, jeśli chcesz użyć innych formantów przenośnych ASP.NET (przestarzałe), możesz należy dodać odwołanie do zestawu System.Web.Mobile.dll do `Web.config` pliku.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Weryfikacja żądania programu ASP.NET

Funkcja sprawdzania poprawności żądania w programie ASP.NET zawiera pewien poziom domyślną ochronę przed atakami skryptów między witrynami (XSS). W poprzednich wersjach programu ASP.NET Weryfikacja żądania została włączona domyślnie. Jednak zastosowane tylko do stron ASP.NET (`.aspx` plików i ich pliki klasy) i tylko gdy wykonywały tych stron.

W przypadku programu ASP.NET 4, domyślnie Weryfikacja żądania jest włączona dla wszystkich żądań, ponieważ jest ona włączona przed **powstaniem zdarzenia BeginRequest** fazy żądania HTTP. W związku z tym weryfikacji żądań ma zastosowanie do żądań dla wszystkich zasobów platformy ASP.NET, nie tylko żądania strony .aspx. W tym żądania, takie jak wywołania usługi sieci Web i niestandardowe programy obsługi HTTP. Weryfikacja żądania jest aktywny również w przypadku, gdy niestandardowe moduły HTTP są odczytu treści żądania HTTP.

W związku z tym żądań, które wcześniej nie był przyczyną błędów teraz mogą wystąpić błędy sprawdzania poprawności żądania. Aby przywrócić działanie funkcji weryfikacji żądania programu ASP.NET 2.0, Dodaj następujące ustawienie w `Web.config` pliku:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Jednak zaleca się, że analizowanie jakieś błędy sprawdzania poprawności żądania, aby określić, czy istniejących programów obsługi, modułów lub innych niestandardowych kod uzyskuje dostęp do potencjalnie niebezpiecznych danych wejściowych HTTP, które mogą być XSS ataki wektory.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Domyślne algorytmem wyznaczania wartości skrótu jest obecnie HMACSHA256

Program ASP.NET używa szyfrowania i algorytmów mieszania do ułatwia zabezpieczenie danych, takich jak pliki cookie uwierzytelniania formularzy oraz stanu widoku. Domyślnie programu ASP.NET 4 teraz używa algorytmu HMACSHA256 dla operacji skrótu na pliki cookie i stan widoku. Starsze wersje programu ASP.NET używane starsze algorytmu HMACSHA1.

Aplikacje mogą mieć one wpływ po uruchomieniu mieszane 2.0/ASP.NET ASP.NET 4 środowisk, w którym dane, takie jak pliki cookie uwierzytelniania formularzy, należy skontaktować across.NET Framework w wersji. Aby skonfigurować aplikację sieci Web programu ASP.NET 4 do używania starszej algorytmu HMACSHA1, Dodaj następujące ustawienie w `Web.config` pliku:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Błędy konfiguracji związane z nowej konfiguracji głównego 4 ASP.NET

Pliki konfiguracji głównego ( `machine.config` plików i katalog główny `Web.config` pliku) dla programu .NET Framework 4 (i w związku z tym ASP.NET 4) zostały zaktualizowane większość informacji o ASP.NET 3.5 został znaleziony w konfiguracji standardowej Aplikacja `Web.config` plików. Ze względu na złożoność zarządzanych systemach konfiguracji usług IIS 7 i IIS 7.5 uruchamiania aplikacji ASP.NET 3.5 w ASP.NET 4 oraz w usługach IIS 7 i IIS 7.5 może spowodować ASP.NET lub usługi IIS błędów konfiguracji.

Zaleca się uaktualnienie aplikacji ASP.NET 3.5 dla platformy ASP.NET 4 za pomocą narzędzia uaktualniania projektu w programie Visual Studio 2010, jeśli jest to możliwe. Program Visual Studio 2010 automatycznie modyfikuje aplikacji ASP.NET 3.5 `Web.config` plik zawiera odpowiednie ustawienia dla programu ASP.NET 4.

Jednak jest obsługiwany scenariusz do uruchamiania aplikacji ASP.NET 3.5 przy użyciu programu .NET Framework 4 bez ponownej kompilacji. W takim przypadku należy ręcznie zmodyfikować aplikacji `Web.config` plików przed uruchomieniem aplikacji w obszarze programu .NET Framework 4 i w usługach IIS 7 i IIS 7.5.

W dwóch następnych sekcjach opisano zmiany, które są potrzebne do różnych kombinacji oprogramowania.

**Windows Vista z dodatkiem SP1 lub Windows Server 2008 z dodatkiem SP1, którym są zainstalowane poprawki KB958854 ani z dodatkiem SP2.** W tej konfiguracji system konfiguracji usług IIS 7 niepoprawnie scala konfiguracji zarządzanych aplikacji na na podstawie porównania ilości na poziomie aplikacji `Web.config` plik do składnika ASP.NET 2.0 `machine.config` plików. Ze względu na to, poziomie aplikacji `Web.config` pliki z programu .NET Framework 3.5 lub nowszego muszą mieć **system.web.extensions** definicji sekcji konfiguracji (element), aby nie spowodować niepowodzenie sprawdzania poprawności IIS 7.

Jednak ręcznie zmodyfikować poziom aplikacji `Web.config` wpisy, które nie pasują dokładnie oryginalnego standardowej konfiguracji sekcji definicje, które zostały wprowadzone w programie Visual Studio 2008 spowoduje, że błędy konfiguracji ASP.NET. (Domyślnych wpisów konfiguracji, które są generowane przez program Visual Studio 2008 poprawnie działać.) Jest to powszechny problem, że ręcznie zmodyfikować `Web.config` Opuść pliki **allowDefinition** i **requirePermission** atrybuty konfiguracji, które znajdują się w różnych sekcji konfiguracyjnej definicje. Powoduje niezgodność między sekcji konfiguracji skróconej w aplikacjach na poziomie `Web.config` plików i pełnej definicji w ASP.NET 4 `machine.config` pliku. W związku z tym w czasie wykonywania, system konfiguracji programu ASP.NET 4 zgłasza błąd konfiguracji.

**Windows Vista z dodatkiem SP2, Windows Server 2008 z dodatkiem SP2, Windows 7, Windows Server 2008 R2 i również Windows Vista z dodatkiem SP1 i Windows Server 2008 SP1, na którym zainstalowano poprawkę KB958854.**

W tym scenariuszu systemu macierzystego konfiguracji usług IIS 7 i IIS 7.5 zwraca błąd konfiguracji, ponieważ wykonuje porównanie tekst na **typu** atrybut, który jest zdefiniowany dla obsługi sekcji konfiguracji zarządzanych. Ponieważ wszystkie `Web.config` pliki, które są generowane przez program Visual Studio 2008 i Visual Studio 2008 z dodatkiem SP1 ma "3.5" w parametrach typu **system.web.extensions** (i pokrewne) obsługi sekcji konfiguracji i dlatego platformy ASP.NET 4 `machine.config` w pliku jest "4.0" **typu** atrybutu dla tej samej obsługi sekcji konfiguracji, aplikacje, które są generowane w programie Visual Studio 2008 lub Visual Studio 2008 z dodatkiem SP1, zawsze niepowodzenie sprawdzania poprawności konfiguracji w usługach IIS 7 i USŁUGI IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Rozwiązywania tych problemów

Obejście dla pierwszego scenariusza jest zaktualizowanie na poziomie aplikacji `Web.config` pliku, umieszczając w niej tekstu Konfiguracja standardowego z `Web.config` pliku, który został wygenerowany automatycznie przez program Visual Studio 2008.

Inne obejście dla pierwszego scenariusza jest zainstalowanie dodatku Service Pack 2 dla Vista lub Windows Server 2008 na tym komputerze lub zainstaluj poprawkę KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) Aby rozwiązać nieprawidłowym zachowanie scalania konfiguracji systemu konfiguracyjnego usług IIS. Jednak po wykonaniu dowolnej z tych akcji aplikacji najprawdopodobniej wystąpi błąd konfiguracji z powodu problemu opisanego w scenariuszu drugiego.

Obejście drugi scenariusz polega na usuwanie lub komentarz wszystkie **system.web.extensions** definicji sekcji konfiguracji oraz sekcji konfiguracji grupy definicji z poziomu aplikacji `Web.config` pliku. Te definicje są zwykle na początku na poziomie aplikacji `Web.config` plików i mogą zostać zidentyfikowane przez **configSections** elementu i jego elementów podrzędnych.

W obu przypadkach zaleca się, że możesz również ręcznie usunąć **system.codedom** sekcji, chociaż nie jest to wymagane.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Aplikacje programu ASP.NET 4 podrzędnych nie Start w ASP.NET 2.0 lub ASP.NET 3.5 aplikacji

ASP.NET 4 aplikacje, które są skonfigurowane jako elementy podrzędne aplikacji ze starszymi wersjami programu ASP.NET może się nie uruchomić z powodu błędów kompilacji lub konfiguracji. W poniższym przykładzie przedstawiono strukturę katalogów dla aplikacji.

`/parentwebapp`(skonfigurowana do używania programu ASP.NET 2.0 lub ASP.NET 3.5)  
`/childwebapp`(skonfigurowana do używania programu ASP.NET 4)

Stosowanie w `childwebapp` folderu nie będzie można uruchomić w usługach IIS 7 i IIS 7.5 i będzie raportu błąd konfiguracji. Tekst błędu będzie zawierał komunikat podobny do następującego:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

W usługach IIS 6 do aplikacji w `childwebapp` folderu również nie zostanie uruchomiony, ale będzie zgłaszać innego błędu. Na przykład tekst błędu może zawierać następujące:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Te scenariusze wystąpić, ponieważ informacje o konfiguracji z aplikacji nadrzędnej w `parentwebapp` folder jest częścią hierarchii informacje o konfiguracji, który określa ustawienia konfiguracji końcowej scalone, które są używane przez sieci web podrzędnych Aplikacja w `childwebapp` folderu. W zależności od tego, czy jest uruchomiona aplikacja sieci Web programu ASP.NET 4, w usługach IIS 7 (lub usług IIS 7.5) lub w usługach IIS 6 system konfiguracji usług IIS lub system kompilacji platformy ASP.NET 4 spowoduje zwrócenie błędu.

Kroki, które należy wykonać, aby rozwiązać ten problem i uzyskać dziecka do działania aplikacji programu ASP.NET 4 zależą od tego, czy aplikacja ASP.NET 4 działa w usługach IIS 6 lub w usługach IIS 7 (lub usług IIS 7.5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Krok 1 (IIS 7 lub tylko IIS 7.5)

Ten krok jest niezbędny tylko w systemach operacyjnych, które są uruchamiane usługi IIS 7 i 7.5 usług IIS, w tym Windows Vista, Windows Server 2008, Windows 7 i Windows Server 2008 R2.

Przenieś **configSections** definicji w `Web.config` pliku aplikacji nadrzędnej (aplikacja ASP.NET 2.0 lub ASP.NET 3.5) w folderze głównym `Web.config` pliku dla środowiska.NET Framework 2.0. Skanuje system natywnego konfiguracji usług IIS 7 i IIS 7.5 **configSections** elementu podczas jego scala hierarchii plików konfiguracji. Przenoszenie **configSections** definicji z elementu nadrzędnego, aplikacji sieci Web `Web.config` plik do katalogu głównego `Web.config` pliku skutecznie ukrywa element z konfiguracji procesu scalania, który występuje dla obiekt podrzędny programu ASP.NET 4 aplikacja.

W 32-bitowym systemie operacyjnym lub dla pul aplikacji 32-bitowych, katalog główny `Web.config` plik do składnika ASP.NET 2.0 i programu ASP.NET 3.5 zwykle znajduje się w następującym folderze:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

W 64-bitowym systemie operacyjnym lub dla pul aplikacji 64-bitowych, katalog główny `Web.config` plik do składnika ASP.NET 2.0 i programu ASP.NET 3.5 zwykle znajduje się w następującym folderze:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Jeśli zarówno 32-bitowe i 64-bitowej aplikacji sieci Web jest uruchomiony na komputerze 64-bitowym, należy przenieść **configSections** element w górę do głównego `Web.config` plików dla 32-bitowych i 64-bitowym.

Po umieszczeniu **configSections** elementu w folderze głównym `Web.config` plików, Wklej sekcji natychmiast po **konfiguracji** elementu. W poniższym przykładzie pokazano, jakie górnej części katalogu głównego `Web.config` plik powinien wyglądać po zakończeniu przenoszenia elementów.

> [!NOTE]
> W poniższym przykładzie opakowane wierszy dla czytelności.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Krok 2 (wszystkie wersje usług IIS)

Ten krok jest wymagany, czy podrzędny programu ASP.NET 4 aplikacji sieci Web jest uruchomiony w usługach IIS 6 lub IIS 7 (lub usług IIS 7.5).

W `Web.config` Dodaj plik nadrzędnej aplikacji sieci Web, która działa 2 platformy ASP.NET lub ASP.NET 3.5 **lokalizacji** tag, który określa jawnie (dla systemów konfiguracji usług IIS i platformy ASP.NET) który tylko wpisy konfiguracji mają zastosowanie do nadrzędnej aplikacji sieci Web. W poniższym przykładzie przedstawiono składnię **lokalizacji** elementu do dodania:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

W poniższym przykładzie przedstawiono sposób **lokalizacji** tag jest stosowanego do opakowywania wszystkich sekcji konfiguracyjnych w programie **appSettings** sekcji i kończąc **system.webServer**sekcji.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Jeśli zostały wykonane kroki 1 i 2, aplikacji sieci Web programu ASP.NET 4 podrzędnych zostanie uruchomiony bez błędów.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 witryn sieci Web nie można uruchomić na komputerach, na którym jest zainstalowany program SharePoint

Serwery sieci Web, z programem SharePoint mają `Web.config` pliku, który jest wdrażany w katalogu głównym witryny sieci Web programu SharePoint (na przykład `c:\inetpub\wwwroot\web.config` dla domyślnej witryny sieci Web). W tym `Web.config` pliku, SharePoint ustawia niestandardowych częściowym zaufaniem poziomu o nazwie WSS\_minimalny.

Jeśli zostanie podjęta próba uruchomienia witryny sieci Web programu ASP.NET 4, która została wdrożona jako element podrzędny tego typu witryny sieci Web programu SharePoint, zostanie wyświetlony następujący błąd:

`Could not find permission set named 'ASP.NET'.`

Ten błąd występuje, ponieważ zestaw uprawnień o nazwie ASP.NET sprawdza infrastruktury zabezpieczeń (CAS) programu ASP.NET 4 kod dostępu. Jednak częściowego zaufania pliku konfiguracji, który odwołuje się do niego WSS\_minimalnego nie zawiera żadnych zestawów uprawnień o takiej nazwie.

Obecnie nie istnieje wersja programu SharePoint dostępne, który jest zgodny z programem ASP.NET. W związku z tym nie powinny podejmować uruchomić witryny sieci Web programu ASP.NET 4 jako lokację podrzędną poniżej sieci Web programu SharePoint witryny.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>Właściwość HttpRequest.FilePath nie zawiera już PATHINFO zawiera wartości

Poprzednie wersje programu ASP.NET uwzględnione **PATHINFO zawiera** wartość wartość zwracana z różnych związany ze ścieżką właściwości pliku, w tym **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, i **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 nie zawiera już **PATHINFO zawiera** wartości zwracanych wartości z tych właściwości. Zamiast tego **PATHINFO zawiera** informacje są dostępne w **HttpRequest.PathInfo**. Załóżmy na przykład następujące fragmentu adresu URL:

`/testapp/Action.mvc/SomeAction`

We wcześniejszych wersjach programu ASP.NET **HttpRequest** właściwości mieć następujące wartości:

**HttpRequest.FilePath**:`/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (pusta)

W przypadku programu ASP.NET 4 **HttpRequest** właściwości zamiast mieć następujące wartości:

**HttpRequest.FilePath**:`/testapp/Action.mvc`

**HttpRequest.PathInfo**:`SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>Platforma ASP.NET 2.0 aplikacji może generować błędy HttpException, które odwołują się eurl.axd

Po włączeniu programu ASP.NET 4 w usługach IIS 6, aplikacje programu ASP.NET 2.0, które działają w usługach IIS 6 (w systemie Windows Server 2003 lub Windows Server 2003 R2) może generować błędy podobne do poniższych:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Ten błąd występuje, ponieważ podczas ASP.NET wykryje, że witryny sieci Web jest skonfigurowany do używania programu ASP.NET 4, natywny składnik programu ASP.NET 4 przekazuje adresu URL bez rozszerzeń zarządzanych części ASP.NET dla dalszego przetwarzania. Jednak jeśli katalogi wirtualne, które znajdują się poniżej witryny sieci Web programu ASP.NET 4 są skonfigurowane do używania programu ASP.NET 2.0, przetwarzanie URL bez rozszerzenia w ten sposób spowoduje modyfikacji adresu URL, który zawiera ciąg "eurl.axd". Zmodyfikowane adres URL jest następnie wysyłana do aplikacji programu ASP.NET 2.0. Platforma ASP.NET 2.0 nie można rozpoznać formatu "eurl.axd". W związku z tym, ASP.NET 2.0 próbuje odnaleźć plik o nazwie `eurl.axd` i wykonaj go. Ponieważ taki plik nie istnieje, żądanie kończy się niepowodzeniem z **HttpException** wyjątku.

Można obejść ten problem przy użyciu jednej z następujących opcji.

### <a name="option-1"></a>Opcja 1

Jeśli programu ASP.NET 4 nie jest wymagana, aby można było uruchomić witrynę sieci Web, należy ponownie zamapować lokacji zamiast tego użyć programu ASP.NET 2.0.

### <a name="option-2"></a>Opcja 2

Jeśli programu ASP.NET 4 jest wymagany w celu uruchamiania witryny sieci Web, należy przenieść wszystkie katalogi wirtualne ASP.NET 2.0 podrzędnych do innej witryny sieci Web, który jest zamapowany na platformie ASP.NET 2.0.

### <a name="option-3"></a>Opcja 3

Jeśli nie jest praktyczne, aby ponownie zamapować witryny sieci Web programu ASP.NET 2.0 lub aby zmienić lokalizację katalogu wirtualnego, jawnie Wyłącz URL bez rozszerzenia przetwarzania w ASP.NET 4. Użyj następującej procedury:

1. W rejestrze systemu Windows należy otworzyć następującego węzła:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Utwórz nową **DWORD** wartość o nazwie **EnableExtensionlessUrls**.
2. Ustaw **EnableExtensionlessUrls** na 0. Powoduje wyłączenie zachowanie adresów URL bez rozszerzenia.
3. Zapisywanie wartości rejestru, a następnie zamknij Edytor rejestru.
4. Uruchom **iisreset** narzędzie wiersza polecenia, co powoduje, że usługi IIS do nowej wartości rejestru do odczytu.

> [!NOTE]
> Ustawienie **EnableExtensionlessUrls** 1 umożliwia zachowanie adresów URL bez rozszerzenia. To ustawienie domyślne, jeśli nie określono wartości.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Programy obsługi zdarzeń nie może zostać nie wywołane w dokumentu domyślnego w usługach IIS 7 lub usług IIS 7.5 zintegrowane tryb

Program ASP.NET 4 zawiera zmiany, które zmienić sposób **akcji** atrybutu HTML **formularza** element jest renderowany, gdy adres URL bez rozszerzeń jest rozpoznawana jako dokument domyślny. Oto przykład adres URL bez rozszerzeń rozpoznawania dokument domyślny [http://contoso.com/](http://contoso.com/), co w żądaniu skierowanym do [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).

Platforma ASP.NET 4 teraz renderuje kod HTML **formularza** elementu **akcji** wartość atrybutu jako pustego ciągu, po wysłaniu żądania bez rozszerzenia adresu URL, który ma do niej przyporządkowany dokument domyślny. Na przykład we wcześniejszych wersjach programu ASP.NET, żądanie [http://contoso.com](http://contoso.com) spowodowałoby żądanie `Default.aspx`. W tym dokumencie, otwarcie **formularza** tag będzie renderowany jak w poniższym przykładzie:

`<form action="Default.aspx" />`

W ASP.NET 4 żądanie [http://contoso.com](http://contoso.com) także wyników w żądaniu skierowanym do `Default.aspx`. Jednak ASP.NET teraz powoduje otwarcie HTML **formularza** tag, jak w poniższym przykładzie:

`<form action="" />`

Różnica w sposobie **akcji** atrybut jest odwzorowywany może spowodować niewielkie zmiany w przetwarzaniu post formularza przez usługi IIS i platformy ASP.NET. Gdy **akcji** atrybut jest pusty ciąg, usługi IIS **DefaultDocumentModule** obiekt utworzy żądania podrzędnego `Default.aspx`. W większości warunkach, jest niewidoczny dla kodu aplikacji, to żądania podrzędnego i `Default.aspx` strona działa normalnie.

Potencjalne interakcji między kodu zarządzanego i IIS 7 i IIS 7.5 zintegrowanego trybu może jednak spowodować strony .aspx zarządzanych do przestają działać prawidłowo podczas żądania podrzędnego. Jeśli występują następujące warunki, żądania podrzędnego do `Default.aspx` dokumentu spowoduje błąd lub nieoczekiwane zachowanie:

1. Strony .aspx jest wysyłany do przeglądarki z **formularza** elementu **akcji** określić dla atrybutu "".
2. Powrót do ASP.NET opublikowania formularza.
3. Moduł zarządzany HTTP odczytuje część treści jednostki. Na przykład moduł odczytuje **Request.Form** lub **Request.Params**. Powoduje to treści jednostki żądania POST do odczytu do pamięci zarządzanej. W związku z tym treści jednostki nie jest już dostępny do wszelkich modułów kodu natywnego, które działają w trybie IIS 7 lub usług IIS 7.5 zintegrowanego.
4. Usługi IIS **DefaultDocumentModule** obiektu ostatecznie działa i tworzy żądanie podrzędne `Default.aspx` dokumentu. Jednakże ponieważ została już odczytana treść jednostki przez fragment kodu zarządzanego, brak treści jednostki dostępne do wysyłania do żądania podrzędnego.
5. Po uruchomieniu potoku HTTP dla żądania podrzędnego, program obsługi dla `.aspx` plików jest uruchamiana w fazie handler-execute.
6. Ponieważ nie ma żadnych treść jednostki, istnieją żadnych zmiennych formularza i bez stanu widoku, a w związku z tym nie ma dostępnych informacji dla obsługi strony .aspx ustalić, jakie zdarzenia (jeśli istnieje) powinien być wywoływane. W związku z tym brak obsługi zdarzenia odświeżania strony dla strony .aspx dotyczy Uruchom.

Ten problem można obejść, w następujący sposób:

- Zidentyfikować moduł protokołu HTTP, który uzyskuje dostęp do treści jednostki żądania podczas żądań dokumentu domyślnego, aby ustalić, czy może być skonfigurowana do uruchamiania tylko dla żądań zarządzanych. W trybie zintegrowanym dla usług IIS 7 i IIS 7.5, moduły HTTP może być oznaczony do uruchamiania tylko dla żądań zarządzanych przez dodanie następującego atrybutu do modułu **lokalizacji system.webServer/modules** wpis:

- `precondition="managedHandler"`

- To wyłącza ustawienie modułu dla żądań, że usług IIS 7 i IIS 7.5 określić jako nie jest zarządzane żądania. Pierwsze żądanie żądań dokumentu domyślnego, jest adres URL bez rozszerzenia. W związku z tym usługi IIS nie jest uruchomiony zarządzanych modułów, które są oznaczone o warunku wstępnego zarządzanego programu obsługi podczas przetwarzania żądania początkowego. W związku z tym modułów zarządzanych nie zostaną przypadkowo odczytać treści jednostki i w związku z tym treść jednostki, jest nadal dostępny i jest przekazywane do żądania podrzędnego i dokument domyślny.

- Jeśli problemy moduły HTTP ma potrzeby uruchamiania dla wszystkich żądań (dotyczących plików statycznych dla adresów URL bez rozszerzenia, które rozpoznają **DefaultDocumentModule** obiektu zarządzanego żądania, itp.), jawnie modyfikować strony .aspx wykorzystywanych przez ustawienie **akcji** właściwości strony **System.Web.UI.HtmlControls.HtmlForm** formantu niepustym ciągiem. Na przykład, jeśli dokument domyślny jest `Default.aspx`, modyfikować kodu strony jawnie ustaw **HtmlForm** formantu **akcji** dla właściwości "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Zmiany w implementacji zabezpieczeń (CAS) dostępu do kodu platformy ASP.NET

Program ASP.NET 2.0 i przez rozszerzenie funkcje platformy ASP.NET, które zostały dodane w wersji 3.5, użyj programu .NET Framework 1.1 i model zabezpieczeń (CAS) 2.0 kod dostępu. Jednak implementacja urzędy certyfikacji w ASP.NET 4 została znacząco overhauled. W związku z tym aplikacji ASP.NET częściowego zaufania, które zależą od zaufanego kodu uruchamianego w globalnej pamięci podręcznej zestawów (GAC) może zakończyć się niepowodzeniem z różnych wyjątki zabezpieczeń. Aplikacje częściowo zaufane, które opierają się na rozległych modyfikacji na komputerze zasad CAS może również zakończyć się niepowodzeniem z wyjątkami związanymi z zabezpieczeniami.

Możesz przywrócić częściowego zaufania aplikacji programu ASP.NET 4 do zachowania ASP.NET 1.1 i 2.0 przy użyciu nowego **legacyCasModel** atrybutu w **zaufania** element konfiguracji, jak pokazano w poniższym przykładzie :

`<trust level= "Medium" legacyCasModel="true" />`

Podczas przywracania starszej wersji modelu urzędów certyfikacji, są włączone następujące starego zachowania urzędów certyfikacji:

- Zasady CAS maszyny jest przestrzegana.
- Wiele zestawów różne uprawnienia w domenie, w jednej aplikacji są dozwolone.
- Potwierdzenia jawne uprawnienia nie są wymagane dla zestawów w pamięci podręcznej GAC, które są wywoływane, gdy tylko ASP.NET lub inny kod .NET Framework jest na stosie.

Jednym ze scenariuszy nie można przywrócić w .NET Framework 4: aplikacje częściowo zaufane Web nie można wywołać niektórych interfejsów API w System.Web.dll i System.Web.Extensions.dll. W poprzednich wersjach programu .NET Framework został-Web częściowo zaufane aplikacje mogą być jawnie przyznane uprawnienia **AspNetHostingPermission** uprawnienia. Następnie można użyć tych aplikacji **System.Web.HttpUtility**, typy w **System.Web.ClientServices.\***  obszary nazw i typy związanych z członkostwo i role oraz profile. Wywoływanie tych typów z aplikacji sieci Web częściowej relacji zaufania nie jest już obsługiwana w .NET Framework 4.

> [!NOTE]
> **HtmlEncode** i **HtmlDecode** funkcjonalność **System.Web.HttpUtility** klasy została przeniesiona do nowego .NET Framework 4  **System.Net.WebUtility** klasy. Jeśli który jest używany tylko funkcje platformy ASP.NET, modyfikowania kodu aplikacji do używania nowych **WebUtility** zamiast klasy.


Poniżej znajduje się podsumowanie wysokiego poziomu zmian, aby Domyślna implementacja urzędów certyfikacji w ASP.NET 4:

- Domeny aplikacji ASP.NET są teraz domeny jednorodnych aplikacji. Tylko częściowo zaufane i pełnego zaufania grant zestawy są dostępne w domenie aplikacji.
- ASP.NET grant częściowo zaufane zestawy są niezależne od zasad urzędy certyfikacji przedsiębiorstw, poziom maszyny lub na poziomie użytkownika.
- Zestawy ASP.NET, które zostały wydane w 3.5 i 3.5 z dodatkiem SP1 zostały przekonwertowane do korzystania z modelu przezroczystości .NET Framework 4.
- Korzystanie z platformy ASP.NET **AspNetHostingPermission** znacznie zmniejszyć atrybutu. Większość wystąpień tego atrybutu zostały usunięte z publiczne interfejsy API programu ASP.NET.
- Dynamicznie skompilowane zestawy, które zostały utworzone przy użyciu dostawcy kompilacji platformy ASP.NET zostały zaktualizowane do jawnie Oznacz zestawy jako przezroczysty.
- Wszystkie zestawy ASP.NET zostały oznaczone w taki sposób, że atrybut APTCA jest honorowane tylko w środowiskach hostingu w sieci Web. Częściowo zaufany środowiskach hostingu sieci Web takich jak ClickOnce nie będzie wywołują zestawów ASP.NET.

Aby uzyskać więcej informacji o nowy model zabezpieczeń dostępu kodu platformy ASP.NET 4, zobacz [przy użyciu zabezpieczenia dostępu kodu w aplikacjach ASP.NET](https://msdn.microsoft.com/en-us/library/dd984947%28VS.100%29.aspx) w witrynie MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser i innych typów w Namespace System.Web.Security został przeniesiony

Niektóre typy, które są używane w członkostwie w ASP.NET zostały przeniesione z `System.Web.dll` do nowego zestawu System.Web.ApplicationServices.dll. Typy zostało przeniesione, aby rozpoznać zależności warstwy architektury między typami w klienta i rozszerzone .NET Framework w wersji produktu.

Projektów witryny sieci Web nie ma problemów wyniku przeniesienie tych typów, ponieważ System.Web.ApplicationServices.dll została dodana do listy zestawów występujących w odwołaniach używany domyślnie przez system kompilacji programu ASP.NET. Jeżeli uaktualnisz projekt witryny sieci Web, który został utworzony przy użyciu starszej wersji programu ASP.NET lub ASP.NET 4, otwierając go w programie Visual Studio 2010, projekt zostanie skompilowany bez błędów.

Podobnie w przypadku uaktualniania projektu aplikacji sieci Web, który został utworzony we wcześniejszej wersji programu ASP.NET lub ASP.NET 4, otwierając go w programie Visual Studio 2010 proces uaktualniania dodaje do projektu odwołanie do System.Web.ApplicationServices.dll. W związku z tym uaktualnienia sieci Web, projektów aplikacji również zostanie skompilowany bez błędów.

Skompilowane pliki (binarnego), które zostały utworzone we wcześniejszych wersjach programu ASP.NET zostanie też uruchomiony bez błędów na platformy ASP.NET 4, mimo że typów członkostwa zostały przeniesione do innego zestawu. Przekazywanie informacji typ został dodany do wersji platformy ASP.NET 4 `System.Web.dll` który automatycznie rozsyła odwołania do środowiska wykonawczego dla tych typów do nowej lokalizacji dla typów.

Jednak bibliotek klas używający typów określonych członkostwa i uaktualnienia z wcześniejszej wersji platformy ASP.NET zakończy się niepowodzeniem skompilować, gdy jest używany w projekcie platformy ASP.NET 4. Na przykład projektu biblioteki klas może zakończyć się niepowodzeniem do kompilowania i zgłoś błąd, takie jak następujące:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Ten problem można obejść przez dodanie odwołania do System.Web.ApplicationServices.dll Twojego projektu biblioteki klas.

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

## <a name="output-caching-changes-to-vary--http-header"></a>Dane wyjściowe buforowania zmiany różnią się \* nagłówka HTTP

W programie ASP.NET 1.0 spowodował błąd w pamięci podręcznej stron, które określono `Location="ServerAndClient"` jako ustawienie — pamięci podręcznej danych wyjściowych, aby emitować `Vary:*` nagłówka HTTP w odpowiedzi. To w efekcie informuje przeglądarki klienta nigdy nie buforowania strony lokalnie.

W ASP.NET 1.1 **System.Web.HttpCachePolicy.SetOmitVaryStar** metody został dodany, który można wywołać, aby pominąć `Vary:*` nagłówka. Ta metoda została wybrana, ponieważ zmiana emitowany nagłówka HTTP została uznana za potencjalnie fundamentalne zmiany w czasie. Jednak deweloperzy mają zastanawia zachowanie w programie ASP.NET i umieszczania raportów o błędach wskazujące, deweloperzy i znają istniejącej **SetOmitVaryStar** zachowanie.

W technologii ASP.NET 4 podjęto decyzję, aby rozwiązać ten problem głównego. `Vary:*` Nagłówka HTTP nie jest emitowany z odpowiedzi, które określają następujące dyrektywy:

`<%@OutputCache Location="ServerAndClient" %>`

W związku z tym **SetOmitVaryStar** jest już potrzebne, aby pominąć `Vary:*` nagłówka.

W aplikacjach, które określają `Location="ServerAndClient"` w **@ OutputCache** dyrektywy na stronie, zostanie wyświetlona zachowanie niejawnego o nazwie **lokalizacji** wartość atrybutu —, będzie stron w przeglądarce bez konieczności wywołania buforowalnej **SetOmitVaryStar** metody.

Jeśli na stronach aplikacji należy Emituj `Vary:*`, wywołaj **AppendHeader** metody, jak w poniższym przykładzie:

`HttpResponse.AppendHeader("Vary","*");`

Alternatywnie można zmienić wartości buforowanie danych wyjściowych **lokalizacji** atrybutu "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Typy System.Web.Security na potrzeby usługi Passport jest przestarzałe

Obsługa usługi Passport wbudowanych w programie ASP.NET 2.0 została obsługiwany przez kilka lat z powodu zmian w Passport (teraz identyfikator Live ID). W związku z tym pięć typów związane z usługą Passport w **System.Web.Security** teraz są oznaczone ikoną z **ObsoleteAttribute** atrybutu.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Właściwość MenuItem.PopOutImageUrl nie powiedzie się, by renderować obraz w programie ASP.NET 4

W programie ASP.NET 3.5 *MenuItem.PopOutImageUrl* właściwości pozwala określić adres URL obrazu, który jest wyświetlany w elemencie menu, aby wskazać, czy element menu ma dynamiczne podmenu. Poniższy przykład pokazuje, jak można określić tę właściwość w znaczniku w programie ASP.NET 3.5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

W wyniku zmiany projektu programu ASP.NET 4, żadne dane wyjściowe nie jest renderowany *PopOutImageUrl* Jeśli ustawiona jest właściwość *MenuItem* klasy. Zamiast tego należy określić adres URL obrazu, bezpośrednio w *Menu* kontrolować za pomocą *StaticPopOutImageUrl* właściwości lub *DynamicPopOutImageUrl* właściwości. Podczas pracy z menu statyczne, *Menu.StaticPopOutImageUrl* właściwość określa adres URL obrazu, który jest wyświetlany w celu wskazania, że element menu statyczne ma podmenu, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Jeśli pracujesz z menu dynamiczne, możesz użyć *Menu.DynamicPopOutImageUrl* właściwość, aby określić adres URL obrazu, który wskazuje, że element menu dynamiczne ma podmenu. W poniższym przykładzie jest podobny do poprzedniego, ale przedstawiono sposób ustawiania *DynamicPopOutImageUrl* właściwości dynamicznej menu.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Jeśli *Menu.DynamicPopOutImageUrl* nie ustawiono właściwości i *Menu.DynamicEnableDefaultPopOutImage* właściwość jest ustawiona na *false*, obraz nie jest wyświetlany. Podobnie jeśli *StaticPopOutImageUrl* nie ustawiono właściwości i *StaticEnableDefaultPopOutImage* właściwość jest ustawiona na *false*, obraz nie jest wyświetlany.

Po ustawieniu ścieżki tych właściwości, należy użyć ukośnika (/) zamiast ukośnik odwrotny (\). Aby uzyskać więcej informacji, zobacz [Menu.StaticPopOutImageUrl i nie są Menu.DynamicPopOutImageUrl renderowanie obrazów podczas ścieżki zawierać ukośników odwrotnych](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") w tym dokumencie.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl i Menu.DynamicPopOutImageUrl się nie powieść do realizacji obrazów, gdy ścieżki zawierać ukośników odwrotnych

W przypadku platformy ASP.NET 4, obrazów, które można określić za pomocą *Menu.StaticPopOutImageUrl* i *Menu.DynamicPopOutImageUrl* właściwości nie będzie zwracał, jeśli ścieżka zawiera backlashes (\). Jest to zmiana z wcześniejszych wersji programu ASP.NET.

Poniższy przykład przedstawia *Menu* kontrolować pokazuje znaczników *StaticPopOutImageUrl* właściwość przy użyciu ścieżki, która zawiera ukośnik odwrotny. W programie ASP.NET 4 nie będzie zwracał określonego we właściwości obrazu.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Aby rozwiązać ten problem, zmień wartości ścieżki, które są określone w *StaticPopOutImageUrl* i *DynamicPopOutImageUrl* właściwości, aby używać ukośnikami (/). W poniższym przykładzie przedstawiono tę zmianę:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Zauważ, że aplikacje, które zostały zmigrowane z wcześniejszych wersji programu ASP.NET lub ASP.NET 4 może również być zmienia się, ponieważ *MenuItem.PopOutImageUrl* zmieniono właściwość. Aby uzyskać więcej informacji, zobacz [MenuItem.PopOutImageUrl właściwości nie może renderować obraz w ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") w innym miejscu w tym dokumencie.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

To jest dokument wstępny i może znacznie zmienione przed końcowej wersji oprogramowania opisanych tutaj.

Informacje zawarte w tym dokumencie reprezentuje bieżący widok elementu firmy Microsoft Corporation na zagadnień omówionych w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki na rynku, nie powinny być rozumiane jako zobowiązanie ze strony firmy Microsoft i firma Microsoft nie może zagwarantować dokładności informacji po opublikowaniu.

Ten dokument jest tylko do celów informacyjnych. MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH LUB USTAWOWYCH, ODNOŚNIE DO INFORMACJI W TYM DOKUMENCIE.

Zgodne z wszystkich stosownych praw autorskich jest odpowiedzialny za użytkownika. Bez ograniczenia praw autorskich, żadnej części tego dokumentu może być odtworzyć, przechowywane w wprowadzane do systemu pobierania lub przekazywać w jakimkolwiek formularzu za pomocą jakichkolwiek metod (elektronicznych, mechanicznych, postaci kopii, nagrań lub innych) w jakimkolwiek celu, bez pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może mieć patenty, patentowe, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej będących przedmiotem tego dokumentu. Z wyjątkiem wyraźnie określonych w umowach licencyjnych firmy Microsoft, otrzymanie tego dokumentu nie oznacza udzielenia licencji na te patenty, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej.

Jeśli nie podano inaczej, przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia opisywane w przykładach są fikcyjne i żadne skojarzenia z rzeczywistymi firmami, organizacji, produktu, nazwa domeny, poczty e-mail adres, logo, osoby, miejsca lub zdarzeń jest przeznaczone lub powinny być zakładane.

© 2010 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stanach Zjednoczonych i/lub innych krajach.

Nazwy firm i produktów wymienione w niniejszym dokumencie mogą być znakami towarowymi ich prawnych właścicieli.
