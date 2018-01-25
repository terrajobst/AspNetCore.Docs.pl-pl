---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Zapobieganie XSRF/CSRF w platformie ASP.NET MVC i stron sieci Web | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Sfałszowaniem żądania między witrynami (znanej także jako XSRF lub CSRF) jest atak wykorzystujący aplikacje obsługiwane w sieci web, zgodnie z którymi złośliwa witryna sieci web może mieć wpływ interakcyjne..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 6cf30daa7ed966b11405cec715c5bc803b567249
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Zapobieganie XSRF/CSRF w platformie ASP.NET MVC i stron sieci Web
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Żądania międzywitrynowego sfałszowaniem (znanej także jako XSRF lub CSRF) jest atak wykorzystujący aplikacje obsługiwane w sieci web, zgodnie z którymi złośliwa witryna sieci web może mieć wpływ interakcji między przeglądarką klienta i witryny sieci web ufa tej przeglądarki. Tego rodzaju ataki są możliwe, ponieważ przeglądarki sieci web do witryny sieci web wysyła tokeny uwierzytelniania automatycznie z każdym żądaniem. Canonical przykładem jest pliku cookie uwierzytelniania, takich jak ASP. Biletu uwierzytelniania formularzy w sieci. Jednak tego rodzaju ataki może być celem witryn sieci web, korzystających z dowolnego mechanizmu uwierzytelniania trwałe (takich jak uwierzytelnianie systemu Windows, Basic i tak dalej).
> 
> Atak XSRF różni się od ataku wyłudzaniem informacji. Wyłudzania wymagają interakcji z ofiary. Atak phishing złośliwa witryna sieci web będzie naśladować docelowej witryny sieci web i ofiary jest fooled na dostarczanie informacji poufnych dla osoby atakującej. W atak XSRF nie ma często bez interakcji z ofiary niezbędne. Osoba atakująca jest raczej jednostki uzależnionej w przeglądarce, wszystkie odpowiednie pliki cookie są automatycznie wysyłane do docelowej witryny sieci web.
> 
> Aby uzyskać więcej informacji, zobacz [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomia atak

Przechodzenia przez atak XSRF, należy wziąć pod uwagę użytkownika, który chce wykonać niektóre transakcje bankowość online. Ten użytkownik odwiedza najpierw WoodgroveBank.com i dzienniki, po czym nagłówka odpowiedzi będzie zawierać jej pliku cookie uwierzytelniania:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Ponieważ plik cookie uwierzytelniania jest plik cookie sesji, jego zostanie automatycznie usunięty przez przeglądarkę, gdy proces przeglądarki kończy się. Jednak do tego czasu, przeglądarka automatycznie będzie zawierać plik cookie z każdym żądaniem do WoodgroveBank.com. Użytkownik chce teraz transfer 1000 USD na inne konto, dzięki czemu użytkownik wypełnia formularz na stronie bankowości i przeglądarka wysyła żądanie do serwera:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Ponieważ ta operacja ma efekt uboczny (go inicjuje transakcję finansowe), lokacji bankowości włączył wymagają HTTP POST do zainicjowania tej operacji. Serwer otrzymuje token uwierzytelniania w żądaniu, wyszukuje numer konta bieżącego użytkownika, sprawdza, czy istnieje nałożone, a następnie inicjuje transakcję pod uwagę docelowego.

Jej bankowość online pełną, użytkownik przechodzi witryny bankowości i odwiedza innych lokalizacjach w sieci web. Jeden z tych lokacji — fabrikam.com — zawiera następujący kod znaczników na stronie osadzone w &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Co powoduje, że przeglądarka do zgłoszenia tego żądania:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Osoba atakująca wykorzystuje fakt, że użytkownik nadal może być token uwierzytelniania prawidłowy dla docelowej witryny sieci web, a użytkownik używa małych fragment kodu języka JavaScript w przeglądarce automatycznie wprowadzić HTTP POST do lokacji docelowej. Jeśli token uwierzytelniania jest nadal ważny, lokacji bankowości zainicjuje przeniesienia 250 USD przeznaczony na konto Wybieranie osoby atakującej.

### <a name="ineffective-mitigations"></a>Środki zaradcze żadnego efektu

Jest interesujące należy pamiętać, że w powyższym scenariuszu fakt, że WoodgroveBank.com został uzyskiwany za pomocą protokołu SSL i miał pliku cookie uwierzytelniania tylko do protokołu SSL jest za mało, aby zablokować próby ataku. Osoba atakująca jest w stanie określić [schemat identyfikatora URI](http://en.wikipedia.org/wiki/URI_scheme) (https) w jej &lt;formularza&gt; element i przeglądarka będzie kontynuować wysyłanie niewygasłe plików cookie do lokacji docelowej tak długo, jak te pliki cookie są zgodne z identyfikatorem URI schemat z docelową.

Co można argumentowało, że użytkownik nie po prostu odwiedzić niezaufanych witryn, jako odwiedzający tylko zaufane witryny jest pozwala pozostały bezpieczne w trybie online. Brak niektórych prawdy do tego, ale Niestety te porady nie zawsze jest praktyczne. Prawdopodobnie użytkownik "ufa" lokacji lokalnych wiadomości ConsolidatedMessenger. ConsolidatedMessenger.com i umieszczane odwiedź stronę, która zamiast lokacji, ale w tej lokacji ma XSS luka, która umożliwia osobie atakującej iniekcję tego samego fragmentu kodu, która była uruchomiona na fabrikam.com.

Możesz sprawdzić, czy żądania przychodzące mają [nagłówka Odnośnik](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) odwołuje się do domeny. Spowoduje to zatrzymanie żądań przesłanych nieświadomie z domeny innej firmy. Jednak niektóre osoby wyłączyć nagłówka Odnośnik ich przeglądarki przyczyn prywatności i osoby atakujące czasami może spreparować nagłówka, jeśli ofiary zainstalowano określone oprogramowanie niebezpieczne. Weryfikowanie [nagłówka Odnośnik](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) nie jest uznawane za bezpieczne podejście do zapobieganie atakom XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Środki zaradcze XSRF środowiska uruchomieniowego stosu sieci Web

Środowisko uruchomieniowe stosu sieci Web platformy ASP.NET używa wariant [wzorzec tokenu Synchronizatora](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) obrony przed atakami XSRF. Ogólne formularz wzorca tokenu Synchronizatora jest, że dwa tokeny anti-XSRF są przesyłane do serwera za pomocą każdej metody POST protokołu HTTP (oprócz token uwierzytelniania): jednego tokenu pliku cookie, a drugi jako wartości formularza. Wartości tokenów generowane przez moduł wykonawczy platformy ASP.NET nie są deterministyczne lub prognozowanych przez osobę atakującą. Gdy tokeny są przesyłane, serwer będzie zezwalać na żądania kontynuować tylko wtedy, gdy oba tokeny przekazać wyboru porównania.

Weryfikacja żądania XSRF *tokenu sesji* jest przechowywana jako plik cookie HTTP i obecnie zawiera następujące informacje w jego ładunku:

- Token zabezpieczający, składające się z losowy identyfikator 128-bitowego.   
 Na poniższej ilustracji przedstawiono tokenu sesji weryfikacji żądania XSRF wyświetlane przy użyciu narzędzi deweloperskich programu Internet Explorer F12: (Uwaga jest bieżąca implementacja i podlega, nawet prawdopodobnie ulegnie zmianie.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*Token pola* jest przechowywana jako `<input type="hidden" />` i zawiera następujące informacje w jego ładunku:

- Nazwa zalogowanego użytkownika (Jeśli uwierzytelnienie).
- Wszelkie dodatkowe dane dostarczone przez [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Ładunki tokenów anti-XSRF są zaszyfrowana i podpisana, więc nie można wyświetlić nazwy użytkownika podczas korzystania z narzędzi do sprawdzenia tokenów. Gdy aplikacja sieci web jest docelowo ASP.NET 4.0, usługi kryptograficzne są dostarczane przez [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) procedury. Gdy aplikacji sieci web jest docelowo ASP.NET 4.5 lub nowszej, kryptograficzne usługi są dostarczane przez [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) procedury, która oferuje lepszą wydajność, rozszerzeń i zabezpieczeń. Można znaleźć w następującym wpisie zapisuje więcej szczegółów:

- [Ulepszenia usług kryptograficznych w programie ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Ulepszenia usług kryptograficznych w programie ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Ulepszenia usług kryptograficznych w programie ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generowanie tokenów

Do generowania tokenów anti-XSRF, wywołaj [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) metody z widoku MVC lub @AntiForgery.GetHtml() na stronie aparatu Razor. Środowisko uruchomieniowe zostanie następnie wykonaj następujące czynności:

1. Jeśli bieżące żądanie HTTP zawiera już token anti-XSRF sesji (plik cookie anti-XSRF \_ \_RequestVerificationToken), token zabezpieczający jest wyodrębniana z niego. Żądanie HTTP nie zawiera tokenu anti-XSRF sesji lub nie powiodło się wyodrębnienie tokenu zabezpieczającego, zostanie wygenerowany nowy token anti-XSRF losowych.
2. Token anti-XSRF pola jest generowana z użyciem tokenu zabezpieczającego z powyższym kroku (1) i tożsamości bieżącego zalogowanego użytkownika. (Aby uzyskać więcej informacji na temat określania tożsamości użytkownika, zobacz  **[scenariusze z obsługą specjalne](#_Scenarios_with_special)**  poniżej.) Ponadto jeśli [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) jest skonfigurowany, wywoła środowisko uruchomieniowe jego [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) — metoda i Dołącz zwracany ciąg token pola. (Zobacz  **[konfiguracji i rozszerzalność](#_Configuration_and_extensibility)**  sekcji, aby uzyskać więcej informacji.)
3. Jeśli nowy token anti-XSRF został wygenerowany w kroku (1), token nowej sesji zostanie utworzony dla niej i zostanie dodany do kolekcji wychodzące pliki cookie HTTP. Token pola w kroku (2) zostaną opakowane w `<input type="hidden" />` elementu, a ten kod znaczników HTML będzie zwracana wartość `Html.AntiForgeryToken()` lub `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Sprawdzanie poprawności tokenów

Do weryfikacji tokenów przychodzących anti-XSRF, deweloper obejmuje [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) atrybutu jej MVC akcji lub kontrolera lub wywołań tych `@AntiForgery.Validate()` ze swojej strony Razor. Środowisko uruchomieniowe zostaną wykonane następujące kroki:

1. Przychodzące tokenu sesji i token pola są odczytywane i tokenu anti-XSRF wyodrębniony z każdej. Tokeny anti-XSRF muszą być identyczne na każdym etapie (2) w procedurze generacji.
2. Jeśli bieżący użytkownik jest uwierzytelniony, jego nazwa użytkownika jest porównywana z przechowywanych w tokenie pole nazwy użytkownika. Nazwy użytkowników muszą być zgodne.
3. Jeśli [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) jest skonfigurowany, wywołania środowiska uruchomieniowego jego *ValidateAdditionalData* metody. Metoda musi zwracać wartość logiczną *true*.

Jeśli weryfikacja zakończy się powodzeniem, żądanie może kontynuować. Jeśli weryfikacja zakończy się niepowodzeniem, spowoduje zgłoszenie platformę *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Warunki awarii

Począwszy od środowiska uruchomieniowego stosu sieci Web ASP.NET w wersji 2 żadnych *HttpAntiForgeryException* który jest zgłoszony podczas sprawdzania poprawności zawierają szczegółowe informacje na temat co poszło źle. Obecnie zdefiniowane awarię są:

- Tokenu sesji lub tokenu formularza nie jest obecne w żądaniu.
- Nie będzie można odczytać tokenu sesji lub tokenu formularza. Najbardziej prawdopodobną przyczyną jest to farma niezgodnej wersji środowiska uruchomieniowego stosu sieci Web ASP.NET lub farmy gdzie &lt;machineKey&gt; elementu w pliku Web.config różni się między komputerami. Aby wymusić tego wyjątku przez manipulowanie albo tokenu anti-XSRF, można użyć narzędzia, takiego jak Fiddler.
- Token sesji i token pola zostały zamienione.
- Tokenu sesji i token pola zawiera niedopasowane tokenów.
- Osadzone w tokenie pole Nazwa użytkownika jest niezgodna z bieżącego zalogowanego użytkownika.
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)*  metoda zwróciła *false*.

Urządzenia anti-XSRF może także przeprowadzić dodatkowe sprawdzenie podczas generowania tokenów lub sprawdzania poprawności i może spowodować błędy podczas kontroli wyjątki są zgłaszane. Zobacz [WIF / ACS / opartego na oświadczeniach uwierzytelniania](#_WIF_ACS) i  **[konfiguracji i rozszerzalność](#_Configuration_and_extensibility)**  sekcjach, aby uzyskać więcej informacji.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scenariusze z obsługą specjalne

### <a name="anonymous-authentication"></a>Uwierzytelnianie anonimowe

System anti-XSRF zawiera specjalne pomocy technicznej dla użytkowników anonimowych, gdzie "anonymous" jest zdefiniowany jako użytkownik gdzie *IIdentity.IsAuthenticated* zwraca *false*. Scenariusze obejmują zapewnienie ochrony XSRF do strony logowania (przed uwierzytelnieniem użytkownika) i schematy uwierzytelniania niestandardowego, gdzie aplikacja korzysta z mechanizmu innych niż *tożsamości* do identyfikowania użytkowników.

Do obsługi tych scenariuszy, odwołanie, że tokeny sesji i pola są sprzęgane przy token zabezpieczający, który jest identyfikatorem nieprzezroczyste losowo generowany 128-bitowego. Ten token zabezpieczający służy do śledzenia sesji użytkownika w lokacji, użytkownik przechodzi więc skutecznie pełni celem anonimowy identyfikator. Pusty ciąg jest używany zamiast nazwy użytkownika dla procedury generowania i sprawdzania poprawności, opisane powyżej.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / opartego na oświadczeniach uwierzytelniania

Zwykle *tożsamości* klasy wbudowanej w programie .NET Framework mają właściwość który *IIdentity.Name* jest wystarczająca do unikatowego identyfikowania użytkownika określonego w ramach określonej aplikacji. Na przykład *FormsIdentity.Name* zwraca nazwę użytkownika przechowywane w bazie danych członkostwa (która jest unikatowa dla wszystkich aplikacji, w zależności od tego, że baza danych), *WindowsIdentity.Name* zwraca kwalifikowana domeny tożsamości użytkownika i tak dalej. Te systemy zapewnia nie tylko uwierzytelniania; one również *zidentyfikować* użytkowników aplikacji.

W przypadku uwierzytelniania opartego na oświadczeniach z drugiej strony, nie wymaga identyfikujący określonego użytkownika. Zamiast tego *ClaimsPrincipal* i *ClaimsIdentity* typy są skojarzone z zestawem *oświadczeń* wystąpienia, gdzie poszczególne oświadczeń może być "jest lat 18 +" lub " jest administratorem"inaczej. Ponieważ użytkownik nie musi identyfikowany, nie można użyć środowiska uruchomieniowego *ClaimsIdentity.Name* właściwość jako unikatowy identyfikator dla tego użytkownika. Zespół widział przykłady rzeczywistych gdzie *ClaimsIdentity.Name* zwraca *null*, zwraca nazwę przyjazny (wyświetlanie) lub w przeciwnym razie zwraca wartość typu ciąg, które nie są odpowiednie do użycia jako unikatowy identyfikator dla użytkownika.

Wiele wdrożeń, które korzystają z uwierzytelniania opartego na oświadczeniach za pomocą [usłudze kontroli dostępu platformy Azure](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) w szczególności. ACS umożliwia deweloperowi konfigurowania poszczególnych *dostawców tożsamości* (np. ADFS, dostawcy Account firmy Microsoft, dostawców uwierzytelniania OpenID, takich jak Yahoo!, itp.), i zwróć dostawców tożsamości *nazwy identyfikatorów*. Te identyfikatory nazwa może zawierać osobiście informacji osobowych, jak adres e-mail lub może być anonimowe jak prywatnego identyfikatora osobistego (PPID). Niezależnie od tego, krotki (dostawcy tożsamości, nazwa identyfikatora) wystarczająco służy jako token śledzenia odpowiednie dla danego użytkownika, gdy jest ona Przeglądanie witryny, więc Runtime stosu sieci Web platformy ASP.NET można użyć spójnej kolekcji zamiast nazwy użytkownika podczas generowania i Sprawdzanie poprawności tokenów pola anti-XSRF. Określonego identyfikatorów URI dla dostawcy tożsamości i identyfikator nazwy są:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(zobacz [strony doc ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) Aby uzyskać więcej informacji.)

Podczas tworzenia lub sprawdzania poprawności tokenu, Runtime stosu sieci Web ASP.NET w czasie wykonywania spróbuje powiązania do typów:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35`(Dla zestawu SDK programu WIF.)
- `System.Security.Claims.ClaimsIdentity`(Dla platformy .NET 4.5).

Jeśli te typy istnieje, a bieżący użytkownik *IIIIdentity* implementuje lub podklasy jeden z tych typów, funkcji anti-XSRF będzie używać (dostawcy tożsamości, nazwa identyfikatora) krotki zamiast nazwy użytkownika podczas generowania i Sprawdzanie poprawności tokenów. Jeśli nie takie spójnej kolekcji, żądanie zakończy się niepowodzeniem z powodu błędu opisujące deweloperowi sposób skonfigurowania systemu anti-XSRF, aby zrozumieć mechanizm określonego uwierzytelniania opartego na oświadczeniach w użyciu. Zobacz  **[konfiguracji i rozszerzalność](#_Configuration_and_extensibility)**  sekcji, aby uzyskać więcej informacji.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID uwierzytelniania

Na koniec funkcji anti-XSRF ma specjalną obsługę dla aplikacji, które używają uwierzytelniania OAuth lub OpenID. Ta obsługa jest oparta na protokole heurystyki: Jeśli bieżący *IIdentity.Name* rozpoczyna się od http:// lub https://, username porównania zostaną wykonane, a następnie za pomocą liczby porządkowej porównania niż domyślna funkcja porównująca OrdinalIgnoreCase.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Konfiguracja i rozszerzalność

Czasami deweloperzy może być zwiększenie poziomu kontroli nad generowania anti-XSRF i zachowania sprawdzania poprawności. Na przykład prawdopodobnie pomocników MVC i stron sieci Web domyślne zachowanie automatycznie dodawane do odpowiedzi plików cookie protokołu HTTP jest niepożądane, a deweloper może chcieć zachować tokenów w innym miejscu. Istnieją dwa interfejsy API, co ułatwi to:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens* metoda przyjmuje jako wejście istniejących XSRF żądania weryfikacji tokenu sesji (co może mieć wartości zerowej) i i tworzy jako wyjście nowe XSRF żądania weryfikacji tokenu sesji i token pola. Tokeny są po prostu nieprzezroczyste ciągów nie decoration; *formToken* wartość dla wystąpienia nie będzie zawijany w &lt;wejściowych&gt; tagu. *NewCookieToken* wartość może mieć wartości null; w takim przypadku to *oldCookieToken* wartość jest nadal prawidłowa, a nie nowy plik cookie odpowiedzi należy ustawić. Element wywołujący *GetTokens* jest odpowiedzialny za przechowywanie żadnych plików cookie na potrzeby odpowiedzi lub generowania wszelkie niezbędne znaczników; *GetTokens* metody nie można zmienić odpowiedzi jako efekt uboczny. *Weryfikacji* metoda przyjmuje przychodzących sesji i pola tokeny i uruchamia logikę weryfikacji wyżej nad nimi.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Deweloper może skonfigurować system anti-XSRF z aplikacji\_Start. Konfiguracja jest programowe. Właściwości statycznych *AntiForgeryConfig* typu są opisane poniżej. Większość użytkowników przy użyciu oświadczeń należy ustawić właściwość UniqueClaimTypeIdentifier.

| **Właściwość** | **Opis** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) udostępnia dodatkowe dane podczas generowania tokenu i używa dodatkowych danych podczas sprawdzania poprawności tokenu. Wartość domyślna to *null*. Aby uzyskać więcej informacji, zobacz [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) sekcji. |
| **CookieName** | Ciąg, który zawiera nazwę pliku cookie HTTP, który jest używany do przechowywania tokenu anti-XSRF sesji. Jeśli ta wartość nie jest ustawiona, nazwy będą automatycznie generowane na podstawie ścieżki wirtualnej wdrożonej aplikacji. Wartość domyślna to *null*. |
| **Parametru RequireSsl** | Wartość logiczna określająca, czy tokeny anti-XSRF są wymagane do przesłania za pośrednictwem kanału zabezpieczonego protokołem SSL. Jeśli ta wartość jest *true*, plików cookie generowane automatycznie ma flagę "bezpiecznej" i interfejsów API anti-XSRF nie jest wykonywana żadna wywołane z wnętrza żądanie, które nie są przesyłane za pośrednictwem protokołu SSL. Wartość domyślna to *false*. |
| **SuppressIdentityHeuristicChecks** | Wartość logiczna określająca, czy system anti-XSRF dezaktywować jego obsługę tożsamości opartego na oświadczeniach. Jeśli ta wartość jest *true*, system zakłada, że *IIdentity.Name* jest odpowiednie do użycia jako identyfikator unikatowy dla poszczególnych użytkowników i nie będzie próbował specjalny przypadek *IClaimsIdentity*lub *ClClaimsIdentity* zgodnie z opisem w [WIF / ACS / opartego na oświadczeniach uwierzytelniania](#_WIF_ACS) sekcji. Wartość domyślna to `false`. |
| **UniqueClaimTypeIdentifier** | Ciąg, który wskazuje typ oświadczenia, które jest odpowiednie do użycia jako identyfikator unikatowy dla poszczególnych użytkowników. Jeśli ta wartość jest ustawiony, a bieżące *tożsamości* opartego na oświadczeniach, system będzie podejmować próby wyodrębnienia oświadczenie typu określonym przez *UniqueClaimTypeIdentifier*, i będzie używana wartość odpowiadająca zamiast nazwy użytkownika podczas generowania token pola. Nie znaleziono typu oświadczenia, zakończy się niepowodzeniem żądania przez system. Wartość domyślna to *null*, co oznacza, że system powinien używać (dostawcy tożsamości, nazwa identyfikatora) spójnej kolekcji, jak opisano wcześniej zamiast nazwy użytkownika. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)*  typu umożliwia deweloperom rozszerzyć zachowanie systemu anti-XSRF przez dwustronną komunikację w każdym token dodatkowe dane. *GetAdditionalData* metoda jest wywoływana za każdym razem zostanie wygenerowany token pola i wartość zwracana jest osadzony w wygenerowany token. Realizator może zwrócić sygnaturę czasową, identyfikatora jednorazowego lub wszelkie inne wartości, które użytkownik chce z tej metody.

Podobnie *ValidateAdditionalData* metoda jest wywoływana za każdym razem token pola jest zweryfikowany, a ciąg "dane dodatkowe", które zostały osadzone w tokenie jest przekazywany do metody. Procedury weryfikacji można zaimplementować przekroczenie limitu czasu (sprawdzając aktualny czas od czasu, które były przechowywane, podczas tworzenia tokenu), identyfikatora jednorazowego sprawdzanie procedury lub inne potrzeby logiki.

## <a name="design-decisions-and-security-considerations"></a>Zagadnienia dotyczące zabezpieczeń i decyzje dotyczące projektu

Tokeny sesji i pola łączy token zabezpieczający jest technicznie wymagane tylko podczas próby ochrony anonimowe / nieuwierzytelnionych użytkowników przed atakami XSRF. Gdy użytkownik jest uwierzytelniony, token uwierzytelniania sam (prawdopodobnie przesłane w postaci pliku cookie) może być użyta jako jeden połowy Synchronizator token pary. Jednak są prawidłowe scenariusze ochrony strony logowania trafień przez nieuwierzytelnionych użytkowników i logiki anti-XSRF wykonał prostsze zawsze Generowanie i sprawdzania poprawności tokenu zabezpieczeń, nawet dla użytkowników uwierzytelnionych. On również zawierał niektórych dodatkową ochronę w przypadku, gdy token pola kiedykolwiek jest kontrolowany przez osobę atakującą, jako ustawienie lub odgadnięcie tokenu sesji może być inny próg wykluczający atakującemu rozwiązać.

Deweloperzy należy zachować ostrożność, gdy wiele aplikacji znajdują się w jednej domenie. Na przykład nawet jeśli *example1.cloudapp.net* i *example2.cloudapp.net* są różnych hostach, istnieje relacja zaufania niejawne między wszystkie hosty w  *\*. cloudapp.net* domeny. Ta relacja zaufania niejawne [umożliwia potencjalnie niezaufane hosty wpływ na pliki cookie siebie nawzajem](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (zasad tego samego źródła, które będą zarządzały sposobem żądania AJAX nie zawsze dotyczą plików cookie protokołu HTTP). Środowisko uruchomieniowe stosu sieci Web ASP.NET zawiera pewne środki zaradcze w tym nazwa użytkownika jest osadzony w token pola, tak więc nawet, jeśli złośliwe poddomeny jest w stanie zastąpić tokenu sesji będzie nie można wygenerować pola prawidłowy token dla użytkownika. Jednak w przypadku hostowania w takim środowisku procedury wbudowanych anti-XSRF nadal nie obrony przed przejęcie kontroli sesji lub logowania XSRF.

Procedury anti-XSRF aktualnie nie obrony przed [porywaniu kliknięć](https://www.owasp.org/index.php/Clickjacking). Aplikacje, które chcesz bronić się przed porywaniu kliknięć mogą łatwo to zrobić, wysyłając X-Frame-Options: nagłówek SAMEORIGIN z każdym odpowiedzi. Ten nagłówek jest obsługiwana przez wszystkie ostatnie przeglądarki. Aby uzyskać więcej informacji, zobacz [IE blog](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [blogu SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), i [OWASP](https://www.owasp.org/index.php/Clickjacking). Środowisko uruchomieniowe stosu sieci Web ASP.NET może dokonać niektórych przyszłej wersji platformy MVC i pomocników anti-XSRF stron sieci Web automatycznie Ustaw ten nagłówek tak, aby aplikacje są automatycznie chronione przed takiego ataku.

Aby upewnić się, że ich lokacji nie jest narażony na ataki XSS powinno być kontynuowane deweloperów sieci Web. Atakom XSS są bardzo zaawansowane i wykorzystać pomyślne czy Przerwij zabezpieczenia środowiska uruchomieniowego stosu sieci Web ASP.NET przed atakami XSRF.

## <a name="acknowledgment"></a>Potwierdzenia

[@LeviBroderick](https://twitter.com/LeviBroderick), autorze znacznie kod zabezpieczeń ASP.NET zbiorczego te informacje.
