---
title: Potwierdzenie konta i odzyskiwanie hasła w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację ASP.NET Core przy użyciu potwierdzenia wiadomości e-mail i resetowania hasła.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: a4ecc2d91fb72915703dfaa146260f0c1360bded
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880767"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="7084a-103">Potwierdzenie konta i odzyskiwanie hasła w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7084a-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="7084a-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant)i Jan [Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="7084a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="7084a-105">W tym samouczku pokazano, jak utworzyć aplikację ASP.NET Core przy użyciu potwierdzenia wiadomości e-mail i resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="7084a-106">Ten samouczek **nie** jest tematem początkowym.</span><span class="sxs-lookup"><span data-stu-id="7084a-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="7084a-107">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="7084a-107">You should be familiar with:</span></span>

* [<span data-ttu-id="7084a-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7084a-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="7084a-109">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="7084a-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="7084a-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7084a-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="7084a-111">Zobacz [ten plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) dla wersji ASP.NET Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="7084a-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="7084a-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7084a-112">Prerequisites</span></span>

[<span data-ttu-id="7084a-113">.NET Core 3,0 SDK lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7084a-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="7084a-114">Tworzenie i testowanie aplikacji sieci Web przy użyciu uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="7084a-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="7084a-115">Uruchom następujące polecenia, aby utworzyć aplikację sieci Web z uwierzytelnianiem.</span><span class="sxs-lookup"><span data-stu-id="7084a-115">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="7084a-116">Uruchom aplikację, wybierz łącze **zarejestruj** i zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7084a-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="7084a-117">Po zarejestrowaniu nastąpi przekierowanie do strony programu do `/Identity/Account/RegisterConfirmation`, która zawiera link umożliwiający zasymulowanie potwierdzenia wiadomości e-mail:</span><span class="sxs-lookup"><span data-stu-id="7084a-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="7084a-118">Wybierz łącze `Click here to confirm your account`.</span><span class="sxs-lookup"><span data-stu-id="7084a-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="7084a-119">Wybierz łącze **logowania** i zaloguj się przy użyciu tych samych poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="7084a-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="7084a-120">Wybierz łącze `Hello YourEmail@provider.com!`, które przekierowuje Cię do strony `/Identity/Account/Manage/PersonalData`.</span><span class="sxs-lookup"><span data-stu-id="7084a-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="7084a-121">Wybierz kartę **dane osobowe** po lewej stronie, a następnie wybierz pozycję **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="7084a-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="7084a-122">Konfigurowanie dostawcy poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-122">Configure an email provider</span></span>

<span data-ttu-id="7084a-123">W tym samouczku [SendGrid](https://sendgrid.com) jest używany do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="7084a-124">Aby wysłać wiadomość e-mail, musisz mieć konto SendGrid i klucz.</span><span class="sxs-lookup"><span data-stu-id="7084a-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="7084a-125">Możesz użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-125">You can use other email providers.</span></span> <span data-ttu-id="7084a-126">Zalecamy użycie SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="7084a-127">Protokół SMTP jest trudny do poprawnego zabezpieczania i konfigurowania.</span><span class="sxs-lookup"><span data-stu-id="7084a-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="7084a-128">Utwórz klasę, aby pobrać bezpieczny klucz poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="7084a-129">Dla tego przykładu Utwórz *usługi/AuthMessageSenderOptions. cs*:</span><span class="sxs-lookup"><span data-stu-id="7084a-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="7084a-130">Konfigurowanie kluczy tajnych użytkownika SendGrid</span><span class="sxs-lookup"><span data-stu-id="7084a-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="7084a-131">Ustaw `SendGridUser` i `SendGridKey` za pomocą [Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7084a-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7084a-132">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7084a-132">For example:</span></span>

```dotnetcli
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="7084a-133">W systemie Windows Menedżer wpisów tajnych przechowuje pary klucz/wartość w pliku Secret *. JSON* w katalogu `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="7084a-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="7084a-134">Zawartość pliku Secret *. JSON* nie jest zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="7084a-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="7084a-135">W poniższym znaczniku przedstawiono plik Secret *. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7084a-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="7084a-136">Wartość `SendGridKey` została usunięta.</span><span class="sxs-lookup"><span data-stu-id="7084a-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="7084a-137">Aby uzyskać więcej informacji, zobacz [Opcje wzorca](xref:fundamentals/configuration/options) i [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7084a-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="7084a-138">Zainstaluj SendGrid</span><span class="sxs-lookup"><span data-stu-id="7084a-138">Install SendGrid</span></span>

<span data-ttu-id="7084a-139">W tym samouczku pokazano, jak dodać powiadomienia e-mail za pośrednictwem usługi [SendGrid](https://sendgrid.com/), ale można wysyłać wiadomości e-mail przy użyciu protokołu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="7084a-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="7084a-140">Zainstaluj pakiet NuGet `SendGrid`:</span><span class="sxs-lookup"><span data-stu-id="7084a-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7084a-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7084a-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7084a-142">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7084a-142">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7084a-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7084a-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7084a-144">W konsoli programu wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7084a-144">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="7084a-145">Zobacz artykuł Wprowadzenie do usługi [SendGrid bezpłatnie](https://sendgrid.com/free/) , aby zarejestrować się w celu uzyskania bezpłatnego konta SendGrid.</span><span class="sxs-lookup"><span data-stu-id="7084a-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="7084a-146">Implementuj IEmailSender</span><span class="sxs-lookup"><span data-stu-id="7084a-146">Implement IEmailSender</span></span>

<span data-ttu-id="7084a-147">Aby zaimplementować `IEmailSender`, Utwórz *usługi/EmailSender. cs* z kodem podobnym do poniższego:</span><span class="sxs-lookup"><span data-stu-id="7084a-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="7084a-148">Konfigurowanie uruchamiania do obsługi poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-148">Configure startup to support email</span></span>

<span data-ttu-id="7084a-149">Dodaj następujący kod do metody `ConfigureServices` w pliku *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="7084a-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="7084a-150">Dodaj `EmailSender` jako usługę przejściową.</span><span class="sxs-lookup"><span data-stu-id="7084a-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="7084a-151">Zarejestruj wystąpienie konfiguracji `AuthMessageSenderOptions`.</span><span class="sxs-lookup"><span data-stu-id="7084a-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="7084a-152">Rejestrowanie, Potwierdzanie poczty e-mail i resetowanie hasła</span><span class="sxs-lookup"><span data-stu-id="7084a-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="7084a-153">Uruchom aplikację internetową, a następnie przetestuj potwierdzenie konta i przepływ odzyskiwania hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="7084a-154">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="7084a-154">Run the app and register a new user</span></span>
* <span data-ttu-id="7084a-155">Sprawdź wiadomość e-mail z linkiem potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="7084a-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="7084a-156">Jeśli nie otrzymasz wiadomości e-mail, zobacz [debugowanie poczty e-mail](#debug) .</span><span class="sxs-lookup"><span data-stu-id="7084a-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="7084a-157">Kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="7084a-158">Zaloguj się przy użyciu adresu e-mail i hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="7084a-159">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="7084a-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="7084a-160">Resetowanie hasła do testu</span><span class="sxs-lookup"><span data-stu-id="7084a-160">Test password reset</span></span>

* <span data-ttu-id="7084a-161">Jeśli użytkownik jest zalogowany, wybierz pozycję **Wyloguj**.</span><span class="sxs-lookup"><span data-stu-id="7084a-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="7084a-162">Wybierz link **Zaloguj** i wybierz łącze nie **pamiętasz hasła?**</span><span class="sxs-lookup"><span data-stu-id="7084a-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="7084a-163">Wprowadź adres e-mail, który został użyty do zarejestrowania konta.</span><span class="sxs-lookup"><span data-stu-id="7084a-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="7084a-164">Zostanie wysłana wiadomość e-mail z linkiem służącym do resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="7084a-165">Sprawdź swój adres e-mail i kliknij link, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="7084a-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="7084a-166">Po pomyślnym zresetowaniu hasła możesz zalogować się przy użyciu adresu e-mail i nowego hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="7084a-167">Zmień limit czasu wiadomości e-mail i aktywności</span><span class="sxs-lookup"><span data-stu-id="7084a-167">Change email and activity timeout</span></span>

<span data-ttu-id="7084a-168">Domyślny limit czasu braku aktywności wynosi 14 dni.</span><span class="sxs-lookup"><span data-stu-id="7084a-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="7084a-169">Następujący kod ustawia limit czasu braku aktywności na 5 dni:</span><span class="sxs-lookup"><span data-stu-id="7084a-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="7084a-170">Zmień wszystkie lifespans tokenów ochrony danych</span><span class="sxs-lookup"><span data-stu-id="7084a-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="7084a-171">Następujący kod zmienia limit czasu tokenów ochrony danych na 3 godziny:</span><span class="sxs-lookup"><span data-stu-id="7084a-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="7084a-172">Wbudowane tokeny użytkownika tożsamości (zobacz [AspNetCore/src/Identity/Extensions. Core/src/TokenOptions. cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) mają [jeden dzień limitu czasu](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7084a-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="7084a-173">Zmień cykl życia tokenu poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-173">Change the email token lifespan</span></span>

<span data-ttu-id="7084a-174">Domyślny token cykl życia tokenów [użytkownika tożsamości](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) to [jeden dzień](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7084a-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="7084a-175">W tej sekcji przedstawiono sposób zmiany cykl życia tokenu poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="7084a-176">Dodaj niestandardową [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) i <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="7084a-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="7084a-177">Dodaj dostawcę niestandardowego do kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="7084a-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="7084a-178">Potwierdzenie ponownego wysłania wiadomości e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-178">Resend email confirmation</span></span>

<span data-ttu-id="7084a-179">Zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/5410)w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="7084a-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="7084a-180">Debuguj wiadomość e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-180">Debug email</span></span>

<span data-ttu-id="7084a-181">Jeśli nie możesz uzyskać pracy z wiadomościami e-mail:</span><span class="sxs-lookup"><span data-stu-id="7084a-181">If you can't get email working:</span></span>

* <span data-ttu-id="7084a-182">Ustaw punkt przerwania w `EmailSender.Execute`, aby sprawdzić, czy `SendGridClient.SendEmailAsync` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="7084a-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="7084a-183">Utwórz [aplikację konsolową do wysyłania wiadomości e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) przy użyciu podobnego kodu do `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="7084a-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="7084a-184">Przejrzyj stronę [działania e-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) .</span><span class="sxs-lookup"><span data-stu-id="7084a-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="7084a-185">Sprawdź swój folder spamu.</span><span class="sxs-lookup"><span data-stu-id="7084a-185">Check your spam folder.</span></span>
* <span data-ttu-id="7084a-186">Wypróbuj inny alias poczty e-mail na innym dostawcy poczty e-mail (Microsoft, Yahoo, Gmail itp.)</span><span class="sxs-lookup"><span data-stu-id="7084a-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="7084a-187">Spróbuj wysłać konto do innych kont e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="7084a-188">**Najlepszym rozwiązaniem** w zakresie zabezpieczeń jest **nieużywanie produkcyjnych** wpisów tajnych w testowaniu i programowaniu.</span><span class="sxs-lookup"><span data-stu-id="7084a-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="7084a-189">Jeśli opublikujesz aplikację na platformie Azure, ustaw wpisy tajne SendGrid jako ustawienia aplikacji w portalu aplikacji sieci Web platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="7084a-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="7084a-190">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="7084a-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7084a-191">Łączenie kont logowania społecznościowych i lokalnych</span><span class="sxs-lookup"><span data-stu-id="7084a-191">Combine social and local login accounts</span></span>

<span data-ttu-id="7084a-192">Aby ukończyć tę sekcję, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7084a-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="7084a-193">Zobacz [uwierzytelnianie w usługach Facebook, Google i dostawcy zewnętrznym](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7084a-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="7084a-194">Konta lokalne i społecznościowe można łączyć przez kliknięcie linku do poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7084a-195">W następującej kolejności "RickAndMSFT@gmail.com" jest najpierw tworzony jako logowanie lokalne; można jednak najpierw utworzyć konto jako nazwę logowania w sieci społecznościowej, a następnie dodać lokalną nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="7084a-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="7084a-197">Kliknij link **Zarządzaj** .</span><span class="sxs-lookup"><span data-stu-id="7084a-197">Click on the **Manage** link.</span></span> <span data-ttu-id="7084a-198">Zwróć uwagę na wartość 0 External (logowanie społeczne) skojarzoną z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="7084a-198">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzaj widokiem](accconfirm/_static/manage.png)

<span data-ttu-id="7084a-200">Kliknij link do innej usługi logowania i zaakceptuj żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7084a-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="7084a-201">Na poniższym obrazie serwis Facebook jest dostawcą zewnętrznym uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="7084a-201">In the following image, Facebook is the external authentication provider:</span></span>

![Zarządzanie widokiem logowania zewnętrznego w serwisie Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="7084a-203">Te dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="7084a-203">The two accounts have been combined.</span></span> <span data-ttu-id="7084a-204">Możesz zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="7084a-204">You are able to sign in with either account.</span></span> <span data-ttu-id="7084a-205">Możesz chcieć, aby użytkownicy mogli dodawać konta lokalne w przypadku, gdy usługa uwierzytelniania przy logowaniu w sieci społecznościowej nie działa, lub prawdopodobnie utraciła dostęp do konta społecznościowego.</span><span class="sxs-lookup"><span data-stu-id="7084a-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="7084a-206">Włącz potwierdzenie konta po stronie, w której znajdują się użytkownicy</span><span class="sxs-lookup"><span data-stu-id="7084a-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="7084a-207">Włączenie potwierdzenia konta w witrynie z użytkownikami powoduje zablokowanie wszystkich istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7084a-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="7084a-208">Istniejący użytkownicy są Zablokowani, ponieważ ich konta nie zostały potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="7084a-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="7084a-209">Aby obejść istniejącą blokadę użytkownika, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="7084a-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="7084a-210">Zaktualizuj bazę danych, aby oznaczyć wszystkich istniejących użytkowników jako potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="7084a-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="7084a-211">Potwierdź istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7084a-211">Confirm existing users.</span></span> <span data-ttu-id="7084a-212">Na przykład wysyłanie wsadowe wiadomości e-mail z linkami potwierdzającymi.</span><span class="sxs-lookup"><span data-stu-id="7084a-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="7084a-213">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7084a-213">Prerequisites</span></span>

[<span data-ttu-id="7084a-214">.NET Core 2,2 SDK lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7084a-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="7084a-215">Tworzenie aplikacji sieci Web i tożsamości szkieletu</span><span class="sxs-lookup"><span data-stu-id="7084a-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="7084a-216">Uruchom następujące polecenia, aby utworzyć aplikację sieci Web z uwierzytelnianiem.</span><span class="sxs-lookup"><span data-stu-id="7084a-216">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="7084a-217">Testowanie rejestracji nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="7084a-217">Test new user registration</span></span>

<span data-ttu-id="7084a-218">Uruchom aplikację, wybierz łącze **zarejestruj** i zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7084a-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="7084a-219">W tym momencie jedynym sprawdzaniem poprawności w wiadomości e-mail jest atrybut [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) .</span><span class="sxs-lookup"><span data-stu-id="7084a-219">At this point, the only validation on the email is with the [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="7084a-220">Po przesłaniu rejestracji nastąpi zalogowanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7084a-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="7084a-221">W dalszej części tego samouczka kod zostanie zaktualizowany, więc nowi użytkownicy nie będą mogli się zalogować do momentu zweryfikowania ich poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="7084a-222">Zwróć uwagę na to, że pole `EmailConfirmed` tabeli jest `False`.</span><span class="sxs-lookup"><span data-stu-id="7084a-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="7084a-223">Możesz chcieć ponownie użyć tej wiadomości e-mail w następnym kroku, gdy aplikacja wyśle wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="7084a-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="7084a-224">Kliknij prawym przyciskiem myszy wiersz i wybierz polecenie **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="7084a-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="7084a-225">Usunięcie aliasu e-mail ułatwia wykonanie następujących czynności.</span><span class="sxs-lookup"><span data-stu-id="7084a-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="7084a-226">Żądaj potwierdzenia wiadomości e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-226">Require email confirmation</span></span>

<span data-ttu-id="7084a-227">Najlepszym rozwiązaniem jest potwierdzenie wiadomości e-mail o nowej rejestracji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7084a-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="7084a-228">Potwierdzenie poczty e-mail pomaga sprawdzić, czy nie personifikują kogoś innego (oznacza to, że nie zostały zarejestrowane w wiadomości e-mail innej osoby).</span><span class="sxs-lookup"><span data-stu-id="7084a-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="7084a-229">Załóżmy, że masz forum dyskusyjne i chcesz zapobiec rejestrowaniu się "yli@example.com" jako "nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="7084a-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="7084a-230">Bez potwierdzenia wiadomości e-mail "nolivetto@contoso.com" może odbierać niechciane wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7084a-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="7084a-231">Załóżmy, że użytkownik przypadkowo zarejestrował się jako "ylo@example.com" i nie błąd pisowni "yli".</span><span class="sxs-lookup"><span data-stu-id="7084a-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="7084a-232">Nie będą one mogły korzystać z odzyskiwania hasła, ponieważ aplikacja nie ma poprawnego adresu e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="7084a-233">Potwierdzenie poczty e-mail zapewnia ograniczoną ochronę z botów.</span><span class="sxs-lookup"><span data-stu-id="7084a-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="7084a-234">Potwierdzenie poczty e-mail nie zapewnia ochrony przed złośliwymi użytkownikami z wieloma kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="7084a-235">Zazwyczaj chcesz uniemożliwić nowym użytkownikom ogłaszanie danych w witrynie sieci Web przed potwierdzeniem wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="7084a-236">Zaktualizuj `Startup.ConfigureServices`, aby wymagały potwierdzonej wiadomości e-mail:</span><span class="sxs-lookup"><span data-stu-id="7084a-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="7084a-237">`config.SignIn.RequireConfirmedEmail = true;` uniemożliwia rejestrowanie zalogowanych użytkowników do momentu potwierdzenia ich wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="7084a-238">Konfigurowanie dostawcy poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-238">Configure email provider</span></span>

<span data-ttu-id="7084a-239">W tym samouczku [SendGrid](https://sendgrid.com) jest używany do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="7084a-240">Aby wysłać wiadomość e-mail, musisz mieć konto SendGrid i klucz.</span><span class="sxs-lookup"><span data-stu-id="7084a-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="7084a-241">Możesz użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-241">You can use other email providers.</span></span> <span data-ttu-id="7084a-242">ASP.NET Core 2. x zawiera `System.Net.Mail`, co umożliwia wysyłanie wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7084a-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="7084a-243">Zalecamy użycie SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="7084a-244">Protokół SMTP jest trudny do poprawnego zabezpieczania i konfigurowania.</span><span class="sxs-lookup"><span data-stu-id="7084a-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="7084a-245">Utwórz klasę, aby pobrać bezpieczny klucz poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="7084a-246">Dla tego przykładu Utwórz *usługi/AuthMessageSenderOptions. cs*:</span><span class="sxs-lookup"><span data-stu-id="7084a-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="7084a-247">Konfigurowanie kluczy tajnych użytkownika SendGrid</span><span class="sxs-lookup"><span data-stu-id="7084a-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="7084a-248">Ustaw `SendGridUser` i `SendGridKey` za pomocą [Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7084a-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7084a-249">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7084a-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="7084a-250">W systemie Windows Menedżer wpisów tajnych przechowuje pary klucz/wartość w pliku Secret *. JSON* w katalogu `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="7084a-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="7084a-251">Zawartość pliku Secret *. JSON* nie jest zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="7084a-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="7084a-252">W poniższym znaczniku przedstawiono plik Secret *. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7084a-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="7084a-253">Wartość `SendGridKey` została usunięta.</span><span class="sxs-lookup"><span data-stu-id="7084a-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="7084a-254">Aby uzyskać więcej informacji, zobacz [Opcje wzorca](xref:fundamentals/configuration/options) i [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7084a-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="7084a-255">Zainstaluj SendGrid</span><span class="sxs-lookup"><span data-stu-id="7084a-255">Install SendGrid</span></span>

<span data-ttu-id="7084a-256">W tym samouczku pokazano, jak dodać powiadomienia e-mail za pośrednictwem usługi [SendGrid](https://sendgrid.com/), ale można wysyłać wiadomości e-mail przy użyciu protokołu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="7084a-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="7084a-257">Zainstaluj pakiet NuGet `SendGrid`:</span><span class="sxs-lookup"><span data-stu-id="7084a-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7084a-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7084a-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7084a-259">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7084a-259">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7084a-260">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7084a-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7084a-261">W konsoli programu wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7084a-261">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="7084a-262">Zobacz artykuł Wprowadzenie do usługi [SendGrid bezpłatnie](https://sendgrid.com/free/) , aby zarejestrować się w celu uzyskania bezpłatnego konta SendGrid.</span><span class="sxs-lookup"><span data-stu-id="7084a-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="7084a-263">Implementuj IEmailSender</span><span class="sxs-lookup"><span data-stu-id="7084a-263">Implement IEmailSender</span></span>

<span data-ttu-id="7084a-264">Aby zaimplementować `IEmailSender`, Utwórz *usługi/EmailSender. cs* z kodem podobnym do poniższego:</span><span class="sxs-lookup"><span data-stu-id="7084a-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="7084a-265">Konfigurowanie uruchamiania do obsługi poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-265">Configure startup to support email</span></span>

<span data-ttu-id="7084a-266">Dodaj następujący kod do metody `ConfigureServices` w pliku *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="7084a-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="7084a-267">Dodaj `EmailSender` jako usługę przejściową.</span><span class="sxs-lookup"><span data-stu-id="7084a-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="7084a-268">Zarejestruj wystąpienie konfiguracji `AuthMessageSenderOptions`.</span><span class="sxs-lookup"><span data-stu-id="7084a-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="7084a-269">Włącz potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="7084a-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="7084a-270">Szablon zawiera kod służący do potwierdzenia konta i odzyskiwania hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="7084a-271">Znajdź metodę `OnPostAsync` w *obszarach/Identity/Pages/Account/register. cshtml. cs*.</span><span class="sxs-lookup"><span data-stu-id="7084a-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="7084a-272">Zablokuj automatyczne logowanie nowo zarejestrowanych użytkowników, dodając komentarz do następującego wiersza:</span><span class="sxs-lookup"><span data-stu-id="7084a-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="7084a-273">Pełna metoda jest pokazywana z wyróżnioną zmienionym wierszem:</span><span class="sxs-lookup"><span data-stu-id="7084a-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="7084a-274">Rejestrowanie, Potwierdzanie poczty e-mail i resetowanie hasła</span><span class="sxs-lookup"><span data-stu-id="7084a-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="7084a-275">Uruchom aplikację internetową, a następnie przetestuj potwierdzenie konta i przepływ odzyskiwania hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="7084a-276">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="7084a-276">Run the app and register a new user</span></span>
* <span data-ttu-id="7084a-277">Sprawdź wiadomość e-mail z linkiem potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="7084a-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="7084a-278">Jeśli nie otrzymasz wiadomości e-mail, zobacz [debugowanie poczty e-mail](#debug) .</span><span class="sxs-lookup"><span data-stu-id="7084a-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="7084a-279">Kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="7084a-280">Zaloguj się przy użyciu adresu e-mail i hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="7084a-281">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="7084a-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="7084a-282">Wyświetlanie strony zarządzania</span><span class="sxs-lookup"><span data-stu-id="7084a-282">View the manage page</span></span>

<span data-ttu-id="7084a-283">Wybierz swoją nazwę użytkownika w przeglądarce: ![okno przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="7084a-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="7084a-284">Zostanie wyświetlona strona zarządzanie z wybraną kartą **profil** .</span><span class="sxs-lookup"><span data-stu-id="7084a-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="7084a-285">**Wiadomość e-mail** zawiera pole wyboru informujące o potwierdzeniu wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="7084a-286">Resetowanie hasła do testu</span><span class="sxs-lookup"><span data-stu-id="7084a-286">Test password reset</span></span>

* <span data-ttu-id="7084a-287">Jeśli użytkownik jest zalogowany, wybierz pozycję **Wyloguj**.</span><span class="sxs-lookup"><span data-stu-id="7084a-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="7084a-288">Wybierz link **Zaloguj** i wybierz łącze nie **pamiętasz hasła?**</span><span class="sxs-lookup"><span data-stu-id="7084a-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="7084a-289">Wprowadź adres e-mail, który został użyty do zarejestrowania konta.</span><span class="sxs-lookup"><span data-stu-id="7084a-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="7084a-290">Zostanie wysłana wiadomość e-mail z linkiem służącym do resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="7084a-291">Sprawdź swój adres e-mail i kliknij link, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="7084a-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="7084a-292">Po pomyślnym zresetowaniu hasła możesz zalogować się przy użyciu adresu e-mail i nowego hasła.</span><span class="sxs-lookup"><span data-stu-id="7084a-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="7084a-293">Zmień limit czasu wiadomości e-mail i aktywności</span><span class="sxs-lookup"><span data-stu-id="7084a-293">Change email and activity timeout</span></span>

<span data-ttu-id="7084a-294">Domyślny limit czasu braku aktywności wynosi 14 dni.</span><span class="sxs-lookup"><span data-stu-id="7084a-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="7084a-295">Następujący kod ustawia limit czasu braku aktywności na 5 dni:</span><span class="sxs-lookup"><span data-stu-id="7084a-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="7084a-296">Zmień wszystkie lifespans tokenów ochrony danych</span><span class="sxs-lookup"><span data-stu-id="7084a-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="7084a-297">Następujący kod zmienia limit czasu tokenów ochrony danych na 3 godziny:</span><span class="sxs-lookup"><span data-stu-id="7084a-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="7084a-298">Wbudowane tokeny użytkownika tożsamości (zobacz [AspNetCore/src/Identity/Extensions. Core/src/TokenOptions. cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) mają [jeden dzień limitu czasu](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7084a-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="7084a-299">Zmień cykl życia tokenu poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-299">Change the email token lifespan</span></span>

<span data-ttu-id="7084a-300">Domyślny token cykl życia tokenów [użytkownika tożsamości](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) to [jeden dzień](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7084a-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="7084a-301">W tej sekcji przedstawiono sposób zmiany cykl życia tokenu poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="7084a-302">Dodaj niestandardową [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) i <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="7084a-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="7084a-303">Dodaj dostawcę niestandardowego do kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="7084a-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="7084a-304">Potwierdzenie ponownego wysłania wiadomości e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-304">Resend email confirmation</span></span>

<span data-ttu-id="7084a-305">Zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/5410)w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="7084a-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="7084a-306">Debuguj wiadomość e-mail</span><span class="sxs-lookup"><span data-stu-id="7084a-306">Debug email</span></span>

<span data-ttu-id="7084a-307">Jeśli nie możesz uzyskać pracy z wiadomościami e-mail:</span><span class="sxs-lookup"><span data-stu-id="7084a-307">If you can't get email working:</span></span>

* <span data-ttu-id="7084a-308">Ustaw punkt przerwania w `EmailSender.Execute`, aby sprawdzić, czy `SendGridClient.SendEmailAsync` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="7084a-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="7084a-309">Utwórz [aplikację konsolową do wysyłania wiadomości e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) przy użyciu podobnego kodu do `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="7084a-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="7084a-310">Przejrzyj stronę [działania e-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) .</span><span class="sxs-lookup"><span data-stu-id="7084a-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="7084a-311">Sprawdź swój folder spamu.</span><span class="sxs-lookup"><span data-stu-id="7084a-311">Check your spam folder.</span></span>
* <span data-ttu-id="7084a-312">Wypróbuj inny alias poczty e-mail na innym dostawcy poczty e-mail (Microsoft, Yahoo, Gmail itp.)</span><span class="sxs-lookup"><span data-stu-id="7084a-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="7084a-313">Spróbuj wysłać konto do innych kont e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="7084a-314">**Najlepszym rozwiązaniem** w zakresie zabezpieczeń jest **nieużywanie produkcyjnych** wpisów tajnych w testowaniu i programowaniu.</span><span class="sxs-lookup"><span data-stu-id="7084a-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="7084a-315">Jeśli opublikujesz aplikację na platformie Azure, możesz ustawić wpisy tajne SendGrid jako ustawienia aplikacji w portalu aplikacji sieci Web platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="7084a-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="7084a-316">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="7084a-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7084a-317">Łączenie kont logowania społecznościowych i lokalnych</span><span class="sxs-lookup"><span data-stu-id="7084a-317">Combine social and local login accounts</span></span>

<span data-ttu-id="7084a-318">Aby ukończyć tę sekcję, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7084a-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="7084a-319">Zobacz [uwierzytelnianie w usługach Facebook, Google i dostawcy zewnętrznym](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7084a-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="7084a-320">Konta lokalne i społecznościowe można łączyć przez kliknięcie linku do poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7084a-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7084a-321">W następującej kolejności "RickAndMSFT@gmail.com" jest najpierw tworzony jako logowanie lokalne; można jednak najpierw utworzyć konto jako nazwę logowania w sieci społecznościowej, a następnie dodać lokalną nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="7084a-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="7084a-323">Kliknij link **Zarządzaj** .</span><span class="sxs-lookup"><span data-stu-id="7084a-323">Click on the **Manage** link.</span></span> <span data-ttu-id="7084a-324">Zwróć uwagę na wartość 0 External (logowanie społeczne) skojarzoną z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="7084a-324">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzaj widokiem](accconfirm/_static/manage.png)

<span data-ttu-id="7084a-326">Kliknij link do innej usługi logowania i zaakceptuj żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7084a-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="7084a-327">Na poniższym obrazie serwis Facebook jest dostawcą zewnętrznym uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="7084a-327">In the following image, Facebook is the external authentication provider:</span></span>

![Zarządzanie widokiem logowania zewnętrznego w serwisie Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="7084a-329">Te dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="7084a-329">The two accounts have been combined.</span></span> <span data-ttu-id="7084a-330">Możesz zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="7084a-330">You are able to sign in with either account.</span></span> <span data-ttu-id="7084a-331">Możesz chcieć, aby użytkownicy mogli dodawać konta lokalne w przypadku, gdy usługa uwierzytelniania przy logowaniu w sieci społecznościowej nie działa, lub prawdopodobnie utraciła dostęp do konta społecznościowego.</span><span class="sxs-lookup"><span data-stu-id="7084a-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="7084a-332">Włącz potwierdzenie konta po stronie, w której znajdują się użytkownicy</span><span class="sxs-lookup"><span data-stu-id="7084a-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="7084a-333">Włączenie potwierdzenia konta w witrynie z użytkownikami powoduje zablokowanie wszystkich istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7084a-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="7084a-334">Istniejący użytkownicy są Zablokowani, ponieważ ich konta nie zostały potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="7084a-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="7084a-335">Aby obejść istniejącą blokadę użytkownika, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="7084a-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="7084a-336">Zaktualizuj bazę danych, aby oznaczyć wszystkich istniejących użytkowników jako potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="7084a-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="7084a-337">Potwierdź istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7084a-337">Confirm existing users.</span></span> <span data-ttu-id="7084a-338">Na przykład wysyłanie wsadowe wiadomości e-mail z linkami potwierdzającymi.</span><span class="sxs-lookup"><span data-stu-id="7084a-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
