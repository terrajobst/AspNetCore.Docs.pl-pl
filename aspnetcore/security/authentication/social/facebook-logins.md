---
title: "Ustawienia logowania zewnętrznego Facebook w ASP.NET Core"
author: rick-anderson
description: "W tym samouczku przedstawiono integrację uwierzytelniania użytkownika serwisu Facebook konta do istniejącej aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2d36aa78f82b4a52a7c6a152bee2c4ca9923409f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="dfced-103">Konfigurowanie uwierzytelniania usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="dfced-103">Configuring Facebook authentication</span></span>

<span data-ttu-id="dfced-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dfced-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dfced-105">W tym samouczku przedstawiono sposób umożliwić użytkownikom logowanie za pomocą swojego konta usługi Facebook za pomocą przykładowy projekt platformy ASP.NET Core 2.0, utworzony na [poprzedniej strony](index.md).</span><span class="sxs-lookup"><span data-stu-id="dfced-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="dfced-106">Firma Microsoft Rozpocznij od utworzenia identyfikator aplikacji usługi Facebook, wykonując [oficjalnego kroki](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="dfced-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="dfced-107">Tworzenie aplikacji w usłudze Facebook</span><span class="sxs-lookup"><span data-stu-id="dfced-107">Create the app in Facebook</span></span>

