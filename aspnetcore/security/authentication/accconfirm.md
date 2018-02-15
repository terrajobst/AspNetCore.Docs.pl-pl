---
title: "Potwierdzenie konta i hasła odzyskiwania w platformy ASP.NET Core"
author: rick-anderson
description: "Informacje o sposobie tworzenia aplikacji platformy ASP.NET Core z funkcją resetowania hasła i potwierdzania poczty e-mail."
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: e8f73d58bdf626910b2101ef310385f588315e26
ms.sourcegitcommit: 725cb18ad23013e15d3dbb527958481dee79f9f8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)

W tym samouczku przedstawiono sposób tworzenia aplikacji platformy ASP.NET Core z funkcją resetowania hasła i potwierdzania poczty e-mail. W tym samouczku jest **nie** początku tematu. Należy zapoznać się z:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Uwierzytelnianie](xref:security/authentication/index)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Zobacz [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC 1.1 i 2.x.

## <a name="prerequisites"></a>Wymagania wstępne

[Oprogramowanie .NET core 2.1.4 SDK](https://www.microsoft.com/net/core) lub nowszym.

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Utwórz nowy projekt platformy ASP.NET Core z poziomu interfejsu wiersza polecenia platformy .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* `--auth Individual` Określa szablon projektu indywidualnych kont użytkowników.
* W systemie Windows, należy dodać `-uld` opcji. Określa ona, że LocalDB powinna być używana zamiast funkcji SQLite.
* Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli używasz interfejsu wiersza polecenia lub SQLite, uruchom następujące polecenie w oknie poleceń:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Określa szablon projektu indywidualnych kont użytkowników.
* W systemie Windows, należy dodać `-uld` opcji. Określa ona, że LocalDB powinna być używana zamiast funkcji SQLite.
* Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.

---

Można również utworzyć nowy projekt platformy ASP.NET Core z programem Visual Studio:

* W programie Visual Studio Utwórz nową **aplikacji sieci Web** projektu.
* Wybierz **platformy ASP.NET Core 2.0**. **Oprogramowanie .NET core** jest zaznaczona na poniższej ilustracji, ale można wybrać **.NET Framework**.
* Wybierz **Zmień uwierzytelnianie** ustawiono **indywidualnych kont użytkowników**.
* Zachowaj ustawienie domyślne **kont magazynu użytkowników w aplikacji**.

![Okno dialogowe nowego projektu przedstawiający "Indywidualne konta użytkowników radio" wybrane](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Testowanie nowej rejestracji użytkownika

Uruchom aplikację, wybierz **zarejestrować** łącza, a następnie zarejestrować użytkownika. Postępuj zgodnie z instrukcjami, aby uruchomić program Entity Framework Core migracji. W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu. Po przesłaniu rejestracji, są rejestrowane w aplikacji. Później w samouczku kod jest aktualizowana, aby nowi użytkownicy nie może logować się do momentu sprawdzania poprawności poczty e-mail.

## <a name="view-the-identity-database"></a>Widok bazy danych tożsamości

Zobacz [pracy z bazy danych SQLite w projekcie platformy ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) instrukcje dotyczące sposobu wyświetlania bazy danych SQLite.

Dla programu Visual Studio:

* Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server** (SSOX).
* Przejdź do **(localdb) MSSQLLocalDB (SQL Server 13)**. Kliknij prawym przyciskiem myszy **dbo. AspNetUsers** > **wyświetlania danych**:

![Menu kontekstowe tabeli AspNetUsers w Eksploratorze obiektów SQL Server](accconfirm/_static/ssox.png)

Należy zwrócić uwagę tabeli `EmailConfirmed` pole jest `False`.

Można użyć tej wiadomości e-mail ponownie w następnym kroku gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem. Kliknij prawym przyciskiem myszy na wiersz i wybierz **usunąć**. Usuwanie aliasów e-mail ułatwia w kolejnych krokach.

---

## <a name="require-https"></a>Wymagać protokołu HTTPS

Zobacz [wymagać protokołu HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Wymagaj wiadomości e-mail z potwierdzeniem

Jest najlepszym rozwiązaniem w celu potwierdzenia adresu e-mail nowej rejestracji użytkownika. Wiadomości e-mail potwierdzenie pomaga sprawdzić ich jest nie przeprowadza personifikacji ktoś (to znaczy one nie został zarejestrowany przy do kogoś innego adresu e-mail). Załóżmy, że masz forum dyskusyjne i chcesz zapobiec "yli@example.com"z rejestracją jako"nolivetto@contoso.com." Bez wiadomości e-mail z potwierdzeniem "nolivetto@contoso.com" może otrzymywać niechcianych wiadomości e-mail z aplikacji. Załóżmy, że użytkownik przypadkowo zarejestrowany jako "ylo@example.com", a nie wystąpieniem słowa "yli". Nie można używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail. Wiadomości e-mail z potwierdzeniem chroni tylko ograniczone z robotów. Wiadomości e-mail z potwierdzeniem nie zapewnia ochrony przed złośliwymi użytkownikami z wielu kont e-mail.

Zazwyczaj chcesz uniemożliwić użytkownikom nowe przesyłanie danych do witryny sieci web, zanim potwierdzony adres e-mail.

Aktualizacja `ConfigureServices` aby wymagać potwierdzenia poczty e-mail:

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` zarejestrowani użytkownicy uniemożliwia logowanie do momentu swój adres e-mail został potwierdzony.

### <a name="configure-email-provider"></a>Skonfiguruj dostawcę poczty e-mail

W tym samouczku SendGrid służy do wysyłania wiadomości e-mail. Konieczne jest włączenie konta i klucz do wysyłania wiadomości e-mail. Można użyć innych dostawców poczty e-mail. Platformy ASP.NET Core zawiera 2.x `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji. Firma Microsoft zaleca się, że używasz SendGrid lub inna usługa poczty e-mail do wysyłania wiadomości e-mail. SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.

[Wzorzec opcje](xref:fundamentals/configuration/options) umożliwia dostęp do konta i klucz Ustawienia użytkownika. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

Utwórz klasę, aby pobrać klucz zabezpieczanie poczty e-mail. Dla tego przykładu `AuthMessageSenderOptions` klasy jest tworzony w *Services/AuthMessageSenderOptions.cs* pliku:

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets). Na przykład:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

W systemie Windows, klucz tajny Manager przechowuje par kluczy i wartości w *secrets.json* w pliku `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` katalogu.

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

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Konfigurowanie klasy AuthMessageSender

W tym samouczku przedstawiono sposób dodawania powiadomień pocztą e-mail za pomocą [SendGrid](https://sendgrid.com/), ale mogą wysyłać poczty e-mail przy użyciu SMTP i innych mechanizmów.

Zainstaluj `SendGrid` pakietu NuGet:

* W wierszu polecenia:

    `dotnet add package SendGrid`

* W konsoli Menedżera pakietów wprowadź następujące polecenie:

 `Install-Package SendGrid`

Zobacz [zacznij pracę bezpłatnie sendgrid](https://sendgrid.com/free/) zarejestrować bezpłatne konto SendGrid.

#### <a name="configure-sendgrid"></a>Skonfiguruj SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aby skonfigurować SendGrid, Dodaj kod podobny do następującego *Services/EmailSender.cs*:

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Dodaj kod w *Services/MessageServices.cs* podobne do następujących czynności, aby skonfigurować SendGrid:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Włącz odzyskiwanie potwierdzenie i hasło konta

Szablon ma kod odzyskiwania potwierdzenie i hasło konta. Znajdź `OnPostAsync` metody w *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Uniemożliwić użytkownikom nowo zarejestrowanych automatycznie zalogowania się przez komentowania się następujący wiersz:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Metody ukończenia jest wyświetlany z wierszem zmienione wyróżnione:

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aby włączyć potwierdzenie konta, usuń znaczniki komentarza z następującego kodu:

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Uwaga:** kod uniemożliwia nowo zarejestrowanym użytkownikiem automatycznie zalogowania się przez komentowania się następujący wiersz:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Włącz odzyskiwanie haseł przez usunięcie komentarza kod w `ForgotPassword` akcji *Controllers/AccountController.cs*:

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Usuń znaczniki komentarza element form w *Views/Account/ForgotPassword.cshtml*. Możesz usunąć `<p> For more information on how to enable reset password ... </p>` element, który zawiera link do tego artykułu.

[!code-cshtml[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

Jest to wymienione w dalszej części tego samouczka.
![Strona Zarządzanie](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Resetowanie hasła testu

* Jeśli użytkownik jest zalogowany, wybierz **wylogowania**.
* Wybierz **Zaloguj** łącza, a następnie wybierz **nie pamiętasz hasła?** łącza.
* Wprowadź adres e-mail używanego do rejestrowania konta.
* Zostanie wysłana wiadomość e-mail zawierającą łącze do resetowania hasła. Sprawdź pocztę i kliknij łącze, aby zresetować hasło. Po pomyślnie zresetowano hasło możesz zalogować się przy użyciu Twój adres e-mail i nowe hasło.

<a name="debug"></a>

### <a name="debug-email"></a>Debugowanie poczty e-mail

Jeśli nie można rozpocząć pracę poczty e-mail:

* Utwórz [aplikacji konsoli do wysyłania wiadomości e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.
* Sprawdź folder wiadomości-śmieci.
* Spróbuj inny alias e-mail przez dostawcę inny adres e-mail (Microsoft, Yahoo, Gmail itp.)
* Spróbuj wysłać do kont inny adres e-mail.

**Ze względów bezpieczeństwa** jest **nie** Użyj hasła produkcji testowych i programistycznych. Jeśli publikowanie aplikacji na platformie Azure, możesz ustawić kluczy tajnych SendGrid zgodnie z ustawieniami aplikacji w portalu Azure Web App. System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.

## <a name="combine-social-and-local-login-accounts"></a>Łączenie kont społecznościowych i lokalne logowanie

Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania. Zobacz [Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych](social/index.md).

Konta lokalne i społecznościowych można łączyć, klikając link do wiadomości e-mail. W następującej kolejności "RickAndMSFT@gmail.com" najpierw zostanie utworzona jako lokalne logowanie; jednak można najpierw utwórz konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

Polecenie **Zarządzaj** łącza. Uwaga zewnętrznych 0 (społecznościowych logowania) skojarzone z tym kontem.

![Zarządzanie widoku](accconfirm/_static/manage.png)

Kliknij łącze do innej usługi logowania i akceptowania żądań aplikacji. Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznego:

![Zarządzanie wyświetlania serwisu Facebook widoku logowań zewnętrznych](accconfirm/_static/fb.png)

Dwa konta zostały połączone. Będą mogli logować się na każdym koncie. Możesz użytkowników, aby dodać konta lokalnego, w przypadku ich społecznościowych logowania uwierzytelniania usługa nie działa lub najprawdopodobniej one utraty dostępu do swojego konta społecznościowych.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Włącz potwierdzenie konta po lokacja ma użytkowników

Włączanie potwierdzenia konta w witrynie użytkownikom do zablokowania wszystkich istniejących użytkowników. Istniejący użytkownicy są zablokowane, ponieważ nie są potwierdzone ich kont. Aby obejść Kończenie blokady użytkownika, użyj jednej z następujących metod:

* Aktualizacja bazy danych, aby oznaczyć wszystkich istniejących użytkowników, jak zostało potwierdzone
* Potwierdzanie istniejących użytkowników. Na przykład partii — wysyłanie wiadomości e-mail z potwierdzeniem łącza.
