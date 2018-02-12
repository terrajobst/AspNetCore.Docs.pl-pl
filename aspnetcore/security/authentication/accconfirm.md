---
title: "Potwierdzenie konta i hasła odzyskiwania w platformy ASP.NET Core"
author: rick-anderson
description: "Przedstawia sposób tworzenia aplikacji platformy ASP.NET Core z funkcją resetowania hasła i potwierdzania poczty e-mail."
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 14c7fdfc1ed8b87aac8ca937298c7da6373bf06d
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette) 

W tym samouczku przedstawiono sposób tworzenia aplikacji platformy ASP.NET Core z funkcją resetowania hasła i potwierdzania poczty e-mail.

## <a name="create-a-new-aspnet-core-project"></a>Utwórz nowy projekt platformy ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Ten krok dotyczy programu Visual Studio w systemie Windows. Zobacz następną sekcję, aby uzyskać instrukcje interfejsu wiersza polecenia.

Samouczek wymaga programu Visual Studio 2017 Preview 2 lub nowszym.

* W programie Visual Studio Utwórz nowy projekt aplikacji sieci Web.
* Wybierz **platformy ASP.NET Core 2.0**. Poniższy obraz Pokaż **.NET Core** zaznaczone, ale można wybrać **.NET Framework**.
* Wybierz **Zmień uwierzytelnianie** ustawiono **indywidualnych kont użytkowników**.
* Zachowaj ustawienie domyślne **kont magazynu użytkowników w aplikacji**.

