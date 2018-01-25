---
title: "Potwierdzenie konta i hasła odzyskiwania w platformy ASP.NET Core"
author: rick-anderson
description: "Przedstawia sposób tworzenia aplikacji platformy ASP.NET Core z funkcją resetowania hasła i potwierdzania poczty e-mail."
ms.author: riande
manager: wpickett
ms.date: 12/1/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: bc9febc41d0637be9f83a02799d360489f257849
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="a5909-103">Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5909-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="a5909-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="a5909-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="a5909-105">W tym samouczku przedstawiono sposób tworzenia aplikacji platformy ASP.NET Core z funkcją resetowania hasła i potwierdzania poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="a5909-106">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5909-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5909-107">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5909-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5909-108">Ten krok dotyczy programu Visual Studio w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="a5909-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="a5909-109">Zobacz następną sekcję, aby uzyskać instrukcje interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="a5909-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="a5909-110">Samouczek wymaga programu Visual Studio 2017 Preview 2 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="a5909-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="a5909-111">W programie Visual Studio Utwórz nowy projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a5909-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="a5909-112">Wybierz **platformy ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="a5909-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="a5909-113">Poniższy obraz Pokaż **.NET Core** zaznaczone, ale można wybrać **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="a5909-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="a5909-114">Wybierz **Zmień uwierzytelnianie** ustawiono **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="a5909-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="a5909-115">Zachowaj ustawienie domyślne **kont magazynu użytkowników w aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="a5909-115">Keep the default **Store user accounts in-app**.</span></span>

