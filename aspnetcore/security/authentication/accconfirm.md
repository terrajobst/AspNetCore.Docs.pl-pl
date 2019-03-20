---
title: Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.
ms.author: riande
ms.date: 3/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 3bfc2ce46cfbc2ee308940f9e04eb2ffeec09073
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265495"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="c19d3-103">Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c19d3-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="c19d3-104">Zobacz [plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) dla platformy ASP.NET Core 1.1 i wersja 2.1.</span><span class="sxs-lookup"><span data-stu-id="c19d3-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c19d3-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c19d3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="c19d3-106">W tym samouczku przedstawiono sposób kompilowania aplikacji platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.</span><span class="sxs-lookup"><span data-stu-id="c19d3-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="c19d3-107">Niniejszy samouczek jest **nie** początku tematu.</span><span class="sxs-lookup"><span data-stu-id="c19d3-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="c19d3-108">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="c19d3-108">You should be familiar with:</span></span>

* [<span data-ttu-id="c19d3-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c19d3-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="c19d3-110">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="c19d3-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="c19d3-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c19d3-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="c19d3-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c19d3-112">Prerequisites</span></span>

[<span data-ttu-id="c19d3-113">Zestaw SDK programu .NET core 2.2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="c19d3-113">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="c19d3-114">Tworzenie aplikacji sieci web i tworzenia szkieletu tożsamości</span><span class="sxs-lookup"><span data-stu-id="c19d3-114">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="c19d3-115">Uruchom następujące polecenia, aby utworzyć aplikację sieci web przy użyciu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c19d3-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="c19d3-116">Rejestrowanie nowego użytkownika testowego</span><span class="sxs-lookup"><span data-stu-id="c19d3-116">Test new user registration</span></span>

<span data-ttu-id="c19d3-117">Uruchom aplikację, wybierz **zarejestrować** łącze, a następnie zarejestrować użytkownik.</span><span class="sxs-lookup"><span data-stu-id="c19d3-117">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="c19d3-118">W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c19d3-118">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="c19d3-119">Po przesłaniu rejestracji, użytkownik jest zalogowany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c19d3-119">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="c19d3-120">W dalszej części samouczka kod jest aktualizowana, aby nowi użytkownicy nie może się zalogować, dopóki nie zostanie zweryfikowana poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-120">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="c19d3-121">Należy pamiętać, tabeli `EmailConfirmed` pole jest `False`.</span><span class="sxs-lookup"><span data-stu-id="c19d3-121">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="c19d3-122">Można używać tego adresu e-mail ponownie w następnym kroku, gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="c19d3-122">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="c19d3-123">Kliknij prawym przyciskiem myszy na wiersz i wybierz **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="c19d3-123">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="c19d3-124">Usuwanie aliasu adresu e-mail ułatwia w poniższych krokach.</span><span class="sxs-lookup"><span data-stu-id="c19d3-124">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="c19d3-125">Wymagaj potwierdzenie adresu e-mail</span><span class="sxs-lookup"><span data-stu-id="c19d3-125">Require email confirmation</span></span>

<span data-ttu-id="c19d3-126">Jest najlepszym rozwiązaniem, aby potwierdzić adres e-mail rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c19d3-126">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="c19d3-127">Wiadomość e-mail potwierdzenia pomaga sprawdzić, mogą one nie personifikuje kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby).</span><span class="sxs-lookup"><span data-stu-id="c19d3-127">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="c19d3-128">Załóżmy, że trzeba było forum dyskusyjnego i chcesz uniemożliwić "yli@example.com"z rejestracją jako"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="c19d3-128">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="c19d3-129">Bez potwierdzenia e-mail "nolivetto@contoso.com" może otrzymywać niechciane wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c19d3-129">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="c19d3-130">Załóżmy, że użytkownik przypadkowo zarejestrowana jako "ylo@example.com" i nie zostało już słowa "yli".</span><span class="sxs-lookup"><span data-stu-id="c19d3-130">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="c19d3-131">W takich sytuacjach przydałaby się mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-131">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="c19d3-132">Potwierdzenie adresu e-mail zapewnia ograniczoną ochronę, od botów.</span><span class="sxs-lookup"><span data-stu-id="c19d3-132">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="c19d3-133">Potwierdzenie adresu e-mail nie zapewniają ochronę przed złośliwymi użytkownikami z wieloma kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-133">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="c19d3-134">Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do witryny sieci web, zanim potwierdzone pocztą e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-134">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="c19d3-135">Aktualizacja `Startup.ConfigureServices` będą musieli potwierdzony adres e-mail:</span><span class="sxs-lookup"><span data-stu-id="c19d3-135">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="c19d3-136">`config.SignIn.RequireConfirmedEmail = true;` zarejestrowani użytkownicy uniemożliwia logowanie do momentu ich adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="c19d3-136">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="c19d3-137">Konfigurowanie dostawcy poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="c19d3-137">Configure email provider</span></span>

<span data-ttu-id="c19d3-138">W tym samouczku [SendGrid](https://sendgrid.com) służy do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-138">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="c19d3-139">Potrzebujesz konta SendGrid i klucz do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-139">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="c19d3-140">Można użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-140">You can use other email providers.</span></span> <span data-ttu-id="c19d3-141">Platforma ASP.NET Core 2.x obejmuje `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c19d3-141">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="c19d3-142">Zaleca się, że używasz usługi SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-142">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="c19d3-143">SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="c19d3-143">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="c19d3-144">Utwórz klasę, można pobrać klucza zabezpieczanie poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-144">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="c19d3-145">W tym przykładzie należy utworzyć *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="c19d3-145">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="c19d3-146">Konfigurowanie wpisami tajnymi użytkowników usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="c19d3-146">Configure SendGrid user secrets</span></span>

<span data-ttu-id="c19d3-147">Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c19d3-147">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="c19d3-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c19d3-148">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="c19d3-149">Na Windows, klucza tajnego Manager przechowuje par kluczy i wartości w *secrets.json* w pliku `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="c19d3-149">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="c19d3-150">Zawartość *secrets.json* pliku nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="c19d3-150">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="c19d3-151">Ilustruje poniższy kod znaczników *secrets.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="c19d3-151">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="c19d3-152">`SendGridKey` Wartość została usunięta.</span><span class="sxs-lookup"><span data-stu-id="c19d3-152">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="c19d3-153">Aby uzyskać więcej informacji, zobacz [wzorzec opcje](xref:fundamentals/configuration/options) i [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c19d3-153">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="c19d3-154">Instalowanie usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="c19d3-154">Install SendGrid</span></span>

<span data-ttu-id="c19d3-155">W tym samouczku przedstawiono sposób dodawania powiadomienia e-mail za pośrednictwem [SendGrid](https://sendgrid.com/), ale możesz wysłać wiadomość e-mail przy użyciu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="c19d3-155">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="c19d3-156">Zainstaluj `SendGrid` pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="c19d3-156">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c19d3-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c19d3-157">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c19d3-158">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c19d3-158">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c19d3-159">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c19d3-159">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c19d3-160">Z poziomu konsoli wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c19d3-160">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="c19d3-161">Zobacz [bezpłatnie Rozpocznij pracę za pomocą usługi SendGrid](https://sendgrid.com/free/) zarejestrować o utworzenie bezpłatnego konta SendGrid.</span><span class="sxs-lookup"><span data-stu-id="c19d3-161">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="c19d3-162">Implementowanie IEmailSender</span><span class="sxs-lookup"><span data-stu-id="c19d3-162">Implement IEmailSender</span></span>

<span data-ttu-id="c19d3-163">Zaimplementowanie `IEmailSender`, Utwórz *Services/EmailSender.cs* kodem podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="c19d3-163">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="c19d3-164">Konfigurowanie uruchamiania do obsługi poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="c19d3-164">Configure startup to support email</span></span>

<span data-ttu-id="c19d3-165">Dodaj następujący kod do `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="c19d3-165">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="c19d3-166">Dodaj `EmailSender` jako przejściowe usługi.</span><span class="sxs-lookup"><span data-stu-id="c19d3-166">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="c19d3-167">Zarejestruj `AuthMessageSenderOptions` wystąpienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c19d3-167">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="c19d3-168">Włącz odzyskiwanie potwierdzenia i hasło konta</span><span class="sxs-lookup"><span data-stu-id="c19d3-168">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="c19d3-169">Szablon zawiera kod odzyskiwania potwierdzenia i hasło konta.</span><span class="sxs-lookup"><span data-stu-id="c19d3-169">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="c19d3-170">Znajdź `OnPostAsync` method in Class metoda *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="c19d3-170">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="c19d3-171">Uniemożliwić nowym użytkownikom Trwa automatyczne logowanie, zakomentowując następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="c19d3-171">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c19d3-172">Za pomocą zmieniony wiersz wyróżniony pokazano całą metodę:</span><span class="sxs-lookup"><span data-stu-id="c19d3-172">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="c19d3-173">Rejestrowania i resetowania hasła oraz Potwierdź adres e-mail</span><span class="sxs-lookup"><span data-stu-id="c19d3-173">Register, confirm email, and reset password</span></span>

<span data-ttu-id="c19d3-174">Uruchamianie aplikacji sieci web i przetestować potwierdzenie konta i hasła odzyskiwania przepływu.</span><span class="sxs-lookup"><span data-stu-id="c19d3-174">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="c19d3-175">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="c19d3-175">Run the app and register a new user</span></span>
* <span data-ttu-id="c19d3-176">Sprawdź pocztę e-mail, aby uzyskać link do potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="c19d3-176">Check your email for the account confirmation link.</span></span> <span data-ttu-id="c19d3-177">Zobacz [debugowania e-mail](#debug) otrzymasz wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-177">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="c19d3-178">Kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-178">Click the link to confirm your email.</span></span>
* <span data-ttu-id="c19d3-179">Zaloguj się przy użyciu poczty e-mail i hasło.</span><span class="sxs-lookup"><span data-stu-id="c19d3-179">Sign in with your email and password.</span></span>
* <span data-ttu-id="c19d3-180">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="c19d3-180">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="c19d3-181">Wyświetlanie strony zarządzania</span><span class="sxs-lookup"><span data-stu-id="c19d3-181">View the manage page</span></span>

<span data-ttu-id="c19d3-182">Wybierz swoją nazwę użytkownika w przeglądarce: ![okno przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="c19d3-182">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="c19d3-183">Zostanie wyświetlona strona zarządzania, z **profilu** wybraną kartą.</span><span class="sxs-lookup"><span data-stu-id="c19d3-183">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="c19d3-184">**E-mail** przedstawia pole wyboru, wskazujący adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="c19d3-184">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="c19d3-185">Resetowanie hasła testu</span><span class="sxs-lookup"><span data-stu-id="c19d3-185">Test password reset</span></span>

* <span data-ttu-id="c19d3-186">Jeśli zalogowano Cię, wybierz opcję **wylogowania**.</span><span class="sxs-lookup"><span data-stu-id="c19d3-186">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="c19d3-187">Wybierz **Zaloguj** łącze, a następnie wybierz pozycję **nie pamiętasz hasła?** łącza.</span><span class="sxs-lookup"><span data-stu-id="c19d3-187">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="c19d3-188">Wprowadź adres e-mail, którego użyłeś do zarejestrować konto.</span><span class="sxs-lookup"><span data-stu-id="c19d3-188">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="c19d3-189">Wiadomość e-mail z linkiem do zresetowania hasła są wysyłane.</span><span class="sxs-lookup"><span data-stu-id="c19d3-189">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="c19d3-190">Sprawdź pocztę e-mail, a następnie kliknij link, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="c19d3-190">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="c19d3-191">Po pomyślnie zresetowano hasło możesz zarejestrować się przy użyciu poczty e-mail i nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="c19d3-191">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="c19d3-192">Limit czasu wiadomości e-mail i działania zmiany</span><span class="sxs-lookup"><span data-stu-id="c19d3-192">Change email and activity timeout</span></span>

<span data-ttu-id="c19d3-193">Domyślny limit czasu braku aktywności to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="c19d3-193">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="c19d3-194">Poniższy kod ustawia limit czasu braku aktywności na 5 dni:</span><span class="sxs-lookup"><span data-stu-id="c19d3-194">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="c19d3-195">Zmień wszystkie lifespans tokenu ochrony danych</span><span class="sxs-lookup"><span data-stu-id="c19d3-195">Change all data protection token lifespans</span></span>

<span data-ttu-id="c19d3-196">Poniższy kod zmienia wszystkie dane ochrony tokenów limit czasu do 3 godzin:</span><span class="sxs-lookup"><span data-stu-id="c19d3-196">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="c19d3-197">Wbudowanej w tożsamości tokeny użytkowników (zobacz [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) mają [limitu czasu w ciągu jednego dnia](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="c19d3-197">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="c19d3-198">Zmień czas tokenu poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="c19d3-198">Change the email token lifespan</span></span>

<span data-ttu-id="c19d3-199">Domyślny token żywotność [tokeny użytkowników tożsamości](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) jest [jeden dzień](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="c19d3-199">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="c19d3-200">W tej sekcji pokazano, jak zmienić czas tokenu wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-200">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="c19d3-201">Dodaj niestandardową [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) i <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="c19d3-201">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="c19d3-202">Dodaj niestandardowego dostawcę do kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="c19d3-202">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="c19d3-203">Ponownie wyślij e-mail z potwierdzeniem</span><span class="sxs-lookup"><span data-stu-id="c19d3-203">Resend email confirmation</span></span>

<span data-ttu-id="c19d3-204">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="c19d3-204">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="c19d3-205">Debugowanie poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="c19d3-205">Debug email</span></span>

<span data-ttu-id="c19d3-206">Jeśli nie można rozpocząć pracę poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="c19d3-206">If you can't get email working:</span></span>

* <span data-ttu-id="c19d3-207">Ustaw punkt przerwania `EmailSender.Execute` Aby zweryfikować `SendGridClient.SendEmailAsync` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c19d3-207">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="c19d3-208">Tworzenie [aplikacji konsoli, aby wysłać wiadomość e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) przy użyciu kodu podobne do `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="c19d3-208">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="c19d3-209">Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.</span><span class="sxs-lookup"><span data-stu-id="c19d3-209">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="c19d3-210">Sprawdź folder wiadomości-śmieci.</span><span class="sxs-lookup"><span data-stu-id="c19d3-210">Check your spam folder.</span></span>
* <span data-ttu-id="c19d3-211">Spróbuj inny alias poczty e-mail na innego dostawcy poczty e-mail (Microsoft Yahoo, Gmail, itp.)</span><span class="sxs-lookup"><span data-stu-id="c19d3-211">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="c19d3-212">Spróbuj wysłać do różnymi kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-212">Try sending to different email accounts.</span></span>

<span data-ttu-id="c19d3-213">**Ze względów bezpieczeństwa** jest **nie** używania wpisów tajnych produkcyjnych, w projektowania i testowania.</span><span class="sxs-lookup"><span data-stu-id="c19d3-213">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="c19d3-214">Jeśli opublikujesz aplikację na platformie Azure, możesz ustawić wpisy tajne usługi SendGrid, jako ustawienia aplikacji w portalu usługi Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="c19d3-214">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="c19d3-215">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="c19d3-215">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c19d3-216">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="c19d3-216">Combine social and local login accounts</span></span>

<span data-ttu-id="c19d3-217">Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c19d3-217">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="c19d3-218">Zobacz [Facebook, Google i uwierzytelniania zewnętrznego dostawcy](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c19d3-218">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="c19d3-219">Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c19d3-219">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c19d3-220">W następującej kolejności "RickAndMSFT@gmail.com" najpierw jest tworzony jako lokalny identyfikator logowania; jednak możesz najpierw utworzyć konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="c19d3-220">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="c19d3-222">Kliknij pozycję **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="c19d3-222">Click on the **Manage** link.</span></span> <span data-ttu-id="c19d3-223">Należy pamiętać, zewnętrzne 0 (logowania społecznościowego) skojarzony z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="c19d3-223">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzanie widoku](accconfirm/_static/manage.png)

<span data-ttu-id="c19d3-225">Kliknij link do innej usługi, zaloguj się i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c19d3-225">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="c19d3-226">Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznych:</span><span class="sxs-lookup"><span data-stu-id="c19d3-226">In the following image, Facebook is the external authentication provider:</span></span>

![Zarządzanie wyświetlania listy Facebooku widoku logowań zewnętrznych](accconfirm/_static/fb.png)

<span data-ttu-id="c19d3-228">Te dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="c19d3-228">The two accounts have been combined.</span></span> <span data-ttu-id="c19d3-229">Jesteś w stanie zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="c19d3-229">You are able to sign in with either account.</span></span> <span data-ttu-id="c19d3-230">Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania uwierzytelniania usługa nie działa lub większe prawdopodobieństwo ich utraty dostępu do swojego konta w sieci społecznościowej.</span><span class="sxs-lookup"><span data-stu-id="c19d3-230">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="c19d3-231">Włącz potwierdzenie konta, po lokacji ma użytkowników</span><span class="sxs-lookup"><span data-stu-id="c19d3-231">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="c19d3-232">Włączanie potwierdzenie konta w witrynie użytkownikom blokuje istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c19d3-232">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="c19d3-233">Istniejący użytkownicy są zablokowane, ponieważ ich konta nie są potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="c19d3-233">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="c19d3-234">Aby obejść istniejące blokady użytkownika, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="c19d3-234">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="c19d3-235">Aktualizuj bazę danych, aby oznaczyć wszyscy istniejący użytkownicy, co zostało potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="c19d3-235">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="c19d3-236">Upewnij się, liczba istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c19d3-236">Confirm existing users.</span></span> <span data-ttu-id="c19d3-237">Na przykład usługi batch — wysyłanie wiadomości e-mail przy użyciu linki do potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="c19d3-237">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