![Okno dialogowe nowego projektu przedstawiający "Indywidualne konta użytkowników radio" wybrane](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Samouczek wymaga programu Visual Studio 2017 lub nowszej.

* W programie Visual Studio Utwórz nowy projekt aplikacji sieci Web.
* Wybierz **Zmień uwierzytelnianie** ustawiono **indywidualnych kont użytkowników**.

![Okno dialogowe nowego projektu przedstawiający "Indywidualne konta użytkowników radio" wybrane](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>Tworzenie projektu interfejsu wiersza polecenia platformy .NET core macOS i Linux

Jeśli używasz interfejsu wiersza polecenia lub SQLite, uruchom następujące polecenie w oknie poleceń:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`Określa szablon indywidualnych kont użytkowników.
* W systemie Windows, należy dodać `-uld` opcji. `-uld` Opcja tworzy ciąg połączenia bazy danych LocalDB, a nie bazy danych SQLite.
* Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.

## <a name="test-new-user-registration"></a>Testowanie nowej rejestracji użytkownika

Uruchom aplikację, wybierz **zarejestrować** łącza, a następnie zarejestrować użytkownika. Postępuj zgodnie z instrukcjami, aby uruchomić program Entity Framework Core migracji. W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu. Po przesłaniu rejestracji są rejestrowane w aplikacji. Później w samouczku zmienimy to aby nowi użytkownicy nie może logować się do momentu sprawdzania poprawności poczty e-mail.

## <a name="view-the-identity-database"></a>Widok bazy danych tożsamości

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server** (SSOX). 
* Przejdź do **(localdb) MSSQLLocalDB (SQL Server 13)**. Kliknij prawym przyciskiem myszy **dbo. AspNetUsers** > **wyświetlania danych**:

![Menu kontekstowe tabeli AspNetUsers w Eksploratorze obiektów SQL Server](accconfirm/_static/ssox.png)

Uwaga `EmailConfirmed` pole jest `False`.

Można użyć tej wiadomości e-mail ponownie w następnym kroku gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem. Kliknij prawym przyciskiem myszy na wiersz i wybierz **usunąć**. Usuwanie wiadomości e-mail alias teraz ułatwi w kolejnych krokach.

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

Zobacz [pracy z bazy danych SQLite w projekcie platformy ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) instrukcje dotyczące sposobu wyświetlania bazy danych SQLite. 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>Wymagaj protokołu SSL i skonfigurować usługi IIS Express do używania protokołu SSL

Zobacz [wymuszania SSL](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Wymagaj wiadomości e-mail z potwierdzeniem

Jest najlepszym rozwiązaniem, aby potwierdzić nowej rejestracji użytkownika, aby sprawdzić ich jest nie przeprowadza personifikacji ktoś inny adres e-mail (to znaczy one nie został zarejestrowany przy do kogoś innego adresu e-mail). Załóżmy, że masz forum dyskusyjne i chcesz zapobiec "yli@example.com"z rejestracją jako"nolivetto@contoso.com." Bez wiadomości e-mail z potwierdzeniem "nolivetto@contoso.com" można uzyskać niechcianych wiadomości e-mail z aplikacji. Załóżmy, że użytkownik przypadkowo zarejestrowany jako "ylo@example.com", a nie wystąpieniem słowa z "yli", nie można używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail. Wiadomości e-mail z potwierdzeniem chroni tylko ograniczone z robotów i nie zapewnia ochrony z określone nadawcy, którzy mają wiele aliasów e-mail pracy służące do rejestrowania.

Zazwyczaj chcesz uniemożliwić użytkownikom nowe przesyłanie danych do witryny sieci web, zanim potwierdzony adres e-mail. 

Aktualizacja `ConfigureServices` aby wymagać potwierdzenia poczty e-mail:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
Poprzedni wiersz zapobiega zarejestrowanych użytkowników są rejestrowane, do momentu swój adres e-mail został potwierdzony. Jednak tego wiersza nie uniemożliwić nowych użytkowników po zarejestrowaniu się. Domyślny kod loguje użytkownika, po zarejestrowaniu się. Po zalogowaniu będą mogli się zalogować ponownie do czasu ich zarejestrować. Później w samouczku zmienimy użytkownika kodu, więc nowo zarejestrowane są **nie** zalogowany.

### <a name="configure-email-provider"></a>Skonfiguruj dostawcę poczty e-mail

W tym samouczku SendGrid służy do wysyłania wiadomości e-mail. Konieczne jest włączenie konta i klucz do wysyłania wiadomości e-mail. Można użyć innych dostawców poczty e-mail. Platformy ASP.NET Core zawiera 2.x `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji. Firma Microsoft zaleca się, że używasz SendGrid lub inna usługa poczty e-mail do wysyłania wiadomości e-mail. SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.

[Wzorzec opcje](xref:fundamentals/configuration/options) umożliwia dostęp do konta i klucz Ustawienia użytkownika. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

Utwórz klasę, aby pobrać klucz zabezpieczanie poczty e-mail. Dla tego przykładu `AuthMessageSenderOptions` klasy jest tworzony w *Services/AuthMessageSenderOptions.cs* pliku.

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](../app-secrets.md). Na przykład:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

W systemie Windows, klucz tajny Manager przechowuje z par kluczy i wartości w *secrets.json* pliku w katalogu %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.

Zawartość *secrets.json* pliku nie są szyfrowane. *Secrets.json* plików są wyświetlane poniżej ( `SendGridKey` wartości została usunięta.)

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Konfigurowanie uruchamiania do użycia AuthMessageSenderOptions

Dodaj `AuthMessageSenderOptions` w kontenerze usługi na końcu `ConfigureServices` metody w *Startup.cs* pliku:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurowanie klasy AuthMessageSender

W tym samouczku przedstawiono sposób dodawania powiadomień pocztą e-mail za pomocą [SendGrid](https://sendgrid.com/), ale mogą wysyłać poczty e-mail przy użyciu SMTP i innych mechanizmów.

* Zainstaluj `SendGrid` pakietu NuGet. W konsoli Menedżera pakietów, wprowadź następujące polecenie:

  `Install-Package SendGrid`

* Zobacz [zacznij pracę bezpłatnie sendgrid](https://sendgrid.com/free/) zarejestrować bezpłatne konto SendGrid.

#### <a name="configure-sendgrid"></a>Skonfiguruj SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* Dodaj kod w *Services/EmailSender.cs* podobne do następujących czynności, aby skonfigurować SendGrid:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Dodaj kod w *Services/MessageServices.cs* podobne do następujących czynności, aby skonfigurować SendGrid:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Włącz odzyskiwanie potwierdzenie i hasło konta

Szablon ma kod odzyskiwania potwierdzenie i hasło konta. Znajdź `[HttpPost] Register` metody w *AccountController.cs* pliku.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Uniemożliwić użytkownikom nowo zarejestrowanych automatycznie zalogowania się przez komentowania się następujący wiersz:

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

Metody ukończenia jest wyświetlany z wierszem zmienione wyróżnione:

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

Uwaga: Poprzedni kod zakończy się niepowodzeniem w przypadku zastosowania `IEmailSender` i wysłać wiadomość e-mail w formacie zwykłego tekstu. Zobacz [ten problem](https://github.com/aspnet/Home/issues/2152) uzyskać więcej informacji i obejście tego problemu.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Usuń komentarz kodu w celu włączenia potwierdzenie konta.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

Uwaga: Firma Microsoft są także uniemożliwia nowo zarejestrowany użytkownik automatycznie zalogowania się przez komentowania się następujący wiersz:

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Włącz odzyskiwanie haseł przez usunięcie komentarza kod w `ForgotPassword` akcji w *Controllers/AccountController.cs* pliku.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Usuń znaczniki komentarza element form w *Views/Account/ForgotPassword.cshtml*. Możesz usunąć `<p> For more information on how to enable reset password ... </p>` element, który zawiera łącze do tego artykułu.

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Rejestracji, potwierdź adres e-mail i zresetuj hasło

Uruchamianie aplikacji sieci web i testów potwierdzenie konta i hasła odzyskiwania przepływu.

* Uruchom aplikację i zarejestrować nowego użytkownika

 ![Widok zarejestrować konto aplikacji sieci Web](accconfirm/_static/loginaccconfirm1.png)

* Sprawdź pocztę łącza potwierdzenie konta. Zobacz [debugowania e-mail](#debug) otrzymasz wiadomość e-mail.
* Kliknij łącze, aby potwierdzić swój adres e-mail.
* Zaloguj się za pomocą Twój adres e-mail i hasło.
* Wyloguj.

### <a name="view-the-manage-page"></a>Wyświetl stronę Zarządzanie

Wybierz nazwę użytkownika w przeglądarce: ![okna przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)

Konieczne może być Rozwiń pasek nawigacyjny, aby wyświetlić nazwy użytkownika.

![pasek nawigacyjny](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Na stronie Zarządzanie zostanie wyświetlony z **profilu** kartę zaznaczone. **E-mail** zawiera pole wyboru, wskazując wiadomości e-mail został potwierdzony. 

![Strona Zarządzanie](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ta strona będzie opisano później w samouczku.
![Strona Zarządzanie](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Resetowanie hasła testu

* Jeśli użytkownik jest zalogowany, wybierz **wylogowania**.  
* Wybierz **Zaloguj** łącza, a następnie wybierz **nie pamiętasz hasła?** łącza.
* Wprowadź adres e-mail używanego do rejestrowania konta.
* Zostanie wysłana wiadomość e-mail zawierającą łącze do resetowania hasła. Sprawdź pocztę i kliknij łącze, aby zresetować hasło.  Po pomyślnie zresetowano hasło może się zalogować z poczty e-mail i nowe hasło.

<a name="debug"></a>

### <a name="debug-email"></a>Debugowanie poczty e-mail

Jeśli nie można rozpocząć pracę poczty e-mail:

* Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.
* Sprawdź folder wiadomości-śmieci.
* Spróbuj inny alias e-mail przez dostawcę inny adres e-mail (Microsoft, Yahoo, Gmail itp.)
* Utwórz [aplikacji konsoli do wysyłania wiadomości e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Spróbuj wysłać do kont inny adres e-mail.

**Uwaga:** ma nie używać hasła produkcji testowych i programistycznych ze względów bezpieczeństwa. Jeśli publikowanie aplikacji na platformie Azure, możesz ustawić kluczy tajnych SendGrid zgodnie z ustawieniami aplikacji w portalu Azure Web App. System konfiguracji jest skonfigurowana do odczytu klucze zmiennych środowiskowych.

## <a name="prevent-login-at-registration"></a>Zapobiegaj logowania podczas rejestracji

Przy użyciu bieżącego szablonów, gdy użytkownik zamyka formularz rejestracji są rejestrowane w (uwierzytelniony). Zazwyczaj mają Potwierdź swój adres e-mail przed ich zalogowaniem się. W poniższej sekcji, firma Microsoft będzie zmodyfikuj kod do wymagają nowych użytkowników miał potwierdzony adres e-mail jest zalogowany. Aktualizacja `[HttpPost] Login` akcji w *AccountController.cs* pliku z następującymi zmianami zaznaczony.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**Uwaga:** ma nie używać hasła produkcji testowych i programistycznych ze względów bezpieczeństwa. Jeśli publikowanie aplikacji na platformie Azure, możesz ustawić kluczy tajnych SendGrid zgodnie z ustawieniami aplikacji w portalu Azure Web App. System konfiguracji jest skonfigurowana do odczytu klucze zmiennych środowiskowych.


## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont społecznościowych i lokalne logowanie

Uwaga: Ta sekcja dotyczy tylko z platformy ASP.NET Core 1.x. Dla platformy ASP.NET Core 2.x, zobacz [to](https://github.com/aspnet/Docs/issues/3753) problem.

Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania. Zobacz [Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych](social/index.md).

Konta lokalne i społecznościowych można łączyć, klikając link do wiadomości e-mail. W następującej kolejności "RickAndMSFT@gmail.com" najpierw zostanie utworzona jako lokalne logowanie; jednak można najpierw utwórz konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

Polecenie **Zarządzaj** łącza. Uwaga zewnętrznych 0 (społecznościowych logowania) skojarzone z tym kontem.

![Zarządzanie widoku](accconfirm/_static/manage.png)

Kliknij łącze do innej usługi logowania i akceptowania żądań aplikacji. Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznego:

![Zarządzanie wyświetlania serwisu Facebook widoku logowań zewnętrznych](accconfirm/_static/fb.png)

Dwa konta zostały połączone. Będzie można logować się na każdym koncie. Możesz użytkowników, aby dodać konta lokalnego, w przypadku ich społecznościowych dziennika w usłudze uwierzytelniania jest wyłączony lub najprawdopodobniej one utraty dostępu do swojego konta społecznościowych.
