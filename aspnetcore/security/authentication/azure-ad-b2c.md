---
title: "Uwierzytelnianie w chmurze z usługi Azure Active Directory B2C"
author: camsoper
description: "Wykryj sposobu konfigurowania uwierzytelniania usługi Azure Active Directory B2C za pomocą platformy ASP.NET Core."
ms.author: casoper
manager: wpickett
ms.date: 01/12/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/azure-ad-b2c
custom: mvc
ms.openlocfilehash: 5c4716022c61e33b0301fa0077f911dcc4b3628c
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c"></a>Uwierzytelnianie w chmurze z usługi Azure Active Directory B2C

Przez [Soper kamery](https://twitter.com/camsoper)

[Usługa Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) jest rozwiązaniem do zarządzania tożsamościami chmury dla sieci web i aplikacji mobilnych. Usługa zapewnia uwierzytelnianie dla aplikacji hostowanych w chmurze i lokalnie. Typy uwierzytelniania obejmują indywidualnych kont, kont sieci społecznościowych i federacyjnych konta przedsiębiorstwa.  Ponadto usługi Azure AD B2C zapewniają uwierzytelnianie wieloskładnikowe z minimalną konfiguracją.

W tym samouczku przedstawiono sposób:

> [!div class="checklist"]
> * Tworzenie dzierżawy usługi Azure Active Directory B2C
> * Zarejestrować aplikację w Azure AD B2C
> * Tworzenie aplikacji sieci web platformy ASP.NET Core skonfigurowany do używania dzierżawy usługi Azure AD B2C do uwierzytelniania za pomocą programu Visual Studio
> * Konfigurowanie zasad kontroli zachowania dzierżawy usługi Azure AD B2C

## <a name="prerequisites"></a>Wymagania wstępne

Poniżej przedstawiono wymagania dla tego przewodnika:

* [Subskrypcja Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). 
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (dowolna wersja)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Tworzenie dzierżawy usługi Azure Active Directory B2C

Tworzenie dzierżawy usługi Azure Active Directory B2C [zgodnie z opisem w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-get-started). Po wyświetleniu monitu skojarzenie dzierżawcy z subskrypcją platformy Azure jest opcjonalne dla tego samouczka.

## <a name="register-the-app-in-azure-ad-b2c"></a>Zarejestrować aplikację w Azure AD B2C

W nowo utworzone dzierżawy usługi Azure AD B2C, zarejestrować swoją aplikację przy użyciu [kroki opisane w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) w obszarze **zarejestrować aplikacji sieci web** sekcji. Zatrzymaj przy **Utwórz klucz tajny klienta aplikacji sieci web** sekcji. Klucz tajny klienta nie jest wymagane dla tego samouczka. 

Użyj następujących wartości:

| Ustawienie                       | Wartość                     | Uwagi                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                      | *\<Nazwa aplikacji\>*            | Wprowadź **nazwa** aplikacji, który opisuje aplikacji dla konsumentów.                                                                                                                                 |
| **Uwzględnij aplikację sieci web / interfejs API sieci web** | Tak                       |                                                                                                                                                                                                    |
| **Zezwalaj na niejawnego przepływu**       | Tak                       |                                                                                                                                                                                                    |
| **Adres URL odpowiedzi**                 | `https://localhost:44300` | Adresy URL odpowiedzi są punkty końcowe, w którym usługi Azure AD B2C zwraca wszystkie tokeny żądań aplikacji. Program Visual Studio udostępnia adres URL odpowiedzi służący do użycia. Teraz, wprowadź `https://localhost:44300` jest wypełnienie formularza. |
| **Identyfikator URI aplikacji**                | Pozostaw puste               | Nie są wymagane dla tego samouczka.                                                                                                                                                                    |
| **Zawierają natywnego klienta**     | Nie                        |                                                                                                                                                                                                    |

> [!WARNING]
> W przypadku konfigurowania adresu URL odpowiedzi z systemem innym niż localhost, należy pamiętać o [ograniczenia dotyczące dozwolonych na liście adres URL odpowiedzi](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Po zarejestrowaniu aplikacji, zostanie wyświetlona lista aplikacji w dzierżawie. Wybierz aplikację, która właśnie została zarejestrowana. Wybierz **kopiowania** ikonę z prawej strony **identyfikator aplikacji** pola, aby skopiować identyfikator aplikacji do Schowka.

Nic więcej w tym momencie można skonfigurować w dzierżawie usługi Azure AD B2C, ale pozostaw otwarte okno przeglądarki. Brak dodatkowych czynności konfiguracyjnych po utworzeniu aplikacji platformy ASP.NET Core.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Tworzenie aplikacji platformy ASP.NET Core w Visual Studio 2017 r.

Aby korzystać z dzierżawy usługi Azure AD B2C do uwierzytelniania można skonfigurować szablonu aplikacji sieci Web w usłudze Visual Studio.

W programie Visual Studio:

1. Utwórz nową aplikację sieci Web platformy ASP.NET Core. 
2. Wybierz **aplikacji sieci Web** z listy szablonów.
3. Wybierz **Zmień uwierzytelnianie** przycisku.
    
    ![Zmień przycisk uwierzytelniania](./azure-ad-b2c/_static/changeauth.png)

4. W **Zmień uwierzytelnianie** okno dialogowe, wybierz opcję **indywidualnych kont użytkowników**, a następnie wybierz **Połącz z istniejącym magazynem użytkownika w chmurze** na liście rozwijanej. 
    
    ![Dialog uwierzytelniania zmiany](./azure-ad-b2c/_static/changeauthdialog.png)

5. Wypełnienie formularza z następujących wartości:
    
    | Ustawienie                       | Wartość                                             |
    |-------------------------------|---------------------------------------------------|
    | **Nazwa domeny**               | *\<nazwę domeny dzierżawy usługi B2C\>*          |
    | **Identyfikator aplikacji**            | *\<Wklej identyfikator aplikacji ze Schowka\>* |
    | **Ścieżka wywołania zwrotnego**             | *\<Użyj wartości domyślnej\>*                       |
    | **Zasady rejestracji i logowania** | `B2C_1_SiUpIn`                                    |
    | **Zasady resetowania hasła**     | `B2C_1_SSPR`                                      |
    | **Edytuj profil zasady**       | *\<Pozostaw puste\>*                                 |

    Wybierz **kopiowania** znajdujący się obok podsekcji **identyfikatora URI odpowiedzi** można skopiować do Schowka identyfikatora URI odpowiedzi. Wybierz **OK** zamknąć **Zmień uwierzytelnianie** okna dialogowego. Wybierz **OK** do utworzenia aplikacji sieci web.

## <a name="finish-the-b2c-app-registration"></a>Zakończ rejestracji aplikacji B2C

Wróć do okna przeglądarki z właściwościami aplikacji B2C nadal otwarte. Zmień tymczasowy **adres URL odpowiedzi** określony wcześniej wartości skopiowanych z programu Visual Studio. Wybierz **zapisać** w górnej części okna.

> [!TIP]
> Jeśli nie skopiuj adres URL odpowiedzi, we właściwościach projektu sieci web za pomocą adresu protokołu SSL z karty debugowania i Dołącz **CallbackPath** wartość z *appsettings.json*.

## <a name="configure-policies"></a>Konfigurowanie zasad

W dokumentacji usługi Azure AD B2C, wykonaj kroki [utworzyć zasadę tworzenia konta lub logowanie](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), a następnie [Utwórz zasady resetowania hasła](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Użyj przykładowe wartości podane w dokumentacji dotyczącej **dostawców tożsamości**, **atrybuty rejestracji**, i **oświadczenia aplikacji**. Przy użyciu **Uruchom teraz** przycisk, aby przetestować zasady, zgodnie z opisem w dokumentacji są opcjonalne.

> [!WARNING]
> Upewnij się, nazwy zasad są dokładnie zgodnie z opisem w dokumentacji, jak te zasady były używane w **Zmień uwierzytelnianie** okna dialogowego w programie Visual Studio. Nazwy zasad można sprawdzić w *appsettings.json*.

## <a name="run-the-app"></a>Uruchamianie aplikacji

W programie Visual Studio, naciśnij klawisz **F5** Aby skompilować i uruchomić aplikację. Po uruchomieniu aplikacji sieci web, wybierz **Zaloguj**.

![Zaloguj się do aplikacji](./azure-ad-b2c/_static/signin.png)

Przeglądarka przekierowuje do dzierżawy usługi Azure AD B2C. Zaloguj się przy użyciu istniejącego konta (Jeśli utworzono jedną testowania zasad) lub wybierz **Zamów teraz** Aby utworzyć nowe konto. **Nie pamiętasz hasła?** łącze służy do zresetować zapomniane hasło.

![Logowanie w usłudze Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

Po pomyślnym zalogowaniu przeglądarka przekierowuje do aplikacji sieci web.

![Powodzenie](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną rozpoznane jak:

> [!div class="checklist"]
> * Tworzenie dzierżawy usługi Azure Active Directory B2C
> * Zarejestrować aplikację w Azure AD B2C
> * Tworzenie aplikacji sieci Web ASP.NET Core skonfigurowany do używania dzierżawy usługi Azure AD B2C do uwierzytelniania za pomocą programu Visual Studio
> * Konfigurowanie zasad kontroli zachowania dzierżawy usługi Azure AD B2C

Teraz, gdy aplikacja platformy ASP.NET Core jest skonfigurowany do używania usługi Azure AD B2C do uwierzytelniania, [atrybutu autoryzacji](xref:security/authorization/simple) może służyć do zabezpieczenia aplikacji. Kontynuuj tworzenie aplikacji przez uczenia:

* [Dostosowywanie interfejsu użytkownika usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Skonfiguruj wymagania dotyczące złożoności hasła](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Włączanie uwierzytelniania wieloskładnikowego](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Skonfiguruj dostawców tożsamości dodatkowe, takie jak [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [w usłudze Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)i inne.
* [Za pomocą interfejsu API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) można pobrać dodatkowe informacje dotyczące użytkownika, takich jak członkostwo w grupie z dzierżawy usługi Azure AD B2C.