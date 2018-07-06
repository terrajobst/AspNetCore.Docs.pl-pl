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
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="c12ac-103">Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c12ac-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="c12ac-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c12ac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="c12ac-105">W tym samouczku przedstawiono sposób kompilowania aplikacji platformy ASP.NET Core za pomocą poczty e-mail potwierdzenia i resetowaniem hasła.</span><span class="sxs-lookup"><span data-stu-id="c12ac-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="c12ac-106">Niniejszy samouczek jest **nie** początku tematu.</span><span class="sxs-lookup"><span data-stu-id="c12ac-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="c12ac-107">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="c12ac-107">You should be familiar with:</span></span>

* [<span data-ttu-id="c12ac-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c12ac-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c12ac-109">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="c12ac-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="c12ac-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c12ac-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="c12ac-111">Zobacz [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC 1.1 i 2.x.</span><span class="sxs-lookup"><span data-stu-id="c12ac-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c12ac-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c12ac-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="c12ac-113">Utwórz nowy projekt ASP.NET Core przy użyciu interfejsu wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="c12ac-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c12ac-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

* <span data-ttu-id="c12ac-115">`--auth Individual` Określa szablon projektu indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c12ac-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="c12ac-116">Windows, Dodaj `-uld` opcji.</span><span class="sxs-lookup"><span data-stu-id="c12ac-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="c12ac-117">Określa, że LocalDB powinny być używane zamiast bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="c12ac-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="c12ac-118">Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="c12ac-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c12ac-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c12ac-120">Jeśli używasz interfejsu wiersza polecenia lub bazy danych SQLite, uruchom następujące polecenie w oknie polecenia:</span><span class="sxs-lookup"><span data-stu-id="c12ac-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="c12ac-121">`--auth Individual` Określa szablon projektu indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c12ac-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="c12ac-122">Windows, Dodaj `-uld` opcji.</span><span class="sxs-lookup"><span data-stu-id="c12ac-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="c12ac-123">Określa, że LocalDB powinny być używane zamiast bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="c12ac-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="c12ac-124">Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="c12ac-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="c12ac-125">Alternatywnie można utworzyć nowy projekt ASP.NET Core z programem Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c12ac-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="c12ac-126">W programie Visual Studio Utwórz nowy **aplikacji sieci Web** projektu.</span><span class="sxs-lookup"><span data-stu-id="c12ac-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="c12ac-127">Wybierz **platformy ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="c12ac-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="c12ac-128">**.NET core** jest zaznaczona na poniższej ilustracji, ale można wybrać **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="c12ac-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="c12ac-129">Wybierz **Zmień uwierzytelnianie** i ustaw **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="c12ac-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="c12ac-130">Zachowaj wartość domyślną **Store użytkownika konta w aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="c12ac-130">Keep the default **Store user accounts in-app**.</span></span>

![Okno dialogowe nowego projektu przedstawiający "Indywidualne konta użytkowników radio" wybrana](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="c12ac-132">Rejestrowanie nowego użytkownika testowego</span><span class="sxs-lookup"><span data-stu-id="c12ac-132">Test new user registration</span></span>

<span data-ttu-id="c12ac-133">Uruchom aplikację, wybierz **zarejestrować** łącze, a następnie zarejestrować użytkownik.</span><span class="sxs-lookup"><span data-stu-id="c12ac-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="c12ac-134">Postępuj zgodnie z instrukcjami, aby uruchomić migracje platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c12ac-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="c12ac-135">W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c12ac-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="c12ac-136">Po przesłaniu rejestracji, użytkownik jest zalogowany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c12ac-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="c12ac-137">W dalszej części samouczka kod jest aktualizowana, dzięki czemu nowi użytkownicy nie mogą zalogować, dopóki nie został zweryfikowany poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="c12ac-138">Widok bazy danych tożsamości</span><span class="sxs-lookup"><span data-stu-id="c12ac-138">View the Identity database</span></span>

<span data-ttu-id="c12ac-139">Zobacz [Praca z SQLite w projektach programu ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) instrukcje dotyczące sposobu wyświetlania bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="c12ac-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="c12ac-140">Dla programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c12ac-140">For Visual Studio:</span></span>

* <span data-ttu-id="c12ac-141">Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="c12ac-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="c12ac-142">Przejdź do **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="c12ac-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="c12ac-143">Kliknij prawym przyciskiem myszy **dbo. AspNetUsers** > **wyświetlania danych**:</span><span class="sxs-lookup"><span data-stu-id="c12ac-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu kontekstowe tabeli AspNetUsers w Eksploratorze obiektów SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="c12ac-145">Należy pamiętać, tabeli `EmailConfirmed` pole jest `False`.</span><span class="sxs-lookup"><span data-stu-id="c12ac-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="c12ac-146">Można używać tego adresu e-mail ponownie w następnym kroku, gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="c12ac-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="c12ac-147">Kliknij prawym przyciskiem myszy na wiersz i wybierz **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="c12ac-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="c12ac-148">Usuwanie aliasu adresu e-mail ułatwia w poniższych krokach.</span><span class="sxs-lookup"><span data-stu-id="c12ac-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="c12ac-149">Wymaganie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="c12ac-149">Require HTTPS</span></span>

<span data-ttu-id="c12ac-150">Zobacz [wymagania protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="c12ac-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="c12ac-151">Wymagaj potwierdzenie adresu e-mail</span><span class="sxs-lookup"><span data-stu-id="c12ac-151">Require email confirmation</span></span>

<span data-ttu-id="c12ac-152">Jest najlepszym rozwiązaniem, aby potwierdzić adres e-mail rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c12ac-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="c12ac-153">Wiadomość e-mail potwierdzenia pomaga sprawdzić, mogą one nie personifikuje kogoś innego (oznacza to one nie zostały zarejestrowane przy użyciu adresu e-mail osoby).</span><span class="sxs-lookup"><span data-stu-id="c12ac-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="c12ac-154">Załóżmy, że trzeba było forum dyskusyjnego i chcesz uniemożliwić "yli@example.com"z rejestracją jako"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="c12ac-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="c12ac-155">Bez potwierdzenia e-mail "nolivetto@contoso.com" może otrzymywać niechciane wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c12ac-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="c12ac-156">Załóżmy, że użytkownik przypadkowo zarejestrowana jako "ylo@example.com" i nie zostało już słowa "yli".</span><span class="sxs-lookup"><span data-stu-id="c12ac-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="c12ac-157">W takich sytuacjach przydałaby się mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="c12ac-158">Potwierdzenie adresu e-mail zawiera tylko ograniczoną ochronę, od botów.</span><span class="sxs-lookup"><span data-stu-id="c12ac-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="c12ac-159">Potwierdzenie adresu e-mail nie zapewniają ochronę przed złośliwymi użytkownikami z wieloma kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="c12ac-160">Zazwyczaj chcesz uniemożliwić nowym użytkownikom publikowanie żadnych danych do witryny sieci web, zanim potwierdzone pocztą e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="c12ac-161">Aktualizacja `ConfigureServices` będą musieli potwierdzony adres e-mail:</span><span class="sxs-lookup"><span data-stu-id="c12ac-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="c12ac-162">`config.SignIn.RequireConfirmedEmail = true;` zarejestrowani użytkownicy uniemożliwia logowanie do momentu ich adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="c12ac-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="c12ac-163">Konfigurowanie dostawcy poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="c12ac-163">Configure email provider</span></span>

<span data-ttu-id="c12ac-164">W tym samouczku SendGrid umożliwia wysyłanie wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="c12ac-165">Potrzebujesz konta SendGrid i klucz do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="c12ac-166">Można użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-166">You can use other email providers.</span></span> <span data-ttu-id="c12ac-167">Platforma ASP.NET Core 2.x obejmuje `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c12ac-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="c12ac-168">Zaleca się, że używasz usługi SendGrid lub innej usługi poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="c12ac-169">SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="c12ac-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="c12ac-170">[Wzorzec opcje](xref:fundamentals/configuration/options) umożliwia dostęp do ustawień konta i klucza użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c12ac-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="c12ac-171">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c12ac-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="c12ac-172">Utwórz klasę, można pobrać klucza zabezpieczanie poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="c12ac-173">W tym przykładzie `AuthMessageSenderOptions` klasa jest tworzona w *Services/AuthMessageSenderOptions.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="c12ac-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="c12ac-174">Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c12ac-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="c12ac-175">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c12ac-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="c12ac-176">Na Windows, klucza tajnego Manager przechowuje par kluczy i wartości w *secrets.json* w pliku `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="c12ac-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="c12ac-177">Zawartość *secrets.json* pliku nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="c12ac-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="c12ac-178">*Secrets.json* plików znajdują się poniżej ( `SendGridKey` wartość została usunięta.)</span><span class="sxs-lookup"><span data-stu-id="c12ac-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="c12ac-179">Konfigurowanie uruchamiania, aby użyć AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="c12ac-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="c12ac-180">Dodaj `AuthMessageSenderOptions` do kontenera usługi na końcu `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="c12ac-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c12ac-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c12ac-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="c12ac-183">Konfigurowanie klasy AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="c12ac-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="c12ac-184">W tym samouczku przedstawiono sposób dodawania powiadomienia e-mail za pośrednictwem [SendGrid](https://sendgrid.com/), ale możesz wysłać wiadomość e-mail przy użyciu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="c12ac-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="c12ac-185">Zainstaluj `SendGrid` pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="c12ac-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="c12ac-186">W wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="c12ac-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="c12ac-187">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c12ac-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="c12ac-188">Zobacz [bezpłatnie Rozpocznij pracę za pomocą usługi SendGrid](https://sendgrid.com/free/) zarejestrować o utworzenie bezpłatnego konta SendGrid.</span><span class="sxs-lookup"><span data-stu-id="c12ac-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="c12ac-189">Konfigurowanie usługi SendGrid</span><span class="sxs-lookup"><span data-stu-id="c12ac-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c12ac-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="c12ac-191">Aby skonfigurować usługi SendGrid, Dodaj kod, które są podobne do następującego *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="c12ac-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c12ac-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="c12ac-193">Dodaj kod w *Services/MessageServices.cs* podobne do następujących czynności, aby skonfigurować usługi SendGrid:</span><span class="sxs-lookup"><span data-stu-id="c12ac-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="c12ac-194">Włącz odzyskiwanie potwierdzenia i hasło konta</span><span class="sxs-lookup"><span data-stu-id="c12ac-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="c12ac-195">Szablon zawiera kod odzyskiwania potwierdzenia i hasło konta.</span><span class="sxs-lookup"><span data-stu-id="c12ac-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="c12ac-196">Znajdź `OnPostAsync` method in Class metoda *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="c12ac-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c12ac-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="c12ac-198">Uniemożliwić nowym użytkownikom zalogowanie się automatycznie, zakomentowując następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="c12ac-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c12ac-199">Za pomocą zmieniony wiersz wyróżniony pokazano całą metodę:</span><span class="sxs-lookup"><span data-stu-id="c12ac-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c12ac-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="c12ac-201">Aby włączyć potwierdzenie konta, usuń znaczniki komentarza z następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="c12ac-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="c12ac-202">**Uwaga:** kodu uniemożliwia nowo zarejestrowanego użytkownika z logowaniem się automatycznie, zakomentowując następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="c12ac-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c12ac-203">Włącz odzyskiwanie hasła przez kod w uncommenting `ForgotPassword` akcji *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="c12ac-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="c12ac-204">Usuń znaczniki komentarza element formularza w *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c12ac-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="c12ac-205">Możesz chcieć usunąć `<p> For more information on how to enable reset password ... </p>` element, który zawiera łącze do tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="c12ac-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="c12ac-206">Rejestrowania i resetowania hasła oraz Potwierdź adres e-mail</span><span class="sxs-lookup"><span data-stu-id="c12ac-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="c12ac-207">Uruchamianie aplikacji sieci web i przetestować potwierdzenie konta i hasła odzyskiwania przepływu.</span><span class="sxs-lookup"><span data-stu-id="c12ac-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="c12ac-208">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="c12ac-208">Run the app and register a new user</span></span>

  ![Widok zarejestrować konto aplikacji sieci Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="c12ac-210">Sprawdź pocztę e-mail, aby uzyskać link do potwierdzenia konta.</span><span class="sxs-lookup"><span data-stu-id="c12ac-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="c12ac-211">Zobacz [debugowania e-mail](#debug) otrzymasz wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="c12ac-212">Kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="c12ac-213">Zaloguj się przy użyciu poczty e-mail i hasło.</span><span class="sxs-lookup"><span data-stu-id="c12ac-213">Log in with your email and password.</span></span>
* <span data-ttu-id="c12ac-214">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="c12ac-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="c12ac-215">Wyświetlanie strony zarządzania</span><span class="sxs-lookup"><span data-stu-id="c12ac-215">View the manage page</span></span>

<span data-ttu-id="c12ac-216">Wybierz swoją nazwę użytkownika w przeglądarce: ![okno przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="c12ac-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="c12ac-217">Może być konieczne Rozwiń pasek nawigacyjny, aby wyświetlić nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c12ac-217">You might need to expand the navbar to see user name.</span></span>

![pasek nawigacyjny](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c12ac-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c12ac-220">Zostanie wyświetlona strona zarządzania, z **profilu** wybraną kartą.</span><span class="sxs-lookup"><span data-stu-id="c12ac-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="c12ac-221">**E-mail** przedstawia pole wyboru, wskazujący adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="c12ac-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![Zarządzanie stroną](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c12ac-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c12ac-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c12ac-224">Jest to opisane w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c12ac-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="c12ac-225">![Strona Zarządzanie](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="c12ac-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="c12ac-226">Resetowanie hasła testu</span><span class="sxs-lookup"><span data-stu-id="c12ac-226">Test password reset</span></span>

* <span data-ttu-id="c12ac-227">Jeśli zalogowano Cię, wybierz opcję **wylogowania**.</span><span class="sxs-lookup"><span data-stu-id="c12ac-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="c12ac-228">Wybierz **Zaloguj** łącze, a następnie wybierz pozycję **nie pamiętasz hasła?** łącza.</span><span class="sxs-lookup"><span data-stu-id="c12ac-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="c12ac-229">Wprowadź adres e-mail, którego użyłeś do zarejestrować konto.</span><span class="sxs-lookup"><span data-stu-id="c12ac-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="c12ac-230">Wiadomość e-mail z linkiem do zresetowania hasła są wysyłane.</span><span class="sxs-lookup"><span data-stu-id="c12ac-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="c12ac-231">Sprawdź pocztę e-mail, a następnie kliknij link, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="c12ac-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="c12ac-232">Po pomyślnie zresetowano hasło można rejestrować się za pomocą poczty e-mail i nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="c12ac-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="c12ac-233">Debugowanie poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="c12ac-233">Debug email</span></span>

<span data-ttu-id="c12ac-234">Jeśli nie można rozpocząć pracę poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="c12ac-234">If you can't get email working:</span></span>

* <span data-ttu-id="c12ac-235">Tworzenie [aplikacji konsoli, aby wysłać wiadomość e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="c12ac-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="c12ac-236">Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.</span><span class="sxs-lookup"><span data-stu-id="c12ac-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="c12ac-237">Sprawdź folder wiadomości-śmieci.</span><span class="sxs-lookup"><span data-stu-id="c12ac-237">Check your spam folder.</span></span>
* <span data-ttu-id="c12ac-238">Spróbuj inny alias poczty e-mail na innego dostawcy poczty e-mail (Microsoft Yahoo, Gmail, itp.)</span><span class="sxs-lookup"><span data-stu-id="c12ac-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="c12ac-239">Spróbuj wysłać do różnymi kontami e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="c12ac-240">**Ze względów bezpieczeństwa** jest **nie** używania wpisów tajnych produkcyjnych, w projektowania i testowania.</span><span class="sxs-lookup"><span data-stu-id="c12ac-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="c12ac-241">Jeśli opublikujesz aplikację na platformie Azure, możesz ustawić wpisy tajne usługi SendGrid, jako ustawienia aplikacji w portalu usługi Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="c12ac-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="c12ac-242">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="c12ac-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c12ac-243">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="c12ac-243">Combine social and local login accounts</span></span>

<span data-ttu-id="c12ac-244">Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c12ac-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="c12ac-245">Zobacz [Facebook, Google i uwierzytelniania zewnętrznego dostawcy](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c12ac-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="c12ac-246">Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c12ac-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c12ac-247">W następującej kolejności "RickAndMSFT@gmail.com" najpierw jest tworzony jako lokalny identyfikator logowania; jednak możesz najpierw utworzyć konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="c12ac-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="c12ac-249">Kliknij pozycję **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="c12ac-249">Click on the **Manage** link.</span></span> <span data-ttu-id="c12ac-250">Należy pamiętać, zewnętrzne 0 (logowania społecznościowego) skojarzony z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="c12ac-250">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzanie widoku](accconfirm/_static/manage.png)

<span data-ttu-id="c12ac-252">Kliknij link do innej usługi, zaloguj się i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c12ac-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="c12ac-253">Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznych:</span><span class="sxs-lookup"><span data-stu-id="c12ac-253">In the following image, Facebook is the external authentication provider:</span></span>

![Zarządzanie wyświetlania listy Facebooku widoku logowań zewnętrznych](accconfirm/_static/fb.png)

<span data-ttu-id="c12ac-255">Te dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="c12ac-255">The two accounts have been combined.</span></span> <span data-ttu-id="c12ac-256">Możesz się zalogować przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="c12ac-256">You are able to log on with either account.</span></span> <span data-ttu-id="c12ac-257">Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania uwierzytelniania usługa nie działa lub większe prawdopodobieństwo ich utraty dostępu do swojego konta w sieci społecznościowej.</span><span class="sxs-lookup"><span data-stu-id="c12ac-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="c12ac-258">Włącz potwierdzenie konta, po lokacji ma użytkowników</span><span class="sxs-lookup"><span data-stu-id="c12ac-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="c12ac-259">Włączanie potwierdzenie konta w witrynie użytkownikom blokuje istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c12ac-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="c12ac-260">Istniejący użytkownicy są zablokowane, ponieważ ich konta nie są potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="c12ac-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="c12ac-261">Aby obejść istniejące blokady użytkownika, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="c12ac-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="c12ac-262">Aktualizuj bazę danych, aby oznaczyć wszyscy istniejący użytkownicy, co zostało potwierdzone.</span><span class="sxs-lookup"><span data-stu-id="c12ac-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="c12ac-263">Upewnij się, użytkownicy istniejącej.</span><span class="sxs-lookup"><span data-stu-id="c12ac-263">Confirm exiting users.</span></span> <span data-ttu-id="c12ac-264">Na przykład usługi batch — wysyłanie wiadomości e-mail przy użyciu linki do potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="c12ac-264">For example, batch-send emails with confirmation links.</span></span>
