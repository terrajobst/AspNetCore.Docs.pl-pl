---
title: Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.
ms.author: riande
ms.date: 2/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 77d7b209d57f9ee44f158798ff780ce85c87aaf2
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56159411"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="79a01-103">Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79a01-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="79a01-104">Zobacz [plik PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) dla platformy ASP.NET Core 1.1 i wersja 2.1.</span><span class="sxs-lookup"><span data-stu-id="79a01-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="79a01-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="79a01-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="79a01-106">W tym samouczku przedstawiono sposób kompilowania aplikacji platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.</span><span class="sxs-lookup"><span data-stu-id="79a01-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="79a01-107">Niniejszy samouczek jest **nie** początku tematu.</span><span class="sxs-lookup"><span data-stu-id="79a01-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="79a01-108">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="79a01-108">You should be familiar with:</span></span>

* [<span data-ttu-id="79a01-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79a01-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="79a01-110">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="79a01-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="79a01-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="79a01-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="79a01-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="79a01-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="79a01-113">Tworzenie aplikacji sieci web i tworzenia szkieletu tożsamości</span><span class="sxs-lookup"><span data-stu-id="79a01-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79a01-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79a01-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="79a01-115">W programie Visual Studio Utwórz nowy **aplikacji sieci Web** projektu o nazwie **WebPWrecover**.</span><span class="sxs-lookup"><span data-stu-id="79a01-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="79a01-116">Select **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="79a01-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="79a01-117">Zachowaj wartość domyślną **uwierzytelniania** równa **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="79a01-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="79a01-118">Uwierzytelnianie jest dodawany w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="79a01-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="79a01-119">W następnym kroku:</span><span class="sxs-lookup"><span data-stu-id="79a01-119">In the next step:</span></span>

* <span data-ttu-id="79a01-120">Ustaw układ strony *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="79a01-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="79a01-121">Wybierz *konta/rejestrowanie*</span><span class="sxs-lookup"><span data-stu-id="79a01-121">Select *Account/Register*</span></span>
* <span data-ttu-id="79a01-122">Utwórz nową **klasa kontekstu danych**</span><span class="sxs-lookup"><span data-stu-id="79a01-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="79a01-123">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="79a01-123">.NET Core CLI</span></span>](#tab/netcore-cli)

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

<span data-ttu-id="79a01-124">Uruchom `dotnet aspnet-codegenerator identity --help` Aby uzyskać pomoc na temat narzędzia do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="79a01-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="79a01-125">Postępuj zgodnie z instrukcjami w [Włącz uwierzytelnianie](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="79a01-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="79a01-126">Dodaj `app.UseAuthentication();` do `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="79a01-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="79a01-127">Dodaj `<partial name="_LoginPartial" />` do pliku układu.</span><span class="sxs-lookup"><span data-stu-id="79a01-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="79a01-128">Rejestrowanie nowego użytkownika testowego</span><span class="sxs-lookup"><span data-stu-id="79a01-128">Test new user registration</span></span>

<span data-ttu-id="79a01-129">Uruchom aplikację, wybierz **zarejestrować** łącze, a następnie zarejestrować użytkownik.</span><span class="sxs-lookup"><span data-stu-id="79a01-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="79a01-130">W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="79a01-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="79a01-131">Po przesłaniu rejestracji, użytkownik jest zalogowany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79a01-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="79a01-132">W dalszej części samouczka kod jest aktualizowana, aby nowi użytkownicy nie może się zalogować, dopóki nie zostanie zweryfikowana poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-132">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="79a01-133">Należy pamiętać, tabeli `EmailConfirmed` pole jest `False`.</span><span class="sxs-lookup"><span data-stu-id="79a01-133">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="79a01-134">Można używać tego adresu e-mail ponownie w następnym kroku, gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="79a01-134">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="79a01-135">Kliknij prawym przyciskiem myszy na wiersz i wybierz **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="79a01-135">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="79a01-136">Usuwanie aliasu adresu e-mail ułatwia w poniższych krokach.</span><span class="sxs-lookup"><span data-stu-id="79a01-136">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="79a01-137">Wymagaj potwierdzenie adresu e-mail</span><span class="sxs-lookup"><span data-stu-id="79a01-137">Require email confirmation</span></span>

<span data-ttu-id="79a01-138">Jest najlepszym rozwiązaniem, aby potwierdzić adres e-mail rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="79a01-138">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="79a01-139">Wiadomość e-mail potwierdzenia pomaga sprawdzić, mogą one nie personifikuje kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby).</span><span class="sxs-lookup"><span data-stu-id="79a01-139">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="79a01-140">Załóżmy, że trzeba było forum dyskusyjnego i chcesz uniemożliwić "yli@example.com"z rejestracją jako"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="79a01-140">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="79a01-141">Bez potwierdzenia e-mail "nolivetto@contoso.com" może otrzymywać niechciane wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79a01-141">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="79a01-142">Załóżmy, że użytkownik przypadkowo zarejestrowana jako "ylo@example.com" i nie zostało już słowa "yli".</span><span class="sxs-lookup"><span data-stu-id="79a01-142">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="79a01-143">W takich sytuacjach przydałaby się mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-143">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="79a01-144">Potwierdzenie adresu e-mail zapewnia ograniczoną ochronę, od botów.</span><span class="sxs-lookup"><span data-stu-id="79a01-144">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="79a01-145">Potwierdzenie adresu e-mail nie zapewniają ochronę przed złośliwymi użytkownikami z wieloma kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-145">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="79a01-146">Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do witryny sieci web, zanim potwierdzone pocztą e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-146">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="79a01-147">Aktualizacja *Areas/Identity/IdentityHostingStartup.cs* będą musieli potwierdzony adres e-mail:</span><span class="sxs-lookup"><span data-stu-id="79a01-147">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="79a01-148">`config.SignIn.RequireConfirmedEmail = true;` zarejestrowani użytkownicy uniemożliwia logowanie do momentu ich adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="79a01-148">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="79a01-149">Konfigurowanie dostawcy poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="79a01-149">Configure email provider</span></span>

<span data-ttu-id="79a01-150">W tym samouczku [SendGrid](https://sendgrid.com) służy do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-150">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="79a01-151">Potrzebujesz konta SendGrid i klucz do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-151">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="79a01-152">Można użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-152">You can use other email providers.</span></span> <span data-ttu-id="79a01-153">Platforma ASP.NET Core 2.x obejmuje `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79a01-153">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="79a01-154">Zaleca się, że używasz usługi SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-154">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="79a01-155">SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="79a01-155">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="79a01-156">Utwórz klasę, można pobrać klucza zabezpieczanie poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-156">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="79a01-157">W tym przykładzie należy utworzyć *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="79a01-157">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="79a01-158">Konfigurowanie wpisami tajnymi użytkowników usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="79a01-158">Configure SendGrid user secrets</span></span>

<span data-ttu-id="79a01-159">Dodaj unikatowy `<UserSecretsId>` wartość `<PropertyGroup>` elementu w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="79a01-159">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="79a01-160">Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="79a01-160">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="79a01-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="79a01-161">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="79a01-162">Na Windows, klucza tajnego Manager przechowuje par kluczy i wartości w *secrets.json* w pliku `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="79a01-162">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="79a01-163">Zawartość *secrets.json* pliku nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="79a01-163">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="79a01-164">*Secrets.json* plików znajdują się poniżej ( `SendGridKey` wartość została usunięta.)</span><span class="sxs-lookup"><span data-stu-id="79a01-164">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
<span data-ttu-id="79a01-165">Aby uzyskać więcej informacji, zobacz [wzorzec opcje](xref:fundamentals/configuration/options) i [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="79a01-165">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="79a01-166">Instalowanie usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="79a01-166">Install SendGrid</span></span>

<span data-ttu-id="79a01-167">W tym samouczku przedstawiono sposób dodawania powiadomienia e-mail za pośrednictwem [SendGrid](https://sendgrid.com/), ale możesz wysłać wiadomość e-mail przy użyciu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="79a01-167">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="79a01-168">Zainstaluj `SendGrid` pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="79a01-168">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79a01-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79a01-169">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="79a01-170">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="79a01-170">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="79a01-171">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="79a01-171">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="79a01-172">Z poziomu konsoli wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="79a01-172">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="79a01-173">Zobacz [bezpłatnie Rozpocznij pracę za pomocą usługi SendGrid](https://sendgrid.com/free/) zarejestrować o utworzenie bezpłatnego konta SendGrid.</span><span class="sxs-lookup"><span data-stu-id="79a01-173">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="79a01-174">Implementowanie IEmailSender</span><span class="sxs-lookup"><span data-stu-id="79a01-174">Implement IEmailSender</span></span>

<span data-ttu-id="79a01-175">Zaimplementowanie `IEmailSender`, Utwórz *Services/EmailSender.cs* kodem podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="79a01-175">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="79a01-176">Konfigurowanie uruchamiania do obsługi poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="79a01-176">Configure startup to support email</span></span>

<span data-ttu-id="79a01-177">Dodaj następujący kod do `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="79a01-177">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="79a01-178">Dodaj `EmailSender` jako przejściowe usługi.</span><span class="sxs-lookup"><span data-stu-id="79a01-178">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="79a01-179">Zarejestruj `AuthMessageSenderOptions` wystąpienia konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="79a01-179">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="79a01-180">Włącz odzyskiwanie potwierdzenia i hasło konta</span><span class="sxs-lookup"><span data-stu-id="79a01-180">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="79a01-181">Szablon zawiera kod odzyskiwania potwierdzenia i hasło konta.</span><span class="sxs-lookup"><span data-stu-id="79a01-181">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="79a01-182">Znajdź `OnPostAsync` method in Class metoda *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="79a01-182">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="79a01-183">Uniemożliwić nowym użytkownikom zalogowanie się automatycznie, zakomentowując następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="79a01-183">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="79a01-184">Za pomocą zmieniony wiersz wyróżniony pokazano całą metodę:</span><span class="sxs-lookup"><span data-stu-id="79a01-184">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="79a01-185">Rejestrowania i resetowania hasła oraz Potwierdź adres e-mail</span><span class="sxs-lookup"><span data-stu-id="79a01-185">Register, confirm email, and reset password</span></span>

<span data-ttu-id="79a01-186">Uruchamianie aplikacji sieci web i przetestować potwierdzenie konta i hasła odzyskiwania przepływu.</span><span class="sxs-lookup"><span data-stu-id="79a01-186">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="79a01-187">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="79a01-187">Run the app and register a new user</span></span>

  ![Widok zarejestrować konto aplikacji sieci Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="79a01-189">Sprawdź pocztę e-mail, aby uzyskać link do potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="79a01-189">Check your email for the account confirmation link.</span></span> <span data-ttu-id="79a01-190">Zobacz [debugowania e-mail](#debug) otrzymasz wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-190">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="79a01-191">Kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-191">Click the link to confirm your email.</span></span>
* <span data-ttu-id="79a01-192">Zaloguj się przy użyciu poczty e-mail i hasło.</span><span class="sxs-lookup"><span data-stu-id="79a01-192">Sign in with your email and password.</span></span>
* <span data-ttu-id="79a01-193">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="79a01-193">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="79a01-194">Wyświetlanie strony zarządzania</span><span class="sxs-lookup"><span data-stu-id="79a01-194">View the manage page</span></span>

<span data-ttu-id="79a01-195">Wybierz swoją nazwę użytkownika w przeglądarce: ![okno przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="79a01-195">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="79a01-196">Może być konieczne Rozwiń pasek nawigacyjny, aby wyświetlić nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="79a01-196">You might need to expand the navbar to see user name.</span></span>

![pasek nawigacyjny](accconfirm/_static/x.png)

<span data-ttu-id="79a01-198">Zostanie wyświetlona strona zarządzania, z **profilu** wybraną kartą.</span><span class="sxs-lookup"><span data-stu-id="79a01-198">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="79a01-199">**E-mail** przedstawia pole wyboru, wskazujący adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="79a01-199">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="79a01-200">Resetowanie hasła testu</span><span class="sxs-lookup"><span data-stu-id="79a01-200">Test password reset</span></span>

* <span data-ttu-id="79a01-201">Jeśli zalogowano Cię, wybierz opcję **wylogowania**.</span><span class="sxs-lookup"><span data-stu-id="79a01-201">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="79a01-202">Wybierz **Zaloguj** łącze, a następnie wybierz pozycję **nie pamiętasz hasła?** łącza.</span><span class="sxs-lookup"><span data-stu-id="79a01-202">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="79a01-203">Wprowadź adres e-mail, którego użyłeś do zarejestrować konto.</span><span class="sxs-lookup"><span data-stu-id="79a01-203">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="79a01-204">Wiadomość e-mail z linkiem do zresetowania hasła są wysyłane.</span><span class="sxs-lookup"><span data-stu-id="79a01-204">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="79a01-205">Sprawdź pocztę e-mail, a następnie kliknij link, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="79a01-205">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="79a01-206">Po pomyślnie zresetowano hasło możesz zarejestrować się przy użyciu poczty e-mail i nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="79a01-206">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="79a01-207">Debugowanie poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="79a01-207">Debug email</span></span>

<span data-ttu-id="79a01-208">Jeśli nie można rozpocząć pracę poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="79a01-208">If you can't get email working:</span></span>

* <span data-ttu-id="79a01-209">Ustaw punkt przerwania `EmailSender.Execute` Aby zweryfikować `SendGridClient.SendEmailAsync` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="79a01-209">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="79a01-210">Tworzenie [aplikacji konsoli, aby wysłać wiadomość e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) przy użyciu kodu podobne do `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="79a01-210">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="79a01-211">Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.</span><span class="sxs-lookup"><span data-stu-id="79a01-211">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="79a01-212">Sprawdź folder wiadomości-śmieci.</span><span class="sxs-lookup"><span data-stu-id="79a01-212">Check your spam folder.</span></span>
* <span data-ttu-id="79a01-213">Spróbuj inny alias poczty e-mail na innego dostawcy poczty e-mail (Microsoft Yahoo, Gmail, itp.)</span><span class="sxs-lookup"><span data-stu-id="79a01-213">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="79a01-214">Spróbuj wysłać do różnymi kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-214">Try sending to different email accounts.</span></span>

<span data-ttu-id="79a01-215">**Ze względów bezpieczeństwa** jest **nie** używania wpisów tajnych produkcyjnych, w projektowania i testowania.</span><span class="sxs-lookup"><span data-stu-id="79a01-215">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="79a01-216">Jeśli opublikujesz aplikację na platformie Azure, możesz ustawić wpisy tajne usługi SendGrid, jako ustawienia aplikacji w portalu usługi Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="79a01-216">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="79a01-217">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="79a01-217">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="79a01-218">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="79a01-218">Combine social and local login accounts</span></span>

<span data-ttu-id="79a01-219">Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="79a01-219">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="79a01-220">Zobacz [Facebook, Google i uwierzytelniania zewnętrznego dostawcy](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="79a01-220">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="79a01-221">Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="79a01-221">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="79a01-222">W następującej kolejności "RickAndMSFT@gmail.com" najpierw jest tworzony jako lokalny identyfikator logowania; jednak możesz najpierw utworzyć konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="79a01-222">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="79a01-224">Kliknij pozycję **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="79a01-224">Click on the **Manage** link.</span></span> <span data-ttu-id="79a01-225">Należy pamiętać, zewnętrzne 0 (logowania społecznościowego) skojarzony z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="79a01-225">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzanie widoku](accconfirm/_static/manage.png)

<span data-ttu-id="79a01-227">Kliknij link do innej usługi, zaloguj się i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79a01-227">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="79a01-228">Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznych:</span><span class="sxs-lookup"><span data-stu-id="79a01-228">In the following image, Facebook is the external authentication provider:</span></span>

![Zarządzanie wyświetlania listy Facebooku widoku logowań zewnętrznych](accconfirm/_static/fb.png)

<span data-ttu-id="79a01-230">Te dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="79a01-230">The two accounts have been combined.</span></span> <span data-ttu-id="79a01-231">Jesteś w stanie zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="79a01-231">You are able to sign in with either account.</span></span> <span data-ttu-id="79a01-232">Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania uwierzytelniania usługa nie działa lub większe prawdopodobieństwo ich utraty dostępu do swojego konta w sieci społecznościowej.</span><span class="sxs-lookup"><span data-stu-id="79a01-232">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="79a01-233">Włącz potwierdzenie konta, po lokacji ma użytkowników</span><span class="sxs-lookup"><span data-stu-id="79a01-233">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="79a01-234">Włączanie potwierdzenie konta w witrynie użytkownikom blokuje istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="79a01-234">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="79a01-235">Istniejący użytkownicy są zablokowane, ponieważ ich konta nie są potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="79a01-235">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="79a01-236">Aby obejść istniejące blokady użytkownika, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="79a01-236">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="79a01-237">Aktualizuj bazę danych, aby oznaczyć wszyscy istniejący użytkownicy, co zostało potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="79a01-237">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="79a01-238">Upewnij się, liczba istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="79a01-238">Confirm existing users.</span></span> <span data-ttu-id="79a01-239">Na przykład usługi batch — wysyłanie wiadomości e-mail przy użyciu linki do potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="79a01-239">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
