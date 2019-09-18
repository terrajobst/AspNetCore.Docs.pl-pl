---
title: Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core
author: rick-anderson
description: Samouczek z przykładami kodu demonstrujących integrację uwierzytelniania użytkownika konta w serwisie Facebook w istniejącej aplikacji ASP.NET Core.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/04/2019
uid: security/authentication/facebook-logins
ms.openlocfilehash: f7b21de7e5fe9d77804588280c3d8be9df8afee5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082550"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="b71b9-103">Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b71b9-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="b71b9-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b71b9-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b71b9-105">Ten samouczek z przykładami kodu pokazuje, jak umożliwić użytkownikom zalogowanie się za pomocą konta w serwisie Facebook przy użyciu przykładowego projektu ASP.NET Core 2,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b71b9-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="b71b9-106">Zaczniemy od utworzenia identyfikator aplikacji Facebook, postępując zgodnie z [oficjalnych kroków](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="b71b9-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="b71b9-107">Tworzenie aplikacji w usłudze Facebook</span><span class="sxs-lookup"><span data-stu-id="b71b9-107">Create the app in Facebook</span></span>

* <span data-ttu-id="b71b9-108">Przejdź do [aplikacji usługi Facebook deweloperów](https://developers.facebook.com/apps/) strony, a następnie zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="b71b9-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="b71b9-109">Jeśli nie masz jeszcze konta w serwisie Facebook, użyj **zarejestrować się za pomocą konta Facebook** łącze na stronie logowania, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="b71b9-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="b71b9-110">Naciśnij pozycję **Dodaj nową aplikację** przycisk w prawym górnym rogu, aby utworzyć nowy identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b71b9-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook dla portalu deweloperów Otwórz w programie Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="b71b9-112">Wypełnij formularz, a następnie naciśnij przycisk **utworzyć identyfikator aplikacji** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b71b9-112">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Tworzenie formularza nowego Identyfikatora aplikacji](index/_static/FBNewAppId.png)

* <span data-ttu-id="b71b9-114">Na **wybierz produkt** kliknij **Set Up** na **logowania do usługi Facebook** karty.</span><span class="sxs-lookup"><span data-stu-id="b71b9-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Strona Ustawienia produktu](index/_static/FBProductSetup.png)

* <span data-ttu-id="b71b9-116">**Szybki Start** z zostanie otwarty Kreator **wybierz platformę** jako pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="b71b9-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="b71b9-117">Pomiń Kreatora teraz, klikając **ustawienia** łącza w menu po lewej stronie:</span><span class="sxs-lookup"><span data-stu-id="b71b9-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Pomiń Szybki Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="b71b9-119">Zostanie wyświetlona **ustawienia uwierzytelniania OAuth klienta** strony:</span><span class="sxs-lookup"><span data-stu-id="b71b9-119">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Strona Ustawienia uwierzytelniania OAuth klienta](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="b71b9-121">Wprowadź identyfikator URI programistycznych dzięki */signin-facebook* dołączany do **prawidłowe OAuth identyfikatory URI przekierowań** pola (na przykład: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="b71b9-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="b71b9-122">Uwierzytelnianie serwisu Facebook skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w */signin-facebook* trasy do zaimplementowania przepływu uwierzytelniania OAuth.</span><span class="sxs-lookup"><span data-stu-id="b71b9-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="b71b9-123">Identyfikator URI */signin-facebook* jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania serwisu Facebook.</span><span class="sxs-lookup"><span data-stu-id="b71b9-123">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="b71b9-124">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowanie pośredniczące uwierzytelniania serwisu Facebook za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) Klasa.</span><span class="sxs-lookup"><span data-stu-id="b71b9-124">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="b71b9-125">Kliknij przycisk **Zapisz zmiany**.</span><span class="sxs-lookup"><span data-stu-id="b71b9-125">Click **Save Changes**.</span></span>

* <span data-ttu-id="b71b9-126">Kliknij przycisk **ustawienia** > **podstawowe** link w obszarze nawigacji po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="b71b9-126">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="b71b9-127">Na tej stronie, zwróć uwagę na Twoje `App ID` i `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="b71b9-127">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="b71b9-128">Doda zarówno do aplikacji platformy ASP.NET Core w następnej sekcji:</span><span class="sxs-lookup"><span data-stu-id="b71b9-128">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="b71b9-129">W przypadku wdrażania w witrynie konieczne ponownie przeanalizowanie **logowania do usługi Facebook** strona instalacji i zarejestrować nowy identyfikator URI publicznego.</span><span class="sxs-lookup"><span data-stu-id="b71b9-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="b71b9-130">Identyfikator aplikacji Facebook Store i klucza tajnego aplikacji</span><span class="sxs-lookup"><span data-stu-id="b71b9-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="b71b9-131">Link ustawień poufnych, takich jak Facebook `App ID` i `App Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b71b9-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="b71b9-132">Do celów tego samouczka, nazwa tokeny `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="b71b9-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="b71b9-133">Wykonaj następujące polecenia, aby bezpiecznie przechowywać `App ID` i `App Secret` za pomocą Menedżera klucz tajny:</span><span class="sxs-lookup"><span data-stu-id="b71b9-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="b71b9-134">Konfigurowanie uwierzytelniania serwisu Facebook</span><span class="sxs-lookup"><span data-stu-id="b71b9-134">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b71b9-135">Dodawanie usługi Facebook w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b71b9-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b71b9-136">Zainstaluj [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pakietu.</span><span class="sxs-lookup"><span data-stu-id="b71b9-136">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="b71b9-137">Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b71b9-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="b71b9-138">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="b71b9-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="b71b9-139">Dodaj oprogramowaniu pośredniczącym usługi Facebook w `Configure` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b71b9-139">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="b71b9-140">Zobacz [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie serwisu Facebook.</span><span class="sxs-lookup"><span data-stu-id="b71b9-140">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="b71b9-141">Opcje konfiguracji może służyć do:</span><span class="sxs-lookup"><span data-stu-id="b71b9-141">Configuration options can be used to:</span></span>

* <span data-ttu-id="b71b9-142">Żądanie różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="b71b9-142">Request different information about the user.</span></span>
* <span data-ttu-id="b71b9-143">Dodaj argumenty ciągu zapytania dostosowywaniu środowiska logowania.</span><span class="sxs-lookup"><span data-stu-id="b71b9-143">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="b71b9-144">Zaloguj się za pomocą usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="b71b9-144">Sign in with Facebook</span></span>

<span data-ttu-id="b71b9-145">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="b71b9-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="b71b9-146">Zostanie wyświetlona opcja logowania się za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="b71b9-146">You see an option to sign in with Facebook.</span></span>

![Aplikacja sieci Web: Użytkownik nieuwierzytelniony](index/_static/DoneFacebook.png)

<span data-ttu-id="b71b9-148">Po kliknięciu **Facebook**, nastąpi przekierowanie do usługi Facebook do uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="b71b9-148">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLogin.png)

<span data-ttu-id="b71b9-150">Uwierzytelnianie serwisu Facebook żądań publicznego profilu i adres e-mail, domyślnie:</span><span class="sxs-lookup"><span data-stu-id="b71b9-150">Facebook authentication requests public profile and email address by default:</span></span>

![Ekranie wyrażania zgody strony uwierzytelniania usługi Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="b71b9-152">Po wprowadzeniu poświadczeń serwisu Facebook nastąpi przekierowanie do witryny, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b71b9-152">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="b71b9-153">Obecnie zalogowano Cię przy użyciu poświadczeń serwisu Facebook:</span><span class="sxs-lookup"><span data-stu-id="b71b9-153">You are now logged in using your Facebook credentials:</span></span>

![Aplikacja sieci Web: Użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="b71b9-155">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="b71b9-155">Troubleshooting</span></span>

* <span data-ttu-id="b71b9-156">**Tylko ASP.NET Core 2. x:** Jeśli tożsamość nie jest skonfigurowana przez `services.AddIdentity` wywołanie `ConfigureServices`w, próba uwierzytelnienia spowoduje powstanie *argumentuexception: Należy podać*opcję "SignInScheme".</span><span class="sxs-lookup"><span data-stu-id="b71b9-156">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="b71b9-157">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="b71b9-157">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="b71b9-158">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="b71b9-158">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="b71b9-159">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="b71b9-159">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b71b9-160">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b71b9-160">Next steps</span></span>

* <span data-ttu-id="b71b9-161">Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) do projektu w celu zaawansowania scenariuszy uwierzytelniania w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="b71b9-161">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to your project for advanced Facebook authentication scenarios.</span></span> <span data-ttu-id="b71b9-162">Ten pakiet nie jest wymagany do zintegrowania funkcjonalności logowania zewnętrznego w usłudze Facebook z Twoją aplikacją.</span><span class="sxs-lookup"><span data-stu-id="b71b9-162">This package isn't required to integrate Facebook external login functionality with your app.</span></span> 

* <span data-ttu-id="b71b9-163">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="b71b9-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="b71b9-164">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b71b9-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="b71b9-165">Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `AppSecret` w portalu dla deweloperów usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="b71b9-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="b71b9-166">Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="b71b9-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="b71b9-167">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="b71b9-167">The configuration system is set up to read keys from environment variables.</span></span>
