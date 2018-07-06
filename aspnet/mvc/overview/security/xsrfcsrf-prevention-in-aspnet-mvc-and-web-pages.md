---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Zapobieganie XSRF/CSRF w ASP.NET MVC i Web Pages | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Fałszowanie żądań między witrynami (znany także jako XSRF lub CSRF) jest ataku na aplikacje hostowane w sieci web, według której złośliwych witryn sieci web mogą mieć wpływ na interakcyjne...
ms.author: aspnetcontent
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: be0e8ebe521e9952d7525b581f9b91af6edca1da
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820259"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Zapobieganie XSRF/CSRF w ASP.NET MVC i Web Pages
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Fałszerstwo żądania międzywitrynowego (znany także jako XSRF lub CSRF) jest ataku na aplikacje hostowane w sieci web, według której złośliwych witryn sieci web mogą mieć wpływ na interakcję między przeglądarką klienta i witryny sieci web, któremu ufają tej przeglądarki. Te ataki są możliwe, ponieważ przeglądarki sieci web wysyła tokeny uwierzytelniania automatycznie za pomocą każdego żądania do witryny sieci web. Canonical przykładem jest plik cookie uwierzytelniania, np. ASP. Bilet uwierzytelniania formularzy w sieci. Jednak te ataki można zastosować witryn sieci web, które używają każdy mechanizm uwierzytelniania trwałe (na przykład uwierzytelniania Windows, Basic i tak dalej).
> 
> Atak XSRF różni się od ataku. Wyłudzanie informacji wymaga interakcji z ofiary. W celu wyłudzanie informacji złośliwych witryn sieci web przypominające docelowej witryny sieci web, a ofiary jest fooled do dostarczania informacji poufnych dla osoby atakującej. W przypadku ataków XSRF często występuje bez interakcji ze ofiary. Przeciwnie osoba atakująca powołuje się na przeglądarce wszystkie odpowiednie pliki cookie są automatycznie wysyłane do docelowej witryny sieci web.
> 
> Aby uzyskać więcej informacji, zobacz [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomia ataku

Przeprowadzenie ataku XSRF, należy uwzględnić użytkownika, który chce wykonać niektóre transakcje bankowość online. Ten użytkownik odwiedzi najpierw WoodgroveBank.com dzienniki i, w tym momencie nagłówek odpowiedzi będzie zawierać jej pliku cookie uwierzytelniania:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Ponieważ plik cookie uwierzytelniania jest plik cookie sesji, jego zostanie automatycznie usunięty przez przeglądarkę podczas procesu przeglądarki. Jednak do tego czasu przeglądarki automatycznie będzie zawierać plik cookie z każdym żądaniem, aby WoodgroveBank.com. Użytkownik chce teraz przenieść 1000 USD do innego konta, dzięki czemu użytkownik wypełnia formularz w witrynie bankowości i przeglądarce wysyła żądanie do serwera:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Ta operacja jest efektem (inicjuje pieniężnych transakcji), dlatego witrynie bankowej wybrał wymagać metodę POST protokołu HTTP, aby zainicjować tę operację. Serwer odczytuje token uwierzytelniania w żądaniu, wyszukuje numer konta bieżącego użytkownika, sprawdza, czy istnieje nałożone i następnie inicjuje transakcji na docelowym koncie.

Jej bankowość online pełną, użytkownik przechodzi witryny bankowości i odwiedza inne lokalizacje w sieci web. Jedną z tych witryn — fabrikam.com — obejmuje następujące znaczniki na stronie osadzane &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Który następnie powoduje, że przeglądarka do wykonania tego żądania:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Osoba atakująca to wykorzystania fakt, że użytkownik nadal może być ważny token uwierzytelniania dla docelowej witryny sieci web i ona używa mały fragment kodu języka JavaScript w przeglądarce do metody HTTP POST do lokacji docelowej automatycznie. Jeśli token uwierzytelniania jest nadal ważny, witrynie bankowej zainicjuje przeniesienia 250 USD pod uwagę w Wybieranie osoby atakującej.

### <a name="ineffective-mitigations"></a>Środki zaradcze nieefektywne

Należy zauważyć, że w powyższym scenariuszu Usługa fakt, że WoodgroveBank.com był uzyskiwany za pośrednictwem protokołu SSL i miał pliku cookie uwierzytelniania tylko do protokołu SSL jest za mała, aby zablokować próby ataku. Osoba atakująca jest w stanie określić [schemat identyfikatora URI](http://en.wikipedia.org/wiki/URI_scheme) (https) w jej &lt;formularza&gt; elementu, a przeglądarka kontynuuje wysyłanie niewygasłe plików cookie do lokacji docelowej, tak długo, jak te pliki cookie są zgodne z identyfikatorem URI Schemat zamierzonym obiektem docelowym.

Jeden można dokumentu uważają, że użytkownik nie po prostu odwiedź niezaufanych witryn jako odwiedzający tylko zaufane witryny jest pomaga pozostały bezpieczne w trybie online. Istnieje kilka prawdziwych danych do tego, ale Niestety te porady nie zawsze jest praktyczne. Być może użytkownik "ufa" lokacji lokalnych grup dyskusyjnych ConsolidatedMessenger. ConsolidatedMessenger.com i zbliża się do odwiedziny tej lokacji zamiast tego, ale w tej lokacji ma luki w zabezpieczeniach XSS, która umożliwia osobie atakującej iniekcję ten sam fragment kodu, która była uruchomiona na fabrikam.com.

Możesz sprawdzić, czy żądania przychodzące mają [nagłówka Odnośnik](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) odwołuje się do domeny. Spowoduje to zatrzymanie żądania wirus przesłane z domeny innej firmy. Jednak niektóre osoby wyłączyć nagłówka Odnośnik ich przeglądarki w celu zachowania prywatności i osoby atakujące mogą czasami podszywały się pod nagłówka Jeśli ofiary zainstalowano określone oprogramowanie niebezpieczne. Weryfikowanie [nagłówka Odnośnik](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) nie jest uważany za bezpieczny sposobem zapobieganie atakom XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Środki zaradcze stosu środowiska uruchomieniowego XSRF sieci Web

Środowisko uruchomieniowe stosu sieci Web ASP.NET przechowuje wariant [wzorzec tokenu Synchronizator](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) obrony przed atakami XSRF. Ogólna postać wzorzec tokenu Synchronizator jest, że dwa tokeny anti-XSRF są przesyłane do serwera z każdej metody POST protokołu HTTP (oprócz tokenu uwierzytelniania): jeden token pliku cookie, a drugi jako wartości formularza. Token wartości generowane przez środowisko uruchomieniowe programu ASP.NET nie są deterministyczne lub przewidywalnym przez osobę atakującą. Po przesłaniu tokenów serwer będzie zezwalać na żądania można kontynuować tylko wtedy, gdy oba tokeny przekazać wyboru porównania.

Weryfikacja żądania XSRF *tokenu sesji* jest przechowywana jako plik cookie protokołu HTTP i obecnie zawiera następujące informacje w ładunku:

- Token zabezpieczający składający się z identyfikatorem 128-bitowego losowych.   
 Na poniższej ilustracji przedstawiono tokenu sesji weryfikacji żądania XSRF wyświetlane przy użyciu narzędzi deweloperskich programu Internet Explorer F12: (Uwaga jest bieżąca implementacja parametru i podlega, nawet prawdopodobne zmienić.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*Token pole je mimo* jest przechowywany jako `<input type="hidden" />` i zawiera następujące informacje w ładunku:

- Nazwa zalogowanego użytkownika (Jeśli uwierzytelnienie).
- Wszelkie dodatkowe dane dostarczane przez [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Ładunki tokenów anti-XSRF są zaszyfrowana i podpisana, więc nie można wyświetlić nazwy użytkownika, korzystając z narzędzi do sprawdzenia tokenów. W przypadku aplikacji sieci web jest przeznaczony dla programu ASP.NET 4.0, usługi kryptograficzne jest określany przez [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) procedury. Gdy aplikacji sieci web jest docelowo ASP.NET 4.5 lub nowszej, kryptograficzne usługi są dostarczane przez [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) procedura, która oferuje lepszą wydajność, rozszerzalność i bezpieczeństwo. Zobacz, że wpisy następujący wpis w blogu, aby uzyskać więcej informacji:

- [Ulepszenia usług kryptograficznych w programie ASP.NET 4.5, (czas pacyficzny). 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Ulepszenia usług kryptograficznych w programie ASP.NET 4.5, (czas pacyficzny). 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Ulepszenia usług kryptograficznych w programie ASP.NET 4.5, (czas pacyficzny). 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generowanie tokenów

Aby wygenerować tokeny anti-XSRF, należy wywołać [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) metody z widoku MVC lub @AntiForgery.GetHtml() ze strony Razor. Środowisko uruchomieniowe będzie następnie wykonaj następujące czynności:

1. Jeśli bieżące żądanie HTTP zawiera już token anti-XSRF sesji (plik cookie anti-XSRF \_ \_RequestVerificationToken), token zabezpieczający jest wyodrębniany z niego. Żądanie HTTP nie zawiera token anti-XSRF sesji lub nie powiodło się wyodrębnienie token zabezpieczający, zostanie wygenerowany nowy token anti-XSRF losowych.
2. Token anti-XSRF pola jest generowana z użyciem tokenu zabezpieczającego z poprzednim kroku (1) i tożsamości bieżącego zalogowanego użytkownika. (Aby uzyskać więcej informacji na temat określania tożsamości użytkownika, zobacz **[scenariusze z obsługą specjalne](#_Scenarios_with_special)** poniższej sekcji.) Ponadto jeśli [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) jest skonfigurowany, środowisko uruchomieniowe będzie wywoływać jego [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) metody i dołącza token pola zwracanego ciągu. (Zobacz **[konfiguracji i rozszerzalność](#_Configuration_and_extensibility)** sekcji, aby uzyskać więcej informacji.)
3. Jeśli nowy token anti-XSRF został wygenerowany w kroku (1), token nowej sesji zostaną utworzone dla niej i zostanie dodany do kolekcji wychodzące pliki cookie protokołu HTTP. Token pole z kroku (2) zostaną opakowane w `<input type="hidden" />` elementu, a ten kod znaczników HTML będzie wartość zwracaną przez `Html.AntiForgeryToken()` lub `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Sprawdzanie poprawności tokenów

Do weryfikacji tokenów przychodzących anti-XSRF, deweloper obejmuje [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) atrybut jej akcja kontrolera MVC lub kontrolera lub wywołań nagrywa `@AntiForgery.Validate()` ze swojej strony Razor. Środowisko uruchomieniowe wykona następujące czynności:

1. Przychodzący token sesji i token pola są odczytywane i token anti-XSRF wyodrębnione każdego z nich. Tokeny anti-XSRF muszą być identyczne na każdym etapie (2) w procedurze generacji.
2. Jeśli bieżący użytkownik jest uwierzytelniony, jej nazwa użytkownika jest porównywana z przechowywanych w tokenie pole nazwy użytkownika. Nazwy użytkowników muszą być zgodne.
3. Jeśli [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) jest skonfigurowany, środowisko uruchomieniowe wywołuje swoją *ValidateAdditionalData* metody. Metoda musi zwracać wartość logiczną *true*.

Jeśli weryfikacja zakończy się powodzeniem, żądanie jest dozwolone, aby kontynuować. Jeśli weryfikacja zakończy się niepowodzeniem, spowoduje zgłoszenie ramach *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Warunki błędów

Począwszy od środowiska uruchomieniowego stosu sieci Web platformy ASP.NET w wersji 2 wszelkie *HttpAntiForgeryException* , jest zgłaszany podczas sprawdzania poprawności będzie zawierać szczegółowe informacje na temat tego, co poszło. Warunki błędów aktualnie zdefiniowanych są:

- Token sesji lub tokenu formularza nie jest obecne w żądaniu.
- Nie będzie można odczytać tokenu sesji lub tokenu formularza. Najbardziej prawdopodobną przyczyną tego jest farmy niezgodnymi wersjami środowiska uruchomieniowego stosu sieci Web platformy ASP.NET lub farmy gdzie &lt;machineKey&gt; elementu w pliku Web.config, który różni się między maszynami. Aby wymusić ten wyjątek przez naruszeniu albo token anti-XSRF, można użyć narzędzia, takiego jak Fiddler.
- Token sesji i token pola zostały zamienione.
- Token sesji i token pole je mimo zawierają tokenów zabezpieczających niezgodne.
- Osadzone w tokenie pola Nazwa użytkownika jest niezgodna z bieżącego zalogowanego użytkownika.
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* zwróconej metody *false*.

Urządzenia anti-XSRF także przeprowadzić dodatkowe sprawdzenie podczas generowania tokenu lub sprawdzania poprawności i błędy podczas kontroli może spowodować zgłaszane wyjątki. Zobacz [WIF / ACS / opartego na oświadczeniach uwierzytelniania](#_WIF_ACS) i **[konfiguracji i rozszerzalność](#_Configuration_and_extensibility)** sekcje, aby uzyskać więcej informacji.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scenariusze z obsługą specjalne

### <a name="anonymous-authentication"></a>Uwierzytelnianie anonimowe

System anti-XSRF zawiera specjalne pomocy technicznej dla użytkowników anonimowych, w którym "anonimowy" jest zdefiniowana jako użytkownik gdzie *IIdentity.IsAuthenticated* właściwość zwraca *false*. Scenariusze obejmują zapewnienie ochrony XSRF do strony logowania (zanim użytkownik jest uwierzytelniany) i schematów uwierzytelniania niestandardowego, których aplikacja używa mechanizmu innych niż *IIdentity* do identyfikacji użytkowników.

Do obsługi tych scenariuszy, pamiętaj, że tokeny sesji i pola są sprzęgane przy token zabezpieczający, który jest identyfikatorem nieprzezroczyste generowany losowo 128-bitowego. Ten token zabezpieczający służy do śledzenia sesji użytkownika, jak ona powoduje przejście lokacji, aby skutecznie służy celem anonimowy identyfikator. Pusty ciąg jest używany zamiast nazwy użytkownika dla generowania i sprawdzanie poprawności procedur opisanych powyżej.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>Program WIF / ACS / opartego na oświadczeniach uwierzytelniania

Zwykle *IIdentity* klasy wbudowane w platformę .NET Framework ma właściwości, *IIdentity.Name* jest wystarczająca do unikatowego identyfikowania użytkownika określonego w ramach określonej aplikacji. Na przykład *FormsIdentity.Name* zwraca nazwę użytkownika, przechowywane w bazie danych członkostwa (która jest unikatowa dla wszystkich aplikacji, w zależności od tego, że ta baza danych), *WindowsIdentity.Name* zwraca kwalifikowanej domeny tożsamość użytkownika i tak dalej. Te systemy zapewnia nie tylko uwierzytelniania; mogą również *zidentyfikować* użytkowników w aplikacji.

W przypadku uwierzytelniania opartego na oświadczeniach z drugiej strony, nie wymaga identyfikujący określonego użytkownika. Zamiast tego *ClaimsPrincipal* i *ClaimsIdentity* typy są skojarzone z zestawem *oświadczenia* wystąpień, w której poszczególne oświadczeń może być "is lat 18 +" lub " gdy administrator"na jakąkolwiek inną. Ponieważ użytkownik nie musi być identyfikowany, środowisko uruchomieniowe nie może używać *ClaimsIdentity.Name* właściwość jako unikatowy identyfikator dla tego użytkownika. Rzeczywiste przykłady obserwowała, jak zespół gdzie *ClaimsIdentity.Name* zwraca *null*, zwraca nazwę przyjazną (wyświetlanie) lub, w przeciwnym razie zwraca wartość typu ciąg, który nie jest przeznaczone do użycia jako unikatowy identyfikator dla użytkownika.

Wiele wdrożeń, w których korzystanie z uwierzytelniania opartego na oświadczeniach za pomocą [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) w szczególności. Usługi ACS umożliwia deweloperowi skonfigurować poszczególne *dostawców tożsamości* (np. ADFS, dostawcy Account firmy Microsoft, dostawców uwierzytelniania OpenID, takich jak Yahoo!, itp.), i zwracają dostawców tożsamości *nazwy identyfikatorów*. Identyfikatory te nazwy mogą zawierać identyfikowalne dane osobowe, jak adres e-mail, lub można zachować anonimowość takich jak prywatny osobistego identyfikatora (PPID). Niezależnie od tego, krotki (dostawcy tożsamości, identyfikator nazwy) wystarczająco służy jako token śledzenia odpowiednie dla danego użytkownika, gdy użytkownik przegląda witryny, dzięki środowisko uruchomieniowe stosu sieci Web ASP.NET za pomocą spójnej kolekcji, zamiast nazwy użytkownika, podczas generowania i Sprawdzanie poprawności tokenów pola anti-XSRF. Identyfikatory URI określonego dostawcy tożsamości i identyfikator nazwy są następujące:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(zobacz ten [strony dokumentacji usługi ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) Aby uzyskać więcej informacji.)

Podczas tworzenia lub weryfikacji tokenu, środowisko uruchomieniowe stosu sieci Web ASP.NET w środowisku uruchomieniowym spróbuje powiązanie z typami:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (W przypadku zestawu SDK programu WIF.)
- `System.Security.Claims.ClaimsIdentity` (Dla platformy .NET 4.5).

Jeśli istnieje tych typów, a bieżący użytkownik *IIIIdentity* implementuje lub podklasy jeden z tych typów, funkcji anti-XSRF użyje (dostawcy tożsamości, identyfikator nazwy) krotki zamiast nazwy użytkownika podczas generowania i Sprawdzanie poprawności tokenów. Jeśli ma nie takich spójnej kolekcji, żądanie zakończy się niepowodzeniem z powodu błędu dla dewelopera z opisem skonfigurować system anti-XSRF zrozumienie mechanizm określonego uwierzytelniania opartego na oświadczeniach w użyciu. Zobacz **[konfiguracji i rozszerzalność](#_Configuration_and_extensibility)** sekcji, aby uzyskać więcej informacji.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID uwierzytelniania

Na koniec funkcji anti-XSRF ma specjalne obsługę aplikacji, które korzystają z uwierzytelniania OAuth lub OpenID. Ta funkcja jest oparta na Algorytm heurystyczny: Jeśli bieżący *IIdentity.Name* rozpoczyna się od http:// lub https://, porównywanie nazwy użytkownika zostanie wykonane, a następnie za pomocą porównania porządkowego a nie domyślny moduł porównujący OrdinalIgnoreCase.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Konfiguracja i rozszerzalność

Od czasu do czasu deweloperów może być ściślejszą kontrolę generowania anti-XSRF i zachowania poprawności. Na przykład może być niepożądane jest pomocników MVC i Web Pages domyślne zachowanie automatyczne dodawanie plików cookie protokołu HTTP do odpowiedzi, a deweloper pragną zachować tokenów w innym miejscu. Istnieją dwa interfejsy API, aby pomóc w tym:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens* metoda przyjmuje jako dane wejściowe istniejących XSRF żądania weryfikacji tokenu sesji (która może być null) i tworzy jako dane wyjściowe w nowy XSRF żądania weryfikacji tokenu sesji i token pole je mimo. Tokeny są po prostu nieprzezroczyste ciągów nie dekoracji; *formToken* wartość dla wystąpienia nie będzie zawijany w &lt;wejściowych&gt; tagu. *NewCookieToken* wartość może mieć wartości null; Jeśli ten problem wystąpi, a następnie *oldCookieToken* wartość jest nadal ważny, a nie nowy plik cookie odpowiedzi, należy ustawić. Obiekt wywołujący *GetTokens* jest odpowiedzialny za przechowywanie żadnych plików cookie na potrzeby odpowiedzi lub generowania marżę konieczne; *GetTokens* sama metoda nie ma wpływu na odpowiedź jako efekt uboczny. *Weryfikacji* metoda przyjmuje przychodzących sesji i pole tokenów i uruchamia logikę weryfikacji wyżej nad nimi.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Deweloper może skonfigurować system anti-XSRF z aplikacji\_Start. Konfiguracja jest programowe. Właściwości statyczne *AntiForgeryConfig* typu zostały opisane poniżej. Większość użytkowników przy użyciu oświadczeń chcesz ustawić właściwość UniqueClaimTypeIdentifier.

| **Property** | **Opis** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) który udostępnia dodatkowe dane podczas generowania tokenu i używa dodatkowych danych podczas weryfikowania tokenu. Wartość domyślna to *null*. Aby uzyskać więcej informacji, zobacz [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) sekcji. |
| **CookieName** | Ciąg, który zawiera nazwę pliku cookie HTTP, który jest używany do przechowywania tokenu anti-XSRF sesji. Jeśli ta wartość nie jest ustawiona, nazwy będą automatycznie generowane na podstawie aplikacji wdrożonej ścieżki wirtualnej. Wartość domyślna to *null*. |
| **Wartość parametru RequireSsl** | Wartość logiczna, która określa, czy tokeny anti-XSRF są wymagane do przesłania za pośrednictwem kanału zabezpieczonego protokołem SSL. Jeśli ta wartość jest *true*, plików cookie generowane automatycznie, będzie miał "bezpieczne" ustawiona jest flaga i interfejsów API anti-XSRF będzie zgłaszać wyjątek, jeśli wywołane z wnętrza żądania, który nie jest przesyłany za pośrednictwem protokołu SSL. Wartość domyślna to *false*. |
| **SuppressIdentityHeuristicChecks** | Wartość logiczna, która określa, czy system anti-XSRF dezaktywować jego obsługę tożsamości opartej na oświadczeniach. Jeśli ta wartość jest *true*, system założy, że *IIdentity.Name* jest przeznaczone do użycia jako identyfikator unikatowy dla poszczególnych użytkowników i nie będzie próbował szczególny *IClaimsIdentity*lub *ClClaimsIdentity* zgodnie z opisem w [WIF / ACS / opartego na oświadczeniach uwierzytelniania](#_WIF_ACS) sekcji. Wartość domyślna to `false`. |
| **UniqueClaimTypeIdentifier** | Ciąg wskazujący typ oświadczenia, które jest przeznaczone do użycia jako identyfikator unikatowy dla poszczególnych użytkowników. Jeśli ta wartość jest ustawiony, bieżący *IIdentity* opartego na oświadczeniach, system spróbuje wyodrębnić oświadczenia o typie określonym przez *UniqueClaimTypeIdentifier*, i zostanie użyta wartość odpowiedniego zamiast nazwy użytkownika podczas generowania tokenu pola. Jeśli typ oświadczenia nie zostanie znaleziony, system nie obsłuży żądania. Wartość domyślna to *null*, co oznacza, że system powinien używać (dostawcy tożsamości, identyfikator nazwy) krotki, jak opisano wcześniej zamiast nazwy użytkownika. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* typu umożliwia deweloperom rozszerzania działania systemu anti-XSRF przez obustronne konwertowanie w każdy token dodatkowe dane. *GetAdditionalData* metoda jest wywoływana za każdym razem generowania tokenu pola i wartość zwracana jest osadzony w generowanych token. Implementujący może zwrócić sygnaturę czasową, identyfikatora jednorazowego lub dowolnej innej wartości, które użytkownik chce z tej metody.

Podobnie *ValidateAdditionalData* metoda jest wywoływana za każdym razem token pola jest weryfikowana, a ciąg "dodatkowe dane", które zostały osadzone w tokenie jest przekazywany do metody. Procedury weryfikacji implementacji przekroczenie limitu czasu (sprawdzając bieżący czas od czasu, która została zapisana, gdy został utworzony token), identyfikatora jednorazowego sprawdzania, czy procedura lub inne potrzeby logiki.

## <a name="design-decisions-and-security-considerations"></a>Zagadnienia dotyczące zabezpieczeń i decyzje dotyczące projektu

Token zabezpieczający, który łączy tokenów sesji i pola jest technicznie tylko niezbędne podczas próby ochrony anonimowe / nieuwierzytelnionych użytkowników przed atakami XSRF. Gdy użytkownik jest uwierzytelniany, token uwierzytelniania, sama (prawdopodobnie przesyłany w postaci pliku cookie) może być używany jako jedną połowę Synchronizator token pary. Jednak istnieją scenariusze prawidłowy na potrzeby ochrony strony logowania testowanych przez nieuwierzytelnionych użytkowników i logiki anti-XSRF wykonał prostsze zawsze generowania i sprawdzanie poprawności tokenu zabezpieczeń, nawet w przypadku uwierzytelnionych użytkowników. Również zapewnia dodatkową ochronę w przypadku, gdy token pola nigdy nie zostanie naruszone przez osobę atakującą, jako ustawienie lub zgadywania tokenu sesji może być inny próg wykluczający dla osoby atakującej do pokonania.

Deweloperzy należy zachować ostrożność, gdy wiele aplikacji znajdują się w jednej domenie. Na przykład nawet jeśli *example1.cloudapp.net* i *example2.cloudapp.net* różnych hostów istnieje relacja nawiązywanie niejawnych relacji zaufania między wszystkimi hostami w ramach  *\*. cloudapp.net* domeny. Ta relacja zaufania niejawne [umożliwia potencjalnie niezaufane hosty wpływa na pliki cookie siebie nawzajem](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (zasady tego samego źródła, które określają sposób wysyłanie żądań AJAX nie zawsze dotyczą plików cookie protokołu HTTP). Środowisko uruchomieniowe stosu sieci Web ASP.NET zapewnia pewne ograniczenia, w tym, że nazwa użytkownika jest wbudowane w token pola, więc nawet, jeśli złośliwe domeny podrzędnej jest w stanie zastępowania tokenu sesji będzie on mógł wygenerować token prawidłowym polem dla użytkownika. Jednak w przypadku hostowania w środowisku usługi procedury wbudowanych anti-XSRF nadal nie obrony przed przejęcie kontroli sesji lub logowania XSRF.

Procedury anti-XSRF aktualnie nie obrony przed [porywaniu kliknięć](https://www.owasp.org/index.php/Clickjacking). Aplikacje, które chcesz chronić się przed porywaniu kliknięć łatwo zrobić, wysyłając X-Frame-Options: nagłówek SAMEORIGIN z każdej odpowiedzi. Tego pliku nagłówkowego jest obsługiwana przez wszystkie najnowsze przeglądarki. Aby uzyskać więcej informacji, zobacz [blog programu Internet Explorer](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [blogu SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), i [OWASP](https://www.owasp.org/index.php/Clickjacking). Środowisko uruchomieniowe stosu sieci Web ASP.NET może w niektórych upewnij przyszłej wersji platformy MVC i pomocników anti-XSRF stron sieci Web automatycznie ustawić tego pliku nagłówkowego, dzięki czemu aplikacje są automatycznie chronione przed tego rodzaju atak.

Deweloperzy sieci Web powinna w dalszym ciągu upewnij się, że ich lokacji nie jest narażony na ataki XSS. Atakom XSS są bardzo zaawansowane i wykorzystać pomyślne zaburzyłaby również zabezpieczenia środowiska uruchomieniowego stosu sieci Web platformy ASP.NET przed atakami XSRF.

## <a name="acknowledgment"></a>Po potwierdzeniu

[@LeviBroderick](https://twitter.com/LeviBroderick), który napisał znaczną część kodu zabezpieczeń platformy ASP.NET duża część tych informacji.
