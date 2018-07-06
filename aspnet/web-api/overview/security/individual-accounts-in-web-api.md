---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Zabezpieczanie interfejsu API sieci Web za pomocą indywidualnych kont i logowania lokalnego we wzorcu ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym temacie pokazano, jak zabezpieczyć internetowy interfejs API za pomocą protokołu OAuth2, do uwierzytelniania względem bazy danych członkostwa. Wersje oprogramowania używany w samouczku Visual 201 Studio...
ms.author: aspnetcontent
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0a520333492a60014f7e9f9182a16f0ce514ba1d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842088"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Zabezpieczanie interfejsu API sieci Web za pomocą indywidualnych kont i logowania lokalnego we wzorcu ASP.NET Web API 2.2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz przykładową aplikację](https://github.com/MikeWasson/LocalAccountsApp)

> W tym temacie pokazano, jak zabezpieczyć internetowy interfejs API za pomocą protokołu OAuth2, do uwierzytelniania względem bazy danych członkostwa.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Składnik Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


W programie Visual Studio 2013 szablon projektu interfejsu API sieci Web udostępnia trzy metody uwierzytelniania:

- **Indywidualne konta.** Aplikacja korzysta z bazy danych członkostwa.
- **Konta organizacji.** Użytkownicy zalogować się przy użyciu ich usługi Azure Active Directory, usługi Office 365, poświadczeń usługi Active Directory w środowisku lokalnym.
- **Uwierzytelnianie Windows.** Ta opcja jest przeznaczona dla aplikacji intranetowych i używa modułu Windows uwierzytelniania w usługach IIS.

