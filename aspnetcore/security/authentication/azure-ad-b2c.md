---
title: Uwierzytelnianie w chmurze za pomocą Azure Active Directory B2C w ASP.NET Core
author: camsoper
description: Dowiedz się, jak skonfigurować uwierzytelnianie Azure Active Directory B2C przy użyciu ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 136fa47788456492a9a7fe6d9d9e5996c13e8c20
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727274"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Uwierzytelnianie w chmurze za pomocą Azure Active Directory B2C w ASP.NET Core

Przez [Soper kamery](https://twitter.com/camsoper)

[Usługa Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji internetowych i mobilnych. Usługa zapewnia uwierzytelnianie dla aplikacji hostowanych w chmurze i lokalnych. Typy uwierzytelniania obejmują indywidualnych kont, kont sieci społecznościowych i federacyjnych konta przedsiębiorstwa. Ponadto Azure AD B2C może zapewnić uwierzytelnianie wieloskładnikowe z minimalną konfiguracją.

> [!TIP]
> Usługa Azure Active Directory (Azure AD) i Azure AD B2C są osobne oferty. Dzierżawy usługi Azure AD organizacja, podczas gdy dzierżawy usługi Azure AD B2C reprezentuje kolekcję tożsamości do użycia z aplikacjami danej firmy. Aby dowiedzieć się więcej, zobacz [usługi Azure AD B2C: często zadawane pytania (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

W tym samouczku pokazano, jak:

> [!div class="checklist"]
> * Tworzenie dzierżawy Azure Active Directory B2C
> * Rejestrowanie aplikacji w Azure AD B2C
> * Użyj programu Visual Studio, aby utworzyć aplikację sieci Web ASP.NET Core skonfigurowaną do korzystania z dzierżawy Azure AD B2C na potrzeby uwierzytelniania
> * Konfigurowanie zasad kontrolujących zachowanie dzierżawy Azure AD B2C

## <a name="prerequisites"></a>Wymagania wstępne

Wymagane jest spełnienie następujących w ramach tego przewodnika:

* [Subskrypcja Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Tworzenie dzierżawy usługi Azure Active Directory B2C

Utwórz dzierżawę Azure Active Directory B2C [, zgodnie z opisem w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-get-started). Po wyświetleniu monitu skojarzenie dzierżawcy z subskrypcją platformy Azure jest opcjonalny w tym samouczku.

## <a name="register-the-app-in-azure-ad-b2c"></a>Zarejestruj aplikację w Azure AD B2C

W nowo utworzonym dzierżawie Azure AD B2C Zarejestruj swoją aplikację, korzystając [z procedury opisanej w dokumentacji](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) w sekcji **Rejestrowanie aplikacji sieci Web** . Zatrzyma **Tworzenie klucza tajnego klienta aplikacji sieci web** sekcji. Klucz tajny klienta nie jest wymagana na potrzeby tego samouczka. 

Użyj następujących wartości:

| Ustawienie                       | Wartość                     | Uwagi                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                      | *Nazwa &lt;aplikacji&gt;*        | Wprowadź **nazwa** dla aplikacji, który opisuje aplikację dla użytkowników.                                                                                                                                 |
| **Uwzględnij aplikację sieci web / interfejs API sieci web** | Tak                       |                                                                                                                                                                                                    |
| **Zezwalaj na niejawny przepływ**       | Tak                       |                                                                                                                                                                                                    |
| **Adres URL odpowiedzi**                 | `https://localhost:44300/signin-oidc` | Adresy URL odpowiedzi to punkty końcowe, w którym usługi Azure AD B2C zwraca wszelkie tokeny żądane przez aplikację. Program Visual Studio udostępnia adres URL odpowiedzi, który ma być używany. Na razie wprowadź `https://localhost:44300/signin-oidc`, aby zakończyć formularz. |
| **Identyfikator URI Identyfikatora aplikacji**                | Pozostaw puste               | Nie jest wymagane na potrzeby tego samouczka.                                                                                                                                                                    |
| **Dołącz klienta natywnego**     | Nie                        |                                                                                                                                                                                                    |

> [!WARNING]
> Jeśli konfigurujesz adres URL odpowiedzi bez hosta lokalnego, weź pod uwagę [ograniczenia dotyczące tego, co jest dozwolone na liście adresów URL odpowiedzi](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application). 

Po zarejestrowaniu aplikacji zostanie wyświetlona lista aplikacji w dzierżawie. Wybierz aplikację, która została właśnie zarejestrowana. Wybierz **kopiowania** ikony po prawej stronie **identyfikator aplikacji** pola, aby skopiować go do Schowka.

W tej chwili nie można skonfigurować niczego w dzierżawie Azure AD B2C, ale pozostawić otwarte okno przeglądarki. Po utworzeniu aplikacji ASP.NET Core jest dostępna większa konfiguracja.

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>Tworzenie aplikacji ASP.NET Core w programie Visual Studio

Szablon aplikacji sieci Web w usłudze Visual Studio można skonfigurować do korzystania z dzierżawy usługi Azure AD B2C do uwierzytelniania.

W programie Visual Studio:

1. Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. 
2. Z listy szablonów wybierz pozycję **aplikacja sieci Web** .
3. Wybierz **Zmień uwierzytelnianie** przycisku.
    
    ![Zmień przycisk uwierzytelniania](./azure-ad-b2c/_static/changeauth.png)

4. W oknie dialogowym **Zmienianie uwierzytelniania** wybierz pozycję **indywidualne konta użytkowników**, a następnie wybierz pozycję **Połącz z istniejącym magazynem użytkowników w chmurze** na liście rozwijanej. 
    
    ![Okno dialogowe uwierzytelniania zmiany](./azure-ad-b2c/_static/changeauthdialog.png)

5. Wypełnij formularz następującymi wartościami:
    
    | Ustawienie                       | Wartość                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nazwa domeny**               | *&lt;nazwę domeny dzierżawy B2C&gt;*          |
    | **Identyfikator aplikacji**            | *&lt;wkleić identyfikator aplikacji ze schowka&gt;* |
    | **Ścieżka wywołania zwrotnego**             | *&lt;użyć wartości domyślnej&gt;*                       |
    | **Zasady tworzenia konta lub logowania** | `B2C_1_SiUpIn`                                        |
    | **Zasady resetowania haseł**     | `B2C_1_SSPR`                                          |
    | **Edytowanie zasad profilu**       | *&lt;pozostawić puste&gt;*                                 |
    
    Wybierz link **Kopiuj** obok **identyfikatora URI odpowiedzi** , aby skopiować identyfikator URI odpowiedzi do Schowka. Wybierz **OK** zamknąć **Zmień uwierzytelnianie** okna dialogowego. Wybierz **OK** umożliwiające utworzenie aplikacji sieci web.

## <a name="finish-the-b2c-app-registration"></a>Kończenie rejestracji aplikacji B2C

Wróć do okna przeglądarki z wciąż otwartymi właściwościami aplikacji B2C. Zmień tymczasowy **adres URL odpowiedzi** określony wcześniej na wartość skopiowaną z programu Visual Studio. Wybierz pozycję **Zapisz** w górnej części okna.

> [!TIP]
> Jeśli adres URL odpowiedzi nie został skopiowany, użyj adresu HTTPS z karty debugowanie we właściwościach projektu sieci Web i Dołącz wartość **CallbackPath** z pliku *appSettings. JSON*.

## <a name="configure-policies"></a>Konfigurowanie zasad

Wykonaj kroki opisane w dokumentacji Azure AD B2C, aby [utworzyć zasady tworzenia konta lub logowania](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), a następnie [Utwórz zasady resetowania hasła](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions). Użyj przykładowych wartości podane w dokumentacji dotyczącej **dostawców tożsamości**, **atrybuty tworzenia konta**, i **oświadczeń aplikacji**. Użycie przycisku **Uruchom teraz** w celu przetestowania zasad zgodnie z opisem w dokumentacji jest opcjonalne.

> [!WARNING]
> Upewnij się, że nazwy zasad są dokładnie zgodnie z opisem w dokumentacji, ponieważ te zasady były używane w oknie dialogowym **Zmienianie uwierzytelniania** w programie Visual Studio. Nazwy zasad można weryfikować w pliku *appSettings. JSON*.

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a>Konfigurowanie podstawowych opcji OpenIdConnectOptions/JwtBearer/cookie

Aby bezpośrednio skonfigurować podstawowe opcje, użyj odpowiedniej stałej schematu w `Startup.ConfigureServices`:

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a>Uruchamianie aplikacji

W programie Visual Studio naciśnij klawisz **F5** , aby skompilować i uruchomić aplikację. Po uruchomieniu aplikacji sieci Web wybierz pozycję **Akceptuj** , aby zaakceptować użycie plików cookie (Jeśli zostanie wyświetlony monit), a następnie wybierz pozycję **Zaloguj się**.

![Zaloguj się do aplikacji](./azure-ad-b2c/_static/signin.png)

Przeglądarka przekieruje do dzierżawy Azure AD B2C. Zaloguj się przy użyciu istniejącego konta, (Jeśli utworzono jedną testowania zasad) lub wybierz **Zarejestruj się teraz** do utworzenia nowego konta. **Nie pamiętasz hasła?** łącze służy do zresetować zapomniane hasło.

![Azure AD B2C logowanie](./azure-ad-b2c/_static/b2csts.png)

Po pomyślnym zalogowaniu przeglądarka przekieruje ją do aplikacji sieci Web.

![Powodzenie](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie dzierżawy Azure Active Directory B2C
> * Rejestrowanie aplikacji w Azure AD B2C
> * Użyj programu Visual Studio, aby utworzyć aplikację sieci Web ASP.NET Core skonfigurowaną do korzystania z dzierżawy Azure AD B2C na potrzeby uwierzytelniania
> * Konfigurowanie zasad kontrolujących zachowanie dzierżawy Azure AD B2C

Teraz, gdy aplikacja ASP.NET Core jest skonfigurowana do używania Azure AD B2C do uwierzytelniania, do zabezpieczenia aplikacji można użyć [atrybutu Autoryzuj](xref:security/authorization/simple) . Kontynuuj opracowywanie aplikacji, przepoznając się z:

* [Dostosowywanie interfejsu użytkownika usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Skonfiguruj wymagania dotyczące złożoności hasła](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Włącz uwierzytelnianie wieloskładnikowe](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Konfigurowanie dostawców tożsamości dodatkowe, takie jak [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [w usłudze Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)i innym osobom.
* [Za pomocą interfejsu API programu Graph usługi Azure AD](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) do pobierania dodatkowych informacji dotyczących użytkowników, takich jak członkostwo w grupie z dzierżawy usługi Azure AD B2C.
* [Zabezpiecz internetowy interfejs API ASP.NET Core przy użyciu Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).
* [Wywoływanie interfejsu web API platformy .NET z aplikacji sieci web platformy .NET przy użyciu usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).