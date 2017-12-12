---
uid: web-api/overview/security/individual-accounts-in-web-api
title: "Zabezpieczanie interfejsu API sieci Web z indywidualnych kont i logowania lokalnego w składniku ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym temacie pokazano, jak zabezpieczyć interfejs API sieci web przy użyciu protokołu OAuth2 do uwierzytelniania na bazie danych członkostwa. Używane w samouczek Visual Studio 201 wersje oprogramowania..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8207df79c1e915b33a0ba095d917a6dc69550173
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Zabezpieczanie interfejsu API sieci Web z indywidualnych kont i logowania lokalnego w składniku ASP.NET Web API 2.2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobierz przykładową aplikację](https://github.com/MikeWasson/LocalAccountsApp)

> W tym temacie pokazano, jak zabezpieczyć interfejs API sieci web przy użyciu protokołu OAuth2 do uwierzytelniania na bazie danych członkostwa.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [2.2 interfejsu API sieci Web](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


W programie Visual Studio 2013 szablon projektu interfejsu API sieci Web oferuje trzy opcje uwierzytelniania:

- **Indywidualne konta.** Aplikacja korzysta z bazy danych członkostwa.
- **Konta organizacyjne.** Użytkownicy Zaloguj się przy użyciu ich usługi Azure Active Directory, usługi Office 365 lub lokalnego poświadczeń usługi Active Directory.
- **Uwierzytelnianie systemu Windows.** Ta opcja jest przeznaczona dla aplikacji sieci Intranet i używa modułu IIS uwierzytelniania systemu Windows.

Aby uzyskać więcej informacji o tych opcjach, zobacz [tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Indywidualnych kont dostarcza użytkownikowi na logowanie na dwa sposoby:

- **Lokalny identyfikator logowania**. Użytkownik rejestruje w danej lokacji, wprowadzania nazwy użytkownika i hasła. Aplikacja przechowuje wartość skrótu hasła w bazie danych członkostwa. Gdy użytkownik się zaloguje, system ASP.NET Identity weryfikuje hasło.
- **Logowania społecznościowych**. Użytkownik zaloguje się za pomocą zewnętrznej usługi, takie jak Facebook, Microsoft lub Google. Aplikacja nadal tworzy wpis dla użytkownika w bazie danych członkostwa, ale nie przechowuje żadnych poświadczeń. Użytkownik jest uwierzytelniany po zalogowaniu się do zewnętrznej usługi.

W tym artykule analizuje scenariusz logowania lokalnego. Dla nazwy logowania zarówno lokalnych, jak i społecznościowych interfejsu API sieci Web używa protokołu OAuth2, do uwierzytelniania żądań. Jednak logowania lokalnego i społecznościowych różnią się przepływy poświadczeń.

W tym artykule I będzie pokazują prostej aplikacji, w którym użytkownik może zalogować się i wysyłać uwierzytelnione wywołania AJAX do interfejsu API sieci web. Możesz pobrać przykładowy kod [tutaj](https://github.com/MikeWasson/LocalAccountsApp). Plik readme zawiera opis sposobu tworzenia przykładowej od podstaw w programie Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Przykładowa aplikacja używa Knockout.js dla powiązania danych i jQuery podczas wysyłania żądania AJAX. I będzie można koncentrujących się na wywołania AJAX, więc nie trzeba znać Knockout.js tego artykułu.

Na bieżąco będzie I opisano:

- Aplikacja czynności po stronie klienta.
- Co się dzieje na serwerze.
- Ruch HTTP w środku.

Najpierw należy zdefiniować niektóre terminologii OAuth2.

- *Zasób*. Niektóre z nich dane, które mogą być chronione.
- *Serwer zasobów*. Serwer, który obsługuje zasobu.
- *Właściciel zasobu*. Jednostka, która może udzielić uprawnień do uzyskania dostępu do zasobu. (Zazwyczaj użytkownik.)
- *Klient*: aplikacja, która chce uzyskać dostęp do zasobu. W tym artykule klienta to przeglądarka sieci web.
- *Token dostępu*. Token, która udziela dostępu do zasobu.
- *Token elementu nośnego*. Określonego typu tokenu dostępu, razem z właściwością, że każda osoba, która może użyć tokenu. Innymi słowy klient nie wymaga klucza kryptograficznego lub innych klucz tajny, aby użyć tokenu elementu nośnego. Z tego powodu tokenów elementu nośnego powinien być używany tylko za pośrednictwem protokołu HTTPS, a powinien mieć stosunkowo krótkich okresach ważności.
- *Serwer autoryzacji*. Serwer, który daje się tokenów dostępu.

Aplikacja może działać jako serwer autoryzacji i serwer zasobów. Szablon projektu interfejsu API sieci Web wykonuje tego wzorca.

## <a name="local-login-credential-flow"></a>Przepływ poświadczeń logowania lokalnego

W przypadku logowania lokalnego interfejsu API sieci Web używa [przepływu hasło właściciela zasobów](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) zdefiniowane w OAuth2.

1. Użytkownik wprowadza nazwę i hasło do klienta.
2. Klient wysyła tych poświadczeń do serwera autoryzacji.
3. Serwer autoryzacji uwierzytelnia poświadczenia i zwraca token dostępu.
4. Aby uzyskać dostęp do chronionych zasobów, klient dołącza token dostępu w nagłówku autoryzacji żądania HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Po wybraniu **indywidualnych kont** w szablonie projektu interfejsu API sieci Web, projekt zawiera serwer autoryzacji, która weryfikuje poświadczenia użytkownika i wystawia tokeny. Na poniższym diagramie przedstawiono takim samym przepływie poświadczeń w postaci składniki interfejsu API sieci Web.

![](individual-accounts-in-web-api/_static/image4.png)

W tym scenariuszu kontrolerów interfejsu API sieci Web działa jako serwery zasobów. Filtr uwierzytelniania weryfikuje tokeny dostępu i **[Authorize]** atrybut służy do ochrony zasobu. Jeśli kontroler lub akcję zawiera **[Authorize]** atrybut, wszystkie żądania kierowane do tego kontrolera lub akcji musi zostać uwierzytelniony. W przeciwnym razie wartość odmowy autoryzacji i interfejsu API sieci Web zwraca błąd 401 (nieautoryzowanych).

Serwer autoryzacji i filtr uwierzytelnienia zarówno wywołują [oprogramowanie pośredniczące OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) składnik, który obsługuje szczegóły OAuth2. I będzie opisano projekt szczegółowo w dalszej części tego samouczka.

## <a name="sending-an-unauthorized-request"></a>Wysyłanie nieautoryzowanego żądania

Aby rozpocząć, uruchom aplikację, a następnie kliknij przycisk **wywołania interfejsu API** przycisku. Po zakończeniu żądania, powinien zostać wyświetlony komunikat o błędzie w **wynik** pole. To, ponieważ żądanie nie zawiera tokenu dostępu, więc żądanie jest autoryzowane.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**Wywołania interfejsu API** przycisk wysyła żądanie AJAX ~/api/wartości, który wywołuje akcję kontrolera interfejsu API sieci Web. Oto sekcji kodu JavaScript, która wysyła żądanie AJAX. W przykładowej aplikacji cały kod JavaScript aplikacji znajduje się w pliku Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Dopóki użytkownik się zaloguje, brak nie tokenu elementu nośnego i w związku z tym nie nagłówek autoryzacji w żądaniu. Powoduje to żądanie zwracany jest błąd 401.

Oto żądania HTTP. (Użycie [Fiddler](http://www.telerik.com/fiddler) do przechwytywania ruchu HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Zwróć uwagę, że odpowiedź zawiera nagłówek Www-Authenticate z żądaniem ustawioną elementu nośnego. Wskazuje, że serwer oczekuje tokenu elementu nośnego.

## <a name="register-a-user"></a>Zarejestruj użytkownika

W **zarejestrować** sekcji aplikacji, wprowadź adres e-mail i hasło, a następnie kliknij przycisk **zarejestrować** przycisku.

Nie trzeba używać prawidłowy adres e-mail dla tego przykładu, ale rzeczywistym aplikacji potwierdzenie adresu. (Zobacz [utworzenia bezpiecznego aplikacji sieci web platformy ASP.NET MVC 5 z dziennika w resetowania hasła i potwierdzania poczty e-mail](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Hasła należy użyć ciągu "Password1!", z wielkie litery, małe litery, liczby i znaków innych niż alfanumeryczne. Aby zachować proste aplikacji, po lewej I limit weryfikacji po stronie klienta, więc w przypadku problemu z formatu hasła, zostanie wyświetlony błąd 400 (nieprawidłowe żądanie).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**Zarejestrować** przycisku spowoduje wysłanie żądania POST do ~/api/Account/Register /. Treść żądania jest obiekt JSON, który zawiera nazwę i hasło. Oto kod JavaScript, który wysyła żądanie:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Żądania HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

To żądanie jest obsługiwane przez `AccountController` klasy. Wewnętrznie `AccountController` tożsamość ASP.NET będzie używana do zarządzania bazą danych członkostwa.

Jeśli lokalne uruchamianie aplikacji w programie Visual Studio konta użytkowników są przechowywane w LocalDB, w tabeli AspNetUsers. Aby wyświetlić tabele programu Visual Studio, kliknij przycisk **widoku** menu, wybierz opcję **Eksploratora serwera**, następnie rozwiń węzeł **połączenia danych**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Uzyskaj Token dostępu

Do tej pory firma Microsoft nie zostało zrobione żadnych OAuth, ale teraz zajmiemy się serwera autoryzacji OAuth akcji, gdy żąda tokenu dostępu. W **dziennika w** obszaru przykładowej aplikacji, wprowadź adres e-mail i hasło i kliknij przycisk **dziennika w**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**Dziennika w** przycisk wysyła żądanie do punktu końcowego tokena. Treść żądania zawiera następujące dane formularza zakodowanych w adresie url:

- Przyznaj\_typu: "password"
- Nazwa użytkownika: &lt;adres e-mail użytkownika&gt;
- hasło: &lt;hasła&gt;

Oto kod JavaScript, która wysyła żądanie AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Jeśli żądanie zakończy się powodzeniem, serwer autoryzacji zwraca token dostępu w treści odpowiedzi. Zwróć uwagę, że token są przechowywane w magazynie sesji, do użycia w przyszłości podczas wysyłania żądań do interfejsu API. Inaczej niż w przypadku niektórych metod uwierzytelniania (na przykład uwierzytelnianie na podstawie plików cookie) przeglądarka nie będzie automatycznie zawierać token dostępu w kolejnych żądań. Aplikacja musi to zrobić jawnie. Że jest to przydatne, ponieważ jest ograniczone [luk w zabezpieczeniach CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Żądania HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Widać, że żądanie zawiera poświadczenia użytkownika. Możesz *musi* Podaj zabezpieczeń warstwy transportu przy użyciu protokołu HTTPS.

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Aby zwiększyć czytelność I wcięty JSON i obcięty tokenu dostępu, który jest bardzo długa.

`access_token`, `token_type`, I `expires_in` właściwości są definiowane przez specyfikację OAuth2. Inne właściwości (`userName`, `.issued`, i `.expires`) są tylko do celów informacyjnych. Możesz znaleźć kod, który dodaje te dodatkowe właściwości w `TokenEndpoint` metody w pliku /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Wyślij żądanie uwierzytelniony

Teraz, gdy mamy tokenu elementu nośnego, firma Microsoft może wprowadzać żądania uwierzytelnionego do interfejsu API. Jest to realizowane przez ustawienie nagłówek autoryzacji w żądaniu. Kliknij przycisk **wywołania interfejsu API** przycisk ponownie, aby zobaczyć.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Żądania HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Odpowiedź HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Wyloguj się

Ponieważ przeglądarka nie buforują poświadczeń lub tokenu dostępu, wylogowaniu jest po prostu "zapomniane" token, usuwając go z magazynu sesji:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Opis szablonu projektu indywidualnych kont

Po wybraniu **indywidualnych kont** w szablonie projektu aplikacji sieci Web ASP.NET projekt zawiera:

- OAuth2 serwera autoryzacji.
- Punkt końcowy interfejsu API sieci Web do zarządzania kontami użytkowników
- Model EF do przechowywania kont użytkowników.

Poniżej przedstawiono klasy głównym aplikacji, które implementują tych funkcji:

- `AccountController`. Udostępnia punkt końcowy interfejsu API sieci Web do zarządzania kontami użytkowników. `Register` Akcja jest jedyną, które zostały użyte w tym samouczku. Inne metody w klasie obsługuje resetowania hasła, logowania społecznościowych i innych funkcji.
- `ApplicationUser`, zdefiniowane w /Models/IdentityModels.cs. Ta klasa jest model EF dla kont użytkowników w bazie danych członkostwa.
- `ApplicationUserManager`, zdefiniowane w App\_Start/IdentityConfig.cs ta klasa pochodzi od [interfejs UserManager](https://msdn.microsoft.com/en-us/library/dn613290.aspx) i wykonuje operacje na kontach użytkowników, takich jak tworzenie nowego użytkownika, weryfikowanie hasła i tak dalej i automatycznie będzie się powtarzać zmiany w bazie danych.
- `ApplicationOAuthProvider`. Ten obiekt podłącza się do oprogramowania pośredniczącego OWIN i przetwarzania zdarzeń zgłaszanych przez oprogramowanie pośredniczące. Dziedziczy [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/en-us/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Konfigurowanie serwera autoryzacji

W StartupAuth.cs poniższy kod konfiguruje OAuth2 serwera autoryzacji.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath` Właściwość jest ścieżkę URL do serwera punktu końcowego autoryzacji. Adres URL, który jest używa danej aplikacji, aby uzyskać tokenów elementu nośnego.

`Provider` Właściwość określa dostawcę, który podłącza się do oprogramowania pośredniczącego OWIN i przetwarzania zdarzeń zgłaszanych przez oprogramowanie pośredniczące.

Poniżej przedstawiono podstawowy przepływ, gdy aplikacja chce uzyskać token:

1. Aby uzyskać token dostępu, aplikacja wysyła żądanie ~ / Token.
2. Wywołania oprogramowanie pośredniczące uwierzytelniania OAuth `GrantResourceOwnerCredentials` dla dostawcy.
3. Wywołania dostawcy `ApplicationUserManager` do zweryfikowania poświadczeń i tworzyć tożsamość oświadczeń.
4. Jeśli który zakończy się powodzeniem, Dostawca tworzy bilet uwierzytelniania, który jest używany do wygenerowania tokenu.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Oprogramowanie pośredniczące uwierzytelniania OAuth nie może ustalić żadnych informacji o kontach użytkownika. Dostawca komunikuje się między oprogramowanie pośredniczące i ASP.NET Identity. Aby uzyskać więcej informacji o implementacji serwera autoryzacji, zobacz [OWIN OAuth 2.0 autoryzacji serwera](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Konfigurowanie interfejsu API sieci Web za pomocą tokenów Bearer

W `WebApiConfig.Register` metody następujący kod konfiguruje potok składnika Web API uwierzytelniania:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter** klasa umożliwia uwierzytelnianie za pomocą tokenów elementu nośnego.

**SuppressDefaultHostAuthentication** metody informuje interfejsu API sieci Web, aby zignorować żadnego uwierzytelniania, która jest wywoływana przed żądanie dotrze potok składnika Web API, przez usługi IIS lub przez oprogramowanie pośredniczące OWIN. W ten sposób stosujemy ograniczenia interfejsu API sieci Web do uwierzytelniania tylko za pomocą tokenów elementu nośnego.

> [!NOTE]
> W szczególności MVC części aplikacji może używać uwierzytelniania formularzy, które przechowuje poświadczenia w pliku cookie. Plik cookie uwierzytelniania wymagane jest użycie tokenów zabezpieczających przed sfałszowaniem, aby zapobiec atakom CSRF. To problemu z interfejsów API, sieci web, ponieważ nie istnieje żadne wygodny sposób dla interfejsu API sieci web wysyła do klienta, token zabezpieczający przed sfałszowaniem. (Aby uzyskać więcej podstawowe informacje dotyczące tego problemu, zobacz [zapobieganie atakom CSRF w składniku Web API](preventing-cross-site-request-forgery-csrf-attacks.md).) Wywoływanie **SuppressDefaultHostAuthentication** gwarantuje, że interfejs API sieci Web nie jest narażony na ataki CSRF z poświadczeń przechowywanych w plikach cookie.


Gdy klient żąda zasobu chronionego, Oto co się stanie w potoku interfejsu API sieci Web:

1. **HostAuthentication** filtru wywołuje oprogramowanie pośredniczące uwierzytelniania OAuth do sprawdzania poprawności tokenu.
2. Oprogramowanie pośredniczące przetwarza token na tożsamość oświadczeń.
3. W tym momencie żądania jest *uwierzytelniony* , ale nie *autoryzowany*.
4. Filtr autoryzacji sprawdza, czy tożsamość oświadczeń. Jeśli oświadczenia autoryzację użytkownika dla tego zasobu, żądanie jest autoryzowane. Domyślnie **[Authorize]** atrybutu autoryzacji każde żądanie, który jest uwierzytelniony. Jednak można autoryzować przez rolę lub inne oświadczenia. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie i autoryzacja w składniku Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Jeśli powyższe kroki zostały wykonane pomyślnie, kontrolera zwraca chronionego zasobu. W przeciwnym razie klient odbiera 401 Błąd (nieautoryzowanych).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Tożsamość platformy ASP.NET](../../../identity/index.md)
- [Opis funkcji zabezpieczeń w szablonie SPA dla VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN w blogu przez Hongye Sun.
- [Pincety poszczególne interfejsu API sieci Web kont szablonu — część 2: kont lokalnych](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Wpis w blogu przez Dominick Baier.
- [Host uwierzytelniania i interfejsu API sieci Web z oprogramowaniem OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Dobrym wyjaśnienie `SuppressDefaultHostAuthentication` i `HostAuthenticationFilter` przez firmy Brock Allen.
- [Dostosowywanie informacji o profilu w produkcie ASP.NET Identity w szablonach VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). MSDN blogu Pranav Rastogi.
- [Na żądanie Zarządzanie okresem istnienia klasy interfejs UserManager w produkcie ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). MSDN wpis w blogu przez Suhas Joshi z dobrym wyjaśnienie `UserManager` klasy.