![Okno dialogowe nowego projektu przedstawiający "Indywidualne konta użytkowników radio" wybrane](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5909-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5909-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a5909-118">Samouczek wymaga programu Visual Studio 2017 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="a5909-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="a5909-119">W programie Visual Studio Utwórz nowy projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a5909-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="a5909-120">Wybierz **Zmień uwierzytelnianie** ustawiono **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="a5909-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Okno dialogowe nowego projektu przedstawiający "Indywidualne konta użytkowników radio" wybrane](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="a5909-122">Tworzenie projektu interfejsu wiersza polecenia platformy .NET core macOS i Linux</span><span class="sxs-lookup"><span data-stu-id="a5909-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="a5909-123">Jeśli używasz interfejsu wiersza polecenia lub SQLite, uruchom następujące polecenie w oknie poleceń:</span><span class="sxs-lookup"><span data-stu-id="a5909-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="a5909-124">`--auth Individual`Określa szablon indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a5909-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="a5909-125">W systemie Windows, należy dodać `-uld` opcji.</span><span class="sxs-lookup"><span data-stu-id="a5909-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="a5909-126">`-uld` Opcja tworzy ciąg połączenia bazy danych LocalDB, a nie bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="a5909-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="a5909-127">Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="a5909-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="a5909-128">Testowanie nowej rejestracji użytkownika</span><span class="sxs-lookup"><span data-stu-id="a5909-128">Test new user registration</span></span>

<span data-ttu-id="a5909-129">Uruchom aplikację, wybierz **zarejestrować** łącza, a następnie zarejestrować użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a5909-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="a5909-130">Postępuj zgodnie z instrukcjami, aby uruchomić program Entity Framework Core migracji.</span><span class="sxs-lookup"><span data-stu-id="a5909-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="a5909-131">W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a5909-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="a5909-132">Po przesłaniu rejestracji są rejestrowane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5909-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="a5909-133">Później w samouczku zmienimy to aby nowi użytkownicy nie może logować się do momentu sprawdzania poprawności poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="a5909-134">Widok bazy danych tożsamości</span><span class="sxs-lookup"><span data-stu-id="a5909-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="a5909-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a5909-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="a5909-136">Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="a5909-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="a5909-137">Przejdź do **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="a5909-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="a5909-138">Kliknij prawym przyciskiem myszy **dbo. AspNetUsers** > **wyświetlania danych**:</span><span class="sxs-lookup"><span data-stu-id="a5909-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu kontekstowe tabeli AspNetUsers w Eksploratorze obiektów SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="a5909-140">Uwaga `EmailConfirmed` pole jest `False`.</span><span class="sxs-lookup"><span data-stu-id="a5909-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="a5909-141">Można użyć tej wiadomości e-mail ponownie w następnym kroku gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="a5909-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="a5909-142">Kliknij prawym przyciskiem myszy na wiersz i wybierz **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="a5909-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="a5909-143">Usuwanie wiadomości e-mail alias teraz ułatwi w kolejnych krokach.</span><span class="sxs-lookup"><span data-stu-id="a5909-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="a5909-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="a5909-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="a5909-145">Zobacz [pracy z bazy danych SQLite w projekcie platformy ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) instrukcje dotyczące sposobu wyświetlania bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="a5909-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="a5909-146">Wymagaj protokołu SSL i skonfigurować usługi IIS Express do używania protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="a5909-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="a5909-147">Zobacz [wymuszania SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="a5909-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="a5909-148">Wymagaj wiadomości e-mail z potwierdzeniem</span><span class="sxs-lookup"><span data-stu-id="a5909-148">Require email confirmation</span></span>

<span data-ttu-id="a5909-149">Jest najlepszym rozwiązaniem, aby potwierdzić nowej rejestracji użytkownika, aby sprawdzić ich jest nie przeprowadza personifikacji ktoś inny adres e-mail (to znaczy one nie został zarejestrowany przy do kogoś innego adresu e-mail).</span><span class="sxs-lookup"><span data-stu-id="a5909-149">It's a best practice to confirm the email of a new user registration to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="a5909-150">Załóżmy, że masz forum dyskusyjne i chcesz zapobiec "yli@example.com"z rejestracją jako"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="a5909-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="a5909-151">Bez wiadomości e-mail z potwierdzeniem "nolivetto@contoso.com" można uzyskać niechcianych wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5909-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="a5909-152">Załóżmy, że użytkownik przypadkowo zarejestrowany jako "ylo@example.com", a nie wystąpieniem słowa z "yli", nie można używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="a5909-153">Wiadomości e-mail z potwierdzeniem chroni tylko ograniczone z robotów i nie zapewnia ochrony z określone nadawcy, którzy mają wiele aliasów e-mail pracy służące do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a5909-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="a5909-154">Zazwyczaj chcesz uniemożliwić użytkownikom nowe przesyłanie danych do witryny sieci web, zanim potwierdzony adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="a5909-155">Aktualizacja `ConfigureServices` aby wymagać potwierdzenia poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="a5909-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5909-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5909-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5909-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5909-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="a5909-158">Poprzedni wiersz zapobiega zarejestrowanych użytkowników są rejestrowane, do momentu swój adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="a5909-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="a5909-159">Jednak tego wiersza nie uniemożliwić nowych użytkowników po zarejestrowaniu się.</span><span class="sxs-lookup"><span data-stu-id="a5909-159">However, that line doesn't prevent new users from being logged in after they register.</span></span> <span data-ttu-id="a5909-160">Domyślny kod loguje użytkownika, po zarejestrowaniu się.</span><span class="sxs-lookup"><span data-stu-id="a5909-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="a5909-161">Po zalogowaniu będą mogli się zalogować ponownie do czasu ich zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="a5909-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="a5909-162">Później w samouczku zmienimy użytkownika kodu, więc nowo zarejestrowane są **nie** zalogowany.</span><span class="sxs-lookup"><span data-stu-id="a5909-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="a5909-163">Skonfiguruj dostawcę poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="a5909-163">Configure email provider</span></span>

<span data-ttu-id="a5909-164">W tym samouczku SendGrid służy do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="a5909-165">Konieczne jest włączenie konta i klucz do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="a5909-166">Można użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-166">You can use other email providers.</span></span> <span data-ttu-id="a5909-167">Platformy ASP.NET Core zawiera 2.x `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5909-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="a5909-168">Firma Microsoft zaleca się, że używasz SendGrid lub inna usługa poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-168">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="a5909-169">[Wzorzec opcje](xref:fundamentals/configuration/options) umożliwia dostęp do konta i klucz Ustawienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a5909-169">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="a5909-170">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a5909-170">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="a5909-171">Utwórz klasę, aby pobrać klucz zabezpieczanie poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-171">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="a5909-172">Dla tego przykładu `AuthMessageSenderOptions` klasy jest tworzony w *Services/AuthMessageSenderOptions.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="a5909-172">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="a5909-173">Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="a5909-173">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="a5909-174">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a5909-174">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="a5909-175">W systemie Windows, klucz tajny Manager przechowuje z par kluczy i wartości w *secrets.json* pliku w katalogu %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="a5909-175">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="a5909-176">Zawartość *secrets.json* pliku nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="a5909-176">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="a5909-177">*Secrets.json* plików są wyświetlane poniżej ( `SendGridKey` wartości została usunięta.)</span><span class="sxs-lookup"><span data-stu-id="a5909-177">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="a5909-178">Konfigurowanie uruchamiania do użycia AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="a5909-178">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="a5909-179">Dodaj `AuthMessageSenderOptions` w kontenerze usługi na końcu `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="a5909-179">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5909-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5909-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5909-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5909-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="a5909-182">Konfigurowanie klasy AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="a5909-182">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="a5909-183">W tym samouczku przedstawiono sposób dodawania powiadomień pocztą e-mail za pomocą [SendGrid](https://sendgrid.com/), ale mogą wysyłać poczty e-mail przy użyciu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="a5909-183">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="a5909-184">Zainstaluj `SendGrid` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="a5909-184">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="a5909-185">W konsoli Menedżera pakietów, wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a5909-185">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="a5909-186">Zobacz [zacznij pracę bezpłatnie sendgrid](https://sendgrid.com/free/) zarejestrować bezpłatne konto SendGrid.</span><span class="sxs-lookup"><span data-stu-id="a5909-186">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="a5909-187">Skonfiguruj SendGrid</span><span class="sxs-lookup"><span data-stu-id="a5909-187">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5909-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5909-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="a5909-189">Dodaj kod w *Services/EmailSender.cs* podobne do następujących czynności, aby skonfigurować SendGrid:</span><span class="sxs-lookup"><span data-stu-id="a5909-189">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5909-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5909-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="a5909-191">Dodaj kod w *Services/MessageServices.cs* podobne do następujących czynności, aby skonfigurować SendGrid:</span><span class="sxs-lookup"><span data-stu-id="a5909-191">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="a5909-192">Włącz odzyskiwanie potwierdzenie i hasło konta</span><span class="sxs-lookup"><span data-stu-id="a5909-192">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="a5909-193">Szablon ma kod odzyskiwania potwierdzenie i hasło konta.</span><span class="sxs-lookup"><span data-stu-id="a5909-193">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="a5909-194">Znajdź `[HttpPost] Register` metody w *AccountController.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="a5909-194">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5909-195">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5909-195">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5909-196">Uniemożliwić użytkownikom nowo zarejestrowanych automatycznie zalogowania się przez komentowania się następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="a5909-196">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="a5909-197">Metody ukończenia jest wyświetlany z wierszem zmienione wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="a5909-197">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="a5909-198">Uwaga: Poprzedni kod zakończy się niepowodzeniem w przypadku zastosowania `IEmailSender` i wysłać wiadomość e-mail w formacie zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="a5909-198">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="a5909-199">Zobacz [ten problem](https://github.com/aspnet/Home/issues/2152) uzyskać więcej informacji i obejście tego problemu.</span><span class="sxs-lookup"><span data-stu-id="a5909-199">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5909-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5909-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a5909-201">Usuń komentarz kodu w celu włączenia potwierdzenie konta.</span><span class="sxs-lookup"><span data-stu-id="a5909-201">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="a5909-202">Uwaga: Firma Microsoft są także uniemożliwia nowo zarejestrowany użytkownik automatycznie zalogowania się przez komentowania się następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="a5909-202">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="a5909-203">Włącz odzyskiwanie haseł przez usunięcie komentarza kod w `ForgotPassword` akcji w *Controllers/AccountController.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="a5909-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="a5909-204">Usuń znaczniki komentarza element form w *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5909-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="a5909-205">Możesz usunąć `<p> For more information on how to enable reset password ... </p>` element, który zawiera łącze do tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="a5909-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="a5909-206">Rejestracji, potwierdź adres e-mail i zresetuj hasło</span><span class="sxs-lookup"><span data-stu-id="a5909-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="a5909-207">Uruchamianie aplikacji sieci web i testów potwierdzenie konta i hasła odzyskiwania przepływu.</span><span class="sxs-lookup"><span data-stu-id="a5909-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="a5909-208">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="a5909-208">Run the app and register a new user</span></span>

 ![Widok zarejestrować konto aplikacji sieci Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="a5909-210">Sprawdź pocztę łącza potwierdzenie konta.</span><span class="sxs-lookup"><span data-stu-id="a5909-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="a5909-211">Zobacz [debugowania e-mail](#debug) otrzymasz wiadomość e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="a5909-212">Kliknij łącze, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="a5909-213">Zaloguj się za pomocą Twój adres e-mail i hasło.</span><span class="sxs-lookup"><span data-stu-id="a5909-213">Log in with your email and password.</span></span>
* <span data-ttu-id="a5909-214">Wyloguj.</span><span class="sxs-lookup"><span data-stu-id="a5909-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="a5909-215">Wyświetl stronę Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="a5909-215">View the manage page</span></span>

<span data-ttu-id="a5909-216">Wybierz nazwę użytkownika w przeglądarce: ![okna przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="a5909-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="a5909-217">Konieczne może być Rozwiń pasek nawigacyjny, aby wyświetlić nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a5909-217">You might need to expand the navbar to see user name.</span></span>

![pasek nawigacyjny](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5909-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5909-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5909-220">Na stronie Zarządzanie zostanie wyświetlony z **profilu** kartę zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="a5909-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="a5909-221">**E-mail** zawiera pole wyboru, wskazując wiadomości e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="a5909-221">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![Strona Zarządzanie](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5909-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5909-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a5909-224">Ta strona będzie opisano później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="a5909-224">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="a5909-225">![Strona Zarządzanie](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="a5909-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="a5909-226">Resetowanie hasła testu</span><span class="sxs-lookup"><span data-stu-id="a5909-226">Test password reset</span></span>

* <span data-ttu-id="a5909-227">Jeśli użytkownik jest zalogowany, wybierz **wylogowania**.</span><span class="sxs-lookup"><span data-stu-id="a5909-227">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="a5909-228">Wybierz **Zaloguj** łącza, a następnie wybierz **nie pamiętasz hasła?** łącza.</span><span class="sxs-lookup"><span data-stu-id="a5909-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="a5909-229">Wprowadź adres e-mail używanego do rejestrowania konta.</span><span class="sxs-lookup"><span data-stu-id="a5909-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="a5909-230">Zostanie wysłana wiadomość e-mail zawierającą łącze do resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="a5909-230">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="a5909-231">Sprawdź pocztę i kliknij łącze, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="a5909-231">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="a5909-232">Po pomyślnie zresetowano hasło może się zalogować z poczty e-mail i nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="a5909-232">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="a5909-233">Debugowanie poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="a5909-233">Debug email</span></span>

<span data-ttu-id="a5909-234">Jeśli nie można rozpocząć pracę poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="a5909-234">If you can't get email working:</span></span>

* <span data-ttu-id="a5909-235">Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.</span><span class="sxs-lookup"><span data-stu-id="a5909-235">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="a5909-236">Sprawdź folder wiadomości-śmieci.</span><span class="sxs-lookup"><span data-stu-id="a5909-236">Check your spam folder.</span></span>
* <span data-ttu-id="a5909-237">Spróbuj inny alias e-mail przez dostawcę inny adres e-mail (Microsoft, Yahoo, Gmail itp.)</span><span class="sxs-lookup"><span data-stu-id="a5909-237">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="a5909-238">Utwórz [aplikacji konsoli do wysyłania wiadomości e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="a5909-238">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="a5909-239">Spróbuj wysłać do kont inny adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="a5909-240">**Uwaga:** ma nie używać hasła produkcji testowych i programistycznych ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="a5909-240">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="a5909-241">Jeśli publikowanie aplikacji na platformie Azure, możesz ustawić kluczy tajnych SendGrid zgodnie z ustawieniami aplikacji w portalu Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="a5909-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="a5909-242">System konfiguracji jest skonfigurowana do odczytu klucze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="a5909-242">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="a5909-243">Zapobiegaj logowania podczas rejestracji</span><span class="sxs-lookup"><span data-stu-id="a5909-243">Prevent login at registration</span></span>

<span data-ttu-id="a5909-244">Przy użyciu bieżącego szablonów, gdy użytkownik zamyka formularz rejestracji są rejestrowane w (uwierzytelniony).</span><span class="sxs-lookup"><span data-stu-id="a5909-244">With the current templates, once a user completes the registration form, they're logged in (authenticated).</span></span> <span data-ttu-id="a5909-245">Zazwyczaj mają Potwierdź swój adres e-mail przed ich zalogowaniem się.</span><span class="sxs-lookup"><span data-stu-id="a5909-245">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="a5909-246">W poniższej sekcji, firma Microsoft będzie zmodyfikuj kod do wymagają nowych użytkowników miał potwierdzony adres e-mail jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="a5909-246">In the section below, we will modify the code to require new users have a confirmed email before they're logged in.</span></span> <span data-ttu-id="a5909-247">Aktualizacja `[HttpPost] Login` akcji w *AccountController.cs* pliku z następującymi zmianami zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="a5909-247">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="a5909-248">**Uwaga:** ma nie używać hasła produkcji testowych i programistycznych ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="a5909-248">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="a5909-249">Jeśli publikowanie aplikacji na platformie Azure, możesz ustawić kluczy tajnych SendGrid zgodnie z ustawieniami aplikacji w portalu Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="a5909-249">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="a5909-250">System konfiguracji jest skonfigurowana do odczytu klucze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="a5909-250">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="a5909-251">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="a5909-251">Combine social and local login accounts</span></span>

<span data-ttu-id="a5909-252">Uwaga: Ta sekcja dotyczy tylko z platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a5909-252">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="a5909-253">Dla platformy ASP.NET Core 2.x, zobacz [to](https://github.com/aspnet/Docs/issues/3753) problem.</span><span class="sxs-lookup"><span data-stu-id="a5909-253">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="a5909-254">Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="a5909-254">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="a5909-255">Zobacz [Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="a5909-255">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="a5909-256">Konta lokalne i społecznościowych można łączyć, klikając link do wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="a5909-256">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="a5909-257">W następującej kolejności "RickAndMSFT@gmail.com" najpierw zostanie utworzona jako lokalne logowanie; jednak można najpierw utwórz konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="a5909-257">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="a5909-259">Polecenie **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="a5909-259">Click on the **Manage** link.</span></span> <span data-ttu-id="a5909-260">Uwaga zewnętrznych 0 (społecznościowych logowania) skojarzone z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="a5909-260">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzanie widoku](accconfirm/_static/manage.png)

<span data-ttu-id="a5909-262">Kliknij łącze do innej usługi logowania i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5909-262">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="a5909-263">Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznego:</span><span class="sxs-lookup"><span data-stu-id="a5909-263">In the image below, Facebook is the external authentication provider:</span></span>

![Zarządzanie wyświetlania serwisu Facebook widoku logowań zewnętrznych](accconfirm/_static/fb.png)

<span data-ttu-id="a5909-265">Dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="a5909-265">The two accounts have been combined.</span></span> <span data-ttu-id="a5909-266">Będzie można logować się na każdym koncie.</span><span class="sxs-lookup"><span data-stu-id="a5909-266">You will be able to log on with either account.</span></span> <span data-ttu-id="a5909-267">Możesz użytkowników, aby dodać konta lokalnego, w przypadku ich społecznościowych dziennika w usłudze uwierzytelniania jest wyłączony lub najprawdopodobniej one utraty dostępu do swojego konta społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="a5909-267">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they've lost access to their social account.</span></span>
