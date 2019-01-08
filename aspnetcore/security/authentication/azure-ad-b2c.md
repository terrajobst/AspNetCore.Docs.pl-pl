---
title: Uwierzytelnianie w chmurze za pomocą usługi Azure Active Directory B2C w programie ASP.NET Core
author: camsoper
description: Dowiedz się, jak skonfigurować uwierzytelnianie usługi Azure Active Directory B2C przy użyciu platformy ASP.NET Core.
ms.date: 01/25/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 2c544475ccd3eb76f2737fec1cf269ac86add372
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098990"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Uwierzytelnianie w chmurze za pomocą usługi Azure Active Directory B2C w programie ASP.NET Core

Przez [Soper kamery](https://twitter.com/camsoper)

[Usługa Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji internetowych i mobilnych. Usługa zapewnia uwierzytelnianie dla aplikacji hostowanych w chmurze i lokalnych. Typy uwierzytelniania obejmują indywidualnych kont, kont sieci społecznościowych i federacyjnych konta przedsiębiorstwa. Ponadto usługa Azure AD B2C zapewniają uwierzytelnianie wieloskładnikowe z minimalną konfiguracją.

> [!TIP]
> Usługa Azure Active Directory (Azure AD) i Azure AD B2C są osobne oferty. Dzierżawy usługi Azure AD organizacja, podczas gdy dzierżawy usługi Azure AD B2C reprezentuje kolekcję tożsamości do użycia z aplikacjami danej firmy. Aby dowiedzieć się więcej, zobacz [usługi Azure AD B2C: Często zadawane pytania (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

W tym samouczku pokazano, jak:

> [!div class="checklist"]
> * Tworzenie dzierżawy usługi Azure Active Directory B2C
> * Rejestrowanie aplikacji w usłudze Azure AD B2C
> * Tworzenie aplikacji internetowej platformy ASP.NET Core skonfigurowane do korzystania z dzierżawy usługi Azure AD B2C do uwierzytelniania przy użyciu programu Visual Studio
> * Konfigurowanie zasad kontroli zachowania dzierżawy usługi Azure AD B2C

## <a name="prerequisites"></a>Wymagania wstępne

Wymagane jest spełnienie następujących w ramach tego przewodnika:

* [Subskrypcja Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Program Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (w każdej wersji)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Tworzenie dzierżawy usługi Azure Active Directory B2C

Tworzenie dzierżawy usługi Azure Active Directory B2C [zgodnie z opisem w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-get-started). Po wyświetleniu monitu skojarzenie dzierżawcy z subskrypcją platformy Azure jest opcjonalny w tym samouczku.

## <a name="register-the-app-in-azure-ad-b2c"></a>Rejestrowanie aplikacji w usłudze Azure AD B2C

W nowo utworzonej dzierżawy usługi Azure AD B2C, rejestrowania aplikacji przy użyciu [kroki opisane w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) w obszarze **rejestrowanie aplikacji internetowej** sekcji. Zatrzyma **Tworzenie klucza tajnego klienta aplikacji sieci web** sekcji. Klucz tajny klienta nie jest wymagana na potrzeby tego samouczka. 

Użyj następujących wartości:

| Ustawienie                       | Wartość                     | Uwagi                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                      | *&lt;Nazwa aplikacji&gt;*        | Wprowadź **nazwa** dla aplikacji, który opisuje aplikację dla użytkowników.                                                                                                                                 |
| **Uwzględnij aplikację sieci web / interfejs API sieci web** | Tak                       |                                                                                                                                                                                                    |
| **Zezwalaj na niejawny przepływ**       | Tak                       |                                                                                                                                                                                                    |
| **Adres URL odpowiedzi**                 | `https://localhost:44300/signin-oidc` | Adresy URL odpowiedzi to punkty końcowe, w którym usługi Azure AD B2C zwraca wszelkie tokeny żądane przez aplikację. Program Visual Studio udostępnia adres URL odpowiedzi do użycia. Teraz wprowadź `https://localhost:44300/signin-oidc` na wypełnienie formularza. |
| **Identyfikator URI Identyfikatora aplikacji**                | Pozostaw to pole puste               | Nie jest wymagane na potrzeby tego samouczka.                                                                                                                                                                    |
| **Dołącz klienta natywnego**     | Nie                        |                                                                                                                                                                                                    |

> [!WARNING]
> Konfigurowanie adresu URL odpowiedzi inne niż localhost, należy pamiętać o [ograniczenia dotyczące dozwolonych na liście adres URL odpowiedzi](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Po zarejestrowaniu aplikacji zostanie wyświetlona lista aplikacji w dzierżawie. Wybierz aplikację, który właśnie został zarejestrowany. Wybierz **kopiowania** ikony po prawej stronie **identyfikator aplikacji** pola, aby skopiować go do Schowka.

Nie rób więcej w tym momencie można skonfigurować w dzierżawie usługi Azure AD B2C, ale pozostaw otwarte okno przeglądarki. Po utworzeniu aplikacji ASP.NET Core jest większej liczby czynności konfiguracyjnych.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Tworzenie aplikacji platformy ASP.NET Core w programie Visual Studio 2017

Szablon aplikacji sieci Web w usłudze Visual Studio można skonfigurować do korzystania z dzierżawy usługi Azure AD B2C do uwierzytelniania.

W programie Visual Studio:

1. Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. 
2. Wybierz **aplikacji sieci Web** z listy szablonów.
3. Wybierz **Zmień uwierzytelnianie** przycisku.
    
    ![Zmień przycisk uwierzytelniania](./azure-ad-b2c/_static/changeauth.png)

4. W **Zmień uwierzytelnianie** okno dialogowe, wybierz opcję **indywidualne konta użytkowników**, a następnie wybierz pozycję **Połącz z istniejącym magazynem użytkownika w chmurze** na liście rozwijanej. 
    
    ![Okno dialogowe uwierzytelniania zmiany](./azure-ad-b2c/_static/changeauthdialog.png)

5. Wypełnij formularz następującymi wartościami:
    
    | Ustawienie                       | Wartość                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nazwa domeny**               | *&lt;Nazwa domeny dzierżawy usługi B2C&gt;*          |
    | **Identyfikator aplikacji**            | *&lt;Wklej identyfikator aplikacji ze Schowka&gt;* |
    | **Ścieżka wywołania zwrotnego**             | *&lt;Użyj wartości domyślnej&gt;*                       |
    | **Zasady tworzenia konta lub logowania** | `B2C_1_SiUpIn`                                        |
    | **Zasady resetowania haseł**     | `B2C_1_SSPR`                                          |
    | **Edytuj profil zasady**       | *&lt;Pozostaw to pole puste&gt;*                                 |
    
    Wybierz **kopiowania** łącze obok **identyfikator URI odpowiedzi** można skopiować identyfikator URI odpowiedzi do Schowka. Wybierz **OK** zamknąć **Zmień uwierzytelnianie** okna dialogowego. Wybierz **OK** umożliwiające utworzenie aplikacji sieci web.

## <a name="finish-the-b2c-app-registration"></a>Zakończ rejestrację aplikacji B2C

Wróć do okna przeglądarki, za pomocą właściwości aplikacji B2C wciąż otwarty. Zmień tymczasową **adres URL odpowiedzi** określony skopiowany wcześniej do wartości z programu Visual Studio. Wybierz **Zapisz** w górnej części okna.

> [!TIP]
> Jeśli jeszcze nie został skopiowany adres URL odpowiedzi, we właściwościach projektu sieci web za pomocą adresu HTTPS na karcie debugowania i Dołącz **CallbackPath** wartość z *appsettings.json*.

## <a name="configure-policies"></a>Konfigurowanie zasad

W dokumentacji usługi Azure AD B2C, wykonaj kroki [Tworzenie zasad rejestracji lub logowania](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), a następnie [Tworzenie zasad resetowania haseł](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Użyj przykładowych wartości podane w dokumentacji dotyczącej **dostawców tożsamości**, **atrybuty tworzenia konta**, i **oświadczeń aplikacji**. Za pomocą **Uruchom teraz** przycisk, aby przetestować zasady, zgodnie z opisem w dokumentacji jest opcjonalne.

> [!WARNING]
> Upewnij się, nazwy zasad dokładnie zgodnie z opisem zamieszczonym w dokumentacji, jak te zasady zostały użyte w **Zmień uwierzytelnianie** okna dialogowego w programie Visual Studio. Nazwy zasad można sprawdzić w *appsettings.json*.

## <a name="run-the-app"></a>Uruchamianie aplikacji

W programie Visual Studio, naciśnij klawisz **F5** Aby skompilować i uruchomić aplikację. Po uruchomieniu aplikacji sieci web wybierz **Akceptuj** aby zaakceptować użycie plików cookie (Jeśli pojawi się monit), a następnie wybierz pozycję **Zaloguj**.

![Zaloguj się do aplikacji](./azure-ad-b2c/_static/signin.png)

Przekierowuje przeglądarki do dzierżawy usługi Azure AD B2C. Zaloguj się przy użyciu istniejącego konta, (Jeśli utworzono jedną testowania zasad) lub wybierz **Zarejestruj się teraz** do utworzenia nowego konta. **Nie pamiętasz hasła?** łącze służy do zresetować zapomniane hasło.

![Logowanie w usłudze Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

Po pomyślnym zalogowaniu się przeglądarka przekierowuje do aplikacji sieci web.

![Powodzenie](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie dzierżawy usługi Azure Active Directory B2C
> * Rejestrowanie aplikacji w usłudze Azure AD B2C
> * Tworzenie aplikacji internetowej ASP.NET Core skonfigurowane do korzystania z dzierżawy usługi Azure AD B2C do uwierzytelniania przy użyciu programu Visual Studio
> * Konfigurowanie zasad kontroli zachowania dzierżawy usługi Azure AD B2C

Teraz, gdy aplikacja platformy ASP.NET Core jest skonfigurowany do używania usługi Azure AD B2C do uwierzytelniania, [atrybut Autoryzuj](xref:security/authorization/simple) może służyć do zabezpieczenia aplikacji. Kontynuuj, opracowywanie aplikacji, ucząc się:

* [Dostosowywanie interfejsu użytkownika usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Skonfiguruj wymagania dotyczące złożoności hasła](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Włącz uwierzytelnianie wieloskładnikowe](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Konfigurowanie dostawców tożsamości dodatkowe, takie jak [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [w usłudze Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)i innym osobom.
* [Za pomocą interfejsu API programu Graph usługi Azure AD](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) do pobierania dodatkowych informacji dotyczących użytkowników, takich jak członkostwo w grupie z dzierżawy usługi Azure AD B2C.
* [Zabezpieczanie interfejsu API sieci web za pomocą usługi Azure AD B2C platformy ASP.NET Core](xref:security/authentication/azure-ad-b2c-webapi).
* [Wywoływanie interfejsu web API platformy .NET z aplikacji sieci web platformy .NET przy użyciu usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
