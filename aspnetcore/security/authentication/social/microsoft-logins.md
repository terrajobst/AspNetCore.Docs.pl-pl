---
title: Konfigurowanie logowania zewnętrznego Account firmy Microsoft z platformy ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelnianie użytkownika konta Microsoft do istniejącej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 7244f7a808899a2846bb8b40e626208f168d40b8
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="b6dbb-103">Konfigurowanie logowania zewnętrznego Account firmy Microsoft z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6dbb-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="b6dbb-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b6dbb-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b6dbb-105">W tym samouczku przedstawiono sposób umożliwić użytkownikom logowanie za pomocą swojego konta Microsoft, za pomocą przykładowy projekt platformy ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b6dbb-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="b6dbb-106">Utwórz aplikację w portalu dla deweloperów firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="b6dbb-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="b6dbb-107">Przejdź do [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) i utworzyć lub zaloguj się do konta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Zaloguj się w oknie dialogowym](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="b6dbb-109">Jeśli nie masz już konto Microsoft, naciśnij przycisk  **[utwórz je!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="b6dbb-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="b6dbb-110">Po zalogowaniu użytkownik zostanie przekierowany do **aplikacje** strony:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-110">After signing in you are redirected to **My applications** page:</span></span>

![Otwórz w programie Microsoft Edge Microsoft Developer Portal](index/_static/MicrosoftDev.png)

* <span data-ttu-id="b6dbb-112">Wybierz **Dodaj aplikację** w prawym górnym rogu, a następnie wprowadź Twojej **Nazwa aplikacji** i **E-mail kontaktu**:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Okno dialogowe nowego Rejestracja aplikacji](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="b6dbb-114">Do celów tego samouczka, wyczyść **instrukcje konfiguracji** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="b6dbb-115">Wybierz **Utwórz** w dalszym ciągu **rejestracji** strony.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="b6dbb-116">Podaj **nazwa** i zanotuj wartość **identyfikator aplikacji**, który jest używany jako `ClientId` dalszej części samouczka:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Strony rejestracji](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="b6dbb-118">Wybierz **dodać platformy** w **platformy** a następnie wybierz opcję **Web** platformy:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Platforma okno dialogowe Dodawanie](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="b6dbb-120">W nowym **Web** platformy wprowadź adres URL programowanie z */signin-microsoft* dołączany do **adresów URL przekierowań** pola (na przykład: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="b6dbb-120">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="b6dbb-121">Schemat uwierzytelniania firmy Microsoft, skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań */signin-microsoft* trasy do zaimplementowania przepływu OAuth:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Sekcja platformy sieci Web](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="b6dbb-123">Wybierz **Dodaj adres URL** zapewnienie adres URL został dodany.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-123">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="b6dbb-124">Wypełnij wszystkie ustawienia aplikacji, w razie potrzeby, a następnie naciśnij pozycję **zapisać** w dolnej części strony, aby zapisać zmiany w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-124">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="b6dbb-125">W przypadku wdrażania lokacji należy ponownie **rejestracji** i ustaw nowy publiczny adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-125">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="b6dbb-126">Przechowywanie identyfikator aplikacji firmy Microsoft i hasła</span><span class="sxs-lookup"><span data-stu-id="b6dbb-126">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="b6dbb-127">Uwaga `Application Id` wyświetlany na **rejestracji** strony.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-127">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="b6dbb-128">Wybierz **wygenerować nowe hasło** w **klucze tajne aplikacji** sekcji.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-128">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="b6dbb-129">Spowoduje to wyświetlenie pola, w którym można kopiować hasło aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-129">This displays a box where you can copy the application password:</span></span>

![Nowe hasło wygenerowane okno dialogowe](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="b6dbb-131">Link ustawień poufnych, takich jak Microsoft `Application ID` i `Password` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b6dbb-131">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="b6dbb-132">Do celów tego samouczka, nazwa tokeny `Authentication:Microsoft:ApplicationId` i `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-132">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="b6dbb-133">Konfigurowanie uwierzytelniania konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="b6dbb-133">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="b6dbb-134">Szablon projektu używany w tym samouczku upewnia się, że [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pakiet jest już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="b6dbb-135">Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="b6dbb-136">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6dbb-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6dbb-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="b6dbb-138">Dodaj usługę Microsoft Account w `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-138">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6dbb-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6dbb-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="b6dbb-140">Dodaj oprogramowanie pośredniczące Account Microsoft w `Configure` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="b6dbb-141">Chociaż te tokeny nazwy z terminologią używaną w portalu usługi Microsoft Developer `ApplicationId` i `Password`, są one widoczne jako `ClientId` i `ClientSecret` konfiguracji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="b6dbb-142">Zobacz [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie Account firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="b6dbb-143">To może być używane do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="b6dbb-144">Zaloguj się przy użyciu konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="b6dbb-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="b6dbb-145">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="b6dbb-146">Pojawia się opcję, aby zalogować się przy użyciu firmy Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-146">An option to sign in with Microsoft appears:</span></span>

![Aplikacja dziennika na stronie sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneMicrosoft.png)

<span data-ttu-id="b6dbb-148">Po kliknięciu firmy Microsoft są przekierowywane do firmy Microsoft do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="b6dbb-149">Po zarejestrowaniu się za pomocą Account firmy Microsoft (Jeśli nie jest już zalogowany) pojawi się monit, aby umożliwić aplikacji dostęp do informacji:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Dialog uwierzytelniania firmy Microsoft](index/_static/MicrosoftLogin.png)

<span data-ttu-id="b6dbb-151">Wybierz **tak** i nastąpi przekierowanie do witryny sieci web, w którym można ustawić adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="b6dbb-152">Użytkownik jest obecnie zalogowany przy użyciu poświadczeń konta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b6dbb-152">You are now logged in using your Microsoft credentials:</span></span>

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="b6dbb-154">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="b6dbb-154">Troubleshooting</span></span>

* <span data-ttu-id="b6dbb-155">Jeśli dostawca Account Microsoft przekierowuje do logowania strony błędu, weź pod uwagę błąd tytuł i opis parametrów ciągu zapytania bezpośrednio po `#` (hasztagiem) w identyfikatorze Uri.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="b6dbb-156">Mimo że komunikat o błędzie jest prawdopodobnie wskazywać na problem z uwierzytelniania firmy Microsoft, Najczęstszą przyczyną jest aplikacja nie zgodne z żadnym z identyfikatora Uri **identyfikator URI przekierowania** określona dla **Web** platformy .</span><span class="sxs-lookup"><span data-stu-id="b6dbb-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="b6dbb-157">**Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamości nie jest skonfigurowana pod wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="b6dbb-158">Szablon projektu używany w tym samouczku zapewnia to zrobić.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="b6dbb-159">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, otrzyma *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="b6dbb-160">Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6dbb-161">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b6dbb-161">Next steps</span></span>

* <span data-ttu-id="b6dbb-162">W tym artykule pokazano, jak można uwierzytelniać z firmą Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="b6dbb-163">Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b6dbb-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="b6dbb-164">Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy utworzyć nowy `Password` w portalu dla deweloperów firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="b6dbb-165">Ustaw `Authentication:Microsoft:ApplicationId` i `Authentication:Microsoft:Password` zgodnie z ustawieniami aplikacji w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="b6dbb-166">System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="b6dbb-166">The configuration system is set up to read keys from environment variables.</span></span>
