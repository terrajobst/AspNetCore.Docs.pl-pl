---
title: Ustawienia logowania zewnętrznego Google w programie ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Google do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: dfda83e1d7cf3c5ff8e31de20c15d468de5d15c0
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708455"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Ustawienia logowania zewnętrznego Google w programie ASP.NET Core

Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku dowiesz się, jak umożliwić użytkownikom logowanie się przy użyciu swojego konta Google + przy użyciu przykładowy projekt programu ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index). Rozpoczniemy pracę, postępując zgodnie z [oficjalnych kroków](https://developers.google.com/identity/sign-in/web/devconsole-project) do utworzenia nowej aplikacji w konsoli interfejsu API Google.

## <a name="create-the-app-in-google-api-console"></a>Tworzenie aplikacji w konsoli interfejsu API Google

* Przejdź do [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) i zaloguj się. Jeśli nie masz już konto Google, użyj **więcej opcji** > **[Utwórz konto](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  łącze, aby go utworzyć:

![Konsola interfejsu API Google](index/_static/GoogleConsoleLogin.png)

* Nastąpi przekierowanie do **Menedżer interfejsu API biblioteki** strony:

![Strona biblioteki Menedżer interfejsu API](index/_static/GoogleConsoleSwitchboard.png)

* Naciśnij pozycję **Utwórz** i wprowadź swoje **Nazwa projektu**:

![Okno dialogowe nowego projektu](index/_static/GoogleConsoleNewProj.png)

* Po zaakceptowaniu okna dialogowego, nastąpi przekierowanie do strony biblioteki umożliwia wybranie funkcji dla nowej aplikacji. Znajdź **interfejsu API Google +** na liście i kliknij jego łącze, aby dodać funkcję interfejsu API:

![Strona biblioteki Menedżer interfejsu API](index/_static/GoogleConsoleChooseApi.png)

* Zostanie wyświetlona strona dla nowo dodanego interfejsu API. Naciśnij pozycję **Włącz** dodać Google + znak w funkcji do aplikacji:

![Strona Menedżer interfejsu API Google + interfejsu API](index/_static/GoogleConsoleEnableApi.png)

* Po włączeniu interfejsu API, naciśnij przycisk **Utwórz poświadczenia** skonfigurować wpisy tajne:

![Strona Menedżer interfejsu API Google + interfejsu API](index/_static/GoogleConsoleGoCredentials.png)

* Wybierz:
  * **Google+ API**
  * **Serwer sieci Web (np. środowisko node.js, Tomcat)**, i
  * **Dane użytkownika**:

![Strona poświadczeń Menedżer interfejsu API: Dowiedz się, jaki rodzaj poświadczeń należy panelu](index/_static/GoogleConsoleChooseCred.png)

* Naciśnij pozycję **jakie poświadczenia są potrzebne?** której przejście do drugiego kroku konfiguracji aplikacji **Utwórz identyfikator klienta OAuth 2.0**:

![Strona poświadczeń Menedżer interfejsu API: Utwórz identyfikator klienta OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Ponieważ tworzymy projektu Google + przy użyciu tylko jednej funkcji (Zaloguj), możemy wprowadzić ten sam **nazwa** dla Identyfikatora klienta OAuth 2.0, użyliśmy dla projektu.

* Wprowadź identyfikator URI programistycznych dzięki `/signin-google` dołączany do **identyfikatory URI przekierowania autoryzowanych** pola (na przykład: `https://localhost:44320/signin-google`). Uwierzytelnianie serwisu Google skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w `/signin-google` trasy do zaimplementowania przepływu uwierzytelniania OAuth.

> [!NOTE]
> Segmentem identyfikatora URI `/signin-google` jest ustawiony jako zwrotnym domyślnego dostawcę uwierzytelniania serwisu Google. Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania Google za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) klasy.

* Naciśnij klawisz TAB, aby dodać **identyfikatory URI przekierowania autoryzowanych** wpisu.

* Naciśnij pozycję **Utwórz identyfikator klienta**, która umożliwia przejście do trzeci krok **skonfigurować ekran zgody OAuth 2.0**:

![Strona poświadczeń Menedżer interfejsu API: Konfigurowanie ekran zgody OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Wprowadź swoje użytek publiczny **adres E-mail** i **nazwa produktu** wyświetlany dla aplikacji, gdy Google + monituje użytkownika do logowania. Dodatkowe opcje są dostępne w obszarze **dodatkowe opcje dostosowywania**.

* Naciśnij pozycję **Kontynuuj** do przejdź do ostatniego kroku **Pobierz poświadczenia**:

![Strona poświadczeń Menedżer interfejsu API: Pobierz poświadczenia](index/_static/GoogleConsoleFinish.png)

* Naciśnij pozycję **Pobierz** można zapisać plik w formacie JSON za pomocą kluczy tajnych aplikacji, a **gotowe** aby ukończyć tworzenie nowej aplikacji.

* W przypadku wdrażania w witrynie będzie konieczne ponownie przeanalizowanie **konsoli Google** i rejestrowanie nowego publicznego adresu url.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID i ClientSecret

Link ustawień poufnych, takich jak Google `Client ID` i `Client Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets). Do celów tego samouczka, nazwa tokeny `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret`.

Wartości dla tych tokenów można znaleźć w pliku JSON, pobrany w poprzednim kroku, w obszarze `web.client_id` i `web.client_secret`.

## <a name="configure-google-authentication"></a>Konfigurowanie uwierzytelniania serwisu Google

::: moniker range=">= aspnetcore-2.0"

Dodaj usługę Google w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

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

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Szablon projektu, w tym samouczku używane zapewnia, że [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) pakiet jest zainstalowany.

* Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
* Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Dodaj oprogramowanie pośredniczące Google w `Configure` method in Class metoda *Startup.cs* pliku:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

Zobacz [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie serwisu Google. Może to służyć do żądania różne informacje o użytkowniku.

## <a name="sign-in-with-google"></a>Zaloguj się przy użyciu Google

Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**. Zostanie wyświetlona opcja Zaloguj się przy użyciu Google:

![Aplikację sieci Web działającą w programie Microsoft Edge: użytkownik nie jest uwierzytelniony](index/_static/DoneGoogle.png)

Po kliknięciu w usłudze Google, nastąpi przekierowanie do usługi Google do uwierzytelniania:

![Okno dialogowe uwierzytelniania Google](index/_static/GoogleLogin.png)

Po wprowadzeniu poświadczeń Google, następnie nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.

Obecnie zalogowano Cię przy użyciu poświadczeń konta Google:

![Aplikację sieci Web działającą w programie Microsoft Edge: użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

* Jeśli zostanie wyświetlony `403 (Forbidden)` stronę błędu z własnej aplikacji, gdy działa w trybie projektowania (lub przerwanie w debugerze z tym samym błędem), upewnij się, że **interfejsu API Google +** zostało włączone w **Menedżer interfejsu API biblioteki** przez wykonanie kroków podanych [wcześniej na tej stronie](#create-the-app-in-google-api-console). Jeśli nie otrzymujesz błędy logowania nie rozwiąże problemu, przełącz się do trybu opracowywania, aby ułatwić debugowanie problemu.
* **Platforma ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*. Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.
* Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu. Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.

## <a name="next-steps"></a>Następne kroki

* W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google. Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).

* Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `ClientSecret` w konsoli interfejsu API Google.

* Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.
