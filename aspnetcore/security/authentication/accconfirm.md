---
title: Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803275"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)

W tym samouczku przedstawiono sposób kompilowania aplikacji platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła. Niniejszy samouczek jest **nie** początku tematu. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Uwierzytelnianie](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Zobacz [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC 1.1 i 2.x.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Utwórz nowy projekt ASP.NET Core przy użyciu interfejsu wiersza polecenia platformy .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Określa szablon projektu indywidualnych kont użytkowników.
* Windows, Dodaj `-uld` opcji. Określa, że LocalDB powinny być używane zamiast bazy danych SQLite.
* Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli używasz interfejsu wiersza polecenia lub bazy danych SQLite, uruchom następujące polecenie w oknie polecenia:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Określa szablon projektu indywidualnych kont użytkowników.
* Windows, Dodaj `-uld` opcji. Określa, że LocalDB powinny być używane zamiast bazy danych SQLite.
* Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.

---

Alternatywnie można utworzyć nowy projekt ASP.NET Core z programem Visual Studio:

* W programie Visual Studio Utwórz nowy **aplikacji sieci Web** projektu.
* Wybierz **platformy ASP.NET Core 2.0**. **.NET core** jest zaznaczona na poniższej ilustracji, ale można wybrać **.NET Framework**.
* Wybierz **Zmień uwierzytelnianie** i ustaw **indywidualne konta użytkowników**.
* Zachowaj wartość domyślną **Store użytkownika konta w aplikacji**.

![Okno dialogowe nowego projektu przedstawiający "Indywidualne konta użytkowników radio" wybrana](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Rejestrowanie nowego użytkownika testowego

Uruchom aplikację, wybierz **zarejestrować** łącze, a następnie zarejestrować użytkownik. Postępuj zgodnie z instrukcjami, aby uruchomić migracje platformy Entity Framework Core. W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu. Po przesłaniu rejestracji, użytkownik jest zalogowany do aplikacji. W dalszej części samouczka kod jest aktualizowana, dzięki czemu nowi użytkownicy nie mogą zalogować, dopóki nie został zweryfikowany poczty e-mail.

## <a name="view-the-identity-database"></a>Widok bazy danych tożsamości

Zobacz [Praca z SQLite w projektach programu ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) instrukcje dotyczące sposobu wyświetlania bazy danych SQLite.

Dla programu Visual Studio:

* Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server** (SSOX).
* Przejdź do **(localdb) MSSQLLocalDB (SQL Server 13)**. Kliknij prawym przyciskiem myszy **dbo. AspNetUsers** > **wyświetlania danych**:

![Menu kontekstowe tabeli AspNetUsers w Eksploratorze obiektów SQL Server](accconfirm/_static/ssox.png)

Należy pamiętać, tabeli `EmailConfirmed` pole jest `False`.

Można używać tego adresu e-mail ponownie w następnym kroku, gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem. Kliknij prawym przyciskiem myszy na wiersz i wybierz **Usuń**. Usuwanie aliasu adresu e-mail ułatwia w poniższych krokach.

---

## <a name="require-https"></a>Wymaganie protokołu HTTPS

Zobacz [wymagania protokołu HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Wymagaj potwierdzenie adresu e-mail

Jest najlepszym rozwiązaniem, aby potwierdzić adres e-mail rejestrowanie nowego użytkownika. Wiadomość e-mail potwierdzenia pomaga sprawdzić, mogą one nie personifikuje kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby). Załóżmy, że trzeba było forum dyskusyjnego i chcesz uniemożliwić "yli@example.com"z rejestracją jako"nolivetto@contoso.com". Bez potwierdzenia e-mail "nolivetto@contoso.com" może otrzymywać niechciane wiadomości e-mail z aplikacji. Załóżmy, że użytkownik przypadkowo zarejestrowana jako "ylo@example.com" i nie zostało już słowa "yli". W takich sytuacjach przydałaby się mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail. Potwierdzenie adresu e-mail zawiera tylko ograniczoną ochronę, od botów. Potwierdzenie adresu e-mail nie zapewniają ochronę przed złośliwymi użytkownikami z wieloma kontami e-mail.

Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do witryny sieci web, zanim potwierdzone pocztą e-mail.

Aktualizacja `ConfigureServices` będą musieli potwierdzony adres e-mail:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` zarejestrowani użytkownicy uniemożliwia logowanie do momentu ich adres e-mail został potwierdzony.

### <a name="configure-email-provider"></a>Konfigurowanie dostawcy poczty e-mail

W tym samouczku SendGrid umożliwia wysyłanie wiadomości e-mail. Potrzebujesz konta SendGrid i klucz do wysyłania wiadomości e-mail. Można użyć innych dostawców poczty e-mail. Platforma ASP.NET Core 2.x obejmuje `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji. Zaleca się, że używasz usługi SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail. SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.

[Wzorzec opcje](xref:fundamentals/configuration/options) umożliwia dostęp do ustawień konta i klucza użytkownika. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

Utwórz klasę, można pobrać klucza zabezpieczanie poczty e-mail. W tym przykładzie `AuthMessageSenderOptions` klasa jest tworzona w *Services/AuthMessageSenderOptions.cs* pliku:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets). Na przykład:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
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

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Konfigurowanie uruchamiania, aby użyć AuthMessageSenderOptions

Dodaj `AuthMessageSenderOptions` do kontenera usługi na końcu `ConfigureServices` method in Class metoda *Startup.cs* pliku:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurowanie klasy AuthMessageSender

W tym samouczku przedstawiono sposób dodawania powiadomienia e-mail za pośrednictwem [SendGrid](https://sendgrid.com/), ale możesz wysłać wiadomość e-mail przy użyciu SMTP i innych mechanizmów.

Zainstaluj `SendGrid` pakietu NuGet:

* W wierszu polecenia:

    `dotnet add package SendGrid`

* W konsoli Menedżera pakietów wprowadź następujące polecenie:

  `Install-Package SendGrid`

Zobacz [bezpłatnie Rozpocznij pracę za pomocą usługi SendGrid](https://sendgrid.com/free/) zarejestrować o utworzenie bezpłatnego konta SendGrid.

#### <a name="configure-sendgrid"></a>Konfigurowanie usługi SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Aby skonfigurować usługi SendGrid, Dodaj kod, które są podobne do następującego *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Dodaj kod w *Services/MessageServices.cs* podobne do następujących czynności, aby skonfigurować usługi SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Włącz odzyskiwanie potwierdzenia i hasło konta

Szablon zawiera kod odzyskiwania potwierdzenia i hasło konta. Znajdź `OnPostAsync` method in Class metoda *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Uniemożliwić nowym użytkownikom zalogowanie się automatycznie, zakomentowując następujący wiersz:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Za pomocą zmieniony wiersz wyróżniony pokazano całą metodę:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Aby włączyć potwierdzenie konta, usuń znaczniki komentarza z następującego kodu:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Uwaga:** kodu uniemożliwia nowo zarejestrowanego użytkownika z logowaniem się automatycznie, zakomentowując następujący wiersz:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Włącz odzyskiwanie hasła przez kod w uncommenting `ForgotPassword` akcji *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Usuń znaczniki komentarza element formularza w *Views/Account/ForgotPassword.cshtml*. Możesz chcieć usunąć `<p> For more information on how to enable reset password ... </p>` element, który zawiera łącze do tego artykułu.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Zostanie wyświetlona strona zarządzania, z **profilu** wybraną kartą. **E-mail** przedstawia pole wyboru, wskazujący adres e-mail został potwierdzony.

![Zarządzanie stroną](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Jest to opisane w dalszej części tego samouczka.
![Strona Zarządzanie](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Resetowanie hasła testu

* Jeśli zalogowano Cię, wybierz opcję **wylogowania**.
* Wybierz **Zaloguj** łącze, a następnie wybierz pozycję **nie pamiętasz hasła?** łącza.
* Wprowadź adres e-mail, którego użyłeś do zarejestrować konto.
* Wiadomość e-mail z linkiem do zresetowania hasła są wysyłane. Sprawdź pocztę e-mail, a następnie kliknij link, aby zresetować hasło. Po pomyślnie zresetowano hasło można rejestrować się za pomocą poczty e-mail i nowe hasło.

<a name="debug"></a>

### <a name="debug-email"></a>Debugowanie poczty e-mail

Jeśli nie można rozpocząć pracę poczty e-mail:

* Tworzenie [aplikacji konsoli, aby wysłać wiadomość e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
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
