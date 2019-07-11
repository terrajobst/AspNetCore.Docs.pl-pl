---
title: Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 802ba446af04df6a35ac73187ad693b8ec80c654
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814840"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="65e12-103">Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65e12-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="65e12-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="65e12-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="65e12-105">W tym samouczku przedstawiono sposób kompilowania aplikacji platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.</span><span class="sxs-lookup"><span data-stu-id="65e12-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="65e12-106">Niniejszy samouczek jest **nie** początku tematu.</span><span class="sxs-lookup"><span data-stu-id="65e12-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="65e12-107">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="65e12-107">You should be familiar with:</span></span>

* [<span data-ttu-id="65e12-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65e12-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="65e12-109">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="65e12-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="65e12-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="65e12-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="65e12-111">Zobacz [plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="65e12-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="65e12-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="65e12-112">Prerequisites</span></span>

[<span data-ttu-id="65e12-113">Zestaw SDK programu .NET core 3.0 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="65e12-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="65e12-114">Tworzenie i testowanie aplikacji sieci web z uwierzytelnianiem</span><span class="sxs-lookup"><span data-stu-id="65e12-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="65e12-115">Uruchom następujące polecenia, aby utworzyć aplikację sieci web przy użyciu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="65e12-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="65e12-116">Uruchom aplikację, wybierz **zarejestrować** łącze, a następnie zarejestrować użytkownik.</span><span class="sxs-lookup"><span data-stu-id="65e12-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="65e12-117">Po zarejestrowaniu nastąpi przekierowanie do celu `/Identity/Account/RegisterConfirmation` strona, która zawiera link do symulowania potwierdzenie adresu e-mail:</span><span class="sxs-lookup"><span data-stu-id="65e12-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="65e12-118">Wybierz `Click here to confirm your account` łącza.</span><span class="sxs-lookup"><span data-stu-id="65e12-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="65e12-119">Wybierz **logowania** link i zaloguj się przy użyciu tych samych poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="65e12-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="65e12-120">Wybierz `Hello YourEmail@provider.com!` łącza, który przekierowuje do `/Identity/Account/Manage/PersonalData` strony.</span><span class="sxs-lookup"><span data-stu-id="65e12-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="65e12-121">Wybierz **danych osobowych** po lewej stronie, a następnie wybierz pozycję **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="65e12-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="65e12-122">Konfigurowanie dostawcy poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-122">Configure an email provider</span></span>

<span data-ttu-id="65e12-123">W tym samouczku [SendGrid](https://sendgrid.com) służy do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="65e12-124">Potrzebujesz konta SendGrid i klucz do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="65e12-125">Można użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-125">You can use other email providers.</span></span> <span data-ttu-id="65e12-126">Zaleca się, że używasz usługi SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="65e12-127">SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="65e12-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="65e12-128">Utwórz klasę, można pobrać klucza zabezpieczanie poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="65e12-129">W tym przykładzie należy utworzyć *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="65e12-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="65e12-130">Konfigurowanie wpisami tajnymi użytkowników usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="65e12-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="65e12-131">Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="65e12-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="65e12-132">Przykład:</span><span class="sxs-lookup"><span data-stu-id="65e12-132">For example:</span></span>

```console
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="65e12-133">Na Windows, klucza tajnego Manager przechowuje par kluczy i wartości w *secrets.json* w pliku `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="65e12-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="65e12-134">Zawartość *secrets.json* pliku nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="65e12-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="65e12-135">Ilustruje poniższy kod znaczników *secrets.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="65e12-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="65e12-136">`SendGridKey` Wartość została usunięta.</span><span class="sxs-lookup"><span data-stu-id="65e12-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="65e12-137">Aby uzyskać więcej informacji, zobacz [wzorzec opcje](xref:fundamentals/configuration/options) i [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="65e12-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="65e12-138">Instalowanie usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="65e12-138">Install SendGrid</span></span>

<span data-ttu-id="65e12-139">W tym samouczku przedstawiono sposób dodawania powiadomienia e-mail za pośrednictwem [SendGrid](https://sendgrid.com/), ale możesz wysłać wiadomość e-mail przy użyciu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="65e12-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="65e12-140">Zainstaluj `SendGrid` pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="65e12-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65e12-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65e12-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="65e12-142">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="65e12-142">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="65e12-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="65e12-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="65e12-144">Z poziomu konsoli wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="65e12-144">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="65e12-145">Zobacz [bezpłatnie Rozpocznij pracę za pomocą usługi SendGrid](https://sendgrid.com/free/) zarejestrować o utworzenie bezpłatnego konta SendGrid.</span><span class="sxs-lookup"><span data-stu-id="65e12-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="65e12-146">Implementowanie IEmailSender</span><span class="sxs-lookup"><span data-stu-id="65e12-146">Implement IEmailSender</span></span>

<span data-ttu-id="65e12-147">Zaimplementowanie `IEmailSender`, Utwórz *Services/EmailSender.cs* kodem podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="65e12-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="65e12-148">Konfigurowanie uruchamiania do obsługi poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-148">Configure startup to support email</span></span>

<span data-ttu-id="65e12-149">Dodaj następujący kod do `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="65e12-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="65e12-150">Dodaj `EmailSender` jako przejściowe usługi.</span><span class="sxs-lookup"><span data-stu-id="65e12-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="65e12-151">Zarejestruj `AuthMessageSenderOptions` wystąpienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="65e12-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="65e12-152">Rejestrowania i resetowania hasła oraz Potwierdź adres e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="65e12-153">Uruchamianie aplikacji sieci web i przetestować potwierdzenie konta i hasła odzyskiwania przepływu.</span><span class="sxs-lookup"><span data-stu-id="65e12-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="65e12-154">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="65e12-154">Run the app and register a new user</span></span>
* <span data-ttu-id="65e12-155">Sprawdź pocztę e-mail, aby uzyskać link do potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="65e12-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="65e12-156">Zobacz [debugowania e-mail](#debug) otrzymasz wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="65e12-157">Kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="65e12-158">Zaloguj się przy użyciu poczty e-mail i hasło.</span><span class="sxs-lookup"><span data-stu-id="65e12-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="65e12-159">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="65e12-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="65e12-160">Resetowanie hasła testu</span><span class="sxs-lookup"><span data-stu-id="65e12-160">Test password reset</span></span>

* <span data-ttu-id="65e12-161">Jeśli zalogowano Cię, wybierz opcję **wylogowania**.</span><span class="sxs-lookup"><span data-stu-id="65e12-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="65e12-162">Wybierz **Zaloguj** łącze, a następnie wybierz pozycję **nie pamiętasz hasła?** łącza.</span><span class="sxs-lookup"><span data-stu-id="65e12-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="65e12-163">Wprowadź adres e-mail, którego użyłeś do zarejestrować konto.</span><span class="sxs-lookup"><span data-stu-id="65e12-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="65e12-164">Wiadomość e-mail z linkiem do zresetowania hasła są wysyłane.</span><span class="sxs-lookup"><span data-stu-id="65e12-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="65e12-165">Sprawdź pocztę e-mail, a następnie kliknij link, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="65e12-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="65e12-166">Po pomyślnie zresetowano hasło możesz zarejestrować się przy użyciu poczty e-mail i nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="65e12-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="65e12-167">Limit czasu wiadomości e-mail i działania zmiany</span><span class="sxs-lookup"><span data-stu-id="65e12-167">Change email and activity timeout</span></span>

<span data-ttu-id="65e12-168">Domyślny limit czasu braku aktywności to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="65e12-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="65e12-169">Poniższy kod ustawia limit czasu braku aktywności na 5 dni:</span><span class="sxs-lookup"><span data-stu-id="65e12-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="65e12-170">Zmień wszystkie lifespans tokenu ochrony danych</span><span class="sxs-lookup"><span data-stu-id="65e12-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="65e12-171">Poniższy kod zmienia wszystkie dane ochrony tokenów limit czasu do 3 godzin:</span><span class="sxs-lookup"><span data-stu-id="65e12-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="65e12-172">Wbudowanej w tożsamości tokeny użytkowników (zobacz [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) mają [limitu czasu w ciągu jednego dnia](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="65e12-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="65e12-173">Zmień czas tokenu poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-173">Change the email token lifespan</span></span>

<span data-ttu-id="65e12-174">Domyślny token żywotność [tokeny użytkowników tożsamości](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) jest [jeden dzień](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="65e12-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="65e12-175">W tej sekcji pokazano, jak zmienić czas tokenu wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="65e12-176">Dodaj niestandardową [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) i <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="65e12-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="65e12-177">Dodaj niestandardowego dostawcę do kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="65e12-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="65e12-178">Ponownie wyślij e-mail z potwierdzeniem</span><span class="sxs-lookup"><span data-stu-id="65e12-178">Resend email confirmation</span></span>

<span data-ttu-id="65e12-179">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="65e12-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="65e12-180">Debugowanie poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-180">Debug email</span></span>

<span data-ttu-id="65e12-181">Jeśli nie można rozpocząć pracę poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="65e12-181">If you can't get email working:</span></span>

* <span data-ttu-id="65e12-182">Ustaw punkt przerwania `EmailSender.Execute` Aby zweryfikować `SendGridClient.SendEmailAsync` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="65e12-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="65e12-183">Tworzenie [aplikacji konsoli, aby wysłać wiadomość e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) przy użyciu kodu podobne do `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="65e12-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="65e12-184">Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.</span><span class="sxs-lookup"><span data-stu-id="65e12-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="65e12-185">Sprawdź folder wiadomości-śmieci.</span><span class="sxs-lookup"><span data-stu-id="65e12-185">Check your spam folder.</span></span>
* <span data-ttu-id="65e12-186">Spróbuj inny alias poczty e-mail na innego dostawcy poczty e-mail (Microsoft Yahoo, Gmail, itp.)</span><span class="sxs-lookup"><span data-stu-id="65e12-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="65e12-187">Spróbuj wysłać do różnymi kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="65e12-188">**Ze względów bezpieczeństwa** jest **nie** używania wpisów tajnych produkcyjnych, w projektowania i testowania.</span><span class="sxs-lookup"><span data-stu-id="65e12-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="65e12-189">Jeśli opublikujesz aplikację na platformie Azure, należy ustawić wpisy tajne usługi SendGrid jako ustawienia aplikacji w portalu usługi Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="65e12-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="65e12-190">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="65e12-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="65e12-191">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="65e12-191">Combine social and local login accounts</span></span>

<span data-ttu-id="65e12-192">Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="65e12-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="65e12-193">Zobacz [Facebook, Google i uwierzytelniania zewnętrznego dostawcy](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="65e12-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="65e12-194">Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="65e12-195">W następującej kolejności "RickAndMSFT@gmail.com" najpierw jest tworzony jako lokalny identyfikator logowania; jednak możesz najpierw utworzyć konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="65e12-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="65e12-197">Kliknij pozycję **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="65e12-197">Click on the **Manage** link.</span></span> <span data-ttu-id="65e12-198">Należy pamiętać, zewnętrzne 0 (logowania społecznościowego) skojarzony z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="65e12-198">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzanie widoku](accconfirm/_static/manage.png)

<span data-ttu-id="65e12-200">Kliknij link do innej usługi, zaloguj się i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="65e12-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="65e12-201">Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznych:</span><span class="sxs-lookup"><span data-stu-id="65e12-201">In the following image, Facebook is the external authentication provider:</span></span>

![Zarządzanie wyświetlania listy Facebooku widoku logowań zewnętrznych](accconfirm/_static/fb.png)

<span data-ttu-id="65e12-203">Te dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="65e12-203">The two accounts have been combined.</span></span> <span data-ttu-id="65e12-204">Jesteś w stanie zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="65e12-204">You are able to sign in with either account.</span></span> <span data-ttu-id="65e12-205">Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania uwierzytelniania usługa nie działa lub większe prawdopodobieństwo ich utraty dostępu do swojego konta w sieci społecznościowej.</span><span class="sxs-lookup"><span data-stu-id="65e12-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="65e12-206">Włącz potwierdzenie konta, po lokacji ma użytkowników</span><span class="sxs-lookup"><span data-stu-id="65e12-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="65e12-207">Włączanie potwierdzenie konta w witrynie użytkownikom blokuje istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="65e12-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="65e12-208">Istniejący użytkownicy są zablokowane, ponieważ ich konta nie są potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="65e12-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="65e12-209">Aby obejść istniejące blokady użytkownika, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="65e12-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="65e12-210">Aktualizuj bazę danych, aby oznaczyć wszyscy istniejący użytkownicy, co zostało potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="65e12-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="65e12-211">Upewnij się, liczba istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="65e12-211">Confirm existing users.</span></span> <span data-ttu-id="65e12-212">Na przykład usługi batch — wysyłanie wiadomości e-mail przy użyciu linki do potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="65e12-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="65e12-213">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="65e12-213">Prerequisites</span></span>

[<span data-ttu-id="65e12-214">Zestaw SDK programu .NET core 2.2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="65e12-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="65e12-215">Tworzenie aplikacji sieci web i tworzenia szkieletu tożsamości</span><span class="sxs-lookup"><span data-stu-id="65e12-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="65e12-216">Uruchom następujące polecenia, aby utworzyć aplikację sieci web przy użyciu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="65e12-216">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="65e12-217">Rejestrowanie nowego użytkownika testowego</span><span class="sxs-lookup"><span data-stu-id="65e12-217">Test new user registration</span></span>

<span data-ttu-id="65e12-218">Uruchom aplikację, wybierz **zarejestrować** łącze, a następnie zarejestrować użytkownik.</span><span class="sxs-lookup"><span data-stu-id="65e12-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="65e12-219">W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="65e12-219">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="65e12-220">Po przesłaniu rejestracji, użytkownik jest zalogowany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="65e12-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="65e12-221">W dalszej części samouczka kod jest aktualizowana, aby nowi użytkownicy nie może się zalogować, dopóki nie zostanie zweryfikowana poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="65e12-222">Należy pamiętać, tabeli `EmailConfirmed` pole jest `False`.</span><span class="sxs-lookup"><span data-stu-id="65e12-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="65e12-223">Można używać tego adresu e-mail ponownie w następnym kroku, gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="65e12-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="65e12-224">Kliknij prawym przyciskiem myszy na wiersz i wybierz **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="65e12-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="65e12-225">Usuwanie aliasu adresu e-mail ułatwia w poniższych krokach.</span><span class="sxs-lookup"><span data-stu-id="65e12-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="65e12-226">Wymagaj potwierdzenie adresu e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-226">Require email confirmation</span></span>

<span data-ttu-id="65e12-227">Jest najlepszym rozwiązaniem, aby potwierdzić adres e-mail rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="65e12-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="65e12-228">Wiadomość e-mail potwierdzenia pomaga sprawdzić, mogą one nie personifikuje kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby).</span><span class="sxs-lookup"><span data-stu-id="65e12-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="65e12-229">Załóżmy, że trzeba było forum dyskusyjnego i chcesz uniemożliwić "yli@example.com"z rejestracją jako"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="65e12-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="65e12-230">Bez potwierdzenia e-mail "nolivetto@contoso.com" może otrzymywać niechciane wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="65e12-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="65e12-231">Załóżmy, że użytkownik przypadkowo zarejestrowana jako "ylo@example.com" i nie zostało już słowa "yli".</span><span class="sxs-lookup"><span data-stu-id="65e12-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="65e12-232">W takich sytuacjach przydałaby się mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="65e12-233">Potwierdzenie adresu e-mail zapewnia ograniczoną ochronę, od botów.</span><span class="sxs-lookup"><span data-stu-id="65e12-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="65e12-234">Potwierdzenie adresu e-mail nie zapewniają ochronę przed złośliwymi użytkownikami z wieloma kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="65e12-235">Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do witryny sieci web, zanim potwierdzone pocztą e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="65e12-236">Aktualizacja `Startup.ConfigureServices` będą musieli potwierdzony adres e-mail:</span><span class="sxs-lookup"><span data-stu-id="65e12-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="65e12-237">`config.SignIn.RequireConfirmedEmail = true;` zarejestrowani użytkownicy uniemożliwia logowanie do momentu ich adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="65e12-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="65e12-238">Konfigurowanie dostawcy poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-238">Configure email provider</span></span>

<span data-ttu-id="65e12-239">W tym samouczku [SendGrid](https://sendgrid.com) służy do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="65e12-240">Potrzebujesz konta SendGrid i klucz do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="65e12-241">Można użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-241">You can use other email providers.</span></span> <span data-ttu-id="65e12-242">Platforma ASP.NET Core 2.x obejmuje `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="65e12-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="65e12-243">Zaleca się, że używasz usługi SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="65e12-244">SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="65e12-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="65e12-245">Utwórz klasę, można pobrać klucza zabezpieczanie poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="65e12-246">W tym przykładzie należy utworzyć *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="65e12-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="65e12-247">Konfigurowanie wpisami tajnymi użytkowników usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="65e12-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="65e12-248">Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="65e12-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="65e12-249">Przykład:</span><span class="sxs-lookup"><span data-stu-id="65e12-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="65e12-250">Na Windows, klucza tajnego Manager przechowuje par kluczy i wartości w *secrets.json* w pliku `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="65e12-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="65e12-251">Zawartość *secrets.json* pliku nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="65e12-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="65e12-252">Ilustruje poniższy kod znaczników *secrets.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="65e12-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="65e12-253">`SendGridKey` Wartość została usunięta.</span><span class="sxs-lookup"><span data-stu-id="65e12-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="65e12-254">Aby uzyskać więcej informacji, zobacz [wzorzec opcje](xref:fundamentals/configuration/options) i [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="65e12-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="65e12-255">Instalowanie usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="65e12-255">Install SendGrid</span></span>

<span data-ttu-id="65e12-256">W tym samouczku przedstawiono sposób dodawania powiadomienia e-mail za pośrednictwem [SendGrid](https://sendgrid.com/), ale możesz wysłać wiadomość e-mail przy użyciu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="65e12-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="65e12-257">Zainstaluj `SendGrid` pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="65e12-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65e12-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65e12-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="65e12-259">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="65e12-259">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="65e12-260">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="65e12-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="65e12-261">Z poziomu konsoli wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="65e12-261">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="65e12-262">Zobacz [bezpłatnie Rozpocznij pracę za pomocą usługi SendGrid](https://sendgrid.com/free/) zarejestrować o utworzenie bezpłatnego konta SendGrid.</span><span class="sxs-lookup"><span data-stu-id="65e12-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="65e12-263">Implementowanie IEmailSender</span><span class="sxs-lookup"><span data-stu-id="65e12-263">Implement IEmailSender</span></span>

<span data-ttu-id="65e12-264">Zaimplementowanie `IEmailSender`, Utwórz *Services/EmailSender.cs* kodem podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="65e12-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="65e12-265">Konfigurowanie uruchamiania do obsługi poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-265">Configure startup to support email</span></span>

<span data-ttu-id="65e12-266">Dodaj następujący kod do `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="65e12-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="65e12-267">Dodaj `EmailSender` jako przejściowe usługi.</span><span class="sxs-lookup"><span data-stu-id="65e12-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="65e12-268">Zarejestruj `AuthMessageSenderOptions` wystąpienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="65e12-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="65e12-269">Włącz odzyskiwanie potwierdzenia i hasło konta</span><span class="sxs-lookup"><span data-stu-id="65e12-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="65e12-270">Szablon zawiera kod odzyskiwania potwierdzenia i hasło konta.</span><span class="sxs-lookup"><span data-stu-id="65e12-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="65e12-271">Znajdź `OnPostAsync` method in Class metoda *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="65e12-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="65e12-272">Uniemożliwić nowym użytkownikom Trwa automatyczne logowanie, zakomentowując następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="65e12-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="65e12-273">Za pomocą zmieniony wiersz wyróżniony pokazano całą metodę:</span><span class="sxs-lookup"><span data-stu-id="65e12-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="65e12-274">Rejestrowania i resetowania hasła oraz Potwierdź adres e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="65e12-275">Uruchamianie aplikacji sieci web i przetestować potwierdzenie konta i hasła odzyskiwania przepływu.</span><span class="sxs-lookup"><span data-stu-id="65e12-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="65e12-276">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="65e12-276">Run the app and register a new user</span></span>
* <span data-ttu-id="65e12-277">Sprawdź pocztę e-mail, aby uzyskać link do potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="65e12-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="65e12-278">Zobacz [debugowania e-mail](#debug) otrzymasz wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="65e12-279">Kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="65e12-280">Zaloguj się przy użyciu poczty e-mail i hasło.</span><span class="sxs-lookup"><span data-stu-id="65e12-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="65e12-281">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="65e12-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="65e12-282">Wyświetlanie strony zarządzania</span><span class="sxs-lookup"><span data-stu-id="65e12-282">View the manage page</span></span>

<span data-ttu-id="65e12-283">Wybierz swoją nazwę użytkownika w przeglądarce: ![okno przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="65e12-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="65e12-284">Zostanie wyświetlona strona zarządzania, z **profilu** wybraną kartą.</span><span class="sxs-lookup"><span data-stu-id="65e12-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="65e12-285">**E-mail** przedstawia pole wyboru, wskazujący adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="65e12-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="65e12-286">Resetowanie hasła testu</span><span class="sxs-lookup"><span data-stu-id="65e12-286">Test password reset</span></span>

* <span data-ttu-id="65e12-287">Jeśli zalogowano Cię, wybierz opcję **wylogowania**.</span><span class="sxs-lookup"><span data-stu-id="65e12-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="65e12-288">Wybierz **Zaloguj** łącze, a następnie wybierz pozycję **nie pamiętasz hasła?** łącza.</span><span class="sxs-lookup"><span data-stu-id="65e12-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="65e12-289">Wprowadź adres e-mail, którego użyłeś do zarejestrować konto.</span><span class="sxs-lookup"><span data-stu-id="65e12-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="65e12-290">Wiadomość e-mail z linkiem do zresetowania hasła są wysyłane.</span><span class="sxs-lookup"><span data-stu-id="65e12-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="65e12-291">Sprawdź pocztę e-mail, a następnie kliknij link, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="65e12-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="65e12-292">Po pomyślnie zresetowano hasło możesz zarejestrować się przy użyciu poczty e-mail i nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="65e12-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="65e12-293">Limit czasu wiadomości e-mail i działania zmiany</span><span class="sxs-lookup"><span data-stu-id="65e12-293">Change email and activity timeout</span></span>

<span data-ttu-id="65e12-294">Domyślny limit czasu braku aktywności to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="65e12-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="65e12-295">Poniższy kod ustawia limit czasu braku aktywności na 5 dni:</span><span class="sxs-lookup"><span data-stu-id="65e12-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="65e12-296">Zmień wszystkie lifespans tokenu ochrony danych</span><span class="sxs-lookup"><span data-stu-id="65e12-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="65e12-297">Poniższy kod zmienia wszystkie dane ochrony tokenów limit czasu do 3 godzin:</span><span class="sxs-lookup"><span data-stu-id="65e12-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="65e12-298">Wbudowanej w tożsamości tokeny użytkowników (zobacz [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) mają [limitu czasu w ciągu jednego dnia](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="65e12-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="65e12-299">Zmień czas tokenu poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-299">Change the email token lifespan</span></span>

<span data-ttu-id="65e12-300">Domyślny token żywotność [tokeny użytkowników tożsamości](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) jest [jeden dzień](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="65e12-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="65e12-301">W tej sekcji pokazano, jak zmienić czas tokenu wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="65e12-302">Dodaj niestandardową [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) i <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="65e12-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="65e12-303">Dodaj niestandardowego dostawcę do kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="65e12-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="65e12-304">Ponownie wyślij e-mail z potwierdzeniem</span><span class="sxs-lookup"><span data-stu-id="65e12-304">Resend email confirmation</span></span>

<span data-ttu-id="65e12-305">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="65e12-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="65e12-306">Debugowanie poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="65e12-306">Debug email</span></span>

<span data-ttu-id="65e12-307">Jeśli nie można rozpocząć pracę poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="65e12-307">If you can't get email working:</span></span>

* <span data-ttu-id="65e12-308">Ustaw punkt przerwania `EmailSender.Execute` Aby zweryfikować `SendGridClient.SendEmailAsync` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="65e12-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="65e12-309">Tworzenie [aplikacji konsoli, aby wysłać wiadomość e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) przy użyciu kodu podobne do `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="65e12-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="65e12-310">Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.</span><span class="sxs-lookup"><span data-stu-id="65e12-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="65e12-311">Sprawdź folder wiadomości-śmieci.</span><span class="sxs-lookup"><span data-stu-id="65e12-311">Check your spam folder.</span></span>
* <span data-ttu-id="65e12-312">Spróbuj inny alias poczty e-mail na innego dostawcy poczty e-mail (Microsoft Yahoo, Gmail, itp.)</span><span class="sxs-lookup"><span data-stu-id="65e12-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="65e12-313">Spróbuj wysłać do różnymi kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="65e12-314">**Ze względów bezpieczeństwa** jest **nie** używania wpisów tajnych produkcyjnych, w projektowania i testowania.</span><span class="sxs-lookup"><span data-stu-id="65e12-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="65e12-315">Jeśli opublikujesz aplikację na platformie Azure, możesz ustawić wpisy tajne usługi SendGrid, jako ustawienia aplikacji w portalu usługi Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="65e12-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="65e12-316">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="65e12-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="65e12-317">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="65e12-317">Combine social and local login accounts</span></span>

<span data-ttu-id="65e12-318">Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="65e12-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="65e12-319">Zobacz [Facebook, Google i uwierzytelniania zewnętrznego dostawcy](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="65e12-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="65e12-320">Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="65e12-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="65e12-321">W następującej kolejności "RickAndMSFT@gmail.com" najpierw jest tworzony jako lokalny identyfikator logowania; jednak możesz najpierw utworzyć konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="65e12-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="65e12-323">Kliknij pozycję **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="65e12-323">Click on the **Manage** link.</span></span> <span data-ttu-id="65e12-324">Należy pamiętać, zewnętrzne 0 (logowania społecznościowego) skojarzony z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="65e12-324">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzanie widoku](accconfirm/_static/manage.png)

<span data-ttu-id="65e12-326">Kliknij link do innej usługi, zaloguj się i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="65e12-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="65e12-327">Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznych:</span><span class="sxs-lookup"><span data-stu-id="65e12-327">In the following image, Facebook is the external authentication provider:</span></span>

![Zarządzanie wyświetlania listy Facebooku widoku logowań zewnętrznych](accconfirm/_static/fb.png)

<span data-ttu-id="65e12-329">Te dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="65e12-329">The two accounts have been combined.</span></span> <span data-ttu-id="65e12-330">Jesteś w stanie zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="65e12-330">You are able to sign in with either account.</span></span> <span data-ttu-id="65e12-331">Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania uwierzytelniania usługa nie działa lub większe prawdopodobieństwo ich utraty dostępu do swojego konta w sieci społecznościowej.</span><span class="sxs-lookup"><span data-stu-id="65e12-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="65e12-332">Włącz potwierdzenie konta, po lokacji ma użytkowników</span><span class="sxs-lookup"><span data-stu-id="65e12-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="65e12-333">Włączanie potwierdzenie konta w witrynie użytkownikom blokuje istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="65e12-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="65e12-334">Istniejący użytkownicy są zablokowane, ponieważ ich konta nie są potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="65e12-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="65e12-335">Aby obejść istniejące blokady użytkownika, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="65e12-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="65e12-336">Aktualizuj bazę danych, aby oznaczyć wszyscy istniejący użytkownicy, co zostało potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="65e12-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="65e12-337">Upewnij się, liczba istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="65e12-337">Confirm existing users.</span></span> <span data-ttu-id="65e12-338">Na przykład usługi batch — wysyłanie wiadomości e-mail przy użyciu linki do potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="65e12-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
