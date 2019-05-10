---
title: Konfigurowanie logowania zewnętrznego Account firmy Microsoft za pomocą programu ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Microsoft do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1733d049d6752c24d7749b5b5ae2a4b866492358
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903424"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="b8eba-103">Konfigurowanie logowania zewnętrznego Account firmy Microsoft za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8eba-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="b8eba-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b8eba-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b8eba-105">W tym samouczku dowiesz się, jak umożliwić użytkownikom logowanie się przy użyciu swojego konta Microsoft, przy użyciu przykładowy projekt programu ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b8eba-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="b8eba-106">Tworzenie aplikacji w portalu dla deweloperów firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="b8eba-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="b8eba-107">Przejdź do [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) i tworzyć ani logować się do konta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b8eba-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Zaloguj się w oknie dialogowym](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="b8eba-109">Jeśli nie masz jeszcze konta Microsoft, naciśnij przycisk  **[Zrób to!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="b8eba-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="b8eba-110">Po zarejestrowaniu się nastąpi przekierowanie do **aplikacje** strony:</span><span class="sxs-lookup"><span data-stu-id="b8eba-110">After signing in you are redirected to **My applications** page:</span></span>

![Otwórz w programie Microsoft Edge portalu dla deweloperów firmy Microsoft](index/_static/MicrosoftDev.png)

* <span data-ttu-id="b8eba-112">Naciśnij pozycję **Dodaj aplikację** w prawym górnym rogu, a następnie wprowadź swoje **Nazwa aplikacji** i **E-mail kontaktowy**:</span><span class="sxs-lookup"><span data-stu-id="b8eba-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Okno dialogowe nowej rejestracji aplikacji](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="b8eba-114">Do celów tego samouczka, należy wyczyścić **instrukcje konfiguracji** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="b8eba-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="b8eba-115">Naciśnij pozycję **Utwórz** w dalszym ciągu **rejestracji** strony.</span><span class="sxs-lookup"><span data-stu-id="b8eba-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="b8eba-116">Podaj **nazwa** i zanotuj wartość ustawienia **identyfikator aplikacji**, którego używasz jako `ClientId` później w samouczku:</span><span class="sxs-lookup"><span data-stu-id="b8eba-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Strona rejestracji](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="b8eba-118">Naciśnij pozycję **Dodaj platformy** w **platform** i wybierz pozycję **Web** platformy:</span><span class="sxs-lookup"><span data-stu-id="b8eba-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Dodaj okno dialogowe platformy](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="b8eba-120">W nowym **Web** platformy sekcji, wprowadź adres URL programowania za pomocą `/signin-microsoft` dołączany do **adresy URL przekierowania** pola (na przykład: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="b8eba-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="b8eba-121">Schemat uwierzytelniania firmy Microsoft, które są skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w `/signin-microsoft` trasy do zaimplementowania przepływu uwierzytelniania OAuth:</span><span class="sxs-lookup"><span data-stu-id="b8eba-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Sekcja platformy sieci Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="b8eba-123">Segmentem identyfikatora URI `/signin-microsoft` jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b8eba-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="b8eba-124">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania firmy Microsoft za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="b8eba-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="b8eba-125">Naciśnij pozycję **Dodawanie adresu URL** aby upewnić się, adres URL został dodany.</span><span class="sxs-lookup"><span data-stu-id="b8eba-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="b8eba-126">Wypełnij wszystkie ustawienia aplikacji, jeśli to konieczne, a następnie naciśnij przycisk **Zapisz** w dolnej części strony Aby zapisać zmiany w konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b8eba-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="b8eba-127">W przypadku wdrażania w witrynie będzie konieczne ponownie przeanalizowanie **rejestracji** strony, a następnie ustaw nowy publiczny adres URL.</span><span class="sxs-lookup"><span data-stu-id="b8eba-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="b8eba-128">Store aplikacji firmy Microsoft, identyfikator i hasło</span><span class="sxs-lookup"><span data-stu-id="b8eba-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="b8eba-129">Uwaga `Application Id` wyświetlany na **rejestracji** strony.</span><span class="sxs-lookup"><span data-stu-id="b8eba-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="b8eba-130">Naciśnij pozycję **wygenerować nowe hasło** w **wpisów tajnych aplikacji** sekcji.</span><span class="sxs-lookup"><span data-stu-id="b8eba-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="b8eba-131">Spowoduje to wyświetlenie okno, w którym kopiujesz hasło aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b8eba-131">This displays a box where you can copy the application password:</span></span>

![Nowe hasło wygenerowane okno dialogowe](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="b8eba-133">Link ustawienia poufne, takie jak Microsoft `Application ID` i `Password` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b8eba-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="b8eba-134">Do celów tego samouczka, nazwa tokeny `Authentication:Microsoft:ApplicationId` i `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="b8eba-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="b8eba-135">Skonfiguruj uwierzytelnianie za pomocą konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="b8eba-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="b8eba-136">Szablon projektu, w tym samouczku używane zapewnia, że [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pakiet jest już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="b8eba-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="b8eba-137">Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b8eba-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="b8eba-138">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="b8eba-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b8eba-139">Dodaj usługę Microsoft Account w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b8eba-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b8eba-140">Dodaj oprogramowanie pośredniczące Account Microsoft w `Configure` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b8eba-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

<span data-ttu-id="b8eba-141">Mimo że z terminologią używaną w portalu dla deweloperów firmy Microsoft, nazwy te tokeny `ApplicationId` i `Password`, są one widoczne jako `ClientId` i `ClientSecret` konfiguracji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b8eba-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="b8eba-142">Zobacz [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie Account firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b8eba-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="b8eba-143">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="b8eba-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="b8eba-144">Zaloguj się przy użyciu konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="b8eba-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="b8eba-145">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="b8eba-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="b8eba-146">Zostanie wyświetlona opcja Zaloguj się przy użyciu konta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b8eba-146">An option to sign in with Microsoft appears:</span></span>

![Aplikacja sieci Web, dziennik na stronie: Użytkownik nie jest uwierzytelniony](index/_static/DoneMicrosoft.png)

<span data-ttu-id="b8eba-148">Po kliknięciu firmy Microsoft są przekierowywane do firmy Microsoft do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b8eba-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="b8eba-149">Po zarejestrowaniu się przy użyciu Account firmy Microsoft (Jeśli nie zostało to zrobione) użytkownik jest monitowany, aby umożliwić aplikacji dostęp do informacji:</span><span class="sxs-lookup"><span data-stu-id="b8eba-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Okno dialogowe uwierzytelniania firmy Microsoft](index/_static/MicrosoftLogin.png)

<span data-ttu-id="b8eba-151">Naciśnij pozycję **tak** i nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b8eba-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="b8eba-152">Obecnie zalogowano Cię przy użyciu poświadczeń konta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b8eba-152">You are now logged in using your Microsoft credentials:</span></span>

![Aplikacja sieci Web: Użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="b8eba-154">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="b8eba-154">Troubleshooting</span></span>

* <span data-ttu-id="b8eba-155">Jeśli dostawca Account Microsoft przekieruje Cię do strony logowania w błąd, weź pod uwagę błąd tytuł i opis parametrów ciągu zapytania bezpośrednio po `#` (hasztag) w identyfikatorze Uri.</span><span class="sxs-lookup"><span data-stu-id="b8eba-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="b8eba-156">Mimo, że komunikat o błędzie wydaje się, że wskazywać na problem z uwierzytelniania firmy Microsoft, Najczęstszą przyczyną jest niezgodne żadnego z identyfikatora Uri aplikacji **identyfikatory URI przekierowań** określony dla **Web** platformy .</span><span class="sxs-lookup"><span data-stu-id="b8eba-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="b8eba-157">**ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*.</span><span class="sxs-lookup"><span data-stu-id="b8eba-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="b8eba-158">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="b8eba-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="b8eba-159">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="b8eba-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="b8eba-160">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="b8eba-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8eba-161">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b8eba-161">Next steps</span></span>

* <span data-ttu-id="b8eba-162">W tym artykule pokazano, jak można uwierzytelniać z firmą Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b8eba-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="b8eba-163">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b8eba-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="b8eba-164">Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy utworzyć nową `Password` w portalu dla deweloperów firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b8eba-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="b8eba-165">Ustaw `Authentication:Microsoft:ApplicationId` i `Authentication:Microsoft:Password` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="b8eba-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="b8eba-166">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="b8eba-166">The configuration system is set up to read keys from environment variables.</span></span>
