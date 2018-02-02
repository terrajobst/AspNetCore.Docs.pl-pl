---
title: "Uwierzytelnianie w chmurze w składniku web API z usługi Azure Active Directory B2C"
author: camsoper
description: "Wykryj sposobu konfigurowania uwierzytelniania usługi Azure Active Directory B2C za pomocą interfejsu API platformy ASP.NET Core sieci Web. Przetestuj uwierzytelnionego składnika web API z Postman."
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: a63bfc26bb6b0f5ea1c64641d6f57a3555d7f401
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c"></a>Uwierzytelnianie w chmurze w składniku web API z usługi Azure Active Directory B2C

Przez [Soper kamery](https://twitter.com/camsoper)

[Usługa Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) jest rozwiązaniem do zarządzania tożsamościami chmury dla aplikacji sieci web i aplikacji mobilnych. Usługa zapewnia uwierzytelnianie dla aplikacji hostowanych w chmurze i lokalnie. Typy uwierzytelniania obejmują indywidualnych kont, kont sieci społecznościowych i federacyjnych konta przedsiębiorstwa. Ponadto usługi Azure AD B2C zapewniają uwierzytelnianie wieloskładnikowe z minimalną konfiguracją.

> [!TIP]
> Azure Active Directory (Azure AD) usługi Azure AD B2C są oferty oddzielny produkt. Dzierżawa usługi Azure AD reprezentuje organizacji, podczas gdy dzierżawy usługi Azure AD B2C reprezentuje kolekcję tożsamości do użycia z aplikacjami danej firmy. Aby dowiedzieć się więcej, zobacz [usługi Azure AD B2C: często zadawane pytania (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Ponieważ interfejsów API sieci web mają bez interfejsu użytkownika, są one nie można przekierować użytkownika do bezpiecznego tokenu usług, takich jak usługi Azure AD B2C. Zamiast tego interfejsu API jest przekazywany tokenu elementu nośnego z wywołania aplikacji, który został już uwierzytelniony użytkownik z usługi Azure AD B2C. Interfejs API następnie weryfikuje token bez bezpośredniej interakcji użytkownika.

W tym samouczku przedstawiono sposób:

> [!div class="checklist"]
> * Tworzenie dzierżawy usługi Azure Active Directory B2C.
> * Zarejestruj interfejsu API sieci Web w usługi Azure AD B2C.
> * Tworzenie interfejsu API sieci Web skonfigurowana do używania dzierżawy usługi Azure AD B2C do uwierzytelniania za pomocą programu Visual Studio.
> * Konfigurowanie zasad kontroli zachowania dzierżawy usługi Azure AD B2C.
> * Użyj Postman, aby symulować aplikacji sieci web, który przedstawia okno dialogowe logowania pobiera token i używa go do zgłoszenia żądania względem interfejsu API sieci web.

## <a name="prerequisites"></a>Wymagania wstępne

Poniżej przedstawiono wymagania dla tego przewodnika:

* [Subskrypcja Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (dowolna wersja)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Tworzenie dzierżawy usługi Azure Active Directory B2C

Tworzenie dzierżawy usługi Azure AD B2C [zgodnie z opisem w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-get-started). Po wyświetleniu monitu skojarzenie dzierżawcy z subskrypcją platformy Azure jest opcjonalne dla tego samouczka.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Konfigurowanie zasad rejestracji i logowania

Wykonaj kroki w dokumentacji usługi Azure AD B2C [utworzyć zasadę tworzenia konta lub logowanie](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). Nazwa zasady **SiUpIn**.  Użyj przykładowe wartości podane w dokumentacji dotyczącej **dostawców tożsamości**, **atrybuty rejestracji**, i **oświadczenia aplikacji**. Przy użyciu **Uruchom teraz** przycisk, aby przetestować zasady, zgodnie z opisem w dokumentacji są opcjonalne.

## <a name="register-the-api-in-azure-ad-b2c"></a>Zarejestruj interfejsu API w Azure AD B2C

W nowo utworzone dzierżawy usługi Azure AD B2C, zarejestrować za pomocą interfejsu API [kroki opisane w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) w obszarze **zarejestrować składnika web API** sekcji.

Użyj następujących wartości:

| Ustawienie                       | Wartość               | Uwagi                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Nazwa**                      | *&lt;Nazwa interfejsu API&gt;*  | Wprowadź **nazwa** aplikacji, który opisuje aplikacji dla konsumentów.                     |
| **Uwzględnij aplikację sieci web / interfejs API sieci web** | Tak                 |                                                                                        |
| **Zezwalaj na niejawnego przepływu**       | Tak                 |                                                                                        |
| **Adres URL odpowiedzi**                 | `https://localhost` | Adresy URL odpowiedzi są punkty końcowe, w którym usługi Azure AD B2C zwraca wszystkie tokeny żądań aplikacji. |
| **Identyfikator URI aplikacji**                | *Interfejs API*               | Identyfikator URI nie musi rozpoznać adres fizyczny. Tylko musi być unikatowy.     |
| **Zawierają natywnego klienta**     | Nie                  |                                                                                        |

Po zarejestrowaniu interfejsu API jest wyświetlana lista aplikacji i interfejsów API w dzierżawie. Wybierz interfejs API, który właśnie został zarejestrowany. Wybierz **kopiowania** ikonę z prawej strony **identyfikator aplikacji** pola, aby skopiować go do Schowka. Wybierz **opublikowane zakresy** i zweryfikowanie domyślnego *user_impersonation* zakres jest obecny.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Tworzenie aplikacji platformy ASP.NET Core w Visual Studio 2017 r.

Aby korzystać z dzierżawy usługi Azure AD B2C do uwierzytelniania można skonfigurować szablonu aplikacji sieci Web w usłudze Visual Studio.

W programie Visual Studio:

1. Utwórz nową aplikację sieci Web platformy ASP.NET Core. 
2. Wybierz **interfejsu API sieci Web** z listy szablonów.
3. Wybierz **Zmień uwierzytelnianie** przycisku.
    
    ![Zmień przycisk uwierzytelniania](./azure-ad-b2c-webapi/change-auth-button.png)

4. W **Zmień uwierzytelnianie** okno dialogowe, wybierz opcję **indywidualnych kont użytkowników**, a następnie wybierz **Połącz z istniejącym magazynem użytkownika w chmurze** na liście rozwijanej. 
    
    ![Dialog uwierzytelniania zmiany](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Wypełnienie formularza z następujących wartości:
    
    | Ustawienie                       | Wartość                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nazwa domeny**               | *&lt;nazwę domeny dzierżawy usługi B2C&gt;*          |
    | **Identyfikator aplikacji**            | *&lt;Wklej identyfikator aplikacji ze Schowka&gt;* |
    | **Zasady rejestracji i logowania** | `B2C_1_SiUpIn`                                        |
    
    Wybierz **OK** zamknąć **Zmień uwierzytelnianie** okna dialogowego. Wybierz **OK** do utworzenia aplikacji sieci web.

Program Visual Studio tworzy interfejs API sieci web za pomocą kontrolera o nazwie *ValuesController.cs* zwracającą zakodowanych wartości dla żądania GET. Klasa zostanie nadany [atrybutu autoryzacji](xref:security/authorization/simple), więc wszystkie żądania wymagają uwierzytelniania.

## <a name="run-the-web-api"></a>Uruchamianie interfejsu API sieci web

W programie Visual Studio uruchomić interfejsu API. Visual Studio spowoduje uruchomienie przeglądarki odnosi się do adresu URL katalogu głównego w interfejsie API. Zanotuj adres URL na pasku adresu i pozostaw API uruchomione w tle.

> [!NOTE]
> Ponieważ nie istnieje żaden kontroler zdefiniowane dla adresu URL katalogu głównego, w przeglądarce pojawi się błąd 404 (nie można odnaleźć strony). Jest to oczekiwane zachowanie.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Użyj Postman, aby uzyskać token i testowania interfejsu API

[Postman](https://getpostman.com/postman) jest narzędziem do testowania interfejsów API sieci web. W tym samouczku Postman symuluje aplikacji sieci web, który uzyskuje dostęp do interfejsu API sieci web w imieniu użytkownika.

### <a name="register-postman-as-a-web-app"></a>Zarejestruj Postman jako aplikacji sieci web

Ponieważ Postman symuluje aplikacji sieci web, które mogą uzyskać tokeny od dzierżawcy usługi Azure AD B2C, musi być zarejestrowana w dzierżawie jako aplikacji sieci web. Zarejestruj Postman przy użyciu [kroki opisane w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) w obszarze **zarejestrować aplikacji sieci web** sekcji. Zatrzymaj przy **Utwórz klucz tajny klienta aplikacji sieci web** sekcji. Klucz tajny klienta nie jest wymagane dla tego samouczka. 

Użyj następujących wartości:

| Ustawienie                       | Wartość                            | Uwagi                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Nazwa**                      | Postman                          |                                 |
| **Uwzględnij aplikację sieci web / interfejs API sieci web** | Tak                              |                                 |
| **Zezwalaj na niejawnego przepływu**       | Tak                              |                                 |
| **Adres URL odpowiedzi**                 | `https://getpostman.com/postman` |                                 |
| **Identyfikator URI aplikacji**                | *&lt;Pozostaw puste&gt;*            | Nie są wymagane dla tego samouczka. |
| **Zawierają natywnego klienta**     | Nie                               |                                 |

Aplikacja sieci web nowo zarejestrowanych wymaga zgody na dostęp do interfejsu API sieci web w imieniu użytkownika.  

1. Wybierz **Postman** na liście aplikacji, a następnie wybierz **dostępu do interfejsu API** z menu po lewej stronie.
2. Wybierz **+ Dodaj**.
3. W **wybierz interfejsu API** listy rozwijanej, wybierz nazwę interfejsu API sieci web.
4. W **wybierz zakresy** listy rozwijanej, upewnij się, wszystkie zakresy są wybrane.
5. Wybierz **Ok**.

Należy zwrócić uwagę aplikacji Postman identyfikator aplikacji, ponieważ są wymagane do uzyskania tokenu elementu nośnego.

### <a name="create-a-postman-request"></a>Utwórz żądanie Postman

Uruchom Postman. Domyślnie są wyświetlane Postman **Utwórz nowy** okna dialogowego przy uruchamianiu. Jeśli nie jest wyświetlane okno dialogowe, wybierz **+ nowy** przycisk w lewym górnym rogu.

Z **Utwórz nowy** okna dialogowego:

1. Wybierz **żądania**.
    
    ![Przycisk żądania](./azure-ad-b2c-webapi/postman-create-new.png)

2. Wprowadź *uzyskać wartości* w **Nazwa żądania** pole.
3. Wybierz **+ Utwórz kolekcję** Aby utworzyć nową kolekcję do przechowywania żądania. Nazwa kolekcji *samouczki platformy ASP.NET Core* , a następnie wybierz znacznik wyboru.
    
    ![Tworzenie nowej kolekcji](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Wybierz **zapisać samouczki platformy ASP.NET Core** przycisku.

### <a name="test-the-web-api-withoutauthentication"></a>Testowanie withoutauthentication interfejsu API sieci web

Aby sprawdzić, czy interfejs API sieci web wymaga uwierzytelniania, najpierw złożyć wniosek bez uwierzytelniania.

1. W **wprowadź adres URL żądania** wprowadź adres URL `ValuesController`. Adres URL jest taka sama jak wartość wyświetlana w przeglądarce z **interfejsu api/wartości** dołączane. Przykładem może być `https://localhost:44375/api/values`.
2. Wybierz **wysyłania** przycisku.
3. Stan odpowiedzi jest *401 nieautoryzowane*.

    ![odpowiedzi 401 nieautoryzowane](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>Uzyskania tokenu elementu nośnego

Aby wykonać uwierzytelnione żądania interfejsu API sieci web, wymagany jest token elementu nośnego. Postman ułatwia Zaloguj się do dzierżawy usługi Azure AD B2C i uzyskania tokenu.

1. Na **autoryzacji** karcie **typu** listy rozwijanej wybierz **OAuth 2.0**. W **dodać dane autoryzacji** listy rozwijanej wybierz **nagłówkami żądań**. Wybierz **Uzyskaj Token dostępu nowe**.
    
    ![Karta autoryzacji z ustawieniami](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Zakończenie **UZYSKAĆ nowy TOKEN dostępu** okno dialogowe w następujący sposób:
    
    | Ustawienie                   | Wartość                                                                                         | Uwagi                                                                                      |
    |---------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
    | **Nazwa tokenu**            | *&lt;Nazwa tokenu&gt;*                                                                          | Wprowadź nazwę opisową dla tokenu.                                                    |
    | **Typ przydziału**            | Niejawne                                                                                      |                                                                                            |
    | **Adres URL wywołania zwrotnego**          | `https://getpostman.com/postman`                                                              |                                                                                            |
    | **Adres URL uwierzytelniania**              | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` | Zastąp  *&lt;nazwę domeny dzierżawy&gt;*  z nazwą domeny dzierżawcy bez nawiasy. |
    | **Identyfikator klienta**             | *&lt;Wprowadź aplikacji Postman <b>identyfikator aplikacji</b>&gt;*                                       |                                                                                            |
    | **Klucz tajny klienta**         | *&lt;Pozostaw puste&gt;*                                                                         |                                                                                            |
    | **Zakres**                 | `https://<tenant domain name>/api/user_impersonation openid offline_access`                   | Zastąp  *&lt;nazwę domeny dzierżawy&gt;*  z nazwą domeny dzierżawcy bez nawiasy. |
    | **Uwierzytelnianie klienta** | Wyślij poświadczeń klienta w treści                                                               |                                                                                            |
    
3. Wybierz **żądania tokenu** przycisku.

4. Postman otwiera nowe okno zawierające znak dzierżawy usługi Azure AD B2C w oknie dialogowym. Zaloguj się przy użyciu istniejącego konta (Jeśli utworzono jedną testowania zasad) lub wybierz **Zamów teraz** Aby utworzyć nowe konto. **Nie pamiętasz hasła?** łącze służy do zresetować zapomniane hasło.

5. Po pomyślnym zalogowaniu okno zostanie zamknięte i **Zarządzanie TOKENÓW dostępu** zostanie wyświetlone okno dialogowe. Przewiń w dół do dolnej i wybierz **Użyj tokenu** przycisku.
    
    ![Gdzie można znaleźć przycisk "Użyj tokenu"](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Test sieci web interfejsu API za pomocą uwierzytelniania

Wybierz **wysyłania** przycisk, aby ponownie wysłać żądanie. Teraz, stan odpowiedzi jest *200 OK* i ładunek JSON jest widoczny w odpowiedzi **treści** kartę.
    
![Ładunek i Powodzenie stanu](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób:

> [!div class="checklist"]
> * Tworzenie dzierżawy usługi Azure Active Directory B2C.
> * Zarejestruj interfejsu API sieci Web w usługi Azure AD B2C.
> * Tworzenie interfejsu API sieci Web skonfigurowana do używania dzierżawy usługi Azure AD B2C do uwierzytelniania za pomocą programu Visual Studio.
> * Konfigurowanie zasad kontroli zachowania dzierżawy usługi Azure AD B2C.
> * Użyj Postman, aby symulować aplikacji sieci web, który przedstawia okno dialogowe logowania pobiera token i używa go do zgłoszenia żądania względem interfejsu API sieci web.

Kontynuuj tworzenie interfejsu API przez uczenia:

* [Zabezpieczenia aplikacji sieci web przy użyciu usługi Azure AD B2C platformy ASP.NET Core](xref:security/authentication/azure-ad-b2c).
* [Wywołanie interfejsu API sieci web .NET z aplikacji sieci web .NET przy użyciu usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Dostosowywanie interfejsu użytkownika usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Skonfiguruj wymagania dotyczące złożoności hasła](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Włączanie uwierzytelniania wieloskładnikowego](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Skonfiguruj dostawców tożsamości dodatkowe, takie jak [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [w usłudze Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)i inne.
* [Za pomocą interfejsu API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) można pobrać dodatkowe informacje dotyczące użytkownika, takich jak członkostwo w grupie z dzierżawy usługi Azure AD B2C.