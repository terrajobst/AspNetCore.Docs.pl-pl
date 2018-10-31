---
title: Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 1fae5af24359afc991a30cd2b8e2f6927845962b
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244804"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Zobacz [plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) dla platformy ASP.NET Core 1.1 i wersja 2.1.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)

W tym samouczku przedstawiono sposób kompilowania aplikacji platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła. Niniejszy samouczek jest **nie** początku tematu. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [Uwierzytelnianie](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a>Tworzenie aplikacji sieci web i tworzenia szkieletu tożsamości

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* W programie Visual Studio Utwórz nowy **aplikacji sieci Web** projektu o nazwie **WebPWrecover**.
* Wybierz **platformy ASP.NET Core 2.1**.
* Zachowaj wartość domyślną **uwierzytelniania** równa **bez uwierzytelniania**. Uwierzytelnianie jest dodawany w następnym kroku.

W następnym kroku:

* Ustaw układ strony *~/Pages/Shared/_Layout.cshtml*
* Wybierz *konta/rejestrowanie*
* Utwórz nową **klasa kontekstu danych**

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

Uruchom `dotnet aspnet-codegenerator identity --help` Aby uzyskać pomoc na temat narzędzia do tworzenia szkieletów.

------

Postępuj zgodnie z instrukcjami w [Włącz uwierzytelnianie](xref:security/authentication/scaffold-identity#useauthentication):

* Dodaj `app.UseAuthentication();` do `Startup.Configure`
* Dodaj `<partial name="_LoginPartial" />` do pliku układu.

## <a name="test-new-user-registration"></a>Rejestrowanie nowego użytkownika testowego

Uruchom aplikację, wybierz **zarejestrować** łącze, a następnie zarejestrować użytkownik. W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu. Po przesłaniu rejestracji, użytkownik jest zalogowany do aplikacji. W dalszej części samouczka kod jest aktualizowana, dzięki czemu nowi użytkownicy nie mogą zalogować, dopóki nie zostanie zweryfikowana poczty e-mail.

[!INCLUDE[](~/includes/view-identity-db.md)]

Należy pamiętać, tabeli `EmailConfirmed` pole jest `False`.

Można używać tego adresu e-mail ponownie w następnym kroku, gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem. Kliknij prawym przyciskiem myszy na wiersz i wybierz **Usuń**. Usuwanie aliasu adresu e-mail ułatwia w poniższych krokach.

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Wymagaj potwierdzenie adresu e-mail

Jest najlepszym rozwiązaniem, aby potwierdzić adres e-mail rejestrowanie nowego użytkownika. Wiadomość e-mail potwierdzenia pomaga sprawdzić, mogą one nie personifikuje kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby). Załóżmy, że trzeba było forum dyskusyjnego i chcesz uniemożliwić "yli@example.com"z rejestracją jako"nolivetto@contoso.com". Bez potwierdzenia e-mail "nolivetto@contoso.com" może otrzymywać niechciane wiadomości e-mail z aplikacji. Załóżmy, że użytkownik przypadkowo zarejestrowana jako "ylo@example.com" i nie zostało już słowa "yli". W takich sytuacjach przydałaby się mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail. Potwierdzenie adresu e-mail zapewnia ograniczoną ochronę, od botów. Potwierdzenie adresu e-mail nie zapewniają ochronę przed złośliwymi użytkownikami z wieloma kontami e-mail.

Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do witryny sieci web, zanim potwierdzone pocztą e-mail.

Aktualizacja *Areas/Identity/IdentityHostingStartup.cs* będą musieli potwierdzony adres e-mail:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

`config.SignIn.RequireConfirmedEmail = true;` zarejestrowani użytkownicy uniemożliwia logowanie do momentu ich adres e-mail został potwierdzony.

### <a name="configure-email-provider"></a>Konfigurowanie dostawcy poczty e-mail

W tym samouczku [SendGrid](https://sendgrid.com) służy do wysyłania wiadomości e-mail. Potrzebujesz konta SendGrid i klucz do wysyłania wiadomości e-mail. Można użyć innych dostawców poczty e-mail. Platforma ASP.NET Core 2.x obejmuje `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji. Zaleca się, że używasz usługi SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail. SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.

Utwórz klasę, można pobrać klucza zabezpieczanie poczty e-mail. W tym przykładzie należy utworzyć *Services/AuthMessageSenderOptions.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>Konfigurowanie wpisami tajnymi użytkowników usługi SendGrid

Dodaj unikatowy `<UserSecretsId>` wartość `<PropertyGroup>` elementu w pliku projektu:

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets). Na przykład:

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Na Windows, klucza tajnego Manager przechowuje par kluczy i wartości w *secrets.json* w pliku `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` katalogu.

Zawartość *secrets.json* pliku nie są szyfrowane. *Secrets.json* plików znajdują się poniżej ( `SendGridKey` wartość została usunięta.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
Aby uzyskać więcej informacji, zobacz [wzorzec opcje](xref:fundamentals/configuration/options) i [konfiguracji](xref:fundamentals/configuration/index).

### <a name="install-sendgrid"></a>Instalowanie usługi SendGrid

W tym samouczku przedstawiono sposób dodawania powiadomienia e-mail za pośrednictwem [SendGrid](https://sendgrid.com/), ale możesz wysłać wiadomość e-mail przy użyciu SMTP i innych mechanizmów.

Zainstaluj `SendGrid` pakietu NuGet:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

W konsoli Menedżera pakietów wprowadź następujące polecenie:

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Z poziomu konsoli wprowadź następujące polecenie:

```cli
dotnet add package SendGrid
```

------

Zobacz [bezpłatnie Rozpocznij pracę za pomocą usługi SendGrid](https://sendgrid.com/free/) zarejestrować o utworzenie bezpłatnego konta SendGrid.
### <a name="implement-iemailsender"></a>Implementowanie IEmailSender

Zaimplementowanie `IEmailSender`, Utwórz *Services/EmailSender.cs* kodem podobny do następującego:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>Konfigurowanie uruchamiania do obsługi poczty e-mail

Dodaj następujący kod do `ConfigureServices` method in Class metoda *Startup.cs* pliku:

* Dodaj `EmailSender` jako usługi singleton.
* Zarejestruj `AuthMessageSenderOptions` wystąpienia konfiguracji.

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>Włącz odzyskiwanie potwierdzenia i hasło konta

Szablon zawiera kod odzyskiwania potwierdzenia i hasło konta. Znajdź `OnPostAsync` method in Class metoda *Areas/Identity/Pages/Account/Register.cshtml.cs*.

Uniemożliwić nowym użytkownikom zalogowanie się automatycznie, zakomentowując następujący wiersz:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Za pomocą zmieniony wiersz wyróżniony pokazano całą metodę:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a>Rejestrowania i resetowania hasła oraz Potwierdź adres e-mail

Uruchamianie aplikacji sieci web i przetestować potwierdzenie konta i hasła odzyskiwania przepływu.

* Uruchom aplikację i zarejestrować nowego użytkownika

  ![Widok zarejestrować konto aplikacji sieci Web](accconfirm/_static/loginaccconfirm1.png)

* Sprawdź pocztę e-mail, aby uzyskać link do potwierdzenia konta. Zobacz [debugowania e-mail](#debug) otrzymasz wiadomości e-mail.
* Kliknij link, aby potwierdzić swój adres e-mail.
* Zaloguj się przy użyciu poczty e-mail i hasło.
* Wyloguj się.

### <a name="view-the-manage-page"></a>Wyświetlanie strony zarządzania

Wybierz swoją nazwę użytkownika w przeglądarce: ![okno przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)

Może być konieczne Rozwiń pasek nawigacyjny, aby wyświetlić nazwę użytkownika.

![pasek nawigacyjny](accconfirm/_static/x.png)

Zostanie wyświetlona strona zarządzania, z **profilu** wybraną kartą. **E-mail** przedstawia pole wyboru, wskazujący adres e-mail został potwierdzony.

### <a name="test-password-reset"></a>Resetowanie hasła testu

* Jeśli zalogowano Cię, wybierz opcję **wylogowania**.
* Wybierz **Zaloguj** łącze, a następnie wybierz pozycję **nie pamiętasz hasła?** łącza.
* Wprowadź adres e-mail, którego użyłeś do zarejestrować konto.
* Wiadomość e-mail z linkiem do zresetowania hasła są wysyłane. Sprawdź pocztę e-mail, a następnie kliknij link, aby zresetować hasło. Po pomyślnie zresetowano hasło można rejestrować się za pomocą poczty e-mail i nowe hasło.

<a name="debug"></a>

### <a name="debug-email"></a>Debugowanie poczty e-mail

Jeśli nie można rozpocząć pracę poczty e-mail:

* Ustaw punkt przerwania `EmailSender.Execute` Aby zweryfikować `SendGridClient.SendEmailAsync` jest wywoływana.
* Tworzenie [aplikacji konsoli, aby wysłać wiadomość e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) przy użyciu kodu podobne do `EmailSender.Execute`.
* Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.
* Sprawdź folder wiadomości-śmieci.
* Spróbuj inny alias poczty e-mail na innego dostawcy poczty e-mail (Microsoft Yahoo, Gmail, itp.)
* Spróbuj wysłać do różnymi kontami e-mail.

**Ze względów bezpieczeństwa** jest **nie** używania wpisów tajnych produkcyjnych, w projektowania i testowania. Jeśli opublikujesz aplikację na platformie Azure, możesz ustawić wpisy tajne usługi SendGrid, jako ustawienia aplikacji w portalu usługi Azure Web App. System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.

## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont społecznościowych i lokalne logowanie

Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania. Zobacz [Facebook, Google i uwierzytelniania zewnętrznego dostawcy](xref:security/authentication/social/index).

Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail. W następującej kolejności "RickAndMSFT@gmail.com" najpierw jest tworzony jako lokalny identyfikator logowania; jednak możesz najpierw utworzyć konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

Kliknij pozycję **Zarządzaj** łącza. Należy pamiętać, zewnętrzne 0 (logowania społecznościowego) skojarzony z tym kontem.

![Zarządzanie widoku](accconfirm/_static/manage.png)

Kliknij link do innej usługi, zaloguj się i akceptowania żądań aplikacji. Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznych:

![Zarządzanie wyświetlania listy Facebooku widoku logowań zewnętrznych](accconfirm/_static/fb.png)

Te dwa konta zostały połączone. Możesz się zalogować przy użyciu dowolnego konta. Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania uwierzytelniania usługa nie działa lub większe prawdopodobieństwo ich utraty dostępu do swojego konta w sieci społecznościowej.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Włącz potwierdzenie konta, po lokacji ma użytkowników

Włączanie potwierdzenie konta w witrynie użytkownikom blokuje istniejących użytkowników. Istniejący użytkownicy są zablokowane, ponieważ ich konta nie są potwierdzone. Aby obejść istniejące blokady użytkownika, użyj jednej z następujących metod:

* Aktualizuj bazę danych, aby oznaczyć wszyscy istniejący użytkownicy, co zostało potwierdzone.
* Upewnij się, użytkownicy istniejącej. Na przykład usługi batch — wysyłanie wiadomości e-mail przy użyciu linki do potwierdzenia.

::: moniker-end
