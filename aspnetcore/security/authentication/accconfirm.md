---
title: Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core
author: rick-anderson
description: Informacje o sposobie tworzenia aplikacji platformy ASP.NET Core z funkcją resetowania hasła i potwierdzania poczty e-mail.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: e0bca48fcaa9a29847fdda714698ed8562d30707
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="b6f6e-103">Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6f6e-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="b6f6e-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="b6f6e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="b6f6e-105">W tym samouczku przedstawiono sposób tworzenia aplikacji platformy ASP.NET Core z funkcją resetowania hasła i potwierdzania poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="b6f6e-106">W tym samouczku jest **nie** początku tematu.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="b6f6e-107">Należy zapoznać się z:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-107">You should be familiar with:</span></span>

* [<span data-ttu-id="b6f6e-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6f6e-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="b6f6e-109">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="b6f6e-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="b6f6e-110">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="b6f6e-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b6f6e-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b6f6e-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="b6f6e-112">Zobacz [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) dla wersji platformy ASP.NET Core MVC 1.1 i 2.x.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6f6e-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b6f6e-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="b6f6e-114">Utwórz nowy projekt platformy ASP.NET Core z poziomu interfejsu wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="b6f6e-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6f6e-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* <span data-ttu-id="b6f6e-116">`--auth Individual` Określa szablon projektu indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-116">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="b6f6e-117">W systemie Windows, należy dodać `-uld` opcji.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-117">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="b6f6e-118">Określa ona, że LocalDB powinna być używana zamiast funkcji SQLite.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-118">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="b6f6e-119">Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-119">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6f6e-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b6f6e-121">Jeśli używasz interfejsu wiersza polecenia lub SQLite, uruchom następujące polecenie w oknie poleceń:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-121">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="b6f6e-122">`--auth Individual` Określa szablon projektu indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-122">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="b6f6e-123">W systemie Windows, należy dodać `-uld` opcji.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-123">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="b6f6e-124">Określa ona, że LocalDB powinna być używana zamiast funkcji SQLite.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-124">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="b6f6e-125">Uruchom `new mvc --help` Aby uzyskać pomoc dotyczącą tego polecenia.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-125">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="b6f6e-126">Można również utworzyć nowy projekt platformy ASP.NET Core z programem Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-126">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="b6f6e-127">W programie Visual Studio Utwórz nową **aplikacji sieci Web** projektu.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-127">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="b6f6e-128">Wybierz **platformy ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-128">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="b6f6e-129">**Oprogramowanie .NET core** jest zaznaczona na poniższej ilustracji, ale można wybrać **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-129">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="b6f6e-130">Wybierz **Zmień uwierzytelnianie** ustawiono **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-130">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="b6f6e-131">Zachowaj ustawienie domyślne **kont magazynu użytkowników w aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-131">Keep the default **Store user accounts in-app**.</span></span>

![Okno dialogowe nowego projektu przedstawiający "Indywidualne konta użytkowników radio" wybrane](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="b6f6e-133">Testowanie nowej rejestracji użytkownika</span><span class="sxs-lookup"><span data-stu-id="b6f6e-133">Test new user registration</span></span>

<span data-ttu-id="b6f6e-134">Uruchom aplikację, wybierz **zarejestrować** łącza, a następnie zarejestrować użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-134">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="b6f6e-135">Postępuj zgodnie z instrukcjami, aby uruchomić program Entity Framework Core migracji.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-135">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="b6f6e-136">W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-136">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="b6f6e-137">Po przesłaniu rejestracji, są rejestrowane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-137">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="b6f6e-138">Później w samouczku kod jest aktualizowana, aby nowi użytkownicy nie może logować się do momentu sprawdzania poprawności poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-138">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="b6f6e-139">Widok bazy danych tożsamości</span><span class="sxs-lookup"><span data-stu-id="b6f6e-139">View the Identity database</span></span>

<span data-ttu-id="b6f6e-140">Zobacz [pracy z bazy danych SQLite w projekcie platformy ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) instrukcje dotyczące sposobu wyświetlania bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-140">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="b6f6e-141">Dla programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-141">For Visual Studio:</span></span>

* <span data-ttu-id="b6f6e-142">Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="b6f6e-142">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="b6f6e-143">Przejdź do **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-143">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="b6f6e-144">Kliknij prawym przyciskiem myszy **dbo. AspNetUsers** > **wyświetlania danych**:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-144">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu kontekstowe tabeli AspNetUsers w Eksploratorze obiektów SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="b6f6e-146">Należy zwrócić uwagę tabeli `EmailConfirmed` pole jest `False`.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-146">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="b6f6e-147">Można użyć tej wiadomości e-mail ponownie w następnym kroku gdy aplikacja wysyła wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-147">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="b6f6e-148">Kliknij prawym przyciskiem myszy na wiersz i wybierz **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-148">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="b6f6e-149">Usuwanie aliasów e-mail ułatwia w kolejnych krokach.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-149">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="b6f6e-150">Wymagać protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="b6f6e-150">Require HTTPS</span></span>

<span data-ttu-id="b6f6e-151">Zobacz [wymagać protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="b6f6e-151">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="b6f6e-152">Wymagaj wiadomości e-mail z potwierdzeniem</span><span class="sxs-lookup"><span data-stu-id="b6f6e-152">Require email confirmation</span></span>

<span data-ttu-id="b6f6e-153">Jest najlepszym rozwiązaniem w celu potwierdzenia adresu e-mail nowej rejestracji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-153">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="b6f6e-154">Wiadomości e-mail potwierdzenie pomaga sprawdzić ich jest nie przeprowadza personifikacji ktoś (to znaczy one nie został zarejestrowany przy do kogoś innego adresu e-mail).</span><span class="sxs-lookup"><span data-stu-id="b6f6e-154">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="b6f6e-155">Załóżmy, że masz forum dyskusyjne i chcesz zapobiec "yli@example.com"z rejestracją jako"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="b6f6e-155">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="b6f6e-156">Bez wiadomości e-mail z potwierdzeniem "nolivetto@contoso.com" może otrzymywać niechcianych wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-156">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="b6f6e-157">Załóżmy, że użytkownik przypadkowo zarejestrowany jako "ylo@example.com", a nie wystąpieniem słowa "yli".</span><span class="sxs-lookup"><span data-stu-id="b6f6e-157">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="b6f6e-158">Nie można używać hasła odzyskiwania, ponieważ aplikacja nie ma ich prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-158">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="b6f6e-159">Wiadomości e-mail z potwierdzeniem chroni tylko ograniczone z robotów.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-159">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="b6f6e-160">Wiadomości e-mail z potwierdzeniem nie zapewnia ochrony przed złośliwymi użytkownikami z wielu kont e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-160">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="b6f6e-161">Zazwyczaj chcesz uniemożliwić użytkownikom nowe przesyłanie danych do witryny sieci web, zanim potwierdzony adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-161">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="b6f6e-162">Aktualizacja `ConfigureServices` aby wymagać potwierdzenia poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-162">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="b6f6e-163">`config.SignIn.RequireConfirmedEmail = true;` zarejestrowani użytkownicy uniemożliwia logowanie do momentu swój adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-163">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="b6f6e-164">Skonfiguruj dostawcę poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="b6f6e-164">Configure email provider</span></span>

<span data-ttu-id="b6f6e-165">W tym samouczku SendGrid służy do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-165">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="b6f6e-166">Konieczne jest włączenie konta i klucz do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-166">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="b6f6e-167">Można użyć innych dostawców poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-167">You can use other email providers.</span></span> <span data-ttu-id="b6f6e-168">Platformy ASP.NET Core zawiera 2.x `System.Net.Mail`, co pozwala na wysyłanie wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-168">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="b6f6e-169">Firma Microsoft zaleca się, że używasz SendGrid lub inna usługa poczty e-mail do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-169">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="b6f6e-170">SMTP jest trudne do zabezpieczania i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-170">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="b6f6e-171">[Wzorzec opcje](xref:fundamentals/configuration/options) umożliwia dostęp do konta i klucz Ustawienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-171">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="b6f6e-172">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b6f6e-172">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="b6f6e-173">Utwórz klasę, aby pobrać klucz zabezpieczanie poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-173">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="b6f6e-174">Dla tego przykładu `AuthMessageSenderOptions` klasy jest tworzony w *Services/AuthMessageSenderOptions.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-174">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="b6f6e-175">Ustaw `SendGridUser` i `SendGridKey` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b6f6e-175">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="b6f6e-176">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-176">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="b6f6e-177">W systemie Windows, klucz tajny Manager przechowuje par kluczy i wartości w *secrets.json* w pliku `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-177">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="b6f6e-178">Zawartość *secrets.json* pliku nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-178">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="b6f6e-179">*Secrets.json* plików są wyświetlane poniżej ( `SendGridKey` wartości została usunięta.)</span><span class="sxs-lookup"><span data-stu-id="b6f6e-179">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="b6f6e-180">Konfigurowanie uruchamiania do użycia AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="b6f6e-180">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="b6f6e-181">Dodaj `AuthMessageSenderOptions` w kontenerze usługi na końcu `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-181">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6f6e-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6f6e-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="b6f6e-184">Konfigurowanie klasy AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="b6f6e-184">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="b6f6e-185">W tym samouczku przedstawiono sposób dodawania powiadomień pocztą e-mail za pomocą [SendGrid](https://sendgrid.com/), ale mogą wysyłać poczty e-mail przy użyciu SMTP i innych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-185">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="b6f6e-186">Zainstaluj `SendGrid` pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-186">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="b6f6e-187">W wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-187">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="b6f6e-188">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-188">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="b6f6e-189">Zobacz [zacznij pracę bezpłatnie sendgrid](https://sendgrid.com/free/) zarejestrować bezpłatne konto SendGrid.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-189">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="b6f6e-190">Skonfiguruj SendGrid</span><span class="sxs-lookup"><span data-stu-id="b6f6e-190">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6f6e-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="b6f6e-192">Aby skonfigurować SendGrid, Dodaj kod podobny do następującego *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-192">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6f6e-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="b6f6e-194">Dodaj kod w *Services/MessageServices.cs* podobne do następujących czynności, aby skonfigurować SendGrid:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-194">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="b6f6e-195">Włącz odzyskiwanie potwierdzenie i hasło konta</span><span class="sxs-lookup"><span data-stu-id="b6f6e-195">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="b6f6e-196">Szablon ma kod odzyskiwania potwierdzenie i hasło konta.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-196">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="b6f6e-197">Znajdź `OnPostAsync` metody w *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-197">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6f6e-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="b6f6e-199">Uniemożliwić użytkownikom nowo zarejestrowanych automatycznie zalogowania się przez komentowania się następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-199">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="b6f6e-200">Metody ukończenia jest wyświetlany z wierszem zmienione wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-200">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6f6e-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="b6f6e-202">Aby włączyć potwierdzenie konta, usuń znaczniki komentarza z następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-202">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="b6f6e-203">**Uwaga:** kod uniemożliwia nowo zarejestrowanym użytkownikiem automatycznie zalogowania się przez komentowania się następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-203">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="b6f6e-204">Włącz odzyskiwanie haseł przez usunięcie komentarza kod w `ForgotPassword` akcji *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="b6f6e-205">Usuń znaczniki komentarza element form w *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="b6f6e-206">Możesz usunąć `<p> For more information on how to enable reset password ... </p>` element, który zawiera link do tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="b6f6e-207">Rejestracji, potwierdź adres e-mail i zresetuj hasło</span><span class="sxs-lookup"><span data-stu-id="b6f6e-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="b6f6e-208">Uruchamianie aplikacji sieci web i testów potwierdzenie konta i hasła odzyskiwania przepływu.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="b6f6e-209">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="b6f6e-209">Run the app and register a new user</span></span>

  ![Widok zarejestrować konto aplikacji sieci Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="b6f6e-211">Sprawdź pocztę łącza potwierdzenie konta.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="b6f6e-212">Zobacz [debugowania e-mail](#debug) otrzymasz wiadomość e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="b6f6e-213">Kliknij łącze, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="b6f6e-214">Zaloguj się za pomocą Twój adres e-mail i hasło.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-214">Log in with your email and password.</span></span>
* <span data-ttu-id="b6f6e-215">Wyloguj.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="b6f6e-216">Wyświetl stronę Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="b6f6e-216">View the manage page</span></span>

<span data-ttu-id="b6f6e-217">Wybierz nazwę użytkownika w przeglądarce: ![okna przeglądarki z nazwą użytkownika](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="b6f6e-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="b6f6e-218">Konieczne może być Rozwiń pasek nawigacyjny, aby wyświetlić nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-218">You might need to expand the navbar to see user name.</span></span>

![pasek nawigacyjny](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6f6e-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b6f6e-221">Na stronie Zarządzanie zostanie wyświetlony z **profilu** kartę zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="b6f6e-222">**E-mail** zawiera pole wyboru, wskazując wiadomości e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-222">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![Strona Zarządzanie](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6f6e-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6f6e-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b6f6e-225">Jest to wymienione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-225">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="b6f6e-226">![Strona Zarządzanie](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="b6f6e-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="b6f6e-227">Resetowanie hasła testu</span><span class="sxs-lookup"><span data-stu-id="b6f6e-227">Test password reset</span></span>

* <span data-ttu-id="b6f6e-228">Jeśli użytkownik jest zalogowany, wybierz **wylogowania**.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-228">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="b6f6e-229">Wybierz **Zaloguj** łącza, a następnie wybierz **nie pamiętasz hasła?** łącza.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="b6f6e-230">Wprowadź adres e-mail używanego do rejestrowania konta.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="b6f6e-231">Zostanie wysłana wiadomość e-mail zawierającą łącze do resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-231">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="b6f6e-232">Sprawdź pocztę i kliknij łącze, aby zresetować hasło.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-232">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="b6f6e-233">Po pomyślnie zresetowano hasło możesz zalogować się przy użyciu Twój adres e-mail i nowe hasło.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-233">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="b6f6e-234">Debugowanie poczty e-mail</span><span class="sxs-lookup"><span data-stu-id="b6f6e-234">Debug email</span></span>

<span data-ttu-id="b6f6e-235">Jeśli nie można rozpocząć pracę poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-235">If you can't get email working:</span></span>

* <span data-ttu-id="b6f6e-236">Utwórz [aplikacji konsoli do wysyłania wiadomości e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="b6f6e-236">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="b6f6e-237">Przegląd [działania pocztą E-mail](https://sendgrid.com/docs/User_Guide/email_activity.html) strony.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-237">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="b6f6e-238">Sprawdź folder wiadomości-śmieci.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-238">Check your spam folder.</span></span>
* <span data-ttu-id="b6f6e-239">Spróbuj inny alias e-mail przez dostawcę inny adres e-mail (Microsoft, Yahoo, Gmail itp.)</span><span class="sxs-lookup"><span data-stu-id="b6f6e-239">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="b6f6e-240">Spróbuj wysłać do kont inny adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="b6f6e-241">**Ze względów bezpieczeństwa** jest **nie** Użyj hasła produkcji testowych i programistycznych.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-241">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="b6f6e-242">Jeśli publikowanie aplikacji na platformie Azure, możesz ustawić kluczy tajnych SendGrid zgodnie z ustawieniami aplikacji w portalu Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="b6f6e-243">System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-243">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="b6f6e-244">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="b6f6e-244">Combine social and local login accounts</span></span>

<span data-ttu-id="b6f6e-245">Do ukończenia tej sekcji, należy najpierw włączyć zewnętrznego dostawcę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-245">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="b6f6e-246">Zobacz [Facebook, Google i zewnętrznego dostawcy uwierzytelniania](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b6f6e-246">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="b6f6e-247">Konta lokalne i społecznościowych można łączyć, klikając link do wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-247">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="b6f6e-248">W następującej kolejności "RickAndMSFT@gmail.com" najpierw zostanie utworzona jako lokalne logowanie; jednak można najpierw utwórz konto jako społecznościowych logowania, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-248">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplikacja sieci Web: RickAndMSFT@gmail.com użytkownik uwierzytelniony](accconfirm/_static/rick.png)

<span data-ttu-id="b6f6e-250">Polecenie **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-250">Click on the **Manage** link.</span></span> <span data-ttu-id="b6f6e-251">Uwaga zewnętrznych 0 (społecznościowych logowania) skojarzone z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-251">Note the 0 external (social logins) associated with this account.</span></span>

![Zarządzanie widoku](accconfirm/_static/manage.png)

<span data-ttu-id="b6f6e-253">Kliknij łącze do innej usługi logowania i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-253">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="b6f6e-254">Na poniższej ilustracji Facebook jest dostawcy uwierzytelniania zewnętrznego:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-254">In the following image, Facebook is the external authentication provider:</span></span>

![Zarządzanie wyświetlania serwisu Facebook widoku logowań zewnętrznych](accconfirm/_static/fb.png)

<span data-ttu-id="b6f6e-256">Dwa konta zostały połączone.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-256">The two accounts have been combined.</span></span> <span data-ttu-id="b6f6e-257">Będą mogli logować się na każdym koncie.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-257">You are able to log on with either account.</span></span> <span data-ttu-id="b6f6e-258">Możesz użytkowników, aby dodać konta lokalnego, w przypadku ich społecznościowych logowania uwierzytelniania usługa nie działa lub najprawdopodobniej one utraty dostępu do swojego konta społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-258">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="b6f6e-259">Włącz potwierdzenie konta po lokacja ma użytkowników</span><span class="sxs-lookup"><span data-stu-id="b6f6e-259">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="b6f6e-260">Włączanie potwierdzenia konta w witrynie użytkownikom do zablokowania wszystkich istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-260">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="b6f6e-261">Istniejący użytkownicy są zablokowane, ponieważ nie są potwierdzone ich kont.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-261">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="b6f6e-262">Aby obejść Kończenie blokady użytkownika, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="b6f6e-262">To work around exiting user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="b6f6e-263">Aktualizacja bazy danych, aby oznaczyć wszystkich istniejących użytkowników, jak zostało potwierdzone</span><span class="sxs-lookup"><span data-stu-id="b6f6e-263">Update the database to mark all existing users as being confirmed</span></span>
* <span data-ttu-id="b6f6e-264">Potwierdzanie istniejących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-264">Confirm exiting users.</span></span> <span data-ttu-id="b6f6e-265">Na przykład partii — wysyłanie wiadomości e-mail z potwierdzeniem łącza.</span><span class="sxs-lookup"><span data-stu-id="b6f6e-265">For example, batch-send emails with confirmation links.</span></span>
