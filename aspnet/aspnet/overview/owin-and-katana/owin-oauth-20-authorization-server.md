---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Serwera autoryzacji OAuth 2.0 OWIN | Dokumentacja firmy Microsoft
author: hongyes
description: "Ten samouczek przeprowadzi Cię implementowania serwera autoryzacji OAuth 2.0 za pomocą oprogramowania pośredniczącego OWIN OAuth. To jest zaawansowane samouczek tego tylko outlin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="owin-oauth-20-authorization-server"></a>Serwera autoryzacji OAuth 2.0 OWIN
====================
przez [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek przeprowadzi Cię implementowania serwera autoryzacji OAuth 2.0 za pomocą oprogramowania pośredniczącego OWIN OAuth. Jest to zaawansowane samouczek, tylko przedstawiono kroki, aby utworzyć serwer autoryzacji OWIN OAuth 2.0. Nie jest to samouczek krok po kroku. [Pobierz przykładowy kod](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Ten konspekt nie powinien mieć na celu można użyć do tworzenia aplikacji bezpiecznego produkcyjnej. W tym samouczku mają na celu dostarczenie tylko konspekt implementowania serwera autoryzacji OAuth 2.0 za pomocą oprogramowania pośredniczącego OWIN OAuth.
> 
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Wyświetlany w samouczku** | **Współdziała również z** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio Express 2013 for pulpitu](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Programu Visual Studio 2012 z najnowszymi aktualizacjami powinny działać, ale samouczek nie był testowany z nim, a niektóre opcje menu i okien dialogowych różnią się. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je pod adresem [Katana projektu w witrynie GitHub](https://github.com/aspnet/AspNetKatana/). Pytania i komentarze dotyczące samouczka się w sekcji uwag w dolnej części strony.


[Framework OAuth 2.0](http://tools.ietf.org/html/rfc6749) umożliwia aplikacji innych firm uzyskać ograniczony dostęp do usług HTTP. Zamiast przy użyciu poświadczeń właściciela zasobu, do uzyskania dostępu do chronionego zasobu, klient otrzymuje token dostępu (czyli ciągu określające określonego zakresu, okres istnienia i inne atrybuty dostępu). Tokeny dostępu są wydawane innych firm klientów przez serwer autoryzacji za zgodą właściciela zasobów.

W tym samouczku omówiono:

- Jak utworzyć serwer autoryzacji do obsługi autoryzacji cztery typy przydziałów i tokenów odświeżania:
    - Udzielania kodu autoryzacji
    - Niejawne Przyznaj
    - Udziel poświadczenia hasła właściciela zasobu
    - Udziel poświadczeń klienta
- Tworzenie serwera zasobu, która jest chroniona przez token dostępu.
- Tworzenie klientów uwierzytelniania OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Wymagania wstępne

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) lub wolnych [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), jak określono w **wersje oprogramowania** w górnej części strony.
- Znajomość OWIN. Zobacz [wprowadzenie projektu Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) i [What's new in OWIN i Katana](index.md).
- Znajomość [OAuth](http://tools.ietf.org/html/rfc6749) terminologii, łącznie z [ról](http://tools.ietf.org/html/rfc6749#section-1.1), [przepływ protokołu](http://tools.ietf.org/html/rfc6749#section-1.2), i [udzielania autoryzacji](http://tools.ietf.org/html/rfc6749#section-1.3). [Wprowadzenie protokołu OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) stanowi dobry wprowadzenie.

## <a name="create-an-authorization-server"></a>Tworzenie serwera autoryzacji

W tym samouczku zostanie możemy około szkic się, jak używać [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) i ASP.NET MVC do utworzenia serwera autoryzacji. Mamy nadzieję wkrótce zapewnienie pobieranie przykładowej ukończone, zgodnie z tego samouczka nie ma każdego kroku. Najpierw utwórz pustą aplikację sieci web o nazwie *AuthorizationServer* i zainstaluj następujące pakiety:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (lub inne społecznościowych logowania pakietu Microsoft.Owin.Security.Facebook np.)

Dodaj [klasy początkowej OWIN](owin-startup-class-detection.md) w folderze głównym projektu o nazwie *uruchamiania*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Utwórz *aplikacji\_Start* folderu. Wybierz *aplikacji\_Start* folder i użyj Shift + Alt + A, aby dodać wersję pobrany *AuthorizationServer\App\_Start\Startup.Auth.cs* pliku.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Powyższy kod umożliwia logowania aplikacji/zewnętrzne pliki cookie i uwierzytelniania serwisu Google, które są używane przez serwer autoryzacji do zarządzania kontami.

`UseOAuthAuthorizationServer` — Metoda rozszerzenia jest konfiguracja serwera autoryzacji. Dostępne są następujące opcje konfiguracji:

- `AuthorizeEndpointPath`Zgoda ścieżka żądania, gdzie aplikacja kliencka przekieruje agenta użytkownika w celu uzyskania użytkowników można wystawić tokenu lub kodu. Musi zaczynać się od ukośnika, na przykład "`/Authorize`".
- `TokenEndpointPath`Ścieżka żądania aplikacji klienckiej komunikują się bezpośrednio do uzyskania tokenu dostępu. Musi zaczynać się od ukośnika, takie jak "/ tokenu". Jeśli klient wystawił [klienta\_klucz tajny](http://tools.ietf.org/html/rfc6749#appendix-A.2), należy podać ten punkt końcowy.
- `ApplicationCanDisplayErrors`: Ustawioną `true` jeśli chce Generuj stronę błędu niestandardowego błędy weryfikacji w aplikacji sieci web `/Authorize` punktu końcowego. Wymagany tylko dla przypadków, gdy przeglądarka nie zostanie przekierowana z powrotem do aplikacji klienckiej, na przykład, jeśli `client_id` lub `redirect_uri` są nieprawidłowe. `/Authorize` Punktu końcowego oczekiwać "oauth. Błąd","oauth. ErrorDescription"i"oauth. Właściwości ErrorUri"są dodawane do środowiska OWIN. 

    > [!NOTE]
    > Jeśli nie ma wartość PRAWDA, serwer autoryzacji zwraca domyślną stronę błędów z szczegóły błędu.
- `AllowInsecureHttp`: True na Zezwalaj żądań autoryzacji i tokena odebranie na adresy HTTP identyfikatora URI i umożliwia przychodzące `redirect_uri` parametry mają adresy HTTP identyfikatora URI żądania autoryzacji. 

    > [!WARNING]
    > Zabezpieczenia — jest to tylko programowanie.
- `Provider`: Obiekt udostępniany przez aplikację do przetwarzania zdarzeń zgłaszanych przez oprogramowanie pośredniczące serwera autoryzacji. Aplikacja może wdrożyć interfejs w całości lub może utworzyć wystąpienia `OAuthAuthorizationServerProvider` i przypisać delegatów wymaganych przez ten serwer obsługuje przepływy OAuth.
- `AuthorizationCodeProvider`: Tworzy autoryzacji jednorazowy kod, aby powrócić do aplikacji klienckiej. Dla serwera OAuth, należy zabezpieczyć aplikacji **musi** zapewnić wystąpienie dla `AuthorizationCodeProvider` gdzie token utworzony przez `OnCreate/OnCreateAsync` zdarzeń jest uważany za ważny tylko dla jednego wywołania do `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Tworzy token odświeżania, która może być użyta do wyprodukowania nowy token dostępu w razie potrzeby. Jeśli nie podano serwera autoryzacji nie zwróci tokenów odświeżania z `/Token` punktu końcowego.

## <a name="account-management"></a>Zarządzanie kontami

OAuth nowe zależy, gdzie i jak zarządzać informacje o koncie użytkownika. Ma ona [ASP.NET Identity](../../../identity/index.md) który jest odpowiedzialny za go. W tym samouczku możemy uprościć kod zarządzania konta i po prostu upewnij się, że może się zalogować tego użytkownika za pomocą oprogramowania pośredniczącego OWIN pliku cookie. Oto kod przykładowy uproszczony `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`Służy do sprawdzania klienta z jego adres URL przekierowania zarejestrowany. `ValidateClientAuthentication`sprawdza podstawowy schemat nagłówek i treść formularza, aby uzyskać poświadczenia klienta.

Poniżej przedstawiono strony logowania:

![](owin-oauth-20-authorization-server/_static/image1.png)

Przejrzyj IETF OAuth 2 [udzielania kodu autoryzacji](http://tools.ietf.org/html/rfc6749#section-4.1) sekcji teraz. 

**Dostawca** (w poniższej tabeli) jest [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Dostawcy, który jest typu `OAuthAuthorizationServerProvider`, który zawiera wszystkie zdarzenia serwera OAuth. 

| Przepływ czynności z sekcji udzielania kodu autoryzacji | Pobieranie próbki wykonuje te kroki: |
| --- | --- |
|  |  |
| (A klientów inicjuje przepływ kierując agenta użytkownika właściciel zasobu do punktu końcowego autoryzacji. Klient dołącza identyfikatora klienta, żądany zakres stanu lokalnego i identyfikatora URI przekierowania, do której serwer autoryzacji wyśle agenta użytkownika Wstecz, gdy dostęp jest udzielono (lub odmówiono). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) serwera autoryzacji uwierzytelnia właściciela zasobów (za pomocą agenta użytkownika) i ustala, czy właściciel zasobu lub go przydziela nie zezwala na żądanie dostępu klienta. | **&lt;Jeśli użytkownik zezwala na dostęp&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) przy założeniu właściciel zasobu nieograniczony dostęp, serwer autoryzacji przekierowuje użytkownika agenta do klienta przy użyciu dostarczonego identyfikatora URI przekierowania wcześniejszych (w żądaniu lub podczas rejestracji klienta). ... |  |
|  |  |
| D klient żąda token dostępu z serwera autoryzacji punktu końcowego tokena umieszczając w niej kod autoryzacji, odebrany w poprzednim kroku. W żądaniu skierowanym, klient jest uwierzytelniany w usłudze serwera autoryzacji. Klient dołącza przekierowania, który identyfikator URI używany do uzyskania kodu autoryzacji do weryfikacji. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Przykładowe zastosowanie dla `AuthorizationCodeProvider.CreateAsync` i `ReceiveAsync` do kontrolowania tworzenia i weryfikacji kodu autoryzacji są wyświetlane poniżej.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Powyższy kod używa współbieżny słownik w pamięci do przechowywania biletu kod i tożsamości i przywrócić tożsamości po otrzymaniu kod. W rzeczywistej aplikacji może zostać zastąpione przez Magazyn danych. Punkt końcowy autoryzacji jest dla właściciela zasobu udzielić dostępu do klienta. Zazwyczaj wymaga interfejsu użytkownika, aby zezwolić użytkownikowi na kliknięcie przycisku i Potwierdź przyznanie. Oprogramowanie pośredniczące OWIN OAuth umożliwia kodu aplikacji do obsługi punktu końcowego autoryzacji. W naszym Przykładowa aplikacja używamy kontroler MVC wywołuje `OAuthController` do jego obsługi. Oto przykładowe zastosowanie:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize` Akcja zostanie najpierw sprawdza, jeśli użytkownik został zalogowany do serwera autoryzacji. Jeśli nie, oprogramowanie pośredniczące uwierzytelniania będzie wymagał obiekt wywołujący, aby przeprowadzić uwierzytelnianie przy użyciu pliku cookie "Aplikacja" i przekierowuje do strony logowania. (Zobacz wyróżniony kod powyżej). Jeśli użytkownik jest zalogowany, będą zawierały widoku autoryzacji, jak pokazano poniżej:

![](owin-oauth-20-authorization-server/_static/image2.png)

Jeśli **Grant** przycisk jest zaznaczony, `Authorize` akcja spowoduje utworzenie nowej tożsamości "Bearer" i zaloguj się przy użyciu go. Wyzwoli on serwera autoryzacji do wygenerowania tokenu elementu nośnego i wysyłania go do klienta z ładunek JSON. 

### <a name="implicit-grant"></a>Niejawne Przyznaj

Odwołuje się do grupy roboczej IETF OAuth 2 [niejawne Przyznaj](http://tools.ietf.org/html/rfc6749#section-4.2) sekcji teraz.

 [Niejawne Przyznaj](http://tools.ietf.org/html/rfc6749#section-4.2) przepływ pokazano na rysunku 4 jest przepływu i mapowania, które OAuth interfejsu OWIN oprogramowanie pośredniczące jest zgodna.  

| Przepływ czynności z sekcji niejawne Przyznaj | Pobieranie próbki wykonuje te kroki: |
| --- | --- |
|  |  |
| (A klientów inicjuje przepływ kierując agenta użytkownika właściciel zasobu do punktu końcowego autoryzacji. Klient dołącza identyfikatora klienta, żądany zakres stanu lokalnego i identyfikatora URI przekierowania, do której serwer autoryzacji wyśle agenta użytkownika Wstecz, gdy dostęp jest udzielono (lub odmówiono). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) serwera autoryzacji uwierzytelnia właściciela zasobów (za pomocą agenta użytkownika) i ustala, czy właściciel zasobu lub go przydziela nie zezwala na żądanie dostępu klienta. | **&lt;Jeśli użytkownik zezwala na dostęp&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) przy założeniu właściciel zasobu nieograniczony dostęp, serwer autoryzacji przekierowuje użytkownika agenta do klienta przy użyciu dostarczonego identyfikatora URI przekierowania wcześniejszych (w żądaniu lub podczas rejestracji klienta). ... |  |
|  |  |
| D klient żąda token dostępu z serwera autoryzacji punktu końcowego tokena umieszczając w niej kod autoryzacji, odebrany w poprzednim kroku. W żądaniu skierowanym, klient jest uwierzytelniany w usłudze serwera autoryzacji. Klient dołącza przekierowania, który identyfikator URI używany do uzyskania kodu autoryzacji do weryfikacji. |  |

Ponieważ wprowadziliśmy punktu końcowego autoryzacji (`OAuthController.Authorize` akcji) do udzielania kodu autoryzacji, automatycznie umożliwia również niejawnego przepływu. Uwaga: `Provider.ValidateClientRedirectUri` używany do weryfikowania Identyfikatora klienta z URL przekierowania, chroniącego przepływu niejawne Przyznaj wysyłania tokenu do złośliwych klientów dostępu ([ataku Man-in--middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Udziel poświadczenia hasła właściciela zasobu

Odwołuje się do grupy roboczej IETF OAuth 2 [Grant poświadczenia hasła właściciela zasobu](http://tools.ietf.org/html/rfc6749#section-4.3) sekcji teraz.

 [Grant poświadczenia hasła właściciela zasobu](http://tools.ietf.org/html/rfc6749#section-4.3) przepływ pokazano na rysunku 5 jest przepływu i mapowania, które OAuth interfejsu OWIN oprogramowanie pośredniczące jest zgodna.  

| Przepływ czynności z sekcji Grant poświadczenia hasła właściciela zasobu | Pobieranie próbki wykonuje te kroki: |
| --- | --- |
|  |  |
| (A) właściciel zasobu przekazuje temu klientowi swoją nazwę użytkownika i hasło. |  |
|  |  |
| (B) klient żąda token dostępu z serwera autoryzacji punktu końcowego tokena w tym poświadczenia otrzymanych od właściciela zasobów. W żądaniu skierowanym, klient jest uwierzytelniany w usłudze serwera autoryzacji. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) serwera autoryzacji uwierzytelnia klientów i sprawdza poprawność poświadczeń właściciela zasobu, a jeśli jest prawidłowa, wystawia token dostępu. |  |

Oto przykładowe zastosowanie dla `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Powyższy kod jest przeznaczony do wyjaśnienia tej części samouczka i nie powinna być używana w bezpiecznej lub aplikacje w środowisku produkcyjnym. Nie sprawdza poświadczenia właścicieli zasobu. Zakłada się, co poświadczenia są prawidłowe i tworzy nową tożsamość. Nową tożsamość będzie służyć do generowania tokenu dostępu i token odświeżania. Należy zastąpić kod swoim własnym kodem zarządzania bezpieczne konto.


### <a name="client-credentials-grant"></a>Udziel poświadczeń klienta

Odwołuje się do grupy roboczej IETF OAuth 2 [przyznania poświadczeń klienta](http://tools.ietf.org/html/rfc6749#section-4.4) sekcji teraz.

 [Przyznania poświadczeń klienta](http://tools.ietf.org/html/rfc6749#section-4.4) przepływ przedstawiono na rysunku 6 jest przepływ i mapowania, które OAuth interfejsu OWIN oprogramowanie pośredniczące jest zgodna.  

| Przepływ czynności z sekcji przyznania poświadczeń klienta | Pobieranie próbki wykonuje te kroki: |
| --- | --- |
|  |  |
| (A) klient jest uwierzytelniany w usłudze serwera autoryzacji i żądania tokenu dostępu z punktu końcowego tokena. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) serwera autoryzacji uwierzytelnia klientów, a jeśli jest prawidłowa, wystawia token dostępu. |  |

Oto przykładowe zastosowanie dla `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Powyższy kod jest przeznaczony do wyjaśnienia tej części samouczka i nie powinna być używana w bezpiecznej lub aplikacje w środowisku produkcyjnym. Należy zainstalować kod swoim własnym kodem zarządzania bezpiecznego klienta.


### <a name="refresh-token"></a>Token odświeżania

Odwołuje się do grupy roboczej IETF OAuth 2 [Token odświeżania](http://tools.ietf.org/html/rfc6749#section-1.5) sekcji teraz.

 [Token odświeżania](http://tools.ietf.org/html/rfc6749#section-1.5) przepływ pokazany na rysunku 2 jest przepływ i mapowania, które OAuth interfejsu OWIN oprogramowanie pośredniczące jest zgodna.  

| Przepływ czynności z sekcji przyznania poświadczeń klienta | Pobieranie próbki wykonuje te kroki: |
| --- | --- |
|  |  |
| G klient żąda nowy token dostępu przez uwierzytelnianie z serwerem autoryzacji i prezentować token odświeżania. Wymagania dotyczące uwierzytelniania klienta są oparte na typ klienta i zasad serwera autoryzacji. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| H serwera autoryzacji uwierzytelnia klientów i sprawdza poprawność tokenu odświeżania, a jeśli jest prawidłowa, generuje nowy token dostępu (i, opcjonalnie, nowy token odświeżania). |  |

Oto przykładowe zastosowanie dla `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Utwórz serwer zasobów, która jest chroniona przez Token dostępu

Utwórz pusty projekt sieci web app i zainstalować następujących pakietów do projektu:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Utwórz klasę uruchamiania i konfigurowania uwierzytelniania i interfejsu API sieci Web. Zobacz *AuthorizationServer\ResourceServer\Startup.cs* w pobieranie próbki.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Zobacz *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* w pobieranie próbki.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Zobacz *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* w pobieranie próbki.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`Metoda pozwala CORS dla wszystkich domen.
- `UseOAuthBearerAuthentication`Metoda umożliwia OAuth elementu nośnego tokenu pośredniczącego, które będą odbierać i sprawdzanie poprawności tokenu elementu nośnego z nagłówek autoryzacji w żądaniu.
- `Config.SuppressDefaultHostAuthenticaiton`Pomija domyślne hosta uwierzytelniony podmiot zabezpieczeń za pomocą aplikacji, w związku z tym wszystkie żądania będą anonimowy po tym wywołaniu.
- `HostAuthenticationFilter`Włącza uwierzytelnianie tylko dla określonego typu uwierzytelniania. W takim przypadku jest typ uwierzytelniania elementu nośnego.

W celu zaprezentowania uwierzytelniona tożsamość, utworzymy klasy ApiController do wyjściowego oświadczeń bieżącego użytkownika.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Jeśli serwera autoryzacji i serwer zasobów nie znajdują się na tym samym komputerze, oprogramowanie pośredniczące uwierzytelniania OAuth użyje klucze inną maszynę do szyfrowania i odszyfrowywania tokenu dostępu elementu nośnego. Aby współużytkować ten sam klucz prywatny między oba projekty, możemy dodać takie same `machinekey` ustawienie zarówno *web.config* plików.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Utwórz klientów uwierzytelniania OAuth 2.0

 Używamy [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) pakiet NuGet w celu uproszczenia kodu klienta.

### <a name="authorization-code-grant-client"></a>Klient Grant kod autoryzacji

 Ten klient jest aplikacji MVC. Wyzwoli on przepływu grant kodu autoryzacji do Uzyskaj token dostępu z wewnętrznej bazy danych. Posiada jednej strony, jak pokazano poniżej:

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Autoryzacji** przycisku spowoduje przekierowanie przeglądarki do serwera autoryzacji poinformować o tym właściciela zasobu, aby udzielić dostępu do tego klienta.
- **Odśwież** przycisk pobierze nowy token dostępu i token odświeżania przy użyciu bieżącego odświeżania tokenu.
- **Dostęp do chronionych zasobów API** przycisk wywoła serwer zasobów, aby uzyskać dane oświadczeń bieżącego użytkownika i wyświetlić je na stronie.

Oto przykładowy kod z `HomeController` klienta.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`wymaga protokołu SSL domyślnie. Ponieważ naszych pokaz jest przy użyciu protokołu HTTP, należy dodać następujące ustawienia w pliku konfiguracji:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Zabezpieczenia — nigdy nie wyłączenie protokołu SSL w aplikacji produkcyjnej. Poświadczenia logowania są teraz wysyłane w postaci zwykłego tekstu przez sieć. Powyższy kod jest tylko do lokalnej próbki debugowania i eksploracji.


### <a name="implicit-grant-client"></a>Niejawne Przyznaj klienta

Ten klient jest przy użyciu języka JavaScript do:

1. Otwórz nowe okno i Przekierowanie do punktu końcowego autoryzacji serwera autoryzacji.
2. Uzyskaj token dostępu z adresu URL fragmenty podczas przekierowania ponownie.

Na poniższej ilustracji przedstawiono ten proces:

![](owin-oauth-20-authorization-server/_static/image4.png)

Klient powinien mieć dwie strony: jeden dla strony głównej, a drugie dla wywołania zwrotnego. Oto przykładowe JavaScript znaleźć kodu w *Index.cshtml* pliku:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Oto kod w obsługi wywołania zwrotnego *SignIn.cshtml* pliku:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Najlepszym rozwiązaniem jest przeniesienie JavaScript do zewnętrznego pliku, a nie osadzaj ze znacznikami Razor. Aby zachować ten przykład prostego, zostały one połączone.


### <a name="resource-owner-password-credentials-grant-client"></a>Udziel klienta poświadczenia hasła właściciela zasobu

Firma Microsoft korzysta z aplikacji konsoli w celu demonstracją tego klienta. Oto kod:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Klient przyznania poświadczeń klienta

Podobnie jak Grant poświadczenia hasła właściciela zasobu, Oto kod aplikacji konsoli:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
