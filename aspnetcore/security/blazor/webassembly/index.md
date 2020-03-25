---
title: Bezpieczny ASP.NET Core Blazor webassembly
author: guardrex
description: Dowiedz się, jak zabezpieczyć Blazor aplikacje WebAssemlby jako aplikacje jednostronicowe (aplikacji jednostronicowych).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: 652d4c61110f786396d9d5af4f131b817c40e333
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219249"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a>Bezpieczny ASP.NET Core Blazor webassembly

Autor [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

aplikacje webassembly Blazor są zabezpieczone w taki sam sposób, jak aplikacje jednostronicowe (aplikacji jednostronicowych). Istnieje kilka podejścia do uwierzytelniania użytkowników w aplikacji jednostronicowych, ale najpopularniejsze i kompleksowe podejście polega na użyciu implementacji opartej na [protokole oAuth 2,0](https://oauth.net/), takim jak [Open ID Connect (OIDC)](https://openid.net/connect/).

## <a name="authentication-library"></a>Biblioteka uwierzytelniania

Blazor webassembly obsługuje uwierzytelnianie i Autoryzowanie aplikacji przy użyciu programu OIDC za pośrednictwem biblioteki `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. Biblioteka zawiera zestaw elementów pierwotnych do bezproblemowego uwierzytelniania w odniesieniu do ASP.NET Core. Biblioteka integruje tożsamość ASP.NET Core z obsługą autoryzacji interfejsu API wbudowaną na [serwerze tożsamości](https://identityserver.io/). Biblioteka może być uwierzytelniana względem dowolnego innego dostawcy tożsamości (IP), który obsługuje OIDC, które są nazywane dostawcami OpenID Connect (OP).

Obsługa uwierzytelniania w programie Blazor webassembly jest oparta na bibliotece *OIDC-Client. js* , która jest używana do obsługi szczegółowych informacji o podstawowym protokole uwierzytelniania.

Dostępne są inne opcje uwierzytelniania aplikacji jednostronicowych, takie jak używanie plików cookie SameSite. Jednak projekt inżynieryjny Blazor webassembly jest rozliczany przy użyciu protokołu oAuth i OIDC jako najlepszą opcję uwierzytelniania w aplikacjach Blazor webassembly. [Uwierzytelnianie oparte na tokenach](xref:security/anti-request-forgery#token-based-authentication) oparte na [tokenach sieci Web JSON (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) zostało wybrane w ramach [uwierzytelniania opartego na plikach cookie](xref:security/anti-request-forgery#cookie-based-authentication) dla powodów funkcjonalnych i zabezpieczeń:

* Użycie protokołu opartego na tokenach oferuje mniejszy obszar obszaru ataków, ponieważ tokeny nie są wysyłane we wszystkich żądaniach.
* Punkty końcowe serwera nie wymagają ochrony przed [fałszerstwem żądania między lokacjami (CSRF)](xref:security/anti-request-forgery) , ponieważ tokeny są wysyłane jawnie. Dzięki temu można hostować Blazor aplikacje webassembly w aplikacjach MVC lub Razor.
* Tokeny mają węższe uprawnienia niż pliki cookie. Na przykład tokeny nie mogą być używane do zarządzania kontem użytkownika ani zmieniać hasła użytkownika, chyba że te funkcje są jawnie zaimplementowane.
* Tokeny mają krótki okres istnienia, domyślnie jedną godzinę, która ogranicza okno ataku. Tokeny można również odwołać w dowolnym momencie.
* Samodzielna oferta JWTs gwarantuje klientowi i serwerowi proces uwierzytelniania. Na przykład klient ma środki do wykrycia i sprawdzenia, czy otrzymane tokeny są wiarygodne i są emitowane w ramach danego procesu uwierzytelniania. Jeśli osoba trzecia podejmie próbę przełączenia tokenu w trakcie procesu uwierzytelniania, klient może wykryć przełączony token i uniknąć korzystania z niego.
* Tokeny z uwierzytelnianiem oAuth i OIDC nie polegają na tym, że agent użytkownika działa prawidłowo, aby upewnić się, że aplikacja jest bezpieczna.
* Protokoły oparte na tokenach, takie jak oAuth i OIDC, umożliwiają uwierzytelnianie i Autoryzowanie aplikacji hostowanych i autonomicznych z tym samym zestawem charakterystyk zabezpieczeń.

## <a name="authentication-process-with-oidc"></a>Proces uwierzytelniania za pomocą OIDC

Biblioteka `Microsoft.AspNetCore.Components.WebAssembly.Authentication` oferuje kilka elementów podstawowych, które implementują uwierzytelnianie i autoryzację przy użyciu OIDC. Uwierzytelnianie odbywa się w następujący sposób:

* Gdy użytkownik anonimowy wybierze przycisk Zaloguj lub zażąda strony z zastosowanym atrybutem [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) , użytkownik zostanie przekierowany do strony logowania (`/authentication/login`) aplikacji.
* Na stronie Logowanie Biblioteka uwierzytelniania przygotowuje przekierowanie do punktu końcowego autoryzacji. Punkt końcowy autoryzacji jest poza aplikacją Blazor webassembly i może być hostowany w osobnym źródle. Punkt końcowy jest odpowiedzialny za określenie, czy użytkownik jest uwierzytelniany i w celu wystawiania co najmniej jednego tokenu w odpowiedzi. Biblioteka uwierzytelniania umożliwia wywołanie zwrotne logowania w celu uzyskania odpowiedzi uwierzytelniania.
  * Jeśli użytkownik nie jest uwierzytelniony, użytkownik zostanie przekierowany do podstawowego systemu uwierzytelniania, który jest zwykle ASP.NET Core tożsamości.
  * Jeśli użytkownik został już uwierzytelniony, punkt końcowy autoryzacji generuje odpowiednie tokeny i przekierowuje przeglądarkę z powrotem do punktu końcowego wywołania zwrotnego logowania (`/authentication/login-callback`).
* Gdy aplikacja webassembly Blazor ładuje punkt końcowy wywołania zwrotnego logowania (`/authentication/login-callback`), odpowiedź uwierzytelniania zostanie przetworzona.
  * Jeśli proces uwierzytelniania zakończy się pomyślnie, użytkownik zostanie uwierzytelniony i opcjonalnie zostanie wysłany z powrotem do oryginalnego chronionego adresu URL, którego zażądał użytkownik.
  * Jeśli z jakiegoś powodu proces uwierzytelniania zakończy się niepowodzeniem, użytkownik zostanie wysłany na stronę niepowodzenie logowania (`/authentication/login-failed`) i zostanie wyświetlony komunikat o błędzie.
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a>Opcje aplikacji hostowanych i dostawców logowania innych firm

Podczas uwierzytelniania i autoryzowania hostowanej Blazor aplikacji sieci webassembly przy użyciu dostawcy innej firmy dostępnych jest kilka opcji uwierzytelniania użytkownika. Wybór jednego z nich zależy od danego scenariusza.

Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/additional-claims>.

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a>Uwierzytelnianie użytkowników tylko w celu wywołania chronionych interfejsów API innych firm

Uwierzytelnij użytkownika za pomocą przepływu oAuth po stronie klienta dla dostawcy interfejsu API innej firmy:

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 W tym scenariuszu:

* Serwer hostujący aplikację nie odgrywa roli.
* Nie można chronić interfejsów API na serwerze.
* Aplikacja może wywoływać tylko chronione interfejsy API innych firm.

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a>Uwierzytelnianie użytkowników za pomocą dostawcy innych firm i wywoływanie chronionych interfejsów API na serwerze hosta i stronie trzeciej

Skonfiguruj tożsamość przy użyciu innego dostawcy logowania. Uzyskaj tokeny wymagane przez dostęp do interfejsu API innych firm i Zapisz je.

Gdy użytkownik loguje się, tożsamość zbiera tokeny dostępu i odświeżania w ramach procesu uwierzytelniania. W tym momencie istnieje kilka podejścia dostępnych do wykonywania wywołań interfejsu API do interfejsów API innych firm.

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a>Korzystanie z tokenu dostępu do serwera w celu pobrania tokenu dostępu innej firmy

Użyj tokenu dostępu wygenerowanego na serwerze, aby pobrać token dostępu innej firmy z punktu końcowego interfejsu API serwera. Z tego miejsca Użyj tokenu dostępu innej firmy do wywołania zasobów interfejsu API innych firm bezpośrednio z tożsamości na kliencie.

Nie zalecamy tego podejścia. Takie podejście wymaga traktowania tokenu dostępu innej firmy, tak jakby został wygenerowany dla klienta publicznego. W przypadku postanowień uwierzytelniania oAuth publiczna aplikacja nie ma tajnego klienta, ponieważ nie może być zaufana do bezpiecznego przechowywania wpisów tajnych, a token dostępu jest generowany dla klienta poufnego. Klient poufny jest klientem, który ma klucz tajny klienta i ma możliwość bezpiecznego przechowywania wpisów tajnych.

* Token dostępu innej firmy może mieć przyznane dodatkowe zakresy do wykonywania poufnych operacji na podstawie faktu, że firma zewnętrzna emituje token dla bardziej zaufanego klienta.
* Podobnie tokeny odświeżania nie powinny być wystawiane dla klienta, który nie jest zaufany. w takim przypadku klient nie będzie miał nieograniczonego dostępu, chyba że zostaną zastosowane inne ograniczenia.

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a>Wykonywanie wywołań interfejsu API z klienta do interfejsu API serwera w celu wywołania interfejsów API innych firm

Wykonaj wywołanie interfejsu API z klienta do interfejsu API serwera. Na serwerze programu Pobierz token dostępu dla zasobu interfejsu API innej firmy i wystaw, w jaki sposób jest wymagane.

Chociaż to podejście wymaga dodatkowego skoku sieci przez serwer w celu wywołania interfejsu API innej firmy, ostatecznie powoduje bezpieczniejsze środowisko:

* Serwer może przechowywać tokeny odświeżania i upewnić się, że aplikacja nie utraci dostępu do zasobów innych firm.
* Aplikacja nie może wyciekować tokenów dostępu z serwera, który może zawierać bardziej poufne uprawnienia.