*  <span data-ttu-id="dfced-108">Przejdź do [aplikacji usługi Facebook deweloperzy](https://developers.facebook.com/apps/) strony i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="dfced-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="dfced-109">Jeśli nie masz jeszcze konta w usłudze Facebook, użyj **Załóż Facebook** łącze na stronie logowania, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="dfced-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="dfced-110">Wybierz **Dodaj nową aplikację** przycisk w prawym górnym rogu, aby utworzyć nowy identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dfced-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Otwórz Facebook dla portalu deweloperów w programie Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="dfced-112">Wypełnij formularz, a następnie naciśnij pozycję **utworzyć identyfikator aplikacji** przycisku.</span><span class="sxs-lookup"><span data-stu-id="dfced-112">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Tworzenie formularza nowy identyfikator aplikacji](index/_static/FBNewAppId.png)

* <span data-ttu-id="dfced-114">Na **wybierz produkt** kliknij przycisk **Set Up** na **logowania serwisu Facebook** karty.</span><span class="sxs-lookup"><span data-stu-id="dfced-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Strona instalacji produktu](index/_static/FBProductSetup.png)
  
* <span data-ttu-id="dfced-116">**Szybkiego startu** z zostanie otwarty Kreator **wybierz platformę** jako pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="dfced-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="dfced-117">Pomiń Kreatora teraz klikając **ustawienia** łącza w menu po lewej stronie:</span><span class="sxs-lookup"><span data-stu-id="dfced-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Pomiń Szybki Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="dfced-119">Dostępne są **ustawień klienta OAuth** strony:</span><span class="sxs-lookup"><span data-stu-id="dfced-119">You are presented with the **Client OAuth Settings** page:</span></span>

![Strona Ustawienia OAuth klienta](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="dfced-121">Wprowadź identyfikator URI programowania z */signin-facebook* dołączany do **prawidłowy OAuth identyfikator URI przekierowania** pola (na przykład: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="dfced-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="dfced-122">Uwierzytelniania serwisu Facebook skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań */signin-facebook* trasy do zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="dfced-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="dfced-123">Kliknij przycisk **zapisać zmiany**.</span><span class="sxs-lookup"><span data-stu-id="dfced-123">Click **Save Changes**.</span></span>

* <span data-ttu-id="dfced-124">Kliknij przycisk **pulpitu nawigacyjnego** łącza na lewym pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="dfced-124">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="dfced-125">Na tej stronie, zanotuj Twojej `App ID` i `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="dfced-125">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="dfced-126">Doda zarówno do aplikacji platformy ASP.NET Core w następnej sekcji:</span><span class="sxs-lookup"><span data-stu-id="dfced-126">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Pulpit nawigacyjny dewelopera usługi Facebook](index/_static/FBDashboard.png)

* <span data-ttu-id="dfced-128">W przypadku wdrażania lokacji należy ponownie **logowania serwisu Facebook** strona instalacji i Zarejestruj nowy identyfikator URI publicznego.</span><span class="sxs-lookup"><span data-stu-id="dfced-128">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="dfced-129">Identyfikator aplikacji Facebook magazynu i klucz tajny aplikacji</span><span class="sxs-lookup"><span data-stu-id="dfced-129">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="dfced-130">Link ustawień poufnych, takich jak Facebook `App ID` i `App Secret` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="dfced-130">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="dfced-131">Do celów tego samouczka, nazwa tokeny `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="dfced-131">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="dfced-132">Wykonaj następujące polecenia, aby bezpiecznie przechowywać `App ID` i `App Secret` za pomocą Menedżera klucz tajny:</span><span class="sxs-lookup"><span data-stu-id="dfced-132">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="dfced-133">Konfigurowanie uwierzytelniania serwisu Facebook</span><span class="sxs-lookup"><span data-stu-id="dfced-133">Configure Facebook Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfced-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfced-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dfced-135">Dodaj usługę Facebook w `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="dfced-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfced-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfced-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dfced-137">Zainstaluj [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pakietu.</span><span class="sxs-lookup"><span data-stu-id="dfced-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="dfced-138">Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="dfced-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="dfced-139">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="dfced-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="dfced-140">Dodaj oprogramowaniu pośredniczącym usługi Facebook w `Configure` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="dfced-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="dfced-141">Zobacz [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelniania serwisu Facebook.</span><span class="sxs-lookup"><span data-stu-id="dfced-141">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="dfced-142">Opcje konfiguracji może służyć do:</span><span class="sxs-lookup"><span data-stu-id="dfced-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="dfced-143">Żądanie różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="dfced-143">Request different information about the user.</span></span>
* <span data-ttu-id="dfced-144">Dodaj argumenty ciągu zapytania dostosowywaniu środowiska logowania.</span><span class="sxs-lookup"><span data-stu-id="dfced-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="dfced-145">Zaloguj się za pomocą usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="dfced-145">Sign in with Facebook</span></span>

<span data-ttu-id="dfced-146">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="dfced-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="dfced-147">Widzisz opcję, aby zalogować się za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="dfced-147">You see an option to sign in with Facebook.</span></span>

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneFacebook.png)

<span data-ttu-id="dfced-149">Po kliknięciu **Facebook**, nastąpi przekierowanie do usługi Facebook dla uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="dfced-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLogin.png)

<span data-ttu-id="dfced-151">Uwierzytelniania serwisu Facebook żądań publicznego profilu i adres e-mail domyślnie:</span><span class="sxs-lookup"><span data-stu-id="dfced-151">Facebook authentication requests public profile and email address by default:</span></span>

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="dfced-153">Po wprowadzeniu poświadczeń usługi Facebook nastąpi przekierowanie do witryny, w którym można ustawić adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="dfced-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="dfced-154">Teraz jest zalogowany przy użyciu poświadczeń usługi Facebook:</span><span class="sxs-lookup"><span data-stu-id="dfced-154">You are now logged in using your Facebook credentials:</span></span>

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="dfced-156">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="dfced-156">Troubleshooting</span></span>

* <span data-ttu-id="dfced-157">**Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamość nie jest skonfigurowana przez wywołanie metody `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*.</span><span class="sxs-lookup"><span data-stu-id="dfced-157">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="dfced-158">Szablon projektu używany w tym samouczku zapewnia to zrobić.</span><span class="sxs-lookup"><span data-stu-id="dfced-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="dfced-159">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="dfced-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="dfced-160">Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.</span><span class="sxs-lookup"><span data-stu-id="dfced-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfced-161">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="dfced-161">Next steps</span></span>

* <span data-ttu-id="dfced-162">W tym artykule pokazano, jak można uwierzytelniać z serwisem Facebook.</span><span class="sxs-lookup"><span data-stu-id="dfced-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="dfced-163">Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](index.md).</span><span class="sxs-lookup"><span data-stu-id="dfced-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="dfced-164">Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy zresetować `AppSecret` w portalu dla deweloperów usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="dfced-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="dfced-165">Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` zgodnie z ustawieniami aplikacji w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="dfced-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="dfced-166">System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="dfced-166">The configuration system is set up to read keys from environment variables.</span></span>