Aby uzyskać więcej informacji o tych opcjach, zobacz [tworzenia projektów sieci Web platformy ASP.NET w programie Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Indywidualne konta zapewniają użytkownikowi na logowanie na dwa sposoby:

- **Lokalny identyfikator logowania**. Użytkownik rejestruje w danej lokacji, wprowadzając nazwę użytkownika i hasło. Aplikacja przechowuje wartość skrótu hasła w bazie danych członkostwa. Gdy użytkownik się zaloguje, system produktu ASP.NET Identity weryfikuje hasło.
- **Logowania społecznościowych**. Użytkownik loguje się przy użyciu usługi zewnętrznej, np. Facebook, Microsoft lub Google. Aplikacja nadal tworzy wpis dla użytkownika w bazie danych członkostwa, ale nie przechowuje żadnych poświadczeń. Użytkownik jest uwierzytelniany, logując się do usługi zewnętrznej.

W tym artykule przedstawiono w scenariuszu do logowania lokalnego. Do logowania zarówno lokalnych, jak i społecznościowych internetowy interfejs API używa protokołu OAuth2, do uwierzytelniania żądań. Przepływy poświadczeń są jednak inne w przypadku logowania lokalnego i społecznościowych.

W tym artykule zademonstruję prosta aplikacja, którą użytkownicy będą mogli zalogować się i wysyłać uwierzytelnionych wywołań AJAX do internetowego interfejsu API. Możesz pobrać przykładowy kod [tutaj](https://github.com/MikeWasson/LocalAccountsApp). Plik readme zawiera opis sposobu tworzenia przykładu od podstaw w programie Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Przykładowa aplikacja korzysta z użyciem Knockout.js powiązanie danych i jQuery wysyłanie żądań AJAX. I będzie można koncentrujących się na wywołań AJAX, więc nie trzeba znać struktura Knockout.js na potrzeby tego artykułu.

Po drodze będzie I opisano:

- Jakie działania aplikacji po stronie klienta.
- Co się dzieje na serwerze.
- Ruch HTTP w środku.

Najpierw należy zdefiniować niektóre terminologii OAuth2.

- *Zasób*. Niektóre części danych, które mogą być chronione.
- *Serwer zasobów*. Serwer hostujący zasób.
- *Właściciel zasobu*. Jednostka, która może udzielić uprawnień do uzyskania dostępu do zasobu. (Zazwyczaj użytkownik.)
- *Klient*: aplikację, która chce uzyskać dostęp do zasobu. W tym artykule klient jest przeglądarki sieci web.
- *Token dostępu*. Token udzielający dostępu do zasobu.
- *Token elementu nośnego*. Określony typ tokenu dostępu, za pomocą właściwości, każda osoba może użyć tokenu. Innymi słowy klient nie wymaga klucza kryptograficznego lub inne hasło do użycia tokenu elementu nośnego. Z tego powodu tokenów elementu nośnego powinna służyć wyłącznie za pośrednictwem protokołu HTTPS, a powinien mieć stosunkowo krótkich okresach ważności.
- *Serwer autoryzacji*. Serwer, który daje się tokenów dostępu.

Aplikacja może działać jako serwer zasobów i serwera autoryzacji. Szablon projektu interfejsu API sieci Web to wzorcem.

## <a name="local-login-credential-flow"></a>Przepływ poświadczeń do logowania lokalnego

W przypadku logowania lokalnego używa interfejsu API sieci Web [przepływu hasła właściciela zasobu](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) zdefiniowany typ tokenu protokołu oauth2.

1. Użytkownik wprowadza nazwę i hasło do klienta.
2. Klient wysyła te poświadczenia do autoryzacji serwera.
3. Serwer autoryzacji uwierzytelnia poświadczenia i zwraca token dostępu.
4. Aby uzyskać dostęp do chronionego zasobu, klient dołącza token dostępu w nagłówku autoryzacji żądania HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Po wybraniu **indywidualnych kont** w szablonie projektu interfejsu API sieci Web, projekt obejmuje serwera autoryzacji, weryfikuje poświadczenia użytkownika, która wystawia tokeny. Na poniższym diagramie przedstawiono sam przepływ poświadczeń w zakresie składników interfejsu API sieci Web.

![](individual-accounts-in-web-api/_static/image4.png)

W tym scenariuszu kontrolerów internetowych interfejsów API działać jako serwery zasobów. Filtr uwierzytelniania weryfikuje tokeny dostępu i **[Authorize]** atrybut jest używany w celu ochrony zasobu. Jeśli kontroler lub akcję ma **[Authorize]** atrybut, wszystkie żądania do tego kontrolera lub akcji, które musi zostać uwierzytelniony. W przeciwnym razie odmowy autoryzacji i internetowy interfejs API zwraca (błąd nieupoważnionego dostępu 401).

Serwer autoryzacji i filtr uwierzytelniania zarówno wywoływać [oprogramowania pośredniczącego OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) składnik, który obsłuży OAuth2. Czy mogę będzie opisują projektowania bardziej szczegółowo w dalszej części tego samouczka.

## <a name="sending-an-unauthorized-request"></a>Wysyłanie nieautoryzowanego żądania

Aby rozpocząć pracę, uruchomić aplikację, a następnie kliknij przycisk **Wywołaj interfejs API** przycisku. Po ukończeniu żądania powinien zostać wyświetlony komunikat o błędzie w **wynik** pole. To, ponieważ żądanie nie zawiera token dostępu, więc nie ma autoryzacji żądania.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**Wywołaj interfejs API** przycisk wysyła żądanie AJAX ~/api/wartości, które wywołuje akcji kontrolera interfejsu API sieci Web. Poniżej przedstawiono sekcję kodu JavaScript, który wysyła żądanie AJAX. W przykładowej aplikacji całego kodu aplikacji JavaScript znajduje się w pliku Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Dopóki użytkownik loguje się, istnieje żaden token elementu nośnego i dlatego nie nagłówek autoryzacji w żądaniu. Powoduje to, że żądanie zwróciło błąd 401 — dostęp.

Oto żądania HTTP. (Dawniej [Fiddler](http://www.telerik.com/fiddler) do przechwytywania ruchu HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Należy zauważyć, że odpowiedź zawiera nagłówka Www-Authenticate z żądaniem równa elementu nośnego. Wskazuje, że token elementu nośnego oczekiwanych przez serwer.

## <a name="register-a-user"></a>Rejestrowanie użytkownika

W **zarejestrować** części aplikacji, wprowadź adres e-mail i hasło, a następnie kliknij przycisk **zarejestrować** przycisku.

Nie trzeba używać prawidłowy adres e-mail dla tego przykładu, ale potwierdzenie adresu, rzeczywistych aplikacji. (Zobacz [tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Hasła należy użyć ciągu "Password1!", z wielkiej litery, małe litery, liczby i znaków innych niż alfanumeryczne. W celu uproszczenia aplikacji pozostawiono I walidacji po stronie klienta, dzięki czemu w przypadku problemu z formatem hasła, otrzymasz błąd 400 (nieprawidłowe żądanie).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**Zarejestrować** przycisk wysyła żądanie POST do ~/api/Account/Register /. Treść żądania jest obiekt JSON, który zawiera nazwę i hasło. Poniżej przedstawiono kod JavaScript, który wysyła żądanie:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Żądania HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

To żądanie jest obsługiwane przez `AccountController` klasy. Wewnętrznie `AccountController` używa produktu ASP.NET Identity do zarządzania bazą danych członkostwa.

Jeśli lokalne uruchamianie aplikacji w programie Visual Studio konta użytkowników są przechowywane w LocalDB, w tabeli AspNetUsers. Aby wyświetlić tabele w programie Visual Studio, kliknij **widoku** menu, wybierz opcję **Eksploratora serwera**, następnie rozwiń **połączeń danych**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Uzyskiwanie tokenu dostępu

Do tej pory firma Microsoft nie wykonano żadnych OAuth, ale teraz zobaczymy, serwer autoryzacji OAuth w działaniu, gdy żąda tokenu dostępu. W **logowanie** obszaru przykładowej aplikacji, wprowadź adres e-mail i hasło i kliknij przycisk **logowanie**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**Logowanie** przycisk wysyła żądanie do punktu końcowego tokenu. Treść żądania zawiera następujące dane zakodowane jako adres url formularza:

- Przyznaj\_typu: "password"
- Nazwa użytkownika: &lt;adres e-mail użytkownika&gt;
- hasło: &lt;hasła&gt;

Poniżej przedstawiono kod JavaScript, który wysyła żądanie AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Jeśli żądanie zakończy się powodzeniem, serwer autoryzacji zwraca token dostępu w treści odpowiedzi. Zwróć uwagę, czy token są przechowywane w pamięci masowej sesji, do użycia w przyszłości podczas wysyłania żądań do interfejsu API. Inaczej niż w przypadku niektórych metod uwierzytelniania (np. na podstawie plików cookie uwierzytelniania) przeglądarka nie będzie automatycznie zawierać token dostępu w kolejnych żądań. Aplikacja musi zrobić jawnie. Który jest to przydatne, ponieważ ogranicza [CSRF luk w zabezpieczeniach](preventing-cross-site-request-forgery-csrf-attacks.md).

Żądania HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Aby zobaczyć, czy żądanie zawiera poświadczenia użytkownika. Możesz *musi* używać protokołu HTTPS w celu zapewnienia zabezpieczeń warstwy transportu.

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Aby zwiększyć czytelność I wcięty za pomocą pliku JSON i obcięte tokenu dostępu, który jest bardzo długa.

`access_token`, `token_type`, I `expires_in` właściwości są definiowane przez specyfikację OAuth2. Inne właściwości (`userName`, `.issued`, i `.expires`) są przeznaczone tylko dla celów informacyjnych. Można znaleźć kod, który dodaje te dodatkowe właściwości w `TokenEndpoint` metody w pliku /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Wyślij żądanie uwierzytelnionego

Skoro mamy już token elementu nośnego, firma Microsoft może wprowadzać uwierzytelnionego żądania interfejsu API. Odbywa się przez ustawienie nagłówek autoryzacji w żądaniu. Kliknij przycisk **Wywołaj interfejs API** przycisk ponownie, aby to zobaczyć.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Żądania HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Wyloguj

Ponieważ przeglądarki nie buforuje poświadczeń lub tokenu dostępu, wylogowanie jest po prostu kwestią "podczas zapominania konta" token, usuwając go z sesji magazynu:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Opis szablonu projektu indywidualnych kont

Po wybraniu **indywidualnych kont** w szablonie projektu aplikacji sieci Web ASP.NET, projekt obejmuje:

- OAuth2 serwera autoryzacji.
- Punkt końcowy interfejsu API sieci Web do zarządzania kontami użytkowników
- Modelu platformy EF do przechowywania kont użytkowników.

Poniżej przedstawiono klasy głównej aplikacji, które implementują te funkcje:

- `AccountController`. Udostępnia punkt końcowy interfejsu API sieci Web do zarządzania kontami użytkowników. `Register` Akcja jest jedyną, która była używana w tym samouczku. Inne metody w klasie obsługuje resetowania haseł, społecznościowych nazw logowania i inne funkcje.
- `ApplicationUser`, zdefiniowane w /Models/IdentityModels.cs. Ta klasa jest modelu platformy EF dla kont użytkowników w bazie danych członkostwa.
- `ApplicationUserManager`, zdefiniowane w App\_Start/IdentityConfig.cs ta klasa jest pochodną [Menedżera UserManager](https://msdn.microsoft.com/library/dn613290.aspx) i wykonuje operacje na kontach użytkowników, takich jak tworzenie nowego użytkownika, weryfikowanie, hasła i tak dalej i automatycznie będzie się powtarzać zmiany w bazie danych.
- `ApplicationOAuthProvider`. Ten obiekt podłącza się do oprogramowania pośredniczącego OWIN i przetwarza zdarzenia wygenerowane przez oprogramowanie pośredniczące. Pochodzi od klasy [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Konfigurowanie serwera autoryzacji

W StartupAuth.cs poniższy kod służy do konfigurowania OAuth2 serwera autoryzacji.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath` Właściwość jest ścieżka adresu URL punktu końcowego serwera autoryzacji. To adres URL tej aplikacji używa w celu uzyskania tokenów elementu nośnego.

`Provider` Właściwość określa dostawcę, który zapewnia Ci dostęp do oprogramowania pośredniczącego OWIN i przetwarza zdarzeń zgłaszanych przez oprogramowanie pośredniczące.

Poniżej przedstawiono podstawowy przepływ, gdy aplikacja chce uzyskać token:

1. Aby uzyskać token dostępu, aplikacja wysyła żądanie do ~ / Token.
2. Wywołuje oprogramowanie pośredniczące uwierzytelniania OAuth `GrantResourceOwnerCredentials` dla dostawcy.
3. Wywołania dostawcy `ApplicationUserManager` do sprawdzania poprawności poświadczeń i tworzyć tożsamość oświadczeń.
4. Jeśli się to powiedzie, Dostawca tworzy bilet uwierzytelniania, który jest używany w celu wygenerowania tokenu.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Oprogramowanie pośredniczące uwierzytelniania OAuth nie wiedzieli cokolwiek o kont użytkowników. Dostawca komunikuje się między oprogramowanie pośredniczące i tożsamości ASP.NET. Aby uzyskać więcej informacji na temat wdrażania serwera autoryzacji, zobacz [OWIN OAuth 2.0 autoryzacji serwera](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Konfigurowanie interfejsu API sieci Web za pomocą tokenów Bearer

W `WebApiConfig.Register` metody, poniższy kod ustawia uwierzytelniania dla potok składnika Web API:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter** klasa umożliwia uwierzytelnianie za pomocą tokenów elementu nośnego.

**SuppressDefaultHostAuthentication** metoda informuje interfejsu API sieci Web, aby zignorować wszelkie uwierzytelniania, który znajduje się przed żądanie osiąga potok składnika Web API, usług IIS lub oprogramowania pośredniczącego OWIN. Dzięki temu będziemy ograniczać interfejsu API sieci Web do uwierzytelniania tylko za pomocą tokenów elementu nośnego.

> [!NOTE]
> W szczególności MVC części aplikacji może użyć uwierzytelniania formularzy, w której są przechowywane poświadczenia w pliku cookie. Na podstawie plików cookie uwierzytelniania wymagane jest użycie tokenów zabezpieczających przed sfałszowaniem, aby zapobiec atakom CSRF. To problemem w przypadku internetowych interfejsów API, ponieważ nie istnieje żadne wygodny sposób dla interfejsu API sieci web wysyła do klienta, token zabezpieczający przed sfałszowaniem. (Aby uzyskać więcej ogólnych informacji na ten problem, zobacz [zapobieganie atakom CSRF w interfejsie API sieci Web](preventing-cross-site-request-forgery-csrf-attacks.md).) Wywoływanie **SuppressDefaultHostAuthentication** gwarantuje, że interfejs API sieci Web nie jest narażony na ataki CSRF z poświadczeniami przechowywanymi w plikach cookie.


Gdy klient żąda zasobu chronionego, Oto, co się dzieje w potok składnika Web API:

1. **HostAuthentication** filtr wywołuje oprogramowanie pośredniczące uwierzytelniania OAuth, można zweryfikować tokenu.
2. Oprogramowanie pośredniczące konwertuje token tożsamości oświadczeń.
3. W tym momencie żądania jest *uwierzytelniony* , ale nie *autoryzacji*.
4. Filtr autoryzacji sprawdza, czy tożsamość oświadczeń. Jeśli oświadczenia autoryzacji użytkownika dla tego zasobu, żądanie jest autoryzowane. Domyślnie **[Authorize]** atrybut będzie autoryzować każde żądanie, które jest uwierzytelniony. Jednak można autoryzować według roli lub inne oświadczenia. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie i autoryzacja w interfejsie API sieci Web](authentication-and-authorization-in-aspnet-web-api.md).
5. Jeśli powyższe czynności zakończą się powodzeniem, ten kontroler zwraca chronionego zasobu. W przeciwnym razie klient odbiera (błąd nieupoważnionego dostępu 401).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [ASP.NET Identity](../../../identity/index.md)
- [Omówienie funkcji zabezpieczeń w szablonie SPA w wersji RC VS2013](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN wpis w blogu przez Hongye Sun.
- [Analiza poszczególnych interfejsów API sieci Web konta szablonu — część 2: kont lokalnych](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Wpis na blogu autorstwa Dominick Baier.
- [Hostowanie uwierzytelniania i internetowego interfejsu API z OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Dobry opis `SuppressDefaultHostAuthentication` i `HostAuthenticationFilter` przez firmy Brock Allen.
- [Dostosowywanie informacje o profilu w produkcie ASP.NET Identity w szablonach VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Wpis na blogu MSDN, autorem jest Pranav Rastogi.
- [Na żądanie Zarządzanie okresem istnienia dla klasy interfejs UserManager w produkcie ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Wpis na blogu MSDN autorstwa Suhas Joshi z wyjaśnieniem dobre `UserManager` klasy.
