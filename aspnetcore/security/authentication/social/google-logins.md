---
title: Ustawienia logowania zewnętrznego Google w ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelnianie użytkownika konto Google do istniejącej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: aba12a94a573db35eadaa6a38f2fcf074b7b64c2
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Ustawienia logowania zewnętrznego Google w ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku przedstawiono sposób umożliwić użytkownikom logowanie za pomocą swojego konta Google + przy użyciu przykładowy projekt platformy ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index). Rozpoczniemy wykonując [kroki oficjalnego](https://developers.google.com/identity/sign-in/web/devconsole-project) do utworzenia nowej aplikacji w konsoli interfejsu API firmy Google.

## <a name="create-the-app-in-google-api-console"></a>Tworzenie aplikacji w konsoli interfejsu API firmy Google

* Przejdź do [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) i zaloguj się. Jeśli nie masz już konto Google, użyj **więcej opcji** > **[Tworzenie konta](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  łącze, aby go utworzyć:

![Konsoli interfejsu API firmy Google](index/_static/GoogleConsoleLogin.png)

* Nastąpi przekierowanie do **biblioteki interfejsu API menedżera** strony:

![Strona Menedżer interfejsu API biblioteki](index/_static/GoogleConsoleSwitchboard.png)

* Wybierz **Utwórz** , a następnie wprowadź Twojej **Nazwa projektu**:

![Okno dialogowe nowego projektu](index/_static/GoogleConsoleNewProj.png)

* Po zaakceptowaniu okno dialogowe, są przekierowany do strony biblioteki, dzięki czemu można wybrać funkcje dla nowej aplikacji. Znajdź **interfejsu API Google +** na liście i kliknij jego łącze, aby dodać funkcję interfejsu API:

![Strona Menedżer interfejsu API biblioteki](index/_static/GoogleConsoleChooseApi.png)

* Zostanie wyświetlona strona dla nowo dodanego interfejsu API. Wybierz **włączyć** dodać Google + znak w funkcji do aplikacji:

![Strona Menedżer interfejsu API Google + interfejsu API](index/_static/GoogleConsoleEnableApi.png)

* Po włączeniu interfejs API, naciśnij przycisk **Utwórz poświadczenia** skonfigurować kluczy tajnych:

![Strona Menedżer interfejsu API Google + interfejsu API](index/_static/GoogleConsoleGoCredentials.png)

* Wybierz:
   * **Google+ API**
   * **Serwer sieci Web (np. node.js, Tomcat)**, i
   * **Dane użytkownika**:

![Strona poświadczeń Menedżer interfejsu API: Sprawdź, jaki rodzaj poświadczeń można muszą panelu](index/_static/GoogleConsoleChooseCred.png)

* Wybierz **poświadczeniami, które są potrzebne?** który przyjmuje do drugiego kroku konfiguracji aplikacji **Utwórz identyfikator klienta OAuth 2.0**:

![Strona poświadczeń Menedżer interfejsu API: Utwórz identyfikator klienta OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Ponieważ tworzymy projektu Google + z tylko jedną funkcję (logowania), możemy wprowadzić ten sam **nazwa** dla Identyfikatora klienta OAuth 2.0, użyliśmy dla projektu.

* Wprowadź identyfikator URI programowania z */signin-google* dołączany do **autoryzowanych przekierowania URI** pola (na przykład: `https://localhost:44320/signin-google`). Uwierzytelnianie serwisu Google skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań w */signin-google* trasy do zaimplementowania przepływu OAuth.

* Naciśnij klawisz TAB, aby dodać **autoryzowanych przekierowania URI** wpisu.

* Wybierz **Utwórz identyfikator klienta**, który umożliwia przejście do trzeciego stopnia **ustawienie ekranu zgoda OAuth 2.0**:

![Strona poświadczeń Menedżer interfejsu API: ustawianie ekranu zgoda OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Wprowadź skierowane do publicznego **adres E-mail** i **nazwa produktu** wyświetlany dla aplikacji, jeśli Google + monity użytkownika do logowania. Dodatkowe opcje są dostępne w obszarze **więcej opcji dostosowania**.

* Wybierz **Kontynuuj** do przejdź do ostatniego kroku **Pobierz poświadczenia**:

![Strona poświadczeń Menedżer interfejsu API: Pobierz poświadczenia](index/_static/GoogleConsoleFinish.png)

* Wybierz **Pobierz** można zapisać pliku JSON z haseł aplikacji i **gotowe** aby zakończyć tworzenie nowej aplikacji.

* W przypadku wdrażania lokacji należy ponownie **konsoli Google** i zarejestrować nowego publicznego adresu url.

## <a name="store-google-clientid-and-clientsecret"></a>Sklep Google ClientID i ClientSecret

Link ustawień poufnych, takich jak Google `Client ID` i `Client Secret` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](xref:security/app-secrets). Do celów tego samouczka, nazwa tokeny `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret`.

Tokeny te wartości można znaleźć w pliku JSON pobranego w poprzednim kroku, w obszarze `web.client_id` i `web.client_secret`.

## <a name="configure-google-authentication"></a>Konfigurowanie uwierzytelniania serwisu Google

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Dodaj usługę Google w `ConfigureServices` metody w *Startup.cs* pliku:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Szablon projektu używany w tym samouczku upewnia się, że [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) pakiet jest zainstalowany.

 * Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.
 * Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

Dodaj oprogramowanie pośredniczące Google w `Configure` metody w *Startup.cs* pliku:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

* * *
Zobacz [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelniania serwisu Google. To może być używane do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-google"></a>Zaloguj się przy użyciu usługi Google

Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Pojawia się opcję, aby zalogować się przy użyciu usługi Google:

![Aplikacji sieci Web w programie Microsoft Edge: użytkownik nie jest uwierzytelniony](index/_static/DoneGoogle.png)

Po kliknięciu Google, nastąpi przekierowanie do sklepu Google uwierzytelniania:

![Dialog uwierzytelniania Google](index/_static/GoogleLogin.png)

Po wprowadzeniu poświadczeń Google, następnie nastąpi przekierowanie do witryny sieci web, w którym można ustawić adres e-mail.

Użytkownik jest obecnie zalogowany przy użyciu poświadczeń konta Google:

![Aplikacji sieci Web w programie Microsoft Edge: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli zostanie wyświetlony `403 (Forbidden)` strony błędu z własną aplikację podczas pracy w trybie Programowanie (lub podziału do debugera z tego samego błędu), upewnij się, że **interfejsu API Google +** została włączona w **biblioteki interfejsu API menedżera** przez wykonanie kroków podanych [wcześniej na tej stronie](#create-the-app-in-google-api-console). Jeśli logowanie nie działa, a nie ma dostępnych żadnych błędów, przełącz się do trybu programowanie ułatwiające debugować ten problem.
* **Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamości nie jest skonfigurowana pod wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*. Szablon projektu używany w tym samouczku zapewnia to zrobić.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, otrzyma *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać z serwisem Google. Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](xref:security/authentication/social/index).

* Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy zresetować `ClientSecret` w konsoli interfejsu API firmy Google.

* Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` zgodnie z ustawieniami aplikacji w portalu Azure. System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.
