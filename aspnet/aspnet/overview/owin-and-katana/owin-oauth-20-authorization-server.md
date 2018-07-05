---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Serwer autoryzacji OAuth 2.0 OWIN | Dokumentacja firmy Microsoft
author: hongyes
description: Ten samouczek przeprowadzi Cię o tym, jak wdrożyć serwer autoryzacji OAuth 2.0 przy użyciu oprogramowania pośredniczącego uwierzytelniania OWIN OAuth. To jest to zaawansowane samouczek z tej tylko outlin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: d2c5e984fb5866ab4bc01e10600bad4ee88bd4ed
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387341"
---
<a name="owin-oauth-20-authorization-server"></a>Serwer autoryzacji OAuth 2.0 OWIN
====================
przez [Hongye Sun](https://github.com/hongyes), [projektu Praburaj](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek przeprowadzi Cię o tym, jak wdrożyć serwer autoryzacji OAuth 2.0 przy użyciu oprogramowania pośredniczącego uwierzytelniania OWIN OAuth. Jest to zaawansowane samouczek, który przedstawia tylko kroki, aby utworzyć serwer autoryzacji OWIN OAuth 2.0. Nie jest to samouczek krok po kroku. [Pobierz przykładowy kod](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Ten konspekt powinien mieć na nie ma być używany do tworzenia aplikacji produkcyjnych bezpieczne. Ten samouczek jest przeznaczony do świadczenia tylko konspekt w sposób implementacji serwera autoryzacji OAuth 2.0 przy użyciu oprogramowania pośredniczącego uwierzytelniania OWIN OAuth.
> 
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Pokazane w tym samouczku** | **Współpracuje również z** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Program Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 z najnowszymi aktualizacjami powinna działać, ale samouczka nie był testowany z nim, a niektóre opcje menu i okien dialogowych różnią się. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je pod [projektu Katana w usłudze GitHub](https://github.com/aspnet/AspNetKatana/). Pytania i komentarze dotyczące tego samouczka, sama sekcja komentarze w dolnej części strony.


[Framework OAuth 2.0](http://tools.ietf.org/html/rfc6749) umożliwia aplikacji innych firm uzyskać ograniczony dostęp do usługi HTTP. Zamiast używania poświadczenia właściciela zasobu, aby uzyskać dostęp do chronionego zasobu, klient uzyskuje token dostępu (czyli ciąg oznaczający określonego zakresu, okres istnienia i inne atrybuty dostępu). Tokeny dostępu są wydawane klienci firm przez serwer autoryzacji z zatwierdzeniem właściciela zasobów.

W tym samouczku omówiono:

- Jak utworzyć serwer autoryzacji do obsługi autoryzacji cztery typy przydziałów i tokenów odświeżania:
    - Przyznawanie kodu autoryzacji
    - Przyznawanie niejawne
    - Hasło właściciela zasobu poświadczenia przydział
    - Przydział poświadczeń klienta
- Tworzenie serwera zasobów, która jest chroniona przez token dostępu.
- Tworzenie klientów uwierzytelniania OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Wymagania wstępne

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) lub bezpłatnych [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), jak wskazano w **wersje oprogramowania** w górnej części strony.
- Znajomość OWIN. Zobacz [wprowadzenie do projektu Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) i [What's new in OWIN i Katana](index.md).
- Znajomość [OAuth](http://tools.ietf.org/html/rfc6749) terminologii, w tym [role](http://tools.ietf.org/html/rfc6749#section-1.1), [przepływ protokołu](http://tools.ietf.org/html/rfc6749#section-1.2), i [autoryzację](http://tools.ietf.org/html/rfc6749#section-1.3). [Wprowadzenie protokołu OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) stanowi dobry wprowadzenie.

## <a name="create-an-authorization-server"></a>Tworzenie serwera autoryzacji

W tym samouczku zostanie możemy około szkic informacje o tym, jak używać [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) i ASP.NET MVC w celu utworzenia serwera autoryzacji. Mamy nadzieję, że wkrótce udostępni dostępny do pobrania dla przykładu ukończone, ponieważ ten samouczek zawiera każdy krok. Najpierw utwórz pustą aplikację internetową o nazwie *AuthorizationServer* i zainstaluj następujące pakiety:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (lub innych społecznościowych logowania pakietów, takich jak Microsoft.Owin.Security.Facebook)

Dodaj [Klasa początkowa OWIN](owin-startup-class-detection.md) w folderze głównym projektu o nazwie *uruchamiania*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Tworzenie *aplikacji\_Start* folderu. Wybierz *aplikacji\_Start* folder i użyj Shift + Alt + A, aby dodać wersję pobrany *AuthorizationServer\App\_Start\Startup.Auth.cs* pliku.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Powyższy kod umożliwia logowanie aplikacji/zewnętrzną pliki cookie i uwierzytelniania serwisu Google, które są używane przez serwer autoryzacji do zarządzania kontami.

`UseOAuthAuthorizationServer` — Metoda rozszerzenia jest konfiguracja serwera autoryzacji. Dostępne są opcje instalacji:

- `AuthorizeEndpointPath`Zgoda ścieżka żądania, w którym aplikacja kliencka przekieruje agenta użytkownika w celu uzyskania użytkowników można wystawić tokenu lub kodu. Musi zaczynać się ukośnika, na przykład "`/Authorize`".
- `TokenEndpointPath`Ścieżka żądania aplikacji klienckiej komunikują się bezpośrednio do uzyskania tokenu dostępu. Musi zaczynać się ukośnika, np. "/ Token". Jeśli klient wystawił [klienta\_klucz tajny](http://tools.ietf.org/html/rfc6749#appendix-A.2), musi on zostać udostępniony do tego punktu końcowego.
- `ApplicationCanDisplayErrors`: Ustaw `true` Jeśli aplikacja sieci web chce, aby wygenerować niestandardowej strony błędu dla błędów sprawdzania poprawności klienta na `/Authorize` punktu końcowego. Jest to tylko potrzebne dla przypadków, gdy przeglądarka nie zostanie przekierowana z powrotem do aplikacji klienckiej, na przykład, gdy `client_id` lub `redirect_uri` są nieprawidłowe. `/Authorize` Punktu końcowego powinno pojawić się "oauth. Błąd","protokołu oauth. ErrorDescription"i"protokołu oauth. Właściwości ErrorUri"są dodawane do środowiska OWIN. 

    > [!NOTE]
    > W przeciwnym razie ma wartość true, serwer autoryzacji zwróci domyślnej strony błędu przy użyciu szczegółów błędu.
- `AllowInsecureHttp`: Wartość True Aby zezwolić na żądań autoryzacji i tokena pojawić się na adresy HTTP identyfikatora URI i zezwolić na przychodzący `redirect_uri` autoryzować parametry żądania, aby mieć adresy HTTP identyfikatora URI. 

    > [!WARNING]
    > Zabezpieczenia — dotyczy to tylko rozwoju.
- `Provider`: Obiekt udostępniany przez aplikację do przetwarzania zdarzeń zgłaszanych przez oprogramowanie pośredniczące serwera autoryzacji. Aplikacja może wdrożyć interfejs w całości lub może utworzyć wystąpienie `OAuthAuthorizationServerProvider` i przypisać delegatów niezbędne dla przepływów uwierzytelniania OAuth, ten serwer obsługuje.
- `AuthorizationCodeProvider`: Tworzy kod autoryzacji jednorazowy, aby powrócić do aplikacji klienckiej. W przypadku serwera OAuth, aby był zabezpieczyć aplikację **musi** udostępniać wystąpienie dla `AuthorizationCodeProvider` gdzie token utworzony przez `OnCreate/OnCreateAsync` zdarzenie jest traktowane jako prawidłowe dla tylko jedno wywołanie `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Tworzy token odświeżania, która może być użyta do wyprodukowania nowy token dostępu w razie. Jeśli nie podano serwera autoryzacji nie zwróci tokenów odświeżania z `/Token` punktu końcowego.

## <a name="account-management"></a>Zarządzanie kontami

OAuth zależy, gdzie i jak zarządzać informacje o koncie użytkownika. Ma ona [produktu ASP.NET Identity](../../../identity/index.md) który jest odpowiedzialny za go. W tym samouczku Rozpoczniemy upraszczają kod zarządzania kontem i po prostu upewnij się, że użytkownik może logować się za pomocą oprogramowania pośredniczącego OWIN pliku cookie. Oto uproszczony przykładowego kodu na potrzeby `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` Służy do sprawdzania klienta za pomocą jej adresu URL przekierowania zarejestrowany. `ValidateClientAuthentication` sprawdza, czy schemat podstawowe nagłówek i treść formularza, aby uzyskać poświadczenia klienta.

Na stronie logowania znajdują się poniżej:

![](owin-oauth-20-authorization-server/_static/image1.png)

Przejrzyj IETF protokołu OAuth 2 [przyznawania kodu autoryzacji](http://tools.ietf.org/html/rfc6749#section-4.1) sekcji teraz. 

**Dostawca** (w poniższej tabeli) to [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Dostawca, który jest typu `OAuthAuthorizationServerProvider`, który zawiera wszystkie zdarzenia serwera OAuth. 

| Przepływ kroków z sekcji przyznawania kodu autoryzacji | Pobieranie przykładowych wykonuje te czynności przy użyciu: |
| --- | --- |
|  |  |
| (A klient inicjuje przepływ, kierując właściciel zasobu agenta użytkownika do punktu końcowego autoryzacji. Klient dołącza identyfikator klienta, żądanym zakresie, stan lokalnego i identyfikator URI przekierowania, do którego serwer autoryzacji wyśle do agenta użytkownika, ponownie, gdy dostęp jest udzielany (lub odmowa). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) serwer autoryzacji uwierzytelnia właściciel zasobu (za pośrednictwem agenta użytkownika) i określa, czy właściciel zasobu lub go przydziela odrzuca żądanie dostępu klienta. | **&lt;Jeśli użytkownik udziela dostępu&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) przy założeniu właściciel zasobu przyzna dostęp, serwer autoryzacji przekieruje agenta użytkownika do klienta przy użyciu podanego identyfikatora URI przekierowania wcześniej (w żądaniu lub podczas rejestracji klienta). ... |  |
|  |  |
| (D) klient żąda tokenu dostępu od punktu końcowego tokenu serwera autoryzacji, dołączając kod autoryzacji odebrany w poprzednim kroku. Podczas wykonywania żądania, klient uwierzytelnia się za pomocą serwera autoryzacji. Klient dołącza przekierowania URI używany do uzyskania kodu autoryzacji do weryfikacji. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Przykładowe zastosowanie dla `AuthorizationCodeProvider.CreateAsync` i `ReceiveAsync` do kontrolowania, tworzenie i sprawdzanie poprawności kodu autoryzacji znajdują się poniżej.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Powyższy kod używa współbieżny słownik w pamięci do przechowywania biletu kodu i tożsamości i przywrócić tożsamość po otrzymaniu kod. W rzeczywistej aplikacji może być zastąpione przez w trwałym magazynie danych. Punkt końcowy autoryzacji jest dla właściciela zasobu udzielić dostępu do klienta. Zwykle wymaga interfejsu użytkownika, aby zezwolić użytkownikowi na kliknięciu przycisku i upewnij się, Przydziel. Oprogramowanie pośredniczące OWIN OAuth umożliwia aplikacji kod służący do obsługi punktu końcowego autoryzacji. W naszej aplikacji przykładowych używamy kontroler MVC wywołuje `OAuthController` do jego obsługi. Poniżej przedstawiono przykładowe zastosowanie:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize` Najpierw sprawdzić w akcji, jeśli użytkownik jest zalogowany do autoryzacji serwera. Jeśli nie, oprogramowanie pośredniczące uwierzytelniania wyzwań obiekt wywołujący do uwierzytelniania za pomocą plików cookie "Application" i wykonuje przekierowanie do strony logowania. (Zobacz wyróżniony kod powyżej). Jeśli użytkownik jest zalogowany, renderowanie zostanie przeprowadzone w widoku autoryzacji, jak pokazano poniżej:

![](owin-oauth-20-authorization-server/_static/image2.png)

Jeśli **Grant** przycisk jest zaznaczony, `Authorize` akcja spowoduje utworzenie nowej tożsamości "Bearer" i zaloguj się przy użyciu go. Wywoła serwera autoryzacji do wygenerowania tokenu elementu nośnego i wysyłania jej do klienta przy użyciu ładunku JSON. 

### <a name="implicit-grant"></a>Przyznawanie niejawne

Zobacz organizację IETF protokołu OAuth 2 [przyznawanie niejawne](http://tools.ietf.org/html/rfc6749#section-4.2) sekcji teraz.

 [Przyznawanie niejawne](http://tools.ietf.org/html/rfc6749#section-4.2) przepływ pokazano na rysunku 4 przedstawiono przepływ i mapowania, które OAuth interfejsu OWIN oprogramowanie pośredniczące jest zgodna.  

| Przepływ kroków z sekcji przyznawanie niejawne | Pobieranie przykładowych wykonuje te czynności przy użyciu: |
| --- | --- |
|  |  |
| (A klient inicjuje przepływ, kierując właściciel zasobu agenta użytkownika do punktu końcowego autoryzacji. Klient dołącza identyfikator klienta, żądanym zakresie, stan lokalnego i identyfikator URI przekierowania, do którego serwer autoryzacji wyśle do agenta użytkownika, ponownie, gdy dostęp jest udzielany (lub odmowa). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) serwer autoryzacji uwierzytelnia właściciel zasobu (za pośrednictwem agenta użytkownika) i określa, czy właściciel zasobu lub go przydziela odrzuca żądanie dostępu klienta. | **&lt;Jeśli użytkownik udziela dostępu&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) przy założeniu właściciel zasobu przyzna dostęp, serwer autoryzacji przekieruje agenta użytkownika do klienta przy użyciu podanego identyfikatora URI przekierowania wcześniej (w żądaniu lub podczas rejestracji klienta). ... |  |
|  |  |
| (D) klient żąda tokenu dostępu od punktu końcowego tokenu serwera autoryzacji, dołączając kod autoryzacji odebrany w poprzednim kroku. Podczas wykonywania żądania, klient uwierzytelnia się za pomocą serwera autoryzacji. Klient dołącza przekierowania URI używany do uzyskania kodu autoryzacji do weryfikacji. |  |

Ponieważ wprowadziliśmy punkt końcowy autoryzacji (`OAuthController.Authorize` akcji) dla przyznawanie kodu autoryzacji automatycznie umożliwia także niejawny przepływ. Uwaga: `Provider.ValidateClientRedirectUri` służy do sprawdzania poprawności Identyfikatora klienta z jej Przekierowywanie adresu URL, który chroni przepływie przyznawania niejawnego wysyłanie tokenu do złośliwego klientów dostępu ([atak typu Man-in--middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Hasło właściciela zasobu poświadczenia przydział

Zobacz organizację IETF protokołu OAuth 2 [przydział poświadczeń hasła właściciela zasobu](http://tools.ietf.org/html/rfc6749#section-4.3) sekcji teraz.

 [Przydział poświadczeń hasła właściciela zasobu](http://tools.ietf.org/html/rfc6749#section-4.3) przepływ pokazano na rysunku 5 jest przepływ i mapowania, które OAuth interfejsu OWIN oprogramowanie pośredniczące jest zgodna.  

| Przepływ kroków z sekcji przydział poświadczeń hasła właściciela zasobu | Pobieranie przykładowych wykonuje te czynności przy użyciu: |
| --- | --- |
|  |  |
| (A) właściciel zasobu przekazuje temu klientowi swoją nazwę użytkownika i hasło. |  |
|  |  |
| (B) klient żąda tokenu dostępu od punktu końcowego tokenu serwera autoryzacji, dołączając poświadczenia otrzymane od właściciela zasobu. Podczas wykonywania żądania, klient uwierzytelnia się za pomocą serwera autoryzacji. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) serwer autoryzacji uwierzytelnia klienta i weryfikuje poświadczenia właściciela zasobu, a jeśli są one prawidłowe, wystawia token dostępu. |  |

Poniżej przedstawiono przykładowe zastosowanie dla `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Powyższy kod jest przeznaczony do wyjaśnienia tej części samouczka i nie należy używać w bezpiecznym lub aplikacje produkcyjne. Nie sprawdza poświadczenia właścicieli zasobu. Zakłada się, co poświadczeń jest prawidłowy i tworzy nową tożsamość. Nową tożsamość będzie służyć do generowania tokenu dostępu i token odświeżania. Zamień kod na Twój własny kod do zarządzania bezpiecznym kontem.


### <a name="client-credentials-grant"></a>Przydział poświadczeń klienta

Zobacz organizację IETF protokołu OAuth 2 [przyznanie poświadczenia klienta](http://tools.ietf.org/html/rfc6749#section-4.4) sekcji teraz.

 [Przyznanie poświadczenia klienta](http://tools.ietf.org/html/rfc6749#section-4.4) przepływ pokazano na rysunku 6 jest przepływ i mapowania, które OAuth interfejsu OWIN oprogramowanie pośredniczące jest zgodna.  

| Przepływ kroków z sekcji przyznanie poświadczenia klienta | Pobieranie przykładowych wykonuje te czynności przy użyciu: |
| --- | --- |
|  |  |
| (A) klient uwierzytelnia się za pomocą serwera autoryzacji i żąda tokenu dostępu z punktu końcowego tokenu. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) serwer autoryzacji uwierzytelnia klienta, a jeśli są one prawidłowe, wystawia token dostępu. |  |

Poniżej przedstawiono przykładowe zastosowanie dla `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Powyższy kod jest przeznaczony do wyjaśnienia tej części samouczka i nie należy używać w bezpiecznym lub aplikacje produkcyjne. Zamień kod na własnych bezpiecznego zarządzania kod klienta.


### <a name="refresh-token"></a>Token odświeżania

Zobacz organizację IETF protokołu OAuth 2 [odświeżanie tokenu](http://tools.ietf.org/html/rfc6749#section-1.5) sekcji teraz.

 [Odświeżanie tokenu](http://tools.ietf.org/html/rfc6749#section-1.5) przepływ pokazano na rysunku 2 przedstawiono przepływ i mapowania, które OAuth interfejsu OWIN oprogramowanie pośredniczące jest zgodna.  

| Przepływ kroków z sekcji przyznanie poświadczenia klienta | Pobieranie przykładowych wykonuje te czynności przy użyciu: |
| --- | --- |
|  |  |
| (G) klient żąda nowy token dostępu przez uwierzytelnianie za pomocą serwera autoryzacji i prezentowanie token odświeżania. Wymagania dotyczące uwierzytelniania klienta są oparte na typ klienta i zasady serwera autoryzacji. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| H serwer autoryzacji uwierzytelnia klienta i weryfikuje token odświeżania, a jeśli są one prawidłowe, wystawia nowy token dostępu (i, opcjonalnie, nowego tokena odświeżania). |  |

Poniżej przedstawiono przykładowe zastosowanie dla `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Utwórz serwer zasobów, która jest chroniona przez Token dostępu

Utwórz pusty projekt sieci web app i zainstaluj następujące pakiety w projekcie:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Klasa początkowa i tworzyć i konfigurować uwierzytelnianie interfejsu API sieci Web. Zobacz *AuthorizationServer\ResourceServer\Startup.cs* do pobrania próbki.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Zobacz *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* do pobrania próbki.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Zobacz *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* do pobrania próbki.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` Metoda umożliwia CORS dla wszystkich domen.
- `UseOAuthBearerAuthentication` Metoda umożliwia OAuth elementu nośnego tokenu pośredniczącego, które będą odbierać i sprawdzanie poprawności tokenu elementu nośnego z nagłówka autoryzacji w żądaniu.
- `Config.SuppressDefaultHostAuthenticaiton` Pomija domyślne hosta uwierzytelniony podmiot zabezpieczeń z poziomu aplikacji, dlatego wszystkie żądania będą anonimowy po tym wywołaniu.
- `HostAuthenticationFilter` Umożliwia użycie uwierzytelniania tylko dla określonego typu uwierzytelniania. W tym przypadku jest to typ uwierzytelniania elementu nośnego.

W celu przedstawienia uwierzytelnionej tożsamości, możemy utworzyć klasy ApiController w danych wyjściowych oświadczeń bieżącego użytkownika.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Serwer autoryzacji i serwer zasobów nie znajdują się na tym samym komputerze, oprogramowanie pośredniczące uwierzytelniania OAuth użyje klucze inną maszynę do szyfrowania i odszyfrowywania tokenu dostępu do elementu nośnego. Aby współużytkować ten sam klucz prywatny między obu projektów, możemy dodać takie same `machinekey` ustawienie w obu *web.config* plików.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Tworzenie klientów uwierzytelniania OAuth 2.0

 Używamy [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) pakietu NuGet w celu uproszczenia kodu klienta.

### <a name="authorization-code-grant-client"></a>Klient Grant kod autoryzacji

 Ten klient jest aplikacji MVC. Wywoła przepływie przyznawania kodu autoryzacji można uzyskać tokenu dostępu z wewnętrznej bazy danych. Ma jednej strony, jak pokazano poniżej:

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Autoryzuj** przycisku spowoduje przekierowanie przeglądarki do serwera autoryzacji, aby powiadomić właściciela zasobu, aby udzielić dostępu do tego klienta.
- **Odśwież** przycisk otrzyma nowy token dostępu i token odświeżania za pomocą bieżące odświeżanie tokenu.
- **Interfejsu API dostępu do chronionych zasobów** przycisk będzie wywoływać serwer zasobów, aby pobrać dane oświadczeń bieżącego użytkownika i wyświetlić je na stronie.

Poniżej przedstawiono przykładowy kod z `HomeController` klienta.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` wymaga protokołu SSL, domyślnie. Ponieważ naszą wersję demonstracyjną, korzysta z protokołu HTTP, należy dodać następujące ustawienia w pliku konfiguracji:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Zabezpieczenia — nigdy nie wyłączenie protokołu SSL w aplikacji produkcyjnej. Poświadczenia logowania są teraz wysyłane w postaci zwykłego tekstu w sieci. Powyższy kod jest tylko przykładowe lokalne debugowanie i eksploracji.


### <a name="implicit-grant-client"></a>Niejawne przyznanie klienta

Ten klient używa języka JavaScript, aby:

1. Otwórz nowe okno i Przekierowanie do punktu końcowego autoryzacji serwera autoryzacji.
2. Uzyskaj token dostępu z adresu URL fragmentów, gdy zostanie przekierowany ponownie.

Na poniższej ilustracji przedstawiono ten proces:

![](owin-oauth-20-authorization-server/_static/image4.png)

Klient powinien mieć dwie strony: jeden dla strony głównej i inne wywołania zwrotnego. Poniżej przedstawiono przykładowy kod JavaScript znaleźć kodu w *Index.cshtml* pliku:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

W tym miejscu to wywołanie zwrotne, Obsługa kodu w *SignIn.cshtml* pliku:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Najlepszym rozwiązaniem jest Przenieś kod JavaScript do pliku zewnętrznego i osadzaj je ze znacznikami Razor. W celu uproszczenia w tym przykładzie zostały one połączone.


### <a name="resource-owner-password-credentials-grant-client"></a>Hasło właściciela zasobu poświadczeń klienta przydział

Firma Microsoft korzysta z aplikacji konsoli na targach tego klienta. Oto kod:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Klient przydział poświadczeń klienta

Podobnie jak przydział poświadczeń hasła właściciela zasobu, poniżej przedstawiono kod aplikacji konsoli:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
