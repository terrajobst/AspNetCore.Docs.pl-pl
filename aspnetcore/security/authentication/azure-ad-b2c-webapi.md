---
title: Uwierzytelnianie w interfejsie web API za pomocą usługi Azure Active Directory B2C w programie ASP.NET Core
author: camsoper
description: Dowiedz się, jak skonfigurować uwierzytelnianie usługi Azure Active Directory B2C przy użyciu interfejsu API sieci Web programu ASP.NET Core. Przetestuj uwierzytelniony interfejs API sieci web za pomocą narzędzia Postman.
ms.author: casoper
ms.date: 09/21/2018
ms.custom: mvc, seodec18
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 00e7c9a438260ef8e12e567e521fc44b2ffa9cca
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264670"
---
# <a name="authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>Uwierzytelnianie w interfejsie web API za pomocą usługi Azure Active Directory B2C w programie ASP.NET Core

Przez [Soper kamery](https://twitter.com/camsoper)

[Usługa Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji internetowych i mobilnych. Usługa zapewnia uwierzytelnianie dla aplikacji hostowanych w chmurze i lokalnych. Typy uwierzytelniania obejmują indywidualnych kont, kont sieci społecznościowych i federacyjnych konta przedsiębiorstwa. Usługa Azure AD B2C zapewnia również usługi Multi-Factor authentication z minimalną konfiguracją.

Usługa Azure Active Directory (Azure AD) i Azure AD B2C są osobne oferty. Dzierżawy usługi Azure AD organizacja, podczas gdy dzierżawy usługi Azure AD B2C reprezentuje kolekcję tożsamości do użycia z aplikacjami danej firmy. Aby dowiedzieć się więcej, zobacz [usługi Azure AD B2C: Często zadawane pytania (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Ponieważ interfejsy API sieci web mają bez interfejsu użytkownika, są one nie można przekierować użytkownika do usługa bezpiecznych tokenów takich jak usługi Azure AD B2C. Zamiast tego interfejsu API są przekazywane tokenu elementu nośnego z aplikacji wywołującej, która już uwierzytelnił użytkownika za pomocą usługi Azure AD B2C. Interfejs API następnie weryfikuje token bez bezpośredniej interakcji użytkownika.

W tym samouczku pokazano, jak:

> [!div class="checklist"]
> * Tworzenie dzierżawy usługi Azure Active Directory B2C.
> * Rejestrowanie internetowego interfejsu API w usłudze Azure AD B2C.
> * Tworzenie interfejsu API sieci Web skonfigurowana do korzystania z dzierżawy usługi Azure AD B2C do uwierzytelniania przy użyciu programu Visual Studio.
> * Konfigurowanie zasad kontroli zachowania dzierżawy usługi Azure AD B2C.
> * Użyj narzędzia Postman w celu symulowania aplikację internetową, która wyświetla okno dialogowe logowania, pobiera token i używa go w celu wykonania żądania względem interfejsu API sieci web.

## <a name="prerequisites"></a>Wymagania wstępne

Wymagane jest spełnienie następujących w ramach tego przewodnika:

* [Subskrypcja Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Program Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (w każdej wersji)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Tworzenie dzierżawy usługi Azure Active Directory B2C

Tworzenie dzierżawy usługi Azure AD B2C [zgodnie z opisem w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-get-started). Po wyświetleniu monitu skojarzenie dzierżawcy z subskrypcją platformy Azure jest opcjonalny w tym samouczku.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Konfigurowanie zasad rejestracji lub logowania

W dokumentacji usługi Azure AD B2C, wykonaj kroki [Tworzenie zasad rejestracji lub logowania](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). Nazwa zasad **SiUpIn**.  Użyj przykładowych wartości podane w dokumentacji dotyczącej **dostawców tożsamości**, **atrybuty tworzenia konta**, i **oświadczeń aplikacji**. Za pomocą **Uruchom teraz** przycisk, aby przetestować zasady, zgodnie z opisem w dokumentacji jest opcjonalne.

## <a name="register-the-api-in-azure-ad-b2c"></a>Rejestrowanie interfejsu API w usłudze Azure AD B2C

W nowo utworzonej dzierżawy usługi Azure AD B2C należy zarejestrować przy użyciu interfejsu API [kroki opisane w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) w obszarze **zarejestrować internetowy interfejs API** sekcji.

Użyj następujących wartości:

| Ustawienie                       | Wartość               | Uwagi                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Nazwa**                      | *{Nazwa interfejsu API}*        | Wprowadź **nazwa** dla aplikacji, który opisuje aplikację dla użytkowników.                     |
| **Uwzględnij aplikację sieci web / interfejs API sieci web** | Tak                 |                                                                                        |
| **Zezwalaj na niejawny przepływ**       | Tak                 |                                                                                        |
| **Adres URL odpowiedzi**                 | `https://localhost` | Adresy URL odpowiedzi to punkty końcowe, w którym usługi Azure AD B2C zwraca wszelkie tokeny żądane przez aplikację. |
| **Identyfikator URI Identyfikatora aplikacji**                | *Interfejs API*               | Identyfikator URI nie musi prowadzić do adresu fizycznego. Tylko musi być unikatowa.     |
| **Dołącz klienta natywnego**     | Nie                  |                                                                                        |

Po zarejestrowaniu interfejsu API zostanie wyświetlona lista aplikacji i interfejsów API w dzierżawie. Wybierz interfejs API, który wcześniej został zarejestrowany. Wybierz **kopiowania** ikony po prawej stronie **identyfikator aplikacji** pola, aby skopiować go do Schowka. Wybierz **opublikowane zakresy** i zweryfikowanie domyślnego *user_impersonation* występuje zakresie.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Tworzenie aplikacji platformy ASP.NET Core w programie Visual Studio 2017

Szablon aplikacji sieci Web w usłudze Visual Studio można skonfigurować do korzystania z dzierżawy usługi Azure AD B2C do uwierzytelniania.

W programie Visual Studio:

1. Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. 
2. Wybierz **interfejsu API sieci Web** z listy szablonów.
3. Wybierz **Zmień uwierzytelnianie** przycisku.

    ![Zmień przycisk uwierzytelniania](./azure-ad-b2c-webapi/change-auth-button.png)

4. W **Zmień uwierzytelnianie** okno dialogowe, wybierz opcję **indywidualne konta użytkowników** > **Połącz z istniejącym magazynem użytkownika w chmurze**.

    ![Okno dialogowe uwierzytelniania zmiany](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Wypełnij formularz następującymi wartościami:

    | Ustawienie                       | Wartość                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nazwa domeny**               | *{Nazwa domeny dzierżawy usługi B2C}*                |
    | **Identyfikator aplikacji**            | *{Wklej identyfikator aplikacji ze Schowka}*       |
    | **Zasady tworzenia konta lub logowania** | B2C_1_SiUpIn                                          |

    Wybierz **OK** zamknąć **Zmień uwierzytelnianie** okna dialogowego. Wybierz **OK** umożliwiające utworzenie aplikacji sieci web.

Program Visual Studio tworzy interfejs API sieci web za pomocą kontrolera o nazwie *ValuesController.cs* zwracającego zakodowanych wartości dla żądania GET. Klasa zostanie nadany [atrybut Autoryzuj](xref:security/authorization/simple), więc wszystkie żądania wymagają uwierzytelniania.

## <a name="run-the-web-api"></a>Uruchom internetowy interfejs API

W programie Visual Studio, uruchamianie interfejsu API. Visual Studio otworzy w przeglądarce wskazywanej adresu URL katalogu głównego interfejsu API. Zanotuj adres URL w pasku adresu i pozostaw interfejsu API uruchomionego w tle.

> [!NOTE]
> Ponieważ nie istnieje żaden kontroler zdefiniowane dla adresu URL katalogu głównego, przeglądarka może być wyświetlany błąd 404 (nie można odnaleźć strony). Jest to oczekiwane zachowanie.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Pobierz token i testowanie interfejsu API przy użyciu narzędzia Postman

[Postman](https://getpostman.com/postman) jest narzędzie do testowania interfejsy API sieci web. W tym samouczku Postman symuluje aplikacji sieci web, który uzyskuje dostęp do internetowego interfejsu API w imieniu użytkownika.

### <a name="register-postman-as-a-web-app"></a>Zarejestruj Postman jako aplikacja sieci web

Ponieważ Postman symuluje aplikację sieci web, która uzyskuje dostęp do tokenów z dzierżawy usługi Azure AD B2C, musi być zarejestrowany w dzierżawie jako aplikacja sieci web. Rejestrowanie przy użyciu narzędzia Postman [kroki opisane w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) w obszarze **rejestrowanie aplikacji internetowej** sekcji. Zatrzyma **Tworzenie klucza tajnego klienta aplikacji sieci web** sekcji. Klucz tajny klienta nie jest wymagana na potrzeby tego samouczka. 

Użyj następujących wartości:

| Ustawienie                       | Wartość                            | Uwagi                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Nazwa**                      | Postman                          |                                 |
| **Uwzględnij aplikację sieci web / interfejs API sieci web** | Tak                              |                                 |
| **Zezwalaj na niejawny przepływ**       | Tak                              |                                 |
| **Adres URL odpowiedzi**                 | `https://getpostman.com/postman` |                                 |
| **Identyfikator URI Identyfikatora aplikacji**                | *{puste}*                  | Nie jest wymagane na potrzeby tego samouczka. |
| **Dołącz klienta natywnego**     | Nie                               |                                 |

Nowo rejestrowanej aplikacji internetowej wymaga zgody na dostęp do internetowego interfejsu API w imieniu użytkownika.  

1. Wybierz **Postman** na liście aplikacji, a następnie wybierz **dostęp do interfejsu API** z menu po lewej stronie.
1. Wybierz pozycję **+ Dodaj**.
1. W **wybierz interfejs API** listy rozwijanej wybierz nazwę interfejsu API sieci web.
1. W **wybierz zakresy** listy rozwijanej, upewnij się, wszystkie zakresy są zaznaczone.
1. Wybierz przycisk **OK**.

Identyfikator aplikacji w aplikacji Postman, należy pamiętać, ponieważ są wymagane do uzyskania tokenu elementu nośnego.

### <a name="create-a-postman-request"></a>Tworzenie żądania narzędzia Postman

Uruchom narzędzie Postman. Domyślnie wyświetla Postman **Utwórz nowy** okna dialogowego przy uruchamianiu. Jeśli nie jest wyświetlane okno dialogowe, wybierz opcję **+ nowy** przycisku w lewym górnym rogu.

Z **Utwórz nowy** okno dialogowe:

1. Wybierz **żądania**.

    ![Przycisk żądania](./azure-ad-b2c-webapi/postman-create-new.png)

2. Wprowadź *uzyskać wartości* w **nazwy żądania** pole.
3. Wybierz **+ Utwórz kolekcję** do utworzenia nowej kolekcji do przechowywania żądania. Nazwij kolekcję *samouczki platformy ASP.NET Core* , a następnie wybierz znacznik wyboru.

    ![Utwórz nową kolekcję](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Wybierz **zapisać samouczki platformy ASP.NET Core** przycisku.

### <a name="test-the-web-api-without-authentication"></a>Testowanie interfejsu API sieci web bez uwierzytelniania

Aby sprawdzić, czy internetowy interfejs API wymaga uwierzytelniania, należy najpierw zgłosić wniosek bez uwierzytelniania.

1. W **wprowadź adres URL żądania** wprowadź adres URL `ValuesController`. Adres URL jest taka sama jak wartość wyświetlana w przeglądarce, za pomocą **wartości/interfejsu api** dołączane. Na przykład `https://localhost:44375/api/values`.
2. Wybierz **wysyłania** przycisku.
3. Należy pamiętać, stan odpowiedzi jest *401 nieautoryzowane*.

    ![uzyskanie odpowiedzi 401 nieautoryzowane](./azure-ad-b2c-webapi/postman-401-status.png)

> [!IMPORTANT]
> Jeśli zostanie wyświetlony błąd "Nie można pobrać odpowiedzi", może być konieczne Wyłącz weryfikację certyfikatu SSL w [ustawienia Postman](https://learning.getpostman.com/docs/postman/launching_postman/settings).

### <a name="obtain-a-bearer-token"></a>Uzyskiwanie tokenu elementu nośnego

Aby wprowadzić uwierzytelnionego żądania interfejsu API sieci web, token elementu nośnego jest wymagana. Postman ułatwia logować się do dzierżawy usługi Azure AD B2C i uzyskania tokenu.

1. Na **autoryzacji** na karcie **typu** listy rozwijanej wybierz **OAuth 2.0**. W **danych autoryzacji, aby dodać** listy rozwijanej wybierz **nagłówkami żądań**. Wybierz **uzyskiwanie tokenu dostępu do nowych**.

    ![Karta autoryzacji przy użyciu ustawień](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Wykonaj **UZYSKAĆ nowy TOKEN dostępu** okno dialogowe w następujący sposób:

   |                Ustawienie                 |                                             Wartość                                             |                                                                                                                                    Uwagi                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      **Nazwa tokenu**       |                                          *{Nazwa tokenu}*                                       |                                                                                                                   Wprowadź nazwę opisową dla tokenu.                                                                                                                    |
   |      **Typ udzielania**       |                                           Niejawne                                            |                                                                                                                                                                                                                                                                              |
   |     **Adres URL wywołania zwrotnego**      |                                 `https://getpostman.com/postman`                              |                                                                                                                                                                                                                                                                              |
   |       **Adres URL uwierzytelniania**        | `https://login.microsoftonline.com/{tenant domain name}/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` |  Zastąp *{nazwa domeny dzierżawy}* z nazwą domeny dzierżawy. **WAŻNE**: Ten adres URL musi mieć taką samą nazwę domeny, jak co znajduje się w `AzureAdB2C.Instance` w sieci web, interfejsów API *appsettings.json* pliku. Patrz Uwaga&dagger;.                                                  |
   |       **Identyfikator klienta**       |                *{Wprowadź aplikację Postman **identyfikator aplikacji**}*                              |                                                                                                                                                                                                                                                                              |
   |         **Zakres**         |         `https://{tenant domain name}/{api}/user_impersonation openid offline_access`       | Zastąp *{nazwa domeny dzierżawy}* z nazwą domeny dzierżawy. Zastąp *{interfejsu api}* z identyfikator URI Identyfikatora aplikacji należy nadać interfejsu API sieci web podczas pierwszej rejestracji (w tym przypadku `api`). Wzorzec adresu URL: `https://{tenant}.onmicrosoft.com/{api-id-uri}/{scope name}`.         |
   |         **State**         |                                      *{puste}*                                          |                                                                                                                                                                                                                                                                              |
   | **Uwierzytelnianie klienta** |                                Wyślij poświadczeń klienta w treści                                |                                                                                                                                                                                                                                                                              |

    > [!NOTE]
    > &dagger; Okno dialogowe Ustawienia zasad w portalu Azure Active Directory B2C zawiera dwa możliwe adresy URL: W formacie `https://login.microsoftonline.com/`{nazwa domeny dzierżawy} / {dodatkowe informacje o ścieżce}, a druga w formacie `https://{tenant name}.b2clogin.com/`{nazwa domeny dzierżawy} / {dodatkowe informacje o ścieżce}. Ma ona **krytyczne** wykrytym przez domenę w w `AzureAdB2C.Instance` w sieci web, interfejsów API *appsettings.json* pliku użytemu w aplikacji sieci web *appsettings.json* pliku. Jest to tej samej domeny, które są używane dla pola Adres URL uwierzytelniania w narzędziu Postman. Należy pamiętać, że program Visual Studio używa nieco inny format adresu URL, niż jest wyświetlana w portalu. Tak długo, jak domeny są zgodne, adres URL działa.

3. Wybierz **żądania tokenu** przycisku.

4. Postman spowoduje otwarcie nowego okna dialogowego logowania dla dzierżawy usługi Azure AD B2C. Zaloguj się przy użyciu istniejącego konta, (Jeśli utworzono jedną testowania zasad) lub wybierz **Zarejestruj się teraz** do utworzenia nowego konta. **Nie pamiętasz hasła?** łącze służy do zresetować zapomniane hasło.

5. Po pomyślnym zalogowaniu okno zostanie zamknięte i **Zarządzanie TOKENÓW dostępu** zostanie wyświetlone okno dialogowe. Przewiń w dół do dołu i wybierz pozycję **użycia tokenu** przycisku.

    ![Gdzie można znaleźć przycisk "Użyj tokenu"](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Testowanie interfejsu API sieci web z uwierzytelnianiem

Wybierz **wysyłania** przycisk, aby ponownie wysłać żądanie. Tym razem stan odpowiedzi jest *200 OK* i ładunek JSON jest widoczny w odpowiedzi **treści** kartę.

![Stan ładunku i Powodzenie](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie dzierżawy usługi Azure Active Directory B2C.
> * Rejestrowanie internetowego interfejsu API w usłudze Azure AD B2C.
> * Tworzenie interfejsu API sieci Web skonfigurowana do korzystania z dzierżawy usługi Azure AD B2C do uwierzytelniania przy użyciu programu Visual Studio.
> * Konfigurowanie zasad kontroli zachowania dzierżawy usługi Azure AD B2C.
> * Użyj narzędzia Postman w celu symulowania aplikację internetową, która wyświetla okno dialogowe logowania, pobiera token i używa go w celu wykonania żądania względem interfejsu API sieci web.

Kontynuuj tworzenie interfejsu API, ucząc się:

* [Zabezpieczenia platformy ASP.NET Core z aplikacji internetowej przy użyciu usługi Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* [Wywoływanie interfejsu web API platformy .NET z aplikacji sieci web platformy .NET przy użyciu usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Dostosowywanie interfejsu użytkownika usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Skonfiguruj wymagania dotyczące złożoności hasła](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Włącz uwierzytelnianie wieloskładnikowe](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Konfigurowanie dostawców tożsamości dodatkowe, takie jak [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [w usłudze Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)i innym osobom.
* [Za pomocą interfejsu API programu Graph usługi Azure AD](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) do pobierania dodatkowych informacji dotyczących użytkowników, takich jak członkostwo w grupie z dzierżawy usługi Azure AD B2C.
